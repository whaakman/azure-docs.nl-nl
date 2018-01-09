---
title: Fouttolerantie in Azure Data Factory-Kopieeractiviteit toevoegen incompatibel rijen overgeslagen | Microsoft Docs
description: "Informatie over het toevoegen van fouttolerantie in Azure Data Factory-Kopieeractiviteit door niet-compatibele rijen overgeslagen tijdens het kopiëren"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6e7923e2e0a23f22f7dff8c316050a1757310456
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Fouttolerantie toevoegen in de kopieerbewerking door niet-compatibele rijen overgeslagen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [fouttolerantie in de kopieerbewerking van de Data Factory versie 2](../copy-activity-fault-tolerance.md).

Azure Data Factory [Kopieeractiviteit](data-factory-data-movement-activities.md) biedt twee manieren om af te handelen incompatibel rijen bij het kopiëren van gegevens tussen de bron- en sink gegevensarchieven:

- U kunt afbreken en mislukt de kopie activiteit als niet-compatibele gegevens aangetroffen (standaardinstelling).
- U kunt doorgaan met het kopiëren alle gegevens door toe te voegen fouttolerantie en niet-compatibele gegevensrijen wordt overgeslagen. Bovendien kunt u de niet-compatibele rijen registreren in Azure Blob-opslag. Bekijk vervolgens het logboek voor meer informatie over de oorzaak van de fout, los van de gegevens op de gegevensbron en probeer het opnieuw met de kopieerbewerking.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
Kopieeractiviteit ondersteunt drie scenario's voor het detecteren, wordt overgeslagen en logboekregistratie incompatibel gegevens:

- **Incompatibiliteit tussen het gegevensbrontype en het systeemeigen sink-type**

    Bijvoorbeeld: gegevens kopiëren van een CSV-bestand in Blob storage naar een SQL-database met de schemadefinitie van een dat drie bevat **INT** kolommen van het type. De CSV-bestand rijen die numerieke gegevens, zoals bevatten `123,456,789` is gekopieerd naar de store sink. Echter, de rijen die niet-numerieke waarden bevatten, zoals `123,456,abc` zijn gedetecteerd als incompatibele en worden overgeslagen.

- **Het aantal kolommen tussen de bron en de sink komt niet overeen**

    Bijvoorbeeld: gegevens kopiëren van een CSV-bestand in Blob storage naar een SQL-database met de schemadefinitie van een dat zes kolommen bevat. De CSV-bestand-rijen met zes kolommen worden gekopieerd naar de store sink. De CSV-bestand rijen die minder dan zes of meer kolommen bevatten als niet compatibel zijn gedetecteerd en worden overgeslagen.

- **Primaire sleutel is geschonden bij het schrijven naar een relationele database**

    Bijvoorbeeld: gegevens kopiëren van een SQL-server naar een SQL-database. Een primaire sleutel is gedefinieerd in de sink SQL-database, maar die geen primaire sleutel is gedefinieerd in de bron-SQL-server. De dubbele rijen die in de bron voorkomen kunnen niet worden gekopieerd naar de sink. Alleen de eerste rij van de brongegevens Kopieeractiviteit gekopieerd naar de sink. De volgende bronrijen met dubbele primaire sleutelwaarde zijn gedetecteerd als niet compatibel en worden overgeslagen.

>[!NOTE]
>Deze functie is niet van toepassing wanneer kopieeractiviteit is geconfigureerd voor het aanroepen van externe gegevens laden met inbegrip van mechanisme [Azure SQL Data Warehouse PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) of [Amazon Redshift Unload](data-factory-amazon-redshift-connector.md#use-unload-to-copy-data-from-amazon-redshift). Gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase, PolyBase van systeemeigen fouttolerantie ondersteuning door te geven '[polyBaseSettings](data-factory-azure-sql-data-warehouse-connector.md#sqldwsink)' in de kopieerbewerking.

## <a name="configuration"></a>Configuratie
Het volgende voorbeeld bevat een JSON-definitie voor het configureren van de niet-compatibele rijen in de kopieerbewerking wordt overgeslagen:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Schakel overslaan incompatibel rijen tijdens het kopiëren van of niet. | True<br/>False (standaard) | Nee |
| **redirectIncompatibleRowSettings** | Een groep met eigenschappen die kunnen worden opgegeven wanneer u wilt vastleggen van de niet-compatibele rijen. | &nbsp; | Nee |
| **linkedServiceName** | De gekoppelde service van Azure Storage voor het opslaan van het logboek dat overgeslagen rijen bevat. | De naam van een [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) gekoppelde service die naar de opslag-instantie die u gebruiken verwijst wilt voor het opslaan van het logboekbestand. | Nee |
| **pad** | Het pad van het logboekbestand dat overgeslagen rijen bevat. | Geef het pad voor Blob-opslag die u wilt gebruiken om de niet-compatibele gegevens te registreren. Als u niet een pad opgeeft, wordt in de service een container voor u gemaakt. | Nee |

## <a name="monitoring"></a>Bewaking
Nadat de kopieeractiviteit uitgevoerd is voltooid, ziet u het aantal overgeslagen rijen in de sectie bewaking:

![Monitor incompatibel rijen overgeslagen](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Als u voor de niet-compatibele rijen logboekregistratie hebt geconfigureerd, kunt u het logboekbestand op dit pad vinden: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In het logboekbestand kunt u de rijen die zijn overgeslagen en de hoofdoorzaak van deze incompatibiliteit bekijken.

Zowel de oorspronkelijke gegevens en de bijbehorende fout worden geregistreerd in het bestand. Een voorbeeld van de inhoud van het logboek-bestand is als volgt:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure Data Factory-Kopieeractiviteit, [verplaatsen van gegevens met behulp van de Kopieeractiviteit](data-factory-data-movement-activities.md).

---
title: Fouttolerantie toevoegen in de Kopieeractiviteit van Azure Data Factory door het overslaan van incompatibele rijen | Microsoft Docs
description: Informatie over het toevoegen van fouttolerantie in de Kopieeractiviteit van Azure Data Factory door het overslaan van incompatibele rijen tijdens het kopiëren van
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 44703a2547f685aaa0b6c583c39c08ef6a570d56
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016525"
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Fouttolerantie in de Kopieeractiviteit toevoegen door het overslaan van incompatibele rijen

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-copy-activity-fault-tolerance.md)
> * [Versie 2 (huidige versie)](../copy-activity-fault-tolerance.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [fouttolerantie in de kopieeractiviteit van Data Factory](../copy-activity-fault-tolerance.md).

Azure Data Factory [Kopieeractiviteit](data-factory-data-movement-activities.md) biedt twee manieren voor het afhandelen van incompatibele rijen tijdens het kopiëren van gegevens tussen de bron-en sink:

- U kunt afbreken en het kopiëren van een activiteit als niet-compatibele gegevens is opgetreden (standaardinstelling).
- U kunt doorgaan met het kopiëren alle gegevens die door toe te voegen fouttolerantie en niet-compatibele gegevensrijen wordt overgeslagen. Bovendien kunt u de niet-compatibele rijen registreren in Azure Blob-opslag. U kunt vervolgens het logboek voor informatie over de oorzaak van de fout, los van de gegevens op de gegevensbron en probeer het opnieuw met de kopieeractiviteit onderzoeken.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
Kopieeractiviteit ondersteunt drie scenario's voor het detecteren, wordt overgeslagen en logboekregistratie niet-compatibele gegevens:

- **Incompatibiliteit tussen het gegevenstype van de bron en het systeemeigen sink-type**

    Bijvoorbeeld: Gegevens uit een CSV-bestand in Blob-opslag kopiëren naar een SQL-database met de schemadefinitie van een met drie **INT** kolommen van het type. De CSV-bestand rijen die numerieke gegevens, zoals bevatten `123,456,789` is gekopieerd naar de sink-store. Echter, de rijen die niet-numerieke waarden bevatten, zoals `123,456,abc` als compatibel worden gedetecteerd en worden overgeslagen.

- **Het aantal kolommen tussen de bron en de sink komen niet overeen**

    Bijvoorbeeld: Gegevens uit een CSV-bestand in Blob-opslag kopiëren naar een SQL-database met de schemadefinitie van een dat zes kolommen bevat. De CSV-bestand-rijen met zes kolommen is gekopieerd naar de sink-store. De CSV-bestand rijen die meer of minder dan zes kolommen bevatten als niet-compatibele worden gedetecteerd en worden overgeslagen.

- **Schending van primaire sleutel bij het schrijven naar SQL Server/Azure SQL Database-/ Azure Cosmos DB**

    Bijvoorbeeld: Gegevens kopiëren van een SQL-server naar een SQL-database. Een primaire sleutel is gedefinieerd in de sink-SQL-database, maar die geen primaire sleutel is gedefinieerd in de bron-SQL-server. De dubbele rijen die zijn opgenomen in de bron kunnen niet worden gekopieerd naar de sink. Kopieeractiviteit kopieert alleen de eerste rij van de brongegevens in de gootsteen. De volgende bronrijen met de dubbele waarde voor de primaire sleutel zijn gedetecteerd als niet-compatibel en worden overgeslagen.

>[!NOTE]
>Deze functie is niet van toepassing wanneer copy-activiteit is geconfigureerd voor het aanroepen van externe gegevens laden met inbegrip van mechanisme [Azure SQL Data Warehouse PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) of [verwijderen van Amazon Redshift](data-factory-amazon-redshift-connector.md#use-unload-to-copy-data-from-amazon-redshift). Voor het laden van gegevens in SQL Data Warehouse met PolyBase, gebruikt u PolyBase van systeemeigen fouttolerantie ondersteuning door op te geven "[polyBaseSettings](data-factory-azure-sql-data-warehouse-connector.md#sqldwsink)' in de kopieeractiviteit.

## <a name="configuration"></a>Configuratie
Het volgende voorbeeld bevat een JSON-definitie voor het configureren van het overslaan van de niet-compatibele rijen in de Kopieeractiviteit:

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

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **enableSkipIncompatibleRow** | Schakel overslaan incompatibele rijen tijdens het kopiëren van of niet. | True<br/>False (standaard) | Nee |
| **redirectIncompatibleRowSettings** | Een groep met eigenschappen die kunnen worden opgegeven wanneer u wilt registreren van incompatibele rijen. | &nbsp; | Nee |
| **linkedServiceName** | De gekoppelde service van Azure Storage voor het opslaan van het logboek dat de overgeslagen rijen bevat. | De naam van een [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) of [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) gekoppelde service, die verwijst naar de opslag-instantie die u wilt gebruiken voor het opslaan van het logboekbestand. | Nee |
| **Pad** | Het pad van het logboekbestand dat overgeslagen rijen bevat. | Geef het pad op Blob-opslag die u wilt gebruiken om de niet-compatibele gegevens te registreren. Als u een pad opgeeft, wordt in de service een container voor u gemaakt. | Nee |

## <a name="monitoring"></a>Bewaking
Nadat de copy-activiteit uitvoeren is voltooid, ziet u het aantal overgeslagen rijen in de sectie bewaking:

![Monitor incompatibele rijen overgeslagen](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Als u configureert voor logboekregistratie van het incompatibele rijen, kunt u het logbestand op dit pad kunt vinden: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In het logboekbestand ziet u de rijen die zijn overgeslagen en de hoofdoorzaak van de niet-compatibel.

Zowel de oorspronkelijke gegevens en de bijbehorende fout worden geregistreerd in het bestand. Een voorbeeld van de inhoud van het log-bestand is als volgt:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure Data Factory-Kopieeractiviteit, [gegevens verplaatsen met behulp van Kopieeractiviteit](data-factory-data-movement-activities.md).

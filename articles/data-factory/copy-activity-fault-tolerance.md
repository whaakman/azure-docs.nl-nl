---
title: Fouttolerantie van de kopieeractiviteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over het toevoegen van fouttolerantie voor de kopieeractiviteit in Azure Data Factory door het overslaan van de niet-compatibele rijen.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: 293ffb2a56ae970c71d495d7d929720ddf758307
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Fouttolerantie van de kopieeractiviteit in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Versie 2 - Preview](copy-activity-fault-tolerance.md)

De kopieeractiviteit in Azure Data Factory biedt twee manieren om af te handelen incompatibel rijen bij het kopiëren van gegevens tussen de bron- en sink gegevensarchieven:

- U kunt afbreken en mislukt de kopie activiteit als niet-compatibele gegevens aangetroffen (standaardinstelling).
- U kunt doorgaan met het kopiëren alle gegevens door toe te voegen fouttolerantie en niet-compatibele gegevensrijen wordt overgeslagen. Bovendien kunt u de niet-compatibele rijen registreren in Azure Blob storage of Azure Data Lake Store. Bekijk vervolgens het logboek voor meer informatie over de oorzaak van de fout, los van de gegevens op de gegevensbron en probeer het opnieuw met de kopieerbewerking.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [activiteit fouttolerantie in V1 kopiëren](v1/data-factory-copy-activity-fault-tolerance.md).


 ## <a name="supported-scenarios"></a>Ondersteunde scenario's
Kopieeractiviteit ondersteunt drie scenario's voor het detecteren, wordt overgeslagen en logboekregistratie incompatibel gegevens:

- **Incompatibiliteit tussen het gegevensbrontype en het systeemeigen type sink**. <br/><br/> Bijvoorbeeld: gegevens kopiëren van een CSV-bestand in Blob storage naar een SQL-database met de schemadefinitie van een dat drie kolommen van het type INT bevat. De CSV-bestand rijen die numerieke gegevens, zoals 123.456.789 bevatten is gekopieerd naar de store sink. Echter, de rijen met niet-numerieke waarden, zoals 123,456, abc zijn gedetecteerd als niet compatibel en worden overgeslagen.
- **Het aantal kolommen tussen de bron en de sink komt niet overeen**. <br/><br/> Bijvoorbeeld: gegevens kopiëren van een CSV-bestand in Blob storage naar een SQL-database met de schemadefinitie van een dat zes kolommen bevat. De CSV-bestand-rijen met zes kolommen worden gekopieerd naar de store sink. De CSV-bestand rijen die minder dan zes of meer kolommen bevatten als niet compatibel zijn gedetecteerd en worden overgeslagen.
- **Primaire sleutel is geschonden bij het schrijven naar een relationele database**.<br/><br/> Bijvoorbeeld: gegevens kopiëren van een SQL-server naar een SQL-database. Een primaire sleutel is gedefinieerd in de sink SQL-database, maar die geen primaire sleutel is gedefinieerd in de bron-SQL-server. De dubbele rijen die in de bron voorkomen kunnen niet worden gekopieerd naar de sink. Alleen de eerste rij van de brongegevens Kopieeractiviteit gekopieerd naar de sink. De volgende bronrijen met dubbele primaire sleutelwaarde zijn gedetecteerd als niet compatibel en worden overgeslagen.

>[!NOTE]
>Deze functie is niet van toepassing wanneer kopieeractiviteit is geconfigureerd voor het aanroepen van externe gegevens laden met inbegrip van mechanisme [Azure SQL Data Warehouse PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) of [Amazon Redshift Unload](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift). Gebruiken om gegevens te laden in SQL Data Warehouse met PolyBase, PolyBase van systeemeigen fouttolerantie ondersteuning door te geven '[polyBaseSettings](connector-azure-sql-data-warehouse.md#azure-sql-data-warehouse-as-sink)' in de kopieerbewerking.

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
         "linkedServiceName": {
              "referenceName": "<Azure Storage or Data Lake Store linked service>",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Geeft aan of niet-compatibele rijen overslaan tijdens het kopiëren van of niet. | True<br/>False (standaard) | Nee
redirectIncompatibleRowSettings | Een groep met eigenschappen die kunnen worden opgegeven wanneer u wilt vastleggen van de niet-compatibele rijen. | &nbsp; | Nee
linkedServiceName | De gekoppelde service van [Azure Storage](connector-azure-blob-storage.md#linked-service-properties) of [Azure Data Lake Store](connector-azure-data-lake-store.md#linked-service-properties) voor het opslaan van het logboek dat overgeslagen rijen bevat. | De naam van een `AzureStorage` of `AzureDataLakeStore` type gekoppelde service, die naar het exemplaar dat u gebruiken verwijst wilt voor het opslaan van het logboekbestand. | Nee
pad | Het pad van het logboekbestand dat overgeslagen rijen bevat. | Geef het pad dat u wilt gebruiken om de niet-compatibele gegevens te registreren. Als u niet een pad opgeeft, wordt in de service een container voor u gemaakt. | Nee

## <a name="monitor-skipped-rows"></a>Monitor overgeslagen rijen
Nadat de kopieeractiviteit uitgevoerd is voltooid, ziet u het aantal overgeslagen rijen in de uitvoer van de kopieeractiviteit:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Als u voor de niet-compatibele rijen logboekregistratie hebt geconfigureerd, kunt u het logboekbestand op dit pad vinden: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

De logboekbestanden kunnen alleen worden de csv-bestanden. De oorspronkelijke gegevens wordt overgeslagen worden geregistreerd met door komma's als kolomscheidingsteken indien nodig. We twee of meer kolommen 'ErrorCode' en 'ErrorMessage' in extra toevoegen aan de oorspronkelijke brongegevens in het logboekbestand, waarin u de hoofdmap kunt zien oorzaak van de niet-compatibel. De ErrorCode en ErrorMessage wordt door dubbele aanhalingstekens worden vermeld. 

Een voorbeeld van de inhoud van het logboek-bestand is als volgt:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen voor de Kopieeractiviteit:

- [Activiteit-overzicht](copy-activity-overview.md)
- [Uitvoering van de activiteit](copy-activity-performance.md)



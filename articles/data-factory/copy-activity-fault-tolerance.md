---
title: Fouttolerantie van kopieeractiviteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over-fouttolerantie gebruikt voor het kopiëren van activiteit in Azure Data Factory door het overslaan van de niet-compatibele rijen toevoegen.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: jingwang
ms.openlocfilehash: f1a40c09c2d08eddedd3b6b51d2a138ec403f6bc
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014910"
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Fouttolerantie van kopieeractiviteit in Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Huidige versie](copy-activity-fault-tolerance.md)

De kopieeractiviteit in Azure Data Factory biedt twee manieren voor het afhandelen van incompatibele rijen tijdens het kopiëren van gegevens tussen de bron-en sink:

- U kunt afbreken en het kopiëren van een activiteit als niet-compatibele gegevens is opgetreden (standaardinstelling).
- U kunt doorgaan met het kopiëren alle gegevens die door toe te voegen fouttolerantie en niet-compatibele gegevensrijen wordt overgeslagen. Bovendien kunt u de niet-compatibele rijen registreren in Azure Blob storage of Azure Data Lake Store. U kunt vervolgens het logboek voor informatie over de oorzaak van de fout, los van de gegevens op de gegevensbron en probeer het opnieuw met de kopieeractiviteit onderzoeken.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
Kopieeractiviteit ondersteunt drie scenario's voor het detecteren, wordt overgeslagen en logboekregistratie niet-compatibele gegevens:

- **Incompatibiliteit tussen het gegevenstype van de bron en het systeemeigen sink-type**. 

    Bijvoorbeeld: Gegevens uit een CSV-bestand in Blob-opslag kopiëren naar een SQL-database met de schemadefinitie van een met drie kolommen van het gegevenstype INT. De rijen van de CSV-bestand die numerieke gegevens bevatten, zoals 123.456.789 is gekopieerd naar de sink-store. Echter, de rijen met niet-numerieke waarden, zoals 123,456, abc worden gedetecteerd als niet compatibel en worden overgeslagen.

- **Komt niet overeen in het aantal kolommen tussen de bron en de sink**.

    Bijvoorbeeld: Gegevens uit een CSV-bestand in Blob-opslag kopiëren naar een SQL-database met de schemadefinitie van een dat zes kolommen bevat. De CSV-bestand-rijen met zes kolommen is gekopieerd naar de sink-store. De CSV-bestand rijen die meer of minder dan zes kolommen bevatten als niet-compatibele worden gedetecteerd en worden overgeslagen.

- **Schending van primaire sleutel bij het schrijven naar SQL Server/Azure SQL Database-/ Azure Cosmos DB**.

    Bijvoorbeeld: Gegevens kopiëren van een SQL-server naar een SQL-database. Een primaire sleutel is gedefinieerd in de sink-SQL-database, maar die geen primaire sleutel is gedefinieerd in de bron-SQL-server. De dubbele rijen die zijn opgenomen in de bron kunnen niet worden gekopieerd naar de sink. Kopieeractiviteit kopieert alleen de eerste rij van de brongegevens in de gootsteen. De volgende bronrijen met de dubbele waarde voor de primaire sleutel zijn gedetecteerd als niet-compatibel en worden overgeslagen.

>[!NOTE]
>- Voor het laden van gegevens in SQL Data Warehouse PolyBase van systeemeigen fouttolerantie-instellingen met PolyBase, configureren door op te geven afwijzen beleid via '[polyBaseSettings](connector-azure-sql-data-warehouse.md#azure-sql-data-warehouse-as-sink)' in de kopieeractiviteit. U kunt nog steeds omleiding PolyBase incompatibele rijen Blob of ADLS die normaal werken zoals hieronder wordt weergegeven.
>- Deze functie is niet van toepassing wanneer de kopieeractiviteit is geconfigureerd om aan te roepen [verwijderen van Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).


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
         "linkedServiceName": {
              "referenceName": "<Azure Storage or Data Lake Store linked service>",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Geeft aan of om over te slaan van incompatibele rijen tijdens het kopiëren van of niet. | True<br/>False (standaard) | Nee
redirectIncompatibleRowSettings | Een groep met eigenschappen die kunnen worden opgegeven wanneer u wilt registreren van incompatibele rijen. | &nbsp; | Nee
linkedServiceName | De gekoppelde service van [Azure Storage](connector-azure-blob-storage.md#linked-service-properties) of [Azure Data Lake Store](connector-azure-data-lake-store.md#linked-service-properties) voor het opslaan van het logboek dat de overgeslagen rijen bevat. | De naam van een `AzureStorage` of `AzureDataLakeStore` gekoppelde service, die verwijst naar het exemplaar dat u wilt gebruiken voor het opslaan van het logboekbestand typt. | Nee
pad | Het pad van het logboekbestand dat overgeslagen rijen bevat. | Geef het pad dat u wilt gebruiken om de niet-compatibele gegevens te registreren. Als u een pad opgeeft, wordt in de service een container voor u gemaakt. | Nee

## <a name="monitor-skipped-rows"></a>Monitor overgeslagen rijen
Nadat de copy-activiteit uitvoeren is voltooid, ziet u het aantal overgeslagen rijen in de uitvoer van de kopieeractiviteit:

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
Als u configureert voor logboekregistratie van het incompatibele rijen, kunt u het logbestand op dit pad vinden: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

De logboekbestanden kunnen alleen worden voor de csv-bestanden. De oorspronkelijke gegevens wordt overgeslagen worden vastgelegd met door komma's als kolomscheidingsteken indien nodig. We twee of meer kolommen "ErrorCode" en "Foutbericht" in extra toevoegen aan de oorspronkelijke brongegevens in een logboekbestand, waarin u de basis kunt zien oorzaak van de niet-compatibel. De foutcode en het foutbericht wordt door dubbele aanhalingstekens worden vermeld. 

Een voorbeeld van de inhoud van het log-bestand is als volgt:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen van de Kopieeractiviteit:

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Prestaties van de kopieeractiviteit](copy-activity-performance.md)



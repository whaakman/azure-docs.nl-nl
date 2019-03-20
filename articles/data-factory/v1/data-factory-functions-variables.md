---
title: Data Factory-functies en systeemvariabelen | Microsoft Docs
description: Geeft een lijst van Azure Data Factory-functies en systeemvariabelen
documentationcenter: ''
author: sharonlo101
manager: craigg
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 5293c44a3e4494593e069ab45fbc38806c6999ee
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57976773"
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - functies en systeemvariabelen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [systeemvariabelen in Data Factory](../control-flow-system-variables.md).

Dit artikel bevat informatie over functies en variabelen die worden ondersteund door Azure Data Factory.

## <a name="data-factory-system-variables"></a>Data Factory-systeemvariabelen

| De naam van variabele | Description | Bereik van het object | JSON-bereik en Use-Cases |
| --- | --- | --- | --- |
| WindowStart |Begin van het tijdsinterval voor het huidige venster de uitvoering van activiteiten |activiteit |<ol><li>Geef gegevensselectiequery's. Zie de artikelen van connector waarnaar wordt verwezen in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel.</li> |
| WindowEnd |Einde van het tijdsinterval voor het huidige venster de uitvoering van activiteiten |activiteit |hetzelfde als WindowStart. |
| SliceStart |Begin van het tijdsinterval voor het segment wordt geproduceerd |activiteit<br/>Gegevensset |<ol><li>Geef dynamische paden en bestandsnamen tijdens het werken met [Azure Blob](data-factory-azure-blob-connector.md) en [bestandssysteem gegevenssets](data-factory-onprem-file-system-connector.md).</li><li>Invoer afhankelijkheden met data factory-functies in activiteit invoer verzameling opgeven.</li></ol> |
| SliceEnd |Einde van de tijdsinterval voor het huidige gegevenssegment. |activiteit<br/>Gegevensset |hetzelfde als de slicestart-waarde. |

> [!NOTE]
> Op dit moment vereist gegevensfactory dat de planning die is opgegeven in de activiteit exact overeenkomt met het schema dat is opgegeven in de beschikbaarheid van de uitvoergegevensset. Daarom WindowStart, WindowEnd, en SliceStart en SliceEnd altijd toegewezen aan dezelfde periode en een uitvoersegment één.
> 

### <a name="example-for-using-a-system-variable"></a>Voorbeeld voor het gebruik van een systeemvariabele
In het volgende voorbeeld, jaar, maand, dag en tijd van **SliceStart** zijn uitgepakt naar afzonderlijke variabelen die worden gebruikt door **folderPath** en **fileName** eigenschappen.

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a>Data Factory-functies
U kunt functies gebruiken in data factory, samen met de systeemvariabelen voor de volgende doeleinden:

1. Gegevensselectiequery's op te geven (Zie connector artikelen waarnaar wordt verwezen door de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel.
   
   De syntaxis voor het aanroepen van een data factory-functie is: **$$ <function>** voor gegevensselectiequery's en andere eigenschappen in de activiteiten en gegevenssets.  
2. Invoer afhankelijkheden met data factory-functies in activiteit invoer verzameling op te geven.
   
    $ is niet nodig voor het opgeven van invoer afhankelijkheid expressies.     

In het volgende voorbeeld, **sqlReaderQuery** eigenschap in een JSON-bestand is toegewezen aan een waarde die wordt geretourneerd door de `Text.Format` functie. In dit voorbeeld wordt ook een systeemvariabele met de naam **WindowStart**, die staat voor de begintijd van het venster van de activiteit die wordt uitgevoerd.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Zie [aangepaste datum en tijd opmaaktekenreeksen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) onderwerp waarin wordt beschreven verschillende opmaakopties die u kunt gebruiken (bijvoorbeeld: ay versus jjjj). 

### <a name="functions"></a>Functions
De volgende tabellen worden de functies in Azure Data Factory:

| Categorie | Function | Parameters | Description |
| --- | --- | --- | --- |
| Time |AddHours(X,Y) |X: DateTime <br/><br/>Y: int |Y-uren toevoegt aan de opgegeven tijd X. <br/><br/>Voorbeeld: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Time |AddMinutes(X,Y) |X: DateTime <br/><br/>Y: int |X van Y minuten toevoegen<br/><br/>Voorbeeld: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Time |StartOfHour(X) |X: Datum en tijd |Hiermee haalt u de begintijd voor het uur wordt vertegenwoordigd door het uurgedeelte van X. <br/><br/>Voorbeeld: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Date |AddDays(X,Y) |X: DateTime<br/><br/>Y: int |Y dagen toevoegt aan X. <br/><br/>Voorbeeld: 9/15/2013 12:00:00 uur + 2 dagen = 9/17 //build/ 2013 12:00:00 PM.<br/><br/>U kunt ook dagen aftrekken door Y op te geven als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Date |AddMonths(X,Y) |X: DateTime<br/><br/>Y: int |X van Y maanden toevoegen<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>U kunt maanden te aftrekken door Y op te geven als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Date |AddQuarters(X,Y) |X: DateTime <br/><br/>Y: int |Voegt Y * 3 maanden op X.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Date |AddWeeks(X,Y) |X: DateTime<br/><br/>Y: int |Voegt Y * 7 dagen X<br/><br/>Voorbeeld: 9/15/2013 12:00:00 uur + 1 week = 9/22 //build/ 2013 12:00:00 PM<br/><br/>U kunt weken te aftrekken door Y op te geven als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Date |AddYears(X,Y) |X: DateTime<br/><br/>Y: int |X van Y jaar toevoegen<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>U kunt jaar te aftrekken door Y op te geven als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Date |Day(X) |X: DateTime |Haalt het daggedeelte van X.<br/><br/>Voorbeeld: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Date |DayOfWeek(X) |X: DateTime |Hiermee haalt u het onderdeel dag van week van X.<br/><br/>Voorbeeld: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Date |DayOfYear(X) |X: DateTime |Hiermee haalt u de dag van het jaar wordt vertegenwoordigd door het jaargedeelte van X.<br/><br/>Voorbeelden:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Date |DaysInMonth(X) |X: DateTime |Hiermee haalt u de dagen van de maand wordt vertegenwoordigd door het maandgedeelte van de parameter X.<br/><br/>Voorbeeld: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`. |
| Date |EndOfDay(X) |X: DateTime |Hiermee haalt u de datum / tijd die het einde van de dag (dagonderdeel) x vertegenwoordigt.<br/><br/>Voorbeeld: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Date |EndOfMonth(X) |X: DateTime |Hiermee haalt u het einde van de maand wordt vertegenwoordigd door het maandgedeelte van de parameter X. <br/><br/>Voorbeeld: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum en tijd die het einde van de maand September) |
| Date |StartOfDay(X) |X: DateTime |Hiermee haalt u het begin van de dag wordt vertegenwoordigd door het dagonderdeel van de parameter X.<br/><br/>Voorbeeld: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| DateTime |FROM(X) |X: String |Parseren van tekenreeks X naar een datum-tijd. |
| DateTime |Ticks(X) |X: DateTime |Hiermee haalt u de ticks eigenschap van de X-parameter. Een tick is gelijk aan 100 nanoseconden. De waarde van deze eigenschap geeft het aantal maatstreepjes dat is verstreken sinds 12:00:00 middernacht, 1 januari 0001. |
| Tekst |Format(X) |X: String-variabele |Hiermee wordt de tekst (Gebruik `\\'` combinatie als escape voor `'` tekens).|

> [!IMPORTANT]
> Wanneer u een functie binnen een andere functie gebruikt, u niet wilt gebruiken **$$** voorvoegsel voor de binnenste functie. Bijvoorbeeld: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' en RowKey ge \\' {0: DD-MM-jjjj uu: mm:}\\'', Time.AddHours (slicestart-waarde -6)). In dit voorbeeld ziet u dat **$$** voorvoegsel wordt niet gebruikt voor de **Time.AddHours** functie. 

#### <a name="example"></a>Voorbeeld
In het volgende voorbeeld wordt de invoer- en parameters voor de Hive-activiteit worden bepaald met behulp van de `Text.Format` functie en de systeemvariabele slicestart-waarde. 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a>Voorbeeld 2

In het volgende voorbeeld wordt de datum/tijd-parameter voor de activiteit opgeslagen Procedure bepaald met behulp van de tekst. Functie en de variabele van de slicestart-waarde-indeling. 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "usp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a>Voorbeeld 3
Gebruik de functie AddDays zoals wordt weergegeven in het volgende voorbeeld om te lezen van gegevens uit de vorige dag in plaats van dag wordt vertegenwoordigd door de slicestart-waarde: 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
                        "Month": "$$Text.Format('{0:MM}',WindowStart)",
                        "Day": "$$Text.Format('{0:dd}',WindowStart)"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 2,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

Zie [aangepaste datum en tijd opmaaktekenreeksen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) onderwerp waarin wordt beschreven verschillende opmaakopties die u kunt gebruiken (bijvoorbeeld: jj versus jjjj). 


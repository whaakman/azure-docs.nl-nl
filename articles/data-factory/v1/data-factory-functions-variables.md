---
title: Data Factory-functies en systeemvariabelen | Microsoft Docs
description: Geeft een lijst van Azure Data Factory-functies en systeemvariabelen
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: f42ba7ed9c07a9d0bc73929db2a095248ad7d56f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - functies en systeemvariabelen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [systeemvariabelen in Data Factory versie 2](../control-flow-system-variables.md).

Dit artikel bevat informatie over functies en variabelen die worden ondersteund door Azure Data Factory.

## <a name="data-factory-system-variables"></a>Data Factory-systeemvariabelen
| Naam variabele | Beschrijving | Bereik van het object | JSON-bereik en gebruiksvoorbeelden |
| --- | --- | --- | --- |
| WindowStart |Begin van het tijdsinterval voor de huidige activiteit venster uitvoeren |activiteit |<ol><li>Geef op query's voor selectie. Zie connector artikelen waarnaar wordt verwezen in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel.</li> |
| WindowEnd |Einde van het tijdsinterval voor de huidige activiteit venster uitvoeren |activiteit |hetzelfde als WindowStart. |
| SliceStart |Begin van het tijdsinterval voor het segment wordt geproduceerd |activiteit<br/>dataset |<ol><li>Geef dynamische paden en bestandsnamen tijdens het werken met [Azure Blob](data-factory-azure-blob-connector.md) en [bestandssysteem gegevenssets](data-factory-onprem-file-system-connector.md).</li><li>Invoer afhankelijkheden data factory-functies in activiteit invoer verzameling opgeven.</li></ol> |
| SliceEnd |Einde van de tijdsinterval voor het huidige segment. |activiteit<br/>dataset |hetzelfde als SliceStart. |

> [!NOTE]
> Op dit moment vereist gegevensfactory dat de planning die is opgegeven in de activiteit exact overeenkomt met het schema dat is opgegeven in de beschikbaarheid van de uitvoergegevensset. Daarom WindowStart, WindowEnd, en SliceStart en SliceEnd altijd toegewezen aan dezelfde periode en een segment één uitvoer.
> 

### <a name="example-for-using-a-system-variable"></a>Voorbeeld voor het gebruik van een systeemvariabele
In het volgende voorbeeld, jaar, maand, dag en tijd van **SliceStart** worden uitgepakt in verschillende variabelen die worden gebruikt door **folderPath** en **fileName** eigenschappen.

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
U kunt functies gebruiken in gegevensfactory samen met de variabelen voor de volgende doeleinden:

1. Query's voor selectie opgeven (Zie connector artikelen waarnaar wordt verwezen door de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel.
   
   De syntaxis voor het aanroepen van een data factory-functie is:  **$$ <function>**  voor selectie van query's en andere eigenschappen in de activiteit en gegevenssets.  
2. Invoer afhankelijkheden opgeven data factory-functies in activiteit invoer verzameling.
   
    $$ is niet nodig voor het opgeven van invoer afhankelijkheid expressies.     

In het volgende voorbeeld, **sqlReaderQuery** eigenschap in een JSON-bestand is toegewezen aan een waarde die is geretourneerd door de `Text.Format` functie. Dit voorbeeld gebruikt ook een systeemvariabele met de naam **WindowStart**, die staat voor de begintijd van het venster van de activiteit die wordt uitgevoerd.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Zie [aangepaste datum en tijd-indeling tekenreeksen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) onderwerp dat beschrijft de verschillende opmaakopties die u kunt gebruiken (bijvoorbeeld: ay versus jjjj). 

### <a name="functions"></a>Functies
De volgende tabellen worden de functies in Azure Data Factory:

| Category | Functie | Parameters | Beschrijving |
| --- | --- | --- | --- |
| Time |AddHours(X,Y) |X: datum/tijd <br/><br/>Y: int |Y uren toevoegt aan de opgegeven tijd X. <br/><br/>Voorbeeld:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Time |AddMinutes(X,Y) |X: datum/tijd <br/><br/>Y: int |Voegt Y minuten tot en met X.<br/><br/>Voorbeeld:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Time |StartOfHour(X) |X: datum/tijd |Hiermee haalt u de begintijd voor het uur dat wordt vertegenwoordigd door het uurgedeelte van X. <br/><br/>Voorbeeld:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Date |AddDays(X,Y) |X: datum/tijd<br/><br/>Y: int |Voegt Y dagen tot en met X. <br/><br/>Voorbeeld: 9, 15/2013 12:00:00 PM + 2 dagen = 9/17/2013 12:00:00 PM.<br/><br/>U kunt dagen te aftrekken door te geven Y als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Date |AddMonths(X,Y) |X: datum/tijd<br/><br/>Y: int |Voegt Y maanden tot en met X.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>U kunt maanden te aftrekken door te geven Y als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Date |AddQuarters(X,Y) |X: datum/tijd <br/><br/>Y: int |Voegt Y * X 3 maanden.<br/><br/>Voorbeeld:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Date |AddWeeks(X,Y) |X: datum/tijd<br/><br/>Y: int |Voegt Y * X 7 dagen<br/><br/>Voorbeeld: 9, 15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 uur<br/><br/>U kunt de weken te aftrekken door te geven Y als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Date |AddYears(X,Y) |X: datum/tijd<br/><br/>Y: int |Voegt Y jaar tot en met X.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>U kunt jaar te aftrekken door te geven Y als een negatief getal.<br/><br/>Voorbeeld: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Date |Day(X) |X: datum/tijd |Hiermee haalt u het daggedeelte van X.<br/><br/>Voorbeeld: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Date |DayOfWeek(X) |X: datum/tijd |Hiermee haalt u de dag van het onderdeel van de week van X.<br/><br/>Voorbeeld: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Date |DayOfYear(X) |X: datum/tijd |Hiermee haalt u de dag van het jaar, uitgedrukt in het jaargedeelte van X.<br/><br/>Voorbeelden:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Date |DaysInMonth(X) |X: datum/tijd |Hiermee haalt u de dagen van de maand dat wordt vertegenwoordigd door het maandgedeelte van de parameter X.<br/><br/>Voorbeeld: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`. |
| Date |EndOfDay(X) |X: datum/tijd |Hiermee haalt u de datum-tijd die het einde van de dag (daggedeelte) van X.<br/><br/>Voorbeeld: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Date |EndOfMonth(X) |X: datum/tijd |Het einde van de maand dat wordt vertegenwoordigd door maandgedeelte van de parameter X opgehaald. <br/><br/>Voorbeeld: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum / tijd die het einde van de maand September vertegenwoordigt) |
| Date |StartOfDay(X) |X: datum/tijd |Hiermee haalt u het begin van de dag dat wordt vertegenwoordigd door het daggedeelte van de parameter X.<br/><br/>Voorbeeld: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| Datum en tijd |FROM(X) |X: de tekenreeks |Parseren van tekenreeks X naar een datum-tijd. |
| Datum en tijd |Ticks(X) |X: datum/tijd |Hiermee haalt u de maatstreepjes eigenschap van de parameter X. Eén tik is gelijk aan 100 nanoseconden. De waarde van deze eigenschap geeft het aantal maatstreepjes dat is verstreken sinds 12:00:00 middernacht, 1 januari 0001. |
| Tekst |Format(X) |X: tekenreeksvariabele |Hiermee maakt u de tekst (Gebruik `\\'` combinatie escape `'` teken).|

> [!IMPORTANT]
> Wanneer u een functie binnen een andere functie, hoeft u niet te gebruiken  **$$**  voorvoegsel voor de interne functie. Bijvoorbeeld: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' en RowKey ge \\' {0: jjjj-MM-dd: mm: SS}\\'', Time.AddHours (SliceStart -6)). U ziet dat in dit voorbeeld  **$$**  voorvoegsel wordt niet gebruikt voor de **Time.AddHours** functie. 

#### <a name="example"></a>Voorbeeld
In het volgende voorbeeld invoer- en parameters voor de Hive-activiteit worden bepaald met behulp van de `Text.Format` functie en SliceStart systeemvariabele. 

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

In het volgende voorbeeld wordt de datum/tijd-parameter voor de activiteit opgeslagen Procedure met behulp van de tekst bepaald. Functie en de variabele SliceStart-indeling. 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
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
Als u wilt gegevens lezen uit de vorige dag in plaats van de dag dat wordt vertegenwoordigd door de SliceStart, gebruikt u de functie AddDays zoals weergegeven in het volgende voorbeeld: 

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

Zie [aangepaste datum en tijd-indeling tekenreeksen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) onderwerp dat beschrijft de verschillende opmaakopties die u kunt gebruiken (bijvoorbeeld: jj versus jjjj). 


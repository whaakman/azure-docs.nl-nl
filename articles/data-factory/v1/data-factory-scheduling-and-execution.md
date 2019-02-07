---
title: Plannen en uitvoeren met Data Factory | Microsoft Docs
description: Meer informatie over plannings- en uitvoeringsaspecten van Azure Data Factory-toepassingsmodel.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2d7fc45faf1fb77c7d9181e5a2419096dd1ad0f1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55817417"
---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory plannen en uitvoeren
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [pijplijnen uitvoeren en triggers](../concepts-pipeline-execution-triggers.md) artikel.

In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. In dit artikel wordt ervan uitgegaan dat u van Data Factory toepassing model concepten basisbeginselen, met inbegrip van activiteiten, pijplijnen, gekoppelde services en gegevenssets. Zie voor de basisconcepten van Azure Data Factory, de volgende artikelen:

* [Inleiding tot Data Factory](data-factory-introduction.md)
* [Pijplijnen](data-factory-create-pipelines.md)
* [Gegevenssets](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Begin- en eindtijden van pijplijn
Een pijplijn is actief waren alleen tussen de **start** tijd en **end** tijd. Het is niet uitgevoerd voor de begintijd liggen of na de eindtijd. Als de pijplijn is onderbroken, is het niet uitgevoerd, ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. U vindt deze instellingen (starten, beëindigen, onderbroken) in de pijplijndefinitie van de: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Zie voor meer informatie deze eigenschappen [pijplijnen maken](data-factory-create-pipelines.md) artikel. 


## <a name="specify-schedule-for-an-activity"></a>Planning voor een activiteit opgeven
Het is niet de pijplijn die wordt uitgevoerd. Het is de activiteiten in de pijplijn die worden uitgevoerd in de context van de pijplijn. U kunt een terugkerend schema voor een activiteit opgeven met behulp van de **scheduler** sectie van de JSON van de activiteit. U kunt bijvoorbeeld plannen om uit te voeren per uur als volgt:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Zoals in het volgende diagram wordt weergegeven, op te geven dat een schema voor een activiteit wordt een reeks van windows met tumbling in de pijplijn- en eindtijd. Tumblingvensters bestaan uit een reeks van vaste grootte niet-overlappende, aaneengesloten tijdsintervallen. Deze logische tumblingvensters voor een activiteit heten **activiteitsvensters**.

![Voorbeeld van de activiteit scheduler](media/data-factory-scheduling-and-execution/scheduler-example.png)

De **scheduler** eigenschap voor een activiteit is optioneel. Als u deze eigenschap opgeeft, moet deze overeenkomen met de frequentie die u in de definitie van de uitvoergegevensset voor de activiteit opgeeft. Momenteel is de uitvoergegevensset dat wat de planning aanstuurt. Daarom moet u een uitvoergegevensset maken, zelfs als de activiteit geen uitvoer wordt geproduceerd. 

## <a name="specify-schedule-for-a-dataset"></a>Geef de planning voor een gegevensset
Een activiteit in een Data Factory-pijplijn kan nul of meer duren **gegevenssets** en een of meer uitvoergegevenssets produceren. Voor een activiteit, kunt u de frequentie waarmee de ingevoerde gegevens beschikbaar is of de uitvoergegevens wordt gemaakt met behulp van de **beschikbaarheid** sectie in de definities van de gegevensset. 

**Frequentie** in de **beschikbaarheid** sectie Hiermee geeft u de tijdseenheid. De toegestane waarden voor de frequentie zijn: Minuut, uur, dag, Week en maand. De **interval** eigenschap in de beschikbaarheidssectie geeft u een vermenigvuldiger voor de frequentie. Bijvoorbeeld: als de frequentie is ingesteld op de dag en interval is ingesteld op 1 voor een uitvoergegevensset, de uitvoergegevens dagelijks wordt geproduceerd. Als u de frequentie als minuut opgeeft, wordt u aangeraden dat u het interval ingesteld op niet kleiner zijn dan 15. 

In het volgende voorbeeld wordt de ingevoerde gegevens beschikbaar is per uur en de uitvoergegevens uur wordt gemaakt (`"frequency": "Hour", "interval": 1`). 

**Invoergegevensset:** 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


**Uitvoergegevensset**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Op dit moment **uitvoergegevensset stuurt het schema**. Met andere woorden, wordt de planning die is opgegeven voor de uitvoergegevensset die gebruikt om uit te voeren van een activiteit tijdens runtime. Daarom moet u een uitvoergegevensset maken, zelfs als de activiteit geen uitvoer wordt geproduceerd. Als er voor de activiteit geen invoer nodig is, kunt u het maken van de invoergegevensset overslaan. 

In de volgende pijplijndefinitie van de de **scheduler** eigenschap wordt gebruikt om op te geven van de planning voor de activiteit. Deze eigenschap is optioneel. Op dit moment de planning voor de uitvoergegevensset opgegeven moet overeenkomen met het schema voor de activiteit.
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

In dit voorbeeld wordt de activiteit uitgevoerd per uur tussen de begin- en eindtijden van de pijplijn. De uitvoergegevens wordt uur gemaakt voor drie uur windows (8 AM - 9 uur, 9: 00 - 10 uur, en 10: 00 - 11 uur). 

Elke eenheid van de gegevens die worden gebruikt of die worden geproduceerd door een activiteit die wordt uitgevoerd, heet een **gegevenssegment**. Het volgende diagram toont een voorbeeld van een activiteit met één invoergegevensset en één uitvoergegevensset: 

![Beschikbaarheid van scheduler](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Het diagram toont de Uurlijkse gegevenssegmenten voor de gegevensset voor invoer en uitvoer. Het diagram toont de drie invoersegmenten die gereed voor verwerking zijn. De activiteit 10-11 uur wordt uitgevoerd, het uitvoersegment 10-11 uur produceren. 

U kunt toegang tot het tijdsinterval die zijn gekoppeld aan het huidige segment in de gegevensset JSON met behulp van variabelen: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) en [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). Op deze manier kunt u het interval dat is gekoppeld aan een activiteitsvenster met behulp van de WindowStart en WindowEnd openen. De planning van een activiteit moet overeenkomen met het schema van de uitvoergegevensset voor de activiteit. Daarom de SliceStart en SliceEnd waarden zijn hetzelfde als waarden WindowStart en WindowEnd respectievelijk. Zie voor meer informatie over deze variabelen [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables) artikelen.  

U kunt deze variabelen gebruiken voor verschillende doeleinden in de JSON van de activiteit. Bijvoorbeeld, kunt u deze gegevens selecteren uit de invoer- en uitvoergegevenssets voor time series-gegevens (bijvoorbeeld: 8 AM tot en met 9 uur). In dit voorbeeld gebruikt ook **WindowStart** en **WindowEnd** selecteren relevante gegevens voor een activiteit uitvoeren en kopieer het naar een blob met de juiste **folderPath**. De **folderPath** met parameters om een afzonderlijke map voor elk uur.  

In het voorgaande voorbeeld wordt het schema is opgegeven voor de invoer en uitvoergegevenssets is de dezelfde (elk uur). Als de invoergegevensset voor de activiteit beschikbaar op een andere frequentie, bijvoorbeeld om de 15 minuten is, de activiteit die deze uitvoergegevensset produceert eenmaal per uur nog steeds uitgevoerd als de uitvoergegevensset is wat de activiteitenplanning aanstuurt. Zie voor meer informatie, [Model gegevenssets met verschillende frequenties](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>Beschikbaarheid van gegevenssets en het beleid
U hebt gezien dat het gebruik van de frequentie en interval eigenschappen in de sectie van de beschikbaarheid van de definitie van de gegevensset. Er zijn een paar andere eigenschappen die invloed hebben op de planning en uitvoering van een activiteit. 

### <a name="dataset-availability"></a>Beschikbaarheid van gegevenssets 
De volgende tabel beschrijft de eigenschappen die u kunt gebruiken in de **beschikbaarheid** sectie:

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| frequency |Hiermee geeft u de tijdseenheid voor de gegevensset segment productie.<br/><br/><b>Ondersteunde frequentie</b>: Minuut, uur, dag, Week, maand |Ja |N.v.t. |
| interval |Hiermee geeft u een vermenigvuldiger voor de frequentie<br/><br/>"X Synchronisatiefrequentie-interval" bepaalt hoe vaak het segment wordt geproduceerd.<br/><br/>Als u de gegevensset om te worden gesegmenteerd op uurbasis, stelt u <b>frequentie</b> naar <b>uur</b>, en <b>interval</b> naar <b>1</b>.<br/><br/><b>Opmerking</b>: Als u de frequentie als minuut opgeeft, wordt aangeraden dat u het interval ingesteld op niet kleiner zijn dan 15 |Ja |N.v.t. |
| stijl |Hiermee geeft u op of het segment aan het begin/einde van het interval moet worden gemaakt.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Als Frequency wordt ingesteld op maand en stijl is ingesteld op EndOfInterval, wordt het segment op de laatste dag van maand geproduceerd. Als de stijl is ingesteld op StartOfInterval, wordt het segment op de eerste dag van maand geproduceerd.<br/><br/>Als Frequency wordt ingesteld op de dag en stijl is ingesteld op EndOfInterval, kan het segment in het afgelopen uur van de dag wordt geproduceerd.<br/><br/>Als Frequency wordt ingesteld op uur en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd aan het einde van het uur. Het segment wordt bijvoorbeeld voor een segment voor 1-2 uur periode worden geproduceerd om 2 uur. |Nee |EndOfInterval |
| anchorDateTime |Hiermee definieert u de absolute positie in de tijd die door de scheduler wordt gebruikt voor het berekenen van de grenzen van de gegevensset-segment. <br/><br/><b>Opmerking</b>: De gedetailleerdere onderdelen worden genegeerd als de AnchorDateTime heeft de datumonderdelen die gedetailleerder zijn dan de frequentie zijn. <br/><br/>Bijvoorbeeld, als de <b>interval</b> is <b>per uur</b> (frequentie: uur en interval: (1) en de <b>AnchorDateTime</b> bevat <b>minuten en seconden</b>, dan zal de <b>minuten en seconden</b> onderdelen van de AnchorDateTime worden genegeerd. |Nee |01/01/0001 |
| offset |TimeSpan waarmee het begin en einde van alle segmenten van de gegevensset worden verschoven. <br/><br/><b>Opmerking</b>: Als zowel anchorDateTime en offset zijn opgegeven, is het resultaat van de gecombineerde verschuiving. |Nee |N.v.t. |

### <a name="offset-example"></a>Voorbeeld van de offset
Standaard dagelijks (`"frequency": "Day", "interval": 1`) segmenten beginnen bij 12: 00 uur UTC-tijd (middernacht). Als u de begintijd moet 6 AM UTC-tijd in plaats daarvan wilt, stelt u de verschuiving zoals wordt weergegeven in het volgende codefragment: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Voorbeeld van de anchorDateTime
In het volgende voorbeeld wordt de elke 23 uur in de gegevensset wordt geproduceerd. Het eerste segment wordt gestart op het moment dat is opgegeven door de anchorDateTime, die is ingesteld op `2017-04-19T08:00:00` (UTC-tijd).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>Voorbeeld van de offset/stijl
De volgende gegevensset is een maandelijkse gegevensset en 3rd van elke maand om 8:00 uur wordt geproduceerd (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>Beleid voor gegevensset
Een gegevensset kan een validatiebeleid gedefinieerd waarmee wordt aangegeven hoe de gegevens die worden gegenereerd door de uitvoering van een segment kan worden gevalideerd voordat deze klaar voor verbruik is hebben. In dergelijke gevallen kan dat wanneer het segment is voltooid, wordt uitgevoerd, de uitvoer segment de status wordt gewijzigd naar **wachten** met een substatus van **validatie**. Als de segmenten worden gevalideerd, wordt het de segmentstatus verandert in **gereed**. Als een gegevenssegment is geproduceerd, maar heeft de validatie niet doorstaan, worden de uitvoeringen van activiteit downstream segmenten die afhankelijk van dit segment zijn niet verwerkt. [Pijplijnen bewaken en beheren](data-factory-monitor-manage-pipelines.md) bevat informatie over de verschillende statussen van gegevenssegmenten in Data Factory.

De **beleid** sectie in het definitie van de gegevensset worden gedefinieerd de criteria of de voorwaarde die moeten voldoen aan de gegevensset segmenten. De volgende tabel beschrijft de eigenschappen die u kunt gebruiken in de **beleid** sectie:

| Policy Name | Description | Toegepast op | Vereist | Standaard |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Valideert dat de gegevens in een **Azure blob** voldoet aan de minimale grootte (in MB). |Azure Blob |Nee |N.v.t. |
| minimumRows | Valideert dat de gegevens in een **Azure SQL-database** of een **Azure table** het minimum aantal rijen bevat. |<ul><li>Azure SQL Database</li><li>Azure Table</li></ul> |Nee |N.v.t. |

#### <a name="examples"></a>Voorbeelden
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

Zie voor meer informatie over deze eigenschappen en voorbeelden, [gegevenssets](data-factory-create-datasets.md) artikel. 

## <a name="activity-policies"></a>Activiteitenbeleid
Beleid van invloed op het runtimegedrag van een activiteit, specifiek wanneer het segment de status van een tabel wordt verwerkt. De volgende tabel bevat de details.

| Eigenschap | Toegestane waarden | Standaardwaarde | Description |
| --- | --- | --- | --- |
| gelijktijdigheid van taken |Geheel getal <br/><br/>De maximale waarde: 10 |1 |Het aantal gelijktijdige uitvoeringen van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. Bijvoorbeeld, als een activiteit moet doorlopen versnelt een groot aantal beschikbare gegevens, met een grotere waarde voor gelijktijdigheid de verwerking van gegevens. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Bepaalt de volgorde van de gegevenssegmenten dat wordt verwerkt.<br/><br/>Als er 2 segmenten (één gebeurt: 00 uur en een andere naam om 17: 00) en beide uitvoering in behandeling zijn. Als u de executionPriorityOrder NewestFirst worden ingesteld, wordt het segment om 17: 00 eerst verwerkt. Op dezelfde manier als u de executionPriorityORder OldestFIrst worden ingesteld, klikt u vervolgens het segment: 00 uur is verwerkt. |
| retry |Geheel getal<br/><br/>De maximale waarde is 10 |0 |Het aantal nieuwe pogingen voordat de verwerking van gegevens voor het segment is gemarkeerd als fout. De uitvoeringsomgeving van de activiteit van een gegevenssegment is opnieuw uitgevoerd tot het opgegeven aantal nieuwe pogingen. De nieuwe poging wordt gedaan zo snel mogelijk na de fout. |
| timeout |TimeSpan |00:00:00 |Een time-out opgetreden voor de activiteit. Voorbeeld: 00:10:00 (impliceert time-out 10 minuten)<br/><br/>Als een waarde niet opgegeven is of is ingesteld op 0, wordt de time-out is oneindig.<br/><br/>Als de verwerkingstijd van de gegevens op een segment de time-outwaarde overschrijdt, wordt geannuleerd en het systeem probeert om opnieuw te proberen de verwerking. Het aantal nieuwe pogingen, is afhankelijk van de eigenschap opnieuw proberen. Wanneer een time-out optreedt, wordt de status is ingesteld op time-out. |
| vertraging |TimeSpan |00:00:00 |Geef de vertraging voor gegevensverwerking van het segment wordt gestart.<br/><br/>De uitvoering van activiteit van een gegevenssegment wordt gestart nadat de vertraging de verwachte tijd voor uitvoering in het verleden is.<br/><br/>Voorbeeld: 00:10:00 (betekent vertraging van 10 minuten) |
| longRetry |Geheel getal<br/><br/>De maximale waarde: 10 |1 |Het aantal pogingen lang voordat de segment-uitvoering is mislukt.<br/><br/>longRetry pogingen zijn door longRetryInterval verdeeld. Dus als u nodig hebt om op te geven van een tijd tussen nieuwe pogingen, longRetry gebruiken. Als zowel nieuwe pogingen als longRetry zijn opgegeven, elke poging longRetry bevat nieuwe pogingen en het maximale aantal pogingen wordt opnieuw geprobeerd * longRetry.<br/><br/>Bijvoorbeeld, als we de volgende instellingen hebben in de beleidsregels voor activiteiten:<br/>Opnieuw uitvoeren: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Wordt ervan uitgegaan dat er wordt slechts één segment om uit te voeren (status is in afwachting van) en de activiteit is uitgevoerd elke keer mislukt. Er zou worden in eerste instantie 3 opeenvolgende uitvoeringspogingen. Na elke poging zou de segmentstatus opnieuw proberen. Nadat de eerste 3 pogingen staan, kan het de segmentstatus LongRetry zou zijn.<br/><br/>Na een uur (dat wil zeggen, de longRetryInteval waarde) wordt een andere set 3 opeenvolgende uitvoeringspogingen. Hierna zou het de segmentstatus mislukt en geen pogingen meer zou worden uitgevoerd. Daarom algemene 6 pogingen zijn gedaan.<br/><br/>Als een uitvoering is geslaagd, het de segmentstatus gereed zijn en geen pogingen meer worden geprobeerd.<br/><br/>longRetry kan worden gebruikt in situaties waarin afhankelijke gegevens binnenkomen op niet-deterministisch tijdstippen of de totale omgeving is flaky onder welke gegevensverwerking plaatsvindt. In dergelijke gevallen tijd nieuwe pogingen na elkaar niet kunt doen, en dit na een interval van resultaten in de gewenste uitvoer.<br/><br/>Waarschuwing: hoge waarde voor longRetry of longRetryInterval niet instelt. Normaal gesproken dat andere systematische problemen hoofdgeheugen worden bij hogere waarden. |
| longRetryInterval |TimeSpan |00:00:00 |De vertraging tussen pogingen lang opnieuw proberen |

Zie voor meer informatie, [pijplijnen](data-factory-create-pipelines.md) artikel. 

## <a name="parallel-processing-of-data-slices"></a>Parallelle verwerking van gegevenssegmenten
U kunt de begindatum voor de pijplijn in het verleden instellen. Als u dit doet, wordt Data Factory automatisch berekend (back-opvullingen) alle gegevenssegmenten in het verleden en ze worden verwerkt. Bijvoorbeeld: als u een pijplijn met de begindatum 2017-04-01 maken en de huidige datum 2017-04-10 is. Als de frequentie van de uitvoergegevensset die dagelijks wordt Data Factory wordt gestart voor het verwerken van alle segmenten van 2017-04-01 op 2017-04-09 onmiddellijk omdat de begindatum in het verleden ligt. Het segment de status van 2017-04-10 is nog niet verwerkt omdat de waarde van eigenschap in de beschikbaarheidssectie style EndOfInterval standaard. Het oudste segment wordt verwerkt eerst als de standaard waarde van executionPriorityOrder OldestFirst is. Zie voor een beschrijving van de eigenschap style [beschikbaarheid van gegevenssets](#dataset-availability) sectie. Zie voor een beschrijving van de sectie executionPriorityOrder de [beleidsregels voor activiteiten](#activity-policies) sectie. 

Kunt u back-gevuld gegevenssegmenten parallel worden verwerkt door in te stellen de **gelijktijdigheid** eigenschap in de **beleid** sectie van de JSON van de activiteit. Deze eigenschap bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. De standaardwaarde voor de eigenschap gelijktijdigheid is 1. Daarom wordt een segment verwerkt op een tijdstip standaard. De maximale waarde is 10. Wanneer een pijplijn via een groot aantal beschikbare gegevens, met een hogere gelijktijdigheid-waarde moet sneller worden de gegevens verwerkt. 

## <a name="rerun-a-failed-data-slice"></a>Opnieuw uitvoeren van een mislukte gegevenssegment
Wanneer er een fout optreedt tijdens het verwerken van een gegevenssegment, vindt u waarom de verwerking van een segment is mislukt met behulp van Azure portal-blades of Monitor and Manage App gebruiken. Zie [bewaken en beheren van pijplijnen met behulp van Azure portal-blades](data-factory-monitor-manage-pipelines.md) of [app voor bewaking en beheer](data-factory-monitor-manage-app.md) voor meer informatie.

Bekijk het volgende voorbeeld, waarin twee activiteiten. Activiteit1 en activiteit 2. Activiteit1 een segment van Dataset1 verbruikt en een segment van Dataset2, die wordt gebruikt als invoer door activiteit2 voor het produceren van een segment van de uiteindelijke gegevensset wordt geproduceerd.

![Mislukte segment](./media/data-factory-scheduling-and-execution/failed-slice.png)

Het diagram toont die uit drie recente segmenten, er is een fout opgetreden bij het ontwikkelen van het segment 9-10 uur voor Dataset2. Data Factory-afhankelijkheid voor de gegevensset van de reeks tijd automatisch worden bijgehouden. Als gevolg hiervan wordt de activiteit die wordt uitgevoerd voor de downstream segment 9-10 uur niet worden gestart.

Data Factory bewakings- en -hulpprogramma's kunnen u inzoomen op de diagnostische logboeken voor de mislukte segment eenvoudig de hoofdoorzaak voor het probleem te vinden en op te lossen. Nadat u het probleem hebt opgelost, kunt u eenvoudig de activiteit die wordt uitgevoerd om te produceren van het mislukte segment te starten. Zie voor meer informatie over het opnieuw uitvoeren en begrijpen van statusovergangen voor gegevenssegmenten [bewaken en beheren van pijplijnen met behulp van Azure portal-blades](data-factory-monitor-manage-pipelines.md) of [app voor bewaking en beheer](data-factory-monitor-manage-app.md).

Nadat u het segment 9-10 uur voor opnieuw uitvoeren **Dataset2**, Data Factory wordt het uitvoeren van de afhankelijke segment 9-10 uur gestart op de uiteindelijke gegevensset.

![Mislukte segment opnieuw uitvoeren](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Meerdere activiteiten in een pijplijn
Een pijplijn kan echter meer dan één activiteit hebben. Als u meerdere activiteiten in een pijplijn hebt en de uitvoer van een activiteit geen invoer van een andere activiteit is, de activiteiten kunnen parallel worden uitgevoerd als invoergegevens segmenten voor de activiteiten klaar bent.

U kunt twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. De activiteiten kunnen worden in de pijplijn dezelfde of verschillende pijplijnen. De tweede activiteit wordt uitgevoerd alleen als het eerste item is voltooid.

Neem bijvoorbeeld het volgende geval waarbij een pijplijn twee activiteiten heeft:

1. A1 activiteit waarvoor externe invoergegevensset D1, en de uitvoergegevensset produceert D2.
2. Activiteit A2 die invoer van de gegevensset D2 is vereist en produceert uitvoergegevensset D3.

In dit scenario zijn activiteiten A1 en A2 in de dezelfde pijplijn. De activiteit A1 wordt uitgevoerd als de externe gegevens beschikbaar zijn en de frequentie van geplande beschikbaarheid wordt bereikt. De activiteit A2 wordt uitgevoerd als de geplande segmenten in D2 beschikbaar en de frequentie van geplande beschikbaarheid wordt bereikt. Als er een fout in een van de segmenten in de gegevensset D2, wordt niet A2 voor dat segment uitgevoerd totdat deze beschikbaar.

De diagramweergave met beide activiteiten in de pijplijn dezelfde eruit als in het volgende diagram:

![Koppelen van activiteiten in de dezelfde pijplijn](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Zoals eerder vermeld, kunnen de activiteiten worden in andere pijplijnen. In een dergelijk scenario eruit de diagramweergave als in het volgende diagram:

![Koppelen van activiteiten in twee pijplijnen](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Zie de kopiëren sequentieel sectie in de bijlage voor een voorbeeld.

## <a name="model-datasets-with-different-frequencies"></a>Model gegevenssets met verschillende frequenties
In de voorbeelden zijn de frequentie voor gegevenssets voor invoer en uitvoer en het venster van de planning activiteit hetzelfde. Sommige scenario's vereist de mogelijkheid om uitvoer met een frequentie anders dan de frequenties van een of meer invoerwaarden te produceren. Data Factory biedt ondersteuning voor deze scenario's modelleren.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Voorbeeld 1: Maken van een dagelijkse uitvoer rapport voor de invoergegevens die beschikbaar is per uur
U hebt een scenario waarin u hebt ingevoerd meetgegevens van sensoren die beschikbaar is per uur in Azure Blob-opslag. U wilt dat voor het produceren van een dagelijkse cumulatieve rapport met statistieken, zoals gemiddelde, maximum- en minimaal voor de dag met [hive-activiteit van Data Factory](data-factory-hive-activity.md).

Hier ziet u hoe u dit scenario met Data Factory kunt model:

**Invoergegevensset**

De invoerbestanden per uur worden verwijderd in de map voor de opgegeven dag. Beschikbaarheid voor invoer is ingesteld op **uur** (frequentie: Uur en interval: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Uitvoergegevensset**

Een uitvoerbestand wordt elke dag gemaakt in de map van de dag. Beschikbaarheid van de uitvoer is ingesteld op **dag** (frequentie: Dag en interval: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Activiteit: hive-activiteit in een pijplijn**

Het hive-script ontvangt de juiste *datum-/* informatie als parameters die gebruikmaken van de **WindowStart** variabele zoals wordt weergegeven in het volgende codefragment. Het hive-script maakt gebruik van deze variabele laden van de gegevens van de juiste map voor de dag en het uitvoeren van de aggregatie voor het genereren van de uitvoer.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
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

Het volgende diagram toont het scenario vanuit het oogpunt van een gegevens-afhankelijkheid.

![Gegevensafhankelijkheid](./media/data-factory-scheduling-and-execution/data-dependency.png)

Het uitvoersegment voor elke dag is afhankelijk van 24 Uurlijkse segmenten van een invoergegevensset. Data Factory berekent deze afhankelijkheden automatisch door het bepalen van de ingevoerde gegevens segmenten die kunnen worden onderverdeeld in dezelfde periode als het uitvoersegment wordt geproduceerd. Als een van de 24 segmenten van de invoer niet beschikbaar is, wacht de Data Factory het invoersegment deze klaar is voordat u begint met de dagelijkse activiteit die wordt uitgevoerd.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Voorbeeld 2: Geef op afhankelijkheid met expressies en functies van Data Factory
Laten we eens een ander scenario. Stel dat u hebt een hive-activiteit die twee invoergegevenssets verwerkt. Een van deze nieuwe gegevens dagelijks heeft, maar een van deze nieuwe gegevens elke week opgehaald. Stel dat u wilt doen van een join tussen de twee invoer en uitvoer produceren elke dag.

De eenvoudige benadering in welke Data Factory automatisch cijfers uit het recht invoer segmenten worden verwerkt door naar de uitvoer van de gegevens segmenttijd periode niet werkt.

U moet opgeven dat voor elke activiteit die wordt uitgevoerd, de Data Factory gegevenssegment afgelopen week voor wekelijks invoergegevensset gebruiken moeten. U gebruikt Azure Data Factory-functies zoals weergegeven in het volgende fragment voor het implementeren van dit gedrag.

**Input1: Azure-blob**

De invoer van de eerste is de Azure-blob wordt dagelijks bijgewerkt.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Input2: Azure-blob**

Input2 is de Azure-blob wordt wekelijks bijgewerkt.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**De uitvoer: Azure-blob**

Een uitvoerbestand wordt elke dag gemaakt in de map voor die dag. Beschikbaarheid van de uitvoer is ingesteld op **dag** (frequentie: Dag, interval: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Activiteit: hive-activiteit in een pijplijn**

De hive-activiteit neemt de twee invoeren en produceert een uitvoersegment elke dag. U kunt van elke dag uitvoersegment als volgt afhankelijk van de vorige week invoersegment voor wekelijkse invoer opgeven.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
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

Zie [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md) voor een lijst met functies en systeemvariabelen die ondersteuning biedt voor Data Factory.

## <a name="appendix"></a>Bijlage

### <a name="example-copy-sequentially"></a>Voorbeeld: kopiëren sequentieel worden verwerkt
Het is mogelijk meerdere kopieerbewerkingen na elkaar uitgevoerd op een manier opeenvolgende/gerangschikt. Bijvoorbeeld, mogelijk hebt u twee kopieeractiviteiten in een pijplijn met de volgende uitvoergegevenssets voor invoer (CopyActivity1 en CopyActivity2):   

CopyActivity1

Invoer: Dataset. Uitvoer: Dataset2.

CopyActivity2

Invoer: Dataset2.  Uitvoer: Dataset3.

CopyActivity2 uitgevoerd alleen als de CopyActivity1 met succes is uitgevoerd en Dataset2 beschikbaar is.

Hier volgt de voorbeeldpijplijn JSON:

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

U ziet dat in het voorbeeld wordt de uitvoergegevensset van de eerste copy-activiteit (Dataset2) is opgegeven als invoer voor de tweede activiteit. Daarom de tweede activiteit wordt alleen uitgevoerd als de uitvoergegevensset van de eerste activiteit gereed is.  

In het voorbeeld CopyActivity2 kan hebben een andere invoer, zoals Dataset3, maar u Dataset2 opgeven als invoer voor CopyActivity2, zodat de activiteit wordt niet uitgevoerd tenzij CopyActivity1 is voltooid. Bijvoorbeeld:

CopyActivity1

Invoer: Dataset1. Uitvoer: Dataset2.

CopyActivity2

Invoer: Dataset3, Dataset2. Uitvoer: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

U ziet dat de twee gegevenssets voor invoer in het voorbeeld zijn opgegeven voor de tweede kopieeractiviteit. Wanneer meerdere invoergegevens zijn opgegeven, wordt alleen de eerste invoergegevensset wordt gebruikt voor het kopiëren van gegevens, maar andere gegevenssets worden gebruikt als afhankelijkheden. CopyActivity2 zou start pas als de volgende voorwaarden wordt voldaan:

* CopyActivity1 is voltooid en Dataset2 is beschikbaar. Deze gegevensset wordt niet gebruikt bij het kopiëren van gegevens naar Dataset4. Het alleen fungeert als een afhankelijkheid van de planning voor CopyActivity2.   
* Dataset3 is beschikbaar. Deze gegevensset vertegenwoordigt de gegevens die worden gekopieerd naar de bestemming. 

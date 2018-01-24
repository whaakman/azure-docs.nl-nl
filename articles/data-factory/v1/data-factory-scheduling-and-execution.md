---
title: Plannen en uitvoeren met Data Factory | Microsoft Docs
description: Meer informatie over plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 22005f6b0f655a68dbf8fab600872d8d20102dfe
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="data-factory-scheduling-and-execution"></a>Data Factory plannen en uitvoeren
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [pipeline-uitvoering en triggers](../concepts-pipeline-execution-triggers.md) artikel.

In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. In dit artikel wordt ervan uitgegaan dat u van de Data Factory toepassing model concepten basisbeginselen, met inbegrip van activiteiten, pijplijnen, gekoppelde services en gegevenssets. Voor de basisconcepten van Azure Data Factory, Zie de volgende artikelen:

* [Inleiding tot Data Factory](data-factory-introduction.md)
* [Pijplijnen](data-factory-create-pipelines.md)
* [Gegevenssets](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Begin- en eindtijden van pijplijn
Een pijplijn is active alleen tussen de **start** tijd en **end** tijd. Het is niet uitgevoerd vóór de begintijd of na de eindtijd. Als de pijplijn wordt onderbroken, is het niet uitgevoerd ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. U kunt deze instellingen (start, eindgebruikers, onderbroken) vinden in de definitie van de pijplijn: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Zie voor meer informatie deze eigenschappen [pijplijnen maken](data-factory-create-pipelines.md) artikel. 


## <a name="specify-schedule-for-an-activity"></a>Planning voor een activiteit opgeven
Het is niet de pijplijn die wordt uitgevoerd. De activiteiten in de pijplijn die worden uitgevoerd in de context van de pijplijn is. U kunt een terugkerend schema voor een activiteit opgeven met behulp van de **scheduler** sectie van de JSON van de activiteit. U kunt bijvoorbeeld plannen dat een activiteit per uur als volgt uitvoeren:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

In het volgende diagram ziet opgeven van dat een planning voor een activiteit wordt een reeks van windows met tumbling in de pipeline-begin- en eindtijden. Windows daling zijn een reeks met vaste grootte niet-overlappende, aaneengesloten tijdsintervallen. Deze logische daling vensters voor een activiteit worden genoemd **activiteitsvensters**.

![Voorbeeld van de activiteit scheduler](media/data-factory-scheduling-and-execution/scheduler-example.png)

De **scheduler** eigenschap voor een activiteit is optioneel. Als u deze eigenschap opgeeft, moet overeenkomen met de frequentie die u in de definitie van de uitvoergegevensset voor de activiteit opgeeft. Momenteel is de uitvoergegevensset dat wat de planning aanstuurt. Daarom moet u een uitvoergegevensset maken, zelfs als de activiteit geen uitvoer produceert. 

## <a name="specify-schedule-for-a-dataset"></a>Geef de planning voor een gegevensset
Een activiteit in een Data Factory-pijplijn kan duren voordat de invoer van nul of meer **gegevenssets** en een of meer uitvoergegevenssets te produceren. Voor een activiteit, kunt u de frequentie waarmee de invoergegevens beschikbaar is of de uitvoergegevens wordt gemaakt met behulp van de **beschikbaarheid** sectie in de definities van de gegevensset. 

**Frequentie** in de **beschikbaarheid** sectie geeft de tijdseenheid. De toegestane waarden voor de frequentie zijn: minuut, uur, dag, Week en maand. De **interval** geeft een vermenigvuldiger voor de frequentie van eigenschap in de beschikbaarheidssectie. Bijvoorbeeld: als de frequentie is ingesteld op dagen en interval is ingesteld op 1 voor een uitvoergegevensset, de uitvoergegevens dagelijks wordt geproduceerd. Als u de frequentie als minuut opgeeft, wordt u aangeraden dat u het interval op niet kleiner zijn dan 15 instellen. 

In het volgende voorbeeld wordt de invoergegevens beschikbaar per uur en de uitvoergegevens per uur wordt geproduceerd (`"frequency": "Hour", "interval": 1`). 

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

Op dit moment **uitvoergegevensset stations de planning**. Met andere woorden, wordt de planning die is opgegeven voor de uitvoergegevensset die gebruikt voor het uitvoeren van een activiteit tijdens runtime. Daarom moet u een uitvoergegevensset maken, zelfs als de activiteit geen uitvoer produceert. Als er voor de activiteit geen invoer nodig is, kunt u het maken van de invoergegevensset overslaan. 

In de volgende definitie van de pipeline, de **scheduler** eigenschap wordt gebruikt om de planning voor de activiteit te specificeren. Deze eigenschap is optioneel. Op dit moment wordt de planning voor de uitvoergegevensset opgegeven moet overeenkomen met het schema voor de activiteit.
 
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

In dit voorbeeld wordt de activiteit uitgevoerd per uur tussen de begin- en eindtijden van de pijplijn. De uitvoergegevens wordt per uur gemaakt voor drie uur windows (8 AM - 9 uur, 9: 00 uur - 10: 00 uur, en 10: 00 - 11 uur). 

Elke eenheid van gegevens gebruikt of die wordt geproduceerd door een activiteit die wordt uitgevoerd, heet een **gegevenssegment**. Het volgende diagram toont een voorbeeld van een activiteit met één invoergegevensset en één uitvoergegevensset: 

![Beschikbaarheid scheduler](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Het diagram toont de gegevenssegmenten per uur voor de gegevensset invoer- en uitvoergegevens. Het diagram toont de drie invoer segmenten die gereed voor verwerking zijn. De activiteit 10 11 uur wordt uitgevoerd, het opstellen van het segment van de uitvoer 10 11 uur. 

U hebt toegang tot de tijdsinterval die is gekoppeld aan het huidige segment in de JSON van de gegevensset met behulp van variabelen: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) en [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables). Op deze manier kunt u de tijdsinterval die is gekoppeld aan het venster van een activiteit met de WindowStart en WindowEnd openen. De planning van een activiteit moet overeenkomen met de planning van de uitvoergegevensset voor de activiteit. Daarom de SliceStart en SliceEnd waarden zijn hetzelfde als de waarden voor WindowStart en WindowEnd respectievelijk. Zie voor meer informatie over deze variabelen [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables) artikelen.  

U kunt deze variabelen voor verschillende doeleinden gebruiken in de JSON van de activiteit. Bijvoorbeeld, kunt u gegevens selecteren uit de invoer- en uitvoergegevenssets representatie van time series-gegevens (bijvoorbeeld: 8 uur op 9: 00 uur). Dit voorbeeld wordt tevens **WindowStart** en **WindowEnd** selecteren relevante gegevens voor een activiteit uitvoeren en kopieer het naar een blob met de juiste **folderPath**. De **folderPath** parameters om een afzonderlijke map voor elk uur wordt gebruikt.  

In het voorgaande voorbeeld de planning die is opgegeven voor de invoer en uitvoergegevenssets is de dezelfde (elk uur). Als invoergegevensset voor de activiteit beschikbaar op een andere frequentie spreek om de 15 minuten is, de activiteit die deze uitvoergegevensset produceert nog eens per uur uitgevoerd zoals de uitvoergegevensset is wat de activiteitenplanning stations. Zie voor meer informatie [Model gegevenssets frequentie](#model-datasets-with-different-frequencies).

## <a name="dataset-availability-and-policies"></a>Beschikbaarheid van de gegevensset en het beleid
U kunt het gebruik van de frequentie en het interval eigenschappen in de beschikbaarheidssectie van de definitie van gegevensset hebt gezien. Er zijn enkele andere eigenschappen die betrekking hebben op de planning en uitvoering van een activiteit. 

### <a name="dataset-availability"></a>Beschikbaarheid van gegevensset 
De volgende tabel beschrijft de eigenschappen kunt u in de **beschikbaarheid** sectie:

| Eigenschap | Beschrijving | Vereist | Standaard |
| --- | --- | --- | --- |
| frequency |Hiermee geeft u tijdseenheid voor de gegevensset segment productie.<br/><br/><b>Ondersteunde frequentie</b>: minuut, uur, dag, Week, maand |Ja |N.v.t. |
| interval |Hiermee geeft u een vermenigvuldiger voor de frequentie<br/><br/>"Frequentie x-interval" bepaalt hoe vaak het segment wordt geproduceerd.<br/><br/>Als u de gegevensset worden gesegmenteerd op uurbasis moet, stelt u <b>frequentie</b> naar <b>uur</b>, en <b>interval</b> naar <b>1</b>.<br/><br/><b>Opmerking</b>: als u frequentie als minuut opgeeft, raden wij aan dat u het interval aan niet lager zijn dan 15 |Ja |N.v.t. |
| stijl |Hiermee geeft u op of het segment aan het begin/einde van het interval moet worden geproduceerd.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Als de frequentie is ingesteld op maand en stijl is ingesteld op EndOfInterval, wordt het segment op de laatste dag van de maand geproduceerd. Als de stijl is ingesteld op StartOfInterval, wordt het segment op de eerste dag van de maand geproduceerd.<br/><br/>Als de frequentie is ingesteld op de dag en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd in het afgelopen uur van de dag.<br/><br/>Als de frequentie is ingesteld op uur en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd aan het einde van het uur. Voor een segment 1-2 uur gedurende het segment geproduceerd om 2 uur. |Nee |EndOfInterval |
| anchorDateTime |Hiermee definieert u de absolute positie in de tijd die wordt gebruikt door de planner gegevensset segmentgrenzen berekenen. <br/><br/><b>Opmerking</b>: als de AnchorDateTime bevat de datumonderdelen die meer gedetailleerd dan de frequentie zijn, wordt de gedetailleerdere onderdelen worden genegeerd. <br/><br/>Bijvoorbeeld, als de <b>interval</b> is <b>per uur</b> (frequentie: uur en interval: 1) en de <b>AnchorDateTime</b> bevat <b>minuten en seconden</b>, wordt de <b>minuten en seconden</b> delen van de AnchorDateTime worden genegeerd. |Nee |01/01/0001 |
| offset |TimeSpan waarmee het begin en einde van alle segmenten van de gegevensset zijn verplaatst. <br/><br/><b>Opmerking</b>: als zowel anchorDateTime als offset worden opgegeven, het resultaat is een gecombineerde verschuiving. |Nee |N.v.t. |

### <a name="offset-example"></a>offset voorbeeld
Standaard dagelijks (`"frequency": "Day", "interval": 1`) segmenten beginnen bij 12: 00 A.M. UTC-tijd (middernacht). Als u de begintijd moet 6 uur UTC-tijd in plaats daarvan wilt, stelt u de verschuiving zoals weergegeven in het volgende fragment: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Voorbeeld van anchorDateTime
In het volgende voorbeeld wordt de elke 23 uur in de gegevensset wordt geproduceerd. Het eerste segment wordt gestart op het moment dat is opgegeven door de anchorDateTime die is ingesteld op `2017-04-19T08:00:00` (UTC-tijd).

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>offset/stijl voorbeeld
De volgende gegevensset is een maandelijkse gegevensset en op 3rd van elke maand om 8:00 uur wordt geproduceerd (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>DataSet beleid
Een dataset kan een validatiebeleid gedefinieerd waarmee wordt aangegeven hoe de gegevens die worden gegenereerd door de uitvoering van een segment kan worden gevalideerd voordat deze gereed voor verbruik is hebben. In dergelijke gevallen dat nadat het segment is uitgevoerd, de status van de uitvoer-segment wordt gewijzigd naar **wachten** met een substatus van **validatie**. Nadat de segmenten zijn geverifieerd, de segment-status gewijzigd in **gereed**. Als een gegevenssegment is geproduceerd, maar zijn niet worden gevalideerd, worden de activiteiten bij uitvoering downstream segmenten die afhankelijk van dit segment zijn niet verwerkt. [Bewaken en beheren van pijplijnen](data-factory-monitor-manage-pipelines.md) bevat informatie over de verschillende toestanden van gegevenssegmenten in Data Factory.

De **beleid** sectie in de definitie van gegevensset definieert u de criteria of de conditie van de segmenten van de gegevensset moeten voldoen. De volgende tabel beschrijft de eigenschappen kunt u in de **beleid** sectie:

| Beleidsnaam | Beschrijving | Toegepast op | Vereist | Standaard |
| --- | --- | --- | --- | --- |
| minimumSizeMB | Valideert dat de gegevens in een **Azure blob** voldoet aan de minimale grootte (in MB). |Azure Blob |Nee |N.v.t. |
| minimumRows | Valideert dat de gegevens in een **Azure SQL-database** of een **Azure-tabel** het minimum aantal rijen bevat. |<ul><li>Azure SQL Database</li><li>Azure-tabel</li></ul> |Nee |N.v.t. |

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

Zie voor meer informatie over deze eigenschappen en voorbeelden [gegevenssets maken](data-factory-create-datasets.md) artikel. 

## <a name="activity-policies"></a>Activiteitenbeleid
Beleid geldt voor de run-time-gedrag van een activiteit specifiek wanneer het segment van een tabel wordt verwerkt. De volgende tabel bevat de details.

| Eigenschap | Toegestane waarden | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| Gelijktijdigheid van taken |Geheel getal <br/><br/>De maximale waarde: 10 |1 |Het aantal gelijktijdige uitvoeringen van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. Bijvoorbeeld, als een activiteit moet doorlopen sneller een groot aantal beschikbare gegevens, een grotere waarde voor gelijktijdigheid het verwerken van gegevens. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Bepaalt de volgorde van de gegevenssegmenten dat wordt verwerkt.<br/><br/>Als er 2 segmenten (één gebeurt om 4 uur en 17: 00 uur een andere één voor één) en beide zijn in behandeling kan worden uitgevoerd. Als u de executionPriorityOrder NewestFirst worden ingesteld, wordt het segment om 17.00 eerst verwerkt. Op dezelfde manier als u de executionPriorityORder OldestFIrst worden ingesteld, klikt u vervolgens het segment om 4 uur is verwerkt. |
| retry |Geheel getal<br/><br/>De maximale waarde is 10 |0 |Aantal nieuwe pogingen voordat de verwerking van de gegevens voor het segment is gemarkeerd als mislukt. Uitvoering van de activiteit voor een gegevenssegment wordt herhaald tot maximaal het aantal pogingen opgegeven. De nieuwe poging wordt gedaan zo snel mogelijk na de fout. |
| timeout |TimeSpan |00:00:00 |Time-out voor de activiteit. Voorbeeld: 00:10:00 (impliceert time-out 10 minuten)<br/><br/>Als een waarde niet opgegeven is of 0 is, is de time-out van oneindig.<br/><br/>Als de verwerkingstijd van de gegevens op een segment de time-outwaarde overschrijdt, deze wordt geannuleerd en wordt geprobeerd om opnieuw te proberen de verwerking. Het aantal nieuwe pogingen, is afhankelijk van de eigenschap probeer het opnieuw. Wanneer een time-out optreedt, wordt de status ingesteld op een time-out opgetreden bij. |
| Vertraging |TimeSpan |00:00:00 |Geef de vertraging optreden voordat de verwerking van het segment wordt gestart.<br/><br/>De uitvoering van de activiteit voor een gegevenssegment wordt gestart nadat de vertraging voorbij de verwachte tijd voor uitvoering is.<br/><br/>Voorbeeld: 00:10:00 (betekent vertraging van 10 minuten) |
| longRetry |Geheel getal<br/><br/>De maximale waarde: 10 |1 |Het aantal pogingen lang voordat de uitvoering van het segment is mislukt.<br/><br/>longRetry pogingen door longRetryInterval gelijkmatig verdeeld. Als u een tijd tussen pogingen opgeven moet, gebruikt u dus longRetry. Als zowel de nieuwe pogingen en de longRetry worden opgegeven, elke poging longRetry bevat nieuwe pogingen en het maximale aantal pogingen is opnieuw * longRetry.<br/><br/>Bijvoorbeeld, als we hebben de volgende instellingen in het activiteitenbeleid voor:<br/>Probeer: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Wordt ervan uitgegaan dat er is slechts één segment uit te voeren (status Waiting) en de activiteit is uitgevoerd elke keer mislukt. Er zou worden in eerste instantie 3 opeenvolgende uitvoering pogingen. De status van het segment is na elke poging probeer het opnieuw. Nadat de eerste 3 pogingen hebben bekeken, zou de status van het segment LongRetry zijn.<br/><br/>Na een uur (dat wil zeggen, de longRetryInteval waarde), zou er een andere set 3 opeenvolgende uitvoering pogingen. Hierna is de status van het segment zou niet en geen pogingen meer zou worden uitgevoerd. Daarom is algemene 6 geprobeerd.<br/><br/>Als een uitvoering is voltooid, het segment de status gereed zijn en er worden geen pogingen meer geprobeerd.<br/><br/>longRetry kan worden gebruikt in situaties waarbij afhankelijke gegevens aankomen op niet-deterministische tijdstippen of de hele omgeving flaky onder welke gegevensverwerking plaatsvindt. In dergelijke gevallen tijd nieuwe pogingen achter elkaar niet kunt doen, en dit na een interval van resultaten in de gewenste uitvoer.<br/><br/>Waarschuwing: geen hoge waarden voor de longRetry of longRetryInterval instelt. Hogere waarden dat normaal gesproken andere al problemen. |
| longRetryInterval |TimeSpan |00:00:00 |De vertraging tussen pogingen lang |

Zie voor meer informatie [pijplijnen](data-factory-create-pipelines.md) artikel. 

## <a name="parallel-processing-of-data-slices"></a>Parallelle verwerking van gegevenssegmenten
U kunt de begindatum van de pijplijn in het verleden instellen. Wanneer u doet dit, wordt Data Factory automatisch berekend (back-opvulling) alle gegevenssegmenten in het verleden en ze worden verwerkt. Bijvoorbeeld: als u een pijplijn met een begindatum 2017-04-01 maken en de huidige datum 2017-04-10 valt. Als de frequentie van de uitvoergegevensset die dagelijks wordt verwerkt alle segmenten van 2017-04-01 op 2017-04-09 Data Factory-begint onmiddellijk omdat de begindatum in het verleden ligt. Het segment uit 2017-04-10 is niet verwerkt nog omdat de waarde van de eigenschap style in de beschikbaarheidssectie EndOfInterval standaard. Het oudste segment is verwerkt eerst als de standaard waarde van executionPriorityOrder OldestFirst is. Zie voor een beschrijving van de eigenschap style [gegevensset beschikbaarheid](#dataset-availability) sectie. Zie voor een beschrijving van de sectie executionPriorityOrder de [beleidsregels voor activiteiten](#activity-policies) sectie. 

Kunt u back-gevuld gegevenssegmenten parallel worden verwerkt door in te stellen de **gelijktijdigheid** eigenschap in de **beleid** sectie van de JSON van de activiteit. Deze eigenschap bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. De standaardwaarde voor de eigenschap gelijktijdigheid van taken is 1. Daarom is een segment verwerkt op een tijdstip standaard. De maximale waarde is 10. Wanneer een pijplijn moet doorlopen een groot aantal beschikbare gegevens, een grotere waarde voor gelijktijdigheid van taken de verwerking van gegevens sneller. 

## <a name="rerun-a-failed-data-slice"></a>Voer een mislukte gegevenssegment
Wanneer een fout optreedt tijdens het verwerken van een gegevenssegment, vindt u waarom de verwerking van een segment met behulp van Azure portal-blades of Monitor and Manage App is mislukt. Zie [controleren en beheren met Azure portal-blades pijplijnen](data-factory-monitor-manage-pipelines.md) of [app voor bewaking en beheer](data-factory-monitor-manage-app.md) voor meer informatie.

Bekijk het volgende voorbeeld, waarin twee activiteiten. Activiteit1 en activiteit 2. Activiteit1 een segment van Dataset1 verbruikt en een segment van Dataset2 die wordt gebruikt als invoer door activiteit2 voor het produceren van een segment van de laatste gegevensset wordt geproduceerd.

![Mislukte segment](./media/data-factory-scheduling-and-execution/failed-slice.png)

Het diagram toont die uit drie recente segmenten, er is een fout opgetreden bij het opstellen van het segment 9 10 uur voor Dataset2. Data Factory-afhankelijkheid voor de gegevensset van de reeks tijd automatisch worden bijgehouden. Hierdoor wordt de activiteit die wordt uitgevoerd voor de downstream segment 9-10: 00 uur niet worden gestart.

Data Factory bewaking en beheer-hulpprogramma's kunnen u inzoomen op de diagnostische logboeken voor het segment de mislukte eenvoudig de hoofdoorzaak voor het probleem te vinden en op te lossen. Nadat u het probleem hebt opgelost, kunt u eenvoudig de activiteit die wordt uitgevoerd om te produceren van het mislukte segment te starten. Zie voor meer informatie over het opnieuw uitvoeren en begrijpen van statusovergangen voor gegevenssegmenten [controleren en beheren met Azure portal-blades pijplijnen](data-factory-monitor-manage-pipelines.md) of [app voor bewaking en beheer](data-factory-monitor-manage-app.md).

Nadat u het segment 9 10 uur voor opnieuw uitvoeren **Dataset2**, Data Factory begint de uitvoeren voor de afhankelijke segment 9 10 uur op de laatste gegevensset.

![Mislukte segment opnieuw uitvoeren](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Meerdere activiteiten in een pijplijn
Een pijplijn kan echter meer dan één activiteit hebben. Als er meerdere activiteiten in een pijplijn en de uitvoer van een activiteit geen invoer van een andere activiteit is, de activiteiten kunnen parallel worden uitgevoerd als invoergegevens segmenten voor de activiteiten klaar bent.

U kunt twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. De activiteiten kunnen worden in de dezelfde pijplijn of in verschillende pijplijnen. De tweede activiteit wordt uitgevoerd alleen wanneer de eerste die met succes wordt voltooid.

Neem bijvoorbeeld het volgende geval waarbij een pijplijn twee activiteiten heeft:

1. A1 activiteit waarvoor externe invoergegevensset D1 en D2 uitvoergegevensset die wordt gegenereerd.
2. Activiteit A2 die invoer uit gegevensset D2 is vereist en produceert uitvoergegevensset D3.

In dit scenario zijn activiteiten A1 en A2 in de dezelfde pijplijn. De activiteit A1 wordt uitgevoerd als de externe gegevens beschikbaar is en de frequentie van geplande beschikbaarheid is bereikt. De activiteit A2 wordt uitgevoerd als de geplande segmenten van D2 beschikbaar en de frequentie van geplande beschikbaarheid is bereikt. Als er een fout in een van de segmenten in de gegevensset D2, A2 kan niet worden uitgevoerd voor die segment totdat deze beschikbaar.

De diagramweergave met beide activiteiten in de pijplijn voor dezelfde eruit als in het volgende diagram:

![Activiteiten in de dezelfde pijplijn-koppeling](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Zoals eerder vermeld, kunnen de activiteiten worden in verschillende pijplijnen. In een dergelijk scenario eruit de diagramweergave als in het volgende diagram:

![Koppeling van activiteiten in twee pijplijnen](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Zie de [sequentieel kopiëren](#copy-sequentially) sectie in de bijlage voor een voorbeeld.

## <a name="model-datasets-with-different-frequencies"></a>Model gegevenssets frequentie
In de voorbeelden zijn de frequenties voor invoer- en uitvoergegevenssets en het venster van de planning activiteit hetzelfde. Sommige scenario's moet de mogelijkheid om uitvoer met een frequentie afwijken van de frequenties van een of meer invoerwaarden te produceren. Data Factory biedt ondersteuning voor het modelleren van deze scenario's.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Voorbeeld 1: Een dagelijkse uitvoer-rapport voor invoergegevens die beschikbaar is om het uur produceren
Neem bijvoorbeeld een scenario waarin u hebt ingevoerd meetgegevens van sensoren beschikbaar om het uur in Azure Blob-opslag. U wilt dat voor het produceren van een dagelijks cumulatieve rapport met statistieken zoals gemiddelde, maximum en minimum voor de dag met [Data Factory hive-activiteit](data-factory-hive-activity.md).

Hier ziet u hoe u dit scenario met Data Factory kunt model:

**Invoergegevensset**

De Uurlijkse invoerbestanden zijn verwijderd in de map voor de opgegeven dag. Beschikbaarheid voor invoer is ingesteld op **uur** (frequentie: uur, interval: 1).

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

Een bestand voor uitvoer wordt elke dag worden gemaakt in de map van de dag. Beschikbaarheid van de uitvoer is ingesteld op **dag** (frequentie: dagen en interval: 1).

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

Het hive-script ontvangt de juiste *DateTime* informatie als parameters die gebruikmaken van de **WindowStart** variabele, zoals wordt weergegeven in het volgende fragment. Het hive-script gebruikt deze variabele om de gegevens uit de juiste map laden voor de dag en het uitvoeren van de aggregatie voor het genereren van de uitvoer.

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

Het volgende diagram toont het scenario uit het oogpunt van een gegevens-afhankelijkheid.

![Gegevensafhankelijkheid](./media/data-factory-scheduling-and-execution/data-dependency.png)

Het segment uitvoer voor elke dag, is afhankelijk van 24 uur segmenten van een invoergegevensset. Data Factory berekent automatisch deze afhankelijkheden als u weet de invoergegevens segmenten die in dezelfde periode als het segment uitvoer vallen te produceren. Als een van de 24 invoersegmenten één keer niet beschikbaar is, wacht Data Factory het invoersegment worden gereed is voordat u begint de dagelijkse activiteit die wordt uitgevoerd.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Voorbeeld 2: Geef afhankelijkheid met expressies en functies van de Data Factory
Laten we eens een ander scenario. Stel dat u hebt een hive-activiteit die twee invoergegevenssets verwerkt. Een van deze nieuwe gegevens dagelijks heeft, maar één van deze nieuwe gegevens worden elke week opgehaald. Stel dat u wilt doen van een join tussen de twee invoeren en uitvoer produceert elke dag.

De eenvoudige benadering in welke Data Factory automatisch cijfers uit het recht invoer segmenten worden verwerkt door aan te passen aan de uitvoer van het gegevenssegment tijd periode niet werkt.

U moet opgeven dat voor elke activiteit die wordt uitgevoerd, de Data Factory gegevenssegment afgelopen week voor wekelijkse invoergegevensset gebruiken moeten. U Azure Data Factory functies gebruiken zoals wordt weergegeven in het volgende codefragment voor het implementeren van dit probleem.

**Input1: Azure blob**

De eerste invoer is de blob van Azure wordt dagelijks bijgewerkt.

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

**Input2: Azure blob**

Input2 is de Azure blob wekelijks worden bijgewerkt.

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

**Uitvoer: Azure blob**

Een bestand voor uitvoer wordt elke dag gemaakt in de map voor de dag. Beschikbaarheid van de uitvoer is ingesteld op **dag** (frequentie: dag, interval: 1).

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

Het hive-activiteit duurt de twee invoeren en een uitvoer-segment wordt geproduceerd elke dag. U kunt elke dag uitvoer segment invoersegment met de vorige week voor wekelijkse invoer als volgt afhankelijk opgeven.

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

### <a name="example-copy-sequentially"></a>Voorbeeld: sequentieel kopiëren
Het is mogelijk meerdere kopieerbewerkingen na elkaar uitgevoerd op een manier opeenvolgende/gerangschikt. U wellicht bijvoorbeeld twee kopie activiteiten in een pijplijn met de volgende invoergegevens uitvoergegevenssets (CopyActivity1 en CopyActivity2):   

CopyActivity1

Invoer: Dataset. Uitvoer: Dataset2.

CopyActivity2

Invoer: Dataset2.  Uitvoer: Dataset3.

CopyActivity2 uitgevoerd alleen als de CopyActivity1 met succes is uitgevoerd en Dataset2 beschikbaar is.

Hier volgt de JSON van de voorbeeldpijplijn:

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

Merk op dat in het voorbeeld wordt de uitvoergegevensset van de eerste kopieeractiviteit (Dataset2) is opgegeven als invoer voor de tweede activiteit. Daarom de tweede activiteit wordt alleen uitgevoerd als de uitvoergegevensset van de eerste activiteit gereed is.  

In het voorbeeld kunt CopyActivity2 een andere invoer, zoals Dataset3, maar u Dataset2 opgeven als invoer voor CopyActivity2, zodat de activiteit kan niet worden uitgevoerd totdat CopyActivity1 is voltooid. Bijvoorbeeld:

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

U ziet in het voorbeeld worden twee invoergegevenssets opgegeven voor de kopieeractiviteit van de tweede. Wanneer meerdere invoer zijn opgegeven, wordt alleen de eerste invoer gegevensset wordt gebruikt voor het kopiëren van gegevens, maar andere gegevenssets worden gebruikt als afhankelijkheden. CopyActivity2 zou pas beginnen nadat de volgende voorwaarden wordt voldaan:

* CopyActivity1 is voltooid en Dataset2 is beschikbaar. Deze gegevensset wordt niet gebruikt bij het kopiëren van gegevens naar Dataset4. Deze alleen fungeert als een afhankelijkheid van de planning voor CopyActivity2.   
* Dataset3 is beschikbaar. Deze gegevensset vertegenwoordigt de gegevens die worden gekopieerd naar de bestemming. 
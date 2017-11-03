---
title: Azure Stream Analytics JavaScript gebruiker gedefinieerde aggregaties | Microsoft Docs
description: Geavanceerde query mechanisme met de gebruiker gedefinieerde aggregaties JavaScript uitvoeren
keywords: JavaScript, door de gebruiker gedefinieerde aggregaties, uda
services: stream-analytics
author: minhe-msft
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/28/2017
ms.author: minhe
ms.openlocfilehash: b3863a34ed146e54c6d60e035957b942a1976ff9
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-aggregates-preview"></a>Azure Stream Analytics JavaScript gebruiker gedefinieerde aggregaties (Preview)

Azure Stream Analytics ondersteunt de gebruiker gedefinieerde aggregaties (UDA) geschreven in JavaScript, Hiermee kunt u complexe stateful bedrijfslogica implementeren. Binnen UDA hebt u volledige controle van de gegevensstructuur van status, status opeenstapeling, status feit en statistische resultaat berekening. Het artikel bevat de twee verschillende die JavaScript UDA-interfaces, stappen voor het maken van een UDA en het gebruik van UDA met bewerkingen op basis van het venster in Stream Analytics-query.

## <a name="javascript-user-defined-aggregates"></a>De gebruiker gedefinieerde aggregaties JavaScript

Een door de gebruiker gedefinieerde aggregatie wordt bovenop een specificatie van het tijd gebruikt voor het aggregeren via de gebeurtenissen in dit venster en produceren van een enkele waarde als resultaat. Er zijn twee typen Stream Analytics ondersteunt nu AccumulateOnly en AccumulateDeaccumulate UDA-interfaces. Beide typen UDA kunnen worden gebruikt door Tumblingvenster, Hopping venster en Verschuivend venster. AccumulateDeaccumulate UDA presteert beter dan AccumulateOnly UDA wanneer gebruikt in combinatie met Hopping en Verschuivend venster. U kunt een van de twee typen op basis van de algoritme die u gebruikt.

### <a name="accumulateonly-aggregates"></a>AccumulateOnly statistische functies

AccumulateOnly statistische functies kunnen alleen nieuwe gebeurtenissen naar de status verzamelen, het algoritme kan geen deaccumulation van waarden. Deze cumulatieve kiezen wanneer een gebeurtenis deaccumulate informatie van de waarde van de status is niet mogelijk om te implementeren. Hier volgt de JavaScript-sjabloon voor AccumulatOnly statistische functies:

````JavaScript
// Sample UDA which state can only be accumulated.
function main() {
    this.init = function () {
        this.state = 0;
    }

    this.accumulate = function (value, timestamp) {
        this.state += value;
    }

    this.computeResult = function () {
        return this.state;
    }
}
````

### <a name="accumulatedeaccumulate-aggregates"></a>AccumulateDeaccumulate statistische functies

AccumulateDeaccumulate statistische functies toestaan deaccumulation van een vorige cumulatieve waarde van de status, bijvoorbeeld, een sleutel / waarde-paar verwijderen uit een lijst met waarden van de gebeurtenis of afgetrokken van een waarde van een status van de totale som. Hier volgt de JavaScript-sjabloon voor AccumulateDeaccumulate statistische functies:

````JavaScript
// Sample UDA which state can be accumulated and deaccumulated.
function main() {
    this.init = function () {
        this.state = 0;
    }

    this.accumulate = function (value, timestamp) {
        this.state += value;
    }

    this.deaccumulate = function (value, timestamp) {
        this.state -= value;
    }

    this.deaccumulateState = function (otherState){
        this.state -= otherState.state;
    }

    this.computeResult = function () {
        return this.state;
    }
}
````

## <a name="uda---javascript-function-declaration"></a>UDA - declaratie van JavaScript-functie

Elke UDA JavaScript wordt gedefinieerd door de declaratie van een functie-object. Hier volgen de belangrijkste elementen in de definitie van een UDA.

### <a name="function-alias"></a>Functie alias

Functie-alias is de UDA-id. Als deze wordt aangeroepen in de Stream Analytics query, altijd gebruik van UDA alias samen met een 'uda." voorvoegsel.

### <a name="function-type"></a>Functietype

Voor UDA, het functietype moet **Javascript UDA**.

### <a name="output-type"></a>Het uitvoertype

Een specifiek type die Stream Analytics-taak ondersteund of '' als u wilt verwerken van het type in uw query.

### <a name="function-name"></a>Functienaam

De naam van deze functie-object. Naam van de functie moet letterlijk overeenkomen met de UDA-alias (gedrag, bekijken we anonieme ondersteuningsfunctie overweegt wanneer GA).

### <a name="method---init"></a>Methode - init()

De methode init() initialiseert de status van de statistische functie. Deze methode wordt aangeroepen wanneer venster wordt gestart.

### <a name="method--accumulate"></a>Methode – accumulate()

De methode accumulate() wordt berekend op basis van de vorige status en de huidige waarden van de gebeurtenis status van de UDA. Deze methode wordt aangeroepen wanneer een gebeurtenis in een tijdvenster (TUMBLINGWINDOW, HOPPINGWINDOW of SLIDINGWINDOW).

### <a name="method--deaccumulate"></a>Methode – deaccumulate()

De methode deaccumulate() opnieuw berekend op basis van de vorige status en de huidige waarden van de gebeurtenis status. Deze methode wordt aangeroepen wanneer een gebeurtenis een SLIDINGWINDOW verlaat.

### <a name="method--deaccumulatestate"></a>Methode – deaccumulateState()

De methode deaccumulateState() opnieuw berekend op basis van de vorige status en de status van een hop status. Deze methode wordt aangeroepen wanneer een reeks gebeurtenissen laat een HOPPINGWINDOW.

### <a name="method--computeresult"></a>Methode – computeResult()

De methode computeResult() resultaat cumulatieve op basis van de huidige status. Deze methode is aangeroepen aan het einde van een tijdvenster (TUMBLINGWINDOW, HOPPINGWINDOW en SLIDINGWINDOW).

## <a name="javascript-uda-supported-input-and-output-data-types"></a>JavaScript UDA ondersteund invoer en uitvoer-gegevenstypen
Voor de gegevenstypen JavaScript UDA, Raadpleeg het gedeelte **Stream Analytics en JavaScript typen converteren** van [integreren JavaScript UDF's](stream-analytics-javascript-user-defined-functions.md).

## <a name="adding-a-javascript-uda-from-the-azure-portal"></a>Het toevoegen van een JavaScript UDA van de Azure-portal

Hieronder doorlopen we het proces van het maken van een UDA van de Portal. In het voorbeeld dat hier is berekenen van gemiddelden.

Nu gaan we een JavaScript UDA onder een bestaande ASA-taak maken door stap.

1. Aanmelden bij Azure portal en zoek uw bestaande Stream Analytics-taak.
1. Klik vervolgens op de koppeling onder functies **taak TOPOLOGIE**.
1. Klik op de **toevoegen** pictogram voor het toevoegen van een nieuwe functie.
1. Selecteer op de nieuwe functie-weergave, **JavaScript UDA** als het Type functie vervolgens ziet u een standaardsjabloon UDA weergegeven in de editor.
1. Invullen 'TWA' als de UDA-alias en wijzig de implementatie van de functie als het volgende:

    ````JavaScript
    // Sample UDA which calculate Time-Weighted Average of incoming values.
    function main() {
        this.init = function () {
            this.totalValue = 0.0;
            this.totalWeight = 0.0;
        }

        this.accumulate = function (value, timestamp) {
            this.totalValue += value.level * value.weight;
            this.totalWeight += value.weight;

        }

        // Uncomment below for AccumulateDeaccumulate implementation
        /*
        this.deaccumulate = function (value, timestamp) {
            this.totalValue -= value.level * value.weight;
            this.totalWeight -= value.weight;
        }

        this.deaccumulateState = function (otherState){
            this.state -= otherState.state;
            this.totalValue -= otherState.totalValue;
            this.totalWeight -= otherState.totalWeight;
        }
        */

        this.computeResult = function () {
            if(this.totalValue == 0) {
                result = 0;
            }
            else {
                result = this.totalValue/this.totalWeight;
            }
            return result;
        }
    }
    ````

1. Nadat u op de knop 'Opslaan', weergegeven uw UDA op de lijst met de functie.

1. Klik op de nieuwe functie 'TWA', kunt u de functiedefinitie controleren.

## <a name="calling-javascript-uda-in-asa-query"></a>JavaScript UDA aanroepen in de ASA-query

In Azure-portal en open uw taak, de query bewerken en roep TWA()-functie met een voorvoegsel mandaat 'uda.'. Bijvoorbeeld:

````SQL
WITH value AS
(
    SELECT
    NoiseLevelDB as level,
    DurationSecond as weight
FROM
    [YourInputAlias] TIMESTAMP BY EntryTime
)
SELECT
    System.Timestamp as ts,
    uda.TWA(value) as NoseDoseTWA
FROM value
GROUP BY TumblingWindow(minute, 5)
````

## <a name="testing-query-with-uda"></a>Query met UDA testen

Maakt u een lokale JSON-bestand met de onderstaande inhoud en upload het bestand naar een Stream Analytics-taak hierboven query testen.

````JSON
[
  {"EntryTime": "2017-06-10T05:01:00-07:00", "NoiseLevelDB": 80, "DurationSecond": 22.0},
  {"EntryTime": "2017-06-10T05:02:00-07:00", "NoiseLevelDB": 81, "DurationSecond": 37.8},
  {"EntryTime": "2017-06-10T05:02:00-07:00", "NoiseLevelDB": 85, "DurationSecond": 26.3},
  {"EntryTime": "2017-06-10T05:03:00-07:00", "NoiseLevelDB": 95, "DurationSecond": 13.7},
  {"EntryTime": "2017-06-10T05:03:00-07:00", "NoiseLevelDB": 88, "DurationSecond": 10.3},
  {"EntryTime": "2017-06-10T05:05:00-07:00", "NoiseLevelDB": 103, "DurationSecond": 5.5},
  {"EntryTime": "2017-06-10T05:06:00-07:00", "NoiseLevelDB": 99, "DurationSecond": 23.0},
  {"EntryTime": "2017-06-10T05:07:00-07:00", "NoiseLevelDB": 108, "DurationSecond": 1.76},
  {"EntryTime": "2017-06-10T05:07:00-07:00", "NoiseLevelDB": 79, "DurationSecond": 17.9},
  {"EntryTime": "2017-06-10T05:08:00-07:00", "NoiseLevelDB": 83, "DurationSecond": 27.1},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 91, "DurationSecond": 17.1},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 115, "DurationSecond": 7.9},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 80, "DurationSecond": 28.3},
  {"EntryTime": "2017-06-10T05:10:00-07:00", "NoiseLevelDB": 55, "DurationSecond": 18.2},
  {"EntryTime": "2017-06-10T05:10:00-07:00", "NoiseLevelDB": 93, "DurationSecond": 25.8},
  {"EntryTime": "2017-06-10T05:11:00-07:00", "NoiseLevelDB": 83, "DurationSecond": 11.4},
  {"EntryTime": "2017-06-10T05:12:00-07:00", "NoiseLevelDB": 89, "DurationSecond": 7.9},
  {"EntryTime": "2017-06-10T05:15:00-07:00", "NoiseLevelDB": 112, "DurationSecond": 3.7},
  {"EntryTime": "2017-06-10T05:15:00-07:00", "NoiseLevelDB": 93, "DurationSecond": 9.7},
  {"EntryTime": "2017-06-10T05:18:00-07:00", "NoiseLevelDB": 96, "DurationSecond": 3.7},
  {"EntryTime": "2017-06-10T05:20:00-07:00", "NoiseLevelDB": 108, "DurationSecond": 0.99},
  {"EntryTime": "2017-06-10T05:20:00-07:00", "NoiseLevelDB": 113, "DurationSecond": 25.1},
  {"EntryTime": "2017-06-10T05:22:00-07:00", "NoiseLevelDB": 110, "DurationSecond": 5.3}
]
````

## <a name="get-help"></a>Help opvragen

Voor aanvullende hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)

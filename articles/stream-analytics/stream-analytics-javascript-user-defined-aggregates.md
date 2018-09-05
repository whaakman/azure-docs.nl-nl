---
title: Door de gebruiker gedefinieerde JavaScript-verzamelingen in Azure Stream Analytics
description: Dit artikel wordt beschreven hoe u geavanceerde querymechanismen met de gebruiker gedefinieerde JavaScript-verzamelingen in Azure Stream Analytics uitvoert.
services: stream-analytics
author: rodrigoamicrosoft
ms.author: rodrigoa
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2017
ms.openlocfilehash: bdf5b5188dd584c5eb20f72ff4a98ba6904bc53e
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702371"
---
# <a name="azure-stream-analytics-javascript-user-defined-aggregates-preview"></a>Azure Stream Analytics gebruiker gedefinieerde JavaScript-verzamelingen (Preview)
 
Azure Stream Analytics-de gebruiker gedefinieerde verzamelingen (UDA) die zijn geschreven in JavaScript ondersteunt, kunt u om complexe stateful zakelijke logica te implementeren. U hebt volledige controle over de gegevensstructuur van status, status opeenstapeling, status feit en statistische resultaat berekening in UDA. Het artikel bevat de twee verschillende die JavaScript-UDA-interfaces, stappen voor het maken van een UDA en UDA gebruiken met bewerkingen op basis van een venster in Stream Analytics-query.

## <a name="javascript-user-defined-aggregates"></a>Gebruiker gedefinieerde JavaScript-verzamelingen

Een door de gebruiker gedefinieerde aggregatie wordt gebruikt op een specificatie van het tijdstip voor samenvoegen over de gebeurtenissen in dit venster en produceren van een enkele waarde als resultaat. Er zijn twee typen van Stream Analytics ondersteunt nu AccumulateOnly en AccumulateDeaccumulate UDA-interfaces. Beide typen UDA kunnen worden gebruikt door Tumblingvenster, venster Hopping plaatsvindt en Sliding Window. AccumulateDeaccumulate UDA presteert beter dan AccumulateOnly UDA wanneer gebruikt in combinatie met Hopping plaatsvindt en schuiven venster. U kunt een van de twee typen op basis van de algoritme die u gebruikt.

### <a name="accumulateonly-aggregates"></a>AccumulateOnly statistische functies

AccumulateOnly statistische functies kunnen alleen nieuwe gebeurtenissen naar de status oplopen, deaccumulation van waarden niet wordt toegestaan door de algoritme. Kies dit type aggregatie dat wanneer een gebeurtenis deaccumulate gegevens uit de statuswaarde is niet mogelijk om te implementeren. Hieronder vindt u de JavaScript-sjabloon voor AccumulatOnly statistische functies:

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

AccumulateDeaccumulate statistische functies toestaan deaccumulation van een vorige samengevoegde waarde van de status, bijvoorbeeld, een sleutel / waarde-paar verwijderen uit een lijst met waarden van de gebeurtenis of een waarde van een toestand van de totale som aftrekken. Hieronder vindt u de JavaScript-sjabloon voor AccumulateDeaccumulate statistische functies:

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

## <a name="uda---javascript-function-declaration"></a>UDA - declaratie voor JavaScript-functie

Elke JavaScript-UDA wordt gedefinieerd door de declaratie van een functie-object. Hier volgen de belangrijkste elementen in de definitie van een UDA.

### <a name="function-alias"></a>Functiealias

De functiealias is de UDA-id. Wanneer met de naam in Stream Analytics-query, gebruik altijd UDA alias samen met een 'uda." het voorvoegsel.

### <a name="function-type"></a>Functietype

Voor de UDA, het functietype moet **Javascript-UDA**.

### <a name="output-type"></a>Uitvoertype

Een specifieke typen die Stream Analytics-taak die wordt ondersteund of '' als u wilt verwerken van het type in de query.

### <a name="function-name"></a>Functienaam

De naam van deze functie-object. Naam van de functie moet letterlijk overeenkomen met de UDA-alias (preview-gedrag, we ondersteuning voor anonieme functie overweegt bij algemene beschikbaarheid).

### <a name="method---init"></a>Methode - init()

De methode init() initialiseert de status van de statistische functie. Deze methode wordt aangeroepen wanneer het venster wordt gestart.

### <a name="method--accumulate"></a>Methode – accumulate()

De methode accumulate() berekent de UDA-status op basis van de vorige status en de huidige waarden van de gebeurtenis. Deze methode wordt aangeroepen wanneer een gebeurtenis in een bepaalde periode (TUMBLINGWINDOW, HOPPINGWINDOW of SLIDINGWINDOW).

### <a name="method--deaccumulate"></a>Methode – deaccumulate()

De methode deaccumulate() worden opnieuw berekend op basis van de vorige status en de huidige waarden van de gebeurtenis status. Deze methode wordt aangeroepen wanneer een gebeurtenis een SLIDINGWINDOW verlaat.

### <a name="method--deaccumulatestate"></a>Methode – deaccumulateState()

De methode deaccumulateState() worden opnieuw berekend op basis van de vorige status en de status van een hop staat. Deze methode wordt aangeroepen wanneer een reeks gebeurtenissen laat een HOPPINGWINDOW.

### <a name="method--computeresult"></a>Methode – computeResult()

De methode computeResult() retourneert geaggregeerde resultaat op basis van de huidige status. Deze methode wordt aangeroepen aan het einde van een bepaalde periode (TUMBLINGWINDOW, HOPPINGWINDOW en SLIDINGWINDOW).

## <a name="javascript-uda-supported-input-and-output-data-types"></a>JavaScript-UDA ondersteunde invoer- en gegevenstypen
Zie sectie voor JavaScript-UDA-gegevenstypen, **typeconversie van Stream Analytics en JavaScript** van [JavaScript-UDF's integreren](stream-analytics-javascript-user-defined-functions.md).

## <a name="adding-a-javascript-uda-from-the-azure-portal"></a>Een JavaScript-UDA toevoegen vanuit de Azure-portal

Hieronder stapsgewijs we door het proces voor het maken van een UDA vanuit de Portal. Het voorbeeld we hier gebruiken is tijdgewogen gemiddelden computing.

Nu gaan we een JavaScript-UDA onder een bestaande ASA-taak maken door de stappen te volgen.

1. Meld u aan bij Azure portal en zoek uw bestaande Stream Analytics-taak.
1. Klik vervolgens op de koppeling onder functies **TAAKTOPOLOGIE**.
1. Klik op de **toevoegen** pictogram voor het toevoegen van een nieuwe functie.
1. Selecteer op de nieuwe functie-weergave, **JavaScript-UDA** als het Type functie vervolgens ziet u een standaardsjabloon UDA weergegeven in de editor.
1. Vul "TWA' als de UDA-alias en wijzigt u de functie-implementatie als het volgende:

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

1. Zodra u op de knop 'Opslaan' klikt, wordt de status van uw UDA wordt weergegeven in de lijst met de functie.

1. Klik op de nieuwe functie 'TWA', kunt u de functiedefinitie controleren.

## <a name="calling-javascript-uda-in-asa-query"></a>JavaScript-UDA aanroepen in de ASA-query

In Azure portal en open uw taak, de query bewerken en roep TWA()-functie met een voorvoegsel voor opdracht 'uda.'. Bijvoorbeeld:

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

## <a name="testing-query-with-uda"></a>Testen van query's uitvoeren met UDA

Maken van een lokaal JSON-bestand met de onderstaande inhoud en upload het bestand naar Stream Analytics-taak testen boven de query.

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

Meer hulp vindt u mogelijk op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language-referentie](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)

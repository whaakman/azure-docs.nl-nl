---
title: Batch test uw LUIS-app - Azure | Microsoft Docs
description: Gebruik batch testen om te werken continu op uw toepassing het verfijnen en verbeteren van de taal begrijpen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/06/2018
ms.author: diberry
ms.openlocfilehash: bba3f2ff942fbe5dffc9b694990964e4e3078dbe
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222650"
---
# <a name="batch-testing-in-luis"></a>Batch testen in LUIS

Batch testen valideert uw [active](luis-concept-version.md#active-version) getrainde model voor het meten van de nauwkeurigheid. Een batch-test, kunt u de nauwkeurigheid van de intentie en de entiteit in uw huidige, getrainde model in een grafiek weergeven. De resultaten van de batch voor de passende maatregelen nemen om de nauwkeurigheid, zoals het toevoegen van meer voorbeeld uitingen aan een doel als uw app vaak mislukt voor het identificeren van de juiste intentie te bekijken.

## <a name="group-data-for-batch-test"></a>Groepsgegevens voor batch-test
Het is belangrijk dat uitingen die wordt gebruikt voor het testen van de batch niet bekend bent met LUIS. Als u een gegevensset van uitingen hebt, waardoor de uitingen in drie groepen: uitingen toegevoegd aan een doel en uitingen ontvangen van het eindpunt van de gepubliceerde uitingen gebruikt voor het batch-test LUIS nadat deze is getraind. 

## <a name="a-dataset-of-utterances"></a>Een gegevensset van uitingen
Indienen van een batch-bestand van uitingen, ook wel een *gegevensset*, voor het testen van batch. De gegevensset is een JSON-indeling bestand met maximaal 1000 met het label **unieke** uitingen. U kunt maximaal 10 gegevenssets testen in een app. Als u nodig hebt voor het testen van meer, een gegevensset verwijdert en vervolgens een nieuwe toevoegen.

|**Regels**|
|--|
|* Er zijn geen dubbele uitingen|
|Er is geen onderliggende hiërarchische entiteit|
|uitingen van 1000 of minder|

* Dubbele waarden worden beschouwd als de exacte tekenreeks komt overeen met, niet overeenkomt met die eerst worden getokeniseerd. 

## <a name="entities-allowed-in-batch-tests"></a>Entiteiten die zijn toegestaan in de batchtests
Entiteiten zijn eenvoudige, hiërarchische ouders en samengesteld. Alle entiteiten van de volgende typen worden weergegeven in het filter entiteiten van batch test, zelfs als er geen bijbehorende entiteiten in de batch-bestand zijn.


<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Batch-bestandsindeling
De batch-bestand bestaat uit uitingen. Elke utterance ze beschikken over een verwachte intentie voorspelling samen met een [machine geleerde entiteiten](luis-concept-entity-types.md#types-of-entities) u verwacht te worden gedetecteerd. 

Hier volgt een voorbeeld van een batch-bestand met de juiste syntaxis:

```JSON
[
  {
    "text": "Are there any janitorial jobs currently open?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 14,
            "endPos": 23
        }
    ]
  },
  {
    "text": "I would like a fullstack typescript programming with azure job",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 15,
            "endPos": 46
        }
    ]
  },
  {
    "text": "Is there a database position open in Los Colinas?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 11,
            "endPos": 18
        }
    ]
  },
  {
    "text": "Please find database jobs open today in Seattle",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 12,
            "endPos": 19
        }
    ]
  }
]
```

## <a name="batch-syntax-template"></a>Batch syntaxis van de sjabloon

Gebruik de volgende sjabloon om te starten van uw batch-bestand:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": 
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

De batch-bestand maakt gebruik van de **startPos** en **endPos** eigenschappen om te weten het begin en einde van een entiteit. De waarden mag op nul gebaseerde zijn en niet beginnen of eindigen op een spatie. 

Dit wijkt af van de logboeken voor query's, die startIndex en endIndex eigenschappen gebruiken. 


## <a name="common-errors-importing-a-batch"></a>Veelvoorkomende fouten in een batch importeren
Veelvoorkomende fouten zijn onder andere: 

> * Uitingen van meer dan 1000
> * Een utterance JSON-object dat niet een eigenschap entiteiten hebben
> * Woorden die met het label in meerdere entiteiten

## <a name="batch-test-state"></a>Status van de batch-test
LUIS houdt de status van de laatste test van elke gegevensset. Dit omvat de grootte (aantal uitingen in de batch), voor het laatst uitgevoerd datum en de laatste resultaat (aantal is voorspelde uitingen).

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>De resultaten van batch
Het resultaat van de test batch is een spreidingsgrafiek bekend als een matrix van de fout. Deze grafiek bevat een vergelijking 4-weg van de uitingen in het bestand en het huidige model voorspelde intentie en entiteiten. 

Gegevenspunten op de **ONWAAR positief** en **False negatieve** secties duiden op fouten, die moeten worden onderzocht. Als u alle gegevenspunten zijn op de **waar positieve** en **waar negatieve** secties, en vervolgens de nauwkeurigheid van uw app is bij uitstek geschikt voor deze gegevensset.

![Vier secties van grafiek](./media/luis-concept-batch-test/chart-sections.png)

In deze grafiek helpt u uitingen die LUIS voorspelt ten onrechte op basis van de huidige training te vinden. De resultaten worden weergegeven per regio van de grafiek. Selecteer afzonderlijke punten in de grafiek Lees de informatie utterance of selecteer regionaam utterance resultaten in die regio bekijken.

![Batchgewijs testen](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Fouten in de resultaten
Fouten in de batch-test duiden intents die niet worden voorspeld, zoals vermeld in de batch-bestand. Fouten worden aangegeven in de twee rode secties van de grafiek. 

De waarde false positieve sectie geeft aan dat een utterance overeenkomen met een doel of de entiteit als het al dan niet mogen hebben. De negatieve waarde false geeft aan dat een utterance komt niet overeen met een doel of de entiteit wanneer deze moet hebben. 

## <a name="fixing-batch-errors"></a>Batchfouten oplossen
Als er fouten in de batch-tests, u kunt een meer utterances toevoegen aan een doel en/of meer uitingen aan de entiteit te maken van het onderscheid tussen intents LUIS label. Als u hebt toegevoegd uitingen en deze en nog steeds get gelabeld voorspelling fouten in de batch testen, kunt u toevoegen een [woordgroepenlijst](luis-concept-feature.md) -functie met domeinspecifieke vocabulaire om u te helpen sneller meer LUIS. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [testen van een batch](luis-how-to-batch-test.md)

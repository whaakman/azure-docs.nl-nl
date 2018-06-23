---
title: Uw LUIS voor app - Azure batch testen | Microsoft Docs
description: Gebruik de batch voor testdoeleinden continu werken op uw toepassing voor het verfijnen en taal inzicht te verbeteren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 3803df32d6431b8413e8df0837ed62b2e4344cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345063"
---
# <a name="batch-testing-in-luis"></a>Batch in LUIS testen

Tests in batch valideert uw [active](luis-concept-version.md#active-version) getrainde model voor het meten van de nauwkeurigheid. Een batch-test kunt u de nauwkeurigheid van elke doel en de entiteit in uw huidige getraind model in een grafiek weergeven. Bekijk de testresultaten van batch om passende maatregelen te verbeteren, nauwkeurigheid, zoals het toevoegen van meer voorbeeld utterances aan de opzet als uw app vaak niet de juiste bedoeling identificeren.

## <a name="group-data-for-batch-test"></a>Groepsgegevens voor batch-test
Het is belangrijk dat utterances gebruikt voor het testen van de batch niet bekend met LUIS bent. Als u een gegevensset van utterances hebt, deelt de utterances in drie sets: utterances toegevoegd aan de opzet, utterances ontvangen van het gepubliceerde eindpunt en utterances gebruikt voor het batch-test LUIS nadat deze is getraind. 

## <a name="a-dataset-of-utterances"></a>Een gegevensset van utterances
Verzenden van een batch-bestand van utterances, ook wel een *gegevensset*, voor het testen van de batch. De gegevensset is een JSON-indeling bestand met een maximum van 1000 gelabeld **niet dubbele** utterances. U kunt maximaal 10 gegevenssets testen in een app. Als u testen meer wilt, een gegevensset verwijderen en voegt u een nieuwe.

|**Regels**|
|--|
|* Geen dubbele utterances|
|Er is geen onderliggende hiërarchische entiteit|
|utterances 1000 of minder|

* Duplicaten worden beschouwd als de exacte tekenreeks overeenkomt met, niet overeenkomt met die eerst worden getokeniseerd. 

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Batch-bestandsindeling
Het batchbestand bestaat uit utterances. Elke utterance moet een verwachte opzet prediction samen hebben [machine geleerde entiteiten](luis-concept-entity-types.md#types-of-entities) u verwacht te worden gedetecteerd. 

Hier volgt een voorbeeld van batchbestand:

   [!code-json[Valid batch test](~/samples-luis/documentation-samples/batch-testing/travel-agent-1.json)]


## <a name="common-errors-importing-a-batch"></a>Veelvoorkomende fouten bij het importeren van een batch
Veelvoorkomende fouten zijn onder andere: 

> * Meer dan 1000 utterances
> * Een utterance JSON-object dat geen van de eigenschap van een entiteiten

## <a name="batch-test-state"></a>Teststatus batch
LUIS houdt de status van de laatste test elke gegevensset. Dit omvat de datum van de grootte (aantal utterances in de batch), het laatst is uitgevoerd, en het laatste resultaat (aantal met succes voorspelde utterances).

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>Batch-testresultaten
Het resultaat van de test batch is een spreidingsgrafiek bekend als een matrix van de fout. Deze grafiek is een 4-weg vergelijking van de utterances in het bestand en het huidige model voorspelde bedoeling en entiteiten. 

Gegevenspunten op de **onjuiste positieve** en **False negatieve** secties duiden op fouten, die moeten worden onderzocht. Als alle gegevenspunten op de **True positieve** en **True negatieve** secties, en vervolgens de nauwkeurigheid van uw app is bij uitstek geschikt voor deze gegevensset.

![Vier secties van de grafiek](./media/luis-concept-batch-test/chart-sections.png)

Dit diagram kunt u utterances die LUIS voorspelt onjuist op basis van de huidige training vinden. De resultaten worden weergegeven per regio van de grafiek. Selecteer de afzonderlijke punten op de grafiek Lees de informatie utterance of regionaam te bekijken van resultaten utterance in deze regio selecteren.

![Batch testen](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Fouten in de resultaten
Fouten in de batch-test duiden intents die niet zijn voorspeld zoals aangegeven in de batch-bestand op. Fouten worden aangeduid in de twee rode secties van de grafiek. 

De waarde false positief sectie geeft aan dat een utterance komt overeen met een doel of de entiteit wanneer het mag geen. De waarde false negatief geeft een utterance komt niet overeen met een doel of de entiteit wanneer heeft. 

## <a name="fixing-batch-errors"></a>Batchfouten
Als er fouten in de batch-tests zijn, u kunt ofwel meer utterances toevoegen aan de opzet en/of meer utterances aan de entiteit bij het maken van het onderscheid tussen intents LUIS labelen. Als u hebt toegevoegd utterances en gelabeld ze en toch get voorspelling fouten in het testen van batch, overweeg een [woordgroepenlijst](luis-concept-feature.md) onderdeel met domeinspecifieke vocabulaire LUIS leert sneller help. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [testen van een batch](luis-how-to-batch-test.md)
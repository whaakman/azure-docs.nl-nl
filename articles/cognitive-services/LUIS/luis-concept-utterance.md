---
title: Utterances in LUIS apps in Azure | Microsoft Docs
description: Utterances in Language Understanding Intelligent Service (LUIS) apps toevoegen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/13/2018
ms.author: v-geberr
ms.openlocfilehash: 66a23876eebe177c767b20f60f86891c35da3385
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301859"
---
# <a name="utterances-in-luis"></a>Utterances in LUIS

**Utterances** zijn invoer van de gebruiker die uw app moet worden geïnterpreteerd. Als u wilt trainen LUIS intents en entiteiten ophalen uit deze, is het belangrijk voor het vastleggen van tal van verschillende ingangen voor elk doel. Actieve learning of het proces voor u doorgaat met het trainen op nieuwe utterances is essentieel voor de machine geleerd intelligence LUIS biedt.

Verzamel zinnen die u denkt dat gebruikers wordt ingevoerd. Utterances die zijn gebouwd, maar dezelfde betekenis anders in word lengte en de plaatsing van de word opnemen. 

## <a name="how-to-choose-varied-utterances"></a>Uiteenlopende utterances kiezen
Wanneer u eerst aan de slag door [toe te voegen voorbeeld utterances] [ add-example-utterances] aan uw model LUIS hier zijn enkele principes rekening moet houden.

### <a name="utterances-arent-always-well-formed"></a>Utterances zijn niet altijd goed gevormd.
Het is mogelijk een zin, zoals 'Boek mij een ticket naar Parijs' of een fragment van een zin, zoals 'Reservering' of "Parijs vlucht."  Gebruikers maken vaak spellingcontrole uit te voeren. Overweeg of u spellingcontrole gebruikersinvoer voordat deze aan LUIS doorgegeven bij het plannen van uw app. De [Bing spellen controleren API] [ BingSpellCheck] kan worden geïntegreerd met LUIS. U kunt uw app LUIS koppelen met een externe sleutel voor de Bing-API voor het controleren van spellen wanneer u deze hebt gepubliceerd. Als het selectievakje gebruiker utterances niet is gespeld, moet u LUIS trainen op utterances die typefouten en typefouten bevatten.

### <a name="use-the-representative-language-of-the-user"></a>Gebruik de representatieve taal van de gebruiker
Wanneer u kiest utterances, zich bewust dat wat u denkt dat is een algemene term of woordgroep mogelijk niet op de normale gebruiker van de clienttoepassing. Zij mag geen domein ervaring hebben. Dus wees voorzichtig bij het gebruik van termen of woordgroepen dat een gebruiker alleen spreken zou als ze een expert zijn.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Kies uiteenlopende terminologie als formulering
U ziet dat zelfs als u inspanningen om uiteenlopende zin patronen te maken, nog steeds sommige woordenlijst herhalen wordt.

Deze utterances voorbeeld nemen:
```
how do I get a computer?
Where do I get a computer?
I want to get a computer, how do I go about it?
When can I have a computer? 
```
De term core hier is 'computer' niet gewijzigd. Ze kunnen desktopcomputer, laptop, werkstation of zelfs alleen machine spreken. LUIS infereert op intelligente wijze synoniemen uit de context, maar wanneer u utterances voor training maakt, is het nog beter te variëren.

## <a name="example-utterances-in-each-intent"></a>Voorbeeld utterances in elk doel
Elk doel moet voorbeeld utterances, ten minste 10 tot 15 hebben. Als u een doel dat geen eventuele utterances voorbeeld hebt, kunt u zich niet LUIS trainen. Als u de opzet met één of enkele voorbeeld utterances hebt, wordt het doel niet nauwkeurig voorspellen door LUIS. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Kleine groepen 10-15 utterances voor elke herhaling authoring toevoegen
In elke herhaling van het model, Voeg een grote hoeveelheid utterances geen. Utterances in hoeveelheden tientallen toevoegen. [Train](luis-how-to-train.md), [publiceren](publishapp.md), en [testen](interactive-test.md) opnieuw.  

LUIS bouwt effectieve modellen met utterances die zorgvuldig zijn geselecteerd. Het toevoegen van te veel utterances is geen waardevolle omdat hierdoor verwarring.  

Het is beter om te beginnen met een paar utterances vervolgens [eindpunt utterances bekijken](label-suggested-utterances.md) voor het juiste opzet voorspelling en entiteit ophalen.

## <a name="ignoring-words-and-punctuation"></a>Woorden en leestekens worden genegeerd
Als u negeren specifieke woorden of leestekens in het voorbeeld utterance wilt, gebruikt u een [patroon](luis-concept-patterns.md#pattern-syntax) met de _negeren_ syntaxis. 

## <a name="training-utterances"></a>Training utterances
Training is niet-deterministisch: de voorspelling utterance kan enigszins verschillen voor verschillende versies of apps.

## <a name="testing-utterances"></a>Utterances testen 

Ontwikkelaars moeten beginnen met het testen van hun toepassing LUIS met echte verkeer door te sturen utterances naar het eindpunt. Deze utterances worden gebruikt voor het verbeteren van de intents en entiteiten met [utterances controleren](label-suggested-utterances.md). Tests die worden verzonden met de website van de LUIS deelvenster testen niet worden verzonden via het eindpunt en dus niet bijdragen tot actieve learning. 

## <a name="review-utterances"></a>Utterances controleren
Nadat uw model getraind en gepubliceerd ontvangende is [eindpunt](luis-glossary.md#endpoint) query's [controleren de utterances](label-suggested-utterances.md) LUIS worden voorgesteld. LUIS selecteert eindpunt utterances waarvoor lage scores voor het doel of de entiteit. 

## <a name="best-practices"></a>Aanbevolen procedures
Bekijk [aanbevolen procedures](luis-concept-best-practices.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Zie [voorbeeld utterances toevoegen] [ add-example-utterances] voor meer informatie over het trainen van een app LUIS gebruiker utterances begrijpen.

[add-example-utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-add-example-utterances
[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text
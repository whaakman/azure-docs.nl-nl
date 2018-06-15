---
title: Functies in LUIS apps in Azure begrijpen | Microsoft Docs
description: Meer informatie over de functies, die een LUIS-app-prestaties te verbeteren. Functies omvatten woordgroep lijsten en patronen voor het herkennen van reguliere expressies.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/18/2018
ms.author: v-geberr
ms.openlocfilehash: 416fadaf52d7a71dcaab81aab3bf6099213e5af5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35350056"
---
# <a name="phrase-list-features-in-luis"></a>Woordgroep lijst met functies in LUIS

In machine learning, een *functie* is een onderscheidende kenmerk hebben of kenmerk van de gegevens die uw systeem toetsenbordinvoer. 

Functies toevoegen aan een taalmodel hints over het herkennen van invoer die u wilt markeren of classificeren geven. Functies zorgen LUIS herkent zowel intents en entiteiten, maar functies zijn niet intents of entiteiten zelf. Functies mogelijk in plaats daarvan vindt u voorbeelden van de bijbehorende voorwaarden.  

## <a name="what-is-a-phrase-list-feature"></a>Wat is een functie van de lijst woordgroep?
Een woordgroepenlijst bevat een groep waarden (woorden of zinnen) die deel uitmaken van dezelfde klasse en op dezelfde manier (bijvoorbeeld namen van steden of producten) moet worden behandeld. LUIS gegevens verzamelt over één hiervan wordt automatisch toegepast op de andere. Dit is niet gesloten [entiteit lijst](luis-concept-entity-types.md#types-of-entities) (exacte tekst) van overeenkomende woorden.

Een woordgroepenlijst wordt toegevoegd aan de woordenlijst van het domein van de app als een tweede signaal LUIS over deze woorden.

## <a name="how-to-use-phrase-lists"></a>Het gebruik van een lijst met woordgroep
Maken in een app reizen agent een wachtwoordzin op te geven met de naam 'Steden' lijst die de waarden, Londen, Parijs en Caïro bevat. Als u een van deze waarden als een eenvoudige entiteit in een label een [voorbeeld utterance](luis-how-to-add-example-utterances.md#add-simple-entity-label) in de opzet LUIS leert voor het herkennen van de andere. 

Een woordgroepenlijst mogelijk uitwisselbaar of niet-verwisselbaar. Een *uitwisselbaar* woordgroepenlijst is voor waarden die synoniemen zijn, en een *niet uitwisselbaar* woordgroepenlijst is bedoeld voor waarden die niet zijn synoniemen maar lijken op een andere manier. 

## <a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>Woordgroep bevat help eenvoudige exchangeable entiteiten identificeren
Exchangeable woordgroep lijsten zijn een goede manier om het afstemmen van de prestaties van uw app LUIS. Als uw app problemen met het voorspellen van utterances naar de juiste bedoeling of te herkennen entiteiten heeft, moet u bedenken dat de utterances ongebruikelijke woorden of woorden die mogelijk niet eenduidig in betekenis bevatten. Deze woorden zijn goede kandidaten moeten worden opgenomen in een lijst van de wachtwoordzin op te geven.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Woordgroep bevat help intents identificeren door betere understanding context
Woordgroep lijsten voor zeldzame, eigen en externe woorden gebruiken. LUIS is mogelijk niet worden herkend zeldzame en eigen woorden, evenals vreemde woorden (buiten de cultuur van de app) en daarom ze moeten worden toegevoegd aan de lijst van een wachtwoordzin op te geven. Deze woordgroepenlijst moet de status niet-verwisselbaar, om aan te geven dat de set van zeldzame woorden een klasse die LUIS moet meer vormt herkennen, maar ze geen synoniemen zijn of uitwisselbaar met elkaar.

Een woordgroepenlijst is niet een instructie voor LUIS uitvoeren strikte overeenkomende of alle voorwaarden in de woordgroepenlijst altijd label precies hetzelfde. Het is gewoon een hint. Bijvoorbeeld, kan er een woordgroepenlijst die aangeeft dat 'Patti' en 'Selma' namen zijn, maar LUIS nog steeds contextuele informatie kunt gebruiken voor het herkennen van dat ze iets anders in "Maak een reservering voor 2 op de Patti Diner voor diner" en "vijf me besturen betekenen aanwijzingen voor het Selma, Georgia'. 

Toevoegen van een woordgroepenlijst vormt een alternatief voor het meer voorbeeld utterances toe te voegen aan de opzet. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Wanneer gebruikt u lijsten woordgroep versus lijst entiteiten
Een woordgroepenlijst zowel lijst entiteiten kunnen utterances over alle intents beïnvloeden, wordt elk dit op een andere manier. Gebruik een woordgroep lijst met opzet voorspelling score beïnvloeden. Een entiteit lijst gebruiken bij het uitpakken van de entiteit voor een overeenkomst exact tekst. 

### <a name="use-a-phrase-list"></a>Een woordgroepenlijst gebruiken
LUIS kan nog steeds een lijst woordgroep rekening gehouden met context en generalize om te identificeren van items die vergelijkbaar zijn met, maar niet een exacte overeenkomst als items in een lijst. Als u uw app LUIS kunnen generaliseren en identificeren van nieuwe items in een categorie nodig hebt, moet u een woordgroepenlijst gebruiken. 

Als u kunnen herkennen van nieuwe exemplaren van een entiteit, zoals een vergadering scheduler die herkennen wilt, moet de namen van nieuwe contactpersonen of een inventarisatie-app die, nieuwe producten herkennen moet gebruiken een ander type entiteit machine geleerd zoals een eenvoudige of hiërarchische entiteit. Maak vervolgens een woordgroepenlijst met woorden en woordgroepen waarmee LUIS andere woorden zoeken die lijken op de entiteit. Deze lijst begeleidt LUIS dat voorbeelden van de entiteit wordt herkend door aanvullende significante toe te voegen aan de waarde van deze woorden. 

Woordgroep lijsten zijn zoals domeinspecifieke woordenlijst die u helpen bij het verbeteren van de kwaliteit van de kennis van intents en -entiteiten. Een algemene informatie over het gebruik van een woordgroepenlijst is juiste woorden zoals de plaatsnamen van. Naam van een stad mag verschillende woorden inclusief afbreekstreepjes of enkele aanhalingstekens.
 
### <a name="dont-use-a-phrase-list"></a>Een woordgroepenlijst niet gebruiken 
Een entiteit van de lijst wordt elke waarde in een entiteit kan duren, en identificeert alleen waarden die exact overeenkomen. Een entiteit lijst mogelijk geschikt is voor een app waarin alle exemplaren van een entiteit bekend zijn en vaak, zoals de voeding items in een restaurantmenu die niet vaak veranderen niet te wijzigen. Als u een overeenkomst exact tekst van een entiteit moet, gebruik niet een lijst van de wachtwoordzin op te geven. 

## <a name="best-practices"></a>Aanbevolen procedures
Meer informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

Zie [onderdelen toevoegen](luis-how-to-add-features.md) voor meer informatie over het toevoegen van functies aan uw app LUIS.
---
title: Functies van LUIS-apps in Azure Cognitive Services
titleSuffix: Azure Cognitive Services
description: Functies toevoegen aan een taalmodel voor tips over het herkennen van de invoer die u wilt een label of classificeren. De functies kunt LUIS herkent zowel intenties en entiteiten.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 906e63c94666d2e24b486f5ae34f99133408c4dc
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45628706"
---
# <a name="phrase-list-features-in-luis"></a>Woordgroep lijst met functies in LUIS

In machine learning, een *functie* is een onderscheidende eigenschap of kenmerk van de gegevens die uw systeem toetsenbordinvoer. 

Functies toevoegen aan een taalmodel voor tips over het herkennen van de invoer die u wilt een label of classificeren. Functies helpen LUIS zowel intenties en entiteiten herkennen, maar functies zijn niet intents of entiteiten zelf. Functies mogelijk in plaats daarvan vindt u voorbeelden van de bijbehorende voorwaarden.  

## <a name="what-is-a-phrase-list-feature"></a>Wat is een functie van de lijst woordgroep?
Een woordgroepenlijst bevat een reeks waarden (woorden of zinsdelen) die deel uitmaken van dezelfde klasse en op dezelfde manier (bijvoorbeeld de namen van steden of producten) moet worden behandeld. LUIS leert over een van beide wordt automatisch toegepast op de andere. Deze lijst is niet een gesloten [entiteit lijst](luis-concept-entity-types.md#types-of-entities) (exacte overeenkomsten tekst) van de overeenkomende woorden.

Een woordgroepenlijst wordt toegevoegd aan het vocabulaire van het domein van de app als een tweede signaal dat moet worden LUIS over deze woorden.

## <a name="how-to-use-phrase-lists"></a>Het gebruik van een lijst met woorden
In de Human Resource-app [eenvoudige entiteit zelfstudie](luis-quickstart-primary-and-secondary-data.md), de app gebruikt een **taak** taaktypen zoals programmeur roofer en secretary woordgroepenlijst. Als u een van deze waarden als een entiteit hebt geleerd van een machine label, leren LUIS te herkennen de andere. 

Een woordgroepenlijst kan niet onderling verwisselbaar of niet-verwisselbaar. Een *uitwisselbaar* woordgroepenlijst is voor waarden die synoniemen worden, en een *niet uitwisselbaar* woordgroepenlijst is bedoeld voor waarden die niet van synoniemen, maar nog steeds een extra signaal in de app moeten. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>
## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Woordgroep geeft een lijst van de help eenvoudige uitwisselbaar entiteiten identificeren
Verwisselbaar woordgroep lijsten zijn een goede manier om af te stemmen van de prestaties van uw LUIS-app. Als uw app problemen bij het voorspellen van uitingen met de juiste intent of entiteiten herkennen heeft, moet u bedenken dat de uitingen ongebruikelijke woorden of woorden die mogelijk niet-eenduidige in betekenis bevatten. Deze woorden zijn goede kandidaten om op te nemen in een woordgroepenlijst met.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Woordgroep geeft een lijst van de help intents identificeren door beter inzicht in context
Een woordgroepenlijst is niet een instructie aan LUIS uit te voeren strikte die overeenkomen met of alle voorwaarden in de woordgroepenlijst met altijd label precies hetzelfde. Het is gewoon een hint. Bijvoorbeeld, u kunt een woordgroepenlijst die aangeeft dat "Patti" en "Selma" namen zijn hebben, maar LUIS kunt contextuele informatie nog steeds gebruiken voor het herkennen van dat ze iets anders in betekenen 'Maak een reservering voor 2 op de Patti Diner voor diner' en 'vinden me te stimuleren aanwijzingen voor het Selma, Georgië'. 

Toevoegen van een woordgroepenlijst vormt een alternatief voor het toevoegen van meer voorbeeld uitingen aan een doel. 

## <a name="an-interchangeable-phrase-list"></a>Een woordgroepenlijst met uitwisselbaar
Een woordgroepenlijst uitwisselbaar gebruiken wanneer de lijst met woorden of fasen maakt een klasse of groep. Een voorbeeld is een lijst van maanden, zoals "Januari", 'Februari', 'Maart'; of namen, zoals 'John', 'Primaire', 'Frank'.  Deze lijsten zijn verwisselbaar in dat de utterance zou worden gelabeld met hetzelfde doel of entiteit als een ander woord in de woordgroepenlijst met zijn gebruikt. Bijvoorbeeld, als 'de agenda weergeven voor januari"heeft dezelfde intentie als 'Toon de agenda voor februari' en vervolgens de woorden moeten zich op een verwisselbare-lijst. 

## <a name="a-non-interchangeable-phrase-list"></a>Een woordgroepenlijst met niet-verwisselbaar
Gebruik een woordgroepenlijst met niet-verwisselbaar voor niet-hoczoekmogelijkheden woorden of zinsdelen die kunnen worden gegroepeerd in uw domein. 

Als een voorbeeld gebruikt u een woordgroepenlijst met niet-verwisselbaar zeldzaam, eigen en externe woorden. LUIS is mogelijk niet worden herkend zeldzame en eigen woorden, evenals de woorden in vreemde talen (buiten de cultuur van de app). De niet-verwisselbaar instelling geeft aan dat de set zeldzaam woorden vormt een klasse die LUIS moet leren herkennen, maar ze niet synoniemen zijn of uitwisselbaar met elkaar.

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Wanneer u lijsten woordgroep ten opzichte van de lijst met entiteiten
Zowel een woordgroep en lijst met entiteiten kunnen uitingen over alle intents beïnvloeden, elk gebeurt op een andere manier. Een woordgroepenlijst met gebruiken om te bepalen van intentie voorspelling score. Een lijst met entiteit gebruiken voor het extraheren van entiteiten af voor een overeenkomst exact overeenkomende tekst van invloed zijn op. 

### <a name="use-a-phrase-list"></a>Gebruik een zin-lijst
LUIS kunt nog steeds met een woordgroepenlijst rekening gehouden met context en generaliseren voor het identificeren van items die vergelijkbaar zijn met, maar niet een exacte overeenkomst als items in een lijst. Als u uw LUIS-app moet kunnen generaliseren en nieuwe items in een categorie te identificeren, gebruikt u een woordgroepenlijst met. 

Als u mogelijk wilt voor het herkennen van nieuwe instanties van een entiteit, zoals een vergadering scheduler die de namen van nieuwe contactpersonen of een inventarisatie-app die, nieuwe producten herkennen moet, moet herkennen gebruiken een ander type machine geleerd entiteit, zoals een eenvoudige of hiërarchische entiteit. Maak vervolgens een woordgroepenlijst met woorden en zinnen waarmee LUIS andere woorden die vergelijkbaar is met de entiteit zoeken. Deze lijst begeleidt LUIS zodat voorbeelden van de entiteit worden herkend door extra betekenis toe te voegen aan de waarde van deze woorden. 

Een lijst met woorden zijn domeinspecifieke-woordenlijst die u helpen bij het verbeteren van de kwaliteit van het inzicht van zowel intenties en entiteiten. Een veelvoorkomend gebruik van een woordgroepenlijst is de juiste zelfstandige naamwoorden, zoals plaatsnamen. Naam van een plaats kan meerdere woorden, inclusief afbreekstreepjes of apostroffen zijn.
 
### <a name="dont-use-a-phrase-list"></a>Gebruik niet een woordgroepenlijst met 
Een lijst met entiteiten definieert expliciet elke waarde in een entiteit kan duren, en identificeert alleen waarden die precies overeenkomen. Een lijst met entiteit mogelijk geschikt is voor een app waarin alle exemplaren van een entiteit bekend zijn en niet vaak te wijzigen. Voorbeelden zijn food items in een restaurantmenu die veelvuldig wordt gewijzigd. Als u een overeenkomst exact overeenkomende tekst van een entiteit nodig hebt, gebruik niet een woordgroepenlijst met. 

## <a name="best-practices"></a>Aanbevolen procedures
Informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

Zie [onderdelen toevoegen](luis-how-to-add-features.md) voor meer informatie over hoe u functies toevoegt aan uw LUIS-app.

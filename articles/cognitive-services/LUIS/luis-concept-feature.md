---
title: Functies
titleSuffix: Language Understanding - Azure Cognitive Services
description: Functies toevoegen aan een taalmodel voor tips over het herkennen van de invoer die u wilt een label of classificeren.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/04/2019
ms.author: diberry
ms.openlocfilehash: fa0a8c4baa9458a3784babe1e342d768748f8a4d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882451"
---
# <a name="phrase-list-features-in-your-luis-app"></a>Woordgroep lijst met functies in uw LUIS-app

In machine learning, een *functie* is een onderscheidende eigenschap of kenmerk van de gegevens die uw systeem toetsenbordinvoer. 

Functies toevoegen aan een taalmodel voor tips over het herkennen van de invoer die u wilt een label of classificeren. Functies helpen LUIS zowel intenties en entiteiten herkennen, maar functies zijn niet intents of entiteiten zelf. Functies mogelijk in plaats daarvan vindt u voorbeelden van de bijbehorende voorwaarden.  

## <a name="what-is-a-phrase-list-feature"></a>Wat is een functie van de lijst woordgroep?
Een woordgroepenlijst wordt een lijst met woorden of zinsdelen die belangrijk om uw app zijn, meer dus dan andere woorden in uitingen. Een woordgroepenlijst wordt toegevoegd aan het vocabulaire van het domein van de app als een aanvullende signaal dat moet worden LUIS over deze woorden. LUIS leert over een van beide wordt automatisch toegepast op de andere. Deze lijst is niet een gesloten [entiteit lijst](luis-concept-entity-types.md#types-of-entities) van exact overeenkomende tekst komt overeen met.

Woordgroep lijsten niet hebt kunnen met als gevolg van daarom voor utterance voorbeelden die gebruikmaken van tal van afleiding voor een aanzienlijke vocabulaire woorden en woordgroepen toevoegen.

## <a name="phrase-lists-help-all-models"></a>Woordgroep lijsten helpen alle modellen

Woordgroep lijsten niet zijn gekoppeld aan een bepaald doel of de entiteit, maar als een aanzienlijke verbetering zijn toegevoegd aan alle intenties en entiteiten. Het doel hiervan is voor het verbeteren van intentie detectie en entiteit classificatie.

## <a name="how-to-use-phrase-lists"></a>Het gebruik van een lijst met woorden

Een woordgroepenlijst maken wanneer uw app heeft woorden of zinsdelen die belangrijk voor de app, zoals zijn:

* voorwaarden voor de bedrijfstak
* slang
* afkortingen
* bedrijfsspecifieke taal
* taal die afkomstig is van een andere taal, maar vaak gebruikt in uw app
* sleutel woorden en woordgroepen in uw voorbeeld-uitingen

Zodra u een paar woorden of zinsdelen hebt ingevoerd, gebruikt u de **raden** functie om gerelateerde waarden te vinden. Bekijk de gerelateerde waarden voordat u aan de lijst met waarden in de woordgroep toevoegt.

|Lijsttype|Doel|
|--|--|
|Verwisselbaar|Synoniemen of woorden dat, wanneer gewijzigd in een ander woord in de lijst, de dezelfde intentie en entiteiten extraheren hebben.|
|Niet-verwisselbaar|App vocabulaire, specifiek zijn voor uw app, meer, zodat er dan in het algemeen andere woorden in die taal.|

### <a name="interchangeable-lists"></a>Verwisselbaar lijsten

Een *uitwisselbaar* woordgroepenlijst is voor waarden die synoniemen worden. Bijvoorbeeld, als u wilt dat alle instanties van water gevonden en u beschikt over voorbeeld uitingen zoals: 

* Wat steden zich dicht bij de grote meren? 
* Welke weg wordt uitgevoerd langs Lake Havasu?
* Waar de Nijl beginnen en eindigen? 

Elke utterance moet worden vastgesteld voor zowel de intentie en de entiteiten, ongeacht de hoofdtekst van het water: 

* Wat steden zich dicht bij [bodyOfWater]?
* Welke weg wordt uitgevoerd op [bodyOfWater]?
* Waar de [bodyOfWater] starten en beëindigen? 

Omdat het woorden of zinsdelen voor de hoofdtekst van het water synoniem zijn en kunnen door elkaar worden gebruikt in de uitingen, gebruikt u de **verwisselbaar** instellen in de woordgroepenlijst met. 

### <a name="non-interchangeable-lists"></a>Niet-verwisselbaar lijsten

Een woordgroepenlijst met niet-verwisselbaar wordt een signaal die de loyaliteit van detectie van LUIS. De woordgroepenlijst met geeft woorden of zinsdelen die belangrijker zijn die andere woorden. Dit helpt met beide bepalende intentie en entiteit detecteren. Stel dat u hebt een onderwerp-domein, zoals traject dat globale (dat wil zeggen over culturen maar nog steeds in één taal). Er zijn woorden en zinnen die belangrijk zijn voor de app, maar niet hetzelfde zijn. 

Gebruik voor een ander voorbeeld: een woordgroepenlijst met niet-verwisselbaar zeldzaam, eigen en externe woorden. LUIS is mogelijk niet worden herkend zeldzame en eigen woorden, evenals de woorden in vreemde talen (buiten de cultuur van de app). De niet-verwisselbaar instelling geeft aan dat de set zeldzaam woorden vormt een klasse die LUIS moet leren herkennen, maar ze niet synoniemen zijn of uitwisselbaar met elkaar.

Niet elke mogelijke woord of woordgroep toevoegen aan een woordgroepenlijst met, een paar woorden of zinsdelen tegelijk, toevoegen en vervolgens opnieuw trainen en publiceren. 

Als de woordgroepenlijst na verloop van tijd groeit, merkt u misschien enkele termen vele vormen (synoniemen). Lichten deze in een andere zin-lijst die is uitwisselbaar. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Woordgroep geeft een lijst van de help eenvoudige uitwisselbaar entiteiten identificeren
Verwisselbaar woordgroep lijsten zijn een goede manier om af te stemmen van de prestaties van uw LUIS-app. Als uw app problemen bij het voorspellen van uitingen met de juiste intent of entiteiten herkennen heeft, moet u bedenken dat de uitingen ongebruikelijke woorden of woorden die mogelijk niet-eenduidige in betekenis bevatten. Deze woorden zijn goede kandidaten om op te nemen in een woordgroepenlijst met.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Woordgroep geeft een lijst van de help intents identificeren door beter inzicht in context
Een woordgroepenlijst is niet een instructie aan LUIS uit te voeren strikte die overeenkomen met of alle voorwaarden in de woordgroepenlijst met altijd label precies hetzelfde. Het is gewoon een hint. Bijvoorbeeld, u kunt een woordgroepenlijst die aangeeft dat "Patti" en "Selma" namen zijn hebben, maar LUIS kunt contextuele informatie nog steeds gebruiken voor het herkennen van dat ze iets anders in betekenen 'Maak een reservering voor 2 op de Patti Diner voor diner' en 'vinden me te stimuleren aanwijzingen voor het Selma, Georgië'. 

Toevoegen van een woordgroepenlijst vormt een alternatief voor het toevoegen van meer voorbeeld uitingen aan een doel. 

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

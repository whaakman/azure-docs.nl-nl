---
title: Goed voorbeeld uitingen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uitingen zijn invoer van de gebruiker die uw app moet worden geïnterpreteerd. Items waarvan u denkt dat gebruikers verzamelen. Uitingen die dezelfde betekenis, maar zijn samengesteld anders opnemen in word lengte en de plaatsing van word.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: fdf5508475d868ccb8c271daaac7449d3c940301
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65073159"
---
# <a name="understand-what-good-utterances-are-for-your-luis-app"></a>Inzicht in wat goede uitingen zijn voor uw LUIS-app

**Uitingen** zijn invoer van de gebruiker die uw app moet worden geïnterpreteerd. LUIS intenties en entiteiten ophalen uit deze trainen, is het belangrijk om vast te leggen van tal van verschillende voorbeeld-uitingen voor elk doel. Actief leren, of het proces voor u doorgaat met het trainen op nieuwe uitingen is essentieel voor hebt geleerd van machine-intelligentie die LUIS biedt.

Uitingen waarvan u denkt dat gebruikers verzamelen. Uitingen die dezelfde betekenis, maar in een aantal verschillende manieren zijn samengesteld omvatten:

* Lengte van utterance - korte, middellange en lang is voor uw clienttoepassing
* Lengte van Word en woordgroep 
* Plaatsing in Word - entiteit aan begin, midden en einde van utterance
* Grammatica 
* Pluralization
* Als gevolg
* Zelfstandig naamwoord en werkwoord keuze
* Leestekens - goede verschillende met behulp van de juiste, onjuiste en geen grammatica

## <a name="how-to-choose-varied-utterances"></a>Uiteenlopende uitingen kiezen

Wanneer u eerst aan de slag door [toe te voegen voorbeeld uitingen](luis-how-to-add-example-utterances.md) aan uw LUIS-model, worden hier enkele principes rekening moet houden.

### <a name="utterances-arent-always-well-formed"></a>Uitingen zijn niet altijd goed gevormd.

Het is mogelijk een zin, zoals 'Book een ticket naar Parijs voor mij' of een fragment van een zin, zoals 'Reservering' of "Parijs vlucht."  Gebruikers maken vaak spelfouten. Bij het plannen van uw app, moet u overwegen of u gebruikt [Bing spellingcontrole](luis-tutorial-bing-spellcheck.md) om op te lossen invoer van de gebruiker voordat deze wordt doorgegeven aan LUIS. 

Als het selectievakje gebruiker uitingen niet is gespeld, moet u LUIS trainen op uitingen die typefouten en spelfouten bevatten.

### <a name="use-the-representative-language-of-the-user"></a>Gebruik de representatieve taal van de gebruiker

Bij het kiezen van uitingen er rekening mee dat wat u denkt dat een algemene term of woordgroep mogelijk niet juist zijn voor de meeste gebruikers van uw clienttoepassing. Ze hebben mogelijk niet de domein-ervaring. Wees voorzichtig bij het gebruik van termen of zinnen dat een gebruiker alleen zou bijvoorbeeld als ze een expert zijn.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Kies uiteenlopende terminologie, evenals formulering

U vindt dat zelfs als u inspanningen voor het maken van uiteenlopende zin patronen, u nog steeds herhaald sommige vocabulaire.

Deze voorbeeld-uitingen nemen:

|Voorbeelden van utterances|
|--|
|hoe krijg ik een computer?|
|Waar vind ik een computer?|
|Ik wil ophalen van een computer, hoe gaat het erover?|
|Wanneer kan ik een computer hebben?| 

De term core hier is niet 'computer', gewijzigd. Alternatieven zoals desktopcomputer, laptop, werkstation of zelfs alleen machine gebruiken. LUIS afleidt op intelligente wijze synoniemen uit de context, maar wanneer u uitingen voor training maakt, is het nog steeds beter ze variëren.

## <a name="example-utterances-in-each-intent"></a>Voorbeeld-uitingen in elk doel

Elk doel moet voorbeeld uitingen, ten minste 15 hebben. Als u een doel dat geen alle uitingen voorbeeld hebt, wordt het niet mogelijk met het trainen van LUIS. Hebt u een doel met één of enkele voorbeeld-uitingen, LUIS wordt niet nauwkeurig te voorspellen de bedoeling. 

## <a name="add-small-groups-of-15-utterances-for-each-authoring-iteration"></a>Kleine groepen van 15 uitingen voor elke herhaling authoring toevoegen

In elke herhaling van het model, Voeg een grote hoeveelheid uitingen niet. Utterances toevoegen in hoeveelheden van 15. [Train](luis-how-to-train.md), [publiceren](luis-how-to-publish-app.md), en [testen](luis-interactive-test.md) opnieuw.  

LUIS bouwt effectieve modellen met uitingen die zorgvuldig worden geselecteerd door de auteur van de LUIS-model. Te veel uitingen toe te voegen is niet nuttig omdat het verwarring leidt.  

Het is beter om te beginnen met een paar uitingen vervolgens [eindpunt uitingen bekijken](luis-how-to-review-endpoint-utterances.md) voor het juiste intentie voorspelling en entiteit ophalen.

## <a name="utterance-normalization"></a>Utterance normalisering

Utterance normalisatie is het proces van de gevolgen van interpunctie en diakritische tekens worden genegeerd tijdens het trainen en voorspellen.

## <a name="utterance-normalization-for-diacritics-and-punctuation"></a>Utterance normalisering voor diakritische tekens en leestekens

Utterance normalisering wordt gedefinieerd bij het maken of importeren van de app omdat het een instelling in de JSON-bestand van de app. De utterance normalisatie-instellingen zijn standaard uitgeschakeld. 

Diakritische tekens zijn merken of tekens in de tekst, zoals: 

```
İ ı Ş Ğ ş ğ ö ü
```

Als uw app normalisering ingeschakeld wordt, beoordeelt de **Test** deelvenster,, batchtests en eindpunt query's verandert er voor alle uitingen diakritische tekens of leestekens.

Schakel utterance normalisering voor diakritische tekens of de interpunctieteken naar uw LUIS-JSON-app-bestand in de `settings` parameter.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"},
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

Normaliseren **interpunctie** betekent dat voordat uw modellen ontvang training en voordat u uw eindpunt query's ophalen voorspeld, interpunctie wordt verwijderd uit de uitingen. 

Normaliseren **diakritische tekens** vervangt de tekens met diakritische tekens in uitingen met gewone tekens. Bijvoorbeeld: `Je parle français` wordt `Je parle francais`. 

Normalisering betekent niet dat u wordt niet Zie interpunctie en diakritische tekens in uw voorbeeld-uitingen of voorspelling antwoorden, alleen dat ze worden genegeerd tijdens het trainen en voorspellen.


### <a name="punctuation-marks"></a>Leestekens

Als leestekens niet is genormaliseerd, negeren niet LUIS leestekens, standaard, omdat sommige clienttoepassingen betekenis op deze markeringen kunnen plaatsen. Zorg ervoor dat uw voorbeeld-uitingen gebruikt zowel interpunctie en geen interpunctie in volgorde voor beide stijlen naar de dezelfde relatieve scores. 

Als interpunctie geen specifieke betekenis in uw clienttoepassing heeft, kunt u overwegen [wordt genegeerd interpunctie](#utterance-normalization) door het normaliseren van leestekens. 

### <a name="ignoring-words-and-punctuation"></a>Woorden en leestekens worden genegeerd

Als u negeren specifieke woorden of de interpunctieteken in patronen wilt, gebruikt u een [patroon](luis-concept-patterns.md#pattern-syntax) met de _negeren_ syntaxis van tussen vierkante haken, `[]`. 

## <a name="training-utterances"></a>Training-uitingen

Training is in het algemeen niet-deterministisch: de voorspelling utterance kan enigszins verschillen voor verschillende versies of apps. U kunt niet-deterministisch training verwijderen door bij te werken de [versie instellingen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API met de `UseAllTrainingData` naam/waarde-paar om alle trainingsgegevens te gebruiken.

## <a name="testing-utterances"></a>Uitingen testen 

Ontwikkelaars moeten beginnen met het testen van hun LUIS-toepassing met echte verkeer door te sturen uitingen naar de [voorspelling eindpunt](luis-how-to-azure-subscription.md) URL. Deze uitingen worden gebruikt voor het verbeteren van de prestaties van de intenties en entiteiten met [bekijken uitingen](luis-how-to-review-endpoint-utterances.md). Tests verzonden met het testen van deelvenster LUIS-website niet via het eindpunt zijn verzonden, en dus niet bijdragen tot actief leren. 

## <a name="review-utterances"></a>Uitingen bekijken

Nadat uw model getraind, gepubliceerde en ontvangende is [eindpunt](luis-glossary.md#endpoint) query's, [bekijken van de uitingen](luis-how-to-review-endpoint-utterances.md) LUIS worden voorgesteld. LUIS selecteert endpoint-uitingen waarvoor lage scores voor de doel- of entiteit. 

## <a name="best-practices"></a>Aanbevolen procedures

Beoordeling [aanbevolen procedures](luis-concept-best-practices.md) en deze als onderdeel van uw normale authoring cyclus toepassen.

## <a name="next-steps"></a>Volgende stappen
Zie [voorbeeld utterances toevoegen](luis-how-to-add-example-utterances.md) voor meer informatie over het trainen van een LUIS-app om te begrijpen uitingen van de gebruiker.


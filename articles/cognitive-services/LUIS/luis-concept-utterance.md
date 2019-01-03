---
title: Goed voorbeeld uitingen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uitingen zijn invoer van de gebruiker die uw app moet worden geïnterpreteerd. Items waarvan u denkt dat gebruikers verzamelen. Uitingen die dezelfde betekenis, maar zijn samengesteld anders opnemen in word lengte en de plaatsing van word.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: ae090a8cd812868f63c9805b2f5b59769a715090
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53975267"
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

Het is mogelijk een zin, zoals 'Book een ticket naar Parijs voor mij' of een fragment van een zin, zoals 'Reservering' of "Parijs vlucht."  Gebruikers maken vaak spelfouten. Bij het plannen van uw app, moet u overwegen of u [Bing spellingcontrole Check][(luis-tutorial-bing-spellcheck) om op te lossen invoer van de gebruiker voordat deze wordt doorgegeven aan LUIS. 

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

Het is beter om te beginnen met een paar uitingen vervolgens [eindpunt uitingen bekijken](luis-how-to-review-endoint-utt.md) voor het juiste intentie voorspelling en entiteit ophalen.

## <a name="punctuation-marks"></a>Leestekens

LUIS negeren niet leestekens, standaard, omdat sommige clienttoepassingen betekenis op deze markeringen kunnen plaatsen. Zorg ervoor dat uw voorbeeld-uitingen gebruikt zowel interpunctie en geen interpunctie in volgorde voor beide stijlen naar de dezelfde relatieve scores. Als er interpunctie is geen specifieke betekenis in uw clienttoepassing, kunt u overwegen [wordt genegeerd interpunctie](#ignoring-words-and-punctuation) met behulp van patronen. 

## <a name="ignoring-words-and-punctuation"></a>Woorden en leestekens worden genegeerd

Als u wilt dat specifieke woorden of de interpunctieteken in het voorbeeld utterance negeren, gebruikt u een [patroon](luis-concept-patterns.md#pattern-syntax) met de _negeren_ syntaxis. 

## <a name="training-utterances"></a>Training-uitingen

Training is in het algemeen niet-deterministisch: de voorspelling utterance kan enigszins verschillen voor verschillende versies of apps. U kunt niet-deterministisch training verwijderen door bij te werken de [versie instellingen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API met de `UseAllTrainingData` naam/waarde-paar om alle trainingsgegevens te gebruiken.

## <a name="testing-utterances"></a>Uitingen testen 

Ontwikkelaars moeten beginnen met het testen van hun LUIS-toepassing met echte verkeer door te sturen uitingen naar de [voorspelling eindpunt](luis-how-to-manage-keys.md) URL. Deze uitingen worden gebruikt voor het verbeteren van de prestaties van de intenties en entiteiten met [bekijken uitingen](luis-how-to-review-endoint-utt.md). Tests verzonden met het testen van deelvenster LUIS-website niet via het eindpunt zijn verzonden, en dus niet bijdragen tot actief leren. 

## <a name="review-utterances"></a>Uitingen bekijken

Nadat uw model getraind, gepubliceerde en ontvangende is [eindpunt](luis-glossary.md#endpoint) query's, [bekijken van de uitingen](luis-how-to-review-endoint-utt.md) LUIS worden voorgesteld. LUIS selecteert endpoint-uitingen waarvoor lage scores voor de doel- of entiteit. 

## <a name="best-practices"></a>Aanbevolen procedures

Beoordeling [aanbevolen procedures](luis-concept-best-practices.md) en deze als onderdeel van uw normale authoring cyclus toepassen.

## <a name="next-steps"></a>Volgende stappen
Zie [voorbeeld utterances toevoegen](luis-how-to-add-example-utterances.md) voor meer informatie over het trainen van een LUIS-app om te begrijpen uitingen van de gebruiker.


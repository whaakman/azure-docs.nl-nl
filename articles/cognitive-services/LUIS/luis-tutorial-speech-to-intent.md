---
title: Speech C# SDK gebruiken
titleSuffix: Azure Cognitive Services
description: Met de Speech-service kunt u één aanvraag gebruiken om audio te ontvangen en JSON-objecten met LUIS-voorspelling te retourneren. In dit artikel downloadt en gebruikt u een C#-project in Visual Studio om een utterance in te spreken in een microfoon en LUIS-voorspellingsinformatie te ontvangen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: c247ef748628c14074c2b5ea37df251793840b16
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55876031"
---
# <a name="integrate-speech-service-with-your-language-understanding-app"></a>De Speech-service integreren in uw Language Understanding-app
Met de [Speech-service](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) kunt u één aanvraag gebruiken om audio te ontvangen en JSON-objecten met LUIS-voorspelling te retourneren. In dit artikel downloadt en gebruikt u een C#-project in Visual Studio om een utterance in te spreken in een microfoon en LUIS-voorspellingsinformatie te ontvangen. Het project maakt gebruik van het Speech [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/)-pakket, dat al als referentie is opgenomen. 

Voor dit artikel hebt u een gratis [LUIS][LUIS]-websiteaccount nodig om de toepassing te importeren.

## <a name="create-luis-endpoint-key"></a>LUIS-eindpuntsleutel maken
[Maak](luis-how-to-azure-subscription.md) in de Microsoft Azure-portal een **Language Understanding** (LUIS)-sleutel. 

## <a name="import-human-resources-luis-app"></a>Human Resources LUIS-app importeren
De intenties en utterances voor dit artikel zijn afkomstig van de Human Resources LUIS-app beschikbaar in de GitHub-opslagplaats [Azure-Samples](https://github.com/Azure-Samples/cognitive-services-language-understanding). Download het bestand [HumanResources.json](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources.json), sla het op met de extensie `.json` en [importeer](luis-how-to-start-new-app.md#import-new-app) het in LUIS. 

Deze app heeft intenties, entiteiten en utterances gerelateerd aan het Human Resources-domein. Hier zijn enkele voorbeelden van utterances:

|Voorbeelden van utterances|
|--|
|Wie is Jan Smits manager?|
|Wie is de manager van Jan Smit?|
|Waar kan ik formulier 123456 vinden?|
|Heb ik nog betaald verlof?|


## <a name="add-keyphrase-prebuilt-entity"></a>Vooraf gemaakte KeyPhrase-entiteit toevoegen
Wanneer u de app hebt geïmporteerd, selecteert u **Entiteiten** en klikt u op **Vooraf gemaakte entiteit toevoegen**. Voeg de entiteit **KeyPhrase** toe. De entiteit KeyPhrase extraheert het belangrijkste onderwerp uit de utterance.

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren
1. Selecteer, in de navigatiebalk rechtsboven, de knop **Train** om de LUIS-app te trainen.

2. Selecteer **Beheren** in de balk rechtsboven en schakel **Sleutels en eindpunten** in het linkernavigatievenster in. 

3. Wijs op de pagina **Sleutels en eindpunten** de LUIS-sleutel toe die is gemaakt in de sectie [LUIS-eindpuntsleutel maken](#create-luis-endpoint-key).

  Verzamel op deze pagina de app-id, het publicatiegebied en de abonnement-id van de LUIS-sleutel die is gemaakt in de sectie [LUIS-eindpuntsleutel maken](#create-luis-endpoint-key). U moet de code wijzigen om deze waarden later in dit artikel te gebruiken. 
  
  Gebruik **niet** de gratis startersleutel voor deze oefening. Alleen een **Language Understanding**-sleutel die in de Microsoft Azure-portal is gemaakt, werkt voor deze oefening. 

  https://**REGION**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**?subscription-key=**LUISKEY**&q=


4. Publiceer de LUIS-app door de knop **Publiceren** te selecteren in de balk rechtsboven. 

## <a name="audio-device"></a>Audioapparaat
In dit artikel wordt het audioapparaat op uw computer gebruikt. Dat kan een headset met microfoon of een ingebouwd audioapparaat zijn. Controleer de audio-invoerniveaus om te zien of u harder moet spreken dan normaal om uw spraak door het audioapparaat te laten detecteren. 

## <a name="download-the-luis-sample-project"></a>Het LUIS-voorbeeldproject downloaden
 Kloon of download de [Azure-Samples](https://github.com/Azure-Samples/cognitive-services-language-understanding)-opslagplaats. Open het [spraak-naar-intentie-project](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-speech-intent-recognition) met Visual Studio en herstel de NuGet-pakketten. Het Visual Studio-oplossingsbestand is documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

De Speech-SDK is al opgenomen als verwijzing. 

[![Schermopname van Visual Studio 2017 met Microsoft.CognitiveServices.Speech NuGet-pakket](./media/luis-tutorial-speech-to-intent/nuget-package.png "Schermopname van Visual Studio 2017 met Microsoft.CognitiveServices.Speech NuGet-pakket")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>De C#-code wijzigen
Open het bestand `Program.cs` en wijzig de volgende variabelen:

|Naam van de variabele|Doel|
|--|--|
|LUIS_assigned_endpoint_key|Komt overeen met de toegekende waarde van de subsleutelwaarde van de eindpunt-URL van de pagina Publiceren|
|LUIS_endpoint_key_region|Komt overeen met het eerste subdomein van de eindpunt-URL, bijvoorbeeld `westus`|
|LUIS_app_ID|Komt overeen met de route van de eindpunt-URL na **apps /**|

De Human Resources-intenties zijn al toegewezen aan het bestand `Program.cs`.

Bouw de app en voer deze uit. 

## <a name="test-code-with-utterance"></a>Code testen met utterance
Zeg in de microfoon "Wie zijn erkende tandartsen in Redmond?"

[!code-console[Command line response from spoken utterance](~/samples-luis/documentation-samples/tutorial-speech-intent-recognition/console-output.txt "Command line response from spoken utterance")]

De juiste intentie, **GetEmployeeBenefits**, is gevonden met een betrouwbaarheid van 85%. De keyPhrase-entiteit is geretourneerd. 

De Speech-SDK retourneert het hele LUIS-antwoord. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS HumanResources-app niet meer nodig hebt, kunt u deze verwijderen. Dit kunt u doen door de app te selecteren en vervolgens **Verwijderen** te selecteren op de contextuele werkbalk boven de lijst. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

Vergeet niet om de map te verwijderen wanneer u klaar bent met de voorbeeldcode.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS integreren met een bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website

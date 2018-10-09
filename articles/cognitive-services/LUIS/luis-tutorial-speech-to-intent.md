---
title: Gebruik spraak SDK voor C# met LUIS
titleSuffix: Azure Cognitive Services
description: De spraak-service kunt u een enkele aanvraag gebruiken voor het ontvangen van audio en LUIS voorspelling JSON-objecten retourneren. In dit artikel, downloaden en gebruiken om een C#-project in Visual Studio te spreken een utterance in een microfoon LUIS voorspelling informatie ontvangen. De spraak-NuGet-pakket, al is opgenomen als een verwijzing maakt gebruik van het project.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: fb17e2d8c0ef1df5a6d4965730d3ddd3764d58f5
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868748"
---
# <a name="integrate-speech-service"></a>Integreer Speech-service
De [spraakservice](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) kunt u gebruikmaken van een enkele aanvraag voor het ontvangen van audio en LUIS voorspelling JSON-objecten retourneren. In dit artikel, downloaden en gebruiken om een C#-project in Visual Studio te spreken een utterance in een microfoon LUIS voorspelling informatie ontvangen. Het project gebruikmaakt van de gesproken tekst [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) pakket, al is opgenomen als een verwijzing. 

Voor dit artikel, moet u een gratis [LUIS] [ LUIS] website-account voor het importeren van de toepassing.

## <a name="create-luis-endpoint-key"></a>LUIS-eindpuntsleutel maken
In de Azure-portal [maken](luis-how-to-azure-subscription.md#create-luis-endpoint-key) een **Language Understanding** (LUIS)-sleutel. 

## <a name="import-human-resources-luis-app"></a>Importeren van Human Resources LUIS app
De intenties en uitingen voor dit artikel zijn van het Human Resources LUIS-app beschikbaar is via de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) Github-opslagplaats. Download de [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/HumanResources.json) bestand opslaan met de `.json` -extensie en [importeren](luis-how-to-start-new-app.md#import-new-app) in LUIS. 

Deze app heeft intenties en entiteiten uitingen met betrekking tot het Human Resources-domein. Voorbeeld-uitingen zijn onder andere:

|Voorbeelden van utterances|
|--|
|Wie is de manager van John Smith?|
|Wie het beheren van John Smith?|
|Waar bevindt zich formulier 123456?|
|Heb ik een betaalde tijd uitgeschakeld?|


## <a name="add-keyphrase-prebuilt-entity"></a>Toevoegen van KeyPhrase vooraf gemaakte entiteiten
Na het importeren van de app, selecteer **entiteiten**, klikt u vervolgens **vooraf gemaakte entiteiten beheren**. Voeg de **KeyPhrase** entiteit. De entiteit KeyPhrase sleutels onderwerp van de utterance worden uitgepakt.

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren
1. Selecteer in de bovenste, rechts navigatiebalk de **trainen** knop met het trainen van de LUIS-app.

2. Selecteer **beheren** in de rechterbovenhoek van de balk, schakelt u vervolgens **sleutels en eindpunten** in het linkernavigatievenster. 

3. Op de **sleutels en eindpunten** pagina, wijzen de LUIS-sleutel hebt gemaakt de [maken LUIS eindpuntsleutel](#create-luis-endpoint-key) sectie.

  Op deze pagina, verzamelen van de app-ID, de regio te publiceren en abonnements-ID van de sleutel LUIS gemaakt in de [maken LUIS eindpuntsleutel](#create-luis-endpoint-key) sectie. U moet de code voor het gebruik van deze waarden verderop in dit artikel te wijzigen. 
  
  Voer **niet** gratis starter-toets gebruiken voor deze oefening. Alleen een **Language Understanding** sleutel hebt gemaakt in Azure portal werkt voor deze oefening. 

  https://**regio**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**?-abonnementssleutel =**LUISKEY**& q =


4. De LUIS-app publiceren door het selecteren van de **publiceren** knop in de rechterbovenhoek van de balk. 

## <a name="audio-device"></a>Audio-apparaat
In dit artikel wordt de audio-apparaat op uw computer. Die een hoofdtelefoon, microfoon of een ingebouwde audio-apparaat kan zijn. Controleer de audio invoer niveaus om te zien als u dan u gewend bent als u wilt dat uw spraak gedetecteerd door de audio-apparaat harder te spreken. 

## <a name="download-the-luis-sample-project"></a>Het project LUIS Sample downloaden
 Klonen of downloaden de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) opslagplaats. Open de [spraak naar intentie project](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) met Visual Studio en herstel de NuGet-pakketten. Het Visual Studio-oplossingsbestand is.\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

De spraak-SDK is al opgenomen als een verwijzing. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Schermopname van Visual Studio 2017 weergeven Microsoft.CognitiveServices.Speech NuGet-pakket")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>De C#-code wijzigen
Open de `Program.cs` bestand en wijzig de volgende variabelen:

|De naam van variabele|Doel|
|--|--|
|LUIS_assigned_endpoint_key|Komt overeen met de eindpunt-URL's abonnementssleutel waarde vanuit de pagina publiceren toegewezen|
|LUIS_endpoint_key_region|Komt overeen met de eerste subdomein eindpunt-URL, bijvoorbeeld `westus`|
|LUIS_app_ID|Komt overeen met de eindpunt-URL-route te volgen **apps /**|

De `Program.cs` bestand al heeft het Human Resources-intents toegewezen.

Ontwikkel en voer de app. 

## <a name="test-code-with-utterance"></a>Testen van code met utterance
In de microfoon praat "Wie zijn de goedgekeurde tandartsen in Redmond?".

[!code-console[Command line response from spoken utterance](~/samples-luis/documentation-samples/tutorial-speech-intent-recognition/console-output.txt "Command line response from spoken utterance")]

De juiste intentie **GetEmployeeBenefits**, met een betrouwbaarheid van 85% is gevonden. De entiteit keyPhrase is geretourneerd. 

De SDK spraak retourneert het gehele LUIS-antwoord. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u niet meer nodig hebt, verwijdert u de app LUIS HumanResources. Om dit te doen, selecteert u de app en vervolgens op de werkbalk boven de lijst met een contextuele **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

Houd er rekening mee te verwijderen van de LUIS-Samples-directory wanneer u klaar bent met behulp van de voorbeeldcode.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS integreren met een BOT](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
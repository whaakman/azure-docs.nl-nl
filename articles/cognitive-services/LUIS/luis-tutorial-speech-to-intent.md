---
title: Spraak C# SDK gebruikt met LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Gebruik het voorbeeld spraak C# SDK te spreken in microfoon en LUIS bedoeling en entiteiten voorspellingen geretourneerd.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/10/2018
ms.author: v-geberr;
ms.openlocfilehash: 6f2bf2ae454d5af1bba0c176940db1820268a129
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266274"
---
# <a name="integrate-speech-service"></a>Spraak service integreren
De [spraak service](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) kunt u met één aanvraag ontvangen van audio en LUIS voorspelling JSON-objecten retourneren.

In dit artikel die u kunt downloaden en gebruiken van een C#-project in Visual Studio een utterance in een microfoon spreken en LUIS voorspelling informatie ontvangen. Het project maakt gebruik van de spraak [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) pakket, al als een verwijzing zijn opgenomen. 

Voor dit artikel, moet u een gratis [LUIS] [ LUIS] website-account om te importeren van de toepassing.

## <a name="import-human-resources-luis-app"></a>Human Resources LUIS importeren app
De intents en utterances voor dit artikel zijn van de app LUIS Human Resources beschikbaar is via de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) Github-opslagplaats. Download de [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) bestand opslaan met de extensie *.json en [importeren](create-new-app.md#import-new-app) in LUIS. 

Deze app heeft intents en entiteiten utterances gerelateerd aan het domein Human Resources. Voorbeeld utterances zijn onder andere:

```
Who is John Smith's manager?
Who does John Smith manage?
Where is Form 123456?
Do I have any paid time off?
```

## <a name="add-keyphrase-prebuilt-entity"></a>Toevoegen van KeyPhrase vooraf gedefinieerde entiteit
Na het importeren van de app, selecteer **entiteiten**, klikt u vervolgens **vooraf gemaakte entiteiten beheren**. Voeg de **KeyPhrase** entiteit. De entiteit KeyPhrase haalt sleutel onderwerp uit de utterance.

## <a name="train-and-publish-the-app"></a>Trainen en publiceer de app
Trainen en publiceer de app. Op de **publiceren** pagina, het verzamelen van de app-ID, het publiceren van regio en abonnements-ID. U moet de code voor het gebruik van deze waarden verderop in dit artikel te wijzigen. 

Deze waarden zijn opgenomen in de eindpunt-URL aan de onderkant van de **publiceren** pagina. 

https://**regio**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**? abonnement sleutel =**LUISKEY**& q =

## <a name="download-the-luis-sample-project"></a>Het project LUIS Sample downloaden
 Klonen of downloaden de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) opslagplaats. Open de [spraak aan opzet project](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) met Visual Studio en de NuGet-pakketten te herstellen. Het oplossingsbestand tegenover is.\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

De SDK spraak is al als een verwijzing opgenomen. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Schermafbeelding van de Visual Studio-2017 weergeven Microsoft.CognitiveServices.Speech NuGet-pakket")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Wijzig de C#-code
Open de **LUIS_samples.cs** bestand en wijzig de volgende variabelen:

|Naam variabele|Doel|
|--|--|
|luisSubscriptionKey|Komt overeen met het abonnement-sleutelwaarde eindpunt-URL van pagina publiceren|
|luisRegion|Komt overeen met de eerste subdomein eindpunt-URL|
|luisAppId|Komt overeen met de eindpunt-URL route na **apps /**|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Schermopname van Visual Studio 2017 LUIS_samples.cs variabelen weergeven")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

Het bestand heeft al het Human Resources intents toegewezen.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Schermopname van Visual Studio 2017 LUIS_samples.cs intents weergeven")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Ontwikkel en voer de app. 

![Schermopname van het programma uitvoeren vanaf de opdrachtregel](./media/luis-tutorial-speech-to-intent/cmdline-1.png)

## <a name="test-code-with-utterance"></a>Code met utterance testen
Selecteer **8** en spreek in de microfoon 'Die is de manager van John Smith'.

```cmd
1. Speech recognition with microphone input.
2. Speech recognition in the specified language.
3. Speech recognition with file input.
4. Speech recognition using customized model.
5. Speech continuous recognition using events.
6. Translation with microphone input.
7. Translation with file input.
8. Speech recognition of LUIS intent.
0. Stop.
Your choice: 8
LUIS...
Say something...
ResultId:cc83cebc9d6040d5956880bcdc5f5a98 Status:Recognized IntentId:<GetEmployeeOrgChart> Recognized text:<Who is the manager of John Smith?> Recognized Json:{"DisplayText":"Who is the manager of John Smith?","Duration":25700000,"Offset":9200000,"RecognitionStatus":"Success"}. LanguageUnderstandingJson:{
  "query": "Who is the manager of John Smith?",
  "topScoringIntent": {
    "intent": "GetEmployeeOrgChart",
    "score": 0.617331
  },
  "entities": [
    {
      "entity": "manager of john smith",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 31
    }
  ]
}

Recognition done. Your Choice:

```

De juiste bedoeling **GetEmployeeOrgChart**, is gevonden met een betrouwbaarheid 61%. De entiteit keyphrase is geretourneerd. 

De SDK spraak retourneert het gehele LUIS-antwoord. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, verwijdert u de app LUIS HumanResources. Om dit te doen, selecteert u het menu drie punt (...) rechts van de naam van de app in de lijst met Apps, selecteer **verwijderen**. In het pop-updialoogvenster **app verwijderen?**, selecteer **Ok**.

Houd er rekening mee te verwijderen van de map LUIS-Samples wanneer u klaar bent met de voorbeeldcode.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [LUIS integreren met een BOT](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
---
title: 'Quickstart: Een miniatuur maken - REST, cURL'
titleSuffix: Azure Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision-API en cURL.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 03/27/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: af303544be7b4e7686105b7d57a71c33c0a6a314
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009039"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-curl-in-computer-vision"></a>Snelstartgids: Een miniatuur maken met de REST API en cURL in Computer Vision

In deze snelstartgids hebt u een miniatuur genereren van een installatiekopie met behulp van de Computer Vision-REST-API. U de gewenste hoogte en breedte, die in de hoeveelheid aspect van de afbeelding verschillen kan. Computer Vision maakt gebruik van slim bijsnijden op intelligente wijze identificeren van het gebied van belang en genereren van bijsnijden coördinaten rond deze regio.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- U moet [cURL](https://curl.haxx.se/windows) hebben.
- U moet beschikken over een abonnementssleutel voor Computer Vision. U krijgt een gratis proefversie sleutel van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Of, volg de instructies in [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) abonneren op de Computer Vision en haal uw sleutel.

## <a name="get-thumbnail-request"></a>Get Thumbnail-aanvraag

Met de methode [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) kunt u een miniatuur van een afbeelding genereren.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code in een editor.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Vervang `<File>` door het pad en de bestandsnaam waar de miniatuur moet worden opgeslagen.
1. Wijzig zo nodig aanvraag-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) in de locatie waar u de abonnementssleutels hebt verkregen.
1. Wijzig eventueel de te analyseren afbeelding (`{\"url\":\"...`).
1. Open een opdrachtvenster op een computer waarop cURL is geïnstalleerd.
1. Plak de code in het venster en voer de opdracht uit.

>[!NOTE]
>U moet in uw REST-aanroep dezelfde locatie gebruiken waar u uw abonnementssleutels hebt verkregen. Als u bijvoorbeeld uw abonnementssleutels van 'westus' hebt verkregen, vervangt u 'westcentralus' in de onderstaande URL door 'westus'.

## <a name="create-and-run-the-sample-command"></a>Een voorbeeldopdracht maken en uitvoeren

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende opdracht naar een teksteditor.
1. Breng waar nodig de volgende wijzigingen in de opdracht aan:
    1. Vervang de waarde van `<subscriptionKey>` door uw abonnementssleutel.
    1. Vervang de waarde van `<thumbnailFile>` door het pad en de naam van het bestand waarin u de miniatuur opslaat.
    1. Vervang de aanvraag-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail`) door de eindpunt-URL van de methode [Miniatuur ophalen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) uit de Azure-regio waar u uw abonnementssleutels hebt verkregen (indien nodig).
    1. Wijzig eventueel de afbeeldings-URL in de aanvraagtekst (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) in een URL van een andere afbeelding van waaruit u de miniatuur genereert.
1. Open een opdrachtpromptvenster.
1. Plak de opdracht van de teksteditor in het opdrachtpromptvenster en voer de opdracht uit.

    ```console
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagd antwoord schrijft de miniatuurafbeelding naar het bestand dat is opgegeven in `<thumbnailFile>`. Als de aanvraag mislukt, bevat het antwoord een foutcode en een bericht om u te helpen bepalen wat er mis is gegaan. Als de aanvraag lijkt te voltooien, maar de miniatuur van het gemaakte geen geldig afbeeldingsbestand is, is het mogelijk dat de abonnementssleutel van uw niet geldig is.

## <a name="next-steps"></a>Volgende stappen

Verken de Computer Vision-API aan de analyseer een afbeelding, detecteren van beroemdheden en oriëntatiepunten, een miniatuur en afgedrukt en handgeschreven tekst extraheren. Als u snel wilt experimenteren met de Computer Vision-API, gebruikt u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API verkennen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

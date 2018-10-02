---
title: 'Snelstartgids: Herkennen gesproken tekst in JavaScript in een browser met behulp van de Cognitive Services spraak-SDK'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in JavaScript in een browser met behulp van de Cognitive Services spraak-SDK
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: 9ac48bd77087a8dc050ccd1ee8fe63ec5c25a643
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018381"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-cognitive-services-speech-sdk"></a>Snelstartgids: Herkennen gesproken tekst in JavaScript in een browser met behulp van de Cognitive Services spraak-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u over het maken van een website met de JavaScript-binding van de Cognitive Services Speech SDK spraak naar tekst te transcriberen.
De toepassing is gebaseerd op de Microsoft Cognitive Services Speech SDK ([downloaden versie 1.0.0](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een PC of Mac, met een microfoon werken.
* Een teksteditor.
* Een actuele versie van Chrome of Microsoft Edge.
* (Optioneel) een webserver die ondersteunt het hosten van PHP-scripts.

## <a name="create-a-new-website-folder"></a>Maak een nieuwe websitemap

Maak een nieuwe, lege map. Als u wilt hosten van de steekproef op een webserver, en zorg ervoor dat de webserver, toegang heeft tot de map.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>De spraak-SDK voor JavaScript in die map uitpakken

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Download de SDK spraak als een [-ZIP-pakket](https://aka.ms/csspeech/jsbrowserpackage) en pak het uit in de zojuist gemaakte map. Dit moet resulteren in twee bestanden wordt uitgepakt, dat wil zeggen, `microsoft.cognitiveservices.speech.sdk.bundle.js` en `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
Het laatste bestand is optioneel en wordt gebruikt om u te helpen bij foutopsporing in SDK-code, indien nodig.

## <a name="create-an-indexhtml-page"></a>Maak een index.html-pagina

Maak een nieuw bestand in de map met de naam `index.html` en open dit bestand met een teksteditor.

1. Maak de volgende HTML-basis:

  ```html
  <html>
  <head>
      <title>Microsoft Cognitive Service Speech SDK JavaScript Quickstart</title>
  </head>
  <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
  </body>
  </html>
  ```

1. Voeg de volgende code in de gebruikersinterface toe aan het bestand, onder de eerste opmerking:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Voeg een verwijzing naar de spraak-SDK

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Wire-up handlers voor de opname-knop, herkenningsresultaat en abonnement gerelateerde velden gedefinieerd door de code van de gebruikersinterface:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Maak de token bron (optioneel)

Als u wilt voor het hosten van de webpagina wordt weergegeven op een webserver, kunt u eventueel een token bron opgeven voor de demo-toepassing.
Op die manier kunnen uw abonnementssleutel wordt nooit laat uw server en gebruikers toestaat met spraakmogelijkheden zonder in te voeren van een autorisatiecode zelf.

1. Maak een nieuw bestand met de naam `token.php`. In dit voorbeeld veronderstellen we dat uw webserver de PHP-scripttaal. Voer de volgende code:

  [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Bewerk de `index.html` -bestand en voeg de volgende code toe aan het bestand:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Autorisatietokens slechts hebben een beperkte levensduur.
> In dit eenvoudige voorbeeld weergegeven autorisatietokens automatisch vernieuwen niet. Als een gebruiker, kunt u handmatig laad de pagina of druk op F5 om te vernieuwen.

## <a name="build-and-run-the-sample-locally"></a>Bouw en het voorbeeld lokaal uitvoeren

U start de app, dubbelklik op het bestand index.html of index.html met uw favoriete webbrowser te openen. Het biedt een eenvoudige GUI zodat u kunt uw abonnementssleutel invoeren en [regio](regions.md) en activeren van een opname met behulp van de microfoon.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Bouw en voer het voorbeeld uit via een webserver

Start uw app, open uw favoriete webbrowser en wijst deze naar de openbare URL die u bij het hosten van de map op, Geef uw [regio](regions.md), en een opname met behulp van de microfoon te activeren. Als geconfigureerd, wordt er een token van de bron-token verkrijgen.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/js-browser` map.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze voorbeelden ophalen](speech-sdk.md#get-the-samples)

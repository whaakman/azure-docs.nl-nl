---
title: 'Quickstart: Spraak herkennen, Javascript (browser) - Speech Services'
titleSuffix: Azure Cognitive Services
description: Informatie over het herkennen van gesproken tekst in JavaScript in een browser met behulp van de Speech Service SDK
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: fmegen
ms.openlocfilehash: a38553bc2c549989fe24f3a95e972858f6813fc5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211148"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-speech-service-sdk"></a>Snelstart: gesproken tekst in JavaScript herkennen in een browser met behulp van de Speech Service SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een website maakt met behulp van de JavaScript binding van de Cognitive Services Speech SDK om spraak om te zetten naar tekst.
De toepassing is gebaseerd op de Microsoft Cognitive Services Speech SDK ([download versie 1.2.0](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [Probeer de Speech-service gratis uit](get-started.md).
* Een pc of Mac, met een werkende microfoon.
* Een teksteditor.
* Een actuele versie van Chrome of Microsoft Edge.
* Een webserver die het hosten van PHP-scripts ondersteunt (optioneel).

## <a name="create-a-new-website-folder"></a>Een nieuwe websitemap maken

Maak een nieuwe, lege map. Als u de voorbeeldtoepassing op een webserver wilt hosten, moet u ervoor zorgen dat de webserver toegang heeft tot de map.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>De Speech SDK voor JavaScript in die map uitpakken

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Download de Speech SDK als een [ZIP-pakket](https://aka.ms/csspeech/jsbrowserpackage) en pak dit uit in de zojuist gemaakte map. Er worden twee bestanden uitgepakt, `microsoft.cognitiveservices.speech.sdk.bundle.js` en `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
Het laatste bestand is optioneel en is handig voor foutopsporing in de SDK-code.

## <a name="create-an-indexhtml-page"></a>Een index.html-pagina maken

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

1. Voeg de volgende gebruikersinterfacecode toe aan het bestand, onder de eerste opmerking:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Voeg een verwijzing toe aan de Speech SDK

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Stel handlers in voor de velden die gerelateerd zijn aan de spraakherkenningsknop, het spraakherkenningsresultaat en het abonnement die zijn gedefinieerd door de gebruikersinterfacecode:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>De tokenbron maken (optioneel)

Als u de webpagina wilt hosten op een webserver, kunt u desgewenst een tokenbron opgeven voor uw voorbeeldtoepassing.
Op die manier verlaat uw abonnementssleutel nooit uw server en is het gebruikers toegestaan gebruik te maken van spraakmogelijkheden zonder zelf een autorisatiecode in te hoeven voeren.

1. Maak een nieuw bestand met de naam `token.php`. In dit voorbeeld gaan we er van uit dat uw webserver de PHP-scripttaal ondersteunt. Voer de volgende code in:

  [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Bewerk het bestand `index.html` en voeg de volgende code toe aan het bestand:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Autorisatietokens hebben slechts een beperkte levensduur.
> In dit eenvoudige voorbeeld wordt niet weergegeven hoe autorisatietokens automatisch worden vernieuwd. Als een gebruiker kunt u de pagina handmatig opnieuw laden of op F5 drukken om te vernieuwen.

## <a name="build-and-run-the-sample-locally"></a>Het voorbeeld bouwen en lokaal uitvoeren

Om de app te starten, dubbelklikt u op het bestand index.html of opent u index.html met uw favoriete webbrowser. Er wordt een eenvoudige grafische gebruikersinterface weergegeven waarin u uw abonnementssleutel en [regio](regions.md) kunt invoeren spraakherkenning met behulp van de microfoon kunt activeren.

## <a name="build-and-run-the-sample-via-a-web-server"></a>De voorbeeldtoepassing bouwen en uitvoeren via een webserver

Om uw app te starten, opent u uw favoriete webbrowser en laat u deze wijzen naar de openbare URL waarop u de map host, geeft u uw [regio](regions.md) op en activeert u spraakherkenning met behulp van de microfoon. Indien dit is geconfigureerd, wordt er een token uit uw tokenbron opgehaald.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bekijk JavaScript-voorbeelden op GitHub](https://aka.ms/csspeech/samples)

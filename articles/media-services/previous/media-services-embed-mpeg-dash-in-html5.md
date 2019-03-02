---
title: Een MPEG-DASH adaptieve Streaming video's insluiten in een HTML5-toepassing met DASH.js | Microsoft Docs
description: In dit onderwerp ziet u hoe u een MPEG-DASH adaptieve Streaming Video insluiten in een HTML5-toepassing met DASH.js.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: ab9bf868191238d933ae301b9a7df293842e8e36
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247049"
---
# <a name="embedding-an-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Een MPEG-DASH adaptieve Streaming video's insluiten in een HTML5-toepassing met DASH.js  

## <a name="overview"></a>Overzicht
MPEG-DASH is een ISO-norm voor adaptief streamen van video-inhoud, biedt aanzienlijke voordelen voor ontwikkelaars die aan het leveren van hoge kwaliteit, adaptief streamen uitvoer. Met MPEG-DASH, de videostream automatisch aangepast aan de definitie van een lagere wanneer het netwerk wordt overbelast. Dit vermindert de kans van de viewer voor een 'onderbroken' video zien terwijl de speler wordt gedownload de volgende paar seconden om af te spelen (ook wel buffer). Zoals opstoppingen in het netwerk wordt beperkt, wordt op zijn beurt de videospeler naar een hogere kwaliteit-stream geretourneerd. Deze mogelijkheid om aan te passen aan de bandbreedte die vereist zijn ook van resulteert in een snellere starttijd voor video. Dat betekent dat de eerste paar seconden kunnen worden afgespeeld in een lagere kwaliteit segment voor snel te downloaden en vervolgens de buffer van stap tot een hogere kwaliteit eenmaal voldoende inhoud is opgeslagen.

Dash.js is een open-source MPEG-DASH videospeler die zijn geschreven in JavaScript. Ons doel is om te bieden een robuuste, platformonafhankelijke speler die naar eigen inzicht in toepassingen waarvoor afspelen van video's kan worden hergebruikt. Het biedt MPEG-DASH afspelen in een browser die ondersteuning biedt voor de W3C Media bron extensies (MSE) vandaag die Chrome, Microsoft Edge en IE11 (andere browsers hebt aangegeven dat de bedoeling ter ondersteuning van MSE). Voor meer informatie over DASH.js Zie js dash.js GitHub-opslagplaats.

## <a name="creating-a-browser-based-streaming-video-player"></a>Het maken van een browser gebaseerde streaming videospeler
Voor het maken van een eenvoudige pagina met een videospeler met de verwachte bepaalt u of deze een play, onderbreken, terugspoelen enz., moet u:

1. Maak een HTML-pagina
2. De video-tag toevoegen
3. De speler dash.js toevoegen
4. Initialiseren van de speler
5. Toevoegen van een CSS-stijl
6. Bekijk de resultaten in een browser die wordt geïmplementeerd MSE

Tijdens de initialisatie van de speler kan worden uitgevoerd in een paar regels van JavaScript-code. Met dash.js echt is het eenvoudig MPEG-DASH-videostream insluiten in uw browser gebaseerde toepassingen.

## <a name="creating-the-html-page"></a>Het maken van de HTML-pagina
De eerste stap is het maken van een standaard HTML-pagina met de **video** -element, sla dit bestand als basicPlayer.html, zoals het volgende voorbeeld laat zien:

```html
    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>
```

## <a name="adding-the-dashjs-player"></a>Toevoegen van de speler DASH.js
De dash.js referentie-implementatie toevoegen aan de toepassing, moet u voor het bestand dash.all.js van versie 1.0 van dash.js project. Dit moet worden opgeslagen in de JavaScript-map van uw toepassing. Dit bestand is een gemak-bestand dat alle benodigde dash.js code samen tot één bestand haalt. Hebt u een overzicht over de opslagplaats dash.js, u de afzonderlijke bestanden niet vinden, test code en nog veel meer, maar als u wilt doen is dash.js, gebruik dan het bestand dash.all.js is wat u nodig hebt.

De speler dash.js toevoegen aan uw toepassingen, toevoegen een scriptcode op de head-sectie van basicPlayer.html:

```html
    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>
```

Maak vervolgens een functie voor het initialiseren van de speler wanneer de pagina wordt geladen. Het volgende script toevoegen na de regel waarin u dash.all.js laden:

```html
    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>
```

Deze functie maakt eerst een DashContext. Dit wordt gebruikt om de toepassing voor een specifieke runtime-omgeving te configureren. Vanuit technisch oogpunt Hiermee definieert u de klassen die voor de afhankelijkheid injectie framework wordt gebruikt bij het maken van de toepassing. In de meeste gevallen moet u Dash.di.DashContext gebruiken.

Vervolgens exemplaar maken van de primaire klasse van het framework dash.js, Media Player. Deze klasse bevat de belangrijkste methoden die nodig zijn, zoals starten en onderbreken, beheert de relatie met de video-element en beheert ook de interpretatie van het bestand Media presentatie beschrijving (MPD), waarin wordt beschreven van de video wordt afgespeeld.

De functie startup() van de Media Player-klasse wordt aangeroepen om ervoor te zorgen dat de speler klaar is voor video afspelen. Onder andere de functie zorgt ervoor dat alle benodigde klassen (zoals gedefinieerd door de context) zijn geladen. Zodra de speler gereed is, kunt u het video-element toe met de functie attachView() koppelen. De opstartfunctie kunt de Media Player op de videostream invoeren in het element en ook het afspelen indien nodig.

De URL van het bestand MPD doorgeven aan de Media Player, zodat deze op de hoogte van de video dat is het waarschijnlijk om af te spelen. De zojuist gemaakte setupVideo()-functie moet worden uitgevoerd nadat de pagina volledig is geladen. Dit doen met behulp van de gebeurtenis onload van de body-element. Wijzig uw <body> element:

```html
    <body onload="setupVideo()">
```

Tot slot stelt u de grootte van de video-element met CSS. In een omgeving met adaptieve streaming is dit vooral belangrijk omdat de grootte van de video wordt afgespeeld verschilt mogelijk als afspelen zich aanpast aan de veranderende netwerkomstandigheden. In deze eenvoudige demo gewoon het video-element moet 80% van de beschikbare browservenster door de volgende CSS toe te voegen aan de hoofd-sectie van de pagina afdwingen:

```html
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>
```

## <a name="playing-a-video"></a>Een Video afspelen
Een video afspelen, wijst u in uw browser naar het bestand basicPlayback.html en klikt u op play op de video speler weergegeven.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Videospelertoepassingen ontwikkelen](media-services-develop-video-players.md)

[GitHub-opslagplaats voor dash.js](https://github.com/Dash-Industry-Forum/dash.js) 


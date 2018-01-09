---
title: Een MPEG-DASH adaptieve Streaming Video insluiten in een toepassing HTML5 met DASH.js | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u een MPEG-DASH adaptieve Streaming Video in een toepassing HTML5 met DASH.js insluiten.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: be0fc51574950cad0558a85b3f20f8b14eafda13
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="embedding-an-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Een MPEG-DASH adaptieve Streaming Video insluiten in een toepassing HTML5 met DASH.js
## <a name="overview"></a>Overzicht
MPEG-DASH is een ISO-norm voor adaptief streamen van video-inhoud, die biedt aanzienlijke voordelen voor gebruikers die u wilt leveren van hoge kwaliteit, adaptief videostreaming-uitvoer. Met MPEG-DASH wordt de video-stream automatisch verwijderen voor de definitie van een lagere wanneer het netwerk overbelast raakt. Dit vermindert de kans van de viewer 'onderbroken' video zien terwijl de speler wordt gedownload de volgende enkele seconden om af te spelen (aka buffer). Zoals opstoppingen in het netwerk wordt beperkt, wordt op zijn beurt de video speler naar een hogere kwaliteit-stroom geretourneerd. Deze mogelijkheid aan te passen aan de vereiste bandbreedte resulteert ook in een snellere begintijd voor video. Dat betekent dat de eerste paar seconden kunnen worden afgespeeld in een lagere quality-segment van fast te downloaden en vervolgens de buffer van stap tot een hogere eenmaal voldoende kwaliteit-inhoud is opgeslagen.

Dash.js is een open source MPEG-DASH-speler geschreven in JavaScript. Het doel is te bieden een robuuste, platformoverschrijdende-speler vrijelijk in toepassingen waarvoor afspelen van video's kan worden gebruikt. Het biedt MPEG-DASH afspelen in browsers die ondersteuning biedt voor W3C Media bron extensies (muis) vandaag Chrome, Microsoft Edge en IE11 (andere browsers hebt aangegeven dat hun bedoeld ter ondersteuning van de muis). Js Zie voor meer informatie over DASH.js de GitHub-opslagplaats dash.js.

## <a name="creating-a-browser-based-streaming-video-player"></a>Maken van een browser gebaseerde streaming video-speler
Besturingselementen voor het maken van een eenvoudige pagina die wordt weergegeven een video-speler met de verwachte dergelijke een play, onderbreken, terugspoelen enz., moet u:

1. Maken van een HTML-pagina
2. De video-tag toevoegen
3. Windows media player dash.js toevoegen
4. Windows media player initialiseren
5. Sommige CSS-stijl toevoegen
6. Bekijk de resultaten in een browser die u, muis implementeert

Tijdens de initialisatie van de speler kan worden voltooid in slechts een handvol regels van JavaScript-code. Met dash.js echt is het eenvoudig voor het insluiten van video MPEG-DASH in uw browser gebaseerde toepassingen.

## <a name="creating-the-html-page"></a>Maken van de HTML-pagina
De eerste stap is het maken van een standaard HTML-pagina met de **video** element Sla dit bestand als basicPlayer.html, zoals het volgende voorbeeld laat zien:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-the-dashjs-player"></a>Windows Media Player DASH.js toevoegen
Voor de implementatie van dash.js verwijzing toevoegen aan de toepassing, moet u het bestand dash.all.js van versie 1.0 van dash.js project halen. Dit moet worden opgeslagen in de JavaScript-map van uw toepassing. Dit bestand is een gemak-bestand dat alle benodigde dash.js code tot één bestand verzamelt. Als u een kijkje rond de opslagplaats dash.js hebt, u de afzonderlijke bestanden niet vinden, test code en nog veel meer, maar als u wilt doen is dash.js, gebruik dan het bestand dash.all.js is wat u nodig hebt.

Toevoegen als u wilt de speler dash.js toevoegen aan uw toepassingen, scriptcode naar de sectie head van basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Maak vervolgens een functie voor het initialiseren van de speler als de pagina wordt geladen. Het volgende script toevoegen na de regel waar u dash.all.js laden:

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

Deze functie maakt eerst een DashContext. Dit wordt gebruikt voor het configureren van de toepassing voor een specifieke runtime-omgeving. Vanuit technisch oogpunt Hiermee definieert u de klassen die voor de afhankelijkheid injectie framework wordt gebruikt bij het maken van de toepassing. In de meeste gevallen moet u Dash.di.DashContext gebruiken.

Vervolgens exemplaar maken van de primaire klasse van het framework dash.js, Media Player. Deze klasse bevat de belangrijkste methoden die nodig zijn, zoals afspelen en onderbreken, beheert de relatie met de video-element en beheert ook de interpretatie van het bestand Media presentatie beschrijving (MPD), die beschrijft de video wordt afgespeeld.

De functie startup() van de Media Player-klasse wordt aangeroepen om ervoor te zorgen dat Windows media player gereed is voor het afspelen van video. Onder andere deze functie zorgt ervoor dat alle benodigde klassen (zoals gedefinieerd door de context) zijn geladen. Zodra de speler gereed is, kunt u de video element aan met behulp van de functie attachView() koppelen. Hierdoor kunnen de Media Player naar de videostream invoeren in het element en ook het afspelen indien nodig.

De URL van het bestand MPD doorgeven aan de Media Player, zodat deze op de hoogte van de video dat naar verwachting af te spelen. De zojuist gemaakte setupVideo()-functie moet worden uitgevoerd nadat de pagina volledig is geladen. Dit doen met behulp van de gebeurtenis onload van het body-element. Wijzig uw <body> element op:

    <body onload="setupVideo()">

Tot slot stelt u de grootte van de video-element met CSS. In een omgeving met adaptieve streaming is dit vooral belangrijk omdat de grootte van de video wordt afgespeeld veranderen kan als afspelen aanpast aan veranderende netwerkomstandigheden. In deze eenvoudige demo gewoon de video-element moet 80% van de beschikbare browservenster door de volgende CSS toe te voegen aan de head-sectie van de pagina afdwingen:

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a>Het afspelen van Video
Als u wilt afspelen van video, wijst u in uw browser naar het bestand basicPlayback.html en op afspelen op de video player weergegeven.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Videospelertoepassingen ontwikkelen](media-services-develop-video-players.md)

[GitHub-opslagplaats voor dash.js](https://github.com/Dash-Industry-Forum/dash.js) 


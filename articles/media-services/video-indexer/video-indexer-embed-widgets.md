---
title: Video Indexer widgets insluiten in uw toepassingen
titlesuffix: Azure Media Services
description: Ontdek hoe u Video Indexer-widgets insluit in uw toepassingen.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 73ceb0a92b97e90b1fdb0c5562d623505e86b870
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891580"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Video Indexer widgets insluiten in uw toepassingen

In dit artikel leeft u hoe u Video Indexer-widgets insluit in uw toepassingen. Video Indexer biedt ondersteuning voor insluiten twee typen widgets in uw toepassing: **Cognitieve inzichten** en **Player**. 

Vanaf versie 2, bevat de widget basis-URL van het account regio. Bijvoorbeeld, een account in de regio VS-West genereert: `https://wus2.videoindexer.ai/embed/insights/...`.

## <a name="widget-types"></a>Typen widget

### <a name="cognitive-insights-widget"></a>Widget Inzichten

De widget **Inzichten** bevat alle visuele inzichten die tijdens het indexeringsproces zijn opgehaald uit uw video. De widget voor inzichten ondersteunt de volgende optionele URL-parameters:

|Name|Definitie|Description|
|---|---|---|
|widgets|Tekenreeksen gescheiden door komma's|Hiermee beheert u welke inzichten u wilt weergeven. <br/>Voorbeeld: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search` geeft alleen inzichten over personen en merken weer in de gebruikersinterface<br/>Beschikbare opties: mensen, trefwoorden, aantekeningen, merken, gevoelens, transcript, zoeken.<br/>niet ondersteund via de URL bij version=2<br/><br/>**Opmerking:** De widgets URL-parameter wordt niet ondersteund in versie 2. |

### <a name="player-widget"></a>Widget Speler

Met de widget **Speler** kunt u de video streamen met gebruik van een adaptieve bitrate. De spelerwidget ondersteunt de volgende optionele URL-parameters:

|Name|Definitie|Description|
|---|---|---|
|t|Seconden vanaf het begin|Laat de speler starten met afspelen vanaf het opgegeven moment.<br/>Voorbeeld: t=60|
|captions|Taalcode|Haalt de ondertiteling in de opgegeven taal op tijdens het laden van de widget zodat deze beschikbaar is in het menu voor ondertiteling.<br/>Voorbeeld: captions=nl-NL|
|showCaptions|Een booleaanse waarde|Zorgt ervoor dat speler wordt geladen met de ondertiteling al ingeschakeld.<br/>Voorbeeld: showCaptions=true|
|type||Hiermee activeert u de weergave van een audiospeler (videogedeelte wordt verwijderd).<br/>Bijvoorbeeld: type=audio|
|autoplay|Een booleaanse waarde|Geeft aan of de speler het afspelen van de video moet starten wanneer deze wordt geladen (de standaardinstelling is true).<br/>Voorbeeld: autoplay=false|
|language|Taalcode|Hiermee bepaalt u de taal van de speler (de standaardinstelling is en-US)<br/>Voorbeeld: language=nl-NL|

## <a name="embedding-public-content"></a>Openbare inhoud insluiten

1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.
2. Klik op de video waarmee u wilt werken.
3. Klik op de knop Insluiten die onder de video wordt weergegeven.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Nadat u op de knop hebt geklikt, wordt een modaal venster weergegeven waarin u kunt kiezen welke widget u wilt insluiten in uw toepassing.
    Als u een widget selecteert (**Speler** of **Inzichten**), wordt de invoegcode voor u gegenereerd. Deze kunt u plakken in uw toepassing.
 
4. Kies het type widget dat u wilt gebruiken (**Inzichten** of **Speler**).
5. Kopieer de invoegcode en voeg deze toe aan uw toepassing. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Persoonlijke inhoud insluiten

U kunt u invoegcodes van insluitingspop-ups (zoals weergegeven in de vorige sectie) alleen voor **openbare** video's gebruiken. 

Als u een **persoonlijke** video wilt insluiten, moet u een toegangstoken doorgeven in het kenmerk **src** van de **iframe**:

`https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>/?accessToken=<accessToken>`
    
Gebruik de API [**Inzichtenwidget ophalen**](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) om de inhoud van de widget Inzichten op te halen, of gebruik [**Toegangstoken van video ophalen**](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) en voeg deze toe als een queryparameter aan de URL, zoals hierboven. Geef deze URL op als de waarde **src** van de **iframe**.

Als u de inzichtmogelijkheden wilt bewerken (zoals we in de webtoepassing hebben gedaan) in uw ingesloten widget, moet u een toegangstoken doorgeven met machtigingen voor bewerken. Gebruik [**Inzichtenwidget ophalen**](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) of [**Toegangstoken van video ophalen**](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) met **&allowEdit=true**. 

## <a name="widgets-interaction"></a>Interactie van widgets

De widget **Inzichten** kan communiceren met een video in uw toepassing. In deze sectie wordt beschreven hoe u deze interactie bereikt.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Communicatie van verschillende bronnen

Als u Video Indexer-widgets wilt laten communiceren met andere onderdelen, doet de Video Indexer-service het volgende:

- De HTML5-methode **postMessage** voor communicatie van verschillende bronnen wordt gebruikt en 
- Het bericht wordt gevalideerd op basis van de bron VideoIndexer.ai. 

Als u kiest voor het implementeren van uw eigen spelercode en het uitvoeren van de integratie met **Inzichten**-widgets, is het uw verantwoordelijkheid om de oorsprong van het bericht dat afkomstig is van VideoIndexer.ai te valideren.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Beide typen widgets insluiten in uw toepassing/blog (aanbevolen) 

In deze sectie wordt beschreven hoe u interactie bereikt tussen twee Video Indexer-widgets. Wanneer een gebruiker op het besturingselement voor inzicht in uw toepassing klikt, gaat de speler dan naar het gewenste moment.

`<script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>`

1. Kopieer de invoegcode van de widget **Speler**.
2. Kopieer de invoegcode van de widget **Inzichten**.
3. Voeg het [**Mediator-bestand**](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) voor het afhandelen van de communicatie tussen de twee widgets toe:

`<script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>`

Wanneer een gebruiker nu op het besturingselement voor inzicht in uw toepassing klikt, gaat de speler naar het gewenste moment.

Ga voor meer informatie naar [deze demo](https://codepen.io/videoindexer/pen/NzJeOb).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>De widget Inzichten insluiten en Azure Media Player gebruiken om de inhoud af te spelen

In deze sectie wordt beschreven hoe u interactie tussen de widget **Inzichten** en een Azure Media Player-exemplaar bereikt met de [AMP-invoegtoepassing](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Voeg een Video Indexer-invoegtoepassing voor de AMP-speler toe.<br/> `<script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>`
2. Instantieer Azure Media Player met de Video Indexer-invoegtoepassing.

        // Init Source
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // Here is how to load vtt from VI, you can replace it with your vtt url.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plugin
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true
            });

            // Set the source dynamically
            initSource.call(this);
        });

3. Kopieer de invoegcode van de widget **Inzichten**.

Het zou nu mogelijk moeten zijn om te communiceren met uw Azure Media Player.

Ga voor meer informatie naar [deze demo](https://codepen.io/videoindexer/pen/rYONrO).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>De widget Inzichten van Video Indexer insluiten en uw eigen speler (kan elke speler zijn) gebruiken

Als u uw eigen speler gebruikt, moet u er zelf voor zorgen dat u de speler bewerkt zodat u ermee kunt communiceren. 

1. Voeg uw videospeler in.

    Gebruik bijvoorbeeld een standaard HTML5-speler.

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Sluit de widget Inzichten in.
3. Implementeer communicatie voor de speler door te luisteren naar de gebeurtenis 'message'. Bijvoorbeeld:

        <script>
    
            (function(){
            // Reference your player instance
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Here you need to call your player "jumpTo" implementation
                playerInstance.currentTime = evt.data.time;
               
                // Confirm arrival to us
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to message event
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>

Ga voor meer informatie naar [deze demo](https://codepen.io/videoindexer/pen/YEyPLd).

## <a name="adding-subtitles"></a>Ondertiteling toevoegen

Als u Video Indexer-inzichten insluit met uw eigen AMP-speler, kunt u de **GetVttUrl**-methode voor het ophalen van ondertiteling gebruiken. U kunt ook een JavaScript-methode aanroepen vanuit de AMP-invoegtoepassing **getSubtitlesUrl** van Video Indexer (zoals eerder besproken). 

## <a name="customizing-embeddable-widgets"></a>Insluitbare widgets aanpassen

### <a name="cognitive-insights-widget"></a>Widget Inzichten

U kunt de typen inzichten die u wilt dat door deze op te geven als een waarde kiezen op de volgende URL-parameter toegevoegd aan de ingesloten code die u krijgt (van de API of van de web-App): `&widgets=<list of wanted widgets>`.

De mogelijke waarden zijn: mensen, trefwoorden, gevoelens, transcript, zoeken.

Bijvoorbeeld, als u wilt insluiten er een widget alleen inzichten voor personen en zoek het iframe insluitings-URL die als volgt:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search`

De titel van het venster iframe kan ook worden aangepast door te geven `&title=<YourTitle>` naar de URL van de iframe. (Hiermee wordt de HTML-code aangepast naar \<title>waarde).
    
Bijvoorbeeld, als u wilt uw iframe-venster geeft de titel 'MyInsights', er de URL als volgt:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?title=MyInsights`

U ziet dat deze optie alleen relevant is voor gevallen waarbij u de inzichten in een nieuw venster moet openen.

### <a name="player-widget"></a>Widget Speler

Bij het insluiten van een Video Indexer-speler kunt u de grootte van de speler kiezen door de grootte van de iframe op te geven.

Bijvoorbeeld:

`<iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/" frameborder="0" allowfullscreen />`

De Video Indexer-speler heeft standaard automatisch gegenereerde ondertiteling op basis van het transcript van de video dat is geëxtraheerd uit de video met de brontaal die is geselecteerd bij het uploaden van de video.

Als u insluiten met een andere taal wilt, kunt u toevoegen `&captions=< Language | ”all” | “false” >` op de ingesloten speler-URL of put 'alle' als de waarde als u wilt dat alle beschikbare talen bijschriften.
Als u wilt dat de ondertiteling moet worden standaard weergegeven, kunt u doorgeven `&showCaptions=true`.

De ingesloten URL wordt vervolgens er als volgt: 

`https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/?captions=italian`

Als u ondertiteling wilt uitschakelen, kunt u 'false' als waarde opgeven voor de ondertitelingsparameter.

Automatisch afspelen: standaard speelt de speler de video af. U ervoor kiezen dit niet te doen door &autoplay=false op te geven in de insluitings-URL hierboven.

## <a name="next-steps"></a>Volgende stappen

Zie [dit](video-indexer-view-edit.md) artikel voor meer informatie over het weergeven en bewerken van Video Indexer-inzichten.

Lees ook, [Video indexer CodePen](https://codepen.io/videoindexer/pen/eGxebZ).

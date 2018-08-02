---
title: Azure Video Indexer widgets insluiten in uw toepassingen | Microsoft Docs
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 4ce1eedac69b06f491510be93fcfbdfbc879ff7e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398129"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Video Indexer widgets insluiten in uw toepassingen

Video Indexer biedt ondersteuning voor insluiten twee typen widgets in uw toepassing: **cognitieve inzichten** en **Player**. 

* Een **cognitieve inzichten** widget bevat alle visual inzichten die zijn opgehaald uit uw video verwerken indexeren. 
    De widget insights ondersteunt de volgende optionele parameters van URL:

    |Naam|Definitie|Beschrijving|
    |---|---|---|
    |widgets|Tekenreeksen gescheiden door komma 's|Kunt u voor het beheren van de inzichten die u wilt weergeven. <br/>Voorbeeld: **widgets personen, merken =** worden weergegeven alleen personen en merken ui-inzichten<br/>Beschikbare opties: personen, trefwoorden, aantekeningen, merken, Sentimenten, Transcript, zoeken | 
* Een **Player** widget kunt u de video die gebruikmaakt van adaptieve bitrate stream.

    De widget player ondersteunt de volgende optionele parameters van URL:

    |Naam|Definitie|Beschrijving|
    |---|---|---|
    |t|Seconden vanaf het begin|Maakt de speler afspelen starten vanaf het moment dat bepaalde verwijzen.<br/>Voorbeeld: t = 60|
    |bijschriften|Taalcode|Het bijschrift in de opgegeven taal kan worden opgehaald tijdens de widget zijn beschikbaar in het menu van bijschriften wordt geladen.<br/>Voorbeeld: bijschriften = en-Us|
    |showCaptions|Een Booleaanse waarde|Maakt de speler laden met de bijschriften al ingeschakeld.<br/>Voorbeeld: showCaptions = true|
    |type||Hiermee activeert u de weergave van een audiospeler (video onderdeel wordt verwijderd).<br/>Bijvoorbeeld: Typ = audio|
    |automatisch afspelen|Een Booleaanse waarde|Bepalen of de speler het afspelen van video starten moet wanneer geladen (de standaardinstelling is true).<br/>Voorbeeld: automatisch afspelen = false|
    |Taal|Taalcode|Besturingselement de speler te lokaliseren besturingselementen (standaard is en-US)<br/>Voorbeeld: taal = nl-nl|

## <a name="embedding-public-content"></a>Openbare inhoud insluiten

1. Aanmelden bij uw [Video Indexer](https://api-portal.videoindexer.ai/) account. 
2. Klik op de knop 'ingesloten' wordt weergegeven onder de video.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Na het klikken op de knop, een modale insluiten wordt weergegeven op het scherm waarin u welk object kiezen kunt u wilt insluiten in uw toepassing.
    Een widget selecteren (**Player** of **cognitieve inzichten**), genereert de ingesloten code voor u om te plakken in uw toepassing.
 
3. Kies het type object dat u wilt dat (**cognitieve inzichten** of **Player**).
4. Kopieer de ingesloten code en toevoegen aan uw toepassing. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Persoonlijke inhoud insluiten

U kunt u insluiten-codes van insluiten pop-ups (zoals weergegeven in de vorige sectie) voor **openbare** alleen video's. 

Als u wilt insluiten een **persoonlijke** video's die u hebt om door te geven van een toegangstoken in de **iframe**van **src** kenmerk:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<VideoId>/?accessToken=<accessToken>
    
Gebruik de [ **ophalen Insights Widget** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) API om op te halen van de inhoud van de widget cognitieve inzichten of gebruik [ **Video Access Token ophalen** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) en toe te voegen die als een query param naar de url, zoals hierboven. Geef deze URL als de **iframe**van **src** waarde.

Als u wilt bewerken insights-mogelijkheden (zoals we in de webtoepassing hebben) in uw ingesloten widget bieden, moet u doorgeven van een toegangstoken met machtigingen voor bewerken. Gebruik [ **ophalen Insights Widget** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) of [ **Video Access Token ophalen** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) met **& bewerken = true**. 

## <a name="widgets-interaction"></a>Widgets interactie

De **cognitieve inzichten** widget kan communiceren met een video op uw toepassing. Deze sectie wordt beschreven hoe u deze interactie bereiken.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Cross-origin-communicatie

Als u Video Indexer-widgets om te communiceren met andere onderdelen, doet de Video Indexer-service in het volgende:

- De cross-origin-communicatie HTML5 methode **postMessage** en 
- Het bericht valideert over VideoIndexer.ai oorsprong. 

Als u kiest voor het implementeren van uw eigen player-code en voer de integratie met **cognitieve inzichten** widgets, is uw verantwoordelijkheid om te valideren van de oorsprong van het bericht dat afkomstig van VideoIndexer.ai is.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Beide typen widgets insluiten in uw toepassing / blog (aanbevolen) 

Deze sectie wordt beschreven hoe interactie tussen twee Video Indexer-widgets bereiken, wanneer een gebruiker op het besturingselement inzicht in uw toepassing, de speler gaat u naar het gewenste moment.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Kopieer de **Player** invoegcode widget.
2. Kopieer de **cognitieve inzichten** invoegcode.
3. Voeg de [ **Mediator bestand** ](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) voor het afhandelen van de communicatie tussen de twee widgets:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Wanneer een gebruiker op het besturingselement inzicht in uw toepassing, gaat de speler nu het gewenste moment.

Zie voor meer informatie, [deze demo](https://api-portal.videoindexer.ai/demo-all-widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Sluit de widget cognitieve inzichten in en gebruik van Azure Media Player de inhoud af te spelen

Deze sectie wordt beschreven voor het bereiken van interactie tussen een **cognitieve inzichten** widget en een Azure Media Player-exemplaar met de [AMP invoegtoepassing](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Een Video Indexer-invoegtoepassing voor de speler AMP toevoegen.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Azure Media Player instantiëren met de Video Indexer-invoegtoepassing.

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

3. Kopieer de **cognitieve inzichten** invoegcode.

U moet mogelijk nu om te communiceren met uw Azure Media Player.

Zie voor meer informatie, [deze demo](https://api-portal.videoindexer.ai/demo-your-amp).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Insluiten van Video Indexer-cognitieve inzichten widget en uw eigen player (kan elk speler worden) gebruiken

Als u uw eigen player gebruikt, hebt u zorgen voor het bewerken van de speler zelf om te realiseren van de communicatie. 

1. Plaats uw video speler.

    Bijvoorbeeld, een standaard HTML5-speler

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Sluit de widget cognitieve inzichten.
3. Communicatie van de speler door te luisteren naar de gebeurtenis "message" implementeert. Bijvoorbeeld:

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


Zie voor meer informatie, [deze demo](https://api-portal.videoindexer.ai/demo-your-player).

## <a name="adding-subtitles"></a>Ondertiteling toe te voegen

Als het insluiten van Video Indexer-inzichten met uw eigen AMP-speler kunt u de **GetVttUrl** methode voor het ophalen van ondertiteling (ondertiteling). U kunt ook een javascript-methode aanroepen vanuit de invoegtoepassing Video Indexer AMP **getSubtitlesUrl** (zoals eerder besproken). 

## <a name="customizing-embeddable-widgets"></a>Ingesloten widgets aanpassen

### <a name="cognitive-insights-widget"></a>Widget cognitieve inzichten
U kunt de typen inzichten die u wilt dat door deze op te geven als een waarde kiezen op de volgende URL-parameter toegevoegd aan de ingesloten code die u krijgt (van de API of van de web-App):

**& widgets =** \<lijst van de gewenste widgets >

De mogelijke waarden zijn: mensen, trefwoorden, sentimenten, transcript, zoeken.

Bijvoorbeeld, als u wilt insluiten een widget alleen inzichten voor personen en zoek het iframe insluitings-URL die er als volgt: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?widgets=people, zoeken

De titel van het venster iframe kan ook worden aangepast door te geven **& titel =** <YourTitle> naar de url van de iframe. (Deze wordt de HTML-code aanpassen \<titel > waarde).
Bijvoorbeeld, als u wilt uw iframe-venster geeft de titel 'MyInsights', de url er als volgt: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?title=MyInsights. U ziet dat deze optie alleen relevant voor gevallen is wanneer moet u de inzichten in een nieuw venster openen.

### <a name="player-widget"></a>Player-widget
Als het insluiten van Video Indexer-speler kunt u de grootte van de speler kiezen door op te geven van de grootte van de iframe.

Bijvoorbeeld:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/{id}” frameborder="0" allowfullscreen />

Door Video Indexer-standaardspeler wordt hebt automatisch gegenereerde ondertiteling op basis van het transcript van de video die is geëxtraheerd uit de video met de source-taal die is geselecteerd bij de video is geüpload.

Als u wilt insluiten met een andere taal kunt u toevoegen **& bijschriften = < taal | 'alle' | "false" >** op de ingesloten speler-URL of put 'alle' als de waarde als u wilt dat alle beschikbare talen bijschriften.
Als u wilt dat de ondertiteling moet worden weergegeven standaard kunt u doorgeven **& showCaptions = true**

De ingesloten URL vervolgens wordt er als volgt: https://www.videoindexer.ai/embed/player/9a296c6ec3/?captions=italian. Als u wilt uitschakelen bijschriften kunt u 'false' als waarde voor bijschriften parameter doorgeven.

Automatisch afspelen – standaard die de speler de video wordt afgespeeld. u kunt geen door te geven & automatisch afspelen = false om de ingesloten URL hierboven.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het weergeven en bewerken van Video Indexer-inzichten [dit](video-indexer-view-edit.md) artikel.

## <a name="see-also"></a>Zie ook

[Video Indexer-overzicht](video-indexer-overview.md)

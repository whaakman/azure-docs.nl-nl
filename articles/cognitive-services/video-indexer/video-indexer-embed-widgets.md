---
title: Azure Video indexeerfunctie widgets insluiten in uw toepassingen | Microsoft Docs
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 04/04/2018
ms.author: juliako
ms.openlocfilehash: 8da06253c58a2de474e879f18013fa1e57f5668d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345826"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Het insluiten van Video indexeerfunctie widgets in uw toepassingen

Video indexeerfunctie ondersteunt insluiten twee soorten widgets in uw toepassing: **cognitieve Insights** en **Player**. 

* Een **cognitieve Insights** widget bevat alle visual inzichten die zijn opgehaald uit uw video proces te indexeren. 
    De widget insights ondersteunt de volgende optionele parameters voor URL:

    |Naam|Definitie|Beschrijving|
    |---|---|---|
    |widgets|Tekenreeksen gescheiden door komma|Kunt u bepalen de inzichten die u wilt weergeven. <br/>Voorbeeld: **widgets personen, merken =** verschijnt alleen mensen en merken ui insights<br/>Beschikbare opties: personen, trefwoorden, aantekeningen, merken, patronen, de tekst, zoeken | 
* Een **Player** widget kunt u de video met adaptive bitrate stream.

    Het object player ondersteunt de volgende optionele parameters voor URL:

    |Naam|Definitie|Beschrijving|
    |---|---|---|
    |t|Seconden van starten|Maakt de player start vanaf het moment dat opgegeven spelen verwijzen.<br/>Voorbeeld: t = 60|
    |bijschriften|Taalcode|Het bijschrift in de opgegeven taal tijdens het laden zijn beschikbaar in het menu bijschriften widget kan worden opgehaald.<br/>Voorbeeld: bijschriften = en-Us|
    |showCaptions|Een Booleaanse waarde|Maakt de speler geladen met de bijschriften al ingeschakeld.<br/>Voorbeeld: showCaptions = true|
    |type||Hiermee activeert u een audio player skin (video onderdeel wordt verwijderd).<br/>Bijvoorbeeld: Typ = audio|
    |automatisch afspelen|Een Booleaanse waarde|Bepalen of de speler moet worden gestart wordt afgespeeld wanneer geladen (de standaardwaarde is true).<br/>Voorbeeld: automatisch afspelen = false|
    |taal|Taalcode|Besturingselement Windows media player besturingselementen lokalisatie (de standaardwaarde is en-US)<br/>Voorbeeld: taal = nl-nl|

## <a name="embedding-public-content"></a>Openbare inhoud insluiten

1. Aanmelden bij uw [Video indexeerfunctie](https://api-portal.videoindexer.ai/) account. 
2. Klik op de knop 'insluiten' onder de video wordt weergegeven.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Wanneer u op de knop, een insluiten modaal wordt weergegeven op het scherm waarin u welke widget kiezen kunt u wilt insluiten in uw toepassing.
    Als u een widget (**Player** of **cognitieve Insights**), genereert de ingesloten code kunt plakken in uw toepassing.
 
3. Kies het type object die u wilt dat (**cognitieve Insights** of **Player**).
4. De ingesloten code kopiëren en toevoegen aan uw toepassing. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Persoonlijke inhoud insluiten

U kunt ophalen insluiten-codes van insluiten pop-ups (zoals weergegeven in de vorige sectie) voor **openbare** alleen video's. 

Als u wilt insluiten een **persoonlijke** video die u hebt om door te geven van een toegangstoken in de **iframe**van **src** kenmerk:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<VideoId>/?accessToken=<accessToken>
    
Gebruik de [ **ophalen Insights Widget** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) API om op te halen van de inhoud van de widget cognitieve Insights of gebruik [ **Video Access Token ophalen** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) en toe te voegen die als een query param naar de url, zoals hierboven. Geef de URL als de **iframe**van **src** waarde.

Als u bewerken insights mogelijkheden wilt (zoals we in onze webtoepassing hebben) in uw ingesloten widget geven, moet u een toegangstoken met bewerkingsmachtigingen doorgeven. Gebruik [ **ophalen Insights Widget** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) of [ **Video Access Token ophalen** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) met **& allowEdit = true**. 

## <a name="widgets-interaction"></a>Widgets interactie

De **cognitieve Insights** widget kan communiceren met een video over uw toepassing. Deze sectie laat zien hoe deze interactie bereiken.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Cross-origin-communicatie

Als u Video indexeerfunctie widgets om te communiceren met andere onderdelen, de Video indexeerfunctie-service, gebeurt het volgende:

- De cross-origin-communicatie HTML5-methode **postMessage** en 
- Het bericht valideert over VideoIndexer.ai oorsprong. 

Als u kiest voor het implementeren van uw eigen player-code en voer de integratie met **cognitieve Insights** widgets, is uw verantwoordelijkheid voor het valideren van de oorsprong van het bericht dat afkomstig van VideoIndexer.ai is.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Beide typen widgets insluiten in uw toepassing / blog (aanbevolen) 

Deze sectie wordt beschreven hoe als u de interactie tussen twee Video indexeerfunctie widgets doet wanneer een gebruiker op het besturingselement inzicht in uw toepassing, de speler gaat u naar het gewenste moment.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Kopieer de **Player** widget invoegcode ontvangen.
2. Kopieer de **cognitieve Insights** invoegcode.
3. Voeg de [ **Mediator bestand** ](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) voor het afhandelen van de communicatie tussen de twee widgets:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Wanneer een gebruiker op het besturingselement inzicht in uw toepassing, gaat de speler nu naar het gewenste moment.

Zie voor meer informatie [deze demo](https://api-portal.videoindexer.ai/demo-all-widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>De widget cognitieve Insights insluiten en gebruiken van Azure Media Player de inhoud af te spelen

Deze sectie wordt beschreven hoe als u de interactie tussen een **cognitieve Insights** widgets en een Azure Media Player-exemplaar met de [AMP invoegtoepassing](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Een Video-indexeerfunctie-invoegtoepassing voor de speler AMP toevoegen.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Azure Media Player instantiëren met de invoegtoepassing Video indexeerfunctie.

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

3. Kopieer de **cognitieve Insights** invoegcode.

U moet nu kan communiceren met uw Azure Media Player.

Zie voor meer informatie [deze demo](https://api-portal.videoindexer.ai/demo-your-amp).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Voor het insluiten van Video indexeerfunctie cognitieve Insights widget en gebruikt u uw eigen speler (mogelijk een speler zijn)

Als u uw eigen player gebruikt, hebt u zorgt voor het bewerken van uw speler zelf met het oog op de communicatie. 

1. Voeg uw video player.

    Bijvoorbeeld, een standaard HTML5-speler

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. De widget cognitieve Insights insluiten.
3. Communicatie van de speler door te luisteren naar de berichtgebeurtenis '' implementeren. Bijvoorbeeld:

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


Zie voor meer informatie [deze demo](https://api-portal.videoindexer.ai/demo-your-player).

## <a name="adding-subtitles"></a>Ondertiteling toevoegen

Als u Video indexeerfunctie insights met uw eigen AMP-speler insluiten, kunt u de **GetVttUrl** methode ondertiteling (ondertiteling) ophalen. U kunt ook een javascript-methode aanroepen vanuit de invoegtoepassing Video indexeerfunctie AMP **getSubtitlesUrl** (zoals eerder besproken). 

## <a name="customizing-embeddable-widgets"></a>Ingesloten widgets aanpassen

### <a name="cognitive-insights-widget"></a>Cognitieve insights widget
U kunt de soorten insights die u wilt opgeven als een waarde op de volgende URL-parameter toegevoegd aan de ingesloten code die u krijgt (van API of van de webtoepassing):

**& widgets =** \<lijst met de gewenste widgets >

De mogelijke waarden zijn: mensen trefwoorden, patronen, de tekst, zoeken.

Bijvoorbeeld, als u wilt insluiten een widget met alleen insights voor personen en zoek de iframe insluiten URL ziet er als volgt: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?widgets=people, zoeken

De titel van het venster iframe kan ook worden aangepast door **& titel =** <YourTitle> naar de url iframe. (Het gaat de HTML-code aanpassen \<titel > waarde).
Bijvoorbeeld, als u wilt dat uw iframe-venster de titel 'MyInsights', de url ziet er als volgt: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?title=MyInsights. U ziet dat deze optie alleen relevant voor gevallen is wanneer moet u de inzichten in een nieuw venster openen.

### <a name="player-widget"></a>Player-widget
Als u Video indexeerfunctie speler kunt u de grootte van de speler door te geven van de grootte van de iframe insluiten.

Bijvoorbeeld:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/{id}” frameborder="0" allowfullscreen />

Door standaard Video indexeerfunctie player wordt er automatisch gegenereerd ondertiteling op basis van de inhoud van de video die is opgehaald uit de video met de bron-taal die is geselecteerd toen de video is geüpload.

Als u wilt insluiten met een andere taal kunt u toevoegen **& bijschriften = < taal | 'alle' | "false" >** op de URL van de player insluiten of put 'alle' als de waarde als u wilt alle beschikbare talen bijschriften hebben.
Als u wilt dat de bijschriften worden standaard weergegeven kunt u doorgeven **& showCaptions = true**

De URL insluiten vervolgens ziet er als volgt: https://www.videoindexer.ai/embed/player/9a296c6ec3/?captions=italian. Als u wilt uitschakelen bijschriften kunt u 'false' als waarde voor bijschriften parameter doorgeven.

Automatisch afspelen – standaard die Windows media player de video wordt afgespeeld. u kunt ervoor kiezen geen door doorgeven & automatisch afspelen = false met de bovenstaande insluiten-URL.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het weergeven en bewerken van Video indexeerfunctie insights [dit](video-indexer-view-edit.md) artikel.

## <a name="see-also"></a>Zie ook

[Video indexeerfunctie-overzicht](video-indexer-overview.md)

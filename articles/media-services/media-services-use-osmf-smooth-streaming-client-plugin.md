---
title: Smooth Streaming-invoegtoepassing voor de Media Open-Source Framework
description: Informatie over het gebruik van de Azure Media Services Smooth Streaming-invoegtoepassing voor Adobe Open Source Media Framework.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6068151f-b6b0-4507-9346-f03416d3d572
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 9c764f176ae75085320882de3fb26d8e7d52daaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Het gebruik van de Microsoft-Smooth Streaming-invoegtoepassing voor Adobe Open-Source Media Framework
## <a name="overview"></a>Overzicht
De Microsoft Smooth Streaming-invoegtoepassing voor Open Source Media Framework 2.0 (SS voor OSMF) breidt de voorzieningen standaard van OSMF en inhoud afspelen Microsoft Smooth Streaming naar nieuwe en bestaande OSMF spelers toegevoegd. De invoegtoepassing worden Smooth Streaming afspelen mogelijkheden ook toegevoegd aan stroboscoop Media afspelen (SMP).

SS voor OSMF bevat twee versies van de invoegtoepassing:

* Statische Smooth Streaming-invoegtoepassing voor OSMF (SWC)
* Dynamische Smooth Streaming-invoegtoepassing voor OSMF (.swf)

Dit document wordt ervan uitgegaan dat de lezer een algemene praktische kennis van OSMF en OSMF heeft invoegtoepassingen. Voor meer informatie over OSMF, Raadpleeg de documentatie van de [officiële OSMF site](http://osmf.org/).

### <a name="smooth-streaming-plugin-for-osmf-20"></a>Smooth Streaming-invoegtoepassing voor OSMF 2.0
De invoegtoepassing biedt ondersteuning voor laden en afspelen van Smooth Streaming inhoud op aanvraag met de volgende functies:

* Afspelen op aanvraag Smooth Streaming (Play, onderbreken, Seek, Stop)
* Live afspelen Smooth Streaming (Play)
* Live DVR-functies (onderbreken, Seek, DVR afspelen, Ga-to-Live)
* Ondersteuning voor een video-codecs - H.264
* Ondersteuning voor audiocodecs - AAC
* Meerdere audio taal overschakelen met ingebouwde OSMF-API 's
* Maximale afspelen kwaliteit selectie met ingebouwde OSMF-API 's
* Ter gesloten bijschriften met OSMF bijschriften-invoegtoepassing
* Adobe&reg; Flash&reg; Player 11,4 of hoger.
* Deze versie ondersteunt alleen OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Ondersteunde functies en bekende problemen
Raadpleeg voor een volledige lijst van ondersteunde functies, niet-ondersteunde functies en bekende problemen [dit document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).

## <a name="loading-the-plugin"></a>Bij het laden van de invoegtoepassing
OSMF invoegtoepassingen kunnen worden geladen (op het tijdstip van compilatie) statisch of dynamisch (tijdens runtime). De invoegtoepassing Smooth Streaming OSMF downloaden bevat versies van dynamische en statische.

* Statische laden: als u wilt laden statisch, een statische bibliotheek (SWC)-bestand is vereist. Statische plugins worden toegevoegd als een verwijzing naar de projecten en samenvoegen in de uiteindelijke uitvoer-bestand op het moment van compileren.
* Dynamisch laden: dynamische taakverdeling, een vooraf gecompileerde (SWF)-bestand is vereist. Dynamische invoegtoepassingen zijn geladen in de runtime en niet zijn opgenomen in de projectuitvoer. (Gecompileerde uitvoer) Dynamische invoegtoepassingen kunnen worden geladen met behulp van HTTP- en protocollen.

Zie voor meer informatie over statische en dynamische laden de officiële [OSMF invoegtoepassing pagina](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

### <a name="ss-for-osmf-static-loading"></a>SS voor OSMF statische laden
Het onderstaande codefragment laat zien hoe de invoegtoepassing SS statisch voor OSMF laden en een basic video met behulp van de klasse OSMF MediaFactory afspelen. Voordat u met inbegrip van de SS voor OSMF code, zorg ervoor dat het project een verwijzing de statische 'MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc'-invoegtoepassing bevat.

```
package 
{

    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;



    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }

        private function initMediaPlayer():void
        {

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;

            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a>SS voor OSMF dynamisch laden
Het onderstaande codefragment laat zien hoe de SS-invoegtoepassing voor OSMF dynamisch laden en een eenvoudige afspelen video met behulp van de klasse OSMF MediaFactory. Voordat u met inbegrip van de SS voor OSMF code, de dynamische invoegtoepassing 'MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf' naar de projectmap kopiëren als u wilt laden met bestandsprotocol of kopieer onder een webserver voor HTTP-belasting. Er is niet nodig om op te nemen 'MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc' in de projectverwijzingen.

{het pakket

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets the size of the SWF

    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }

        private function initMediaPlayer():void
        {

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}

## <a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Stroboscoop Media afspelen met de dynamische SS ODMF-invoegtoepassing
De Smooth Streaming voor dynamische OSMF-invoegtoepassing is compatibel met [stroboscoop Media afspelen (SMP)](http://osmf.org/strobe_mediaplayback.html). U kunt de SS voor OSMF invoegtoepassing Smooth Streaming afspelen van inhoud toevoegen aan SMP gebruiken. U doet dit door kopiëren 'MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf' onder een webserver voor HTTP-belasting met behulp van de volgende stappen uit:

1. Bladeren de [stroboscoop Media afspelen installatiepagina](http://osmf.org/dev/2.0gm/setup.html). 
2. De src ingesteld op een bron Smooth Streaming, (bijvoorbeeld http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3. De gewenste configuratiewijzigingen aanbrengen en klikt u op Preview en Update.
   
   **Opmerking** uw webserver inhoud moet een geldige crossdomain.xml. 
4. Kopieer en plak de code in een eenvoudig HTML-pagina met behulp van uw favoriete teksteditor, zoals in het volgende voorbeeld:

        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



1. Smooth Streaming OSMF-invoegtoepassing toevoegen aan de ingesloten code en opslaan.
   
        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>
2. De HTML-pagina opslaan en publiceren naar een webserver. Blader naar de gepubliceerde webpagina met uw favoriete Flash&reg; Player ingeschakeld internetbrowser (Internet Explorer, Chrome, Firefox, enzovoort).
3. Profiteer van Smooth Streaming inhoud in Adobe&reg; Flash&reg; Player.

Zie voor meer informatie over algemene OSMF ontwikkeling de officiële [OSMF ontwikkeling pagina](http://osmf.org/resources.html).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Microsoft adaptieve Streaming-invoegtoepassing voor OSMF Update](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 


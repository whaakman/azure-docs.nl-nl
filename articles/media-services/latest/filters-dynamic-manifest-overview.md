---
title: Filters en Azure Media Services dynamische manifesten | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten voor het bereiken van deze selectief streaming.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/25/2019
ms.author: juliako
ms.openlocfilehash: 7d40a29a0d9d708deb5485b18327d809ded0022d
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959541"
---
# <a name="dynamic-manifests"></a>Dynamische manifesten

Media Services biedt **dynamische manifesten** op basis van vooraf gedefinieerde filters. Nadat u hebt filters gedefinieerd (Zie [filters definiëren](filters-concept.md)), uw clients kunnen gebruiken om een specifieke weergave of subquery korte clips van uw video te streamen. Ze zou filter (s) in de streaming-URL opgeven. Filters kunnen worden toegepast op adaptieve bitrate streamingprotocollen: Apple HTTP Live Streaming (HLS), MPEG-DASH en Smooth Streaming. 

De volgende tabel ziet u enkele voorbeelden van URL's met filters:

|Protocol|Voorbeeld|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|


> [!NOTE]
> De asset en de standaard-manifest voor de activa wijzigen dynamische manifesten niet. De client kunt kiezen om aan te vragen van een stream met of zonder filters. 
> 

Dit onderwerp wordt uitgelegd concepten met betrekking tot **dynamische manifesten** en bevat voorbeelden van scenario's waarin kunt u deze functie wilt gebruiken.

## <a name="manifest-files-overview"></a>Overzicht van de manifest-bestanden

Een **manifest** ()-afspeellijstbestand (op basis van tekst of XML-indeling) bevat metagegevens zoals streaming: type (audio, video of tekst) bijhouden, bijhouden van de naam, begintijd en eindtijd, bitrate (kenmerken), talen, bijhouden, presentatievenster (schuiven venster van vaste duur), video-codec (code). Hiermee geeft u ook de speler om op te halen van het volgende fragment door te geven informatie over het volgende kan worden afgespeeld video fragmenten beschikbaar en hun locatie. Fragmenten (of segmenten) zijn de werkelijke 'segmenten' van een video-inhoud.

Hier volgt een voorbeeld van een manifestbestand HLS: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="get-and-examine-manifest-files"></a>Ophalen en onderzoeken-manifestbestanden

U geeft een lijst van de eigenschap van de filtervoorwaarden bijhouden op basis van waarop de nummers van uw stream (Live of Video on Demand) moeten worden opgenomen in de dynamisch gemaakte manifest. U kunt ophalen en controleert u de eigenschappen van de nummers, die u moet eerst het manifest Smooth Streaming laden.

De [uploaden, coderen en streamen van bestanden met .NET](stream-files-tutorial-with-api.md) zelfstudie leert u hoe u kunt de streaming-URL's met .NET bouwen (zien, de [het bouwen van URL's](stream-files-tutorial-with-api.md#get-streaming-urls) sectie). Als u de app uitvoert, een van de URL verwijst naar het manifest Smooth Streaming: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Kopieer en plak de URL in de adresbalk van een browser. Het bestand wordt gedownload. U kunt deze openen in een teksteditor van uw keuze.

Zie voor het voorbeeld REST [uploaden, coderen en streamen van bestanden met REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>De bitrate van een videostream bewaken

U kunt de [Azure Media Player-demo pagina](http://aka.ms/amp) voor het bewaken van de bitrate van een videostream. De demo-pagina wordt weergegeven voor informatie over de diagnostische gegevens in de **Diagnostics** tabblad:

![Azure Media Player diagnostics][amp_diagnostics]

## <a name="rendition-filtering"></a>Weergavefiltering

U kunt uw asset voor meerdere codering profielen (H.264 basislijn H.264 hoog, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheden coderen. Niet alle clientapparaten ondersteunen echter de profielen en bitrates van uw asset. Oudere Android-apparaten ondersteunen bijvoorbeeld alleen H.264 basislijn + AACL. Hogere bitsnelheden verzenden naar een apparaat, wat kan niet aan de voordelen, verspilt netwerkbandbreedte en apparaat-berekeningen. Dit apparaat moet worden gedecodeerd alle opgegeven informatie, alleen voor omlaag schalen om weer te geven.

Met dynamische Manifest u apparaatprofielen maken, zoals mobiele apparaten, console, HD/SD, enz., en omvatten de sporen te wissen en de kenmerken die u wilt worden van een onderdeel van elk profiel.

![Weergave filteren voorbeeld][renditions2]

In het volgende voorbeeld is een coderingsprogramma gebruikt een tussentijds asset coderen in zeven ISO MP4s video voorinstelling (van 180p 1080p). De gecodeerde asset kan dynamisch worden verpakt in een van de volgende protocollen voor streaming: HLS, MPEG DASH en Smooth.  Aan de bovenkant van het diagram, het HLS-manifest voor de asset met geen filters wordt weergegeven (bevat alle zeven voorinstelling).  In de linksonder, wordt het HLS-manifest waarop een filter met de naam "ott" is toegepast weergegeven. Het filter "ott" Hiermee geeft u als u wilt verwijderen van alle bitsnelheden hieronder 1 Mbps, wat leidde tot de twee quality-niveaus van onder, worden verwijderd uit in het antwoord. In de rechts onderaan wordt het HLS-manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Het filter 'mobiel' Hiermee geeft u het verwijderen van vertoningen waar de oplossing is groter dan 720p, wat leidde tot de twee 1080p voorinstelling wordt overblijft.

![Weergavefiltering][renditions1]

## <a name="removing-language-tracks"></a>Taal verwijderen
Uw activa advies inwinnen bij meerdere audio talen zoals Engels, Spaans, Frans, enzovoort. Normaal gesproken standaardselectie audiotrack voor de Windows Media Player SDK-managers en beschikbaar audio worden bijgehouden per Gebruikersselectie. Het is lastig voor het ontwikkelen van dergelijke Player SDK's, hiervoor verschillende implementaties voor apparaat-specifieke player-frameworks. Ook Player-API's op sommige platformen moet kennis zijn beperkt en omvatten geen audio selectie functie waar gebruikers niet selecteren of het audiospoor standaardinstelling wijzigen. Met asset filters, kunt u het gedrag beheren met het maken van de filters die zijn alleen de gewenste audio talen.

![Taal filteren][language_filter]

## <a name="trimming-start-of-an-asset"></a>Begin van een asset bijsnijden
Operators Voer enkele tests uit voordat u de werkelijke gebeurtenis in de meeste live streaming-gebeurtenissen. Ze zijn bijvoorbeeld onder andere een slate als volgt vóór het begin van de gebeurtenis: 'Programma wordt overgenomen door'. Als het archiveren van het programma is zijn de test- en slates gegevens ook gearchiveerd en opgenomen in de presentatie. Deze informatie moet niet worden weergegeven op de clients. U kunt met dynamische Manifest maken van een filter voor start en de ongewenste gegevens verwijderen uit het manifest.

![Start bijsnijden][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Subclips (weergaven) te maken van een live-archief
Veel live gebeurtenissen zijn langere tijd worden uitgevoerd en livearchief meerdere gebeurtenissen kan opnemen. Nadat de live-gebeurtenis is geëindigd, willen tv-zenders splitst u het livearchief in logische programma starten en stoppen van reeksen. Vervolgens deze virtuele-programma's publiceren afzonderlijk zonder na verwerking van het livearchief en niet het maken van afzonderlijke activa (dit is het voordeel van het bestaande in de cache-fragmenten niet ophalen in het CDN's). Voorbeelden van dergelijke virtuele programma's zijn het kwartaal van een football of basketbalwedstrijd, innings in baseball of afzonderlijke gebeurtenissen van een sport-programma.

U kunt met dynamische Manifest filters met de begin-/ eindtijd maken en virtuele weergaven maken via de bovenkant van uw live-archief. 

![Subclip filter][subclip_filter]

Gefilterde Asset:

![Skiën][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Presentatievenster (DVR) aanpassen
Op dit moment biedt Azure Media Services circulaire archief waar de duur tussen 5 minuten: 25 uur kan worden geconfigureerd. Manifest filteren kan worden gebruikt om u te maken van een rolling DVR-venster op de bovenkant van het archief, zonder te verwijderen van media. Er zijn veel scenario's waarin tv-zenders wilt met een beperkte DVR-venster als u wilt gaan met de rand van het live en op hetzelfde moment een grotere archiveren venster bieden. Een tv-station mogelijk wilt u de gegevens die zich buiten de DVR-venster om te markeren clips gebruiken of ze wilt voor verschillende DVR-vensters voor verschillende apparaten. De meeste van de mobiele apparaten verwerken niet zo groot DVR windows (u kunt een 2 minuten DVR-venster voor mobiele apparaten en één uur voor bureaublad-clients hebben).

![DVR-venster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (live positie) aanpassen
Manifest filteren kan worden gebruikt om enkele seconden verwijderen uit de live rand van een live-programma. Filteren, kunt de presentatie op het punt van de publicatie preview bekijken en maken aankondiging voordat de viewers de stream (back-off van 30 seconden) ontvangt invoegen punten tv-zenders. Tv-zenders vervolgens push deze aankondigingen op de client-frameworks in tijd te ontvangen en verwerken van de gegevens voordat u de advertentie-mogelijkheid.

Naast de ondersteuning van de aankondiging, kan de instelling LiveBackoff worden gebruikt met het aanpassen van de positie viewers, zodat wanneer clients drift en druk op de rand van het live ze nog steeds fragmenten kunnen ophalen van de server in plaats van een HTTP 404 of 412-fout.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Het combineren van meerdere regels in één filter
U kunt meerdere regels voor filteren in één filter combineren. Zo kunt u een regel' bereik' slates verwijderen uit een live-archief en ook filteren om beschikbare bitsnelheden. Bij het toepassen van meerdere regels voor filteren, is het eindresultaat het snijpunt van alle regels.

![meerdere regels][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters (filter samenstelling) combineren

U kunt ook meerdere filters in een URL voor eenmalige combineren. 

De volgende scenario wordt uitgelegd waarom u kunt filters combineren:

1. U moet voor het filteren van uw video kenmerken voor mobiele apparaten, zoals Android- of iPAD (om te kunnen beperken video kenmerken). Als u wilt verwijderen van de ongewenste kenmerken, maakt u een filter Account geschikt is voor de apparaatprofielen. Accountfilters kunnen worden gebruikt voor al uw activa onder de dezelfde media services-account zonder verdere koppeling. 
2. U wilt ook de begin- en -tijd van een asset trim. Om dit te doen, zou u een filter Asset maken en de begin-/ eindtijd. 
3. U wilt beide van deze filters combineren (zonder combinatie, moet u om toe te voegen kwaliteit filteren om het filter bijsnijden, waardoor filter gebruik lastiger zijn).

Als u wilt combineren filters, moet u de filternamen instellen op het manifest/afspeellijst-URL met door puntkomma's gescheiden. Stel, u hebt een filter met de naam *MyMobileDevice* die kenmerken filters en hebt u een ander met de naam *MyStartTime* om in te stellen een specifieke begintijd. U kunt ze combineren als volgt:

U kunt maximaal drie filters combineren. 

Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

- De waarden voor **forceEndTimestamp**, **presentationWindowDuration**, en **liveBackoffDuration** mag niet worden ingesteld voor een VoD-filter. Ze worden alleen gebruikt voor scenario's voor live-filter. 
- Dynamische manifest werkt in GOP grenzen (sleutel Frames) daarom bijsnijden heeft GOP nauwkeurigheid. 
- U kunt dezelfde filternaam voor Account- en Asset filters gebruiken. Asset-filters hebben een hogere prioriteit en accountfilters wordt overschreven.
- Als u een filter bijwerkt, kan het tot 2 minuten voor streaming-eindpunt om te vernieuwen van de regels duren. Als de inhoud is uitgevoerd met bepaalde filters (en caches in proxy's en CDN-cache), kan deze filters bijwerken resulteren in fouten player. Het verdient aanbeveling om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is is, kunt u overwegen een ander filter.
- Klanten moeten handmatig het manifest downloaden en parseren van de exacte startTimestamp en schaal.
    
    - Eigenschappen van de nummers in een actief [ophalen en bekijk het manifestbestand](#get-and-examine-manifest-files).
    - De formule voor het instellen van het filter asset timestamp-eigenschappen: <br/>startTimestamp = &lt;begintijd in het manifest&gt; +  &lt;verwacht filter starttijd in seconden&gt;* tijdschaal


## <a name="next-steps"></a>Volgende stappen

De volgende artikelen laten zien hoe om filters te maken via een programma.  

- [Filters maken met REST-API 's](filters-dynamic-manifest-rest-howto.md)
- [Filters maken met .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Filters maken met CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png

---
title: Filters en dynamische manifesten | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten als u wilt archiveren deze selectief streaming.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/06/2018
ms.author: cenkd;juliako
ms.openlocfilehash: 6060f294820281df3124fb2fc702ece59a006af1
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282404"
---
# <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten
Media Services kunt vanaf 2,17 release, u filters voor uw assets definiëren. Deze filters zijn serverzijde regels die zorgen uw klanten om te kiezen dat ervoor voor handelingen zoals: alleen een gedeelte van een video (in plaats van de hele video afspelen) af te spelen, of alleen een subset van audio en video voorinstelling van uw klant apparaat aankan (opgeven in plaats van alle voorinstelling die zijn gekoppeld aan de asset). Deze filtering van uw activa wordt bereikt door **dynamische Manifest**s die zijn gemaakt op verzoek om te streamen van een video van uw klant op basis van opgegeven filter (s).

In dit onderwerp worden algemene scenario's waarin zeer nuttig zijn voor uw klanten en koppelingen naar onderwerpen die laten zien hoe u filters maken via een programma met behulp van filters zou zijn.

## <a name="overview"></a>Overzicht
Wanneer uw inhoud levert aan klanten (streaming live gebeurtenissen of video on demand) is het doel een video van hoge kwaliteit te leveren op verschillende apparaten met verschillende netwerkomstandigheden. Voor dit doel het volgende te doen:

* uw stream coderen naar multi-bitrate ([adaptieve bitrate](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) videostream (Dit zorgt voor kwaliteit en netwerkomstandigheden) en 
* Media Services gebruiken [dynamische verpakking](media-services-dynamic-packaging-overview.md) dynamisch opnieuw pakket opslaan van uw stream in verschillende protocollen (Dit zorgt voor streaming op verschillende apparaten). Media Services ondersteunt de levering van de volgende adaptive bitrate streaming-technologieën: HTTP Live Streaming (HLS), Smooth Streaming en MPEG DASH. 

### <a name="manifest-files"></a>Manifestbestanden
Wanneer u een asset voor adaptive bitratestreaming, codeert een **manifest** ()-afspeellijstbestand wordt gemaakt (het bestand is op basis van tekst of XML-indeling). De **manifest** bestand bevat metagegevens zoals streaming: bijhouden van type (audio, video of tekst), naam, begintijd en eindtijd, bitrate (kenmerken), bijhouden talen, presentatievenster (sliding window van vaste duur), video-codec (bijhouden Code). Hiermee geeft u ook de speler om op te halen van het volgende fragment door te geven informatie over het volgende kan worden afgespeeld video fragmenten beschikbaar en hun locatie. Fragmenten (of segmenten) zijn de werkelijke 'segmenten' van een video-inhoud.

Hier volgt een voorbeeld van een manifestbestand: 

    <?xml version="1.0" encoding="UTF-8"?>    
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">

    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />

    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>

    </SmoothStreamingMedia>

### <a name="dynamic-manifests"></a>Dynamische manifesten
Er zijn [scenario's](media-services-dynamic-manifest-overview.md#scenarios) wanneer de client moet meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-asset. Bijvoorbeeld:

* Apparaat in een bepaalde: leveren alleen de opgegeven voorinstelling en/of opgegeven taal nummers die worden ondersteund door het apparaat dat wordt gebruikt voor het afspelen van de inhoud ("weergavefiltering"). 
* Verminder het manifest om weer te geven van een subclips van live-gebeurtenis ("subclips filteren').
* Verwijder het begin van een video ("een video bijsnijden').
* Presentatie venster (DVR) aanpassen om te voorzien van een beperkt de lengte van de DVR-venster in de speler ("aanpassen presentatievenster").

Als u wilt bereiken deze flexibiliteit, Media Services biedt **dynamische manifesten** gebaseerd op vooraf gedefinieerde [filters](media-services-dynamic-manifest-overview.md#filters).  Nadat u de filters definiëren, kunnen uw clients deze gebruiken om een specifieke weergave of subquery korte clips van uw video te streamen. Ze zou filter (s) in de streaming-URL opgeven. Filters kunnen worden toegepast op adaptive bitrate streaming-protocollen die worden ondersteund door [dynamische verpakking](media-services-dynamic-packaging-overview.md): HLS, MPEG-DASH en Smooth Streaming. Bijvoorbeeld:

URL voor MPEG-DASH met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Smooth Streaming-URL met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Zie voor meer informatie over het leveren van inhoud en build streaming-URL's [overzicht inhoud leveren](media-services-deliver-content-overview.md).

> [!NOTE]
> Houd er rekening mee dat dynamische manifesten niet de asset en de standaard-manifest voor de activa veranderen. De client kunt kiezen om aan te vragen van een stream met of zonder filters. 
> 
> 

### <a id="filters"></a>Filters
Er zijn twee typen asset filters: 

* Algemene filters (kan worden toegepast op alle activa in het Azure Media Services-account, hebben een levensduur van het account) en 
* Lokale filters (kan alleen worden toegepast op een asset waarmee het filter gekoppeld nadat deze is gemaakt is, hebben een levensduur van de asset). 

Globale en lokale filtertypen hebben exact de dezelfde eigenschappen. Het belangrijkste verschil tussen de twee is voor welke scenario's voor welk type een filer beter geschikt is. Algemene filters zijn in het algemeen geschikt voor apparaatprofielen (weergavefiltering) waar lokale filters kunnen worden gebruikt voor een specifieke asset trim.

## <a id="scenarios"></a>Algemene scenario 's
Zoals is vermeld voordat wanneer uw inhoud levert aan klanten (streaming live gebeurtenissen of video on demand) het doel is een video van hoge kwaliteit te leveren op verschillende apparaten met verschillende netwerkomstandigheden. Daarnaast kunnen uw mogelijk andere vereisten die betrekking hebben op uw assets te filteren en het gebruik van **dynamische Manifest**s. De volgende secties geven een kort overzicht van verschillende scenario's voor filteren.

* Geef alleen een subset van audio en video voorinstelling die bepaalde apparaten kunnen verwerken (in plaats van alle voorinstelling die gekoppeld aan de asset zijn). 
* Alleen een gedeelte van een video (in plaats van de hele video afspelen) afspelen.
* Pas de presentatie-DVR-venster.

## <a name="rendition-filtering"></a>Weergavefiltering
U kunt uw asset voor meerdere codering profielen (H.264 basislijn H.264 hoog, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheden coderen. Niet alle clientapparaten ondersteunen echter de profielen en bitrates van uw asset. Oudere Android-apparaten ondersteunen bijvoorbeeld alleen H.264 basislijn + AACL. Hogere bitsnelheden verzendt naar een apparaat dat niet kan profiteren, verspilt netwerkbandbreedte en apparaat berekening. Dit apparaat moet worden gedecodeerd alle opgegeven informatie, alleen voor omlaag schalen om weer te geven.

Met dynamische Manifest u apparaatprofielen maken, zoals mobiele apparaten, console, HD/SD, enz., en omvatten de sporen te wissen en de kenmerken die u wilt worden van een onderdeel van elk profiel.

![Weergave filteren voorbeeld][renditions2]

In het volgende voorbeeld is een coderingsprogramma gebruikt een tussentijds asset coderen in zeven ISO MP4s video voorinstelling (van 180p 1080p). De gecodeerde asset dynamisch kan worden verpakt in een van de volgende protocollen voor streaming: HLS, Smooth en MPEG DASH.  Aan de bovenkant van het diagram, het HLS-manifest voor de asset met geen filters wordt weergegeven (bevat alle zeven voorinstelling).  In de linksonder, wordt het HLS-manifest waarop een filter met de naam "ott" is toegepast weergegeven. Het filter "ott" Hiermee geeft u als u wilt verwijderen van alle bitsnelheden hieronder 1 Mbps, wat leidde tot de twee quality-niveaus van onder, worden verwijderd uit in het antwoord. In de rechts onderaan wordt het HLS-manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Het filter 'mobiel' Hiermee geeft u het verwijderen van vertoningen waar de oplossing is groter dan 720p, wat leidde tot de twee 1080p voorinstelling wordt overblijft.

![Weergavefiltering][renditions1]

## <a name="removing-language-tracks"></a>Taal verwijderen
Uw activa advies inwinnen bij meerdere audio talen zoals Engels, Spaans, Frans, enzovoort. Normaal gesproken standaardselectie audiotrack voor de Windows Media Player SDK-managers en beschikbaar audio worden bijgehouden per Gebruikersselectie. Het is lastig voor het ontwikkelen van dergelijke Player SDK's, hiervoor verschillende implementaties voor apparaat-specifieke player-frameworks. Ook Player-API's op sommige platformen moet kennis zijn beperkt en omvatten geen audio selectie functie waar gebruikers niet selecteren of het audiospoor standaardinstelling wijzigen. Met asset filters, kunt u het gedrag beheren met het maken van de filters die zijn alleen de gewenste audio talen.

![Taal filteren][language_filter]

## <a name="trimming-start-of-an-asset"></a>Begin van een asset bijsnijden
Operators Voer enkele tests uit voordat u de werkelijke gebeurtenis in de meeste live streaming-gebeurtenissen. Bijvoorbeeld, ze een slate als volgt vóór het begin van de gebeurtenis kunnen bevatten: 'Programma wordt overgenomen door'. Als het archiveren van het programma is zijn de test- en slates gegevens ook gearchiveerd en opgenomen in de presentatie. Deze informatie moet niet worden weergegeven op de clients. U kunt met dynamische Manifest maken van een filter voor start en de ongewenste gegevens verwijderen uit het manifest.

![Start bijsnijden][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Subclips (weergaven) te maken van een live-archief
Veel live gebeurtenissen zijn langere tijd worden uitgevoerd en livearchief meerdere gebeurtenissen kan opnemen. Nadat de live-gebeurtenis is geëindigd, willen tv-zenders splitst u het livearchief in logische programma starten en stoppen van reeksen. Vervolgens deze virtuele-programma's publiceren afzonderlijk zonder na verwerking van het livearchief en niet het maken van afzonderlijke activa (dit is het voordeel van het bestaande in de cache-fragmenten niet ophalen in het CDN's). Voorbeelden van dergelijke virtuele programma's zijn het kwartaal van een football of basketbalwedstrijd, innings in baseball of afzonderlijke gebeurtenissen van een sport-programma.

U kunt met dynamische Manifest filters met de begin-/ eindtijd maken en virtuele weergaven maken via de bovenkant van uw live-archief. 

![Subclip filter][subclip_filter]

Gefilterde Asset:

![Skiën][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Presentatievenster (DVR) aanpassen
Op dit moment biedt Azure Media Services circulaire archief waar de duur tussen 5 minuten: 25 uur kan worden geconfigureerd. Manifest filteren kan worden gebruikt om u te maken van een rolling DVR-venster op de bovenkant van het archief, zonder te verwijderen van media. Er zijn veel scenario's waarin tv-zenders wilt met een beperkte DVR-venster als u wilt gaan met de rand van het live en op hetzelfde moment een grotere archiveren venster bieden. Een tv-station wilt gebruiken de gegevens die zich buiten de DVR-venster om te markeren clips of he\she wilt voor verschillende DVR-vensters voor verschillende apparaten. De meeste van de mobiele apparaten verwerken niet zo groot DVR windows (u kunt een 2 minuten DVR-venster voor mobiele apparaten en één uur voor bureaublad-clients hebben).

![DVR-venster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (live positie) aanpassen
Manifest filteren kan worden gebruikt om enkele seconden verwijderen uit de live rand van een live-programma. Filteren, kunt de presentatie op het punt van de publicatie preview bekijken en maken aankondiging voordat de viewers de stream (back-off van 30 seconden) ontvangt invoegen punten tv-zenders. Tv-zenders vervolgens push deze aankondigingen op de client-frameworks in tijd te ontvangen en verwerken van de gegevens voordat u de advertentie-mogelijkheid.

Naast de ondersteuning van de aankondiging, kan de instelling LiveBackoff worden gebruikt met het aanpassen van de positie viewers, zodat wanneer clients drift en druk op de rand van het live ze nog steeds fragmenten kunnen ophalen van de server in plaats van een HTTP 404 of 412-fout.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Het combineren van meerdere regels in één filter
U kunt meerdere regels voor filteren in één filter combineren. Zo kunt u een regel' bereik' slates verwijderen uit een live-archief en ook filteren om beschikbare bitsnelheden. Bij het toepassen van meerdere regels voor filteren, is het eindresultaat het snijpunt van alle regels.

![meerdere regels][multiple-rules]

## <a name="create-filters-programmatically"></a>Filters maken via een programma
Het volgende artikel wordt beschreven Media Services-entiteiten die zijn gerelateerd aan filters. Ook wordt uitgelegd hoe u programmatisch om filters te maken.  

[Filters maken met REST-API's](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters (filter samenstelling) combineren
U kunt ook meerdere filters in een URL voor eenmalige combineren. 

De volgende scenario wordt uitgelegd waarom u kunt filters combineren:

1. U moet voor het filteren van uw video kenmerken voor mobiele apparaten, zoals Android- of iPAD (om te kunnen beperken video kenmerken). Als u wilt verwijderen van de ongewenste kenmerken, maakt u een algemeen filter geschikt is voor de apparaatprofielen. Zoals eerder in dit artikel is vermeld, kunnen globale filters worden gebruikt voor al uw activa onder de dezelfde media services-account zonder verdere koppeling. 
2. U wilt ook de begin- en -tijd van een asset trim. Om dit te doen, zou u een lokale filter maken en de begin-/ eindtijd. 
3. U wilt beide van deze filters combineren (zonder combinatie, moet u om toe te voegen kwaliteit filteren om het filter trimming waardoor filter gebruik lastiger zijn).

Als u wilt combineren filters, moet u de filternamen instellen op het manifest/afspeellijst-URL met door puntkomma's gescheiden. Stel, u hebt een filter met de naam *MyMobileDevice* die kenmerken filters en hebt u een ander met de naam *MyStartTime* om in te stellen een specifieke begintijd. U kunt ze combineren als volgt:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

U kunt maximaal drie filters combineren. 

Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a name="know-issues-and-limitations"></a>Kent problemen en beperkingen
* Dynamische manifest werkt in GOP grenzen (sleutel Frames) daarom bijsnijden heeft GOP nauwkeurigheid. 
* U kunt dezelfde filternaam voor lokale en globale filters gebruiken. Lokale filters hebben een hogere prioriteit en algemene filters wordt overschreven.
* Als u een filter bijwerkt, kan het tot 2 minuten voor streaming-eindpunt om te vernieuwen van de regels duren. Als de inhoud is uitgevoerd met bepaalde filters (en caches in proxy's en CDN-cache), kan deze filters bijwerken resulteren in fouten player. Het verdient aanbeveling om de cache leeg na het bijwerken van het filter. Als deze optie niet mogelijk is is, kunt u overwegen een ander filter.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Inhoud leveren aan klanten-overzicht](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png

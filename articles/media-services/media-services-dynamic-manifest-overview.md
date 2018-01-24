---
title: Filters en dynamische manifesten | Microsoft Docs
description: Dit onderwerp beschrijft het maken van filters zodat de client naar specifieke secties van de stroom van een stroom gebruiken kan. Media Services maakt dynamische manifesten als u wilt archiveren deze selectief streaming.
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/22/2018
ms.author: cenkd;juliako
ms.openlocfilehash: d3c7cfad5ce9b25c88aa11b53194b6e06b1cc034
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten
Media Services kunt vanaf 2,17 versie, u filters definiëren voor de activa. Deze filters zijn server side regels die zorgen uw klanten dat ervoor te kiezen voor handelingen zoals: afspelen alleen een gedeelte van een video (in plaats van de hele video afspelen), of geef alleen een subset van audio en video vertoningen dat uw klant apparaat (in plaats daarvan kan verwerken met de vertoningen die zijn gekoppeld aan de asset). Via dit filteren van uw assets wordt gearchiveerd **dynamische Manifest**s die zijn gemaakt op verzoek om te streamen van een video van uw klant op basis van opgegeven filter.

In dit onderwerp wordt besproken algemene scenario's waarin zeer nuttig zijn om uw klanten en koppelingen naar onderwerpen die laten zien hoe u filters maken via een programma met filters zou zijn.

## <a name="overview"></a>Overzicht
Wanneer u uw inhoud levert aan klanten (streaming live gebeurtenissen of video-on-demand) is het doel een video van hoge kwaliteit te leveren aan verschillende apparaten met verschillende netwerkomstandigheden. Als u dit doel het volgende te doen:

* uw stream coderen naar multi-bitrate ([adaptive bitrate](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) videostream (Dit zorgt voor kwaliteit en netwerkomstandigheden) en 
* Media Services gebruiken [dynamische pakketten](media-services-dynamic-packaging-overview.md) dynamisch opnieuw pakket opslaan van uw stream in verschillende protocollen (Dit zorgt voor streaming op verschillende apparaten). Media Services ondersteunt de levering van de volgende adaptive bitrate streaming-technologieën: HTTP Live Streaming (HLS), Smooth Streaming- en MPEG DASH. 

### <a name="manifest-files"></a>Manifestbestanden
Wanneer u een asset voor adaptive bitrate streaming coderen een **manifest** ()-afspeellijstbestand wordt gemaakt (het bestand is op basis van tekst of XML-indeling). De **manifest** bestand bevat metagegevens zoals streaming: type (audio-, video- of tekst) bijhouden, naam, begintijd en eindtijd, bitrate (kenmerken), bijhouden talen, presentatievenster (sliding window van vaste duur), video-codec (bijhouden Code). Ook worden via de speler voor het ophalen van het volgende fragment op basis van informatie over de volgende afgespeeld video fragmenten beschikbaar en de locatie. Fragmenten (of segmenten) zijn de werkelijke 'reeksen' van een video-inhoud.

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
Er zijn [scenario's](media-services-dynamic-manifest-overview.md#scenarios) wanneer de client moet meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard asset. Bijvoorbeeld:

* Apparaat in een bepaalde: leveren alleen de opgegeven vertoningen en/of opgegeven taal nummers die worden ondersteund door het apparaat dat is gebruikt voor het afspelen van de inhoud ('weergave filteren'). 
* Verminder het manifest om weer te geven van een onderliggende clip van een live gebeurtenis ('onderliggende clip filteren').
* Trim het begin van een video ('een video bijsnijden').
* Presentatie venster (DVR) aanpassen om te voorzien van een beperkt de lengte van het venster DVR in Windows media player ('aanpassen presentatievenster').

Zodat deze flexibiliteit Media Services biedt **dynamische manifesten** gebaseerd op vooraf gedefinieerde [filters](media-services-dynamic-manifest-overview.md#filters).  Nadat u de filters definiëren, kunnen deze een specifieke weergave of subquery illustraties van uw video te streamen door uw clients gebruiken. Deze zou filters in de streaming-URL opgeven. Filters kunnen worden toegepast op adaptive bitrate streaming-protocollen die worden ondersteund door [dynamische pakketten](media-services-dynamic-packaging-overview.md): HLS, MPEG DASH en Smooth Streaming. Bijvoorbeeld:

URL voor MPEG-DASH met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Smooth Streaming-URL met filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Zie voor meer informatie over het leveren van inhoud en streaming-URL's samenstellen [leveren van inhoud overzicht](media-services-deliver-content-overview.md).

> [!NOTE]
> Houd er rekening mee dat dynamische manifesten niet de activa en het standaardmanifest voor de activa veranderen. De client kunt kiezen om aan te vragen van een stream met of zonder filters. 
> 
> 

### <a id="filters"></a>Filters
Er zijn twee typen asset filters: 

* Globale filters (kunnen worden toegepast op een willekeurige asset in het Azure Media Services-account, hebben een levensduur van het account) en 
* Lokale filters (kan alleen worden toegepast op een actief waarmee het filter gekoppeld nadat deze is gemaakt is, hebben een levensduur van de asset). 

Globale en lokale filtertypen hebben dezelfde eigenschappen. Het belangrijkste verschil tussen de twee is voor welke scenario's voor welk type een filer beter geschikt is. Globale filters in het algemeen geschikt zijn voor de apparaatprofielen (weergave filteren) waarin lokale filters kunnen worden gebruikt voor een bepaald activum trim.

## <a id="scenarios"></a>Algemene scenario 's
Zoals is vermeld voordat u uw inhoud leveren aan klanten (streaming live gebeurtenissen of video-on-demand) het doel is een video van hoge kwaliteit te leveren aan verschillende apparaten met verschillende netwerkomstandigheden. Daarnaast biedt de mogelijk andere vereisten die betrekking hebben op uw assets te filteren en het gebruik van hebben **dynamische Manifest**s. De volgende secties geven een kort overzicht van scenario's voor verschillende filteren.

* Geef alleen een subset van audio en video vertoningen dat bepaalde apparaten kunnen verwerken (in plaats van alle vertoningen die gekoppeld aan de asset zijn). 
* Alleen een gedeelte van een video (in plaats van de hele video afspelen) afspelen.
* DVR presentatievenster aanpassen.

## <a name="rendition-filtering"></a>Weergave filteren
U kunt uw asset voor meerdere codering profielen (H.264 basislijn H.264 hoog, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteit bitsnelheden coderen. Niet alle clientapparaten ondersteunen echter van uw asset profielen en bitsnelheden. Oudere Android-apparaten ondersteunt bijvoorbeeld alleen H.264 basislijn + AACL. Hogere bitsnelheden verzendt naar een apparaat dat de voordelen kan niet ophalen, verspild bandbreedte en apparaat berekening. Dergelijke apparaten moet alle bepaalde informatie, alleen voor omlaag schalen om weer te geven decoderen.

Met dynamische Manifest, kunt u profielen van apparaten zoals mobile, console, HD/SD, enz., maar ook de houdt en kenmerken die u wilt worden opgenomen als onderdeel van elk profiel.

![Voorbeeld van de weergave filteren][renditions2]

In het volgende voorbeeld is een coderingsprogramma gebruikt voor het coderen van een activum tussentijds in zeven ISO MP4s video vertoningen (van 180p 1080p). De gecodeerde asset dynamisch kan worden verpakt in een van de volgende protocollen voor streaming: HLS, Smooth en MPEG DASH.  Aan de bovenkant van het diagram het manifest HLS voor de activa zonder filters wordt weergegeven (bevat alle zeven vertoningen).  In de onderste links, wordt het HLS manifest waarop een filter met de naam 'ott' is toegepast weergegeven. Het filter 'ott' geeft aan dat alle bitsnelheden lager dan 1 Mbps, wat leidde tot de twee quality-niveaus van onder, worden verwijderd uit in het antwoord verwijderen.  In het rechterdeelvenster onder wordt het HLS manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Het filter 'mobiel' geeft aan waar de oplossing is groter dan 720p, wat tot de twee leidde vertoningen verwijderen 1080p vertoningen worden verwijderd uit.

![Weergave filteren][renditions1]

## <a name="removing-language-tracks"></a>Taal verwijderen
Uw assets kunnen meerdere audio talen zoals Engels, Spaans, Frans, enzovoort bevatten. Normaal gesproken standaardselectie-nummer van de Windows Media Player SDK-managers en audio beschikbaar houdt per Gebruikersselectie. Is het lastig voor het ontwikkelen van dergelijke Player SDK's, verschillende implementaties over apparaatspecifieke-frameworks voor spelers is vereist. Ook op sommige platformen Neem Player-API's zijn beperkt en geen audio selectie functie waar gebruikers niet selecteren of wijzigen van de standaard-nummer. U kunt het gedrag bepalen door het maken van de filters die alleen de gewenste audio talen opnemen met asset filters.

![Taal houdt filteren][language_filter]

## <a name="trimming-start-of-an-asset"></a>Hoe worden ingekort begin van een asset
Sommige tests voor de werkelijke gebeurtenis is in de meeste live streaming-gebeurtenissen, operators uitvoeren. Ze bevatten bijvoorbeeld een lei als volgt vóór het begin van de gebeurtenis: "Programma begint tijdelijk worden". Als het archiveren van het programma is zijn de test en lei gegevens ook gearchiveerd en opgenomen in de presentatie. Deze informatie moet niet worden weergegeven op de clients. U kunt met dynamische Manifest maken van een tijdfilter start en de ongewenste gegevens verwijderen uit het manifest.

![Hoe worden ingekort starten][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Maken van subclips (weergaven) van een live-archief
Veel live gebeurtenissen zijn uitgevoerd en live archief meerdere gebeurtenissen kan bevatten. Na afloop van de live gebeurtenis omroeporganisaties mogelijk wilt onderverdelen in het archief live in logische programma starten en stoppen van reeksen. Vervolgens deze virtuele-programma's publiceren afzonderlijk zonder na verwerking van het live archief en afzonderlijke elementen niet maken (dit heeft het voordeel van de bestaande in de cache fragmenten niet ophalen in de CDN). Voorbeelden van dergelijke virtuele programma's zijn het kwartaal van een football of basketbalwedstrijd, innings in baseball of afzonderlijke gebeurtenissen van een sport-programma.

U kunt met dynamische Manifest keren beginnen of eindigen met filters maken en virtuele weergaven maken via de bovenkant van uw live-archief. 

![Filter subclip][subclip_filter]

Gefilterde Asset:

![Skiën][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Presentatievenster (DVR) aan te passen
Azure Media Services biedt momenteel cirkelvormige archief waarin de duur tussen 5 minuten: 25 uur kan worden geconfigureerd. Manifest filteren kan geen rolling DVR-venster maken via de bovenkant van het archief zonder te verwijderen van de media worden gebruikt. Er zijn veel scenario's waar omroeporganisaties wilt bieden een beperkte DVR-venster als u wilt gaan met de live rand en tegelijkertijd een grotere archiveren venster. Een tv-station mogelijk wilt u gebruiken de gegevens die buiten het venster DVR illustraties markeren of he\she wilt bieden verschillende DVR vensters voor verschillende apparaten. De meeste van de mobiele apparaten niet bijvoorbeeld grote DVR windows (u kunt een venster van de DVR 2 minuten voor mobiele apparaten en één uur voor bureaublad-clients hebt) worden verwerkt.

![DVR venster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Het aanpassen van LiveBackoff (live positie)
Manifest filteren kan enkele seconden verwijderen uit de live rand van een live programma worden gebruikt. Filteren, kunt omroeporganisaties bekijken van de presentatie op het punt van de publicatie preview en aankondiging maken invoegen punten voordat de afbeeldingsviewers de stroom ontvangen (back-uitgeschakeld met 30 seconden). Omroeporganisaties kunnen push deze advertenties naar hun frameworks client in de tijd te ontvangen en verwerken van gegevens voordat de gelegenheid aankondiging.

Naast de advertentie-ondersteuning kan de instelling LiveBackoff worden gebruikt om de positie viewers zodanig aangepast dat wanneer clients nemen en klik op de rand live nog steeds krijgen fragmenten van server in plaats van het ophalen van een HTTP 404 of 412 fout.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Combineren van meerdere regels in een enkel filter
U kunt meerdere regels voor filteren in één filter combineren. Als een voorbeeld kunt u een regel' bereik' slates verwijderen uit een live-archief en beschikbare bitsnelheden ook filteren. Wanneer u meerdere filterregels toepast, is het eindresultaat het snijpunt van alle regels.

![meerdere regels][multiple-rules]

## <a name="create-filters-programmatically"></a>Programmatisch filters maken
Het volgende artikel behandelt Media Services-entiteiten die zijn gerelateerd aan de filters. Ook wordt uitgelegd hoe u programmatisch om filters te maken.  

[Filters maken met de REST-API's](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Combineren van meerdere filters (filter samenstelling)
U kunt ook meerdere filters in één URL van een combineren. 

De volgende scenario wordt uitgelegd waarom kunt u filters combineren:

1. U moet uw video kenmerken voor mobiele apparaten, zoals Android of iPAD filteren (om te beperken video kenmerken). Als u wilt verwijderen van de ongewenste eigenschappen, maakt u een globaal filter geschikt is voor de apparaatprofielen. Zoals eerder in dit artikel wordt vermeld, kunnen de globale filters worden gebruikt voor alle activa onder dezelfde media services-account zonder verdere koppeling. 
2. U wilt ook trim de begin- en -tijd van een asset. Om dit te bereiken, zou u een lokale filter maken en de begin-/ eindtijd niet instellen. 
3. U wilt combineren van deze filters (zonder combinatie, moet u kwaliteit filteren om het filter Bijsnijden dit gebruik van filter moeilijker maakt toevoegen).

Als u wilt combineren filters, moet u de filternamen ingesteld op de manifest/afspeellijst-URL met door puntkomma's gescheiden. Stel u hebt een filter met de naam *MyMobileDevice* die eigenschappen filters en hebt u een andere naam *MyStartTime* om in te stellen een bepaalde tijd starten. U kunt ze combineren als volgt:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

U kunt maximaal drie filters combineren. 

Zie voor meer informatie [dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a name="know-issues-and-limitations"></a>Kent problemen en beperkingen
* Dynamische manifest draait in GOP grenzen (sleutel Frames) daarom bijsnijden GOP nauwkeurig is. 
* U kunt dezelfde filternaam voor lokale en globale filters gebruiken. Houd er rekening mee dat lokale filter hebben een hogere prioriteit en globale filters overschrijven.
* Als u een filter bijwerkt, kan het maximaal 2 minuten duren voordat een streaming-eindpunt om te vernieuwen van de regels. Als de inhoud is uitgevoerd met behulp van een aantal filters (en in het cachegeheugen van proxy's en CDN caches), kan het bijwerken van deze filters leiden tot player-fouten. Het is raadzaam om de cache wissen na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Het leveren van inhoud naar klanten-overzicht](media-services-deliver-content-overview.md)

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

---
title: Het leveren van inhoud aan klanten | Microsoft Docs
description: In dit onderwerp geeft een overzicht van wat is betrokken bij het leveren van uw inhoud met Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: juliako
ms.openlocfilehash: 815aae57af93b0e4870bd9f61da248e4be328db4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deliver-content-to-customers"></a>Inhoud leveren aan klanten
Wanneer u uw inhoud streamen of video-on-demand aan klanten bieden, wordt het doel is om te leveren van video van hoge kwaliteit aan verschillende apparaten met verschillende netwerkomstandigheden.

Voor dit doel te bereiken, kunt u het volgende doen:

* Codeer uw stream naar een multi-bitrate (adaptieve bitrate) videostream. Dit zorgt voor kwaliteit en netwerkomstandigheden.
* Gebruik Microsoft Azure Media Services [dynamische pakketten](media-services-dynamic-packaging-overview.md) dynamisch de uw stream in verschillende protocollen opnieuw inpakken. Dit zorgt voor streaming op verschillende apparaten. Media Services ondersteunt de levering van de volgende adaptive bitrate streaming-technologieën: <br/>
    * **HTTP Live Streaming** (HLS) - toevoegen ' (format = m3u8-aapl) ' pad naar het gedeelte '/ Manifest' van de URL van de streaming oorsprong-server om te retourneren back HLS-inhoud voor verbruik op laten weten **Apple iOS** systeemeigen apparaten (voor meer informatie. Zie [locators](#locators) en [URL's](#URLs)),
    * **MPEG-DASH** -toevoegen ' (format = mpd-time-csf) ' pad naar het gedeelte '/ Manifest' van de URL die u wilt zien van de bronserver streaming om terug te keren back MPEG-DASH (Zie voor meer informatie [locators](#locators) en [URL's](#URLs) ),
    * **Smooth Streaming**.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

In dit artikel biedt een overzicht van belangrijke contentlevering concepten.

Bekende problemen Zie [bekende problemen](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamische verpakking
De dynamische pakketten die Media Services biedt, dat kunt u uw adaptive bitrate MP4- of Smooth Streaming gecodeerde inhoud in de streaming-indelingen die worden ondersteund door Media Services (MPEG DASH, HLS, Smooth Streaming) leveren zonder opnieuw verpakken in een van deze streaming-indelingen. We raden aan met het leveren van uw inhoud met dynamische pakketten.

Om te profiteren van dynamische pakketten, moet u Codeer uw tussentijds (bron) bestand in een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden.

Dynamische pakketten, opslaan en betaalt voor één opslagindeling de bestanden. Media Services bouwt en levert de juiste reactie op basis van uw aanvragen.

Dynamische pakketten is beschikbaar voor standard en premium streaming-eindpunten. 

Zie voor meer informatie [dynamische pakketten](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten
U kunt filters definiëren voor de activa met Media Services. Deze filters zijn serverzijde regels waarmee uw klanten acties als het afspelen van een specifieke sectie van een video- of een subset van audio en video vertoningen dat uw klant apparaat kan verwerken (in plaats van alle vertoningen die gekoppeld aan de asset zijn) opgeven. Dit filteren wordt bereikt door *dynamische manifesten* die worden gemaakt wanneer de klant aanvragen om te streamen video op basis van een of meer filters hebt opgegeven.

Zie voor meer informatie [Filters en dynamische manifesten](media-services-dynamic-manifest-overview.md).

## <a name="a-idlocatorslocators"></a><a id="locators"/>Locators
Om uw gebruikers met een URL die kan worden gebruikt om te streamen of uw inhoud downloaden, moet u eerst uw asset publiceren door een locator te maken. Een locator biedt een invoerpunt voor toegang tot de bestanden in een asset. Media Services ondersteunt twee typen locators:

* OnDemandOrigin-locators. Deze worden gebruikt voor het streamen van media (bijvoorbeeld MPEG DASH, HLS of Smooth Streaming) of bestanden progressief te downloaden.
* Locators voor Shared access signature (SAS)-URL. Deze worden gebruikt voor het downloaden van media-bestanden op uw lokale computer.

Een *toegangsbeleid* wordt gebruikt voor het definiëren van de machtigingen op (zoals lezen, schrijven en lijst) en duur waarvoor een client toegang voor een bepaalde asset heeft. Houd er rekening mee dat de machtiging lijst (AccessPermissions.List) niet moet worden gebruikt bij het maken van een OrDemandOrigin-locator.

Locators hebben een verloopdatum. De Azure portal een vervaldatum van 100 jaar in de toekomst voor locators ingesteld.

> [!NOTE]
> Als u de Azure-portal gebruikt om locators te maken vóór maart 2015, zijn deze locators verloopt na twee jaar ingesteld.
> 
> 

Als u de vervaldatum van een locator wilt bijwerken, gebruikt u [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator)- of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259)-API's. Wanneer u de vervaldatum van een SAS-locator bijwerkt, wordt de URL gewijzigd.

Locators zijn niet ontworpen voor het beheren van toegangsbeheer per gebruiker. U kunt verschillende toegangsrechten naar afzonderlijke gebruikers geven met behulp van oplossingen voor beheer van digitale rechten (DRM). Zie voor meer informatie [Media beveiligen](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Wanneer u een locator maakt, is er mogelijk een vertraging van 30 seconden vanwege opslag vereist en de doorgifte van processen in Azure Storage.

## <a name="adaptive-streaming"></a>Adaptief streamen
Adaptive bitrate technologieën kunnen-speler toepassingen netwerkomstandigheden bepalen en selecteert u in meerdere bitsnelheden. Wanneer de netwerkcommunicatie vermindert, kan de client een lagere bitrate selecteren zodat afspelen kunt doorgaan met de lagere videokwaliteit. Als netwerkomstandigheden verbeteren, wordt de client kunt overschakelen naar een hogere bitsnelheid met verbeterde videokwaliteit. Azure Media Services ondersteunt de volgende adaptive bitrate-technologieën: HTTP Live Streaming (HLS), Smooth Streaming- en MPEG-DASH.

U kunt gebruikers met het streaming-URL's, moet u eerst een OnDemandOrigin-locator maken. De locator hebt waarvan u het basispad voor de activa die de inhoud die u wilt streamen bevat. Als u deze inhoud streamen, moet u echter meer dit pad wijzigen. Kan een volledige URL van het manifestbestand van streaming, moet u de locator padwaarde en het manifest (filename.ism) samenvoegen bestandsnaam. Voeg **/Manifest** en de juiste indeling (indien nodig) naar het pad locator.

> [!NOTE]
> U kunt ook de inhoud streamen via een SSL-verbinding. Hiervoor moet dat de URL van uw streaming begint met HTTPS. Houd er rekening mee dat op dit moment AMS biedt geen ondersteuning voor SSL met aangepaste domeinen.  
> 

U kunt alleen streamen via SSL als het streaming-eindpunt van waaruit u uw inhoud wilt leveren na 10 September 2014 werd gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten gemaakt na 10 September 2014, bevat de URL 'streaming.mediaservices.windows.net'. Streaming-URL's die 'origin.mediaservices.windows.net' (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als de URL in de oude indeling en u wilt streamen via SSL, maakt u een nieuwe streaming-eindpunt. URL's op basis van het nieuwe streaming-eindpunt gebruiken de inhoud te streamen via SSL.

## <a name="a-idurlsstreaming-url-formats"></a><a id="URLs"/>Streaming-URL-indelingen

### <a name="mpeg-dash-format"></a>MPEG-DASH-indeling
{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-time-csf)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS)-V4-indeling
{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-indeling
{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS)-indeling met alleen audio-filter
Standaard alleen audio-nummers zijn opgenomen in de HLS manifest. Dit is vereist voor Apple Store certificeringsinstantie voor mobiele netwerken. In dit geval als een client beschikt niet over voldoende bandbreedte of is verbonden via een verbinding 2G, verandert afspelen in een alleen-audio. Hierdoor blijven inhoud streaming zonder buffer, maar er is geen video. In sommige scenario's mogelijk player buffer voorkeur worden via alleen audio. Als u de nummer alleen audio wilt verwijderen wilt, voegt u **alleen audio = false** naar de URL.

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,audio-Only=False)

Zie voor meer informatie [ondersteuning voor dynamische samenstelling Manifest- en HLS uitvoer aanvullende functies](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Smooth Streaming-indeling
{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

Voorbeeld:

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Smooth Streaming 2.0-manifest (verouderde manifest)
Standaard bevat manifest indeling Smooth Streaming de herhaaldelijk tag (r-code). Sommige spelers bieden echter geen ondersteuning voor de r-tag. Clients met deze spelers kunnen een indeling die de r-tag schakelt gebruiken:

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Progressief downloaden
Met progressief downloaden, kunt u beginnen met het afspelen van media voordat het hele bestand is gedownload. U downloaden niet ISM * (isma ismv ismt of ismc)-bestanden progressief.

Gebruik het type OnDemandOrigin locator om inhoud progressief te downloaden. Het volgende voorbeeld ziet u de URL die is gebaseerd op het type OnDemandOrigin locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

U moet de opslag versleuteld elementen die u wilt dat kan worden gestreamd vanaf oorsprong van de service voor progressief downloaden decoderen.

## <a name="download"></a>Downloaden
Uw om inhoud te downloaden naar een clientapparaat, moet u een SAS-Locator te maken. De SAS-locator hebt u toegang tot de Azure Storage-container waarin het bestand zich bevindt. De download-URL bouwen, hebt u de bestandsnaam tussen de host en de SAS-handtekening insluiten.

Het volgende voorbeeld ziet u de URL die is gebaseerd op de SAS-locator:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Het volgende letten:

* U moet de opslag versleuteld elementen die u wilt dat kan worden gestreamd vanaf oorsprong van de service voor progressief downloaden decoderen.
* Een download die nog niet voltooid binnen de 12 uur zal mislukken.

## <a name="streaming-endpoints"></a>Streaming-eindpunten

Een streaming-eindpunt vertegenwoordigt een streaming-service die inhoud rechtstreeks naar een clienttoepassing player of met een netwerk voor contentlevering (CDN) voor verdere distributie leveren kunt. De uitgaande stroom van een streaming-eindpunt-service is een live stream of een activum video-on-demand in uw Media Services-account. Er zijn twee soorten streaming-eindpunten, **standaard** en **premium**. Zie voor meer informatie [Streaming-eindpunten overzicht](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

## <a name="known-issues"></a>Bekende problemen
### <a name="changes-to-smooth-streaming-manifest-version"></a>Wijzigingen in Smooth Streaming manifest versie
Vóór de release van juli 2016 service--wanneer de activa die is geproduceerd met Media Encoder Standard Media Encoder Premium werkstroom of de eerdere Azure Media Encoder zijn gestreamd met behulp van dynamische pakketten--Smooth Streaming geretourneerd manifest zou voldoen aan versie 2.0. In versie 2.0 Gebruik de duur van het fragment niet de zogenaamde herhalen (r)-codes. Bijvoorbeeld:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

In de release van juli 2016-service voldoet het gegenereerde Smooth Streaming-manifest aan versie 2.2, met een fragment duur met herhalingen tags. Bijvoorbeeld:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Sommige van de verouderde clients van Smooth Streaming mogelijk niet ondersteund door de herhaaldelijk tags en niet worden geladen van het manifest. Om dit probleem beperken, kunt u de indelingsparameter verouderde manifest **(format = fmp4 v20)** of bijwerken van de client naar de nieuwste versie die ondersteuning biedt voor herhaaldelijk labels. Zie voor meer informatie [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Media Services-locators na rolling opslagsleutels bijwerken](media-services-roll-storage-access-keys.md)


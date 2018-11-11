---
title: Inhoud leveren aan klanten | Microsoft Docs
description: In dit onderwerp biedt een overzicht van wat is betrokken bij het leveren van uw inhoud met Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: juliako
ms.openlocfilehash: ae0ff36c7e83120a9571e0f87788c25193027616
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240134"
---
# <a name="deliver-content-to-customers"></a>Lever inhoud aan klanten
Wanneer u uw inhoud streamen of video on demand aan klanten leveren bent, wordt het doel is om te hoge kwaliteit video leveren aan verschillende apparaten met verschillende netwerkomstandigheden.

Als u wilt dit doel te bereiken, kunt u het volgende doen:

* Uw stream coderen naar een multi-bitrate (adaptieve bitrate) videostream. Dit zorgt voor kwaliteit en netwerkomstandigheden.
* Microsoft Azure Media Services gebruiken [dynamische verpakking](media-services-dynamic-packaging-overview.md) dynamisch de uw stream in verschillende protocollen opnieuw inpakken. Dit zorgt voor streaming op verschillende apparaten. Media Services ondersteunt de levering van de volgende adaptive bitrate streaming-technologieën: <br/>
    * **HTTP Live Streaming** (HLS) - toevoegen ' (format = m3u8-aapl) ' pad naar het gedeelte '/ Manifest' van de URL om te zien van de server voor streaming oorsprong om terug te keren back HLS-inhoud voor gebruik op **Apple iOS** systeemeigen apparaten (voor meer informatie. Zie [locators](#locators) en [URL's](#URLs)),
    * **MPEG-DASH** -toevoegen ' (format = mpd-time-csf) ' pad naar het gedeelte '/ Manifest' van de URL om te zien van de server voor streaming oorsprong om terug te keren MPEG-DASH back-ups maken (Zie voor meer informatie, [locators](#locators) en [URL's](#URLs)),
    * **Smooth Streaming**.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

In dit artikel biedt een overzicht van belangrijke contentlevering concepten.

Bekende problemen, Zie [bekende problemen](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamische verpakking
Met de dynamische pakketten die Media Services biedt, dat kunt u uw adaptive bitrate MP4- of Smooth Streaming gecodeerde inhoud in de streaming-indelingen die worden ondersteund door Media Services (MPEG-DASH, HLS, Smooth Streaming) leveren zonder te hoeven opnieuw verpakken in een van deze streaming-indelingen. Het is raadzaam om uw inhoud met dynamische verpakking leveren.

Als u wilt profiteren van dynamische pakketten, moet u Codeer uw tussentijds (bron) bestand in een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden.

Met dynamische verpakking, opslaan en betaalt voor de bestanden in één opslagindeling. Media Services bouwt en levert de juiste reactie op basis van uw aanvragen.

Dynamische pakketten is beschikbaar voor standard en premium streaming-eindpunten. 

Zie voor meer informatie, [dynamische verpakking](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filters en dynamische manifesten
U kunt filters definiëren voor uw activa met Media Services. Deze filters zijn serverzijde regels waarmee uw klanten die zaken als een bepaald deel van een video afspelen of een subset van audio en video voorinstelling die van uw klant apparaat kan verwerken (in plaats van alle voorinstelling die gekoppeld aan de asset zijn) opgeven. Dit filter wordt bereikt door *dynamische manifesten* die worden gemaakt wanneer de klant aanvraagt of het streamen van een video die op basis van een of meer filters hebt opgegeven.

Zie voor meer informatie, [Filters en dynamische manifesten](media-services-dynamic-manifest-overview.md).

## <a name="a-idlocatorslocators"></a><a id="locators"/>Locators
Als u wilt uw gebruiker voorzien van een URL die kan worden gebruikt om te streamen of te downloaden van uw inhoud, moet u eerst uw asset publiceren door een locator te maken. Een locator biedt een invoerpunt voor toegang tot de bestanden die deel uitmaken van een asset. Media Services ondersteunt twee typen locators:

* OnDemandOrigin-locators. Deze worden gebruikt voor het streamen van media (bijvoorbeeld MPEG-DASH, HLS of Smooth Streaming) of bestanden progressief te downloaden.
* Shared access signature (SAS)-URL locators. Deze worden gebruikt voor het downloaden van mediabestanden op uw lokale computer.

Een *toegangsbeleid* wordt gebruikt voor het definiëren van de machtigingen (zoals lezen, schrijven en lijst) en de duur waarvoor een client toegang voor een bepaalde asset heeft. Houd er rekening mee dat de machtiging van de lijst (AccessPermissions.List) niet moet worden gebruikt bij het maken van een locator OrDemandOrigin.

Locators hebben een vervaldatum. De Azure-portal een vervaldatum in de toekomst 100 jaar voor locators ingesteld.

> [!NOTE]
> Als u de Azure-portal gebruikt om locators te maken vóór maart 2015, worden de locators zijn ingesteld op verlopen na twee jaar.
> 
> 

Als u de vervaldatum van een locator wilt bijwerken, gebruikt u [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator)- of [.NET](https://go.microsoft.com/fwlink/?LinkID=533259)-API's. Wanneer u de vervaldatum van een SAS-locator bijwerkt, wordt de URL gewijzigd.

Locators zijn niet ontworpen om toegangsbeheer per gebruiker. U kunt afzonderlijke gebruikers verschillende toegangsrechten geven met behulp van Digital Rights Management (DRM)-oplossingen. Zie voor meer informatie, [Media beveiligen](https://msdn.microsoft.com/library/azure/dn282272.aspx).

Wanneer u een locator maakt, is er mogelijk een vertraging van 30 seconden vanwege een vereiste voor opslag en doorgifte processen in Azure Storage.

## <a name="adaptive-streaming"></a>Adaptieve streaming
Adaptieve bitrate-technologieën kunt videospelertoepassingen voor netwerkomstandigheden bepalen en selecteren van verschillende bitsnelheden. Wanneer de netwerkcommunicatie minder, kan de client een lagere bitsnelheid selecteren zodat afspelen kunt doorgaan met videokwaliteit. Als netwerkomstandigheden verbeteren, wordt de client kunt overschakelen naar een hogere bitrate met verbeterde videokwaliteit. Azure Media Services ondersteunt de volgende adaptive bitrate-technologieën: HTTP Live Streaming (HLS), Smooth Streaming en MPEG-DASH.

Voor gebruikers met het streaming-URL's, moet u eerst een OnDemandOrigin-locator maken. Maakt de locator, ziet u het basispad aan de asset die met de inhoud die u wilt streamen. Als u deze inhoud streamen, moet u echter meer dit pad wijzigen. Kan geen volledige URL naar het manifestbestand van streaming, samenvoegen van de locator padwaarde en het manifest (filename.ism) bestandsnaam. Voeg **/Manifest** en een juiste indeling (indien nodig) naar het pad locator.

> [!NOTE]
> U kunt ook uw inhoud streamen via een SSL-verbinding. Om dit te doen, zorg ervoor dat uw streaming-URL's beginnen met HTTPS. Houd er rekening mee dat op dit moment AMS biedt geen ondersteuning voor SSL met aangepaste domeinen.  
> 

U kunt alleen streamen via SSL als het streaming-eindpunt van waaruit u uw inhoud leveren na 10 September 2014 is gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten die zijn gemaakt na 10 September 2014, bevat de URL "streaming.mediaservices.windows.net." Streaming-URL's die "origin.mediaservices.windows.net" (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als de URL in de oude notatie is en u wilt streamen via SSL, maakt u een nieuwe streaming-eindpunt. URL's op basis van het nieuwe streaming-eindpunt gebruiken om uw inhoud streamen via SSL.

## <a name="a-idurlsstreaming-url-formats"></a><a id="URLs"/>Streaming-URL-indelingen

### <a name="mpeg-dash-format"></a>MPEG-DASH-indeling
{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS)-V4-indeling
{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-indeling
{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS)-indeling met alleen audio-filter
Standaard alleen audio-nummers zijn opgenomen in de HLS manifest. Dit is vereist voor de Apple Store-certificering voor mobiele netwerken. In dit geval, als een client beschikt niet over voldoende bandbreedte of is verbonden via een verbinding 2G, afspelen schakelt over naar alleen audio. Hierdoor blijven inhoudsstreaming zonder buffer, maar er is geen video. In sommige scenario's mogelijk player bufferfuncties voorkeur worden via alleen audio. Als u verwijderen van het spoor alleen audio wilt, voegt u toe **alleen audio = false** naar de URL.

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3alleen audio = false)

Zie voor meer informatie, [ondersteuning voor dynamische Manifest van de samenstelling en HLS uitvoer extra functies](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Smooth Streaming-indeling
{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

Voorbeeld:

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

### <a id="fmp4_v20"></a>Smooth Streaming 2.0-manifest (verouderde manifest)
Standaard bevat manifestindeling Smooth Streaming de herhalingen tag (-r-code). Sommige spelers bieden echter geen ondersteuning voor de r-code. Clients met deze spelers kunnen een indeling waarbij de r-code gebruiken:

{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Progressief downloaden
Met progressief downloaden, kunt u beginnen met het afspelen van media voordat het hele bestand is gedownload. U downloaden niet (ISM * ismv, isma, ismt of ismc)-bestanden progressief.

Voor het progressief downloaden van inhoud, gebruik het OnDemandOrigin type locator. Het volgende voorbeeld ziet u de URL die is gebaseerd op het OnDemandOrigin type locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

U moet alle opslag versleuteld activa die u wilt streamen van de oorsprong-service voor het progressief downloaden decoderen.

## <a name="download"></a>Downloaden
Als u wilt uw inhoud naar een client-apparaat worden gedownload, moet u een SAS-Locator maken. De SAS-locator geeft u toegang tot de Azure Storage-container waarin uw bestand zich bevindt. U hebt voor het bouwen van de download-URL, om in te sluiten van de bestandsnaam tussen de host en de SAS-handtekening.

Het volgende voorbeeld ziet u de URL die is gebaseerd op de SAS-locator:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

De volgende overwegingen zijn van toepassing:

* U moet alle opslag versleuteld activa die u wilt streamen van de oorsprong-service voor het progressief downloaden decoderen.
* Een download is niet voltooid binnen de 12 uur mislukken.

## <a name="streaming-endpoints"></a>Streaming-eindpunten

Een streaming-eindpunt vertegenwoordigt een streamingservice waarmee inhoud kan worden geleverd rechtstreeks naar een clientafspeeltoepassing of naar een netwerk voor contentlevering (CDN) voor verdere distributie. De uitgaande stroom van een streaming-eindpunt-service is een live stream of een activum video on demand in Media Services-account. Er zijn twee soorten streaming-eindpunten, **standard** en **premium**. Zie [Streaming endpoints overview](media-services-streaming-endpoints-overview.md) (Overzicht van streaming-eindpunten) voor meer informatie.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

## <a name="known-issues"></a>Bekende problemen
### <a name="changes-to-smooth-streaming-manifest-version"></a>Wijzigingen in de Smooth Streaming manifest versie
Vóór de release van juli 2016 service--wanneer activa die worden geproduceerd door Media Encoder Standard Media Encoder Premium Workflow, of de eerdere versie van Azure Media Encoder zijn gestreamd met behulp van dynamische pakketten--de Smooth Streaming manifest geretourneerd dan in overeenstemming zijn met versie 2.0. In versie 2.0 Gebruik de duur van het fragment niet de zogeheten herhalen (r)-codes. Bijvoorbeeld:

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

In de release van juli 2016-service voldoet het gegenereerde Smooth Streaming-manifest naar versie 2.2, met een fragment duur met herhalingen tags. Bijvoorbeeld:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Sommige van de verouderde Smooth Streaming-clients bieden mogelijk geen ondersteuning voor de herhalingen tags en niet worden geladen van het manifest. Risico's te beperken, kunt u de parameter verouderde manifestindeling **(format = fmp4-v20)** of bijwerken van de client naar de nieuwste versie, die ondersteuning biedt voor herhalingen tags. Zie voor meer informatie, [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwante onderwerpen
[Locators voor Media Services bijwerken na gebruik opslagsleutels](media-services-roll-storage-access-keys.md)


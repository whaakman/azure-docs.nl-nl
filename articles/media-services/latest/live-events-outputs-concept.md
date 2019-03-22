---
title: Live streamen concepten in Azure Media Services - Live-evenementen en Live uitvoer | Microsoft Docs
description: In dit artikel biedt een overzicht van live streaming-concepten in Azure Media Services v3.
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
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: c446a71a363a9a81eeb7d0dddcdbd90ccee08b7d
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189356"
---
# <a name="live-events-and-live-outputs"></a>Live gebeurtenissen en live uitvoer

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Voor het configureren van uw live streaming-gebeurtenissen in Media Services v3, moet u inzicht in concepten die in dit artikel worden besproken:

* [Live-evenementen](#live-events)
* Live gebeurtenistypen
* Vergelijking van de typen Live gebeurtenis
* [Opties voor het maken van Live gebeurtenis](#live-event-creation-options)
* [URL's voor Live-gebeurtenis opnemen](#live-event-ingest-urls)
* [Live gebeurtenis de voorbeeld-URL](#live-event-preview-url)
* [Live uitvoer](#live-outputs).

## <a name="live-events"></a>Livegebeurtenissen

[Live gebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het opnemen en verwerken van de live-videofeeds. Wanneer u een Live gebeurtenis maakt, wordt een invoereindpunt gemaakt waarmee u kunt een live signaal verzenden vanaf een externe coderingsprogramma. De externe live codering verzendt de bijdrage feed aan die een invoereindpunt met behulp van de [RTMP](https://www.adobe.com/devnet/rtmp.html) of [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (gefragmenteerde MP4)-protocol. Voor de Smooth Streaming-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `http://` of `https://`. Voor het RTMP-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `rtmp://` of `rtmps://`. 

## <a name="live-event-types"></a>Live gebeurtenistypen

Een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: Pass Through- en live codering. 

### <a name="pass-through"></a>Pass-through

![Pass Through-query](./media/live-streaming/pass-through.svg)

Bij het gebruik van de pass-through **Live gebeurtenis**, afhankelijk van uw on-premises live coderingsprogramma meerdere bitrate videostream genereren en verzenden dat er als de bijdrage feed aan de Live-gebeurtenis (met behulp van RTMP- of gefragmenteerde MP4-protocol). De Live gebeurtenis wordt vervolgens via de binnenkomende video stromen zonder verdere verwerking. Dergelijke een Pass Through-LiveEvent is geoptimaliseerd voor langlopende live gebeurtenissen of 24 x lineair 365 live streamen. Bij het maken van dit soort Live gebeurtenis, geeft u geen (LiveEventEncodingType.None).

U kunt verzenden de bijdrage feed met een resolutie van maximaal 4 K en bij een frame 60 aantal frames per seconde, met H.264/AVC of H.265/HEVC videocodecs en AAC (AAC-LC, hij AACv1 of hij AACv2)-audiocodec.  Zie de [Live gebeurtenis van het type vergelijking](live-event-types-comparison.md) artikel voor meer informatie.

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

Zie een .NET-codevoorbeeld in [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![Live encoding](./media/live-streaming/live-encoding.svg)

Wanneer u live coderen met Media Services, zou u uw on-premises live coderingsprogramma voor het verzenden van een single-bitrate video als de bijdrage feed om de Live-gebeurtenis (met behulp van RTMP- of Fragmented Mp4-protocol) te configureren. De Live gebeurtenis codeert die binnenkomende single-bitrate stream naar een [meerdere bitrate videostream](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), maakt deze beschikbaar zijn voor de levering van voor het afspelen van apparaten via protocollen, zoals MPEG-DASH, HLS en Smooth Streaming. Bij het maken van dit soort Live gebeurtenis, geef het type codering als **Standard** (LiveEventEncodingType.Standard).

U kunt de bijdrage op maximaal 1080 p oplossing tegen een tarief van het kader van 30 frames/seconde, met de video codec H.264/AVC en AAC-kanaal verzenden (AAC-LC, hij AACv1 of hij AACv2)-audiocodec. Zie de [Live gebeurtenis van het type vergelijking](live-event-types-comparison.md) artikel voor meer informatie.

Bij het gebruik van live codering (Live gebeurtenis wordt ingesteld op **Standard**), de vooraf ingestelde standaardcodering definieert hoe de binnenkomende stream wordt gecodeerd in meerdere bitsnelheden of lagen. Zie voor meer informatie, [systeemwaarden](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Op dit moment de enige toegestane vooraf ingestelde waarde voor het type standaard Live gebeurtenis is *Default720p*. Als u gebruiken een aangepaste voorinstelling voor live codering wilt, contact op met amshelp@microsoft.com. U moet de gewenste tabel van de oplossing en bitrates opgeven. Controleer of er slechts één laag bij 720p en maximaal 6 lagen.

## <a name="live-event-creation-options"></a>Opties voor het maken van Live gebeurtenis

Bij het maken van een Live-gebeurtenis, kunt u de volgende opties opgeven:

* Het streaming-protocol voor de livegebeurtenis (momenteel worden de protocollen RTMP en Smooth Streaming ondersteund).<br/>U kunt de protocoloptie niet wijzigen terwijl de livegebeurtenis of de daaraan gekoppelde live-uitvoer worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u voor elk streaming-protocol een afzonderlijke livegebeurtenis.  
* IP-beperkingen voor de opname en voorbeeldweergave. U kunt de IP-adressen definiëren die zijn toegestaan om een video van deze livegebeurtenis op te nemen. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').<br/>Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.<br/>De IP-adressen moeten een van de volgende indelingen hebben: IpV4-adres met 4 cijfers, CIDR-adresbereik.
* Bij het maken van de gebeurtenis kunt u opgeven dat deze automatisch wordt gestart. <br/>Wanneer autostart is ingesteld op True, wordt de Live gebeurtenis gestart na het maken ervan. De facturering begint zodra de Live gebeurtenis wordt gestart. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten. U kunt ook kun u de gebeurtenis wanneer u klaar bent om te streamen. 

    Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.

## <a name="live-event-ingest-urls"></a>URL's voor Live-gebeurtenis opnemen

Nadat de Live-gebeurtenis is gemaakt, krijgt u URL's die u aan de live on-premises coderingsprogramma voor opnemen. Het live coderingsprogramma gebruikt deze URL's voor het invoeren van een live stream. Zie voor meer informatie, [aanbevolen on-premises live coderingsprogramma's](recommended-on-premises-live-encoders.md). 

U kunt geen aangepaste URL's of aangepaste URL's gebruiken. 

* Niet-vanity-URL

    Niet-vanity-URL is de standaardmodus in AMS v3. U mogelijk snel de Live gebeurtenis maar URL voor opnemen staat bekend als er op de live-gebeurtenis wordt gestart. De URL zal veranderen als u stoppen/starten de Live-gebeurtenis. <br/>Niet-Vanity is handig in scenario's wanneer een gebruiker wil om te streamen met behulp van een app wanneer de app wil ophalen van een live gebeurtenis ASAP en met een dynamische opname-URL is geen probleem.
* Vanity URL

    De voorkeur vanity-modus door grote media tv-zenders die gebruikmaken van hardware broadcast-coderingsprogramma's en niet wilt dat hun coderingsprogramma's opnieuw configureren wanneer ze de Live-gebeurtenis starten. Ze willen een voorspellende opname-URL, die niet na verloop van tijd verandert.

> [!NOTE] 
> Voor een URL voor opnemen worden voorspeld, moet u de modus 'aangepaste' gebruiken en uw eigen token (om te voorkomen dat een willekeurige token in de URL) doorgeven.

### <a name="live-ingest-url-naming-rules"></a>Live naamgevingsregels URL voor opnemen

De *willekeurige* onderstaande tekenreeks is een 128-bits hexadecimaal nummer (die bestaat uit 32 tekens van 0-9, a-f).<br/>
De *toegangstoken* hieronder vindt u wat u nodig hebt om op te geven voor vaste-URL. Het is ook 128-bits hexadecimaal nummer.

#### <a name="non-vanity-url"></a>Niet-vanity-URL

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/<access token>`
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/<access token>`
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/<access token>`
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/<access token>`

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml`
`https://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml`

#### <a name="vanity-url"></a>Vanity URL

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/<access token>`
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/<access token>`
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/<access token>`
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/<access token>`

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml`
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml`

## <a name="live-event-preview-url"></a>Live gebeurtenis de voorbeeld-URL

Zodra de **Live gebeurtenis** ontvangen van de bijdrage feed is gestart, kunt u de preview-eindpunt om te bekijken en te valideren dat u de live stream ontvangt voordat u verder publiceert. Nadat u hebt gecontroleerd dat de stroom preview goed is, kunt u de LiveEvent gebruiken om de live stream beschikbaar voor levering via een of meer (vooraf gemaakt) **Streaming-eindpunten**. Om dit te realiseren, maakt u een nieuw [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs) op de **Live gebeurtenis**. 

> [!IMPORTANT]
> Zorg ervoor dat de video worden doorgestuurd naar de URL van de Preview-versie voordat u verdergaat!

## <a name="live-outputs"></a>Live-uitvoer

Zodra u de stroom doorgestuurd naar de Live gebeurtenis hebt, kunt u de streaminggebeurtenis starten door het maken van een [Asset](https://docs.microsoft.com/rest/api/media/assets), [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs), en [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators). Live uitvoer wordt de stream gearchiveerd en beschikbaar maken aan kijkers via de [Streaming-eindpunt](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Live-uitvoer starten zodra ze zijn gemaakt en stoppen wanneer ze worden verwijderd. Wanneer u de live-uitvoer verwijdert, verwijdert u de onderliggende Asset en de inhoud van de Asset niet. 

De relatie tussen een **Live gebeurtenis** en de bijbehorende **uitvoer Live** is vergelijkbaar met traditionele televisie broadcast, waarbij een kanaal (**Live gebeurtenis**) vertegenwoordigt een constante streamen van video en een opname (**uitvoer Live**) is afgestemd op een specifiek tijdstip-segment (bijvoorbeeld 's avonds nieuws van 18:30:00 uur op 19:00 uur). U kunt met behulp van een Digital Video Recorder (DVR) televisie vastleggen – de vergelijkbare functie in Live gebeurtenissen wordt beheerd de **ArchiveWindowLength** eigenschap. Het is een ISO 8601-timespan duur (bijvoorbeeld PTHH:MM:SS), waarmee wordt Hiermee geeft u de capaciteit van de DVR en kan worden ingesteld van minimaal 3 minuten tot een maximum van 25 uur.

De **uitvoer Live** object lijkt op een tape-recorder die variabel en noteer de live stream in een activum in Media Services-account. De opgenomen inhoud wordt permanent worden opgeslagen in de Azure Storage-account dat is gekoppeld aan uw account in de container die wordt gedefinieerd door de resource actief. De **uitvoer Live** ook kunt u enkele eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom wordt opgeslagen in het archief opnemen (bijvoorbeeld, de capaciteit van de cloud-DVR) en of viewers kunnen starten Volg de live stream. Het archief op schijf is een circulaire archief "venster" die bevat alleen de hoeveelheid inhoud die is opgegeven in de **archiveWindowLength** eigenschap van de **uitvoer Live**. Inhoud die buiten dit venster valt wordt automatisch verwijderd uit de opslagcontainer en kan niet worden hersteld. U kunt meerdere maken **uitvoer Live** (maximaal drie maximum) op een **Live gebeurtenis** met verschillende archief sleutellengten en -instellingen.  

Als u hebt gepubliceerd de **uitvoer Live**van **Asset** met behulp van een **Streaming-Locator gemaakt**, wordt de **Live gebeurtenis** wordt (maximaal de lengte van het DVR-venster) verder kan worden weergegeven totdat de Streaming-Locator verlopen of verwijderd, afhankelijk van wat het eerste komt.

Zie voor meer informatie, [met behulp van een cloud-DVR](live-event-cloud-dvr.md).

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)

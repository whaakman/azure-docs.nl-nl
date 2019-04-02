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
ms.date: 03/30/2019
ms.author: juliako
ms.openlocfilehash: 1d28701dd35b9d80fd52a1f102c53f2d59d63b09
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762351"
---
# <a name="live-events-and-live-outputs"></a>Live gebeurtenissen en live uitvoer

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Voor het configureren van uw live streaming-gebeurtenissen in Media Services v3, moet u inzicht in concepten die in dit artikel worden besproken. <br/>De lijst met secties wordt aan de rechterkant van de pagina weergegeven.

## <a name="live-events"></a>Livegebeurtenissen

[Livegebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het opnemen en verwerken van de live videofeeds. Als u een livegebeurtenis maakt, wordt er een invoereindpunt gemaakt dat u kunt gebruiken om een livesignaal vanaf een externe encoder te verzenden. De externe live-encoder verzendt de bijdragefeed naar dat invoereindpunt met behulp van het protocol [RTMP](https://www.adobe.com/devnet/rtmp.html) of [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (gefragmenteerd MP4). Voor de Smooth Streaming-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `http://` of `https://`. Voor het RTMP-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `rtmp://` of `rtmps://`. 

## <a name="live-event-types"></a>Live gebeurtenistypen

Een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: Pass Through- en live codering. 

### <a name="pass-through"></a>Pass-through

![Pass Through-query](./media/live-streaming/pass-through.svg)

Bij gebruik van de pass-through **livegebeurtenis** bent u afhankelijk van de on-premises live-encoder voor het genereren van een multiple-bitrate videostream en het verzenden ervan als de bijdragefeed voor de livegebeurtenis (met behulp van RTMP of een gefragmenteerd MP4-protocol). De livegebeurtenis voert de binnenkomende videostream vervolgens zonder verdere verwerking uit. Een dergelijke pass-through livegebeurtenis wordt geoptimaliseerd voor langdurige livegebeurtenissen of 24x365 lineair live streamen. Bij het maken van dit type livegebeurtenis, geeft u None op (LiveEventEncodingType.None).

U kunt de bijdragefeed verzenden bij een resolutie tot maximaal 4K en een framesnelheid van 60 frames per seconde, met ofwel H.264/AVC- of H.265/HEVC-videocodecs, en AAC (AAC-LC, HE-AACv1 of HE-AACv2)-audiocodec.  Zie het artikel [Vergelijking van typen livegebeurtenissen](live-event-types-comparison.md) voor meer informatie.

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

Zie een .NET-codevoorbeeld in [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![Live encoding](./media/live-streaming/live-encoding.svg)

Bij gebruik van live-codering met Media Services zou u uw on-premises live-encoder zodanig configureren dat één bitrate-video als bijdragefeed wordt verzonden naar de livegebeurtenis (met behulp van het protocol RTMP of gefragmenteerde MP4). De livegebeurtenis codeert de binnenkomende enkelvoudige bitrate gegevensstroom in een [videostroom met meerdere bitrates](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) en maakt deze beschikbaar voor afgifte naar afspeelapparaten via protocollen als MPEG-DASH, HLS en Smooth Streaming. Bij het maken van dit type livegebeurtenis, geeft u het coderingstype **Standard** (LiveEventEncodingType.Standard) op.

U kunt de bijdragefeed verzenden bij een resolutie tot maximaal 1080 p en een framesnelheid van 30 frames per seconde, met H.264/AVC-videocodec en AAC (AAC-LC, HE-AACv1 of HE-AACv2)-audiocodec. Zie het artikel [Vergelijking van typen livegebeurtenissen](live-event-types-comparison.md) voor meer informatie.

Bij gebruik van live codering (livegebeurtenis ingesteld op **Standard**), bepaalt de vooraf ingestelde codering hoe de binnenkomende stream in meerdere bitrates of lagen wordt gecodeerd. Zie [Systeemwaarden](live-event-types-comparison.md#system-presets) voor meer informatie.

> [!NOTE]
> Momenteel is *Default720p* de enige toegestane vooraf ingestelde waarde voor het type Standard van de livegebeurtenis. Als u een aangepaste vooraf ingestelde waarde voor live codering wilt gebruiken, neemt u contact op met amshelp@microsoft.com. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag is op 720 p, en maximaal zes lagen.

## <a name="live-event-creation-options"></a>Opties voor het maken van Live gebeurtenis

Bij het maken van een Live-gebeurtenis, kunt u de volgende opties opgeven:

* Het streaming-protocol voor de livegebeurtenis (momenteel worden de protocollen RTMP en Smooth Streaming ondersteund).<br/>U kunt de protocoloptie niet wijzigen terwijl de livegebeurtenis of de daaraan gekoppelde live-uitvoer worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u voor elk streaming-protocol een afzonderlijke livegebeurtenis.  
* IP-beperkingen voor de opname en voorbeeldweergave. U kunt de IP-adressen definiëren die zijn toegestaan om een video van deze livegebeurtenis op te nemen. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').<br/>Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.<br/>De IP-adressen moeten een van de volgende indelingen hebben: IpV4-adres met 4 cijfers, CIDR-adresbereik.
* Bij het maken van de gebeurtenis kunt u opgeven dat deze automatisch wordt gestart. <br/>Wanneer autostart is ingesteld op True, wordt de Live gebeurtenis gestart na het maken ervan. De facturering begint zodra de Live gebeurtenis wordt gestart. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten. U kunt ook kun u de gebeurtenis wanneer u klaar bent om te streamen. 

    Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.

## <a name="live-event-ingest-urls"></a>URL's voor Live-gebeurtenis opnemen

Wanneer de livegebeurtenis is gemaakt, kunt u URL's voor opnemen ophalen die u aan de on-premises live-encoder levert. De live-encoder gebruikt deze URL's voor het invoeren van een live-stream. Zie voor meer informatie, [aanbevolen on-premises live coderingsprogramma's](recommended-on-premises-live-encoders.md). 

U kunt niet-vanity-URL's en vanity-URL's gebruiken. 

* Niet-vanity-URL

    Een niet-vanity-URL is de standaardmodus in AMS v3. U krijgt de livegebeurtenis mogelijk snel, maar de opname-URL is alleen bekend als de livegebeurtenis wordt gestart. De URL wordt gewijzigd als u de livegebeurtenis stopt of start. <br/>Niet-vanity is handig in scenario's waarbij een eindgebruiker wil streamen met een app waarbij de app een livegebeurtenis zo snel mogelijk wil downloaden en waarbij een dynamische opname-URL geen probleem is.
* Vanity URL

    De vanity-modus heeft de voorkeur bij grote tv-media die gebruikmaken van hardware broadcast-encoders en die hun encoders niet opnieuw willen configureren als de livegebeurtenis wordt gestart. Ze willen een voorspellende opname-URL, die niet na verloop van tijd verandert.

> [!NOTE] 
> Voor een URL voor opnemen worden voorspeld, moet u de modus 'aangepaste' gebruiken en uw eigen token (om te voorkomen dat een willekeurige token in de URL) doorgeven.

### <a name="live-ingest-url-naming-rules"></a>Live naamgevingsregels URL voor opnemen

De *willekeurige* tekenreeks hieronder is een 128-bits hexadecimaal getal (bestaande uit 32 tekens, van 0-9 en a-f).<br/>
Het *toegangstoken* hieronder dient u op te geven voor een vaste URL. Dit is ook een 128-bits hexadecimaal getal.

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

## <a name="live-event-long-running-operations"></a>Live gebeurtenis langlopende bewerkingen

Zie voor meer informatie, [langlopende bewerkingen](entities-overview.md#long-running-operations)

## <a name="live-outputs"></a>Live-uitvoer

Zodra u de stroom doorgestuurd naar de Live gebeurtenis hebt, kunt u de streaminggebeurtenis starten door het maken van een [Asset](https://docs.microsoft.com/rest/api/media/assets), [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs), en [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators). Live uitvoer wordt de stream gearchiveerd en beschikbaar maken aan kijkers via de [Streaming-eindpunt](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Live-uitvoer starten zodra ze zijn gemaakt en stoppen wanneer ze worden verwijderd. Wanneer u de live-uitvoer verwijdert, verwijdert u de onderliggende Asset en de inhoud van de Asset niet. 

De relatie tussen een **Live gebeurtenis** en de bijbehorende **uitvoer Live** is vergelijkbaar met traditionele televisie broadcast, waarbij een kanaal (**Live gebeurtenis**) vertegenwoordigt een constante streamen van video en een opname (**uitvoer Live**) is afgestemd op een specifiek tijdstip-segment (bijvoorbeeld 's avonds nieuws van 18:30:00 uur op 19:00 uur). U kunt televisie opnemen met een digitale videorecorder (DVR). De overeenkomstige functie in livegebeurtenissen wordt beheerd via de eigenschap **ArchiveWindowLength**. Het is een ISO 8601-timespan duur (bijvoorbeeld PTHH:MM:SS), waarmee wordt Hiermee geeft u de capaciteit van de DVR en kan worden ingesteld van minimaal 3 minuten tot een maximum van 25 uur.

De **uitvoer Live** object lijkt op een tape-recorder die variabel en noteer de live stream in een activum in Media Services-account. De opgenomen inhoud wordt permanent worden opgeslagen in de Azure Storage-account dat is gekoppeld aan uw account in de container die wordt gedefinieerd door de resource actief. De **uitvoer Live** ook kunt u enkele eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom wordt opgeslagen in het archief opnemen (bijvoorbeeld, de capaciteit van de cloud-DVR) en of viewers kunnen starten Volg de live stream. Het archief op schijf is een circulaire archief "venster" die bevat alleen de hoeveelheid inhoud die is opgegeven in de **archiveWindowLength** eigenschap van de **uitvoer Live**. Inhoud die buiten dit venster valt wordt automatisch verwijderd uit de opslagcontainer en kan niet worden hersteld. U kunt meerdere maken **uitvoer Live** (maximaal drie maximum) op een **Live gebeurtenis** met verschillende archief sleutellengten en -instellingen.  

Als u hebt gepubliceerd de **uitvoer Live**van **Asset** met behulp van een **Streaming-Locator gemaakt**, wordt de **Live gebeurtenis** wordt (maximaal de lengte van het DVR-venster) verder kan worden weergegeven totdat de Streaming-Locator verlopen of verwijderd, afhankelijk van wat het eerste komt.

Zie voor meer informatie, [met behulp van een cloud-DVR](live-event-cloud-dvr.md).

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)

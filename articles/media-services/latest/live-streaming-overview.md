---
title: Overzicht van Live streamen met Azure Media Services | Microsoft Docs
description: Dit artikel biedt een overzicht van live streamen met Azure Media Services v3.
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
ms.date: 01/22/2019
ms.author: juliako
ms.openlocfilehash: e7e5ddf06fc98f13b9848bf5ede208eef47efd69
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54886659"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Live streamen met Azure Media Services v3

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Om te streamen met Media Services uw live-evenementen, moet u het volgende:  

- Een camera die wordt gebruikt om vast te leggen van de live-gebeurtenis.
- Een coderingsprogramma voor live video dat signalen van de camera (of een ander apparaat, zoals een laptop) converteert naar een bijdrage feed die wordt verzonden naar Media Services. De feed bijdrage kan signalen met betrekking tot reclame, zoals SCTE 35 markeringen bevatten.
- Onderdelen in Media Services, waarmee u om op te nemen, preview, inpakken, registreren, versleutelen en uitzenden van de live-gebeurtenis aan uw klanten of aan een CDN voor verdere distributie.

In dit artikel biedt een gedetailleerd overzicht, richtlijnen en diagrammen van de belangrijkste onderdelen die betrokken zijn bij het live streamen met Media Services bevat.

## <a name="live-streaming-workflow"></a>Werkstroom voor live streaming

Hier volgen de stappen voor een live streaming-werkstroom:

1. Zorg ervoor dat de **Streaming-eindpunt** wordt uitgevoerd. 
2. Maak een **Live evenement**. <br/>Bij het maken van de gebeurtenis, kunt u op automatisch starten het. U kunt ook kun u de gebeurtenis wanneer u klaar bent om te streamen.<br/> Wanneer autostart is ingesteld op true, wordt de Live gebeurtenis wordt gestart rechts nadat is gemaakt. Dit betekent dat, de facturering begint zodra de Live gebeurtenis wordt gestart. U moet stoppen expliciet aanroepen op de bron van de Live gebeurtenis verder facturering is gestopt. Zie voor meer informatie, [Live gebeurtenis Staten en facturering](live-event-states-billing.md).
3. De opname-URL's ophalen en uw on-premises coderingsprogramma voor het gebruik van de URL voor het verzenden van de bijdrage feed configureren.<br/>Zie [aanbevolen live coderingsprogramma's](recommended-on-premises-live-encoders.md).
4. De voorbeeld-URL ophalen en deze gebruiken om te controleren dat de invoer van het coderingsprogramma daadwerkelijk worden ontvangen.
5. Maak een nieuwe **Asset** object.
6. Maak een **uitvoer Live** en gebruikt u de assetnaam van de die u hebt gemaakt.<br/>De **uitvoer Live** worden gearchiveerd de stroom in de **Asset**.
7. Maak een **Streaming-Locator gemaakt** met de ingebouwde **beleid Streaming** typen.<br/>Als u van plan bent uw inhoud coderen, raadpleegt u [Content protection overzicht](content-protection-overview.md).
8. Lijst van de paden op de **Streaming-Locator gemaakt** om terug te gaan de URL's te gebruiken (dit zijn deterministische).
9. Ophalen van de hostnaam voor de **Streaming-eindpunt** u wilt streamen uit.
10. De URL in stap 8 worden gecombineerd met de hostnaam in stap 9 om op te halen van de volledige URL.
11. Als u wilt stoppen met het maken van uw **Live gebeurtenis** zichtbaar, dat u wilt stoppen met het streamen van de gebeurtenis en verwijderen de **Streaming-Locator gemaakt**.<br/>Zie voor meer informatie de [Live streaming zelfstudie](stream-live-tutorial-with-api.md).

## <a name="overview-of-main-components"></a>Overzicht van de belangrijkste onderdelen

Met het oog op de on-demand of live streams met Media Services, moet u beschikken over ten minste één [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/streamingendpoints). Wanneer uw Media Services-account wordt gemaakt een **standaard** streamingendpoint zo wordt toegevoegd aan uw account in de **gestopt** staat. U moet de streamingendpoint zo van waaruit u uw inhoud streamen naar uw viewers wilt starten. U kunt de standaardwaarde **Streaming-eindpunt**, of maak een ander aangepast **Streaming-eindpunt** met de gewenste configuratie- en CDN-instellingen. U besluiten om in te schakelen van meerdere **Streaming-eindpunten**, elke regel die gericht is op een andere CDN en bieden een unieke hostnaam voor het leveren van inhoud. 

In Media Services [Live gebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het opnemen en verwerken van de live-videofeeds. Wanneer u een Live gebeurtenis maakt, wordt een invoereindpunt gemaakt waarmee u kunt een live signaal verzenden vanaf een externe coderingsprogramma. De externe live codering verzendt de bijdrage feed aan die een invoereindpunt met behulp van de [RTMP](https://www.adobe.com/devnet/rtmp.html) of [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (gefragmenteerde MP4)-protocol. Voor de Smooth Streaming-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `http://` of `https://`. Voor het RTMP-opnameprotocol worden gebruikt, zijn de ondersteunde URL-schema's `rtmp://` of `rtmps://`. Zie voor meer informatie, [aanbevolen live coderingsprogramma's streaming](recommended-on-premises-live-encoders.md).<br/>
Bij het maken van een **Live gebeurtenis**, kunt u toegestane IP-adressen in een van de volgende indelingen: IpV4-adres met 4 cijfers, CIDR-adresbereik.

Zodra de **Live gebeurtenis** ontvangen van de bijdrage feed is gestart, kunt u de preview-eindpunt (de voorbeeld-URL te bekijken en te valideren dat u de live stream voordat u verdere publiceert ontvangt. Nadat u hebt gecontroleerd dat de stroom preview goed is, kunt u de LiveEvent gebruiken om de live stream beschikbaar voor levering via een of meer (vooraf gemaakt) **Streaming-eindpunten**. Om dit te realiseren, maakt u een nieuw [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs) op de **Live gebeurtenis**. 

De **uitvoer Live** object lijkt op een tape-recorder die variabel en noteer de live stream in een activum in Media Services-account. De opgenomen inhoud wordt permanent worden opgeslagen in de Azure Storage-account dat is gekoppeld aan uw account in de container die wordt gedefinieerd door de resource actief.  De **Live uitvoer geproduceerd** ook kunt u enkele eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom wordt opgeslagen in het archief opnemen (bijvoorbeeld, de capaciteit van de cloud-DVR) bepalen. Het archief op schijf is een circulaire archief "venster" die bevat alleen de hoeveelheid inhoud die is opgegeven in de **archiveWindowLength** eigenschap van de **uitvoer Live**. Inhoud die buiten dit venster valt wordt automatisch verwijderd uit de opslagcontainer en kan niet worden hersteld. U kunt meerdere maken **uitvoer Live** (maximaal drie maximum) op een **Live gebeurtenis** met verschillende archief sleutellengten en -instellingen.  

Met Media Services kunt u profiteren van **dynamische pakketten**, waarmee u preview uit te zenden van uw live streams in [MPEG DASH, HLS en Smooth Streaming-indelingen](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) van de bijdrage feed te sturen naar de service. De doelgroep kunnen de live stream met een compatibele spelers HLS, DASH of Smooth Streaming worden afgespeeld. U kunt [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) in uw web- of mobiele toepassingen voor het leveren van uw stroom in een van deze protocollen.

Media Services kunt u uw inhoud dynamisch wordt versleuteld bezorgen (**dynamische versleuteling**) met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties naar geautoriseerde clients. Zie voor meer informatie over het versleutelen van uw inhoud met Media Services [overzicht inhoud beveiligen](content-protection-overview.md)

Indien gewenst, kunt u ook toepassen dynamisch filteren, die kan worden gebruikt voor het beheren van het aantal sporen te wissen, indelingen bitsnelheden en presentatie tijdvensters die worden verzonden naar de spelers. Zie voor meer informatie, [Filters en dynamische manifesten](filters-dynamic-manifest-overview.md).

Zie voor meer informatie over nieuwe mogelijkheden voor live streamen in v3 [migratierichtlijnen voor het verplaatsen van Media Services v2 naar v3](migrate-from-v2-to-v3.md).

## <a name="live-event-types"></a>Live gebeurtenistypen

Een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: Pass Through- en live codering. 

### <a name="pass-through"></a>Pass-through

![Pass Through-query](./media/live-streaming/pass-through.png)

Bij het gebruik van de pass-through **Live gebeurtenis**, afhankelijk van uw on-premises live coderingsprogramma meerdere bitrate videostream genereren en verzenden dat er als de bijdrage feed aan de Live-gebeurtenis (met behulp van RTMP- of gefragmenteerde MP4-protocol). De Live gebeurtenis wordt vervolgens via de binnenkomende video stromen zonder verdere verwerking. Dergelijke een Pass Through-LiveEvent is geoptimaliseerd voor langlopende live gebeurtenissen of 24 x lineair 365 live streamen. Bij het maken van dit soort Live gebeurtenis, geeft u geen (LiveEventEncodingType.None).

U kunt verzenden de bijdrage feed met een resolutie van maximaal 4 K en bij een frame 60 aantal frames per seconde, met H.264/AVC of H.265/HEVC videocodecs en AAC (AAC-LC, hij AACv1 of hij AACv2)-audiocodec.  Zie de [Live gebeurtenis vergelijkings- en beperkingen van het type](live-event-types-comparison.md) artikel voor meer informatie.

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

Zie het voorbeeld van een live in [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![Live encoding](./media/live-streaming/live-encoding.png)

Wanneer u live coderen met Media Services, zou u uw on-premises live coderingsprogramma voor het verzenden van een single-bitrate video als de bijdrage feed om de Live-gebeurtenis (met behulp van RTMP- of Fragmented Mp4-protocol) te configureren. De Live gebeurtenis codeert die binnenkomende single-bitrate stream naar een [meerdere bitrate videostream](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), maakt deze beschikbaar zijn voor de levering van voor het afspelen van apparaten via protocollen, zoals MPEG-DASH, HLS en Smooth Streaming. Bij het maken van dit soort Live gebeurtenis, geef het type codering als **Standard** (LiveEventEncodingType.Standard).

U kunt de bijdrage op maximaal 1080 p oplossing tegen een tarief van het kader van 30 frames/seconde, met de video codec H.264/AVC en AAC-kanaal verzenden (AAC-LC, hij AACv1 of hij AACv2)-audiocodec. Zie de [Live gebeurtenis vergelijkings- en beperkingen van het type](live-event-types-comparison.md) artikel voor meer informatie.

## <a name="live-event-types-comparison"></a>Vergelijking van de typen Live gebeurtenis

Het volgende artikel bevat een tabel met de vergelijking tussen de functies van de twee typen van de Live gebeurtenis: [Vergelijking](live-event-types-comparison.md).

## <a name="live-output"></a>Live-uitvoer

Een [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs) kunt u voor het beheren van de eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom is geregistreerd (bijvoorbeeld, de capaciteit van de cloud-DVR) en of viewers bekijken van de live stream kunnen starten. De relatie tussen een **Live gebeurtenis** en de bijbehorende **uitvoer Live**s relatie is vergelijkbaar met traditionele televisie broadcast, waarbij een kanaal (**Live gebeurtenis**) Hiermee geeft u een constante stream met video's en een opname (**uitvoer Live**) is afgestemd op een specifiek tijdstip-segment (bijvoorbeeld 's avonds nieuws van 18:30:00 uur op 19:00 uur). U kunt met behulp van een Digital Video Recorder (DVR) televisie vastleggen – de vergelijkbare functie in LiveEvents wordt beheerd via de eigenschap ArchiveWindowLength. Het is een ISO 8601-timespan duur (bijvoorbeeld PTHH:MM:SS), waarmee wordt Hiermee geeft u de capaciteit van de DVR en kan worden ingesteld van minimaal 3 minuten tot een maximum van 25 uur.

> [!NOTE]
> **Live uitvoer**s starten bij het maken en te stoppen wanneer verwijderd. Wanneer u verwijdert de **uitvoer Live**, verwijdert u niet de onderliggende **Asset** en de inhoud in de asset. 
>
> Als u hebt gepubliceerd de **uitvoer Live** asset met behulp van een **Streaming-Locator gemaakt**, wordt de **Live gebeurtenis** (tot de lengte van het DVR-venster) blijft weergegeven tot de **Streaming-Locator gemaakt**van verlopen of verwijderd, afhankelijk van wat eerst komt.

Zie voor meer informatie, [werken met cloud-DVR](live-event-cloud-dvr.md).

## <a name="streaming-endpoint"></a>Streaming-eindpunt

Zodra u de stroom doorgestuurd hebt naar de **Live gebeurtenis**, kunt u de streaminggebeurtenis starten door het maken van een **Asset**, **uitvoer Live**, en **Streaming-Locator gemaakt** . Hiermee wordt de stream gearchiveerd en beschikbaar maken aan kijkers via de [Streaming-eindpunt](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Wanneer uw Media Services-account is gemaakt wordt een standaardstreaming-eindpunt wordt toegevoegd aan uw account in de status gestopt. Als u wilt uw inhoud wilt streamen en te profiteren van dynamische pakketten en dynamische versleuteling, is het streaming-eindpunt van waaruit u inhoud streamen wilt in de uitvoeringsstatus van.

## <a name="a-idbilling-live-event-states-and-billing"></a><a id="billing" />Live gebeurtenis Staten en facturering

Een **Live gebeurtenis** begint zodra de status verandert in facturering **met**. Als u wilt stoppen met de Live-gebeurtenis van facturering, moet u de Live-gebeurtenis beëindigen.

Zie voor gedetailleerde informatie [Staten en facturering](live-event-states-billing.md).

## <a name="latency"></a>Latentie

Zie voor gedetailleerde informatie over LiveEvents latentie, [latentie](live-event-latency.md).

## <a name="next-steps"></a>Volgende stappen

- [Vergelijking van de typen Live gebeurtenis](live-event-types-comparison.md)
- [Statussen en facturering](live-event-states-billing.md)
- [Latentie](live-event-latency.md)

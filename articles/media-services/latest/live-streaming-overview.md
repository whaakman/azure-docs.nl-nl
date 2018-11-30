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
ms.date: 11/26/2018
ms.author: juliako
ms.openlocfilehash: 634563a2010562e20691abae132dc7540ef8faf2
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632695"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Live streamen met Azure Media Services v3

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Om te streamen met Media Services uw live-evenementen, moet u het volgende:  

- Een camera die wordt gebruikt om vast te leggen van de live-gebeurtenis.
- Een coderingsprogramma voor live video dat signalen van de camera (of een ander apparaat, zoals een laptop) converteert naar een bijdrage feed die wordt verzonden naar Media Services. De feed bijdrage kan signalen met betrekking tot reclame, zoals SCTE 35 markeringen bevatten.
- Onderdelen in Media Services, waarmee u om op te nemen, preview, inpakken, registreren, versleutelen en uitzenden van de live-gebeurtenis aan uw klanten of aan een CDN voor verdere distributie.

In dit artikel biedt een gedetailleerd overzicht, richtlijnen en diagrammen van de belangrijkste onderdelen die betrokken zijn bij het live streamen met Media Services bevat.

## <a name="overview-of-main-components"></a>Overzicht van de belangrijkste onderdelen

Met het oog op de on-demand of live streams met Media Services, moet u beschikken over ten minste één [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/streamingendpoints). Wanneer uw Media Services-account wordt gemaakt een **standaard** streamingendpoint zo wordt toegevoegd aan uw account in de **gestopt** staat. U moet de streamingendpoint zo van waaruit u uw inhoud streamen naar uw viewers wilt starten. U kunt de standaardwaarde **streamingendpoint zo**, of maak een ander aangepast **streamingendpoint zo** met de gewenste configuratie- en CDN-instellingen. U besluiten om in te schakelen van meerdere door, elke regel die gericht is op een andere CDN en bieden een unieke hostnaam voor het leveren van inhoud. 

In Media Services [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het opnemen en verwerken van de live-videofeeds. Wanneer u een LiveEvent maakt, wordt een invoereindpunt gemaakt waarmee u kunt een live signaal verzenden vanaf een externe coderingsprogramma. De externe live codering verzendt de bijdrage feed aan die een invoereindpunt met behulp van de [RTMP](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) of [Smooth Streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming#Microsoft_Smooth_Streaming) (gefragmenteerde MP4)-protocol.  

Zodra de **LiveEvent** ontvangen van de bijdrage feed is gestart, kunt u de preview-eindpunt (de voorbeeld-URL te bekijken en te valideren dat u de live stream voordat u verdere publiceert ontvangt. Nadat u hebt gecontroleerd dat de stroom preview goed is, kunt u de LiveEvent gebruiken om de live stream beschikbaar voor levering via een of meer (vooraf gemaakt) **door**. Om dit te realiseren, maakt u een nieuw [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) op de **LiveEvent**. 

De **LiveOutput** object lijkt op een tape-recorder die variabel en noteer de live stream in een activum in Media Services-account. De opgenomen inhoud wordt permanent worden opgeslagen in de Azure Storage-account dat is gekoppeld aan uw account in de container die wordt gedefinieerd door de resource actief.  De **LiveOuput** ook kunt u enkele eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom wordt opgeslagen in het archief opnemen (bijvoorbeeld, de capaciteit van de cloud-DVR) bepalen. Het archief op schijf is een circulaire archief "venster" die bevat alleen de hoeveelheid inhoud die is opgegeven in de **archiveWindowLength** eigenschap van de **LiveOutput**. Inhoud die buiten dit venster valt wordt automatisch verwijderd uit de opslagcontainer en kan niet worden hersteld. U kunt meerdere LiveOutputs (maximaal drie maximum) maken op een LiveEvent met verschillende archief sleutellengten en -instellingen.  

Met Media Services kunt u profiteren van **dynamische pakketten**, waarmee u preview uit te zenden van uw live streams in [MPEG DASH, HLS en Smooth Streaming-indelingen](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) van de bijdrage feed te sturen naar de service. De doelgroep kunnen de live stream met een compatibele spelers HLS, DASH of Smooth Streaming worden afgespeeld. U kunt [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) in uw web- of mobiele toepassingen voor het leveren van uw stroom in een van deze protocollen.

Media Services kunt u uw inhoud dynamisch wordt versleuteld bezorgen (**dynamische versleuteling**) met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties naar geautoriseerde clients. Zie voor meer informatie over het versleutelen van uw inhoud met Media Services [overzicht inhoud beveiligen](content-protection-overview.md)

Indien gewenst, kunt u ook toepassen dynamisch filteren, die kan worden gebruikt voor het beheren van het aantal sporen te wissen, indelingen bitsnelheden en presentatie tijdvensters die worden verzonden naar de spelers. 

### <a name="new-capabilities-for-live-streaming-in-v3"></a>Nieuwe mogelijkheden voor live streamen in v3

Met de v3 API's van Media Services kunt profiteren u van de volgende nieuwe functies:

- Nieuwe modus voor lage latentie. Zie voor meer informatie, [latentie](live-event-latency.md).
- Verbeterde RTMP-ondersteuning (verbeterde stabiliteit en meer bron encoder-ondersteuning).
- Beveiligde RTMPS opnemen.<br/>Als u een LiveEvent maakt, krijgt u 4 URL's voor opnemen. Opname van de 4 URL's zijn bijna identiek, hetzelfde streaming-token (AppId), alleen de poort nummer onderdeel verschilt. Twee van de URL's zijn primaire en back-up voor RTMPS.   
- U kunt live gebeurtenissen die tot 24 uur lang wanneer met behulp van Media Services voor een single-bitrate bijdrage transcodering feed in een uitvoerstroom met meerdere bitrates streamen. 

## <a name="liveevent-types"></a>LiveEvent typen

Een [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: Pass Through- en live codering. 

### <a name="pass-through"></a>Pass-through

![Pass Through-query](./media/live-streaming/pass-through.png)

Wanneer u de Pass Through-LiveEvent gebruikt, wordt u afhankelijk zijn van uw on-premises live coderingsprogramma meerdere bitrate videostream genereren en verzenden dat er als de bijdrage-naar de LiveEvent (met behulp van RTMP- of gefragmenteerde MP4-protocol). De LiveEvent voert vervolgens via de binnenkomende video stromen zonder verdere verwerking. Dergelijke een Pass Through-LiveEvent is geoptimaliseerd voor langlopende live gebeurtenissen of 24 x lineair 365 live streamen. Bij het maken van dit type LiveEvent, geeft u geen (LiveEventEncodingType.None).

U kunt verzenden de bijdrage feed met een resolutie van maximaal 4 K en bij een frame 60 aantal frames per seconde, met H.264/AVC of H.265/HEVC videocodecs en AAC (AAC-LC, hij AACv1 of hij AACv2)-audiocodec.  Zie de [LiveEvent vergelijkings- en beperkingen van het type](live-event-types-comparison.md) artikel voor meer informatie.

> [!NOTE]
> Het gebruik van de passthrough-methode is de meest voordelige manier om live te streamen wanneer u meerdere gebeurtenissen gedurende een langere periode streamt en u al hebt geïnvesteerd in on-premises coderingsprogramma’s. Zie de details over de [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
> 

Zie het voorbeeld van een live in [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![Live encoding](./media/live-streaming/live-encoding.png)

Wanneer u live coderen met Media Services, zou u uw on-premises live coderingsprogramma voor het verzenden van een single-bitrate video als de bijdrage feed om de LiveEvent (met behulp van RTMP- of Fragmented Mp4-protocol) te configureren. De LiveEvent codeert die binnenkomende single-bitrate stream naar een [meerdere bitrate videostream](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), maakt deze beschikbaar zijn voor de levering van voor het afspelen van apparaten via protocollen, zoals MPEG-DASH, HLS en Smooth Streaming. Bij het maken van dit type LiveEvent, geef het type codering als **Basic** (LiveEventEncodingType.Basic).

U kunt de bijdrage op maximaal 1080 p oplossing tegen een tarief van het kader van 30 frames/seconde, met de video codec H.264/AVC en AAC-kanaal verzenden (AAC-LC, hij AACv1 of hij AACv2)-audiocodec. Zie de [LiveEvent vergelijkings- en beperkingen van het type](live-event-types-comparison.md) artikel voor meer informatie.

## <a name="liveevent-types-comparison"></a>LiveEvent typen vergelijking

Het volgende artikel bevat een tabel met de vergelijking tussen de functies van de twee LiveEvent-typen: [vergelijking](live-event-types-comparison.md).

## <a name="liveoutput"></a>LiveOutput

Een [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) kunt u voor het beheren van de eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom is geregistreerd (bijvoorbeeld, de capaciteit van de cloud-DVR) en of viewers bekijken van de live stream kunnen starten. De relatie tussen een **LiveEvent** en de bijbehorende **LiveOutput**s relatie is vergelijkbaar met traditionele televisie broadcast, waarbij een kanaal (**LiveEvent**) Hiermee geeft u een constante stream met video's en een opname (**LiveOutput**) is afgestemd op een specifiek tijdstip-segment (bijvoorbeeld 's avonds nieuws van 18:30:00 uur op 19:00 uur). U kunt met behulp van een Digital Video Recorder (DVR) televisie vastleggen – de vergelijkbare functie in LiveEvents wordt beheerd via de eigenschap ArchiveWindowLength. Het is een ISO 8601-timespan duur (bijvoorbeeld PTHH:MM:SS), waarmee wordt Hiermee geeft u de capaciteit van de DVR en kan worden ingesteld van minimaal 3 minuten tot een maximum van 25 uur.


> [!NOTE]
> **LiveOutput**s starten bij het maken en te stoppen wanneer verwijderd. Wanneer u verwijdert de **LiveOutput**, verwijdert u niet de onderliggende **Asset** en de inhoud in de Asset.  

Zie voor meer informatie, [werken met cloud-DVR](live-event-cloud-dvr.md).

## <a name="streamingendpoint"></a>Streamingendpoint zo

Zodra u de stroom doorgestuurd hebt naar de **LiveEvent**, kunt u de streaminggebeurtenis starten door het maken van een **Asset**, **LiveOutput**, en **StreamingLocator** . Hiermee wordt de stream gearchiveerd en beschikbaar maken aan kijkers via de [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Wanneer uw Media Services-account is gemaakt wordt een standaardstreaming-eindpunt wordt toegevoegd aan uw account in de status gestopt. Als u wilt uw inhoud wilt streamen en te profiteren van dynamische pakketten en dynamische versleuteling, is het streaming-eindpunt van waaruit u inhoud streamen wilt in de uitvoeringsstatus van.

## <a name="a-idbilling-liveevent-states-and-billing"></a><a id="billing" />LiveEvent Staten en facturering

Een LiveEvent begint zodra de status verandert in facturering **met**. Als u wilt stoppen met het LiveEvent van facturering, moet u de LiveEvent stoppen.

Zie voor gedetailleerde informatie [Staten en facturering](live-event-states-billing.md).

## <a name="latency"></a>Latentie

Zie voor gedetailleerde informatie over LiveEvents latentie, [latentie](live-event-latency.md).

## <a name="live-streaming-workflow"></a>Werkstroom voor live streaming

Hier volgen de stappen voor een live streaming-werkstroom:

1. Maak een LiveEvent.
2. Maak een nieuwe Asset-object.
3. Maak een LiveOutput en gebruik van de assetnaam van de die u hebt gemaakt.
4. Maak een Streaming-beleid en de sleutel van de inhoud als u van plan bent voor het versleutelen van uw inhoud met DRM.
5. Als dat niet met DRM, maken van een Streaming-Locator gemaakt met de ingebouwde Streaming beleidstypen.
6. Lijst van de paden op het Streaming-beleid om terug te gaan de URL's te gebruiken (dit zijn deterministische).
7. De hostnaam voor de Streaming-eindpunt u streamen wilt vanaf ophalen. 
8. De URL uit stap 6 worden gecombineerd met de hostnaam in stap 7 om de volledige URL te achterhalen.

Zie voor meer informatie, een [Live streaming zelfstudie](stream-live-tutorial-with-api.md) die is gebaseerd op de [Live .NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live) voorbeeld.

## <a name="next-steps"></a>Volgende stappen

- [LiveEvent typen vergelijking](live-event-types-comparison.md)
- [Statussen en facturering](live-event-states-billing.md)
- [Latentie](live-event-latency.md)

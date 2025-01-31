---
title: Overzicht van Live streamen met Azure Media Services v3 | Microsoft Docs
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
ms.date: 06/16/2019
ms.author: juliako
ms.openlocfilehash: 0abc3eec380cccae2672d0e9aa4a3a4c7199362f
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295663"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Live streamen met Azure Media Services v3

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Om te streamen met Media Services uw live-evenementen, moet u het volgende:  

- Een camera die wordt gebruikt om vast te leggen van de live-gebeurtenis.<br/>Bekijk voor setup ideeën [video versnelling met eenvoudige en draagbare gebeurtenisinstelling]( https://link.medium.com/KNTtiN6IeT).

    Als u geen toegang tot een camera, hulpprogramma's zoals [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) kan worden gebruikt voor het genereren van een live feed uit een videobestand.
- Een coderingsprogramma voor live video dat signalen vanaf een camera (of een ander apparaat, zoals een laptop) converteert naar een bijdrage feed die wordt verzonden naar Media Services. De feed bijdrage kan signalen met betrekking tot reclame, zoals SCTE 35 markeringen bevatten.<br/>Zie voor een lijst met aanbevolen live streaming-coderingsprogramma's, [live coderingsprogramma's streamen](recommended-on-premises-live-encoders.md). Controleer ook of deze blog bekijken: [Live streaming productie met IB](https://link.medium.com/ttuwHpaJeT).
- Onderdelen in Media Services, waarmee u om op te nemen, preview, inpakken, registreren, versleutelen en uitzenden van de live-gebeurtenis aan uw klanten of aan een CDN voor verdere distributie.

In dit artikel biedt een overzicht en richtlijnen voor live streamen met Media Services en koppelingen naar andere relevante artikelen.
 
> [!NOTE]
> U kunt momenteel geen gebruik maken van de Azure-portal om v3-resources te beheren. Gebruik de [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) of een van de ondersteunde [SDK's](media-services-apis-overview.md#sdks).

## <a name="dynamic-packaging"></a>Dynamische verpakking

Met Media Services, kunt u profiteren van [dynamische pakketten](dynamic-packaging-overview.md), waarmee u preview uit te zenden van uw live streams in [MPEG DASH, HLS en Smooth Streaming-indelingen](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) van de bijdrage feed die wordt verzonden naar de service. De doelgroep kunnen de live stream met een compatibele spelers HLS, DASH of Smooth Streaming worden afgespeeld. U kunt [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) in uw web- of mobiele toepassingen voor het leveren van uw stroom in een van deze protocollen.

## <a name="dynamic-encryption"></a>Dynamische versleuteling

Dynamische versleuteling kunt u uw inhoud live of on-demand met AES-128 of een van de drie belangrijkste digital rights management (DRM)-systemen dynamisch wordt versleuteld: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients. Zie voor meer informatie, [dynamische versleuteling](content-protection-overview.md).

## <a name="dynamic-manifest"></a>Dynamische Manifest

Dynamische filtering is gebruikt om te bepalen het aantal sporen te wissen, indelingen bitsnelheden en presentatie tijdvensters die worden verzonden naar de spelers. Zie voor meer informatie, [filters en dynamische manifesten](filters-dynamic-manifest-overview.md).

## <a name="live-event-types"></a>Live gebeurtenistypen

[Livegebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) zijn verantwoordelijk voor het opnemen en verwerken van de live videofeeds. Een Live-gebeurtenis kan een van twee typen zijn: Pass Through- en live codering. Zie voor meer informatie over live streamen in Media Services v3 [Live evenementen en Live uitvoer](live-events-outputs-concept.md).

### <a name="pass-through"></a>Pass-through

![passthrough](./media/live-streaming/pass-through.svg)

Bij het gebruik van de pass-through **Live gebeurtenis**, afhankelijk van uw on-premises live coderingsprogramma meerdere bitrate videostream genereren en verzenden dat er als de bijdrage feed aan de Live-gebeurtenis (met behulp van invoerprotocol RTMP- of gefragmenteerde MP4). De Live gebeurtenis wordt vervolgens via de binnenkomende video stromen naar de dynamische pakketbouwer (Streaming-eindpunt) zonder eventuele verdere transcodering. Dergelijke een Pass Through-Live-gebeurtenis is geoptimaliseerd voor langlopende live gebeurtenissen of 24 x lineair 365 live streamen. 

### <a name="live-encoding"></a>Live Encoding  

![live encoding](./media/live-streaming/live-encoding.svg)

Bij het gebruik van cloud met Media Services encoding, configureert u uw on-premises live coderingsprogramma voor het verzenden van een single-bitrate video als de bijdrage feed (maximaal 32Mbps cumulatieve) aan de Live-gebeurtenis (met behulp van invoerprotocol RTMP- of gefragmenteerde MP4). De Live gebeurtenis transcodeert de binnenkomende single-bitrate stream naar [meerdere bitrate video stromen](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) bij verschillende resoluties voor het verbeteren van levering en maakt ze beschikbaar zijn voor levering aan apparaten via standaardprotocollen afspelen zoals MPEG-DASH, Apple HTTP Live Streaming (HLS) en Microsoft Smooth Streaming. 

## <a name="live-streaming-workflow"></a>Werkstroom voor live streaming

Voor meer informatie over de werkstroom voor live streaming in Media Services v3, moet u eerst controleren en begrijpen van de volgende concepten: 

- [Streaming-eindpunten](streaming-endpoint-concept.md)
- [Livegebeurtenissen en live-uitvoer](live-events-outputs-concept.md)
- [Streaming-locators](streaming-locators-concept.md)

### <a name="general-steps"></a>Algemene stappen

1. In Media Services-account, zorg ervoor dat de **Streaming-eindpunt** (oorsprong) wordt uitgevoerd. 
2. Maak een [Live evenement](live-events-outputs-concept.md). <br/>Bij het maken van de gebeurtenis, kunt u op automatisch starten het. U kunt ook kun u de gebeurtenis wanneer u klaar bent om te streamen.<br/> Wanneer autostart is ingesteld op true, wordt de Live gebeurtenis wordt gestart rechts nadat is gemaakt. De facturering begint zodra de Live gebeurtenis wordt gestart. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten. Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
3. De opname-URL's ophalen en uw on-premises coderingsprogramma voor het gebruik van de URL voor het verzenden van de bijdrage feed configureren.<br/>Zie [aanbevolen live coderingsprogramma's](recommended-on-premises-live-encoders.md).
4. De voorbeeld-URL ophalen en deze gebruiken om te controleren dat de invoer van het coderingsprogramma daadwerkelijk worden ontvangen.
5. Maak een nieuwe **Asset** object.
6. Maak een **uitvoer Live** en gebruikt u de assetnaam van de die u hebt gemaakt.<br/>De **uitvoer Live** worden gearchiveerd de stroom in de **Asset**.
7. Maak een **Streaming-Locator gemaakt** met de [ingebouwde Streaming beleidstypen](streaming-policy-concept.md)
8. Lijst van de paden op de **Streaming-Locator gemaakt** om terug te gaan de URL's te gebruiken (dit zijn deterministische).
9. Ophalen van de hostnaam voor de **Streaming-eindpunt** (oorsprong) die u wilt streamen uit.
10. De URL in stap 8 worden gecombineerd met de hostnaam in stap 9 om op te halen van de volledige URL.
11. Als u wilt stoppen met het maken van uw **Live gebeurtenis** zichtbaar, dat u wilt stoppen met het streamen van de gebeurtenis en verwijderen de **Streaming-Locator gemaakt**.

## <a name="other-important-articles"></a>Andere belangrijke artikelen

- [Aanbevolen live coderingsprogramma 's](recommended-on-premises-live-encoders.md)
- [Een cloud-DVR gebruiken](live-event-cloud-dvr.md)
- [Live gebeurtenistypen vergelijking van functies](live-event-types-comparison.md)
- [Statussen en facturering](live-event-states-billing.md)
- [Latentie](live-event-latency.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)
* [Hulp bij de migratie voor het verplaatsen van Media Services v2 naar v3](migrate-from-v2-to-v3.md)

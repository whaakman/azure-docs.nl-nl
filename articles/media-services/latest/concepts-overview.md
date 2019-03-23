---
title: Concepten van Azure Media Services - Azure | Microsoft Docs
description: In dit onderwerp biedt een kort overzicht van Azure Media Services-concepten en koppelingen voor meer informatie.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d8790eac93b288d5d5254f188fe5c901b5d5df14
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351487"
---
# <a name="media-services-concepts"></a>Media Services-concepten

Dit onderwerp biedt een kort overzicht van Azure Media Services-concepten en bevat koppelingen naar artikelen met gedetailleerde uitleg van Media Services v3-concepten en functies. Bekijk de basisconcepten die worden beschreven in de volgende onderwerpen voordat u start met de ontwikkeling.

> [!NOTE]
> U kunt de Azure-portal op dit moment niet gebruiken om v3-resources te beheren. Gebruik de [REST-API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref), of een van de ondersteunde [SDK's](developers-guide.md).

## <a name="cloud-upload-and-storage"></a>Uploaden naar en opslaan in de cloud

Als u wilt beheren, coderen, codering, analyseren en streaming van media-inhoud in Azure starten, moet u een Media Services-account maken en uploaden van uw digitale bestanden naar **activa**.

- [Uploaden van de cloud en opslag](storage-account-concept.md)
- [Activa-concept](assets-concept.md)

## <a name="encoding"></a>Encoding

Nadat u uw hoogwaardige digitale media-bestanden naar Assets uploaden, kunt u ze coderen in indelingen die kunnen worden afgespeeld op een groot aantal browsers en apparaten. 

Als u wilt coderen met Media Services v3, moet u maken **transformeert** en **taken**.

![Transformaties](./media/encoding/transforms-jobs.png)

- [Transformaties en taken](transforms-jobs-concept.md)
- [Codering met mediaservices](encoding-concept.md)

## <a name="media-analytics"></a>Media-analyses

Voor het analyseren van uw video en audio-bestanden, moet u ook maken **transformeert** en **taken**.

- [Video-en audiobestanden analyseren](analyzing-video-audio-files-concept.md)

## <a name="packaging-delivery-protection"></a>Verpakking, levering, beveiliging

Zodra uw inhoud wordt gecodeerd, kunt u profiteren van **dynamische verpakking**. **Streaming-eindpunt** is de service voor dynamische pakketten in Media Services gebruikt voor het leveren van media-inhoud naar client spelers. Video's in de uitvoer als beschikbaar wilt maken voor clients om te worden afgespeeld, u moet maken een **Streaming-Locator gemaakt** en bouw vervolgens de streaming-URL's. 

Bij het maken van de **Streaming-Locator gemaakt**, naast de assetnaam, moet u om op te geven **Streaming beleid**. **Beleid voor streaming** kunt u voor het definiëren van protocollen voor streaming en versleuteling opties (indien aanwezig) voor uw **Streaming-Locators**.

Dynamische pakketten wordt gebruikt of u uw inhoud live of on-demand streamen. Het volgende diagram toont de streaming on demand met dynamische verpakking werkstroom.

![Dynamische verpakking](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

Met Media Services van uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) kan worden geleverd of / en een van de drie belangrijkste digital rights management (DRM)-systemen: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.

Als versleutelingsopties voor uw stroom op te geven, maakt u de **inhoud sleutel beleid** en koppel deze aan uw **Streaming-Locator gemaakt**. De **inhoud sleutel beleid** kunt u configureren hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients.

De volgende afbeelding ziet u de Media Services content protection-werkstroom: 

![Inhoud beveiligen](./media/content-protection/content-protection.svg)

&#42;dynamische versleuteling ondersteunt AES-128 "clear key', CBCS en CENC. 

U kunt Media Services gebruiken **dynamische manifesten** alleen een specifieke weergave of subclips van uw video te streamen. In het volgende voorbeeld is een coderingsprogramma gebruikt een tussentijds asset coderen in zeven ISO MP4s video voorinstelling (van 180p 1080p). De gecodeerde asset kan dynamisch worden verpakt in een van de volgende protocollen voor streaming: HLS, MPEG DASH en Smooth.  Aan de bovenkant van het diagram, het HLS-manifest voor de asset met geen filters wordt weergegeven (bevat alle zeven voorinstelling).  In de linksonder, wordt het HLS-manifest waarop een filter met de naam "ott" is toegepast weergegeven. Het filter "ott" Hiermee geeft u als u wilt verwijderen van alle bitsnelheden hieronder 1 Mbps, wat leidde tot de twee quality-niveaus van onder, worden verwijderd uit in het antwoord. In de rechts onderaan wordt het HLS-manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Het filter 'mobiel' Hiermee geeft u het verwijderen van vertoningen waar de oplossing is groter dan 720p, wat leidde tot de twee 1080p voorinstelling wordt overblijft.

![Weergavefiltering](./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png)

- [Dynamische pakketten](dynamic-packaging-overview.md)
- [Streaming-eindpunten](streaming-endpoint-concept.md)
- [Streaming-Locators](streaming-locators-concept.md)
- [Beleid voor streaming](streaming-policy-concept.md)
- [Beleid voor inhoud sleutels](content-key-policy-concept.md)
- [Beveiliging van inhoud](content-protection-overview.md)
- [Dynamische manifesten](filters-dynamic-manifest-overview.md)
- [Filters](filters-concept.md)

## <a name="live-streaming"></a>Live streamen

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. **Live gebeurtenissen** zijn verantwoordelijk voor het opnemen en verwerken van de live-videofeeds. Bij het maken van een **Live gebeurtenis**, een invoereindpunt waarmee u kunt een live signaal verzenden vanaf een externe coderingsprogramma wordt gemaakt. Zodra u de stroom doorgestuurd hebt naar de **Live gebeurtenis**, kunt u de streaminggebeurtenis starten door het maken van een **Asset**, **uitvoer Live**, en **Streaming-Locator gemaakt** . **Live uitvoer** worden gearchiveerd de stroom in de **Asset** en het beschikbaar maken voor gebruikers via de **Streaming-eindpunt**. Een **Live gebeurtenis** kan bestaan uit een van de twee typen: **pass-through** en **van realtime codering**.

De volgende afbeelding illustreert de werkstroom van Pass Through-type:

![Pass Through-query](./media/live-streaming/pass-through.svg)

- [Overzicht van live streaming](live-streaming-overview.md)
- [Live-evenementen en Live-uitvoer](live-events-outputs-concept.md)

## <a name="monitoring"></a>Bewaking

### <a name="event-grid"></a>Event Grid

Als u wilt zien van de voortgang van de taak, moet u **Event Grid**. Media Services verzendt ook de Live-gebeurtenis-typen. Met Event Grid kunnen uw apps luisteren naar en reageren op gebeurtenissen uit vrijwel alle Azure-services, evenals aangepaste bronnen. 

- [Event Grid-gebeurtenissen verwerken](reacting-to-media-services-events.md)
- [Schema 's](media-services-event-schemas.md)
- [Diagnostische logboeken en metrische gegevens](media-services-metrics-diagnostic-logs.md)
- [Schema's voor diagnostische logboeken](media-services-diagnostic-logs-schema.md)

## <a name="player-clients"></a>Afspeel-clients

U kunt Azure Media Player gebruiken voor het afspelen van media-inhoud gestreamd door Media Services op een groot aantal browsers en apparaten. Azure Media Player maakt gebruik van industriële standaarden, zoals HTML5, Media bron-extensies (MSE) en Encrypted Media Extensions (EME) voor een geavanceerde adaptieve streamingervaring. 

- [Overzicht van Azure Media Player](use-azure-media-player.md)

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen en streamen met Media Services](stream-files-tutorial-with-api.md)

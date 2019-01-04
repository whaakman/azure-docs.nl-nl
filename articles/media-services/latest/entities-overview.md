---
title: Overzicht van entiteiten voor Azure Media Services - Azure | Microsoft Docs
description: In dit artikel biedt een overzicht van Azure Media Services-entiteiten.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 1d309bb550282d5245a2fbf74f14a931cf5c2279
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53746032"
---
# <a name="azure-media-services-entities-overview"></a>Overzicht van Azure Media Services-entiteiten

Dit artikel geeft een kort overzicht van Azure Media Services-entiteiten en verwijst naar een artikel voor meer informatie over elke entiteit. 

| Onderwerp | Description |
|---|---|
| [Accountfilters en filters actief](filters-dynamic-manifest-overview.md)|Wanneer uw inhoud levert aan klanten (streaming Live gebeurtenissen of Video on Demand) is de client mogelijk meer flexibiliteit dan wat wordt beschreven in het manifestbestand van de standaard-Asset. Azure Media Services kunt u definiëren [Accountfilters](https://docs.microsoft.com/rest/api/media/accountfilters) en [Asset Filters](https://docs.microsoft.com/rest/api/media/assetfilters). Vervolgens gebruikt u **dynamische manifesten** op basis van de vooraf gedefinieerde filters. |
| [Activa](assets-concept.md)|Een [Asset](https://docs.microsoft.com/rest/api/media/assets) entiteit digitale bestanden (met inbegrip van de video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden-bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een Asset, kunnen ze worden gebruikt in de Media Services, codering, streaming, inhoud werkstromen analyseren.|
| [Beleid voor inhoud sleutels](content-key-policy-concept.md)|U kunt Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.|
| [LiveEvents en LiveOutputs](live-events-outputs-concept.md)|Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. Als u wilt uw live streaming-gebeurtenissen in Media Services v3 configureert, moet u meer informatie over [Live gebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) en [uitvoer Live](https://docs.microsoft.com/rest/api/media/liveoutputs).|
| [Streaming-eindpunten](streaming-endpoint-concept.md)|Een [Streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints) entiteit vertegenwoordigt een streamingservice waarmee inhoud kan worden geleverd rechtstreeks naar een clientafspeeltoepassing of aan een Content Delivery Network (CDN) voor verdere distributie. De uitgaande stroom van de service van een Streaming-eindpunt mag bestaan uit een live stream of een video op aanvraag actief in Media Services-account. Wanneer u een Media Services-account, maakt een **standaard** Streaming-eindpunt is voor u gemaakt in een status ' gestopt '. U kunt niet verwijderen de **standaard** Streaming-eindpunt. Aanvullende Streaming-eindpunten kunnen worden gemaakt onder het account. Om video's wilt streamen, moet u de Streaming-eindpunt van waaruit u atream uw video wilt te starten. |
| [Streaming-Locators](streaming-locators-concept.md)|U moet uw clients met een URL die kan worden gebruikt om af te spelen gecodeerde video of audio-bestanden opgeven, moet u maken een [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators) en de streaming-URL's maken.|
| [Beleid voor streaming](streaming-policy-concept.md)| [Beleid voor streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) inschakelen u protocollen voor streaming- en versleutelingsopties voor uw StreamingLocators definiëren. U kunt de naam opgeven van een aangepast beleid Streaming u hebt gemaakt of gebruik een van de vooraf gedefinieerde Streaming beleidsregels die worden aangeboden door mediaservices. <br/><br/>Wanneer u een aangepast beleid voor Streaming, moet u een beperkte set van dergelijk beleid ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer de dezelfde versleutelingsopties en protocollen die nodig zijn. U moet niet worden het maken van een nieuw beleid voor Streaming voor elke Streaming-Locator gemaakt.|
| [Transformaties en taken](transforms-jobs-concept.md)|Gebruik [transformeert](https://docs.microsoft.com/rest/api/media/transforms) het configureren van algemene taken voor het coderen of video's analyseren. Elke **transformeren** beschrijft een recept of een workflow van taken voor het verwerken van uw video- of audio-bestanden.<br/><br/>Een [taak](https://docs.microsoft.com/rest/api/media/jobs) is van de werkelijke aanvraag voor Azure Media Services om toe te passen de **transformeren** aan een bepaalde invoer video of audio-inhoud. De **taak** bevat informatie zoals de locatie van de video-invoer en de locatie voor de uitvoer. U kunt de locatie van uw video met invoer opgeven: HTTPS-URL's, SAS-URL's of een Asset.|

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)

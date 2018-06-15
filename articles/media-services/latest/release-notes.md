---
title: Azure Media Services v3 release-opmerkingen | Microsoft Docs
description: Om te blijven up-to-date blijven met de meest recente ontwikkelingen, biedt in dit artikel u de meest recente updates op Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: fc6c5ba6cd97c261dd44eade33bf21e8d1b74bf0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788114"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Azure Media Services v3 releaseopmerkingen (preview) 

Om te blijven up-to-date blijven met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

* De meest recente versies
* Bekende problemen
* Oplossingen voor problemen
* Afgeschafte functionaliteit
* Plannen voor wijzigingen

## <a name="may-07-2018"></a>07 mei 2018

### <a name="net-sdk"></a>.NET SDK

De volgende functies zijn aanwezig in de .net SDK:

1. **Transformeert** en **taken** coderen of media-inhoud analyseren. Zie voor voorbeelden [Stream bestanden](stream-files-tutorial-with-api.md) en [analyseren](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** voor het publiceren en streaming-inhoud aan eindgebruikers apparaten
3. **StreamingPolicies** en **ContentKeyPolicies** sleutellevering en beveiliging van inhoud (DRM) configureren bij het leveren van inhoud.
4. **LiveEvents** en **LiveOutputs** voor het configureren van de opnemen en archivering van live streaming inhoud.
5. **Activa** op te slaan en media-inhoud publiceren in Azure Storage. 
6. **Streaming-eindpunten** configureren en schalen van dynamische pakketten, versleuteling en streaming voor live en on-demand media-inhoud.

### <a name="known-issues"></a>Bekende problemen

Bekende problemen:

Bij het indienen van een taak met een HTTPS-URL (JobInputHttp) die verwijst naar de bron van de inhoud, zorg er dan voor dat de HTTP-server biedt ondersteuning voor de aanvraag 'HEAD'. Anders wordt wordt de taak geweigerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht](media-services-overview.md)

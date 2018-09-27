---
title: Azure Media Services v3 opmerkingen bij de release | Microsoft Docs
description: Als u wilt bijhouden met de meest recente ontwikkelingen, biedt in dit artikel u de meest recente updates voor Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/25/2018
ms.author: juliako
ms.openlocfilehash: ed2550c1df4645933fb968c54ee536995c810136
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219321"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Azure Media Services v3 (preview) release-opmerkingen 

Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

* De meest recente versies
* Bekende problemen
* Opgeloste fouten
* Afgeschafte functies
* Plannen voor wijzigingen

## <a name="may-07-2018"></a>07 mei 2018

### <a name="net-sdk"></a>.NET SDK

De volgende functies zijn aanwezig in de .net SDK:

1. **Transformeert** en **taken** om te coderen of media-inhoud analyseren. Zie voor voorbeelden van [Stream bestanden](stream-files-tutorial-with-api.md) en [analyseren](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** voor het publiceren en streaming van inhoud naar apparaten van eindgebruikers
3. **StreamingPolicies** en **ContentKeyPolicies** sleutellevering en beveiliging van inhoud (DRM) configureren bij het leveren van inhoud.
4. **LiveEvents** en **LiveOutputs** om de opname- en archivering van live streaming-inhoud te configureren.
5. **Activa** opslaan en publiceren van media-inhoud in Azure Storage. 
6. **Door** configureren en schalen van dynamische pakketten, versleuteling en streaming voor live en on-demand media-inhoud.

### <a name="known-issues"></a>Bekende problemen

* Bij het indienen van een taak, kunt u opgeven voor opname van de bronvideo met behulp van HTTPS-URL's, SAS-URL's of paden tot bestanden in Azure Blob-opslag. AMS v3 biedt op dit moment geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht](media-services-overview.md)

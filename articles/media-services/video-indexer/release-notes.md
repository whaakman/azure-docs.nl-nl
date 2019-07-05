---
title: Releaseopmerkingen voor Azure Media Services Video Indexer | Microsoft Docs
description: Als u wilt bijhouden met de meest recente ontwikkelingen, biedt in dit artikel u de meest recente updates voor Azure Media Services Video Indexer.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: f1c5f43316292f17547b84d970a0f03a1a2c366f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454020"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Releaseopmerkingen voor Azure Media Services Video Indexer

Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

* De meest recente versies
* Bekende problemen
* Opgeloste fouten
* Afgeschafte functies

## <a name="june-2019"></a>Juni 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer geïmplementeerd in Japan-Oost

U kunt nu een Video Indexer-account in de regio Japan-Oost betaalde maken.

### <a name="create-and-repair-account-api-preview"></a>Maken en herstellen van de account-API (Preview)

Een nieuwe API waarmee u toegevoegd [bijwerken van het verbindingseindpunt van Azure Media Service of de sleutel](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Fout bij het afhandelen van uploaden verbeteren 

Een beschrijving weergegeven wordt in het geval van onjuiste configuratie van de onderliggende Azure Media Services-account geretourneerd.

### <a name="player-timeline-keyframes-preview"></a>Player tijdlijn hoofdframes preview 

U ziet nu een voorbeeld van afbeelding voor elk tijdstip op de tijdlijn van de speler.

### <a name="editor-semi-select"></a>Editor semi select

U ziet nu een Preview-versie van de inzichten die zijn geselecteerd als gevolg van het kiezen van een tijdskader specifieke inzicht in de editor.

## <a name="may-2019"></a>Mei 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Aangepaste taalmodel van ondertitelingsbestand bijwerken

[Maken van aangepaste taalmodel](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) en [aangepaste taalmodellen bijwerken](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) API's bieden nu ondersteuning voor VTT, SRT, en TTML-bestandsindelingen als invoer voor de taalmodellen.

Bij het aanroepen van de [Update Video transcript API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript), het transcript wordt automatisch toegevoegd. De training-model dat is gekoppeld aan de video wordt ook automatisch bijgewerkt. Zie voor meer informatie over het aanpassen en uw taalmodellen trainen [aanpassen van een taalmodel met Video Indexer](customize-language-model-overview.md).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Nieuwe downloaden transcript indelingen: TXT en CSV

Naast de gesloten closed captioning indeling al ondersteund (SRT, VTT en TTML), Video Indexer biedt nu ondersteuning voor het downloaden van het transcript in een TXT- en CSV-indeling.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](video-indexer-overview.md)

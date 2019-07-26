---
title: Release opmerkingen bij Azure Media Services Video Indexer | Microsoft Docs
description: Om up-to-date te blijven met de meest recente ontwikkelingen, biedt dit artikel u de meest recente updates op Azure Media Services Video Indexer.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 07/22/2019
ms.author: juliako
ms.openlocfilehash: fd67e867b5eac58838551ac29e1fca5e860b15ff
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414213"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Release opmerkingen bij Azure Media Services Video Indexer

Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

* De meest recente versies
* Bekende problemen
* Opgeloste fouten
* Afgeschafte functies

## <a name="june-2019"></a>Juni 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer geïmplementeerd in Japan-Oost

U kunt nu een Video Indexer betaalde account maken in de regio Japan Oost.

### <a name="create-and-repair-account-api-preview"></a>Account-API maken en herstellen (preview-versie)

Er is een nieuwe API toegevoegd waarmee u [het eind punt of de sleutel van de Azure media service-verbinding kunt bijwerken](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Fout afhandeling bij uploaden verbeteren 

Er wordt een beschrijvende bericht weer gegeven in geval van een onjuiste configuratie van het onderliggende Azure Media Services-account.

### <a name="player-timeline-keyframes-preview"></a>Voor beeld van tijdlijn hoofdframes in de speler 

U kunt nu een voor beeld van een afbeelding zien voor elke keer op de tijd lijn van de speler.

### <a name="editor-semi-select"></a>De editor semi-Select

U kunt nu een voor beeld zien van alle inzichten die zijn geselecteerd als gevolg van het kiezen van een specifieke inzichten periode in de editor.

## <a name="may-2019"></a>Mei 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Aangepast taal model bijwerken vanuit ondertitelings bestand

[Aangepaste taal model maken](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) en [aangepaste taal modellen bijwerken](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) api's bieden nu ondersteuning voor VTT-, SRT-en TTML-bestands indelingen als invoer voor taal modellen.

Bij het aanroepen van de [Update video transcriptie-API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript)wordt de transcripten automatisch toegevoegd. Het trainings model dat is gekoppeld aan de video wordt automatisch bijgewerkt. Zie [een taal model aanpassen met video indexer](customize-language-model-overview.md)voor meer informatie over het aanpassen en trainen van uw taal modellen.

### <a name="new-download-transcript-formats--txt-and-csv"></a>Nieuwe indelingen voor het downloaden van transcripten – TXT en CSV

Naast de indeling voor ondertiteling die al wordt ondersteund (SRT, VTT en TTML), ondersteunt Video Indexer nu het downloaden van transcripten in TXT-en CSV-indelingen.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](video-indexer-overview.md)

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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: db68f979239a5783338d99360209ae231a75c936
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945032"
---
# <a name="azure-media-services-v3-release-notes"></a>Azure Media Services v3 release-opmerkingen 

Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

* De meest recente versies
* Bekende problemen
* Opgeloste fouten
* Afgeschafte functies
* Plannen voor wijzigingen

## <a name="october-2018---ga"></a>Oktober 2018 - GA

Deze sectie beschrijft de updates van oktober van Azure Media Services (AMS).

### <a name="rest-v3-ga-release"></a>REST v3 GA-versie

De [REST v3 GA-versie](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) bevat meer API's voor Live, filters op niveau manifest van Account/Asset en DRM-ondersteuning.

#### <a name="azure-resource-management"></a>Azure Resources Management 

Ondersteuning voor Azure Resource Management maakt geïntegreerd beheer en bewerkingen API (nu alles op één plek).

Vanaf deze versie, kunt u Resource Manager-sjablonen Live gebeurtenissen maken.

#### <a name="improvement-of-asset-operations"></a>Verbetering van de Asset bewerkingen 

De volgende verbeteringen zijn geïntroduceerd:

- Van HTTP (s) URL's of Azure Blob Storage SAS-URL's opnemen.
- Geef u de namen van de eigen containers voor activa. 
- Eenvoudiger uitvoer-ondersteuning voor het maken van aangepaste werkstromen met Azure Functions.

#### <a name="new-transform-object"></a>Nieuwe transformatie-object

De nieuwe **transformeren** object vereenvoudigt het Encoding-model. Het nieuwe object kunt eenvoudig maken en delen codering Resource Manager-sjablonen en standaardinstellingen. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Active Directory-verificatie en RBAC

Azure AD-verificatie en toegangsbeheer op basis van rollen (RBAC) inschakelen beveiligde transformaties, LiveEvents, beleid voor inhoud sleutels of activa op rol of gebruikers in Azure AD.

#### <a name="client-sdks"></a>Client-SDK 's  

Ondersteunde talen in Media Services v3: .NET Core, Java, Node.js, Ruby, Typescript, Python, Go.

#### <a name="live-encoding-updates"></a>Live encoding updates

De volgende live encoding updates worden geïntroduceerd:

- Nieuwe modus voor lage latentie voor live (10 seconden end-to-end).
- Verbeterde RTMP-ondersteuning (verbeterde stabiliteit en meer bron encoder-ondersteuning).
- Beveiligde RTMPS opnemen.

    Wanneer u een LiveEvent maakt, u nu get 4 URL's voor opnemen. Opname van de 4 URL's zijn bijna identiek, hetzelfde streaming-token (AppId), alleen de poort nummer onderdeel verschilt. Twee van de URL's zijn primaire en back-up voor RTMPS. 
- transcodering van 24-uurs ondersteuning. 
- Verbeterde ondersteuning voor ad-signalering in RTMP via SCTE35.

#### <a name="improved-event-grid-support"></a>Verbeterde ondersteuning voor Event Grid

Hier ziet u de volgende Event Grid verbeteringen ondersteund:

- Integratie van Azure EventGrid voor eenvoudigere ontwikkeling met Logic Apps en Azure Functions. 
- Abonneren op gebeurtenissen voor codering, Live kanalen en meer.

### <a name="cmaf-support"></a>Ondersteuning voor CMAF

Ondersteuning voor Apple HLS (iOS 11 +) en MPEG-DASH CMAF en 'cbcs' spelers die ondersteuning bieden voor CMAF.

### <a name="video-indexer"></a>Video Indexer

Video Indexer-GA-versie is in augustus is aangekondigd. Zie voor nieuwe informatie over de momenteel ondersteunde functies [wat Video Indexer is](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Plannen voor wijzigingen

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
De Azure CLI 2.0-module die heeft ook betrekking op alle functies (met inbegrip van Live, inhoudsbeleid voor sleutels, Account/Asset Filters, Streaming-beleid) is binnenkort beschikbaar. 

### <a name="known-issues"></a>Bekende problemen

Alleen klanten die de preview-API voor Asset of AccountFilters gebruikt worden beïnvloed door het volgende probleem.

Als u activa of Accountfilters tussen 09/28 en 10 gemaakt/12 met Media Services v3 CLI of API's, moet u alle Asset en AccountFilters verwijderen en opnieuw gemaakt vanwege een versieconflict. 

## <a name="may-2018---preview"></a>Mei 2018 - Preview

### <a name="net-sdk"></a>.NET SDK

De volgende functies zijn aanwezig in de .net SDK:

* **Transformeert** en **taken** om te coderen of media-inhoud analyseren. Zie voor voorbeelden van [Stream bestanden](stream-files-tutorial-with-api.md) en [analyseren](analyze-videos-tutorial-with-api.md).
* **StreamingLocators** voor het publiceren en streaming van inhoud naar apparaten van eindgebruikers
* **StreamingPolicies** en **ContentKeyPolicies** sleutellevering en beveiliging van inhoud (DRM) configureren bij het leveren van inhoud.
* **LiveEvents** en **LiveOutputs** om de opname- en archivering van live streaming-inhoud te configureren.
* **Activa** opslaan en publiceren van media-inhoud in Azure Storage. 
* **Door** configureren en schalen van dynamische pakketten, versleuteling en streaming voor live en on-demand media-inhoud.

### <a name="known-issues"></a>Bekende problemen

* Bij het indienen van een taak, kunt u opgeven voor opname van de bronvideo met behulp van HTTPS-URL's, SAS-URL's of paden tot bestanden in Azure Blob-opslag. Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht](media-services-overview.md)

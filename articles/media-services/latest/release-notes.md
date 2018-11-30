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
ms.date: 11/21/2018
ms.author: juliako
ms.openlocfilehash: 598587a0fe726ccf65f062833f84b352ca03c077
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315525"
---
# <a name="azure-media-services-v3-release-notes"></a>Azure Media Services v3 release-opmerkingen 

Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

* De meest recente versies
* Bekende problemen
* Opgeloste fouten
* Afgeschafte functies
* Plannen voor wijzigingen

## <a name="known-issues"></a>Bekende problemen

> [!NOTE]
> U kunt de Azure-portal op dit moment niet gebruiken om v3-resources te beheren. Gebruik de [REST-API](https://aka.ms/ams-v3-rest-sdk), CLI of een van de ondersteunde SDK's.

Zie voor meer informatie, [migratierichtlijnen voor het verplaatsen van Media Services v2 naar v3](migrate-from-v2-to-v3.md#known-issues).

## <a name="november-2018"></a>November 2018

De CLI 2.0-module is nu beschikbaar voor [Azure Media Services v3 GA](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest) – v 2.0.50.

### <a name="new-commands"></a>Nieuwe CLI-opdrachten

- [AZ ams-account](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest)
- [AZ ams-account-filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)
- [AZ ams asset](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest)
- [AZ ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)
- [ams-inhoud-sleutel AZ-beleid](https://docs.microsoft.com/cli/azure/ams/content-key-policy?view=azure-cli-latest)
- [AZ ams-taak](https://docs.microsoft.com/cli/azure/ams/job?view=azure-cli-latest)
- [AZ ams live-gebeurtenis](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)
- [AZ ams live-uitvoer](https://docs.microsoft.com/cli/azure/ams/live-output?view=azure-cli-latest)
- [AZ ams streaming-eindpunt](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest)
- [AZ ams streaming-locator](https://docs.microsoft.com/cli/azure/ams/streaming-locator?view=azure-cli-latest)
- [ams AZ account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) -kunt u voor het beheren van gereserveerde Media-eenheden

### <a name="new-features-and-breaking-changes"></a>Nieuwe functies en wijzigingen die fouten veroorzaken

#### <a name="asset-commands"></a>Asset-opdrachten

- ```--storage-account``` en ```--container``` argumenten die zijn toegevoegd.
- Standaardwaarden voor vervaltijd (nu + 23 uur) en machtigingen (lezen) in ```az ams asset get-sas-url``` opdracht toegevoegd.

#### <a name="job-commands"></a>Taak opdrachten

- ```--correlation-data``` en ```--label``` argumenten die zijn toegevoegd
- ```--output-asset-names``` gewijzigd in ```--output-assets```. Nu de cmdlet accepteert een door spaties gescheiden lijst van activa in ' assetName label =' indeling. Een asset zonder label kan worden verzonden als volgt: ' assetName ='.

#### <a name="streaming-locator-commands"></a>Streaming-Locator opdrachten

- ```az ams streaming locator``` basis opdracht vervangen door ```az ams streaming-locator```.
- ```--streaming-locator-id``` en ```--alternative-media-id support``` argumenten die zijn toegevoegd.
- ```--content-keys argument``` het argument is bijgewerkt.
- ```--content-policy-name``` gewijzigd in ```--content-key-policy-name```.

#### <a name="streaming-policy-commands"></a>Streaming beleid-opdrachten

- ```az ams streaming policy``` basis opdracht vervangen door ```az ams streaming-policy```.
- Ondersteuning voor versleuteling parameters in ```az ams streaming-policy create``` toegevoegd.

#### <a name="transform-commands"></a>Opdrachten transformeren

- ```--preset-names``` argument vervangen door ```--preset```. Nu u alleen 1 uitvoer/voorinstelling tegelijk instellen kunt (om toe te voegen informatie u moet uitvoeren ```az ams transform output add```). U kunt ook aangepaste StandardEncoderPreset instellen door het pad naar uw aangepaste JSON.
- ```az ams transform output remove``` kan worden uitgevoerd door door te geven van de uitvoer-index te verwijderen.
- ```--relative-priority, --on-error, --audio-language and --insights-to-extract``` argumenten toegevoegd aan ```az ams transform create``` en ```az ams transform output add``` opdrachten.

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

[Overzicht](media-services-overview.md)

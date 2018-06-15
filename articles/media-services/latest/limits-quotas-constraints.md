---
title: Quota's en beperkingen in Azure Media Services v3 | Microsoft Docs
description: Dit onderwerp wordt beschreven, quota's en beperkingen in Azure Media Services v3
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
ms.openlocfilehash: 21fc80d7cb274197ae75d2fd5524e76e1e6288d9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788429"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Quota's en beperkingen in Azure Media Services v3

Dit onderwerp beschrijft de quota's en beperkingen in Azure Media Services v3.

| Resource | Standaardlimiet | 
| --- | --- | 
| Activa per Azure Media Services-account | 1.000.000|
| JobInputs per taak | 100 |
| JobOutputs per taak | 30 (vast) |
| Bestandsgrootte| In bepaalde situaties geldt er een limiet voor de maximale bestandsgrootte voor de verwerking van Media Services wordt ondersteund. <sup>(1)</sup> |
| Taken per Media Services-account | 50,000<sup>(2)</sup> |
| LiveEvents per Media Services-account |5|
| Media Services-accounts in een enkel abonnement | 25 (vast) |
| StreamingPolicies | 1.000.000<sup>(3)</sup> |
| LiveOutputs in uitvoeringstoestand per LiveEvent |3|
| LiveOutputs gestopt per LiveEvent |50|
| Opslagaccounts | 1000<sup>(4)</sup> (opgelost) |
| Streaming-eindpunten in uitvoeringstoestand per Media Services-account|2|
| Transformaties per Media Services-account | 20 |
| Unieke StreamingLocators die tegelijk zijn gekoppeld aan een Asset | 20<sup>(5)</sup> |
  
<sup>1</sup>de maximale grootte voor één blob momenteel maximaal 5 TB in Azure Blob-opslag is ondersteund. Aanvullende limieten gelden echter in Azure Media Services op basis van de VM-grootten die worden gebruikt door de service. Als het bronbestand groter dan 260 GB is mislukt uw taak waarschijnlijk. Hebt u 4K-inhoud die groter is dan de limiet van 260 GB, contact met ons op amshelp@microsoft.com voor mogelijke oplossingen voor de ondersteuning van uw scenario.

<sup>2</sup> dit aantal bevat in de wachtrij, voltooide, actieve en geannuleerde taken. Dit omvat geen verwijderde taken. 

Een record van de taak in uw account ouder is dan 90 dagen worden, automatisch verwijderd, zelfs als het totale aantal records lager dan de maximale quota is. 

<sup>3</sup> er geldt een limiet van 1.000.000 StreamingPolicy vermeldingen voor verschillende Media Services-beleidsregels (bijvoorbeeld voor StreamingLocator beleid of ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Gebruik dezelfde beleids-id als u altijd dezelfde dagen/toegangsmachtigingen/enzovoort gebruikt. 

<sup>4</sup> de storage-accounts moet van hetzelfde Azure-abonnement.

<sup>5</sup> StreamingLocators zijn niet ontworpen voor het beheren van toegangsbeheer per gebruiker. Gebruik DRM-oplossingen (Digital Rights Management) als u afzonderlijke gebruikers verschillende toegangsrechten wilt geven.

## <a name="support-ticket"></a>Ondersteuningsticket

Voor bronnen die niet worden opgelost, kan u vragen voor de quota wordt gegenereerd door het openen van een [ondersteunen ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Neem de gedetailleerde informatie in de aanvraag op de gewenste quotum wijzigingen, use case-scenario's en regio's die zijn vereist. <br/>Maak **geen** extra Azure Media Services-accounts in een poging om de limieten te verhogen.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](media-services-overview.md)

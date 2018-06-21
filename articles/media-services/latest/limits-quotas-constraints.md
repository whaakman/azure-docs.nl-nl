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
ms.date: 06/13/2018
ms.author: juliako
ms.openlocfilehash: 14779306815681c368a98d698a6688d528a6c747
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294026"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Quota's en beperkingen in Azure Media Services v3

Dit artikel wordt beschreven, quota's en beperkingen in Azure Media Services v3.

| Resource | Standaardlimiet | 
| --- | --- | 
| Activa per Azure Media Services-account | 1.000.000|
| JobInputs per taak | 50 (opgelost)|
| JobOutputs per taak/TransformOutputs in een transformatie | 20 (opgelost) |
| Bestanden per JobInput|10 (opgelost)|
| Bestandsgrootte| In bepaalde situaties geldt er een limiet voor de maximale bestandsgrootte voor de verwerking van Media Services wordt ondersteund. <sup>(1)</sup> |
| Taken per Media Services-account | 500.000 <sup>(2)</sup> (opgelost)|
| Aanbieding transformaties|Het antwoord, met 1000 transformaties per pagina pagineren|
| Taken weergeven|Het antwoord, met 500 taken per pagina pagineren|
| LiveEvents per Media Services-account |5|
| Media Services-accounts in een enkel abonnement | 25 (vast) |
| StreamingPolicies | 1.000.000<sup>(3)</sup> |
| LiveOutputs in uitvoeringstoestand per LiveEvent |3|
| LiveOutputs gestopt per LiveEvent |50|
| Opslagaccounts | 100<sup>(4)</sup> (opgelost) |
| Streaming-eindpunten in uitvoeringstoestand per Media Services-account|2|
| Transformaties per Media Services-account | 100 (opgelost)|
| Unieke StreamingLocators die tegelijk zijn gekoppeld aan een Asset | 20<sup>(5)</sup> |

<sup>1</sup> de maximale grootte voor één blob momenteel maximaal 5 TB in Azure Blob-opslag is ondersteund. Aanvullende limieten gelden echter in Azure Media Services op basis van de VM-grootten die worden gebruikt door de service. Als het bronbestand groter dan 260 GB is, mislukt de taak waarschijnlijk. Hebt u 4K-inhoud die groter is dan de limiet van 260 GB, contact met ons op amshelp@microsoft.com voor mogelijke oplossingen voor de ondersteuning van uw scenario.

<sup>2</sup> dit aantal bevat in de wachtrij, voltooide, actieve en geannuleerde taken. Dit omvat geen verwijderde taken. 

Een record van de taak in uw account ouder is dan 90 dagen worden, automatisch verwijderd, zelfs als het totale aantal records lager dan de maximale quota is. 

<sup>3</sup> wanneer u een aangepaste [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), moet u een beperkte set van dergelijk beleid ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw StreamingLocators wanneer dezelfde versleuteling opties en protocollen nodig zijn. U dient geen nieuw StreamingPolicy voor elke StreamingLocator te maken.

<sup>4</sup> de storage-accounts moet van hetzelfde Azure-abonnement.

<sup>5</sup> StreamingLocators zijn niet ontworpen voor het beheren van toegangsbeheer per gebruiker. Gebruik DRM-oplossingen (Digital Rights Management) als u afzonderlijke gebruikers verschillende toegangsrechten wilt geven.

## <a name="support-ticket"></a>Ondersteuningsticket

Voor bronnen die niet worden opgelost, kan u vragen voor de quota wordt gegenereerd door het openen van een [ondersteunen ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Gedetailleerde informatie opnemen in de aanvraag op de gewenste quotum wijzigingen, use case-scenario's en regio's die zijn vereist. <br/>Maak **geen** extra Azure Media Services-accounts in een poging om de limieten te verhogen.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](media-services-overview.md)

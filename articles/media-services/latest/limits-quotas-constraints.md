---
title: Quota en beperkingen in Azure Media Services v3 | Microsoft Docs
description: Dit onderwerp wordt beschreven, quota en beperkingen in Azure Media Services v3
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/17/2018
ms.author: juliako
ms.openlocfilehash: b50ba825f675c84f551f9a1d191aa93eaed9a628
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070850"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Quota en beperkingen in Azure Media Services v3

Dit artikel wordt beschreven, quota en beperkingen in Azure Media Services v3.

| Resource | Standaardlimiet | 
| --- | --- | 
| Activa per Azure Media Services-account | 1.000.000|
| Dynamisch-Manifestfilters|100|
| JobInputs per taak | 50 (vast)|
| JobOutputs per taak/TransformOutputs in een transformatie | 20 (vast) |
| Bestanden per JobInput|10 (vast)|
| Bestandsgrootte| In sommige scenario's, moet u er een limiet is op de maximale bestandsgrootte voor verwerking in Media Services wordt ondersteund. <sup>(1)</sup> |
| Taken per Media Services-account | 500.000 <sup>(2)</sup> (vast)|
| Aanbieding transformaties|Het antwoord, met 1000 transformaties per pagina pagineren|
| Taken weergeven|Het antwoord, met 500 taken per pagina pagineren|
| LiveEvents per Media Services-account |5|
| Media Services-accounts in één abonnement | 25 (vast) |
| LiveOutputs status per LiveEvent actief |3|
| LiveOutputs gestopt per LiveEvent |50|
| Opslagaccounts | 100<sup>(4)</sup> (vast) |
| Streaming-eindpunten met de status per Media Services-account wordt uitgevoerd|2|
| StreamingPolicies | 100 <sup>(3)</sup> |
| Transformaties per Media Services-account | 100 (vast)|
| De unieke StreamingLocators die tegelijk zijn gekoppeld aan een Asset | 100<sup>(5)</sup> (vast) |

<sup>1</sup> de maximale grootte voor één blob momenteel maximaal 5 TB in Azure Blob Storage wordt wordt ondersteund. Aanvullende beperkingen gelden echter in Azure Media Services op basis van de VM-grootten die worden gebruikt door de service. Als het bronbestand groter dan 260 GB is, wordt waarschijnlijk uw taak mislukt. Als u 4K-inhoud die groter is dan de limiet van 260 GB hebt, contact met ons op amshelp@microsoft.com voor mogelijke oplossingen ter ondersteuning van uw scenario.

<sup>2</sup> hieronder vallen de in de wachtrij, voltooide, actieve en geannuleerde taken. Deze omvatten geen verwijderde taken. 

Elke taakrecord in uw account die ouder is dan 90 dagen, automatisch verwijderd, zelfs als het totale aantal records lager dan het maximale quotum is. 

<sup>3</sup> bij het gebruik van een aangepaste [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), moet u een beperkte set van dergelijk beleid ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw StreamingLocators wanneer dezelfde versleuteling opties en protocollen nodig zijn. U dient geen nieuw StreamingPolicy voor elke StreamingLocator te maken.

<sup>4</sup> de storage-accounts moeten afkomstig zijn van hetzelfde Azure-abonnement.

<sup>5</sup> StreamingLocators zijn niet ontworpen om toegangsbeheer per gebruiker beheren. Gebruik DRM-oplossingen (Digital Rights Management) als u afzonderlijke gebruikers verschillende toegangsrechten wilt geven.

## <a name="support-ticket"></a>Ondersteuningsticket

Voor de resources die niet zijn opgelost, kan u vraagt om de quota kunnen worden verhoogd door het openen van een [ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Gedetailleerde informatie opnemen in de aanvraag op de gewenste quota wijzigingen, use-casescenario's en regio's vereist. <br/>Maak **geen** extra Azure Media Services-accounts in een poging om de limieten te verhogen.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](media-services-overview.md)

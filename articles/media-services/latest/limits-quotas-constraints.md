---
title: Quota en beperkingen in Azure Media Services v3 | Microsoft Docs
description: Dit onderwerp wordt beschreven, quota en beperkingen in Azure Media Services v3
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/16/2019
ms.author: juliako
ms.openlocfilehash: 709ed293dbb0550dc1bb43bf1e1e1cc50906cc31
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293448"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Quota en beperkingen in Azure Media Services v3

Dit artikel wordt beschreven, quota en beperkingen in Azure Media Services v3.

| Resource | Standaardlimiet | 
| --- | --- | 
| Assets per Azure Media Services-account | 1\.000.000|
| Dynamisch-manifestfilters|100|
| JobInputs per taak | 50 (vast)|
| JobOutputs per taak | 20 (vast) |
| TransformOutputs in een transformatie | 20 (vast) |
| Bestanden per JobInput|10 (vast)|
| Bestandsgrootte| In sommige scenario's, moet u er een limiet is op de maximale bestandsgrootte voor verwerking in Media Services wordt ondersteund. <sup>(1)</sup> |
| Taken per Media Services-account | 500.000 <sup>(2)</sup> (vast)|
| Lijst met transformaties|Pagineer het antwoord met 1000 transformaties per pagina|
| Lijst met taken|Pagineer het antwoord met 500 taken per pagina|
| Live gebeurtenissen per Azure Media Services-account |5|
| Media Services-accounts in één abonnement | 25 (vast) |
| Live uitvoerwaarden per Live-gebeurtenis |3 <sup>(3)</sup> |
| Duur van de maximale Live uitvoer | 25 uur |
| Opslagaccounts | 100<sup>(4)</sup> (vast) |
| Streaming-eindpunten (gestopt of wordt uitgevoerd) per Media Services-account|2 (vast)|
| Beleid voor streaming | 100 <sup>(5)</sup> |
| Transformaties per Media Services-account | 100 (vast)|
| De unieke Streaming-Locators die tegelijk zijn gekoppeld aan een Asset | 100<sup>(6)</sup> (vast) |
| Opties per beleid voor inhoud sleutels |30 | 
| Licenties per maand voor elk van de typen DRM op Media Services sleutel leveringsservice per account|1\.000.000|

<sup>1</sup> de maximale grootte voor één blob momenteel maximaal 5 TB in Azure Blob Storage wordt wordt ondersteund. Aanvullende beperkingen gelden in Media Services op basis van de VM-grootten die worden gebruikt door de service. De maximale grootte is van toepassing op de bestanden die u uploadt en de bestanden die worden gegenereerd als gevolg van Media Services verwerken (codering of analyseren). Als het bronbestand groter dan 260 GB is, wordt waarschijnlijk uw taak mislukt. 

De volgende tabel bevat de limieten op de gereserveerde media-eenheden S1, S2 en S3. Als de bronbestand groter dan de limieten die zijn gedefinieerd in de tabel is, wordt uw coderingstaak mislukt. Als u 4K resolutie bronnen van lange duur van de codeert, moet u bent S3 gereserveerde media-eenheden gebruik voor de prestaties die nodig zijn. Hebt u 4K-inhoud die groter is dan de limiet van 260-GB op de S3 gereserveerde media-eenheden, contact met ons op amshelp@microsoft.com voor mogelijke oplossingen ter ondersteuning van uw scenario.

|Gereserveerde Media-eenheid   |Maximale invoer grootte (GB)|
|---|---|
|S1 |   26|
|S2 | 60|
|S3 |260|

<sup>2</sup> hieronder vallen de in de wachtrij, voltooide, actieve en geannuleerde taken. Deze omvatten geen verwijderde taken. 

Elke taakrecord in uw account die ouder is dan 90 dagen, automatisch verwijderd, zelfs als het totale aantal records lager dan het maximale quotum is. 

<sup>3</sup> uitvoer live bij het maken van starten en stoppen wanneer verwijderd.

<sup>4</sup> de storage-accounts moeten afkomstig zijn van hetzelfde Azure-abonnement.

<sup>5</sup> bij het gebruik van een aangepaste [beleid Streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies), moet u een beperkte set van dergelijk beleid ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw StreamingLocators wanneer dezelfde versleuteling opties en protocollen nodig zijn. U hoeft geen nieuw streaming-beleid te maken voor elke streaming-locator.

<sup>6</sup> streaming-Locators zijn niet ontworpen om toegangsbeheer per gebruiker beheren. Gebruik DRM-oplossingen (Digital Rights Management) als u afzonderlijke gebruikers verschillende toegangsrechten wilt geven.

## <a name="support-ticket"></a>Ondersteuningsticket

Voor de resources die niet zijn opgelost, kan u vraagt om de quota kunnen worden verhoogd door het openen van een [ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Gedetailleerde informatie opnemen in de aanvraag op de gewenste quota wijzigingen, use-casescenario's en regio's vereist. <br/>Maak **geen** extra Azure Media Services-accounts in een poging om de limieten te verhogen.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](media-services-overview.md)

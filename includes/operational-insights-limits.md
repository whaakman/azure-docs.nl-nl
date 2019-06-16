---
title: bestand opnemen
description: bestand opnemen
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 06/10/2019
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: c5fedc59c80c68fc222693a67664ef60ddd210a9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133870"
---
De volgende limieten gelden voor elke Log Analytics-werkruimte in de huidige op basis van gebruik prijscategorie in April 2018 geïntroduceerd:

|     | Per GB-2018 |
| --- | --- | 
| Gegevensvolume verzameld per dag | Geen |
| Bewaarperiode van gegevens | 30-730 dagen<sup>1</sup> |

De volgende limieten gelden voor elke Log Analytics-werkruimte de meest recente oudere Prijscategorieën:

|  | Gratis | Zelfstandig (Per GB) | Per knooppunt (OMS) |
| --- | --- | --- | --- | --- | --- |--- |
| Gegevensvolume verzameld per dag |500 MB<sup>2</sup> |Geen |Geen |
| Bewaarperiode van gegevens |7 dagen | 30-730 dagen<sup>1</sup> | 30-730 dagen<sup>1</sup> |

De volgende limieten gelden voor elke Log Analytics-werkruimte de oudste oudere Prijscategorieën:

|  | Standard | Premium | 
| --- | --- | --- | --- | --- | --- |--- |
| Gegevensvolume verzameld per dag | Geen | Geen | 
| Bewaarperiode van gegevens |30 dagen | 365 dagen |

<sup>1</sup>bewaren van gegevens langer dan 31 dagen is beschikbaar voor extra kosten in rekening gebracht. Meer informatie over [prijzen van Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

<sup>2</sup>wanneer uw werkruimte bereikt de 500 MB dagelijkse limiet voor gegevensoverdracht, analyse van gegevens gestopt en wordt aan het begin van de volgende dag hervat. Een dag is gebaseerd op UTC.

>[!NOTE]
>Afhankelijk van hoe lang u hebt gebruikt Log Analytics, wellicht u toegang tot de oudere Prijscategorieën. Meer informatie over [Log Analytics oudere Prijscategorieën](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 
>

De volgende limieten gelden voor Azure Log Analytics-resources (werkruimten) per abonnement.

| Prijscategorie    | Aantal werkruimten per abonnement | Opmerkingen
| --- | --- | --- |
| Gratis laag  | 10 | Deze limiet kan niet worden verhoogd. |
| Alle lagen dan gratis | N/A | U bent beperkt door het aantal resources binnen een resourcegroep en het aantal resourcegroepen per abonnement. | 

De volgende limieten gelden voor de Log Analytics-API's:

| Category | Limits | Opmerkingen
| --- | --- | --- |
| Gegevensverzamelaar-API | Maximale grootte voor een enkel bericht is 30 MB.<br>Maximumgrootte voor veldwaarden is 32 KB. | Grotere volumes splitsen in meerdere berichten.<br>Velden die langer zijn dan 32 KB worden afgebroken. |
| API voor zoeken | 5000 records geretourneerd voor niet-samengevoegde gegevens.<br>500\.000 records voor samengevoegde gegevens. | Samengevoegde gegevens is een zoekopdracht met de `summarize` opdracht.
 

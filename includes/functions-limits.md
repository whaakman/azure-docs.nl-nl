---
author: ggailey777
ms.service: billing
ms.topic: include
ms.date: 05/09/2019
ms.author: glenga
ms.openlocfilehash: f2470f937d2d812bf79cea3c23d89a50717a5a92
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277202"
---
| Resource | [Verbruiksabonnement](../articles/azure-functions/functions-scale.md#consumption-plan) | [Premium-abonnement](../articles/azure-functions/functions-scale.md#premium-plan) | [App Service-plan](../articles/azure-functions/functions-scale.md#app-service-plan)<sup>1</sup> |
| --- | --- | --- | --- |
| Uitschalen | Op gebeurtenissen gebaseerde | Op gebeurtenissen gebaseerde | [Handmatig/voor automatisch schalen](../articles/app-service/web-sites-scale.md) | 
| Maximumaantal exemplaren | 200 | 20 | 10-20 |
|Standaard [duur van de time-out](../articles/azure-functions/functions-scale.md#timeout) (min.) |5 | 30 |30<sup>2</sup> |
|Maximale [duur van de time-out](../articles/azure-functions/functions-scale.md#timeout) (min.) |10 | niet-gebonden | niet-gebonden<sup>3</sup> |
| Maximum aantal uitgaande verbindingen (per exemplaar) | 600 active (1200 totaal) | niet-gebonden | niet-gebonden |
| Maximumgrootte van aanvraag (MB)<sup>4</sup> | 100 | 100 | 100 |
| Maximumlengte van het query-tekenreeks<sup>4</sup> | 4096 | 4096 | 4096 |
| Maximale aanvraag-URL-lengte<sup>4</sup> | 8192 | 8192 | 8192 |
| [ACU](../articles/virtual-machines/windows/acu.md) per exemplaar | 100 | 210-840 | 100-840 |
| Maximale geheugen (GB per exemplaar) | 1.5 | 3.5-14 | 1.75-14 |
| Functie-apps per abonnement |100 |100 |niet-gebonden<sup>5</sup> |
| [App Service-abonnementen](../articles/app-service/overview-hosting-plans.md) | 100 per [regio](https://azure.microsoft.com/global-infrastructure/regions/) |100 per resourcegroep |100 per resourcegroep |
| Opslag<sup>6</sup> |1 GB |250 GB |50-1000 GB |
| Aangepaste domeinen per app</a> |500<sup>7</sup> |500 |500 |
| Aangepast domein [SSL-ondersteuning](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |niet-gebonden SNI SSL-verbinding inbegrepen | niet-gebonden SNI SSL en 1 IP SSL-verbindingen opgenomen |niet-gebonden SNI SSL en 1 IP SSL-verbindingen opgenomen | 

<sup>1</sup> voor bepaalde limieten voor de verschillende opties voor App Service-plan, Zie de [limieten van App Service-plan](../articles/azure-subscription-service-limits.md#app-service-limits).  
<sup>2</sup> de time-out voor de runtime 1.x van functies in een App Service-plan is standaard niet-gebonden.  
<sup>3</sup> moet de App Service-plan worden ingesteld op [Always On](../articles/azure-functions/functions-scale.md#always-on). Betalen tegen de standaardtarieven [tarieven](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup> deze limieten zijn [instellen in de host](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config).  
<sup>5</sup> het werkelijke aantal functie-apps die u kunt hosten is afhankelijk van de activiteit van de apps, de grootte van de machine-instanties en het bijbehorende gebruik van bronnen.
<sup>6</sup> de limiet voor opslag is de totale grootte van de inhoud in de tijdelijke opslag voor alle apps in hetzelfde App Service-plan. Verbruiksabonnement maakt gebruik van Azure Files voor tijdelijke opslag.  
<sup>7</sup> wanneer uw functie-app wordt gehost in een [verbruiksabonnement](../articles/azure-functions/functions-scale.md#consumption-plan), alleen de optie CNAME wordt ondersteund. Voor functie-apps in een [Premium-abonnement](../articles/azure-functions/functions-scale.md#premium-plan) of een [App Service-plan](../articles/azure-functions/functions-scale.md#app-service-plan), kunt u een aangepast domein met behulp van een CNAME- of een A-record toewijzen.

---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: d66f36e737e100a0d8e60b85b51f6dcf632e6d3e
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54404672"
---
| Resource | Gratis | Gedeeld | Basic | Standard | Premium (v2) | Isolated </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Web, mobiele Apps of API apps](https://azure.microsoft.com/services/app-service/) per [App Service-plan](../articles/app-service/overview-hosting-plans.md)<sup>1</sup> |10 |100 |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup> |Onbeperkte<sup>2</sup>|
| [App Service-plan](../articles/app-service/overview-hosting-plans.md) |10 per regio |10 per resourcegroep |100 per resourcegroep |100 per resourcegroep |100 per resourcegroep |100 per resourcegroep|
| COMPUTE exemplaartype |Gedeeld |Gedeeld |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup> |Toegewezen<sup>3</sup></p> |Toegewezen<sup>3</sup>|
| [Scale-Out](../articles/app-service/web-sites-scale.md) (max. exemplaren) |1 gedeeld |1 gedeeld |3 toegewezen<sup>3</sup> |10 toegewezen<sup>3</sup> |20 toegewezen<sup>3</sup>|100 toegewezen<sup>4</sup>|
| Storage<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |250 GB<sup>5</sup></p> |1 TB<sup>5</sup>|
| CPU-tijd (5 min)<sup>6</sup> |3 minuten |3 minuten |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a>|
| CPU-tijd (dag)<sup>6</sup> |60 minuten |240 minuten |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |Onbeperkte, betaal normale [tarieven](https://azure.microsoft.com/pricing/details/app-service/)</a> |
| Geheugen (1 uur) |1024 MB per App Service-plan |1024 MB per app |N/A |N/A |N/A |N/A |
| Bandbreedte |165 MB |Onbeperkte, [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |Onbeperkte, [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |Onbeperkte, [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |Onbeperkte, [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |Onbeperkte, [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) toepassen |
| Toepassingsarchitectuur |32-bits |32-bits |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |
| Web-Sockets per exemplaar<sup>7</sup> |5 |35 |350 |Onbeperkt |Onbeperkt |Onbeperkt |
| Gelijktijdige [verbindingen het foutopsporingsprogramma](../articles/app-service/troubleshoot-dotnet-visual-studio.md) per toepassing |1 |1 |1 |5 |5 |5 |
| App Service-certificaten per abonnement<sup>10</sup>| Niet ondersteund | Niet ondersteund |10 |10 |10 |10 |
| Aangepaste domeinen per app</a> |0 (alleen azurewebsites.net-subdomein)|500 |500 |500 |500 |500 |
| Aangepast domein [SSL-ondersteuning](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |Wordt niet ondersteund. Certificaat met jokertekens voor *. azurewebsites.net standaard beschikbaar zijn.|Wordt niet ondersteund. Certificaat met jokertekens voor *. azurewebsites.net standaard beschikbaar zijn.|Onbeperkt aantal SNI SSL-verbindingen |Onbeperkt aantal SNI SSL en 1 IP SSL-verbindingen opgenomen |Onbeperkt aantal SNI SSL en 1 IP SSL-verbindingen opgenomen | Onbeperkt aantal SNI SSL en 1 IP SSL-verbindingen opgenomen|
| Geïntegreerde belastingverdeler | |X |X |X |X |X<sup>9</sup> |
| [Altijd ingeschakeld](../articles/app-service/web-sites-configure.md) | | |X |X |X |X |
| [Geplande back-ups](../articles/app-service/manage-backup.md) | | | | Geplande back-ups elke 2 uur maximaal 12 back-ups per dag (handmatige + gepland) | Geplande back-ups elk uur een maximum van 50 back-ups per dag (handmatige + gepland) | Geplande back-ups elk uur een maximum van 50 back-ups per dag (handmatige + gepland) |
| [Automatisch schalen](../articles/app-service/web-sites-scale.md) | | | |X |X |X |
| [WebJobs](../articles/app-service/webjobs-create.md)<sup>8</sup> |X |X |X |X |X |X |
| [Azure Scheduler](https://azure.microsoft.com/services/scheduler/) ondersteunen | |X |X |X |X |X |
| [Eindpuntbewaking](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| [Faseringssleuven](../articles/app-service/deploy-staging-slots.md) | | | |5 |20 |20 |
| SLA | |  |99,9% |99.95%|99.95%|99.95%|  

<sup>1</sup> apps en opslagquota zijn per App Service-plan, tenzij anders vermeld.  
<sup>2</sup> het werkelijke aantal apps die u op deze machines hosten kunt is afhankelijk van de activiteit van de apps, de grootte van de machine-instanties en het bijbehorende gebruik van bronnen.  
<sup>3</sup> toegewezen instanties van verschillende grootten kunnen zijn. Zie [prijzen voor App Service](https://azure.microsoft.com/pricing/details/app-service/) voor meer informatie.  
<sup>4</sup> meer toegestaan op aanvraag.  
<sup>5</sup> de limiet voor opslag is de totale grootte van de inhoud in alle apps in hetzelfde App Service-plan.  
<sup>6</sup> deze resources worden beperkt door fysieke resources op de specifieke instanties (de grootte van het exemplaar en het aantal exemplaren).  
<sup>7</sup> als u een app in de Basic-laag naar twee exemplaren schalen, hebt u 350 gelijktijdige verbindingen voor elk van de twee exemplaren.  
<sup>8</sup> voert aangepaste uitvoerbare bestanden en/of scripts op aanvraag of volgens een schema of continu uitvoeren als een achtergrondtaak van uw App Service-exemplaar. Altijd beschikbaar is vereist voor de continue uitvoering van WebJobs. Azure Scheduler Gratis of Standaard is vereist voor geplande WebJobs. Er is geen vooraf gedefinieerde limiet voor het nummer van webtaken die kunnen worden uitgevoerd in een App Service-exemplaar, maar er zijn praktische grenzen die afhankelijk zijn van wat de code van de toepassing probeert uit te voeren.  
<sup>9</sup> app Service Isolated-SKU's hebben de mogelijkheid om te worden met gelijke taakverdeling (ILB) met Azure Load Balancer, dit is zonder openbare verbinding intern laden vanaf het internet. Hierdoor moeten bepaalde functies van een ILB die is geïsoleerd van een App Service worden gebruikt op machines die rechtstreeks toegang hebben tot het eindpunt van het ILB-netwerk.  
<sup>10</sup> App Service Certificate heeft de limiet per abonnement kan worden verhoogd via een ondersteuningsaanvraag met een maximale limiet van 200.  
---
title: Quota en beschikbaarheid in regio’s voor Azure Container Instances
description: Quota, limieten en beschikbaarheid in regio's van de Azure Container Instances-service.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 02/15/2019
ms.author: danlep
ms.openlocfilehash: c676989b4b882f2b1887a1b6a5091b60027f61d0
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328400"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Beschikbaarheid in regio’s voor Azure Container Instances

Alle Azure-services bevatten bepaalde standaardlimieten en -quota voor resources en functies. In de volgende secties worden de standaardresourcelimieten voor verschillende Azure Container Instances-resources gedetailleerd beschreven, en ziet u de beschikbaarheid van de Azure Container Instances-service in Azure-regio's.

## <a name="service-quotas-and-limits"></a>Servicequota en -limieten

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="feature-availability"></a>Beschikbaarheid van functies

Met Azure Container Instances kunt u Windows- en Linux-containers met dezelfde API plannen. De volgende functies zijn echter momenteel alleen beschikbaar in Linux-containergroepen. Windows-ondersteuning staat op de planning.

* Meerdere containers per containergroep
* Volume koppelen (Azure-bestanden, emptyDir, GitRepo, geheim)
* Virtueel netwerk (preview)
* GPU-resources (preview)

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Azure Container Instances is beschikbaar in de volgende regio's met de opgegeven CPU en geheugenlimieten voor elke containergroep. Waarden zijn actueel op het moment van publicatie. Gebruik de API [List Capabilities](/rest/api/container-instances/listcapabilities/listcapabilities) voor actuele informatie. 

Beschikbaarheids- en resourcelimieten kunnen verschillen wanneer u Azure Container Instances gebruikt met een [virtueel netwerk](container-instances-vnet.md) (preview) of met [GPU-resources](container-instances-gpu.md) (preview).

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| Canada - centraal, US - centraal, US - oost 2, US - zuid-centraal | Linux | 4 | 16 |
| US - oost, Europa - noord, Europa - west, US - west, US - west 2 | Linux | 4 | 14 |
| Japan - oost | Linux | 2 | 8 |
| Australië - oost, Azië - zuidoost | Linux | 2 | 7 |
| India - centraal, Azië - oost, US - noord-centraal, India - zuid | Linux | 2 | 3,5 |
| US - oost, Europa - west, US - west | Windows | 4 | 14 |
| Australië - oost, Canada - centraal, India - centraal, US - centraal, Azië - oost, US - oost 2, Japan - oost, US - noord-centraal, Europa - noord, US - zuid-centraal, India - zuid, Azië - zuidoost, US - west 2 | Windows | 2 | 3,5 |

Containerinstanties die binnen deze resourcelimieten zijn gemaakt, zijn afhankelijk van de beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties. Als u dergelijke implementatiefouten wilt minimaliseren, implementeert u instanties met een lagere CPU en geheugeninstellingen, of voert u de implementatie op een later tijdstip uit.

Brengt het team op de hoogte van aanvullende vereiste regio’s of verhoogd CPU/verhoogde geheugenlimieten op [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Zie [Troubleshoot deployment issues with Azure Container Instances](container-instances-troubleshooting.md) (Implementatieproblemen met Azure Container Instances oplossen) voor meer informatie over het oplossen van problemen met de implementatie van containerinstanties.

## <a name="next-steps"></a>Volgende stappen

Bepaalde standaardlimieten en -quota kunnen worden verhoogd. Als u een verhoging wilt aanvragen voor een of meer resources die een dergelijke verhoging ondersteunen, dient u een [Azure-ondersteuningsaanvraag][azure-support] in (selecteer Quota bij **Type probleem**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest

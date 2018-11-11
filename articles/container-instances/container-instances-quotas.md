---
title: Quota en beschikbaarheid in regio’s voor Azure Container Instances
description: De standaardquota en beschikbaarheid in regio's van de Azure Container Instances-service.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 02/27/2018
ms.author: danlep
ms.openlocfilehash: 2694e8cdc4f1918aab36794804ff48f5a70b44be
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739682"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Beschikbaarheid in regio’s voor Azure Container Instances

Alle Azure-services bevatten bepaalde standaardlimieten en -quota voor resources en functies. In de volgende secties worden de standaardresourcelimieten voor verschillende ACI-resources (Azure Container Instances) gedetailleerd beschreven, en ziet u de beschikbaarheid van de ACI-service in Azure-regio’s.

## <a name="service-quotas-and-limits"></a>Servicequota en -limieten

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Azure Container Instances is beschikbaar in de volgende regio's met de opgegeven CPU en geheugenlimieten.

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| US - oost, Europa - noord, Europa - west, US - west, US - west 2 | Linux | 4 | 14 |
| Australië - oost, US - oost 2, Azië - zuidoost | Linux | 2 | 7 |
| Canada - centraal, India - centraal, US - zuid-centraal | Linux | 2 | 3,5 |
| US - oost, Europa - west, US - west | Windows | 4 | 14 |
| Australië - oost, Canada - centraal, India - centraal, US - oost 2, Europa - noord, US - zuid-centraal, Azië - zuidoost, US - west 2 | Windows | 2 | 3,5 |

Containerinstanties die binnen deze resourcelimieten zijn gemaakt, zijn afhankelijk van de beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties. Als u dergelijke implementatiefouten wilt minimaliseren, implementeert u instanties met een lagere CPU en geheugeninstellingen, of voert u de implementatie op een later tijdstip uit.

Brengt het team op de hoogte van aanvullende vereiste regio’s of verhoogd CPU/verhoogde geheugenlimieten op [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Zie [Troubleshoot deployment issues with Azure Container Instances](container-instances-troubleshooting.md) (Implementatieproblemen met Azure Container Instances oplossen) voor meer informatie over het oplossen van problemen met de implementatie van containerinstanties.

## <a name="next-steps"></a>Volgende stappen

Bepaalde standaardlimieten en -quota kunnen worden verhoogd. Als u een verhoging wilt aanvragen voor een of meer resources die een dergelijke verhoging ondersteunen, dient u een [Azure-ondersteuningsaanvraag][azure-support] in (selecteer Quota bij **Type probleem**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest

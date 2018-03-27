---
title: Quota en beschikbaarheid in regio’s voor Azure Container Instances
description: De standaardquota en beschikbaarheid in regio's van de Azure Container Instances-service.
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 02/27/2018
ms.author: marsma
ms.openlocfilehash: 28177e17a15c5e3b92b9af52e05fa2f8e95db95f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Beschikbaarheid in regio’s voor Azure Container Instances

Alle Azure-services bevatten bepaalde standaardlimieten en -quota voor resources en functies. In de volgende secties worden de standaardresourcelimieten voor verschillende ACI-resources (Azure Container Instances) gedetailleerd beschreven, en ziet u de beschikbaarheid van de ACI-service in Azure-regio’s.

## <a name="service-quotas-and-limits"></a>Servicequota en -limieten

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Azure Container Instances is beschikbaar in de volgende regio's met de opgegeven CPU en geheugenlimieten.

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| West-Europa, VS West, VS Oost | Linux | 4 | 14 |
| VS-west 2, Zuidoost-Azië | Linux | 2 | 7 |
| West-Europa, VS West, VS Oost | Windows | 4 | 14 |
| VS-west 2, Zuidoost-Azië | Windows | 2 | 3,5 |

Containerinstanties die binnen deze resourcelimieten zijn gemaakt, zijn afhankelijk van de beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties. Als u dergelijke implementatiefouten wilt minimaliseren, implementeert u instanties met een lagere CPU en geheugeninstellingen, of voert u de implementatie op een later tijdstip uit.

Brengt het team op de hoogte van aanvullende vereiste regio’s of verhoogd CPU/verhoogde geheugenlimieten op [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Zie [Troubleshoot deployment issues with Azure Container Instances](container-instances-troubleshooting.md) (Implementatieproblemen met Azure Container Instances oplossen) voor meer informatie over het oplossen van problemen met de implementatie van containerinstanties.

## <a name="next-steps"></a>Volgende stappen

Bepaalde standaardlimieten en -quota kunnen worden verhoogd. Als u een verhoging wilt aanvragen voor een of meer resources die een dergelijke verhoging ondersteunen, dient u een [Azure-ondersteuningsaanvraag][azure-support] in (selecteer Quota bij **Type probleem**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest

---
title: Beschikbaarheid van regio's en resources voor Azure Container Instances
description: Ontdek in welke Azure-regio's de implementatie van containerinstanties en de CPU en geheugenlimieten voor deze instanties worden ondersteund.
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 12/15/2017
ms.author: marsma
ms.openlocfilehash: ec7f469c47924f4ae22d6509996ca9cf498fc9ad
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Beschikbaarheid van regio’s voor Azure Container Instances

Tijdens de preview-periode is Azure Container Instances beschikbaar in de volgende regio's met de opgegeven CPU en geheugenlimieten.

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| West-Europa, VS West, VS Oost | Linux | 4 | 14 |
| West-Europa, VS West, VS Oost | Windows | 4 | 14 |

## <a name="resource-availability"></a>Beschikbaarheid van resources

Containerinstanties die binnen deze resourcelimieten zijn gemaakt, zijn afhankelijk van de beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties.

Als u dergelijke implementatiefouten wilt minimaliseren, implementeert u instanties met een lagere CPU en geheugeninstellingen, of voert u de implementatie op een later tijdstip uit.

## <a name="next-steps"></a>Volgende stappen

Zie [Troubleshoot deployment issues with Azure Container Instances](container-instances-troubleshooting.md) (Implementatieproblemen met Azure Container Instances oplossen) voor meer informatie over het oplossen van problemen met de implementatie van containerinstanties.
---
title: Beschikbaarheid van regio's en resources voor Azure Container Instances
description: Ontdek in welke Azure-regio's de implementatie van containerinstanties en de CPU en geheugenlimieten voor deze instanties worden ondersteund.
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 08/31/2017
ms.author: marsma
ms.openlocfilehash: ace4eb6b284f2c1b2caeb54c1d686e68cacb1725
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Beschikbaarheid van regio’s voor Azure Container Instances

Tijdens de preview-periode is Azure Container Instances beschikbaar in de volgende regio's met de opgegeven CPU en geheugenlimieten.

| Locatie | OS | CPU | Geheugen (GB) |
| -------- | -- | :---: | :-----------: |
| West-Europa, VS West, VS Oost | Linux | 2 | 7 |
| West-Europa, VS West, VS Oost | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Beschikbaarheid van resources

Containerinstanties die binnen deze resourcelimieten zijn gemaakt, zijn afhankelijk van de beschikbaarheid in de implementatieregio. Wanneer een regio zwaar wordt belast, kan er een fout optreden bij het implementeren van instanties.

Als u dergelijke implementatiefouten wilt minimaliseren, implementeert u instanties met een lagere CPU en geheugeninstellingen, of voert u de implementatie op een later tijdstip uit.

## <a name="next-steps"></a>Volgende stappen

Zie [Troubleshoot deployment issues with Azure Container Instances](container-instances-troubleshooting.md) (Implementatieproblemen met Azure Container Instances oplossen) voor meer informatie over het oplossen van problemen met de implementatie van containerinstanties.
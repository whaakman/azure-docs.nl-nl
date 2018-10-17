---
title: Met behulp van toegangsbeheer op basis van rollen voor het beheren van Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u wilt toepassen en rollen gebaseerd toegangsbeheer (RBAC) gebruiken voor het beheren van uw Azure Site Recovery-implementaties
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/16/2018
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 9ec9acab4ad83af22504058c21b102df94b8692d
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353234"
---
# <a name="use-role-based-access-control-to-manage-site-recovery-access"></a>Toegangsbeheer op basis van rollen gebruiken voor het beheren van Site Recovery-toegang

Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u taken scheiden binnen uw team en alleen specifieke machtigingen verlenen aan gebruikers worden weergegeven als die nodig zijn voor specifieke taken uitvoeren.

Azure Site Recovery biedt 3 ingebouwde rollen voor het beheren van Site Recovery-beheerbewerkingen. Meer informatie over [ingebouwde Azure RBAC-rollen](../role-based-access-control/built-in-roles.md)

* [Site Recovery-inzender](../role-based-access-control/built-in-roles.md#site-recovery-contributor) - deze rol bevat alle machtigingen die nodig zijn om Azure Site Recovery-bewerkingen in een Recovery Services-kluis te kunnen beheren. Een gebruiker met deze rol kan echter geen Recovery Services-kluis maken of verwijderen, noch toegangsrechten aan andere gebruikers toewijzen. Deze rol is het meest geschikt zijn voor beheerders die kunnen inschakelen en beheren van herstel na noodgevallen voor toepassingen of hele organisaties, zoals het geval kunnen zijn.
* [Site Recovery-operator](../role-based-access-control/built-in-roles.md#site-recovery-operator): deze rol heeft machtigingen om failover- en failback-bewerkingen uit te voeren en te beheren. Een gebruiker met deze rol kan niet inschakelen replicatie uitschakelen, maken of verwijderen van kluizen, nieuwe infrastructuur registreren of toegangsrechten toewijzen aan andere gebruikers. Deze rol is het meest geschikt is voor een operator die failover-virtuele machines kunt of applicaties in opdracht van toepassingseigenaren en IT-beheerders in een situatie werkelijke of gesimuleerd na noodgevallen, zoals een DR inzoomen. Post-omzetting van de ramp, de DR-operator opnieuw kunt beveiligen en failback de virtuele machines.
* [Site Recovery-lezer](../role-based-access-control/built-in-roles.md#site-recovery-reader) - deze rol heeft machtigingen voor het weergeven van alle beheerbewerkingen van Site Recovery. Deze rol is het meest geschikt is voor een IT-leidinggevende die u kunt de huidige status van de beveiliging bewaken en ondersteuningstickets indien nodig.

Als u op zoek bent voor het definiëren van uw eigen rollen voor nog meer controle, Zie hoe u [bouwen van aangepaste rollen](../role-based-access-control/custom-roles.md) in Azure.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Vereiste machtigingen voor het inschakelen van replicatie voor de nieuwe virtuele machines
Wanneer een nieuwe virtuele Machine is gerepliceerd naar Azure met behulp van Azure Site Recovery, worden de toegangsniveaus voor de gebruiker is gekoppeld gevalideerd om ervoor te zorgen dat de gebruiker de vereiste machtigingen heeft voor het gebruik van de Azure-resources die Site Recovery.

U schakelt replicatie voor een nieuwe virtuele machine, moet een gebruiker hebben:
* Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
* Machtiging voor het maken van een virtuele machine in de geselecteerde virtuele netwerk
* Machtigingen voor schrijven naar het geselecteerde opslagaccount

Een gebruiker moet de volgende machtigingen voor volledige replicatie van een nieuwe virtuele machine.

> [!IMPORTANT]
>Zorg ervoor dat de relevante machtigingen per het implementatiemodel worden toegevoegd (Resource Manager / klassieke) gebruikt voor de resource-implementatie.

| **Resourcetype** | **Implementatiemodel** | **Machtiging** |
| --- | --- | --- |
| Compute | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klassiek | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Netwerk | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klassiek | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Storage | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klassiek | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Resourcegroep | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Overweeg het gebruik van de 'Inzender voor virtuele machines' en 'Inzender voor klassieke virtuele Machine' [ingebouwde rollen](../role-based-access-control/built-in-roles.md) modellen respectievelijk voor Resource Manager en klassieke implementatie.

## <a name="next-steps"></a>Volgende stappen
* [Role-Based Access Control](../role-based-access-control/role-assignments-portal.md): aan de slag met RBAC in Azure portal.
* Informatie over het beheren van toegang met:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure-CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Role-Based Access Control probleemoplossing](../role-based-access-control/troubleshooting.md): Profiteer van tips voor veelvoorkomende problemen oplossen.

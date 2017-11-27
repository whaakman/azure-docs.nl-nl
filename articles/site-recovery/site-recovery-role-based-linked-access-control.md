---
title: Met behulp van toegangsbeheer op basis van rollen voor het beheren van Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u moeten worden toegepast en op rollen gebaseerde toegangsbeheer (RBAC) gebruiken voor het beheren van uw Azure Site Recovery-implementaties
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: ce579bc2844d321e4fbc70726b57120e17e4788d
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="use-role-based-access-control-to-manage-azure-site-recovery-deployments"></a>Toegangsbeheer op basis van rollen gebruiken voor het beheren van Azure Site Recovery-implementaties

Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u taken scheiden binnen uw team en alleen specifieke machtigingen verlenen aan gebruikers desgewenst specifieke taken uitvoeren.

Azure Site Recovery biedt 3 ingebouwde functies voor het beheren van Site Recovery-beheerbewerkingen. Meer informatie over [ingebouwde Azure RBAC-rollen](../active-directory/role-based-access-built-in-roles.md)

* [Site Recovery Inzender](../active-directory/role-based-access-built-in-roles.md#site-recovery-contributor) -deze rol heeft alle machtigingen nodig voor het beheren van Azure Site Recovery-bewerkingen in een Recovery Services-kluis. Een gebruiker met deze rol echter niet maken of een Recovery Services-kluis verwijderen of toewijzen van rechten aan andere gebruikers. Deze rol is het meest geschikt voor disaster recovery beheerders die kunnen inschakelen en herstel na noodgevallen voor toepassingen of volledige organisaties, zoals het geval worden beheren.
* [Site Recovery-Operator](../active-directory/role-based-access-built-in-roles.md#site-recovery-operator) -deze rol heeft de machtigingen om uit te voeren en de Failover en Failback operations manager. Een gebruiker met deze rol kan niet- of uitschakelen replicatie, maken of verwijderen van kluizen, registreren van nieuwe infrastructuur of toewijzen van rechten aan andere gebruikers. Deze rol is het meest geschikt is voor een disaster recovery operator die failover virtuele machines kunt of toepassingen wanneer u hierom wordt gevraagd door toepassingseigenaars en IT-beheerders in een situatie met een bestaande of gesimuleerde na noodgevallen zoals een DR zoomen. Post resolutie van de noodherstelservice, de operator DR opnieuw kunt beveiligen en failback van de virtuele machines.
* [Site Recovery-lezer](../active-directory/role-based-access-built-in-roles.md#site-recovery-reader) -deze rol machtigingen heeft voor het weergeven van alle beheerbewerkingen van Site Recovery. Deze rol is het meest geschikt is voor een IT-bewaking leidinggevende die kunt bewaken van de huidige status van de beveiliging en ondersteuningstickets verhogen, indien nodig.

Als u op zoek bent om te definiëren van uw eigen rollen voor nog meer Zie hoe [aangepaste rollen maken](../active-directory/role-based-access-control-custom-roles.md) in Azure.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Vereiste machtigingen voor replicatie inschakelen voor de nieuwe virtuele Machines
Wanneer een nieuwe virtuele Machine naar Azure met Azure Site Recovery worden gerepliceerd, worden de bijbehorende gebruiker toegangsniveaus om ervoor te zorgen dat de gebruiker de vereiste machtigingen heeft voor het gebruik van de Azure-resources die aan de Site Recovery gevalideerd.

Schakel replicatie voor een nieuwe virtuele machine door moet een gebruiker hebben:
* Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
* Machtiging voor het maken van een virtuele machine in de geselecteerde virtuele netwerk
* Machtigingen voor schrijven naar het geselecteerde opslagaccount

Een gebruiker moet de volgende machtigingen tot de replicatie van een nieuwe virtuele machine is voltooid.

> [!IMPORTANT]
>Zorg ervoor dat de relevante machtigingen per het implementatiemodel worden toegevoegd (Resource Manager / klassieke) gebruikt voor de implementatie van de resource.

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

Overweeg het gebruik van de 'Virtual Machine Contributor' en 'Klassieke Virtual Machine Contributor' [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md) modellen respectievelijk voor de implementatie van Resource Manager en Classic.

## <a name="next-steps"></a>Volgende stappen
* [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* Meer informatie over het beheren van toegang met:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Probleemoplossing voor toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-troubleshooting.md): Profiteer van tips voor het oplossen van veelvoorkomende problemen.

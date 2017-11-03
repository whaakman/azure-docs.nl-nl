---
title: 'ExpressRoute gekoppeld virtuele netwerken migreren van klassiek naar Resource Manager: Azure: PowerShell | Microsoft Docs'
description: Deze pagina wordt beschreven hoe het migreren van de gekoppelde virtuele netwerken in Resource Manager na het verplaatsen van het circuit.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 336f68308f7d4b4dd3c7476a4fabd793939e9e85
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-to-resource-manager"></a>ExpressRoute gekoppeld virtuele netwerken migreren van klassiek naar Resource Manager

In dit artikel wordt uitgelegd hoe virtuele netwerken van Azure ExpressRoute gekoppeld vanuit het klassieke implementatiemodel migreren naar het Azure Resource Manager-implementatiemodel na het verplaatsen van uw ExpressRoute-circuit. 


## <a name="before-you-begin"></a>Voordat u begint
* Controleer of u de nieuwste versie van de Azure PowerShell-modules. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.
* Zorg ervoor dat u hebt bekeken de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* Lees de informatie die is opgegeven onder [een ExpressRoute-circuit verplaatsen van klassiek naar Resource Manager](expressroute-move.md). Zorg ervoor dat u volledig limieten en beperkingen begrijpt.
* Controleer of het circuit volledig operationeel is in het klassieke implementatiemodel.
* Zorg ervoor dat u een resourcegroep die is gemaakt in het Resource Manager-implementatiemodel.
* Controleer de volgende documentatie voor de resource-migratie:

    * [Migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [Veelgestelde vragen: Platform ondersteund migratie van IaaS-middelen van klassiek naar Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Bekijk meest voorkomende migratiefouten en oplossingen](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Ondersteunde en niet-ondersteunde scenario 's

* Een ExpressRoute-circuit kan worden verplaatst van het klassieke naar het Resource Manager-omgeving zonder uitvaltijd. U kunt elk ExpressRoute-circuit verplaatsen van het klassieke naar het Resource Manager-omgeving zonder uitvaltijd. Volg de instructies in [ExpressRoute-circuits verplaatsen van het klassieke naar het Resource Manager-implementatiemodel met behulp van PowerShell](expressroute-howto-move-arm.md). Dit is een vereiste voor het verplaatsen van resources die zijn verbonden met het virtuele netwerk.
* Virtuele netwerken, gateways en bijbehorende implementaties binnen het virtuele netwerk die zijn gekoppeld aan een ExpressRoute-circuit in hetzelfde abonnement kunnen worden gemigreerd naar de Resource Manager-omgeving zonder uitvaltijd. U kunt de stappen verderop voor het migreren van resources, zoals virtuele netwerken, gateways en virtuele machines die worden geïmplementeerd in het virtuele netwerk. U moet ervoor zorgen dat de virtuele netwerken correct zijn geconfigureerd voordat ze worden gemigreerd. 
* Virtuele netwerken, gateways en bijbehorende implementaties binnen het virtuele netwerk die zich niet in hetzelfde abonnement als het ExpressRoute-circuit moeten enige downtime om de migratie te voltooien. De laatste sectie van het document beschrijft de stappen te volgen bronnen worden gemigreerd.
* Een virtueel netwerk met zowel de ExpressRoute-Gateway en de VPN-Gateway kan niet worden gemigreerd.

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>Een ExpressRoute-circuit verplaatsen van klassiek naar Resource Manager
U moet een ExpressRoute-circuit verplaatsen van het klassieke naar het Resource Manager-omgeving voordat u probeert te migreren van de resources die zijn gekoppeld aan het ExpressRoute-circuit. Zie de volgende artikelen voor het uitvoeren van deze taak:

* Lees de informatie die is opgegeven onder [een ExpressRoute-circuit verplaatsen van klassiek naar Resource Manager](expressroute-move.md).
* [Een circuit verplaatsen van klassiek naar Resource Manager, met Azure PowerShell](expressroute-howto-move-arm.md).
* De Azure Service Management portal gebruiken. U kunt de werkstroom te volgen [maken van nieuwe ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en selecteer de optie importeren. 

Deze bewerking leidt tot uitvaltijd. U kunt blijven gegevensoverdracht tussen uw lokale en Microsoft terwijl de migratie uitgevoerd wordt.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Migreren van virtuele netwerken, gateways en bijbehorende implementaties

De stappen die u volgt als u wilt migreren, is afhankelijk van of uw resources zich in hetzelfde abonnement en/of verschillende abonnementen behoren.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Migreren van virtuele netwerken, gateways en bijbehorende implementaties in hetzelfde abonnement als het ExpressRoute-circuit
Deze sectie beschrijft de stappen te volgen voor het migreren van een virtueel netwerk, gateway en bijbehorende implementaties in hetzelfde abonnement als het ExpressRoute-circuit. Er is geen downtime gekoppeld aan deze migratie. U kunt alle resources via het migratieproces blijven. Het management vlak is vergrendeld terwijl de migratie uitgevoerd wordt. 

1. Zorg ervoor dat het ExpressRoute-circuit is verplaatst van het klassieke naar het Resource Manager-omgeving.
2. Zorg ervoor dat het virtuele netwerk op de juiste wijze is bedoeld voor de migratie.
3. Registreer uw abonnement voor de Resourcemigratie. Gebruik de volgende PowerShell-codefragment voor het registreren van uw abonnement voor de Resourcemigratie:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Valideren, voorbereiden en migreren. Gebruik de volgende PowerShell-codefragment voor het verplaatsen van het virtuele netwerk:

  ```powershell
  Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
  Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
  Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
  ```

  Ook kunt u migratie afbreken door de volgende PowerShell-cmdlet:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

## <a name="next-steps"></a>Volgende stappen
* [Migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Veelgestelde vragen: Platform ondersteund migratie van IaaS-middelen van klassiek naar Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Bekijk meest voorkomende migratiefouten en oplossingen](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

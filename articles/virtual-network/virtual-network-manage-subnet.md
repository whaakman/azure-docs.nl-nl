---
title: Toevoegen, wijzigen of verwijderen van een virtueel Azure-netwerksubnet
titlesuffix: Azure Virtual Network
description: Informatie over het toevoegen, wijzigen of verwijderen van het subnet van een virtueel netwerk in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: f621bb562a963aa3c8b7296e4d75bbea5e9d47a2
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052782"
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Toevoegen, wijzigen of verwijderen van een subnet van een virtueel netwerk

Informatie over het toevoegen, wijzigen of verwijderen van een subnet van een virtueel netwerk. Alle Azure-resources geïmplementeerd in een virtueel netwerk worden geïmplementeerd in een subnet binnen een virtueel netwerk. Als u geen ervaring met virtuele netwerken, kunt u meer informatie over deze in de [overzicht van Virtual network](virtual-networks-overview.md) of door te voeren een [zelfstudie](quick-create-portal.md). Als u wilt maken, wijzigen, of verwijderen van een virtueel netwerk, Zie [beheren van een virtueel netwerk](manage-virtual-network.md).

## <a name="before-you-begin"></a>Voordat u begint

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="add-a-subnet"></a>Een subnet toevoegen

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u wilt toevoegen van een subnet.
3. Selecteer onder **INSTELLINGEN** **Subnetten**.
4. Selecteer **+ Subnet**.
5. Voer waarden in voor de volgende parameters:
    - **Naam**: De naam moet uniek zijn binnen het virtuele netwerk. Voor een maximale compatibiliteit met andere Azure-services, wordt u aangeraden een letter als het eerste teken van de naam. Azure Application Gateway wordt niet bijvoorbeeld implementeren in een subnet met een naam die met een getal begint.
    - **Adresbereik**: Het bereik moet uniek zijn binnen de adresruimte voor het virtuele netwerk. Het bereik mogen niet overlappen met adresbereiken van andere subnet binnen het virtuele netwerk. De adresruimte moet worden opgegeven met behulp van notatie (Classless Inter-Domain Routing). U kunt bijvoorbeeld een subnet een adresruimte van 10.0.0.0/24 definiëren in een virtueel netwerk met de adresruimte 10.0.0.0/16. Het kleinste bereik dat u kunt opgeven is /29, waarmee u acht IP-adressen voor het subnet. Azure reserveert de eerste en laatste adres in elk subnet voor conformiteit van protocol. Drie extra adressen zijn gereserveerd voor gebruik met Azure-service. Als gevolg hiervan, het definiëren van een subnet met een/29 adres bereik resultaten in drie bruikbare IP-adressen in het subnet. Als u van plan bent een virtueel netwerk verbinden met een VPN-gateway, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is toegevoegd, klikt u onder bepaalde omstandigheden. Zie voor informatie over het wijzigen van het adresbereik van een subnet, [-subnet-instellingen wijzigen](#change-subnet-settings).
    - **Netwerkbeveiligingsgroep**: U kunt nul of een bestaande netwerkbeveiligingsgroep aan een subnet voor het filteren van binnenkomend en uitgaand netwerkverkeer voor het subnet koppelen. De netwerkbeveiligingsgroep moet bestaan in hetzelfde abonnement en dezelfde locatie als het virtuele netwerk. Meer informatie over [netwerkbeveiligingsgroepen](security-overview.md) en [over het maken van een netwerkbeveiligingsgroep](tutorial-filter-network-traffic.md).
    - **Routetabel**: U kunt geen of een bestaande routetabel aan een subnet voor het beheren van routering van netwerkverkeer met andere netwerken kunt koppelen. De routetabel moet bestaan in hetzelfde abonnement en dezelfde locatie als het virtuele netwerk. Meer informatie over [Azure routering](virtual-networks-udr-overview.md) en [een routetabel maken](tutorial-create-route-table-portal.md)
    - **Service-eindpunten:** Een subnet kan nul of meerdere service-eindpunten ingeschakeld voor deze hebben. Als een service-eindpunt voor een service, schakelt u de service of -services die u wilt inschakelen, service-eindpunten voor uit de **Services** lijst. De locatie wordt automatisch geconfigureerd voor een eindpunt. Service-eindpunten zijn standaard geconfigureerd voor de regio van het virtuele netwerk. Voor Azure Storage, ter ondersteuning van regionale failover-scenario's, worden eindpunten automatisch geconfigureerd naar [gekoppelde Azure-regio's](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).
    - **Delegatie van subnet:** Een subnet kan nul op meerdere overdrachten zijn ingeschakeld voor deze hebben. Subnet delegering biedt expliciete machtigingen voor de service servicespecifieke om resources te maken in het subnet met behulp van een unieke id bij het implementeren van de service. Voor het overdragen van een service, selecteert u de service die u overdragen wilt aan van de **Services** lijst. 

    Als u wilt verwijderen van een service-eindpunt, hef de selectie van de service die u wilt verwijderen van de service-eindpunt voor. Zie voor meer informatie over service-eindpunten en de services die kan worden ingeschakeld voor [overzicht van Virtual network service endpoints](virtual-network-service-endpoints-overview.md). Wanneer u een service-eindpunt voor een service hebt ingeschakeld, moet u ook toegang tot het netwerk voor het subnet voor een resource die zijn gemaakt met de service inschakelen. Bijvoorbeeld, als u de service-eindpunt voor inschakelen *Microsoft.Storage*, moet u ook toegang tot het netwerk aan alle Azure Storage-accounts die u wilt verlenen van toegang tot het netwerk inschakelen. Zie de documentatie voor de afzonderlijke service die u voor het service-eindpunt ingeschakeld voor meer informatie over het inschakelen op subnetten die een service-eindpunt is ingeschakeld voor toegang tot het netwerk.

    Om te valideren dat een service-eindpunt is ingeschakeld voor een subnet, geven de [effectieve routes](diagnose-network-routing-problem.md) voor een netwerkinterface in het subnet. Wanneer u een eindpunt is geconfigureerd, ziet u een *standaard* route met de adresvoorvoegsels van de service en een nextHopType van **VirtualNetworkServiceEndpoint**. Zie voor meer informatie over routering, [routeringoverzicht](virtual-networks-udr-overview.md).
6. Het subnet toevoegen aan het virtuele netwerk dat u hebt geselecteerd, selecteert u **OK**.

**Opdrachten**

- Azure CLI: [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>Subnet-instellingen wijzigen

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waarin het subnet dat u wilt wijzigen van instellingen voor.
3. Selecteer onder **INSTELLINGEN** **Subnetten**.
4. Selecteer het subnet dat u wilt wijzigen van instellingen voor in de lijst met subnetten. U kunt de volgende instellingen wijzigen:

    - **Adresbereik:** Als er geen resources binnen het subnet worden geïmplementeerd, kunt u het adresbereik kunt wijzigen. Als alle resources in het subnet bestaat, moet u de resources verplaatsen naar een ander subnet of deze eerst verwijderen uit het subnet. De stappen die u moet een resource verwijderen of verplaatsen, is afhankelijk van de resource. Voor informatie over het verplaatsen of verwijderen van resources die zich in subnetten, Raadpleeg de documentatie voor elk resourcetype die u wilt verplaatsen of verwijderen. Zie de beperkingen voor **adresbereik** in stap 5 van [toevoegen van een subnet](#add-a-subnet).
    - **Gebruikers**: U kunt toegang tot het subnet beheren met behulp van ingebouwde rollen of uw eigen aangepaste rollen. Zie voor meer informatie over het toewijzen van rollen en gebruikers toegang krijgen tot het subnet, [roltoewijzing gebruiken voor het beheren van toegang tot uw Azure-resources](../role-based-access-control/role-assignments-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-role-assignment).
    - **Netwerkbeveiligingsgroep** en **routetabel**: Zie stap 5 van [toevoegen van een subnet](#add-a-subnet).
    - **Service-eindpunten**: Service-eindpunten in stap 5 van Zie [toevoegen van een subnet](#add-a-subnet). Bij het inschakelen van een service-eindpunt voor een bestaand subnet, zorg ervoor dat er geen kritieke taken worden uitgevoerd voor de resources in het subnet. Service-eindpunten overschakelen routes op elke netwerkinterface in het subnet van het gebruik van de standaard-route met de *0.0.0.0/0* voorvoegsel en volgend hoptype van *Internet*, voor het gebruik van een nieuwe route met de adres van de voorvoegsels van de service en een volgend hoptype van *VirtualNetworkServiceEndpoint*. Tijdens de switch kunnen geopende TCP-verbindingen worden beëindigd. Het service-eindpunt is niet ingeschakeld totdat verkeersstromen tot de service voor alle netwerkinterfaces worden bijgewerkt met de nieuwe route. Zie voor meer informatie over routering, [routeringoverzicht](virtual-networks-udr-overview.md).
    - **Delegatie van subnet:** Service-eindpunten in stap 5 van Zie [toevoegen van een subnet](#add-a-subnet). Subnet overdracht kan worden gewijzigd op nul of meerdere overdrachten zijn ingeschakeld voor deze. Als een resource voor een service is al geïmplementeerd in het subnet, kunnen subnet overdracht kan niet worden verwijderd totdat de alle bronnen voor de service zijn verwijderd. Als u wilt delegeren voor een andere service, selecteert u de service die u overdragen wilt aan van de **Services** lijst. 
5. Selecteer **Opslaan**.

**Opdrachten**

- Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Een subnet verwijderen

U kunt een subnet alleen verwijderen als er geen bronnen in het subnet zijn. Als er resources in het subnet zijn, moet u de resources die zich in het subnet voordat u het subnet kunt verwijderen. De stappen waarmee u een resource verwijderen, is afhankelijk van de resource. Voor informatie over resources verwijderen die zich in subnetten, Raadpleeg de documentatie voor elk resourcetype dat u wilt verwijderen.

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waarin het subnet dat u wilt verwijderen.
3. Selecteer onder **INSTELLINGEN** **Subnetten**.
4. Selecteer in de lijst met subnetten, **...** , aan de rechterkant van het subnet u wilt verwijderen
5. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az network vnet verwijderen](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-delete)
- PowerShell: [Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op subnetten, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

|Bewerking                                                                   |   Name                                       |
|-----------------------------------------------------------------------  |   -----------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Een virtueel netwerksubnet lezen              |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Maken of bijwerken van een subnet van een virtueel netwerk  |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Subnet van een virtueel netwerk verwijderen            |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Deelnemen aan een virtueel netwerk                     |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Een service-eindpunt voor een subnet inschakelen     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Ophalen van de virtuele machines in een subnet       |

## <a name="next-steps"></a>Volgende stappen

- Maak een virtueel netwerk en subnetten met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken

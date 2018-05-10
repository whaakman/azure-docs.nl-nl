---
title: Toevoegen, wijzigen of verwijderen van een virtueel netwerk van Azure-subnet | Microsoft Docs
description: Informatie over het toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 68d4c54b2648dc3b40e69dcde9828d18de318796
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Toevoegen, wijzigen of een virtueel netwerksubnet verwijderen

Informatie over het toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet. Alle Azure-resources geïmplementeerd in een virtueel netwerk worden geïmplementeerd in een subnet binnen een virtueel netwerk. Als u geen ervaring met virtuele netwerken, kunt u meer informatie over deze in de [Virtual network-overzicht](virtual-networks-overview.md) of door te voeren een [zelfstudie](quick-create-portal.md). Als u wilt maken, wijzigen, of verwijderen van een virtueel netwerk, Zie [beheren van een virtueel netwerk](manage-virtual-network.md).

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Deze zelfstudie vereist de Azure PowerShell-moduleversie 5.7.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure met, moet worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="add-a-subnet"></a>Een subnet toevoegen

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer het virtuele netwerk dat u wilt toevoegen van een subnet uit de lijst van virtuele netwerken.
3. Selecteer onder **INSTELLINGEN** **Subnetten**.
4. Selecteer **+ Subnet**.
5. Geef waarden voor de volgende parameters:
    - **Naam**: de naam moet uniek zijn binnen het virtuele netwerk.
    - **-Adresbereik**: het bereik moet uniek zijn binnen de adresruimte voor het virtuele netwerk. Het bereik kan niet overlappen met adresbereiken van andere subnet binnen het virtuele netwerk. De adresruimte moet worden opgegeven met behulp van notatie (Classless Inter-Domain Routing). U kunt bijvoorbeeld een subnet-adresruimte van 10.0.0.0/24 definiëren in een virtueel netwerk met adresruimte 10.0.0.0/16. Het kleinste bereik dat kunt u opgeven is slechts/29, waarmee u acht IP-adressen voor het subnet. Azure behoudt zich het eerste en laatste adres in elk subnet voor het protocol overeenstemming. Drie extra adressen zijn gereserveerd voor gebruik van Azure service. Als gevolg hiervan definiëren van een subnet met een/29 adres bereik resulteert in drie bruikbare IP-adressen in het subnet. Als u een virtueel netwerk verbinden met een VPN-gateway wilt, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is toegevoegd, klikt u onder bepaalde omstandigheden. Zie voor meer informatie over het wijzigen van een subnet-adresbereik, [subnetinstellingen wijzigen](#change-subnet-settings).
    - **Netwerkbeveiligingsgroep**: U kunt nul of een bestaande netwerkbeveiligingsgroep aan een subnet voor het filteren van binnenkomende en uitgaande netwerkverkeer voor het subnet koppelen. De netwerkbeveiligingsgroep moet zich in hetzelfde abonnement en dezelfde locatie als het virtuele netwerk. Meer informatie over [netwerkbeveiligingsgroepen](security-overview.md) en [het maken van een netwerkbeveiligingsgroep](tutorial-filter-network-traffic.md).
    - **Routetabel**: U kunt geen of een bestaande routetabel aan een subnet om te beheren met het netwerk voor verkeersroutering met andere netwerken koppelen. De routetabel moet zich in hetzelfde abonnement en dezelfde locatie als het virtuele netwerk. Meer informatie over [Azure routering](virtual-networks-udr-overview.md) en [een routetabel maken](tutorial-create-route-table-portal.md)
    - **Service-eindpunten:** een subnet nul of meerdere service-eindpunten ingeschakeld voor deze kan hebben. Als u een service-eindpunt voor een service, selecteert u de service of de services die u wilt inschakelen, service-eindpunten voor uit de **Services** lijst. De locatie wordt automatisch geconfigureerd voor een eindpunt. Service-eindpunten zijn standaard geconfigureerd voor het virtuele netwerk regio. Voor Azure Storage, ter ondersteuning van regionale failover-scenario's, eindpunten zijn automatisch geconfigureerd voor [Azure regio's gekoppeld](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

    Als u wilt verwijderen van een service-eindpunt, moet u de service die u wilt verwijderen van het service-eindpunt voor verwijderen. Zie voor meer informatie over service-eindpunten en de services kunnen worden ingeschakeld voor [Virtual network service-eindpunten overzicht](virtual-network-service-endpoints-overview.md). Wanneer u een service-eindpunt voor een service hebt ingeschakeld, moet u ook toegang tot het netwerk voor het subnet voor een resource die is gemaakt met de service inschakelen. Bijvoorbeeld, als u het service-eindpunt voor inschakelen *Microsoft.Storage*, moet u ook toegang tot het netwerk aan alle Azure Storage-accounts die u wilt verlenen toegang tot het netwerk inschakelen. Zie voor meer informatie over het inschakelen van netwerktoegang tot subnetten die een service-eindpunt is ingeschakeld voor de documentatie voor de afzonderlijke service ingeschakeld van het service-eindpunt voor.

    Om te valideren dat een service-eindpunt is ingeschakeld voor een subnet, geven de [effectieve routes](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-virtual-machine) voor elke netwerkinterface in het subnet. Als een eindpunt is geconfigureerd, ziet u een *standaard* route met de adresvoorvoegsels van de service en een nextHopType van **VirtualNetworkServiceEndpoint**. Zie voor meer informatie over routering, [Routering-overzicht](virtual-networks-udr-overview.md).
6. Selecteer om het subnet toe aan het virtuele netwerk die u hebt geselecteerd **OK**.

**Opdrachten**

- Azure CLI: [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>De subnet-wijzigingsinstellingen

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waarin het subnet dat u wilt wijzigen van instellingen voor.
3. Selecteer onder **INSTELLINGEN** **Subnetten**.
4. Selecteer het subnet dat u wilt wijzigen van instellingen voor in de lijst met subnetten. U kunt de volgende instellingen wijzigen:

    - **-Adresbereik:** als geen resources binnen het subnet zijn geïmplementeerd, kunt u het adresbereik wijzigen. Als alle resources in het subnet bestaat, moet u de resources verplaatsen naar een ander subnet of deze eerst verwijderen uit het subnet. De stappen waarmee u verplaatsen of verwijderen van een resource, is afhankelijk van de resource. Voor informatie over het verplaatsen of verwijderen van bronnen die zich in subnetten, Raadpleeg de documentatie voor elk resourcetype die u wilt verplaatsen of verwijderen. Zie de beperkingen voor **-adresbereik** in stap 5 van [een subnet toevoegen](#add-a-subnet).
    - **Gebruikers**: U kunt toegang tot het subnet met behulp van ingebouwde rollen of uw eigen aangepaste rollen beheren. Zie voor meer informatie over het toewijzen van rollen en gebruikers toegang krijgen tot het subnet, [roltoewijzing gebruiken voor het beheren van toegang tot uw Azure-resources](../role-based-access-control/role-assignments-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
    - **Netwerkbeveiligingsgroep** en **routetabel**: Zie stap 5 van [een subnet toevoegen](#add-a-subnet).
    - **Service-eindpunten**: service-eindpunten in stap 5 van [een subnet toevoegen](#add-a-subnet). Wanneer u een service-eindpunt voor een bestaand subnet inschakelt, zorg ervoor dat er geen kritieke taken worden uitgevoerd op alle bronnen in het subnet. Service-eindpunten switch routes op elke netwerkinterface in het subnet van het gebruik van de standaardroute met de *0.0.0.0/0* adres voorvoegsel en volgende hoptype *Internet*, voor het gebruik van een nieuwe route met de adres van de voorvoegsels van de service en een volgend hoptype van *VirtualNetworkServiceEndpoint*. Tijdens de switch, worden open TCP-verbindingen beëindigd. Het service-eindpunt is niet ingeschakeld totdat verkeersstromen aan de service voor alle netwerkinterfaces worden bijgewerkt met de nieuwe route. Zie voor meer informatie over routering, [Routering-overzicht](virtual-networks-udr-overview.md).
5. Selecteer **Opslaan**.

**Opdrachten**

- Azure CLI: [az network vnet subnet bijwerken](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Een subnet verwijderen

U kunt een subnet alleen verwijderen als er geen resources in het subnet zijn. Als er bronnen in het subnet, moet u de resources die zich in het subnet voordat u kunt het subnet verwijderen verwijderen. De stappen waarmee u een bron verwijderen, is afhankelijk van de resource. Als u wilt weten hoe u resources verwijderen die in subnetten zijn, Raadpleeg de documentatie voor elk resourcetype dat u wilt verwijderen.

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer het virtuele netwerk waarin het subnet dat u wilt verwijderen uit de lijst van virtuele netwerken.
3. Selecteer onder **INSTELLINGEN** **Subnetten**.
4. Selecteer in de lijst met subnetten **...** , aan de rechterkant van het subnet u wilt verwijderen
5. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az network vnet verwijderen](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-delete)
- PowerShell: [Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>Machtigingen

Om taken te voeren op subnetten, moet uw account worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

|Bewerking                                                                   |   Naam                                       |
|-----------------------------------------------------------------------  |   -----------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Een virtueel netwerksubnet lezen              |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Maken of bijwerken van een virtueel netwerksubnet  |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Een virtueel netwerksubnet verwijderen            |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Een virtueel netwerk koppelen                     |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Een service-eindpunt voor een subnet inschakelen     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Ophalen van de virtuele machines in een subnet       |

## <a name="next-steps"></a>Volgende stappen

- Maken van een virtueel netwerk en subnetten met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) steekproef scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen van [Azure beleid](policy-samples.md) voor virtuele netwerken
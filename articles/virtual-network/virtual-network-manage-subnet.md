---
title: Toevoegen, wijzigen of verwijderen van een virtueel netwerk van Azure-subnet | Microsoft Docs
description: Informatie over het toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 85ba6ef3e51c339a77eb9b4198c4f87e2a64cf09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Toevoegen, wijzigen of een virtueel netwerksubnet verwijderen

Informatie over het toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet. 

Als u niet bekend met virtuele netwerken, bent voordat u toevoegen, wijzigen of verwijderen van een subnet, raden wij aan dat u leest [Azure Virtual Network-overzicht](virtual-networks-overview.md) en [maken, wijzigen of verwijderen van een virtueel netwerk](virtual-network-manage-network.md). Alle Azure-resources geïmplementeerd in een virtueel netwerk worden geïmplementeerd in een subnet binnen een virtueel netwerk. Meestal worden meerdere subnetten in een virtueel netwerk te gemaakt:
- **Filteren van verkeer tussen subnetten**. U kunt netwerkbeveiligingsgroepen toepassen op subnetten voor het filteren van binnenkomende en uitgaande netwerkverkeer voor alle bronnen (zoals virtuele machines) die zich in het virtuele netwerk. Zie voor meer informatie over het maken van een netwerkbeveiligingsgroep, [netwerkbeveiligingsgroepen maken](virtual-networks-create-nsg-arm-pportal.md).
- **Beheren van routering tussen subnetten**. Azure maakt standaardroutes zodat het verkeer automatisch doorgestuurd tussen subnetten. U kunt Azure standaardroutes onderdrukken door de gebruiker gedefinieerde routes maken. Zie voor meer informatie over de gebruiker gedefinieerde routes, [maken van de gebruiker gedefinieerde routes](virtual-network-create-udr-arm-ps.md). 

In dit artikel wordt uitgelegd hoe u wilt toevoegen, wijzigen en verwijderen van een subnet voor de virtuele netwerken die zijn gemaakt met de Azure Resource Manager-implementatiemodel.
 
## <a name="before"></a>Voordat u begint

Voordat u de taken die worden beschreven in dit artikel, moet u de volgende vereisten voldoen:

- Als u geen ervaring met het werken met virtuele netwerken, raden wij aan u wordt aangeraden de oefening in [maken van uw eerste virtuele Azure-netwerk](virtual-network-get-started-vnet-subnet.md). In deze oefening kunt u vertrouwd raken met virtuele netwerken.
- Bekijk voor meer informatie over limieten voor virtuele netwerken, [Azure limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Aanmelden bij de Azure-portal, Azure-opdrachtregelprogramma (Azure CLI) of Azure PowerShell met behulp van uw Azure-account. Als u geen Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u gebruiken van PowerShell-opdrachten wilt voor het voltooien van de taken die in dit artikel wordt beschreven, moet u eerst [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u de meest recente versie van de Azure PowerShell-cmdlets die is geïnstalleerd. Als u de help voor PowerShell-opdrachten in de voorbeelden, voer `get-help <command> -full`.
- Als u wilt dat Azure CLI-opdrachten gebruiken voor het voltooien van de taken die in dit artikel wordt beschreven, moet u een van:
    - [Installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u de meest recente versie van Azure CLI is geïnstalleerd.
    - Gebruik de Azure-Cloud-Shell. In plaats van installatie van de CLI en de bijbehorende afhankelijkheden, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Cloud-Shell (**> _**) pictogram aan de bovenkant van de Azure-portal. 

  Als u hulp bij de Azure CLI-opdrachten, voer `az <command> --help`.

## <a name="create-subnet"></a>Een subnet toevoegen

Een subnet toevoegen:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Voer in het zoekvak portal **virtuele netwerken**. Klik in de zoekresultaten op **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, klikt u op het virtuele netwerk dat u wilt toevoegen van een subnet.
4. Klik op de blade virtueel netwerk **subnetten**.
5. Klik op **+ Subnet**.
6. Op de **subnet toevoegen** blade Voer waarden in voor de volgende parameters:
    - **Naam**: de naam moet uniek zijn binnen het virtuele netwerk.
    - **-Adresbereik**: het bereik moet uniek zijn binnen de adresruimte voor het virtuele netwerk. Het bereik kan niet overlappen met adresbereiken van andere subnet binnen het virtuele netwerk. De adresruimte moet worden opgegeven met behulp van notatie (Classless Inter-Domain Routing). U kunt bijvoorbeeld een subnet-adresruimte van 10.0.0.0/24 definiëren in een virtueel netwerk met adresruimte 10.0.0.0/16. Het kleinste bereik dat kunt u opgeven is slechts/29, waarmee u acht IP-adressen voor het subnet. Azure behoudt zich het eerste en laatste adres in elk subnet voor het protocol overeenstemming. Drie extra adressen zijn gereserveerd voor gebruik van Azure service. Als gevolg hiervan definiëren van een subnet met een/29 adres bereik resulteert in drie bruikbare IP-adressen in het subnet. Als u een virtueel netwerk verbinden met een VPN-gateway wilt, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is toegevoegd, klikt u onder bepaalde omstandigheden. Zie voor meer informatie over het wijzigen van een subnet-adresbereik, [subnetinstellingen wijzigen](#change-subnet) in dit artikel.
    - **Netwerkbeveiligingsgroep**: u kunt desgewenst een bestaande netwerkbeveiligingsgroep koppelen aan het subnet moet het inkomende en uitgaande netwerkverkeer filteren voor het subnet. De netwerkbeveiligingsgroep moet zich in hetzelfde abonnement en dezelfde locatie als het virtuele netwerk. Deze moet ook worden gemaakt met behulp van het Resource Manager-implementatiemodel. Zie voor meer informatie over het maken van een netwerkbeveiligingsgroep, [Netwerkbeveiligingsgroepen](virtual-networks-create-nsg-arm-pportal.md).
    - **Routetabel**: u kunt desgewenst een bestaande routetabel koppelen aan het subnet om te beheren met het netwerk voor verkeersroutering met andere netwerken. De routetabel moet zich in hetzelfde abonnement en dezelfde locatie als het virtuele netwerk. Deze moet ook worden gemaakt met behulp van het Resource Manager-implementatiemodel. Zie voor meer informatie over het maken van routetabellen, [gebruiker gedefinieerde routes](virtual-network-create-udr-arm-ps.md).
    - **Gebruikers**: U kunt toegang tot het subnet met behulp van ingebouwde rollen of uw eigen aangepaste rollen beheren. Zie voor meer informatie over het toewijzen van rollen en gebruikers toegang krijgen tot het subnet, [roltoewijzing gebruiken voor het beheren van toegang tot uw Azure-resources](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Als het subnet toevoegen aan het virtuele netwerk die u hebt geselecteerd, klikt u op **OK**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[AZ network vnet subnet maken](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nieuwe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [toevoegen AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>De subnet-wijzigingsinstellingen

U kunt netwerkbeveiligingsgroepen, routetabellen en gebruikerstoegang aan een subnet wijzigen door het beheer van bronnen die zich in een subnet. Voor meer informatie over deze instellingen [een subnet toevoegen](#create-subnet), Zie stap 6. Als u wijzigen van de adresruimte van een subnet wilt, moet u eerst alle bronnen die zich in het subnet te verwijderen. De stappen waarmee u een bron verwijderen, is afhankelijk van de resource. Als u wilt weten hoe u resources verwijderen die in subnetten zijn, Raadpleeg de documentatie voor elk resourcetype dat u wilt verwijderen. Het adresbereik voor een subnet wijzigen:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Voer in het zoekvak portal **virtuele netwerken**. Klik in de zoekresultaten op **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, klikt u op het virtuele netwerk waarvoor u wilt wijzigen van een subnet-adresbereik.
4. Klik op het subnet waarvoor u wilt het adresbereik wijzigen.
5. Op de blade subnet in de **-adresbereik** Voer het nieuwe adresbereik. Het bereik moet uniek zijn binnen de adresruimte voor het virtuele netwerk. Het bereik kan niet overlappen met adresbereiken van andere subnet binnen het virtuele netwerk. De adresruimte moet worden opgegeven met behulp van de CIDR-notatie. U kunt bijvoorbeeld een subnet-adresruimte van 10.0.0.0/24 definiëren in een virtueel netwerk met adresruimte 10.0.0.0/16. Het kleinste bereik dat kunt u opgeven is slechts/29, waarmee u acht IP-adressen voor het subnet. Azure behoudt zich het eerste en laatste adres in elk subnet voor het protocol overeenstemming. Drie extra adressen zijn gereserveerd voor gebruik van Azure service. Als gevolg hiervan, een subnet met een/29-adresbereik heeft drie bruikbare IP-adressen. Als u een virtueel netwerk verbinden met een VPN-gateway wilt, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is gemaakt, klikt u onder bepaalde omstandigheden. Zie voor meer informatie over het wijzigen van een subnet-adresbereik, [subnetinstellingen wijzigen](#change-subnet) in dit artikel.
6. Klik op **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[AZ network vnet subnet bijwerken](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Een subnet verwijderen

U kunt een subnet alleen verwijderen als er geen resources in het subnet zijn. Als er bronnen in het subnet, moet u de resources die zich in het subnet voordat u kunt het subnet verwijderen verwijderen. De stappen waarmee u een bron verwijderen, is afhankelijk van de resource. Als u wilt weten hoe u resources verwijderen die in subnetten zijn, Raadpleeg de documentatie voor elk resourcetype dat u wilt verwijderen. Een subnet te verwijderen:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Voer in het zoekvak portal **virtuele netwerken**. Klik in de zoekresultaten op **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, klikt u op het virtuele netwerk dat u wilt verwijderen van een subnet uit.
4. Op het virtuele netwerk blade onder **instellingen**, klikt u op **subnetten**.
5. In de lijst met subnetten die wordt weergegeven op de blade subnetten, met de rechtermuisknop op het subnet dat u wilt verwijderen, klikt u op **verwijderen**, en klik vervolgens op **Ja** verwijderen van het subnet.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[vnet-AZ netwerk het verwijderen](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Verwijder AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het maken van een virtuele machine in een subnet [een virtueel netwerk maken en implementeren van virtuele machines in het subnet](virtual-network-get-started-vnet-subnet.md#create-vms).

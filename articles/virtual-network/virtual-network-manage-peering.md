---
title: Maken, wijzigen of een virtueel netwerk van Azure-peering verwijderen | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een virtueel netwerk peering.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial;anavin
ms.openlocfilehash: 314920d4aae2bd364ea93451fcbda2034a36cc7e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332096"
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Maken, wijzigen of een virtueel netwerk-peering verwijderen

Informatie over het maken, wijzigen of verwijderen van een virtueel netwerk peering. Virtueel netwerk peering, kunt u virtuele netwerken zijn verbonden via de Azure-backbone-netwerk. Zodra de peer is ingesteld, worden de virtuele netwerken nog steeds beheerd als afzonderlijke resources. Als u geen ervaring met het virtuele netwerk peering, kunt u meer informatie over het in de [peering overzicht van virtual network](virtual-network-peering-overview.md) of door te voeren een [zelfstudie](tutorial-connect-virtual-networks-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met een account met de [noodzakelijke machtigingen](#permissions) werken met peerings.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook uitvoeren `Connect-AzureRmAccount` met een account met de [noodzakelijke machtigingen](#permissions) werken met de peering, een verbinding wilt maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` met een account met de [noodzakelijke machtigingen](#permissions) werken met de peering, een verbinding wilt maken met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure met, moet worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-peering"></a>Een peering maken

Voordat u maakt een peering, raken met de [vereisten en beperkingen](#requirements-and-contstraints) en [noodzakelijke machtigingen](#permissions).

1. Voer in het zoekvak boven aan de Azure-portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze. Selecteer niet **virtuele netwerken (klassiek)** als deze wordt weergegeven in de lijst als u een peering van een virtueel netwerk te implementeren via het klassieke implementatiemodel niet maken.
2. Selecteer het virtuele netwerk in de lijst die u maken van een peering wilt voor.
3. Selecteer het virtuele netwerk dat u maken van een peering wilt voor in de lijst met virtuele netwerken.
4. Onder **instellingen**, selecteer **Peerings**.
5. Selecteer **+ Toevoegen**. 
6. <a name="add-peering"></a>Typ of Selecteer waarden voor de volgende instellingen:
    - **Naam:** de naam voor de peering moet uniek zijn binnen het virtuele netwerk.
    - **Virtueel netwerk implementatiemodel:** selecteren welke deployment model voor het virtuele netwerk dat u wilt met peer via is geïmplementeerd.
    - **Ik weet mijn resource-ID:** hebt u leestoegang tot het virtuele netwerk dat u wilt met peer, laat u dit selectievakje uitgeschakeld. Als er geen leestoegang tot het virtuele netwerk of een abonnement dat u wilt met peer, schakel dit selectievakje in. Voer de volledige resource-ID van het virtuele netwerk dat u wilt met peer in de **Resource-ID** vak die werden weergegeven wanneer u het selectievakje ingeschakeld. De resource-ID u moet voor een virtueel netwerk dat in hetzelfde zijn bestaat, of [ondersteund verschillende](#requirements-and-constraints) Azure [regio](https://azure.microsoft.com/regions) als dit virtuele netwerk. De volledige resource-ID op /subscriptions/ lijkt<Id>/resourceGroups/ < resource-group-name > /providers/Microsoft.Network/virtualNetworks/ < virtuele-netwerk-name >. U kunt de resource-ID voor een virtueel netwerk ophalen door de eigenschappen voor een virtueel netwerk weer te geven. Zie voor meer informatie over de eigenschappen voor een virtueel netwerk, [virtuele netwerken beheren](manage-virtual-network.md#view-virtual-networks-and-settings).
    - **Abonnement:** selecteert u de [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) van het virtuele netwerk dat u wilt dat als peer dienen. Een of meer abonnementen worden vermeld, afhankelijk van hoeveel abonnementen uw account leestoegang tot heeft. Als u gecontroleerd of de **Resource-ID** selectievakje, deze instelling is niet beschikbaar.
    - **Virtueel netwerk:** Selecteer het virtuele netwerk dat u wilt dat als peer dienen. U kunt een virtueel netwerk gemaakt via de Azure-implementatiemodel selecteren. Als u een virtueel netwerk selecteert in een andere regio wilt, moet u een virtueel netwerk in een [regio ondersteund](#cross-region). U moet beschikken over leestoegang aan het virtuele netwerk om te worden weergegeven in de lijst. Als een virtueel netwerk wordt vermeld, maar lichter gekleurd weergegeven, is het mogelijk is de adresruimte voor het virtuele netwerk met de adresruimte voor dit virtuele netwerk overlapt. Als het virtuele netwerk adresruimten elkaar overlappen, ze kunnen niet worden ingesteld als peer. Als u gecontroleerd of de **Resource-ID** selectievakje, deze instelling is niet beschikbaar.
    - **Toestaan van toegang tot het virtuele netwerk:** Selecteer **ingeschakeld** (standaard) als u wilt inschakelen van communicatie tussen de twee virtuele netwerken. Inschakelen van communicatie tussen virtuele netwerken kunt resources die zijn verbonden met een virtueel netwerk met elkaar communiceren met de bandbreedte en de latentie als ze zijn verbonden met hetzelfde virtuele netwerk. Alle communicatie tussen bronnen in de twee virtuele netwerken is via het particuliere netwerk van Azure. De **VirtualNetwork** servicetag voor netwerkbeveiligingsgroepen omvat het virtuele netwerk en het virtuele netwerk peer is ingesteld. Zie voor meer informatie over network security group service-tags, [netwerk beveiligingsgroepen overzicht](security-overview.md#service-tags). Selecteer **uitgeschakelde** als u niet dat verkeer wilt toe aan het peered virtuele netwerk. U kunt bijvoorbeeld selecteren **uitgeschakelde** als u een virtueel netwerk met een ander virtueel netwerk hebt brengen, maar soms wilt uitschakelen van netwerkverkeer tussen de twee virtuele netwerken. U kunt in-/ uitschakelen handiger vindt dan verwijderen en opnieuw maken van peerings. Wanneer deze instelling is uitgeschakeld, verkeersstroom niet tussen de peered virtuele netwerken.
    - **Toestaan van doorgestuurd verkeer:** Schakel dit selectievakje in zodat verkeer *doorgestuurd* door een virtueel netwerkapparaat in een virtueel netwerk (die niet afkomstig zijn van het virtuele netwerk) naar de stroom met dit virtuele netwerk via een peering . Denk bijvoorbeeld drie virtuele netwerken met de naam Spoke1 Spoke2 en Hub. Een peering bestaat tussen elke spoke virtueel netwerk en het virtuele netwerk Hub, maar peerings niet bestaan tussen de spoke virtuele netwerken. Een virtueel netwerkapparaat wordt geïmplementeerd in het virtuele netwerk Hub en de gebruiker gedefinieerde routes worden toegepast op elke spoke virtueel netwerk dat verkeer tussen de subnetten via het virtuele netwerkapparaat routeren. Als dit selectievakje niet is ingeschakeld voor de peering tussen elke spoke virtueel netwerk en het virtuele netwerk hub, verkeersstroom niet tussen de virtuele netwerken spoke omdat de hub is het verkeer tussen virtuele netwerken doorsturen. Inschakelen van deze mogelijkheid kunt u de doorgestuurd verkeer via de peering, creëert ze geen een gebruiker gedefinieerde routes of virtuele netwerkapparaten. Gebruiker gedefinieerde routes en virtuele netwerkapparaten worden afzonderlijk gemaakt. Meer informatie over [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined). U hoeft niet te controleren met deze instelling als verkeer wordt doorgestuurd tussen virtuele netwerken via een Azure VPN-Gateway.
    - **Gateway-doorvoer toestaan:** Schakel dit selectievakje in als u een virtuele netwerkgateway is gekoppeld aan dit virtuele netwerk hebt en wilt toestaan dat verkeer van de peered virtueel netwerk via de gateway stromen. Dit virtuele netwerk kan bijvoorbeeld worden gekoppeld aan een on-premises netwerk via een virtuele netwerkgateway. De gateway kan een ExpressRoute- of VPN-gateway zijn. Dit selectievakje inschakelt, kunt verkeer van de peered virtueel netwerk via de gateway die is gekoppeld aan dit virtuele netwerk naar de on-premises netwerk stromen. Als u dit selectievakje inschakelt, is het peered virtuele netwerk geen een gateway is geconfigureerd. De peered virtuele netwerk moet hebben de **externe gateways gebruiken** selectievakje gecontroleerd bij het instellen van de peering van het virtuele netwerk in dit virtuele netwerk. Als u laat dit selectievakje uitgeschakeld (standaard), verkeer uit het virtuele netwerk brengen nog steeds stromen in dit virtuele netwerk, maar kan niet door een virtuele netwerkgateway is gekoppeld aan dit virtuele netwerk stromen. Als de peering tussen een virtueel netwerk (Resource Manager) en een virtueel netwerk (klassiek), moet de gateway in het virtuele netwerk (Resource Manager). U kunt deze optie niet inschakelen als u virtuele netwerken in verschillende regio's bent peering.
    
        Naast het doorsturen van verkeer naar een on-premises netwerk, kan een VPN-gateway doorsturen van netwerkverkeer tussen virtuele netwerken die brengen met het virtuele netwerk dat de gateway, zonder de virtuele netwerken dat is moet worden ingesteld als peer met elkaar. Een VPN-gateway gebruiken voor het doorsturen van verkeer is handig als u wilt gebruiken een VPN-gateway in een hub (Zie de hub en spoke-voorbeeld beschreven voor **doorgestuurd verkeer toestaan**) virtuele netwerk verkeer leiden tussen spoke virtuele netwerken die niet zijn brengen met elkaar. Zie voor meer informatie over het toestaan van het gebruik van een gateway voor de doorvoer, [configureren van een VPN-gateway voor de doorvoer in een virtueel netwerk peering](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit scenario vereist de implementatie van de gebruiker gedefinieerde routes die de virtuele netwerkgateway als het volgende hoptype opgeeft. Meer informatie over [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined). U kunt een VPN-gateway alleen opgeven als een volgend hoptype in een door de gebruiker gedefinieerde route, u kunt een ExpressRoute-gateway als het volgende hoptype in een door de gebruiker gedefinieerde route opgeven. U kunt deze optie niet inschakelen als u virtuele netwerken in verschillende regio's bent peering.

    - **Externe gateways gebruiken:** Schakel dit selectievakje in zodat het verkeer van dit virtuele netwerk door een virtuele netwerkgateway gekoppeld aan het virtuele netwerk u bent peering met stromen. Het virtuele netwerk dat u bent peering met heeft bijvoorbeeld een VPN-gateway gekoppeld waarmee de communicatie met een on-premises netwerk.  Dit selectievakje inschakelt, kunt verkeer van dit virtuele netwerk via de VPN-gateway gekoppeld aan het peered virtuele netwerk. Als u dit selectievakje inschakelt, het peered virtuele netwerk moet een virtuele netwerkgateway is gekoppeld en moet de **toestaan gateway onderweg** selectievakje ingeschakeld. Als u niets in dit vak uitgeschakeld (standaard), het verkeer van de peered virtueel netwerk kunt nog steeds in dit virtuele netwerk, maar het kan niet verloopt via een virtuele netwerkgateway gekoppeld aan deze virtuele netwerk. 
    Slechts één peering voor dit virtuele netwerk, kan deze instelling is ingeschakeld hebben.

        U kunt externe gateways niet gebruiken als u al een gateway is geconfigureerd in uw virtuele netwerk. U kunt deze optie niet inschakelen als u virtuele netwerken in verschillende regio's bent peering. Zie voor meer informatie over het gebruik van een gateway voor de doorvoer, [een VPN-gateway voor de doorvoer in een virtueel netwerk peering configureren](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

7. Selecteer **OK** om toe te voegen van de peering aan het virtuele netwerk dat u hebt geselecteerd.

Zie voor stapsgewijze instructies voor het implementeren van de peering tussen virtuele netwerken tot verschillende abonnementen behoren en implementatiemodellen [Vervolgstappen](#next-steps). 


### <a name="commands"></a>Opdrachten

- **Azure CLI**: [az network vnet-peering maken](/cli/azure/network/vnet/peering#create)
- **PowerShell**: [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Peering instellingen weergeven of wijzigen

Voordat u wijzigt een peering, raken met de [vereisten en beperkingen](#requirements-and-contstraints) en [noodzakelijke machtigingen](#permissions).

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze. Selecteer niet **virtuele netwerken (klassiek)** als deze wordt weergegeven in de lijst als u een peering van een virtueel netwerk te implementeren via het klassieke implementatiemodel niet maken.
2. Selecteer het virtuele netwerk in de lijst die u wilt wijzigen van de peering-instellingen voor.
3. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u wilt wijzigen van de peering-instellingen voor.
4. Onder **instellingen**, selecteer **Peerings**.
5. De peering selecteren die u wilt weergeven of wijzigen van instellingen voor.
6. De juiste instelling wijzigen. Meer informatie over de opties voor elke instelling in [stap 6](#add-peering) van een peering maken. 
7. Selecteer **Opslaan**.

**Opdrachten**

- **Azure CLI**: [az vnet peering lijst met netwerken](/cli/azure/network/vnet/peering#az_network_vnet_peering_list) naar lijst peerings voor een virtueel netwerk [az network vnet-peering weergeven](/cli/azure/network/vnet/peering#az_network_vnet_peering_show) om weer te geven van instellingen voor een specifieke peering en [az netwerk vnet-peering update](/cli/azure/network/vnet/peering#az_network_vnet_peering_update) peering instellingen te wijzigen. |
- **PowerShell**: [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering) peering weergave-instellingen op te halen en [Set AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering) om instellingen te wijzigen.

## <a name="delete-a-peering"></a>Een peering verwijderen

Voordat u verwijdert een peering, zorg ervoor dat uw account heeft de [noodzakelijke machtigingen](#permissions).

Wanneer een peering wordt verwijderd, loopt de peered virtueel netwerk niet meer verkeer van een virtueel netwerk. Wanneer u virtuele netwerken die zijn geïmplementeerd via Resource Manager brengen, wordt in elk virtueel netwerk heeft een peering aan het virtuele netwerk. Hoewel het verwijderen van de peering van een virtueel netwerk, schakelt de communicatie tussen de virtuele netwerken, worden niet verwijderd de peering van het virtuele netwerk. De status peering in het virtuele netwerk bestaat voor de peering **verbroken**. U kan niet opnieuw maken de peering totdat u opnieuw de peering in het eerste virtuele netwerk en de status van de peering beide wijzigingen van virtuele netwerken maken *verbonden*. 

Als u wilt dat virtuele netwerken om te communiceren soms, maar niet altijd, in plaats van het verwijderen van een peering, kunt u instellen de **toestaan van toegang tot het virtuele netwerk** instelt op **uitgeschakelde** in plaats daarvan. Voor meer informatie over hoe, lees stap 6 van de [maken van een peering](#create-peering) sectie van dit artikel. Wellicht eenvoudiger dan verwijderd en opnieuw gemaakt peerings netwerktoegang en uitschakelen.

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze. Selecteer niet **virtuele netwerken (klassiek)** als deze wordt weergegeven in de lijst als u een peering van een virtueel netwerk te implementeren via het klassieke implementatiemodel niet maken.
2. Selecteer het virtuele netwerk in de lijst die u verwijderen van een peering wilt voor.
3. Selecteer het virtuele netwerk dat u een peering wilt voor verwijderen uit de lijst van virtuele netwerken.
4. Onder **instellingen**, selecteer **Peerings**.
5. Aan de rechterkant van de peering die u wilt verwijderen, selecteert u **...** , selecteer **verwijderen**, selecteer daarna **Ja** peering van het eerste virtuele netwerk verwijderen.
6. De vorige stappen voor het verwijderen van de peering van het virtuele netwerk in de peering te voltooien.

**Opdrachten**

- **Azure CLI**: [az network vnet peering verwijderen](/cli/azure/network/vnet/peering#az_network_vnet_peering_delete)
- **PowerShell**: [Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Vereisten en beperkingen 

- <a name="cross-region"></a>U kunt virtuele netwerken in de dezelfde regio, of verschillende regio's op hetzelfde niveau. De volgende beperkingen zijn niet van toepassing wanneer beide virtuele netwerken zich in de *dezelfde* regio, maar zijn van toepassing wanneer de virtuele netwerken globaal brengen: 
    - De virtuele netwerken kunnen bestaan in een openbare Azure-cloud-regio, maar niet in Azure nationale clouds.
    - Resources in een virtueel netwerk kunnen niet communiceren met het IP-adres van een Azure interne load balancer in peered virtuele netwerk. De load balancer en de resources die met het communiceren moeten zich in hetzelfde virtuele netwerk.
    - Externe gateways gebruiken of niet toestaan van gateway-doorvoer. Als u externe gateways gebruiken of gateway onderweg toestaan, moeten beide virtuele netwerken in de peering in dezelfde regio hebben. 
    - Communicatie tussen virtuele netwerken globaal brengen via de volgende typen van de VM wordt niet ondersteund: [hoge prestaties compute](../virtual-machines/windows/sizes-hpc.md) en [GPU](../virtual-machines/windows/sizes-gpu.md). Dit omvat H, NC, NV, NCv2 NCv3 en virtuele machines ND-serie.
- De virtuele netwerken kunnen zich in dezelfde of verschillende abonnementen. Wanneer u virtuele netwerken tot verschillende abonnementen behoren peer, is beide abonnementen gekoppeld aan dezelfde Azure Active Directory-tenant. Als u nog een AD-tenant hebt, kunt u snel [maken van een](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant). U kunt een [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) twee virtuele netwerken die bestaan uit verschillende abonnementen die gekoppeld aan verschillende Active Directory-tenants zijn verbinding maken.
- De virtuele netwerken die u peer moeten niet-overlappende IP-adresruimtes hebben.
- U kan niet toevoegen-adresbereiken aan of te verwijderen-adresbereiken in de adresruimte van een virtueel netwerk van zodra een virtueel netwerk is gekoppeld aan een ander virtueel netwerk. Als u wilt toevoegen of verwijderen-adresbereiken, verwijderen van de peering, toevoegen of verwijderen van de adresbereiken, maakt u opnieuw de peering. Als u wilt toevoegen-adresbereiken aan of verwijderen van adresbereiken van virtuele netwerken, Zie [virtuele netwerken beheren](manage-virtual-network.md).
- U kunt twee virtuele netwerken te implementeren via Resource Manager of een virtueel netwerk is geïmplementeerd via Resource Manager met een virtueel netwerk te implementeren via het klassieke implementatiemodel peer. U kunt geen peer twee virtuele netwerken die zijn gemaakt met behulp van het klassieke implementatiemodel. Als u niet bekend met Azure-implementatiemodellen bent, leest u de [begrijpen Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. U kunt [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om twee virtuele netwerken te koppelen die zijn gemaakt via het klassieke implementatiemodel.
- Wanneer u twee virtuele netwerken koppelt die zijn gemaakt via Resource Manager, moet voor elk virtueel netwerk een peering worden geconfigureerd. U ziet een van de volgende typen voor de status van de peering: 
    - *Gestarte:* bij het maken van de peering naar het tweede virtuele netwerk van het eerste virtuele netwerk, de status van de peering is *gestarte*. 
    - *Verbonden:* bij het maken van de peering van het tweede virtuele netwerk naar het eerste virtuele netwerk, de status van de peering is *verbonden*. Als u de status van de peering voor het eerste virtuele netwerk bekijkt, ziet u de status ervan is gewijzigd van *gestarte* naar *verbonden*. De peering is niet tot stand gebracht totdat de status van de peering voor beide virtuele netwerk peerings *verbonden*.
- Wanneer een virtueel netwerk via Resource Manager gemaakt met een virtueel netwerk gemaakt met behulp van het klassieke implementatiemodel peering, configureert u alleen een peering voor het virtuele netwerk dat is geïmplementeerd via Resource Manager. U kunt voor een virtueel netwerk (klassiek) of tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel peering niet configureren. Bij het maken van de peering van het virtuele netwerk (Resource Manager) aan het virtuele netwerk (klassiek), de status van de peering is *Updating*, wordt binnenkort worden gewijzigd in *verbonden*.
- Een peering tot stand is gebracht tussen twee virtuele netwerken. Peerings zijn niet transitief. Als u peerings tussen maakt:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Er is geen peering tussen VirtualNetwork1 en VirtualNetwork3 via VirtualNetwork2. Als u maken van een virtueel netwerk peering tussen VirtualNetwork1 en VirtualNetwork3 wilt, hebt u een peering tussen VirtualNetwork1 en VirtualNetwork3 maken.
- Namen in virtuele netwerken peer is ingesteld met standaard Azure-naamomzetting kan niet worden omgezet. Voor het omzetten van namen in andere virtuele netwerken, moet u [Azure DNS voor persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of een aangepaste DNS-server. Zie voor meer informatie over het instellen van uw eigen DNS-server, [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
- Resources in virtuele netwerken peer is ingesteld in dezelfde regio kunnen met elkaar communiceren met de bandbreedte en de latentie alsof ze zich in hetzelfde virtuele netwerk. De grootte van elke virtuele machine heeft echter een eigen maximale netwerkbandbreedte. Zie voor meer informatie over maximale netwerkbandbreedte voor andere virtuele machine grootten, [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) grootten van virtuele machines.
- Een virtueel netwerk kan worden ingesteld als peer met een ander virtueel netwerk en ook worden verbonden met een ander virtueel netwerk met een virtueel netwerk van Azure-gateway. Wanneer u virtuele netwerken zijn verbonden via peering en een gateway, wordt verkeer tussen virtuele netwerken loopt via de peeringconfiguratie in plaats van de gateway.
- Er wordt een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van een virtueel netwerk-peering. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/virtual-network) voor meer informatie.

## <a name="permissions"></a>Machtigingen

De accounts die u werken kunt met het virtuele netwerk peering moeten worden toegewezen aan de volgende rollen:

- [Netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor): voor een virtueel netwerk is geïmplementeerd via Resource Manager.
- [Klassieke Network Contributor](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor): voor een virtueel netwerk te implementeren via het klassieke implementatiemodel.

Als uw account niet aan een van de vorige rollen toegewezen is, moet deze worden toegewezen aan een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die de benodigde acties uit de volgende tabel is toegewezen:

| Actie | Naam |
|---|---|
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write  | Vereist voor het maken van een peering van het virtuele netwerk een virtueel netwerk B. virtuele moet netwerk A een virtueel netwerk (Resource Manager)                            |
| Microsoft.Network/virtualNetworks/peer/action                   | Vereist voor het maken van een peering van virtueel netwerk B (Resource Manager) voor het virtuele netwerk A                                                                                |
| Microsoft.ClassicNetwork/virtualNetworks/peer                   | Vereist voor het maken van een peering van virtueel netwerk B (klassiek) voor het virtuele netwerk A                                                                                    |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read   | Een virtueel netwerk peering lezen   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Een virtueel netwerk-peering verwijderen |

## <a name="next-steps"></a>Volgende stappen

* Peering in virtuele netwerken kan tot stand worden gebracht tussen virtuele netwerken die zijn gemaakt via hetzelfde implementatiemodel of via verschillende implementatiemodellen die tot hetzelfde abonnement of tot verschillende abonnementen behoren. Volg een zelfstudie voor een van de volgende scenario's:

    |Azure-implementatiemodel             | Abonnement  |
    |---------                          |---------|
    |Beide in Resource Manager              |[Hetzelfde](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Verschillend](create-peering-different-subscriptions.md)|
    |Eén in Resource Manager, één klassiek  |[Hetzelfde](create-peering-different-deployment-models.md)|
    |                                   |[Verschillend](create-peering-different-deployment-models-subscriptions.md)|

* Meer informatie over het maken van een [hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)
* Maak een virtueel netwerk peering met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) steekproef scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
* Maken en toepassen van [Azure beleid](policy-samples.md) voor virtuele netwerken

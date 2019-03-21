---
title: Maken, wijzigen of verwijderen van een peering op virtueel netwerk | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een peering op virtueel netwerk.
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
ms.date: 02/21/2019
ms.author: jdial;anavin
ms.openlocfilehash: 28783b61a9361d97c151294140819249c9a100c2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57875205"
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Maken, wijzigen of een virtueel netwerk-peering verwijderen

Informatie over het maken, wijzigen of verwijderen van een peering op virtueel netwerk. Peering op virtueel netwerk kunt u verbinding maken met virtuele netwerken in dezelfde regio en tussen regio's (ook wel bekend als wereldwijde VNet-Peering) via de Azure-backbone-netwerk. Na de peering wordt de virtuele netwerken worden nog steeds beheerd als afzonderlijke resources. Als u geen ervaring met peering op virtueel netwerk, kunt u meer informatie over het in de [overzicht van virtual network-peering](virtual-network-peering-overview.md) of door te voeren een [zelfstudie](tutorial-connect-virtual-networks-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met een account met de [benodigde machtigingen](#permissions) om te werken met peerings.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. In deze zelfstudie vereist de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Connect-AzAccount` met een account met de [benodigde machtigingen](#permissions) om te werken met peering, voor het maken van een verbinding met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` met een account met de [benodigde machtigingen](#permissions) om te werken met peering, voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-peering"></a>Maken van een peering

Voordat u een peering, raken met de vereisten en beperkingen en [benodigde machtigingen](#permissions).

1. Voer in het zoekvak boven aan de Azure-portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze. Schakel niet **virtuele netwerken (klassiek)** als deze wordt weergegeven in de lijst, zoals u kunt geen maken een peering van een virtueel netwerk dat is geïmplementeerd via het klassieke implementatiemodel.
2. Selecteer het virtuele netwerk in de lijst die u maken van een peering wilt voor.
3. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u maken van een peering wilt voor.
4. Onder **instellingen**, selecteer **Peerings**.
5. Selecteer **+ Toevoegen**. 
6. <a name="add-peering"></a>Typ of Selecteer waarden voor de volgende instellingen:
    - **Naam:** De naam voor de peering moet uniek zijn binnen het virtuele netwerk.
    - **Implementatiemodel voor virtueel netwerk:** Selecteer welke het virtuele netwerk dat u koppelen wilt aan is geïmplementeerd via implementatiemodel.
    - **Ik weet mijn resource-ID:** Als u leestoegang tot het virtuele netwerk dat u koppelen wilt aan hebt, laat u dit selectievakje uitgeschakeld. Als u geen leestoegang tot het virtuele netwerk of het abonnement dat u koppelen wilt aan, schakel dit selectievakje in. Voer de volledige resource-ID van het virtuele netwerk dat u koppelen wilt aan de **Resource-ID** vak dat verscheen toen u het selectievakje is ingeschakeld. De resource-ID u moet voor een virtueel netwerk dat zich in dezelfde of [ondersteund verschillende](#requirements-and-constraints) Azure [regio](https://azure.microsoft.com/regions) als dit virtuele netwerk. De volledige resource-ID ziet er ongeveer /subscriptions/<Id>/resourceGroups/ < resource-group-name > /providers/Microsoft.Network/virtualNetworks/ < naam-virtueel netwerk->. U kunt de resource-ID voor een virtueel netwerk ophalen door de eigenschappen voor een virtueel netwerk weer te geven. Zie voor meer informatie over de eigenschappen voor een virtueel netwerk, [virtuele netwerken beheren](manage-virtual-network.md#view-virtual-networks-and-settings). Als het abonnement gekoppeld aan een andere Azure Active Directory-tenant dan het abonnement met het virtuele netwerk dat u de peering is vanuit maakt, moet u eerst een gebruiker toevoegen vanaf elke tenant als een [gastgebruiker](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) in de tegengestelde tenant.
    - **Abonnement:** Selecteer de [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) van het virtuele netwerk dat u koppelen wilt aan. Een of meer abonnementen worden weergegeven, afhankelijk van het aantal abonnementen uw account, leestoegang tot heeft. Als u dit selectievakje is ingeschakeld de **Resource-ID** selectievakje, deze instelling is niet beschikbaar.
    - **Virtueel netwerk:** Selecteer het virtuele netwerk dat u koppelen wilt aan. U kunt een virtueel netwerk die zijn gemaakt via een Azure-implementatiemodel selecteren. Als u een virtueel netwerk in een andere regio selecteren wilt, moet u een virtueel netwerk in een [ondersteunde regio](#cross-region). U moet leestoegang tot het virtuele netwerk om te worden weergegeven in de lijst hebben. Als een virtueel netwerk wordt weergegeven, maar lichter gekleurd weergegeven, is het mogelijk omdat de adresruimte voor het virtuele netwerk met de adresruimte voor dit virtuele netwerk overlapt. Als het virtuele netwerk adresruimten elkaar overlappen, kunnen ze kunnen niet worden gekoppeld. Als u dit selectievakje is ingeschakeld de **Resource-ID** selectievakje, deze instelling is niet beschikbaar.
    - **Toegang tot het virtuele netwerk toestaan:** Selecteer **ingeschakeld** (standaard) als u wilt inschakelen van communicatie tussen de twee virtuele netwerken. Inschakelen van communicatie tussen virtuele netwerken, kunt resources die zijn verbonden met een van beide virtueel netwerk om te communiceren met elkaar worden verbonden met de bandbreedte en de latentie als ze zijn verbonden met hetzelfde virtuele netwerk. Alle communicatie tussen resources in de twee virtuele netwerken is via het Azure particuliere netwerk. De **VirtualNetwork** servicetag voor netwerkbeveiligingsgroepen omvat het virtuele netwerk en de gekoppelde virtuele netwerk. Zie voor meer informatie over network security group servicetags [overzicht van netwerkbeveiligingsgroepen](security-overview.md#service-tags). Selecteer **uitgeschakelde** als u niet wilt dat verkeer naar het gekoppelde virtuele netwerk. U kunt bijvoorbeeld selecteren **uitgeschakelde** als u een virtueel netwerk met een ander virtueel netwerk hebt gekoppeld, maar soms wilt uitschakelen van de verkeersstroom tussen de twee virtuele netwerken. U merkt u misschien in-/ uitschakelen is gemakkelijker dan verwijderen en opnieuw peerings te maken. Wanneer deze instelling is uitgeschakeld, wordt verkeer niet stromen tussen de gekoppelde virtuele netwerken.
    - **Doorgestuurd verkeer toestaan:** Schakel dit selectievakje in om verkeer te staan *doorgestuurd* door een virtueel netwerkapparaat in een virtueel netwerk (die niet afkomstig zijn van het virtuele netwerk) aan de stroom in dit virtuele netwerk via een peering. Denk bijvoorbeeld drie virtuele netwerken met de naam Spoke1 Spoke2 en Hub. Een peering tussen elk knooppunt virtueel netwerk en de virtuele Hub-netwerk bestaat, maar peerings niet bestaan tussen de virtuele spoke-netwerken. Een virtueel netwerkapparaat is geïmplementeerd in het Hub-netwerk en gebruiker gedefinieerde routes worden toegepast op elk knooppunt virtueel netwerk die verkeer tussen de subnetten via het netwerk virtuele apparaat routeren. Als dit selectievakje niet is ingeschakeld voor de peering tussen elk knooppunt virtueel netwerk en de virtuele hub-netwerk, wordt verkeer niet stromen tussen de virtuele spoke-netwerken omdat de hub het verkeer tussen de virtuele netwerken wordt doorgestuurd. Inschakelen van deze mogelijkheid kunt u het doorgestuurde verkeer via de peering, maakt het geen een gebruiker gedefinieerde routes of virtuele netwerkapparaten. Gebruiker gedefinieerde routes en virtuele netwerkapparaten worden afzonderlijk gemaakt. Meer informatie over [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined). U hoeft niet te controleren met deze instelling als verkeer tussen virtuele netwerken via een Azure VPN-Gateway wordt doorgestuurd.
    - **Gatewayoverdracht toestaan:** Schakel dit selectievakje in als u een virtuele netwerkgateway die is gekoppeld aan dit virtuele netwerk hebt en wilt toestaan dat verkeer van het gekoppelde virtuele netwerk door de gateway kunnen stromen. Dit virtuele netwerk kan bijvoorbeeld worden gekoppeld aan een on-premises netwerk via een virtuele netwerkgateway. De gateway kan een ExpressRoute of VPN-gateway zijn. Dit selectievakje inschakelt, kunt verkeer van het gekoppelde virtuele netwerk stromen via de gateway die is gekoppeld aan dit virtuele netwerk naar de on-premises netwerk. Als u dit selectievakje inschakelt, kan niet het gekoppelde virtuele netwerk een geconfigureerde gateway hebben. Het gekoppelde virtuele netwerk moet hebben de **externe gateways gebruiken** vakje geselecteerd bij het instellen van de peering vanuit het virtuele netwerk naar dit virtuele netwerk. Als u laat dit selectievakje uitgeschakeld (standaard), verkeer van de gekoppelde virtuele netwerk nog steeds stromen aan dit virtuele netwerk, maar door een virtuele netwerkgateway die is gekoppeld aan dit virtuele netwerk niet kan stromen. Als de peering tussen een virtueel netwerk (Resource Manager) en een virtueel netwerk (klassiek), wordt de gateway moet zich in het virtuele netwerk (Resource Manager). U kunt deze optie niet inschakelen als u bent peering van virtuele netwerken in verschillende regio's.

       Naast het doorsturen van verkeer naar een on-premises netwerk, kan een VPN-gateway het netwerkverkeer tussen virtuele netwerken die zijn gekoppeld met het virtuele netwerk dat de gateway, zonder dat de virtuele netwerken dat wordt moet worden gepeerd met elkaar doorsturen. Een VPN-gateway gebruiken voor het doorsturen van verkeer is handig als u wilt gebruiken een VPN-gateway in een hub (Zie de hub en spoke-voorbeeld beschreven voor **doorgestuurd verkeer toestaan**) virtueel netwerk om verkeer te routeren tussen virtuele spoke-netwerken die niet gekoppeld aan elkaar worden verbonden. Zie voor meer informatie over het toestaan van het gebruik van een gateway voor de doorvoer, [configureren van een VPN-gateway voor de doorvoer in een virtueel-netwerkpeering](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit scenario vereist de implementatie van de gebruiker gedefinieerde routes die de virtuele netwerkgateway als het volgende hoptype opgeven. Meer informatie over [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined). U kunt alleen een VPN-gateway opgeven als een volgende hoptype in een door de gebruiker gedefinieerde route, u kunt een ExpressRoute-gateway niet opgeven als het volgende hoptype in een door de gebruiker gedefinieerde route. U kunt deze optie niet inschakelen als u bent peering van virtuele netwerken in verschillende regio's.

    - **Externe gateways gebruiken:** Schakel dit selectievakje in om toe te staan van verkeer van dit virtuele netwerk door een virtuele netwerkgateway die is gekoppeld aan het virtuele netwerk die bent u peering met stromen. Het virtuele netwerk dat u bent peering met heeft bijvoorbeeld een VPN-gateway die zijn gekoppeld waarmee communicatie met een on-premises netwerk.  Dit selectievakje inschakelt, kunt verkeer van dit virtuele netwerk door de VPN-gateway die is gekoppeld aan het gekoppelde virtuele netwerk kunnen stromen. Als u dit selectievakje inschakelt, het gekoppelde virtuele netwerk moet een virtuele netwerkgateway die is gekoppeld aan deze hebben en moet de **gatewayoverdracht toestaan** selectievakje is ingeschakeld. Als u dit vak is niet ingeschakeld (standaard laat), het verkeer van het gekoppelde virtuele netwerk nog steeds aan dit virtuele netwerk stromen kan, maar het kan geen stroom door een virtuele netwerkgateway die zijn gekoppeld aan dit virtuele netwerk.
    Maar voor één peer voor dit virtuele netwerk, kan deze instelling is ingeschakeld hebben.

        U kunt geen externe gateways gebruiken als u al een gateway hebt geconfigureerd in uw virtuele netwerk. U kunt deze optie niet inschakelen als u bent peering van virtuele netwerken in verschillende regio's. Zie voor meer informatie over het gebruik van een gateway voor de doorvoer, [een VPN-gateway voor de doorvoer in een virtueel netwerk-peering configureren](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

7. Selecteer **OK** om toe te voegen de peering naar het virtuele netwerk dat u hebt geselecteerd.

Zie voor stapsgewijze instructies voor het implementeren van peering tussen virtuele netwerken in verschillende abonnementen en implementatiemodellen [Vervolgstappen](#next-steps).

### <a name="commands"></a>Opdrachten

- **Azure CLI**: [az network vnet-peering maken](/cli/azure/network/vnet/peering)
- **PowerShell**: [Add-AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Weergeven of wijzigen van instellingen van peering

Voordat u een peering, raken met de vereisten en beperkingen en [benodigde machtigingen](#permissions).

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze. Schakel niet **virtuele netwerken (klassiek)** als deze wordt weergegeven in de lijst, zoals u kunt geen maken een peering van een virtueel netwerk dat is geïmplementeerd via het klassieke implementatiemodel.
2. Selecteer het virtuele netwerk in de lijst die u wilt wijzigen van instellingen van peering voor.
3. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u wilt wijzigen van instellingen van peering voor.
4. Onder **instellingen**, selecteer **Peerings**.
5. De peering selecteren die u wilt weergeven of wijzigen van instellingen voor.
6. De juiste instelling wijzigen. Meer informatie over de opties voor elke instelling in [stap 6](#add-peering) van een peering maken.
7. Selecteer **Opslaan**.

**Opdrachten**

- **Azure CLI**: [az network vnet peering list](/cli/azure/network/vnet/peering) aan lijst peerings voor een virtueel netwerk, [az network vnet peering show](/cli/azure/network/vnet/peering) om instellingen voor een specifieke peering weer te geven en [az-netwerk vnet peering update](/cli/azure/network/vnet/peering) peering-instellingen te wijzigen. |
- **PowerShell**: [Get-AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering) peering weergave-instellingen op te halen en [Set AzVirtualNetworkPeering](/powershell/module/az.network/set-azvirtualnetworkpeering) om instellingen te wijzigen.

## <a name="delete-a-peering"></a>Een peering verwijderen

Voordat u een peering te verwijderen, moet uw account heeft de [benodigde machtigingen](#permissions).

Wanneer een peering wordt verwijderd, wordt verkeer van een virtueel netwerk niet langer stroomt naar het gekoppelde virtuele netwerk. Wanneer virtuele netwerken die zijn geïmplementeerd via Resource Manager zijn gekoppeld, heeft elk virtueel netwerk een peering voor het virtuele netwerk. Hoewel de peering van een virtueel netwerk verwijderen, schakelt de communicatie tussen de virtuele netwerken, worden niet verwijderd de peering vanuit het virtuele netwerk. De peeringstatus voor de peering die deel uitmaakt van het virtuele netwerk **verbroken**. U de peering niet opnieuw totdat u opnieuw maken van de peering in het eerste virtuele netwerk en de peeringstatus van beide virtuele netwerken wijzigingen in *verbonden*.

Als u virtuele netwerken om te communiceren soms, maar niet altijd, in plaats van een peering te verwijderen, kunt u instellen de **toestaan van toegang tot het virtuele netwerk** instelt op **uitgeschakelde** in plaats daarvan. Voor meer informatie over hoe een peering sectie van dit artikel lezen in stap 6 van de maken. Mogelijk vindt u uitschakelen en verwijderen en opnieuw maken van peerings gemakkelijker dan netwerktoegang in te schakelen.

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze. Schakel niet **virtuele netwerken (klassiek)** als deze wordt weergegeven in de lijst, zoals u kunt geen maken een peering van een virtueel netwerk dat is geïmplementeerd via het klassieke implementatiemodel.
2. Selecteer het virtuele netwerk in de lijst die u verwijderen van een peering wilt voor.
3. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u verwijderen van een peering wilt voor.
4. Onder **instellingen**, selecteer **Peerings**.
5. Aan de rechterkant van de peering die u wilt verwijderen, selecteert u **...** , selecteer **verwijderen**en selecteer vervolgens **Ja** verwijderen van de peering vanuit het eerste virtuele netwerk.
6. De vorige stappen als u wilt verwijderen van de peering vanuit het virtuele netwerk in de peering.

**Opdrachten**

- **Azure CLI**: [az network vnet peering delete](/cli/azure/network/vnet/peering)
- **PowerShell**: [Remove-AzVirtualNetworkPeering](/powershell/module/az.network/remove-azvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Vereisten en beperkingen

- <a name="cross-region"></a>U kunt virtuele netwerken in de dezelfde regio bevinden, of verschillende regio's koppelen. Peering van virtuele netwerken in verschillende regio's wordt ook aangeduid als *wereldwijde VNet-Peering*. 
- Bij het maken van een wereldwijde peering, worden de gekoppelde virtuele netwerken kunnen bestaan in een openbare cloud van Azure-regio, China-cloud-regio's of Government cloud-regio's. U kan niet koppelen tussen clouds. Bijvoorbeeld, kan niet een VNet in Azure openbare cloud worden gekoppeld aan een VNet in Azure China-cloud.
- Resources in een virtueel netwerk kunnen niet communiceren met de front-end-IP-adres van een algemene interne load balancer in een wereldwijd gekoppelde virtuele netwerk. Ondersteuning voor de basisversie van Load Balancer bestaat alleen binnen dezelfde regio. Ondersteuning voor de standaardversie van Load Balancer bestaat voor zowel, VNet-Peering en wereldwijde VNet-Peering.
- U kunt externe gateways gebruiken of gatewayoverdracht toestaan in wereldwijd gekoppelde virtuele netwerken in de Preview-versie. De Preview-versie is beschikbaar in alle Azure-regio's, China-cloud-regio's en Government cloud-regio's. Er is geen opname in de whitelist is vereist. U kunt in de Preview-versie via de CLI, PowerShell, sjablonen of API testen. Portal wordt niet ondersteund in de Preview-versie.
- De virtuele netwerken kunnen zich in de dezelfde of verschillende abonnementen behoren. Wanneer u virtuele netwerken in verschillende abonnementen koppelen, kunnen beide abonnementen worden gekoppeld aan dezelfde of verschillende Azure Active Directory-tenant. Als u nog een AD-tenant hebt, kunt u [maakt u er een](../active-directory/develop/quickstart-create-new-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json-a-new-azure-ad-tenant). Ondersteuning voor peering tussen virtuele netwerken van abonnementen die zijn gekoppeld aan andere Azure Active Directory-tenants is niet beschikbaar in de Portal. U kunt de CLI, PowerShell of sjablonen gebruiken.
- De virtuele netwerken die u op hetzelfde niveau moeten niet-overlappende IP-adresruimten hebben.
- U kan niet-adresbereiken aan toevoegen of verwijderen-adresbereiken in de adresruimte van een virtueel netwerk van wanneer een virtueel netwerk is gekoppeld aan een ander virtueel netwerk. Als u wilt toevoegen of verwijderen-adresbereiken, de peering verwijderen, toevoegen of verwijderen van de adresbereiken, maakt u opnieuw de peering. -Adresbereiken aan toevoegen of verwijderen-adresbereiken van virtuele netwerken, Zie [virtuele netwerken beheren](manage-virtual-network.md).
- U kunt twee virtuele netwerken die zijn geïmplementeerd via Resource Manager of een virtueel netwerk dat is geïmplementeerd via Resource Manager met een virtueel netwerk dat is geïmplementeerd via het klassieke implementatiemodel kunt koppelen. U kunt geen peer-twee virtuele netwerken die zijn gemaakt via het klassieke implementatiemodel. Als u niet bekend met Azure-implementatiemodellen bent, leest de [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. U kunt [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) gebruiken om twee virtuele netwerken te koppelen die zijn gemaakt via het klassieke implementatiemodel.
- Wanneer u twee virtuele netwerken koppelt die zijn gemaakt via Resource Manager, moet voor elk virtueel netwerk een peering worden geconfigureerd. U ziet een van de volgende typen voor de peeringstatus: 
  - *Gestart:* Wanneer u de peering naar het tweede virtuele netwerk van het eerste virtuele netwerk hebt gemaakt, is het de peeringstatus *gestart*. 
  - *Verbonden:* Wanneer u de peering vanuit het tweede virtuele netwerk naar het eerste virtuele netwerk hebt gemaakt, is het de peeringstatus *verbonden*. Als u de peeringstatus voor de eerste virtuele netwerk bekijkt, ziet u de status ervan gewijzigd van *gestart* naar *verbonden*. De peering is niet tot stand gebracht totdat de peeringstatus van beide virtueel netwerk-peerings *verbonden*.
- Wanneer een virtueel netwerk gemaakt via Resource Manager met een virtueel netwerk die zijn gemaakt via het klassieke implementatiemodel, configureert u alleen een peering voor het virtuele netwerk dat is geïmplementeerd via Resource Manager. U kunt niet configureren voor een virtueel netwerk (klassiek) of tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel-peering. Wanneer u de peering vanuit het virtuele netwerk (Resource Manager) naar het virtuele netwerk (klassiek) hebt gemaakt, is het de peeringstatus *bijwerken*, wordt binnenkort gewijzigd in *verbonden*.
- Een peering tot stand is gebracht tussen twee virtuele netwerken. Peerings zijn niet overdraagbaar. Als u peerings tussen maken:
  - VirtualNetwork1 & VirtualNetwork2
  - VirtualNetwork2 & VirtualNetwork3

  Er is geen peering tussen VirtualNetwork1 en VirtualNetwork3 via VirtualNetwork2. Als u maken van een virtueel netwerk voor peering tussen VirtualNetwork1 en VirtualNetwork3 wilt, hebt u een peering tussen VirtualNetwork1 en VirtualNetwork3 wilt maken.
- Namen in gekoppelde virtuele netwerken met behulp van standaard Azure-naamomzetting kan niet worden omgezet. Als u wilt omzetten van namen in andere virtuele netwerken, moet u [Azure DNS voor persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of een aangepaste DNS-server. Zie voor meer informatie over het instellen van uw eigen DNS-server, [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
- Resources in gekoppelde virtuele netwerken in dezelfde regio kunnen met elkaar communiceren met de bandbreedte en de latentie alsof ze zich in hetzelfde virtuele netwerk. De grootte van elke virtuele machine heeft echter een eigen maximale netwerkbandbreedte. Zie voor meer informatie over de maximale netwerkbandbreedte voor verschillende VM-groottes, [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) grootten van virtuele machines.
- Een virtueel netwerk kan worden gekoppeld aan een ander virtueel netwerk, en ook zijn verbonden met een ander virtueel netwerk met de gateway van een Azure-netwerk. Wanneer virtuele netwerken zijn verbonden via peering en een gateway, wordt het verkeer tussen de virtuele netwerken loopt via de peering-configuratie, in plaats van de gateway.
- Punt-naar-Site VPN-clients moeten opnieuw worden gedownload nadat peering op virtueel netwerk is geconfigureerd om te controleren of dat de nieuwe routes worden gedownload naar de client.
- Er wordt een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van een virtueel netwerk-peering. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/virtual-network) voor meer informatie.

## <a name="permissions"></a>Machtigingen

De accounts die u gebruiken om te werken met virtueel-netwerkpeering moeten worden toegewezen aan de volgende rollen:

- [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor): Voor een virtueel netwerk dat is geïmplementeerd via Resource Manager.
- [Inzender voor klassieke netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor): Voor een virtueel netwerk dat is geïmplementeerd via het klassieke implementatiemodel.

Als uw account niet aan een van de vorige rollen toegewezen is, moet deze worden toegewezen aan een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die de noodzakelijke acties op basis van de volgende tabel is toegewezen:

| Bewerking                                                          | Name |
|---                                                              |---   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write  | Vereist voor het maken van de peering van het virtueel netwerk A met virtual network B. virtueel moet netwerk A een virtueel netwerk (Resource Manager)          |
| Microsoft.Network/virtualNetworks/peer/action                   | Vereist voor het maken van een peering van virtueel netwerk B (Resource Manager) naar een virtueel netwerk A                                                       |
| Microsoft.ClassicNetwork/virtualNetworks/peer                   | Vereist voor het maken van een peering van virtueel netwerk B (klassiek) naar een virtueel netwerk A                                                                |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read   | Lees een peering op virtueel netwerk   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Een virtueel netwerk-peering verwijderen |

## <a name="next-steps"></a>Volgende stappen

- Peering in virtuele netwerken kan tot stand worden gebracht tussen virtuele netwerken die zijn gemaakt via hetzelfde implementatiemodel of via verschillende implementatiemodellen die tot hetzelfde abonnement of tot verschillende abonnementen behoren. Volg een zelfstudie voor een van de volgende scenario's:

  |Azure-implementatiemodel             | Abonnement  |
  |---------                          |---------|
  |Beide in Resource Manager              |[Hetzelfde](tutorial-connect-virtual-networks-portal.md)|
  |                                   |[Verschillend](create-peering-different-subscriptions.md)|
  |Eén in Resource Manager, één klassiek  |[Hetzelfde](create-peering-different-deployment-models.md)|
  |                                   |[Verschillend](create-peering-different-deployment-models-subscriptions.md)|

- Meer informatie over het maken van een [hub-en-spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Een virtueel netwerk-peering maken met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken

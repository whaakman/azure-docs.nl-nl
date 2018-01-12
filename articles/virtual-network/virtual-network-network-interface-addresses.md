---
title: Configureer IP-adressen voor een Azure-netwerk-interface | Microsoft Docs
description: Informatie over het toevoegen, wijzigen en verwijderen van persoonlijke en openbare IP-adressen voor een netwerkinterface.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 637b380dacc91e4ad55044c1d92936be2435138d
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Toevoegen, wijzigen of verwijderen van IP-adressen voor een Azure-netwerk-interface

Informatie over het toevoegen, wijzigen en verwijderen van openbare en particuliere IP-adressen voor een netwerkinterface. Privé IP-adressen die zijn toegewezen aan een netwerkinterface inschakelen voor een virtuele machine om te communiceren met andere resources in een Azure-netwerk en verbonden netwerken. Een persoonlijke IP-adres kan ook uitgaande communicatie met Internet via een onvoorspelbare IP-adres. Een [openbaar IP-adres](virtual-network-public-ip-address.md) toegewezen aan een netwerk interface kunnen binnenkomende communicatie met een virtuele machine vanaf het Internet. Het adres kan ook uitgaande communicatie van de virtuele machine met het Internet via een voorspelbare IP-adres. Zie voor meer informatie [inzicht in uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Als u nodig hebt voor het maken, wijzigen of verwijderen van een netwerkinterface, leest u de [beheren van een netwerkinterface](virtual-network-network-interface.md) artikel. Als u wilt netwerkinterfaces toevoegen of verwijderen van netwerkinterfaces van een virtuele machine, lees de [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md) artikel. 


## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u alle stappen in elke sectie van dit artikel uitvoert:

- Controleer de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel voor meer informatie over limieten voor openbare en particuliere IP-adressen.
- Aanmelden bij de Azure [portal](https://portal.azure.com), Azure-opdrachtregelinterface (CLI) of Azure PowerShell gebruiken met een Azure-account. Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als PowerShell-opdrachten voor het uitvoeren van taken in dit artikel [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure PowerShell-commandlets geïnstalleerd. Typ voor hulp bij PowerShell-opdrachten, met voorbeelden, `get-help <command> -full`.
- Als Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel [installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com).

## <a name="add-ip-addresses"></a>IP-adressen toevoegen

U kunt toevoegen als veel [persoonlijke](#private) en [openbare](#public) [IPv4](#ipv4) adressen zo nodig aan een netwerkinterface binnen de grenzen die worden vermeld in de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel. U kunt de portal niet gebruiken voor een IPv6-adres toevoegen aan een bestaande netwerkinterface (Hoewel de portal kunt u een persoonlijk IPv6-adres toevoegen aan een netwerkinterface bij het maken van de netwerkinterface). U kunt PowerShell of de CLI gebruiken een persoonlijk IPv6-adres toevoegen aan een [secundaire IP-configuratie](#secondary) (zo lang er zijn geen bestaande secundaire IP-configuraties) voor een bestaande netwerkinterface die niet is gekoppeld aan een virtuele machine. U kunt elk hulpprogramma niet gebruiken een openbare IPv6-adres toevoegen aan een netwerkinterface. Zie [IPv6](#ipv6) voor meer informatie over het gebruik van IPv6-adressen. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt toevoegen voor een IPv4-adres.
4. Klik op **IP-configuraties** in de **instellingen** sectie van de blade voor de netwerkinterface die u hebt geselecteerd.
5. Klik op **+ toevoegen** in de blade wordt geopend voor IP-configuraties.
6. Geef het volgende en klik vervolgens op **OK** sluiten de **toevoegen IP-configuratie** blade:

    |Instelling|Vereist?|Details|
    |---|---|---|
    |Naam|Ja|Moet uniek zijn voor de netwerkinterface|
    |Type|Ja|Omdat u een IP-configuratie aan een bestaande netwerkinterface toevoegt, en elke netwerkinterface moet een [primaire](#primary) IP-configuratie, de enige optie is **secundaire**.|
    |Methode voor het privé IP-adres toewijzen|Ja|[**Dynamische**](#dynamic): Azure toegewezen adres van de volgende beschikbaar voor het subnet-adresbereik in de netwerkinterface is geïmplementeerd. [**Statische**](#static): toewijzen van een ongebruikt adres voor het subnet-adresbereik in de netwerkinterface is geïmplementeerd.|
    |Openbaar IP-adres|Nee|**Uitgeschakeld:** geen openbare IP-adres-resource is momenteel gekoppeld aan de IP-configuratie. **Ingeschakeld:** Selecteer een bestaand openbaar IPv4-IP-adres of een nieuwe maken. Lees voor meer informatie over het maken van een openbaar IP-adres, de [openbare IP-adressen](virtual-network-public-ip-address.md#create-a-public-ip-address) artikel.|
7. Secundaire privé IP-adressen handmatig toevoegen aan het besturingssysteem van de virtuele machine door de instructies in te voeren de [meerdere IP-adressen toewijzen aan virtuele machine besturingssystemen](virtual-network-multiple-ip-addresses-portal.md#os-config) artikel. Zie [persoonlijke](#private) IP-adressen voor speciale overwegingen voordat u IP-adressen handmatig toevoegt aan een virtuele machine-besturingssysteem. Niet alle openbare IP-adressen aan het besturingssysteem van de virtuele machine toevoegen.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic ip-configuratie maken](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Voeg AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>Instellingen voor IP-adres wijzigen

U kunt moet wijzigen van de methode voor het toewijzen van een IPv4-adres wijzigen van het statische IPv4-adres, of wijzigen van het openbare IP-adres is toegewezen aan een netwerkinterface. Als u de particulier IPv4-adres van een secundaire IP-configuratie die is gekoppeld aan een secundaire netwerkinterface in een virtuele machine wilt wijzigen (meer informatie over [primaire en secundaire netwerkinterfaces](virtual-network-network-interface-vm.md)), plaats de virtuele machine in de gestopt (toewijzing ongedaan gemaakt) voordat u de volgende stappen: 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt weergeven of wijzigen van IP-adresinstellingen voor.
4. Klik op **IP-configuraties** in de **instellingen** sectie van de blade voor de netwerkinterface die u hebt geselecteerd.
5. Klik op de IP-configuratie die u wilt wijzigen in de lijst in de blade wordt geopend voor IP-configuraties.
6. Wijzig de instellingen naar wens, als u de informatie over de instellingen die in stap 6 van de [toevoegen van een IP-configuratie](#create-ip-config) sectie van dit artikel. Klik op **opslaan** sluit de blade voor de IP-configuratie die u hebt gewijzigd.

>[!NOTE]
>Als de primaire netwerkinterface meerdere IP-configuraties heeft en u de persoonlijke IP-adres van de primaire IP-configuratie wijzigen, moet u de primaire en secundaire IP-adressen aan de netwerkinterface in Windows (niet vereist voor Linux) handmatig opnieuw toewijzen. Lezen voor het IP-adressen handmatig toewijzen aan een netwerkinterface binnen een besturingssysteem, de [meerdere IP-adressen toewijzen aan virtuele machines](virtual-network-multiple-ip-addresses-portal.md#os-config) artikel. Zie [persoonlijke](#private) IP-adressen voor speciale overwegingen voordat u IP-adressen handmatig toevoegt aan een virtuele machine-besturingssysteem. Niet alle openbare IP-adressen aan het besturingssysteem van de virtuele machine toevoegen.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic IP-configuratie bijwerken](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>IP-adressen verwijderen

U kunt verwijderen [persoonlijke](#private) en [openbare](#public) IP-adressen uit een netwerkinterface, maar een netwerkinterface moet altijd ten minste één particulier IPv4-adres toegewezen.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt verwijderen van IP-adressen uit.
4. Klik op **IP-configuraties** in de **instellingen** sectie van de blade voor de netwerkinterface die u hebt geselecteerd.
5. Met de rechtermuisknop op een [secundaire](#secondary) IP-configuratie (u kunt niet verwijderen de [primaire](#primary) configuration) u wilt verwijderen, klikt u op **verwijderen**, klikt u vervolgens op **Ja** om de verwijdering te bevestigen. Als de configuratie van een openbare IP-adres resource die is gekoppeld aan deze had, de bron is ontkoppeld van de IP-configuratie, maar de resource is niet verwijderd.
6. Sluit de **IP-configuraties** blade.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic IP-configuratie verwijderen](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Verwijder AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>IP-configuraties

[Persoonlijke](#private) en (optioneel) [openbare](#public) IP-adressen zijn toegewezen aan een of meer IP-configuraties toegewezen aan een netwerkinterface. Er zijn twee soorten IP-configuraties:

### <a name="primary"></a>Primair

Elke netwerkinterface is één primaire IP-configuratie worden toegewezen. Een primaire IP-configuratie:

- Heeft een [persoonlijke](#private) [IPv4](#ipv4) adres toegewezen. U kunt een persoonlijk niet toewijzen [IPv6](#ipv6) adres aan een primaire IP-configuratie.
- Mogelijk beschikt over een [openbare](#public) IPv4-adres toegewezen. U kunt een openbare IPv6-adres toewijzen aan een primaire of secundaire IP-configuratie. U kunt wel een openbare IPv6-adres toewijzen aan een Azure load balancer, die saldo verkeer naar persoonlijke IPv6-adres van een virtuele machine kunt laden. Zie voor meer informatie [details en beperkingen voor IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Secundair

Naast een primaire IP-configuratie, een netwerkinterface kan nul of meer secundaire IP-configuraties hebt toegewezen. Een secundaire IP-configuratie:

- Een particulier IPv4- of IPv6-adres moet hebben om. Als het adres IPv6 is, kan de netwerkinterface slechts één secundaire IP-configuratie hebben. Als het adres IPv4 is, is de netwerkinterface mogelijk meerdere secundaire IP-configuraties die zijn toegewezen. Zie voor meer informatie over hoeveel persoonlijke en openbare IPv4-adressen kunnen worden toegewezen aan een netwerkinterface, de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel.  
- Ook kan een openbaar IPv4-adres hebben om, als de persoonlijke IP-adres IPv4. Als de privé IP-adres IPv6 is, kunt u een openbaar IPv4- of IPv6-adres niet toewijzen aan de IP-configuratie. Meerdere IP-adressen toewijzen aan een netwerkinterface is nuttig in scenario's, zoals:
    - Het hosten van meerdere websites of services met verschillende IP-adressen en SSL-certificaten op één server.
    - Een virtuele machine fungeert als een virtueel netwerkapparaat, zoals een firewall of een load balancer.
    - De mogelijkheid om een van de persoonlijke IPv4-adressen voor een van de netwerkinterfaces toevoegen aan een Load Balancer van Azure back-end-adresgroep. In het verleden kan alleen het primaire IPv4-adres voor de primaire netwerkinterface worden toegevoegd aan een back-end-pool. Zie voor meer informatie over het verdelen van meerdere IPv4-configuraties, de [taakverdeling voor configuraties met meerdere IP-](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. 
    - De mogelijkheid om te laden in evenwicht één IPv6-adres is toegewezen aan een netwerkinterface. Zie voor meer informatie over het verdelen naar een persoonlijke IPv6-adres, de [taakverdeling van IPv6-adressen](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.


## <a name="address-types"></a>Adrestypen

U kunt de volgende soorten IP-adressen aan een [IP-configuratie](#ip-configurations):

### <a name="private"></a>Privé

Persoonlijke [IPv4](#ipv4) adressen inschakelen in een virtuele machine om te communiceren met andere resources in een virtueel netwerk of andere aangesloten netwerken. Een virtuele machine kan niet worden gecommuniceerd naar binnenkomende noch kan de virtuele machine communiceren met een particulier uitgaande [IPv6](#ipv6) adres, met één uitzondering. Een virtuele machine kan communiceren met de Azure load balancer met een IPv6-adres. Zie voor meer informatie [details en beperkingen voor IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

De Azure-DHCP-servers toewijzen particulier IPv4-adres voor de [primaire IP-configuratie](#primary) van de interface van de Azure-netwerk aan de netwerkinterface in het besturingssysteem van de virtuele machine. Tenzij nodig, moet u nooit handmatig instellen het IP-adres van een netwerkinterface in het besturingssysteem van de virtuele machine. 

> [!WARNING]
> Als het IPv4-adres instellen als de primaire IP-adres van een netwerkinterface in het besturingssysteem van een virtuele machine ooit anders dan de particulier IPv4-adres dat is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface is gekoppeld aan een virtuele machine in Azure, moet u verbinding met de virtuele machine verliezen.

Er zijn scenario's waarin het nodig zijn voor het IP-adres van een netwerkinterface in het besturingssysteem van de virtuele machine handmatig instellen. Bijvoorbeeld, moet u de primaire en secundaire IP-adressen van een Windows-besturingssysteem handmatig instellen als u meerdere IP-adressen toevoegt aan een virtuele machine van Azure. Voor een virtuele Linux-machine moet u mogelijk alleen handmatig in te stellen de secundaire IP-adressen. Zie [toevoegen IP-adressen naar een VM-besturingssysteem](virtual-network-multiple-ip-addresses-portal.md#os-config) voor meer informatie. Als u ooit moet het adres dat is toegewezen aan een IP-configuratie wijzigen, is het raadzaam dat u:

1. Zorg ervoor dat de virtuele machine een adres van de Azure-DHCP-servers ontvangt. Zodra u hebt, wordt de toewijzing van het IP-adres wijzigen in DHCP in het besturingssysteem en de virtuele machine opnieuw opstarten.
2. Gestopt (toewijzing ongedaan maken) de virtuele machine.
3. Wijzig het IP-adres voor de IP-configuratie in Azure.
4. Hiermee wordt de virtuele machine gestart.
5. [Configureer handmatig](virtual-network-multiple-ip-addresses-portal.md#os-config) de secundaire IP-adressen binnen het besturingssysteem (en ook de primaire IP-adres in Windows) overeenkomt met wat u instellen in Azure.
 
Door de vorige stappen hebt uitgevoerd, de privé IP-adres is toegewezen aan de netwerkinterface in Azure, en binnen een virtuele machine-besturingssysteem, moet u hetzelfde blijven. Om bij te houden welke virtuele machines binnen uw abonnement die u handmatig IP-adressen binnen een besturingssysteem voor hebt ingesteld, overweeg een Azure [tag](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) aan de virtuele machines. U kunt gebruiken ' IP-adrestoewijzing: statische ', bijvoorbeeld. Op deze manier kunt u gemakkelijk vinden de virtuele machines binnen uw abonnement dat u het IP-adres voor in het besturingssysteem handmatig hebt ingesteld.

Naast het inschakelen van een virtuele machine om te communiceren met andere bronnen binnen de dezelfde of verbonden virtuele netwerken kan een particulier IP-adres ook een virtuele machine om te communiceren uitgaand naar het Internet. Uitgaande verbindingen zijn Bronnetwerk adres vertaald door Azure een onvoorspelbare openbare IP-adres. Lees voor meer informatie over Azure uitgaande internetverbinding, de [Azure uitgaande internetverbinding](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. U kan niet communiceren inkomend verkeer naar een virtuele machine privé IP-adres van het Internet. Als uw uitgaande verbindingen een voorspelbare openbare IP-adres vereisen, koppelt een openbare IP-adres resource aan een netwerkinterface.

### <a name="public"></a>Openbaar

Openbare IP-adressen toegewezen via een openbare IP-adres resource voor het inschakelen van binnenkomende verbindingen met een virtuele machine vanaf het Internet. Uitgaande verbindingen via Internet gebruiken een voorspelbare IP-adres. Zie [inzicht in uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie. U mag een openbaar IP-adres toewijzen aan een IP-configuratie, maar zijn niet vereist voor. Als u niet een openbaar IP-adres aan een virtuele machine toewijst door te koppelen van de bron van een openbare IP-adres, communiceren de virtuele machine nog steeds uitgaand naar het Internet. In dit geval wordt het privé IP-adres Bronnetwerk adres vertaald door Azure een onvoorspelbare openbare IP-adres. Zie voor meer informatie over openbare IP-adresbronnen, [bron van het openbare IP-adres](virtual-network-public-ip-address.md).

Er gelden beperkingen voor het aantal persoonlijke en openbare IP-adressen die u aan een netwerkinterface toewijzen kunt. Lees voor meer informatie de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel.

> [!NOTE]
> Azure vertaalt het privé IP-adres van een virtuele machine naar een openbare IP-adres. Besturingssysteem van een virtuele machine is daardoor niet bewust zijn van een openbaar IP-adres is toegewezen, zodat er geen nodig ooit handmatig toewijzen van een openbaar IP-adres in het besturingssysteem is.

## <a name="assignment-methods"></a>Van toewijzingsmethoden

Openbare en particuliere IP-adressen worden toegewezen met behulp van een van de volgende toewijzingsmethoden:

### <a name="dynamic"></a>Dynamisch

Dynamische particuliere IPv4 en IPv6 (optioneel) adressen worden standaard toegewezen. 

- **Alleen openbare**: Azure wijst het adres van een unieke scala aan elke Azure-regio. Zie voor meer informatie over welke adresbereiken worden toegewezen aan elke regio, [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Het adres kunt wijzigen als een virtuele machine is gestopt (toewijzing ongedaan gemaakt), vervolgens opnieuw worden gestart. U kunt een openbare IPv6-adres toewijzen aan een IP-configuratie met behulp van een toewijzing van de methoden.
- **Alleen persoonlijke**: Azure de eerste vier adressen in elk subnet-adresbereik zijn gereserveerd en de adressen niet toewijzen. Azure wijst het eerstvolgende beschikbare adres in het adresbereik van het subnet toe aan een resource. Als het adresbereik van het subnet bijvoorbeeld 10.0.0.0/16 is en de adressen 10.0.0.0.4-10.0.0.14 al zijn toegewezen (.0-.3 zijn gereserveerd), wijst Azure 10.0.0.15 toe aan de resource. Dynamisch is de standaardmethode voor toewijzing. Nadat dynamische IP-adressen zijn toegewezen, worden ze alleen vrijgegeven als een netwerkinterface wordt verwijderd of wordt toegewezen aan een ander subnet binnen hetzelfde virtuele netwerk of als de toewijzingsmethode wordt gewijzigd in statisch en een ander IP-adres wordt opgegeven. Standaard wijst Azure het vorige dynamisch toegewezen adres toe als het statische adres wanneer u de toewijzingsmethode verandert van dynamisch in statisch. U kunt alleen een particulier IPv6-adres met behulp van de dynamische toewijzingsmethode toewijzen.

### <a name="static"></a>Statisch

U kunt een openbare of particuliere statische IPv4-adres (optioneel) toewijzen aan een IP-configuratie. U kunt een statische openbare of particuliere IPv6-adres toewijzen aan een IP-configuratie. Zie voor meer informatie over hoe Azure openbare statische IPv4-adressen toewijst, de [openbaar IP-adres](virtual-network-public-ip-address.md) artikel.

- **Alleen openbare**: Azure wijst het adres van een unieke scala aan elke Azure-regio. Zie voor meer informatie over welke adresbereiken worden toegewezen aan elke regio, [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Het adres verandert niet tot het openbare IP-adres resource dat wordt toegewezen aan wordt verwijderd of de toewijzingsmethode naar dynamische wordt gewijzigd. Als de bron van de openbare IP-adres gekoppeld aan een IP-configuratie is, moet deze worden ontkoppeld van de IP-configuratie voordat u de toewijzingsmethode wijzigt.
- **Alleen persoonlijke**: U selecteert en het toewijzen van een adres uit het adresbereik van het subnet. Het adres dat u toewijst, kan elk adres binnen het adresbereik van het subnet zijn dat niet een van de eerste vier adressen in het adresbereik van het subnet is en momenteel niet is toegewezen aan een andere resource in het subnet. Statische adressen worden alleen vrijgegeven als een netwerkinterface wordt verwijderd. Als u de toewijzingsmethode wijzigt in statisch, wijst Azure het eerder toegewezen statische IP-adres toe als dynamisch adres, zelfs als dat adres niet het eerstvolgende beschikbare adres in het adresbereik van het subnet is. Het adres verandert ook als de netwerkinterface wordt toegewezen aan een ander subnet binnen hetzelfde virtuele netwerk, maar als u de netwerkinterface wilt toewijzen aan een ander subnet, moet u eerst de toewijzingsmethode wijzigen van statisch in dynamisch. Nadat u de netwerkinterface hebt toegewezen aan een ander subnet, kunt u de toewijzingsmethode weer wijzigen in statisch en een IP-adres uit het adresbereik van het nieuwe subnet toewijzen.

## <a name="ip-address-versions"></a>IP-adres versies

U kunt de volgende versies opgeven bij het toewijzen van adressen:

### <a name="ipv4"></a>IPv4

Elke netwerkinterface moet een hebben [primaire](#primary) IP-configuratie met een toegewezen [persoonlijke](#private) [IPv4](#ipv4) adres. U kunt een of meer toevoegen [secundaire](#secondary) IP-configuraties die elk een particulier IPv4- en (optioneel) een IPv4-hebben [openbare](#public) IP-adres.

### <a name="ipv6"></a>IPv6

U kunt nul of één persoonlijke toewijzen [IPv6](#ipv6) adres één secundaire IP-configuratie van een netwerkinterface. De netwerkinterface kan niet alle bestaande secundaire IP-configuraties hebben. U kunt een IP-configuratie niet toevoegen met een IPv6-adres met behulp van de portal. Gebruik PowerShell of de CLI een IP-configuratie met een particulier IPv6-adres toevoegen aan een bestaande netwerkinterface. De netwerkinterface kan niet worden gekoppeld aan een bestaande virtuele machine.

> [!NOTE]
> Hoewel u een netwerkinterface met een IPv6-adres met behulp van de portal maken kunt, kan u een bestaande netwerkinterface toevoegen aan een nieuwe of bestaande virtuele machine, met behulp van de portal is. Gebruik PowerShell of de Azure CLI 2.0 een netwerkinterface maken met een particulier IPv6-adres en de netwerkinterface koppelen bij het maken van een virtuele machine. U kunt een netwerkinterface niet koppelen met een particulier IPv6-adres dat is toegewezen aan een bestaande virtuele machine. U kunt een persoonlijke IPv6-adres kan toevoegen aan een IP-configuratie voor een netwerkinterface gekoppeld aan een virtuele machine met behulp van hulpprogramma's (portal, CLI of PowerShell).

U kunt een openbare IPv6-adres toewijzen aan een primaire of secundaire IP-configuratie.

## <a name="skus"></a>Voorraadeenheden

Een openbaar IP-adres is gemaakt met de basis of standaard SKU.  Zie voor meer informatie over de verschillen SKU [openbare IP-adressen beheren](virtual-network-public-ip-address.md).

> [!NOTE]
> Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.

## <a name="next-steps"></a>Volgende stappen
Als een virtuele machine maken met verschillende IP-configuraties, leest u de volgende artikelen:

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere NIC's maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Één NIC VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Maak een enkel NIC-VM met een particulier IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

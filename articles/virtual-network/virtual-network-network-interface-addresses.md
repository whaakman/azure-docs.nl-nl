---
title: Configureer IP-adressen voor de interface van een Azure-netwerk | Microsoft Docs
description: Informatie over het toevoegen, wijzigen en verwijderen van persoonlijke en openbare IP-adressen voor een netwerkinterface.
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 716c229fbd906798d39bf4ef54ba1f47cd5bd980
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56651036"
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Toevoegen, wijzigen of verwijderen van IP-adressen voor een Azure-netwerk-interface

Informatie over het toevoegen, wijzigen en verwijderen van openbare en persoonlijke IP-adressen voor een netwerkinterface. Privé-IP-adressen die zijn toegewezen aan een netwerkinterface kunnen een virtuele machine om te communiceren met andere resources in een Azure-netwerk en verbonden netwerken. Een privé IP-adres kan ook uitgaande communicatie naar Internet met behulp van een onvoorspelbare IP-adres. Een [openbaar IP-adres](virtual-network-public-ip-address.md) toegewezen aan een netwerkinterface kunnen binnenkomende communicatie met een virtuele machine via Internet. Het adres kan ook uitgaande communicatie van de virtuele machine met het Internet via een voorspelbare IP-adres. Zie voor meer informatie, [uitleg over uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Als u nodig hebt voor het maken, wijzigen of verwijderen van een netwerkinterface, leest de [beheren van een netwerkinterface](virtual-network-network-interface.md) artikel. Als u wilt netwerkinterfaces toevoegen of verwijderen van netwerkinterfaces van een virtuele machine, lees de [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md) artikel.

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. In deze zelfstudie vereist de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [netwerk interface-machtigingen](virtual-network-network-interface.md#permissions).

## <a name="add-ip-addresses"></a>IP-adressen toevoegen

U kunt toevoegen als veel [persoonlijke](#private) en [openbare](#public) [IPv4](#ipv4) adressen zo nodig aan een netwerkinterface binnen de grenzen die worden vermeld in de [voorAzure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel. U kunt de portal niet gebruiken voor een IPv6-adres toevoegen aan een bestaande netwerkinterface (Hoewel de portal kunt u een privé-IPv6-adres aan een netwerkinterface toevoegen bij het maken van de netwerkinterface). U kunt PowerShell of de CLI gebruiken om toe te voegen een privé-IPv6-adres aan een [secundaire IP-configuratie](#secondary) (zolang er geen bestaande secundaire IP-configuraties zijn) voor een bestaande netwerkinterface die is niet gekoppeld aan een virtuele machine. U kunt elk hulpprogramma niet gebruiken voor een openbaar IPv6-adres toevoegen aan een netwerkinterface. Zie [IPv6](#ipv6) voor meer informatie over het gebruik van IPv6-adressen.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt toevoegen van een IPv4-adres voor in de lijst.
3. Onder **instellingen**, selecteer **IP-configuraties**.
4. Onder **IP-configuraties**, selecteer **+ toevoegen**.
5. Geef het volgende, en selecteer vervolgens **OK**:

   |Instelling|Vereist?|Details|
   |---|---|---|
   |Name|Ja|Moet uniek zijn voor de netwerkinterface|
   |Type|Ja|Nadat u een IP-configuratie aan een bestaande netwerkinterface toevoegt, en elke netwerkinterface moet een [primaire](#primary) IP-configuratie, de enige mogelijkheid is **secundaire**.|
   |Methode voor het privé-IP-adres toewijzen|Ja|[**Dynamische**](#dynamic): Azure wijst het eerstvolgende beschikbare adres voor het adresbereik van het subnet dat in de netwerkinterface is geïmplementeerd. [**Statische**](#static): U toewijzen een niet-gebruikte adres voor het adresbereik van het subnet dat in de netwerkinterface is geïmplementeerd.|
   |Openbaar IP-adres|Nee|**Uitgeschakeld:** Er zijn geen openbare IP-adresresource is momenteel gekoppeld aan de IP-configuratie. **Ingeschakeld:** Selecteer een bestaande openbare IPv4-adres of een nieuwe maken. Lees voor meer informatie over het maken van een openbaar IP-adres, de [openbare IP-adressen](virtual-network-public-ip-address.md#create-a-public-ip-address) artikel.|
6. Secundaire privé IP-adressen handmatig toevoegen aan het besturingssysteem van de virtuele machine door te voeren van de instructies in de [meerdere IP-adressen toewijzen aan virtuele machine besturingssystemen](virtual-network-multiple-ip-addresses-portal.md#os-config) artikel. Zie [persoonlijke](#private) IP-adressen voor speciale overwegingen voor het handmatig toevoegen van IP-adressen aan het besturingssysteem van een virtuele machine. Voeg geen openbare IP-adressen toe aan het besturingssysteem van de virtuele machine.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az network nic ip-config create](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_create)|
|PowerShell|[Add-AzNetworkInterfaceIpConfig](/powershell/module/az.network/add-aznetworkinterfaceipconfig)|

## <a name="change-ip-address-settings"></a>Instellingen voor IP-adres wijzigen

U mogelijk nodig om te wijzigen van de methode voor het toewijzen van een IPv4-adres wijzigen van het statische IPv4-adres of wijzigen van het openbare IP-adres toegewezen aan een netwerkinterface. Als u de privé IPv4-adres van een secundaire IP-configuratie die is gekoppeld aan een secundaire netwerkinterface in een virtuele machine wijzigt (meer informatie over [primaire en secundaire netwerkinterfaces](virtual-network-network-interface-vm.md)), plaats de virtuele machine in de status ' gestopt (toewijzing opgeheven) ' voordat u de volgende stappen:

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt weergeven of wijzigen van instellingen voor IP-adressen in de lijst.
3. Onder **instellingen**, selecteer **IP-configuraties**.
4. Selecteer de IP-configuratie die u wilt wijzigen in de lijst.
5. De instellingen wijzigen, als u wilt met behulp van de informatie over de instellingen in stap 5 van [toevoegen van een IP-configuratie](#add-ip-addresses).
6. Selecteer **Opslaan**.

>[!NOTE]
>Als de primaire netwerkinterface meerdere IP-configuraties heeft en u het particuliere IP-adres van de primaire IP-configuratie wijzigen, moet u handmatig opnieuw toewijzen de primaire en secundaire IP-adressen aan de netwerkinterface in Windows (niet vereist voor Linux) . Als u wilt IP-adressen handmatig toewijzen aan een netwerkinterface in een besturingssysteem, Zie [meerdere IP-adressen toewijzen aan virtuele machines](virtual-network-multiple-ip-addresses-portal.md#os-config). Zie voor bijzondere overwegingen voordat u IP-adressen handmatig toevoegt aan het besturingssysteem van een virtuele machine [persoonlijke](#private) IP-adressen. Voeg geen openbare IP-adressen toe aan het besturingssysteem van de virtuele machine.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic ip-config update](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzNetworkInterfaceIpConfig](/powershell/module/az.network/set-aznetworkinterfaceipconfig)|

## <a name="remove-ip-addresses"></a>IP-adressen verwijderen

U kunt verwijderen [persoonlijke](#private) en [openbare](#public) IP-adressen uit een netwerkinterface, maar een netwerkinterface moet altijd ten minste één privé IPv4-adres is toegewezen.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt verwijderen van IP-adressen uit de lijst.
3. Onder **instellingen**, selecteer **IP-configuraties**.
4. Selecteer met de rechtermuisknop een [secundaire](#secondary) IP-configuratie (u kunt niet verwijderen de [primaire](#primary) configuration) die u wilt verwijderen, selecteert u **verwijderen**en selecteer vervolgens  **Ja**, om de verwijdering te bevestigen. Als de configuratie had een openbare IP-adresresource zijn gekoppeld, wordt de resource is ontkoppeld van de IP-configuratie, maar de resource is niet verwijderd.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic ip-config delete](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_delete)|
|PowerShell|[Remove-AzNetworkInterfaceIpConfig](/powershell/module/az.network/remove-aznetworkinterfaceipconfig)|

## <a name="ip-configurations"></a>IP-configuraties

[Persoonlijke](#private) en (optioneel) [openbare](#public) IP-adressen zijn toegewezen aan een of meer IP-configuraties die zijn toegewezen aan een netwerkinterface. Er zijn twee typen IP-configuraties:

### <a name="primary"></a>Primair

Elke netwerkinterface wordt toegewezen één primaire IP-configuratie. Een primaire IP-configuratie:

- Heeft een [persoonlijke](#private) [IPv4](#ipv4) -adres is toegewezen. U kunt geen een particulier toewijzen [IPv6](#ipv6) adres aan een primaire IP-configuratie.
- Mogelijk beschikt over een [openbare](#public) IPv4-adres is toegewezen. U kunt een openbaar IPv6-adres kan niet toewijzen aan een primaire of secundaire IP-configuratie. U kunt echter een openbare IPv6-adres toewijzen aan een Azure load balancer die verkeer gelijkmatig verdelen over een virtuele machine privé-IPv6-adres kunt laden. Zie voor meer informatie, [details en beperkingen voor IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Secundair

Naast een primaire IP-configuratie mogelijk een netwerkinterface nul of meer secundaire IP-configuraties zijn toegewezen. Een secundaire IP-configuratie:

- Er moet een particulier IPv4- of IPv6-adres toegewezen aan deze. Als het adres IPv6 is, kan de netwerkinterface slechts één secundaire IP-configuratie hebben. Als het adres IPv4 is, is de netwerkinterface mogelijk meerdere secundaire IP-configuraties zijn toegewezen. Zie voor meer informatie over hoeveel persoonlijke en openbare IPv4-adressen kunnen worden toegewezen aan een netwerkinterface, de [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel.
- Ook kan hebt een openbaar IPv4-adres toegewezen, als het privé IP-adres IPv4 is. Als het privé IP-adres IPv6 is, kunt u een openbaar IPv4- of IPv6-adres niet toewijzen aan de IP-configuratie. Meerdere IP-adressen toewijzen aan een netwerkinterface is nuttig in scenario's zoals:
  - Het hosten van meerdere websites of services met verschillende IP-adressen en SSL-certificaten op één server.
  - Een virtuele machine fungeert als een virtueel netwerkapparaat, zoals een firewall of load balancer.
  - De mogelijkheid om een van de persoonlijke IPv4-adressen voor het gebruik van de netwerkinterfaces toevoegen aan een Azure Load Balancer-back-end-pool. In het verleden kan alleen de primaire IPv4-adres voor de primaire netwerkinterface worden toegevoegd aan een back-end-groep. Zie voor meer informatie over het Verdeel de belasting van configuraties met meerdere IPv4, de [taakverdeling meerdere IP-configuraties](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. 
  - De functie voor taakverdeling in evenwicht één IPv6-adres toegewezen aan een netwerkinterface. Zie voor meer informatie over hoe u taakverdeling op een privé-IPv6-adres, de [verdelen IPv6-adressen](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

## <a name="address-types"></a>-Adrestypen

U kunt de volgende typen IP-adressen aan een [IP-configuratie](#ip-configurations):

### <a name="private"></a>Privé

Persoonlijke [IPv4](#ipv4) adressen inschakelen in een virtuele machine om te communiceren met andere resources in een virtueel netwerk of andere verbonden netwerken. Een virtuele machine kan niet worden gecommuniceerd naar binnenkomende, noch kan de virtuele machine communiceren met een particulier uitgaande [IPv6](#ipv6) adres, met één uitzondering. Een virtuele machine kan communiceren met de Azure load balancer met behulp van een IPv6-adres. Zie voor meer informatie, [details en beperkingen voor IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

De Azure DHCP-servers toewijzen de privé IPv4-adres voor de [primaire IP-configuratie](#primary) van de Azure-netwerkinterface aan de netwerkinterface binnen het besturingssysteem van de virtuele machine. Tenzij die nodig zijn, u moet nooit handmatig instellen de IP-adres van een netwerkinterface in het besturingssysteem van de virtuele machine.

> [!WARNING]
> Als het IPv4-adres als het primaire IP-adres van een netwerkinterface in het besturingssysteem van een virtuele machine instellen ooit anders dan het privé IPv4-adres toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface gekoppeld aan een virtuele machine in Azure verliest u connectiviteit met de virtuele machine.

Er zijn scenario's waarin het nodig het IP-adres van een netwerkinterface in het besturingssysteem van de virtuele machine handmatig heeft ingesteld. Bijvoorbeeld, moet u handmatig de primaire en secundaire IP-adressen van een Windows-besturingssysteem instellen bij het toevoegen van meerdere IP-adressen met een Azure-machine. Voor een virtuele Linux-machine moet u mogelijk alleen handmatig instellen de secundaire IP-adressen. Zie [toevoegen IP-adressen aan een VM-besturingssysteem](virtual-network-multiple-ip-addresses-portal.md#os-config) voor meer informatie. Als u ooit wijzigen van het adres toegewezen aan een IP-configuratie wilt, het is raadzaam dat u:

1. Zorg ervoor dat de virtuele machine een adres van de Azure DHCP-servers ontvangt. Zodra u hebt, wordt de toewijzing van het IP-adres wijzigen in DHCP in het besturingssysteem en start de virtuele machine.
2. Stoppen (toewijzing ongedaan maken) de virtuele machine.
3. Wijzig het IP-adres voor de IP-configuratie in Azure.
4. Hiermee wordt de virtuele machine gestart.
5. [Handmatig configureren](virtual-network-multiple-ip-addresses-portal.md#os-config) de secundaire IP-adressen binnen het besturingssysteem (en ook de primaire IP-adres binnen Windows) zodat deze overeenkomt met wat u in Azure instellen.

Door de vorige stappen hebt uitgevoerd, het privé IP-adres toegewezen aan de netwerkinterface in Azure, en binnen het besturingssysteem van een virtuele machine, moet u hetzelfde blijven. Overweeg om bij te houden welke virtuele machines binnen uw abonnement die u handmatig IP-adressen binnen een besturingssysteem voor hebt ingesteld, het toevoegen van een Azure [tag](../azure-resource-manager/resource-group-using-tags.md) aan de virtuele machines. U kunt "IP-adrestoewijzing: Statische', bijvoorbeeld. Op deze manier kunt u eenvoudig vinden de virtuele machines binnen uw abonnement die u hebt het IP-adres voor het besturingssysteem handmatig ingesteld.

Naast het inschakelen van een virtuele machine om te communiceren met andere resources binnen de dezelfde of verbonden virtuele netwerken kan een privé IP-adres ook een virtuele machine naar de uitgaande communicatie met Internet. Uitgaande verbindingen zijn de bron-netwerkadres vertaald door Azure op een onvoorspelbare openbare IP-adres. Lees voor meer informatie over Azure uitgaande verbinding met Internet heeft, de [Azure uitgaande internetverbinding](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. U kunt geen inkomend communiceren met privé IP-adres van een virtuele machine vanaf het Internet. Als uw uitgaande verbindingen een voorspelbare openbare IP-adres vereist, koppelt u een openbare IP-adresresource aan een netwerkinterface.

### <a name="public"></a>Public

Openbare IP-adressen die via een openbare IP-adresresource is toegewezen voor het inschakelen van binnenkomende verbindingen aan een virtuele machine via Internet. Uitgaande verbindingen via Internet een voorspelbare IP-adres gebruiken. Zie [uitleg over uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie. U mag een openbaar IP-adres toewijzen aan een IP-configuratie, maar zijn niet vereist voor het. Als u geen openbaar IP-adres aan een virtuele machine toewijst door een openbare IP-adresresource koppelen, kunt de virtuele machine nog steeds uitgaande communicatie met Internet. In dit geval is het privé IP-adres bron netwerkadres vertaald door Azure op een onvoorspelbare openbare IP-adres. Zie voor meer informatie over resources voor openbaar IP-adres, [openbare IP-adresresource](virtual-network-public-ip-address.md).

Er gelden beperkingen voor het aantal persoonlijke en openbare IP-adressen die u aan een netwerkinterface toewijzen kunt. Lees voor meer informatie, de [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel.

> [!NOTE]
> Azure zet privé IP-adres van een virtuele machine naar een openbaar IP-adres. Als gevolg hiervan is het besturingssysteem van een virtuele machine niet op de hoogte van alle openbare IP-adres dat is toegewezen, zodat er geen nodig om toe te wijzen ooit handmatig een openbaar IP-adres binnen het besturingssysteem is.

## <a name="assignment-methods"></a>Van toewijzingsmethoden

Openbare en persoonlijke IP-adressen worden toegewezen met behulp van een van de volgende toewijzingsmethoden:

### <a name="dynamic"></a>Dynamisch

Dynamische persoonlijke IPv4 en IPv6-(optioneel)-adressen standaard toegewezen.

- **Alleen openbare**: Azure wijst het adres van een bereik dat uniek toe aan elke Azure-regio. Als u wilt weten welke adresbereiken zijn toegewezen aan elke regio, Zie [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Het adres kan veranderen wanneer een virtuele machine is gestopt (toewijzing opgeheven), klikt u vervolgens opnieuw hebt gestart. U kunt een openbaar IPv6-adres kan niet toewijzen aan een IP-configuratie met behulp van een van beide methoden toewijzing.
- **Alleen persoonlijke**: Azure reserveert de eerste vier adressen in het adresbereik van elk subnet, en de adressen niet toewijzen. Azure wijst het eerstvolgende beschikbare adres in het adresbereik van het subnet toe aan een resource. Als het adresbereik van het subnet bijvoorbeeld 10.0.0.0/16 is en de adressen 10.0.0.0.4-10.0.0.14 al zijn toegewezen (.0-.3 zijn gereserveerd), wijst Azure 10.0.0.15 toe aan de resource. Dynamisch is de standaardmethode voor toewijzing. Nadat dynamische IP-adressen zijn toegewezen, worden ze alleen vrijgegeven als een netwerkinterface wordt verwijderd of wordt toegewezen aan een ander subnet binnen hetzelfde virtuele netwerk of als de toewijzingsmethode wordt gewijzigd in statisch en een ander IP-adres wordt opgegeven. Standaard wijst Azure het vorige dynamisch toegewezen adres toe als het statische adres wanneer u de toewijzingsmethode van dynamisch wijzigt in statisch. U kunt alleen een privé-IPv6-adres met behulp van de dynamische toewijzingsmethode.

### <a name="static"></a>Statisch

U kunt (optioneel) een openbare of particuliere statische IPv4-adres toewijzen aan een IP-configuratie. U kunt een statisch openbaar of privé IPv6-adres kan niet toewijzen aan een IP-configuratie. Zie voor meer informatie over hoe Azure openbare statische IPv4-adressen wijst, [openbare IP-adressen](virtual-network-public-ip-address.md).

- **Alleen openbare**: Azure wijst het adres van een bereik dat uniek toe aan elke Azure-regio. U kunt de lijst met bereiken (voorvoegsels) downloaden voor de Azure-clouds [Openbaar](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) en [Duitsland](https://www.microsoft.com/download/details.aspx?id=57064). Het adres verandert niet tot het openbare IP-adresresource dat is toegewezen aan wordt verwijderd of de toewijzingsmethode wordt gewijzigd in dynamisch. Als het openbare IP-adresresource gekoppeld aan een IP-configuratie is, moet deze worden ontkoppeld van de IP-configuratie voordat u de toewijzingsmethode wijzigt.
- **Alleen persoonlijke**: U selecteert en een adres uit het adresbereik van het subnet toewijzen. Het adres dat u toewijst, kan elk adres binnen het adresbereik van het subnet zijn dat niet een van de eerste vier adressen in het adresbereik van het subnet is en momenteel niet is toegewezen aan een andere resource in het subnet. Statische adressen worden alleen vrijgegeven als een netwerkinterface wordt verwijderd. Als u de toewijzingsmethode wijzigt in statisch, wijst Azure het eerder toegewezen statische IP-adres toe als dynamisch adres, zelfs als dat adres niet het eerstvolgende beschikbare adres in het adresbereik van het subnet is. Het adres verandert ook als de netwerkinterface wordt toegewezen aan een ander subnet binnen hetzelfde virtuele netwerk, maar als u de netwerkinterface wilt toewijzen aan een ander subnet, moet u eerst de toewijzingsmethode wijzigen van statisch in dynamisch. Nadat u de netwerkinterface hebt toegewezen aan een ander subnet, kunt u de toewijzingsmethode weer wijzigen in statisch en een IP-adres uit het adresbereik van het nieuwe subnet toewijzen.

## <a name="ip-address-versions"></a>IP-adres-versies

U kunt de volgende versies opgeven bij het toewijzen van adressen:

### <a name="ipv4"></a>IPv4

Elke netwerkinterface moet een hebben [primaire](#primary) IP-configuratie met een toegewezen [persoonlijke](#private) [IPv4](#ipv4) adres. U kunt een of meer toevoegen [secundaire](#secondary) IP-configuraties die elk een particulier IPv4- en (optioneel) een IPv4-hebben [openbare](#public) IP-adres.

### <a name="ipv6"></a>IPv6

U kunt toewijzen nul of één privé [IPv6](#ipv6) adres aan een secundaire IP-configuratie van een netwerkinterface. De netwerkinterface kan niet alle bestaande secundaire IP-configuraties hebben. U kunt geen een IP-configuratie toevoegen met een IPv6-adres met behulp van de portal. Gebruik PowerShell of CLI om toe te voegen een IP-configuratie met een privé-IPv6-adres aan een bestaande netwerkinterface. De netwerkinterface kan niet worden gekoppeld aan een bestaande virtuele machine.

> [!NOTE]
> Hoewel u een netwerkinterface met een IPv6-adres met behulp van de portal maken kunt, kan u een bestaande netwerkinterface toevoegen aan een nieuwe of bestaande virtuele machine, met behulp van de portal is. PowerShell of Azure CLI gebruiken voor het maken van een netwerkinterface met een privé-IPv6-adres en de netwerkinterface koppelen bij het maken van een virtuele machine. U kunt een netwerkinterface niet koppelen met een persoonlijke IPv6-adres is toegewezen aan een bestaande virtuele machine. U kunt een privé-IPv6-adres niet toevoegen aan een IP-configuratie voor netwerkinterface is gekoppeld aan een virtuele machine met behulp van hulpprogramma's (portal, CLI of PowerShell).

U kunt een openbaar IPv6-adres kan niet toewijzen aan een primaire of secundaire IP-configuratie.

## <a name="skus"></a>Voorraadeenheden

Een openbaar IP-adres wordt gemaakt met de basis of standaard-SKU. Zie voor meer informatie over de verschillen tussen SKU [openbare IP-adressen beheren](virtual-network-public-ip-address.md).

> [!NOTE]
> Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.

## <a name="next-steps"></a>Volgende stappen
Voor het maken van een virtuele machine met verschillende IP-configuraties, lees de volgende artikelen:

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere netwerkinterfaces maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Een enkel NIC-VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Een enkel NIC-VM maken met een privé-IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
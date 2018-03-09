---
title: Maken, wijzigen of verwijderen van een virtuele Azure-netwerk | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een virtueel netwerk in Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 6829ca9ad5ef76b6a96eed1f2cc461d32c0e51cf
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="create-change-or-delete-a-virtual-network"></a>Maken, wijzigen of verwijderen van een virtueel netwerk

Informatie over het maken en verwijderen van een virtueel netwerk en -instellingen, zoals DNS-servers en IP-adresruimte voor een bestaand virtueel netwerk wijzigen.

Een virtueel netwerk is een weergave van uw eigen netwerk in de cloud. Een virtueel netwerk is een logische isolatie van de Azure-cloud die is toegewezen aan uw Azure-abonnement. Voor elk virtueel netwerk die u maakt, kunt u het volgende doen:
- Kies een adresruimte toewijzen. Een adresruimte bestaat uit een of meer adresbereiken die zijn gedefinieerd met de notatie (Classless Inter-Domain Routing), zoals 10.0.0.0/16.
- Kies de Azure DNS-server, gebruiken of uw eigen DNS-server. Alle resources die zijn verbonden met het virtuele netwerk zijn toegewezen deze DNS-server voor naamomzetting in het virtuele netwerk.
- Het virtuele netwerk segmenteren in subnetten, elk met een eigen-adresbereik op in de adresruimte van het virtuele netwerk. Zie voor informatie over het maken, wijzigen en verwijderen van subnetten, [toevoegen, wijzigen of verwijderen subnetten](virtual-network-manage-subnet.md).

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, open https://portal.azure.com en meld u aan met uw Azure-account.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Deze zelfstudie vereist de Azure PowerShell-moduleversie 5.2.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.26 of hoger. Voer `az --version` de geïnstalleerde versie vinden. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ maken van een resource** > **Networking** > **virtueel netwerk**.
2. Typ of Selecteer waarden voor de volgende instellingen en selecteer vervolgens **maken**:
    - **Naam**: de naam moet uniek zijn in de [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) dat u selecteert voor het maken van het virtuele netwerk in. U kunt de naam niet wijzigen nadat het virtuele netwerk is gemaakt. U kunt meerdere virtuele netwerken maken gedurende een bepaalde periode. Zie voor de naamgeving van suggesties [naamconventies](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Na een naamgevingsconventie kunt maken het gemakkelijker om meerdere virtuele netwerken te beheren.
    - **Adresruimte**: de adresruimte voor een virtueel netwerk bestaat uit een of meer niet-overlappende-adresbereiken die zijn opgegeven in CIDR-notatie. Het adresbereik dat u definieert mag public of private (RFC 1918). Of u het adresbereik als openbare of particuliere definiëren, is het adresbereik dat bereikbaar is alleen van binnen het virtuele netwerk van onderling verbonden virtuele netwerken en van een on-premises netwerken die u hebt gekoppeld aan het virtuele netwerk. U kunt de volgende adresbereiken niet toevoegen:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (Broadcast)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (Internal DNS)

      Hoewel u slechts één adresbereik definiëren kunt wanneer u het virtuele netwerk maakt, kunt u meer adresbereiken toevoegen aan de adresruimte nadat het virtuele netwerk is gemaakt. Zie voor meer informatie over hoe u een adresbereik toevoegt aan een bestaand virtueel netwerk, [toevoegen of verwijderen van een adresbereik](#add-or-remove-an-address-range).

      >[!WARNING]
      >Als een virtueel netwerk-adresbereiken die met een ander virtueel netwerk overlappen is of on-premises netwerk, kunnen de twee netwerken kunnen niet worden verbonden. Voordat u een adresbereik definiëren, overweeg of wilt u mogelijk het virtueel netwerk verbinden met andere virtuele netwerken of on-premises netwerken in de toekomst.
      >
      >

    - **De subnetnaam van het**: de subnetnaam moet uniek zijn binnen het virtuele netwerk. U kunt de subnetnaam niet wijzigen nadat het subnet is gemaakt. De portal is vereist dat u één subnet definieert bij het maken van een virtueel netwerk, zelfs als een virtueel netwerk is niet beschikken over subnetten. U kunt slechts één subnet in de portal definiëren wanneer u een virtueel netwerk maken. U kunt meer subnetten toevoegen aan het virtuele netwerk later, nadat het virtuele netwerk is gemaakt. Zie voor informatie over het toevoegen van een subnet met een virtueel netwerk [subnetten beheren](virtual-network-manage-subnet.md). U kunt een virtueel netwerk met meerdere subnetten met behulp van Azure CLI of PowerShell maken.

      >[!TIP]
      >Beheerders maken soms verschillende subnetten wilt filteren of beheren met het routeren van verkeer tussen de subnetten. Voordat u subnetten definiëren, houd rekening met hoe het handig om te filteren en verkeer leiden tussen uw subnetten. Zie voor meer informatie over het filteren van verkeer tussen subnetten, [Netwerkbeveiligingsgroepen](security-overview.md). Azure automatisch routes verkeer tussen subnetten, maar u kunt onderdrukken Azure standaardroutes. Zie voor meer informatie over Azures subnet verkeer standaardroutering, [Routering-overzicht](virtual-networks-udr-overview.md).
      >

    - **Adresbereik van**: het bereik moet zich in de adresruimte die u hebt opgegeven voor het virtuele netwerk. Het kleinste bereik dat kunt u opgeven is slechts/29, waarmee u acht IP-adressen voor het subnet. Azure behoudt zich het eerste en laatste adres in elk subnet voor het protocol overeenstemming. Drie extra adressen zijn gereserveerd voor gebruik van Azure service. Als gevolg hiervan is een virtueel netwerk met een adresbereik subnet van slechts/29 slechts drie bruikbare IP-adressen. Als u een virtueel netwerk verbinden met een VPN-gateway wilt, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is gemaakt, klikt u onder bepaalde omstandigheden. Zie voor meer informatie over het wijzigen van een subnet-adresbereik, [subnetten beheren](virtual-network-manage-subnet.md).
    - **Abonnement**: Selecteer een [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). U kunt hetzelfde virtuele netwerk niet gebruiken in meer dan één Azure-abonnement. U kunt echter een virtueel netwerk in één abonnement verbinden met virtuele netwerken in andere abonnementen met [virtueel netwerk peering](virtual-network-peering-overview.md). Een Azure-resource waarmee u verbinding met het virtuele netwerk maakt moet zich in hetzelfde abonnement als het virtuele netwerk.
    - **Resourcegroep**: Selecteer een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) of maak een nieuwe. Een Azure-resource waarmee u verbinding met het virtuele netwerk maakt kan niet in dezelfde resourcegroep bevinden als het virtuele netwerk of in een andere resourcegroep.
    - **Locatie**: Selecteer een Azure [locatie](https://azure.microsoft.com/regions/), ook wel aangeduid als een regio. Een virtueel netwerk kan slechts één Azure-locatie liggen. U kunt echter een virtueel netwerk op één locatie met een virtueel netwerk in een andere locatie verbinden via een VPN-gateway. Een Azure-resource waarmee u verbinding met het virtuele netwerk maakt moet op dezelfde locatie als het virtuele netwerk.

**Opdrachten**

- Azure CLI: [az network vnet maken](/cli/azure/network/vnet)
- PowerShell: [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)

## <a name="view-virtual-networks-and-settings"></a>Virtuele netwerken weergeven en instellingen

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u wilt weergeven van instellingen voor.
3. De volgende instellingen worden weergegeven voor het virtuele netwerk dat u hebt geselecteerd:
    - **Overzicht**: bevat informatie over het virtuele netwerk, met inbegrip van adresruimte en DNS-servers. De volgende schermafbeelding ziet u de instellingen van het overzicht voor een virtueel netwerk met de naam **MyVNet**:

        ![Network interface-overzicht](./media/virtual-network-manage-network/vnet-overview.png)

      U kunt een virtueel netwerk verplaatsen naar een ander abonnement of resourcegroep groep door te selecteren **wijziging** naast **resourcegroep** of **naam abonnement**. Zie voor informatie over het verplaatsen van een virtueel netwerk, [resources verplaatsen naar een andere resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel worden de vereisten en het verplaatsen van resources met behulp van de Azure-portal, PowerShell en Azure CLI. Alle resources die zijn verbonden met het virtuele netwerk moeten worden verplaatst met het virtuele netwerk.
    - **Adresruimte**: de adresruimten die zijn toegewezen aan het virtuele netwerk worden vermeld. Meer informatie over het toevoegen en verwijderen van een adresbereik aan de adresruimte, voer de stappen in [toevoegen of verwijderen van een adresbereik](#add-or-remove-an-address-range).
    - **Verbonden apparaten**: alle bronnen die zijn verbonden met het virtuele netwerk worden vermeld. In de vorige schermafbeelding zijn drie netwerkinterfaces en één load balancer verbonden met het virtuele netwerk. De nieuwe resources die u maakt en verbinding maken met het virtuele netwerk worden weergegeven. Als u een resource die is verbonden met het virtuele netwerk verwijdert, worden deze niet meer weergegeven in de lijst.
    - **Subnetten**: een lijst met subnetten die in het virtuele netwerk voorkomen wordt weergegeven. Zie voor informatie over het toevoegen en verwijderen van een subnet, [subnetten beheren](virtual-network-manage-subnet.md).
    - **DNS-servers**: U kunt opgeven of de Azure interne DNS-server of een aangepaste DNS-server naamomzetting biedt voor apparaten die zijn verbonden met het virtuele netwerk. Wanneer u een virtueel netwerk met behulp van de Azure-portal maakt, worden de Azure DNS-servers voor naamomzetting binnen een virtueel netwerk standaard gebruikt. Voor het wijzigen van de DNS-servers, voer de stappen in [wijziging DNS-servers](#change-dns-servers) in dit artikel.
    - **Peerings**: als er bestaande peerings in het abonnement, worden deze hier weergegeven. U kunt instellingen voor bestaande peerings weergeven of maken, wijzigen of verwijderen van peerings. Zie voor meer informatie over peerings, [virtuele netwerk peering](virtual-network-peering-overview.md).
    - **Eigenschappen**: vindt u instellingen over van het virtuele netwerk, met inbegrip van de resource-ID van het virtuele netwerk en het Azure-abonnement in.
    - **Diagram**: het diagram biedt een visuele representatie van alle apparaten die zijn verbonden met het virtuele netwerk. Het diagram heeft enkele belangrijke informatie over de apparaten. Selecteer het apparaat voor het beheren van een apparaat in deze weergave in het diagram.
    - **Algemene instellingen voor Azure**: voor meer informatie over algemene instellingen voor Azure, Zie de volgende informatie:
        *   [Activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

- Azure CLI: [az network vnet show](/cli/azure/network/vnet#az_network_vnet_show)
- PowerShell: [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork)

## <a name="add-or-remove-an-address-range"></a>Toevoegen of verwijderen van een adresbereik in

U kunt toevoegen en verwijderen-adresbereiken voor een virtueel netwerk. Een adresbereik in CIDR-notatie moet worden opgegeven en kan niet overlappen met andere adresbereiken binnen hetzelfde virtuele netwerk. De adresbereiken die u definieert mag public of private (RFC 1918). Of u het adresbereik als openbare of particuliere definiëren, is het adresbereik dat bereikbaar is alleen van binnen het virtuele netwerk van onderling verbonden virtuele netwerken en van een on-premises netwerken die u hebt gekoppeld aan het virtuele netwerk. U kunt de volgende adresbereiken niet toevoegen:

- 224.0.0.0/4 (Multicast)
- 255.255.255.255/32 (Broadcast)
- 127.0.0.0/8 (Loopback)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (Internal DNS)

Toevoegen of verwijderen van een adresbereik:

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer het virtuele netwerk waarvoor u wilt toevoegen of verwijderen van een adresbereik in de lijst van virtuele netwerken.
3. Selecteer **adresruimte**onder **instellingen**.
4. Voer een van de volgende opties:
    - **Toevoegen van een adresbereik**: Geef het nieuwe-adresbereik. Het adresbereik overlappen niet met een bestaande-adresbereik dat is gedefinieerd voor het virtuele netwerk.
    - **Verwijderen van een adresbereik**: aan de rechterkant van het adresbereik dat u wilt verwijderen, selecteert u **...** , selecteer daarna **verwijderen**. Als een subnet in het adresbereik bestaat, kunt u het adresbereik niet verwijderen. Als u wilt een adresbereik verwijderen, moet u eerst verwijderen subnetten (en alle resources in de subnetten) die zijn opgenomen in het adresbereik.
5. Selecteer **Opslaan**.

**Opdrachten**

- Azure CLI: [az network vnet update](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="change-dns-servers"></a>DNS-servers wijzigen

Alle virtuele machines die zijn verbonden met het register van het virtuele netwerk met de DNS-servers die u voor het virtuele netwerk opgeeft. Ze ook de opgegeven DNS-server gebruiken voor naamomzetting. Elke netwerkinterface (NIC) in een virtuele machine kan hebben een eigen DNS-serverinstellingen. Als een NIC een eigen DNS-serverinstellingen heeft, overschrijven ze de DNS-serverinstellingen voor het virtuele netwerk. Zie voor meer informatie over NIC DNS-instellingen, [Network interface-taken en instellingen](virtual-network-network-interface.md#change-dns-servers). Zie voor meer informatie over naamomzetting voor VM's en rolinstanties in Azure Cloud Services, [naamomzetting voor VM's en rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md). Als u wilt toevoegen, wijzigen of verwijderen van een DNS-server:

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waarvoor u wilt wijzigen van de DNS-servers voor.
3.  Selecteer **DNS-servers**onder **instellingen**.
4. Selecteer een van de volgende opties:
    - **Standaard (Azure verschafte)**: alle resourcenamen en privé IP-adressen automatisch worden geregistreerd met de Azure DNS-servers. U kunt omzetten van namen tussen alle bronnen die zijn verbonden met hetzelfde virtuele netwerk. U kunt deze optie niet gebruiken voor het omzetten van namen in virtuele netwerken. Voor het omzetten van namen in virtuele netwerken, moet u een aangepaste DNS-server.
    - **Aangepaste**: U kunt een of meer servers, tot de Azure is bereikt voor een virtueel netwerk toevoegen. Zie voor meer informatie over de limieten van DNS-server, [Azure limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). U hebt de volgende opties:
        - **Toevoegen van een adres**: de server toevoegen aan uw lijst met virtuele netwerk DNS-servers. Deze optie wordt ook de DNS-server geregistreerd bij Azure. Als u hebt al een DNS-server geregistreerd met Azure, kunt u die DNS-server in de lijst.
        - **Verwijder een adres**: naast de server die u wilt verwijderen, selecteert u **...** , klikt u vervolgens **verwijderen**. De server verwijdert, wordt de server alleen uit deze lijst virtueel netwerk. De DNS-server blijft geregistreerd in Azure voor uw virtuele netwerken te gebruiken.
        - **DNS-serveradressen rangschikken**: het is belangrijk om te controleren of weer te geven die uw DNS-servers in de juiste volgorde voor uw omgeving. Een lijst met DNS-server worden gebruikt in de volgorde waarin ze worden opgegeven. Ze werken niet als de installatie van een round robin. Als de eerste DNS-server in de lijst kan worden bereikt, gebruikt de client die DNS-server, ongeacht of de DNS-server correct werkt. Verwijder alle DNS-servers die worden vermeld en vervolgens weer in de volgorde die u wenst toe te voegen.
        - **Een adres wijzigen**: markeert u de DNS-server in de lijst en voer vervolgens het nieuwe adres.
5. Selecteer **Opslaan**.
6. Start opnieuw op de virtuele machines die zijn verbonden met het virtuele netwerk, zodat ze de nieuwe DNS-serverinstellingen worden toegewezen. Virtuele machines blijven hun huidige DNS-instellingen gebruiken totdat ze opnieuw worden gestart.

**Opdrachten**

- Azure CLI: [az network vnet update](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="delete-a-virtual-network"></a>Een virtueel netwerk verwijderen

U kunt een virtueel netwerk alleen verwijderen als er geen resources mee zijn verbonden zijn. Als er bronnen die zijn verbonden met een subnet in het virtuele netwerk, moet u eerst de resources die zijn verbonden met alle subnetten in het virtuele netwerk verwijderen. De stappen waarmee u een bron verwijderen, is afhankelijk van de resource. Als u wilt weten hoe u resources verwijderen die zijn verbonden met subnetten, Raadpleeg de documentatie voor elk resourcetype die u wilt verwijderen. Een virtueel netwerk verwijderen:

1. Voer in het zoekvak boven aan de portal *virtuele netwerken* in het zoekvak. Wanneer **virtuele netwerken** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer het virtuele netwerk dat u wilt verwijderen uit de lijst van virtuele netwerken.
3. Bevestig dat er geen apparaten die verbonden zijn met het virtuele netwerk zijn door het selecteren van **verbonden apparaten**onder **instellingen**. Als er verbonden apparaten, moet u deze verwijderen voordat u het virtuele netwerk kunt verwijderen. Als er geen verbonden apparaten zijn, selecteert u **overzicht**.
4. Selecteer **Verwijderen**.
5. Bevestig het verwijderen van het virtuele netwerk, selecteer **Ja**.

**Opdrachten**

- Azure CLI: [azure network vnet verwijderen](/cli/azure/network/vnet#az_network_vnet_delete)
- PowerShell: [Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork)

## <a name="permissions"></a>Machtigingen

Als u wilt uitvoeren op virtuele netwerken, moet uw account worden toegewezen aan de [netwerk Inzender](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is de juiste machtigingen toegewezen die worden vermeld in de volgende tabel:

|Bewerking                                    |   Bewerkingsnaam                    |
|-------------------------------------------  |   --------------------------------  |
|Microsoft.Network/virtualNetworks/read       |   Virtueel netwerk ophalen               |
|Microsoft.Network/virtualNetworks/write      |   Virtueel netwerk maken of bijwerken  |
|Microsoft.Network/virtualNetworks/delete     |   Virtueel netwerk verwijderen            |

## <a name="next-steps"></a>Volgende stappen

- Zie voor een virtuele machine maken en vervolgens verbinding met een virtueel netwerk, [een virtueel netwerk maken en verbinding maken met virtuele machines](quick-create-portal.md#create-virtual-machines).
- Als u wilt filteren van netwerkverkeer tussen subnetten binnen een virtueel netwerk, Zie [netwerkbeveiligingsgroepen maken](virtual-networks-create-nsg-arm-pportal.md).
- Zie to-peer-een virtueel netwerk met een ander virtueel netwerk, [peering van een virtueel netwerk maken](tutorial-connect-virtual-networks-portal.md).
- Zie voor meer informatie over opties voor het verbinden van een virtueel netwerk met een on-premises netwerk, [over VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

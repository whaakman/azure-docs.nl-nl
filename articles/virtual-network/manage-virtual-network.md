---
title: Maken, wijzigen of verwijderen van een Azure-netwerk
titlesuffix: Azure Virtual Network
description: Informatie over het maken, wijzigen of verwijderen van een virtueel netwerk in Azure.
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
ms.openlocfilehash: 3f158d040654b251faebceaa2e89d0462f13c217
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016020"
---
# <a name="create-change-or-delete-a-virtual-network"></a>Maken, wijzigen of verwijderen van een virtueel netwerk

Informatie over het maken en verwijderen van een virtueel netwerk en het wijzigen van instellingen, zoals DNS-servers en IP-adresruimten, voor een bestaand virtueel netwerk. Als u geen ervaring met virtuele netwerken, kunt u meer informatie over deze in de [overzicht van Virtual network](virtual-networks-overview.md) of door te voeren een [zelfstudie](quick-create-portal.md). Een virtueel netwerk bevat subnetten. Zie voor informatie over het maken, wijzigen en verwijderen van subnetten, [subnetten beheren](virtual-network-manage-subnet.md).

## <a name="before-you-begin"></a>Voordat u begint

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.
- Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ een resource maken** > **netwerken** > **virtueel netwerk**.
2. Typ of Selecteer waarden voor de volgende instellingen en selecteer vervolgens **maken**:
    - **Naam**: De naam moet uniek zijn in de [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) dat u selecteert om te maken van het virtuele netwerk in. U kunt de naam niet wijzigen nadat het virtuele netwerk is gemaakt. U kunt meerdere virtuele netwerken maken na verloop van tijd. Zie voor het benoemen van suggesties, [naamconventies](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). Na een naamconventie kunt maken het gemakkelijker voor het beheren van meerdere virtuele netwerken.
    - **Adresruimte**: De adresruimte voor een virtueel netwerk bestaat uit een of meer niet-overlappende-adresbereiken die zijn opgegeven in de CIDR-notatie. Het adresbereik dat u definieert kan zijn openbaar of privé (RFC 1918). Of u het adresbereik als openbare of particuliere definiëren, is het adresbereik bereikbaar is alleen van binnen het virtuele netwerk van met elkaar verbonden virtuele netwerken en van een on-premises-netwerken die u hebt gekoppeld aan het virtuele netwerk. U kunt de volgende adresbereiken niet toevoegen:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (Broadcast)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (interne DNS)

      Hoewel u slechts één adresbereik definiëren kunt wanneer u het virtuele netwerk maakt, kunt u meer adresbereiken toevoegen aan de adresruimte nadat het virtuele netwerk is gemaakt. Zie voor meer informatie over een adresbereik toevoegen aan een bestaand virtueel netwerk, [toevoegen of verwijderen van een adresbereik](#add-or-remove-an-address-range).

      >[!WARNING]
      >Als een virtueel netwerk-adresbereiken die met een ander virtueel netwerk overlappen is of on-premises netwerk, kunnen de twee netwerken kunnen niet worden verbonden. Voordat u een adresbereik definiëren, moet u overwegen of wilt u mogelijk het virtueel netwerk verbinden met andere virtuele netwerken of on-premises netwerken in de toekomst.
      >
      >

    - **Subnetnaam**: De subnetnaam moet uniek zijn binnen het virtuele netwerk. U kunt de naam van het subnet niet wijzigen nadat het subnet is gemaakt. De portal vereist dat u een subnet definieert bij het maken van een virtueel netwerk, zelfs als een virtueel netwerk is niet vereist voor het geen subnetten. In de portal, kunt u slechts één subnet definiëren wanneer u een virtueel netwerk maken. U kunt meer subnetten toevoegen aan het virtuele netwerk later, nadat het virtuele netwerk is gemaakt. Zie voor informatie over het toevoegen van een subnet met een virtueel netwerk [subnetten beheren](virtual-network-manage-subnet.md). U kunt een virtueel netwerk met meerdere subnetten met behulp van Azure CLI of PowerShell maken.

      >[!TIP]
      >Beheerders maken soms verschillende subnetten te filteren of beheren van routering van verkeer tussen de subnetten. Voordat u subnets kunt definiëren, houd rekening met hoe u mogelijk wilt filteren en verkeer routeren tussen uw subnetten. Zie voor meer informatie over het filteren van verkeer tussen subnetten, [Netwerkbeveiligingsgroepen](security-overview.md). Azure automatisch routeert verkeer tussen subnetten, maar u kunt Azure standaardroutes vervangen. Zie voor meer informatie over het verkeer van Azures standaard subnet, [routeringoverzicht](virtual-networks-udr-overview.md).
      >

    - **Subnetadresbereik**: Het bereik moet zich binnen de adresruimte die u hebt ingevoerd voor het virtuele netwerk. Het kleinste bereik dat u kunt opgeven is /29, waarmee u acht IP-adressen voor het subnet. Azure reserveert de eerste en laatste adres in elk subnet voor conformiteit van protocol. Drie extra adressen zijn gereserveerd voor gebruik met Azure-service. Een virtueel netwerk met een subnet-adresbereik van /29 heeft als gevolg hiervan alleen voor de drie bruikbare IP-adressen. Als u van plan bent een virtueel netwerk verbinden met een VPN-gateway, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is gemaakt, klikt u onder bepaalde omstandigheden. Zie voor informatie over het wijzigen van het adresbereik van een subnet, [subnetten beheren](virtual-network-manage-subnet.md).
    - **Abonnement**: Selecteer een [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). U kunt hetzelfde virtuele netwerk in meer dan één Azure-abonnement gebruiken. U kunt echter een virtueel netwerk in een abonnement verbinding met virtuele netwerken in andere abonnementen met [peering op virtueel netwerk](virtual-network-peering-overview.md). Een Azure-resource die u verbinding met het virtuele netwerk maken moet zich in hetzelfde abonnement bevinden als het virtuele netwerk.
    - **Resourcegroep**: Selecteer een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) of maak een nieuwe. Een Azure-resource die u verbinding met het virtuele netwerk maken kan zich in dezelfde resourcegroep bevinden als het virtuele netwerk of in een andere resourcegroep.
    - **Locatie**: Selecteer een Azure [locatie](https://azure.microsoft.com/regions/), ook wel aangeduid als een regio. Een virtueel netwerk kan zich op slechts één Azure-locatie. U kunt echter een virtueel netwerk op één locatie verbinden met een virtueel netwerk in een andere locatie via een VPN-gateway. Een Azure-resource die u verbinding met het virtuele netwerk maken moet zich in dezelfde locatie als het virtuele netwerk.

**Opdrachten**

- Azure CLI: [az network vnet maken](/cli/azure/network/vnet)
- PowerShell: [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)

## <a name="view-virtual-networks-and-settings"></a>Instellingen en virtuele netwerken weergeven

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u wilt weergeven van instellingen voor.
3. De volgende instellingen worden weergegeven voor het virtuele netwerk dat u hebt geselecteerd:
    - **Overzicht**: Bevat informatie over het virtuele netwerk, met inbegrip van adresruimte en DNS-servers. De volgende schermafbeelding ziet u de instellingen van het overzicht voor een virtueel netwerk met de naam **MyVNet**:

        ![Overzicht van Network interface](./media/manage-virtual-network/vnet-overview.png)

      U kunt een virtueel netwerk verplaatsen naar een ander abonnement of resourcegroep groep door te selecteren **wijziging** naast **resourcegroep** of **abonnementsnaam**. Zie voor meer informatie over het verplaatsen van een virtueel netwerk, [resources verplaatsen naar een andere resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel worden de vereisten en over het verplaatsen van resources met behulp van de Azure portal, PowerShell en Azure CLI. Alle resources die zijn verbonden met het virtuele netwerk moeten verplaatsen met het virtuele netwerk.
    - **Adresruimte**: De adresruimten die zijn toegewezen aan het virtuele netwerk worden weergegeven. Voor informatie over het toevoegen en verwijderen van een adresbereik aan de adresruimte, voer de stappen in [toevoegen of verwijderen van een adresbereik](#add-or-remove-an-address-range).
    - **Verbonden apparaten**: Alle resources die zijn verbonden met het virtuele netwerk worden weergegeven. In de vorige schermafbeelding zijn drie netwerkinterfaces en een load balancer verbonden met het virtuele netwerk. Nieuwe resources die u maakt en verbinding maken met het virtuele netwerk worden weergegeven. Als u een resource die is verbonden met het virtuele netwerk verwijdert, worden deze niet meer weergegeven in de lijst.
    - **Subnetten**: Een lijst met subnetten die zijn opgeslagen in het virtuele netwerk wordt weergegeven. Zie voor informatie over het toevoegen en verwijderen van een subnet, [subnetten beheren](virtual-network-manage-subnet.md).
    - **DNS-servers**: U kunt opgeven of de Azure interne DNS-server of een aangepaste DNS-server naamomzetting biedt voor apparaten die zijn verbonden met het virtuele netwerk. Wanneer u een virtueel netwerk met behulp van Azure portal maakt, worden voor het omzetten van binnen een virtueel netwerk van Azure DNS-servers gebruikt standaard. Als u wilt wijzigen van de DNS-servers, voer de stappen in [wijziging DNS-servers](#change-dns-servers) in dit artikel.
    - **Peerings**: Als er bestaande peerings in het abonnement, worden ze hier weergegeven. U kunt instellingen voor bestaande peerings, weergeven of maken, wijzigen of verwijderen van peerings. Zie voor meer informatie over peerings, [peering van virtuele netwerken](virtual-network-peering-overview.md).
    - **Eigenschappen**: Geeft de instellingen over het virtuele netwerk, met inbegrip van de resource-ID van het virtuele netwerk en het deel uitmaakt van Azure-abonnement.
    - **Diagram**: Het diagram biedt een visuele representatie van alle apparaten die zijn verbonden met het virtuele netwerk. Het diagram heeft enkele belangrijke informatie over de apparaten. Selecteer het apparaat voor het beheren van een apparaat in deze weergave in het diagram.
    - **Algemene instellingen voor Azure**: Zie voor meer informatie over algemene Azure-instellingen, de volgende informatie:
        *   [Activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md)
        *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

- Azure CLI: [az network vnet show](/cli/azure/network/vnet#az_network_vnet_show)
- PowerShell: [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork)

## <a name="add-or-remove-an-address-range"></a>Toevoegen of verwijderen van een adresbereik in

U kunt toevoegen en verwijderen-adresbereiken voor een virtueel netwerk. Een adresbereik in CIDR-notatie moet worden opgegeven en mag niet overlappen met andere adresbereiken binnen hetzelfde virtuele netwerk. De adresbereiken die u definieert kunnen zijn openbaar of privé (RFC 1918). Of u het adresbereik als openbare of particuliere definiëren, is het adresbereik bereikbaar is alleen van binnen het virtuele netwerk van met elkaar verbonden virtuele netwerken en van een on-premises-netwerken die u hebt gekoppeld aan het virtuele netwerk. U kunt de volgende adresbereiken niet toevoegen:

- 224.0.0.0/4 (Multicast)
- 255.255.255.255/32 (Broadcast)
- 127.0.0.0/8 (Loopback)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (interne DNS)

Toevoegen of verwijderen van een adresbereik in:

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waarvoor u wilt toevoegen of verwijderen van een adresbereik.
3. Selecteer **adresruimte**onder **instellingen**.
4. Voer een van de volgende opties:
    - **Toevoegen van een adresbereik**: Voer in het nieuwe adresbereik. Het adresbereik mag niet overlappen met een bestaande-adresbereik dat is gedefinieerd voor het virtuele netwerk.
    - **Verwijderen van een adresbereik**: Aan de rechterkant van het adresbereik dat u wilt verwijderen, selecteert u **...** en selecteer vervolgens **verwijderen**. Als een subnet in het adresbereik bestaat, kunt u het adresbereik niet verwijderen. Als u wilt een adresbereik verwijderen, verwijdert u eerst eventuele subnetten (en alle resources in de subnetten) die zijn opgenomen in het adresbereik.
5. Selecteer **Opslaan**.

**Opdrachten**

- Azure CLI: [az network vnet update](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="change-dns-servers"></a>DNS-servers wijzigen

Alle virtuele machines die zijn verbonden met het virtuele netwerk registreren zich bij de DNS-servers die u voor het virtuele netwerk opgeeft. Ze ook de opgegeven DNS-server gebruiken voor naamomzetting. Elke netwerkinterface (NIC) in een virtuele machine kan hebben een eigen DNS-serverinstellingen. Als een NIC een eigen DNS-serverinstellingen heeft, overschrijven ze de DNS-serverinstellingen voor het virtuele netwerk. Zie voor meer informatie over NIC-DNS-instellingen, [Network interface-taken en instellingen](virtual-network-network-interface.md#change-dns-servers). Zie voor meer informatie over naamomzetting voor virtuele machines en rollen in Azure Cloud Services, [naamomzetting voor VM's en rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md). Als u wilt toevoegen, wijzigen of verwijderen van een DNS-server:

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk waarvoor u wilt wijzigen van de DNS-servers voor.
3.  Selecteer **DNS-servers**onder **instellingen**.
4. Selecteer een van de volgende opties:
    - **Standaard (door Azure geleverd)**: Alle namen van voorbeeldresources en privé-IP-adressen worden automatisch geregistreerd bij de Azure DNS-servers. U kunt omzetten van namen tussen alle resources die zijn verbonden met hetzelfde virtuele netwerk. U kunt deze optie niet gebruiken voor het omzetten van namen tussen virtuele netwerken. Als u wilt omzetten van namen tussen virtuele netwerken, moet u een aangepaste DNS-server.
    - **Aangepaste**: U kunt een of meer servers, tot de Azure is bereikt voor een virtueel netwerk toevoegen. Zie voor meer informatie over limieten voor DNS-servers, [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). U hebt de volgende opties:
        - **Toevoegen van een adres**: De server toevoegen aan uw lijst met virtuele netwerk DNS-servers. Deze optie wordt ook de DNS-server geregistreerd bij Azure. Als u hebt al een DNS-server geregistreerd bij Azure, kunt u deze DNS-server in de lijst selecteren.
        - **Verwijderen van een adres**: Naast de server die u wilt verwijderen, selecteert u **...** , klikt u vervolgens **verwijderen**. De server verwijdert, wordt de server alleen in de lijst van dit virtuele netwerk. De DNS-server blijft ingeschreven in Azure voor uw virtuele netwerken om te gebruiken.
        - **Volgorde van de DNS-serveradressen**: Het is belangrijk om te controleren of dat u uw DNS-servers in de juiste volgorde voor uw omgeving weergeven. Een lijst met DNS-server worden gebruikt in de volgorde waarin ze worden opgegeven. Ze werken niet als een round robin-installatie. Als de eerste DNS-server in de lijst kan worden bereikt, gebruikt de client die DNS-server, ongeacht of de DNS-server correct werkt. Verwijder alle DNS-servers die worden vermeld en deze vervolgens terug in de volgorde die u wilt toevoegen.
        - **Een adres wijzigen**: Markeer de DNS-server in de lijst en voer vervolgens het nieuwe adres.
5. Selecteer **Opslaan**.
6. Start opnieuw op de virtuele machines die zijn verbonden met het virtuele netwerk, zodat ze de nieuwe DNS-serverinstellingen worden toegewezen. Virtuele machines blijven hun huidige DNS-instellingen gebruiken totdat ze opnieuw worden gestart.

**Opdrachten**

- Azure CLI: [az network vnet update](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="delete-a-virtual-network"></a>Een virtueel netwerk verwijderen

U kunt een virtueel netwerk alleen verwijderen als er geen bronnen die zijn verbonden zijn. Als er bronnen zijn verbonden met een subnet binnen het virtuele netwerk, moet u eerst de resources die zijn verbonden met alle subnetten in het virtuele netwerk verwijderen. De stappen waarmee u een resource verwijderen, is afhankelijk van de resource. Als u wilt weten hoe u resources verwijderen die zijn verbonden met subnetten, Raadpleeg de documentatie voor elk resourcetype dat u wilt verwijderen. Een virtueel netwerk verwijderen:

1. Voer in het zoekvak boven aan de portal, *virtuele netwerken* in het zoekvak in. Wanneer **virtuele netwerken** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer in de lijst met virtuele netwerken, het virtuele netwerk dat u wilt verwijderen.
3. Controleer of er zijn geen apparaten die zijn verbonden met het virtuele netwerk door te selecteren **verbonden apparaten**onder **instellingen**. Als er verbonden apparaten, moet u deze verwijderen voordat u het virtuele netwerk kunt verwijderen. Als er geen verbonden apparaten zijn, selecteert u **overzicht**.
4. Selecteer **Verwijderen**.
5. Selecteer om te bevestigen dat de verwijdering van het virtuele netwerk, **Ja**.

**Opdrachten**

- Azure CLI: [azure network vnet verwijderen](/cli/azure/network/vnet#az_network_vnet_delete)
- PowerShell: [Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork)

## <a name="permissions"></a>Machtigingen

Als u wilt uitvoeren op virtuele netwerken, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

| Bewerking                                  |   Name                                |
|---------------------------------------- |   --------------------------------    |
|Microsoft.Network/virtualNetworks/read   |   Lezen van een virtueel netwerk              |
|Microsoft.Network/virtualNetworks/write  |   Een virtueel netwerk maken of bijwerken  |
|Microsoft.Network/virtualNetworks/delete |   Een virtueel netwerk verwijderen            |

## <a name="next-steps"></a>Volgende stappen

- Maak een virtueel netwerk met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken
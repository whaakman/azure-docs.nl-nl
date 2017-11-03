---
title: Maken, wijzigen of verwijderen van een virtuele Azure-netwerk | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een virtueel netwerk in Azure.
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
ms.openlocfilehash: 74aace2136136c25bc56327d38cfbab168265401
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-virtual-network"></a>Maken, wijzigen of verwijderen van een virtueel netwerk

Informatie over het maken en verwijderen van een virtueel netwerk en -instellingen, zoals DNS-servers en IP-adresruimte voor een bestaand virtueel netwerk wijzigen.

Een virtueel netwerk is een weergave van uw eigen netwerk in de cloud. Een virtueel netwerk is een logische isolatie van de Azure-cloud die is toegewezen aan uw Azure-abonnement. Voor elk virtueel netwerk die u maakt, kunt u het volgende doen:
- Kies een adresruimte toewijzen. Een adresruimte bestaat uit een of meer adresbereiken die zijn gedefinieerd met de notatie (Classless Inter-Domain Routing), zoals 10.0.0.0/16.
- Kies de Azure DNS-server, gebruiken of uw eigen DNS-server. Alle resources die zijn verbonden met het virtuele netwerk zijn toegewezen deze DNS-server voor naamomzetting in het virtuele netwerk.
- Het virtuele netwerk segmenteren in subnetten, elk met een eigen-adresbereik op in de adresruimte van het virtuele netwerk. Zie voor informatie over het maken, wijzigen en verwijderen van subnetten, [toevoegen, wijzigen of verwijderen subnetten](virtual-network-manage-subnet.md).

In dit artikel wordt uitgelegd hoe maken, wijzigen en verwijderen van virtuele netwerken met behulp van het Azure Resource Manager-implementatiemodel.

## <a name="before"></a>Voordat u begint

Voordat u de taken die worden beschreven in dit artikel, moet u de volgende vereisten voldoen:

- Als u geen ervaring met het werken met virtuele netwerken, raden wij aan u wordt aangeraden de oefening in [maken van uw eerste virtuele Azure-netwerk](virtual-network-get-started-vnet-subnet.md). In deze oefening kunt u vertrouwd raken met virtuele netwerken.
- Bekijk voor meer informatie over limieten voor virtuele netwerken, [Azure limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Aanmelden bij de Azure-portal, Azure-opdrachtregelprogramma (Azure CLI) of Azure PowerShell met behulp van uw Azure-account. Als u geen Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u gebruiken van PowerShell-opdrachten wilt voor het voltooien van de taken die in dit artikel wordt beschreven, moet u eerst [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u de meest recente versie van de Azure PowerShell-cmdlets die is geïnstalleerd. Als u de help voor PowerShell-opdrachten in de voorbeelden, voer `get-help <command> -full`.
- Als u wilt dat Azure CLI-opdrachten gebruiken voor het voltooien van de taken die in dit artikel wordt beschreven, moet u eerst [Azure CLI installeren en configureren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u de meest recente versie van Azure CLI is geïnstalleerd. Als u hulp bij de Azure CLI-opdrachten, voer `az <command> --help`.


## <a name="create-vnet"></a>Een virtueel netwerk maken

Een virtueel netwerk maken:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Klik op **nieuw** > **Networking** > **virtueel netwerk**.
3. Op de **virtueel netwerk** blade in de **een implementatiemodel selecteren** vak, laat u **Resource Manager** geselecteerd en klik vervolgens op **maken**.
4. Op de **virtueel netwerk maken** blade invoeren of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Naam**: de naam moet uniek zijn in de [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) dat u selecteert voor het maken van het virtuele netwerk in. U kunt de naam niet wijzigen nadat het virtuele netwerk is gemaakt. U kunt meerdere virtuele netwerken maken gedurende een bepaalde periode. Zie voor de naamgeving van suggesties [naamconventies](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Na een naamgevingsconventie kunt maken het gemakkelijker om meerdere virtuele netwerken te beheren.
    - **Adresruimte**: Geef de adresruimte in CIDR-notatie. De adresruimte die u definieert mag public of private (RFC 1918). Of u de adresruimte als openbare of persoonlijke definieert, is de adresruimte bereikbaar zijn alleen binnen het virtuele netwerk van onderling verbonden virtuele netwerken en van een on-premises netwerken die u hebt gekoppeld aan het virtuele netwerk. U kunt de volgende adresruimten niet toevoegen:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (Broadcast)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (interne DNS)

      Hoewel u slechts één adresruimte definiëren kunt wanneer u het virtuele netwerk maakt, kunt u meer adresruimten toevoegen nadat het virtuele netwerk is gemaakt. Zie voor meer informatie over het toevoegen van een adresruimte aan een bestaand virtueel netwerk, [toevoegen of verwijderen van een adresruimte](#add-address-spaces) in dit artikel.

      >[!WARNING]
      >Als een virtueel netwerk bevat adresruimten die met een ander virtueel netwerk overlappen of on-premises netwerk, kunnen de twee netwerken kunnen niet worden verbonden. Voordat u een adresruimte definiëren, overweeg of wilt u mogelijk het virtueel netwerk verbinden met andere virtuele netwerken of on-premises netwerken in de toekomst.
      >
      >

    - **De subnetnaam van het**: de subnetnaam moet uniek zijn binnen het virtuele netwerk. U kunt de subnetnaam niet wijzigen nadat het subnet is gemaakt. De portal is vereist dat u één subnet definieert bij het maken van een virtueel netwerk, zelfs als een virtueel netwerk is niet beschikken over subnetten. U kunt slechts één subnet in de portal definiëren wanneer u een virtueel netwerk maken. U kunt meer subnetten toevoegen aan het virtuele netwerk later, nadat het virtuele netwerk is gemaakt. Zie voor informatie over het toevoegen van een subnet met een virtueel netwerk [een subnet maken](virtual-network-manage-subnet.md#create-subnet) in [maken, wijzigen of verwijderen subnetten](virtual-network-manage-subnet.md). U kunt een virtueel netwerk met meerdere subnetten met behulp van Azure CLI of PowerShell maken.

      >[!TIP]
      >Beheerders maken soms verschillende subnetten wilt filteren of beheren met het routeren van verkeer tussen de subnetten. Voordat u subnetten definiëren, houd rekening met hoe het handig om te filteren en verkeer leiden tussen uw subnetten. Zie voor meer informatie over het filteren van verkeer tussen subnetten, [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md). Azure automatisch routes verkeer tussen subnetten, maar u kunt onderdrukken Azure standaardroutes. Zie voor meer informatie over het onderdrukken van Azure subnet verkeer standaardroutering, [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md).
      >

    - **Adresbereik van**: het bereik moet zich in de adresruimte die u hebt opgegeven voor het virtuele netwerk. Het kleinste bereik dat kunt u opgeven is slechts/29, waarmee u acht IP-adressen voor het subnet. Azure behoudt zich het eerste en laatste adres in elk subnet voor het protocol overeenstemming. Drie extra adressen zijn gereserveerd voor gebruik van Azure service. Als gevolg hiervan is een virtueel netwerk met een adresbereik subnet van slechts/29 slechts drie bruikbare IP-adressen. Als u een virtueel netwerk verbinden met een VPN-gateway wilt, moet u een gatewaysubnet maken. Meer informatie over [specifiek adresbereik overwegingen voor het gateway-subnetten](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). U kunt het adresbereik wijzigen nadat het subnet is gemaakt, klikt u onder bepaalde omstandigheden. Zie voor meer informatie over het wijzigen van een subnet-adresbereik, [subnetinstellingen wijzigen](#change-subnet) in [toevoegen, wijzigen of verwijderen subnetten](virtual-network-manage-subnet.md).
    - **Abonnement**: Selecteer een [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). U kunt hetzelfde virtuele netwerk niet gebruiken in meer dan één Azure-abonnement. U kunt echter een virtueel netwerk in één abonnement verbinden met een virtueel netwerk in andere abonnementen. Als u wilt verbinding maken met virtuele netwerken tot verschillende abonnementen behoren, Azure VPN-Gateway of virtueel netwerk peering te gebruiken. Een Azure-resource waarmee u verbinding met het virtuele netwerk maakt moet zich in hetzelfde abonnement als het virtuele netwerk.
    - **Resourcegroep**: Selecteer een bestaande [resourcegroep](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) of maak een nieuwe. Een Azure-resource waarmee u verbinding met het virtuele netwerk maakt kan niet in dezelfde resourcegroep bevinden als het virtuele netwerk of in een andere resourcegroep.
    - **Locatie**: Selecteer een Azure [locatie](https://azure.microsoft.com/regions/), ook wel aangeduid als een regio. Een virtueel netwerk kan slechts één Azure-locatie liggen. U kunt echter een virtueel netwerk op één locatie met een virtueel netwerk in een andere locatie verbinden via een VPN-gateway. Een Azure-resource waarmee u verbinding met het virtuele netwerk maakt moet op dezelfde locatie als het virtuele netwerk.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[AZ network vnet maken](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nieuwe-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Virtuele netwerken weergeven en instellingen

Virtuele netwerken en instellingen weergeven:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Voer in het zoekvak portal **virtuele netwerken**. Klik in de zoekresultaten op **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, klikt u op het virtuele netwerk dat u wilt weergeven van instellingen voor.
4. De volgende instellingen worden weergegeven op de blade voor het virtuele netwerk dat u hebt geselecteerd:
    - **Overzicht**: bevat informatie over het virtuele netwerk, met inbegrip van adresruimte en DNS-servers. De volgende schermafbeelding ziet u de instellingen van het overzicht voor een virtueel netwerk met de naam **MyVNet**:

        ![Network interface-overzicht](./media/virtual-network-manage-network/vnet-overview.png)

      Op de **overzicht** blade kunt u een virtueel netwerk verplaatsen naar een ander abonnement of de resource-groep. Zie voor informatie over het verplaatsen van een virtueel netwerk, [resources verplaatsen naar een andere resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel worden de vereisten en het verplaatsen van resources met behulp van de Azure-portal, PowerShell en Azure CLI. Alle resources die zijn verbonden met het virtuele netwerk moeten worden verplaatst met het virtuele netwerk.
    - **Adresruimte**: de adresruimten die zijn toegewezen aan het virtuele netwerk worden vermeld. Meer informatie over het toevoegen en verwijderen van een adresruimte, voer de stappen in [toevoegen of verwijderen van een adresruimte](#address-spaces) in dit artikel.
    - **Verbonden apparaten**: alle bronnen die zijn verbonden met het virtuele netwerk worden vermeld. In de vorige schermafbeelding zijn drie netwerkinterfaces en één load balancer verbonden met het virtuele netwerk. De nieuwe resources die u maakt en verbinding maken met het virtuele netwerk worden weergegeven. Als u een resource die is verbonden met het virtuele netwerk verwijdert, worden deze niet meer weergegeven in de lijst.
    - **Subnetten**: een lijst met subnetten die in het virtuele netwerk voorkomen wordt weergegeven. Zie voor informatie over het toevoegen en verwijderen van een subnet, [een subnet maken](virtual-network-manage-subnet.md#create-subnet) en [verwijderen van een subnet](virtual-network-manage-subnet.md#delete-subnet) in [toevoegen, wijzigen of verwijderen subnetten](virtual-network-manage-subnet.md).
    - **DNS-servers**: U kunt opgeven of de Azure interne DNS-server of een aangepaste DNS-server naamomzetting biedt voor apparaten die zijn verbonden met het virtuele netwerk. Wanneer u een virtueel netwerk met behulp van de Azure-portal maakt, worden de Azure DNS-servers voor naamomzetting binnen een virtueel netwerk standaard gebruikt. Voor het wijzigen van de DNS-servers, voer de stappen in [toevoegen, wijzigen of verwijderen van een DNS-server](#dns-servers) in dit artikel.
    - **Peerings**: als er bestaande peerings in het abonnement, worden deze hier weergegeven. U kunt instellingen voor bestaande peerings weergeven of maken, wijzigen of verwijderen van peerings. Zie voor meer informatie over peerings, [virtuele netwerk peering](virtual-network-peering-overview.md).
    - **Eigenschappen**: vindt u instellingen over van het virtuele netwerk, met inbegrip van de resource-ID van het virtuele netwerk en het Azure-abonnement in.
    - **Diagram**: het diagram biedt een visuele representatie van alle apparaten die zijn verbonden met het virtuele netwerk. Het diagram heeft enkele belangrijke informatie over de apparaten. Klik op het apparaat voor het beheren van een apparaat in deze weergave in het diagram.
    - **Algemene instellingen voor Azure**: voor meer informatie over algemene instellingen voor Azure, Zie de volgende informatie:
        *   [Activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[AZ network vnet show](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Toevoegen of verwijderen van een adresruimte

U kunt toevoegen en verwijderen van adresruimten voor virtueel netwerk. Een adresruimte moet worden opgegeven in CIDR-notatie en kan niet overlappen met andere adresruimten binnen hetzelfde virtuele netwerk. De adresruimten die u definieert mag public of private (RFC 1918). Of u de adresruimte als openbare of persoonlijke definieert, is de adresruimte bereikbaar zijn alleen binnen het virtuele netwerk van onderling verbonden virtuele netwerken en van een on-premises netwerken die u hebt gekoppeld aan het virtuele netwerk. U kunt de volgende adresruimten niet toevoegen:

- 224.0.0.0/4 (Multicast)
- 255.255.255.255/32 (Broadcast)
- 127.0.0.0/8 (Loopback)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (interne DNS)

Toevoegen of verwijderen van een adresruimte:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Voer in het zoekvak portal **virtuele netwerken**. Selecteer in de lijst met zoekresultaten **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, klikt u op het virtuele netwerk waarvoor u wilt toevoegen of verwijderen van een adresruimte.
4. Op het virtuele netwerk blade onder **instellingen**, klikt u op **adresruimte**.
5. Op de blade voor de adresruimte, voert u een van de volgende opties:
    - **Voeg een adresruimte**: Voer de nieuwe adresruimte. De adresruimte overlappen niet met een bestaande adresruimte die is gedefinieerd voor het virtuele netwerk.
    - **Verwijderen van een adresruimte**: met de rechtermuisknop op een adresruimte en klik vervolgens op **verwijderen**. Als een subnet in de adresruimte bestaat, kunt u de adresruimte niet verwijderen. Als u wilt verwijderen een adresruimte, moet u eerst verwijderen subnetten (en alle resources die zijn verbonden met de subnetten) die zijn opgenomen in de adresruimte.
6. Klik op **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|Alleen Resource Manager|[AZ network vnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Toevoegen, wijzigen of verwijderen van een DNS-server

Alle virtuele machines die zijn verbonden met het register van het virtuele netwerk met de DNS-servers die u voor het virtuele netwerk opgeeft. Ze ook de opgegeven DNS-server gebruiken voor naamomzetting. Elke netwerkinterface (NIC) in een virtuele machine kan hebben een eigen DNS-serverinstellingen. Als een NIC een eigen DNS-serverinstellingen heeft, overschrijven ze de DNS-serverinstellingen voor het virtuele netwerk. Zie voor meer informatie over NIC DNS-instellingen, [Network interface-taken en instellingen](virtual-network-network-interface.md#change-dns-servers). Zie voor meer informatie over naamomzetting voor VM's en rolinstanties in Azure Cloud Services, [naamomzetting voor VM's en rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md). Als u wilt toevoegen, wijzigen of verwijderen van een DNS-server:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account met machtigingen voor de rol Inzender netwerk (minimaal) voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Typ in het zoekvak portal **virtuele netwerken**. Selecteer in de lijst met zoekresultaten **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, klikt u op het virtuele netwerk dat u wilt wijzigen van de DNS-instellingen voor.
4. Op het virtuele netwerk blade onder **instellingen**, klikt u op **DNS-servers**.
5. Selecteer een van de volgende opties op de blade met een lijst met DNS-servers:
    - **Standaard (Azure verschafte)**: alle resourcenamen en privé IP-adressen automatisch worden geregistreerd met de Azure DNS-servers. U kunt omzetten van namen tussen alle bronnen die zijn verbonden met hetzelfde virtuele netwerk. U kunt deze optie niet gebruiken voor het omzetten van namen in virtuele netwerken. Voor het omzetten van namen in virtuele netwerken, moet u een aangepaste DNS-server.
    - **Aangepaste**: U kunt een of meer servers, tot de Azure is bereikt voor een virtueel netwerk toevoegen. Zie voor meer informatie over de limieten van DNS-server, [Azure limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). U hebt de volgende opties:
        - **Toevoegen van een adres**: de server toevoegen aan uw lijst met virtuele netwerk DNS-servers. Deze optie wordt ook de DNS-server geregistreerd bij Azure. Als u hebt al een DNS-server geregistreerd met Azure, kunt u die DNS-server in de lijst.
        - **Verwijder een adres**: naast de server die u wilt verwijderen, klikt u op **X**. De server verwijdert, wordt de server alleen uit deze lijst virtueel netwerk. De DNS-server blijft geregistreerd in Azure voor uw virtuele netwerken te gebruiken.
        - **DNS-serveradressen rangschikken**: het is belangrijk om te controleren of weer te geven die uw DNS-servers in de juiste volgorde voor uw omgeving. Een lijst met DNS-server worden gebruikt in de volgorde waarin ze worden opgegeven. Ze werken niet als de installatie van een round robin. Als de eerste DNS-server in de lijst kan worden bereikt, gebruikt de client die DNS-server, ongeacht of de DNS-server correct werkt. Verwijder alle DNS-servers die worden vermeld en vervolgens weer in de volgorde die u wenst toe te voegen.
        - **Een adres wijzigen**: markeert u de DNS-server in de lijst en voer vervolgens de nieuwe naam.
6. Klik op **Opslaan**.
7. Start opnieuw op de virtuele machines die zijn verbonden met het virtuele netwerk, zodat ze de nieuwe DNS-serverinstellingen worden toegewezen. Virtuele machines blijven hun huidige DNS-instellingen gebruiken totdat ze opnieuw worden gestart.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[AZ network vnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Een virtueel netwerk verwijderen

U kunt een virtueel netwerk alleen verwijderen als er geen resources mee zijn verbonden zijn. Als er bronnen die zijn verbonden met een subnet in het virtuele netwerk, moet u eerst de resources die zijn verbonden met alle subnetten in het virtuele netwerk verwijderen. De stappen waarmee u een bron verwijderen, is afhankelijk van de resource. Als u wilt weten hoe u resources verwijderen die zijn verbonden met subnetten, Raadpleeg de documentatie voor elk resourcetype die u wilt verwijderen. Een virtueel netwerk verwijderen:

1. Aanmelden bij de [portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Voer in het zoekvak portal **virtuele netwerken**. Klik in de zoekresultaten op **virtuele netwerken**.
3. Op de **virtuele netwerken** blade, selecteer het virtuele netwerk dat u wilt verwijderen.
4. Op de blade virtueel netwerk om te bevestigen dat er geen apparaten zijn verbonden met het virtuele netwerk onder **instellingen**, klikt u op **verbonden apparaten**. Als er verbonden apparaten, moet u deze verwijderen voordat u het virtuele netwerk kunt verwijderen. Als er geen verbonden apparaten zijn, klikt u op **overzicht**.
5. Klik boven aan de blade op de **verwijderen** pictogram.
6. Bevestig het verwijderen van het virtuele netwerk, klikt u op **Ja**.


**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|Azure CLI|[Azure network vnet verwijderen](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Volgende stappen

- Zie voor een virtuele machine maken en vervolgens verbinding met een virtueel netwerk, [een virtueel netwerk maken en verbinding maken met virtuele machines](virtual-network-get-started-vnet-subnet.md#create-vms).
- Als u wilt filteren van netwerkverkeer tussen subnetten binnen een virtueel netwerk, Zie [netwerkbeveiligingsgroepen maken](virtual-networks-create-nsg-arm-pportal.md).
- Zie to-peer-een virtueel netwerk met een ander virtueel netwerk, [peering van een virtueel netwerk maken](virtual-network-create-peering.md#portal).
- Zie voor meer informatie over opties voor het verbinden van een virtueel netwerk met een on-premises netwerk, [over VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

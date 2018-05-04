---
title: Maken, wijzigen of verwijderen van een Azure-netwerk-interface | Microsoft Docs
description: Meer informatie over een netwerkinterface is maken, instellingen voor wijzigen en verwijderen.
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
ms.openlocfilehash: 72c3968b59fda10d81af553cbf2324a2683c596b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="create-change-or-delete-a-network-interface"></a>Maken, wijzigen of verwijderen van een netwerkinterface

Informatie over het maken, de instellingen voor wijzigen en verwijderen van een netwerkinterface. Een netwerkinterface kunt een virtuele Machine van Azure om te communiceren met internet, Azure en lokale bronnen. Wanneer u een virtuele machine met de Azure portal maakt, maakt de portal één netwerkinterface met de standaardinstellingen voor u. U kunt in plaats daarvan netwerkinterfaces te maken met aangepaste instellingen en een of meer netwerkinterfaces aan een virtuele machine toevoegen wanneer u deze maakt. U kunt ook standaard netwerkinterface-instellingen voor een bestaande netwerkinterface wijzigen. In dit artikel wordt uitgelegd hoe een netwerkinterface met aangepaste instellingen te maken, bestaande instellingen, zoals de toewijzing van netwerken filter (netwerkbeveiligingsgroep), subnettoewijzing, DNS-serverinstellingen en doorsturen via IP, wijzigen en verwijderen van een netwerkinterface.

Als u moet toe te voegen, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, Zie [beheren IP-adressen](virtual-network-network-interface-addresses.md). Als u toevoegen netwerkinterfaces wilt aan of verwijderen van netwerkinterfaces van virtuele machines, Zie [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md).


## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Deze zelfstudie vereist de Azure PowerShell moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

Het account dat u zich bij Azure met aanmelden moet worden toegewezen aan een minimum, machtigingen voor de rol Inzender netwerk voor uw abonnement. Zie voor meer informatie over het toewijzen van rollen en machtigingen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).

## <a name="create-a-network-interface"></a>Een netwerkinterface maken

Wanneer u een virtuele machine met de Azure portal maakt, maakt de portal een netwerkinterface met de standaardinstellingen voor u. Als u liever uw instellingen voor de interface netwerk instelt, kunt u een netwerkinterface maken met aangepaste instellingen en de netwerkinterface koppelen aan een virtuele machine bij het maken van de virtuele machine (met PowerShell of Azure CLI). U kunt ook een netwerkinterface maken en toe te voegen aan een bestaande virtuele machine (met PowerShell of Azure CLI). Zie voor informatie over het maken van een virtuele machine met een bestaande netwerkinterface of toevoegen aan of verwijderen van netwerkinterfaces van de bestaande virtuele machines, [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md). Voordat u een netwerkinterface maakt, hebt u een bestaande [virtueel netwerk](manage-virtual-network.md#create-a-virtual-network) in dezelfde locatie en abonnement maakt u een netwerkinterface in.

1. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer **+ toevoegen** onder **netwerkinterfaces**.
3. Voer, of Selecteer waarden voor de volgende instellingen en selecteer **maken**:

    |Instelling|Vereist?|Details|
    |---|---|---|
    |Naam|Ja|De naam moet uniek zijn binnen de resourcegroep die u selecteert. Na verloop van tijd waarschijnlijk hebt u verschillende netwerkinterfaces in uw Azure-abonnement. Zie voor suggesties bij het maken van een naamgevingsconventie voor het beheer van meerdere netwerkinterfaces eenvoudiger, [naamconventies](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). De naam kan niet worden gewijzigd nadat de netwerkinterface is gemaakt.|
    |Virtueel netwerk|Ja|Selecteer het virtuele netwerk voor de netwerkinterface. U kunt alleen een netwerkinterface toewijzen aan een virtueel netwerk dat uit hetzelfde abonnement en dezelfde locatie als de netwerkinterface bestaat. Zodra een netwerkinterface is gemaakt, kunt u het virtuele netwerk dat is toegewezen aan niet wijzigen. De virtuele machine toevoegen van de netwerkinterface te moet tevens aanwezig zijn in dezelfde locatie en abonnement als de netwerkinterface.|
    |Subnet|Ja|Selecteer een subnet in het virtuele netwerk dat u hebt geselecteerd. Het subnet dat de netwerkinterface is toegewezen aan nadat deze gemaakt, kunt u wijzigen.|
    |Privé IP-adrestoewijzing|Ja| Deze instelling kiezen van de toewijzingsmethode voor het IPv4-adres. Kies in de volgende toewijzingsmethoden: **dynamische:** wanneer u deze optie selecteert, Azure wijst automatisch het volgende beschikbare adres van de adresruimte van het subnet dat u hebt geselecteerd. **Statische:** wanneer u deze optie selecteert, moet u handmatig een beschikbaar IP-adres uit in de adresruimte van het subnet geselecteerde toewijzen. Statische en dynamische adressen wijzigen niet totdat u ze wijzigen of de netwerkinterface wordt verwijderd. U kunt de toewijzingsmethode wijzigen nadat de netwerkinterface is gemaakt. De Azure-DHCP-server wijst dit adres toe aan de netwerkinterface binnen het besturingssysteem van de virtuele machine.|
    |Netwerkbeveiligingsgroep|Nee| Laat de eigenschap is ingesteld op **geen**, selecteer een bestaande [netwerkbeveiligingsgroep](virtual-networks-nsg.md), of [maken van een netwerkbeveiligingsgroep](virtual-networks-create-nsg-arm-pportal.md). Netwerkbeveiligingsgroepen kunnen u om netwerkverkeer te filteren en een netwerkinterface. U kunt nul of één netwerkbeveiligingsgroep toepassen op een netwerkinterface. Nul of één netwerkbeveiligingsgroep kan ook worden toegepast met het subnet voor dat de netwerkinterface is toegewezen aan. Wanneer een netwerkbeveiligingsgroep wordt toegepast op een netwerkinterface en het subnet dat aan de netwerkinterface is toegewezen, wordt soms onverwachte resultaten optreden. Zie voor het oplossen van netwerkbeveiligingsgroepen op netwerkinterfaces en subnetten toegepast [netwerkbeveiligingsgroepen oplossen](virtual-network-nsg-troubleshoot-portal.md#nsg).|
    |Abonnement|Ja|Selecteer een van uw Azure [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). De virtuele machine die u een netwerkinterface om te en het virtuele netwerk dat u verbinding om te maakt koppelen, moet zich in hetzelfde abonnement.|
    |Privé IP-adres (IPv6)|Nee| Als u dit selectievakje inschakelt, wordt een IPv6-adres toegewezen aan de netwerkinterface, naast het IPv4-adres dat is toegewezen aan de netwerkinterface. Zie de [IPv6](#IPv6) sectie van dit artikel voor belangrijke informatie over het gebruik van IPv6 met netwerkinterfaces. U kunt een methode voor het toewijzen voor de IPv6-adres niet selecteren. Als u een IPv6-adres toewijzen kiest, wordt het toegewezen met de dynamische methode.
    |IPv6-naam (alleen weergegeven wanneer de **particuliere IP-adres (IPv6)** selectievakje is ingeschakeld) |Ja, als de **particuliere IP-adres (IPv6)** selectievakje is ingeschakeld.| Deze naam wordt toegewezen aan een secundaire IP-configuratie voor de netwerkinterface. Zie voor meer informatie over IP-configuraties, [netwerkinterface-instellingen bekijken](#view-network-interface-settings).|
    |Resourcegroep|Ja|Selecteer een bestaande [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) of maak een. Een netwerkinterface kan bestaan in de groep dezelfde of een andere bron dan de virtuele machine koppelt, of het virtuele netwerk dat u deze verbinding.|
    |Locatie|Ja|De virtuele machine als u een netwerkinterface om te koppelen en het virtuele netwerk u deze met verbindt moet aanwezig zijn in dezelfde [locatie](https://azure.microsoft.com/regions), ook wel een regio.|

De portal biedt niet de optie voor een openbaar IP-adres toewijzen aan de netwerkinterface bij het maken, hoewel de portal een openbaar IP-adres maken en aan een netwerkinterface toewijzen bij het maken van een virtuele machine via de portal. Zie voor meer informatie over het toevoegen van een openbaar IP-adres aan de netwerkinterface nadat deze is gemaakt, [beheren IP-adressen](virtual-network-network-interface-addresses.md). Als u maken van een netwerkinterface met een openbaar IP-adres wilt, moet u de CLI of PowerShell gebruiken voor het maken van de netwerkinterface.

De portal biedt niet de optie voor de netwerkinterface toewijzen aan beveiligingsgroepen van toepassing, maar de Azure CLI en PowerShell doen. Zie voor meer informatie over beveiligingsgroepen van toepassing, [toepassing beveiligingsgroepen](security-overview.md#application-security-groups).

>[!Note]
> Azure wijst een MAC-adres toe aan de netwerkinterface nadat de netwerkinterface is gekoppeld aan een virtuele machine en de virtuele machine voor het eerst wordt gestart. U kunt het MAC-adres waarmee Azure worden toegewezen aan de netwerkinterface niet opgeven. Het MAC-adres blijft toegewezen aan de netwerkinterface tot de netwerkinterface is verwijderd of de privé IP-adres is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface is gewijzigd. Zie voor meer informatie over IP-adressen en IP-configuraties, [beheren IP-adressen](virtual-network-network-interface-addresses.md)

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic#az_network_nic_create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface#create)|

## <a name="view-network-interface-settings"></a>Netwerkinterface-instellingen weergeven

U kunt weergeven en wijzigen van de meeste instellingen voor een netwerkinterface nadat deze gemaakt. DNS-achtervoegsel of toepassing lidmaatschap van de beveiligingsgroep voor de netwerkinterface niet wordt weergegeven in de portal. U kunt de PowerShell of Azure CLI [opdrachten](#view-settings-commands) om de DNS-achtervoegsel en toepassing beveiligingsgroep weer te geven.

1. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt weergeven of wijzigen van instellingen voor in de lijst.
3. De volgende items worden vermeld voor de netwerkinterface die u hebt geselecteerd:
    - **Overzicht:** bevat informatie over de netwerkinterface, zoals het IP-adressen toegewezen aan deze, het virtuele netwerk/subnet aan de netwerkinterface is toegewezen en de virtuele machine de netwerkinterface is gekoppeld aan (als deze is gekoppeld aan een). De volgende afbeelding ziet u de instellingen van het overzicht voor een netwerkinterface met de naam **mywebserver256**: ![Network interface-overzicht](./media/virtual-network-network-interface/nic-overview.png) kunt u een netwerkinterface verplaatsen naar een andere resourcegroep of abonnement door te selecteren (**wijzigen**) naast de **resourcegroep** of **naam abonnement**. Als u de netwerkinterface verplaatst, moet u alle bronnen die betrekking hebben op de netwerkinterface met het verplaatsen. Als de netwerkinterface is gekoppeld aan een virtuele machine, bijvoorbeeld, moet u ook verplaatsen de virtuele machine en andere bronnen betrekking hebben op virtuele machine. Zie voor het verplaatsen van een netwerkinterface [resource verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Dit artikel worden de vereisten en het verplaatsen van resources met behulp van de Azure-portal, PowerShell en de Azure CLI.
    - **IP-configuraties:** openbare en persoonlijke IPv4 en IPv6-adressen zijn toegewezen aan IP-configuraties worden hier vermeld. Als een IPv6-adres is toegewezen aan een IP-configuratie, wordt het adres niet weergegeven. Zie voor meer informatie over IP-configuraties en het toevoegen en verwijderen van IP-adressen, [configureren IP-adressen voor een Azure-netwerk-interface](virtual-network-network-interface-addresses.md). Doorsturen via IP en subnettoewijzing zijn ook in deze sectie geconfigureerd. Zie voor meer informatie over deze instellingen, [in- of uitschakelen van doorsturen via IP](#enable-or-disable-ip-forwarding) en [subnettoewijzing wijzigen](#change-subnet-assignment).
    - **DNS-servers:** kunt u opgeven welke DNS-server een netwerkinterface wordt toegewezen door de Azure-DHCP-servers. De netwerkinterface kunt de instelling overnemen van het virtuele netwerk dat de netwerkinterface is toegewezen aan of een aangepaste instelling die de instelling voor het virtuele netwerk dat toegewezen wordt aan hebben. Om te wijzigen wat wordt weergegeven, Zie [wijziging DNS-servers](#change-dns-servers).
    - **Netwerkbeveiligingsgroep (NSG):** geeft dit NSG is gekoppeld aan de netwerkinterface (indien aanwezig). Een NSG bevat regels voor binnenkomende en uitgaande als u wilt filteren van netwerkverkeer voor de netwerkinterface. Als een NSG gekoppeld aan de netwerkinterface is, wordt de naam van de gekoppelde NSG weergegeven. Om te wijzigen wat wordt weergegeven, Zie [koppelen of ontkoppelen van een netwerkbeveiligingsgroep](#associate-or-dissociate-a-network-security-group).
    - **Eigenschappen:** bevat de belangrijkste instellingen over van de netwerkinterface, met inbegrip van het MAC-adres (leeg als de netwerkinterface is niet gekoppeld aan een virtuele machine) en het abonnement deze voorkomt in.
    - **Effectieve beveiligingsregels:** beveiligingsregels staan als de netwerkinterface is gekoppeld aan een actieve virtuele machine en een NSG gekoppeld aan de netwerkinterface, het subnet dat toegewezen is aan of beide. Zie voor meer informatie over wat wordt weergegeven, [effectieve beveiligingsregels weergeven](#view-effective-security-rules). Zie voor meer informatie over het nsg's, [Netwerkbeveiligingsgroepen](security-overview.md).
    - **Effectieve routes:** Routes worden weergegeven als de netwerkinterface is gekoppeld aan een actieve virtuele machine. De routes zijn een combinatie van de Azure standaardroutes, eventuele door de gebruiker gedefinieerde routes en eventuele BGP-routes die mogelijk aanwezig zijn voor het subnet dat aan de netwerkinterface is toegewezen. Zie voor meer informatie over wat wordt weergegeven, [effectieve routes weergeven](#view-effective-routes). Zie voor meer informatie over Azure standaardroutes en de gebruiker gedefinieerde routes, [Routering-overzicht](virtual-networks-udr-overview.md).
    - **Algemene instellingen van Azure Resource Manager:** Zie voor meer informatie over de algemene instellingen van Azure Resource Manager, [activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [labels](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Vergrendelt](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), en [automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

<a name="view-settings-commands"></a>**Opdrachten**

Als een IPv6-adres is toegewezen aan een netwerkinterface, retourneert de PowerShell-uitvoer het feit dat het adres wordt toegewezen, maar het toegewezen adres wordt niet geretourneerd. Op dezelfde manier de CLI retourneert het feit dat het adres is toegewezen, maar retourneert *null* in de uitvoer voor het adres.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[lijst met AZ netwerken nic](/cli/azure/network/nic#az_network_nic_list) netwerkinterfaces weergeven in het abonnement; [az netwerk nic weergeven](/cli/azure/network/nic#az_network_nic_show) voor een netwerkinterface-instellingen te bekijken|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) netwerkinterfaces weergeven in het abonnement of de weergave-instellingen voor een netwerkinterface|

## <a name="change-dns-servers"></a>DNS-servers wijzigen

De DNS-server wordt toegewezen door de Azure-DHCP-server aan de netwerkinterface in het besturingssysteem van de virtuele machine. De DNS-server toegewezen is wat u ook de DNS-serverinstellingen voor een netwerkinterface. Zie voor meer informatie over instellingen voor het omzetten van de naam voor een netwerkinterface, [naamomzetting voor virtuele machines](virtual-networks-name-resolution-for-vms-and-role-instances.md). De netwerkinterface kan de instellingen overnemen van het virtuele netwerk, of een eigen unieke instellingen die de instelling voor het virtuele netwerk te overschrijven.

1. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt wijzigen van een DNS-server in de lijst.
3. Selecteer **DNS-servers** onder **instellingen**.
4. Selecteer een:
    - **Overnemen van een virtueel netwerk**: Selecteer deze optie worden overgenomen van de DNS-serverinstelling gedefinieerd voor de netwerkinterface is toegewezen aan virtueel netwerk. Op het niveau van het virtuele netwerk is een aangepaste DNS-server of de Azure DNS-server gedefinieerd. De Azure DNS-server kan omzetten van hostnamen voor resources die zijn toegewezen aan hetzelfde virtuele netwerk. FQDN-naam moet worden gebruikt om op te lossen voor bronnen die zijn toegewezen aan verschillende virtuele netwerken.
    - **Aangepaste**: U kunt uw eigen DNS-server voor het omzetten van namen over meerdere virtuele netwerken configureren. Geef het IP-adres van de server die u wilt gebruiken als een DNS-server. De DNS-serveradres die u opgeeft alleen aan deze netwerkinterface is toegewezen en wordt een DNS-instelling voor het virtuele netwerk dat de netwerkinterface is toegewezen aan.
5. Selecteer **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic-update](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>In- of uitschakelen van doorsturen via IP

Doorsturen via IP maakt gebruik van de virtuele machine die een netwerkinterface is gekoppeld aan:
- Ontvangen netwerkverkeer dat niet is bestemd voor een van de IP-adressen toegewezen aan een van de IP-configuraties toegewezen aan de netwerkinterface.
- Verzenden van netwerkverkeer met een ander IP-adres dan de toegewezen aan een van de netwerkinterface-IP-configuraties.

De instelling moet worden ingeschakeld voor elke netwerkinterface die is gekoppeld aan de virtuele machine die verkeer dat de virtuele machine moet voor het doorsturen van ontvangt. Een virtuele machine kan verkeer doorsturen of er meerdere netwerkinterfaces of één netwerkinterface is gekoppeld. Doorsturen via IP is een Azure-instelling, moet ook een toepassing kunnen doorsturen van het verkeer, zoals firewall, WAN-optimalisatie en taakverdeling toepassingen uitvoeren in de virtuele machine. Wanneer een virtuele machine wordt uitgevoerd netwerktoepassingen, is de virtuele machine vaak aangeduid als een virtueel netwerkapparaat. U kunt een lijst weergeven met gereed is voor het implementeren van virtuele netwerkapparaten in het [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Doorsturen via IP wordt doorgaans gebruikt met de gebruiker gedefinieerde routes. Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md).

1. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt in- of uitschakelen van doorsturen via IP voor.
3. Selecteer **IP-configuraties** in de **instellingen** sectie.
4. Selecteer **ingeschakeld** of **uitgeschakelde** (standaardinstelling) om de instelling te wijzigen.
5. Selecteer **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic-update](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="change-subnet-assignment"></a>Subnettoewijzing wijzigen

U kunt het subnet, maar niet het virtuele netwerk, dat een netwerkinterface is toegewezen aan wijzigen.

1. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt wijzigen van subnettoewijzing voor.
3. Selecteer **IP-configuraties** onder **instellingen**. Als een particuliere IP-adressen voor alle IP-configuraties vermeld **(statische)** naast, moet u de methode voor het IP-adres toewijzen wijzigen naar een dynamische via de stappen volgen. Alle particuliere IP-adressen moeten worden toegewezen met de dynamische toewijzingsmethode wijzigen van de subnettoewijzing van het voor de netwerkinterface. Als de adressen zijn toegewezen met de dynamische methode, kunt u verder met stap 5. Als een IPv4-adressen zijn toegewezen met de statische toewijzingsmethode, de volgende stappen uitvoeren voordat de toewijzingsmethode wijzigen naar een dynamische:
    - Selecteer de IP-configuratie die u wilt wijzigen van de toewijzingsmethode voor IPv4-adres voor uit de lijst met IP-configuraties.
    - Selecteer **dynamische** voor de persoonlijke IP-adres **toewijzing** methode. U kunt een IPv6-adres niet toewijzen met de statische toewijzingsmethode.
    - Selecteer **Opslaan**.
4. Selecteer het subnet dat u wilt verplaatsen van de netwerkinterface op uit de **Subnet** vervolgkeuzelijst.
5. Selecteer **Opslaan**. Nieuwe dynamische adressen worden toegewezen uit het adresbereik subnet voor het nieuwe subnet. Na het toewijzen van de netwerkinterface met een nieuwe subnet, kunt u een statisch IPv4-adres uit het nieuwe subnet-adresbereik toewijzen als u kiest. Zie voor meer informatie over toevoegen, wijzigen en verwijderen van IP-adressen voor een netwerkinterface meer [beheren IP-adressen](virtual-network-network-interface-addresses.md).

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic IP-configuratie bijwerken](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig)|

## <a name="add-to-or-remove-from-application-security-groups"></a>Toevoegen aan of verwijderen van beveiligingsgroepen van toepassing

De portal niet de optie voor het toewijzen van een netwerkinterface om op te geven of verwijderen van een netwerkinterface van beveiligingsgroepen van toepassing, maar de Azure CLI en PowerShell doen. Zie voor meer informatie over beveiligingsgroepen van toepassing, [toepassing beveiligingsgroepen](security-overview.md#application-security-groups) en [Maak een beveiligingsgroep toepassing](#create-an-application-security-group).

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic-update](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="associate-or-dissociate-a-network-security-group"></a>Koppelen of ontkoppelen van een netwerkbeveiligingsgroep

1. Voer in het zoekvak boven aan de portal *netwerkinterfaces* in het zoekvak. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkinterface in de lijst die u wilt een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep uit.
3. Selecteer **netwerkbeveiligingsgroep** onder **instellingen**.
4. Selecteer **Bewerken**.
5. Selecteer **netwerkbeveiligingsgroep** en selecteer vervolgens de netwerkbeveiligingsgroep die u wilt koppelen aan de netwerkinterface, of selecteer **geen**, ontkoppelen van een netwerkbeveiligingsgroep.
6. Selecteer **Opslaan**.

**Opdrachten**

- Azure CLI: [az netwerk nic-update](/cli/azure/network/nic#az-network-nic-update)
- PowerShell: [Set AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)

## <a name="delete-a-network-interface"></a>Verwijderen van een netwerkinterface

U kunt een netwerkinterface verwijderen zolang deze niet is gekoppeld aan een virtuele machine. Als een netwerkinterface is gekoppeld aan een virtuele machine, moet u eerst plaats de virtuele machine gestopt (toewijzing ongedaan gemaakt) en vervolgens loskoppelen van de netwerkinterface van de virtuele machine. Als u wilt loskoppelen van een netwerkinterface van een virtuele machine, voer de stappen in [loskoppelen van een netwerkinterface van een virtuele machine](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm). U kunt een netwerkinterface van een virtuele machine kan niet loskoppelen als dit de enige netwerkinterface echter gekoppeld aan de virtuele machine. Een virtuele machine moet altijd ten minste één netwerkinterface is gekoppeld. Verwijderen van een virtuele machine wordt losgekoppeld van alle netwerkinterfaces die zijn gekoppeld, maar niet de netwerkinterfaces verwijdert.

1. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer **...**  aan de rechterkant van de netwerkinterface die u wilt verwijderen uit de lijst met netwerkinterfaces.
3. Selecteer **Verwijderen**.
4. Selecteer **Ja** verwijderen van de netwerkinterface te bevestigen.

Wanneer u een netwerkinterface verwijdert, worden alle MAC of IP-adressen zijn toegewezen worden vrijgegeven.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[nic-AZ netwerk het verwijderen](/cli/azure/network/nic#az_network_nic_delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface)|

## <a name="resolve-connectivity-issues"></a>Verbindingsproblemen oplossen

Als u niet communiceren naar of van een virtuele machine, netwerk-beveiligingsregels of routes effectieve voor een netwerkinterface kunnen het probleem veroorzaakt. U hebt de volgende opties voor het probleem op te lossen:

### <a name="view-effective-security-rules"></a>De effectieve beveiligingsregels weergeven

De regels voor een effectieve beveiligingsmethode voor elke netwerkinterface die is gekoppeld aan een virtuele machine zijn een combinatie van de regels die u hebt gemaakt in een netwerkbeveiligingsgroep en [beveiligingsregels standaard](security-overview.md#default-security-rules). Inzicht in de regels voor een effectieve beveiligingsmethode voor een netwerkinterface kan u helpen bepalen waarom het niet lukt om te communiceren of naar een virtuele machine. U kunt de effectieve regels voor een netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer in het zoekvak boven aan de portal, de naam van een virtuele machine die u wilt weergeven van regels voor een effectieve beveiligingsmethode. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak. Wanneer **virtuele machines** worden weergegeven in de zoekresultaten, selecteert u deze en selecteer vervolgens een virtuele machine in de lijst.
2. Selecteer **Networking** onder **instellingen**.
3. Selecteer de naam van een netwerkinterface.
4. Selecteer **effectieve beveiligingsregels** onder **ondersteuning + probleemoplossing**.
5. Bekijk de lijst met een effectieve beveiligingsmethode regels om te bepalen of de juiste regels bestaan voor uw vereiste binnenkomende en uitgaande communicatie. Meer informatie over wat u ziet in de lijst in [netwerk groep beveiligingsoverzicht](security-overview.md).

De stroom van de IP-Controleer of functie van Azure-netwerk-Watcher kunt u ook bepalen als beveiligingsregels voor communicatie tussen een virtuele machine en een eindpunt worden voorkomen. Zie voor meer informatie, [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Opdrachten**

- Azure CLI: [az netwerk nic lijst effectief nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg)
- PowerShell: [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup) 

### <a name="view-effective-routes"></a>De effectieve routes weergeven

De effectieve routes voor de netwerkinterfaces die zijn gekoppeld aan een virtuele machine zijn een combinatie van standaardroutes, alle routes die u hebt gemaakt en alle routes doorgegeven vanuit de on-premises netwerken via BGP via een gateway virtuele Azure-netwerk. Inzicht in de effectieve routes voor een netwerkinterface kan u helpen bepalen waarom het niet lukt om te communiceren of naar een virtuele machine. U kunt de effectieve routes voor een netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer in het zoekvak boven aan de portal, de naam van een virtuele machine die u wilt weergeven van regels voor een effectieve beveiligingsmethode. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak. Wanneer **virtuele machines** worden weergegeven in de zoekresultaten, selecteert u deze en selecteer vervolgens een virtuele machine in de lijst.
2. Selecteer **Networking** onder **instellingen**.
3. Selecteer de naam van een netwerkinterface.
4. Selecteer **effectieve routes** onder **ondersteuning + probleemoplossing**.
5. Bekijk de lijst met effectieve routes om te bepalen of de juiste routes bestaan voor uw vereiste binnenkomende en uitgaande communicatie. Meer informatie over wat u ziet in de lijst in [Routering-overzicht](virtual-networks-udr-overview.md).

De volgende hop-functie van Azure-netwerk-Watcher kunt u bepalen of routes voorkomen de communicatie tussen een virtuele machine en een eindpunt dat worden ook. Zie voor meer informatie, [volgende hop](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Opdrachten**

- Azure CLI: [az netwerk nic-ingang-route-tabel weergeven](/cli/azure/network/nic#az-network-nic-show-effective-route-table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable)

## <a name="next-steps"></a>Volgende stappen
Als een virtuele machine maken met meerdere netwerkinterfaces of IP-adressen, Zie de volgende artikelen:

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere NIC's maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Één NIC VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Maak een enkel NIC-VM met een particulier IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

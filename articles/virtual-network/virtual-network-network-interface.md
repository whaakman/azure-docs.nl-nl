---
title: Maken, wijzigen of verwijderen van een Azure-netwerk-interface
titlesuffix: Azure Virtual Network
description: Leer wat een netwerkinterface is en hoe u maakt, instellingen wijzigen voor en verwijderen.
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: a26deba97d1ed61f2a6f3b54dafe9f651167ab5b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424367"
---
# <a name="create-change-or-delete-a-network-interface"></a>Maken, wijzigen of verwijderen van een netwerkinterface

Informatie over het maken, instellingen voor wijzigen en verwijderen van een netwerkinterface. Een netwerkinterface kan een Azure-Machine om te communiceren met internet, Azure en on-premises bronnen. Bij het maken van een virtuele machine met behulp van de Azure portal, wordt de portal een netwerkinterface gemaakt met de standaardinstellingen voor u. U kunt in plaats daarvan netwerkinterfaces maken met aangepaste instellingen en een of meer netwerkinterfaces toevoegen aan een virtuele machine wanneer u deze maakt. U kunt ook wijzigen van standaard netwerkinterface-instellingen voor een bestaande netwerkinterface. In dit artikel wordt uitgelegd hoe u een netwerkinterface met aangepaste instellingen maken, bestaande instellingen, zoals netwerk-filter (netwerkbeveiligingsgroep) toewijzen, subnettoewijzing, DNS-serverinstellingen en doorsturen via IP, wijzigen en verwijderen van een netwerkinterface.

Als u nodig hebt om toe te voegen, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, Zie [beheren IP-adressen](virtual-network-network-interface-addresses.md). Als u toevoegen, netwerkinterfaces wilt of verwijderen van netwerkinterfaces van virtuele machines, Zie [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md).

## <a name="before-you-begin"></a>Voordat u begint

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. In deze zelfstudie vereist de Azure PowerShell-moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="create-a-network-interface"></a>Een netwerkinterface maken

Bij het maken van een virtuele machine met behulp van de Azure portal, wordt de portal een netwerkinterface gemaakt met de standaardinstellingen voor u. Als in plaats daarvan u alle instellingen van uw netwerkinterface geeft, kunt u een netwerkinterface met aangepaste instellingen maken en de netwerkinterface koppelen aan een virtuele machine bij het maken van de virtuele machine (met behulp van PowerShell of Azure CLI). U kunt ook een netwerkinterface maken en toe te voegen aan een bestaande virtuele machine (met behulp van PowerShell of Azure CLI). Als u wilt weten hoe u een virtuele machine maken met een bestaande netwerkinterface of als u wilt toevoegen aan of verwijderen van netwerkinterfaces van een bestaande virtuele machines, Zie [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md). Voordat u een netwerkinterface maken, hebt u een bestaande [virtueel netwerk](manage-virtual-network.md#create-a-virtual-network) in dezelfde locatie en abonnement die u maakt een netwerkinterface in.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer **+ toevoegen** onder **netwerkinterfaces**.
3. Voer, of Selecteer waarden voor de volgende instellingen en selecteer vervolgens **maken**:

    |Instelling|Vereist?|Details|
    |---|---|---|
    |Name|Ja|De naam moet uniek zijn binnen de resourcegroep die u selecteert. Na verloop van tijd u waarschijnlijk hebt meerdere netwerkinterfaces in uw Azure-abonnement. Zie voor suggesties bij het maken van een naamgevingsconventie voor het beheer van meerdere netwerkinterfaces gemakkelijker, [naamconventies](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). De naam kan niet worden gewijzigd nadat de netwerkinterface is gemaakt.|
    |Virtueel netwerk|Ja|Selecteer het virtuele netwerk voor de netwerkinterface. U kunt alleen een netwerkinterface toewijzen aan een virtueel netwerk dat zich in hetzelfde abonnement en dezelfde locatie als de netwerkinterface. Zodra een netwerkinterface is gemaakt, kunt u het virtuele netwerk dat is toegewezen aan niet wijzigen. De virtuele machine toevoegen van de netwerkinterface moet tevens aanwezig zijn in dezelfde locatie en abonnement als de netwerkinterface.|
    |Subnet|Ja|Selecteer een subnet binnen het virtuele netwerk dat u hebt geselecteerd. Het subnet dat de netwerkinterface is toegewezen aan nadat deze gemaakt, kunt u wijzigen.|
    |Privé-IP-adrestoewijzing|Ja| In deze instelling is kiest u de toewijzingsmethode voor het IPv4-adres. Kies in de volgende toewijzingsmethoden: **Dynamische:** Wanneer u deze optie selecteert, wijst Azure automatisch de eerstvolgende beschikbare adres van de adresruimte van het subnet dat u hebt geselecteerd. **Statische:** Wanneer u deze optie selecteert, moet u handmatig een beschikbaar IP-adres in de adresruimte van het geselecteerde subnet toewijzen. Statische en dynamische adressen veranderen niet totdat u deze wijzigen of de netwerkinterface wordt verwijderd. U kunt de toewijzingsmethode wijzigen nadat de netwerkinterface is gemaakt. De Azure DHCP-server wijst dit adres toe aan de netwerkinterface binnen het besturingssysteem van de virtuele machine.|
    |Netwerkbeveiligingsgroep|Nee| Laat de eigenschap ingesteld op **geen**, selecteer een bestaande [netwerkbeveiligingsgroep](security-overview.md), of [maken van een netwerkbeveiligingsgroep](tutorial-filter-network-traffic.md). Netwerkbeveiligingsgroepen kunnen u filteren van netwerkverkeer naar en uit een netwerkinterface. U kunt nul of één netwerkbeveiligingsgroep toepassen op een netwerkinterface. Nul of één netwerkbeveiligingsgroep kan ook worden toegepast op het subnet dat de netwerkinterface wordt toegewezen aan. Wanneer een netwerkbeveiligingsgroep is toegepast op een netwerkinterface en het subnet dat aan de netwerkinterface is toegewezen, wordt soms onverwachte resultaten optreden. Zie voor het oplossen van netwerkbeveiligingsgroepen die zijn toegepast op netwerkinterfaces en subnetten, [problemen met netwerkbeveiligingsgroepen oplossen](diagnose-network-traffic-filter-problem.md).|
    |Abonnement|Ja|Selecteer een van uw Azure [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). De virtuele machine die u koppelt een netwerkinterface en het virtuele netwerk dat u deze met verbindt moeten zich in hetzelfde abonnement.|
    |Privé-IP-adres (IPv6)|Nee| Als u dit selectievakje inschakelt, wordt een IPv6-adres toegewezen aan de netwerkinterface, naast het IPv4-adres toegewezen aan de netwerkinterface. Zie de [IPv6](#IPv6) sectie van dit artikel voor belangrijke informatie over het gebruik van IPv6 met netwerkinterfaces. U kunt een methode voor het toewijzen van het IPv6-adres niet selecteren. Als u ervoor kiest om toe te wijzen een IPv6-adres, wordt deze toegewezen met de dynamische methode.
    |IPv6-naam (alleen weergegeven wanneer de **privé-IP-adres (IPv6)** selectievakje is ingeschakeld) |Ja, als de **privé-IP-adres (IPv6)** selectievakje is ingeschakeld.| Deze naam wordt toegewezen aan een secundaire IP-configuratie voor de netwerkinterface. Zie voor meer informatie over IP-configuraties, [netwerkinterface-instellingen weergeven](#view-network-interface-settings).|
    |Resourcegroep|Ja|Selecteer een bestaande [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) of er een maken. Een netwerkinterface kan bestaan in de dezelfde of een andere resourcegroep dan de virtuele machine die u het koppelt, of het virtuele netwerk dat u deze verbinding.|
    |Locatie|Ja|De virtuele machine als u een netwerkinterface om te koppelen en het virtuele netwerk dat u deze met verbindt moet bestaan in dezelfde [locatie](https://azure.microsoft.com/regions), ook wel een regio.|

De portal biedt niet de optie voor een openbaar IP-adres toewijzen aan de netwerkinterface bij het maken, hoewel de portal een openbaar IP-adres maken en aan een netwerkinterface toewijzen bij het maken van een virtuele machine met behulp van de portal. Zie voor informatie over het toevoegen van een openbaar IP-adres aan de netwerkinterface nadat deze is gemaakt, [beheren IP-adressen](virtual-network-network-interface-addresses.md). Als u maken van een netwerkinterface met een openbaar IP-adres wilt, moet u de CLI of PowerShell gebruiken om te maken van de netwerkinterface.

De portal biedt niet de optie voor het toewijzen van de netwerkinterface aan beveiligingsgroepen voor toepassing bij het maken van een netwerkinterface, maar de Azure CLI en PowerShell. U kunt een bestaande netwerkinterface toewijzen aan een toepassingsbeveiligingsgroep met behulp van de portal echter, zolang de netwerkinterface is gekoppeld aan een virtuele machine. Zie voor meer informatie over het toewijzen van een netwerkinterface aan een toepassingsbeveiligingsgroep, [toevoegen aan of verwijderen uit de beveiligingsgroepen voor toepassing](#add-to-or-remove-from-application-security-groups).

>[!Note]
> Azure wijst een MAC-adres toe aan de netwerkinterface, nadat de netwerkinterface is gekoppeld aan een virtuele machine en de virtuele machine voor het eerst wordt gestart. U kunt het MAC-adres waarmee Azure worden toegewezen aan de netwerkinterface niet opgeven. Het MAC-adres blijft toegewezen aan de netwerkinterface totdat de netwerkinterface wordt verwijderd of de privé IP-adres toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface is gewijzigd. Zie voor meer informatie over IP-adressen en IP-configuraties, [beheren IP-adressen](virtual-network-network-interface-addresses.md)

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic#az_network_nic_create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface#create)|

## <a name="view-network-interface-settings"></a>Netwerkinterface-instellingen weergeven

U kunt weergeven en wijzigen van de meeste instellingen voor een netwerkinterface nadat deze gemaakt. DNS-achtervoegsel of toepassing lidmaatschap van de beveiligingsgroep voor de netwerkinterface niet wordt weergegeven in de portal. U kunt de PowerShell of Azure CLI [opdrachten](#view-settings-commands) om DNS-achtervoegsel en toepassing lidmaatschap van de beveiligingsgroep weer te geven.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt weergeven of wijzigen van instellingen voor in de lijst.
3. De volgende items worden weergegeven voor de netwerkinterface die u hebt geselecteerd:
    - **Overzicht:** Bevat informatie over de netwerkinterface, zoals het IP-adressen toegewezen aan deze, het virtuele netwerk/subnet dat aan de netwerkinterface is toegewezen en de virtuele machine die de netwerkinterface is gekoppeld aan (als deze gekoppeld aan een). De volgende afbeelding ziet u de instellingen van het overzicht voor een netwerkinterface met de naam **mywebserver256**: ![Overzicht van Network interface](./media/virtual-network-network-interface/nic-overview.png) kunt u een netwerkinterface verplaatsen naar een andere resourcegroep of abonnement door te selecteren (**wijzigen**) naast de **resourcegroep** of  **De naam van abonnement**. Als u de netwerkinterface hebt verplaatst, moet u alle resources met betrekking tot de netwerkinterface met het verplaatsen. Als de netwerkinterface is gekoppeld aan een virtuele machine, bijvoorbeeld, moet u ook verplaatsen de virtuele machine en andere resources met betrekking tot virtuele machine. Zie voor het verplaatsen van een netwerkinterface [resource verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Dit artikel worden de vereisten en over het verplaatsen van resources met behulp van de Azure portal, PowerShell en de Azure CLI.
    - **IP-configuraties:** Openbare en particuliere IPv4 en IPv6-adressen toegewezen aan IP-configuraties worden hier weergegeven. Als een IPv6-adres is toegewezen aan een IP-configuratie, wordt het adres niet weergegeven. Zie voor meer informatie over IP-configuraties en het toevoegen en verwijderen van IP-adressen, [configureren IP-adressen voor een Azure-netwerkinterface](virtual-network-network-interface-addresses.md). Doorsturen via IP en subnettoewijzing zijn ook in deze sectie geconfigureerd. Zie voor meer informatie over deze instellingen, [in- of uitschakelen van doorsturen via IP](#enable-or-disable-ip-forwarding) en [wijzigen subnettoewijzing](#change-subnet-assignment).
    - **DNS-servers:** U kunt opgeven welke DNS-server een netwerkinterface wordt toegewezen door de Azure DHCP-servers. De netwerkinterface kan de instelling overnemen van het virtuele netwerk dat de netwerkinterface wordt toegewezen aan of een aangepaste instelling de instelling voor het virtuele netwerk dat toegewezen overschrijft aan hebt. Als u wilt wijzigen die wordt weergegeven, Zie [wijziging DNS-servers](#change-dns-servers).
    - **Netwerkbeveiligingsgroep (NSG):** Hiermee wordt de NSG is gekoppeld aan de netwerkinterface (indien aanwezig). Een NSG bevat binnenkomende en uitgaande regels om te filteren van netwerkverkeer voor de netwerkinterface. Als een NSG gekoppeld aan de netwerkinterface is, wordt de naam van de gekoppelde NSG weergegeven. Als u wilt wijzigen die wordt weergegeven, Zie [koppelen of ontkoppelen van een netwerkbeveiligingsgroep](#associate-or-dissociate-a-network-security-group).
    - **Eigenschappen:** Geeft de belangrijkste instellingen over de netwerkinterface, met inbegrip van de MAC-adres (leeg als u de netwerkinterface is niet gekoppeld aan een virtuele machine) en het abonnement dat die het zich bevindt in.
    - **Effectieve beveiligingsregels:**  Beveiligingsregels worden weergegeven als de netwerkinterface is gekoppeld aan een actieve virtuele machine en een NSG gekoppeld aan de netwerkinterface, het subnet dat toegewezen is aan of beide. Zie voor meer informatie over wat wordt weergegeven, [effectieve beveiligingsregels bekijken](#view-effective-security-rules). Zie voor meer informatie over nsg's, [Netwerkbeveiligingsgroepen](security-overview.md).
    - **Effectieve routes:** Routes worden weergegeven als de netwerkinterface is gekoppeld aan een actieve virtuele machine. De routes zijn een combinatie van de Azure standaardroutes, een door de gebruiker gedefinieerde routes en een BGP-routes die mogelijk aanwezig zijn voor het subnet dat de netwerkinterface wordt toegewezen aan. Zie voor meer informatie over wat wordt weergegeven, [effectieve routes weergeven](#view-effective-routes). Zie voor meer informatie over Azure standaardroutes en de gebruiker gedefinieerde routes, [routeringoverzicht](virtual-networks-udr-overview.md).
    - **Algemene Azure Resource Manager-instellingen:**  Zie voor meer informatie over algemene Azure Resource Manager-instellingen, [activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md), [toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), en [ Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

<a name="view-settings-commands"></a>**Commands**

Als een IPv6-adres is toegewezen aan een netwerkinterface, retourneert de PowerShell-uitvoer het feit dat het adres wordt toegewezen, maar de App het toegewezen adres niet retourneren. Op dezelfde manier de CLI geeft als resultaat van het feit dat het adres is toegewezen, maar retourneert *null* in de uitvoer van het adres.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[lijst met AZ network nic](/cli/azure/network/nic#az_network_nic_list) netwerkinterfaces in het abonnement; weergeven [az network nic show](/cli/azure/network/nic#az_network_nic_show) voor een netwerkinterface-instellingen te bekijken|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) netwerkinterfaces in het abonnement of de weergave-instellingen voor een netwerkinterface weergeven|

## <a name="change-dns-servers"></a>DNS-servers wijzigen

De DNS-server wordt toegewezen door de Azure DHCP-server aan de netwerkinterface binnen het besturingssysteem van de virtuele machine. De DNS-server die is toegewezen, is wat is de DNS-serverinstellingen voor een netwerkinterface. Zie voor meer informatie over instellingen voor het omzetten van de naam voor een netwerkinterface, [naamomzetting voor virtuele machines](virtual-networks-name-resolution-for-vms-and-role-instances.md). De netwerkinterface kan overnemen van de instellingen van het virtuele netwerk, of een eigen unieke instellingen die de instelling voor het virtuele netwerk worden overschreven.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt wijzigen van een DNS-server in de lijst.
3. Selecteer **DNS-servers** onder **instellingen**.
4. Selecteer een:
    - **Overnemen van virtueel netwerk**: Selecteer deze optie overgenomen van de DNS-serverinstellingen voor het virtuele netwerk dat de netwerkinterface wordt toegewezen aan gedefinieerd. Op het niveau van het virtuele netwerk, is een aangepaste DNS-server of de Azure DNS-server gedefinieerd. De Azure DNS-server kan omzetten van hostnamen voor resources die zijn toegewezen aan hetzelfde virtuele netwerk. FQDN-naam moet worden gebruikt om op te lossen voor resources in verschillende virtuele netwerken zijn toegewezen.
    - **Aangepast**: U kunt uw eigen DNS-server voor het omzetten van namen aan meerdere virtuele netwerken configureren. Voer het IP-adres van de server die u wilt gebruiken als een DNS-server. Het adres van de DNS-server u geeft alleen voor deze netwerkinterface wordt toegewezen, en overschrijft alle DNS-instellingen voor het virtuele netwerk dat de netwerkinterface wordt toegewezen aan.
5. Selecteer **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic update](/cli/azure/network/nic)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>In- of uitschakelen van doorsturen via IP

Doorsturen via IP kunt de virtuele machine die een netwerkinterface is gekoppeld aan:
- Ontvangen netwerkverkeer dat niet is bestemd voor een van de IP-adressen toegewezen aan een van de IP-configuraties die is toegewezen aan de netwerkinterface.
- Verzenden van netwerkverkeer met de IP-adres van een andere bron dan de versie die is toegewezen aan een van de IP-configuraties van een netwerkinterface.

De instelling moet worden ingeschakeld voor elke netwerkinterface die is gekoppeld aan de virtuele machine die ontvangt verkeer die de virtuele machine nodig heeft om door te sturen. Een virtuele machine kan verkeer doorsturen of er meerdere netwerkinterfaces of één netwerkinterface gekoppeld. Doorsturen via IP is een instelling voor Azure, moet een toepassing kunnen doorsturen van het verkeer, zoals firewall, WAN-optimalisatie en load balancing-toepassingen ook uitvoeren door de virtuele machine. Wanneer een virtuele machine wordt uitgevoerd netwerktoepassingen, wordt de virtuele machine vaak aangeduid als een virtueel netwerkapparaat. U kunt een lijst weergeven met gereed voor het implementeren van virtuele netwerkapparaten in het [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Doorsturen via IP wordt meestal gebruikt met de gebruiker gedefinieerde routes. Zie voor meer informatie over door de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md).

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt in- of uitschakelen van doorsturen via IP voor.
3. Selecteer **IP-configuraties** in de **instellingen** sectie.
4. Selecteer **ingeschakeld** of **uitgeschakelde** (standaardinstelling) om de instelling te wijzigen.
5. Selecteer **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic update](/cli/azure/network/nic)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="change-subnet-assignment"></a>Subnettoewijzing wijzigen

U kunt het subnet, maar niet het virtuele netwerk, dat een netwerkinterface wordt toegewezen om te wijzigen.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface die u wilt wijzigen van de subnettoewijzing van het voor.
3. Selecteer **IP-configuraties** onder **instellingen**. Als een privé IP-adressen voor elk IP-configuraties die worden vermeld **(statisch)** naast, moet u de toewijzingsmethode van IP-adres wijzigt in dynamisch via de stappen die volgen. Alle persoonlijke IP-adressen moeten worden toegewezen met de dynamische toewijzingsmethode wijzigen van de subnettoewijzing voor de netwerkinterface. Als de adressen worden toegewezen met de dynamische methode, gaat u verder met stap 5. Als een IPv4-adressen worden toegewezen met de statische toewijzingsmethode, voert u de volgende stappen uit als u wilt de toewijzingsmethode wijzigt in dynamisch:
    - Selecteer de IP-configuratie die u wilt de IPv4-adres toewijzingsmethode voor het wijzigen van de lijst met IP-configuraties.
    - Selecteer **dynamische** voor het privé IP-adres **toewijzing** methode. U kunt geen een IPv6-adres toewijzen met de statische toewijzingsmethode.
    - Selecteer **Opslaan**.
4. Selecteer het subnet dat u wilt verplaatsen van de netwerkinterface van de **Subnet** vervolgkeuzelijst.
5. Selecteer **Opslaan**. Nieuwe dynamische adressen worden toegewezen uit het adresbereik van subnet voor het nieuwe subnet. Na het toewijzen van de netwerkinterface met een nieuw subnet, kunt u een statische IPv4-adres van het nieuwe subnet-adresbereik toewijzen als u ervoor kiest. Zie voor meer informatie over toevoegen, wijzigen en verwijderen van IP-adressen voor een netwerkinterface meer [beheren IP-adressen](virtual-network-network-interface-addresses.md).

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic ip-config update](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig)|

## <a name="add-to-or-remove-from-application-security-groups"></a>Toevoegen aan of verwijderen van beveiligingsgroepen voor toepassing

U kunt alleen een netwerkinterface toevoegen of verwijderen van een netwerkinterface van een toepassingsbeveiligingsgroep met behulp van de portal als de netwerkinterface is gekoppeld aan een virtuele machine. U kunt PowerShell of Azure CLI om toe te voegen een netwerkinterface om te gebruiken of een netwerkinterface verwijderen uit de beveiligingsgroep van een toepassing, of de netwerkinterface is gekoppeld aan een virtuele machine of niet. Meer informatie over [toepassingsbeveiligingsgroepen](security-overview.md#application-security-groups) en hoe u [Maak een toepassingsbeveiligingsgroep](manage-network-security-group.md#create-an-application-security-group).

1. In de *resources, services en documenten zoeken* vak aan de bovenkant van de portal, te beginnen met het typen van de naam van een virtuele machine waarvoor een netwerkinterface die u wilt toevoegen aan of verwijderen uit een toepassingsbeveiligingsgroep. Wanneer de naam van uw virtuele machine wordt weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer onder **INSTELLINGEN** de optie **Netwerken**.  Selecteer **configureren van de beveiligingsgroepen voor toepassing**, selecteert u de toepassing-beveiligingsgroepen die u wilt toevoegen van de netwerkinterface op of hef de selectie van de toepassing-beveiligingsgroepen die u wilt verwijderen van de netwerkinterface van en selecteer vervolgens **opslaan**. Alleen de netwerkinterfaces die zijn opgenomen in hetzelfde virtuele netwerk kunnen worden toegevoegd aan de dezelfde toepassingsbeveiligingsgroep. De toepassingsbeveiligingsgroep moet zich in dezelfde locatie als de netwerkinterface.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic update](/cli/azure/network/nic)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="associate-or-dissociate-a-network-security-group"></a>Koppelen of ontkoppelen van een netwerkbeveiligingsgroep

1. Voer in het zoekvak boven aan de portal, *netwerkinterfaces* in het zoekvak in. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkinterface in de lijst die u wilt een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep uit.
3. Selecteer **netwerkbeveiligingsgroep** onder **instellingen**.
4. Selecteer **Bewerken**.
5. Selecteer **netwerkbeveiligingsgroep** en selecteer vervolgens de netwerkbeveiligingsgroep die u wilt koppelen aan de netwerkinterface, of selecteer **geen**, ontkoppelen van een netwerkbeveiligingsgroep.
6. Selecteer **Opslaan**.

**Opdrachten**

- Azure CLI: [az network nic update](/cli/azure/network/nic#az-network-nic-update)
- PowerShell: [Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)

## <a name="delete-a-network-interface"></a>Een netwerkinterface verwijderen

Als deze niet gekoppeld aan een virtuele machine, kunt u een netwerkinterface verwijderen. Als een netwerkinterface is gekoppeld aan een virtuele machine, moet u eerst plaats de virtuele machine gestopt (toewijzing opgeheven) en vervolgens de netwerkinterface van de virtuele machine loskoppelen. Als u wilt loskoppelen van een netwerkinterface van een virtuele machine, voer de stappen in [loskoppelen van een netwerkinterface van een virtuele machine](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm). U kunt een netwerkinterface van een virtuele machine niet loskoppelen als dit de enige netwerkinterface die is gekoppeld aan de virtuele machine echter wel. Een virtuele machine moet altijd ten minste één netwerkinterface is gekoppeld. Verwijderen van een virtuele machine losgekoppeld van alle netwerkinterfaces die zijn gekoppeld, maar verwijdert niet de netwerkinterfaces.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *netwerkinterfaces*. Wanneer **netwerkinterfaces** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer **...**  aan de rechterkant van de netwerkinterface die u wilt verwijderen uit de lijst met netwerkinterfaces.
3. Selecteer **Verwijderen**.
4. Selecteer **Ja** om verwijdering van de netwerkinterface te bevestigen.

Wanneer u een netwerkinterface verwijdert, worden alle MAC of het IP-adressen worden toegewezen die zijn uitgebracht.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network nic delete](/cli/azure/network/nic)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface)|

## <a name="resolve-connectivity-issues"></a>Verbindingsproblemen oplossen

Als u niet om te communiceren of naar een virtuele machine, beveiliging van netwerkbeveiligingsgroepen of routes met ingang van een netwerkinterface, kan worden veroorzaakt door het probleem. U hebt de volgende opties voor het probleem op te lossen:

### <a name="view-effective-security-rules"></a>De effectieve beveiligingsregels bekijken

De effectieve beveiligingsregels voor elke netwerkinterface die is gekoppeld aan een virtuele machine zijn een combinatie van de regels die u hebt gemaakt in een netwerkbeveiligingsgroep en [standaard beveiligingsregels](security-overview.md#default-security-rules). Inzicht krijgen in de effectieve beveiligingsregels voor een netwerkinterface kan u helpen bepalen waarom u kan niet communiceren naar of van een virtuele machine. U kunt de effectieve regels voor een netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer in het zoekvak boven aan de portal, de naam van een virtuele machine die u wilt weergeven voor een effectieve beveiligingsregels. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak in. Wanneer **virtuele machines** worden weergegeven in de lijst met zoekresultaten, selecteert u deze en selecteer vervolgens een virtuele machine in de lijst.
2. Selecteer **netwerken** onder **instellingen**.
3. Selecteer de naam van een netwerkinterface.
4. Selecteer **effectieve beveiligingsregels** onder **ondersteuning + probleemoplossing**.
5. Bekijk de lijst van de effectieve beveiligingsregels om te bepalen als de juiste regels bestaan voor uw vereiste binnenkomende en uitgaande communicatie. Meer informatie over wat u ziet in de lijst in [overzicht van netwerkbeveiligingsgroepen](security-overview.md).

De IP-stroom controleren functie van Azure Network Watcher kunt u ook bepalen als beveiligingsregels de communicatie tussen een virtuele machine en een eindpunt worden voorkomen. Zie voor meer informatie, [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Opdrachten**

- Azure CLI: [az network nic list-effectief-nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg)
- PowerShell: [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup) 

### <a name="view-effective-routes"></a>Effectieve routes weergeven

De effectieve routes voor de netwerkinterfaces die zijn gekoppeld aan een virtuele machine zijn een combinatie van standaardroutes, alle routes die u hebt gemaakt en alle routes doorgegeven van on-premises netwerken via BGP via de gateway van een Azure-netwerk. Inzicht krijgen in de effectieve routes voor een netwerkinterface kan u helpen bepalen waarom u kan niet communiceren naar of van een virtuele machine. U kunt de effectieve routes voor elke netwerkinterface die is gekoppeld aan een actieve virtuele machine weergeven.

1. Voer in het zoekvak boven aan de portal, de naam van een virtuele machine die u wilt weergeven voor een effectieve beveiligingsregels. Als u de naam van een virtuele machine niet weet, voert u *virtuele machines* in het zoekvak in. Wanneer **virtuele machines** worden weergegeven in de lijst met zoekresultaten, selecteert u deze en selecteer vervolgens een virtuele machine in de lijst.
2. Selecteer **netwerken** onder **instellingen**.
3. Selecteer de naam van een netwerkinterface.
4. Selecteer **effectieve routes** onder **ondersteuning + probleemoplossing**.
5. Bekijk de lijst met effectieve routes om te bepalen als de juiste routes bestaan voor uw vereiste binnenkomende en uitgaande communicatie. Meer informatie over wat u ziet in de lijst in [routeringoverzicht](virtual-networks-udr-overview.md).

De volgende hop-functie van Azure Network Watcher kunt u bepalen als routes voorkomen de communicatie tussen een virtuele machine en een eindpunt dat. Zie voor meer informatie, [van volgende hop](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Opdrachten**

- Azure CLI: [az network nic show-effectief-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable)

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op netwerkinterfaces, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is de juiste machtigingen toegewezen die worden vermeld in de volgende tabel:

| Bewerking                                                                     | Name                                                      |
| ---------                                                                  | -------------                                             |
| Microsoft.Network/networkInterfaces/read                                   | Ophalen van de netwerkinterface                                     |
| Microsoft.Network/networkInterfaces/write                                  | Maken of bijwerken van de netwerkinterface                        |
| Microsoft.Network/networkInterfaces/join/action                            | Een netwerkinterface koppelen aan een virtuele machine           |
| Microsoft.Network/networkInterfaces/delete                                 | Verwijderen van netwerkinterface                                  |
| Microsoft.Network/networkInterfaces/joinViaPrivateIp/action                | Een resource toevoegen aan een netwerkinterface via een servi...     |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action             | Network interface effectieve routetabel ophalen               |
| Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action  | Network interface effectieve beveiligingsgroepen ophalen           |
| Microsoft.Network/networkInterfaces/loadBalancers/read                     | Network interface load balancers ophalen                      |
| Microsoft.Network/networkInterfaces/serviceAssociations/read               | Koppeling van de service ophalen                                   |
| Microsoft.Network/networkInterfaces/serviceAssociations/write              | Maken of bijwerken van een service-koppeling                    |
| Microsoft.Network/networkInterfaces/serviceAssociations/delete             | Service-koppeling verwijderen                                |
| Microsoft.Network/networkInterfaces/serviceAssociations/validate/action    | Service-koppeling valideren                              |
| Microsoft.Network/networkInterfaces/ipconfigurations/read                  | Interface-IP-configuratie ophalen                    |

## <a name="next-steps"></a>Volgende stappen

- Een virtuele machine maken met meerdere NIC's met behulp van de [Azure CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Maken van een enkel NIC VM met meerdere IPv4-adressen met behulp van de [Azure CLI](virtual-network-multiple-ip-addresses-cli.md) of [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
- Een enkel NIC-VM maken met een persoonlijke IPv6-adres (achter een Azure Load Balancer) met de [Azure CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), of [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Maak een interface die met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjabloon](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken

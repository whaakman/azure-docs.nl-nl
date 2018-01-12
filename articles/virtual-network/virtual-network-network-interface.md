---
title: Maken, wijzigen of verwijderen van een Azure-netwerk-interface | Microsoft Docs
description: Meer informatie over een netwerkinterface is maken, instellingen voor wijzigen en verwijderen.
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
ms.openlocfilehash: 9f1cf113f75bc5a96af8c33d4b83d1bd0f5c6efd
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="create-change-or-delete-a-network-interface"></a>Maken, wijzigen of verwijderen van een netwerkinterface

Informatie over het maken, de instellingen voor wijzigen en verwijderen van een netwerkinterface. Een netwerkinterface kunt een virtuele Machine van Azure om te communiceren met Internet, Azure en lokale bronnen. Wanneer u een virtuele machine met de Azure portal maakt, maakt de portal één netwerkinterface met de standaardinstellingen voor u. U kunt in plaats daarvan netwerkinterfaces te maken met aangepaste instellingen en een of meer netwerkinterfaces aan een virtuele machine toevoegen wanneer u deze maakt. U kunt ook standaard netwerkinterface-instellingen voor een bestaande netwerkinterface wijzigen. In dit artikel wordt uitgelegd hoe een netwerkinterface met aangepaste instellingen te maken, bestaande instellingen, zoals de toewijzing van netwerken filter (netwerkbeveiligingsgroep), subnettoewijzing, DNS-serverinstellingen en doorsturen via IP, wijzigen en verwijderen van een netwerkinterface.

Als u toevoegen wilt, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, lees de [beheren IP-adressen](virtual-network-network-interface-addresses.md) artikel. Als u network interfaces voor het toevoegen of verwijderen van netwerkinterfaces van virtuele machines, lezen, moet de [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md) artikel.


## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u alle stappen in elke sectie van dit artikel uitvoert:

- Controleer de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel voor meer informatie over limieten voor netwerkinterfaces.
- Aanmelden bij de Azure [portal](https://portal.azure.com), Azure-opdrachtregelinterface (CLI) of Azure PowerShell gebruiken met een Azure-account. Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als PowerShell-opdrachten voor het uitvoeren van taken in dit artikel [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure PowerShell-commandlets geïnstalleerd. Typ voor hulp bij PowerShell-opdrachten, met voorbeelden, `get-help <command> -full`.
- Als Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel [installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com).

## <a name="create-a-network-interface"></a>Een netwerkinterface maken

Wanneer u een virtuele machine met de Azure portal maakt, maakt de portal een netwerkinterface met de standaardinstellingen voor u. Als u liever uw instellingen voor de interface netwerk instelt, kunt u een netwerkinterface maken met aangepaste instellingen en de netwerkinterface koppelen aan een virtuele machine bij het maken van de virtuele machine (met PowerShell of Azure CLI). U kunt ook een netwerkinterface maken en toe te voegen aan een bestaande virtuele machine (met PowerShell of Azure CLI). Lees meer informatie over het maken van een virtuele machine met een bestaande netwerkinterface of toevoegen aan of verwijderen van netwerkinterfaces van de bestaande virtuele machines, de [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md) artikel. Voordat u een netwerkinterface maakt, hebt u een bestaande [virtueel netwerk](virtual-networks-create-vnet-arm-pportal.md) in dezelfde locatie en abonnement maakt u een netwerkinterface in.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op **+ toevoegen**.
4. In de **netwerkinterface maken** blade die wordt weergegeven, invoeren, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:

    |Instelling|Vereist?|Details|
    |---|---|---|
    |Naam|Ja|De naam moet uniek zijn binnen de resourcegroep die u selecteert. Na verloop van tijd waarschijnlijk hebt u verschillende netwerkinterfaces in uw Azure-abonnement. Lees de [naamconventies](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) artikel voor suggesties bij het maken van een naamgevingsconventie voor het beheer van meerdere netwerkinterfaces eenvoudiger. De naam kan niet worden gewijzigd nadat de netwerkinterface is gemaakt.|
    |Virtueel netwerk|Ja|Selecteer het virtuele netwerk voor de netwerkinterface. U kunt alleen een netwerkinterface toewijzen aan een virtueel netwerk dat uit hetzelfde abonnement en dezelfde locatie als de netwerkinterface bestaat. Zodra een netwerkinterface is gemaakt, kunt u het virtuele netwerk dat is toegewezen aan niet wijzigen. De virtuele machine toevoegen van de netwerkinterface te moet tevens aanwezig zijn in dezelfde locatie en abonnement als de netwerkinterface.|
    |Subnet|Ja|Selecteer een subnet in het virtuele netwerk dat u hebt geselecteerd. Het subnet dat de netwerkinterface is toegewezen aan nadat deze gemaakt, kunt u wijzigen.|
    |Privé IP-adrestoewijzing|Ja| Deze instelling kiezen van de toewijzingsmethode voor het IPv4-adres. Kies in de volgende toewijzingsmethoden: **dynamische:** wanneer u deze optie selecteert, Azure wijst automatisch het volgende beschikbare adres van de adresruimte van het subnet dat u hebt geselecteerd. **Statische:** wanneer u deze optie selecteert, moet u handmatig een beschikbaar IP-adres uit in de adresruimte van het subnet geselecteerde toewijzen. Statische en dynamische adressen wijzigen niet totdat u ze wijzigen of de netwerkinterface wordt verwijderd. U kunt de toewijzingsmethode wijzigen nadat de netwerkinterface is gemaakt. De Azure-DHCP-server wijst dit adres toe aan de netwerkinterface binnen het besturingssysteem van de virtuele machine.|
    |Netwerkbeveiligingsgroep|Nee| Laat de eigenschap is ingesteld op **geen**, selecteer een bestaande [netwerkbeveiligingsgroep](virtual-networks-nsg.md), of [maken van een netwerkbeveiligingsgroep](virtual-networks-create-nsg-arm-pportal.md). Netwerkbeveiligingsgroepen kunnen u om netwerkverkeer te filteren en een netwerkinterface. U kunt nul of één netwerkbeveiligingsgroep toepassen op een netwerkinterface. Nul of één netwerkbeveiligingsgroep kan ook worden toegepast met het subnet voor dat de netwerkinterface is toegewezen aan. Wanneer een netwerkbeveiligingsgroep wordt toegepast op een netwerkinterface en het subnet dat aan de netwerkinterface is toegewezen, wordt soms onverwachte resultaten optreden. Voor het oplossen van netwerkbeveiligingsgroepen op netwerkinterfaces en subnetten toegepast, lees de [netwerkbeveiligingsgroepen oplossen](virtual-network-nsg-troubleshoot-portal.md#nsg) artikel.|
    |Abonnement|Ja|Selecteer een van uw Azure [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). De virtuele machine die u een netwerkinterface om te en het virtuele netwerk dat u verbinding om te maakt koppelen, moet zich in hetzelfde abonnement.|
    |Privé IP-adres (IPv6)|Nee| Als u dit selectievakje inschakelt, wordt een IPv6-adres toegewezen aan de netwerkinterface, naast het IPv4-adres dat is toegewezen aan de netwerkinterface. Zie de [IPv6](#IPv6) sectie van dit artikel voor belangrijke informatie over het gebruik van IPv6 met netwerkinterfaces. U kunt een methode voor het toewijzen voor de IPv6-adres niet selecteren. Als u een IPv6-adres toewijzen kiest, wordt het toegewezen met de dynamische methode.
    |IPv6-naam (alleen weergegeven wanneer de **particuliere IP-adres (IPv6)** selectievakje is ingeschakeld) |Ja, als de **particuliere IP-adres (IPv6)** selectievakje is ingeschakeld.| Deze naam wordt toegewezen aan een secundaire IP-configuratie voor de netwerkinterface. Meer informatie over IP-configuraties in de [netwerkinterface-instellingen bekijken](#view-network-interface-settings) sectie van dit artikel.|
    |Resourcegroep|Ja|Selecteer een bestaande [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) of maak een. Een netwerkinterface kan bestaan in de groep dezelfde of een andere bron dan de virtuele machine koppelt, of het virtuele netwerk dat u deze verbinding.|
    |Locatie|Ja|De virtuele machine als u een netwerkinterface om te koppelen en het virtuele netwerk u deze met verbindt moet aanwezig zijn in dezelfde [locatie](https://azure.microsoft.com/regions), ook wel een regio.|

De portal biedt niet de optie voor een openbaar IP-adres toewijzen aan de netwerkinterface bij het maken, hoewel de portal een openbaar IP-adres maken en aan een netwerkinterface toewijzen bij het maken van een virtuele machine via de portal. Lees voor meer informatie over het toevoegen van een openbaar IP-adres aan de netwerkinterface nadat deze is gemaakt, de [beheren IP-adressen](virtual-network-network-interface-addresses.md) artikel. Als u maken van een netwerkinterface met een openbaar IP-adres wilt, moet u de CLI of PowerShell gebruiken voor het maken van de netwerkinterface.

>[!Note]
> Azure wijst een MAC-adres toe aan de netwerkinterface nadat de netwerkinterface is gekoppeld aan een virtuele machine en de virtuele machine voor het eerst wordt gestart. U kunt het MAC-adres waarmee Azure worden toegewezen aan de netwerkinterface niet opgeven. Het MAC-adres blijft toegewezen aan de netwerkinterface tot de netwerkinterface is verwijderd of de privé IP-adres is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface is gewijzigd. Lees voor meer informatie over IP-adressen en IP-configuraties, de [beheren IP-adressen](virtual-network-network-interface-addresses.md) artikel.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic maken](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nieuwe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Netwerkinterface-instellingen weergeven

U kunt weergeven en wijzigen van de meeste instellingen voor een netwerkinterface nadat deze gemaakt.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt weergeven of wijzigen van instellingen voor.
4. De volgende instellingen worden vermeld in de blade voor de netwerkinterface die u hebt geselecteerd:
    - **Overzicht:** bevat informatie over de netwerkinterface, zoals het IP-adressen toegewezen aan deze, het virtuele netwerk/subnet aan de netwerkinterface is toegewezen en de virtuele machine de netwerkinterface is gekoppeld aan (als deze is gekoppeld aan een). De volgende afbeelding ziet u de instellingen van het overzicht voor een netwerkinterface met de naam **mywebserver256**: ![Network interface-overzicht](./media/virtual-network-network-interface/nic-overview.png) kunt u een netwerkinterface verplaatsen naar een andere resourcegroep of abonnement door te klikken (**wijzigen**) naast de **resourcegroep** of **naam abonnement**. Als u de netwerkinterface verplaatst, moet u alle bronnen die betrekking hebben op de netwerkinterface met het verplaatsen. Als de netwerkinterface is gekoppeld aan een virtuele machine, bijvoorbeeld, moet u ook verplaatsen de virtuele machine en andere bronnen betrekking hebben op virtuele machine. Lezen voor het verplaatsen van een netwerkinterface, de [resource verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal) artikel. Dit artikel worden de vereisten en het verplaatsen van resources met behulp van de Azure-portal, PowerShell en de Azure CLI.
    - **IP-configuraties:** openbare en persoonlijke IPv4 en IPv6-adressen zijn toegewezen aan IP-configuraties worden hier vermeld. Als een IPv6-adres is toegewezen aan een IP-configuratie, wordt het adres niet weergegeven. Meer informatie over IP-configuraties en het toevoegen en verwijderen van IP-adressen in de [configureren IP-adressen voor een Azure-netwerk-interface](virtual-network-network-interface-addresses.md) artikel. Doorsturen via IP en subnettoewijzing zijn ook in deze sectie geconfigureerd. Lees voor meer informatie over deze instellingen de [in- of uitschakelen van doorsturen via IP](#enable-or-disable-ip-forwarding) en [subnettoewijzing wijzigen](#change-subnet-assignment) secties van dit artikel.
    - **DNS-servers:** kunt u opgeven welke DNS-server een netwerkinterface wordt toegewezen door de Azure-DHCP-servers. De netwerkinterface kunt de instelling overnemen van het virtuele netwerk dat de netwerkinterface is toegewezen aan of een aangepaste instelling die de instelling voor het virtuele netwerk dat toegewezen wordt aan hebben. Om te wijzigen wat wordt weergegeven, voer de stappen in de [wijziging DNS-servers](#change-dns-servers) sectie van dit artikel.
    - **Netwerkbeveiligingsgroep (NSG):** geeft dit NSG is gekoppeld aan de netwerkinterface (indien aanwezig). Een NSG bevat regels voor binnenkomende en uitgaande als u wilt filteren van netwerkverkeer voor de netwerkinterface. Als een NSG gekoppeld aan de netwerkinterface is, wordt de naam van de gekoppelde NSG weergegeven. Om te wijzigen wat wordt weergegeven, voer de stappen in de [beheren netwerk groep beveiligingskoppelingen](virtual-network-manage-nsg-arm-portal.md#manage-associations) artikel.
    - **Eigenschappen:** bevat de belangrijkste instellingen over van de netwerkinterface, met inbegrip van het MAC-adres (leeg als de netwerkinterface is niet gekoppeld aan een virtuele machine) en het abonnement deze voorkomt in.
    - **Effectieve beveiligingsregels:** beveiligingsregels staan als de netwerkinterface is gekoppeld aan een actieve virtuele machine en een NSG gekoppeld aan de netwerkinterface, het subnet dat toegewezen is aan of beide. Lees voor meer informatie over wat wordt weergegeven, de [netwerkbeveiligingsgroepen oplossen](virtual-network-nsg-troubleshoot-portal.md#nsg) artikel. Lees voor meer informatie over Nsg de [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md) artikel.
    - **Effectieve routes:** Routes worden weergegeven als de netwerkinterface is gekoppeld aan een actieve virtuele machine. De routes zijn een combinatie van de Azure standaardroutes, eventuele door de gebruiker gedefinieerde routes (UDR) en een BGP-routes die mogelijk aanwezig zijn voor het subnet dat aan de netwerkinterface is toegewezen. Lees voor meer informatie over wat wordt weergegeven, de [routes oplossen](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface) artikel. Lees voor meer informatie over Azure standaard en udr's, de [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md) artikel.
    - **Algemene instellingen van Azure Resource Manager:** lezen voor meer informatie over algemene instellingen van Azure Resource Manager, de [activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [labels](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Vergrendelt](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), en [automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group) artikelen.

**Opdrachten**

Als een IPv6-adres is toegewezen aan een netwerkinterface, retourneert de PowerShell-uitvoer het feit dat het adres wordt toegewezen, maar het toegewezen adres wordt niet geretourneerd. Op dezelfde manier de CLI retourneert het feit dat het adres is toegewezen, maar retourneert *null* in de uitvoer voor het adres.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[lijst met AZ netwerken nic](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list) netwerkinterfaces weergeven in het abonnement; [az netwerk nic weergeven](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show) voor een netwerkinterface-instellingen te bekijken|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) netwerkinterfaces weergeven in het abonnement of de weergave-instellingen voor een netwerkinterface|

## <a name="change-dns-servers"></a>DNS-servers wijzigen

De DNS-server wordt toegewezen door de Azure-DHCP-server aan de netwerkinterface in het besturingssysteem van de virtuele machine. De DNS-server toegewezen is wat u ook de DNS-serverinstellingen voor een netwerkinterface. Zie voor meer informatie over instellingen voor het omzetten van de naam voor een netwerkinterface, [naamomzetting voor virtuele machines](virtual-networks-name-resolution-for-vms-and-role-instances.md). De netwerkinterface kan de instellingen overnemen van het virtuele netwerk, of een eigen unieke instellingen die de instelling voor het virtuele netwerk te overschrijven.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt weergeven of wijzigen van instellingen voor.
4. In de blade voor de netwerkinterface die u hebt geselecteerd, klikt u op **DNS-servers** onder **instellingen**.
5. Klik op een:
    - **Overnemen van een virtueel netwerk (standaard)**: Selecteer deze optie worden overgenomen van de DNS-serverinstelling gedefinieerd voor de netwerkinterface is toegewezen aan virtueel netwerk. Op het niveau van het virtuele netwerk is een aangepaste DNS-server of de Azure DNS-server gedefinieerd. De Azure DNS-server kan omzetten van hostnamen voor resources die zijn toegewezen aan hetzelfde virtuele netwerk. FQDN-naam moet worden gebruikt om op te lossen voor bronnen die zijn toegewezen aan verschillende virtuele netwerken.
    - **Aangepaste**: U kunt uw eigen DNS-server voor het omzetten van namen over meerdere virtuele netwerken configureren. Geef het IP-adres van de server die u wilt gebruiken als een DNS-server. De DNS-serveradres die u opgeeft alleen aan deze netwerkinterface is toegewezen en wordt een DNS-instelling voor het virtuele netwerk dat de netwerkinterface is toegewezen aan.
6. Klik op **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic-update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>In- of uitschakelen van doorsturen via IP

Doorsturen via IP maakt gebruik van de virtuele machine die een netwerkinterface is gekoppeld aan:
- Ontvangen netwerkverkeer dat niet is bestemd voor een van de IP-adressen toegewezen aan een van de IP-configuraties toegewezen aan de netwerkinterface.
- Verzenden van netwerkverkeer met een ander IP-adres dan de toegewezen aan een van de netwerkinterface-IP-configuraties.

De instelling moet worden ingeschakeld voor elke netwerkinterface die is gekoppeld aan de virtuele machine die verkeer dat de virtuele machine moet voor het doorsturen van ontvangt. Een virtuele machine kan verkeer doorsturen of er meerdere netwerkinterfaces of één netwerkinterface is gekoppeld. Doorsturen via IP is een Azure-instelling, moet ook een toepassing kunnen doorsturen van het verkeer, zoals firewall, WAN-optimalisatie en taakverdeling toepassingen uitvoeren in de virtuele machine. Wanneer een virtuele machine wordt uitgevoerd netwerktoepassingen, is de virtuele machine vaak aangeduid als een virtueel netwerkapparaat. U kunt een lijst weergeven met gereed is voor het implementeren van virtuele netwerkapparaten in het [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Doorsturen via IP wordt doorgaans gebruikt met de gebruiker gedefinieerde routes. Lees het artikel [User-defined routes](virtual-networks-udr-overview.md) (Door de gebruiker gedefinieerde routes) voor meer informatie over door de gebruiker gedefinieerde routes.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt in- of uitschakelen van doorsturen via IP voor.
4. In de blade voor de netwerkinterface die u hebt geselecteerd, klikt u op **IP-configuraties** in de **instellingen** sectie.
5. Klik op **ingeschakeld** of **uitgeschakelde** (standaardinstelling) om de instelling te wijzigen.
6. Klik op **Opslaan**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic-update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Subnettoewijzing wijzigen

U kunt het subnet, maar niet het virtuele netwerk, dat een netwerkinterface is toegewezen aan wijzigen.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **netwerkinterfaces** blade die wordt weergegeven, klikt u op de netwerkinterface die u wilt weergeven of wijzigen van instellingen voor.
4. Klik op **IP-configuraties** onder **instellingen** in de blade voor de netwerkinterface die u hebt geselecteerd. Als een particuliere IP-adressen voor alle IP-configuraties vermeld **(statische)** naast, moet u de methode voor het IP-adres toewijzen wijzigen naar een dynamische via de stappen volgen. Alle particuliere IP-adressen moeten worden toegewezen met de dynamische toewijzingsmethode wijzigen van de subnettoewijzing van het voor de netwerkinterface. Als de adressen zijn toegewezen met de dynamische methode, kunt u verder met stap 5. Als een IPv4-adressen zijn toegewezen met de statische toewijzingsmethode, de volgende stappen uitvoeren voordat de toewijzingsmethode wijzigen naar een dynamische:
    - Klik op de IP-configuratie die u wilt wijzigen van de toewijzingsmethode voor IPv4-adres voor uit de lijst met IP-configuraties.
    - Klik in de blade die wordt weergegeven voor de IP-configuratie op **dynamische** voor de **toewijzing** methode. U kunt een IPv6-adres niet toewijzen met de statische toewijzingsmethode.
    - Klik op **Opslaan**.
5. Selecteer het subnet dat u wilt verbinding maken met de netwerkinterface op uit de **Subnet** vervolgkeuzelijst.
6. Klik op **Opslaan**. Nieuwe dynamische adressen worden toegewezen uit het adresbereik subnet voor het nieuwe subnet. Na het toewijzen van de netwerkinterface met een nieuwe subnet, kunt u een statisch IPv4-adres uit het nieuwe subnet-adresbereik toewijzen als u kiest. Lees voor meer informatie over toevoegen, wijzigen en verwijderen van IP-adressen voor een netwerkinterface meer de [beheren IP-adressen](virtual-network-network-interface-addresses.md) artikel.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk nic IP-configuratie bijwerken](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>Verwijderen van een netwerkinterface

U kunt een netwerkinterface verwijderen zolang deze niet is gekoppeld aan een virtuele machine. Als deze is gekoppeld aan een virtuele machine, moet u eerst plaats de virtuele machine gestopt (toewijzing ongedaan gemaakt) en vervolgens de netwerkinterface van de virtuele machine, loskoppelen voordat u de netwerkinterface kunt verwijderen. Als u wilt loskoppelen van een netwerkinterface van een virtuele machine, voer de stappen in de [loskoppelen van een netwerkinterface van een virtuele machine](virtual-network-network-interface-vm.md#vm-remove-nic) sectie van de [toevoegen of verwijderen van netwerkinterfaces](virtual-network-network-interface-vm.md) artikel. Verwijderen van een virtuele machine wordt losgekoppeld van alle netwerkinterfaces die zijn gekoppeld, maar niet de netwerkinterfaces verwijdert.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *netwerkinterfaces*. Wanneer **netwerkinterfaces** wordt weergegeven in de zoekresultaten, klik erop.
3. Met de rechtermuisknop op de netwerkinterface die u wilt verwijderen en klik op **verwijderen**.
4. Klik op **Ja** verwijderen van de netwerkinterface te bevestigen.

Wanneer u een netwerkinterface verwijdert, worden alle MAC of IP-adressen zijn toegewezen worden vrijgegeven.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[nic-AZ netwerk het verwijderen](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Verwijder AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Volgende stappen
Als u een virtuele machine maken met meerdere netwerk interfaces of IP-adressen, leest u de volgende artikelen:

**Opdrachten**

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere NIC's maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Één NIC VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Maak een enkel NIC-VM met een particulier IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

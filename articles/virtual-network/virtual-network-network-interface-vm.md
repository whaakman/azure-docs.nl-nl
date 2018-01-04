---
title: Netwerkinterfaces toevoegen of verwijderen van virtuele machines in Azure | Microsoft Docs
description: Informatie over het netwerkinterfaces voor het toevoegen of verwijderen van netwerkinterfaces van virtuele machines.
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
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: abe6abb942d206330e809f3aef388b846d7d7c7f
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Netwerkinterfaces toevoegen of verwijderen van netwerkinterfaces van virtuele machines

Informatie over het toevoegen van een bestaande netwerkinterface bij het maken van een virtuele machine of toevoegen of verwijderen van netwerkinterfaces van een bestaande virtuele machine gestopt (toewijzing ongedaan gemaakt). Een netwerkinterface kan een Azure-virtuele Machine (VM) om te communiceren met Internet, Azure en lokale bronnen. Een virtuele machine kan een of meer netwerkinterfaces hebben. 

Als u toevoegen wilt, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, lees de [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md) artikel. Als u nodig hebt voor het maken, wijzigen of verwijderen van netwerkinterfaces, leest u de [beheren netwerkinterfaces](virtual-network-network-interface.md) artikel.

## <a name="before"></a>Voordat u begint

De volgende taken uitvoeren voordat u alle stappen in elke sectie van dit artikel uitvoert:

- Aanmelden bij de Azure [portal](https://portal.azure.com), Azure-opdrachtregelinterface (CLI) of Azure PowerShell gebruiken met een Azure-account. Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als PowerShell-opdrachten voor het uitvoeren van taken in dit artikel [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure PowerShell-commandlets geïnstalleerd. Typ voor hulp bij PowerShell-opdrachten, met voorbeelden, `get-help <command> -full`. In plaats van Azure PowerShell installeren, kunt u de Azure-Cloud-Shell. De Azure-Cloud-Shell is een gratis PowerShell die u rechtstreeks vanuit de Azure-portal kunt uitvoeren. Azure PowerShell vooraf is geïnstalleerd en geconfigureerd voor gebruik met uw account heeft. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com) en PowerShell selecteren in de linkerbovenhoek, wanneer de shell-venster wordt weergegeven.
- Als Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel [installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com) en Bash selecteren in de linkerbovenhoek, wanneer de shell-venster wordt weergegeven.

## <a name="vm-create"></a>Bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine

Wanneer u een virtuele machine via de portal maakt, wordt de portal een netwerkinterface maken met de standaardinstellingen en gekoppeld aan de virtuele machine voor u. U kan niet bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine of een virtuele machine met meerdere netwerkinterfaces maken met de Azure portal. U kunt doen beide met CLI of PowerShell. Voordat u echter een virtuele machine maken met een bestaande netwerkinterface met PowerShell of de CLI, raken met de [beperkingen](#constraints). Als u een virtuele machine met meerdere netwerkinterfaces maakt, moet u ook het besturingssysteem niet normaal kunnen gebruiken wanneer de virtuele machine is gemaakt. Zie voor meer informatie configureren [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) voor meerdere netwerkinterfaces.

**Opdrachten** voordat u de virtuele machine maakt, maakt u een netwerkinterface met de stappen in [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface).

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm maken](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Een netwerkinterface toevoegen aan een bestaande virtuele machine

1. Aanmelden bij de Azure-portal.
2. Zoek in het zoekvak boven aan de portal voor de naam van de virtuele machine die u wilt toevoegen, de netwerkinterface op of blader naar de virtuele machine door te klikken op **alle services**, klikt u vervolgens **virtuele machines**. Als u de virtuele machine hebt gevonden, klikt u erop. De virtuele machine die u wilt toevoegen van een netwerkinterface om te moet ondersteuning voor het aantal netwerkinterfaces die u wilt toevoegen. Voor meer informatie over het aantal netwerkinterfaces van elke VM grootte ondersteunt, lees de [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-groottes artikelen.  
3. Klik op **overzicht**onder **instellingen**. Klik op **stoppen**, en wacht totdat de **Status** van de virtuele machine wordt gewijzigd in *gestopt (toewijzing opgeheven)*. 
4. Klik op **Networking**onder **instellingen**.
5. Klik op **Attach-netwerkinterface**. Klik in de lijst van bestaande netwerkinterfaces die momenteel zijn niet gekoppeld aan een andere virtuele machine op de netwerkinterface die u wilt koppelen. De netwerkinterface die u selecteert kan niet versnelde netwerken is ingeschakeld, kan niet een IPv6-adres toegewezen hebben en moet aanwezig zijn in hetzelfde virtuele netwerk omdat het virtuele netwerk de netwerkinterface is momenteel gekoppeld aan de virtuele machine in. Als u een bestaande netwerkinterface niet hebt, moet u eerst een maken. Klik op om een netwerkinterface **netwerkinterface maken**. Zie voor meer informatie over het maken van een netwerkinterface, [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface). Zie voor meer informatie over aanvullende beperkingen bij het toevoegen van netwerkinterfaces aan virtuele machines, [beperkingen](#constraints).
6. Klik op **OK**.
7. Klik op **overzicht**onder **instellingen**. Klik op **Start** de virtuele machine te starten.
8. Het VM-besturingssysteem voor het gebruik van meerdere netwerkinterfaces goed configureren. Zie voor meer informatie configureren [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) voor meerdere netwerkinterfaces.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm nic toevoegen](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Voeg AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Netwerkinterfaces van de weergave voor een virtuele machine

U kunt de netwerkinterfaces momenteel gekoppeld aan een virtuele machine voor meer informatie over de configuratie voor elke netwerkinterface en de IP-adressen toegewezen aan elke netwerkinterface weergeven. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account met de rol van eigenaar, bijdrager of Network Contributor voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *virtuele machines*. Wanneer **virtuele machines** wordt weergegeven in de zoekresultaten, klik erop.
3. Klik op de naam van de virtuele machine die u wilt weergeven van netwerkinterfaces voor.
4. In de **instellingen** sectie voor de virtuele machine die u hebt geselecteerd, klikt u op **Networking**. Zie voor meer informatie over de netwerkinterface-instellingen en hoe u deze wijzigt, [netwerkinterfaces beheren](virtual-network-network-interface.md). Zie voor meer over het toevoegen, wijzigen of verwijderen van IP-adressen toegewezen aan een netwerkinterface [beheren IP-adressen](virtual-network-network-interface-addresses.md).

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm weergeven](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Een netwerkinterface van een virtuele machine verwijderen

1. Aanmelden bij de Azure-portal.
2. Zoek in het zoekvak boven aan de portal op de naam van de virtuele machine die u wilt verwijderen (loskoppelen) de netwerkinterface van, of blader naar de virtuele machine door te klikken op **alle services**, klikt u vervolgens **virtuele machines**. Als u de virtuele machine hebt gevonden, klikt u erop.
3. Klik op **overzicht**onder **instellingen**. Klik op **stoppen**, en wacht totdat de **Status** van de virtuele machine wordt gewijzigd in *gestopt (toewijzing opgeheven)*. 
4. Klik op **Networking**onder **instellingen**.
5. Klik op **netwerkinterface ontkoppelen**. Klik in de lijst met netwerkinterfaces die momenteel is gekoppeld aan de virtuele machine op de netwerkinterface die u wilt loskoppelen. Als er slechts één netwerkinterface wordt weergegeven, kan niet los te koppelen, omdat een virtuele machine altijd ten minste één netwerkinterface is gekoppeld moet.
6. Klik op **OK**.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[verwijderen van AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Verwijder AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Volgende stappen
Als een virtuele machine maken met meerdere netwerkinterfaces of IP-adressen, leest u de volgende artikelen:

**Opdrachten**

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere NIC's maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Één NIC VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Maak een enkel NIC-VM met een particulier IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="constraints"></a>Beperkingen

- Een virtuele machine moet ten minste één netwerkinterface is gekoppeld.
- Een virtuele machine kan alleen als er veel netwerkinterfaces gekoppeld als de ondersteunt VM-grootte hebben. Zie voor meer informatie over het aantal netwerkinterfaces elke VM-grootte worden ondersteund, [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-grootten. Alle grootten ondersteuning voor ten minste twee netwerkinterfaces.
- De netwerkinterfaces die u aan een virtuele machine toevoegt kunnen niet op dit moment worden gekoppeld aan een andere virtuele machine. Zie voor meer informatie over het maken van de netwerkinterfaces, [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface).
- In het verleden kunnen alleen netwerkinterfaces worden toegevoegd aan virtuele machines die meerdere netwerkinterfaces ondersteund en zijn gemaakt met ten minste twee netwerkinterfaces. U kan een netwerkinterface niet toevoegen aan een virtuele machine die is gemaakt met één netwerkinterface, zelfs als de VM-grootte meerdere netwerkinterfaces ondersteund. U kan echter alleen netwerkinterfaces verwijderen van een virtuele machine met ten minste drie netwerkinterfaces, omdat virtuele machines die zijn gemaakt met ten minste twee network interfaces moest altijd ten minste twee netwerkinterfaces hebben. Geen van deze beperkingen meer van toepassing. U kunt nu een virtuele machine maken met een willekeurig aantal netwerkinterfaces (maximaal het aantal dat wordt ondersteund door de VM-grootte).
- Standaard wordt de eerste netwerkinterface is gekoppeld aan een VM gedefinieerd als de *primaire* netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn *secundaire* netwerkinterfaces.
- Hoewel u kunt bepalen welke netwerkinterface u uitgaand verkeer naar, standaard verzonden, wordt al het uitgaande verkeer van de virtuele machine het IP-adres dat is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface wordt verzonden.
- In het verleden waren alle virtuele machines in dezelfde beschikbaarheidsset beschikken over een enkele of meerdere netwerkinterfaces. Virtuele machines met een willekeurig aantal netwerkinterfaces kunnen nu bestaan in dezelfde beschikbaarheidsset, tot aan het getal dat wordt ondersteund door de VM-grootte. U kunt alleen een virtuele machine toevoegen aan een beschikbaarheidsset als deze al gemaakt. Lees voor meer informatie over beschikbaarheidssets de [de beschikbaarheid van virtuele machines in Azure beheren](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) artikel.
- Terwijl netwerkinterfaces in dezelfde virtuele machine kunnen worden verbonden met verschillende subnetten binnen een VNet, moeten de netwerkinterfaces allemaal verbonden met hetzelfde VNet.
- U kunt elk IP-adres voor alle IP-configuratie van een primaire of secundaire netwerkinterface toevoegen aan een Load Balancer van Azure back-end-adresgroep. In het verleden kan alleen het primaire IP-adres voor de primaire netwerkinterface worden toegevoegd aan een back-end-pool. Lees voor meer informatie over configuraties van IP-adressen en de [toevoegen, wijzigen of verwijderen IP-adressen](virtual-network-network-interface-addresses.md) artikel.
- Verwijderen van een virtuele machine, wordt de netwerkinterfaces die zijn gekoppeld aan deze niet verwijderd. Wanneer een virtuele machine wordt verwijderd, worden de netwerkinterfaces losgekoppeld van de virtuele machine. U kunt de netwerkinterfaces toevoegen aan andere virtuele machines of verwijder ze.
- Als een netwerkinterface een persoonlijke IPv6-adres is toegewezen heeft, moet u toevoegen (koppeling) naar een virtuele machine bij het maken van de virtuele machine. U kunt een netwerkinterface met een toegewezen IPv6-adres niet toevoegen aan een virtuele machine nadat de virtuele machine is gemaakt. Als u een netwerkinterface met een toegewezen persoonlijke IPv6-adres toevoegen bij het maken van een virtuele machine, kunt u alleen netwerkinterface toevoegen aan de virtuele machine, ongeacht hoeveel netwerkinterfaces die ondersteuning biedt voor de VM-grootte. Zie [Network interface-IP-adressen](virtual-network-network-interface-addresses.md) voor meer informatie over het toewijzen van IP-adressen aan netwerkinterfaces.
- Net als bij IPv6, u niet een netwerkinterface koppelen met versnelde netwerken die zijn ingeschakeld voor een virtuele machine nadat de virtuele machine is gemaakt. Verder, om te profiteren van versnelde netwerken, moet u ook stappen uitvoeren in het VM-besturingssysteem. Zie voor meer informatie over versnelde netwerken en andere beperkingen bij het gebruik, Accelerated netwerken voor [Windows](create-vm-accelerated-networking-powershell.md) of [Linux](create-vm-accelerated-networking-cli.md) virtuele machines.

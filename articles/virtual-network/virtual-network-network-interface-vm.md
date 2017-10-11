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
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: 57f95b765b1b116814683a6643db16091c3041f6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Netwerkinterfaces toevoegen of verwijderen van virtuele machines

Informatie over het toevoegen van een bestaande netwerkinterface bij het maken van een virtuele machine of toevoegen of verwijderen van netwerkinterfaces van een bestaande virtuele machine gestopt (toewijzing ongedaan gemaakt). Een netwerkinterface kan een Azure-virtuele Machine (VM) om te communiceren met Internet, Azure en lokale bronnen. Een virtuele machine kan een of meer netwerkinterfaces hebben. 

Als u toevoegen wilt, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, lees de [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md) artikel. Als u nodig hebt voor het maken, wijzigen of verwijderen van netwerkinterfaces, leest u de [beheren netwerkinterfaces](virtual-network-network-interface.md) artikel.

## <a name="before"></a>Voordat u begint

De volgende taken uitvoeren voordat u alle stappen in elke sectie van dit artikel uitvoert:

- Meer informatie over het aantal netwerkinterfaces elke Linux- en Windows-VM-grootte ondersteunt aan de hand van de [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-groottes artikelen.
- Aanmelden bij de Azure [portal](https://portal.azure.com), Azure-opdrachtregelinterface (CLI) of Azure PowerShell gebruiken met een Azure-account. Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als PowerShell-opdrachten voor het uitvoeren van taken in dit artikel [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure PowerShell-commandlets geïnstalleerd. Typ voor hulp bij PowerShell-opdrachten, met voorbeelden, `get-help <command> -full`.
- Als Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel [installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com).

## <a name="about"></a>Over netwerkinterfaces en virtuele machines

U kunt toevoegen (koppeling) een bestaande netwerkinterface voor een virtuele machine bij het maken van de virtuele machine opgegeven van de netwerkinterface op dit moment niet is verbonden met een andere virtuele machine. U kunt een netwerkinterface om te toevoegen of verwijderen (loskoppelen) een netwerkinterface om te van een bestaande virtuele machine opgegeven van de virtuele machine is gestopt (toewijzing ongedaan gemaakt). Als u een virtuele machine met de Azure-portal maken, de portal een netwerkinterface met de standaardinstellingen voor u gemaakt. De portal niet toe dat u naar:

- Geef een bestaande netwerkinterface bij het maken van de virtuele machine toevoegen
- Een virtuele machine met meerdere netwerkinterfaces maken
- Geef een naam voor de netwerkinterface (de portal maakt de netwerkinterface met de naam van een standaard)

U kunt Azure PowerShell of de CLI gebruiken voor het maken van een netwerkinterface of virtuele machine met de vorige kenmerken die u kunt de portal voor niet gebruiken. Houd rekening met de volgende beperkingen en het gedrag voor het voltooien van de taken in de volgende secties:

- Alle VM-groottes ondersteuning ten minste twee netwerkinterfaces, maar sommige VM-grootten ondersteuning van meer dan twee netwerkinterfaces. In het verleden ondersteund enkele VM-grootten alleen voor een netwerkinterface. Voor meer informatie over het aantal netwerkinterfaces van elke VM grootte ondersteunt, lees de [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-groottes artikelen. 
- In het verleden kunnen alleen netwerkinterfaces worden toegevoegd aan virtuele machines die meerdere netwerkinterfaces ondersteund en zijn gemaakt met ten minste twee netwerkinterfaces. U kan een netwerkinterface niet toevoegen aan een virtuele machine die is gemaakt met één netwerkinterface, zelfs als de VM-grootte meerdere netwerkinterfaces ondersteund. U kan echter alleen netwerkinterfaces verwijderen van een virtuele machine met ten minste drie netwerkinterfaces, omdat virtuele machines die zijn gemaakt met ten minste twee network interfaces moest altijd ten minste twee netwerkinterfaces hebben. Geen van deze beperkingen meer van toepassing. U kunt nu een virtuele machine met een willekeurig aantal netwerkinterfaces (maximaal het aantal dat wordt ondersteund door de VM-grootte) maken en toevoegen of verwijderen van een willekeurig aantal netwerkinterfaces (van virtuele machines gestopt (toewijzing ongedaan gemaakt)), zolang de virtuele machine altijd ten minste één netwerkinterface heeft.
- Standaard de eerste netwerkinterface in een virtuele machine is gedefinieerd als de *primaire* netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn *secundaire* netwerkinterfaces.
- Primaire netwerkinterfaces standaardgateway door de Azure-DHCP-servers zijn toegewezen, maar secundaire netwerkinterfaces zijn niet. Omdat secundaire netwerkinterfaces zijn niet standaardgateway toegewezen, niet kunnen ze communiceren met bronnen buiten hun subnet standaard. Om secundaire netwerkinterfaces in een Windows-VM om te communiceren met bronnen buiten hun subnet, routes toevoegen met het besturingssysteem via het `route add` opdracht vanaf een Windows-opdrachtregel. Voor virtuele Linux-machines, omdat het standaardgedrag maakt gebruik van zwakke host routering, wordt geadviseerd om verkeer voor secundaire netwerkinterfaces beperkt tot één subnet. Als u connectiviteit buiten het subnet voor de secundaire netwerkinterfaces vereist, inschakelen om ervoor te zorgen dat dezelfde netwerkinterface maakt gebruik van inkomende en uitgaande verkeer op beleid gebaseerde routering.
- Standaard wordt al het uitgaande verkeer van de virtuele machine het IP-adres dat is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface verzonden. U bepalen welk IP-adres wordt gebruikt voor uitgaand verkeer in het besturingssysteem van de VM, maar deze is standaard via de primaire netwerkinterface.
- In het verleden waren alle virtuele machines in dezelfde beschikbaarheidsset beschikken over een enkele of meerdere netwerkinterfaces. Virtuele machines met een willekeurig aantal netwerkinterfaces kunnen nu bestaan in dezelfde beschikbaarheidsset, tot aan het getal dat wordt ondersteund door de VM-grootte. U kunt alleen een virtuele machine toevoegen aan een beschikbaarheidsset als deze al gemaakt. Lees voor meer informatie over beschikbaarheidssets de [de beschikbaarheid van virtuele machines in Azure beheren](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) artikel.
- Terwijl netwerkinterfaces in dezelfde virtuele machine kunnen worden verbonden met verschillende subnetten binnen een VNet, moeten de netwerkinterfaces allemaal verbonden met hetzelfde VNet.
- U kunt elk IP-adres voor alle IP-configuratie van een primaire of secundaire netwerkinterface toevoegen aan een Load Balancer van Azure back-end-adresgroep. In het verleden kan alleen het primaire IP-adres voor de primaire netwerkinterface worden toegevoegd aan een back-end-pool. Lees voor meer informatie over configuraties van IP-adressen en de [toevoegen, wijzigen of verwijderen IP-adressen](virtual-network-network-interface-addresses.md) artikel.
- Verwijderen van een virtuele machine, wordt de netwerkinterfaces die zijn gekoppeld aan deze niet verwijderd. Wanneer een virtuele machine wordt verwijderd, worden de netwerkinterfaces losgekoppeld van de virtuele machine. U kunt de netwerkinterfaces toevoegen aan andere virtuele machines of verwijder ze.
- Als een netwerkinterface een persoonlijke IPv6-adres is toegewezen heeft, kunt u het koppelen aan een virtuele machine bij het maken van de virtuele machine. U kunt een netwerkinterface met een toegewezen IPv6-adres niet koppelen aan een virtuele machine nadat de virtuele machine is gemaakt. Als u een netwerkinterface met een toegewezen persoonlijke IPv6-adres koppelt bij het maken van een virtuele machine, kunt u alleen netwerkinterface koppelen aan de virtuele machine, ongeacht hoeveel netwerkinterfaces die ondersteuning biedt voor de VM-grootte. Zie [Network interface-IP-adressen](virtual-network-network-interface-addresses.md) voor meer informatie over het toewijzen van IP-adressen aan netwerkinterfaces.

## <a name="vm-create"></a>Bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine

Wanneer u een virtuele machine via de portal maakt, wordt de portal een netwerkinterface maken met de standaardinstellingen en gekoppeld aan de virtuele machine voor u. U kan niet bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine of een virtuele machine maken met meerdere netwerkinterfaces met de Azure portal. U kunt doen beide met CLI of PowerShell. U kunt zoveel netwerkinterfaces voor een virtuele machine ondersteunt de VM-grootte die u maakt, kunt toevoegen. Lees voor meer informatie over het aantal netwerkinterfaces elke VM-grootte ondersteunt, de [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-groottes artikelen. De netwerkinterfaces die u aan een virtuele machine toevoegt kunnen niet op dit moment worden gekoppeld aan een andere virtuele machine. Lees meer informatie over het maken van de netwerkinterfaces, de [beheren netwerkinterfaces](virtual-network-network-interface.md#create-a-network-interface) artikel.

> [!WARNING]
> Als een netwerkinterface een persoonlijke IPv6-adres is toegewezen heeft, kunt u alleen de netwerkinterface op de virtuele machine toevoegen bij het maken van de virtuele machine. U kunt meer dan één netwerkinterface niet koppelen aan de virtuele machine als u de virtuele machine maakt, of nadat de virtuele machine is gemaakt, zo lang als een IPv6-adres wordt toegewezen aan een netwerkinterface gekoppeld aan een virtuele machine. Zie [Network interface-IP-adressen](virtual-network-network-interface-addresses.md) voor meer informatie over het toewijzen van IP-adressen aan netwerkinterfaces.

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm maken](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Een bestaande netwerkinterface toevoegen aan een bestaande virtuele machine

U kunt zoveel netwerkinterfaces voor een virtuele machine omdat de VM-grootte die u bij het toevoegen van netwerkinterfaces ondersteunt toevoegen. Voor meer informatie over het aantal netwerkinterfaces van elke VM grootte ondersteunt, lees de [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-groottes artikelen. De virtuele machine die u wilt een netwerkinterface om te toevoegen, moet het aantal netwerkinterfaces die u wilt toevoegen en moet worden gestopt (toewijzing ongedaan gemaakt) ondersteunen. De netwerkinterfaces die u wilt toevoegen kunnen niet op dit moment worden gekoppeld aan een andere virtuele machine. U kunt netwerkinterfaces niet toevoegen aan een bestaande virtuele machine met de Azure portal. Als u wilt toevoegen netwerkinterfaces naar een bestaande virtuele machine, moet u de CLI of PowerShell. 

> [!WARNING]
> Als een netwerkinterface een persoonlijke IPv6-adres is toegewezen heeft, kan deze kan niet worden toegevoegd aan een bestaande virtuele machine. U kunt alleen een netwerkinterface met een toegewezen persoonlijke IPv6-adres toevoegen aan een virtuele machine bij het maken van een virtuele machine. Zie [Network interface-IP-adressen](virtual-network-network-interface-addresses.md) voor meer informatie over het toewijzen van IP-adressen aan netwerkinterfaces.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm nic toevoegen](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Voeg AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Netwerkinterfaces van de weergave voor een virtuele machine

U kunt de netwerkinterfaces momenteel gekoppeld aan een virtuele machine voor meer informatie over de configuratie voor elke netwerkinterface en de IP-adressen toegewezen aan elke netwerkinterface weergeven. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account met de rol van eigenaar, bijdrager of Network Contributor voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *virtuele machines*. Wanneer **virtuele machines** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **virtuele machines** blade die wordt weergegeven, klikt u op de naam van de virtuele machine die u wilt weergeven van netwerkinterfaces voor.
4. In de **instellingen** sectie van de virtuele machineblade weergegeven voor de virtuele machine die u hebt geselecteerd, klikt u op **netwerkinterfaces**. Lees meer over netwerkinterface-instellingen en hoe u deze wijzigt de [beheren netwerkinterfaces](virtual-network-network-interface.md) artikel. Zie voor meer over het toevoegen, wijzigen of verwijderen van IP-adressen toegewezen aan een netwerkinterface [beheren IP-adressen](virtual-network-network-interface-addresses.md).

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm weergeven](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Een netwerkinterface van een virtuele machine verwijderen

De virtuele machine die u wilt verwijderen (of loskoppelen) op een netwerkinterface van moet zijn gestopt (toewijzing ongedaan gemaakt) en moeten momenteel hebben ten minste twee netwerkinterfaces gekoppeld. U kunt een netwerkinterface verwijderen, maar de virtuele machine moet altijd ten minste één netwerkinterface is gekoppeld. Als u een primaire netwerkinterface verwijdert, wordt het primaire kenmerk met Azure toegewezen aan de netwerkinterface die wordt toegevoegd aan de virtuele machine het langste. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account met de rol van eigenaar, bijdrager of Network Contributor voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *virtuele machines*. Wanneer **virtuele machines** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **virtuele machines** blade die wordt weergegeven, klikt u op de naam van de virtuele machine die u wilt een netwerkinterface voor verwijderen.
4. In de **instellingen** sectie van de virtuele machineblade weergegeven voor de virtuele machine die u hebt geselecteerd, klikt u op **netwerkinterfaces**. Lees meer over netwerkinterface-instellingen en hoe u deze wijzigt de [beheren netwerkinterfaces](virtual-network-network-interface.md) artikel. Zie voor meer over het toevoegen, wijzigen of verwijderen van IP-adressen toegewezen aan een netwerkinterface [beheren IP-adressen](virtual-network-network-interface-addresses.md).
5. In de blade van de netwerk-interfaces die wordt weergegeven, klikt u op de **...**  rechts van de netwerkinterface die u wilt loskoppelen.
6. Klik op **loskoppelen**. Als er slechts één netwerkinterface is gekoppeld aan de virtuele machine de **Detach** optie niet beschikbaar. Klik op **Ja** in het bevestigingsvenster die wordt weergegeven.

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

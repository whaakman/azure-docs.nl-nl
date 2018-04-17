---
title: Netwerkinterfaces toevoegen of verwijderen van virtuele machines in Azure | Microsoft Docs
description: Informatie over het netwerkinterfaces voor het toevoegen of verwijderen van netwerkinterfaces van virtuele machines.
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
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: abccfed6f335ed7febb4fb2f17ad71fa153e3ed6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Netwerkinterfaces toevoegen of verwijderen van netwerkinterfaces van virtuele machines

Informatie over het toevoegen van een bestaande netwerkinterface bij het maken van Azure een virtuele machine (VM), of toevoegen of verwijderen van netwerkinterfaces van een bestaande virtuele machine gestopt (toewijzing ongedaan gemaakt). Een netwerkinterface kunt een virtuele machine van Azure om te communiceren met internet, Azure en lokale bronnen. Een virtuele machine kan een of meer netwerkinterfaces hebben. 

Als u moet toe te voegen, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, Zie [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md). Als u maken wilt, wijzigen, of verwijderen van netwerkinterfaces, Zie [netwerkinterfaces beheren](virtual-network-network-interface.md).

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Deze zelfstudie vereist de Azure PowerShell-moduleversie 5.2.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.26 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

## <a name="add-existing-network-interfaces-to-a-new-vm"></a>Bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine

Wanneer u een virtuele machine via de portal maakt, wordt de portal een netwerkinterface maken met de standaardinstellingen en gekoppeld aan de virtuele machine voor u. U kan niet bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine of een virtuele machine met meerdere netwerkinterfaces te maken met behulp van de Azure-portal. U kunt beide doen door met de CLI of PowerShell, maar zorg ervoor dat om vertrouwd te raken met de [beperkingen](#constraints). Als u een virtuele machine met meerdere netwerkinterfaces maakt, moet u ook het besturingssysteem om deze correct gebruiken nadat u de virtuele machine maken. Meer informatie over het configureren van [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) voor meerdere netwerkinterfaces.

### <a name="commands"></a>Opdrachten

Voordat u de virtuele machine maakt, maakt u een netwerkinterface met behulp van de stappen in [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface).

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Een netwerkinterface toevoegen aan een bestaande virtuele machine

1. Meld u aan bij Azure Portal.
2. Typ de naam van de virtuele machine die u wilt toevoegen aan de netwerkinterface, of blader naar de virtuele machine door te selecteren in het zoekvak boven aan de portal **alle services**, en vervolgens **virtuele machines**. Nadat u de virtuele machine hebt gevonden, selecteert u deze. De virtuele machine moet ondersteuning voor het aantal netwerkinterfaces die u wilt toevoegen. Om erachter te komen hoeveel elke VM-grootte voor netwerkinterfaces ondersteunt, Zie [grootten voor virtuele Linux-machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [grootten voor Windows virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  
3. Selecteer **overzicht**onder **instellingen**. Selecteer **stoppen**, en wacht totdat de **Status** van de virtuele machine wordt gewijzigd in **gestopt (toewijzing opgeheven)**. 
4. Selecteer **Networking**onder **instellingen**.
5. Selecteer **Attach-netwerkinterface**. Selecteer in de lijst met netwerkinterfaces die momenteel zijn niet gekoppeld aan een andere virtuele machine die u wilt koppelen. 

    >[!NOTE]
    De netwerkinterface die u selecteert kan niet versnelde netwerken is ingeschakeld, kan niet een IPv6-adres toegewezen hebben en moet aanwezig zijn in hetzelfde virtuele netwerk als de database met de netwerkinterface die momenteel is gekoppeld aan de virtuele machine. 

    Als u een bestaande netwerkinterface niet hebt, moet u eerst een maken. Om dit te doen, selecteert u **netwerkinterface maken**. Zie voor meer informatie over het maken van een netwerkinterface, [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface). Zie voor meer informatie over aanvullende beperkingen bij het toevoegen van netwerkinterfaces aan virtuele machines, [beperkingen](#constraints).

6. Selecteer **OK**.
7. Selecteer **overzicht**onder **instellingen**, en vervolgens **Start** de virtuele machine te starten.
8. Het VM-besturingssysteem voor het gebruik van meerdere netwerkinterfaces goed configureren. Meer informatie over het configureren van [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) voor meerdere netwerkinterfaces.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm nic toevoegen](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_add) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Voeg AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="view-network-interfaces-for-a-vm"></a>Netwerkinterfaces van de weergave voor een virtuele machine

U kunt de netwerkinterfaces momenteel gekoppeld aan een virtuele machine voor meer informatie over de configuratie voor elke netwerkinterface en de IP-adressen toegewezen aan elke netwerkinterface weergeven. 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met de rol van eigenaar, bijdrager of Network Contributor voor uw abonnement is toegewezen. Zie voor meer informatie over het toewijzen van rollen aan accounts, [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. In het vak dat de tekst bevat **zoeken bronnen** aan de bovenkant van de Azure-portal, typ **virtuele machines**. Wanneer **virtuele machines** wordt weergegeven in de zoekresultaten, selecteer deze.
3. Selecteer de naam van de virtuele machine waarvoor u wilt weergeven van netwerkinterfaces.
4. In de **instellingen** sectie voor de VM die u hebt geselecteerd, selecteer **Networking**. Zie voor meer informatie over de netwerkinterface-instellingen en hoe u deze wijzigt, [netwerkinterfaces beheren](virtual-network-network-interface.md). Zie voor meer informatie over het toevoegen, wijzigen of verwijderen van IP-adressen toegewezen aan een netwerkinterface, [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md).

### <a name="commands"></a>Opdrachten

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-a-network-interface-from-a-vm"></a>Een netwerkinterface van een virtuele machine verwijderen

1. Meld u aan bij Azure Portal.
2. Zoek in het zoekvak boven aan de portal op de naam van de virtuele machine die u wilt verwijderen (loskoppelen) de netwerkinterface van, of blader naar de virtuele machine door te selecteren **alle services**, en vervolgens **virtuele machines**. Nadat u de virtuele machine hebt gevonden, selecteert u deze.
3. Selecteer **overzicht**onder **instellingen**, en vervolgens **stoppen**. Wacht totdat de **Status** van de virtuele machine wordt gewijzigd in **gestopt (toewijzing opgeheven)**. 
4. Selecteer **Networking**onder **instellingen**.
5. Selecteer **netwerkinterface ontkoppelen**. Selecteer in de lijst met netwerkinterfaces die momenteel is gekoppeld aan de virtuele machine, de netwerkinterface die u wilt loskoppelen. 

    >[!NOTE]
    Als er slechts één netwerkinterface wordt weergegeven, kan niet los te koppelen, omdat een virtuele machine altijd ten minste één netwerkinterface is gekoppeld moet.
6. Selecteer **OK**.

### <a name="commands"></a>Opdrachten

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[verwijderen van AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_remove) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Verwijder AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="constraints"></a>Beperkingen

- Een virtuele machine moet ten minste één netwerkinterface is gekoppeld.
- Een virtuele machine kan alleen als er veel netwerkinterfaces gekoppeld als de ondersteunt VM-grootte hebben. Zie voor meer informatie over het aantal netwerkinterfaces elke VM-grootte worden ondersteund, [grootten voor virtuele Linux-machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [grootten voor Windows virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Alle grootten ondersteuning voor ten minste twee netwerkinterfaces.
- De netwerkinterfaces die u aan een virtuele machine toevoegt kunnen niet op dit moment worden gekoppeld aan een andere virtuele machine. Zie voor meer informatie over het maken van de netwerkinterfaces, [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface).
- In het verleden kunnen alleen netwerkinterfaces worden toegevoegd aan virtuele machines die meerdere netwerkinterfaces ondersteund en zijn gemaakt met ten minste twee netwerkinterfaces. U kan een netwerkinterface niet toevoegen aan een virtuele machine die is gemaakt met één netwerkinterface, zelfs als de VM-grootte meerdere netwerkinterfaces ondersteund. U kan echter alleen netwerkinterfaces verwijderen van een virtuele machine met ten minste drie netwerkinterfaces, omdat virtuele machines die zijn gemaakt met ten minste twee network interfaces moest altijd ten minste twee netwerkinterfaces hebben. Geen van deze beperkingen meer van toepassing. U kunt nu een virtuele machine maken met een willekeurig aantal netwerkinterfaces (maximaal het aantal dat wordt ondersteund door de VM-grootte).
- Standaard wordt de eerste netwerkinterface is gekoppeld aan een VM gedefinieerd als de *primaire* netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn *secundaire* netwerkinterfaces.
- Hoewel u kunt bepalen welke netwerkinterface u uitgaand verkeer naar, standaard verzonden, wordt al het uitgaande verkeer van de virtuele machine het IP-adres dat is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface wordt verzonden.
- In het verleden waren alle virtuele machines in dezelfde beschikbaarheidsset beschikken over een enkele of meerdere netwerkinterfaces. Virtuele machines met een willekeurig aantal netwerkinterfaces kunnen nu bestaan in dezelfde beschikbaarheidsset, tot aan het getal dat wordt ondersteund door de VM-grootte. U kunt alleen een virtuele machine toevoegen aan een beschikbaarheidsset wanneer deze wordt gemaakt. Zie voor meer informatie over beschikbaarheidssets, [de beschikbaarheid van virtuele machines in Azure beheren](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Terwijl netwerkinterfaces in dezelfde virtuele machine kunnen worden verbonden met verschillende subnetten binnen een virtueel netwerk, moeten de netwerkinterfaces allemaal verbonden met hetzelfde virtuele netwerk.
- U kunt elk IP-adres voor alle IP-configuratie van een primaire of secundaire netwerkinterface toevoegen aan een Load Balancer van Azure back-end-adresgroep. In het verleden kan alleen het primaire IP-adres voor de primaire netwerkinterface worden toegevoegd aan een back-end-pool. Zie voor meer informatie over IP-adressen en configuraties, [toevoegen, wijzigen of verwijderen IP-adressen](virtual-network-network-interface-addresses.md).
- Verwijderen van een virtuele machine, wordt de netwerkinterfaces die zijn gekoppeld aan deze niet verwijderd. Wanneer u een virtuele machine verwijdert, worden de netwerkinterfaces losgekoppeld van de virtuele machine. U kunt de netwerkinterfaces toevoegen aan andere virtuele machines of verwijder ze.
- Als een netwerkinterface een persoonlijke IPv6-adres is toegewezen heeft, moet u toevoegen (koppeling) naar een virtuele machine bij het maken van de virtuele machine. U kunt een netwerkinterface met een toegewezen IPv6-adres toevoegen aan een virtuele machine nadat u de virtuele machine hebt gemaakt. Als u een netwerkinterface met een toegewezen persoonlijke IPv6-adres toevoegen wanneer u een virtuele machine maakt, kunt u alleen netwerkinterface toevoegen aan de virtuele machine, ongeacht hoeveel netwerkinterfaces die ondersteuning biedt voor de VM-grootte. Zie [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md) voor meer informatie over IP-adressen toewijzen aan netwerkinterfaces.
- Net als bij IPv6, kunt u een netwerkinterface niet koppelen met versnelde netwerken die zijn ingeschakeld voor een virtuele machine nadat u dit hebt gemaakt. Verder, om te profiteren van versnelde netwerken, moet u ook stappen uitvoeren in het VM-besturingssysteem. Meer informatie over versnelde netwerken, en andere beperkingen, met voor [Windows](create-vm-accelerated-networking-powershell.md) of [Linux](create-vm-accelerated-networking-cli.md) virtuele machines.

## <a name="next-steps"></a>Volgende stappen
Als een virtuele machine maken met meerdere netwerkinterfaces of IP-adressen, leest u de volgende artikelen:

### <a name="commands"></a>Opdrachten

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere NIC's maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Één NIC VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Maak een enkel NIC-VM met een particulier IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|



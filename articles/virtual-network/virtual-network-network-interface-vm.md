---
title: Netwerkinterfaces toevoegen of verwijderen van virtuele machines van Azure | Microsoft Docs
description: Leer hoe u netwerkinterfaces toevoegen of verwijderen van netwerkinterfaces van virtuele machines.
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
ms.openlocfilehash: cf856a680601edd950cd0a5fddbc1241782478e2
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648894"
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Netwerkinterfaces toevoegen of verwijderen van netwerkinterfaces van virtuele machines

Leer hoe u een bestaande netwerkinterface toevoegen bij het maken van een Azure virtuele machine (VM), of als u wilt toevoegen of verwijderen van netwerkinterfaces van een bestaande virtuele machine gestopt (toewijzing opgeheven). Een netwerkinterface kan een virtuele machine van Azure om te communiceren met internet, Azure en on-premises bronnen. Een virtuele machine kan een of meer netwerkinterfaces hebben. 

Als u nodig hebt om toe te voegen, wijzigen of verwijderen van IP-adressen voor een netwerkinterface, Zie [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md). Als u maken wilt, wijzigen, of verwijder netwerkinterfaces, Zie [netwerkinterfaces beheren](virtual-network-network-interface.md).

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. In deze zelfstudie vereist de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.26 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

## <a name="add-existing-network-interfaces-to-a-new-vm"></a>Bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine

Wanneer u een virtuele machine via de portal maakt, wordt de portal maakt u een netwerkinterface met de standaardinstellingen en gekoppeld aan de virtuele machine voor u. U kan niet bestaande netwerkinterfaces toevoegen aan een nieuwe virtuele machine of een virtuele machine maken met meerdere netwerkinterfaces, met behulp van de Azure-portal. U kunt beide doen door met de CLI of PowerShell, maar zorg ervoor dat om vertrouwd te raken met de [beperkingen](#constraints). Als u een virtuele machine met meerdere netwerkinterfaces maken, moet u ook het besturingssysteem voor het gebruik ervan goed nadat u de virtuele machine hebt gemaakt. Meer informatie over het configureren van [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) voor meerdere netwerkinterfaces.

### <a name="commands"></a>Opdrachten

Voordat u de virtuele machine maakt, maakt u een netwerkinterface met behulp van de stappen in [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface).

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|PowerShell|[New-AzVM](/powershell/module/az.compute/new-azvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Een netwerkinterface toevoegen aan een bestaande virtuele machine

1. Meld u aan bij Azure Portal.
2. Typ de naam van de virtuele machine die u wilt de netwerkinterface toevoegen of bladert u naar de virtuele machine door te selecteren in het zoekvak boven aan de portal, **alle services**, en vervolgens **virtuele machines**. Nadat u de virtuele machine hebt gevonden, selecteert u dit. De virtuele machine moet ondersteuning voor het aantal netwerkinterfaces dat u wilt toevoegen. Om erachter te komen hoeveel netwerkinterfaces elke VM-grootte ondersteunt, Zie [grootten voor virtuele Linux-machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [grootten voor Windows virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  
3. Selecteer **overzicht**onder **instellingen**. Selecteer **stoppen**, en wacht totdat de **Status** van de virtuele machine wordt gewijzigd in **gestopt (toewijzing opgeheven)**.
4. Selecteer **netwerken**onder **instellingen**.
5. Selecteer **koppelen aan de netwerkinterface**. Selecteer in de lijst met netwerkinterfaces die momenteel niet worden gekoppeld aan een andere virtuele machine, die u wilt koppelen.

   >[!NOTE]
   >De netwerkinterface die u selecteert kan niet hebben versnelde netwerken ingeschakeld, kan niet een IPv6-adres is toegewezen hebben en moet zich in hetzelfde virtuele netwerk als het account dat met de netwerkinterface die is momenteel gekoppeld aan de virtuele machine.

   Als u een bestaande netwerkinterface niet hebt, moet u eerst een maken. Om dit te doen, selecteert u **netwerkinterface maken**. Zie voor meer informatie over het maken van een netwerkinterface, [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface). Zie voor meer informatie over aanvullende beperkingen bij het toevoegen van netwerkinterfaces op virtuele machines, [beperkingen](#constraints).

6. Selecteer **OK**.
7. Selecteer **overzicht**onder **instellingen**, en vervolgens **Start** de virtuele machine te starten.
8. Het besturingssysteem van de virtuele machine voor het gebruik van meerdere netwerkinterfaces correct configureren. Meer informatie over het configureren van [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) of [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) voor meerdere netwerkinterfaces.

### <a name="commands"></a>Opdrachten
|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm nic toevoegen](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_add) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Voeg AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="view-network-interfaces-for-a-vm"></a>Weergave-netwerkinterfaces voor een virtuele machine

U kunt de netwerkinterfaces die momenteel is gekoppeld aan een virtuele machine voor meer informatie over elke netwerkinterface-configuratie en het IP-adressen toegewezen aan elke netwerkinterface weergeven. 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is toegewezen aan de rol van eigenaar, bijdrager of Inzender voor netwerken voor uw abonnement. Zie voor meer informatie over het toewijzen van rollen aan accounts [ingebouwde rollen voor op rollen gebaseerd toegangsbeheer in Azure](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. In het vak met de tekst **zoeken naar resources** aan de bovenkant van de Azure-portal, typt u **virtuele machines**. Wanneer **virtuele machines** wordt weergegeven in de zoekresultaten, selecteert u dit.
3. Selecteer de naam van de virtuele machine waarvan u wilt om netwerkinterfaces weer te geven.
4. In de **instellingen** sectie voor de VM die u hebt geselecteerd, selecteer **netwerken**. Zie voor meer informatie over netwerkinterface-instellingen en hoe u deze kunt aanpassen, [netwerkinterfaces beheren](virtual-network-network-interface.md). Zie voor meer informatie over het toevoegen, wijzigen of verwijderen van IP-adressen die zijn toegewezen aan een netwerkinterface, [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md).

### <a name="commands"></a>Opdrachten

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_show)|
|PowerShell|[Get-AzVM](/powershell/module/az.compute/get-azvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-a-network-interface-from-a-vm"></a>Een netwerkinterface van een virtuele machine verwijderen

1. Meld u aan bij Azure Portal.
2. Zoek in het zoekvak boven aan de portal, de naam van de virtuele machine die u wilt verwijderen (loskoppelen) de netwerkinterface van, of blader voor de virtuele machine door het selecteren van **alle services**, en vervolgens **virtuele machines**. Nadat u de virtuele machine hebt gevonden, selecteert u dit.
3. Selecteer **overzicht**onder **instellingen**, en vervolgens **stoppen**. Wacht totdat de **Status** van de virtuele machine wordt gewijzigd in **gestopt (toewijzing opgeheven)**.
4. Selecteer **netwerken**onder **instellingen**.
5. Selecteer **netwerkinterface ontkoppelen**. Selecteer in de lijst met netwerkinterfaces die momenteel zijn gekoppeld aan de virtuele machine, de netwerkinterface die u wilt loskoppelen.

   >[!NOTE]
   >Als er slechts één netwerkinterface wordt weergegeven, loskoppelen u niet, omdat een virtuele machine altijd ten minste één netwerkinterface is gekoppeld moet.
6. Selecteer **OK**.

### <a name="commands"></a>Opdrachten

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ vm nic verwijderen](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_remove) (verwijzing) of [gedetailleerde stappen](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzVMNetworkInterface](/powershell/module/az.compute/remove-azvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (verwijzing) of [gedetailleerde stappen](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="constraints"></a>Beperkingen

- Een virtuele machine moet ten minste één netwerkinterface die is gekoppeld aan deze hebben.
- Een virtuele machine kan alleen hebben als veel netwerkinterfaces die zijn gekoppeld aan deze als de VM-grootte ondersteunt. Zie voor meer informatie over het aantal netwerkinterfaces elke VM-grootte ondersteunt, [grootten voor virtuele Linux-machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [grootten voor Windows virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Alle grootten ondersteuning voor ten minste twee netwerkinterfaces.
- De netwerkinterfaces die u aan een virtuele machine toevoegt kunnen niet op dit moment worden gekoppeld aan een andere virtuele machine. Zie voor meer informatie over het maken van netwerkinterfaces, [maken van een netwerkinterface](virtual-network-network-interface.md#create-a-network-interface).
- In het verleden kunnen alleen netwerkinterfaces worden toegevoegd aan virtuele machines die meerdere netwerkinterfaces worden ondersteund en zijn gemaakt met ten minste twee netwerkinterfaces. U kan niet een netwerkinterface toevoegen aan een virtuele machine die is gemaakt met één netwerkinterface, zelfs als de grootte van de virtuele machine meerdere netwerkinterfaces ondersteund. U kunt daarentegen alleen netwerkinterfaces verwijderen van een virtuele machine met ten minste drie netwerkinterfaces, omdat virtuele machines die zijn gemaakt met ten minste twee network interfaces heeft altijd ten minste twee netwerkinterfaces hebben. Geen van deze beperkingen zijn niet meer van toepassing. U kunt nu een virtuele machine maken met een willekeurig aantal netwerkinterfaces (maximaal het aantal dat wordt ondersteund door de VM-grootte).
- Standaard de eerste netwerkinterface die is gekoppeld aan een virtuele machine wordt gedefinieerd als de *primaire* netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn *secundaire* netwerkinterfaces.
- Hoewel u kunt bepalen welke netwerkinterface wordt u uitgaand verkeer, standaard verzonden, wordt al het uitgaande verkeer van de virtuele machine het IP-adres toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface is verzonden.
- In het verleden waren alle virtuele machines binnen dezelfde beschikbaarheidsset vereist voor een enkele of meerdere netwerkinterfaces hebben. Virtuele machines met een willekeurig aantal netwerkinterfaces kunnen nu bestaan in dezelfde beschikbaarheidsset, tot aan het aantal dat wordt ondersteund door de VM-grootte. U kunt alleen een virtuele machine toevoegen aan een beschikbaarheidsset wanneer deze wordt gemaakt. Zie voor meer informatie over beschikbaarheidssets, [de beschikbaarheid van virtuele machines in Azure beheren](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Terwijl de netwerkinterfaces in dezelfde virtuele machine kunnen worden verbonden met verschillende subnetten binnen een virtueel netwerk, moeten de netwerkinterfaces allemaal zijn verbonden met hetzelfde virtuele netwerk.
- U kunt elk IP-adres voor alle IP-configuratie van een primaire of secundaire netwerkinterface toevoegen aan een Azure Load Balancer-back-end-pool. In het verleden kon alleen het primaire IP-adres voor de primaire netwerkinterface worden toegevoegd aan een back-end-adrespool. Zie voor meer informatie over IP-adressen en configuraties, [toevoegen, wijzigen of verwijderen-IP-adressen](virtual-network-network-interface-addresses.md).
- Als u een virtuele machine verwijdert, worden de netwerkinterfaces die zijn gekoppeld aan het niet verwijderd. Wanneer u een virtuele machine verwijdert, worden de netwerkinterfaces losgekoppeld van de virtuele machine. U kunt de netwerkinterfaces aan verschillende VM's toevoegen of verwijderen.
- Als heeft een netwerkinterface in een persoonlijke IPv6-adres is toegewezen, moet u toevoegen (koppelen) naar een virtuele machine bij het maken van de virtuele machine. U kunt een netwerkinterface met een toegewezen IPv6-adres niet toevoegen aan een virtuele machine nadat u de virtuele machine hebt gemaakt. Als u een netwerkinterface met een toegewezen privé-IPv6-adres toevoegen wanneer u een virtuele machine maakt, kunt u alleen deze netwerkinterface toevoegen aan de virtuele machine, ongeacht het aantal netwerkinterfaces die ondersteuning biedt voor de VM-grootte. Zie [network interface-IP-adressen beheren](virtual-network-network-interface-addresses.md) voor meer informatie over het IP-adressen toewijzen aan netwerkinterfaces.
- Net als bij IPv6, kunt u een netwerkinterface niet koppelen met versneld netwerkondersteuning ingeschakeld met een virtuele machine nadat u dit hebt gemaakt. Bovendien is om te profiteren van versneld netwerken, moet u ook stappen binnen de VM-besturingssysteem. Meer informatie over versneld netwerken en andere beperkingen bij het gebruik van deze voor [Windows](create-vm-accelerated-networking-powershell.md) of [Linux](create-vm-accelerated-networking-cli.md) virtuele machines.

## <a name="next-steps"></a>Volgende stappen
Voor het maken van een virtuele machine met meerdere netwerkinterfaces of IP-adressen, Zie de volgende artikelen:

|Taak|Hulpprogramma|
|---|---|
|Een virtuele machine met meerdere NIC's maken|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Een enkel NIC-VM maken met meerdere IPv4-adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Een enkel NIC-VM maken met een privé-IPv6-adres (achter een Load Balancer van Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-sjabloon](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
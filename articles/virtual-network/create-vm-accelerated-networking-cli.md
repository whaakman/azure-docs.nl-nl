---
title: Een Azure-machine maken met versnelde netwerken | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine met versnelde netwerken ingeschakeld.
services: virtual-network
documentationcenter: na
author: gsilva5
manager: gedegrac
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/10/2019
ms.author: gsilva
ms.custom: ''
ms.openlocfilehash: 8c913d618313a72f6fb05ea45847a220f6070d42
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55765735"
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Een Linux-machine maken met versnelde netwerken

In deze zelfstudie leert u hoe u een Linux virtuele machine (VM) maken met versnelde netwerken. Zie voor informatie over het maken van een virtuele Windows-machine met versnelde netwerken [een Windows-VM maken met versnelde netwerken](create-vm-accelerated-networking-powershell.md). Versneld netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad krachtige omzeilt de host van het gegevenspad verminderen latentie en jitter CPU-gebruik, voor gebruik met de meest veeleisende workloads netwerk op ondersteunde VM-typen. De volgende afbeelding ziet u de communicatie tussen twee virtuele machines met en zonder versnelde netwerken:

![Vergelijking](./media/create-vm-accelerated-networking/accelerated-networking.png)

Alle netwerkverkeer naar en uit de virtuele machine moet zonder versnelde netwerken passeren de host en de virtuele switch. De virtuele switch biedt alle afdwingen van beleid, zoals netwerkbeveiligingsgroepen, toegangsbeheerlijsten, isolatie en andere netwerkservices gevirtualiseerd netwerkverkeer. Lees voor meer informatie over virtuele switches, de [Hyper-V-netwerkvirtualisatie en virtuele switch](https://technet.microsoft.com/library/jj945275.aspx) artikel.

Netwerkverkeer aankomt op van de virtuele machine-netwerkinterface (NIC) en wordt vervolgens doorgestuurd naar de virtuele machine met versneld netwerken. Alle beleidsregels voor network die de virtuele switch is van toepassing zijn nu offloaded en toegepast in de hardware. Toepassen van beleid in de hardware, kunt de NIC voor doorsturen van netwerkverkeer rechtstreeks naar de virtuele machine, overslaan van de host en de virtuele switch, terwijl alle het beleid dat wordt toegepast op de host.

De voordelen van versneld netwerken zijn alleen van toepassing op de virtuele machine die is ingeschakeld op. Voor de beste resultaten is het ideaal voor deze functie inschakelen op ten minste twee virtuele machines die zijn verbonden met de dezelfde Azure-netwerk (VNet). Tijdens de communicatie tussen vnet's of verbinding maakt on-premises, heeft deze functie minimale gevolgen voor de totale latentie.

## <a name="benefits"></a>Voordelen
* **Lagere latentie / hoger pakketten per seconde (pps):** De virtuele switch verwijderen uit het gegevenspad elimineert de tijd voor pakketten uitgaven in de host voor de beleidsverwerking van en het aantal pakketten dat kan worden verwerkt binnen de virtuele machine wordt verhoogd.
* **Minder jitter:** Verwerking van de virtuele switch, is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU dat de verwerking. Offloading van het afdwingen van beleid voor de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host de communicatie van de virtuele machine en alle software-interrupts en context switches.
* **Minder CPU-gebruik:** Overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende distributies worden gebruiksklaar uit de galerie met Azure ondersteund: 
* **Ubuntu 16.04+** 
* **SLES 12 SP3** 
* **RHEL 7.4**
* **CentOS 7.4**
* **CoreOS-Linux**
* **Debian "uitrekken" backports kernel**
* **Oracle Linux 7.4**

## <a name="limitations-and-constraints"></a>Beperkingen en beperkingen

### <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versneld netwerken wordt ondersteund op de meest algemene en geoptimaliseerde exemplaargrootten met 2 of meer vcpu's.  Deze ondersteunde reeksen zijn: D/DSv2 en F/Fs

Op de VM-exemplaren met 4 of meer vcpu's wordt versnelde netwerken op instanties die ondersteuning bieden voor hyperthreading is, ondersteund. Ondersteunde reeksen zijn: D/DSv3, E/ESv3, Fsv2 en Ms-/ Mms.

Zie voor meer informatie over VM-exemplaren [Linux VM-grootten](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Regio's
Beschikbaar in alle openbare Azure-regio's, evenals Azure Government-Clouds.

<!-- ### Network interface creation 
Accelerated networking can only be enabled for a new NIC. It cannot be enabled for an existing NIC.
removed per issue https://github.com/MicrosoftDocs/azure-docs/issues/9772 -->
### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Versnelde netwerken op een actieve virtuele machine inschakelen
Een ondersteunde VM-grootte zonder versneld netwerkondersteuning ingeschakeld kan alleen hebben voor de functie is ingeschakeld wanneer deze wordt gestopt en toewijzing ongedaan gemaakt.  
### <a name="deployment-through-azure-resource-manager"></a>Implementatie via Azure Resource Manager
Virtuele machines (klassiek) kan niet worden geïmplementeerd met versnelde netwerken.

## <a name="create-a-linux-vm-with-azure-accelerated-networking"></a>Een virtuele Linux-machine maken met Azure-versnelde netwerken

Hoewel dit artikel stappen bevat voor het maken van een virtuele machine met versneld netwerken met de Azure CLI, kunt u ook [maken van een virtuele machine met versneld netwerken met behulp van de Azure-portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bij het maken van een virtuele machine in de portal, onder **instellingen**, selecteer **ingeschakeld**onder **versnelde netwerken**. De optie voor het inschakelen van versneld netwerken niet wordt weergegeven in de portal, tenzij u hebt geselecteerd een [ondersteund besturingssysteem](#supported-operating-systems) en [VM-grootte](#supported-vm-instances). Nadat de virtuele machine is gemaakt, moet u Volg de instructies in [bevestigen dat versneld netwerken is ingeschakeld](#confirm-that-accelerated-networking-is-enabled).

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Installeer de meest recente [Azure CLI](/cli/azure/install-azure-cli) en aan te melden bij een Azure-account met [az login](/cli/azure/reference-index). In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen *myResourceGroup*, *myNic*, en *myVm*.

Maak een resourcegroep maken met [az group create](/cli/azure/group). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *centralus* locatie:

```azurecli
az group create --name myResourceGroup --location centralus
```

Selecteer een ondersteunde Linux-regio die worden vermeld in [Linux versnelde netwerken](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* met één subnet:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken
Maak een netwerkbeveiligingsgroep met [az network nsg maken](/cli/azure/network/nsg). In het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* gemaakt:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

De netwerkbeveiligingsgroep bevat verschillende standaardregels, waarbij een van alle binnenkomende toegang via Internet wordt uitgeschakeld. Open een poort voor SSH-toegang tot de virtuele machine met [az network nsg-regel maken](/cli/azure/network/nsg/rule):

```azurecli
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup \
  --name Allow-SSH-Internet \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 22
```

### <a name="create-a-network-interface-with-accelerated-networking"></a>Maak een netwerkinterface met versneld netwerken

Maak een openbaar IP-adres met [az network public-ip create](/cli/azure/network/public-ip). Een openbaar IP-adres is niet vereist als u niet van plan voor toegang tot de virtuele machine via Internet, maar om de stappen in dit artikel te voltooien, het is vereist.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Maken van een netwerkinterface met [az network nic maken](/cli/azure/network/nic) met versneld netwerkondersteuning ingeschakeld. Het volgende voorbeeld wordt een netwerkinterface met de naam *myNic* in de *mySubnet* subnet van de *myVnet* virtueel netwerk en wordt de  *myNetworkSecurityGroup* netwerkbeveiligingsgroep aan de netwerkinterface:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --accelerated-networking true \
    --public-ip-address myPublicIp \
    --network-security-group myNetworkSecurityGroup
```

### <a name="create-a-vm-and-attach-the-nic"></a>Een virtuele machine maken en koppelen van de NIC
Wanneer u de virtuele machine, maakt de NIC geven u hebt gemaakt met `--nics`. Selecteer een grootte en de distributie die worden vermeld in [Linux versnelde netwerken](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

Maak een VM met [az vm create](/cli/azure/vm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met de UbuntuLTS-installatiekopie en een grootte die ondersteuning biedt voor versnelde netwerken (*Standard_DS4_v2*):

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS4_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Zie voor een lijst van alle VM-grootten en -kenmerken, [Linux VM-grootten](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Nadat de virtuele machine is gemaakt, wordt uitvoer is vergelijkbaar met de volgende voorbeelduitvoer wordt geretourneerd. Let op het **openbare IP-adres**. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stappen.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "centralus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "192.168.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

### <a name="confirm-that-accelerated-networking-is-enabled"></a>Bevestig dat versneld netwerken is ingeschakeld

Gebruik de volgende opdracht om voor de VM een SSH-sessie te maken. Vervang `<your-public-ip-address>` met het openbare IP-adres toegewezen aan de virtuele machine gemaakt en vervangen *azureuser* als u een andere waarde voor gebruikt `--admin-username` tijdens het maken van de virtuele machine.

```bash
ssh azureuser@<your-public-ip-address>
```

Voer vanuit de Bash-shell `uname -r` en controleer of de kernelversie is een van de volgende versies of hoger:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Controleer of het apparaat Mellanox VF wordt blootgesteld aan de virtuele machine met de `lspci` opdracht. De resulterende uitvoer is vergelijkbaar met de volgende uitvoer:

```bash
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Neem contact op voor de activiteit op de VF (virtuele functie) met de `ethtool -S eth0 | grep vf_` opdracht. Als u uitvoer die vergelijkbaar is met het volgende voorbeeld van uitvoer, versneld netwerken is ingeschakeld en werken.

```bash
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Versneld netwerken is nu ingeschakeld voor uw virtuele machine.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Versnelde netwerken op bestaande virtuele machines inschakelen
Als u een virtuele machine zonder versnelde netwerken hebt gemaakt, is het mogelijk is deze functie op een bestaande virtuele machine in te schakelen.  De virtuele machine moet bieden ondersteuning voor versnelde netwerken voldoen aan de volgende vereisten die ook hierboven worden beschreven:

* De virtuele machine moet een ondersteunde grootte voor versnelde netwerken
* De virtuele machine moet een ondersteunde Azure-galerie-installatiekopie (en de kernelversie voor Linux)
* Alle virtuele machines in een beschikbaarheidsset of VMSS moet worden gestopt/de toewijzing ongedaan gemaakt voordat u inschakelt op een NIC versnelde netwerken

### <a name="individual-vms--vms-in-an-availability-set"></a>Afzonderlijke VM's en virtuele machines in een beschikbaarheidsset instellen
Eerst stoppen/toewijzing ongedaan maken de virtuele machine of, als een Beschikbaarheidsset, alle virtuele machines in de Set:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

Belangrijk Let op: als uw virtuele machine afzonderlijk, is gemaakt zonder een beschikbaarheidsset, u alleen moet stoppen/toewijzing van de afzonderlijke virtuele machine om in te schakelen versnelde netwerken.  Als uw virtuele machine is gemaakt met een beschikbaarheidsset, moet alle virtuele machines die zijn opgenomen in de beschikbaarheidsset worden gestopt/de toewijzing ongedaan gemaakt voordat u inschakelt op een van de NIC's versnelde netwerken. 

Nadat gestopt, schakelt u versnelde netwerken op de NIC van de virtuele machine:

```azurecli
az network nic update \
    --name myNic \
    --resource-group myResourceGroup \
    --accelerated-networking true
```

Opnieuw opstarten van uw virtuele machine of als in een Beschikbaarheidsset, alle virtuele machines in de Set en Bevestig dat versnelde netwerken is ingeschakeld: 

```azurecli
az vm start --resource-group myResourceGroup \
    --name myVM
```

### <a name="vmss"></a>VMSS
VMSS is iets anders, maar dezelfde werkstroom volgt.  Stop eerst de virtuele machines:

```azurecli
az vmss deallocate \
    --name myvmss \
    --resource-group myrg
```

Nadat de virtuele machines zijn gestopt, moet u de eigenschap versnelde netwerken onder de netwerkinterface bijwerken:

```azurecli
az vmss update --name myvmss \
    --resource-group myrg \
    --set virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].enableAcceleratedNetworking=true
```

Let op: een VMSS heeft VM-upgrades die met behulp van drie verschillende instellingen, automatische, rolling en handmatige updates toe te passen.  Het beleid is ingesteld op automatisch in deze instructies zodat de VMSS de wijzigingen onmiddellijk na het opnieuw opstarten haalt.  Zodat de wijzigingen direct worden doorgevoerd, moet u deze instellen op automatisch: 

```azurecli
az vmss update \
    --name myvmss \
    --resource-group myrg \
    --set upgradePolicy.mode="automatic"
```

Ten slotte opnieuw starten de VMSS:

```azurecli
az vmss start \
    --name myvmss \
    --resource-group myrg
```

Nadat u opnieuw opstarten, wacht totdat de upgrades te voltooien, maar nadat deze is voltooid, de VF wordt weergegeven in de virtuele machine.  (Zorg ervoor dat u een ondersteunde OS- en VM-grootte.)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Formaat wijzigen van bestaande VM's met versnelde netwerken

VM's met versnelde netwerken ingeschakeld kunnen alleen worden gewijzigd met VM's die ondersteuning bieden voor versnelde netwerken.  

Een virtuele machine met versnelde netwerken ingeschakeld kan niet worden uitgebreid tot een VM-instantie die geen ondersteuning biedt voor versnelde netwerken met behulp van de bewerking formaat wijzigen.  In plaats daarvan om het formaat van een van deze virtuele machines aan: 

* Stoppen/toewijzing ongedaan maken de virtuele machine of als in een beschikbaarheid van set/VMSS, stoppen/toewijzing ongedaan maken alle virtuele machines in de set/VMSS.
* Versneld netwerken moet worden uitgeschakeld op de NIC van de virtuele machine of als in een beschikbaarheid van set/VMSS, alle virtuele machines in de set/VMSS.
* Zodra Accelerated Networking is uitgeschakeld, kan de VM-beschikbaarheid/set/VMSS worden verplaatst naar een nieuwe omvang die biedt geen ondersteuning voor versnelde netwerken en opnieuw gestart.  


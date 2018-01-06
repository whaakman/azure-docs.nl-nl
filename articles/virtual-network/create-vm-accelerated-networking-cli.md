---
title: Maken van een virtuele machine van Azure met versnelde toegang | Microsoft Docs
description: Informatie over het maken van een virtuele Linux-machine met versnelde netwerken.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 908d81c363a556917d211e0bcc92188f849fb690
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Een virtuele Linux-machine maken met het versnelde netwerken

> [!IMPORTANT] 
> Virtuele machines moeten worden gemaakt met de versnelde netwerken ingeschakeld. Deze functie kan niet worden ingeschakeld op de bestaande virtuele machines. U kunt de stappen hieronder om in te schakelen versnelde netwerken:
>   1. Verwijder de virtuele machine.
>   2. De virtuele machine opnieuw maken met de versnelde netwerken ingeschakeld.
>

In deze zelfstudie leert u het maken van virtuele Linux-machine (VM) met versnelde netwerken. Versnelde netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) voor een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad hoge prestaties wordt de host van het gegevenspad, waardoor latentie en jitter CPU-gebruik, voor gebruik met de zwaarste netwerkbelasting op ondersteunde VM typen overgeslagen. De volgende afbeelding ziet communicatie tussen twee virtuele machines met en zonder versnelde netwerken:

![Vergelijking](./media/create-vm-accelerated-networking/accelerated-networking.png)

Alle netwerkverkeer naar en vanuit de virtuele machine moet zonder versnelde netwerken passeren de host en de virtuele switch. De virtuele switch biedt alle afdwingen van beleid, zoals netwerkbeveiligingsgroepen, access control list, isolatie- en andere gevirtualiseerd netwerkservices op netwerkverkeer. Lees voor meer informatie over virtuele switches, de [Hyper-V-netwerkvirtualisatie en virtuele switch](https://technet.microsoft.com/library/jj945275.aspx) artikel.

Met versnelde netwerken netwerkverkeer binnenkomt bij de VM-netwerkadapter (NIC) en wordt vervolgens doorgestuurd naar de virtuele machine. Alle netwerkbeleid dat de virtuele switch is van toepassing zijn nu offloaded en toegepast in de hardware. Toepassen van beleid in de hardware, kunt de NIC forward netwerkverkeer rechtstreeks naar de virtuele machine, het omzeilen van de host en de virtuele switch, terwijl alle het beleid dat wordt toegepast op de host.

De voordelen van versnelde netwerken zijn alleen van toepassing op de virtuele machine die is ingeschakeld. Voor de beste resultaten is het ideaal voor het inschakelen van deze functie op ten minste twee virtuele machines die zijn verbonden met de dezelfde Azure Virtual Network (VNet). Tijdens de communicatie tussen VNets of verbindende on-premises, heeft dit onderdeel minimale gevolgen voor de algehele latentie.

## <a name="benefits"></a>Voordelen
* **Lagere latentie / hoger pakketten per seconde (pps):** verwijderen van de virtuele switch uit het gegevenspad verwijdert u de tijd te besteden aan de pakketten in de host voor de verwerking van beleid en verhoogt het aantal pakketten dat kan worden verwerkt in de virtuele machine.
* **Minder jitter:** virtuele switch verwerking is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU die bezig is met de verwerking. Offloading van het afdwingen van beleid op de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host voor VM-communicatie en alle software-interrupts en context switches.
* **CPU-gebruik verlaagd:** overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
* **Ubuntu 16.04**: 4.11.0-1013 of hogere kernelversie
* **SLES SP3**: 4.4.92-6.18 of hogere kernelversie
* **RHEL**: 7.4.2017120423 of hogere kernelversie
* **CentOS**: 7.4.20171206 of hogere kernelversie

## <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versnelde netwerken wordt ondersteund op de meest algemene doeleinden en geoptimaliseerd voor compute exemplaar groottes met 4 of meer vcpu's. Op exemplaren zoals D/DSv3 of E/ESv3 die ondersteuning bieden voor hyperthreading wordt versnelde toegang ondersteund op VM-instanties met 8 vcpu's.  Ondersteunde reeksen zijn: D/DSv2, D/DSv3 E/ESv3, Fs-F/Fsv2 en Ms-en Mms. 

Zie voor meer informatie over VM-instanties [Linux VM-grootten](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Regio's
Beschikbaar in alle openbare Azure-regio's met uitzondering van Oost-Azië.   Azure Government Cloud is nog niet ondersteund.

## <a name="limitations"></a>Beperkingen
De volgende beperkingen bestaan wanneer deze wordt met deze mogelijkheid:

* **Interface maken van een netwerk:** Accelerated netwerken kan alleen worden ingeschakeld voor een nieuwe NIC. Deze kan niet worden ingeschakeld voor een bestaande NIC.
* **Maken van VM:** een NIC met versnelde netwerken ingeschakeld kan alleen worden gekoppeld aan een VM wanneer de virtuele machine wordt gemaakt. De NIC kan niet worden gekoppeld aan een bestaande virtuele machine. Als de virtuele machine toe te voegen aan de bestaande beschikbaarheidsset is ingesteld, moeten alle virtuele machines in de beschikbaarheidsset ook versnelde netwerken ingeschakeld.
* **Implementatie via Azure Resource Manager alleen:** virtuele machines (klassiek) kan niet worden geïmplementeerd met versnelde toegang.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/#login). In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *myNic*, en *myVm*.

Maak een resourcegroep maken met [az group create](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *centralus* locatie:

```azurecli
az group create --name myResourceGroup --location centralus
```

Moet u een ondersteunde Linux-regio die worden vermeld in [Linux versnelde netwerken](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

Maak een virtueel netwerk met [az network vnet maken](/cli/azure/network/vnet#create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* met één subnet:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken
Maken van een netwerkbeveiligingsgroep met [az netwerk nsg maken](/cli/azure/network/nsg#create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

De netwerkbeveiligingsgroep bevat verschillende standaardregels, waarbij een van alle binnenkomende toegang via het Internet wordt uitgeschakeld. Open een poort voor SSH-toegang tot de virtuele machine met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create):

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

## <a name="create-a-network-interface-with-accelerated-networking"></a>Een netwerkinterface met versnelde netwerken maken

Maken van een openbaar IP-adres met [az netwerk openbare ip-maken](/cli/azure/network/public-ip#az_network_public_ip_create). Een openbaar IP-adres is niet vereist als u niet van plan bent voor toegang tot de virtuele machine vanaf het Internet, maar de stappen in dit artikel uit te voeren, dit is vereist.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Maken van een netwerkinterface met [az netwerk nic maken](/cli/azure/network/nic#create) met versnelde netwerken ingeschakeld. Het volgende voorbeeld wordt een netwerkinterface met de naam *myNic* in de *mySubnet* subnet van de *myVnet* virtueel netwerk en gekoppeld de  *myNetworkSecurityGroup* netwerkbeveiligingsgroep voor de netwerkinterface:

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

## <a name="create-a-vm-and-attach-the-nic"></a>Een virtuele machine maken en koppelen van de NIC
Wanneer u de virtuele machine, maakt de NIC Geef u hebt gemaakt met `--nics`. Moet u een grootte en distributiepunten die worden vermeld in [Linux versnelde netwerken](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

Maak een VM met [az vm create](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met de installatiekopie van het UbuntuLTS en een grootte die ondersteuning biedt voor versnelde Networking (*Standard_DS4_v2*):

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

Zie voor een lijst van alle VM-grootten en kenmerken [Linux VM-grootten](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Zodra de virtuele machine is gemaakt, wordt de uitvoer ziet er als de volgende voorbeelduitvoer geretourneerd. Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stappen.

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

## <a name="confirm-that-accelerated-networking-is-enabled"></a>Controleer of de versnelde netwerken is ingeschakeld

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de virtuele machine. Vervang `<your-public-ip-address>` met het openbare IP-adres is toegewezen aan de virtuele machine hebt gemaakt en vervang *azureuser* als u een andere waarde voor gebruikt `--admin-username` tijdens het maken van de virtuele machine.

```bash
ssh azureuser@<your-public-ip-address>
```

Voer vanuit de Bash-shell `uname -r` en Bevestig dat de kernelversie een van de volgende versies of hoger is:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Bevestig het Mellanox VF-apparaat wordt blootgesteld aan de virtuele machine met de `lspci` opdracht. De resulterende uitvoer is vergelijkbaar met de volgende uitvoer:

```bash
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Controleer voor activiteiten in de VF (virtuele functie) met de `ethtool -S eth0 | grep vf_` opdracht. Als u krijgt uitvoer vergelijkbaar met het volgende voorbeeld uitvoert, versnelde netwerken is ingeschakeld en werkt.

```bash
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Versnelde netwerken is nu ingeschakeld voor uw virtuele machine.

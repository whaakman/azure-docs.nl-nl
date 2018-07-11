---
title: Een Linux-VM maken in Azure met meerdere NIC's | Microsoft Docs
description: Informatie over het maken van een Linux-VM met meerdere NIC's die zijn gekoppeld aan met behulp van de Azure CLI 2.0 of Resource Manager-sjablonen.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: cynthn
ms.openlocfilehash: 257b80c30823be41893be8659845d4fcbc922da3
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932269"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Over het maken van een virtuele Linux-machine in Azure met meerdere netwerkinterfacekaarten
U kunt een virtuele machine (VM) maken in Azure met meerdere virtuele netwerkinterfaces (NIC's) die zijn gekoppeld aan. Een veelvoorkomend scenario is dat verschillende subnetten voor front-end en back-end-connectiviteit of een netwerk dat is toegewezen aan een oplossing voor controle of back-up. Dit artikel wordt uitgelegd hoe u een virtuele machine met meerdere NIC's die zijn gekoppeld aan het maken en hoe u toevoegen of verwijderen van NIC's van een bestaande virtuele machine. Verschillende [VM-grootten](sizes.md) een verschillend aantal NIC's ondersteunen, dus het formaat van uw virtuele machine dienovereenkomstig.

Dit artikel wordt uitgelegd hoe u een virtuele machine maken met meerdere NIC's met de Azure CLI 2.0. 


## <a name="create-supporting-resources"></a>Ondersteunende resources maken
Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en aan te melden bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen *myResourceGroup*, *mystorageaccount*, en *myVM*.

Maak eerst een resourcegroep met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maken van het virtuele netwerk met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* en subnet met de naam *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Maak een subnet voor de back-end-verkeer met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Het volgende voorbeeld wordt een subnet met de naam *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Maak een netwerkbeveiligingsgroep met [az network nsg maken](/cli/azure/network/nsg#az_network_nsg_create). In het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* gemaakt:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Maken en configureren van meerdere NIC 's
Maak twee NIC's met [az network nic maken](/cli/azure/network/nic#az_network_nic_create). Het volgende voorbeeld maakt u twee NIC's, met de naam *myNic1* en *myNic2*, de netwerkbeveiligingsgroep die zijn verbonden met één NIC verbinding te maken met elk subnet:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Een virtuele machine maken en de NIC's koppelen
Wanneer u de virtuele machine, maakt de NIC's geven u hebt gemaakt met `--nics`. U moet ook Wees voorzichtig bij het selecteren van de VM-grootte. Er zijn limieten voor het totale aantal NIC's die u aan een virtuele machine toevoegen kunt. Meer informatie over [Linux VM-grootten](sizes.md). 

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

Routering tabellen toevoegen aan het gastbesturingssysteem te installeren via de stappen in [configureren van het gastbesturingssysteem te installeren voor meerdere NIC's](#configure-guest-os-for- multiple-nics).

## <a name="add-a-nic-to-a-vm"></a>Een NIC toevoegen aan een virtuele machine
De vorige stappen een virtuele machine met meerdere NIC's gemaakt. U kunt ook NIC's toevoegen aan een bestaande virtuele machine met de Azure CLI 2.0. Verschillende [VM-grootten](sizes.md) een verschillend aantal NIC's ondersteunen, dus het formaat van uw virtuele machine dienovereenkomstig. Indien nodig, kunt u [vergroten of verkleinen van een virtuele machine](change-vm-size.md).

Maak een andere NIC met [az network nic maken](/cli/azure/network/nic#az_network_nic_create). Het volgende voorbeeld wordt een NIC met de naam *myNic3* verbonden met het subnet en netwerk-beveiligingsgroep van back-end, die in de vorige stappen is gemaakt:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Een NIC toevoegen aan een bestaande virtuele machine, moet u eerst de virtuele machine met toewijzing [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken *myVM*:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Toevoegen van de NIC met [az vm nic toevoegen](/cli/azure/vm/nic#az_vm_nic_add). Het volgende voorbeeld voegt *myNic3* naar *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Start de virtuele machine met [az vm start](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

Routering tabellen toevoegen aan het gastbesturingssysteem te installeren via de stappen in [configureren van het gastbesturingssysteem te installeren voor meerdere NIC's](#configure-guest-os-for- multiple-nics).

## <a name="remove-a-nic-from-a-vm"></a>Een NIC van een virtuele machine verwijderen
Als u wilt verwijderen een NIC van een bestaande virtuele machine, moet u eerst de virtuele machine met toewijzing [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Verwijderen van de NIC met [az vm nic verwijderen](/cli/azure/vm/nic#az_vm_nic_remove). Het volgende voorbeeld verwijdert u *myNic3* van *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Start de virtuele machine met [az vm start](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC's met behulp van Resource Manager-sjablonen maken
Azure Resource Manager-sjablonen gebruiken declaratieve JSON-bestanden voor het definiëren van uw omgeving. U vindt een [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-sjablonen bieden een manier om meerdere exemplaren van een resource maken tijdens de implementatie, zoals het maken van meerdere NIC's. U gebruikt *kopie* om op te geven van het aantal exemplaren te maken:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Meer informatie over [het maken van meerdere exemplaren met behulp van *kopie*](../../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` toe te voegen vervolgens een nummer aan de naam van een resource, zodat u kunt maken `myNic1`, `myNic2`, enzovoort. De volgende toont een voorbeeld van de indexwaarde toe te voegen:

```json
"name": "[concat('myNic', copyIndex())]", 
```

U kunt een compleet voorbeeld van lezen [het maken van meerdere NIC's met behulp van Resource Manager-sjablonen](../../virtual-network/template-samples.md).

Routering tabellen toevoegen aan het gastbesturingssysteem te installeren via de stappen in [configureren van het gastbesturingssysteem te installeren voor meerdere NIC's](#configure-guest-os-for- multiple-nics).

## <a name="configure-guest-os-for-multiple-nics"></a>Gast-OS configureren voor meerdere NIC 's
Wanneer u meerdere NIC's aan een Linux-VM toevoegen, moet u maken van regels voor doorsturen. Met deze regels kunt de virtuele machine te verzenden en ontvangen van verkeer die deel uitmaakt van een specifieke netwerkadapter. Anders wordt het verkeer dat hoort bij *eth1*, kan niet bijvoorbeeld correct worden verwerkt door de gedefinieerde standaardroute.

Om op te lossen dit probleem routering, moet u eerst twee routering tabellen toevoegen */etc/iproute2/rt_tables* als volgt:

```bash
echo "200 eth0-rt" >> /etc/iproute2/rt_tables
echo "201 eth1-rt" >> /etc/iproute2/rt_tables
```

Bewerken om de wijziging permanent en toegepast tijdens de activering van netwerk-stack, */etc/sysconfig/network-scripts/ifcfg-eth0* en */etc/sysconfig/network-scripts/ifcfg-eth1*. Wijzigen van de regel *"NM_CONTROLLED = yes"* naar *"NM_CONTROLLED = Nee"*. Zonder deze stap wordt worden de extra dat regels/routes niet automatisch toegepast.
 
De routetabellen vervolgens worden uitgebreid. Stel dat we hebben de volgende instellingen:

*Routering*

```bash
default via 10.0.1.1 dev eth0 proto static metric 100
10.0.1.0/24 dev eth0 proto kernel scope link src 10.0.1.4 metric 100
10.0.1.0/24 dev eth1 proto kernel scope link src 10.0.1.5 metric 101
168.63.129.16 via 10.0.1.1 dev eth0 proto dhcp metric 100
169.254.169.254 via 10.0.1.1 dev eth0 proto dhcp metric 100
```

*Interfaces*

```bash
lo: inet 127.0.0.1/8 scope host lo
eth0: inet 10.0.1.4/24 brd 10.0.1.255 scope global eth0    
eth1: inet 10.0.1.5/24 brd 10.0.1.255 scope global eth1
```

U zou vervolgens de volgende bestanden maken en de juiste regels en routes toevoegen aan elk(e):

- */etc/SysConfig/Network-scripts/rule-eth0*

    ```bash
    from 10.0.1.4/32 table eth0-rt
    to 10.0.1.4/32 table eth0-rt
    ```

- */etc/SysConfig/Network-scripts/route-eth0*

    ```bash
    10.0.1.0/24 dev eth0 table eth0-rt
    default via 10.0.1.1 dev eth0 table eth0-rt
    ```

- */etc/SysConfig/Network-scripts/rule-eth1*

    ```bash
    from 10.0.1.5/32 table eth1-rt
    to 10.0.1.5/32 table eth1-rt
    ```

- */etc/SysConfig/Network-scripts/route-eth1*

    ```bash
    10.0.1.0/24 dev eth1 table eth1-rt
    default via 10.0.1.1 dev eth1 table eth1-rt
    ```

De wijzigingen wilt toepassen, opnieuw de *netwerk* service als volgt:

```bash
systemctl restart network
```

De regels voor doorsturen is nu correct voldaan en u kunt verbinding maken met een van beide interface indien nodig.


## <a name="next-steps"></a>Volgende stappen
Beoordeling [Linux VM-grootten](sizes.md) bij het maken van een virtuele machine met meerdere NIC's. Let op het maximum aantal NIC's die ondersteuning biedt voor elke VM-grootte. 

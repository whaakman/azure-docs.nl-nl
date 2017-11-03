---
title: Maak een Linux-VM in Azure met meerdere NIC's | Microsoft Docs
description: Informatie over het maken van een Linux-VM met meerdere NIC's gekoppeld met behulp van de Azure CLI 2.0 of Resource Manager-sjablonen.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: iainfou
ms.openlocfilehash: 0c41388623b82421bd09f31fbc4b3769de758e4c
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Het maken van een virtuele Linux-machine in Azure met meerdere netwerk netwerkinterfacekaarten
U kunt een virtuele machine (VM) maken in Azure met meerdere virtuele netwerkinterfaces (NIC's) is gekoppeld. Een veelvoorkomend scenario is om verschillende subnetten voor front-end en back-end-verbinding of een netwerk dat is toegewezen aan een oplossing met bewaking of back-up. Dit artikel wordt uitgelegd hoe een virtuele machine maken met meerdere NIC's gekoppeld en hoe toevoegen of verwijderen van NIC's van een bestaande virtuele machine. Andere [VM-grootten](sizes.md) ondersteunen een verschillend aantal NIC's, dus het formaat van uw virtuele machine dienovereenkomstig.

Dit artikel wordt uitgelegd hoe u een virtuele machine maken met meerdere NIC's met de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](multiple-nics-nodejs.md).


## <a name="create-supporting-resources"></a>Ondersteunende resources maken
Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/#login).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *mystorageaccount*, en *myVM*.

Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maken van het virtuele netwerk met [az network vnet maken](/cli/azure/network/vnet#create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* en subnet met de naam *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Een subnet maken voor de back-end-verkeer met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#create). Het volgende voorbeeld wordt een subnet met de naam *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Maken van een netwerkbeveiligingsgroep met [az netwerk nsg maken](/cli/azure/network/nsg#create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Maak en configureer meerdere NIC 's
Maak twee NIC's met [az netwerk nic maken](/cli/azure/network/nic#create). Het volgende voorbeeld maakt u twee NIC's met de naam *myNic1* en *myNic2*, de netwerkbeveiligingsgroep verbonden met één NIC die verbinding maken met elk subnet:

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

## <a name="create-a-vm-and-attach-the-nics"></a>Een virtuele machine maken en koppelen van de NIC 's
Wanneer u de virtuele machine, maakt de NIC's geven u hebt gemaakt met `--nics`. U moet ook Wees voorzichtig wanneer u de VM-grootte. Er gelden beperkingen voor het totale aantal NIC's die u aan een virtuele machine toevoegen kunt. Lees meer over [Linux VM-grootten](sizes.md). 

Maak een VM met [az vm create](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM*:

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

## <a name="add-a-nic-to-a-vm"></a>Een NIC toevoegen aan een virtuele machine
De vorige stappen kunt u een virtuele machine met meerdere NIC's gemaakt. U kunt ook NIC's toevoegen aan een bestaande virtuele machine met de Azure CLI 2.0. Andere [VM-grootten](sizes.md) ondersteunen een verschillend aantal NIC's, dus het formaat van uw virtuele machine dienovereenkomstig. Indien nodig, kunt u [vergroten of verkleinen van een virtuele machine](change-vm-size.md).

Maken van een ander NIC met [az netwerk nic maken](/cli/azure/network/nic#create). Het volgende voorbeeld wordt een NIC met de naam *myNic3* verbonden met de back-end subnet netwerk beveiligingsgroep en in de vorige stappen hebt gemaakt:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Als een NIC toevoegen aan een bestaande virtuele machine, met de virtuele machine eerst ongedaan [az vm ongedaan](/cli/azure/vm#deallocate). Het volgende voorbeeld de virtuele machine met de naam deallocates *myVM*:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Toevoegen van de NIC met [az vm nic toevoegen](/cli/azure/vm/nic#add). Het volgende voorbeeld wordt *myNic3* naar *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Start de virtuele machine met [az vm start](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>Een NIC van een virtuele machine verwijderen
Als u wilt verwijderen een NIC van een bestaande virtuele machine, met de virtuele machine eerst ongedaan [az vm ongedaan](/cli/azure/vm#deallocate). Het volgende voorbeeld de virtuele machine met de naam deallocates *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Verwijderen van de NIC met [az vm nic verwijderen](/cli/azure/vm/nic#remove). Het volgende voorbeeld verwijdert u *myNic3* van *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Start de virtuele machine met [az vm start](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC's met behulp van Resource Manager-sjablonen maken
Azure Resource Manager-sjablonen gebruiken declaratieve JSON-bestanden voor het definiëren van uw omgeving. U vindt een [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-sjablonen kunnen u meerdere exemplaren van een bron tijdens implementatie, zoals het maken van meerdere NIC's maken. U gebruikt *kopie* om op te geven van het aantal exemplaren te maken:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Lees meer over [maken van meerdere exemplaren die gebruikmaken van *kopie*](../../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` toe te voegen vervolgens een nummer aan de naam van een resource, zodat u kunt maken `myNic1`, `myNic2`, enzovoort. Hier volgt een voorbeeld van de waarde voor de index:

```json
"name": "[concat('myNic', copyIndex())]", 
```

U kunt een compleet voorbeeld van lezen [meerdere NIC's met behulp van Resource Manager-sjablonen maken](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).


## <a name="configure-guest-os-for-multiple-nics"></a>Configureren van Gast OS voor meerdere NIC 's
Wanneer u meerdere NIC's aan een Linux-VM toevoegt, moet u routeringsregels maken. Met deze regels kunt de virtuele machine te verzenden en ontvangen van verkeer dat tot een specifieke NIC. behoort Anders wordt het verkeer dat hoort bij *eth1*, niet bijvoorbeeld correct worden verwerkt door de gedefinieerde standaardroute.

U lost dit probleem routering door twee routeringstabellen aan eerst toevoegen */etc/iproute2/rt_tables* als volgt:

```bash
echo "200 eth0-rt" >> /etc/iproute2/rt_tables
echo "201 eth1-rt" >> /etc/iproute2/rt_tables
```

Als u de wijziging permanente en toegepast tijdens de activering van network-stack, bewerk */etc/sysconfig/network-scipts/ifcfg-eth0* en */etc/sysconfig/network-scipts/ifcfg-eth1*. De regel ALTER *' NM_CONTROLLED = yes '* naar *' NM_CONTROLLED = Nee "*. Zonder deze stap wordt worden de aanvullende dat regels/routering niet automatisch toegepast.
 
Vervolgens kunt u de routeringstabellen uitbreiden. Stel, dat we hebben de volgende instellingen:

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

U zou vervolgens de volgende bestanden maken en de juiste regels en de routes toevoegen aan elk:

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

De wijzigingen wilt toepassen, start opnieuw op de *netwerk* service als volgt:

```bash
systemctl restart network
```

De routeringsregels zijn nu correct geïmplementeerd en u kunt verbinding maken met een interface naar behoefte.


## <a name="next-steps"></a>Volgende stappen
Bekijk [Linux VM-grootten](sizes.md) bij het maken van een virtuele machine met meerdere NIC's. Let op het maximum aantal NIC's die ondersteuning biedt voor elke VM-grootte. 

---
title: Een Linux-VM maken in Azure met meerdere NIC's | Microsoft Docs
description: Informatie over het maken van een Linux-VM met meerdere NIC's die zijn gekoppeld aan met behulp van de Azure CLI 2.0 of Resource Manager-sjablonen.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/07/2018
ms.author: iainfou
ms.openlocfilehash: aae71dafd3685e44975049c4287c083abc2330bc
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144853"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Over het maken van een virtuele Linux-machine in Azure met meerdere netwerkinterfacekaarten
U kunt een virtuele machine (VM) maken in Azure met meerdere virtuele netwerkinterfaces (NIC's) die zijn gekoppeld aan. Een veelvoorkomend scenario is dat verschillende subnetten voor front-end en back-end-connectiviteit of een netwerk dat is toegewezen aan een oplossing voor controle of back-up. Dit artikel wordt uitgelegd hoe u een virtuele machine met meerdere NIC's die zijn gekoppeld aan het maken en hoe u toevoegen of verwijderen van NIC's van een bestaande virtuele machine. Verschillende [VM-grootten](sizes.md) een verschillend aantal NIC's ondersteunen, dus het formaat van uw virtuele machine dienovereenkomstig.

Dit artikel wordt uitgelegd hoe u een virtuele machine maken met meerdere NIC's met de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](multiple-nics-nodejs.md).


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
    --address-prefix 10.0.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 10.0.1.0/24
```

Maak een subnet voor de back-end-verkeer met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Het volgende voorbeeld wordt een subnet met de naam *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 10.0.2.0/24
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

De vorige stappen hebt gemaakt van een virtueel netwerk en subnet, NIC's die zijn gekoppeld en vervolgens een virtuele machine gemaakt. Een openbare IP-adres en regels voor netwerkbeveiligingsgroepen die SSH-verkeer toestaan zijn niet gemaakt. Voor meer informatie over het configureren van het gastbesturingssysteem te installeren voor meerdere NIC's die u wilt toestaan van externe verbindingen en lokaal uitvoeren van opdrachten op de virtuele machine.

Als u wilt toestaan dat SSH-verkeer, maakt u een netwerkbeveiligingsgroepregel met [az network nsg-regel maken](/cli/azure/network/nsg/rule#az-network-nsg-rule-create) als volgt:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name allow_ssh \
    --priority 101 \
    --destination-port-ranges 22
```

Maken van een openbaar IP-adres met [az network public-ip maken](/cli/azure/network/public-ip#az-network-public-ip-create) en wijs deze toe aan de eerste NIC met [az network nic ip-config update](/cli/azure/network/nic/ip-config#az-network-nic-ip-config-update):

```azurecli
az network public-ip-address create --resource-group myResourceGroup --name myPublicIP

az network nic ip-config update \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --name ipconfig1 \
    --public-ip-addres myPublicIP
```

Als u de weergave openbare IP-adres van de virtuele machine, gebruikt u [az vm show](/cli/azure/vm#az-vm-show) als volgt:

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Nu SSH naar het openbare IP-adres van uw virtuele machine. De standaardgebruikersnaam die is opgegeven in de vorige stap is *azureuser*. Geef uw eigen gebruikersnaam en het openbare IP-adres:

```bash
ssh azureuser@137.117.58.232
```

Als u wilt verzenden naar of van een secundaire netwerkinterface, moet u handmatig permanente routes toevoegen aan het besturingssysteem voor elke secundaire netwerkinterface. In dit artikel *eth1* is de secundaire-interface. Instructies voor het permanente routes toevoegen aan het besturingssysteem zijn afhankelijk van distributie. Raadpleeg de documentatie bij uw distributie voor instructies.

Als de route wordt toegevoegd aan het besturingssysteem, het gatewayadres is *.1* voor elk subnet de netwerkinterface zich bevindt. Bijvoorbeeld, als het adres is toegewezen aan de network interface *10.0.2.4*, de gateway u opgeeft voor de route is *10.0.2.1*. U kunt definiëren van een bepaald netwerk voor het doel van de route of geef een bestemming van *0.0.0.0*, als u wilt dat al het verkeer voor de interface voor de opgegeven gateway doorlopen. De gateway voor elk subnet wordt beheerd door het virtuele netwerk.

Als u de route voor een secundaire netwerkinterface hebt toegevoegd, Controleer of de route is in de routetabel, met `route -n`. De volgende voorbeelduitvoer wordt voor de routetabel waarin die de twee netwerkinterfaces die zijn toegevoegd aan de virtuele machine in dit artikel is:

```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.0.1.1        0.0.0.0         UG    0      0        0 eth0
0.0.0.0         10.0.2.1        0.0.0.0         UG    0      0        0 eth1
10.0.1.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0
10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 eth1
168.63.129.16   10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
169.254.169.254 10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
```

Bevestig dat de route die u hebt toegevoegd clusterverbinding blijven tussen opnieuw wordt opgestart behouden door het controleren van de routetabel opnieuw na het opnieuw opstarten. Connectiviteit wilt testen, kunt u bijvoorbeeld de volgende opdracht, invoeren, waarbij *eth1* is de naam van een secundaire netwerkinterface:

```bash
ping bing.com -c 4 -I eth1
```

## <a name="next-steps"></a>Volgende stappen
Beoordeling [Linux VM-grootten](sizes.md) bij het maken van een virtuele machine met meerdere NIC's. Let op het maximum aantal NIC's die ondersteuning biedt voor elke VM-grootte.

Voor nog betere beveiliging uw virtuele machines, gebruiken just-in-time-VM-toegang. Deze functie wordt geopend regels voor netwerkbeveiligingsgroepen voor SSH-verkeer wanneer dat nodig is, en voor een gedefinieerde periode. Zie [Manage virtual machine access using just in time](../../security-center/security-center-just-in-time.md) (VM-toegang beheren met behulp van JIT) voor meer informatie.

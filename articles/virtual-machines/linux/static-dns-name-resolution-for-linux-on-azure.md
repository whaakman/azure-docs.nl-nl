---
title: Interne DNS gebruiken voor VM-naamomzetting met de Azure CLI | Microsoft Docs
description: Het virtueel netwerk maken netwerkinterfacekaarten en interne DNS gebruiken voor VM-naamomzetting in Azure met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: acfdd9070b49805c20b8ef921b5387c151448aa1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961498"
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Maken van virtuele netwerkinterfacekaarten en interne DNS gebruiken voor VM-naamomzetting in Azure

In dit artikel laat zien hoe statische interne DNS-namen voor virtuele Linux-machines met behulp van de virtuele netwerkinterfacekaarten (vNics) en de namen van de DNS-label met de Azure CLI instellen. Statische DNS-namen worden gebruikt voor permanente infrastructuurservices zoals een Jenkins build-server, die wordt gebruikt voor dit document, of een Git-server.

De vereisten zijn:

* [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)
* [bestanden voor openbare en persoonlijke SSH-sleutels](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Snelle opdrachten
Als u snel uitvoeren van de taak wilt, de volgende sectie wordt beschreven de opdrachten die nodig zijn. Meer gedetailleerde informatie en -context voor elke stap u in de rest van het document vindt [Hier beginnen](#detailed-walkthrough). Als u wilt deze stappen uitvoert, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

Randvoorwaarden voor: De resourcegroep, virtueel netwerk en subnet, Netwerkbeveiligingsgroep met SSH binnenkomende.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Maken van een virtuele netwerkinterfacekaart met een statische interne DNS-naam
Maken van de vNic met [az network nic maken](/cli/azure/network/nic#az_network_nic_create). De `--internal-dns-name` CLI-vlag is voor het instellen van het DNS-label, waarmee u de statische DNS-naam voor de virtuele netwerkinterfacekaart (vNic). Het volgende voorbeeld wordt een vNic met de naam `myNic`, verbindt u deze naar de `myVnet` virtueel netwerk, en maakt een interne DNS-naam-record met de naam `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a>Een virtuele machine implementeren en verbinding maken met de vNic
Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). De `--nics` vlag maakt de vNic verbinding met de virtuele machine tijdens de implementatie naar Azure. Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` met Azure Managed Disks en wordt de vNic met de naam `myNic` uit de vorige stap:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

Een volledige continue integratie en continue implementatie (CiCd)-infrastructuur op Azure vereist dat bepaalde servers statisch of lange levensduur hebben servers. Het verdient aanbeveling dat Azure-activa, zoals de virtuele netwerken en Netwerkbeveiligingsgroepen statisch zijn en resources die zelden worden geïmplementeerd langer bewaard moeten blijven. Zodra een virtueel netwerk is geïmplementeerd, kan deze worden gebruikt door nieuwe implementaties zonder eventuele nadelige is van invloed op de infrastructuur. U kunt later een Git-opslagplaats-server toevoegen of een Jenkins-server voor automatisering CiCd levert aan dit virtuele netwerk voor uw ontwikkelings- of testomgevingen.  

Interne DNS-namen zijn alleen omgezet in een Azure-netwerk. Omdat de DNS-namen interne zijn, zijn ze niet worden opgelost als het internet, bieden van bijkomende beveiliging aan de infrastructuur.

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten `myResourceGroup`, `myNic`, en `myVM`.

## <a name="create-the-resource-group"></a>De resourcegroep maken
Maak eerst de resourcegroep met [az-groep maken](/cli/azure/group#az_group_create). Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westus` locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

De volgende stap is het bouwen van een virtueel netwerk om te starten van de virtuele machines in. Het virtuele netwerk bevat één subnet voor dit scenario. Zie voor meer informatie over virtuele netwerken van Azure, [maken van een virtueel netwerk](../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). 

Maken van het virtuele netwerk met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create). Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` en subnet met de naam `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a>De Netwerkbeveiligingsgroep maken
Azure Network Security Groups komen overeen met een firewall op het niveau van het netwerk. Zie voor meer informatie over Netwerkbeveiligingsgroepen [nsg's maken in de Azure CLI](../../virtual-network/tutorial-filter-network-traffic-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Maken van de netwerkbeveiligingsgroep met [az network nsg maken](/cli/azure/network/nsg#az_network_nsg_create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a>Voeg een binnenkomende regel zodat SSH
Voeg een binnenkomende regel voor de netwerkbeveiligingsgroep met [az network nsg-regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Het volgende voorbeeld wordt een regel met de naam `myRuleAllowSSH`:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-the-subnet-with-the-network-security-group"></a>Het subnet koppelen aan de Netwerkbeveiligingsgroep
Als u wilt het subnet koppelen aan de Netwerkbeveiligingsgroep, gebruikt u [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). Het volgende voorbeeld wordt gekoppeld aan de naam van het subnet `mySubnet` met de Netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a>Maak de virtuele netwerkinterfacekaart en statische DNS-namen
Azure is zeer flexibel, maar voor het gebruik van DNS-namen voor VM-naamomzetting, moet u netwerkinterfacekaarten (vNics) met een DNS-label voor virtueel netwerk maken. vnic's zijn belangrijk als u opnieuw kan worden gebruikt door deze verbinding maken met verschillende virtuele machines gedurende de levenscyclus van infrastructuur. Deze aanpak blijft de vNic als statische resource terwijl de virtuele machines kunnen tijdelijk zijn. Met behulp van DNS-labels op de vNic, kunnen we om in te schakelen van eenvoudige naamomzetting van andere virtuele machines in het VNet. Andere VM's toegang tot de automatiseringsserver door de DNS-naam met omgezette namen kan `Jenkins` of de Git-server als `gitrepo`.  

Maken van de vNic met [az network nic maken](/cli/azure/network/nic#az_network_nic_create). Het volgende voorbeeld wordt een vNic met de naam `myNic`, verbindt u deze naar de `myVnet` virtueel netwerk met de naam `myVnet`, en maakt een interne DNS-naam-record met de naam `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>De virtuele machine implementeren in de infrastructuur van het virtuele netwerk
We hebben nu een virtueel netwerk en subnet, een Netwerkbeveiligingsgroep die fungeert als een firewall blokkeert al het inkomende verkeer met uitzondering van poort 22 voor SSH en een vNic voor het beveiligen van onze subnet. U kunt nu een virtuele machine binnen deze bestaande netwerkinfrastructuur implementeren.

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` met Azure Managed Disks en wordt de vNic met de naam `myNic` uit de vorige stap:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Met behulp van de CLI-vlaggen om aan te roepen uit bestaande resources, geven we Azure de VM in de bestaande netwerk te implementeren. Nogmaals, als een VNet en subnet is geïmplementeerd, kunnen ze worden behouden als statische of permanente resources in uw Azure-regio.  

## <a name="next-steps"></a>Volgende stappen
* [Rechtstreeks uw eigen aangepaste omgeving maken voor een virtuele Linux-machine met Azure CLI-opdrachten](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-VM maken op Azure met behulp van sjablonen](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

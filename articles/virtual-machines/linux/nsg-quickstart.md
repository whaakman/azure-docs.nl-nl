---
title: Openen van poorten op een Linux-VM met Azure CLI | Microsoft Docs
description: Leer hoe u een poort openen / maken van een eindpunt voor uw Linux-VM met behulp van het Azure resource manager-implementatiemodel en Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 4244520fa30fece53e1d01e50044cfeca496066c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973334"
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Poorten en eindpunten openen voor een Linux-VM met de Azure CLI

U een poort openen of maken van een eindpunt dat aan een virtuele machine (VM) in Azure met het maken van een netwerk-filter op een subnet of VM-netwerkinterface. U plaatst deze filters, die binnenkomend en uitgaand verkeer worden beheerd, op een Netwerkbeveiligingsgroep die is gekoppeld aan de resource die het verkeer ontvangt. We gebruiken een algemeen voorbeeld van webverkeer op poort 80. In dit artikel wordt beschreven hoe u een poort openen voor een virtuele machine met de Azure CLI. 


Het maken van een Netwerkbeveiligingsgroep en regels moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten *myResourceGroup*, *myNetworkSecurityGroup*, en *myVnet*.


## <a name="quickly-open-a-port-for-a-vm"></a>Snel een poort voor een virtuele machine openen
Als u snel een poort openen voor een virtuele machine in een scenario voor het ontwikkelen en testen wilt, kunt u de [az vm open-port](/cli/azure/vm#az_vm_open_port) opdracht. Met deze opdracht maakt u een Netwerkbeveiligingsgroep, voegt een regel en toegepast op een virtuele machine of het subnet. Het volgende voorbeeld wordt poort geopend *80* op de virtuele machine met de naam *myVM* in de resourcegroep met de naam *myResourceGroup*.

```azure-cli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Doorgaan met de extra stappen in dit artikel voor meer controle over de regels, bijvoorbeeld voor het definiëren van een bron IP-adresbereik.


## <a name="create-a-network-security-group-and-rules"></a>Een Netwerkbeveiligingsgroep en regels maken
Maken van de netwerkbeveiligingsgroep met [az network nsg maken](/cli/azure/network/nsg#az_network_nsg_create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* in de *eastus* locatie:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Toevoegen van een regel met [az network nsg-regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) naar HTTP-verkeer naar uw webserver (of aanpassen voor uw eigen scenario, zoals SSH-toegang of database connectivity). Het volgende voorbeeld wordt een regel met de naam *myNetworkSecurityGroupRule* TCP-verkeer op poort 80 toe te staan:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>Netwerkbeveiligingsgroep van toepassing op virtuele machine
De Netwerkbeveiligingsgroep koppelen aan van de virtuele machine-netwerkinterface (NIC) met [az network nic update](/cli/azure/network/nic#az_network_nic_update). Het volgende voorbeeld wordt gekoppeld aan een bestaande NIC met de naam *myNic* met de Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

U kunt ook uw Netwerkbeveiligingsgroep koppelen aan een virtueel netwerksubnet met [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) in plaats van alleen aan de netwerkinterface op een enkele virtuele machine. Het volgende voorbeeld wordt gekoppeld aan een bestaand subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk met de Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunnen u aan de slag met de verkeersstroom naar uw virtuele machine. Netwerkbeveiligingsgroepen bevatten veel geweldige functies en granulariteit voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier het maken van een Netwerkbeveiligingsgroep en ACL-regels](tutorial-virtual-network.md#secure-network-traffic).

Voor maximaal beschikbare webtoepassingen, moet u uw virtuele machines achter een Azure Load Balancer plaatsen. De load balancer verdeelt het verkeer naar virtuele machines met een Netwerkbeveiligingsgroep waarmee verkeer filteren. Zie voor meer informatie, [laden saldo van Linux virtuele machines in Azure om te maken van een maximaal beschikbare toepassing](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. U vindt meer informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is een netwerkbeveiligingsgroep (NSG)?](../../virtual-network/security-overview.md)

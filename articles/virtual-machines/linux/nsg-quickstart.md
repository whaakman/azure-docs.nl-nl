---
title: Openen van poorten voor een Linux-VM met Azure CLI 2.0 | Microsoft Docs
description: Meer informatie over het openen van een poort / maken van een eindpunt voor uw Linux-VM met de Azure resource manager-implementatiemodel en Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: f02e9d7c794217824bb2b0fa965d1d581ec94003
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Open poorten en eindpunten voor een Linux-VM met de Azure CLI
U opent een poort of een eindpunt met een virtuele machine (VM) in Azure maken met het maken van een netwerk-filter op een subnet of een VM-netwerkinterface. U kunt deze filters die binnenkomend en uitgaand verkeer worden beheerd, plaatsen op een Netwerkbeveiligingsgroep gekoppeld aan de resource die het verkeer ontvangt. We gebruiken een voorbeeld van webverkeer op poort 80. In dit artikel leest u hoe een poort voor een virtuele machine met de Azure CLI 2.0 te openen. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](nsg-quickstart-nodejs.md).

Maken van een Netwerkbeveiligingsgroep en regels moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup*, *myNetworkSecurityGroup*, en *myVnet*.


## <a name="quickly-open-a-port-for-a-vm"></a>Snel opent een poort voor een virtuele machine
Als u openen snel een poort voor een virtuele machine in een scenario ontwikkelen en testen wilt, kunt u de [az vm open poort](/cli/azure/vm#az_vm_open_port) opdracht. Met deze opdracht een Netwerkbeveiligingsgroep maakt, voegt een regel en toegepast op een virtuele machine of een subnet. Het volgende voorbeeld wordt poort geopend *80* op de virtuele machine met de naam *myVM* in de resourcegroep met de naam *myResourceGroup*.

```azure-cli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Doorgaan met de overige stappen in dit artikel voor meer controle over de regels, zoals het definiëren van een bron IP-adresbereik.


## <a name="create-a-network-security-group-and-rules"></a>Een Netwerkbeveiligingsgroep en regels maken
Maken van de netwerkbeveiligingsgroep met [az netwerk nsg maken](/cli/azure/network/nsg#az_network_nsg_create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* in de *eastus* locatie:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Toevoegen van een regel met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) toestaan HTTP-verkeer naar uw webserver (of aanpassen voor uw eigen scenario, zoals SSH-toegang of database connectivity). Het volgende voorbeeld wordt een regel met naam *myNetworkSecurityGroupRule* TCP-verkeer op poort 80 toestaan:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>Netwerkbeveiligingsgroep toepassen op VM
De Netwerkbeveiligingsgroep koppelen aan uw VM-netwerkinterface (NIC) met [az netwerk nic update](/cli/azure/network/nic#az_network_nic_update). Het volgende voorbeeld wordt gekoppeld aan een bestaande NIC met de naam *myNic* met de netwerk-beveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

U kunt ook uw Netwerkbeveiligingsgroep koppelen aan een virtueel netwerksubnet met [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) in plaats van alleen aan de netwerkinterface op een enkele virtuele machine. Het volgende voorbeeld wordt een bestaand subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk met de Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunt u aan de slag te kunnen met verkeer naar uw virtuele machine. Netwerkbeveiligingsgroepen bieden veel handige functies en granulatie voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier maken van een Netwerkbeveiligingsgroep en ACL regels](tutorial-virtual-network.md#secure-network-traffic).

Voor maximaal beschikbare webtoepassingen, moet u uw virtuele machines achter een Load Balancer van Azure plaatsen. De load balancer wordt verkeer naar virtuele machines met een Netwerkbeveiligingsgroep waarmee wordt verkeer gefilterd. Zie voor meer informatie [het laden van Linux virtuele machines in Azure maken van een maximaal beschikbare toepassing in evenwicht](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is een netwerkbeveiligingsgroep (NSG)?](../../virtual-network/virtual-networks-nsg.md)

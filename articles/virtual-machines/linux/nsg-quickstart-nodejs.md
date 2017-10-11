---
title: Open poorten voor een Linux-VM met Azure CLI 1.0 | Microsoft Docs
description: Meer informatie over het openen van een poort / maken van een eindpunt voor uw Linux-VM met behulp van het implementatiemodel van Azure resource manager en de Azure CLI 1.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 847bc76c37ed929851712ba1c12463a01032e267
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure-using-the-azure-cli-10"></a>Openen van poorten en eindpunten voor een Linux-VM in Azure met behulp van de Azure CLI 1.0
U opent een poort of een eindpunt met een virtuele machine (VM) in Azure maken met het maken van een netwerk-filter op een subnet of een VM-netwerkinterface. U kunt deze filters die binnenkomend en uitgaand verkeer worden beheerd, plaatsen op een Netwerkbeveiligingsgroep gekoppeld aan de resource die het verkeer ontvangt. We gebruiken een voorbeeld van webverkeer op poort 80. In dit artikel leest u hoe een poort voor een virtuele machine moet worden geopend met behulp van de Azure CLI 1.0.


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](nsg-quickstart.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten
Maken van een Netwerkbeveiligingsgroep en regels u moet [de Azure CLI 1.0](../../cli-install-nodejs.md) geïnstalleerd en met Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *myNetworkSecurityGroup*, en *myVnet*.

Maak de beveiligingsgroep van uw netwerk, invoeren van uw eigen namen en de locatie op de juiste wijze. Het volgende voorbeeld wordt een Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* in de *eastus* locatie:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Een regel voor het toestaan van HTTP-verkeer naar uw webserver (of aanpassen voor uw eigen scenario, zoals SSH-toegang of database connectivity) toevoegen. Het volgende voorbeeld wordt een regel met naam *myNetworkSecurityGroupRule* TCP-verkeer op poort 80 toestaan:

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

De Netwerkbeveiligingsgroep koppelen aan uw VM-netwerkinterface (NIC). Het volgende voorbeeld wordt gekoppeld aan een bestaande NIC met de naam *myNic* met de netwerk-beveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

U kunt ook uw Netwerkbeveiligingsgroep koppelen met een virtueel netwerksubnet in plaats van alleen aan de netwerkinterface op een enkele virtuele machine. Het volgende voorbeeld wordt een bestaand subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk met de Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunt u aan de slag te kunnen met verkeer naar uw virtuele machine. Netwerkbeveiligingsgroepen bieden veel handige functies en granulatie voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier maken van een Netwerkbeveiligingsgroep en ACL regels](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).

U kunt Netwerkbeveiligingsgroepen en ACL-regels definiëren als onderdeel van Azure Resource Manager-sjablonen. Lees meer over [Netwerkbeveiligingsgroepen maken met behulp van sjablonen](../../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruiken poort-doorsturen wilt naar een unieke externe poort toewijzen aan een interne poort op de virtuele machine, gebruikt u een load balancer en Network Address Translation (NAT)-regels. U wilt bijvoorbeeld TCP-poort 8080 extern beschikbaar en hebt verkeer op TCP-poort 80 op een virtuele machine wordt gestuurd. U kunt meer informatie over [maken van een Internet gerichte load balancer](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is een netwerkbeveiligingsgroep (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Overzicht van Azure Resource Manager voor Load Balancers](../../load-balancer/load-balancer-arm.md)


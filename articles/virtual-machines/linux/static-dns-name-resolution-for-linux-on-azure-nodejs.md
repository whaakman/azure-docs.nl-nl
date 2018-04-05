---
title: Met behulp van de interne DNS voor naamomzetting van de virtuele machine in Azure | Microsoft Docs
description: Interne DNS gebruiken voor naamomzetting van de virtuele machine in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: 6072374cd7a37df6d7cc75479e24996dff3ee667
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a>Met behulp van de interne DNS voor naamomzetting van de virtuele machine in Azure

In dit artikel laat zien hoe ingesteld statische interne DNS-namen voor virtuele Linux-machines met virtuele NIC kaarten (VNic) en DNS-labelnamen. Statische DNS-namen worden gebruikt voor permanente infrastructuurservices zoals een Jenkins build-server, die wordt gebruikt voor dit document of een Git-server.

De vereisten zijn:

* [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)
* [bestanden voor openbare en persoonlijke SSH-sleutels](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten

Als u de taak snel uitvoeren moet, wordt de volgende sectie de opdrachten die nodig zijn. Meer gedetailleerde informatie en context voor elke stap u de rest van het document vindt [vanaf hier](#detailed-walkthrough).  

Randvoorwaarden voor: Resourcegroep, VNet, NSG met SSH inkomende Subnet.

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a>Een VNic maken met een statische interne DNS-naam

De `-r` cli-vlag is voor het instellen van de DNS-label, waardoor de statische DNS-naam voor de VNic.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-the-vm-into-the-vnet-nsg-and-connect-the-vnic"></a>Implementeer de virtuele machine in het VNet, NSG en verbinding maken met de vnic van.

De `-N` de VNic verbindt met de nieuwe virtuele machine tijdens de implementatie naar Azure.

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

Een volledige continue integratie en continue implementatie (CiCd) infrastructuur in Azure bepaalde servers statisch of lange levensduur hebben servers vereist.  Het verdient aanbeveling dat Azure activa op de virtuele netwerken (vnet's) en de Netwerkbeveiligingsgroepen (nsg's), mag niet statisch en resources die zelden zijn geïmplementeerd langer bewaard moeten blijven.  Zodra een VNet is geïmplementeerd, kan dit opnieuw gebruikt door nieuwe implementaties zonder een ongewenst is van invloed op de infrastructuur.  Uw ontwikkeling of tests omgevingen biedt toevoegen aan deze statische netwerk een Git-opslagplaats-server en een automatiseringsserver Jenkins CiCd.  

Interne DNS-namen zijn alleen omgezet in een Azure-netwerk.  Omdat de DNS-namen interne zijn, zijn ze niet worden omgezet naar het internet, bieden van bijkomende beveiliging aan de infrastructuur.

_Vervang geen voorbeelden door uw eigen namen._

## <a name="create-the-resource-group"></a>De resourcegroep maken

Een resourcegroep is nodig voor het ordenen van alles wat die er in dit scenario worden gemaakt.  Zie voor meer informatie over Azure-resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-the-vnet"></a>Het VNet maken

De eerste stap is het bouwen van een VNet Start de virtuele machines in.  Het VNet bevat één subnet voor dit scenario.  Zie voor meer informatie over Azure VNets [een virtueel netwerk maken](../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-the-nsg"></a>Maken van de NSG

Het Subnet wordt achter een bestaande Netwerkbeveiligingsgroep toegevoegd, dus gaan we verder met het NSG voordat het Subnet.  Azure nsg's zijn equivalent aan een firewall op de netwerklaag.  Zie voor meer informatie over Azure nsg's [het nsg's maken in de Azure CLI](../../virtual-network/tutorial-filter-network-traffic-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Regel voor geven van een binnenkomende SSH toevoegen

De Linux-VM moet toegang via het internet, zodat een regel binnenkomende poort 22-verkeer via het netwerk worden doorgegeven aan poort 22 op de Linux-VM nodig is.

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-to-the-vnet"></a>Voeg een subnet toe aan het VNet

Virtuele machines binnen het VNet moeten zich bevinden in een subnet.  Elke VNet kan meerdere subnetten hebben.  Maken van het subnet en het subnet koppelen aan het NSG aan een firewall toevoegen aan het subnet.

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

Het Subnet is nu toegevoegd in het VNet en die zijn gekoppeld aan de NSG en de NSG-regel.

## <a name="creating-static-dns-names"></a>Statische DNS-namen maken

Azure is zeer flexibel, maar voor het gebruik van DNS-namen voor naamomzetting voor virtuele machines, moet u ze als virtueel-netwerkkaarten (VNics) met behulp van DNS-labels maken.  VNics zijn belangrijk omdat u ze hergebruiken kunt door deze te verbinden met andere virtuele machines, blijft de VNic als statische resource terwijl de virtuele machines kunnen tijdelijk zijn.  Met behulp van DNS labels op de VNic, kunnen we eenvoudig naamomzetting van andere VM's in het VNet inschakelen.  Andere VM's toegang tot de automatiseringsserver door de DNS-naam met behulp van omgezette namen kan `Jenkins` of de Git-server als `gitrepo`.  Een VNic maken en deze koppelen aan het Subnet in de vorige stap hebt gemaakt.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Implementeer de virtuele machine in het VNet en het NSG

We hebben nu een VNet, een subnet in dit VNet en een NSG die fungeert als een firewall blokkeert al het binnenkomende verkeer behalve poort 22 voor SSH voor het beveiligen van onze subnet.  De virtuele machine kan nu worden geïmplementeerd in deze bestaande netwerkinfrastructuur.

Met de Azure CLI, en de `azure vm create` opdracht, de Linux-VM wordt geïmplementeerd op de bestaande Azure-resourcegroep, VNet, Subnet en VNic.  Zie voor meer informatie over een volledige virtuele machine implementeren met behulp van de CLI [een volledige Linux-omgeving maken met behulp van de Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

Met behulp van de vlaggen CLI aan te roepen bestaande bronnen, zodat we Azure voor het implementeren van de virtuele machine binnen de bestaande netwerk.  Om aan te wijzen er nogmaals zodra een VNet en het subnet zijn geïmplementeerd, kunnen ze blijven als statisch of permanente resources binnen uw Azure-regio.  

## <a name="next-steps"></a>Volgende stappen
* [Rechtstreeks uw eigen aangepaste omgeving maken voor een virtuele Linux-machine met Azure CLI-opdrachten](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Linux-VM in Azure met behulp van sjablonen maken](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

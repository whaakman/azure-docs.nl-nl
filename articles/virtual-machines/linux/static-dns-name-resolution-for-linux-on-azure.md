---
title: Interne DNS gebruiken voor naamomzetting van de virtuele machine met de Azure CLI 2.0 | Microsoft Docs
description: Het maken van virtueel netwerk netwerkinterfacekaarten en interne DNS gebruiken voor naamomzetting van de virtuele machine in Azure met de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: 992920adb1ae3736d43cc5f0bbb2081a20a1674d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Maken van virtuele netwerkinterfacekaarten en interne DNS gebruiken voor naamomzetting van de virtuele machine in Azure
Dit artikel ziet u het instellen van statische interne DNS-namen voor virtuele Linux-machines met virtuele netwerkinterfacekaarten (vNics) en DNS-labelnamen met de Azure CLI 2.0. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Statische DNS-namen worden gebruikt voor permanente infrastructuurservices zoals een Jenkins build-server, die wordt gebruikt voor dit document of een Git-server.

De vereisten zijn:

* [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)
* [bestanden voor openbare en persoonlijke SSH-sleutels](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Snelle opdrachten
Als u de taak snel uitvoeren moet, wordt de volgende sectie de opdrachten die nodig zijn. Meer gedetailleerde informatie en context voor elke stap u in de rest van het document vindt [vanaf hier](#detailed-walkthrough). Als u wilt deze stappen uitvoert, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

Randvoorwaarden voor: Resourcegroep, virtueel netwerk en subnet, Netwerkbeveiligingsgroep met SSH voor binnenkomend verkeer.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Maken van een virtuele netwerkadapter met een statische interne DNS-naam
Maken van de vNic met [az netwerk nic maken](/cli/azure/network/nic#create). De `--internal-dns-name` CLI-vlag is voor het instellen van de DNS-label, waardoor de statische DNS-naam voor de virtuele netwerkinterfacekaart (vNic). Het volgende voorbeeld wordt een vNic met de naam `myNic`, verbonden aan de `myVnet` virtueel netwerk, en maakt een interne DNS-naam-record genoemd `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a>Een virtuele machine implementeren en verbinding maken met de vnic van.
Maak een VM met [az vm create](/cli/azure/vm#create). De `--nics` vlag de vNic verbindt met de virtuele machine tijdens de implementatie naar Azure. Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` met Azure beheerd schijven en wordt de vNic met de naam `myNic` uit de vorige stap:

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

Een volledige continue integratie en continue implementatie (CiCd) infrastructuur in Azure bepaalde servers statisch of lange levensduur hebben servers vereist. Het verdient aanbeveling dat Azure activa op de virtuele netwerken en Netwerkbeveiligingsgroepen statisch zijn en resources die zelden zijn geïmplementeerd langer bewaard moeten blijven. Zodra een virtueel netwerk is geïmplementeerd, kan dit opnieuw gebruikt door nieuwe implementaties zonder een ongewenst is van invloed op de infrastructuur. U kunt later een Git-opslagplaatsserver toevoegen of een automatiseringsserver Jenkins CiCd levert aan dit virtuele netwerk voor de ontwikkeling of testomgevingen.  

Interne DNS-namen zijn alleen omgezet in een Azure-netwerk. Omdat de DNS-namen interne zijn, zijn ze niet worden omgezet naar het internet, bieden van bijkomende beveiliging aan de infrastructuur.

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `myNic`, en `myVM`.

## <a name="create-the-resource-group"></a>De resourcegroep maken
Maak eerst de resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westus` locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

De volgende stap is het bouwen van een virtueel netwerk voor het starten van de virtuele machines in. Het virtuele netwerk bevat één subnet voor dit scenario. Zie voor meer informatie over virtuele netwerken van Azure [een virtueel netwerk maken met behulp van de Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Maken van het virtuele netwerk met [az network vnet maken](/cli/azure/network/vnet#create). Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` en subnet met de naam `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a>De Netwerkbeveiligingsgroep maken
Beveiligingsgroepen voor Azure-netwerk zijn equivalent aan een firewall op de netwerklaag. Zie voor meer informatie over Netwerkbeveiligingsgroepen [het nsg's maken in de Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Maken van de netwerkbeveiligingsgroep met [az netwerk nsg maken](/cli/azure/network/nsg#create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a>Een inkomende regel om toe te staan SSH toevoegen
Toevoegen van een inkomende regel voor de netwerkbeveiligingsgroep met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#create). Het volgende voorbeeld wordt een regel met naam `myRuleAllowSSH`:

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
Gebruiken om het subnet koppelen aan de Netwerkbeveiligingsgroep, [az network vnet subnet update](/cli/azure/network/vnet/subnet#update). Het volgende voorbeeld wordt de subnetnaam `mySubnet` met de netwerk-beveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a>De virtuele netwerkinterfacekaart en statische DNS-namen maken
Azure is zeer flexibel, maar voor het gebruik van DNS-namen voor naamomzetting van de virtuele machine, moet u netwerkinterfacekaarten (vNics) met een DNS-label voor virtueel netwerk maken. vNics zijn belangrijk omdat u ze hergebruiken kunt door deze te verbinden met andere virtuele machines gedurende de levenscyclus van de infrastructuur. Deze aanpak houdt de vNic als statische resource terwijl de virtuele machines kunnen tijdelijk zijn. Met behulp van DNS labels op de vNic, kunnen we eenvoudig naamomzetting van andere VM's in het VNet inschakelen. Andere VM's toegang tot de automatiseringsserver door de DNS-naam met behulp van omgezette namen kan `Jenkins` of de Git-server als `gitrepo`.  

Maken van de vNic met [az netwerk nic maken](/cli/azure/network/nic#create). Het volgende voorbeeld wordt een vNic met de naam `myNic`, verbonden aan de `myVnet` virtueel netwerk met de naam `myVnet`, en maakt een interne DNS-naam-record genoemd `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Implementeer de virtuele machine in de infrastructuur van het virtuele netwerk
We hebben nu een virtueel netwerk en subnet, een Netwerkbeveiligingsgroep fungeert als een firewall blokkeert al het binnenkomende verkeer behalve poort 22 voor SSH en een vNic voor het beveiligen van onze subnet. U kunt nu een virtuele machine binnen deze bestaande netwerkinfrastructuur implementeren.

Maak een VM met [az vm create](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` met Azure beheerd schijven en wordt de vNic met de naam `myNic` uit de vorige stap:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Met behulp van de vlaggen CLI aan te roepen bestaande bronnen, zodat we Azure voor het implementeren van de virtuele machine binnen de bestaande netwerk. Om aan te wijzen er nogmaals zodra een VNet en het subnet zijn geïmplementeerd, kunnen ze blijven als statisch of permanente resources binnen uw Azure-regio.  

## <a name="next-steps"></a>Volgende stappen
* [Rechtstreeks uw eigen aangepaste omgeving maken voor een virtuele Linux-machine met Azure CLI-opdrachten](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Linux-VM in Azure met behulp van sjablonen maken](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

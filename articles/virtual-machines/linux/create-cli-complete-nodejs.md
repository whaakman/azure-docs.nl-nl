---
title: Een volledige Linux-omgeving maken met de klassieke Azure-CLI | Microsoft Docs
description: Opslag, een Linux-VM, een virtueel netwerk en subnet, een load balancer, een NIC, een openbaar IP-adres en een netwerkbeveiligingsgroep, allemaal vanaf de grond met behulp van de klassieke Azure-CLI maken.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: cynthn
ms.openlocfilehash: d0a38defe41ea7c4e0da32cb73cf2bd73fd80950
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52498212"
---
# <a name="create-a-complete-linux-environment-with-the-azure-classic-cli"></a>Een volledige Linux-omgeving maken met de klassieke Azure-CLI
In dit artikel maken we een eenvoudig netwerk met een load balancer en een combinatie van VM's die nuttig voor de ontwikkeling en eenvoudig computing zijn. We helpen bij de opdracht door de opdracht uit, totdat u twee werkdagen, beveiligde virtuele Linux-machines waarmee u verbinding hebt van een willekeurige plaats op het Internet maken kan. Vervolgens kunt u op verplaatsen naar meer complexe netwerken en omgevingen.

Gaandeweg u meer informatie over de afhankelijkheidshiërarchie van dat het implementatiemodel van Resource Manager u kunt, en over hoeveel inschakelen. Nadat u hoe het systeem is gemaakt ziet, kunt u opnieuw bouwen deze veel sneller met behulp van [Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ook nadat u meer wilt hoe de onderdelen van uw omgeving bij elkaar passen weten, aanzienlijk het maken van sjablonen voor het automatiseren van deze eenvoudiger.

De omgeving bevat:

* Twee virtuele machines in een beschikbaarheidsset.
* Een load balancer met een taakverdelingsregel op poort 80.
* (NSG) regels voor netwerkbeveiligingsgroepen op uw VM te beveiligen tegen ongewenste verkeer.

Als u wilt deze aangepaste omgeving maken, moet u de meest recente [klassieke Azure-CLI](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in Resource Manager-modus (`azure config mode arm`). U moet ook een hulpprogramma voor het parseren JSON. In dit voorbeeld wordt [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI voor klassieke](#quick-commands) : onze CLI voor het klassieke en resource Manager-implementatiemodel (dit artikel)
- [Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) : onze CLI de volgende generatie voor het resource Manager-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie gegevens snel de base-opdrachten voor het uploaden van een virtuele machine naar Azure. Meer gedetailleerde informatie en -context voor elke stap vindt u in de rest van het document, te beginnen [hier](#detailed-walkthrough).

Zorg ervoor dat u hebt [de klassieke Azure-CLI](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aangemeld en met behulp van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten `myResourceGroup`, `mystorageaccount`, en `myVM`.

Maak de resourcegroep. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

Controleer of de resourcegroep met behulp van de JSON-parser:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

De storage-account maken. Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`. (Naam van het opslagaccount moet uniek zijn, zodat uw eigen unieke naam opgeven.)

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Controleer of het opslagaccount met behulp van de JSON-parser:

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Maak het virtuele netwerk. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet`:

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Maak een subnet. Het volgende voorbeeld wordt een subnet met de naam `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Controleer of het virtuele netwerk en subnet met behulp van de JSON-parser:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Maak een openbaar IP-adres. Het volgende voorbeeld wordt een openbaar IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns`. (De DNS-naam moet uniek zijn, zodat uw eigen unieke naam opgeven.)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Maak de load balancer. Het volgende voorbeeld wordt een load balancer met de naam `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Maak een front-end-IP-adresgroep voor de load balancer, en koppel het openbare IP-adres. Het volgende voorbeeld wordt een front-end-IP-adresgroep met de naam `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Maak de back-end-IP-adresgroep voor de load balancer. Het volgende voorbeeld wordt een back-end-IP-adrespool met de naam `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Maak SSH inkomende netwerk regels van address translation (NAT) voor de load balancer. Het volgende voorbeeld maakt u twee load balancer-regels, `myLoadBalancerRuleSSH1` en `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Maken van het web inkomende NAT-regels voor de load balancer. Het volgende voorbeeld wordt een load balancer-regel met de naam `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

De load balancer-statustest maken. Het volgende voorbeeld wordt een TCP-test met de naam `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Controleer of de load balancer, IP-adresgroepen en NAT-regels met behulp van de JSON-parser:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Maak de eerste netwerkinterfacekaart (NIC). Vervang de `#####-###-###` secties door uw eigen Azure-abonnement-ID. Uw abonnement ID wordt vermeld in de uitvoer van **jq** wanneer u de resources die u maakt. U kunt ook uw abonnements-ID met weergeven `azure account list`.

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Maken van de tweede NIC Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Controleer of de twee NIC's met behulp van de JSON-parser:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

De netwerkbeveiligingsgroep maken. Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Voeg twee regels voor binnenkomende verbindingen voor de netwerkbeveiligingsgroep. Het volgende voorbeeld maakt u twee regels `myNetworkSecurityGroupRuleSSH` en `myNetworkSecurityGroupRuleHTTP`:

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Controleer of de netwerkbeveiligingsgroep en de regels voor binnenkomende verbindingen met behulp van de JSON-parser:

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

De netwerkbeveiligingsgroep koppelen aan de twee NIC's:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

De beschikbaarheidsset maken. Het volgende voorbeeld wordt een beschikbaarheidsset met de naam `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

De eerste Linux-VM maken. Het volgende voorbeeld wordt een virtuele machine met de naam `myVM1`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

De tweede Linux-VM maken. Het volgende voorbeeld wordt een virtuele machine met de naam `myVM2`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Gebruik de JSON-parser om te controleren of alles dat is gemaakt:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Uw nieuwe omgeving exporteren naar een sjabloon voor het snel nieuwe exemplaren opnieuw te maken:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
De gedetailleerde stappen die volgen wordt uitgelegd wat elke opdracht doet tijdens het ontwikkelen van uw omgeving. Deze begrippen zijn nuttig wanneer u uw eigen aangepaste omgevingen voor ontwikkeling of productie bouwen.

Zorg ervoor dat u hebt [de klassieke Azure-CLI](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aangemeld en met behulp van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten `myResourceGroup`, `mystorageaccount`, en `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Resourcegroepen maken en implementatie locaties kiezen
Azure-resourcegroepen zijn logische implementatie-entiteiten die bevatten configuratie-informatie en de metagegevens van het inschakelen van de logische beheer van resource-implementaties. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`:

```azurecli
azure group create --name myResourceGroup --location westeurope
```

Uitvoer:

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Create a storage account
Storage-accounts moet u voor uw VM-schijven en voor eventuele extra gegevensschijven die u wilt toevoegen. U kunt storage-accounts maken bijna onmiddellijk na het maken van resourcegroepen.

Hier gebruiken we de `azure storage account create` opdracht, en het type opslagondersteuning dat u wilt doorgeven van de locatie van het account, de resourcegroep die besturingselementen. Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`:

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Uitvoer:

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Het onderzoeken van de resourcegroep met behulp van de `azure group show` opdracht, gebruiken we de [jq](https://stedolan.github.io/jq/) hulpprogramma samen met de `--json` Azure CLI-optie. (U kunt **jsawk** of elke taal-bibliotheek die u liever de JSON parseren.)

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Voor het onderzoeken van het opslagaccount met behulp van de CLI, moet u eerst de accountnamen en sleutels instellen. Vervang de naam van het opslagaccount in het volgende voorbeeld met een naam die u kiest:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Vervolgens kunt u uw storage-gegevens eenvoudig bekijken:

```azurecli
azure storage container list
```

Uitvoer:

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken
Vervolgens gaat u moet maken van een virtueel netwerk die wordt uitgevoerd in Azure en een subnet waarin u uw virtuele machines kunt maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` met de `192.168.0.0/16` adresvoorvoegsel:

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

Uitvoer:

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Nogmaals, gebruiken we de mogelijkheid om--json `azure group show` en `jq` om te zien hoe we onze resources bouwen. We hebben nu een `storageAccounts` resource en een `virtualNetworks` resource.  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nu gaan we maken een subnet in de `myVnet` virtuele netwerk waarin de virtuele machines zijn geïmplementeerd. We gebruiken de `azure network vnet subnet create` opdracht, samen met de resources die we al hebt gemaakt: de `myResourceGroup` resourcegroep en de `myVnet` virtueel netwerk. In het volgende voorbeeld voegen we het subnet met de naam `mySubnet` met het adresvoorvoegsel subnet van `192.168.1.0/24`:

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Uitvoer:

```azurecli
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Omdat het subnet logisch binnen het virtuele netwerk, zoeken we de informatie over het subnet met een iets andere opdracht. De opdracht die we gebruiken is `azure network vnet show`, maar we blijven de JSON-uitvoer onderzoeken met behulp van `jq`.

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Uitvoer:

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken
Nu gaan we maken het openbare IP-adres (PIP) dat we aan uw load balancer toewijzen. Hiermee kunt u verbinding maken met uw VM's via Internet met behulp van de `azure network public-ip create` opdracht. Omdat het standaardadres dynamisch is, maken we een benoemde DNS-vermelding in de **cloudapp.azure.com** domein met behulp van de `--domain-name-label` optie. Het volgende voorbeeld wordt een openbaar IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns`. Omdat de DNS-naam moet uniek zijn, kunt u uw eigen unieke DNS-naam opgeven:

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Uitvoer:

```azurecli
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Het openbare IP-adres is ook een resource op het hoogste niveau, zodat u met zien kunt `azure group show`.

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Uitvoer:

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

U kunt meer Resourcedetails, met inbegrip van de volledig gekwalificeerde domeinnaam (FQDN) van het subdomein, met behulp van de volledige onderzoeken `azure network public-ip show` opdracht. Het openbare IP-adresresource is toegewezen logisch, maar is nog niet een specifiek adres toegewezen. Als u een IP-adres, gaat u moet een load balancer, die we nog geen hebt gemaakt.

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Uitvoer:

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Een load balancer en IP-adresgroepen maken
Wanneer u een load balancer maakt, kunt u verkeer verdelen over meerdere virtuele machines. Het biedt redundantie voor uw toepassingen ook door het uitvoeren van meerdere virtuele machines die op aanvragen van gebruikers in het geval van onderhoud of zware belastingen reageren. Het volgende voorbeeld wordt een load balancer met de naam `myLoadBalancer`:

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Uitvoer:

```azurecli
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Onze load balancer is redelijk leeg, dus gaan we enkele IP-adresgroepen maken. We willen maken van twee IP-adresgroepen voor de load balancer, één voor de front-end en een voor de back-end. De front-end-IP-adresgroep is zichtbaar voor iedereen. Het is ook de locatie waarop we de PIP dat we eerder hebben gemaakt toewijzen. Vervolgens gebruiken we de back-end-adrespool als een locatie voor onze VM's verbinding maken met. Op die manier kunnen door de load balancer met de VM's kan het verkeer stromen.

Eerst gaan we onze front-end-IP-adresgroep maken. Het volgende voorbeeld wordt een front-end-pool met de naam `myFrontEndPool`:

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

Uitvoer:

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Houd er rekening mee hoe we gebruikt de `--public-ip-name` switch om door te geven de `myPublicIP` die we eerder hebben gemaakt. Het openbare IP-adres toewijzen aan de load balancer, kunt u uw VM's via Internet te bereiken.

Vervolgens gaan we onze tweede IP-adresgroep maken voor onze back-end-verkeer. Het volgende voorbeeld wordt een back-end-adrespool met de naam `myBackEndPool`:

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Uitvoer:

```azurecli
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

We zien hoe het gaat met de load balancer door te kijken met `azure network lb show` en onderzoek van de JSON-uitvoer:

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Uitvoer:

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>NAT-regels voor load balancer maken
Als u verkeer via onze load balancer, moeten we netwerkadres translation (NAT) om regels te maken die binnenkomend of uitgaand acties opgeven. U kunt zelf het protocol bepalen om te gebruiken en vervolgens de externe poorten worden toegewezen aan de interne poorten naar wens. Voor onze omgeving gaan we enkele regels waarmee SSH via onze load balancer aan onze VM's maken. We instellen TCP-poorten 4222 en 4223 om te leiden naar TCP-poort 22 op onze VM's (die we later maken). Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleSSH1` TCP-poort 4222 toewijzen aan poort 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Uitvoer:

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Herhaal de procedure voor de tweede NAT-regel voor SSH. Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleSSH2` TCP-poort 4223 toewijzen aan poort 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

We gaan ook de slag gaan en een NAT-regel voor TCP-poort 80 voor internetverkeer, ten slotte verbindt met de regel tot onze IP-adresgroepen. Als we de regel voor een IP-adresgroep, in plaats van het installeren van de regel aan onze VM's afzonderlijk, koppelt kunnen we toevoegen of verwijderen van virtuele machines van de IP-adresgroep. De load balancer wordt automatisch aangepast voor de stroom van verkeer. Het volgende voorbeeld wordt een regel met de naam `myLoadBalancerRuleWeb` op TCP-poort 80 worden toegewezen aan poort 80:

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Uitvoer:

```azurecli
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Een load balancer-statustest maken
Een health test periodieke controles op de virtuele machines die zich achter de load balancer om te controleren of ze werken en reageren op aanvragen, zoals gedefinieerd. Als dat niet het geval is, moet u ze zijn verwijderd uit de bewerking om ervoor te zorgen dat gebruikers worden niet omgeleid naar deze. U kunt aangepaste controles voor de statustest, samen met intervallen en time-outwaarden definiëren. Zie voor meer informatie over statuscontroles [Load Balancer controleert](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het volgende voorbeeld wordt een TCP health dat wordt aangeduid met de naam `myHealthProbe`:

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Uitvoer:

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

We hebben opgegeven hier, een interval van 15 seconden voor onze statuscontroles. We kunnen missen maximaal vier tests (één minuut) voordat de load balancer van mening is dat de host niet meer werkt.

## <a name="verify-the-load-balancer"></a>Controleer of de load balancer
Nu wordt de load balancer-configuratie uitgevoerd. Hier volgen de stappen die u hebt gemaakt:

1. U hebt een load balancer gemaakt.
2. U een front-end-IP-adresgroep gemaakt en een openbaar IP-adres toegewezen.
3. Hebt u een back-end-IP-adresgroep die VM's verbinding met maken kunnen gemaakt.
4. U NAT-regels waarmee SSH naar de virtuele machines voor beheer, samen met een regel waarmee TCP-poort 80 voor onze web-app gemaakt.
5. U hebt toegevoegd een statustest om periodiek te controleren de virtuele machines. Deze statustest zorgt ervoor dat gebruikers geen probeert te krijgen tot een virtuele machine die is niet meer werkt of inhoud.

Laten we bekijken hoe de load balancer eruitziet nu:

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Uitvoer:

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Maak een NIC voor gebruik met de Linux-VM
NIC's zijn via een programma beschikbaar omdat u regels toepassen op het gebruik ervan kunt. U kunt ook meer dan één hebben. In de volgende `azure network nic create` opdracht, u koppelt de NIC bij het laden back-end IP-adresgroep en koppel deze aan de NAT-regel om de SSH-verkeer toestaan.

Vervang de `#####-###-###` secties door uw eigen Azure-abonnement-ID. Uw abonnement ID wordt vermeld in de uitvoer van `jq` wanneer u de resources die u maakt. U kunt ook uw abonnements-ID met weergeven `azure account list`.

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Uitvoer:

```azurecli
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

U kunt de details zien door rechtstreeks onderzoek van de resource. Bestuderen van de resource met behulp van de `azure network nic show` opdracht:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Uitvoer:

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Nu maken we de tweede NIC, ten slotte verbindt met bij onze back-end-IP-adresgroep opnieuw. Deze tijd de tweede NAT-regel worden SSH-verkeer toestaat. Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Een netwerkbeveiligingsgroep en regels maken
Nu maken we een netwerkbeveiligingsgroep en de regels voor binnenkomende verbindingen die toegang tot de NIC. bieden Een netwerkbeveiligingsgroep kan worden toegepast op een NIC of subnet. U definieert regels voor het beheren van de verkeersstroom in en uit uw virtuele machines. Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

We gaan de binnenkomende regel voor de NSG aan het toestaan van binnenkomende verbindingen op poort 22 (tot en met ondersteuning voor SSH) toevoegen. Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRuleSSH` om TCP op poort 22 te staan:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nu gaan we de binnenkomende regel voor de NSG aan het toestaan van binnenkomende verbindingen op poort 80 (voor ondersteuning webverkeer) toevoegen. Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRuleHTTP` om TCP op poort 80 te staan:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> De binnenkomende regel is een filter voor binnenkomende netwerkverbindingen. In dit voorbeeld koppelen we de NSG op de virtuele NIC van virtuele machines, wat betekent dat elke aanvraag naar poort 22 wordt doorgegeven aan de NIC op de virtuele machine. Met deze binnenkomende regel over een netwerkverbinding is en niet over een eindpunt dat is wat het normaal zou zijn over in klassieke implementaties. Als u wilt een poort opent, moet u laten de `--source-port-range` ingesteld op '\*' (de standaardwaarde) om te accepteren van binnenkomende aanvragen van **eventuele** poort aanvraagt. Poorten zijn doorgaans dynamisch zijn.
>
>

## <a name="bind-to-the-nic"></a>Verbinding maken met de NIC
De NSG aan de NIC's koppelen. We moeten onze NIC's verbonden aan de netwerkbeveiligingsgroep. Voer beide opdrachten, aansluiten van onze NIC's:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken
Beschikbaarheidssets help verspreid uw VM's over foutdomeinen en upgradedomeinen. We maken een beschikbaarheidsset voor uw VM's. Het volgende voorbeeld wordt een beschikbaarheidsset met de naam `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Domeinen met fouten definiëren een groepering van virtuele machines die een gemeenschappelijke voedingsbron en netwerkswitch delen. Standaard worden de virtuele machines die zijn geconfigureerd in uw beschikbaarheidsset verdeeld over maximaal drie foutdomeinen. Het idee is een hardwareprobleem in een van deze domeinen met fouten heeft geen invloed op elke virtuele machine waarop uw app wordt uitgevoerd. Azure automatisch virtuele machines wordt verspreid over de foutdomeinen wanneer ze in een beschikbaarheidsset worden geplaatst.

Upgradedomeinen geven aan groepen van virtuele machines en de onderliggende fysieke hardware die op hetzelfde moment opnieuw kan worden opgestart. De volgorde waarin upgradedomeinen opnieuw zijn opgestart niet opeenvolgende kan zijn tijdens gepland onderhoud, maar slechts één upgrade tegelijk opnieuw is opgestart. Nogmaals, Azure automatisch verdeeld over uw virtuele machines upgradedomeinen wanneer ze in een site beschikbaarheid worden geplaatst.

Meer informatie over [beheren van de beschikbaarheid van virtuele machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-the-linux-vms"></a>De Linux-VM's maken
U kunt de opslag- en netwerkresources ter ondersteuning van via Internet toegankelijke VM's hebt gemaakt. Nu gaan we die virtuele machines maken en ze zijn beveiligd met een SSH-sleutel die geen een wachtwoord. In dit geval gaan we om te maken van een Ubuntu VM op basis van de meest recente LTS. Wij vinden dat informatie over de installatiekopie met behulp van `azure vm image list`, zoals beschreven in [Azure VM-installatiekopieën zoeken](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

We een installatiekopie hebt geselecteerd met behulp van de opdracht `azure vm image list westeurope canonical | grep LTS`. In dit geval gebruiken we `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Voor het laatste veld, geven we `latest` zodat in de toekomst we altijd kunnen genieten van de meest recente build. (De tekenreeks die we gebruiken is `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Deze stap is bekend bij iedereen die al is gemaakt door een ssh rsa-sleutel voor openbare en persoonlijke koppelen in Linux of Mac met behulp van **ssh-keygen - t rsa -b 2048**. Als u geen een certificaat-sleutelparen in uw `~/.ssh` directory, kunt u deze maken:

* Automatisch met behulp van de `azure vm create --generate-ssh-keys` optie.
* Handmatig met behulp van [de instructies voor het zelf maken](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt ook kunt u de `--admin-password` methode voor het verifiëren van uw SSH-verbindingen nadat de virtuele machine is gemaakt. Deze methode is doorgaans minder veilig.

We de virtuele machine maken met onze resources en informatie samen met de `azure vm create` opdracht:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Uitvoer:

```azurecli
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

U kunt direct verbinding met uw virtuele machine met behulp van de standaard-SSH-sleutels. Zorg ervoor dat u de juiste poort opgeeft, aangezien we via de load balancer geven. (Voor onze eerste virtuele machine, we instellen van de NAT-regel voor het doorsturen van poort 4222 naar onze VM.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Uitvoer:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

De slag gaan en de tweede virtuele machine op dezelfde manier:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

En u kunt nu de `azure vm show myResourceGroup myVM1` opdracht om te onderzoeken wat u hebt gemaakt. U voert op dit moment uw Ubuntu-VM's achter een load balancer in Azure die u met uw SSH-sleutelpaar aanmelden kunt (omdat wachtwoorden zijn uitgeschakeld). U kunt nginx of httpd installeren, een web-app implementeren en het verkeer stroom via de load balancer op beide van de virtuele machines.

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

Uitvoer:

```azurecli
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>De omgeving als een sjabloon exporteren
Nu dat u hebt gemaakt uit deze omgeving, wat gebeurt er als u wilt maken van een extra ontwikkelomgeving met dezelfde parameters, of een productie-omgeving die overeenkomt met het? Resource Manager JSON-sjablonen die de parameters voor uw omgeving definieert gebruikt. U bouwen volledige omgevingen door te verwijzen naar deze JSON-sjabloon. U kunt [handmatig JSON-sjablonen samenstellen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of een bestaande omgeving voor het maken van de JSON-sjabloon voor u te exporteren:

```azurecli
azure group export --name myResourceGroup
```

Deze opdracht maakt u de `myResourceGroup.json` bestand in uw huidige werkmap. Wanneer u een omgeving met deze sjabloon maakt, wordt u gevraagd voor alle de resourcenamen, met inbegrip van de namen voor de load balancer, netwerkinterfaces of virtuele machines. U kunt deze namen vullen in het sjabloonbestand door toe te voegen de `-p` of `--includeParameterDefaultValue` parameter voor de `azure group export` opdracht die eerder is aangegeven. Bewerk uw JSON-sjabloon om op te geven van de resourcenamen van de of [maken van een bestand parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die Hiermee geeft u de resourcenamen.

Een omgeving maken met de sjabloon:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

U wilt lezen [meer over het implementeren van sjablonen](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Meer informatie over het incrementeel bijwerken omgevingen, het parameterbestand gebruiken en toegang tot sjablonen vanaf één locatie.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om te werken met meerdere netwerkonderdelen en virtuele machines. Deze Voorbeeldomgeving kunt u uw toepassing bouwen met behulp van de belangrijkste onderdelen die zijn geïntroduceerd hier.

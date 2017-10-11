---
title: Een volledige Linux-omgeving maken met de Azure CLI 1.0 | Microsoft Docs
description: Maken van opslag, een Linux-VM, een virtueel netwerk en subnet, een load balancer, een NIC, een openbare IP-adres en een netwerkbeveiligingsgroep via een compleet nieuwe met behulp van de Azure CLI 1.0.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 201ccd523e49d638ace50fbc0ffdceb705b35473
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-environment-with-the-azure-cli-10"></a>Een volledige Linux-omgeving maken met de Azure CLI 1.0
In dit artikel verder gaan we met een eenvoudig netwerk met een combinatie van virtuele machines die gebruikt voor ontwikkeling en eenvoudige computing worden en een load balancer. We doorlopen het proces door opdracht totdat u twee werkt, beveiligde virtuele Linux-machines waarmee u verbinding hebt van een willekeurige plaats op het Internet maken kunt. Vervolgens kunt u op verplaatsen naar complexe netwerken en omgevingen.

Langs de manier u meer informatie over de afhankelijkheidshiërarchie of het implementatiemodel van Resource Manager u biedt en over hoeveel inschakelen biedt. Nadat u hoe het systeem is gemaakt ziet, kunt u opnieuw bouwen het veel sneller met behulp van [Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nadat u meer wilt hoe de onderdelen van uw omgeving bij elkaar passen weten, maken van sjablonen voor het automatiseren van hen wordt ook eenvoudiger.

De omgeving bevat:

* Twee virtuele machines in een beschikbaarheidsset.
* Een load balancer met een regel voor load balancing op poort 80.
* (NSG) netwerkbeveiligingsgroepen uw virtuele machine te beveiligen tegen ongewenste verkeer.

Als u wilt deze aangepaste omgeving maken, moet u de meest recente [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) in de modus Resource Manager (`azure config mode arm`). U moet ook een JSON hulpprogramma parseren. In dit voorbeeld wordt [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie details snel de base-opdrachten voor het uploaden van een virtuele machine in Azure. Meer gedetailleerde informatie en context voor elke stap vindt u in de rest van het document, te beginnen [hier](#detailed-walkthrough).

Zorg ervoor dat u hebt [de Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aangemeld en het gebruik van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `mystorageaccount`, en `myVM`.

De resourcegroep maken. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westeurope` locatie:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

Controleer of de resourcegroep met behulp van de JSON-parser:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Het opslagaccount maken. Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`. (Naam van het opslagaccount moet uniek zijn, zodat uw eigen unieke naam opgeven.)

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

Een subnet maken. Het volgende voorbeeld wordt een subnet met de naam `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Controleer of het virtuele netwerk en subnet met behulp van de JSON-parser:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Maak een openbare IP-adres. Het volgende voorbeeld wordt een openbaar IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns`. (De DNS-naam moet uniek zijn, zodat uw eigen unieke naam opgeven.)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

De load balancer maken. Het volgende voorbeeld wordt een load balancer met de naam `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Maak een front-end-IP-adresgroep voor de load balancer, en koppel het openbare IP-adres. Het volgende voorbeeld wordt een front-end-IP-adresgroep met de naam `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Maak de back-end-IP-adresgroep voor de load balancer. Het volgende voorbeeld wordt een back-end-IP-adresgroep met de naam `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

SSH inkomende network address translation (NAT) regels voor de load balancer maken. Het volgende voorbeeld maakt u twee regels van load balancer, `myLoadBalancerRuleSSH1` en `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Maken van het web inkomende NAT-regels voor de load balancer. Het volgende voorbeeld wordt een regel voor load balancer met de naam `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

De load balancer-test health maken. Het volgende voorbeeld wordt een TCP-test met de naam `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Controleer of de load balancer, IP-adresgroepen en NAT-regels met behulp van de JSON-parser:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

De eerste netwerkinterfacekaart (NIC) maken. Vervang de `#####-###-###` secties met uw eigen Azure-abonnement-ID. Uw abonnement ID wordt vermeld in de uitvoer van **jq** wanneer u de resources die u maakt. U kunt ook uw abonnements-ID met weergeven `azure account list`.

Het volgende voorbeeld wordt een NIC met de naam `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Maken van de tweede netwerkadapter. Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

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

Twee regels voor binnenkomende verbindingen voor de netwerkbeveiligingsgroep toevoegen. Het volgende voorbeeld maakt u twee regels `myNetworkSecurityGroupRuleSSH` en `myNetworkSecurityGroupRuleHTTP`:

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

De netwerkbeveiligingsgroep binden aan de twee NIC's:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

De beschikbaarheidsset maken. Het volgende voorbeeld wordt een benoemde beschikbaarheidsset `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

De eerste Linux VM maken. Het volgende voorbeeld wordt een virtuele machine met de naam `myVM1`:

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

De tweede Linux VM maken. Het volgende voorbeeld wordt een virtuele machine met de naam `myVM2`:

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

Gebruik de JSON-parser om te controleren of alles die is gemaakt:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Uw nieuwe omgeving exporteren naar een sjabloon om snel nieuwe exemplaren opnieuw te maken:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
De gedetailleerde stappen volgen wordt uitgelegd wat elke opdracht doet tijdens het samenstellen van buiten uw omgeving. Deze begrippen zijn handig als u uw eigen aangepaste omgevingen voor ontwikkeling of productie bouwen.

Zorg ervoor dat u hebt [de Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aangemeld en het gebruik van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `mystorageaccount`, en `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Maken van resourcegroepen en locaties implementatie kiezen
Azure-resourcegroepen zijn logische implementatie entiteiten die bevatten informatie over de configuratie en metagegevens inschakelen van de logische beheer van de resource-implementaties. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westeurope` locatie:

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

## <a name="create-a-storage-account"></a>Een opslagaccount maken
Storage-accounts moet u voor uw VM-schijven en voor eventuele extra gegevensschijven die u wilt toevoegen. U kunt storage-accounts maken bijna onmiddellijk na het maken van resourcegroepen.

Hier gebruiken we de `azure storage account create` opdracht, en het type opslagondersteuning die u wilt dat de locatie van het account, de resourcegroep die wordt doorgegeven bepaalt. Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`:

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

Onze resourcegroep controleren met behulp van de `azure group show` opdracht, gebruiken we de [jq](https://stedolan.github.io/jq/) hulpprogramma samen met de `--json` Azure CLI-optie. (U kunt **jsawk** of elke gewenste parseren van de JSON taal-bibliotheek.)

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

Voor het onderzoeken van het opslagaccount met behulp van de CLI, moet u eerst de accountnamen en sleutels ingesteld. Vervang de naam van het opslagaccount in het volgende voorbeeld met een naam die u kiest:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

U kunt vervolgens uw storage-gegevens eenvoudig bekijken:

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

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en subnet maken
Vervolgens gaat u moet maken van een virtueel netwerk in Azure en een subnet van uw virtuele machines maken worden uitgevoerd. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet` met de `192.168.0.0/16` adresvoorvoegsel:

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

Opnieuw, Hiermee kunt u de optie--json gebruiken `azure group show` en `jq` om te zien hoe we onze bronnen bouwen. Nu een `storageAccounts` resource en een `virtualNetworks` resource.  

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

Nu gaan we maken een subnet in de `myVnet` virtueel netwerk waarin de virtuele machines zijn geïmplementeerd. We gebruiken de `azure network vnet subnet create` opdracht samen met de resources die we al hebt gemaakt: de `myResourceGroup` resourcegroep en de `myVnet` virtueel netwerk. In het volgende voorbeeld voegen we het subnet met de naam `mySubnet` met het adres subnetvoorvoegsel van `192.168.1.0/24`:

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

Omdat het subnet logisch binnen het virtuele netwerk, zoeken we de informatie over het subnet met een iets andere opdracht. De opdracht we gebruiken is `azure network vnet show`, maar we verder te onderzoeken van de JSON-uitvoer met behulp van `jq`.

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
Nu gaan we het openbare IP-adres (PIP) die we aan de load balancer toewijzen maken. Hiermee kunt u verbinding maken met uw virtuele machines van het Internet met behulp van de `azure network public-ip create` opdracht. Omdat het standaardadres dynamisch is, maken we een benoemde DNS-vermelding in de **cloudapp.azure.com** domein met behulp van de `--domain-name-label` optie. Het volgende voorbeeld wordt een openbaar IP-adres met de naam `myPublicIP` met de DNS-naam van `mypublicdns`. Omdat de DNS-naam moet uniek zijn, kunt u uw eigen unieke DNS-naam opgeven:

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

Het openbare IP-adres is ook een bron op het hoogste niveau, zodat u deze met ziet `azure group show`.

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

U kunt meer informatie over resource, zoals de volledig gekwalificeerde domeinnaam (FQDN) van het subdomein, met behulp van de volledige onderzoeken `azure network public-ip show` opdracht. Het openbare IP-adres resource logisch is toegewezen, maar een specifiek adres nog niet is toegewezen. Als u een IP-adres, gaat u moet een load balancer die we nog geen hebt gemaakt.

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
Wanneer u een load balancer maakt, kunt u verkeer verdelen over meerdere virtuele machines. Het biedt redundantie voor uw toepassingen ook door het uitvoeren van meerdere virtuele machines die op aanvragen van gebruikers in het geval van onderhoud of zware belasting reageren. Het volgende voorbeeld wordt een load balancer met de naam `myLoadBalancer`:

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

De load balancer is redelijk leeg, dus gaan we een aantal IP-adresgroepen maken. We willen maken van twee IP-adresgroepen voor de load balancer, één voor de front-end en één voor de back-end. De front-end-IP-adresgroep is openbaar zichtbaar. Het is ook de locatie waarop we de PIP die we eerder hebben gemaakt toewijzen. Vervolgens gebruiken we de back-end-pool als locatie voor de virtuele machines verbinding maken met. Op die manier het verkeer door de load balancer naar de virtuele machines kan stromen.

Eerst laten we onze front-end-IP-adresgroep maken. Het volgende voorbeeld wordt een front-groep met de naam `myFrontEndPool`:

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

Houd er rekening mee hoe we gebruikt de `--public-ip-name` switch door te geven de `myPublicIP` die we eerder hebben gemaakt. Het openbare IP-adres toewijzen aan de load balancer, kunt u uw virtuele machines worden bereikt via Internet.

Vervolgens maken we onze tweede IP-adresgroep voor de back-end-verkeer. Het volgende voorbeeld wordt een back-end-pool met de naam `myBackEndPool`:

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

We zien hoe u onze load balancer doet door te zoeken met `azure network lb show` en onderzoeken van de JSON-uitvoer:

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

## <a name="create-load-balancer-nat-rules"></a>NAT-regels van load balancer maken
Als u verkeer via de load balancer, moeten we network address translation (NAT) regels maken die binnenkomend of uitgaand acties opgeven. U kunt het te gebruiken protocol opgeven en vervolgens externe poorten naar interne poorten desgewenst toewijzen. Voor onze omgeving gaan we een aantal regels maken die SSH toestaan via onze load balancer naar de virtuele machines. We instellen TCP-poorten 4222 en 4223 om om te leiden TCP-poort 22 op onze virtuele machines (die we later maken). Het volgende voorbeeld wordt een regel met naam `myLoadBalancerRuleSSH1` TCP-poort 4222 toewijzen aan poort 22:

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

Herhaal de procedure voor de tweede NAT-regel voor SSH. Het volgende voorbeeld wordt een regel met naam `myLoadBalancerRuleSSH2` TCP-poort 4223 toewijzen aan poort 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

We gaan ook opwekken en maak een NAT-regel voor TCP-poort 80 voor internetverkeer, de regel aansluiting tot onze IP-adresgroepen. Als we de regel moet een IP-adresgroep, in plaats van een afzonderlijk, een regel aan onze VMs aansluiting aansluiten kunt we toevoegen of verwijderen van virtuele machines van de IP-adresgroep. De load balancer wordt automatisch de verkeersstroom aangepast. Het volgende voorbeeld wordt een regel met naam `myLoadBalancerRuleWeb` TCP-poort 80 om toe te wijzen poort 80:

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

## <a name="create-a-load-balancer-health-probe"></a>Een load balancer health test maken
Een health test regelmatig controles op de virtuele machines die zich achter de load balancer om te controleren of ze zijn het functioneren en reageren op aanvragen, zoals is gedefinieerd. Als dat niet het geval is, moet u ze zijn verwijderd uit de bewerking om ervoor te zorgen dat gebruikers worden niet omgeleid naar deze. U kunt aangepaste controles voor de health-test, samen met intervallen en time-outwaarden definiëren. Zie voor meer informatie over statuscontroles [Load Balancer-tests](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het volgende voorbeeld wordt een TCP health aangeduid met de naam `myHealthProbe`:

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

Hier wordt een interval van 15 seconden voor onze statuscontroles opgegeven. We kunnen maximaal vier tests (één minuut) voordat de load balancer van oordeel is dat de host niet meer werkt over het hoofd ziet.

## <a name="verify-the-load-balancer"></a>Controleer of de load balancer
Nu wordt de load balancer-configuratie gedaan. Hier volgen de stappen die u hebt gemaakt:

1. U hebt gemaakt voor een load balancer.
2. U een front-end-IP-adresgroep gemaakt en een openbare IP-adres toegewezen.
3. U een back-end-IP-adresgroep die virtuele machines verbinding met maken kunnen gemaakt.
4. U NAT-regels waarmee SSH kunt uitvoeren naar de virtuele machines voor beheer, samen met een regel waarmee u TCP-poort 80 voor de web-app hebt gemaakt.
5. U hebt een health test geregeld wordt gecontroleerd of de virtuele machines toegevoegd. Deze test health zorgt ervoor dat gebruikers niet probeert te krijgen tot een virtuele machine die niet meer werkt of inhoud.

We bekijken wat de load balancer er nu uit:

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

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Maken van een NIC voor gebruik met de Linux-VM
NIC's zijn programmatisch beschikbaar omdat u regels toepassen op het gebruik ervan kunt. U kunt ook meer dan één hebben. In de volgende `azure network nic create` opdracht u aansluiten op de NIC de belasting backend-IP-adresgroep en deze koppelen aan de NAT-regel SSH-verkeer toestaan.

Vervang de `#####-###-###` secties met uw eigen Azure-abonnement-ID. Uw abonnement ID wordt vermeld in de uitvoer van `jq` wanneer u de resources die u maakt. U kunt ook uw abonnements-ID met weergeven `azure account list`.

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

U ziet de details vindt u rechtstreeks van de resource. Bestuderen van de resource met behulp van de `azure network nic show` opdracht:

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

Nu maken we de tweede Netwerkinterfacekaart, in aansluiting bij onze backend-IP-adresgroep opnieuw. Deze tijd de tweede NAT-regel toestaat SSH verkeer. Het volgende voorbeeld wordt een NIC met de naam `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Een netwerkbeveiligingsgroep en regels maken
We maken nu een netwerkbeveiligingsgroep en de regels voor binnenkomende verbindingen die voor de toegang tot de NIC. Een netwerkbeveiligingsgroep kan worden toegepast op een NIC of een subnet. Definieert u regels voor het beheren van de stroom van verkeer van en naar uw virtuele machines. Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Laten we de inkomende regel voor het NSG aan het toestaan van binnenkomende verbindingen op poort 22 (voor ondersteuning van SSH) toevoegen. Het volgende voorbeeld wordt een regel met naam `myNetworkSecurityGroupRuleSSH` om TCP op poort 22 te staan:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nu gaan we de binnenkomende regel voor het NSG aan het toestaan van binnenkomende verbindingen op poort 80 (voor ondersteuning webverkeer) toevoegen. Het volgende voorbeeld wordt een regel met naam `myNetworkSecurityGroupRuleHTTP` om TCP op poort 80 te staan:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> De binnenkomende regel is een filter naar binnenkomende netwerkverbindingen. In dit voorbeeld binden we het NSG aan de virtuele NIC van virtuele machines, wat betekent dat elk verzoek poort 22 wordt doorgegeven aan de NIC op de virtuele machine. Deze regel binnenkomende over een netwerkverbinding is en niet over een eindpunt met dit is wat het normaal zou zijn over in klassieke implementaties. Als u wilt een poort opent, moet u laat de `--source-port-range` ingesteld op '\*' (de standaardwaarde) te accepteren van binnenkomende aanvragen van **eventuele** poort aanvraagt. Poorten zijn meestal dynamisch zijn.
>
>

## <a name="bind-to-the-nic"></a>Verbinding maken met de NIC
Het NSG binden aan de NIC's. Moeten we onze NIC's een verbinding maakt met onze netwerkbeveiligingsgroep. Voer beide opdrachten, koppelt u beide onze NIC's:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken
Beschikbaarheidssets help verspreiding uw virtuele machines in domeinen met fouten en upgradedomeinen. Laten we de beschikbaarheidsset voor uw virtuele machines maken. Het volgende voorbeeld wordt een benoemde beschikbaarheidsset `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Domeinen met fouten definiëren een groepering van virtuele machines die een gemeenschappelijk power-bron- en switch delen. Standaard worden de virtuele machines die worden geconfigureerd in de beschikbaarheidsset gescheiden over maximaal drie domeinen met fouten. Het idee is een hardwareprobleem in een van deze domeinen met fouten heeft geen invloed op elke virtuele machine die uw app wordt uitgevoerd. Virtuele machines verdeelt Azure automatisch over de domeinen met fouten wanneer ze worden geplaatst in een beschikbaarheidsset.

Upgradedomeinen duiden op groepen van virtuele machines en de onderliggende fysieke hardware die op hetzelfde moment kan worden opgestart. De volgorde waarin upgradedomeinen worden opgestart mogelijk geen opeenvolgende tijdens gepland onderhoud, maar slechts één upgrade opnieuw wordt opgestart tegelijk. Opnieuw verdeelt Azure automatisch uw virtuele machines over upgradedomeinen wanneer ze worden geplaatst in een site beschikbaarheid.

Lees meer over [het beheren van de beschikbaarheid van virtuele machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-the-linux-vms"></a>De virtuele Linux-machines maken
U kunt de opslag- en netwerkbronnen ter ondersteuning van Internet toegankelijke VM's hebt gemaakt. Nu gaan we die virtuele machines maken en ze zijn beveiligd met een SSH-sleutel die geen wachtwoord. In dit geval gaan we een Ubuntu VM op basis van de meest recente TNS maken. We vinden die informatie van de installatiekopie met behulp van `azure vm image list`, zoals beschreven in [Azure VM-installatiekopieën vinden](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

We een installatiekopie van een geselecteerd met de opdracht `azure vm image list westeurope canonical | grep LTS`. In dit geval gebruiken we `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Voor het laatste veld, geven we `latest` zodat in de toekomst kunnen we de meest recente build altijd beschikken. (De tekenreeks we gebruiken `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Deze stap is vertrouwd voor iedereen die al is gemaakt door een ssh rsa openbare en persoonlijke sleutel koppelen op Linux- of Mac via **ssh-keygen - t rsa -b 2048**. Als u nog geen sleutelparen die een certificaat uw `~/.ssh` directory, kunt u deze maken:

* Automatisch, met behulp van de `azure vm create --generate-ssh-keys` optie.
* Handmatig met behulp van [de instructies voor het zelf maken](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt ook de `--admin-password` methode voor het verifiëren van uw SSH-verbindingen nadat de virtuele machine is gemaakt. Deze methode is meestal minder goed beveiligd.

We de virtuele machine maken door brengen onze bronnen en informatie samen met de `azure vm create` opdracht:

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

U kunt verbinding maken met uw virtuele machine onmiddellijk met behulp van uw standaard SSH-sleutels. Zorg ervoor dat u de juiste poort opgeeft omdat we via de load balancer geven. (Voor onze eerste VM we instellen de NAT-regel voor het doorsturen van poort 4222 voor onze VM.)

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

Opwekken en maak uw tweede virtuele machine op dezelfde manier:

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

En u kunt nu de `azure vm show myResourceGroup myVM1` opdracht om te onderzoeken wat u hebt gemaakt. U uitvoert op dit moment uw Ubuntu VM's achter een load balancer in Azure die u aanmelden kunt bij alleen met uw SSH-sleutelpaar (omdat wachtwoorden zijn uitgeschakeld). U kunt nginx of httpd installeren, een web-app implementeren en het verkeer verloopt via de load balancer op beide van de virtuele machines.

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


## <a name="export-the-environment-as-a-template"></a>De omgeving exporteren als een sjabloon
Nu dat u hebt gemaakt uit deze omgeving, wat gebeurt er als u wilt maken van een extra development environment met dezelfde parameters of een productie-omgeving die overeenkomt met het? Resource Manager gebruikt de JSON-sjablonen op dat de parameters voor uw omgeving definiëren. U maken uit de volledige omgeving door te verwijzen naar deze JSON-sjabloon. U kunt [handmatig JSON-sjablonen samenstellen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of een bestaande omgeving voor het maken van het JSON-sjabloon voor u te exporteren:

```azurecli
azure group export --name myResourceGroup
```

Deze opdracht maakt u de `myResourceGroup.json` bestand in de huidige werkmap. Wanneer u een omgeving met deze sjabloon maakt, wordt u gevraagd alle de resourcenamen, met inbegrip van de namen voor de load balancer, netwerkinterfaces of virtuele machines. U kunt deze namen in uw sjabloonbestand vullen door toe te voegen de `-p` of `--includeParameterDefaultValue` -parameter voor de `azure group export` opdracht die eerder is aangegeven. Bewerk uw JSON-sjabloon om op te geven van de resourcenamen van de of [maken van een bestand parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die Hiermee worden de resourcenamen.

Een omgeving maken van uw sjabloon:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

U wilt lezen [meer over het implementeren van sjablonen](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Meer informatie over hoe u stapsgewijs omgevingen bijwerken, gebruikt u het parameterbestand en toegang tot sjablonen vanaf één locatie.

## <a name="next-steps"></a>Volgende stappen
U nu kunt aan de slag met meerdere netwerkonderdelen en virtuele machines. Voor het bouwen van uw toepassing met behulp van de belangrijkste onderdelen die zijn geïntroduceerd hier kunt u deze Voorbeeldomgeving.

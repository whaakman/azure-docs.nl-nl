---
title: Een Linux-omgeving maken met de Azure CLI | Microsoft Docs
description: Opslag, een Linux-VM, een virtueel netwerk en subnet, een load balancer, een NIC, een openbaar IP-adres en een netwerkbeveiligingsgroep, allemaal vanaf de grond met behulp van de Azure CLI maken.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: 75853d0fa35d1cb5fe593ad92a88d054d2ed77f6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55662278"
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a>Een volledige Linux-machine maken met de Azure CLI
Als u wilt snel een virtuele machine (VM) maken in Azure, kunt u een enkele Azure CLI-opdracht die gebruikmaakt van standaardwaarden om alle vereiste ondersteunende resources te maken. Resources, zoals een virtueel netwerk, openbare IP-adres en regels voor netwerkbeveiligingsgroepen worden automatisch gemaakt. Voor meer controle over uw omgeving in de productieomgeving gebruikt, u kunt deze resources vooraf maken en vervolgens uw virtuele machines toevoegen aan deze. In dit artikel begeleidt u bij het maken van een virtuele machine en elk van de ondersteunende resources één voor één.

Zorg ervoor dat u de meest recente hebt geïnstalleerd [Azure CLI](/cli/azure/install-az-cli2) en wordt in een Azure-account met [az login](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten *myResourceGroup*, *myVnet*, en *myVM*.

## <a name="create-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep moet worden gemaakt voordat u een virtuele machine en de ondersteunende virtuele-netwerkbronnen. Maak de resourcegroep met [az-groep maken](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

De uitvoer van Azure CLI-opdrachten is standaard in JSON (JavaScript Object Notation). De standaardwaarde uitvoer naar een lijst of tabel wijzigen, gebruikt u bijvoorbeeld [az--uitvoer configureren](/cli/azure/reference-index). U kunt ook toevoegen `--output` wijzigen voor een opdracht voor een één keer in de indeling van uitvoer. In het volgende voorbeeld toont de JSON-uitvoer van de `az group create` opdracht:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken
Volgende die u een virtueel netwerk maken in Azure en een subnet in waarop u uw virtuele machines kunt maken. Gebruik [az network vnet maken](/cli/azure/network/vnet) te maken van een virtueel netwerk met de naam *myVnet* met de *192.168.0.0/16* adresvoorvoegsel. U ook een subnet met de naam toevoegen *mySubnet* met het adresvoorvoegsel van *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

De uitvoer ziet u dat het subnet logisch binnen het virtuele netwerk is gemaakt:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken
Nu gaan we maken een openbaar IP-adres met [az network public-ip maken](/cli/azure/network/public-ip#az_network_public_ip_create). Dit openbare IP-adres kunt u vanaf Internet verbinding maken met uw virtuele machines. Omdat het standaardadres dynamisch is, maakt u een benoemde DNS-vermelding met de `--domain-name-label` parameter. Het volgende voorbeeld wordt een openbaar IP-adres met de naam *myPublicIP* met de DNS-naam van *mypublicdns*. Omdat de DNS-naam moet uniek zijn, Geef uw eigen unieke DNS-naam:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Uitvoer:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken
Voor het beheren van de verkeersstroom in en uit uw virtuele machines, kunt u een netwerkbeveiligingsgroep toepast op een virtuele NIC of subnet. Het volgende voorbeeld wordt [az network nsg maken](/cli/azure/network/nsg#az_network_nsg_create) te maken van een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

U definieert regels die bepaalde verkeer toestaan of weigeren. Als u wilt toestaan voor binnenkomende verbindingen op poort 22 (voor het SSH-toegang), maakt u een inkomende regel met [az network nsg-regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Het volgende voorbeeld wordt een regel met de naam *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

Als u wilt toestaan voor binnenkomende verbindingen op poort 80 (voor webverkeer), een andere regel voor netwerkbeveiligingsgroep toevoegen Het volgende voorbeeld wordt een regel met de naam *myNetworkSecurityGroupRuleHTTP*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

Controleer de netwerkbeveiligingsgroep en de regels met [az network nsg show](/cli/azure/network/nsg#az_network_nsg_show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Uitvoer:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou",
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to Internet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a>Maak een virtuele NIC
Virtuele netwerkinterfacekaarten (NIC's) zijn via een programma beschikbaar omdat u regels toepassen op het gebruik ervan kunt. Afhankelijk van de [VM-grootte](sizes.md), kunt u meerdere virtuele NIC's koppelen aan een virtuele machine. In de volgende [az network nic maken](/cli/azure/network/nic) opdracht, maken van een NIC met de naam *myNic* en koppel deze aan de netwerkbeveiligingsgroep. Het openbare IP-adres *myPublicIP* is ook gekoppeld aan de virtuele NIC

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Uitvoer:

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken
Beschikbaarheidssets help verspreid uw VM's over foutdomeinen en updatedomeinen. Hoewel u slechts één virtuele machine nu maakt, is het beste beschikbaarheidssets gebruiken zodat u gemakkelijk uitbreiden in de toekomst. 

Domeinen met fouten definiëren een groepering van virtuele machines die een gemeenschappelijke voedingsbron en netwerkswitch delen. Standaard worden de virtuele machines die zijn geconfigureerd in uw beschikbaarheidsset verdeeld over maximaal drie foutdomeinen. Een hardwareprobleem in een van deze domeinen met fouten heeft geen invloed op elke virtuele machine waarop uw app wordt uitgevoerd.

Update-domeinen geven aan groepen van virtuele machines en de onderliggende fysieke hardware die op hetzelfde moment opnieuw kan worden opgestart. Tijdens gepland onderhoud, de volgorde in waarvoor updatebeheer domeinen opnieuw zijn opgestart niet mogelijk sequentieel, maar slechts één updatedomein tegelijk opnieuw is opgestart.

Azure automatisch virtuele machines wordt verspreid over de fout- en updatedomeinen wanneer ze in een beschikbaarheidsset worden geplaatst. Zie voor meer informatie, [beheren van de beschikbaarheid van virtuele machines](manage-availability.md).

Maak een beschikbaarheidsset voor de virtuele machine met [az vm availability-set maken](/cli/azure/vm/availability-set#az_vm_availability_set_create). In het volgende voorbeeld wordt een beschikbaarheidsset met de naam *myAvailabilitySet* gemaakt:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

De opmerkingen bij de uitvoer foutdomeinen en updatedomeinen:

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-a-vm"></a>Een virtuele machine maken
U hebt de netwerkbronnen ter ondersteuning van via Internet toegankelijke VM's gemaakt. Nu een virtuele machine maken en beveilig deze met een SSH-sleutel. In dit voorbeeld maken we een Ubuntu VM op basis van de meest recente LTS. U vindt aanvullende installatiekopieën met [az vm image list](/cli/azure/vm/image#az_vm_image_list), zoals beschreven in [Azure VM-installatiekopieën zoeken](cli-ps-findimage.md).

Geef een SSH-sleutel te gebruiken voor verificatie. Als u een openbare SSH-sleutelpaar hebt, kunt u [maken ze](mac-create-ssh-keys.md) of gebruik de `--generate-ssh-keys` parameter te maken. Als u al een sleutelpaar hebt, deze parameter wordt gebruikt voor bestaande sleutels in `~/.ssh`.

De virtuele machine maken door de resources en informatie samen met de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH naar uw virtuele machine met de DNS-vermelding die u hebt opgegeven tijdens het maken van het openbare IP-adres. Dit `fqdn` in de uitvoer wordt weergegeven als u een virtuele machine maken:

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Uitvoer:

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

U kunt NGINX installeren en het verkeer stroom met de virtuele machine. Installeer NGINX als volgt:

```bash
sudo apt-get install -y nginx
```

Als u wilt de standaardsite nginx geopend in actie zien, open uw webbrowser en voer de FQDN-naam:

![Standaard-NGINX-site op de virtuele machine](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Als een sjabloon exporteren
Wat gebeurt er als u wilt nu een extra development environment maken met dezelfde parameters, of een productie-omgeving die overeenkomt met het? Resource Manager JSON-sjablonen die de parameters voor uw omgeving definieert gebruikt. U bouwen volledige omgevingen door te verwijzen naar deze JSON-sjabloon. U kunt [handmatig JSON-sjablonen samenstellen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of exporteren van een bestaande omgeving voor het maken van de JSON-sjabloon voor u. Gebruik [az group exporteren](/cli/azure/group) voor het exporteren van uw resourcegroep als volgt:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Deze opdracht maakt u de `myResourceGroup.json` bestand in uw huidige werkmap. Wanneer u een omgeving met deze sjabloon maakt, wordt u gevraagd voor alle resourcenamen van de. U kunt deze namen vullen in het sjabloonbestand door toe te voegen de `--include-parameter-default-value` parameter voor de `az group export` opdracht. Bewerk uw JSON-sjabloon om op te geven van de resourcenamen van de of [maken van een bestand parameters.json](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die Hiermee geeft u de resourcenamen.

Gebruik voor het maken van een omgeving van uw sjabloon [az group deployment maken](/cli/azure/group/deployment#az_group_deployment_create) als volgt:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

U wilt lezen [meer over het implementeren van sjablonen](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Meer informatie over het incrementeel bijwerken omgevingen, het parameterbestand gebruiken en toegang tot sjablonen vanaf één locatie.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om te werken met meerdere netwerkonderdelen en virtuele machines. Deze Voorbeeldomgeving kunt u uw toepassing bouwen met behulp van de belangrijkste onderdelen die zijn geïntroduceerd hier.

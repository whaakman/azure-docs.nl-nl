---
title: Taakverdeling maken zone-redundante virtuele machines met Azure CLI | Microsoft Docs
description: Informatie over het maken van een openbare Load Balancer Standard met zone redundant frontend met Azure CLI
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/09/2018
ms.author: kumud
ms.openlocfilehash: 29dcfaad840b5498dd859082ce11655a4f1fe8af
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
#  <a name="load-balance-vms-across-all-availability-zones-using-azure-cli"></a>Taakverdeling van VM's via alle beschikbaarheid zones met Azure CLI

Dit artikel begeleidt bij het maken van een openbare [Load Balancer standaard](https://aka.ms/azureloadbalancerstandard) met een zone-redundante frontend als u de zone redundantie zonder afhankelijkheid van meerdere DNS-records. Er is één front-end-IP-adres automatisch zone-redundante.  U kunt nu een elke virtuele machine in een virtueel netwerk in een regio die op alle Zones voor beschikbaarheid bereiken met een zone redundant frontend voor de load balancer, met een enkel IP-adres. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

Zie voor meer informatie over het gebruik van de beschikbaarheid van zones met standaard Load Balancer [standaard Load Balancer en beschikbaarheid Zones](load-balancer-standard-availability-zones.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u Azure CLI versie 2.0.17 zijn uitgevoerd of hoger.  Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Ondersteuning voor de beschikbaarheid van Zones is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupSLB* in de *westeurope* locatie:

```azurecli-interactive
az group create \
--name myResourceGroupSLB \
--location westeurope
```

## <a name="create-a-zone-redundant-public-ip-standard"></a>Maak een zone redundant openbare IP-standaard
Om toegang te krijgen tot uw app op internet, hebt u een openbaar IP-adres nodig voor de load balancer. Een zone-redundante front-end wordt geleverd door alle zones voor beschikbaarheid in een regio tegelijkertijd. Maak een zone redundant openbare IP-adres met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Wanneer u een standaard openbare IP-adres maakt, is het zoneredundant standaard.

Het volgende voorbeeld wordt een zone redundant openbare IP-adres met de naam *myPublicIP* in de *myResourceGroupLoadBalancer* resourcegroep.

```azurecli-interactive
az network public-ip create \
--resource-group myResourceGroupSLB \
--name myPublicIP \
--sku Standard
```

## <a name="create-azure-load-balancer-standard"></a>Azure Load Balancer standaard maken
Deze sectie beschrijft hoe u kunt maken en configureren van de volgende onderdelen van de load balancer:
- een frontend IP-adresgroep die het inkomende netwerkverkeer op de load balancer ontvangt.
- een back-end-IP-adresgroep waar de frontend-groep de belasting verzendt in evenwicht netwerkverkeer.
- een health-test die bepaalt de status van de back-end-VM-exemplaren.
- een load balancer-regel waarmee wordt gedefinieerd hoe verkeer wordt gedistribueerd naar de virtuele machines.

### <a name="create-the-load-balancer"></a>De load balancer maken
Maken van een standaard load balancer met [az network Load Balancer maken](/cli/azure/network/lb#az_network_lb_create). Het volgende voorbeeld wordt een load balancer met de naam *myLoadBalancer* en wijst de *myPublicIP* adres aan de front-end-IP-configuratie.

```azurecli-interactive
az network lb create \
--resource-group myResourceGroupSLB \
--name myLoadBalancer \
--public-ip-address myPublicIP \
--frontend-ip-name myFrontEnd \
--backend-pool-name myBackEndPool \
--sku Standard
```

## <a name="create-health-probe-on-port-80"></a>Maken van de health test op poort 80

Een statuscontrole controleert alle exemplaren van de virtuele machines om ervoor te zorgen dat deze netwerkverkeer kunnen verzenden. Het exemplaar van een virtuele machine met mislukte testcontroles wordt uit de load balancer verwijderd totdat deze weer online komt en een testcontrole bepaalt of deze in orde is. Maken van een health test met az network Load Balancer-test maken voor het controleren van de status van de virtuele machines. U gebruikt [az network lb probe create](/cli/azure/network/lb/probe#az_network_lb_probe_create) om een TCP-statustest te maken. In het volgende voorbeeld wordt een statustest gemaakt met de naam *myHealthProbe*:

```azurecli-interactive
az network lb probe create \
--resource-group myResourceGroupSLB \
--lb-name myLoadBalancer \
--name myHealthProbe \
--protocol tcp \
--port 80
```

## <a name="create-load-balancer-rule-for-port-80"></a>Maken van de load balancer-regel voor poort 80
Een load balancer-regel definieert de front-end-IP-configuratie voor het binnenkomende verkeer en de back-end-IP-adresgroep voor het ontvangen van het verkeer, samen met de vereiste poort van de bron- en doelserver. Maken van een load balancer-regel *myLoadBalancerRuleWeb* met [az network Load Balancer-regel maken](/cli/azure/network/lb/rule#az_network_lb_rule_create) om te luisteren op poort 80 in de groep frontend *myFrontEndPool* en verzenden taakverdeling van netwerkverkeer naar de back-end-adresgroep *myBackEndPool* ook met behulp van poort 80.

```azurecli-interactive
az network lb rule create \
--resource-group myResourceGroupSLB \
--lb-name myLoadBalancer \
--name myLoadBalancerRuleWeb \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--frontend-ip-name myFrontEnd \
--backend-pool-name myBackEndPool \
--probe-name myHealthProbe
```

## <a name="configure-virtual-network"></a>Virtueel netwerk configureren
Voordat u een aantal virtuele machines implementeren en de load balancer kunt testen, moet u de resources van de ondersteunende virtueel netwerk maken.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met de naam *myVnet* met een subnet met de naam *mySubnet* in het myResourceGroup met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create).


```azurecli-interactive
az network vnet create \
--resource-group myResourceGroupSLB \
--location westeurope \
--name myVnet \
--subnet-name mySubnet
```

### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maken van de netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* voor het definiëren van binnenkomende verbindingen met het virtuele netwerk met [az netwerk nsg maken](/cli/azure/network/nsg#az_network_nsg_create).

```azurecli-interactive
az network nsg create \
--resource-group myResourceGroupSLB \
--name myNetworkSecurityGroup
```

Maken van een groep netwerkbeveiligingsregel met de naam *myNetworkSecurityGroupRule* voor poort 80 met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create).

```azurecli-interactive
az network nsg rule create \
--resource-group myResourceGroupSLB \
--nsg-name myNetworkSecurityGroup \
--name myNetworkSecurityGroupRule \
--protocol tcp \
--direction inbound \
--source-address-prefix '*' \
--source-port-range '*' \
--destination-address-prefix '*' \
--destination-port-range 80 \
--access allow \
--priority 200
```
### <a name="create-nics"></a>NIC's maken
Maak drie virtuele NIC's met [az netwerk nic maken](/cli/azure/network/nic#az_network_nic_create) en koppel ze aan het openbare IP-adres en de netwerkbeveiligingsgroep. Het volgende voorbeeld maakt zes virtuele NIC's. (Eén virtuele NIC voor elke VM die u in de volgende stappen voor uw app maakt). U kunt op elk gewenst moment extra virtuele NIC's en VM's maken en toevoegen aan de load balancer:

```azurecli-interactive
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupSLB \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```
## <a name="create-backend-servers"></a>Maken van back-endservers
In dit voorbeeld maakt u drie virtuele machines zich in de zone 1, zone 2 en 3 om te worden gebruikt als back-endservers voor de load balancer-zone. U kunt ook NGINX installeren op de virtuele machines om te controleren of de load balancer is gemaakt.

### <a name="create-cloud-init-config"></a>Een cloud-init-configuratiebestand maken

U kunt een cloud-init-configuratiebestand NGINX installeren en uitvoeren van een 'Hello World' Node.js-app op een virtuele Linux-machine. In uw huidige shell, maakt u een bestand met de naam cloud init.txt en kopieer en plak de volgende configuratie in de shell. Zorg ervoor dat u de volledige kopieert cloud init bestand correct, met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-the-zonal-virtual-machines"></a>De zonal virtuele machines maken
Maken van het VMs met een [az vm maken](/cli/azure/vm#az_vm_create) in zone 1, zone 2 en 3 zone. Het volgende voorbeeld maakt een virtuele machine in elke zone en SSH-sleutels genereert als deze niet al bestaan:

Maken van virtuele machines in de zone 1

```azurecli-interactive
 az vm create \
--resource-group myResourceGroupSLB \
--name myVM$i \
--nics myNic$i \
--image UbuntuLTS \
--generate-ssh-keys \
--zone $i \
--custom-data cloud-init.txt
```
## <a name="test-the-load-balancer"></a>De load balancer testen

Ophalen van het openbare IP-adres van de load balancer met behulp van [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show). 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupSLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
``` 
Vervolgens kunt u het openbare IP-adres invoeren in een webbrowser. Het duurt enkele minuten voordat de virtuele machines gereed zijn en de load balancer begint met het distribueren van verkeer naar de VM's. De app wordt weergegeven met de hostnaam van de VM waarnaar de load balancer verkeer heeft gedistribueerd, zoals in het volgende voorbeeld:

![Node.js-app uitvoeren](./media/load-balancer-standard-public-zone-redundant-cli/running-nodejs-app.png)

Overzicht van de load balancer verkeer verdelen over virtuele machines in alle drie beschikbaarheid zones waarop uw app wordt uitgevoerd, kunt u stoppen van een virtuele machine in een bepaalde zone en vernieuw de browser.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [standaard Load Balancer](./load-balancer-standard-overview.md)




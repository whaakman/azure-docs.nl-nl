---
title: Snelstart - Webverkeer omleiden met Azure Application Gateway - Azure CLI | Microsoft Docs
description: Lees hoe u de Azure CLI kunt gebruiken om een Azure Application Gateway te maken die webverkeer omleidt naar virtuele machines in een back-endpool.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: quickstart
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 23ab97918d7def744f9ac3427faa3743b15124eb
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42022665"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Snelstart: webverkeer omleiden met Azure Application Gateway - Azure CLI

Met Azure Application Gateway kunt u uw het webverkeer van uw toepassing omleiden naar specifieke resources door listeners toe te wijzen aan poorten, regels te maken en resources toe te voegen aan een back-endpool.

In deze snelstart wordt beschreven hoe u de Azure CLI kunt gebruiken om snel de toepassingsgateway te maken met twee virtuele machines in zijn back-endpool. Vervolgens test u de gateway om er zeker van te zijn dat deze correct werkt.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart versie 2.0.4 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

U moet altijd resources in een resourcegroep maken. Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create). 

In het volgende voorbeeld wordt de resourcegroep *myResourceGroupAG* gemaakt op de locatie *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 

U moet een virtueel netwerk maken om de toepassingsgateway in staat te stellen te communiceren met andere resources. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de virtuele machines. 

Maak het virtuele netwerk en de subset de opdracht [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Maak het openbare IP-adres met de opdracht [az network public-ip create](/cli/azure/network/public-ip#az-public-ip-create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die worden gebruikt als back-endservers voor de toepassingsgateway. 

### <a name="create-two-virtual-machines"></a>Twee virtuele machines maken

U installeert ook NGINX op de virtuele machines om te controleren of de toepassingsgateway is gemaakt. U kunt een cloud-init-configuratiebestand maken om NGINX te installeren en een 'Hallo wereld' Node.js-app uit te voeren op een virtuele Linux-machine. 

Maak in uw huidige shell een bestand met de naam cloud-init.txt en plak de volgende configuratie in de shell. Zorg ervoor dat u het hele cloud-init-bestand correct kopieert, met name de eerste regel:

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

Maak de netwerkinterfaces met de opdracht [az network nic create](/cli/azure/network/nic#az-network-nic-create). Maak de virtuele machines met [az vm create](/cli/azure/vm#az-vm-create).

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

U kunt met de opdracht [az network application-gateway create](/cli/azure/network/application-gateway#az-application-gateway-create) een toepassingsgateway maken. Als u een toepassingsgateway maakt met Azure CLI, geeft u configuratiegegevens op, zoals capaciteit, SKU en HTTP-instellingen. De particuliere IP-adressen van de netwerkinterfaces worden toegevoegd als servers in de back-endpool van de toepassingsgateway.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

Het kan tot 30 minuten duren voordat de toepassingsgateway is gemaakt. Nadat de toepassingsgateway is gemaakt, kunt u de volgende deze functies ervan zien:

- *appGatewayBackendPool* - Een toepassingsgateway moet minimaal één back-endadresgroep hebben.
- *appGatewayBackendHttpSettings*: hiermee wordt aangegeven dat voor de communicatie poort 80 en een HTTP-protocol worden gebruikt.
- *appGatewayHttpListener*: de standaard-listener die aan *appGatewayBackendPool* is gekoppeld.
- *appGatewayFrontendIP*: hiermee wordt *myAGPublicIPAddress* aan *appGatewayHttpListener* toegewezen.
- *rule1* - De standaardrouteringsregel die aan *appGatewayHttpListener* is gekoppeld.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

U hebt NGINX niet hoeven installeren om de toepassingsgateway te maken, maar u hebt het in deze snelstart geïnstalleerd om te controleren of het maken van de toepassingsgateway is geslaagd. Gebruik [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) om het openbare IP-adres van de toepassingsgateway op te halen. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
``` 

![Toepassingsgateway testen](./media/quick-create-cli/application-gateway-nginxtest.png)

Als u de browser vernieuwt, ziet u de naam van de andere VM.

## <a name="clean-up-resources"></a>Resources opschonen

Bekijk eerst de resources die met de toepassingsgateway zijn gemaakt en wanneer u deze niet meer nodig hebt, kunt u met de opdracht [az group delete](/cli/azure/group#az-group-delete) de resourcegroep, de toepassingsgateway en alle gerelateerde resources verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```
 
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Webverkeer met een toepassingsgateway beheren met behulp van Azure CLI](./tutorial-manage-web-traffic-cli.md)


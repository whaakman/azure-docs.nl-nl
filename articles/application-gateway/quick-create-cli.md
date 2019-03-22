---
title: Snelstart - Webverkeer omleiden met Azure Application Gateway - Azure CLI | Microsoft Docs
description: Lees hoe u de Azure CLI kunt gebruiken om een Azure Application Gateway te maken die webverkeer omleidt naar virtuele machines in een back-endpool.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: eb0f73d31abced8decbed31e5604a2056584eb98
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549422"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Quickstart: Webverkeer omleiden met Azure Application Gateway - Azure CLI

Deze quickstart laat zien hoe u Azure portal gebruiken om een toepassingsgateway te maken.  Nadat de toepassingsgateway is gemaakt, testen u deze om te controleren of dat deze correct werkt. Met Azure Application Gateway, kunt u uw toepassing beveiligd webverkeer specifieke resources sturen door listeners toewijzen aan poorten, het maken van regels en resources toe te voegen aan een back-endpool. Om het eenvoudig, te worden in dit artikel wordt een eenvoudige configuratie met een openbaar front-end-IP-adres, een basislistener naar één site op deze application gateway-host, twee virtuele machines die worden gebruikt voor de back endpool en een regel voor het doorsturen van een algemene aanvraag.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Vereisten

### <a name="azure-powershell-module"></a>Azure PowerShell-module

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u Azure CLI versie 2.0.4 of hoger nodig. Voer **az --version** uit om de versie te zoeken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) voor meer informatie over installeren en upgraden.

### <a name="resource-group"></a>Resourcegroep

In Azure kunt u verwante resources toewijzen aan een resourcegroep. Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az-group-create). 

In het volgende voorbeeld wordt de resourcegroep *myResourceGroupAG* gemaakt op de locatie *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

### <a name="required-network-resources"></a>Vereiste netwerkbronnen 

Er is een virtueel netwerk nodig voor communicatie tussen de resources die u maakt.  Het subnet van de toepassingsgateway kan alleen bestaan uit toepassingsgateways. Andere resources zijn niet toegestaan.  U kunt een nieuw subnet maken voor Application Gateway of gebruik een bestaande resourcegroep. In dit voorbeeld maakt u twee subnetten in dit voorbeeld: één voor de application gateway en een voor de back-endservers. U kunt configureren dat de Frontend-IP van de toepassingsgateway moet openbaar of privé aan de hand van uw situatie. In dit voorbeeld kiezen we een openbare front-end-IP-adres.

Maak het virtuele netwerk en het subnet met de opdracht [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Voer [az network public-ip create](/cli/azure/network/public-ip) uit om het openbare IP-adres te maken.

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

### <a name="backend-servers"></a>Back-endservers

Back-end kan bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en multitenant back-ends, zoals Azure Appservice. In dit voorbeeld maakt u twee virtuele machines die door Azure worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway in Azure is gemaakt.

#### <a name="create-two-virtual-machines"></a>Twee virtuele machines maken

U installeert ook de [NGINX-webserver](https://docs.nginx.com/nginx/) op de virtuele machines om te controleren of de toepassingsgateway is gemaakt. U kunt een cloud-init-configuratiebestand maken om NGINX te installeren en een 'Hallo wereld' Node.js-app uit te voeren op een virtuele Linux-machine. Zie [Cloud-init-ondersteuning voor virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init) voor meer informatie over cloud-init.

Kopieer in uw Azure Cloud Shell de volgende configuratie en plak deze in een bestand met de naam *cloud-init.txt*. Voer *editor cloud-init.txt* in om het bestand te maken.

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

Maak de netwerkinterfaces met de opdracht [az network nic create](/cli/azure/network/nic#az-network-nic-create). Voor het maken van de virtuele machines gebruikt u [az vm create](/cli/azure/vm#az-vm-create).

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

U kunt met de opdracht [az network application-gateway create](/cli/azure/network/application-gateway) een toepassingsgateway maken. Als u met de Azure CLI een toepassingsgateway maakt, geeft u configuratiegegevens op, zoals capaciteit, SKU en HTTP-instellingen. Azure voegt dan de privé-IP-adressen van de netwerkinterfaces toe als servers in de back-endpool van de toepassingsgateway.

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

Het kan tot 30 minuten duren om de toepassingsgateway te maken in Azure. Wanneer deze is gemaakt, kunt u de volgende instellingen bekijken in het gedeelte **Instellingen** van de pagina **Toepassingsgateway**:

- **appGatewayBackendPool**: Staat op de pagina **back-endpools**. Hier is te zien wat de vereiste back-endpool is.
- **appGatewayBackendHttpSettings**: Staat op de pagina **HTTP-instellingen**. Hier wordt aangegeven dat de toepassingsgateway voor communicatie gebruikmaakt van poort 80 en het HTTP-protocol.
- **appGatewayHttpListener**: Staat op de **pagina Listeners**. Hier staat wat de standaard-listener is die aan **appGatewayBackendPool** is gekoppeld.
- **appGatewayFrontendIP**: Staat op de pagina **Front-end-IP-configuraties**. Hiermee wordt *myAGPublicIPAddress* aan **appGatewayHttpListener** toegewezen.
- **rule1**: Staat op de pagina **Regels**. Hier wordt aangegeven welke standaardrouteringsregel aan **appGatewayHttpListener** is gekoppeld.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

Het is in Azure niet nodig een NGINX-webserver te installeren om de toepassingsgateway te maken, maar u hebt de server in deze quickstart geïnstalleerd om te controleren of het maken van de toepassingsgateway in Azure is geslaagd. Gebruik [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) om het openbare IP-adres van de nieuwe toepassingsgateway op te halen. 

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.
    
![Toepassingsgateway testen](./media/quick-create-cli/application-gateway-nginxtest.png)

Als u de browser vernieuwt, ziet u de naam van de tweede VM. Een geldige reactie wordt gecontroleerd of de application gateway is gemaakt en uitgevoerd wordt om verbinding te maken met de back-end.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de bij de toepassingsgateway gemaakte resources niet meer nodig hebt, gebruikt u de opdracht [az group delete](/cli/azure/group#az-group-delete) om de resourcegroep te verwijderen. Als u de resourcegroep verwijdert, worden ook de toepassingsgateway en alle gerelateerde resources verwijderd.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Webverkeer met een toepassingsgateway beheren met behulp van Azure CLI](./tutorial-manage-web-traffic-cli.md)


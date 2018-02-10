---
title: Een toepassingsgateway maken met URL-pad op basis van een omleiding - Azure CLI | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met URL-pad op basis van een omgeleide verkeer met de Azure CLI.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 73fc20cf738f584008e7d8a019b8f7012dcacab1
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-application-gateway-with-url-path-based-redirection-using-the-azure-cli"></a>Een toepassingsgateway maken met de URL-pad op basis van een omleiding met de Azure CLI

U kunt de Azure CLI gebruiken voor het configureren van [routeringsregels voor URL-pad gebaseerde](application-gateway-url-route-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie maakt u met behulp van back-endpools [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Vervolgens maakt u de URL-routeringsregels die zorg ervoor dat het webverkeer wordt omgeleid naar de juiste back-endpool.

In dit artikel leert u hoe:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken
> * Listeners en routeringsregels toevoegen
> * Maken van virtuele-machineschaalsets voor back-endpools

Het volgende voorbeeld ziet u siteverkeer dat afkomstig is van poort 8080 en 8081 en omgeleid naar dezelfde back-endpools:

![Voorbeeld van de URL-routering](./media/tutorial-url-redirect-cli/scenario.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een resource-groep met [az groep maken](/cli/azure/group#create).

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAG* in de *eastus* locatie.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Maken van netwerkbronnen 

Maken van het virtuele netwerk met de naam *myVNet* en het subnet met de naam *myAGSubnet* met [az network vnet maken](/cli/azure/network/vnet#az_net). Vervolgens kunt u het subnet met de naam toevoegen *myBackendSubnet* die nodig is voor de back-endservers met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Maken van het openbare IP-adres met de naam *myAGPublicIPAddress* met [az netwerk openbare ip-maken](/cli/azure/public-ip#az_network_public_ip_create).

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
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U kunt [az netwerk toepassingsgateway maken](/cli/azure/application-gateway#create) om de toepassingsgateway myAppGateway met de naam te maken. Wanneer u een toepassingsgateway met Azure CLI maakt, kunt u configuratie-informatie, zoals de capaciteit, sku en HTTP-instellingen opgeven. De toepassingsgateway is toegewezen aan *myAGSubnet* en *myPublicIPSddress* die u eerder hebt gemaakt.  

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

 Duurt enkele minuten voor de toepassingsgateway moet worden gemaakt. Nadat de toepassingsgateway is gemaakt, kunt u deze nieuwe functies ervan zien:

- *appGatewayBackendPool* -een toepassingsgateway moet ten minste één back-endadresgroep hebben.
- *appGatewayBackendHttpSettings* -Hiermee geeft u op poort 80 en een HTTP-protocol wordt gebruikt voor communicatie.
- *appGatewayHttpListener* -de standaard-listener die zijn gekoppeld aan *appGatewayBackendPool*.
- *appGatewayFrontendIP* -wijst *myAGPublicIPAddress* naar *appGatewayHttpListener*.
- *Rule1* : de regel die is gekoppeld aan routering standaard *appGatewayHttpListener*.


### <a name="add-backend-pools-and-ports"></a>Back-endpools en poorten toevoegen

U kunt toevoegen met de naam van back-end-adresgroepen *imagesBackendPool* en *videoBackendPool* voor uw toepassingsgateway met behulp van [az netwerk toepassingsgateway adresgroep maken](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). Toevoegen van de frontend-poorten voor de groepen met behulp van [az toepassingsgateway frontend-netwerkpoort maken](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name imagesBackendPool
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name videoBackendPool
az network application-gateway frontend-port create \
  --port 8080 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name bport
az network application-gateway frontend-port create \
  --port 8081 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name rport
```

## <a name="add-listeners-and-rules"></a>Listeners en regels toevoegen

### <a name="add-listeners"></a>Listeners toevoegen

Toevoegen van de back-end-listeners met de naam *backendListener* en *redirectedListener* die nodig zijn voor het routeren van verkeer met behulp van [az netwerk toepassingsgateway http-listener maken](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port bport \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway http-listener create \
  --name redirectedListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port rport \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-default-url-path-map"></a>De standaard URL-pad toewijzing toevoegen

URL-pad maps Zorg ervoor dat er specifieke URL's worden doorgestuurd naar specifieke back-endpools. U kunt maken met de naam van URL-pad maps *imagePathRule* en *videoPathRule* met [az toepassingsgateway url-pad-Netwerkoverzicht maken](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) en [az netwerk toepassingsgateway overzicht van een url-pad regel maken](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name urlpathmap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --address-pool imagesBackendPool \
  --default-address-pool appGatewayBackendPool \
  --default-http-settings appGatewayBackendHttpSettings \
  --http-settings appGatewayBackendHttpSettings \
  --rule-name imagePathRule
az network application-gateway url-path-map rule create \
  --gateway-name myAppGateway \
  --name videoPathRule \
  --resource-group myResourceGroupAG \
  --path-map-name urlpathmap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-redirection-configuration"></a>Omleiding configuratie toevoegen

U kunt configureren omleiding voor de listener met [az netwerk toepassingsgateway omleiding-config maken](/cli/azure/application-gateway#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --gateway-name myAppGateway \
  --name redirectConfig \
  --resource-group myResourceGroupAG \
  --type Found \
  --include-path true \
  --include-query-string true \
  --target-listener backendListener
```

### <a name="add-the-redirection-url-path-map"></a>De toewijzing van omleiding URL-pad toevoegen

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name redirectpathmap \
  --paths /images/* \
  --resource-group myResourceGroupAG \
  --redirect-config redirectConfig \
  --rule-name redirectPathRule
```

### <a name="add-routing-rules"></a>Routeringsregels toevoegen

De URL-pad maps koppelen de routeringsregels aan de-listeners die u hebt gemaakt. U kunt de regels met de naam toevoegen *defaultRule* en *redirectedRule* met [az netwerk toepassingsgateway regel maken](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name defaultRule \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map urlpathmap \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectedRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectedListener \
  --rule-type PathBasedRouting \
  --url-path-map redirectpathmap \
  --address-pool appGatewayBackendPool
```

## <a name="create-virtual-machine-scale-sets"></a>Maken van virtuele-machineschaalsets

In dit voorbeeld maakt u drie virtuele-machineschaalsets die ondersteuning bieden voor de drie back-end-adresgroepen die u hebt gemaakt. De naam van de schaalsets die u maakt *myvmss1*, *myvmss2*, en *myvmss3*. Elke scale set bestaat uit twee virtuele machine-exemplaren op die u NGINX installeren.

```azurecli-interactive
for i in `seq 1 3`; do
  if [ $i -eq 1 ]
  then
    poolName="appGatewayBackendPool" 
  fi
  if [ $i -eq 2 ]
  then
    poolName="imagesBackendPool"
  fi
  if [ $i -eq 3 ]
  then
    poolName="videoBackendPool"
  fi
  az vmss create \
    --name myvmss$i \
    --resource-group myResourceGroupAG \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Azure123456! \
    --instance-count 2 \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --vm-sku Standard_DS2 \
    --upgrade-policy-mode Automatic \
    --app-gateway myAppGateway \
    --backend-pool-name $poolName
done
```

### <a name="install-nginx"></a>NGINX installeren

```azurecli-interactive
for i in `seq 1 3`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Als u het openbare IP-adres van de toepassingsgateway, kunt u [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show). Het openbare IP-adres Kopieer en plak deze in de adresbalk van uw browser. Zoals *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm*, *http://40.121.222.19:8080/video/test.htm*, of *http:// 40.121.222.19:8081/images/test.htm*.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Basis-URL te testen in de toepassingsgateway](./media/tutorial-url-redirect-cli/application-gateway-nginx.png)

Wijzig de URL naar http://&lt;IP-adres&gt;: 8080/video/test.html, vervangen door uw IP-adres voor &lt;IP-adres&gt;, en u ziet ongeveer het volgende voorbeeld:

![Test-URL voor afbeeldingen in de toepassingsgateway](./media/tutorial-url-redirect-cli/application-gateway-nginx-images.png)

Wijzig de URL naar http://&lt;IP-adres&gt;: 8080/video/test.html, vervangen door uw IP-adres voor &lt;IP-adres&gt;, en u ziet ongeveer het volgende voorbeeld.

![Video-URL te testen in de toepassingsgateway](./media/tutorial-url-redirect-cli/application-gateway-nginx-video.png)

Wijzig nu de URL naar http://&lt;IP-adres&gt;: 8081/images/test.htm, vervangen door uw IP-adres voor &lt;IP-adres&gt;, en ziet u verkeer teruggeleid naar de back-endpool van de installatiekopieën op http://&lt;IP-adres&gt;: 8080/installatiekopieën.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken
> * Listeners en routeringsregels toevoegen
> * Maken van virtuele-machineschaalsets voor back-endpools

> [!div class="nextstepaction"]
> [Meer informatie over wat u met application gateway doen kunt](application-gateway-introduction.md)
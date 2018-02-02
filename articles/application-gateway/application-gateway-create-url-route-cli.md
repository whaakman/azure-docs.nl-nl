---
title: Een toepassingsgateway maken met URL-pad gebaseerde routeringsregels - Azure CLI | Microsoft Docs
description: Informatie over het maken van de URL op basis van een pad routeringsregels voor een application gateway en de virtuele machine schaal ingesteld met de Azure CLI.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: davidmu
ms.openlocfilehash: 0593e37def43770efad7e07b306d8290b0590a48
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="create-an-application-gateway-with-url-path-based-routing-rules-using-the-azure-cli"></a>Een toepassingsgateway maken met URL-pad gebaseerde routeringsregels met de Azure CLI

U kunt de Azure CLI gebruiken voor het configureren van [routeringsregels voor URL-pad gebaseerde](application-gateway-url-route-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie maakt u back-endpools met behulp van een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Vervolgens maakt u routeringsregels die zorg ervoor dat het webverkeer binnenkomt op de juiste servers van de groepen.

In dit artikel leert u hoe:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken met URL-kaart
> * Virtuele-machineschaalsets met de back-end-adresgroepen maken

![Voorbeeld van de URL-routering](./media/application-gateway-create-url-route-cli/scenario.png)

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

## <a name="create-the-application-gateway-with-url-map"></a>De toepassingsgateway maken met URL-kaart

U kunt [az netwerk toepassingsgateway maken](/cli/azure/application-gateway#create) voor het maken van de toepassingsgateway met de naam *myAppGateway*. Wanneer u een toepassingsgateway met Azure CLI maakt, kunt u configuratie-informatie, zoals de capaciteit, sku en HTTP-instellingen opgeven. De toepassingsgateway is toegewezen aan *myAGSubnet* en *myAGPublicIPAddress* die u eerder hebt gemaakt. 

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


### <a name="add-image-and-video-backend-pools-and-port"></a>Afbeeldings- en videobestanden back-endpools en poort toevoegen

U kunt toevoegen met de naam van back-endpools *imagesBackendPool* en *videoBackendPool* voor uw toepassingsgateway met behulp van [az netwerk toepassingsgateway adresgroep maken](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). Toevoegen van de frontend-poort voor de groepen met behulp van [az toepassingsgateway frontend-netwerkpoort maken](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

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
  --name port8080
```

### <a name="add-backend-listener"></a>Back-end-listener toevoegen

Toevoegen van de back-end-listener met de naam *backendListener* die nodig is voor het routeren van verkeer met behulp van [az netwerk toepassingsgateway http-listener maken](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>Overzicht van de URL-pad toevoegen

URL-pad maps Zorg ervoor dat er specifieke URL's worden doorgestuurd naar specifieke back-endpools. U kunt maken met de naam van URL-pad maps *imagePathRule* en *videoPathRule* met [az toepassingsgateway url-pad-Netwerkoverzicht maken](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) en [az netwerk toepassingsgateway overzicht van een url-pad regel maken](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

```azurecli-interactive
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name myPathMap \
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
  --path-map-name myPathMap \
  --paths /video/* \
  --address-pool videoBackendPool
```

### <a name="add-routing-rule"></a>Regel voor het doorsturen toevoegen

De regel voor het doorsturen koppelt de URL-toewijzingen aan de listener die u hebt gemaakt. U kunt de regel voor licentiecontrole toevoegen *regel 2* met [az netwerk toepassingsgateway regel maken](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener backendListener \
  --rule-type PathBasedRouting \
  --url-path-map myPathMap \
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

Als u het openbare IP-adres van de toepassingsgateway, kunt u [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show). Het openbare IP-adres Kopieer en plak deze in de adresbalk van uw browser. Zoals *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm*, of *http://40.121.222.19:8080/video/test.htm*.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Basis-URL te testen in de toepassingsgateway](./media/application-gateway-create-url-route-cli/application-gateway-nginx.png)

Wijzig de URL naar http://<ip-address>:8080/video/test.html aan het einde van de basis-URL en u ziet ongeveer het volgende voorbeeld:

![Test-URL voor afbeeldingen in de toepassingsgateway](./media/application-gateway-create-url-route-cli/application-gateway-nginx-images.png)

Wijzig de URL naar http://<ip-address>:8080/video/test.html en u ziet ongeveer het volgende voorbeeld.

![Video-URL te testen in de toepassingsgateway](./media/application-gateway-create-url-route-cli/application-gateway-nginx-video.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken met URL-kaart
> * Virtuele-machineschaalsets met de back-end-adresgroepen maken

Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.

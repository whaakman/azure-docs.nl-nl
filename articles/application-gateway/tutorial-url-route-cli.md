---
title: 'Zelfstudie: Webverkeer routeren op basis van de URL - Azure CLI'
description: Meer informatie over hoe u webverkeer routeert op basis van de URL naar specifieke, schaalbare servergroepen met behulp van de Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 45057a23d54f42a4f5b165fc3eb50c001ce92e8d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069165"
---
# <a name="tutorial-route-web-traffic-based-on-the-url-using-the-azure-cli"></a>Zelfstudie: Webverkeer routeren op basis van de URL met behulp van de Azure CLI

U kunt de Azure CLI gebruiken om routering van webverkeer naar specifieke schaalbare servergroepen te configureren op basis van de URL die wordt gebruikt voor toegang tot uw toepassing. In deze zelfstudie maakt u een [Azure Application Gateway](application-gateway-introduction.md) met drie back-endpools met behulp van [Virtual Machine Scale Sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Elk van de back-endpools heeft een specifiek doel, zoals algemene gegevens, afbeeldingen en video's.  Door verkeer te routeren om pools te scheiden, zorgt u ervoor dat uw klanten de informatie krijgen die ze nodig hebben wanneer ze deze nodig hebben.

Als u verkeersroutering wilt inschakelen, maakt u [routeringsregels](application-gateway-url-route-overview.md) die worden toegewezen aan listeners die op bepaalde poorten luisteren om ervoor te zorgen dat webverkeer binnenkomt op de juiste servers in de pools.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het netwerk instellen
> * Listeners, toewijzing van URL-pad en regels maken
> * Schaalbare back-endpools maken


![Voorbeeld van URL-routering](./media/tutorial-url-route-cli/scenario.png)

U kunt deze zelfstudie desgewenst volgen met behulp van [Azure PowerShell](tutorial-url-route-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie de Azure CLI (versie 2.0.4 of hoger) uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een resourcegroep met [az group create](/cli/azure/group#create).

In het volgende voorbeeld wordt de resourcegroep *myResourceGroupAG* gemaakt op de locatie *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 

Maak het virtuele netwerk *myVNet* en het subnet *myAGSubnet* met [az network vnet create](/cli/azure/network/vnet#az_net). Vervolgens voegt u het subnet *myBackendSubnet*, dat voor de back-endservers vereist is, toe met [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Maak het openbare IP-adres *myAGPublicIPAddress* met [az network public-ip create](/cli/azure/public-ip#az_network_public_ip_create).

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

## <a name="create-the-application-gateway-with-url-map"></a>Toepassingsgateway met URL-toewijzing maken

Gebruik [az network application-gateway create](/cli/azure/application-gateway#create) om een toepassingsgateway met de naam *myAppGateway* te maken. Als u een toepassingsgateway met de Azure CLI maakt, geeft u configuratiegegevens op, zoals capaciteit, SKU en HTTP-instellingen. De toepassingsgateway wordt toegewezen aan *myAGSubnet* en *myAGPublicIPAddress*, die u eerder hebt gemaakt. 

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

 Het kan enkele minuten duren om de toepassingsgateway te maken. Nadat de toepassingsgateway is gemaakt, ziet u de volgende nieuwe functies:

- *appGatewayBackendPool*: een toepassingsgateway moet ten minste één back-endadresgroep hebben.
- *appGatewayBackendHttpSettings*: hiermee wordt aangegeven dat voor de communicatie poort 80 en een HTTP-protocol worden gebruikt.
- *appGatewayHttpListener*: de standaard-listener die aan *appGatewayBackendPool* is gekoppeld.
- *appGatewayFrontendIP*: hiermee wordt *myAGPublicIPAddress* aan *appGatewayHttpListener* toegewezen.
- *rule1*: de standaardrouteringsregel die aan *appGatewayHttpListener* is gekoppeld.


### <a name="add-image-and-video-backend-pools-and-port"></a>Back-endpools en -poort voor afbeeldingen en video toevoegen

Voeg de back-endpools *imagesBackendPool* en *videoBackendPool* toe aan de toepassingsgateway met [az network application-gateway address-pool create](/cli/azure/application-gateway#az_network_application_gateway_address-pool_create). U voegt de front-endpoorten voor de pools toe met [az network application-gateway frontend-port create](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). 

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

### <a name="add-backend-listener"></a>Back-endlistener toevoegen

Voeg back-endlistener *backendListener*, die voor het omleiden van verkeer nodig zijn, toe met [az network application-gateway http-listener create](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).


```azurecli-interactive
az network application-gateway http-listener create \
  --name backendListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port port8080 \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-url-path-map"></a>Toewijzing van URL-pad toevoegen

URL-padtoewijzingen zorgen ervoor dat specifieke URL's naar specifieke back-endpools worden omgeleid. Maak de URL-padtoewijzingen *imagePathRule* en *videoPathRule* met [az network application-gateway url-path-map create](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_create) en [az network application-gateway url-path-map rule create](/cli/azure/application-gateway#az_network_application_gateway_url_path_map_rule_create)

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

### <a name="add-routing-rule"></a>Routeringsregel toevoegen

De routeringsregel koppelt de URL-toewijzingen aan de listener die u hebt gemaakt. Voeg de regel *rule2* toe met [az network application-gateway rule create](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

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

## <a name="create-virtual-machine-scale-sets"></a>Schaalsets voor virtuele machines maken

In deze zelfstudie maakt u drie schaalsets voor virtuele machines die ondersteuning bieden voor de drie back-end-pools die u hebt gemaakt. U maakt schaalsets met de namen *myvmss1*, *myvmss2* en *myvmss3*. Elke schaalset bevat twee exemplaren van virtuele machines waarop u NGINX installeert.

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
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"], "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

Gebruik [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) om het openbare IP-adres van de toepassingsgateway op te halen. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Bijvoorbeeld *http://40.121.222.19*, *http://40.121.222.19:8080/images/test.htm* of *http://40.121.222.19:8080/video/test.htm*.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Basis-URL testen in de toepassingsgateway](./media/tutorial-url-route-cli/application-gateway-nginx.png)

Wijzig de URL in http://&lt;ip-address&gt;: 8080/images/test.html, waarbij u &lt;ip-address&gt; vervangt door uw eigen IP-adres. U krijgt nu iets te zien zoals in het volgende voorbeeld:

![Afbeeldingen-URL in toepassingsgateway testen](./media/tutorial-url-route-cli/application-gateway-nginx-images.png)

Wijzig de URL in http://&lt;ip-address&gt;: 8080/video/test.html, waarbij u &lt;ip-address&gt; vervangt door uw eigen IP-adres. U krijgt nu iets te zien zoals in het volgende voorbeeld.

![Video-URL testen in de toepassingsgateway](./media/tutorial-url-route-cli/application-gateway-nginx-video.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de resourcegroep, de toepassingsgateway en alle gerelateerde resources verwijderen als u deze niet meer nodig hebt.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het netwerk instellen
> * Listeners, toewijzing van URL-pad en regels maken
> * Schaalbare back-endpools maken

> [!div class="nextstepaction"]
> [Een toepassingsgateway maken met een omleiding op basis van een URL-pad](./tutorial-url-redirect-cli.md)

---
title: Een toepassingsgateway maken met het externe verkeer omleiding - Azure CLI | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway waarmee interne webverkeer worden omgeleid naar de juiste groep met de Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: 3ded6fc0950c82d0aa36da89fdd5635afef7be0b
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-external-redirection-using-the-azure-cli"></a>Een toepassingsgateway maken met de externe omleiding met de Azure CLI

U kunt de Azure CLI gebruiken voor het configureren van [web verkeer omleiden](application-gateway-multi-site-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie configureert u een listener en de regel die wordt omgeleid webverkeer dat op de toepassingsgateway met een externe site binnenkomt.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Maak een regel listener en -omleiding
> * Een toepassingsgateway maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een resource-groep met [az groep maken](/cli/azure/group#create).

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAG* in de *eastus* locatie.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 

Maken van het virtuele netwerk met de naam *myVNet* en het subnet met de naam *myAGSubnet* met [az network vnet maken](/cli/azure/network/vnet#az_net). Maken van het openbare IP-adres met de naam *myAGPublicIPAddress* met [az netwerk openbare ip-maken](/cli/azure/public-ip#az_network_public_ip_create). Deze resources worden gebruikt om de netwerkverbinding met de toepassingsgateway en de bijbehorende bronnen opgeven.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U kunt [az netwerk toepassingsgateway maken](/cli/azure/application-gateway#create) voor het maken van de toepassingsgateway met de naam *myAppGateway*. Wanneer u een toepassingsgateway met Azure CLI maakt, kunt u configuratie-informatie, zoals de capaciteit, sku en HTTP-instellingen opgeven. De toepassingsgateway is toegewezen aan *myAGSubnet* en *myPublicIPSddress* die u eerder hebt gemaakt. 

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
  --frontend-port 8080 \
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

### <a name="add-the-redirection-configuration"></a>De configuratie van de omleiding toevoegen

Toevoegen van de configuratie van de omleiding die verkeer van de toepassingsgateway verzendt *bing.com* met [az netwerk toepassingsgateway omleiding-config maken](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name myredirect \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Temporary \
  --target-url "http://bing.com"
```

### <a name="add-a-listener-and-routing-rule"></a>De regel voor het doorsturen en een listener toevoegen

Een listener is vereist voor het inschakelen van de toepassingsgateway verkeer op de juiste wijze te routeren. Maken van de listener met [az netwerk toepassingsgateway http-listener maken](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create) met de frontend-poort gemaakt met [az toepassingsgateway frontend-netwerkpoort maken](/cli/azure/application-gateway#az_network_application_gateway_frontend_port_create). Een regel is vereist voor de listener weet waar binnenkomende verkeer te verzenden. Maken van een eenvoudige regel met naam *redirectRule* met [az netwerk toepassingsgateway regel maken](/cli/azure/application-gateway#az_network_application_gateway_rule_create) met de omleiding-configuratie.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name redirectPort
az network application-gateway http-listener create \
  --name redirectListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port redirectPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectListener \
  --rule-type Basic \
  --redirect-config myredirect
```

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Als u het openbare IP-adres van de toepassingsgateway, kunt u [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show). Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

U ziet *bing.com* worden weergegeven in uw browser.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> * Instellen van het netwerk
> * Maak een regel listener en -omleiding
> * Een toepassingsgateway maken

> [!div class="nextstepaction"]
> [Meer informatie over wat u met application gateway doen kunt](./application-gateway-introduction.md)
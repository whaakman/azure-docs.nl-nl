---
title: Een toepassingsgateway maken met een certificaat - Azure CLI | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway en toevoegen van een certificaat voor SSL-beëindiging met de Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: victorh
ms.openlocfilehash: dc3a15ea66d9ead35247bee1673d120faac631d9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34355165"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-the-azure-cli"></a>Een toepassingsgateway maken met HTTP naar HTTPS-omleiding met de Azure CLI

U kunt de Azure CLI gebruiken voor het maken een [toepassingsgateway](overview.md) met een certificaat voor SSL-beëindiging. Een regel voor doorsturen wordt gebruikt voor het HTTP-verkeer omleiden naar de HTTPS-poort in de toepassingsgateway. In dit voorbeeld maakt u ook een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor de back-endpool van de toepassingsgateway met twee exemplaren van de virtuele machine.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een netwerk instellen
> * Een toepassingsgateway maken met het certificaat
> * Een regel listener en -omleiding toevoegen
> * Een virtuele-machineschaalset ingesteld met het standaard back-end-adresgroep maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Voor gebruik in productieomgevingen, moet u een geldig certificaat dat is ondertekend door een vertrouwde provider importeren. Voor deze zelfstudie maakt u een zelfondertekend certificaat en het pfx-bestand met de opdracht openssl.

```azurecli-interactive
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out appgwcert.crt
```

Voer de waarden die geschikt zijn voor uw certificaat. U kunt de standaardwaarden accepteren.

```azurecli-interactive
openssl pkcs12 -export -out appgwcert.pfx -inkey privateKey.key -in appgwcert.crt
```

Voer het wachtwoord voor het certificaat. In dit voorbeeld *Azure123456!* wordt gebruikt.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een resource-groep met [az groep maken](/cli/azure/group#create).

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAG* in de *eastus* locatie.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken

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

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

U kunt [az netwerk toepassingsgateway maken](/cli/azure/network/application-gateway#az_network_application_gateway_create) voor het maken van de toepassingsgateway met de naam *myAppGateway*. Wanneer u een toepassingsgateway met Azure CLI maakt, kunt u configuratie-informatie, zoals de capaciteit, sku en HTTP-instellingen opgeven. 

De toepassingsgateway is toegewezen aan *myAGSubnet* en *myAGPublicIPAddress* die u eerder hebt gemaakt. In dit voorbeeld koppelen u het certificaat dat u hebt gemaakt en het bijbehorende wachtwoord wanneer u de toepassingsgateway maakt. 

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
  --frontend-port 443 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress \
  --cert-file appgwcert.pfx \
  --cert-password "Azure123456!"

```

 Duurt enkele minuten voor de toepassingsgateway moet worden gemaakt. Nadat de toepassingsgateway is gemaakt, kunt u deze nieuwe functies ervan zien:

- *appGatewayBackendPool* -een toepassingsgateway moet ten minste één back-endadresgroep hebben.
- *appGatewayBackendHttpSettings* -Hiermee geeft u op poort 80 en een HTTP-protocol wordt gebruikt voor communicatie.
- *appGatewayHttpListener* -de standaard-listener die zijn gekoppeld aan *appGatewayBackendPool*.
- *appGatewayFrontendIP* -wijst *myAGPublicIPAddress* naar *appGatewayHttpListener*.
- *Rule1* : de regel die is gekoppeld aan routering standaard *appGatewayHttpListener*.

## <a name="add-a-listener-and-redirection-rule"></a>Een regel listener en -omleiding toevoegen

### <a name="add-the-http-port"></a>De HTTP-poort toevoegen

U kunt [az toepassingsgateway frontend-netwerkpoort maken](/cli/azure/network/application-gateway/frontend-port#az_network_application_gateway_frontend_port_create) de HTTP-poort toevoegen aan de toepassingsgateway.

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name httpPort
```

### <a name="add-the-http-listener"></a>De HTTP-listener toevoegen

U kunt [az netwerk toepassingsgateway http-listener maken](/cli/azure/network/application-gateway/http-listener#az_network_application_gateway_http_listener_create) om toe te voegen van de listener met de naam *myListener* aan de toepassingsgateway.

```azurecli-interactive
az network application-gateway http-listener create \
  --name myListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port httpPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
```

### <a name="add-the-redirection-configuration"></a>De configuratie van de omleiding toevoegen

De HTTP toevoegen aan HTTPS-omleiding configuratie met de application gateway via [az netwerk toepassingsgateway omleiding-config maken](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name httpToHttps \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener appGatewayHttpListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-the-routing-rule"></a>Toevoegen van de regel voor routering

Voeg de routeringsregel met de naam *regel 2* met de configuratie van omleiding wordt gebruikt door de toepassing gateway [az netwerk toepassingsgateway regel maken](/cli/azure/network/application-gateway/rule#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name rule2 \
  --resource-group myResourceGroupAG \
  --http-listener myListener \
  --rule-type Basic \
  --redirect-config httpToHttps
```

## <a name="create-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset maken

In dit voorbeeld maakt u een virtuele-machineschaalset set met de naam *myvmss* servers en biedt voor de back endpool in de toepassingsgateway. De virtuele machines in de schaalset zijn gekoppeld aan *myBackendSubnet* en *appGatewayBackendPool*. Maken van de schaal ingesteld, kunt u [az vmss maken](/cli/azure/vmss#az_vmss_create).

```azurecli-interactive
az vmss create \
  --name myvmss \
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
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>NGINX installeren

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
```

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Als u het openbare IP-adres van de toepassingsgateway, kunt u [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show). Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Waarschuwing beveiligen](./media/redirect-http-to-https-cli/application-gateway-secure.png)

Voor het accepteren van de beveiligingswaarschuwing als u een zelfondertekend certificaat gebruikt, selecteert u **Details** en vervolgens **gaat u naar de webpagina**. Uw beveiligde NGINX-site wordt vervolgens weergegeven zoals in het volgende voorbeeld:

![Basis-URL te testen in de toepassingsgateway](./media/redirect-http-to-https-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een netwerk instellen
> * Een toepassingsgateway maken met het certificaat
> * Een regel listener en -omleiding toevoegen
> * Een virtuele-machineschaalset ingesteld met het standaard back-end-adresgroep maken



---
title: Een toepassingsgateway maken met de interne omleiding - Azure CLI | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway waarmee interne webverkeer worden omgeleid naar de juiste groep met de Azure CLI.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2017
ms.author: davidmu
ms.openlocfilehash: 20f54796f1dd088fa9d6446866151e6ce8ce03f7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-application-gateway-with-internal-redirection-using-the-azure-cli"></a>Een toepassingsgateway maken met de interne omleiding met de Azure CLI

U kunt de Azure CLI gebruiken voor het configureren van [web verkeer omleiden](multiple-site-overview.md) bij het maken van een [toepassingsgateway](overview.md). In deze zelfstudie definieert u een back-endpool met behulp van een scale-set van virtuele machines. Configureert u listeners en regels op basis van domeinnamen waarvan u eigenaar om ervoor te zorgen webverkeer binnenkomt op de juiste groep. Deze zelfstudie wordt ervan uitgegaan dat u voorbeelden van meerdere domeinen en maakt gebruik van de eigenaar *www.contoso.com* en *www.contoso.org*.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken
> * Listeners en omleiding regel toevoegen
> * Een virtuele-machineschaalset ingesteld met de back-endpool maken
> * Een CNAME-record maken in uw domein

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

Maken van het virtuele netwerk met de naam *myVNet* en het subnet met de naam *myAGSubnet* met [az network vnet maken](/cli/azure/network/vnet#az_net). Vervolgens kunt u het subnet met de naam toevoegen *myBackendSubnet* die nodig is voor de back-endpool van servers via [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Maken van het openbare IP-adres met de naam *myAGPublicIPAddress* met [az netwerk openbare ip-maken](/cli/azure/public-ip#az_network_public_ip_create).

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


## <a name="add-listeners-and-rules"></a>Listeners en regels toevoegen 

Een listener is vereist voor het inschakelen van de toepassingsgateway voor het verkeer op de juiste wijze te routeren naar de back-endpool. In deze zelfstudie maakt u twee listeners voor de twee domeinen. In dit voorbeeld listeners worden gemaakt voor de domeinen van *www.contoso.com* en *www.contoso.org*.

Toevoegen van de back-end-listeners die nodig zijn voor het routeren van verkeer met behulp van [az netwerk toepassingsgateway http-listener maken](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoComListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com
az network application-gateway http-listener create \
  --name contosoOrgListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.org   
  ```

### <a name="add-the-redirection-configuration"></a>De configuratie van de omleiding toevoegen

Toevoegen van de configuratie van de omleiding die het verkeer van stuurt *www.consoto.org* voor de listener voor *www.contoso.com* in het application gateway met [az-toepassing-netwerkgateway maken van de omleidings-config](/cli/azure/network/application-gateway/redirect-config#az_network_application_gateway_redirect_config_create).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name orgToCom \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener contosoListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-routing-rules"></a>Routeringsregels toevoegen

Regels worden verwerkt in de volgorde waarin ze zijn gemaakt en verkeer wordt omgeleid naar de toepassingsgateway met behulp van de eerste regel die overeenkomt met de URL verzonden. Bijvoorbeeld, als u een regel met behulp van een basic-listener en een regel met meerdere sites listener beide op dezelfde poort hebt, kan de regel met de listener voor meerdere locaties moet worden vermeld voordat u de regel met de basic-listener in volgorde voor de regel voor meerdere locaties werken zoals verwacht. 

In dit voorbeeld moet u twee nieuwe regels maken en verwijderen van de standaardregel die is gemaakt.  U kunt toevoegen met de regel met behulp van [az netwerk toepassingsgateway regel maken](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoComRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoComListener \
  --rule-type Basic \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoOrgRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoOrgListener \
  --rule-type Basic \
  --redirect-config orgToCom
az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Maken van virtuele-machineschaalsets

In dit voorbeeld maakt u een virtuele-machineschaalset die ondersteuning biedt voor de back-endpool die u hebt gemaakt. De schaalaanpassingsset die u maakt met de naam *myvmss* en bevat twee virtuele machine-exemplaren op die u NGINX installeren.

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

Deze opdracht uitvoeren in het venster shell:

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

## <a name="create-cname-record-in-your-domain"></a>CNAME-record maken in uw domein

Nadat de toepassingsgateway is gemaakt met het openbare IP-adres, kunt u ophalen van de DNS-serveradres en een CNAME-record maken in uw domein. U kunt [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show) ophalen van de DNS-serveradres van de toepassingsgateway. Kopieer de *fqdn* waarde van de DNSSettings en deze gebruiken als de waarde van de CNAME-record die u maakt. Het gebruik van A-records wordt niet aanbevolen omdat het VIP kan worden gewijzigd wanneer de toepassingsgateway opnieuw wordt opgestart.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Voer de domeinnaam van uw in de adresbalk van uw browser. Zoals http://www.contoso.com.

![Testen van contoso-site in de toepassingsgateway](./media/redirect-internal-site-cli/application-gateway-nginxtest.png)

Wijzig het adres bijvoorbeeld aan het andere domein http://www.contoso.org en ziet u dat het verkeer is omgeleid naar de listener voor www.contoso.com.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> * Instellen van het netwerk
> * Een toepassingsgateway maken
> * Listeners en omleiding regel toevoegen
> * Een virtuele-machineschaalset ingesteld met de back-endpool maken
> * Een CNAME-record maken in uw domein
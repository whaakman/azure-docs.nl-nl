---
title: Een toepassingsgateway maken met meerdere hosting-site - Azure CLI | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway die als host fungeert voor meerdere sites met de Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 7728ade5f84cd97f3ff0cebd0d5545d68d0ebd79
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="create-an-application-gateway-with-multiple-site-hosting-using-the-azure-cli"></a>Een toepassingsgateway maken met meerdere hosting-site met de Azure CLI

U kunt de Azure CLI gebruiken voor het configureren van [hosting van meerdere websites](application-gateway-multi-site-overview.md) bij het maken van een [toepassingsgateway](application-gateway-introduction.md). In deze zelfstudie maakt u back-endpools met behulp van virtuele machines-schaalsets. Configureert u listeners en regels op basis van domeinnamen waarvan u eigenaar om ervoor te zorgen dat het webverkeer binnenkomt op de juiste servers van de groepen. Deze zelfstudie wordt ervan uitgegaan dat u voorbeelden van meerdere domeinen en maakt gebruik van de eigenaar *www.contoso.com* en *www.fabrikam.com*.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken
> * Listeners en routeringsregels maken
> * Virtuele-machineschaalsets met de back-end-adresgroepen maken
> * Een CNAME-record maken in uw domein

![Routering multi-site-voorbeeld](./media/tutorial-multisite-cli/scenario.png)

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

### <a name="add-the-backend-pools"></a>De back-endpools toevoegen

Toevoegen van de back-end-adresgroepen met de naam *contosoPool* en *fabrikamPool* die nodig zijn voor de back-endservers met bevatten [az netwerk toepassingsgateway adresgroep maken](/cli/azure/application-gateway#az_network_application_gateway_address_pool_create).

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name contosoPool
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name fabrikamPool
```

### <a name="add-listeners"></a>Listeners toevoegen

Een listener is vereist voor het inschakelen van de toepassingsgateway voor het verkeer op de juiste wijze te routeren naar de back-endpool. In deze zelfstudie maakt u twee listeners voor de twee domeinen. In dit voorbeeld listeners worden gemaakt voor de domeinen van *www.contoso.com* en *www.fabrikam.com*. 

Voeg de listeners met de naam *contosoListener* en *fabrikamListener* die nodig zijn voor het routeren van verkeer met behulp van [az netwerk toepassingsgateway http-listener maken](/cli/azure/application-gateway#az_network_application_gateway_http_listener_create).

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com
az network application-gateway http-listener create \
  --name fabrikamListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.fabrikam.com   
  ```

### <a name="add-routing-rules"></a>Routeringsregels toevoegen

Regels worden verwerkt in de volgorde waarin ze zijn gemaakt en verkeer wordt omgeleid naar de toepassingsgateway met behulp van de eerste regel die overeenkomt met de URL verzonden. Bijvoorbeeld, als u een regel met behulp van een basic-listener en een regel met meerdere sites listener beide op dezelfde poort hebt, kan de regel met de listener voor meerdere locaties moet worden vermeld voordat u de regel met de basic-listener in volgorde voor de regel voor meerdere locaties werken zoals verwacht. 

In dit voorbeeld moet u twee nieuwe regels maken en verwijderen van de standaardregel die is gemaakt tijdens het maken van de toepassingsgateway. U kunt toevoegen met de regel met behulp van [az netwerk toepassingsgateway regel maken](/cli/azure/application-gateway#az_network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoListener \
  --rule-type Basic \
  --address-pool contosoPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name fabrikamRule \
  --resource-group myResourceGroupAG \
  --http-listener fabrikamListener \
  --rule-type Basic \
  --address-pool fabrikamPool
az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Maken van virtuele-machineschaalsets

In dit voorbeeld maakt u drie virtuele-machineschaalsets die ondersteuning bieden voor de drie back-endpools in de toepassingsgateway. De naam van de schaalsets die u maakt *myvmss1*, *myvmss2*, en *myvmss3*. Elke scale set bestaat uit twee virtuele machine-exemplaren op die u NGINX installeren.

```azurecli-interactive
for i in `seq 1 2`; do
  if [ $i -eq 1 ]
  then
    poolName="contosoPool"
  fi
  if [ $i -eq 2 ]
  then
    poolName="fabrikamPool"
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
for i in `seq 1 2`; do
  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{
  "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
done
```

## <a name="create-a-cname-record-in-your-domain"></a>Een CNAME-record maken in uw domein

Nadat de toepassingsgateway is gemaakt met het openbare IP-adres, kunt u ophalen van de DNS-serveradres en een CNAME-record maken in uw domein. U kunt [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show) ophalen van de DNS-serveradres van de toepassingsgateway. Kopieer de *fqdn* waarde van de DNSSettings en deze gebruiken als de waarde van de CNAME-record die u maakt. 

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

Het gebruik van A-records wordt niet aanbevolen omdat het VIP kan worden gewijzigd wanneer de toepassingsgateway opnieuw wordt opgestart.

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Voer de domeinnaam van uw in de adresbalk van uw browser. Zoals http://www.contoso.com.

![Testen van contoso-site in de toepassingsgateway](./media/tutorial-multisite-cli/application-gateway-nginxtest1.png)

Wijzig het adres in het andere domein en u ziet ongeveer het volgende voorbeeld:

![Fabrikam site in de toepassingsgateway testen](./media/tutorial-multisite-cli/application-gateway-nginxtest2.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken
> * Listeners en routeringsregels maken
> * Virtuele-machineschaalsets met de back-end-adresgroepen maken
> * Een CNAME-record maken in uw domein

> [!div class="nextstepaction"]
> [Meer informatie over wat u met application gateway doen kunt](application-gateway-introduction.md)
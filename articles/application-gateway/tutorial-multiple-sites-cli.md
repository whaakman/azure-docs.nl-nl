---
title: Een toepassingsgateway maken waarop meerdere websites worden gehost - Azure CLI
description: Informatie over het maken van een toepassingsgateway waarop meerdere websites worden gehost met Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: b1f1bb268223eb55270ff5dab5dbb346f6299d09
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856244"
---
# <a name="tutorial-create-an-application-gateway-that-hosts-multiple-web-sites-using-the-azure-cli"></a>Zelfstudie: Een toepassingsgateway maken waarop meerdere websites worden gehost met Azure CLI

U kunt Azure CLI gebruiken om [het hosten van meerdere websites](multiple-site-overview.md) te configureren als u een [toepassingsgateway maakt](overview.md). In deze zelfstudie definieert u back-endadresgroepen met behulp van schaalsets voor virtuele machines. Vervolgens configureert u listeners en regels op basis van domeinen waarvan u eigenaar bent om er zeker van te zijn dat webverkeer bij de juiste servers in de pools binnenkomen. In deze zelfstudie wordt ervan uitgegaan dat u eigenaar bent van meerdere domeinen en voorbeelden gebruikt van *www.contoso.com* en *www.fabrikam.com*.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een toepassingsgateway maken
> * Back-endlisteners maken
> * Routeringsregels maken
> * Schaalsets voor virtuele machines maken met de back-endpools
> * Een CNAME-record in uw domein maken

![Voorbeeld van routeren voor meerdere sites](./media/tutorial-multiple-sites-cli/scenario.png)


U kunt deze zelfstudie desgewenst volgen met behulp van [Azure PowerShell](tutorial-multiple-sites-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#create).

In het volgende voorbeeld wordt de resourcegroep *myResourceGroupAG* gemaakt op de locatie *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 

Maak het virtuele netwerk en de subset *myAGSubnet* met [az network vnet create](/cli/azure/network/vnet#az-net). Vervolgens kunt u het subnet dat voor de back-endservers vereist is, toevoegen met [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create). Maak het openbare IP-adres*myAGPublicIPAddress* met [az network public-ip create](/cli/azure/network/public-ip#az-network_public_ip_create).

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

U kunt [az network application-gateway create](/cli/azure/network/application-gateway#create) gebruiken om de toepassingsgateway te maken. Als u een toepassingsgateway met de Azure CLI maakt, geeft u configuratiegegevens op, zoals capaciteit, SKU en HTTP-instellingen. De toepassingsgateway wordt toegewezen aan *myAGSubnet* en *myAGPublicIPAddress*, die u zojuist hebt gemaakt. 

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

Het kan enkele minuten duren voordat de toepassingsgateway is gemaakt. Nadat de toepassingsgateway is gemaakt, kunt u de volgende nieuwe functies ervan zien:

- *appGatewayBackendPool*: een toepassingsgateway moet ten minste één back-endadresgroep hebben.
- *appGatewayBackendHttpSettings*: hiermee wordt aangegeven dat voor de communicatie poort 80 en een HTTP-protocol worden gebruikt.
- *appGatewayHttpListener*: de standaard-listener die aan *appGatewayBackendPool* is gekoppeld.
- *appGatewayFrontendIP*: hiermee wordt *myAGPublicIPAddress* aan *appGatewayHttpListener* toegewezen.
- *rule1*: de standaardrouteringsregel die aan *appGatewayHttpListener* is gekoppeld.

### <a name="add-the-backend-pools"></a>Back-endpools toevoegen

Voeg de back-endpools toe die de back-endservers moeten bevatten met [az network application-gateway address-pool create](/cli/azure/network/application-gateway#az-network_application_gateway_address_pool_create).

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

### <a name="add-backend-listeners"></a>Back-endlisteners toevoegen

Voeg de back-endlisteners, die voor het omleiden van verkeer nodig zijn, toe met [az network application-gateway http-listener create](/cli/azure/network/application-gateway).

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

Regels worden afgehandeld in de volgorde waarop ze staan vermeld. Verkeer wordt omgeleid volgens de eerste regel die overeenkomt, ongeacht de specificiteit. Als u bijvoorbeeld een regel hebt die van een basislistener gebruikmaakt en een regel die via dezelfde poort van een listener voor meerdere sites gebruikmaakt, moet de regel voor de listener voor meerdere sites vermeld worden vóór de regel met de basislistener, opdat de regel voor meerdere sites kan functioneren zoals het hoort. 

In dit voorbeeld maakt u twee nieuwe regels en verwijdert u de standaardregel die is gemaakt toen u de toepassingsgateway maakte. U kunt de regel toevoegen met [az network application-gateway rule create](/cli/azure/network/application-gateway).

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

## <a name="create-virtual-machine-scale-sets"></a>Schaalsets voor virtuele machines maken

In dit voorbeeld maakt u drie schaalsets voor virtuele machines die ondersteuning bieden voor de drie back-end-pools in de toepassingsgateway. De schaalsets die u maakt, hebben de namen *myvmss1*, *myvmss2* en *myvmss3*. Elke schaalset bevat twee instanties van virtuele machines waarop u IIS installeert.

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
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'

done
```

## <a name="create-a-cname-record-in-your-domain"></a>Een CNAME-record in uw domein maken

Als de toepassingsgateway met het bijbehorende IP-adres is gemaakt, kunt u het DNS-adres ophalen en dit gebruiken om een CNAME-record in uw domein te maken. Gebruik [az network public-ip show](/cli/azure/network/public-ip#az-network_public_ip_show) om het DNS-adres van de toepassingsgateway op te halen. Kopieer de waarde *fqdn* van DNSSettings en gebruik deze als de waarde van de CNAME-record die u maakt. 

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

Het gebruik van A-records wordt niet aanbevolen, omdat de VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

## <a name="test-the-application-gateway"></a>Toepassingsgateway testen

Voer uw domeinnaam in de adresbalk van de browser in. Bijvoorbeeld http://www.contoso.com.

![Contoso-site testen in toepassingsgateway](./media/tutorial-multiple-sites-cli/application-gateway-nginxtest1.png)

Wijzig het adres in uw andere domein. U krijgt iets te zien zoals in het volgende voorbeeld:

![Fabrikam-site testen in toepassingsgateway](./media/tutorial-multiple-sites-cli/application-gateway-nginxtest2.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de resourcegroep, de toepassingsgateway en alle gerelateerde resources verwijderen als u deze niet meer nodig hebt.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een toepassingsgateway maken
> * Back-endlisteners maken
> * Routeringsregels maken
> * Schaalsets voor virtuele machines maken met de back-endpools
> * Een CNAME-record in uw domein maken

> [!div class="nextstepaction"]
> [Een toepassingsgateway maken met omleidingsregels op basis van een URL-pad](./tutorial-url-route-cli.md)

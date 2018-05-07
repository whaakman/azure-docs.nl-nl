---
title: Een toepassingsgateway maken met een web application firewall - Azure CLI | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met web application firewall via de Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.openlocfilehash: 87125b68c81af07d0ecd9693fdf7e2dc00a93324
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-cli"></a>Een toepassingsgateway maken met een web application firewall met de Azure CLI

U kunt de Azure CLI gebruiken voor het maken een [toepassingsgateway](application-gateway-introduction.md) met een [web application firewall](application-gateway-web-application-firewall-overview.md) (WAF) die gebruikmaakt van een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Het gebruik van WAF [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) regels voor het beveiligen van uw toepassing. Deze regels omvatten bescherming tegen aanvallen, zoals SQL-injectie, cross-site scripting aanvallen en sessie hijacks. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken met WAF ingeschakeld
> * Een virtuele-machineschaalset maken
> * Een opslagaccount maken en configureren van diagnostische gegevens

![Web application firewall voorbeeld](./media/application-gateway-web-application-firewall-cli/scenario-waf.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een Azure-resourcegroep met de naam *myResourceGroupAG* met [az groep maken](/cli/azure/group#az_group_create).

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken

Het virtuele netwerk en subnetten worden gebruikt voor het bieden van de netwerkverbinding met de toepassingsgateway en de bijbehorende bronnen. Maken van het virtuele netwerk met de naam *myVNet* en subnet met de naam *myAGSubnet* met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create) en [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Maken van een openbaar IP-adres met de naam *myAGPublicIPAddress* met [az netwerk openbare ip-maken](/cli/azure/network/public-ip#az_network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myBackendSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myAGSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway-with-a-waf"></a>Een toepassingsgateway maken met een WAF

U kunt [az netwerk toepassingsgateway maken](/cli/azure/application-gateway#az_application_gateway_create) voor het maken van de toepassingsgateway met de naam *myAppGateway*. Wanneer u een toepassingsgateway met Azure CLI maakt, kunt u configuratie-informatie, zoals de capaciteit, sku en HTTP-instellingen opgeven. De toepassingsgateway is toegewezen aan *myAGSubnet* en *myPublicIPSddress* die u eerder hebt gemaakt.

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --capacity 2 \
  --sku WAF_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
az network application-gateway waf-config set \
  --enabled true \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --firewall-mode Detection \
  --rule-set-version 3.0
```

Duurt enkele minuten voor de toepassingsgateway moet worden gemaakt. Nadat de toepassingsgateway is gemaakt, kunt u deze nieuwe functies ervan zien:

- *appGatewayBackendPool* -een toepassingsgateway moet ten minste één back-endadresgroep hebben.
- *appGatewayBackendHttpSettings* -Hiermee geeft u op poort 80 en een HTTP-protocol wordt gebruikt voor communicatie.
- *appGatewayHttpListener* -de standaard-listener die zijn gekoppeld aan *appGatewayBackendPool*.
- *appGatewayFrontendIP* -wijst *myAGPublicIPAddress* naar *appGatewayHttpListener*.
- *Rule1* : de regel die is gekoppeld aan routering standaard *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset maken

In dit voorbeeld maakt u een virtuele-machineschaalset met twee servers voor de back endpool in de toepassingsgateway. De virtuele machines in de schaalset zijn gekoppeld aan de *myBackendSubnet* subnet. Maken van de schaal ingesteld, kunt u [az vmss maken](/cli/azure/vmss#az_vmss_create).

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
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/vhorne/samplescripts/master/install_nginx.sh"],"commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Een opslagaccount maken en configureren van diagnostische gegevens

In deze zelfstudie gebruikt de toepassingsgateway een opslagaccount voor het opslaan van gegevens voor detectie en preventie doeleinden. U kunt ook logboekanalyse of Event Hub gebruiken om gegevens te noteren. 

### <a name="create-a-storage-account"></a>Create a storage account

Maken van een opslagaccount met de naam *myagstore1* met [az storage-account maken](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create).

```azurecli-interactive
az storage account create \
  --name myagstore1 \
  --resource-group myResourceGroupAG \
  --location eastus \
  --sku Standard_LRS \
  --encryption blob
```

### <a name="configure-diagnostics"></a>Diagnostische gegevens configureren

Diagnostische gegevens te registreren in de logboeken ApplicationGatewayAccessLog ApplicationGatewayPerformanceLog en ApplicationGatewayFirewallLog configureert. Vervang `<subscriptionId>` met uw abonnements-id en configureer vervolgens de diagnostische gegevens met [az monitor diagnose-instellingen maken](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_create).

```azurecli-interactive
appgwid=$(az network application-gateway show --name myAppGateway --resource-group myResourceGroupAG --query id -o tsv)
storeid=$(az storage account show --name myagstore1 --resource-group myResourceGroupAG --query id -o tsv)
az monitor diagnostic-settings create --name appgwdiag --resource $appgwid \
  --logs '[ { "category": "ApplicationGatewayAccessLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayPerformanceLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayFirewallLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --storage-account $storeid
```

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Als u het openbare IP-adres van de toepassingsgateway, gebruikt [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show). Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Basis-URL te testen in de toepassingsgateway](./media/application-gateway-web-application-firewall-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Instellen van het netwerk
> * Een toepassingsgateway maken met WAF ingeschakeld
> * Een virtuele-machineschaalset maken
> * Een opslagaccount maken en configureren van diagnostische gegevens

Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.
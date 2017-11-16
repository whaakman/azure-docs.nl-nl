---
title: Een toepassingsgateway - Azure CLI 2.0 maken | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met behulp van de Azure CLI 2.0 in Resource Manager.
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 9d3732d538f3ed9ecb87247f378a3736692025ca
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Een toepassingsgateway maken met behulp van de Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een toegewezen virtueel apparaat die toepassing levering controller (ADC) biedt een service en biedt u verschillende laag 7 taakverdeling mogelijkheden voor uw toepassing.

## <a name="cli-versions"></a>CLI-versies

U kunt een toepassingsgateway maken met behulp van een van de volgende versies van de opdrachtregelinterface (CLI):

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md): Azure CLI voor het klassieke en Azure Resource Manager-implementatiemodel
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md): Next generation CLI voor het Resource Manager-implementatiemodel

## <a name="prerequisite-install-the-azure-cli-20"></a>Voorwaarde: Installeer de Azure CLI 2.0

Als u wilt de stappen in dit artikel uitvoert, moet u [Azure CLI voor Mac OS-, Linux- en Windows installeren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> U moet een Azure-account om een toepassingsgateway te maken. Als u niet hebt, zich aanmelden voor een [gratis proefversie](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway maken met behulp van de Azure-portal.

Dit scenario wordt:

* Een middelgrote toepassingsgateway maken met twee exemplaren.
* Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerd CIDR-blok van 10.0.0.0/16.
* Maakt u een subnet met de naam Appgatewaysubnet dat gebruikmaakt van 10.0.0.0/28 als CIDR-blok.

> [!NOTE]
> Verdere configuratie van de toepassingsgateway, inclusief aangepaste statuscontroles, back-end-adressen en extra regels wordt uitgevoerd nadat de toepassingsgateway is gemaakt en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Een application gateway vereist een eigen subnet. Wanneer u een virtueel netwerk maakt, moet u ervoor zorgen dat u onvoldoende adresruimte voor meerdere subnetten laat. Nadat u een toepassingsgateway met een subnet implementeert, kunt u alleen extra Toepassingsgateways op dat subnet toevoegen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de **Microsoft Azure-opdrachtprompt** en meld u aan:

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> U kunt ook `az login` zonder dat de switch voor apparaat aanmelding waarvoor een code op aka.ms/devicelogin invoert.

Nadat u de voorgaande opdracht invoert, ontvangt u een code. Ga naar https://aka.ms/devicelogin in een browser om door te gaan met het aanmelden.

![cmd tonen apparaat aanmelding][1]

Voer de code die u hebt ontvangen in de browser. Hiermee wordt u omgeleid naar een aanmeldingspagina.

![browser-code invoeren][2]

Voer de code aan te melden en sluit vervolgens de browser om door te gaan.

![aangemeld][3]

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de toepassingsgateway maakt, maakt u een resourcegroep geplaatst. Gebruik de volgende opdracht:

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Gebruik de back-end-IP-adressen voor uw back-end-server IP-adressen. Deze waarden kunnen worden persoonlijke IP-adressen in het virtuele netwerk, openbare IP-adressen of volledig gekwalificeerde domeinnamen voor uw back-endservers. Het volgende voorbeeld maakt een toepassingsgateway met aanvullende configuraties voor HTTP-instellingen, poorten en regels:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

Het vorige voorbeeld ziet u diverse eigenschappen die niet vereist zijn tijdens het maken van een toepassingsgateway. Het volgende codevoorbeeld maakt een toepassingsgateway met de vereiste informatie:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> Voor een lijst van parameters moeten worden gebruikt tijdens het maken van de volgende opdracht uitvoeren: `az network application-gateway create --help`.

In dit voorbeeld maakt een basic-toepassingsgateway met standaardinstellingen voor de listener, back-end-pool, back-end-HTTP-instellingen en -regels. U kunt deze instellingen aanpassen aan uw implementatie nadat het inrichten voltooid is, kunt wijzigen.

Als u de webtoepassing is gedefinieerd met de back-end-pool in de voorgaande stappen hebt uitgevoerd, begint taakverdeling nu.

## <a name="get-the-application-gateway-dns-name"></a>Ophalen van de DNS-naam van de toepassing-gateway
Nadat u de gateway maakt, configureert u de front-end voor communicatie. Als u een openbaar IP-adres, moet u de toepassingsgateway een dynamisch toegewezen DNS-naam, geen beschrijvende is. Gebruik een CNAME-record om te verwijzen naar het openbare eindpunt van de toepassingsgateway zodat gebruikers kunnen de toepassingsgateway bereikt. Zie voor meer informatie [gebruik Azure DNS-instellingen van aangepast domein voor een Azure-service kan leveren](../dns/dns-custom-domain.md).

Voor het configureren van een alias ophalen van gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de PublicIPAddress-element dat is gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen aan deze DNS-naam wijst. Geen gebruik A-records omdat het VIP kan worden gewijzigd op opnieuw starten van de toepassingsgateway.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Voer de volgende opdracht voor het verwijderen van alle resources in dit artikel is gemaakt:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het maken van aangepaste statuscontroles, gaat u naar [een aangepaste test voor de toepassingsgateway maken met behulp van de portal](application-gateway-create-probe-portal.md).

Zie voor informatie over het configureren van SSL-offloading en nemen de kostbare SSL-ontsleuteling uit uw webservers, [een toepassingsgateway voor SSL-offload configureren met behulp van Azure Resource Manager](application-gateway-ssl-arm.md).

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png

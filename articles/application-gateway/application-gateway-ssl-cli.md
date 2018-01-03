---
title: Configureren van SSL-offload - Azure Application Gateway - Azure CLI 2.0 | Microsoft Docs
description: Dit artikel bevat instructies voor het maken van een toepassingsgateway met SSL-offload met behulp van Azure CLI 2.0
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 4bdca33dae2ce52fdeccdae9a67abb6667593f9d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a>Een toepassingsgateway voor SSL-offload configureren met behulp van Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan zodanig worden geconfigureerd dat de Secure Sockets Layer-sessie (SSL) wordt beëindigd bij de gateway om kostbare SSL-ontsleutelingstaken te voorkomen die worden uitgevoerd in de webfarm. SSL-offload vereenvoudigt ook Certificaatbeheer op de front-end-server.

## <a name="prerequisite-install-the-azure-cli-20"></a>Voorwaarde: Installeer de Azure CLI 2.0

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="required-components"></a>Vereiste onderdelen

* **Back-endserverpool**: de lijst met IP-adressen van de back-endservers. De IP-adressen die worden vermeld moeten deel uitmaken van het subnet van het virtuele netwerk of moeten een openbaar IP-adres of een virtueel IP-adres (VIP).
* **Back-endserverpoolinstellingen**: elke pool heeft instellingen, zoals poort, het protocol en cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort**: dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener**: de listener beschikt over een front-endpoort, een protocol (Http of Https; deze instellingen zijn hoofdlettergevoelig), en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel**: de regel verbindt de listener met de back-endserverpool en definieert naar welke back-endserverpool om het verkeer naar wanneer dit bij een bepaalde listener aankomt. Momenteel wordt alleen de regel *basic* ondersteund. De regel *basic* is een vorm van round-robinbelastingverdeling.

**Aanvullende configuratieopmerkingen**

Voor het configureren van SSL-certificaten moet het protocol in **HttpListener** worden gewijzigd in *Https* (hoofdlettergevoelig). Voeg de **SslCertificate** element op de **HttpListener** met de variabele waarde die is geconfigureerd voor het SSL-certificaat. De front-endpoort moet worden bijgewerkt naar **443**.

**Inschakelen van cookies gebaseerde affiniteit**: U kunt een toepassingsgateway om ervoor te zorgen dat een aanvraag van een clientsessie altijd wordt omgeleid naar dezelfde virtuele machine in de webfarm configureren. Voeg hiervoor een sessiecookie waarmee de gateway om verkeer te regelen op de juiste wijze. Als u op cookies gebaseerde affiniteit wilt inschakelen, stelt u **CookieBasedAffinity** in op *Enabled* in het element **BackendHttpSettings**.

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a>SSL-offload configureren op een bestaande application gateway

Voer de volgende opdrachten voor het configureren van SSL-offload naar een bestaande toepassingsgateway:

```azurecli-interactive
#!/bin/bash

# Create a new front end port to be used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload the .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing the port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool to be used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using the new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking the listener to the back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a>Een toepassingsgateway met SSL-offload maken

Het volgende voorbeeld maakt een toepassingsgateway met SSL-offload. Het certificaat en het wachtwoord voor certificaat moeten worden bijgewerkt naar een geldige persoonlijke sleutel.

```azurecli-interactive
#!/bin/bash

# Creates an application gateway with SSL offload
az network application-gateway create \
  --name "AdatumAppGateway3" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG2" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet" \
  --subnet-address-prefix "10.0.0.0/28" \
  --frontend-port 443 \
  --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 \
  --sku "Standard_Small" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip" \
  --public-ip-address-allocation "dynamic"
```

## <a name="get-an-application-gateway-dns-name"></a>Ophalen van een application gateway DNS-naam

Nadat de gateway is gemaakt, wordt de volgende stap is het front-end voor de communicatie te configureren.  Application Gateway is een dynamisch toegewezen DNS-naam vereist wanneer u een openbaar IP-adres, dit is geen beschrijvende. U kunt een CNAME-record gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway zodat eindgebruikers kunnen de toepassingsgateway bereikt. Zie voor meer informatie [een aangepaste domeinnaam configureren in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Voor het configureren van een alias ophalen van gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de **PublicIPAddress** element gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen aan deze DNS-naam wijst. Niet aanbevolen het gebruik van A-records omdat het VIP kunt wijzigen op opnieuw starten van de toepassingsgateway.


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

## <a name="next-steps"></a>Volgende stappen

Als u configureren van een toepassingsgateway wilt voor gebruik met een interne load balancer, Zie [een toepassingsgateway maken met een interne load balancer](application-gateway-ilb.md).

Zie voor meer informatie over opties voor taakverdeling in het algemeen:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

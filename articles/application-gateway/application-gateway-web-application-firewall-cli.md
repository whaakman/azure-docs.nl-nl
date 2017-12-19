---
title: 'Web application firewall configureren: Azure Application Gateway | Microsoft Docs'
description: In dit artikel biedt richtlijnen voor het gebruik van een web application firewall op een bestaande of nieuwe application gateway.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: davidmu
ms.openlocfilehash: e60bfc89378569b154f4f973d1dceb683fa58482
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Web application firewall configureren op een nieuwe of bestaande toepassingsgateway met Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Informatie over het maken van een web application firewall (WAF)-toepassingsgateway ingeschakeld. Leert ook hoe een WAF toevoegen aan een bestaande toepassingsgateway.

De WAF in Azure Application Gateway beveiligt webtoepassingen van algemene web gebaseerde aanvallen, zoals SQL-injectie, cross-site scripting aanvallen en sessie hijacks.

 Application Gateway is een load balancer laag 7. Het biedt failover, HTTP-aanvragen routeren tussen verschillende servers, of ze zich op de cloud of on-premises. Toepassingsgateway biedt veel functies van de toepassing levering netwerkcontroller (ADC):

 * HTTP-taakverdeling 
 * Sessie cookies gebaseerde affiniteit 
 * Secure Sockets Layer (SSL)-offload 
 * Aangepaste statuscontroles 
 * Ondersteuning voor functionaliteit voor meerdere locaties
 
 Een volledige lijst met ondersteunde functies Zie [overzicht van toepassingsgateway](application-gateway-introduction.md).

Dit artikel laat zien hoe u [web application firewall toevoegen aan een bestaande toepassingsgateway](#add-web-application-firewall-to-an-existing-application-gateway). U ziet ook hoe [maken van een toepassingsgateway die gebruikmaakt van een web application firewall](#create-an-application-gateway-with-web-application-firewall).

![afbeelding van scenario][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Voorwaarde: Installeer de Azure CLI 2.0

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface (Azure CLI) voor Mac, Linux en Windows](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>WAF configuratie verschillen

Als u hebt gelezen [een toepassingsgateway maken met Azure CLI](application-gateway-create-gateway-cli.md), u inzicht in de SKU-instellingen configureren wanneer u een toepassingsgateway maken. De WAF biedt aanvullende instellingen op te geven wanneer u de SKU op een application gateway configureert. Er zijn geen aanvullende wijzigingen die u in de toepassingsgateway zelf aanbrengt.

| **Instelling** | **Details**
|---|---|
|**SKU** |Biedt ondersteuning voor een normale toepassingsgateway zonder een WAF **standaard\_kleine**, **standaard\_gemiddeld**, en **standaard\_grote**grootten. Dankzij de introductie van een WAF, er zijn twee extra SKU's **WAF\_gemiddeld** en **WAF\_grote**. Een WAF wordt niet ondersteund op kleine Toepassingsgateways.|
|**Modus** | Deze instelling wordt de modus van de WAF. toegestane waarden zijn **detectie** en **preventie**. Wanneer de WAF is ingesteld in **detectie** modus alle bedreigingen zijn opgeslagen in een logboekbestand. In **preventie** modus, gebeurtenissen worden nog wel geregistreerd, maar de aanvaller ontvangt een niet-geautoriseerde 403 reactie van de toepassingsgateway.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Web application firewall toevoegen aan een bestaande application gateway

De volgende opdracht wordt een bestaande standaard application gateway met een gateway WAF-toepassing gewijzigd:

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Met deze opdracht werkt de toepassingsgateway met een WAF. Zie voor informatie over het weergeven van Logboeken voor uw toepassingsgateway, [Application Gateway diagnostics](application-gateway-diagnostics.md). Vanwege de aard van de beveiliging van een WAF, Bekijk Logboeken regelmatig om te begrijpen van de beveiligingsstatus van uw webtoepassingen.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Een toepassingsgateway maken met een web application firewall

De volgende opdracht maakt een toepassingsgateway met een WAF:

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> Toepassingsgateways gemaakt met de basisconfiguratie van WAF zijn geconfigureerd met CRS 3.0 voor beveiliging.

## <a name="get-an-application-gateway-dns-name"></a>Ophalen van een application gateway DNS-naam

Nadat de gateway is gemaakt, wordt de volgende stap is het front-end voor de communicatie te configureren. Wanneer u een openbaar IP-adres gebruikt, moet de toepassingsgateway een dynamisch toegewezen DNS-naam, die geen beschrijvende. Om ervoor te zorgen dat gebruikers de toepassingsgateway kunnen bereikt, een CNAME-record te gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway. Zie voor meer informatie [een aangepaste domeinnaam configureren voor een Azure cloudservice](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Voor het configureren van een CNAME-record ophalen van gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de PublicIPAddress-element dat is gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen aan deze DNS-naam wijst. We raden niet met behulp van A-records, omdat het VIP kan worden gewijzigd wanneer de toepassingsgateway opnieuw wordt opgestart.

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

Zie voor meer informatie over het aanpassen van WAF regels, [web application firewall-regels via de Azure CLI 2.0 aanpassen](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png

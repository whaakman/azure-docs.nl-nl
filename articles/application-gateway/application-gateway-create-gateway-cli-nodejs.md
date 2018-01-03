---
title: Maken van een Azure Application Gateway - Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met behulp van de Azure CLI 1.0 in Resource Manager
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: fe50fb3a7434702101dc5ae7a9dd176a33423119
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Een toepassingsgateway maken met behulp van de Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)
> 
> 

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Toepassingsgateway bevat de volgende functies van de toepassing-levering: HTTP load balancing, sessie cookies gebaseerde affiniteit en Secure Sockets Layer (SSL)-offload, aangepaste statuscontroles en ondersteuning voor meerdere locaties.

## <a name="prerequisite-install-the-azure-cli"></a>Voorwaarde: Installeer de Azure CLI

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](../xplat-cli-install.md) en u moet [Meld u aan bij Azure](/cli/azure/authenticate-azure-cli). 

> [!NOTE]
> Als u geen Azure-account hebt, moet u een. U kunt zich [hier aanmelden voor een gratis proefversie](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenario

In dit scenario wordt informatie over het maken van een toepassingsgateway met Azure portal.

Dit scenario wordt:

* Een middelgrote toepassingsgateway maken met twee exemplaren.
* Maak een virtueel netwerk met de naam ContosoVNET met een gereserveerd CIDR-blok van 10.0.0.0/16.
* Maak een subnet met de naam subnet01 dat gebruikmaakt van 10.0.0.0/28 als CIDR-blok.

> [!NOTE]
> Aanvullende configuratie van de toepassingsgateway, met inbegrip van aangepaste health tests, adressen van de groep back-end en extra regels worden geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure Application Gateway vereist een eigen subnet. Zorg ervoor dat u onvoldoende adresruimte voor meerdere subnetten hebben laten bij het maken van een virtueel netwerk. Wanneer u een toepassingsgateway met een subnet implementeert, kunnen alleen andere Toepassingsgateways worden toegevoegd aan het subnet.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Open de **Microsoft Azure-opdrachtprompt**, en zich aanmelden. 

```azurecli-interactive
azure login
```

Wanneer u in het voorgaande voorbeeld typt, wordt een code opgegeven. Ga naar https://aka.ms/devicelogin in een browser om terug te gaan met de aanmelding.

![cmd tonen apparaat aanmelding][1]

Voer de code die u hebt ontvangen in de browser. U wordt omgeleid naar een aanmeldingspagina.

![browser-code invoeren][2]

Zodra de code is ingevoerd. u bent aangemeld, sluit de browser om door te gaan op met het scenario.

![aangemeld][3]

## <a name="switch-to-resource-manager-mode"></a>Overschakelen naar de modus Resource Manager

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de toepassingsgateway maakt, wordt een resourcegroep gemaakt voor de toepassingsgateway. Hieronder ziet u de opdracht.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Zodra de resourcegroep is gemaakt, wordt een virtueel netwerk gemaakt voor de toepassingsgateway.  In het volgende voorbeeld is de adresruimte als 10.0.0.0/16 zoals gedefinieerd in de notities bij de voorgaande scenario.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Een subnet maken

Nadat het virtuele netwerk is gemaakt, wordt een subnet voor de toepassingsgateway toegevoegd.  Als u van plan bent gebruik toepassingsgateway met een web-app gehost in hetzelfde virtuele netwerk als de toepassingsgateway, zorg er dan voor dat onvoldoende ruimte is voor een ander subnet.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Als het virtuele netwerk en het subnet zijn gemaakt, zijn de vereisten voor de toepassingsgateway zijn voltooid. Bovendien een eerder geëxporteerde pfx-certificaat en het wachtwoord voor het certificaat zijn vereist voor de volgende stap: het IP-adressen gebruikt voor de back-end zijn de IP-adressen voor uw back-endserver. Deze waarden kunnen worden persoonlijke IP-adressen in het virtuele netwerk, openbare IP-adressen of volledig gekwalificeerde domeinnamen voor uw back-endservers.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Voor een lijst met parameters die kan worden opgegeven tijdens het maken van de volgende opdracht uitvoeren: **netwerk van azure-toepassingsgateway maken--help**.

In dit voorbeeld maakt een basic-toepassingsgateway met standaardinstellingen voor de listener, back-endpool, back-end-http-instellingen en -regels. U kunt deze instellingen aanpassen aan uw implementatie wanneer het inrichten voltooid is, kunt wijzigen.
Als u al uw webtoepassing gedefinieerd met behulp van de back endpool in de voorgaande stappen hebt uitgevoerd, eenmaal is gemaakt, begint taakverdeling.

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van aangepaste statuscontroles in via [een aangepaste health test maken](application-gateway-create-probe-portal.md)

Informatie over het configureren van SSL-Offloading en nemen de kostbare SSL-ontsleuteling uit uw webservers in via [SSL-Offload configureren](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png

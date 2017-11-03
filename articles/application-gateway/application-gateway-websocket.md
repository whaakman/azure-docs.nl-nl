---
title: Ondersteuning voor WebSocket in Azure Application Gateway | Microsoft Docs
description: Deze pagina bevat een overzicht van de toepassing Gateway WebSocket-ondersteuning.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: amsriva
ms.openlocfilehash: 75b06ddd02da231b7813c609c848c75e42116da5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Overzicht van WebSocket-ondersteuning in Application Gateway

Toepassingsgateway biedt systeemeigen ondersteuning voor WebSocket over alle gateway-grootte. Er is geen gebruiker configureerbare instelling selectief of WebSocket ondersteuning uitschakelen. 

WebSocket-protocol in gestandaardiseerd [RFC6455](https://tools.ietf.org/html/rfc6455) een full-duplex communicatie tussen een server en een client via een langdurige TCP-verbinding worden ingeschakeld. Deze functie kunt u een nieuwe interactieve communicatie tussen de webserver en de client, die bidirectionele zonder de noodzaak voor polling als vereiste in implementaties op basis van HTTP worden kan. WebSocket heeft weinig overhead in tegenstelling tot HTTP en dezelfde TCP-verbinding voor meerdere aanvragen/antwoorden, wat resulteert in een efficiënter gebruik van bronnen kan gebruiken. WebSocket-protocollen zijn ontworpen om te werken via traditionele HTTP-poorten 80 en 443.

U kunt doorgaan met behulp van standaard HTTP-listener op poort 80 of 443 WebSocket-verkeer ontvangen. WebSocket-verkeer wordt vervolgens omgeleid naar de WebSocket-ingeschakelde back-endserver met de juiste back-endpool is opgegeven in de regels voor application gateway. De back-endserver moet reageren op de application gateway tests die worden beschreven in de [health test overzicht](application-gateway-probe-overview.md) sectie. Statuscontroles van Application gateway zijn alleen HTTP/HTTPS. Elke back-endserver moet reageren op HTTP-tests voor de toepassingsgateway WebSocket om gegevensverkeer te routeren naar de server.

## <a name="listener-configuration-element"></a>Configuratie-element Listener

Een bestaande HTTP-listener kan worden gebruikt ter ondersteuning van WebSocket-verkeer. Hier volgt een fragment van een element vanuit een voorbeeldbestand van de sjabloon een httpListeners. U moet zowel HTTP als HTTPS-listeners ter ondersteuning van WebSocket en beveiliging van WebSocket-verkeer. Op dezelfde manier kunt u de [portal](application-gateway-create-gateway-portal.md) of [PowerShell](application-gateway-create-gateway-arm.md) een toepassingsgateway maken met listeners op poort 80/443 ter ondersteuning van WebSocket-verkeer.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Regelconfiguratie BackendAddressPool BackendHttpSetting en routering

Een BackendAddressPool wordt gebruikt voor het definiëren van een back-endpool met servers van WebSocket is ingeschakeld. De backendHttpSetting wordt gedefinieerd met een back endpoort 80 en 443. De eigenschappen voor cookies gebaseerde affiniteit en requestTimeouts zijn niet relevant zijn voor WebSocket-verkeer. Er is geen wijziging vereist in de regel voor het doorsturen, 'Basic' wordt gebruikt voor het koppelen van de juiste listener aan de bijbehorende back-end-adresgroep. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>Back-end van WebSocket is ingeschakeld

Uw back-end moet een HTTP/HTTPS-webserver uitgevoerd op de geconfigureerde poort (meestal 80/443) voor WebSocket om te werken. Deze vereiste is omdat de initiële handshake moet HTTP met een upgrade uit naar de WebSocket-protocol als een headerveld WebSocket-protocol is vereist. Hier volgt een voorbeeld van een koptekst:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Een andere reden hiervoor is dat back-end de statuscontrole van application gateway biedt ondersteuning voor HTTP en HTTPS-protocollen. Als de back-endserver niet op HTTP of HTTPS tests reageert, is deze buiten het back-endpool nodig.

## <a name="next-steps"></a>Volgende stappen

Na het leren over de WebSocket-ondersteuning gaat u naar [een toepassingsgateway maken](application-gateway-create-gateway.md) ingeschakeld om te beginnen met een WebSocket-webtoepassing.


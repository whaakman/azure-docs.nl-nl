---
title: WebSocket-ondersteuning in Azure Application Gateway | Microsoft Docs
description: Deze pagina bevat een overzicht van de Application Gateway WebSocket-ondersteuning.
author: amsriva
ms.author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 03/18/2019
ms.openlocfilehash: bae4b3d955076679a5640717ac6f5446e3951fb9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58168116"
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Overzicht van de WebSocket-ondersteuning in Application Gateway

Application Gateway biedt systeemeigen ondersteuning voor WebSocket over alle gateway-grootten. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen. 

WebSocket-protocol in gestandaardiseerd [RFC6455](https://tools.ietf.org/html/rfc6455) een full-duplex communicatie tussen een server en een client via een langlopende TCP-verbinding mogelijk maakt. Deze functie kunt voor een interactiever communicatie tussen de webserver en de client, die in twee richtingen zonder de noodzaak voor polling als vereiste in op basis van HTTP-implementaties worden kan. WebSocket heeft lage overhead in tegenstelling tot HTTP en dezelfde TCP-verbinding voor meerdere aanvragen/antwoorden, wat resulteert in een efficiënter gebruik van resources kunt hergebruiken. WebSocket-protocollen zijn ontworpen om te werken via traditionele HTTP-poorten 80 en 443.

U kunt met behulp van een standaard HTTP-listener op poort 80 of 443 om WebSocket-verkeer te ontvangen. WebSocket-verkeer wordt vervolgens omgeleid naar de WebSocket-ingeschakelde back-endserver met behulp van de juiste back-endpool die zijn opgegeven in regels voor application gateway. De back-endserver moet reageren op de application gateway-tests, die worden beschreven in de [health test overzicht](application-gateway-probe-overview.md) sectie. Application gateway statuscontroles zijn alleen HTTP/HTTPS. Elke back-endserver moet reageren op HTTP-tests voor application gateway voor het WebSocket-verkeer routeren naar de server.

Het wordt gebruikt in apps die baat bij snelle, realtime communicatie, zoals chat, dashboard en game-apps hebben.

## <a name="how-does-websocket-work"></a>Hoe werkt de WebSocket

Als u wilt een WebSocket-verbinding tot stand brengen, wordt een specifieke HTTP-gebaseerde handshake uitgewisseld tussen de client en de server. Als dit lukt, is application layer protocol 'bijgewerkt' van HTTP naar WebSockets, met behulp van de eerder vastgestelde TCP-verbinding. Wanneer dit gebeurt, is HTTP volledig buiten de afbeelding. gegevens kunnen worden verzonden of ontvangen met behulp van de WebSocket-protocol door beide eindpunten, totdat de WebSocket-verbinding is gesloten. 

![addcert](./media/application-gateway-websocket/websocket.png)

### <a name="listener-configuration-element"></a>Configuratie-element Listener

Een bestaande HTTP-listener kan worden gebruikt ter ondersteuning van WebSocket-verkeer. Hier volgt een fragment van een httpListeners-element van een voorbeeldbestand van de sjabloon. U moet zowel HTTP als HTTPS listeners ter ondersteuning van WebSocket en beveiligde WebSocket-verkeer. Op deze manier kunt u de [portal](application-gateway-create-gateway-portal.md) of [PowerShell](application-gateway-create-gateway-arm.md) voor het maken van een toepassingsgateway met listeners op poort 80/443 ter ondersteuning van WebSocket-verkeer.

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool BackendHttpSetting en routering regelconfiguratie

Een BackendAddressPool wordt gebruikt voor het definiëren van een back endadresgroep met ingeschakeld WebSocket-servers. De backendHttpSetting is gedefinieerd met een back endpoort 80 en 443. De eigenschappen voor cookies gebaseerde affiniteit en requestTimeouts zijn niet relevant zijn voor WebSocket-verkeer. Er is geen wijziging vereist in de regel voor doorsturen, 'Basic' wordt gebruikt voor het koppelen van de juiste listener aan de bijbehorende back-end-adresgroep. 

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

## <a name="websocket-enabled-backend"></a>Back-end WebSocket ingeschakeld

Uw back-end moet een HTTP/HTTPS-webserver die wordt uitgevoerd op de geconfigureerde poort (meestal 80/443) voor WebSocket om te werken. Deze vereiste is omdat WebSocket-protocol is vereist voor de eerste handshake moet HTTP met een upgrade uit naar de WebSocket-protocol als een headerveld. Hier volgt een voorbeeld van een koptekst:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: https://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Een andere reden hiervoor is dat statustest voor back-end van application gateway biedt ondersteuning voor HTTP en HTTPS-protocollen. Als de back-endserver niet op HTTP of HTTPS tests reageert, wordt het opgevat uit back endpool.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over de ondersteuning van WebSocket, gaat u naar [een toepassingsgateway maken](application-gateway-create-gateway.md) aan de slag met een WebSocket-webtoepassing.


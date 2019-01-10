---
title: Overzicht van routering van op URL gebaseerde inhoud van Azure Application Gateway
description: Deze pagina bevat een overzicht van de Azure Application Gateway URL-gebaseerde routering van inhoud, de UrlPathMap-configuratie en het PathBasedRouting-regel.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 1/8/2019
ms.author: victorh
ms.openlocfilehash: 1ada74f5c85ef327957ec4981e83f68bcafea858
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188756"
---
# <a name="url-path-based-routing-overview"></a>Overzicht van op URL-pad gebaseerde routering

Met op URL-pad gebaseerde routering kunt u verkeer routeren naar back-endserverpools die zijn gebaseerd op de URL-paden van de aanvraag. 

Een van de scenario's is het routeren van aanvragen voor verschillende inhoudstypen naar verschillende back-endserverpools.

In het volgende voorbeeld fungeert Application Gateway bijvoorbeeld verkeer voor contoso.com van drie back-end servergroepen: VideoServerPool, ImageServerPool en DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Aanvragen voor http://contoso.com/video/* worden doorgestuurd naar VideoServerPool, en http://contoso.com/images/* worden doorgestuurd naar ImageServerPool. Als geen van de padpatronen overeenkomen, wordt DefaultServerPool geselecteerd.

> [!IMPORTANT]
> Regels worden verwerkt in de volgorde die wordt weergegeven in de portal. Het is raadzaam om eerst listeners voor meerdere locaties te configureren voordat u een basislistener configureert.  Dit zorgt ervoor dat verkeer naar de juiste back-end wordt geleid. Als een basislistener als eerste wordt weergegeven en overeenkomt met een inkomende aanvraag, wordt deze door die listener verwerkt.

## <a name="urlpathmap-configuration-element"></a>Configuratie-element UrlPathMap

Het element UrlPathMap wordt gebruikt om padpatronen op te geven voor back-endservergroepstoewijzingen. Het volgende codevoorbeeld is het fragment van het urlPathMap-element van het sjabloonbestand.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

### <a name="pathpattern"></a>PathPattern

PathPattern is een lijst van padpatronen. Elk hiervan moet beginnen met / en de enige plaats waar een * is toegestaan, is aan het einde na een /. De tekenreeks die is ingevoerd in de matcher pad bevat geen alle tekst na de eerste? of # en deze tekens zijn niet hier toegestaan. Anders zijn alle toegestaan in een URL-tekens toegestaan in de PathPattern.

De ondersteunde patronen, is afhankelijk van of u Application Gateway v1 of v2 implementeert:

#### <a name="v1"></a>V1

Padregels zijn niet hoofdlettergevoelig.

|V1-padpatroon  |Wordt ondersteund?  |
|---------|---------|
|`/images/*`     |ja|
|`/images*`     |nee|
|`/images/*.jpg`     |nee|
|`/*.jpg`     |nee|
|`/Repos/*/Comments/*`     |nee|
|`/CurrentUser/Comments/*`     |ja|

#### <a name="v2"></a>v2

Padregels zijn hoofdlettergevoelig.

|v2-padpatroon  |Wordt ondersteund?  |
|---------|---------|
|`/images/*`     |ja|
|`/images*`     |ja|
|`/images/*.jpg`     |nee|
|`/*.jpg`     |nee|
|`/Repos/*/Comments/*`     |nee|
|`/CurrentUser/Comments/*`     |ja|

U kunt een [Resource Manager-sjabloon met op URL gebaseerde routering](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) bekijken voor meer informatie.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting-regel

RequestRoutingRule van type PathBasedRouting wordt gebruikt voor het verbinden van een listener met een urlPathMap. Alle aanvragen die zijn ontvangen voor deze listener, worden gerouteerd op basis van een beleid dat is opgegeven in urlPathMap.
Fragment van PathBasedRouting-regel:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>Volgende stappen

Nadat u meer hebt geleerd over routering van op URL gebaseerde inhoud, gaat u naar [Een toepassingsgateway maken met behulp van URL-gebaseerde routering](application-gateway-create-url-route-portal.md) om een toepassingsgateway te maken met URL-routeringsregels.

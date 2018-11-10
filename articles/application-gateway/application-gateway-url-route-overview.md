---
title: Overzicht routering van op URL gebaseerde inhoud | Microsoft Docs
description: Deze pagina biedt een overzicht van de routering van op URL gebaseerde inhoud van de toepassingsgateway, de UrlPathMap-configuratie en de PathBasedRouting-regel.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.date: 11/7/2018
ms.author: victorh
ms.openlocfilehash: bc123307a3cc3a5040e93e517c60604dc75fc7e7
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218420"
---
# <a name="url-path-based-routing-overview"></a>Overzicht van op URL-pad gebaseerde routering

Met op URL-pad gebaseerde routering kunt u verkeer routeren naar back-endserverpools die zijn gebaseerd op de URL-paden van de aanvraag. 

Een van de scenario's is het routeren van aanvragen voor verschillende inhoudstypen naar verschillende back-endserverpools.

In het volgende voorbeeld verzorgt de toepassingsgateway het verkeer voor contoso.com van drie back-endserverpools: VideoServerPool, ImageServerPool en DefaultServerPool.

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

> [!NOTE]
> PathPattern: deze instelling is een lijst van padpatronen voor aanpassing. Elk hiervan moet beginnen met / en de enige plaats waar een * is toegestaan, is aan het einde na een /. De tekenreeks die is ingevoerd in de matcher pad bevat geen alle tekst na de eerste? of # en deze tekens zijn niet hier toegestaan. Anders zijn alle toegestaan in een URL-tekens toegestaan in de PathPattern.

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

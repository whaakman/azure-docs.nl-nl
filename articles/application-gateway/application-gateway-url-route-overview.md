<properties
   pageTitle="Overzicht routering op URL gebaseerde inhoud | Microsoft Azure"
   description="Deze pagina biedt een overzicht van de routering van op URL gebaseerde inhoud van de toepassingsgateway, de UrlPathMap-configuratie en de PathBasedRouting-regel."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="gwallace"/>


# <a name="url-path-based-routing-overview"></a>Overzicht van op URL-pad gebaseerde routering

Met op URL-pad gebaseerde routering kunt u verkeer routeren naar back-endserverpools die zijn gebaseerd op de URL-paden van de aanvraag. Een van de scenario's is het routeren van aanvragen voor verschillende inhoudstypen naar verschillende back-endserverpools.
In het volgende voorbeeld verzorgt de toepassingsgateway het verkeer voor contoso.com van drie back-endserverpools: VideoServerPool, ImageServerPool en DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Aanvragen voor http://contoso.com/video* worden gerouteerd naar VideoServerPool en http://contoso.com/images* naar ImageServerPool. Als geen van de padpatronen overeenkomen, wordt DefaultServerPool geselecteerd.

## <a name="urlpathmap-configuration-element"></a>Configuratie-element UrlPathMap

UrlPathMap-element wordt gebruikt om padpatronen op te geven voor back-endservergroepstoewijzingen. Het volgende codevoorbeeld is het fragment van het urlPathMap-element van het sjabloonbestand.

    "urlPathMaps": [
    {
    "name": "<urlPathMapName>",
    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName>"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpSettingsList/<settingsName>"
        },
        "pathRules": [
            {
                "paths": [
                    <pathPattern>
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName2>"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpsettingsList/<settingsName2>"
                },

            },

        ],

    }
    }
    

>[AZURE.NOTE] PathPattern: deze instelling is een lijst van padpatronen voor aanpassing. Elk hiervan moet beginnen met / en de enige plaats waar een "*" is toegestaan, is aan het einde na een "/". De tekenreeks die is ingevoerd in de padvergelijking, bevat geen tekst na de eerste ? of # en die tekens zijn hier niet toegestaan. 

U kunt een [Resource Manager-sjabloon met op URL gebaseerde routering](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) bekijken voor meer informatie.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting-regel

RequestRoutingRule van type PathBasedRouting wordt gebruikt voor het verbinden van een listener met een urlPathMap. Alle aanvragen die zijn ontvangen voor deze listener, worden gerouteerd op basis van een beleid dat is opgegeven in urlPathMap.
Fragment van PathBasedRouting-regel:

    "requestRoutingRules": [
    {

    "name": "<ruleName>",
    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/requestRoutingRules/<ruleName>",
    "properties": {
        "ruleType": "PathBasedRouting",
        "httpListener": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/httpListeners/<listenerName>"
        },
        "urlPathMap": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>"
        },

    }
    
## <a name="next-steps"></a>Volgende stappen

Nadat u meer hebt geleerd over routering van op URL gebaseerde inhoud, gaat u naar [Een toepassingsgateway maken met behulp van URL-gebaseerde routering](application-gateway-create-url-route-portal.md) om een toepassingsgateway te maken met URL-routeringsregels.



<!--HONumber=Oct16_HO3-->



---
title: Een toepassingsgateway maken met behulp van URL routeringsregels - Azure CLI 2.0 | Microsoft Docs
description: Deze pagina vindt u instructies voor het maken en een toepassingsgateway configureren met behulp van regels voor het doorsturen van URL.
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
ms.openlocfilehash: 10d01d5d80e2d111d6b39598eed3612f80162b23
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing-with-azure-cli-20"></a>Een toepassingsgateway maken met behulp van pad gebaseerde routering met Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Met URL-pad gebaseerde routering koppelt u de routes op basis van het URL-pad van de HTTP-aanvragen. Er wordt gecontroleerd of er een route naar een back-endserverpool geconfigureerd voor de URL in de toepassingsgateway is en vervolgens het netwerkverkeer worden verzonden naar de gedefinieerde groep. Vaak gebruikt voor het URL-pad gebaseerde routering is van aanvragen verdelen over voor andere typen inhoud aan andere back-endserver groepen.

Azure Application Gateway gebruikt twee regeltypen: basic en regels voor URL-pad is gebaseerd. Het basic regeltype biedt round robin-service voor de back-end-adresgroepen. Het patroon van het pad van de aanvraag-URL pad gebaseerde regels naast round-robin distributie ook gebruiken om te kiezen, de juiste back-end-pool.

## <a name="scenario"></a>Scenario

In het volgende voorbeeld wordt een toepassingsgateway verkeer voor contoso.com verzendt met twee back-end servergroepen: een servergroep standaard en een installatiekopie-servergroep.

Aanvragen voor http://contoso.com/image * worden doorgestuurd naar de installatiekopie servergroep (**imagesBackendPool**). Als het patroon pad komt niet overeen met, de toepassingsgateway de standaardgroep server geselecteerd (**appGatewayBackendPool**).

![URL-route](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de **Microsoft Azure-opdrachtprompt** en meld u aan:

```azurecli
az login -u "username"
```

> [!NOTE]
> U kunt ook `az login` zonder dat de switch voor apparaat aanmelding waarvoor een code op aka.ms/devicelogin invoert.

Nadat u de voorgaande opdracht invoert, wordt een code. Ga naar https://aka.ms/devicelogin in een browser om door te gaan met het aanmelden.

![cmd tonen apparaat aanmelding][1]

Voer de code die u hebt ontvangen in de browser. Hiermee wordt u omgeleid naar een aanmeldingspagina.

![browser-code invoeren][2]

Voer de code aan te melden en sluit vervolgens de browser om door te gaan.

![aangemeld][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Een regel op basis van het pad toevoegen aan een bestaande application gateway

De volgende stappen laten zien hoe een regel op basis van het pad toevoegen aan een bestaande toepassingsgateway.
### <a name="create-a-new-back-end-pool"></a>Een nieuwe back-end-pool maken

Configureer de instelling voor application gateway **imagesBackendPool** voor het netwerkverkeer van taakverdeling in de groep back-end. In dit voorbeeld configureert u de instellingen van de andere back-end-adresgroep voor de nieuwe back-end-pool. Elke groep back-end kan de eigen instelling hebben. Regels op basis van het pad naar back-end HTTP-instellingen om verkeer te leiden naar de juiste back-end-groepsleden gebruiken. Hiermee bepaalt u het protocol en de poort die wordt gebruikt bij het verzenden van verkeer op de leden van de back-end-pool. De back-end-HTTP-instellingen ook bepalen op basis van het cookie-sessies.  Bij inschakeling verzendt sessie cookies gebaseerde affiniteit verkeer naar dezelfde back-end als vorige aanvragen voor elk pakket.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port-for-an-application-gateway"></a>Een nieuwe front-poort voor een toepassingsgateway maken

Het configuratieobject front-endpoort wordt gebruikt door een listener te definiëren welke poort de toepassingsgateway voor verkeer op de listener luistert.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Maak een nieuwe listener

In deze stap wordt de listener voor het openbare IP-adres en de poort voor het ontvangen van binnenkomend netwerkverkeer geconfigureerd. Het volgende voorbeeld wordt de eerder geconfigureerde front-end-IP-configuratie, front-endpoort configuratie en een protocol (http of https, die hoofdlettergevoelig zijn) en configureert de listener. In dit voorbeeld wordt de listener luistert naar HTTP-verkeer op poort 82 op het openbare IP-adres eerder in dit scenario hebt gemaakt.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Maken van de URL-pad-kaart

Deze stap configureert u het relatieve URL-pad gebruikt door de toepassingsgateway voor het definiëren van de toewijzing tussen het pad en de back-end-pool toegewezen aan de binnenkomend verkeer verwerken.

> [!IMPORTANT]
> Elk pad moet beginnen met '/', en is de enige plaats waar een sterretje is toegestaan aan het einde. Voorbeelden van geldige zijn/xyz, /xyz* of xyz / *. De tekenreeks die is ingevoerd in het pad matcher geen tekst bevatten na de eerste '? ' of '#' en deze tekens zijn niet toegestaan. 

Het volgende voorbeeld wordt een regel voor de/installatiekopieën / * pad, verkeer routering naar de back-end **imagesBackendPool**. Deze regel zorgt ervoor dat verkeer voor elke set van URL's wordt doorgestuurd naar de back-end. Bijvoorbeeld: http://adatum.com/images/figure1.jpg overschakelt naar de **imagesBackendPool**. Als het pad komt niet met de vooraf gedefinieerde padregels overeen, configureert de regel pad kaart configuratie ook een standaard back-end-adresgroep. Bijvoorbeeld: http://adatum.com/shoppingcart/test.html overschakelt naar de **pool1** omdat deze is gedefinieerd als de standaardgroep voor niet-overeenkomende verkeer.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Volgende stappen

Als u weten over Secure Sockets Layer (SSL)-offload wilt, Zie [een toepassingsgateway voor SSL-offload configureren](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png

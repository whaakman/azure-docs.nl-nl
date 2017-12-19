---
title: Een toepassingsgateway maken met behulp van regels voor het doorsturen van URL | Microsoft Docs
description: Deze pagina bevat instructies voor het maken en configureren van een Azure application gateway met behulp van regels voor het doorsturen van URL.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: f0b085ebf922cd5b14acd91bf86b9262a6921e9e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing"></a>Een toepassingsgateway maken met behulp van pad gebaseerde routering

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Pad gebaseerde routering wordt gekoppeld aan de routes op basis van het URL-pad van een HTTP-aanvraag. Er wordt gecontroleerd of er een route naar een back-end-adresgroep geconfigureerd voor de URL die zijn gepresenteerd in de toepassingsgateway is en vervolgens het netwerkverkeer worden verzonden naar de gedefinieerde back-end-pool. Vaak gebruikt voor het doorsturen van op basis van een URL is voor andere typen inhoud aan andere back-endserver groepen van aanvragen verdelen over.

Azure Application Gateway heeft twee regeltypen: basic Routering en het pad gebaseerde routering. Basic biedt round robin-service voor de back-end-adresgroepen. Het patroon van het pad van de aanvraag-URL pad gebaseerde routering, naast round-robin distributie ook worden gebruikt voor het kiezen van de back-end-pool.

## <a name="scenario"></a>Scenario

In het volgende voorbeeld Application Gateway verkeer voor contoso.com verzendt met twee back-end servergroepen: een video-servergroep en een installatiekopie-servergroep.

Aanvragen voor http://contoso.com/image * worden doorgestuurd naar de installatiekopie servergroep (**pool1**), en aanvragen voor http://contoso.com/video * worden doorgestuurd naar de video servergroep (**pool2**). Als geen van de pad-patronen overeenkomen en een server standaardgroep (**pool1**) is geselecteerd.

![URL-route](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
2. Een virtueel netwerk en subnet maken voor een toepassingsgateway. Zorg ervoor dat er geen virtuele machines en cloudimplementaties dat subnet gebruikt. De toepassingsgateway moet afzonderlijk in een subnet van een virtueel netwerk staan.
3. Zorg ervoor dat de servers zijn toegevoegd aan de groep back-end voor de toepassingsgateway bestaan, of dat ze hun eindpunten in het virtuele netwerk of met een openbare hebben IP-/ VIP toegewezen.

## <a name="requirements-to-create-an-application-gateway"></a>Vereisten voor een toepassingsgateway maken

* **Back-endserverpool**: de lijst met IP-adressen van de back-endservers. De IP-adressen die worden vermeld, moeten deel uitmaken van het subnet van het virtuele netwerk of een openbare IP-/ VIP worden.
* **Back-endserverpoolinstellingen**: zoals poort, het protocol en cookies gebaseerde affiniteit. Deze zijn gekoppeld aan een pool en toegepast op alle servers in de pool.
* **Front-endpoort**: de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener**: de listener beschikt over een front-endpoort, een protocol (Http of Https zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel**: de regel verbindt de listener met de back-endserverpool en definieert naar welke groep het verkeer moet worden omgeleid wanneer dit bij een listener aankomt.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van het klassieke implementatiemodel en Azure Resource Manager is de volgorde waarin u de toepassingsgateway en de items die moeten worden geconfigureerd.

Met Resource Manager worden alle items waaruit een toepassingsgateway bestaat, afzonderlijk geconfigureerd en vervolgens samengesteld om de toepassingsgatewayresource te maken.

Volg deze stappen voor het maken van een toepassingsgateway:

1. Maak een resourcegroep voor Resource Manager.
2. Maak een virtueel netwerk, subnet en openbaar IP-adres voor de toepassingsgateway.
3. Maak een configuratieobject voor de toepassingsgateway.
4. Maak een toepassingsgatewayresource.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u de nieuwste versie van Azure PowerShell. Zoeken naar meer informatie op [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Aanmelden bij Azure.

```powershell
Login-AzureRmAccount
```

U wordt gevraagd om te verifiëren met uw referenties.<BR>

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Stap 3

Kies welk Azure-abonnement u wilt gebruiken. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Stap 4

Maak een resourcegroep. (Sla deze stap over als u een bestaande resourcegroep gebruikt.)

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

U kunt ook de labels voor een resourcegroep voor een toepassingsgateway maken:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Azure Resource Manager vereist dat een standaardlocatie, die wordt gebruikt voor alle resources in die groep door resourcegroepen worden opgegeven. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruikt.

In het voorgaande voorbeeld we een resourcegroep genaamd 'appgw-RG' gemaakt en gebruikt de locatie "VS-West."

> [!NOTE]
> Als u configureren voor uw toepassingsgateway een aangepaste test wilt, gaat u naar [met PowerShell een toepassingsgateway maken met aangepaste tests](application-gateway-create-probe-ps.md). Zie [ Application Gateway voor health monitoring overzicht](application-gateway-probe-overview.md) voor meer informatie.
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet maken voor de toepassingsgateway

In het volgende voorbeeld ziet u hoe u een virtueel netwerk maakt met Resource Manager. In dit voorbeeld maakt een virtueel netwerk voor de toepassingsgateway. Toepassingsgateway vereist een eigen subnet. Daarom moet is het subnet dat is gemaakt voor de toepassingsgateway kleiner dan de adresruimte van het virtuele netwerk. Dit kunt andere resources, inclusief maar niet beperkt tot webservers, worden geconfigureerd in hetzelfde virtuele netwerk.

### <a name="step-1"></a>Stap 1

Wijs het adresbereik 10.0.0.0/24 toe aan de subnetvariabele die u gaat gebruiken om een virtueel netwerk te maken.  Hiermee maakt u het configuratieobject subnet voor de toepassingsgateway, die wordt gebruikt in het volgende voorbeeld.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam **appgwvnet** in de resourcegroep **appgw-rg** voor de regio VS-West met behulp van het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24. Hiermee wordt de configuratie van het virtuele netwerk met één subnet voor de toepassingsgateway zich bevinden.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Stap 3

De variabele subnet voor de volgende stappen toewijzen. Dit wordt doorgegeven aan de `New-AzureRMApplicationGateway` cmdlet in een toekomstige stap.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Maak de openbare IP-resource **publicIP01** in de resourcegroep **appgw-rg** voor de regio VS - west. Toepassingsgateway kunt een openbaar IP-adres, een interne IP-adres of beide gebruiken om aanvragen voor taakverdeling te ontvangen.  Dit voorbeeld wordt een openbaar IP-adres. Er is geen DNS-naam is geconfigureerd voor het maken van het openbare IP-adres in het volgende voorbeeld omdat Application Gateway biedt geen ondersteuning voor aangepaste DNS-namen voor openbare IP-adressen.  Als u een aangepaste naam is vereist voor het openbare eindpunt, maakt u een CNAME-record om te verwijzen naar de automatisch gegenereerde DNS-naam voor het openbare IP-adres.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-the-application-gateway-configuration"></a>De gatewayconfiguratie toepassing maken

Alle configuratie-items moeten worden ingesteld voordat u de toepassingsgateway maakt. De volgende stappen uit maken de configuratie-items die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1

Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, wordt deze een IP-adres van het geconfigureerde subnet opgehaald en routes netwerkverkeer naar de IP-adressen in de back-end-IP-adresgroep. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Stap 2

Configureer de backend-IP-adresgroep met de naam **pool1** en **pool2** met IP-adressen voor **pool1** en **pool2**. Dit zijn de IP-adressen van de resources die als host fungeren van de webtoepassing moet worden beveiligd door de toepassingsgateway. Deze back-end-pool leden worden alle gevalideerd om gezond worden door basisauthenticatie of aangepaste tests. Vervolgens wordt verkeer hiernaartoe doorgestuurd wanneer er aanvragen binnenkomen in de toepassingsgateway. Back-end-adresgroepen kunnen worden gebruikt door meerdere regels, binnen de toepassingsgateway. Dit betekent dat één back-end-adresgroep kan worden gebruikt voor meerdere webtoepassingen die zich op dezelfde host bevinden.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

In dit voorbeeld routeren twee back-end-adresgroepen netwerkverkeer op basis van het URL-pad. Een groep ontvangt verkeer van het URL-pad '/ video' en de andere toepassingen verkeer ontvangt van het pad '/ image'. Vervang de bovenstaande IP-adressen door de IP-adreseindpunten van uw eigen toepassing. 

### <a name="step-3"></a>Stap 3

Instellingen voor de toepassingsgateway configureren **poolsetting01** en **poolsetting02** voor het netwerkverkeer van taakverdeling in de groep back-end. In dit voorbeeld configureert u de instellingen van de andere back-end-adresgroep voor de back-end-adresgroepen. Elke groep back-end kan de eigen instellingen hebben.  Regels voor back-end HTTP-instellingen om verkeer te leiden naar de juiste back-end-groepsleden gebruiken. Hiermee bepaalt u het protocol en de poort die wordt gebruikt voor het verzenden van verkeer op de leden van de back-end-pool. Op basis van het cookie sessies worden ook bepaald door de back-end-HTTP-instellingen. Bij inschakeling verzendt sessie cookies gebaseerde affiniteit verkeer naar dezelfde back-end als vorige aanvragen voor elk pakket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Stap 4

Configureer het front-end-IP-adres met openbare IP-eindpunten. Een listener gebruikt de front-end-IP-configuratie-object om de buiten-IP-adres met de listener.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Stap 5

Configureer de front-endpoort voor een toepassingsgateway. De listener gebruikt het front-endpoort configuration-object om te bepalen welke poort de toepassingsgateway voor verkeer op de listener luistert.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Stap 6

Configureer de listener voor de openbare IP-adres en de poort die wordt gebruikt voor het ontvangen van binnenkomend netwerkverkeer. Het volgende voorbeeld wordt de eerder geconfigureerde front-end-IP-configuratie, front-endpoort configuratie en een protocol (Http of Https zijn hoofdlettergevoelig) en configureert u de listener. In dit voorbeeld luistert de listener luistert naar HTTP-verkeer op poort 80 voor het openbare IP-adres dat eerder is gemaakt.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Stap 7

URL-paden regel voor de back-end-adresgroepen configureren. Deze stap configureert u het relatieve pad gebruikt door Application Gateway en de toewijzing tussen het URL-pad en de back-end-pool die is toegewezen aan de verwerking van het binnenkomende verkeer definieert.

> [!IMPORTANT]
> Elk pad moet beginnen met een '/' en een sterretje is alleen toegestaan aan het einde. Voorbeelden van geldige/xyz, / XYZ zijn*, of /xyz/*. De tekenreeks die is ingevoerd in het pad matcher geen tekst bevatten na de eerste '? ' of '#' en deze tekens zijn niet toegestaan. 

Het volgende voorbeeld maakt u twee regels: één voor een '/ afbeelding /' pad routeren van verkeer naar de back-end **pool1**, en een voor een '/ video /' pad routeren van verkeer naar de back-end **pool2**. Deze regels ervoor zorgen dat verkeer voor elke set van URL's wordt doorgestuurd naar de back-end. Bijvoorbeeld: http://contoso.com/image/figure1.jpg overschakelt naar de **pool1** en http://contoso.com/video/example.mp4 overschakelt naar de **pool2**.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Als het pad komt niet met de vooraf gedefinieerde padregels overeen, configureert de regel pad kaart configuratie ook een standaard back-end-adresgroep. Bijvoorbeeld: http://contoso.com/shoppingcart/test.html overschakelt naar de **pool1** omdat deze is gedefinieerd als de standaardgroep voor niet-overeenkomende verkeer.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Stap 8

De instelling van een regel maken. Deze stap configureert u de toepassingsgateway als URL-pad gebaseerde routering wilt gebruiken. De `$urlPathMap` variabele gedefinieerd in de vorige stap nu gebruikt voor het maken van de regel op basis van het pad. In deze stap wordt de regel koppelen aan een listener en de toewijzing van de URL-pad eerder hebt gemaakt.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Stap 9

Configureer het aantal exemplaren en de grootte voor de toepassingsgateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een toepassingsgateway maken met alle configuratie-objecten uit de bovenstaande stappen.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-an-application-gateway-dns-name"></a>Ophalen van een application gateway DNS-naam

Nadat u de gateway hebt gemaakt, configureert u de front-end voor communicatie. Wanneer u een openbaar IP-adres, vereist de toepassingsgateway een dynamisch toegewezen DNS-naam, geen beschrijvende is. Klanten kunnen de toepassingsgateway bereikt, zodat kunt u een CNAME-record gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway. Zie voor meer informatie [configureren van een aangepaste domeinnaam voor een Azure cloudservice](../cloud-services/cloud-services-custom-domain-name-portal.md).

Voor het configureren van de front-end-IP-CNAME-record ophalen van gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de PublicIPAddress-element dat is gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record. Niet aanbevolen het gebruik van A-records omdat het VIP kan worden gewijzigd op opnieuw starten van de toepassingsgateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Volgende stappen

Als u weten over Secure Sockets Layer (SSL)-offload wilt, Zie [een toepassingsgateway voor SSL-offload configureren met behulp van Azure Resource Manager](application-gateway-ssl-arm.md).


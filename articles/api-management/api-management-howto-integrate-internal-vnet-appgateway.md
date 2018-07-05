---
title: Het gebruik van Azure API Management in Virtueelnetwerk met Application Gateway | Microsoft Docs
description: Meer informatie over het installeren en configureren van Azure API Management in intern Virtueelnetwerk met Application Gateway (WAF) als front
services: api-management
documentationcenter: ''
author: solankisamir
manager: kjoshi
editor: vlvinogr
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: sasolank
ms.openlocfilehash: c7d4351a9691c9787c42107306220e075f8648a0
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435120"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>API Management in een intern VNET integreren met Application Gateway

##<a name="overview"> </a> Overzicht

De API Management-service kan worden geconfigureerd in een Virtueelnetwerk in de modus voor interne waardoor alleen toegankelijk vanuit het Virtueelnetwerk. Azure Application Gateway is een PAAS-Service die voorziet in een Layer 7 load balancer. Het fungeert als een omgekeerde proxy-service en biedt onder de aanbieding van een Web Application Firewall (WAF).

API Management is ingericht in een intern VNET met de Application Gateway-frontend combineren, kunt de volgende scenario's:

* Gebruik dezelfde API Management-resource voor gebruik door consumenten interne en externe gebruikers.
* Gebruik één API Management-resource en een subset van API's die zijn gedefinieerd in API Management beschikbaar voor externe gebruikers.
* Bieden een manier directe toegang overschakelen naar API Management via het openbare Internet, in- en uitschakelen.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een APIM-instantie. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).

##<a name="scenario"> </a> Scenario
In dit artikel wordt uitgelegd hoe u gebruik van een geïntegreerde API Management-service voor interne en externe gebruikers en maken deze fungeren als een enkel frontend voor zowel on-premises en cloud-API's. U ziet ook hoe u kunt alleen een subset van uw API's (in het voorbeeld dat ze zijn gemarkeerd in het groen) beschikbaar voor extern verbruik met behulp van de PathBasedRouting-functionaliteit die beschikbaar is in Application Gateway.

Al uw API's worden in het eerste voorbeeld van de instellingen alleen van beheerd binnen uw Virtueelnetwerk. Interne consumenten (gemarkeerd in oranje) hebben toegang tot al uw interne en externe API's. Verkeer wordt nooit verzonden naar Internet wordt geleverd met een hoge prestaties via Expressroute-circuits.

![URL-route](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
2. Maak een Virtueelnetwerk en maak afzonderlijke subnetten voor API Management en Application Gateway.
3. Als u van plan bent om te maken van een aangepaste DNS-server voor het Virtueelnetwerk, doen voordat u begint met de implementatie. Controleer die het werkt door ervoor te zorgen dat een virtuele machine hebt gemaakt in een nieuw subnet in het Virtueelnetwerk omzetten en toegang tot alle Azure-service-eindpunten.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>Wat is vereist voor het maken van een integratie tussen de API Management en Application Gateway?

* **Back-endserverpool:** dit is de interne virtuele IP-adres van de API Management-service.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Te maken met het verkeer wordt omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel:** de regel wordt een listener gebonden aan een back-end-servergroep.
* **Aangepaste Statustest:** Application-Gateway gebruikt standaard IP-adres op basis van tests om te achterhalen welke servers in de BackendAddressPool actief zijn. De API Management-service alleen reageert op aanvragen waarvoor de juiste host-header, dus de tests standaard mislukken. Een aangepaste statustest moet worden gedefinieerd om te bepalen dat de service actief is en moet het doorsturen van aanvragen toepassingsgateway.
* **Aangepast domein-certificaat:** voor toegang tot API Management vanaf het internet moet u een CNAME-toewijzing van de hostnaam aan de front-end-DNS-naam van Application Gateway maken. Dit zorgt ervoor dat de koptekst van hostnaam en het certificaat verzonden naar Application Gateway die wordt doorgestuurd naar de API Management een APIM kunt herkennen als geldig is.

## <a name="overview-steps"> </a> Stappen die nodig zijn voor het integreren van API Management en Application Gateway

1. Maak een resourcegroep voor Resource Manager.
2. Maak een Virtueelnetwerk, subnet en openbaar IP-adres voor de toepassingsgateway. Maak een ander subnet voor API Management.
3. Maak een API Management-service binnen het VNET-subnet dat eerder is gemaakt en zorg ervoor dat u de interne modus.
4. De aangepaste domeinnaam in de API Management-service instellen.
5. Maak een configuratieobject voor Application Gateway.
6. Maak een Application Gateway-resource.
7. Maak een CNAME van de openbare DNS-naam van de Application Gateway naar de API Management-proxy-hostnaam.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Zie [Using Windows PowerShell with Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/powershell-azure-resource-manager) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

### <a name="step-1"></a>Stap 1

Meld u aan bij Azure.

```powershell
Connect-AzureRmAccount
```

Verifiëren met uw referenties.<BR>

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account en selecteer deze.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>Stap 3

Maak een resourcegroep (u kunt deze stap overslaan als u een bestaande resourcegroep gebruikt).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep bevinden.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een Virtueelnetwerk en een subnet voor application gateway maken

Het volgende voorbeeld ziet hoe u een Virtueelnetwerk maken met de resource manager.

### <a name="step-1"></a>Stap 1

Het netwerkadresbereik 10.0.0.0/24 toewijzen aan de subnetvariabele toe voor Application Gateway worden gebruikt tijdens het maken van een Virtueelnetwerk.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Stap 2

Het adresbereik 10.0.1.0/24 toewijzen aan de subnetvariabele toe moet worden gebruikt voor API Management tijdens het maken van een Virtueelnetwerk.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Stap 3

Maak een Virtueelnetwerk met de naam **appgwvnet** in resourcegroep **apim-appGw-RG** voor de regio VS-West, waarbij het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24 en 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Stap 4

Wijs een subnetvariabele voor de volgende stappen toe

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Een API Management-service binnen een VNET dat is geconfigureerd in de modus voor interne maken

Het volgende voorbeeld ziet hoe u een API Management-service maakt in een VNET dat is geconfigureerd voor interne toegang alleen.

### <a name="step-1"></a>Stap 1
Maak een Virtueelnetwerk van API Management-object met behulp van het subnet $apimsubnetdata hierboven hebt gemaakt.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Stap 2
Maak een API Management-service binnen het Virtueelnetwerk.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Nadat de bovenstaande opdracht is geslaagd verwijzen naar [DNS-configuratie vereist voor toegang tot interne VNET API Management-service](api-management-using-with-internal-vnet.md#apim-dns-configuration) om deze te openen.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Een aangepaste domeinnaam in API Management instellen

### <a name="step-1"></a>Stap 1
Upload het certificaat met persoonlijke sleutel voor het domein. In dit voorbeeld worden `*.contoso.net`.

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>Stap 2
Zodra het certificaat is geüpload, maakt u een configuratieobject voor de hostnaam voor de proxy met een hostnaam van `api.contoso.net`, zoals de voorbeeld-certificaat biedt-instantie voor het `*.contoso.net` domein.

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Een openbare IP-resource maakt **publicIP01** in resourcegroep **apim-appGw-RG** voor de regio VS-West.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-application-gateway-configuration"></a>Configuratie van application gateway maken

Alle configuratie-items moeten zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1

Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, wordt er een IP-adres opgehaald via het geconfigureerde subnet en wordt het netwerkverkeer omgeleid naar de IP-adressen in de back-end-IP-pool. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Stap 2

Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Dit is de poort die eindgebruikers kunnen verbinding met maken.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>Stap 3

Configureer het front-end-IP-adres met openbaar IP-eindpunt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Stap 4

Configureer het certificaat voor de toepassingsgateway, gebruikt om te ontsleutelen en opnieuw versleutelen van het verkeer te doorlopen.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Stap 5

Maken van de HTTP-listener voor de toepassingsgateway. De front-end-IP-configuratie, de poort en ssl-certificaat aan toewijzen.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Stap 6

Maak een aangepaste test met de API Management-service `ContosoApi` proxy domein eindpunt. Het pad `/status-0123456789abcdef` wordt een standaardeindpunt status die wordt gehost op alle API Management-services. Stel `api.contoso.net` als de hostnaam van een aangepaste test te beveiligen met SSL-certificaat.

> [!NOTE]
> De hostnaam `contosoapi.azure-api.net` de hostnaam proxy is standaard geconfigureerd als een service met de naam `contosoapi` wordt gemaakt in de openbare Azure.
>

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Stap 7

Upload het certificaat moet worden gebruikt voor de resources van de groep back-end SSL is ingeschakeld. Dit is hetzelfde certificaat dat u hebt opgegeven in stap 4 hierboven.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>Stap 8

Configureer de back-end-HTTP-instellingen voor de toepassingsgateway. Dit omvat het instellen van een time-outlimiet voor back-end-aanvraag, waarna ze worden geannuleerd. Deze waarde verschilt van de time-out voor de test.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Stap 9

Configureer een back-end IP-adresgroep met de naam **apimbackend** met het interne virtuele IP-adres van de API Management-service hierboven hebt gemaakt.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Stap 10

Instellingen voor een tijdelijke (niet-bestaande) back-end maken. Aanvragen voor API-paden die we niet wilt dat deze beschikbaar maken van API Management via Application Gateway wordt bereikt dit back-end en 404 retourneren.

HTTP-instellingen configureren voor de tijdelijke back-end.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Configureren van een dummy back-end **dummyBackendPool**, die verwijzen naar een FQDN-adres **dummybackend.com**. Deze FQDN-adres bestaat niet in het virtuele netwerk.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Maken van de instelling van een regel die de Application Gateway gebruikt standaard die naar de niet-bestaande back-end verwijst **dummybackend.com** in het Virtueelnetwerk.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>Stap 11

URL-paden regel voor de back-end-pools configureren. Hierdoor kunnen slechts een deel van de API's te selecteren in API Management voor worden blootgesteld aan het publiek. Bijvoorbeeld, als er `Echo API` (/ echo /), `Calculator API` (/calc/) enzovoort maken alleen `Echo API` toegankelijk is vanaf Internet).

Het volgende voorbeeld wordt een eenvoudige regel voor het "/ echo /" pad routeren van verkeer naar de back-end 'apimProxyBackendPool'.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Als het pad komt niet overeen met de padregels die we willen inschakelen van API Management, de regelconfiguratie voor de pad-kaart, configureert u een standaard back-end-adrespool met de naam **dummyBackendPool**. Bijvoorbeeld, http://api.contoso.net/calc/sum gaat naar **dummyBackendPool** als het is gedefinieerd als de standaardgroep voor niet-overeenkomende verkeer.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

De bovenstaande stap zorgt u ervoor dat alleen aanvragen voor het pad '/ echo' zijn toegestaan via de Application Gateway. Aanvragen voor andere API's geconfigureerd in API Management genereert 404-fouten van de toepassingsgateway wanneer geopend via Internet.

### <a name="step-12"></a>Stap 12

De instelling van een regel voor Application Gateway te gebruiken URL-pad gebaseerde routering maken.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>Stap 13

Het aantal exemplaren en de grootte configureren voor de toepassingsgateway. We gebruiken hier de [WAF-Voorraadeenheid](../application-gateway/application-gateway-webapplicationfirewall-overview.md) voor een betere beveiliging van de API Management-resource.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>Stap 14

Configureer WAF "Preventie" modus.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Application Gateway maken

Maak een toepassingsgateway met alle configuratieobjecten uit de bovenstaande stappen.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>CNAME de hostnaam van de API Management-proxy aan de openbare DNS-naam van de resource Application Gateway

Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbaar IP-adres, vereist de toepassingsgateway een dynamisch toegewezen DNS-naam, die mogelijk niet eenvoudig te gebruiken.

DNS-naam van de toepassingsgateway moet worden gebruikt voor het maken van een CNAME-record die de hostnaam van de APIM-proxy verwijst (bijvoorbeeld `api.contoso.net` in de bovenstaande voorbeelden) naar deze DNS-naam. Voor het configureren van de front-end-IP-CNAME-record ophalen van de gegevens van de toepassingsgateway en de bijbehorende IP-/ DNS-naam met het PublicIPAddress-element. Het gebruik van A-records wordt niet aanbevolen, omdat het VIP bij opnieuw opstarten van de gateway veranderen kan.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"> </a> Samenvatting
Azure API Management geconfigureerd in een VNET biedt een interface voor één gateway voor alle geconfigureerde API's, ongeacht of deze gehoste on-premises of in de cloud. Application Gateway integreren met API Management biedt de flexibiliteit van selectief inschakelen van bepaalde API's toegankelijk zijn via het Internet, evenals een Web Application Firewall biedt een frontend naar uw API Management-exemplaar.

##<a name="next-steps"> </a> De volgende stappen
* Meer informatie over Azure Application Gateway
  * [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)
  * [Application Gateway Web Application Firewall](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Application Gateway met pad gebaseerde routering](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Meer informatie over API Management en VNETs
  * [Met behulp van API Management is alleen beschikbaar in het VNET](api-management-using-with-internal-vnet.md)
  * [Met behulp van API Management in VNET](api-management-using-with-vnet.md)

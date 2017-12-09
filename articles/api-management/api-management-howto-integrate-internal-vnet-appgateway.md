---
title: Het gebruik van Azure API Management in virtueel netwerk met Application Gateway | Microsoft Docs
description: Meer informatie over het installeren en configureren van Azure API Management in een intern virtueel netwerk met Application Gateway (WAF) als FrontEnd
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: sasolank
ms.openlocfilehash: f9bc3ffda9f943a37fd5aadf440abf7d33a6d1de
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>API Management in een interne VNET integreren met Application Gateway 

##<a name="overview"></a> Overzicht
 
De API Management-service kan worden geconfigureerd in een virtueel netwerk in de interne modus waardoor alleen toegankelijk vanuit het virtuele netwerk. Azure Application Gateway is een PAAS-Service die voorziet in een load balancer van laag 7. Het fungeert als een reverse proxy-service en biedt tussen het aanbieden van een Web Application Firewall (WAF).

Combineren van API Management in een interne VNET met de toepassingsgateway frontend ingericht, kunt de volgende scenario's:

* Gebruik dezelfde API Management-resource voor verbruik door consumenten interne en externe gebruikers.
* Gebruik één API Management-resource en hebt u een subset van API's die zijn gedefinieerd in API Management beschikbaar voor externe gebruikers.
* Geef een directe manier toegang overschakelen naar de API Management via openbaar Internet, in- of uitschakelen. 

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een exemplaar APIM. Zie voor meer informatie [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

##<a name="scenario"></a> Scenario
In dit artikel bevat informatie over het gebruik van één API Management-service voor zowel interne als externe consumenten en maken het fungeren als een enkel frontend voor zowel on-premises en in de cloud-API's. U ziet ook hoe om alleen een subset van uw API's (in het voorbeeld dat ze zijn gemarkeerd in het groen) voor extern verbruik PathBasedRouting functionaliteit die beschikbaar is in Application Gateway met weer te geven.

Uw API's worden in het eerste voorbeeld voor setup alleen via beheerd binnen het virtuele netwerk. Interne consumenten (gemarkeerd in oranje) hebben toegang tot alle uw interne en externe API's. Verkeer wordt nooit gedeeld met Internet wordt geleverd met een hoge prestaties via Express Route-circuits.

![URL-route](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"></a> Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
2. Maak een virtueel netwerk en maak afzonderlijke subnetten voor API Management en Application Gateway. 
3. Als u van plan bent te maken van een aangepaste DNS-server voor het virtuele netwerk, doen voordat u de implementatie start. Controleer die het werkt door te zorgen voor een virtuele machine gemaakt in een nieuw subnet in het virtuele netwerk kunt omzetten en toegang tot alle Azure-service-eindpunten.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>Wat is vereist voor het maken van een integratie tussen API Management en Application Gateway?

* **Back-endserverpool:** dit is het interne virtuele IP-adres van de API Management-service.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Roept het verkeer wordt omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel:** de regel wordt een listener gebonden aan een back-end-servergroep.
* **Aangepaste Health test:** Application Gateway gebruikt standaard IP-adres op basis van tests om te achterhalen welke servers in de BackendAddressPool actief zijn. De API Management service alleen reageert op aanvragen met de juiste host-header, daarom de testpakketten standaard mislukken. Een aangepaste health test moet worden gedefinieerd om te bepalen dat de service actief is en moet deze aanvragen worden doorgestuurd toepassingsgateway.
* **Aangepaste Domeincertificaat:** voor toegang tot API Management vanaf het internet moet u een CNAME-toewijzing van de hostnaam van de toepassingsgateway front-DNS-naam te maken. Dit zorgt ervoor dat de hostnaam header en het certificaat verzonden naar Application Gateway die wordt doorgestuurd naar de API Management is een APIM als geldige kan herkennen.

## <a name="overview-steps"></a> Stappen die nodig zijn voor het integreren van API Management en Application Gateway 

1. Maak een resourcegroep voor Resource Manager.
2. Maak een virtueel netwerk, subnet en openbaar IP-adres voor de toepassingsgateway. Maak een ander subnet voor API Management.
3. Maak een API Management-service binnen het VNET subnet die eerder is gemaakt en zorg ervoor dat u de interne modus.
4. Het instellen van de aangepaste domeinnaam in de API Management-service.
5. Maak een configuratieobject voor de toepassingsgateway.
6. Een Application Gateway-resource maken.
7. Maak een CNAME van de openbare DNS-naam van de toepassingsgateway naar de hostnaam van de proxy API Management.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Zie [Using Windows PowerShell with Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/powershell-azure-resource-manager) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

### <a name="step-1"></a>Stap 1

Meld u aan bij Azure.

```powershell
Login-AzureRmAccount
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
Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruikt.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet voor de toepassingsgateway maken

Het volgende voorbeeld ziet hoe u een virtueel netwerk maken met de resource manager.

### <a name="step-1"></a>Stap 1

Het adresbereik 10.0.0.0/24 toewijzen aan de variabele subnet moet worden gebruikt voor toepassingsgateway tijdens het maken van een virtueel netwerk.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Stap 2

Het adresbereik 10.0.1.0/24 toewijzen aan de variabele subnet moet worden gebruikt voor API Management tijdens het maken van een virtueel netwerk.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Stap 3

Maak een virtueel netwerk met de naam **appgwvnet** in de resourcegroep **apim-appGw-RG** voor de regio VS-West is met het voorvoegsel 10.0.0.0/16 met subnetten 10.0.0.0/24 en 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Stap 4

Wijs een subnetvariabele toe voor de volgende stappen

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Maak een API Management-service binnen een VNET dat is geconfigureerd in de interne modus

Het volgende voorbeeld laat zien hoe een API Management-service in een VNET dat is geconfigureerd voor interne toegang alleen maken.

### <a name="step-1"></a>Stap 1
Maak een virtueel netwerk van API Management-object met behulp van het subnet $apimsubnetdata die eerder is gemaakt.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Stap 2
Maak een API Management-service in het virtuele netwerk.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Nadat de bovenstaande opdracht is geslaagd verwijzen naar [DNS-configuratie vereist voor toegang tot interne VNET API Management-service](api-management-using-with-internal-vnet.md#apim-dns-configuration) om deze te openen.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Installatie van een aangepaste domeinnaam in API Management

### <a name="step-1"></a>Stap 1
Upload het certificaat met persoonlijke sleutel voor het domein. In dit voorbeeld worden `*.contoso.net`. 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>Stap 2
Zodra het certificaat is geüpload, maakt u een hostnaam configuration-object voor de proxy gebruikt met een hostnaam van `api.contoso.net`, zoals de voorbeeld-certificaat biedt autoriteit voor het `*.contoso.net` domein. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Maak een openbare IP-resource **publicIP01** in de resourcegroep **apim-appGw-RG** voor de regio VS-West.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-application-gateway-configuration"></a>De gatewayconfiguratie toepassing maken

Alle configuratie-items moeten zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1

Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, wordt er een IP-adres opgehaald via het geconfigureerde subnet en wordt het netwerkverkeer omgeleid naar de IP-adressen in de back-end-IP-pool. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Stap 2

Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Dit is de poort waarmee gebruikers verbinding maken.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>Stap 3

Configureer het front-end-IP-adres met openbaar IP-eindpunt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Stap 4

Configureer het certificaat voor de toepassingsgateway opnieuw versleutelen van het verkeer te doorlopen en ontsleuteld.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Stap 5

De HTTP-listener voor de toepassingsgateway maken. De front-end-IP-configuratie, poort en de ssl-certificaat aan toewijzen.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Stap 6

Maken van een aangepaste test naar de service Management API `ContosoApi` proxy domein eindpunt. Het pad `/status-0123456789abcdef` is een standaardeindpunt van de status voor alle API Management-services worden gehost. Stel `api.contoso.net` als de hostnaam van een aangepaste test te beveiligen met SSL-certificaat.

> [!NOTE]
> De hostnaam van de `contosoapi.azure-api.net` de hostnaam proxy is standaard geconfigureerd als een service met de naam `contosoapi` in openbare Azure is gemaakt. 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Stap 7

Upload het certificaat moet worden gebruikt op de bronnen van de groep back-end SSL zijn ingeschakeld. Dit is hetzelfde certificaat dat u hebt opgegeven in stap 4 hierboven.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>Stap 8

HTTP-back-end-instellingen voor de toepassingsgateway configureren. Dit omvat het instellen van een time-out optreedt voor de back-end-aanvraag waarna ze worden geannuleerd. Deze waarde verschilt van de time-out voor de test.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Stap 9

Configureer een backend-IP-adresgroep met de naam **apimbackend** met het interne virtuele IP-adres van de API Management-service die eerder is gemaakt.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Stap 10

Instellingen maken voor een dummy (niet-bestaand) back-end. Aanvragen voor API-paden die we niet wilt weergeven van API Management via Application Gateway wordt bereikt deze back-end en 404 retourneren.

HTTP-instellingen configureren voor de dummy back-end.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Configureer een dummy back-end **dummyBackendPool**, die verwijzen naar een FQDN-adres **dummybackend.com**. Dit adres FQDN bestaat niet in het virtuele netwerk.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Maken van de instelling van een regel voor de toepassingsgateway gebruikt standaard die naar de niet-bestaande back-end wijst **dummybackend.com** in het virtuele netwerk.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>Stap 11

URL-paden regel voor de back-end-adresgroepen configureren. Hierdoor is slechts een deel van de API's te selecteren in API Management voor blootgesteld aan het publiek. Bijvoorbeeld, als er `Echo API` (/ echo /), `Calculator API` (/calc/) enz. Controleer alleen `Echo API` toegankelijk is vanaf Internet). 

Het volgende voorbeeld wordt een eenvoudige regel voor het '/ echo /' pad routering verkeer naar de back-end 'apimProxyBackendPool'.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Als het pad komt niet overeen met de padregels wilt inschakelen via de API Management, de regelconfiguratie pad-kaart, configureert u een standaard-back-end-adresgroep met de naam **dummyBackendPool**. Bijvoorbeeld: http://api.contoso.net/calc/ * overschakelt naar de **dummyBackendPool** zoals deze is gedefinieerd als de standaardgroep voor niet-overeenkomende verkeer.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

Deze stap zorgt ervoor dat alleen aanvragen voor het pad '/ echo' door de toepassingsgateway zijn toegestaan. Aanvragen tot andere geconfigureerd in API Management-API's genereert 404-fouten van Application Gateway wanneer deze vanuit het Internet. 

### <a name="step-12"></a>Stap 12

Maak een instelling voor de toepassingsgateway te gebruiken URL-pad gebaseerde routering.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>Stap 13

Het aantal exemplaren en de grootte voor de toepassingsgateway configureren. Hier gebruiken we de [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) voor een betere beveiliging van de API Management-resource.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>Stap 14

Configureer WAF 'Preventie'-modus.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Toepassingsgateway maken

Een toepassingsgateway maken met de configuratieobjecten uit de bovenstaande stappen.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>De hostnaam van de API Management-proxy aan de openbare DNS-naam van de resource Application Gateway CNAME

Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbaar IP-adres, vereist de toepassingsgateway een dynamisch toegewezen DNS-naam die niet eenvoudig te gebruiken. 

De toepassingsgateway DNS-naam moet worden gebruikt voor het maken van een CNAME-record die de APIM proxy-hostnaam wijst (bv. `api.contoso.net` in de bovenstaande voorbeelden) aan deze DNS-naam. Voor het configureren van de frontend-IP-CNAME-record ophalen van de gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de PublicIPAddress-element. Het gebruik van A-records wordt niet aanbevolen, omdat het VIP kan worden gewijzigd bij opnieuw opstarten van de gateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"></a> Samenvatting
Azure API Management geconfigureerd in een VNET biedt een interface één gateway voor alle geconfigureerde API's, ongeacht of deze gehoste on-premises of in de cloud. Integratie van Application Gateway met API Management biedt de flexibiliteit van selectief inschakelen van bepaalde API's voor het toegankelijk is op het Internet, evenals biedt u een Web Application Firewall als een frontend naar uw API Management-exemplaar.

##<a name="next-steps"></a> Volgende stappen
* Meer informatie over Azure Application Gateway
  * [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)
  * [Application Gateway Web Application Firewall](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Application Gateway met behulp van Routing op basis van het pad](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Meer informatie over API Management en VNETs
  * [Met behulp van API Management is alleen beschikbaar in het VNET](api-management-using-with-internal-vnet.md)
  * [Met behulp van API Management in VNET](api-management-using-with-vnet.md)

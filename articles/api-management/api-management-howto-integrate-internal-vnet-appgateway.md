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
ms.date: 06/26/2018
ms.author: sasolank
ms.openlocfilehash: 46678590366021ff0f3ddb714d439c18addc578b
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361065"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>API Management in een intern VNET integreren met Application Gateway

## <a name="overview"> </a> Overzicht

De API Management-service kan worden geconfigureerd in een Virtueelnetwerk in de interne modus, waardoor het alleen toegankelijk vanuit het Virtueelnetwerk. Azure Application Gateway is een PAAS-Service, waarmee u een Layer 7 load balancer. Het fungeert als een omgekeerde proxy-service en biedt onder de aanbieding van een Web Application Firewall (WAF).

API Management is ingericht in een intern VNET met de Application Gateway-frontend combineren, kunt de volgende scenario's:

* Gebruik dezelfde API Management-resource voor gebruik door consumenten interne en externe gebruikers.
* Gebruik één API Management-resource en een subset van API's die zijn gedefinieerd in API Management beschikbaar voor externe gebruikers.
* Bieden een manier directe toegang overschakelen naar API Management via het openbare Internet, in- en uitschakelen.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt volgen de stappen in dit artikel, moet u het volgende hebben:

* Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Certificaten - pfx en het cer voor de API-hostnaam en pfx voor de hostnaam van de developer-portal.

## <a name="scenario"> </a> Scenario

In dit artikel wordt uitgelegd hoe u gebruik van een geïntegreerde API Management-service voor interne en externe gebruikers en maken deze fungeren als een enkel frontend voor zowel on-premises en cloud-API's. U ziet ook hoe u kunt alleen een subset van uw API's (in het voorbeeld dat ze zijn gemarkeerd in het groen) beschikbaar voor extern verbruik met behulp van routering functionaliteit beschikbaar is in Application Gateway.

Al uw API's worden in het eerste voorbeeld van de instellingen alleen van beheerd binnen uw Virtueelnetwerk. Interne consumenten (gemarkeerd in oranje) hebben toegang tot al uw interne en externe API's. Verkeer wordt nooit verzonden naar Internet wordt geleverd met een hoge prestaties via Expressroute-circuits.

![URL-route](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Voordat u begint

* Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Zie de installatie-instructies op [Azure PowerShell installeren](/powershell/azure/install-az-ps). 

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>Wat is vereist voor het maken van een integratie tussen de API Management en Application Gateway?

* **Back-endserverpool:** Dit is de interne virtuele IP-adres van de API Management-service.
* **Instellingen van back-endserverpool:** Elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen worden toegepast op alle servers in de pool.
* **Front-endpoort:** Dit is de openbare poort die in de toepassingsgateway wordt geopend. Te maken met het verkeer wordt omgeleid naar een van de back-endservers.
* **Listener:** De listener beschikt over een front-endpoort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel:** De regel wordt een listener gebonden aan een back-end-servergroep.
* **Aangepaste Statustest:** Application Gateway, standaard, maakt gebruik van IP-adres op basis van tests om te achterhalen welke servers in de BackendAddressPool actief zijn. De API Management-service alleen op aanvragen met de juiste host-header reageert, dus de tests standaard mislukken. Een aangepaste statustest moet worden gedefinieerd om te bepalen dat de service actief is en moet het doorsturen van aanvragen toepassingsgateway.
* **Aangepast domeincertificaten:** Voor toegang tot API Management vanaf het internet, moet u een CNAME-toewijzing van de hostnaam aan de front-end-DNS-naam van Application Gateway maken. Dit zorgt ervoor dat de koptekst van hostnaam en het certificaat verzonden naar Application Gateway die wordt doorgestuurd naar de API Management een APIM kunt herkennen als geldig is. In dit voorbeeld gebruiken we twee certificaten - voor de back-end en de portal voor ontwikkelaars.  

## <a name="overview-steps"> </a> Stappen die nodig zijn voor het integreren van API Management en Application Gateway

1. Maak een resourcegroep voor Resource Manager.
2. Maak een Virtueelnetwerk, subnet en openbaar IP-adres voor de toepassingsgateway. Maak een ander subnet voor API Management.
3. Maak een API Management-service binnen het VNET-subnet dat eerder is gemaakt en zorg ervoor dat u de interne modus.
4. Instellen van een aangepaste domeinnaam in de API Management-service.
5. Maak een configuratieobject voor Application Gateway.
6. Maak een Application Gateway-resource.
7. Maak een CNAME van de openbare DNS-naam van de Application Gateway naar de API Management-proxy-hostnaam.

## <a name="exposing-the-developer-portal-externally-through-application-gateway"></a>Weergeven van de portal voor ontwikkelaars extern via Application Gateway

In deze handleiding we ook wordt weergegeven de **ontwikkelaarsportal** naar een externe publiek via de Application Gateway. Hiervoor is aanvullende stappen voor het maken van de ontwikkelaarsportal listener, test, instellingen en regels. Alle details vindt u in de respectieve stappen.

> [!WARNING]
> Als u Azure AD gebruiken of verificatie van een derde partij, schakel [cookies gebaseerde sessieaffiniteit](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity) functie in Application Gateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

### <a name="step-1"></a>Stap 1

Meld u aan bij Azure.

```powershell
Connect-AzAccount
```

Verifiëren met uw referenties.

### <a name="step-2"></a>Stap 2

Selecteer het gewenste abonnement.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000" # GUID of your Azure subscription
Get-AzSubscription -Subscriptionid $subscriptionId | Select-AzSubscription
```

### <a name="step-3"></a>Stap 3

Maak een resourcegroep (u kunt deze stap overslaan als u een bestaande resourcegroep gebruikt).

```powershell
$resGroupName = "apim-appGw-RG" # resource group name
$location = "West US"           # Azure region
New-AzResourceGroup -Name $resGroupName -Location $location
```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep bevinden.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een Virtueelnetwerk en een subnet voor application gateway maken

Het volgende voorbeeld ziet hoe u een Virtueelnetwerk maken met de resource manager.

### <a name="step-1"></a>Stap 1

Het netwerkadresbereik 10.0.0.0/24 toewijzen aan de subnetvariabele toe voor Application Gateway worden gebruikt tijdens het maken van een Virtueelnetwerk.

```powershell
$appgatewaysubnet = New-AzVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Stap 2

Het adresbereik 10.0.1.0/24 toewijzen aan de subnetvariabele toe moet worden gebruikt voor API Management tijdens het maken van een Virtueelnetwerk.

```powershell
$apimsubnet = New-AzVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Stap 3

Maak een Virtueelnetwerk met de naam **appgwvnet** in resourcegroep **apim-appGw-RG** voor de regio VS-West. Gebruik het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24 en 10.0.1.0/24.

```powershell
$vnet = New-AzVirtualNetwork -Name "appgwvnet" -ResourceGroupName $resGroupName -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Stap 4

Wijs een subnetvariabele voor de volgende stappen toe

```powershell
$appgatewaysubnetdata = $vnet.Subnets[0]
$apimsubnetdata = $vnet.Subnets[1]
```

## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Een API Management-service binnen een VNET dat is geconfigureerd in de modus voor interne maken

Het volgende voorbeeld ziet hoe u een API Management-service maakt in een VNET dat is geconfigureerd voor interne toegang alleen.

### <a name="step-1"></a>Stap 1

Maak een Virtueelnetwerk van API Management-object met behulp van het subnet $apimsubnetdata hierboven hebt gemaakt.

```powershell
$apimVirtualNetwork = New-AzApiManagementVirtualNetwork -SubnetResourceId $apimsubnetdata.Id
```

### <a name="step-2"></a>Stap 2

Maak een API Management-service binnen het Virtueelnetwerk.

```powershell
$apimServiceName = "ContosoApi"       # API Management service instance name
$apimOrganization = "Contoso"         # organization name
$apimAdminEmail = "admin@contoso.com" # administrator's email address
$apimService = New-AzApiManagement -ResourceGroupName $resGroupName -Location $location -Name $apimServiceName -Organization $apimOrganization -AdminEmail $apimAdminEmail -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```

Nadat de bovenstaande opdracht is geslaagd verwijzen naar [DNS-configuratie vereist voor toegang tot interne VNET API Management-service](api-management-using-with-internal-vnet.md#apim-dns-configuration) om deze te openen. Deze stap kan meer dan de helft van een uur duren.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Een aangepaste domeinnaam in API Management instellen

### <a name="step-1"></a>Stap 1

De volgende variabelen met de details van de certificaten worden geïnitialiseerd met persoonlijke sleutels voor de domeinen. In dit voorbeeld gebruiken we `api.contoso.net` en `portal.contoso.net`.  

```powershell
$gatewayHostname = "api.contoso.net"                 # API gateway host
$portalHostname = "portal.contoso.net"               # API developer portal host
$gatewayCertCerPath = "C:\Users\Contoso\gateway.cer" # full path to api.contoso.net .cer file
$gatewayCertPfxPath = "C:\Users\Contoso\gateway.pfx" # full path to api.contoso.net .pfx file
$portalCertPfxPath = "C:\Users\Contoso\portal.pfx"   # full path to portal.contoso.net .pfx file
$gatewayCertPfxPassword = "certificatePassword123"   # password for api.contoso.net pfx certificate
$portalCertPfxPassword = "certificatePassword123"    # password for portal.contoso.net pfx certificate

$certPwd = ConvertTo-SecureString -String $gatewayCertPfxPassword -AsPlainText -Force
$certPortalPwd = ConvertTo-SecureString -String $portalCertPfxPassword -AsPlainText -Force
```

### <a name="step-2"></a>Stap 2

Maak en stel de hostnaam voor de proxy en de portal configuratieobjecten.  

```powershell
$proxyHostnameConfig = New-AzApiManagementCustomHostnameConfiguration -Hostname $gatewayHostname -HostnameType Proxy -PfxPath $gatewayCertPfxPath -PfxPassword $certPwd
$portalHostnameConfig = New-AzApiManagementCustomHostnameConfiguration -Hostname $portalHostname -HostnameType Portal -PfxPath $portalCertPfxPath -PfxPassword $certPortalPwd

$apimService.ProxyCustomHostnameConfiguration = $proxyHostnameConfig
$apimService.PortalCustomHostnameConfiguration = $portalHostnameConfig
Set-AzApiManagement -InputObject $apimService
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Een openbare IP-resource maakt **publicIP01** in de resourcegroep.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $resGroupName -name "publicIP01" -location $location -AllocationMethod Dynamic
```

Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-application-gateway-configuration"></a>Configuratie van application gateway maken

Alle configuratie-items moeten zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1

Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, wordt er een IP-adres opgehaald via het geconfigureerde subnet en wordt het netwerkverkeer omgeleid naar de IP-adressen in de back-end-IP-pool. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Stap 2

Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Dit is de poort die eindgebruikers kunnen verbinding met maken.

```powershell
$fp01 = New-AzApplicationGatewayFrontendPort -Name "port01"  -Port 443
```

### <a name="step-3"></a>Stap 3

Configureer het front-end-IP-adres met openbaar IP-eindpunt.

```powershell
$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Stap 4

Certificaten configureren voor de toepassingsgateway, dat wordt gebruikt om te ontsleutelen en opnieuw versleutelen van het verkeer te doorlopen.

```powershell
$cert = New-AzApplicationGatewaySslCertificate -Name "cert01" -CertificateFile $gatewayCertPfxPath -Password $certPwd
$certPortal = New-AzApplicationGatewaySslCertificate -Name "cert02" -CertificateFile $portalCertPfxPath -Password $certPortalPwd
```

### <a name="step-5"></a>Stap 5

De HTTP-listeners voor Application Gateway maken. De front-end-IP-configuratie, de poort en ssl-certificaten aan hen toewijzen.

```powershell
$listener = New-AzApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert -HostName $gatewayHostname -RequireServerNameIndication true
$portalListener = New-AzApplicationGatewayHttpListener -Name "listener02" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $certPortal -HostName $portalHostname -RequireServerNameIndication true
```

### <a name="step-6"></a>Stap 6

Maken van aangepaste tests uitvoeren voor de API Management-service `ContosoApi` proxy domein eindpunt. Het pad `/status-0123456789abcdef` wordt een standaardeindpunt status die wordt gehost op alle API Management-services. Stel `api.contoso.net` als de hostnaam van een aangepaste test te beveiligen met SSL-certificaat.

> [!NOTE]
> De hostnaam `contosoapi.azure-api.net` de hostnaam proxy is standaard geconfigureerd als een service met de naam `contosoapi` wordt gemaakt in de openbare Azure.
>

```powershell
$apimprobe = New-AzApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName $gatewayHostname -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
$apimPortalProbe = New-AzApplicationGatewayProbeConfig -Name "apimportalprobe" -Protocol "Https" -HostName $portalHostname -Path "/signin" -Interval 60 -Timeout 300 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Stap 7

Upload het certificaat moet worden gebruikt voor de resources van de groep back-end SSL is ingeschakeld. Dit is hetzelfde certificaat dat u hebt opgegeven in stap 4 hierboven.

```powershell
$authcert = New-AzApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile $gatewayCertCerPath
```

### <a name="step-8"></a>Stap 8

Configureer de back-end-HTTP-instellingen voor de toepassingsgateway. Dit omvat het instellen van een time-outlimiet voor back-end-aanvraag, waarna ze worden geannuleerd. Deze waarde verschilt van de time-out voor de test.

```powershell
$apimPoolSetting = New-AzApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
$apimPoolPortalSetting = New-AzApplicationGatewayBackendHttpSettings -Name "apimPoolPortalSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimPortalProbe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Stap 9

Configureer een back-end IP-adresgroep met de naam **apimbackend** met het interne virtuele IP-adres van de API Management-service hierboven hebt gemaakt.

```powershell
$apimProxyBackendPool = New-AzApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.PrivateIPAddresses[0]
```

### <a name="step-10"></a>Stap 10

Regels voor de toepassingsgateway gebruik van eenvoudige routering maken.

```powershell
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType Basic -HttpListener $listener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
$rule02 = New-AzApplicationGatewayRequestRoutingRule -Name "rule2" -RuleType Basic -HttpListener $portalListener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolPortalSetting
```

> [!TIP]
> Wijzig de RuleType - en routering, om toegang te beperken tot bepaalde pagina's van de portal voor ontwikkelaars.

### <a name="step-11"></a>Stap 11

Het aantal exemplaren en de grootte configureren voor de toepassingsgateway. In dit voorbeeld gebruiken we de [WAF-Voorraadeenheid](../application-gateway/application-gateway-webapplicationfirewall-overview.md) voor een betere beveiliging van de API Management-resource.

```powershell
$sku = New-AzApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-12"></a>Stap 12

Configureer WAF "Preventie" modus.

```powershell
$config = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Application Gateway maken

Maak een toepassingsgateway met alle configuratieobjecten uit de bovenstaande stappen.

```powershell
$appgwName = "apim-app-gw"
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $resGroupName -Location $location -BackendAddressPools $apimProxyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $apimPoolPortalSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener, $portalListener -RequestRoutingRules $rule01, $rule02 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert, $certPortal -AuthenticationCertificates $authcert -Probes $apimprobe, $apimPortalProbe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>CNAME de hostnaam van de API Management-proxy aan de openbare DNS-naam van de resource Application Gateway

Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbaar IP-adres, vereist de toepassingsgateway een dynamisch toegewezen DNS-naam, die mogelijk niet eenvoudig te gebruiken.

DNS-naam van de toepassingsgateway moet worden gebruikt voor het maken van een CNAME-record die de hostnaam van de APIM-proxy verwijst (bijvoorbeeld `api.contoso.net` in de bovenstaande voorbeelden) naar deze DNS-naam. Voor het configureren van de front-end-IP-CNAME-record ophalen van de gegevens van de toepassingsgateway en de bijbehorende IP-/ DNS-naam met het PublicIPAddress-element. Het gebruik van A-records wordt niet aanbevolen, omdat het VIP bij opnieuw opstarten van de gateway veranderen kan.

```powershell
Get-AzPublicIpAddress -ResourceGroupName $resGroupName -Name "publicIP01"
```

## <a name="summary"> </a> Samenvatting
Azure API Management geconfigureerd in een VNET biedt een interface voor één gateway voor alle geconfigureerde API's, of ze on-premises of in de cloud worden gehost. Application Gateway integreren met API Management biedt de flexibiliteit van selectief inschakelen van bepaalde API's toegankelijk zijn via het Internet, evenals een Web Application Firewall biedt een frontend naar uw API Management-exemplaar.

## <a name="next-steps"> </a> De volgende stappen
* Meer informatie over Azure Application Gateway
  * [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)
  * [Application Gateway Web Application Firewall](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Application Gateway met pad gebaseerde routering](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Meer informatie over API Management en VNETs
  * [Met behulp van API Management is alleen beschikbaar in het VNET](api-management-using-with-internal-vnet.md)
  * [Met behulp van API Management in VNET](api-management-using-with-vnet.md)

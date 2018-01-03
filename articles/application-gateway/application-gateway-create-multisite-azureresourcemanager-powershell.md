---
title: Een toepassingsgateway voor het hosten van meerdere sites maken | Microsoft Docs
description: Deze pagina vindt u instructies voor het maken, configureren van een toepassingsgateway voor het hosten van meerdere webtoepassingen op dezelfde gateway.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Een toepassingsgateway voor het hosten van meerdere webtoepassingen maken

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Meerdere hosting-site, kunt u meer dan een webtoepassing op dezelfde toepassingsgateway implementeren. Is afhankelijk van de aanwezigheid van de host-header in de binnenkomende HTTP-aanvraag om te bepalen welke listener verkeer ontvangt. De listener stuurt vervolgens verkeer naar de juiste back-endpool zoals geconfigureerd in de definitie van de gateway. In webtoepassingen SSL is ingeschakeld, is de toepassingsgateway afhankelijk van de Server de naam van vermelding (SNI)-extensie voor kiest u de juiste listener voor internetverkeer. Vaak gebruikt voor het hosten van meerdere site is van aanvragen verdelen over voor verschillende webdomeinen op verschillende back-endserver opslaggroepen. Op dezelfde manier kunnen meerdere subdomeinen van het hoofddomein dezelfde ook worden gehost op dezelfde toepassingsgateway.

## <a name="scenario"></a>Scenario

In het volgende voorbeeld toepassingsgateway verkeer voor contoso.com en fabrikam.com bedient met twee back-endserver van toepassingen: contoso-servergroep en fabrikam-servergroep. Vergelijkbare setup kan worden gebruikt voor de host subdomeinen zoals app.contoso.com en blog.contoso.com.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
2. De servers toegevoegd aan de groep back-end gebruiken van de toepassingsgateway moeten bestaan of hebben hun eindpunten gemaakt in het virtuele netwerk in een apart subnet of een openbaar IP-/ VIP-adres toegewezen.

## <a name="requirements"></a>Vereisten

* **Back-endserverpool:** de lijst met IP-adressen van de back-endservers. De IP-adressen moeten ofwel deel uitmaken van het subnet van het virtuele netwerk, ofwel openbare IP-/VIP-adressen zijn. FQDN-naam kan ook worden gebruikt.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert). Voor meerdere locaties ingeschakeld Toepassingsgateways, hostnaam en SNI indicatoren ook worden toegevoegd.
* **Regel:** de regel verbindt de listener met de back-end-servergroep, en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt. Regels worden verwerkt in de volgorde die ze worden weergegeven en verkeer wordt doorgestuurd via de eerste regel die overeenkomt met ongeacht specifieke karakter. Bijvoorbeeld, als u een regel met behulp van een basic-listener en een regel met meerdere sites listener beide op dezelfde poort hebt, kan de regel met de listener voor meerdere locaties moet worden vermeld voordat u de regel met de basic-listener in volgorde voor de regel voor meerdere locaties werken zoals verwacht.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Hier volgen de stappen die nodig zijn om een toepassingsgateway te maken:

1. Maak een resourcegroep voor Resource Manager.
2. Maak een virtueel netwerk, subnetten en openbare IP-adres voor de toepassingsgateway.
3. Maak een configuratieobject voor de toepassingsgateway.
4. Maak een toepassingsgatewayresource.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Meer informatie vindt u op [Windows PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Meld u aan bij Azure.

```powershell
Login-AzureRmAccount
```
U wordt gevraagd om u te verifiëren met uw referenties.

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Stap 3

Kies welk Azure-abonnement u wilt gebruiken.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Stap 4

Maak een resourcegroep (u kunt deze stap overslaan als u een bestaande resourcegroep gebruikt).

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

U kunt ook ook labels voor een resourcegroep voor de toepassingsgateway maken:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruikt.

In het bovenstaande voorbeeld is er een resourcegroep aangeroepen hebt gemaakt **appgw-RG** met een locatie van **VS-West**.

> [!NOTE]
> Als u voor uw toepassingsgateway een aangepaste test moet configureren, raadpleegt u [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Met PowerShell een toepassingsgateway maken met aangepaste tests). Ga naar [aangepaste tests en statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

## <a name="create-a-virtual-network-and-subnets"></a>Een virtueel netwerk en subnetten maken

In het volgende voorbeeld ziet u hoe u een virtueel netwerk maakt met Resource Manager. Twee subnetten worden gemaakt in deze stap. Het eerste subnet is voor de toepassingsgateway zelf. Toepassingsgateway vereist een eigen subnet voor het opslaan van de exemplaren. Alleen andere toepassingsgateways kunnen worden geïmplementeerd in dat subnet. Het tweede subnet wordt gebruikt om de toepassing back-endservers.

### <a name="step-1"></a>Stap 1

Het adresbereik 10.0.0.0/24 toewijzen aan de variabele subnet moet worden gebruikt voor het opslaan van de toepassingsgateway.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>Stap 2

Het adresbereik 10.0.1.0/24 toewijzen aan de variabele subnet2 moet worden gebruikt voor de back-end-adresgroepen.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Stap 3

Maak een virtueel netwerk met de naam **appgwvnet** in de resourcegroep **appgw-rg** voor de regio VS-West is met het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24 en 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>Stap 4

Wijs een subnetvariabele toe voor de volgende stappen, die een application gateway maakt.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Maak de openbare IP-resource **publicIP01** in de resourcegroep **appgw-rg** voor de regio VS - west.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-application-gateway-configuration"></a>De gatewayconfiguratie toepassing maken

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1

Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, neemt over een IP-adres van het geconfigureerde subnet en netwerkverkeer omgeleid naar de IP-adressen in de back-end-IP-adresgroep. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>Stap 2

Configureer de backend-IP-adresgroep met de naam **pool01** en **pool2** met IP-adressen **134.170.185.46**, **134.170.188.221**, **134.170.185.50** voor **pool1** en **134.170.186.46**, **134.170.189.221**, **134.170.186.50** voor **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

In dit voorbeeld zijn er twee groepen van de back-end voor het doorsturen van netwerkverkeer op basis van de gevraagde website. Één groep verkeer ontvangt van de site 'contoso.com' en andere toepassingen verkeer van site 'fabrikam.com' ontvangt. U moet de voorgaande IP-adressen voor het toevoegen van uw eigen toepassing IP-adreseindpunten vervangen. In plaats van interne IP-adressen kan u openbare IP-adressen, FQDN-naam of NIC van een virtuele machine ook gebruiken voor back-end-exemplaren. FQDN-namen in plaats van IP-adressen opgeven in het gebruik van PowerShell '-BackendFQDNs ' parameter.

### <a name="step-3"></a>Stap 3

Configureren van de instelling voor application gateway **poolsetting01** en **poolsetting02** voor het netwerkverkeer van taakverdeling in de groep back-end. In dit voorbeeld configureert u de instellingen van de andere back-end-adresgroep voor de back-end-adresgroepen. Elke back-endpool kan een eigen instelling van de back-endpool hebben.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Stap 4

Configureer het front-end-IP-adres met openbaar IP-eindpunt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Stap 5

Configureer de front-endpoort voor een toepassingsgateway.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>Stap 6

Configureer twee SSL-certificaten voor de twee websites gaan we in dit voorbeeld wilt ondersteunen. Er is één certificaat voor contoso.com verkeer en de andere is voor fabrikam.com verkeer. Deze certificaten moeten een certificeringsinstantie uitgegeven certificaten voor uw websites. Zelfondertekende certificaten worden ondersteund, maar niet aanbevolen voor productie-verkeer.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>Stap 7

Configureer twee listeners voor de twee websites in dit voorbeeld. Deze stap configureert u de listeners voor openbare IP-adres, poort en host die wordt gebruikt voor het ontvangen van binnenkomend verkeer. HostName-parameter is vereist voor ondersteuning voor meerdere site en moet worden ingesteld op de juiste website waarvan het verkeer wordt ontvangen. RequireServerNameIndication-parameter moet worden ingesteld op true voor websites die ondersteuning voor SSL in een scenario voor meerdere host nodig. Als SSL-ondersteuning vereist is, moet u ook het SSL-certificaat dat wordt gebruikt voor de beveiliging van verkeer voor deze webtoepassing opgeven. De combinatie van FrontendIPConfiguration, FrontendPort en host-naam moet uniek zijn voor een listener. Elke listener kan één certificaat ondersteunen.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>Stap 8

Maak twee instelling van regel voor de twee webtoepassingen in dit voorbeeld. Een regel verbindt samen listeners, back-endpools en HTTP-instellingen. Deze stap configureert u de toepassingsgateway voor het gebruik van Basic routeringsregel, één voor elke website. Verkeer naar elke website wordt ontvangen door de geconfigureerde listener en wordt vervolgens omgeleid naar de geconfigureerde back-endpool met behulp van de eigenschappen die zijn opgegeven in de BackendHttpSettings.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>Stap 9

Configureer het aantal exemplaren en de grootte voor de toepassingsgateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Toepassingsgateway maken

Een toepassingsgateway maken met alle configuratie-objecten uit de bovenstaande stappen.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Inrichting van Application Gateway is een langdurige bewerking en kan even duren om te voltooien.
> 
> 

## <a name="get-application-gateway-dns-name"></a>DNS-naam van toepassingsgateway verkrijgen

Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbare IP gebruikt, heeft de toepassingsgateway een dynamisch toegewezen DNS-naam nodig. Dit is niet gebruiksvriendelijk. Om ervoor te zorgen dat eindgebruikers de toepassingsgateway kunnen bereiken, kan een CNAME-record worden gebruikt die verwijst naar het openbare eindpunt van de toepassingsgateway. [Een aangepaste domeinnaam configureren voor in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Daartoe haalt u details van de toepassingsgateway en de bijbehorende IP-/ DNS-naam op met het PublicIPAddress-element gekoppeld aan de toepassingsgateway. De DNS-naam van de toepassingsgateway moet worden gebruikt om een CNAME-record te maken die de twee webtoepassingen naar deze DNS-naam wijst. Het gebruik van A-records wordt niet aanbevolen, omdat het VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

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

Informatie over het beveiligen van uw websites met [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)


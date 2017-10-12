---
title: Een omleiding voor Azure Application Gateway configureren - PowerShell | Microsoft Docs
description: Op deze pagina vindt u scenario's om een omleiding te configureren voor Application Gateway met behulp van PowerShell
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: davidmu
ms.openlocfilehash: cbee070c6bfe17135f37a82c545bf8ee7db2db43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-redirection-on-application-gateway-with-powershell"></a>Omleiding configureren in Application Gateway met PowerShell

Application Gateway ondersteunt het omleiden van verkeer op basis van een gedefinieerde configuratie. Voor meer informatie over omleidingen in het algemeen gaat u naar [Application Gateway redirect overview](application-gateway-redirect-overview.md) (Overzicht van omleiden met Application Gateway). Dit artikel biedt voorbeelden van HTTP-naar-HTTPS-omleidingen, padgebaseerde omleidingen, multi-site-omleidingen en omleidingen naar externe sites.

## <a name="http-to-https-redirect-on-an-existing-application-gateway"></a>HTTP-naar-HTTPS-omleiding in een bestaande toepassingsgateway

In het volgende voorbeeld wordt er een nieuwe HTTP-listener gemaakt op poort 80 om aanvragen om te leiden naar de bestaande listener op poort 443.

```powershell
# Get the application gateway
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

# Get the existing HTTPS listener
$httpslistener = Get-AzureRmApplicationGatewayHttpListener -Name appgatewayhttplistener -ApplicationGateway $gw

# Get the existing front end IP configuration
$fipconfig = Get-AzureRmApplicationGatewayFrontendIPConfig -Name appgatewayfrontendip -ApplicationGateway $gw

# Add a new front end port to support HTTP traffic
Add-AzureRmApplicationGatewayFrontendPort -Name appGatewayFrontendPort2  -Port 80 -ApplicationGateway $gw

# Get the recently created port
$fp = Get-AzureRmApplicationGatewayFrontendPort -Name appGatewayFrontendPort2 -ApplicationGateway $gw

# Create a new HTTP listener using the port created earlier
Add-AzureRmApplicationGatewayHttpListener -Name appgatewayhttplistener2  -Protocol Http -FrontendPort $fp -FrontendIPConfiguration $fipconfig -ApplicationGateway $gw 

# Get the new listener
$listener = Get-AzureRmApplicationGatewayHttpListener -Name appgatewayhttplistener2 -ApplicationGateway $gw

# Add a redirection configuration using a permanent redirect and targeting the existing listener
Add-AzureRmApplicationGatewayRedirectConfiguration -Name redirectHttptoHttps -RedirectType Permanent -TargetListener $httpslistener -IncludePath $true -IncludeQueryString $true -ApplicationGateway $gw

# Get the redirect configuration
$redirectconfig = Get-AzureRmApplicationGatewayRedirectConfiguration -Name redirectHttptoHttps -ApplicationGateway $gw


# Add a new rule to handle the redirect and use the new listener
Add-AzureRmApplicationGatewayRequestRoutingRule -Name rule02 -RuleType Basic -HttpListener $listener -RedirectConfiguration $redirectconfig -ApplicationGateway $gw

# Update the application gateway
Set-AzureRmApplicationGateway -ApplicationGateway $gw 
```

Nadat de toepassingsgateway is bijgewerkt, test u de nieuwe listener op poort 80.  In het volgende voorbeeld ziet u hoe u met behulp van `curl` de omleiding kunt testen.

```
curl http://40.69.161.221 -i
HTTP/1.1 301 Moved Permanently
Content-Type: text/html; charset=UTF-8
Location: https://40.69.161.221/
Server: Microsoft-IIS/10.0
Date: Tue, 11 Jul 2017 20:54:00 GMT
Content-Length: 145

<head><title>Document Moved</title></head>
<body><h1>Object Moved</h1>This document may be found <a HREF="https://40.69.161.221/">here</a></body>
```

## <a name="path-based-redirect"></a>Padgebaseerde omleiding

In het volgende voorbeeld worden een nieuwe listener en een padregel gemaakt in een bestaande toepassingsgateway die alleen het verkeer omleidt dat voldoet aan het specifieke URL-pad.

```powershell
# Get the application gateway
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

# Get the existing HTTPS listener
$httpslistener = Get-AzureRmApplicationGatewayHttpListener -Name appgatewayhttplistener -ApplicationGateway $gw

# Get the existing front end IP configuration
$fipconfig = Get-AzureRmApplicationGatewayFrontendIPConfig -Name appgatewayfrontendip -ApplicationGateway $gw

# Add a new front end port to support HTTP traffic
Add-AzureRmApplicationGatewayFrontendPort -Name appGatewayFrontendPort2  -Port 80 -ApplicationGateway $gw

# Get the recently created port
$fp = Get-AzureRmApplicationGatewayFrontendPort -Name appGatewayFrontendPort2 -ApplicationGateway $gw

# Create a new HTTP listener using the port created earlier
Add-AzureRmApplicationGatewayHttpListener -Name appgatewayhttplistener2  -Protocol Http -FrontendPort $fp -FrontendIPConfiguration $fipconfig -ApplicationGateway $gw 

# Get the new listener
$listener = Get-AzureRmApplicationGatewayHttpListener -Name appgatewayhttplistener2 -ApplicationGateway $gw

# Add a redirection configuration using a permanent redirect and targeting the existing listener
$redirectconfig = Get-AzureRmApplicationGatewayRedirectConfiguration -Name redirectpath6 -ApplicationGateway $gw

# Retrieve the existing backend http settings to be used
$poolSetting = Get-AzureRmApplicationGatewayBackendHttpSettings -Name "appGatewayBackendHttpSettings" -ApplicationGateway $gw

# Retrieve an existing backend pool
$pool = Get-AzureRmApplicationGatewayBackendAddressPool -Name appGatewayBackendPool -ApplicationGateway $gw

# Create a new path based rule
$pathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule6" -Paths "/image/*" -BackendAddressPool $pool -BackendHttpSettings $poolSetting

# Create a path map to add to the rule
Add-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $pathRule -DefaultBackendAddressPool $pool -DefaultBackendHttpSettings $poolSetting -ApplicationGateway $gw

# Retrieve the url path map created
$urlPathMap = Get-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -ApplicationGateway $gw

# Add a new rule to handle the redirect and use the new listener
Add-AzureRmApplicationGatewayRequestRoutingRule -Name "rule6" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap -RedirectConfiguration $redirectconfig -ApplicationGateway $gw

# Update the application gateway
Set-AzureRmApplicationGateway -ApplicationGateway $gw 
```

## <a name="multi-site-redirect"></a>Multi-site-omleiding

In het volgende voorbeeld wordt er een nieuwe toepassingsgateway gemaakt met twee multi-site-listeners op poort 80. De listeners zijn voor adatum.com en adatum.org. Er wordt een omleidingsregel gemaakt om verkeer van adatum.org naar adatum.com om te leiden. Aanvullende configuratie is vereist voor het configureren van CNAME-aliassen met het openbare IP-adres van de toepassingsgateway. Raadpleeg [Een domein delegeren naar Azure DNS](../dns/dns-delegate-domain-azure-dns.md) voor meer informatie over het delegeren van uw domein naar Azure DNS en het maken van CNAME-records voor uw domeinbezoek.

```powershell
# Create a new resource group for the application gateway
New-AzureRmResourceGroup -Name appgw-rg -Location "East US"

# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for Application Gateway instances. With a smaller subnet, you may not be able to add more instance of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

# Create a public IP address for use with the application gateway. Defining the domainnamelabel during creation is not supported for use with application gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "East US" -AllocationMethod Dynamic

# Create a IP configuration. This configures what subnet the Application Gateway uses. When Application Gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $vnet.Subnets[0]

# Create a backend pool to hold the addresses or NICs for the application that application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Conifugre the backend HTTP settings to be used to define how traffic is routed to the backend pool. The authenication certificate used in the previous step is added to the backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled

# Create a frontend port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 80

# Create a frontend IP configuration to associate the public IP address with the application gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Create the HTTP listener for the application gateway for adatum.com. Assign the front-end ip configuration, and port.
$listener1 = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp -HostName adatum.com

# Create the HTTP listener for the application gateway for adatum.org this listener will redirect to the abc.com listener. Assign the front-end ip configuration, and port.
$listener2 = New-AzureRmApplicationGatewayHttpListener -Name listener02 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp -HostName adatum.org

# Create the redirect configuration that will point traffic to the 
$redirectconfig = New-AzureRmApplicationGatewayRedirectConfiguration -Name redirectOrgtoCom -RedirectType Found -TargetListener $listener1 -IncludePath $true -IncludeQueryString $true

#Create a load balancer routing rule that configures the load balancer behavior. In this example, a basic round robin rule is created.
$rule1 = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -HttpListener $listener1 -BackendHttpSettings $poolSetting -BackendAddressPool $pool

#Create a load balancer routing rule that redirects traffic to the adatum.com listener
$rule2 = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule02 -RuleType Basic -HttpListener $listener2 -RedirectConfiguration $redirectconfig

# Configure the SKU of the application gateway
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Medium -Tier Standard -Capacity 2

# Create the application gateway utilizing all the previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener,$listener2 -RequestRoutingRules $rule1,$rule2 -RedirectConfigurations $redirectconfig -Sku $sku
```

## <a name="redirect-to-an-external-site"></a>Omleiden naar een externe site

In het volgende voorbeeld wordt verkeer dat de toepassingsgateway binnenkomt, omgeleid naar een externe website (bing.com). Wanneer u de switch `TargetUrl` gebruikt, zijn de switches `IncludePath` en `IncludeQuery` niet toegestaan.

```powershell
# Create a new resource group for the application gateway
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for Application Gateway instances. With a smaller subnet, you may not be able to add more instance of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

# Create a public IP address for use with the application gateway. Defining the domainnamelabel during creation is not supported for use with application gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

# Create a IP configuration. This configures what subnet the Application Gateway uses. When Application Gateway starts, it picks up an IP address from the subnet configured and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $vnet.Subnets[0]

# Create a backend pool to hold the addresses or NICs for the application that application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Conifugre the backend HTTP settings to be used to define how traffic is routed to the backend pool. The authenication certificate used in the previous step is added to the backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled

# Create a frontend port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 80

# Create a frontend IP configuration to associate the public IP address with the application gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Create the HTTP listener for the application gateway. Assign the front-end ip configuration, and port.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp 

# Create the redirect configuration that will point traffic to the 
$redirectconfig = New-AzureRmApplicationGatewayRedirectConfiguration -Name myredirect -RedirectType Temporary -TargetUrl "http://bing.com" -IncludePath $true -IncludeQueryString $true

#Create a load balancer routing rule that configures the load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -HttpListener $listener -RedirectConfiguration $redirectconfig 

# Configure the SKU of the application gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Create the application gateway utilizing all the previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -RedirectConfigurations $redirectconfig -Sku $sku
```

## <a name="next-steps"></a>Volgende stappen

[Configure end to end SSL with application gateway using PowerShell](application-gateway-end-to-end-ssl-powershell.md) (End-to-end-SSL configureren met toepassingsgateway met behulp van PowerShell) bekijken
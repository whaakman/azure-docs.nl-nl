---
title: 'Web application firewall configureren: Azure Application Gateway | Microsoft Docs'
description: In dit artikel biedt richtlijnen voor het gebruik van een web application firewall op een bestaande of nieuwe application gateway.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: e8106805d21b325e33fb3ab376db75cd783b9042
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Web application firewall op een nieuwe of bestaande toepassingsgateway configureren

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

Informatie over het maken van een web application firewall (WAF)-toepassingsgateway ingeschakeld. Leert ook hoe een WAF toevoegen aan een bestaande toepassingsgateway.

De WAF in Azure Application Gateway beveiligt webtoepassingen van algemene web gebaseerde aanvallen, zoals SQL-injectie, cross-site scripting aanvallen en sessie hijacks.

 Application Gateway is een load balancer laag 7. Het biedt failover, HTTP-aanvragen routeren tussen verschillende servers, of ze zich op de cloud of on-premises. Toepassingsgateway biedt veel functies van de toepassing levering netwerkcontroller (ADC):

 * HTTP-taakverdeling
 * Sessie cookies gebaseerde affiniteit
 * Secure Sockets Layer (SSL)-offload
 * Aangepaste statuscontroles
 * Ondersteuning voor functionaliteit voor meerdere locaties
 
 Een volledige lijst met ondersteunde functies Zie [overzicht van toepassingsgateway](application-gateway-introduction.md).

Dit artikel laat zien hoe u [een WAF toevoegen aan een bestaande toepassingsgateway](#add-web-application-firewall-to-an-existing-application-gateway). U ziet ook hoe [maken van een toepassingsgateway die gebruikmaakt van een WAF](#create-an-application-gateway-with-web-application-firewall).

![afbeelding van scenario][scenario]

## <a name="waf-configuration-differences"></a>WAF configuratie verschillen

Als u hebt gelezen [een toepassingsgateway maken met PowerShell](application-gateway-create-gateway-arm.md), u inzicht in de SKU-instellingen configureren wanneer u een toepassingsgateway maken. De WAF biedt aanvullende instellingen op te geven wanneer u een SKU op een application gateway configureert. Er zijn geen aanvullende wijzigingen die u in de toepassingsgateway zelf aanbrengt.

| **Instelling** | **Details**
|---|---|
|**SKU** |Biedt ondersteuning voor een normale toepassingsgateway zonder een WAF **standaard\_kleine**, **standaard\_gemiddeld**, en **standaard\_grote**grootten. Dankzij de introductie van een WAF, er zijn twee extra SKU's **WAF\_gemiddeld** en **WAF\_grote**. Een WAF wordt niet ondersteund op kleine Toepassingsgateways.|
|**Laag** | De beschikbare waarden zijn **standaard** of **WAF**. Wanneer u een WAF gebruikt, moet u kiezen **WAF**.|
|**Modus** | Deze instelling wordt de modus van de WAF. toegestane waarden zijn **detectie** en **preventie**. Wanneer de WAF is ingesteld in **detectie** modus alle bedreigingen zijn opgeslagen in een logboekbestand. In **preventie** modus, gebeurtenissen worden nog wel geregistreerd, maar de aanvaller ontvangt een niet-geautoriseerde 403 reactie van de toepassingsgateway.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Web application firewall toevoegen aan een bestaande application gateway

Zorg ervoor dat u de nieuwste versie van Azure PowerShell. Zie voor meer informatie [gebruik Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

1. Aanmelden bij uw Azure-account.

    ```powershell
    Login-AzureRmAccount
    ```

2. Selecteer het abonnement moet worden gebruikt voor dit scenario.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Ophalen van de gateway waar u wilt een WAF toevoegen.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

4. De SKU WAF configureren. De beschikbare grootten zijn **WAF\_grote** en **WAF\_gemiddeld**. Wanneer u een WAF, de laag moet **WAF**. Controleer de capaciteit bij het instellen van de SKU.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

5. Configureer de instellingen WAF zoals gedefinieerd in het volgende voorbeeld. Voor **FirewallMode**, de beschikbare waarden zijn **preventie** en **detectie**.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

6. De toepassingsgateway bijwerken met de instellingen die u hebt gedefinieerd in de vorige stap.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Met deze opdracht werkt de toepassingsgateway met een WAF. Zie voor informatie over het weergeven van Logboeken voor uw toepassingsgateway, [Application Gateway diagnostics](application-gateway-diagnostics.md). Vanwege de aard van de beveiliging van een WAF, Bekijk Logboeken regelmatig om te begrijpen van de beveiligingsstatus van uw webtoepassingen.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Een toepassingsgateway maken met een web application firewall

De volgende stappen gaat u door het hele proces van het maken van een toepassingsgateway met een WAF.

Zorg ervoor dat u de nieuwste versie van Azure PowerShell. Zie voor meer informatie [gebruik Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).

1. Aanmelden bij Azure uitgevoerd `Login-AzureRmAccount`. U wordt gevraagd om te verifiëren met uw referenties.

2. Controleer de abonnementen voor het account door het uitvoeren van `Get-AzureRmSubscription`.

3. Kies welk Azure-abonnement te gebruiken.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep voor de toepassingsgateway.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruikt.

In het voorgaande voorbeeld, er een resourcegroep genaamd 'appgw-RG"met de locatie 'VS-West.' hebt gemaakt

> [!NOTE]
> Als u voor uw toepassingsgateway een aangepaste test moet configureren, raadpleegt u [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Met PowerShell een toepassingsgateway maken met aangepaste tests). Zie voor meer informatie [aangepaste tests en statuscontrole](application-gateway-probe-overview.md).

### <a name="configure-a-virtual-network"></a>Een virtueel netwerk configureren

Een application gateway vereist een subnet van een eigen. In deze stap maakt u een virtueel netwerk met een adresruimte van 10.0.0.0/16 en twee subnetten, één voor de toepassingsgateway en één voor leden van de back-end-pool.

```powershell
# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for application gateway instances. With a smaller subnet, you might not be able to add more instances of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the back-end pool members subnet.
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previously created subnets.
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-the-public-ip-address"></a>Het openbare IP-adres configureren

Verwerken van externe aanvragen, vereist de toepassingsgateway een openbaar IP-adres. Dit openbare IP-adres mag niet hebben een `DomainNameLabel` gedefinieerd die worden gebruikt door de toepassingsgateway.

```powershell
# Create a public IP address for use with the application gateway. Defining the `DomainNameLabel` during creation is not supported for use with the application gateway.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>De toepassingsgateway configureren

```powershell
# Create an IP configuration to configure which subnet the application gateway uses. When the application gateway starts, it picks up an IP address from the configured subnet and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a back-end pool to hold the addresses or NIC handles for the application that the application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authentication certificate to be used to communicate with the back-end servers.
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Configure the back-end HTTP settings to be used to define how traffic is routed to the back-end pool. The authentication certificate used in the previous step is added to the back-end HTTP settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a front-end port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a front-end IP configuration to associate the public IP address with the application gateway.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the application gateway. This certificate is used to decrypt and re-encrypt the traffic on the application gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the application gateway. Assign the front-end IP configuration, port, and SSL certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

# Create a load-balancer routing rule that configures the load balancer behavior. In this example, a basic round-robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the application gateway.
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use.
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

# Configure the WAF configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the application gateway by using all the previously created configuration objects.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Toepassingsgateways gemaakt met de basisconfiguratie van WAF zijn geconfigureerd met CRS 3.0 voor beveiliging.

## <a name="get-an-application-gateway-dns-name"></a>Ophalen van een application gateway DNS-naam

Nadat de gateway is gemaakt, wordt de volgende stap is het front-end voor de communicatie te configureren. Wanneer u een openbaar IP-adres gebruikt, moet de toepassingsgateway een dynamisch toegewezen DNS-naam, die geen beschrijvende. Om ervoor te zorgen dat gebruikers de toepassingsgateway kunnen bereikt, een CNAME-record te gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway. Zie voor meer informatie [een aangepaste domeinnaam configureren voor een Azure cloudservice](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Voor het configureren van een alias ophalen van gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de PublicIPAddress-element dat is gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen aan deze DNS-naam wijst. We raden niet met behulp van A-records, omdat het VIP kan worden gewijzigd wanneer de toepassingsgateway opnieuw wordt opgestart.

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

Zie voor meer informatie over het configureren van diagnostische logboekregistratie voor logboekregistratie van de gebeurtenissen die zijn gedetecteerd of voorkomen dat met een WAF, [Application Gateway diagnostics](application-gateway-diagnostics.md).

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png

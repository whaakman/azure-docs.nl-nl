---
title: Een Azure Application Gateway maken en beheren - PowerShell | Microsoft Docs
description: Op deze pagina vindt u instructies voor het maken, configureren, openen en verwijderen van een Azure-toepassingsgateway met Azure Resource Manager
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: c419e1032476818e430251246022ae14e4355024
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Een toepassingsgateway maken, openen of verwijderen met Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Application Gateway bevat veel ADC-functies (Application Delivery Controller), waaronder HTTP-taakverdeling, op cookies gebaseerde sessieaffiniteit, SSL-offload (Secure Sockets Layer), aangepaste statustests en ondersteuning voor meerdere locaties. Een volledige lijst met ondersteunde functies vindt u in [Overzicht van Application Gateway](application-gateway-introduction.md).

In dit artikel vindt u meer informatie over de stappen voor het maken, configureren, openen en verwijderen van een toepassingsgateway.

> [!IMPORTANT]
> Voordat u met Azure-resources gaat werken, is het belangrijk om te weten dat Azure momenteel twee implementatiemodellen heeft: Resource Manager en het klassieke model. Zorg ervoor dat u begrijpt wat [implementatiemodellen en hulpprogramma's](../azure-classic-rm.md) zijn voordat u met een Azure-resource gaat werken. U kunt de documentatie voor verschillende hulpprogramma's bekijken door op de tabbladen boven aan dit artikel te klikken. In dit document leest u meer over het maken van een toepassingsgateway met Azure Resource Manager. Als u de klassieke versie wilt gebruiken, gaat u naar [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md) (Een klassieke toepassingsgatewayimplementatie maken met PowerShell).

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
1. Als u een bestaand virtueel netwerk hebt, selecteert u een bestaand leeg subnet of maakt u een subnet in uw bestaande virtuele netwerk, uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway niet implementeren op een ander virtueel netwerk dan de resources die u wilt implementeren achter de toepassingsgateway.
1. De servers die u voor gebruik van de toepassingsgateway configureert, moeten al bestaan in het virtuele netwerk of hier hun eindpunten hebben. Een andere optie is om er een openbaar IP- of VIP-adres aan toe te wijzen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is er vereist om een toepassingsgateway te maken?

* **Back-endserverpool:** de lijst met IP-adressen, FQDN's of NIC's van de back-endservers. Als er IP-adressen worden gebruikt, moeten deze deel uitmaken van het subnet van het virtuele netwerk of moeten dit openbare IP-/VIP-adressen zijn.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel:** de regel verbindt de listener met de back-endserverpool en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

1. Meld u aan bij Azure en voer uw referenties in.

  ```powershell
  Login-AzureRmAccount
  ```

2. Controleer de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```

3. Kies welk Azure-abonnement u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. Maak een resourcegroep (u kunt deze stap overslaan als u een bestaande resourcegroep gebruikt).

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat bij alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep wordt gebruikt.

In het bovenstaande voorbeeld is er een resourcegroep gemaakt met de naam **ContosoRG** en de locatie **VS - oost**.

> [!NOTE]
> Ga naar [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Met PowerShell een toepassingsgateway maken met aangepaste tests) als u voor uw toepassingsgateway een aangepaste test moet configureren. Bekijk [Custom probes and health monitoring](application-gateway-probe-overview.md) (Aangepaste tests en statusbewaking) voor meer informatie.


## <a name="create-the-application-gateway-configuration-objects"></a>Het configuratieobject voor de toepassingsgateway maken

Alle configuratie-items moeten zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

```powershell
# Create a subnet and assign the address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with the address space of 10.0.0.0/16 and add the subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used to connect to the application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. The gateway picks up an IP addressfrom the configured subnet and routes network traffic to the IP addresses in the backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with the addresses of your web servers. These backend pool members are all validated to be healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed to them when requests come into the application gateway. Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings to determine the protocol and port that is used when sending traffic to the backend servers. Cookie-based sessions are also determined by the backend HTTP settings.  If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used to connect to the application gateway through the public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure the frontend IP configuration with the public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure the listener.  The listener is a combination of the front end IP configuration, protocol, and port and is used to receive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used to route traffic to the backend servers. The backend pool settings, listener, and backend pool created in the previous steps make up the rule. Based on the criteria defined traffic is routed to the appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU for the application gateway, this determines the size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create the application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Haal als u klaar bent DNS- en VIP-details van de toepassingsgateway op van de openbare IP-resource die aan de toepassingsgateway is gekoppeld.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-the-application-gateway"></a>De toepassingsgateway verwijderen

In het volgende voorbeeld wordt de toepassingsgateway verwijderd.

```powershell
# Retrieve the application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops the application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> U kunt de switch **-force** gebruiken om het bevestigingsbericht voor de verwijdering niet te laten weergeven.

Gebruik de cmdlet `Get-AzureRmApplicationGateway` als u wilt controleren of de service is verwijderd. Deze stap is niet vereist.

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a>DNS-naam van toepassingsgateway verkrijgen

Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbare IP gebruikt, heeft de toepassingsgateway een dynamisch toegewezen DNS-naam nodig. Dit is niet gebruiksvriendelijk. Om ervoor te zorgen dat eindgebruikers de toepassingsgateway kunnen bereiken, kan een CNAME-record worden gebruikt die verwijst naar het openbare eindpunt van de toepassingsgateway. Daartoe haalt u details van de toepassingsgateway en de bijbehorende IP-/ DNS-naam op met het PublicIPAddress-element gekoppeld aan de toepassingsgateway. Dit kunt u doen met Azure DNS of andere DNS-providers door een CNAME-record te maken dat verwijst naar het [openbare IP-adres](../dns/dns-custom-domain.md#public-ip-address). Het gebruik van A-records wordt niet aanbevolen, omdat het VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

> [!NOTE]
> Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stap uit:

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a>Volgende stappen

Ga naar: [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Een toepassingsgateway voor SSL-offload configureren) als u SSL-offload wilt configureren.

Ga naar: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Een toepassingsgateway met een interne load balancer (ILB) maken) als u een toepassingsgateway wilt configureren voor gebruik met een interne load balancer.

Als u meer informatie wilt over de algemene opties voor taakverdeling, gaat u naar:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

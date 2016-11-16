---
title: Een toepassingsgateway maken, openen of verwijderen met Azure Resource Manager | Microsoft Docs
description: Op deze pagina vindt u instructies voor het maken, configureren, openen en verwijderen van een Azure-toepassingsgateway met Azure Resource Manager
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d4da5dd65b39011ed0abb8bbbd47542d66e73ad6


---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Een toepassingsgateway maken, openen of verwijderen met Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)
> 
> 

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Application Gateway bevat veel ADC-functies (Application Delivery Controller), waaronder HTTP-taakverdeling, op cookies gebaseerde sessieaffiniteit, SSL-offload (Secure Sockets Layer), aangepaste statustests en ondersteuning voor meerdere locaties. Een volledige lijst met ondersteunde functies vindt u in [Application Gateway Overview](application-gateway-introduction.md) (Overzicht van Application Gateway)

In dit artikel vindt u meer informatie over de stappen voor het maken, configureren, openen en verwijderen van een toepassingsgateway.

> [!IMPORTANT]
> Voordat u met Azure-resources gaat werken, is het belangrijk om te weten dat Azure momenteel twee implementatiemodellen heeft: Resource Manager en het klassieke model. Zorg ervoor dat u begrijpt wat [implementatiemodellen en hulpprogramma's](../azure-classic-rm.md) zijn voordat u met een Azure-resource gaat werken. U kunt de documentatie voor verschillende hulpprogramma's bekijken door op de tabbladen boven aan dit artikel te klikken. In dit document leest u meer over het maken van een toepassingsgateway met Azure Resource Manager. Als u de klassieke versie wilt gebruiken, gaat u naar [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md) (Een klassieke toepassingsgatewayimplementatie maken met PowerShell).
> 
> 

## <a name="before-you-begin"></a>Voordat u begint
1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
2. Als u een bestaand virtueel netwerk hebt, selecteert u een bestaand leeg subnet of maakt u een subnet in uw bestaande virtuele netwerk, uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway niet implementeren op een ander virtueel netwerk dan de resources die u wilt implementeren achter de toepassingsgateway.
3. De servers die u voor gebruik van de toepassingsgateway configureert, moeten al bestaan in het virtuele netwerk of hier hun eindpunten hebben. Een andere optie is om er een openbaar IP- of VIP-adres aan toe te wijzen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is er vereist om een toepassingsgateway te maken?
* **Back-endserverpool:** de lijst met IP-adressen van de back-endservers. De IP-adressen moeten ofwel deel uitmaken van het subnet van het virtuele netwerk, ofwel openbare IP-/VIP-adressen zijn.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel:** de regel verbindt de listener met de back-endserverpool en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken
Het verschil tussen het gebruik van Azure Classic en Azure Resource Manager zit hem in de volgorde waarin u de toepassingsgateway maakt en in de items die u moet configureren.

Met Resource Manager worden alle items waaruit een toepassingsgateway bestaat, afzonderlijk geconfigureerd en vervolgens samengesteld om de toepassingsgatewayresource te maken.

Hieronder worden de stappen voor het maken van een toepassingsgateway beschreven.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager
Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

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
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat bij alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep wordt gebruikt.

In het bovenstaande voorbeeld is er een resourcegroep gemaakt met de naam appgw-RG en de locatie VS - west.

> [!NOTE]
> Als u voor uw toepassingsgateway een aangepaste test moet configureren, raadpleegt u [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Met PowerShell een toepassingsgateway maken met aangepaste tests). Bekijk [Custom probes and health monitoring](application-gateway-probe-overview.md) (Aangepaste tests en statusbewaking) voor meer informatie.
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet maken voor de toepassingsgateway
In het volgende voorbeeld ziet u hoe u een virtueel netwerk maakt met Resource Manager.

### <a name="step-1"></a>Stap 1
Wijs het adresbereik 10.0.0.0/24 toe aan de subnetvariabele die u gaat gebruiken om een virtueel netwerk te maken.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Stap 2
Maak een virtueel netwerk met de naam appgwvnet in de resourcegroep appgw-rg. Doe dit voor de regio VS - west. Gebruik het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Stap 3
Wijs een subnetvariabele toe voor gebruik in de volgende stappen, waarin u een toepassingsgateway gaat maken.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-frontend-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie
Maak de openbare IP-resource publicIP01 in de resourcegroep appgw-rg voor de regio VS - west.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway-configuration-object"></a>Een configuratieobject voor de toepassingsgateway maken
U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1
Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam gatewayIP01. Wanneer de toepassingsgateway wordt geopend, wordt er een IP-adres opgehaald via het geconfigureerde subnet en wordt het netwerkverkeer omgeleid naar de IP-adressen in de groep met back-end-IP's. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Stap 2
Configureer de back-end-IP-adrespool met de naam pool01. Gebruik hiervoor de IP-adressen 134.170.185.46, 134.170.188.221 en 134.170.185.50. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer ontvangen dat afkomstig is van het front-end-IP-eindpunt. U vervangt de bovenstaande IP-adressen met de IP-adreseindpunten van uw eigen toepassing.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

### <a name="step-3"></a>Stap 3
Configureer de toepassingsgateway door voor het netwerkverkeer met load balancing in de back-endpool poolsetting01 in te stellen.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

### <a name="step-4"></a>Stap 4
Configureer de front-end-IP-poort frontendport01 voor het openbare IP-eindpunt.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Stap 5
Maak een front-end-IP-configuratie met de naam fipconfig01 en koppel het openbare IP-adres aan de front-end-IP-configuratie.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Stap 6
Maak een listener met de naam listener01 en koppel de front-endpoort aan de front-end-IP-configuratie.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Stap 7
Maak een load balancer-routeringsregel met de naam rule01 voor het configureren van het load balancer-gedrag.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Stap 8
Configureer de exemplaargrootte van de toepassingsgateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> De standaardwaarde voor *InstanceCount* is 2 en de maximale waarde is 10. De standaardwaarde voor *GatewaySize* is Medium. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.
> 
> 

## <a name="create-an-application-gateway-by-using-newazurermapplicationgateway"></a>Een toepassingsgateway maken met behulp van New-AzureRmApplicationGateway
Maak een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In dit voorbeeld heeft de toepassingsgateway de naam appgwtest.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

### <a name="step-9"></a>Stap 9
Haal DNS- en VIP-details van de toepassingsgateway op van de openbare IP-resource die aan de toepassingsgateway is gekoppeld.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-an-application-gateway"></a>Een toepassingsgateway verwijderen
Volg deze stappen als u een toepassingsgateway wilt verwijderen:

### <a name="step-1"></a>Stap 1
Haal het toepassingsgatewayobject op en koppel dit aan de variabele $getgw.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Stap 2
Gebruik **Stop-AzureRmApplicationGateway** om de toepassingsgateway te stoppen.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

Nadat de toepassingsgateway is gestopt, gebruikt u de cmdlet **Remove-AzureRmApplicationGateway** om de service te verwijderen.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> U kunt de switch **-force** gebruiken om het bevestigingsbericht voor de verwijdering niet te laten weergeven.
> 
> 

Als u wilt controleren of de service is verwijderd, gebruikt u de cmdlet **Get-AzureRmApplicationGateway**. Deze stap is niet vereist.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>DNS-naam van toepassingsgateway verkrijgen
Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbare IP gebruikt, heeft de toepassingsgateway een dynamisch toegewezen DNS-naam nodig. Dit is niet gebruiksvriendelijk. Opdat eindgebruikers de toepassingsgateway kunnen bereiken, kan een CNAME-record worden gebruikt om te wijzen naar het openbare eindpunt van de toepassingsgateway. [Een aangepaste domeinnaam configureren voor in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Daartoe haalt u details van de toepassingsgateway en de bijbehorende IP-/ DNS-naam op met het PublicIPAddress-element gekoppeld aan de toepassingsgateway. De DNS-naam van de toepassingsgateway moet worden gebruikt om een CNAME-record te maken die de twee webtoepassingen naar deze DNS-naam wijst. Het gebruik van A-records wordt niet aanbevolen, omdat het VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

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
Als u SSL-offload wilt configureren, raadpleegt u [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Een toepassingsgateway voor SSL-offload configureren).

Als u een toepassingsgateway wilt configureren voor gebruik met een interne load balancer, raadpleegt u [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Een toepassingsgateway met een interne load balancer (ILB) maken).

Als u meer informatie wilt over de algemene opties voor load balancing, raadpleegt u:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)




<!--HONumber=Nov16_HO2-->



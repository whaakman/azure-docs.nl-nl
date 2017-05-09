---
title: Een Azure Application Gateway maken en beheren - PowerShell | Microsoft Docs
description: Op deze pagina vindt u instructies voor het maken, configureren, openen en verwijderen van een Azure-toepassingsgateway met Azure Resource Manager
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 8a0eb841b1a41a14e443b6ce93e6b8fb8985a803
ms.contentlocale: nl-nl
ms.lasthandoff: 05/02/2017


---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Een toepassingsgateway maken, openen of verwijderen met Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud).
Application Gateway bevat veel ADC-functies (Application Delivery Controller), waaronder HTTP-taakverdeling, op cookies gebaseerde sessieaffiniteit, SSL-offload (Secure Sockets Layer), aangepaste statustests en ondersteuning voor meerdere locaties.

Een volledige lijst met ondersteunde functies vindt u in [Application Gateway Overview](application-gateway-introduction.md) (Overzicht van Application Gateway)

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

In het bovenstaande voorbeeld is er een resourcegroep gemaakt met de naam **appgw-RG** en de locatie **VS - west**.

> [!NOTE]
> Ga naar [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md) (Met PowerShell een toepassingsgateway maken met aangepaste tests) als u voor uw toepassingsgateway een aangepaste test moet configureren. Bekijk [Custom probes and health monitoring](application-gateway-probe-overview.md) (Aangepaste tests en statusbewaking) voor meer informatie.

## <a name="create-a-virtual-network-and-a-subnet"></a>Een virtueel netwerk en een subnet maken

In het volgende voorbeeld ziet u hoe u een virtueel netwerk maakt met Resource Manager. In dit voorbeeld wordt er een VNET gemaakt voor Application Gateway. Voor Application Gateway is een eigen subnet vereist. Daarom is het gemaakte subnet voor Application Gateway kleiner dan de VNET-adresruimte. Door het gebruik van een kleiner subnet kunnen andere resources, waaronder webservers, in hetzelfde VNET worden geconfigureerd.

### <a name="step-1"></a>Stap 1

Wijs het adresbereik 10.0.0.0/24 toe aan de subnetvariabele die u gaat gebruiken om een virtueel netwerk te maken. In deze stap wordt het subnetconfiguratieobject voor Application Gateway gemaakt, dat in het volgende voorbeeld wordt gebruikt.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam **appgwvnet** in de resourcegroep **appgw-rg**. Doe dit voor de regio VS - west. Gebruik het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24. In deze stap wordt de configuratie van het VNET voltooid met één subnet voor Application Gateway.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Stap 3

Wijs de subnetvariabele toe voor de volgende stappen. Deze variabele wordt doorgegeven aan de cmdlet `New-AzureRMApplicationGateway` in een toekomstige stap.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

Maak de openbare IP-resource **publicIP01** in de resourcegroep **appgw-rg** voor de regio VS - west. Voor Application Gateway kan een openbaar IP-adres, een intern IP-adres of beide worden gebruikt om aanvragen voor de taakverdeling te ontvangen.  In dit voorbeeld wordt er alleen een openbaar IP-adres gebruikt. In het volgende voorbeeld is er geen DNS-naam geconfigureerd voor het maken van het openbare IP-adres.  Application Gateway biedt geen ondersteuning voor aangepaste DNS-namen voor openbare IP-adressen.  Als er een aangepaste naam is vereist voor het openbare eindpunt, moet er een CNAME-record worden gemaakt die verwijst naar de automatisch gegenereerde DNS-naam voor het openbare IP-adres.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

> [!NOTE]
> Er wordt een IP-adres toegewezen aan de toepassingsgateway wanneer de service wordt gestart.

## <a name="create-the-application-gateway-configuration-objects"></a>Het configuratieobject voor de toepassingsgateway maken

Alle configuratie-items moeten zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

### <a name="step-1"></a>Stap 1

Maak voor de toepassingsgateway een IP-configuratie en geef deze de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, wordt er een IP-adres opgehaald via het geconfigureerde subnet en wordt het netwerkverkeer omgeleid naar de IP-adressen in de back-end-IP-pool. Onthoud dat elk exemplaar één IP-adres gebruikt.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Stap 2

Configureer de back-end-IP-adrespool met de naam **pool01** met IP-adressen voor **pool1**. Deze IP-adressen zijn de IP-adressen van de resources waarop de webtoepassing wordt gehost die moet worden beveiligd door de toepassingsgateway. Aan de hand van basistests of aangepaste tests wordt gecontroleerd of alle back-endpoolleden in orde zijn.  Vervolgens wordt verkeer hiernaartoe doorgestuurd wanneer er aanvragen binnenkomen in de toepassingsgateway. Back-endpools kunnen worden gebruikt door meerdere regels in de toepassingsgateway. Dit betekent dat een back-endpool kan worden gebruikt voor meerdere webtoepassingen die zich op dezelfde host bevinden.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50
```

In dit voorbeeld zijn er twee back-endpools voor het verzenden van netwerkverkeer op basis van het URL-pad. Een pool ontvangt verkeer vanuit het URL-pad '/video' en andere pool ontvangt verkeer vanuit het pad '/image'. Vervang de bovenstaande IP-adressen door de IP-adreseindpunten van uw eigen toepassing.

### <a name="step-3"></a>Stap 3

Configureer de toepassingsgateway door voor het netwerkverkeer met load balancing in de back-endpool **poolsetting01** in te stellen. Elke back-endpool kan een eigen instelling van de back-endpool hebben.  Back-end-HTTP-instellingen worden gebruikt door regels om verkeer te verzenden naar de juiste back-endpoolleden. Met back-end-HTTP-instellingen wordt bepaald welk protocol en welke poort moeten worden gebruikt voor het verzenden van verkeer naar de back-endpoolleden. Sessies op basis van cookies worden ook bepaald door de back-end-HTTP-instellingen.  Als sessieaffiniteit op basis van cookies is ingeschakeld, wordt verkeer verzonden naar dezelfde back-end als bij de vorige aanvragen voor elk pakket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
```

### <a name="step-4"></a>Stap 4

Configureer de front-endpoort voor een toepassingsgateway. Het configuratieobject front-endpoort wordt gebruikt door een listener om te definiëren via welke poort Application Gateway naar verkeer op de listener luistert.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>Stap 5

Configureer het front-end-IP-adres met openbaar IP-eindpunt. Het configuratie-object front-end-IP-adres wordt gebruikt door een listener om het uitgaande IP-adres te koppelen aan de listener.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>Stap 6

Configureer de listener. In deze stap wordt de listener voor het openbare IP-adres en de poort voor het ontvangen van binnenkomend netwerkverkeer geconfigureerd. In het volgende voorbeeld worden de eerder geconfigureerde front-end-IP-configuratie, front-endpoortconfiguratie en een protocol (http of https) gebruikt en wordt de listener geconfigureerd. In dit voorbeeld luistert de listener luistert naar HTTP-verkeer op poort 80 voor het openbare IP-adres dat eerder is gemaakt.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>Stap 7

Maak een load balancer-routeringsregel met de naam **rule01** voor het configureren van het load balancer-gedrag. De regel bestaat uit de back-endpoolinstellingen, listener en back-endpool die zijn gemaakt in de vorige stappen. Verkeer wordt doorgestuurd naar de desbetreffende back-end op basis van de gedefinieerde criteria.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting01 -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>Stap 8

Configureer het aantal exemplaren en de grootte voor de toepassingsgateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> De standaardwaarde voor **InstanceCount** is 2 en de maximale waarde is 10. De standaardwaarde voor **GatewaySize** is Medium. U kunt kiezen tussen **Standard_Small**, **Standard_Medium** en **Standard_Large**.

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Maak een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In dit voorbeeld heeft de toepassingsgateway de naam **appgwtest**.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Haal DNS- en VIP-details van de toepassingsgateway op van de openbare IP-resource die aan de toepassingsgateway is gekoppeld.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-the-application-gateway"></a>De toepassingsgateway verwijderen

Volg deze stappen als u een toepassingsgateway wilt verwijderen:

### <a name="step-1"></a>Stap 1

Haal het toepassingsgatewayobject op en koppel dit aan de variabele `$getgw`.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Stap 2

Gebruik `Stop-AzureRmApplicationGateway` om de toepassingsgateway te stoppen.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw
```

Nadat de toepassingsgateway is gestopt, gebruikt u de cmdlet `Remove-AzureRmApplicationGateway` om de service te verwijderen.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> U kunt de switch **-force** gebruiken om het bevestigingsbericht voor de verwijdering niet te laten weergeven.

Gebruik de cmdlet `Get-AzureRmApplicationGateway` als u wilt controleren of de service is verwijderd. Deze stap is niet vereist.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>DNS-naam van toepassingsgateway verkrijgen

Wanneer de gateway is gemaakt, gaat u in de volgende stap de front-end voor communicatie configureren. Wanneer u een openbare IP gebruikt, heeft de toepassingsgateway een dynamisch toegewezen DNS-naam nodig. Dit is niet gebruiksvriendelijk. Om ervoor te zorgen dat eindgebruikers de toepassingsgateway kunnen bereiken, kan een CNAME-record worden gebruikt die verwijst naar het openbare eindpunt van de toepassingsgateway. [Een aangepaste domeinnaam configureren voor in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Als u de dynamisch gemaakte DNS-naam wilt zoeken, haalt u details van de toepassingsgateway en de bijbehorende IP-/ DNS-naam op met het PublicIPAddress-element dat is gekoppeld aan de toepassingsgateway. De DNS-naam van de toepassingsgateway moet worden gebruikt om een CNAME-record te maken die de twee webtoepassingen naar deze DNS-naam wijst. Het gebruik van A-records wordt niet aanbevolen, omdat het VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

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

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

```powershell
Remove-AzureRmResourceGroup -Name appgw-RG
```

## <a name="next-steps"></a>Volgende stappen

Ga naar: [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Een toepassingsgateway voor SSL-offload configureren) als u SSL-offload wilt configureren.

Ga naar: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Een toepassingsgateway met een interne load balancer (ILB) maken) als u een toepassingsgateway wilt configureren voor gebruik met een interne load balancer.

Als u meer informatie wilt over de algemene opties voor taakverdeling, gaat u naar:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



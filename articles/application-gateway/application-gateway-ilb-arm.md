---
title: Met behulp van Azure Application Gateway met interne Load Balancer - PowerShell | Microsoft Docs
description: Op deze pagina vindt u instructies voor het maken, configureren, openen en verwijderen van een Azure-toepassingsgateway met een interne load balancer (ILB) voor Azure Resource Manager
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2018
ms.author: victorh
ms.openlocfilehash: 92d0e079f9fafbb6c000c6b1746f37a16add4cf7
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417344"
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Een toepassingsgateway met een interne load balancer (ILB) maken

Azure Application Gateway kan worden geconfigureerd met een internetgerichte VIP of met een intern eindpunt dat geen toegang heeft tot het internet. Dit heet ook wel een ILB-eindpunt (interne load balancer). Het is een goed idee om de gateway te configureren met een ILB als u interne line-of-business-toepassingen gebruikt die geen toegang hebben tot het internet. Ook is dit handig als u services en lagen gebruikt in een toepassing met meerdere lagen die zich binnen een beveiligingsgrens bevinden, en als deze toepassing geen toegang heeft tot het internet, maar er wel round-robinbelastingverdeling, sessiepersistentie of SSL-beëindiging (Secure Sockets Layer) vereist is.

In dit artikel worden de stappen beschreven voor het configureren van een toepassingsgateway met een ILB.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
2. U maakt een virtueel netwerk en een subnet voor de toepassingsgateway. Zorg ervoor dat er geen virtuele machines en cloudimplementaties zijn die gebruikmaken van het subnet. De toepassingsgateway moet afzonderlijk in een subnet van een virtueel netwerk staan.
3. De servers die u voor gebruik van de toepassingsgateway configureert, moeten al bestaan in het virtuele netwerk of hier hun eindpunten hebben. Een andere optie is om er een openbaar IP- of VIP-adres aan toe te wijzen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is er vereist om een toepassingsgateway te maken?

* **Back-endserverpool:** De lijst met IP-adressen van de back-endservers. De IP-adressen moeten ofwel deel uitmaken van het virtueel netwerk, maar zich bevinden in een ander subnet voor de toepassingsgateway, ofwel openbare IP-/VIP-adressen zijn.
* **Instellingen van back-endserverpool:** Elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort:** Dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** De listener beschikt over een front-endpoort, een protocol (Http of Https; deze zijn hoofdlettergevoelig), en de SSL-certificaatnaam (als SSL-offloading configureert).
* **Regel:** De regel verbindt de listener met de back-endserverpool en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt. Momenteel wordt alleen de regel *basic* ondersteund. De regel *basic* is een vorm van round-robinbelastingverdeling.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van Azure Classic en Azure Resource Manager zit hem in de volgorde waarin u de toepassingsgateway maakt en in de items die u moet configureren.
Met Resource Manager worden alle items waaruit een toepassingsgateway bestaat afzonderlijk geconfigureerd en vervolgens samengesteld om de toepassingsgatewayresource te maken.

Dit zijn de stappen voor het maken van een toepassingsgateway:

1. Een resourcegroep maken voor Resource Manager
2. Een virtueel netwerk en een subnet maken voor de toepassingsgateway
3. Een configuratieobject voor de toepassingsgateway maken
4. Een toepassingsgatewayresource maken

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u overschakelt naar de PowerShell-modus om de Azure Resource Manager-cmdlets te gebruiken. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

### <a name="step-1"></a>Stap 1

```powershell
Connect-AzureRmAccount
```

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

U wordt gevraagd om u te verifiëren met uw referenties.

### <a name="step-3"></a>Stap 3

Kies welk Azure-abonnement u wilt gebruiken.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Stap 4

Maak een nieuwe resourcegroep (u kunt deze stap overslaan als u een bestaande resourcegroep gebruikt).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat bij alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep wordt gebruikt.

In het voorgaande voorbeeld wordt een resourcegroep met de naam 'appgw-rg' en de locatie VS-West' gemaakt.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet maken voor de toepassingsgateway

In het volgende voorbeeld ziet u hoe u een virtueel netwerk maakt met Resource Manager:

### <a name="step-1"></a>Stap 1

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Deze stap wijst het netwerkadresbereik 10.0.0.0/24 toe aan een subnetvariabele om te worden gebruikt voor het maken van een virtueel netwerk.

### <a name="step-2"></a>Stap 2

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

Deze stap maakt u een virtueel netwerk met de naam 'appgwvnet' in de resource group 'appgw-rg' voor de regio VS-West, waarbij het voorvoegsel 10.0.0.0/16 met het subnet 10.0.0.0/24.

### <a name="step-3"></a>Stap 3

```powershell
$subnet = $vnet.subnets[0]
```

Deze stap wijst u het subnetobject toe aan de variabele $subnet voor de volgende stappen.

## <a name="create-an-application-gateway-configuration-object"></a>Een configuratieobject voor de toepassingsgateway maken

### <a name="step-1"></a>Stap 1

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Deze stap maakt u een toepassingsgateway IP-configuratie met de naam 'gatewayIP01'. Wanneer de toepassingsgateway wordt geopend, wordt er een IP-adres opgehaald via het geconfigureerde subnet en wordt het netwerkverkeer omgeleid naar de IP-adressen in de back-end-IP-pool. Onthoud dat elk exemplaar één IP-adres gebruikt.

### <a name="step-2"></a>Stap 2

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

Deze stap configureert u de back-end IP-adresgroep met de naam 'pool01' met IP-adressen '10.1.1.8, 10.1.1.9, 10.1.1.10'. Dit zijn de IP-adressen waardoor het netwerkverkeer van het front-end-IP-eindpunt binnenkomt. U vervangt de bovenstaande IP-adressen met de IP-adreseindpunten van uw eigen toepassing.

### <a name="step-3"></a>Stap 3

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Deze stap configureert u netwerkverkeer van application gateway-instelling 'poolsetting01' voor de belasting met gelijke taakverdeling in de back-endpool.

### <a name="step-4"></a>Stap 4

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

Deze stap configureert u de front-end-IP-poort met de naam 'frontendport01' voor de ILB.

### <a name="step-5"></a>Stap 5

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

Deze stap maakt u de front-end-IP-configuratie met de naam 'fipconfig01' en associeert deze met een privé-IP-adres van een subnet voor de huidige virtueel netwerk.

### <a name="step-6"></a>Stap 6

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

Deze stap maakt u een listener met de naam 'listener01' en koppelt u de front-endpoort aan de front-end-IP-configuratie.

### <a name="step-7"></a>Stap 7

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Deze stap maakt u de load balancer-routeringsregel met de naam 'rule01"waarmee het gedrag van de load balancer worden geconfigureerd.

### <a name="step-8"></a>Stap 8

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Deze stap configureert u de exemplaargrootte van de toepassingsgateway.

> [!NOTE]
> De standaardwaarde voor de capaciteit is 2. Voor de Sku-naam, kunt u kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Een toepassingsgateway maken met behulp van New-AzureApplicationGateway

Hiermee maakt een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In dit voorbeeld heeft de toepassingsgateway de naam appgwtest.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Deze stap maakt u een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In dit voorbeeld heeft de toepassingsgateway de naam appgwtest.

## <a name="delete-an-application-gateway"></a>Een toepassingsgateway verwijderen

Als u wilt verwijderen van een application gateway, moet u de volgende stappen in volgorde uit:

1. Gebruik de cmdlet `Stop-AzureRmApplicationGateway` om de gateway te stoppen.
2. Gebruik de cmdlet `Remove-AzureRmApplicationGateway` om de gateway te verwijderen.
3. Gebruik de cmdlet `Get-AzureApplicationGateway` om te controleren of de gateway is verwijderd.

### <a name="step-1"></a>Stap 1

Haal het toepassingsgatewayobject op en koppel dit aan de variabele $getgw.

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Stap 2

Gebruik `Stop-AzureRmApplicationGateway` om de toepassingsgateway te stoppen. Dit voorbeeld laat zien de `Stop-AzureRmApplicationGateway` cmdlet uit op de eerste regel weergegeven, gevolgd door de uitvoer.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Nadat de toepassingsgateway is gestopt, gebruikt u de cmdlet `Remove-AzureRmApplicationGateway` om de service te verwijderen.

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> U kunt de switch **-force** gebruiken om het bevestigingsbericht voor de verwijdering niet te laten weergeven.

Gebruik de cmdlet `Get-AzureRmApplicationGateway` als u wilt controleren of de service is verwijderd. Deze stap is niet vereist.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
```

## <a name="next-steps"></a>Volgende stappen

Als u SSL-offload wilt configureren, raadpleegt u [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Een toepassingsgateway voor SSL-offload configureren).

Als u een Application Gateway wilt configureren voor gebruik met een ILB, raadpleegt u [Een Application Gateway met een interne Load Balancer (ILB) maken](application-gateway-ilb.md).

Als u meer informatie wilt over de algemene opties voor load balancing, raadpleegt u:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)


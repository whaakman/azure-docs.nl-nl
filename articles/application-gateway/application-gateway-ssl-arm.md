---
title: Configureren van SSL-offload - Azure Application Gateway - PowerShell | Microsoft Docs
description: Dit artikel bevat instructies voor het maken van een toepassingsgateway met SSL-offload met Azure Resource Manager
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: ee48ca45ae0d337b5b919dbbb28341caf8af0d45
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Een toepassingsgateway configureren voor SSL-offload met Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan zodanig worden geconfigureerd dat de Secure Sockets Layer-sessie (SSL) wordt beëindigd bij de gateway om kostbare SSL-ontsleutelingstaken te voorkomen die worden uitgevoerd in de webfarm. Met SSL-offload worden ook het instellen van de front-endserver en het beheer van de webtoepassing eenvoudiger.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
2. Een virtueel netwerk en een subnet voor de toepassingsgateway maken. Zorg ervoor dat er geen virtuele machines en cloudimplementaties zijn die gebruikmaken van het subnet. De toepassingsgateway moet afzonderlijk in een subnet van een virtueel netwerk staan.
3. De servers die u configureert voor het gebruik van de toepassingsgateway moeten bestaan of hebben hun eindpunten gemaakt in het virtuele netwerk of met een openbaar IP-adres of een virtueel IP-adres (VIP) toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is er vereist om een toepassingsgateway te maken?

* **Back-endserverpool**: de lijst met IP-adressen van de back-endservers. De IP-adressen die worden vermeld moeten deel uitmaken van het subnet van het virtuele netwerk of moeten een openbare IP-/ VIP.
* **Back-endserverpoolinstellingen**: elke pool heeft instellingen, zoals poort, het protocol en cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort**: dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener**: de listener beschikt over een front-endpoort, een protocol (Http of Https; deze instellingen zijn hoofdlettergevoelig), en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel**: de regel verbindt de listener met de back-endserverpool en definieert naar welke back-endserverpool om het verkeer naar wanneer dit bij een bepaalde listener aankomt. Momenteel wordt alleen de regel *basic* ondersteund. De regel *basic* is een vorm van round-robinbelastingverdeling.

**Aanvullende configuratieopmerkingen**

Voor het configureren van SSL-certificaten moet het protocol in **HttpListener** worden gewijzigd in **Https** (hoofdlettergevoelig). Voeg de **SslCertificate** element op de **HttpListener** met de variabele waarde die is geconfigureerd voor het SSL-certificaat. De front-endpoort moet worden bijgewerkt naar **443**.

**Inschakelen van cookies gebaseerde affiniteit**: U kunt een toepassingsgateway om ervoor te zorgen dat een aanvraag van een clientsessie altijd wordt omgeleid naar dezelfde virtuele machine in de webfarm configureren. Voeg hiervoor een sessiecookie waarmee de gateway om verkeer te regelen op de juiste wijze. Als u op cookies gebaseerde affiniteit wilt inschakelen, stelt u **CookieBasedAffinity** in op **Enabled** in het element **BackendHttpSettings**.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het klassieke implementatiemodel Azure gebruiken en Azure Resource Manager is de volgorde waarin u een application gateway en de items die moeten worden geconfigureerd.

Met Resource Manager worden alle onderdelen van een toepassingsgateway worden afzonderlijk geconfigureerd en deze vervolgens samen voor het maken van een toepassingsgatewayresource.

Dit zijn de stappen voor het maken van een toepassingsgateway:

1. [Een resourcegroep maken voor Resource Manager](#create-a-resource-group-for-resource-manager)
2. [Virtueel netwerk, subnet en openbaar IP-adres voor de toepassingsgateway maken](#create-virtual-network-subnet-and-public-IP-for-the-application-gateway)
3. [Een configuratieobject voor de gateway maken](#create-an-application-gateway-configuration-object)
4. [Een toepassingsgatewayresource maken](#create-an-application-gateway-resource)

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor Resource Manager

Zorg ervoor dat u overschakelt naar de PowerShell-modus om de Azure Resource Manager-cmdlets te gebruiken. Zie [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md) (Windows PowerShell gebruiken met Resource Manager) voor meer informatie.

   1. Voer de volgende opdracht in:

   ```powershell
   Login-AzureRmAccount
   ```

   2. Als u wilt controleren van de abonnementen voor het account, voert u de volgende opdrachten:

   ```powershell
   Get-AzureRmSubscription
   ```

   U wordt gevraagd om u te verifiëren met uw referenties.

   3. Om te selecteren welke van uw Azure-abonnementen te gebruiken, voer de volgende opdracht:

   ```powershell
   Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
   ```

   4. Voer de volgende opdracht voor het maken van een resourcegroep. (Sla deze stap over als u een bestaande resourcegroep gebruikt.)

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze instelling wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruikt.

In het vorige voorbeeld wordt er een resourcegroep aangeroepen hebt gemaakt **appgw-RG** en de locatie is **VS-West**.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet maken voor de toepassingsgateway

In het volgende voorbeeld ziet u hoe u een virtueel netwerk maakt met Resource Manager:

   1. Voer de volgende opdracht in:

   ```powershell
   $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
   ```

   Dit voorbeeld wordt het adresbereik toegewezen **10.0.0.0/24** aan een variabele subnet dat wordt gebruikt om een virtueel netwerk maken.

   2. Voer de volgende opdracht in:

   ```powershell
   $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

   In dit voorbeeld wordt een virtueel netwerk met de naam **appgwvnet** in de resourcegroep **appgw-rg** voor de **VS-West** regio met behulp van het voorvoegsel **10.0.0.0/16** met subnet **10.0.0.0/24**.

   3. Voer de volgende opdracht in:

   ```powershell
   $subnet = $vnet.Subnets[0]
   ```

   Dit voorbeeld wijst u het subnetobject toe aan de variabele **$subnet** voor de volgende stappen.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Voer de volgende opdracht voor het maken van een openbaar IP-adres voor de front-configuratie:

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

In dit voorbeeld wordt een openbare IP-resource **publicIP01** in de resourcegroep **appgw-rg** voor de **VS-West** regio.

## <a name="create-an-application-gateway-configuration-object"></a>Een configuratieobject voor de toepassingsgateway maken

   1. Voer de volgende opdracht voor het maken van een configuratieobject voor de gateway:

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
   ```

   In dit voorbeeld wordt een toepassingsgateway een IP-configuratie met de naam **gatewayIP01**. Wanneer de toepassingsgateway wordt geopend, neemt over een IP-adres van het geconfigureerde subnet en stuurt het netwerkverkeer naar de IP-adressen in de back-end-IP-adresgroep. Onthoud dat elk exemplaar één IP-adres gebruikt.

   2. Voer de volgende opdracht in:

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
   ```

   Dit voorbeeld configureert de backend-IP-adresgroep met de naam **pool01** met IP-adressen **134.170.185.46**, **134.170.188.221**, en **134.170.185.50** . Deze waarden zijn de IP-adressen waardoor het netwerkverkeer van het front-end-IP-eindpunt binnenkomt. Vervang de IP-adressen uit het voorbeeld hierboven door de IP-adressen van de eindpunten van uw webtoepassing.

   3. Voer de volgende opdracht in:

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
   ```

   Dit voorbeeld configureert u de instelling voor application gateway **poolsetting01** voor taakverdeling van netwerkverkeer in de groep back-end.

   4. Voer de volgende opdracht in:

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
   ```

   Dit voorbeeld configureert u de front-end-IP-poort met de naam **frontendport01** voor het openbare IP-eindpunt.

   5. Voer de volgende opdracht in:

   ```powershell
   $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
   ```

   Hiermee configureert u het certificaat dat wordt gebruikt voor de SSL-verbinding. Het certificaat moet zich in een PFX-indeling en het wachtwoord moet bestaan uit 4-12 tekens.

   6. Voer de volgende opdracht in:

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
   ```

   In dit voorbeeld wordt de front-end-IP-configuratie met de naam **fipconfig01** en koppelt u het openbare IP-adres aan de front-end-IP-configuratie.

   7. Voer de volgende opdracht in:

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
   ```

   In dit voorbeeld wordt de listener met de naam **listener01** en koppelt u de front-endpoort aan de front-end-IP-configuratie en het certificaat.

   8. Voer de volgende opdracht in:

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   In dit voorbeeld wordt de load balancer-routeringsregel met de naam **rule01** die het gedrag van de load balancer configureert.

   9. Voer de volgende opdracht in:

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   Hiermee configureert u de exemplaargrootte van de toepassingsgateway.

   > [!NOTE]
   > De standaardwaarde voor **InstanceCount** is **2**, met een maximale waarde is 10. De standaardwaarde voor **GatewaySize** is **gemiddeld**. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

   10. Voer de volgende opdracht in:

   ```powershell
   $policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
   ```

   Deze stap definieert de SSL-beleid gebruiken in de toepassingsgateway. Zie voor meer informatie [SSL configureren voor versies en coderingssuites in toepassingsgateway](application-gateway-configure-ssl-policy-powershell.md).

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Een toepassingsgateway maken met behulp van New-AzureApplicationGateway

Voer de volgende opdracht in:

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Dit voorbeeld maakt een toepassingsgateway met alle configuratie-items uit de bovenstaande stappen. In het voorbeeld wordt de toepassingsgateway wordt aangeroepen **appgwtest**.

## <a name="get-the-application-gateway-dns-name"></a>Ophalen van de DNS-naam van de toepassing-gateway

Nadat de gateway is gemaakt, wordt de volgende stap is het front-end voor de communicatie te configureren. Application Gateway is een dynamisch toegewezen DNS-naam vereist wanneer u een openbaar IP-adres, dit is geen beschrijvende. U kunt een CNAME-record gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway zodat eindgebruikers kunnen de toepassingsgateway bereikt. Zie voor meer informatie [een aangepaste domeinnaam configureren in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Als u de DNS-naam van de toepassing-gateway, ophalen van de gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de **PublicIPAddress** element gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen aan deze DNS-naam wijst. We niet raden het gebruik van A-records, omdat het VIP kunt wijzigen op opnieuw starten van de toepassingsgateway.


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

Als u configureren van een toepassingsgateway wilt voor gebruik met een interne load balancer, Zie [een toepassingsgateway maken met een interne load balancer](application-gateway-ilb.md).

Voor meer informatie over taakverdeling opties in het algemeen, Zie:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

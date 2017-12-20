---
title: Een toepassingsgateway maken, openen of verwijderen | Microsoft Docs
description: Op deze pagina vindt u instructies voor het maken, configureren, openen en verwijderen van een Azure-toepassingsgateway
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 7fb54e96d20d34f453b7b016094b84504348335b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a>Een toepassingsgateway maken, openen of verwijderen met PowerShell 

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sjabloon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Application Gateway bevat veel ADC-functies (Application Delivery Controller), waaronder HTTP-taakverdeling, op cookies gebaseerde sessieaffiniteit, SSL-offload (Secure Sockets Layer), aangepaste statustests en ondersteuning voor meerdere locaties. Een volledige lijst met ondersteunde functies vindt u in [Application Gateway Overview](application-gateway-introduction.md) (Overzicht van Application Gateway)

In dit artikel vindt u meer informatie over de stappen voor het maken, configureren, openen en verwijderen van een toepassingsgateway.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets via het webplatforminstallatieprogramma. U kunt de nieuwste versie downloaden en installeren via het gedeelte **Windows PowerShell** op de pagina [Downloads](https://azure.microsoft.com/downloads/).
2. Als u een bestaand virtueel netwerk hebt, selecteert u een bestaand leeg subnet of maakt u een nieuw subnet in uw bestaande virtuele netwerk, uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway niet implementeren op een ander virtueel netwerk dan de resources die u wilt implementeren achter de toepassingsgateway, tenzij VNET-peering wordt gebruikt. Zie [VNET-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie
3. Controleer of u een werkend virtueel netwerk hebt met een geldig subnet. Zorg ervoor dat er geen virtuele machines en cloudimplementaties zijn die gebruikmaken van het subnet. De toepassingsgateway moet afzonderlijk in een subnet van een virtueel netwerk staan.
4. De servers die u voor gebruik van de toepassingsgateway configureert, moeten al bestaan in het virtuele netwerk of hier hun eindpunten hebben. Een andere optie is om er een openbaar IP- of VIP-adres aan toe te wijzen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is er vereist om een toepassingsgateway te maken?

Als u de opdracht `New-AzureApplicationGateway` gebruikt om de toepassingsgateway te maken, is er op dat moment nog geen configuratie ingesteld en wordt de zojuist gemaakte resource met XML of met een configuratieobject geconfigureerd.

De waarden zijn:

* **Back-endserverpool:** de lijst met IP-adressen van de back-endservers. De IP-adressen moeten ofwel deel uitmaken van het subnet van het virtuele netwerk, ofwel openbare IP-/VIP-adressen zijn.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert).
* **Regel:** de regel verbindt de listener met de back-endserverpool en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Ga als volgt te werk om een toepassingsgateway te maken:

1. Maak een toepassingsgatewayresource.
2. Maak een XML-configuratiebestand of een configuratieobject.
3. Voer de configuratie door voor de zojuist gemaakte toepassingsgatewayresource.

> [!NOTE]
> Als u voor uw toepassingsgateway een aangepaste test moet configureren, raadpleegt u [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md) (Met PowerShell een toepassingsgateway maken met aangepaste tests). Bekijk [Custom probes and health monitoring](application-gateway-probe-overview.md) (Aangepaste tests en statusbewaking) voor meer informatie.

![Voorbeeldscenario][scenario]

### <a name="create-an-application-gateway-resource"></a>Een toepassingsgatewayresource maken

Gebruik de cmdlet `New-AzureApplicationGateway` en vervang de waarden door uw eigen waarden om een gateway te maken. Er worden op dat moment nog geen kosten in rekening gebracht voor gebruik van de gateway. De kosten zijn pas vanaf een latere stap van toepassing, wanneer de gateway wordt geopend.

In het volgende voorbeeld wordt een toepassingsgateway gemaakt met een virtueel netwerk met de naam testvnet1 en een subnet met de naam subnet-1:

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

*Description*, *InstanceCount* en *GatewaySize* zijn optionele parameters.

Gebruik de cmdlet `Get-AzureApplicationGateway` om te controleren of de gateway is gemaakt.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> De standaardwaarde voor *InstanceCount* is 2 en de maximale waarde is 10. De standaardwaarde voor *GatewaySize* is Medium. U kunt kiezen tussen Small, Medium en Large.

*VirtualIPs* en *DnsName* zijn leeg, omdat de gateway nog niet is geopend. Deze parameters worden ingevuld zodra de gateway wordt geactiveerd.

## <a name="configure-the-application-gateway"></a>De toepassingsgateway configureren

U kunt de toepassingsgateway configureren met XML of een configuratieobject.

### <a name="configure-the-application-gateway-by-using-xml"></a>De toepassingsgateway configureren met XML

In het volgende voorbeeld gebruikt u een XML-bestand om alle instellingen voor de toepassingsgateway te configureren en deze door te voeren voor de toepassingsgatewayresource.  

#### <a name="step-1"></a>Stap 1

Kopieer de volgende tekst naar Kladblok.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Bewerk de waarden tussen de haakjes voor de configuratie-items. Sla het bestand op met de bestandsextensie .xml.

> [!IMPORTANT]
> Het protocolitem Http of Https is hoofdlettergevoelig.

In het volgende voorbeeld ziet u hoe u de toepassingsgateway instelt met een configuratiebestand. De voorbeeldbelasting verdeelt het HTTP-verkeer op openbare poort 80 en stuurt netwerkverkeer naar back-endpoort 80 tussen twee IP-adressen.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

#### <a name="step-2"></a>Stap 2

Vervolgens stelt u de toepassingsgateway in. Gebruik de cmdlet `Set-AzureApplicationGatewayConfig` met een XML-configuratiebestand.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>De toepassingsgateway configureren met een configuratieobject

In het volgende voorbeeld ziet u hoe u de toepassingsgateway configureert met configuratieobjecten. Alle configuratie-items moeten afzonderlijk worden geconfigureerd en vervolgens worden toegevoegd aan een configuratieobject voor de toepassingsgateway. Wanneer u het configuratieobject hebt gemaakt, gebruikt u de opdracht `Set-AzureApplicationGateway` om de configuratie door te voeren voor de eerder gemaakte toepassingsgatewayresource.

> [!NOTE]
> Voordat u aan elk configuratieobject een waarde toewijst, moet u opgeven welk soort object PowerShell voor opslag gebruikt. In de eerste regel voor het maken van het afzonderlijke item wordt gedefinieerd welke `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` er worden gebruikt.

#### <a name="step-1"></a>Stap 1

Maak alle afzonderlijke configuratie-items.

Maak het front-end-IP-adres zoals in het volgende voorbeeld wordt weergegeven.

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

Maak de front-endpoort zoals in het volgende voorbeeld wordt weergegeven.

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

Maak de back-endserverpool.

Geef op welke IP-adressen er aan de back-endservergroep moeten worden toegevoegd, zoals in het volgende voorbeeld wordt weergegeven.

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

Gebruik het object $server om de waarden toe te voegen aan het back-endpoolobject ($pool).

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

Maak de back-endserverpoolinstelling.

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

Maak de listener.

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

Maak de regel.

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a>Stap 2

Wijs alle afzonderlijke configuratie-items toe aan een configuratieobject voor de toepassingsgateway ($appgwconfig).

Voeg het front-end-IP-adres toe aan de configuratie.

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

Voeg de front-endpoort toe aan de configuratie.

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
Voeg de back-endserverpool toe aan de configuratie.

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

Voeg de back-endpoolinstelling toe aan de configuratie.

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

Voeg de listener toe aan de configuratie.

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

Voeg de regel toe aan de configuratie.

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>Stap 3
Voer met behulp van `Set-AzureApplicationGatewayConfig` het configuratieobject door in de toepassingsgatewayresource.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-the-gateway"></a>De gateway openen

Nadat de gateway is geconfigureerd, gebruikt u de cmdlet `Start-AzureApplicationGateway` om de gateway te activeren. Voor een toepassingsgateway worden pas kosten doorberekend wanneer de gateway is geactiveerd.

> [!NOTE]
> Het kan 15 tot 20 minuten duren voordat de cmdlet `Start-AzureApplicationGateway` klaar is.

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>De gatewaystatus controleren

Gebruik de cmdlet `Get-AzureApplicationGateway` om de status van de gateway te controleren. Als `Start-AzureApplicationGateway` in de vorige stap zonder fouten is voltooid, moet bij *State* Running staan en moeten *Vip* en *DnsName* geldige waarden bevatten.

In het volgende voorbeeld wordt een toepassingsgateway weergegeven die actief is en verkeer kan verwerken dat is bestemd voor `http://<generated-dns-name>.cloudapp.net`.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-the-application-gateway"></a>De toepassingsgateway verwijderen

De toepassingsgateway verwijderen:

1. Gebruik de cmdlet `Stop-AzureApplicationGateway` om de gateway te stoppen.
2. Gebruik de cmdlet `Remove-AzureApplicationGateway` om de gateway te verwijderen.
3. Gebruik de cmdlet `Get-AzureApplicationGateway` om te controleren of de gateway is verwijderd.

In het volgende voorbeeld wordt de cmdlet `Stop-AzureApplicationGateway` op de eerste regel weergegeven, gevolgd door de uitvoer.

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Nadat de toepassingsgateway is gestopt, gebruikt u de cmdlet `Remove-AzureApplicationGateway` om de service te verwijderen.

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

Gebruik de cmdlet `Get-AzureApplicationGateway` als u wilt controleren of de service is verwijderd. Deze stap is niet vereist.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
.....
```

## <a name="next-steps"></a>Volgende stappen

Als u SSL-offload wilt configureren, raadpleegt u [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Een toepassingsgateway voor SSL-offload configureren).

Als u een toepassingsgateway wilt configureren voor gebruik met een interne load balancer, raadpleegt u [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Een toepassingsgateway met een interne load balancer (ILB) maken).

Als u meer informatie wilt over de algemene opties voor load balancing, raadpleegt u:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png

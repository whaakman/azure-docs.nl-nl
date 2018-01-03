---
title: Met behulp van Azure-toepassingsgateway met interne Load Balancer | Microsoft Docs
description: Deze pagina vindt u instructies voor het configureren van een Azure-toepassingsgateway met een eindpunt interne met gelijke taakverdeling
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 7ca9307e8a78f6dade4b231fa3a0d83a68af3f21
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Een Application Gateway maken met een Internal Load Balancer (ILB)

> [!div class="op_single_selector"]
> * [Azure Classic PowerShell](application-gateway-ilb.md)
> * [Azure Resource Manager PowerShell](application-gateway-ilb-arm.md)

Application Gateway kan worden geconfigureerd met een internetgericht virtueel IP-adres of met een interne eindpunt niet blootgesteld aan internet, ook wel bekend als interne Load Balancer (ILB)-eindpunt. De gateway configureren met een ILB is nuttig voor interne line-of-business-toepassingen niet blootgesteld aan internet. Het is ook nuttig voor servicecategorieën binnen een toepassing met meerdere lagen, die zich binnen een beveiligingsgrens niet blootgesteld aan internet, maar nog steeds vereist verdeling van round-robin, sessiepersistentie of SSL-beëindiging. In dit artikel worden de stappen beschreven voor het configureren van een toepassingsgateway met een ILB.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de nieuwste versie van de Azure PowerShell-cmdlets met behulp van het Webplatforminstallatieprogramma. U kunt downloaden en installeer de nieuwste versie van de **Windows PowerShell** sectie van de [downloadpagina](https://azure.microsoft.com/downloads/).
2. Controleer of u een werkend virtueel netwerk met een geldig subnetmasker.
3. Controleer of u back-endservers in het virtuele netwerk of een openbaar IP-/ VIP-adres toegewezen.

Als u wilt een toepassingsgateway maken, moet u de volgende stappen uitvoeren in de volgorde weergegeven. 

1. [Een toepassingsgateway maken](#create-a-new-application-gateway)
2. [De gateway configureren](#configure-the-gateway)
3. [Configuratie van de gateway instellen](#set-the-gateway-configuration)
4. [Start de gateway](#start-the-gateway)
5. [Controleer of de gateway](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken:

**De gateway te maken**, gebruiken de `New-AzureApplicationGateway` cmdlet, waarbij de waarden vervangt door uw eigen. Er worden op dit moment nog geen kosten in rekening gebracht voor gebruik van de gateway. De kosten zijn pas vanaf een latere stap van toepassing, wanneer de gateway wordt geopend.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

**Valideren** dat de gateway is gemaakt, kunt u de `Get-AzureApplicationGateway` cmdlet. 

In het voorbeeld *beschrijving*, *InstanceCount*, en *GatewaySize* zijn optionele parameters. De standaardwaarde voor *InstanceCount* is 2 en de maximale waarde is 10. De standaardwaarde voor *GatewaySize* is Medium. Kleine en grote andere beschikbare waarden zijn. *VIP* en *DnsName* leeg worden weergegeven omdat de gateway is nog niet gestart. Deze parameters worden ingevuld zodra de gateway wordt geactiveerd. 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-the-gateway"></a>De gateway configureren
Een toepassingsgateway configuratie bestaat uit meerdere waarden. De waarden kunnen worden gekoppeld samen te stellen de configuratie.

De waarden zijn:

* **Back-end-servergroep:** de lijst met IP-adressen van de back-endservers. De IP-adressen moeten ofwel deel uitmaken van het VNet subnet, ofwel moeten een openbare IP-/ VIP. 
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-Endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze zijn hoofdlettergevoelig), en de SSL-certificaatnaam (als u SSL-offloading configureert). 
* **Regel:** de regel verbindt de listener met de back-end-servergroep en definieert naar welke back-end-servergroep die het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt. Momenteel wordt alleen de regel *basic* ondersteund. De regel *basic* is een vorm van round-robinbelastingverdeling.

U kunt de configuratie maken door het maken van een configuratieobject of met behulp van een XML-configuratiebestand. Kan de configuratie met behulp van een XML-configuratiebestand, gebruik het voorbeeld hieronder.

Houd rekening met het volgende:

* De *FrontendIPConfigurations* element beschrijft de ILB details die relevant zijn voor het configureren van Application Gateway met een ILB. 
* De Frontend IP *Type* moet worden ingesteld op 'Privé'
* De *StaticIPAddress* moet worden ingesteld op het gewenste interne IP-adres waarop de gateway verkeer ontvangt. Houd er rekening mee dat de *StaticIPAddress* element is optioneel. Als dat niet is ingesteld, een beschikbare intern IP-adres van het geïmplementeerde subnet is gekozen. 
* De waarde van de *naam* opgegeven element in *FrontendIPConfiguration* moet worden gebruikt in de HTTPListener *FrontendIP* element om te verwijzen naar de FrontendIPConfiguration.
  
  **Configuratie-XML-voorbeeld**
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
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
            <FrontendIP>fip1</FrontendIP>
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


## <a name="set-the-gateway-configuration"></a>Configuratie van de gateway instellen
Vervolgens stelt u de toepassingsgateway. U kunt de `Set-AzureApplicationGatewayConfig` cmdlet met een configuratieobject of met een XML-configuratiebestand. 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-the-gateway"></a>De gateway openen

Nadat de gateway is geconfigureerd, gebruikt u de cmdlet `Start-AzureApplicationGateway` om de gateway te activeren. Voor een toepassingsgateway worden pas kosten doorberekend wanneer de gateway is geactiveerd. 

> [!NOTE]
> De `Start-AzureApplicationGateway` cmdlet mogelijk maximaal 15-20 minuten duren. 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-the-gateway-status"></a>De gatewaystatus controleren

Gebruik de `Get-AzureApplicationGateway` cmdlet om de status van gateway te controleren. Als `Start-AzureApplicationGateway` is voltooid in de vorige stap, de status moet *met*, en het Vip en DnsName moet geldige vermeldingen hebben. Dit voorbeeld ziet u de cmdlet op de eerste regel weergegeven, gevolgd door de uitvoer. In dit voorbeeld wordt de gateway wordt uitgevoerd en gereed is voor verkeer. 

> [!NOTE]
> De toepassingsgateway is geconfigureerd voor het accepteren van verkeer op de geconfigureerde ILB-eindpunt van 10.0.0.10 in dit voorbeeld.

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
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a>Volgende stappen
Als u meer informatie wilt over de algemene opties voor load balancing, raadpleegt u:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)


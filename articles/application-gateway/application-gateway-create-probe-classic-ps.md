---
title: Maken van een aangepaste test - Azure Application Gateway - PowerShell-klassiek | Microsoft Docs
description: Informatie over het maken van een aangepaste test voor Application Gateway met behulp van PowerShell in het klassieke implementatiemodel
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: 01c1768f60da98206f0dfd041745428256f545fc
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861876"
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Een aangepaste test maken voor Azure Application Gateway (klassiek) met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Classic PowerShell](application-gateway-create-probe-classic-ps.md)

In dit artikel, kunt u een aangepaste test toevoegen aan een bestaande toepassingsgateway met PowerShell. Aangepaste tests zijn handig voor toepassingen waarvoor een specifieke statuscontrolepagina of voor toepassingen die een geslaagd antwoord op de standaard web-App bieden.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](application-gateway-create-probe-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Ga als volgt te werk om een toepassingsgateway te maken:

1. Maak een toepassingsgatewayresource.
2. Maak een XML-configuratiebestand of een configuratieobject.
3. Voer de configuratie door voor de zojuist gemaakte toepassingsgatewayresource.

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a>Een toepassingsgatewayresource maken met een aangepaste test

Gebruik de cmdlet `New-AzureApplicationGateway` en vervang de waarden door uw eigen waarden om een gateway te maken. Er worden op dat moment nog geen kosten in rekening gebracht voor gebruik van de gateway. De kosten zijn pas vanaf een latere stap van toepassing, wanneer de gateway wordt geopend.

In het volgende voorbeeld wordt een toepassingsgateway gemaakt met een virtueel netwerk met de naam testvnet1 en een subnet met de naam subnet-1.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Gebruik de cmdlet `Get-AzureApplicationGateway` om te controleren of de gateway is gemaakt.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> De standaardwaarde voor *InstanceCount* is 2 en de maximale waarde is 10. De standaardwaarde voor *GatewaySize* is Medium. U kunt kiezen tussen Small, Medium en Large.
> 
> 

*VirtualIPs* en *DnsName* zijn leeg, omdat de gateway nog niet is geopend. Deze waarden worden ingevuld zodra de gateway in de status running doorbrengt is.

### <a name="configure-an-application-gateway-by-using-xml"></a>Een toepassingsgateway configureren met behulp van XML

In het volgende voorbeeld gebruikt u een XML-bestand om alle instellingen voor de toepassingsgateway te configureren en deze door te voeren voor de toepassingsgatewayresource.  

Kopieer de volgende tekst naar Kladblok.

```xml
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Bewerk de waarden tussen de haakjes voor de configuratie-items. Sla het bestand op met de bestandsextensie .xml.

Het volgende voorbeeld ziet hoe u een configuratiebestand gebruikt voor het instellen van de toepassingsgateway taakverdeling van HTTP-verkeer op openbare poort 80 en verzenden van netwerkverkeer naar back-endpoort 80 tussen twee IP-adressen met behulp van een aangepaste test.

> [!IMPORTANT]
> Het protocolitem Http of Https is hoofdlettergevoelig.

Een nieuwe configuratie-item \<Probe\> wordt toegevoegd aan het configureren van aangepaste tests.

De parameters voor de configuratie zijn:

|Parameter|Description|
|---|---|
|**Name** |De referentienaam voor aangepaste test. |
| **Protocol** | Protocol dat wordt gebruikt (mogelijke waarden zijn HTTP of HTTPS).|
| **Host** en **pad** | Volledige URL-pad dat wordt aangeroepen door de application gateway de status van het exemplaar te bepalen. Bijvoorbeeld, hebt u een website http:\//contoso.com/ en vervolgens de aangepaste test kan worden geconfigureerd voor "http:\//contoso.com/path/custompath.htm ' voor de test wordt gecontroleerd om een geslaagde HTTP-antwoord.|
| **Interval** | Hiermee configureert u de testcontroles interval in seconden.|
| **Time-out** | Definieert de time-out voor de test voor de controle van een HTTP-antwoord.|
| **UnhealthyThreshold** | Het aantal mislukte HTTP-antwoorden die nodig zijn voor het markeren van de back-end-instantie als *niet in orde*.|

Naam van de test wordt verwezen in de \<BackendHttpSettings\> configuratie om toe te wijzen die back-end-pool maakt gebruik van aangepaste test-instellingen.

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a>Een aangepaste test toevoegen aan een bestaande toepassingsgateway

Wijzigen van de huidige configuratie van een application gateway zijn drie stappen vereist: Ophalen van de huidige XML-configuratiebestand, wijzigen om een aangepaste test hebt en de toepassingsgateway configureren met de nieuwe XML-instellingen.

1. Ophalen van het XML-bestand met behulp van `Get-AzureApplicationGatewayConfig`. Deze cmdlet wordt de configuratie-XML worden gewijzigd om toe te voegen testinstelling geëxporteerd.

   ```powershell
   Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
   ```

1. Open het XML-bestand in een teksteditor. Voeg een `<probe>` sectie na `<frontendport>`.

   ```xml
   <Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
   </Probes>
   ```

   Toevoegen in de sectie backendHttpSettings van het XML-bestand, de naam van de test zoals wordt weergegeven in het volgende voorbeeld:

   ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
   ```

   Sla het XML-bestand.

1. De configuratie van de application gateway met het nieuwe XML-bestand bijwerken met behulp van `Set-AzureApplicationGatewayConfig`. Deze cmdlet werkt uw application gateway met de nieuwe configuratie.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a>Volgende stappen

Als u configureren van Secure Sockets Layer (SSL)-offload wilt, Zie [een toepassingsgateway voor SSL-offload configureren](application-gateway-ssl.md).

Als u een toepassingsgateway wilt configureren voor gebruik met een interne load balancer, raadpleegt u [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Een toepassingsgateway met een interne load balancer (ILB) maken).


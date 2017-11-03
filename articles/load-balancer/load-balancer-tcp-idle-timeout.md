---
title: Time-out bij inactiviteit voor Load Balancer TCP configureren | Microsoft Docs
description: Time-out bij inactiviteit voor Load Balancer TCP configureren
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 4625c6a8-5725-47ce-81db-4fa3bd055891
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: f19ac77f7c7f7d4ab8909d628f9dcce08c07c928
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>TCP-time-out voor inactiviteit instellingen configureren voor Azure Load Balancer

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

In de standaardconfiguratie heeft Azure Load Balancer een instelling voor time-out voor inactiviteit van vier minuten. Als een periode van inactiviteit langer dan de time-outwaarde is, is er geen garantie dat de TCP- of HTTP-sessie wordt onderhouden tussen de client en de cloudservice.

Wanneer de verbinding is gesloten, wordt de clienttoepassing het volgende foutbericht weergegeven: ' de onderliggende verbinding is gesloten: een verbinding waarvan wordt verwacht dat worden behouden door de server is gesloten. "

Vaak is het gebruik van een TCP-keepalive. Hierdoor blijft de verbinding actief voor een langere periode. Zie voor meer informatie deze [voorbeelden .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Met keep-alive is ingeschakeld, pakketten worden verzonden tijdens perioden van inactiviteit op de verbinding. Deze keepalive-pakketten Zorg dat de waarde voor time-out voor inactiviteit nooit bereikt is en gedurende een lange periode wordt bijgehouden.

Deze instelling is geschikt voor binnenkomende verbindingen alleen. Om te voorkomen dat de verbinding verloren gaan, moet u de TCP-keepalive configureren met een interval van minder dan de instelling voor time-out voor inactiviteit of verhoog de waarde voor time-out voor inactiviteit. Toegevoegd ter ondersteuning van dergelijke scenario's, ondersteuning voor een configureerbare time-out voor inactiviteit. U kunt het nu instellen voor een duur van 4 tot en met 30 minuten.

TCP-keepalive geschikt is voor scenario's waarin batterij niet een beperking. Dit wordt niet aanbevolen voor mobiele toepassingen. Met een TCP-keepalive in een mobiele toepassing kan de accu raakt door apparaat sneller.

![TCP-time](./media/load-balancer-tcp-idle-timeout/image1.png)

De volgende secties wordt beschreven hoe wijzig de instellingen van de time-out voor inactiviteit in virtuele machines en cloudservices.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>De TCP-time configureren voor uw openbare IP op exemplaarniveau tot 15 minuten

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

`IdleTimeoutInMinutes` is optioneel. Als deze niet is ingesteld, wordt de standaardtime-out vier minuten. Het bereik van acceptabele time-out is 4 tot en met 30 minuten.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>De time-out voor inactiviteit ingesteld bij het maken van een Azure-eindpunt op een virtuele machine

Als u wilt wijzigen van de instelling voor time-out voor een eindpunt, gebruikt u het volgende:

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

Gebruik de volgende opdracht voor het ophalen van de time-out voor inactiviteit-configuratie:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>De TCP-out instellen op een eindpuntset met gelijke taakverdeling

Als eindpunten deel van een eindpuntset met gelijke taakverdeling uitmaken, kan de TCP-out moet worden ingesteld op de eindpuntset met gelijke taakverdeling. Bijvoorbeeld:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Time-outinstellingen voor cloudservices wijzigen

U kunt de Azure SDK gebruiken om bij te werken van uw cloudservice. U eindpuntinstellingen voor cloudservices in het csdef-bestand. Bijwerken van de TCP-out voor de implementatie van een cloudservice, moet een upgrade van een implementatie. Een uitzondering hierop is als de time-out voor de TCP-alleen voor een openbaar IP-adres is opgegeven. Openbare IP-instellingen zijn in het .cscfg-bestand en u die kunt bijwerken via de implementatie-update en de upgrade.

De wijzigingen csdef voor eindpuntinstellingen zijn:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

De wijzigingen cscfg-bestand voor de instelling voor de time-out voor openbare IP-adressen zijn:

```xml
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
    </InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="rest-api-example"></a>REST-API-voorbeeld

U kunt de time-out voor inactiviteit TCP configureren met behulp van de servicebeheer-API. Zorg ervoor dat de `x-ms-version` header is ingesteld op versie `2014-06-01` of hoger. Werk de configuratie van de opgegeven taakverdeling invoer-eindpunten op alle virtuele machines in een implementatie.

### <a name="request"></a>Aanvraag

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Antwoord

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
        <Path>path-of-probe</Path>
        <Port>port-assigned-to-probe</Port>
        <Protocol>probe-protocol</Protocol>
        <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
        <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
        <Rules>
        <Rule>
            <Order>priority-of-the-rule</Order>
            <Action>permit-rule</Action>
            <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
            <Description>description-of-the-rule</Description>
        </Rule>
        </Rules>
    </EndpointACL>
    </InputEndpoint>
</LoadBalancedEndpointList>
```

## <a name="next-steps"></a>Volgende stappen

[Interne load balancer-overzicht](load-balancer-internal-overview.md)

[Configureren van een Internet gerichte load balancer aan de slag](load-balancer-get-started-internet-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

---
title: Time-out voor inactiviteit van TCP voor Load Balancer in Azure configureren
titlesuffix: Azure Load Balancer
description: Time-out voor inactiviteit van TCP voor Load Balancer configureren
services: load-balancer
documentationcenter: na
author: kumudd
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0c57eec4d739da13d98099a6b2f01fbf0ad0051c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57857535"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>TCP-outinstellingen voor inactiviteit voor Azure Load Balancer configureren

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

In de standaardconfiguratie heeft Azure Load Balancer een instelling voor time-out voor inactiviteit van vier minuten. Als een periode van inactiviteit langer dan de time-outwaarde is, is er geen garantie dat de TCP- of HTTP-sessie wordt onderhouden tussen de client en de service in de cloud.

Wanneer de verbinding is gesloten, wordt de clienttoepassing de volgende strekking weergegeven: "De onderliggende verbinding is gesloten: Een verbinding die werd verwacht leven wordt gehouden is gesloten door de server."

Een algemene praktijk is het gebruik van een TCP-keepalive. Hierdoor blijft de verbinding actief voor een langere periode. Zie voor meer informatie deze [.NET-voorbeelden](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Met keepalive is ingeschakeld, pakketten worden verzonden tijdens perioden van inactiviteit van de verbinding. Deze keepalive pakketten ervoor te zorgen dat de waarde voor time-out voor inactiviteit nooit bereikt is en wordt bijgehouden voor een lange periode.

Deze instelling werkt alleen binnenkomende verbindingen. Om te voorkomen dat de verbinding verloren gaat, moet u de TCP-keepalive configureren met een interval van minder dan de instelling voor time-out voor inactiviteit of verhoog de waarde van de time-out voor inactiviteit. Er is ondersteuning voor een configureerbare time-out voor inactiviteit toegevoegd ter ondersteuning van dergelijke scenario's. U kunt dit nu instellen voor een duur van 4 tot en met 30 minuten.

TCP keepalive werkt goed voor scenario's waarin acculevensduur hebben niet een beperking. Het wordt niet aanbevolen voor mobiele toepassingen. Met behulp van een TCP keepalive in een mobiele toepassing kan de accu raakt door apparaat sneller.

![Time-out voor TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

De volgende secties wordt beschreven hoe u instellingen van de time-out voor inactiviteit in virtuele machines en cloudservices.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>De time-out voor TCP configureren voor uw openbare IP op exemplaarniveau tot 15 minuten

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

`IdleTimeoutInMinutes` is optioneel. Als deze niet is ingesteld, is de standaardtime-out 4 minuten. Het bereik van acceptabele time-out is 4 tot en met 30 minuten.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Stel de time-out voor inactiviteit bij het maken van een Azure-eindpunt op een virtuele machine

Als u wilt wijzigen van de time-out voor een eindpunt, gebruikt u het volgende:

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

Als u wilt ophalen van de configuratie van de time-out voor inactiviteit, gebruik de volgende opdracht:

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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>De time-out van de TCP-instelt op een eindpuntset met load balancing

Als eindpunten onderdeel van een eindpuntset met load balancing-zijn, moet de time-out voor TCP worden ingesteld op de eindpuntset met taakverdeling. Bijvoorbeeld:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Wijzigen van de time-outinstellingen voor cloudservices

De Azure SDK kunt u uw cloudservice bijwerken. U maken instellingen van eindpunten voor cloudservices in het csdef-bestand. Bijwerken van de TCP-time-out voor de implementatie van een cloudservice, moet een upgrade van een implementatie. Een uitzondering hierop is als de time-out voor TCP-alleen voor een openbaar IP-adres is opgegeven. Openbare IP-instellingen zijn in het .cscfg-bestand en kunt u deze bijwerken door middel van implementatie-update en upgrade.

De wijzigingen .csdef voor instellingen van eindpunten zijn:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

De cscfg-bestand is gewijzigd voor de time-outinstelling op openbare IP-adressen zijn:

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

## <a name="rest-api-example"></a>Voorbeeld van de REST-API

U kunt de TCP-inactiviteit configureren met behulp van de servicebeheer-API. Zorg ervoor dat de `x-ms-version` header is ingesteld op versie `2014-06-01` of hoger. De configuratie van de opgegeven invoer taakverdelingseindpunten op alle virtuele machines in een implementatie bijgewerkt.

### <a name="request"></a>Aanvraag

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Antwoord

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
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

[Overzicht van interne load balancer](load-balancer-internal-overview.md)

[Aan de slag een internetgerichte load balancer configureren](load-balancer-get-started-internet-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

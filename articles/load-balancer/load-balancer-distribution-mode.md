---
title: Azure Load Balancer-distributie modus configureren | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de distributie-modus voor Azure Load Balancer voor ondersteuning voor affiniteit tussen bron-IP.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: d04a469c04553b7d6a14df7054ad5ef795baa500
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>De modus distributiepunt configureren voor Azure Load Balancer

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

## <a name="hash-based-distribution-mode"></a>Distributiepunten in de hash-modus

De standaardmodus voor distributie voor Azure Load Balancer is een 5-tuple hash. De tuple bestaat uit de bron-IP, bronpoort, doel-IP, doelpoort en protocoltype. De hash wordt gebruikt voor het verkeer worden toegewezen aan de beschikbare servers en de algoritme biedt gebruikerspad alleen binnen een transportsessie. Pakketten die zich in dezelfde sessie worden omgeleid naar de hetzelfde exemplaar van datacenter IP (DIP) achter het eindpunt met gelijke taakverdeling. Wanneer de client wordt gestart van een nieuwe sessie van dezelfde bron-IP, wordt de bronpoort wijzigt en zorgt ervoor dat het verkeer naar een ander DIP-eindpunt.

![5-tuple hash-distributiepunt modus](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

## <a name="source-ip-affinity-mode"></a>Affiniteitsmodus voor IP-bron

Load Balancer kan ook worden geconfigureerd met behulp van de bron op het IP-distributie affiniteitsmodus. Deze distributie-modus wordt ook wel sessie affiniteit of IP-clientaffiniteit. De modus gebruikt een 2-tuple (IP-adres bron en doel-IP) of 3-tuple hash (bron-IP, doel-IP en protocol type)-verkeer worden toegewezen aan de beschikbare servers. Met behulp van de affiniteit tussen bron-IP-gaat verbindingen die geïnitieerd worden vanuit dezelfde clientcomputer u naar hetzelfde DIP-eindpunt.

De volgende afbeelding ziet u een configuratie met 2-tuple. U ziet hoe de 2-tuple wordt uitgevoerd via de load balancer voor virtuele machine, 1 (VM1). VM1 vervolgens back-up door VM2 en VM3.

![2-tuple sessie affiniteitsmodus distributie](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Affiniteitsmodus voor IP-bron is opgelost incompatibiliteit tussen Azure Load Balancer en extern bureaublad-Gateway (RD-Gateway). U kunt een farm RD-Gateway in een enkel cloudservice opbouwen met behulp van deze modus.

Een ander gebruik case scenario is het uploaden van media. Het uploaden van gegevens gebeurt via UDP, maar het besturingselement vlak via TCP wordt bereikt:

* Een client een TCP-sessie in het openbare adres met gelijke taakverdeling initieert en wordt omgeleid naar een specifieke DIP. Het kanaal voor het bewaken van de status van de verbinding actief blijft.
* Een nieuwe sessie UDP van dezelfde clientcomputer wordt gestart met het dezelfde taakverdeling openbaar eindpunt. De verbinding is omgeleid naar hetzelfde DIP eindpunt als de vorige TCP-verbinding. Het uploaden van de media kan worden uitgevoerd op maximale doorvoer en tegelijkertijd een besturingskanaal via TCP.

> [!NOTE]
> Wanneer een set met gelijke taakverdeling door te verwijderen of toevoegen van een virtuele machine wordt gewijzigd, wordt de distributie van clientaanvragen herberekend. U kunt geen afhankelijk van nieuwe verbindingen van bestaande clients om te eindigen op dezelfde server. Bovendien met behulp van de bron-IP affiniteitsmodus voor verdeling kan leiden tot een ongelijke distributie van verkeer. Clients die worden uitgevoerd achter proxy's kunnen worden gezien als een unieke client-toepassing.

## <a name="configure-source-ip-affinity-settings"></a>Bron-IP-affiniteit-instellingen configureren

Voor virtuele machines, moet u Azure PowerShell gebruiken om de time-instellingen wijzigen. Een Azure-eindpunt toevoegen aan een virtuele machine en configureren van de load balancer-distributie modus:

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

Stel de waarde van de `LoadBalancerDistribution` element worden opgegeven voor de gewenste hoeveelheid taakverdeling. Geef sourceIP voor taakverdeling 2-tuple (IP-adres bron en doel-IP). Geef sourceIPProtocol voor taakverdeling 3-tuple (bron-IP, doel-IP en protocol type). Geef geen zijn voor het standaardgedrag van 5-tuple taakverdeling.

Een load balancer distributie modus eindpuntconfiguratie ophalen met behulp van deze instellingen:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Wanneer de `LoadBalancerDistribution` element is niet aanwezig is, de standaard 5-tuple algoritme maakt gebruik van Azure Load Balancer.

### <a name="configure-distribution-mode-on-load-balanced-endpoint-set"></a>Distributie-modus op de eindpuntset met gelijke taakverdeling configureren

Wanneer eindpunten deel van een eindpuntset met gelijke taakverdeling uitmaken, moet de distributie-modus worden geconfigureerd op de eindpuntset met gelijke taakverdeling:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="configure-distribution-mode-for-cloud-services-endpoints"></a>Distributie modus configureren voor Cloud Services-eindpunten

Gebruik de Azure SDK voor .NET 2.5 bijwerken van uw cloudservice. De eindpuntinstellingen voor Cloud-Services zijn aangebracht in het csdef-bestand. De upgrade van een implementatie is voor het bijwerken van de load balancer distributie-modus voor een Cloud Services-implementatie vereist.

Hier volgt een voorbeeld van wijzigingen voor eindpuntinstellingen csdef:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
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

## <a name="api-example"></a>API-voorbeeld

Het volgende voorbeeld laat zien hoe opnieuw configureren van de load balancer distributie-modus voor een opgegeven set taakverdeling in een implementatie. 

### <a name="change-distribution-mode-for-deployed-load-balanced-set"></a>Distributie-modus wijzigen voor geïmplementeerde set met gelijke taakverdeling

Met het klassieke implementatiemodel Azure kunt u een bestaande implementatieconfiguratie. Voeg de `x-ms-version` header en stel de waarde voor versie 2014-09-01 of hoger.

#### <a name="request"></a>Aanvraag

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Zoals eerder beschreven, stel de `LoadBalancerDistribution` element op de sourceIP voor affiniteit tussen de 2-tuple, sourceIPProtocol voor affiniteit tussen de 3-tuple of none voor geen affiniteit (5-tuple affiniteit).

#### <a name="response"></a>Antwoord

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure interne Load Balancer](load-balancer-internal-overview.md)
* [Aan de slag met een internetgerichte load balancer configureren](load-balancer-get-started-internet-arm-ps.md)
* [TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)

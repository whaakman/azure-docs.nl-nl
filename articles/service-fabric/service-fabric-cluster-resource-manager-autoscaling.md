---
title: Azure Service Fabric automatisch vergroten/verkleinen Services en Containers | Microsoft Docs
description: Azure Service Fabric kunt u automatisch schalen van beleidsregels voor services en containers instellen.
services: service-fabric
documentationcenter: .net
author: radicmilos
manager: ''
editor: nipuzovi
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2018
ms.author: miradic
ms.openlocfilehash: cd19c0e51ca1ac7863058d7c3944400719508f9b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="introduction-to-auto-scaling"></a>Inleiding tot automatische schaling
Automatische schaling is een aanvullende functionaliteit van Service Fabric dynamisch schalen uw services op basis van de belasting die services wilt melden, of op basis van hun gebruik van bronnen. Automatische schaling biedt uitstekende elasticiteit en maakt het inrichten van extra exemplaren of partities van uw service op aanvraag. De volledige automatische schaling proces is automatisch en transparant en nadat het instellen van uw beleid voor een service is niet nodig voor handmatige vergroten/verkleinen bewerkingen op het serviceniveau van de. Automatische schaling kan worden ingeschakeld tijdens de aanmaak van de service of op elk gewenst moment door het bijwerken van de service.

Een veelvoorkomend scenario waarbij automatisch schalen nuttig is is wanneer de belasting op een bepaalde service gedurende een bepaalde periode varieert. Bijvoorbeeld, een service, zoals een gateway kunt schalen op basis van de hoeveelheid resources die nodig zijn om inkomende aanvragen te verwerken. Laten we Bekijk een voorbeeld van wat deze regels vergroten/verkleinen kunnen er als volgt uitzien:
* Als alle exemplaren van mijn gateway meer dan twee kernen gemiddeld, moet u vervolgens de gateway-service uit schalen door toevoeging van één meer exemplaar. Hiervoor elk uur, maar nooit meer dan zeven exemplaren hebben in totaal.
* Als alle exemplaren van mijn gateway met minder dan 0,5 kernen gemiddeld, moet u vervolgens de service in schalen door het verwijderen van één exemplaar. Hiervoor elk uur, maar nooit minder dan drie exemplaren hebben in totaal.

Automatische schaling wordt ondersteund voor zowel containers en reguliere Service Fabric-services. De rest van dit artikel wordt het beleid voor vergroten/verkleinen manieren inschakelen of uitschakelen van automatische schaling, beschreven en worden voorbeelden gegeven over het gebruik van deze functie.

## <a name="describing-auto-scaling"></a>Met een beschrijving van automatische schaling
Automatische schaling beleidsregels kan worden gedefinieerd voor elke service in een Service Fabric-cluster. Elk vergroten/verkleinen beleid bestaat uit twee delen:
* **Schalen trigger** wordt beschreven wanneer schalen van de service wordt uitgevoerd. Voorwaarden die zijn gedefinieerd in de trigger worden regelmatig gecontroleerd om te bepalen of een service moet worden aangepast.

* **Schalen mechanisme** wordt beschreven hoe schalen moet worden uitgevoerd wanneer deze wordt geactiveerd. Mechanisme wordt alleen toegepast wanneer de voorwaarden van de trigger wordt voldaan.

Alle triggers die momenteel worden ondersteund werkt met [logische load metrische gegevens](service-fabric-cluster-resource-manager-metrics.md), of met fysieke metrische gegevens zoals CPU of geheugen gebruik. In beide gevallen Service Fabric de gerapporteerde belasting voor de metriek bewaakt en evalueren de trigger regelmatig om te bepalen of schalen nodig is.

Er zijn twee methoden die momenteel worden ondersteund voor automatisch schalen. De eerste die is bedoeld voor stateless services of voor containers waarop automatische schaling wordt uitgevoerd door toe te voegen of te verwijderen [exemplaren](service-fabric-concepts-replica-lifecycle.md). Stateful en staatloze services, automatische schaling kan ook worden uitgevoerd door toe te voegen of te verwijderen met de naam [partities](service-fabric-concepts-partitioning.md) van de service.

> [!NOTE]
> Er is momenteel ondersteuning voor slechts één vergroten/verkleinen beleid per service.

## <a name="average-partition-load-trigger-with-instance-based-scaling"></a>Gemiddelde partitie load trigger met het exemplaar op basis van geschaald
Het eerste type trigger is gebaseerd op de belasting van de exemplaren in een partitie staatloze service. Metrische belastingen worden eerst vloeiend worden gemaakt om de belasting voor elk exemplaar van een partitie ophalen en vervolgens deze waarden zijn gemiddeld over alle exemplaren van de partitie. Er zijn drie factoren die bepalen wanneer de service wordt aangepast:

* _Drempelwaarde voor het laden van lagere_ is een waarde die bepaalt wanneer de service worden **geschaald in**. Als de gemiddelde belasting van alle exemplaren van de partities die lager is dan deze waarde is, wordt de service worden geschaald.
* _Drempelwaarde voor het bovenste laden_ is een waarde die bepaalt wanneer de service worden **uitgebreid**. Als de gemiddelde belasting van alle exemplaren van de partitie lager is dan deze waarde is, zal klikt u vervolgens de service worden uitgebreid.
* _Interval voor vergroten/verkleinen_ bepaalt hoe vaak de trigger wordt gecontroleerd. Zodra de trigger is ingeschakeld, indien nodig schalen is het mechanisme worden toegepast. Als schalen niet vereist is, klikt u vervolgens geen actie ondernomen. In beide gevallen wordt trigger niet gecontroleerd opnieuw voordat u opnieuw vergroten/verkleinen interval is verstreken.

Deze trigger kan alleen worden gebruikt met stateless services (stateless containers of Service Fabric-services). Voor het geval wanneer een service meerdere partities heeft, de trigger wordt geëvalueerd voor elke partitie afzonderlijk en elke partitie het opgegeven mechanisme onafhankelijk toegepast heeft. In dit geval wordt is het dus mogelijk dat sommige van de partities van de service zal worden uitgebreid, sommige wordt geschaald in en sommige won't worden geschaald helemaal op hetzelfde moment op basis van hun laden.

Het enige mechanisme dat kan worden gebruikt met deze trigger is PartitionInstanceCountScaleMechanism. Er zijn drie factoren die bepalen hoe dit mechanisme wordt toegepast:
* _Schalen verhoging_ bepaalt hoeveel exemplaren worden toegevoegd of verwijderd wanneer mechanisme wordt geactiveerd.
* _Maximum aantal exemplaren_ definieert de bovengrens voor het schalen. Als deze limiet wordt bereikt door het aantal exemplaren van de partitie, wordt klikt u vervolgens de service niet worden uitgebreid, ongeacht de belasting. Het is mogelijk om deze limiet weglaten door de waarde van-1 en in dat geval de service wordt aangepast buiten zo veel mogelijk (de limiet is het aantal knooppunten die beschikbaar in het cluster zijn).
* _Minimuminstantieaantal_ definieert de ondergrens voor het schalen. Als deze limiet wordt bereikt door het aantal exemplaren van de partitie, zal klikt u vervolgens service niet worden geschaald ongeacht de belasting.

## <a name="setting-auto-scaling-policy"></a>Automatische schaling beleid instellen

### <a name="using-application-manifest"></a>Met behulp van het toepassingsmanifest
``` xml
<LoadMetrics>
<LoadMetric Name="MetricB" Weight="High"/>
</LoadMetrics>
<ServiceScalingPolicies>
<ScalingPolicy>
    <AveragePartitionLoadScalingTrigger MetricName="MetricB" LowerLoadThreshold="1" UpperLoadThreshold="2" ScaleIntervalInSeconds="100"/>
    <InstanceCountScalingMechanism MinInstanceCount="3" MaxInstanceCount="4" ScaleIncrement="1"/>
</ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>Met C#-API 's
```csharp
FabricClient fabricClient = new FabricClient();
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//set up the rest of the ServiceDescription
AveragePartitionLoadScalingTrigger trigger = new AveragePartitionLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new PartitionInstanceCountScaleMechanism();
mechanism.MaxInstanceCount = 3;
mechanism.MinInstanceCount = 1;
mechanism.ScaleIncrement = 1;
trigger.MetricName = "servicefabric:/_CpuCores";
trigger.ScaleInterval = TimeSpan.FromMinutes(20);
trigger.LowerLoadThreshold = 1.0;
trigger.UpperLoadThreshold = 2.0;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceDescription.ScalingPolicies.Add(policy);
//as we are using scaling on a resource this must be exclusive service
//also resource monitor service needs to be enabled
serviceDescription.ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```
### <a name="using-powershell"></a>Met behulp van Powershell
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.PartitionInstanceCountScaleMechanism
$mechanism.MinInstanceCount = 1
$mechanism.MaxInstanceCount = 6
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AveragePartitionLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_CpuCores"
$trigger.LowerLoadThreshold = 0.3
$trigger.UpperLoadThreshold = 0.8
$trigger.ScaleInterval = New-TimeSpan -Minutes 10
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
Update-ServiceFabricService -Stateless -ServiceName "fabric:/AppName/ServiceName" -ScalingPolicies $scalingpolicies
```

## <a name="average-service-load-trigger-with-partition-based-scaling"></a>Gemiddelde service load trigger met schalen op basis van partitie
De tweede trigger is gebaseerd op de belasting van alle partities van een service. Metrische belastingen worden eerst vloeiend worden gemaakt om op te halen van de belasting voor elke replica of het exemplaar van een partitie. Voor stateful services de belasting van de partitie wordt beschouwd als de belasting van de primaire replica, terwijl de belasting van de partitie voor stateless services de gemiddelde belasting van alle exemplaren van de partitie. Deze waarden zijn met een gemiddelde van alle partities van de service en deze waarde wordt gebruikt voor het activeren van het automatisch schalen. Hetzelfde als in eerdere mechanisme, er zijn drie factoren die bepalen wanneer de service wordt aangepast:

* _Drempelwaarde voor het laden van lagere_ is een waarde die bepaalt wanneer de service worden **geschaald in**. Als de gemiddelde belasting van alle partities van de service lager is dan deze waarde is, wordt de service worden geschaald.
* _Drempelwaarde voor het bovenste laden_ is een waarde die bepaalt wanneer de service worden **uitgebreid**. Als de gemiddelde belasting van alle partities van de service lager is dan deze waarde is, zal klikt u vervolgens de service worden uitgebreid.
* _Interval voor vergroten/verkleinen_ bepaalt hoe vaak de trigger wordt gecontroleerd. Zodra de trigger is ingeschakeld, indien nodig schalen is het mechanisme worden toegepast. Als schalen niet vereist is, klikt u vervolgens geen actie ondernomen. In beide gevallen wordt trigger niet gecontroleerd opnieuw voordat u opnieuw vergroten/verkleinen interval is verstreken.

Deze trigger kan worden gebruikt met stateful en staatloze services. Het enige mechanisme dat kan worden gebruikt met deze trigger is AddRemoveIncrementalNamedParitionScalingMechanism. Wanneer de service is uitgebreid en vervolgens een nieuwe partitie wordt toegevoegd, en als service in een van de bestaande partities wordt geschaald is verwijderd. Er gelden beperkingen die wordt gecontroleerd wanneer service wordt gemaakt of bijgewerkt en service maken/bijwerken mislukt als niet aan deze voorwaarden wordt voldaan:
* Benoemde partitieschema moet worden gebruikt voor de service.
* Partitienamen moet opeenvolgende gehele getallen, zoals '0', '1'...
* Naam van de eerste partitie moet '0' zijn.

Als een service in eerste instantie is gemaakt met drie partities, is de enige geldige mogelijkheid voor de partitienamen van de bijvoorbeeld '0', '1' en '2'.

De werkelijke automatische schaling bewerking die wordt uitgevoerd conform deze schematische ook:
* Als huidige partities van de service zijn met de naam '0', '1' en '2', klikt u vervolgens de partitie die worden toegevoegd voor het uitbreiden van de naam "3".
* Als de huidige partities van de service zijn met de naam '0', '1' en '2', is de partitie die worden verwijderd voor schaling in partitie met de naam '2'.

Hetzelfde als bij mechanisme dat wordt gebruikt door toe te voegen of te verwijderen van instanties schalen, er zijn drie parameters die bepalen hoe dit mechanisme wordt toegepast:
* _Schalen verhoging_ bepaalt hoeveel partities worden toegevoegd of verwijderd wanneer mechanisme wordt geactiveerd.
* _Maximaal aantal partities_ definieert de bovengrens voor het schalen. Als deze limiet wordt bereikt door het aantal partities van de service, wordt klikt u vervolgens de service niet worden uitgebreid, ongeacht de belasting. Het is mogelijk om deze limiet weglaten door de waarde van-1 en in dat geval de service wordt aangepast buiten zo veel mogelijk (de limiet is de werkelijke capaciteit van het cluster).
* _Minimuminstantieaantal_ definieert de ondergrens voor het schalen. Als deze limiet wordt bereikt door het aantal partities van de service, zal vervolgens service niet worden geschaald ongeacht de belasting.

## <a name="setting-auto-scaling-policy"></a>Automatische schaling beleid instellen

### <a name="using-application-manifest"></a>Met behulp van het toepassingsmanifest
``` xml
<ServiceScalingPolicies>
    <ScalingPolicy>
        <AverageServiceLoadScalingTrigger MetricName="servicefabric:/_MemoryInMB" LowerLoadThreshold="300" UpperLoadThreshold="500" ScaleIntervalInSeconds="600"/>
        <AddRemoveIncrementalNamedParitionScalingMechanism MinPartitionCount="1" MaxPartitionCount="3" ScaleIncrement="1"/>
    </ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>Met C#-API 's
```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
AveragePartitionLoadScalingTrigger trigger = new AverageServiceLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new AddRemoveIncrementalNamedParitionScalingMechanism();
mechanism.MaxPartitionCount = 4;
mechanism.MinPartitionCount = 1;
mechanism.ScaleIncrement = 1;
//expecting that the service already has metric NumberOfConnections
trigger.MetricName = "NumberOfConnections";
trigger.ScaleInterval = TimeSpan.FromMinutes(15);
trigger.LowerLoadThreshold = 10000;
trigger.UpperLoadThreshold = 20000;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceUpdate.ScalingPolicies = new List<ScalingPolicyDescription>;
serviceUpdate.ScalingPolicies.Add(policy);
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), serviceUpdate);
```
### <a name="using-powershell"></a>Met behulp van Powershell
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.AddRemoveIncrementalNamedParitionScalingMechanism
$mechanism.MinPartitionCount = 1
$mechanism.MaxPartitionCount = 3
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AverageServiceLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_MemoryInMB"
$trigger.LowerLoadThreshold = 5000
$trigger.UpperLoadThreshold = 10000
$trigger.ScaleInterval = New-TimeSpan -Minutes 25
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -TargetReplicaSetSize 3 -MinReplicaSetSize 2 -HasPersistedState true -PartitionNames @("0","1") -ServicePackageActivationMode ExclusiveProcess -ScalingPolicies $scalingpolicies
```

## <a name="auto-scaling-based-on-resources"></a>Automatisch schalen op basis van bronnen

Om in te schakelen van de resource-monitor-service te schalen op basis van de werkelijke resources

``` json
"fabricSettings": [
...      
],
"addonFeatures": [
    "ResourceMonitorService"
],
```
Er zijn twee metrische gegevens die de daadwerkelijke fysieke resources vertegenwoordigen. Een van beide is servicefabric: / _CpuCores die het werkelijke cpu-gebruik (zodat 0,5 halve een kern vertegenwoordigt) en de andere vertegenwoordigen wordt servicefabric: / _MemoryInMB die het geheugengebruik in MB's vertegenwoordigt.
ResourceMonitorService is verantwoordelijk voor het bijhouden van het gebruik van cpu en geheugen van de services van de gebruiker. Deze service toepassing om het account voor mogelijke tijdelijke pieken gewogen zwevend gemiddelde. Broncontrole wordt ondersteund voor beperkte en niet-beperkte toepassingen op Windows en voor beperkte fragmenten op Linux. Automatisch schalen op resources is alleen ingeschakeld voor services die zijn geactiveerd in [exclusieve procesmodel](service-fabric-hosting-model.md#exclusive-process-model).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
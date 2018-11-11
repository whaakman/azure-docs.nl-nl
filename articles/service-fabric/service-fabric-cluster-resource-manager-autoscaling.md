---
title: Azure Service Fabric automatisch vergroten/verkleinen Services en Containers | Microsoft Docs
description: Azure Service Fabric kunt u automatisch schalen, beleidsregels voor services en containers instellen.
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
ms.openlocfilehash: 8e57c071c9fd93a8581d574aeec2b23b38b3ab95
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281651"
---
# <a name="introduction-to-auto-scaling"></a>Inleiding tot automatisch schalen
Automatisch schalen is een extra functie van Service Fabric voor het dynamisch schalen van uw services op basis van de belasting van services rapporteren of op basis van hun gebruik van resources. Automatisch schalen biedt grote flexibiliteit en inrichting van extra exemplaren of partities van de service op aanvraag. De gehele functie voor automatisch schalen van proces is automatisch en transparant en zodra het instellen van uw beleid voor een service is niet nodig voor handmatige vergroten / verkleinen op het serviceniveau van de. Automatisch schalen kan worden ingeschakeld tijdens de aanmaak van de service of op elk gewenst moment door het bijwerken van de service.

Een veelvoorkomend scenario waarbij automatisch schalen nuttig is is wanneer de belasting van een bepaalde service gedurende een periode varieert. Bijvoorbeeld, een service, zoals een gateway kan worden geschaald op basis van de hoeveelheid resources die nodig zijn om binnenkomende aanvragen te verwerken. We gaan een voorbeeld van wat deze regels vergroten/verkleinen kunnen er als volgt uitzien:
* Als alle exemplaren van de gateway met meer dan twee cores gemiddeld, schaalt u de gateway-service uit door één exemplaar van de meer toe te voegen. Hiervoor elk uur, maar er nooit meer dan zeven exemplaren in totaal.
* Als alle exemplaren van de gateway met minder dan 0,5 kernen gemiddeld, schaalt u de service in door het verwijderen van één exemplaar. Hiervoor elk uur, maar minder dan drie exemplaren nooit in totaal hebben.

Automatisch schalen wordt ondersteund voor containers en reguliere Service Fabric-services. Als u wilt gebruiken voor automatisch schalen, moet u worden uitgevoerd op versie 6.2 of hoger van de Service Fabric-runtime. 

De rest van dit artikel beschrijft de schaalbeleid is, manieren om te schakelen of om uit te schakelen automatisch schalen, en worden voorbeelden gegeven van hoe u deze functie wilt gebruiken.

## <a name="describing-auto-scaling"></a>Met een beschrijving van automatisch schalen
Functie voor automatisch schalen van beleid kan worden gedefinieerd voor elke service in een Service Fabric-cluster. Elk beleid voor vergroten/verkleinen bestaat uit twee onderdelen:
* **Schalen van de trigger** wordt beschreven wanneer het schalen van de service wordt uitgevoerd. Voorwaarden die zijn gedefinieerd in de trigger worden periodiek gecontroleerd om te bepalen als een service of niet moet worden geschaald.

* **Schalen mechanisme** wordt beschreven hoe schalen wordt uitgevoerd wanneer deze wordt geactiveerd. Mechanisme wordt alleen toegepast als de voorwaarden van de trigger wordt voldaan.

Alle triggers die momenteel worden ondersteund werken met [logische meetwaarden](service-fabric-cluster-resource-manager-metrics.md), of met fysieke metrische gegevens zoals CPU-en geheugengebruik. In beide gevallen, Service Fabric wordt de gerapporteerde belasting voor de metrische gegevens controleren en evalueren de trigger regelmatig om te bepalen of schalen nodig is.

Er zijn twee methoden die momenteel worden ondersteund voor automatisch schalen. Het eerste item is bedoeld voor stateless services of voor containers wanneer automatisch schalen wordt uitgevoerd door het toevoegen of verwijderen van [exemplaren](service-fabric-concepts-replica-lifecycle.md). Stateful en stateless services, automatisch schalen kan ook worden uitgevoerd door toe te voegen of te verwijderen met de naam [partities](service-fabric-concepts-partitioning.md) van de service.

> [!NOTE]
> Er is momenteel ondersteuning voor slechts één schaalbeleid per service en slechts één vergroten/verkleinen trigger per schaalbeleid.

## <a name="average-partition-load-trigger-with-instance-based-scaling"></a>Gemiddelde partitie load trigger met schalen op basis van exemplaar
Het eerste type van de trigger is gebaseerd op de belasting van de exemplaren in een servicepartitie stateless. Metrische gegevens geladen worden eerst geëffend om op te halen van de belasting voor elk exemplaar van een partitie en vervolgens deze waarden zijn gemiddeld over alle exemplaren van de partitie. Er zijn drie factoren die bepalen wanneer de service wordt aangepast:

* _Drempelwaarde voor het laden van lagere_ is een waarde die bepaalt wanneer de service worden **geschaald in**. Als de gemiddelde belasting van alle exemplaren van de partities lager is dan deze waarde is, wordt de service worden geschaald.
* _Hoogste belastingsdrempelwaarde_ is een waarde die bepaalt wanneer de service worden **uitgeschaalde**. Als de gemiddelde belasting van alle exemplaren van de partitie hoger dan deze waarde is, zal klikt u vervolgens de service worden uitgebreid.
* _Interval voor vergroten/verkleinen_ bepaalt hoe vaak de trigger wordt gecontroleerd. Zodra de trigger is gecontroleerd, indien nodig schalen is het mechanisme wordt toegepast. Als schalen niet vereist is, klikt u vervolgens geen actie ondernomen. In beide gevallen wordt trigger niet gecontroleerd opnieuw voordat het interval voor vergroten/verkleinen is verstreken opnieuw.

Deze trigger kan alleen worden gebruikt met stateless services (stateless containers of Service Fabric-services). In het geval wanneer een service meerdere partities heeft, de trigger wordt geëvalueerd voor elke partitie afzonderlijk en elke partitie het opgegeven mechanisme onafhankelijk toegepast heeft. Daarom kan dit het geval is, is het mogelijk dat enkele van de partities van de service zal worden uitgebreid, worden sommige worden geschaald, en sommige wordt niet worden geschaald helemaal op hetzelfde moment, op basis van de belasting.

Het enige mechanisme dat kan worden gebruikt met deze trigger is PartitionInstanceCountScaleMechanism. Er zijn drie factoren die bepalen hoe dit mechanisme wordt toegepast:
* _Verhoging schalen_ bepaalt hoeveel exemplaren worden toegevoegd of verwijderd wanneer het mechanisme wordt geactiveerd.
* _Maximum aantal exemplaren_ bepaalt de bovenste limiet voor het schalen. Als het aantal exemplaren van de partitie die deze limiet bereikt, wordt klikt u vervolgens de service niet worden uitgebreid, ongeacht de belasting. Het is mogelijk om deze limiet weglaten door op te geven waarde-1 en in dat geval de service wordt aangepast out zo veel mogelijk (de limiet is het aantal knooppunten die beschikbaar in het cluster zijn).
* _Minimuminstantieaantal_ bepaalt de lagere limiet voor het schalen. Als het aantal exemplaren van de partitie die deze limiet bereikt, zal klikt u vervolgens service niet worden geschaald, ongeacht de belasting.

## <a name="setting-auto-scaling-policy"></a>Automatisch schalen, beleid instellen

### <a name="using-application-manifest"></a>Met behulp van het manifest voor toepassing
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
### <a name="using-c-apis"></a>Met behulp van C#-API 's
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

## <a name="average-service-load-trigger-with-partition-based-scaling"></a>Gemiddelde service load activeren met het schalen van partities op basis van
De tweede trigger is gebaseerd op de belasting van alle partities van een service. Metrische gegevens geladen worden eerst geëffend om op te halen van de belasting voor elke replica of het exemplaar van een partitie. Voor stateful services de belasting van de partitie wordt beschouwd als de belasting van de primaire replica, terwijl voor stateless services de belasting van de partitie de gemiddelde belasting van alle exemplaren van de partitie is. Deze waarden zijn gemiddeld voor alle partities van de service en deze waarde wordt gebruikt voor het activeren van het automatisch schalen. Gelijk aan die in vorige mechanisme, er zijn drie factoren die bepalen wanneer de service wordt aangepast:

* _Drempelwaarde voor het laden van lagere_ is een waarde die bepaalt wanneer de service worden **geschaald in**. Als de gemiddelde belasting van alle partities van de service lager dan deze waarde is, wordt de service worden geschaald.
* _Hoogste belastingsdrempelwaarde_ is een waarde die bepaalt wanneer de service worden **uitgeschaalde**. Als de gemiddelde belasting van alle partities van de service hoger dan deze waarde is, zal klikt u vervolgens de service worden uitgebreid.
* _Interval voor vergroten/verkleinen_ bepaalt hoe vaak de trigger wordt gecontroleerd. Zodra de trigger is gecontroleerd, indien nodig schalen is het mechanisme wordt toegepast. Als schalen niet vereist is, klikt u vervolgens geen actie ondernomen. In beide gevallen wordt trigger niet gecontroleerd opnieuw voordat het interval voor vergroten/verkleinen is verstreken opnieuw.

Deze trigger kan worden gebruikt met stateful en stateless services. Het enige mechanisme dat kan worden gebruikt met deze trigger is AddRemoveIncrementalNamedPartitionScalingMechanism. Wanneer de service is uitgebreid en vervolgens een nieuwe partitie wordt toegevoegd, en als service wordt geschaald in een van de bestaande partities wordt verwijderd. Er zijn beperkingen die worden gecontroleerd als service wordt gemaakt of bijgewerkt en service maken/bijwerken mislukt als niet aan deze voorwaarden wordt voldaan:
* Benoemde partitieschema moet worden gebruikt voor de service.
* Partitienamen moeten bestaan uit getallen opeenvolgende geheel getal, zoals '0', '1',...
* Naam van de eerste partitie moet '0'.

Als een service is in eerste instantie worden gemaakt met drie partities, is de enige geldige mogelijkheid voor partitienamen bijvoorbeeld "0", "1" en "2".

Ook deze naamgevingsschema houdt zich aan de werkelijke functie voor automatisch schalen van de bewerking die wordt uitgevoerd:
* Als huidige partities van de service zijn met de naam "0", "1" en "2", klikt u vervolgens de partitie die worden toegevoegd voor het schalen van de naam '3'.
* Als de huidige partities van de service zijn met de naam "0", "1" en "2", is de partitie die worden verwijderd voor schalen in partitie met de naam '2'.

Hetzelfde als bij mechanisme dat wordt gebruikt door het toevoegen of verwijderen van exemplaren schalen, er zijn drie parameters die bepalen hoe dit mechanisme wordt toegepast:
* _Verhoging schalen_ bepaalt het aantal partities wordt toegevoegd of verwijderd wanneer het mechanisme wordt geactiveerd.
* _Maximaal aantal partities_ bepaalt de bovenste limiet voor het schalen. Als deze limiet wordt bereikt door het aantal partities van de service, wordt klikt u vervolgens de service niet worden uitgebreid, ongeacht de belasting. Het is mogelijk om deze limiet weglaten door op te geven waarde-1 en in dat geval de service wordt aangepast out zo veel mogelijk (de limiet is de werkelijke capaciteit van het cluster).
* _Minimuminstantieaantal_ bepaalt de lagere limiet voor het schalen. Als deze limiet wordt bereikt door het aantal partities van de service, zal klikt u vervolgens service niet worden geschaald, ongeacht de belasting.

> [!WARNING] 
> Wanneer AddRemoveIncrementalNamedPartitionScalingMechanism wordt gebruikt met stateful services, Service Fabric wordt toevoegen of verwijderen van partities **zonder waarschuwing of melding**. Partitioneren van gegevens wordt niet uitgevoerd wanneer het mechanisme voor schalen wordt geactiveerd. In het geval van bewerking omhoog schalen, nieuwe partities niet leeg zijn en in het geval van bewerking, omlaag schalen **partitie worden verwijderd, samen met de gegevens die deze bevat**.

## <a name="setting-auto-scaling-policy"></a>Automatisch schalen, beleid instellen

### <a name="using-application-manifest"></a>Met behulp van het manifest voor toepassing
``` xml
<ServiceScalingPolicies>
    <ScalingPolicy>
        <AverageServiceLoadScalingTrigger MetricName="servicefabric:/_MemoryInMB" LowerLoadThreshold="300" UpperLoadThreshold="500" ScaleIntervalInSeconds="600"/>
        <AddRemoveIncrementalNamedPartitionScalingMechanism MinPartitionCount="1" MaxPartitionCount="3" ScaleIncrement="1"/>
    </ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>Met behulp van C#-API 's
```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
AveragePartitionLoadScalingTrigger trigger = new AverageServiceLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new AddRemoveIncrementalNamedPartitionScalingMechanism();
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
$mechanism = New-Object -TypeName System.Fabric.Description.AddRemoveIncrementalNamedPartitionScalingMechanism
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

Om in te schakelen van de resource-monitor-service om te schalen op basis van werkelijke bronnen

``` json
"fabricSettings": [
...      
],
"addonFeatures": [
    "ResourceMonitorService"
],
```
Er zijn twee metrische gegevens die staan voor daadwerkelijke fysieke resources. Een van beide is Service fabric: / _CpuCores die het werkelijke cpu-gebruik (zodat 0,5 de helft van een kern vertegenwoordigt) en de andere vertegenwoordigen wordt service fabric: / _MemoryInMB die staat voor het gebruik van geheugen in MB/s.
ResourceMonitorService is verantwoordelijk voor het bijhouden van het gebruik van cpu en geheugen van de services gebruiken. Deze service is gewogen zwevend gemiddelde van toepassing om het account voor mogelijke tijdelijke pieken. Controle van bronnen wordt ondersteund voor zowel in containers en niet-App in een container toepassingen op Windows en voor containers die op Linux. Functie voor automatisch schalen van resources is alleen ingeschakeld voor services die zijn geactiveerd in [exclusieve procesmodel](service-fabric-hosting-model.md#exclusive-process-model).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).

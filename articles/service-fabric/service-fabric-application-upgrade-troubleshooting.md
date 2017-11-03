---
title: Het oplossen van problemen toepassingsupgrades | Microsoft Docs
description: In dit artikel bevat enkele veelvoorkomende problemen rond een upgrade van een Service Fabric-toepassing en hoe u deze kunt oplossen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/03/2017
ms.author: subramar
ms.openlocfilehash: acfd26674aafab4ed1925d6b33967f917058b1be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-application-upgrades"></a>Problemen met toepassingsupgrades oplossen
In dit artikel bevat informatie over enkele van de algemene problemen rond een upgrade van een Azure Service Fabric-toepassing en hoe u deze kunt oplossen.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Een upgrade van de mislukte toepassing oplossen
Wanneer een upgrade mislukt, de uitvoer van de **Get-ServiceFabricApplicationUpgrade** -opdracht bevat aanvullende informatie voor foutopsporing van de fout.  De volgende lijst geeft aan hoe de aanvullende informatie kan worden gebruikt:

1. Identificeer het type is mislukt.
2. Identificeer de reden is mislukt.
3. Een of meer onderdelen mislukken voor verder onderzoek isoleren.

Deze informatie is beschikbaar wanneer het Service Fabric de fout ongeacht of detecteert de **FailureAction** is terugdraait of onderbreken van de upgrade.

### <a name="identify-the-failure-type"></a>Identificatie van het fouttype
In de uitvoer van **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identificeert de tijdstempel (in UTC-Notatie) waarmee een upgrade mislukt is gedetecteerd door de Service Fabric en  **FailureAction** is geactiveerd. **FailureReason** identificeert een van drie op hoog niveau mogelijke oorzaken van de fout:

1. UpgradeDomainTimeout - geeft aan dat een bepaald domein upgrade duurt te lang om te voltooien en **UpgradeDomainTimeout** verlopen.
2. OverallUpgradeTimeout - geeft aan dat de algehele upgrade duurt te lang om te voltooien en **UpgradeTimeout** verlopen.
3. Statuscontrole - geeft aan dat na de upgrade van een updatedomein de toepassing nog steeds niet in orde volgens de opgegeven statusbeleid en **HealthCheckRetryTimeout** verlopen.

Deze vermeldingen alleen weergegeven in de uitvoer wanneer de upgrade mislukt en wordt gestart door terug te draaien. Meer informatie wordt weergegeven, afhankelijk van het type van de fout.

### <a name="investigate-upgrade-timeouts"></a>Upgrade-outs onderzoeken
Time-out fouten meestal door problemen met de servicebeschikbaarheid veroorzaakt worden bijwerken. De uitvoer hieronder is Typerend voor upgrades waarbij service replica's of exemplaren niet worden gestart in de nieuwe codeversie. De **UpgradeDomainProgressAtFailure** veld een momentopname van in behandeling upgrade werk op het moment van de fout wordt vastgelegd.

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
```

In dit voorbeeld wordt de upgrade is mislukt op upgradedomein *MYUD1* en twee partities (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* en *4b43f4d8-b26b-424e-9307-7a7a62e79750*) zijn vastgelopen. De partities zijn vastgelopen omdat de runtime kan plaatsen van de primaire replica's (*WaitForPrimaryPlacement*) op de doelknooppunten *knooppunt1* en *Knooppunt4*.

De **Get-ServiceFabricNode** opdracht kan worden gebruikt om te controleren of deze twee knooppunten in het upgradedomein *MYUD1*. De *UpgradePhase* zegt *PostUpgradeSafetyCheck*, wat betekent dat deze controles veiligheid plaatsvinden nadat alle knooppunten in het upgradedomein bijgewerkt hebt. Deze informatie verwijst naar een mogelijk probleem met de nieuwe versie van de toepassingscode. De meest voorkomende problemen zijn fouten in de open of promotie tot primaire codepaden-service.

Een *UpgradePhase* van *PreUpgradeSafetyCheck* betekent dat er zijn problemen met het voorbereiden van het upgradedomein voordat deze werd uitgevoerd. De meest voorkomende problemen zijn in dit geval fouten in de sluiten of degradatie uit naar de primaire code van de service.

De huidige **UpgradeState** is *RollingBackCompleted*, zodat de oorspronkelijke upgrade moet worden uitgevoerd met een rollback **FailureAction**, die automatisch samengevouwen back-de upgrade mislukt. Als de oorspronkelijke upgrade is uitgevoerd met een handmatige **FailureAction**, en vervolgens de upgrade wordt in plaats daarvan worden in een onderbroken staat om toe te staan live foutopsporing van de toepassing.

In zeldzame gevallen, de **UpgradeDomainProgressAtFailure** veld mag niet leeg zijn als de upgrade van de algemene time-out optreedt, net zoals het systeem al het werk voor het huidige upgradedomein is voltooid. Als dit gebeurt, vergroot de **UpgradeTimeout** en **UpgradeDomainTimeout** parameterwaarden upgrade en probeer de upgrade.

### <a name="investigate-health-check-failures"></a>Health selectievakje fouten onderzoeken
Health selectievakje fouten kunnen worden geactiveerd door verschillende problemen die kunnen ontstaan nadat alle knooppunten in een upgradedomein upgraden en alle controles van de veiligheid wordt doorgegeven. De uitvoer hieronder is standaard een upgrade mislukt vanwege een mislukte statuscontroles. De **UnhealthyEvaluations** veld bevat een momentopname van statuscontroles die niet op het moment van de upgrade volgens de opgegeven [statusbeleid](service-fabric-health-introduction.md).

```
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Een goed begrip van de Service Fabric-statusmodel health selectievakje fouten onderzoeken eerst worden vereist. Maar zelfs zonder een diepgaand inzicht, ziet u twee services zijn slecht: *fabric: / DemoApp/Svc3* en *fabric: / DemoApp/Svc2*, samen met de foutenrapporten health ('InjectedFault' in dit geval). In dit voorbeeld twee vier services zijn beschadigd, wat minder is dan het doel van de standaard van 0% slecht (*MaxPercentUnhealthyServices*).

De upgrade van het mislukken is onderbroken door te geven een **FailureAction** handmatig bij het starten van de upgrade. Deze modus, kunnen wij voor het onderzoeken van het live systeem in de mislukte status alvorens verdere actie te ondernemen.

### <a name="recover-from-a-suspended-upgrade"></a>Herstellen van een onderbroken upgrade
Met terugdraaien **FailureAction**, er is geen herstel nodig omdat de upgrade automatisch teruggedraaid in bij mislukken. Met een handmatige **FailureAction**, er zijn verschillende opties voor herstel:

1.  trigger terugdraaien
2. Volg de stappen in de rest van de upgrade handmatig
3. De bewaakte upgrade hervatten

De **Start ServiceFabricApplicationRollback** opdracht kan worden gebruikt op elk gewenst moment worden gestart met het terugdraaien van de toepassing. Zodra de opdracht is retourneert, wordt de rollback-aanvraag is geregistreerd in het systeem en kort nadien begint.

De **hervatten ServiceFabricApplicationUpgrade** opdracht kan worden gebruikt om door te gaan in de rest van de upgrade handmatig één upgradedomein tegelijk. In deze modus worden alleen veiligheid controles uitgevoerd door het systeem. Niet meer health controles worden uitgevoerd. Met deze opdracht kan alleen worden gebruikt wanneer de *UpgradeState* toont *RollingForwardPending*, wat betekent dat het upgraden van het huidige upgradedomein is voltooid, maar de volgende gateway niet gestart is (in behandeling).

De **Update ServiceFabricApplicationUpgrade** opdracht kan worden gebruikt voor het hervatten van de bewaakte upgrade met beide veiligheid en health controleert wordt uitgevoerd.

```
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
```

De upgrade blijft van het upgradedomein waar deze het laatst werd onderbroken en gebruik dezelfde parameters en statusbeleid als vóór upgrade. Indien nodig, kan een van de parameters voor het bijwerken en statusbeleid wordt weergegeven in de voorgaande uitvoer worden gewijzigd in dezelfde opdracht als de upgrade wordt hervat. In dit voorbeeld is de upgrade in de bewaakte modus met de parameters en het statusbeleid ongewijzigd hervat.

## <a name="further-troubleshooting"></a>Meer informatie over
### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric volgt niet het opgegeven statusbeleid
Mogelijke oorzaak 1:

Service Fabric alle percentages vertaalt in het werkelijke aantal entiteiten (bijvoorbeeld, replica's, partities en services) voor de statusevaluatie en altijd rondt af naar boven op hele entiteiten. Bijvoorbeeld, als de maximale *MaxPercentUnhealthyReplicasPerPartition* 21% is en er zijn vijf replica's, en vervolgens de Service Fabric kan maximaal twee slecht replica's (dat wil zeggen,`Math.Ceiling (5*0.21)`). Statusbeleid moeten dus dienovereenkomstig worden ingesteld.

Mogelijke oorzaak 2:

Statusbeleid worden opgegeven in termen van percentage van totale services en niet specifiek service-exemplaren. Bijvoorbeeld, voordat u een upgrade uitvoert als een toepassing vier heeft service-exemplaren A, B, C en D, waarbij D service niet in orde is, maar weinig invloed hebben op de toepassing. We willen het negeren van de bekende slecht service D tijdens de upgrade en stel de parameter *MaxPercentUnhealthyServices* 25%, ervan uitgaande dat alleen A, B en C moet worden in orde.

Tijdens de upgrade kan D worden echter in orde terwijl C slecht. De upgrade zou desondanks slagen omdat alleen 25% van de services slecht zijn. Het kan echter leiden tot onverwachte fouten als gevolg van C wordt onverwacht slecht in plaats van D. In dit geval D gemodelleerd als een andere servicetype uit A, B en c Omdat statusbeleid per servicetype zijn opgegeven, kan ander slecht percentage drempelwaarden kunnen worden toegepast met andere services. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Ik een statusbeleid voor upgrade van de toepassing niet is opgegeven, maar nog steeds mislukt de upgrade voor een aantal time-outs die ik nooit opgegeven
Wanneer statusbeleid zijn niet opgegeven voor de update-aanvraag, ze van overgenomen worden de *ApplicationManifest.xml* van de huidige toepassingsversie. Bijvoorbeeld, als u een X van de toepassing van versie 1.0 naar versie 2.0, statusbeleid voor toepassing opgegeven upgrade uitvoert voor versie 1.0 gebruikt. Als een andere statusbeleid moet worden gebruikt voor de upgrade, klikt u vervolgens moet het beleid worden opgegeven als onderdeel van de toepassing upgrade API-aanroep. Het beleid dat is opgegeven als onderdeel van de API-aanroep zijn alleen van toepassing tijdens de upgrade. Zodra de upgrade voltooid is, het beleid moet worden opgegeven in de *ApplicationManifest.xml* worden gebruikt.

### <a name="incorrect-time-outs-are-specified"></a>Onjuiste time-outs zijn opgegeven
U zich mogelijk afgevraagd over wat er gebeurt wanneer time-outs inconsistent worden ingesteld. Bijvoorbeeld, u wellicht een *UpgradeTimeout* dat kleiner is dan het *UpgradeDomainTimeout*. Het antwoord is dat er een fout is geretourneerd. Fouten worden geretourneerd als de *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckRetryTimeout*, of als  *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Mijn upgrades duurt te lang
De tijd voor een upgrade is voltooid, is afhankelijk van de statuscontroles en time-outs opgegeven. Statuscontroles en time-outs afhankelijk van hoe lang het duurt om te kopiëren, implementeren en regulering van de toepassing. Wordt te agressief met time-outs kan het zijn dat meer mislukte upgrades, zodat u het beste eerst conservatieve met langer time-outs.

Dit is een snelle refresher op hoe de time-outs met de upgrade tijden communiceren:

Voert een upgrade voor een upgradedomein kan niet worden voltooid sneller dan *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Upgrade mislukt is niet mogelijk sneller dan *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

De tijd voor de upgrade voor een upgradedomein wordt beperkt door *UpgradeDomainTimeout*.  Als *HealthCheckRetryTimeout* en *HealthCheckStableDuration* zijn beide niet gelijk is aan nul en de status van de toepassing blijft heen en weer schakelen en vervolgens de upgrade uiteindelijk een optreedt op time-out*UpgradeDomainTimeout*. *UpgradeDomainTimeout* begint te tellen omlaag eenmaal de upgrade voor het huidige upgradedomein begint.

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

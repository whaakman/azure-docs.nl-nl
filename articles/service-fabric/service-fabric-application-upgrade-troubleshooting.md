---
title: Problemen met toepassingsupgrades oplossen | Microsoft Docs
description: In dit artikel bevat enkele veelvoorkomende problemen rond een upgrade van een Service Fabric-toepassing en hoe u ze op te lossen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 19ad152e-ec50-4327-9f19-065c875c003c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: e393eb92e11dc8dc296f1dc5f1c0036566c285c5
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58792447"
---
# <a name="troubleshoot-application-upgrades"></a>Problemen met toepassingsupgrades oplossen

In dit artikel vindt u enkele van de algemene problemen rond een upgrade van een Azure Service Fabric-toepassing en hoe u ze op te lossen.

## <a name="troubleshoot-a-failed-application-upgrade"></a>De upgrade van een mislukte toepassing oplossen

Wanneer een upgrade is mislukt, de uitvoer van de **Get-ServiceFabricApplicationUpgrade** opdracht bevat aanvullende informatie voor foutopsporing van de fout.  De volgende lijst geeft aan hoe de aanvullende informatie kan worden gebruikt:

1. Identificeer het fouttype.
2. Identificeer de reden van fout.
3. Isolatie van een of meer mislukte onderdelen voor verder onderzoek.

Deze informatie is beschikbaar wanneer de Service Fabric detecteert de fout, ongeacht of u de **FailureAction** is het forestfunctionaliteitsniveau terug te zetten of onderbreken van de upgrade.

### <a name="identify-the-failure-type"></a>Het fouttype identificeren

In de uitvoer van **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** identificeert de tijdstempel (in UTC) waarop een upgrade mislukt is gedetecteerd door de Service Fabric en  **FailureAction** is geactiveerd. **FailureReason** identificeert een van drie op hoog niveau mogelijke oorzaken van de fout:

1. UpgradeDomainTimeout - geeft aan dat een bepaalde upgradedomein duurt te lang om te voltooien en **UpgradeDomainTimeout** is verlopen.
2. OverallUpgradeTimeout - geeft aan dat de algehele upgrade duurt te lang om te voltooien en **UpgradeTimeout** is verlopen.
3. Statuscontrole - geeft aan dat na een upgrade van een updatedomein, de toepassing nog steeds niet in orde op basis van de opgegeven statusbeleid en **HealthCheckRetryTimeout** is verlopen.

Deze vermeldingen alleen weergegeven in de uitvoer wanneer de upgrade mislukt en wordt gestart terug te draaien. Meer informatie wordt weergegeven, afhankelijk van het type van de fout.

### <a name="investigate-upgrade-timeouts"></a>Upgrade-outs onderzoeken

Time-out voor fouten meestal door problemen met de servicebeschikbaarheid veroorzaakt worden upgrade. De uitvoer die hieronder is karakteristiek upgrades waarbij service-replica's of exemplaren niet worden gestart in de nieuwe codeversie. De **UpgradeDomainProgressAtFailure** veld een momentopname van in behandeling upgrade werk op het moment van de fout wordt vastgelegd.

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/DemoApp
```

```Output
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

De **Get-ServiceFabricNode** opdracht kan worden gebruikt om te controleren of deze twee knooppunten in het upgradedomein *MYUD1*. De *UpgradePhase* zegt *PostUpgradeSafetyCheck*, wat betekent dat deze controles veiligheid plaatsvinden nadat de upgrade van alle knooppunten in het upgradedomein hebt voltooid. Al deze gegevens verwijst naar een mogelijk probleem met de nieuwe versie van de toepassingscode. De meest voorkomende problemen zijn fouten in de open of promotie naar primaire codepaden-service.

Een *UpgradePhase* van *PreUpgradeSafetyCheck* betekent dat er zijn problemen met het voorbereiden van het upgradedomein voordat deze werd uitgevoerd. De meest voorkomende problemen zijn in dit geval fouten in de sluiten of degradatie van primaire codepaden-service.

De huidige **UpgradeState** is *RollingBackCompleted*, zodat de oorspronkelijke upgrade moet worden uitgevoerd met een terugdraaiactie **FailureAction**, die automatisch samengevouwen back-ups maken de upgrade mislukt. Als de oorspronkelijke upgrade is uitgevoerd met een handmatige **FailureAction**, en vervolgens de upgrade wordt in plaats daarvan worden de status onderbroken om toe te staan live foutopsporing van de toepassing.

In zeldzame gevallen, het **UpgradeDomainProgressAtFailure** veld mag niet leeg zijn als de upgrade van de algemene time-out optreedt, net zoals alle werk voor het huidige upgradedomein is voltooid door het systeem. Als dit gebeurt, verhoog de **UpgradeTimeout** en **UpgradeDomainTimeout** parameterwaarden upgraden en probeer de upgrade.

### <a name="investigate-health-check-failures"></a>Health check-storingen onderzoeken

Controle-statusfouten kunnen worden geactiveerd door verschillende problemen die kunnen ontstaan nadat alle knooppunten in een upgradedomein upgraden en alle controles van de veiligheid te geven. De uitvoer volgen dit lid is van een upgrade mislukt vanwege een mislukte statuscontroles typische. De **UnhealthyEvaluations** veld bevat een momentopname van statuscontroles die niet op het moment van de upgrade op basis van de opgegeven [statusbeleid](service-fabric-health-introduction.md).

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/DemoApp
```

```Output
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

Controle-statusfouten onderzoeken eerst is vereist voor een goed begrip van het statusmodel van de Service Fabric. Maar zelfs zonder een diepgaand inzicht, kunnen we zien dat de twee services niet in orde zijn: *fabric: / DemoApp/Svc3* en *fabric: / DemoApp/Svc2*, samen met de statusrapporten fout ("InjectedFault" in dit geval). In dit voorbeeld 2 van 4 services niet in orde zijn, wat lager is dan de standaard-doel van 0% niet in orde (*MaxPercentUnhealthyServices*).

De upgrade is onderbroken wanneer mislukken door op te geven een **FailureAction** van de handleiding bij het starten van de upgrade. In deze modus kan we voor het onderzoeken van het live systeem in de status mislukt voordat u verdere actie te ondernemen.

### <a name="recover-from-a-suspended-upgrade"></a>Herstellen na een upgrade van een onderbroken

Met een terugdraaiactie **FailureAction**, er is geen herstel nodig omdat de upgrade automatisch teruggedraaid in bij mislukken. Met een handmatige **FailureAction**, er zijn verschillende opties voor herstel:

1.  trigger terugdraaien
2. Volg de stappen in de rest van de upgrade handmatig
3. Hervatten van de bewaakte upgrade

De **Start ServiceFabricApplicationRollback** opdracht kan worden gebruikt op elk gewenst moment om te beginnen met het terugdraaien van de toepassing. Zodra de opdracht is geretourneerd, wordt de rollback-aanvraag is geregistreerd in het systeem en wordt kort daarna gestart.

De **hervatten ServiceFabricApplicationUpgrade** opdracht kan worden gebruikt om door te gaan in de rest van de upgrade handmatig, één upgradedomein tegelijk. In deze modus wordt alleen veiligheid controles uitgevoerd door het systeem. Niet meer statuscontroles zijn uitgevoerd. Met deze opdracht kan alleen worden gebruikt wanneer de *UpgradeState* bevat *RollingForwardPending*, wat betekent dat de upgrade van het huidige upgradedomein is voltooid, maar het volgende object niet gestart is (in behandeling).

De **Update ServiceFabricApplicationUpgrade** opdracht kan worden gebruikt voor het hervatten van de bewaakte upgrade met beide veiligheid en statuscontroles die worden uitgevoerd.

```powershell
Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored
```

```Output
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
```

De upgrade blijft in het upgradedomein waar deze het laatst werd onderbroken en gebruik dezelfde parameters en statusbeleid als vóór upgrade. Indien nodig, kan een van de parameters voor het bijwerken en statusbeleid wordt weergegeven in de voorgaande uitvoer worden gewijzigd in dezelfde opdracht als de upgrade wordt gehaald. In dit voorbeeld is de upgrade in de bewaakte modus, met de parameters en het statusbeleid ongewijzigd hervat.

## <a name="further-troubleshooting"></a>Meer informatie

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric is de opgegeven statusbeleid niet volgen

Mogelijke oorzaak 1:

Service Fabric zet alle percentages in het werkelijke aantal entiteiten (bijvoorbeeld: replica's, partities en -services) voor de evaluatie van de status en altijd rondt af naar boven op hele entiteiten. Bijvoorbeeld, als de maximale *MaxPercentUnhealthyReplicasPerPartition* 21% is en er zijn vijf replica's, en vervolgens de Service Fabric kunt maximaal twee replica's beschadigd (dat wil zeggen,`Math.Ceiling (5*0.21)`). Statusbeleid moeten dus dienovereenkomstig worden ingesteld.

Mogelijke oorzaak 2:

Statusbeleid worden opgegeven in termen van percentage van totale services en niet specifiek service-exemplaren. Bijvoorbeeld, voordat u een upgrade uitvoert, als een toepassing vier heeft instanties A, B, C en D-service waar de D-service niet in orde is, maar weinig invloed hebben op de toepassing. We willen de bekende beschadigde service D tijdens de upgrade negeren en stel de parameter *MaxPercentUnhealthyServices* om 25%, ervan uitgaande dat alleen A, B en C moet in orde zijn.

Tijdens de upgrade, kan D worden echter in orde terwijl C slecht. De upgrade kan desondanks slagen omdat alleen 25% van de services niet in orde zijn. Echter kan leiden tot onverwachte fouten als gevolg van C wordt onverwacht niet in orde in plaats van D. In dit geval D moet worden gemodelleerd als een andere servicetype in A, B en C. Omdat het statusbeleid per servicetype zijn opgegeven, kunnen verschillende niet in orde percentage drempelwaarden naar verschillende services worden toegepast. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Kan ik een statusbeleid voor upgrade van de toepassing niet hebt opgegeven, maar de upgrade nog steeds mislukt voor sommige time-outs die ik heb nooit opgegeven

Wanneer statusbeleid niet zijn opgegeven voor de aanvraag om een upgrade, ze zijn afkomstig uit de *ApplicationManifest.xml* van de huidige toepassingsversie. Bijvoorbeeld, als u de X-toepassing van versie 1.0 naar versie 2.0, statusbeleid toepassing opgegeven bijwerkt voor in versie 1.0 worden gebruikt. Als een andere health-beleid moet worden gebruikt voor de upgrade, klikt u vervolgens moet het beleid worden opgegeven als onderdeel van de toepassing upgrade API-aanroep. Het beleid dat is opgegeven als onderdeel van de API-aanroep zijn alleen van toepassing tijdens de upgrade. Nadat de upgrade voltooid is, het beleid moet worden opgegeven in de *ApplicationManifest.xml* worden gebruikt.

### <a name="incorrect-time-outs-are-specified"></a>Onjuiste time-outs zijn opgegeven

U zich mogelijk afgevraagd over wat er gebeurt wanneer time-outs inconsistent worden ingesteld. Bijvoorbeeld, u mogelijk een *UpgradeTimeout* dat kleiner is dan de *UpgradeDomainTimeout*. Het antwoord is dat er een fout wordt geretourneerd. Er worden fouten geretourneerd als de *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckRetryTimeout*, of als  *UpgradeDomainTimeout* kleiner is dan de som van *HealthCheckWaitDuration* en *HealthCheckStableDuration*.

### <a name="my-upgrades-are-taking-too-long"></a>Mijn upgrades duurt lang

De tijd voor een upgrade te voltooien, is afhankelijk van de statuscontroles en time-outs die zijn opgegeven. Statuscontroles en time-outs afhankelijk van hoe lang het duurt om te kopiëren, distribueren en regulering van de toepassing. Wordt te agressief met time-outs kan het zijn dat meer mislukte upgrades, zodat u kunt het beste beginnen voorzichtig met langer time-outs.

Hier volgt een snel refresher op hoe de time-outs met de upgrade tijden communiceren:

Voert een upgrade voor een upgradedomein kan niet worden voltooid sneller dan *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Upgrade mislukt is niet mogelijk sneller dan *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

De tijd voor de upgrade voor een upgradedomein wordt beperkt door *UpgradeDomainTimeout*.  Als *HealthCheckRetryTimeout* en *HealthCheckStableDuration* beide nul zijn en de status van de toepassing blijft heen en weer schakelen en vervolgens de upgrade uiteindelijk een optreedt op time-out*UpgradeDomainTimeout*. *UpgradeDomainTimeout* begint tellen eenmaal omlaag van de upgrade voor het huidige upgradedomein begint.

## <a name="next-steps"></a>Volgende stappen

[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing met behulp van upgrades [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Upgrades van uw toepassingen compatibel maken door te leren hoe u [Gegevensserialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).
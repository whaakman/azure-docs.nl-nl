---
title: 'Upgrade van de toepassing: upgrade parameters | Microsoft Docs'
description: Beschrijft de parameters voor het upgraden van een Service Fabric-toepassing, met inbegrip van statuscontroles uit te voeren en het beleid om de upgrade automatisch ongedaan te maken.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f09dad590f32c10f75484bba9afb7ea60f29d81e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="application-upgrade-parameters"></a>Parameters toepassingsupgrade
In dit artikel beschrijft de verschillende parameters beschreven die van toepassing tijdens de upgrade van een Azure Service Fabric-toepassing. De parameters omvatten de naam en versie van de toepassing. Deze knoppen waarmee de time-outs en statuscontroles die worden toegepast tijdens de upgrade zijn en ze verwijzen naar de beleidsregels die moeten worden toegepast wanneer een upgrade mislukt.

<br>

| Parameter | Beschrijving |
| --- | --- |
| ApplicationName |Naam van de toepassing die wordt bijgewerkt. Voorbeelden: fabric: / VisualObjects, fabric: / ClusterMonitor |
| TargetApplicationTypeVersion |Typt u de versie van de toepassing die de upgrade doelen. |
| FailureAction |De actie op die door de Service Fabric wanneer de upgrade mislukt. De toepassing kan worden teruggezet naar de updateversie vooraf (rollback) of de upgrade op het huidige upgradedomein kan worden gestopt. In het laatste geval is de upgrademodus gewijzigd in handmatig. Toegestane waarden zijn terugdraaien en voor handmatig. |
| HealthCheckWaitDurationSec |Moet worden gewacht (in seconden) nadat de upgrade is voltooid op het upgradedomein voor Service Fabric evalueert de status van de toepassing. Deze duur kan ook worden beschouwd als de tijd die een toepassing moet worden uitgevoerd voordat deze kan worden overwogen in orde. Als de statuscontrole is verstreken, gaat het upgradeproces door naar het volgende upgradedomein.  Als u de statuscontrole is mislukt, wacht de Service Fabric voor een interval (UpgradeHealthCheckInterval) voordat u nogmaals probeert de statuscontrole totdat de HealthCheckRetryTimeout is bereikt. De standaard- en aanbevolen waarde is 0 seconden. |
| HealthCheckRetryTimeoutSec |De duur (in seconden) doorgaat die Service Fabric met het uitvoeren van de statusevaluatie voordat er wordt gemeld van de upgrade mislukt. De standaardwaarde is 600 seconden. Deze duur wordt gestart nadat HealthCheckWaitDuration is bereikt. In deze HealthCheckRetryTimeout kunt Service Fabric meerdere statuscontroles van de toepassingsstatus uitvoeren. De standaardwaarde is 10 minuten en moet op de juiste wijze worden aangepast voor uw toepassing. |
| HealthCheckStableDurationSec |De duur (in seconden) om te controleren of de toepassing stabiel is voordat het verplaatsen naar het volgende upgradedomein of voltooiing van de upgrade. Deze wachttijd duur wordt gebruikt om te voorkomen dat ongemerkt wijzigingen van de juiste status nadat de statuscontrole is uitgevoerd. De standaardwaarde is 120 seconden en moet op de juiste wijze worden aangepast voor uw toepassing. |
| UpgradeDomainTimeoutSec |Maximale tijd (in seconden) voor het upgraden van een enkel upgradedomein. Als de time-outwaarde is bereikt, wordt de upgrade gestopt en wordt voortgezet op basis van de instelling voor UpgradeFailureAction. De standaardwaarde is nooit (oneindig) en moet op de juiste wijze worden aangepast voor uw toepassing. |
| UpgradeTimeout |Er is een time-out (in seconden) die voor de hele upgrade geldt. Als de time-outwaarde is bereikt, wordt de upgrade gestopt en UpgradeFailureAction geactiveerd. De standaardwaarde is nooit (oneindig) en moet op de juiste wijze worden aangepast voor uw toepassing. |
| UpgradeHealthCheckInterval |Het interval dat de status is ingeschakeld. Deze parameter is opgegeven in de sectie ClusterManager van de *cluster* *manifest*, en niet is opgegeven als onderdeel van de upgrade cmdlet. De standaardwaarde is 60 seconden. |

<br>

## <a name="service-health-evaluation-during-application-upgrade"></a>Evaluatie van de health service tijdens de upgrade van de toepassing
<br>
De criteria voor evaluatie zijn optioneel. Als de criteria voor evaluatie niet zijn opgegeven wanneer een upgrade wordt gestart, Service Fabric maakt gebruik van het statusbeleid voor toepassing opgegeven in de ApplicationManifest.xml van het toepassingsexemplaar.

<br>

| Parameter | Beschrijving |
| --- | --- |
| ConsiderWarningAsError |Standaardwaarde is ONWAAR. De health-waarschuwingen voor de toepassing als fouten behandelen bij het evalueren van de status van de toepassing tijdens de upgrade. Standaard wordt Service Fabric health waarschuwingen als fouten (fouten), zodat de upgrade kan worden voortgezet zelfs als er waarschuwingsgebeurtenissen niet geëvalueerd. |
| MaxPercentUnhealthyDeployedApplications |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal geïmplementeerde toepassingen (Zie de [Health sectie](service-fabric-health-introduction.md)) die kan worden niet in orde voordat de toepassing is in orde beschouwd en de upgrade is mislukt. Deze parameter bepaalt de toepassingsstatus op het knooppunt en kunt u problemen detecteren tijdens de upgrade. De replica's van de toepassing ophalen doorgaans Netwerktaakverdeling naar het andere knooppunt, waardoor de toepassing wordt weergegeven in orde, zodat de upgrade om door te gaan. Door te geven de status van een strikte MaxPercentUnhealthyDeployedApplications, Service Fabric snel een probleem met het toepassingspakket detecteren en te produceren van een snelle upgrade mislukken. |
| MaxPercentUnhealthyServices |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal services in het toepassingsexemplaar die niet in orde zijn mag voordat de toepassing is in orde beschouwd en de upgrade is mislukt. |
| MaxPercentUnhealthyPartitionsPerService |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal partities in een service die niet in orde zijn kan voordat de service wordt beschouwd als niet in orde. |
| MaxPercentUnhealthyReplicasPerPartition |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal replica's in de partitie die niet in orde zijn mag voordat de partitie is in orde beschouwd. |
| UpgradeReplicaSetCheckTimeout |**Staatloze service**--binnen één upgradedomein Service Fabric probeert om ervoor te zorgen dat er extra exemplaren van de service beschikbaar zijn. Als de doel-exemplaren meer dan één is, wacht Service Fabric is meer dan één exemplaar beschikbaar tot een maximale time-outwaarde. De time-outwaarde is opgegeven door de eigenschap UpgradeReplicaSetCheckTimeout. Als de time-out is verlopen, wordt de Service Fabric voortgezet met de upgrade, ongeacht het aantal exemplaren van de service. Als de doel-exemplaren een is Service Fabric wacht niet en onmiddellijk wordt voortgezet met de upgrade. **Stateful service**--binnen één upgradedomein Service Fabric probeert om ervoor te zorgen dat de replicaset een quorum heeft. Service Fabric wacht tot een quorum beschikbaar tot een maximale time-outwaarde (opgegeven door de eigenschap UpgradeReplicaSetCheckTimeout). Als de time-out is verlopen, wordt de Service Fabric voortgezet met de upgrade, ongeacht het quorum. Deze instelling is ingesteld als nooit (oneindig) bij het implementeren van doorsturen en 900 seconden wanneer door terug te draaien. |
| ForceRestart |Als u een configuratie of een gegevenspakket zonder het bijwerken van de servicecode bijwerkt, de service is gestart alleen als de eigenschap ForceRestart is ingesteld op true. Wanneer de update voltooid is, waarschuwt Service Fabric de service dat een nieuwe configuratie of gegevenspakket beschikbaar is. De service is verantwoordelijk voor het toepassen van de wijzigingen. Indien nodig, kan de service zelf opnieuw. |

<br>
<br>
De criteria MaxPercentUnhealthyServices MaxPercentUnhealthyPartitionsPerService en MaxPercentUnhealthyReplicasPerPartition kunnen per type voor een exemplaar van de service worden opgegeven. Instellen van deze parameters per service kunt u een toepassing bevat verschillende services typen met beleidsregels voor verschillende evaluatie. Een stateless gateway servicetype kan bijvoorbeeld een MaxPercentUnhealthyPartitionsPerService die verschilt van een type stateful-engine-service voor een bepaalde toepassingsexemplaar hebben.

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

[Bijwerken van uw toepassing met behulp van Service Fabric CLI op Linux](service-fabric-application-lifecycle-sfctl.md#upgrade-application) begeleidt u bij een upgrade van de toepassing met behulp van Service Fabric CLI.

[Bijwerken van uw toepassing met behulp van Service Fabric Eclipse-invoegtoepassing](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).

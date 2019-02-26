---
title: Lijst met gebeurtenissen van Azure Service Fabric | Microsoft Docs
description: Uitgebreide lijst van gebeurtenissen die worden geleverd door Azure Service Fabric bewaken-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: 51a334557ddfb57ceab318963e319fd046d3a703
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820039"
---
# <a name="list-of-service-fabric-events"></a>Lijst met Service Fabric-gebeurtenissen 

Service Fabric wordt aangegeven dat een primaire set Clustergebeurtenissen om te informeren over de status van het cluster als [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md). Deze zijn gebaseerd op acties die door Service Fabric wordt uitgevoerd op uw knooppunten als uw cluster of management beslissingen door een cluster eigenaar/operator. Deze gebeurtenissen kunnen worden geopend door te configureren op een aantal manieren, inclusief het configureren van [Log Analytics met uw cluster](service-fabric-diagnostics-oms-setup.md), of het uitvoeren van query's de [EventStore](service-fabric-diagnostics-eventstore.md). Deze gebeurtenissen zijn opgenomen in het gebeurtenislogboek - op Windows-machines, zodat u Service Fabric-gebeurtenissen in Logboeken ziet. 

Hier volgen enkele kenmerken van deze gebeurtenissen
* Elke gebeurtenis is gekoppeld aan een bepaalde entiteit in het cluster bijvoorbeeld toepassing, Service, knooppunt, Replica.
* Elke gebeurtenis bevat een set algemene velden: EventInstanceId, EventName en categorie.
* Elke gebeurtenis bevat de velden die de gebeurtenis terug naar de entiteit die is gekoppeld. De gebeurtenis ApplicationCreated hoeft bijvoorbeeld velden die de naam van de toepassing gemaakt.
* Gebeurtenissen zijn gestructureerd zodanig dat ze kunnen worden gebruikt in een aantal hulpprogramma's om u te doen verdere analyse. Bovendien zijn relevante details van een gebeurtenis gedefinieerd als afzonderlijke eigenschappen in plaats van een lange tekenreeks. 
* Gebeurtenissen worden geschreven door verschillende subsystemen weer in Service Fabric worden aangeduid met Source(Task) hieronder. Meer informatie vindt u op deze subsystemen in [Service Fabric-architectuur](service-fabric-architecture.md) en [technische overzicht van Service Fabric](service-fabric-technical-overview.md).

Hier volgt een lijst van deze Service Fabric-gebeurtenissen geordend op entiteit.

## <a name="cluster-events"></a>Clustergebeurtenissen

**Upgrade Clustergebeurtenissen**

Meer informatie over het upgraden van clusters kunnen worden gevonden [hier](service-fabric-cluster-upgrade-windows-server.md).

| Gebeurtenis-id | Name | Categorie | Description |Bron (taak) | Niveau | 
| --- | --- | --- | --- | --- | --- | 
| 29627 | ClusterUpgradeStarted | Upgraden | Een clusterupgrade van een is gestart | CM | Informatief |
| 29628 | ClusterUpgradeCompleted | Upgraden | Een clusterupgrade is voltooid | CM | Informatief | 
| 29629 | ClusterUpgradeRollbackStarted | Upgraden | Een clusterupgrade van een is begonnen met het ongedaan maken  | CM | Waarschuwing | 
| 29630 | ClusterUpgradeRollbackCompleted | Upgraden | Een clusterupgrade van een is voltooid, terug te draaien | CM | Waarschuwing | 
| 29631 | ClusterUpgradeDomainCompleted | Upgraden | Een upgradedomein is voltooid tijdens de upgrade van een cluster upgraden | CM | Informatief | 

## <a name="node-events"></a>Knooppunt-gebeurtenissen

**Levenscyclusgebeurtenissen voor het knooppunt** 

| Gebeurtenis-id | Name | Categorie | Description |Bron (taak) | Niveau |
| --- | --- | ---| --- | --- | --- | 
| 18602 | NodeDeactivateCompleted | StateTransition | Deactivering van een knooppunt is voltooid | FM | Informatief | 
| 18603 | NodeUp | StateTransition | Het cluster is gedetecteerd op dat een knooppunt is gestart | FM | Informatief | 
| 18604 | NodeDown | StateTransition | Het cluster heeft gedetecteerd dat een knooppunt is afgesloten. Tijdens een knooppunt opnieuw wordt opgestart ziet u een gebeurtenis NodeDown, gevolgd door een gebeurtenis NodeUp |  FM | Fout | 
| 18605 | NodeAddedToCluster | StateTransition |  Een nieuw knooppunt is toegevoegd aan het cluster en Service Fabric kunt toepassingen implementeren op dit knooppunt | FM | Informatief | 
| 18606 | NodeRemovedFromCluster | StateTransition |  Een knooppunt is verwijderd uit het cluster. Service Fabric wordt niet meer toepassingen implementeren op dit knooppunt | FM | Informatief | 
| 18607 | NodeDeactivateStarted | StateTransition |  Deactivering van een knooppunt is gestart | FM | Informatief | 
| 25621 | NodeOpenSucceeded | StateTransition |  Een knooppunt is gestart | FabricNode | Informatief | 
| 25622 | NodeOpenFailed | StateTransition |  Een knooppunt kan niet starten en deelnemen aan de ring | FabricNode | Fout | 
| 25624 | NodeClosed | StateTransition |  Een knooppunt is afgesloten | FabricNode | Informatief | 
| 25626 | NodeAborted | StateTransition |  Een knooppunt is ungracefully afgesloten | FabricNode | Fout | 

## <a name="application-events"></a>Toepassingsgebeurtenissen

**Levenscyclus van toepassingsgebeurtenissen**

| Gebeurtenis-id | Name | Categorie | Description |Bron (taak) | Niveau | 
| --- | --- | --- | --- | --- | --- | 
| 29620 | ApplicationCreated | LifeCycle | Een nieuwe toepassing is gemaakt | CM | Informatief | 
| 29625 | ApplicationDeleted | LifeCycle | Een bestaande toepassing is verwijderd | CM | Informatief | 
| 23083 | ApplicationProcessExited | LifeCycle | Een proces in een toepassing is afgesloten | Hosting | Informatief | 

**Upgrade van toepassingsgebeurtenissen**

Meer informatie over upgrades van toepassingen vindt [hier](service-fabric-application-upgrade.md).

| Gebeurtenis-id | Name | Categorie | Description |Bron (taak) | Niveau | 
| --- | --- | ---| --- | --- | --- | 
| 29621 | ApplicationUpgradeStarted | Upgraden | Een upgrade van de toepassing is gestart | CM | Informatief | 
| 29622 | ApplicationUpgradeCompleted | Upgraden | Een upgrade van de toepassing is voltooid | CM | Informatief | 
| 29623 | ApplicationUpgradeRollbackStarted | Upgraden | Een upgrade van de toepassing is begonnen met het ongedaan maken |CM | Waarschuwing | 
| 29624 | ApplicationUpgradeRollbackCompleted | Upgraden | Een upgrade van de toepassing is voltooid, terug te draaien | CM | Waarschuwing | 
| 29626 | ApplicationUpgradeDomainCompleted | Upgraden | Een upgradedomein is voltooid tijdens de upgrade van een toepassing upgraden | CM | Informatief | 

## <a name="service-events"></a>Gebeurtenissen van de service

**Gebeurtenissen van de levenscyclus van service**

| Gebeurtenis-id | Name | Categorie | Description |Bron (taak) | Niveau | 
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | LifeCycle | Een nieuwe service is gemaakt | FM | Informatief | 
| 18658 | ServiceDeleted | LifeCycle | Een bestaande service is verwijderd | FM | Informatief | 

## <a name="partition-events"></a>Partitie-gebeurtenissen

**Partitie verplaatsen gebeurtenissen**

| Gebeurtenis-id | Name | Categorie | Description |Bron (taak) | Niveau | 
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | LifeCycle | Een herconfiguratie van de partitie is voltooid | RA | Informatief | 

## <a name="container-events"></a>Containergebeurtenissen

**Gebeurtenissen in de container levensduur** 

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Een container is gestart | Hosting | Informatief | 1 |
| 23075 | ContainerDeactivated | Een container is gestopt | Hosting | Informatief | 1 |
| 23082 | ContainerExited | Een container is afgesloten: Controleer de vlag UnexpectedTermination | Hosting | Informatief | 1 |

## <a name="health-reports"></a>Statusrapporten

De [Service Fabric-statusmodel](service-fabric-health-introduction.md) biedt een uitgebreide, flexibele en uitbreidbare evalueren en rapportage. Starten van Service Fabric versie 6.2 worden health-gegevens geschreven als Platform-gebeurtenissen voor historische records van de gezondheid van. Om te voorkomen dat het volume van de health-gebeurtenissen lage, schrijven we alleen de volgende Service Fabric-gebeurtenissen:

* Alle `Error` of `Warning` statusrapporten
* `Ok` statusrapporten tijdens overgangen
* Wanneer een `Error` of `Warning` statusgebeurtenis is verlopen. Dit kan worden gebruikt om te bepalen hoe lang een entiteit is niet in orde

**Cluster-rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Een nieuw cluster health rapport is beschikbaar | HM | Informatief | 1 |
| 54437 | ClusterHealthReportExpired | Een bestaand cluster health rapport is verlopen | HM | Informatief | 1 |

**Knooppunt rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Een nieuw knooppunt health rapport is beschikbaar | HM | Informatief | 1 |
| 54432 | NodeHealthReportExpired | Een bestaand knooppunt health rapport is verlopen | HM | Informatief | 1 |

**Toepassing rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Een nieuwe statusrapport van toepassing is gemaakt. Dit is voor niet-geïmplementeerde toepassingen. | HM | Informatief | 1 |
| 54426 | DeployedApplicationNewHealthReport | Een nieuw geïmplementeerde toepassing health rapport is gemaakt | HM | Informatief | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Een nieuw geïmplementeerde service health-rapport is gemaakt | HM | Informatief | 1 |
| 54434 | ApplicationHealthReportExpired | Een bestaande toepassing statusrapport is verlopen | HM | Informatief | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Een bestaande gedistribueerde toepassing statusrapport is verlopen | HM | Informatief | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Een bestaand geïmplementeerde service health-rapport is verlopen | HM | Informatief | 1 |

**Service health rapport-gebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Een nieuwe service health-rapport is gemaakt | HM | Informatief | 1 |
| 54433 | ServiceHealthReportExpired | Een bestaande service-statusrapport is verlopen | HM | Informatief | 1 |

**Partitie rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Een nieuwe statusrapport van de partitie is gemaakt | HM | Informatief | 1 |
| 54431 | PartitionHealthReportExpired | Een bestaande partitie-statusrapport is verlopen | HM | Informatief | 1 |

**Replica rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Een statusrapport stateful replica is gemaakt | HM | Informatief | 1 |
| 54430 | StatelessInstanceNewHealthReport | Een nieuw exemplaar van staatloze health rapport is gemaakt | HM | Informatief | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Een bestaand stateful replica health rapport is verlopen | HM | Informatief | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Een bestaand exemplaar van staatloze health rapport is verlopen | HM | Informatief | 1 |

## <a name="chaos-testing-events"></a>Testen chaos-gebeurtenissen 

**Chaos-sessiegebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Een Chaos testen van de sessie is gestart | Testbaarheid | Informatief | 1 |
| 50023 | ChaosStopped | Een sessie testen Chaos is gestopt | Testbaarheid | Informatief | 1 |

**Gebeurtenissen van chaos-knooppunt**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Een knooppunt is gepland om opnieuw te starten als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50087 | ChaosNodeRestartCompleted | Een knooppunt is opnieuw op te starten als onderdeel van een Chaos sessie testen voltooid | Testbaarheid | Informatief | 1 |

**Chaos-toepassingsgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Een code-pakket opnieuw opstarten is gepland tijdens een sessie testen Chaos | Testbaarheid | Informatief | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Code-pakket opnieuw worden opgestart is tijdens een sessie testen Chaos voltooid | Testbaarheid | Informatief | 1 |

**Chaos-partitie gebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Een primaire partitie is ingesteld om te verplaatsen als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Een secundaire partitie is ingesteld om te verplaatsen als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | De diepgaande analyse van de primaire partitie verplaatsing is beschikbaar | Testbaarheid | Informatief | 1 |

**Chaos replica gebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Replica opnieuw is gepland als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50051 | ChaosReplicaRemovalScheduled | De verwijdering van een replica is gepland als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Een replica verwijderen is voltooid als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |

## <a name="other-events"></a>Andere gebeurtenissen

**Correlatiegebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Een correlatie er is aangetroffen | Testbaarheid | Informatief | 1 |

## <a name="events-prior-to-version-62"></a>Gebeurtenissen voorafgaand aan versie 6.2

Hier volgt een uitgebreide lijst van gebeurtenissen die worden geleverd door de Service Fabric voorafgaand aan versie 6.2.

| Gebeurtenis-id | Name | Bron (taak) | Niveau |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Informatief |
| 25621 | NodeOpenedSuccess | FabricNode | Informatief |
| 25622 | NodeOpenedFailed | FabricNode | Informatief |
| 25623 | NodeClosing | FabricNode | Informatief |
| 25624 | NodeClosed | FabricNode | Informatief |
| 25625 | NodeAborting | FabricNode | Informatief |
| 25626 | NodeAborted | FabricNode | Informatief |
| 29627 | ClusterUpgradeStart | CM | Informatief |
| 29628 | ClusterUpgradeComplete | CM | Informatief |
| 29629 | ClusterUpgradeRollback | CM | Informatief |
| 29630 | ClusterUpgradeRollbackComplete | CM | Informatief |
| 29631 | ClusterUpgradeDomainComplete | CM | Informatief |
| 23074 | ContainerActivated | Hosting | Informatief |
| 23075 | ContainerDeactivated | Hosting | Informatief |
| 29620 | ApplicationCreated | CM | Informatief |
| 29621 | ApplicationUpgradeStart | CM | Informatief |
| 29622 | ApplicationUpgradeComplete | CM | Informatief |
| 29623 | ApplicationUpgradeRollback | CM | Informatief |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Informatief |
| 29625 | ApplicationDeleted | CM | Informatief |
| 29626 | ApplicationUpgradeDomainComplete | CM | Informatief |
| 18566 | ServiceCreated | FM | Informatief |
| 18567 | ServiceDeleted | FM | Informatief |

## <a name="next-steps"></a>Volgende stappen

* Bekijk een overzicht van [diagnostische gegevens in Service Fabric](service-fabric-diagnostics-overview.md)
* Meer informatie over de EventStore in [Eventstore-overzicht van Service Fabric](service-fabric-diagnostics-eventstore.md)
* Wijzigen van uw [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) configuratie om meer logboeken te verzamelen
* [Instellen van Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) om te zien van uw operationele kanaal-Logboeken

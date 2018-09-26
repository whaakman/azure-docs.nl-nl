---
title: Lijst met gebeurtenissen van Azure Service Fabric | Microsoft Docs
description: Uitgebreide lijst van gebeurtenissen die worden geleverd door Azure Service Fabric bewaken-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: da4a779bca806fe6aa392db96eafc6c20f8ddcf6
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182158"
---
# <a name="list-of-service-fabric-events"></a>Lijst met Service Fabric-gebeurtenissen 

Service Fabric wordt aangegeven dat een primaire set Clustergebeurtenissen om te informeren over de status van het cluster als [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md). Deze zijn gebaseerd op acties die door Service Fabric wordt uitgevoerd op uw knooppunten als uw cluster of management beslissingen door een cluster eigenaar/operator. Deze gebeurtenissen kunnen worden geopend door het opvragen van de [EventStore](service-fabric-diagnostics-eventstore.md) in uw cluster, of via het operationele kanaal. Op Windows-machines, is het operationele kanaal ook gekoppeld aan het gebeurtenislogboek - zodat u de Service Fabric-gebeurtenissen in Logboeken kunt zien. 

>[!NOTE]
>Raadpleeg de volgende sectie voor een lijst met Service Fabric-gebeurtenissen voor clusters in versies < 6.2. 

Hier volgt een lijst van alle gebeurtenissen beschikbaar in het platform, gesorteerd op basis van de entiteit die deze worden toegewezen aan.

## <a name="cluster-events"></a>Clustergebeurtenissen

**Upgrade Clustergebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStarted | Een clusterupgrade van een is gestart | CM | Informatief | 1 |
| 29628 | ClusterUpgradeCompleted | Een clusterupgrade is voltooid| CM | Informatief | 1 |
| 29629 | ClusterUpgradeRollbackStarted | Een clusterupgrade van een is begonnen met het ongedaan maken | CM | Informatief | 1 |
| 29630 | ClusterUpgradeRollbackCompleted | Een clusterupgrade van een is voltooid, terug te draaien | CM | Informatief | 1 |
| 29631 | ClusterUpgradeDomainCompleted | De upgrade van een domein is tijdens de upgrade van een cluster voltooid | CM | Informatief | 1 |

## <a name="node-events"></a>Knooppunt-gebeurtenissen

**Levenscyclusgebeurtenissen voor het knooppunt** 

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 18602 | NodeDeactivateCompleted | Deactivering van een knooppunt is voltooid | FM | Informatief | 1 |
| 18603 | NodeUp | Het cluster is gedetecteerd op dat een knooppunt is gestart | FM | Informatief | 1 |
| 18604 | NodeDown | Het cluster is gedetecteerd op dat een knooppunt is afgesloten |  FM | Informatief | 1 |
| 18605 | NodeAddedToCluster | Een nieuw knooppunt is aan het cluster toegevoegd | FM | Informatief | 1 |
| 18606 | NodeRemovedFromCluster | Een knooppunt is uit het cluster verwijderd | FM | Informatief | 1 |
| 18607 | NodeDeactivateStarted | Deactivering van een knooppunt is gestart | FM | Informatief | 1 |
| 25620 | NodeOpening | Een knooppunt wordt gestart. Eerste fase van de levenscyclus van het knooppunt | FabricNode | Informatief | 1 |
| 25621 | NodeOpenSucceeded | Een knooppunt is gestart | FabricNode | Informatief | 1 |
| 25622 | NodeOpenFailed | Een knooppunt kan niet worden gestart | FabricNode | Informatief | 1 |
| 25623 | NodeClosing | Een knooppunt wordt afgesloten. Begin van de laatste fase van de levenscyclus van knooppunt | FabricNode | Informatief | 1 |
| 25624 | NodeClosed | Een knooppunt is afgesloten | FabricNode | Informatief | 1 |
| 25625 | NodeAborting | Een knooppunt wordt gestart om ungracefully af te sluiten | FabricNode | Informatief | 1 |
| 25626 | NodeAborted | Een knooppunt is ungracefully afgesloten | FabricNode | Informatief | 1 |

## <a name="application-events"></a>Toepassingsgebeurtenissen

**Levenscyclus van toepassingsgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 29620 | ApplicationCreated | Een nieuwe toepassing is gemaakt | CM | Informatief | 1 |
| 29625 | ApplicationDeleted | Een bestaande toepassing is verwijderd | CM | Informatief | 1 |
| 23083 | ApplicationProcessExited | Een proces in een toepassing is afgesloten | Die als host fungeert | Informatief | 1 |

**Upgrade van toepassingsgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 29621 | ApplicationUpgradeStarted | Een upgrade van de toepassing is gestart | CM | Informatief | 1 |
| 29622 | ApplicationUpgradeCompleted | Een upgrade van de toepassing is voltooid | CM | Informatief | 1 |
| 29623 | ApplicationUpgradeRollbackStarted | Een upgrade van de toepassing is begonnen met het ongedaan maken |CM | Informatief | 1 |
| 29624 | ApplicationUpgradeRollbackCompleted | Een upgrade van de toepassing is voltooid, terug te draaien | CM | Informatief | 1 |
| 29626 | ApplicationUpgradeDomainCompleted | De upgrade van een domein is tijdens een upgrade van de toepassing voltooid | CM | Informatief | 1 |

## <a name="service-events"></a>Gebeurtenissen van de service

**Gebeurtenissen van de levenscyclus van service**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | Een nieuwe service is gemaakt | FM | Informatief | 1 |
| 18658 | ServiceDeleted | Een bestaande service is verwijderd | FM | Informatief | 1 |

## <a name="partition-events"></a>Partitie-gebeurtenissen

**Partitie verplaatsen gebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Een herconfiguratie van de partitie is voltooid | RA | Informatief | 1 |

## <a name="container-events"></a>Containergebeurtenissen

**Gebeurtenissen in de container levensduur** 

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Een container is gestart | Die als host fungeert | Informatief | 1 |
| 23075 | ContainerDeactivated | Een container is gestopt | Die als host fungeert | Informatief | 1 |
| 23082 | ContainerExited | Een container is afgesloten: Controleer de vlag UnexpectedTermination | Die als host fungeert | Informatief | 1 |

## <a name="health-reports"></a>Statusrapporten

**Cluster-rapport statusgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Een nieuw cluster health rapport is beschikbaar | HM | Informatief | 1 |
| 54437 | ClusterHealthReportExpired | Een bestaand cluster health rapport is verlopen | HM | Informatief | 1 |

**Knooppunt rapport statusgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Een nieuw knooppunt health rapport is beschikbaar | HM | Informatief | 1 |
| 54432 | NodeHealthReportExpired | Een bestaand knooppunt health rapport is verlopen | HM | Informatief | 1 |

**Toepassing rapport statusgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Een nieuwe statusrapport van toepassing is gemaakt. Dit is voor niet-geïmplementeerde toepassingen. | HM | Informatief | 1 |
| 54426 | DeployedApplicationNewHealthReport | Een nieuw geïmplementeerde toepassing health rapport is gemaakt | HM | Informatief | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Een nieuw geïmplementeerde service health-rapport is gemaakt | HM | Informatief | 1 |
| 54434 | ApplicationHealthReportExpired | Een bestaande toepassing statusrapport is verlopen | HM | Informatief | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Een bestaande gedistribueerde toepassing statusrapport is verlopen | HM | Informatief | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Een bestaand geïmplementeerde service health-rapport is verlopen | HM | Informatief | 1 |

**Service health rapport-gebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Een nieuwe service health-rapport is gemaakt | HM | Informatief | 1 |
| 54433 | ServiceHealthReportExpired | Een bestaande service-statusrapport is verlopen | HM | Informatief | 1 |

**Partitie rapport statusgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Een nieuwe statusrapport van de partitie is gemaakt | HM | Informatief | 1 |
| 54431 | PartitionHealthReportExpired | Een bestaande partitie-statusrapport is verlopen | HM | Informatief | 1 |

**Replica rapport statusgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Een statusrapport stateful replica is gemaakt | HM | Informatief | 1 |
| 54430 | StatelessInstanceNewHealthReport | Een nieuw exemplaar van staatloze health rapport is gemaakt | HM | Informatief | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Een bestaand stateful replica health rapport is verlopen | HM | Informatief | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Een bestaand exemplaar van staatloze health rapport is verlopen | HM | Informatief | 1 |

## <a name="chaos-testing-events"></a>Testen chaos-gebeurtenissen 

**Chaos-sessiegebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Een Chaos testen van de sessie is gestart | Testbaarheid | Informatief | 1 |
| 50023 | ChaosStopped | Een sessie testen Chaos is gestopt | Testbaarheid | Informatief | 1 |

**Gebeurtenissen van chaos-knooppunt**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Een knooppunt is gepland om opnieuw te starten als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50087 | ChaosNodeRestartCompleted | Een knooppunt is opnieuw op te starten als onderdeel van een Chaos sessie testen voltooid | Testbaarheid | Informatief | 1 |

**Chaos-toepassingsgebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Een code-pakket opnieuw opstarten is gepland tijdens een sessie testen Chaos | Testbaarheid | Informatief | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Code-pakket opnieuw worden opgestart is tijdens een sessie testen Chaos voltooid | Testbaarheid | Informatief | 1 |

**Chaos-partitie gebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Een primaire partitie is ingesteld om te verplaatsen als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Een secundaire partitie is ingesteld om te verplaatsen als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | De diepgaande analyse van de primaire partitie verplaatsing is beschikbaar | Testbaarheid | Informatief | 1 |

**Chaos replica gebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Replica opnieuw is gepland als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50051 | ChaosReplicaRemovalScheduled | De verwijdering van een replica is gepland als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Een replica verwijderen is voltooid als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |

## <a name="other-events"></a>Andere gebeurtenissen

**Correlatiegebeurtenissen**

| Gebeurtenis-id | Naam | Beschrijving |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Een correlatie is detacted | Testbaarheid | Informatief | 1 |

## <a name="events-prior-to-version-62"></a>Gebeurtenissen voorafgaand aan versie 6.2

Hier volgt een uitgebreide lijst van gebeurtenissen die worden geleverd door de Service Fabric voorafgaand aan versie 6.2.

| Gebeurtenis-id | Naam | Bron (taak) | Niveau |
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
| 23074 | ContainerActivated | Die als host fungeert | Informatief |
| 23075 | ContainerDeactivated | Die als host fungeert | Informatief |
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

* Meer informatie over algemene [gebeurtenisgeneratie op het niveau van het platform](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric
* Wijzigen van uw [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) configuratie om meer logboeken te verzamelen
* [Instellen van Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) om te zien van uw operationele kanaal-Logboeken

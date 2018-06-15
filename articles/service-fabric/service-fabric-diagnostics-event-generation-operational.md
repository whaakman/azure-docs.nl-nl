---
title: Lijst van Azure Service Fabric-gebeurtenis | Microsoft Docs
description: Uitgebreide lijst met gebeurtenissen die worden geleverd door de Azure Service Fabric om u te helpen controleren-clusters.
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
ms.openlocfilehash: 529df0147d2563c62c4a9578e47184bd98b01761
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212956"
---
# <a name="list-of-service-fabric-events"></a>Lijst met Service Fabric-gebeurtenissen 

Beschrijft een primaire set Clustergebeurtenissen zodat u de status van uw cluster als de service Fabric [gebeurtenissen van de Service Fabric](service-fabric-diagnostics-events.md). Deze zijn gebaseerd op de acties die door de Service Fabric worden uitgevoerd op de knooppunten en het cluster of management beslissingen van een cluster eigenaar/operator. Deze gebeurtenissen toegankelijk zijn voor het uitvoeren van query's de [EventStore](service-fabric-diagnostics-eventstore.md) in uw cluster, of via het operationele kanaal. Op Windows-machines, is het operationele kanaal ook verbonden met de EventLog - zodat u de Service Fabric-gebeurtenissen in Logboeken kunt zien. 

>[!NOTE]
>Raadpleeg de volgende sectie voor een lijst van gebeurtenissen voor clusters in versies < 6.2 Service Fabric. 

Hier volgt een lijst van alle gebeurtenissen beschikbaar in het platform, gesorteerd op basis van de entiteit die deze worden toegewezen aan.

## <a name="cluster-events"></a>Clustergebeurtenissen

**Upgrade Clustergebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStartOperational | CM | Informatief | 1 |
| 29628 | ClusterUpgradeCompleteOperational | CM | Informatief | 1 |
| 29629 | ClusterUpgradeRollbackStartOperational | CM | Informatief | 1 |
| 29630 | ClusterUpgradeRollbackCompleteOperational | CM | Informatief | 1 |
| 29631 | ClusterUpgradeDomainCompleteOperational | CM | Informatief | 1 |

**Gebeurtenissen in het health-cluster**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 54428 | ProcessClusterReportOperational | HM | Informatief | 1 |
| 54437 | ExpiredClusterEventOperational | HM | Informatief | 1 |

**Gebeurtenissen van de service chaos**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 50021 | ChaosStartedEvent | Testbaarheid | Informatief | 1 |
| 50023 | ChaosStoppedEvent | Testbaarheid | Informatief | 1 |

## <a name="node-events"></a>Knooppunt-gebeurtenissen

**Knooppunt lifecycle gebeurtenissen** 

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 18602 | DeactivateNodeCompletedOperational | FM | Informatief | 1 |
| 18603 | NodeUpOperational | FM | Informatief | 1 |
| 18604 | NodeDownOperational | FM | Informatief | 1 |
| 18605 | NodeAddedOperational | FM | Informatief | 1 |
| 18606 | NodeRemovedOperational | FM | Informatief | 1 |
| 18607 | DeactivateNodeStartOperational | FM | Informatief | 1 |
| 25620 | NodeOpening | FabricNode | Informatief | 1 |
| 25621 | NodeOpenedSuccess | FabricNode | Informatief | 1 |
| 25622 | NodeOpenedFailed | FabricNode | Informatief | 1 |
| 25623 | NodeClosing | FabricNode | Informatief | 1 |
| 25624 | NodeClosed | FabricNode | Informatief | 1 |
| 25625 | NodeAborting | FabricNode | Informatief | 1 |
| 25626 | NodeAborted | FabricNode | Informatief | 1 |

**Gebeurtenissen in het knooppunt status**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 54423 | ProcessNodeReportOperational | HM | Informatief | 1 |
| 54432 | ExpiredNodeEventOperational | HM | Informatief | 1 |

**Chaos knooppunt gebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 50033 | ChaosRestartNodeFaultScheduledEvent | Testbaarheid | Informatief | 1 |
| 50087 | ChaosRestartNodeFaultCompletedEvent | Testbaarheid | Informatief | 1 |

## <a name="application-events"></a>Toepassingsgebeurtenissen

**De levenscyclus van toepassingsgebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 29620 | ApplicationCreatedOperational | CM | Informatief | 1 |
| 29625 | ApplicationDeletedOperational | CM | Informatief | 1 |
| 23083 | ProcessExitedOperational | Die als host fungeert | Informatief | 1 |

**Upgrade van toepassingsgebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 29621 | ApplicationUpgradeStartOperational | CM | Informatief | 1 |
| 29622 | ApplicationUpgradeCompleteOperational | CM | Informatief | 1 |
| 29623 | ApplicationUpgradeRollbackStartOperational | CM | Informatief | 1 |
| 29624 | ApplicationUpgradeRollbackCompleteOperational | CM | Informatief | 1 |
| 29626 | ApplicationUpgradeDomainCompleteOperational | CM | Informatief | 1 |

**Toepassingsgebeurtenissen health rapport**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 54425 | ProcessApplicationReportOperational | HM | Informatief | 1 |
| 54426 | ProcessDeployedApplicationReportOperational | HM | Informatief | 1 |
| 54427 | ProcessDeployedServicePackageReportOperational | HM | Informatief | 1 |
| 54434 | ExpiredApplicationEventOperational | HM | Informatief | 1 |
| 54435 | ExpiredDeployedApplicationEventOperational | HM | Informatief | 1 |
| 54436 | ExpiredDeployedServicePackageEventOperational | HM | Informatief | 1 |

**Chaos toepassingsgebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 50053 | ChaosRestartCodePackageFaultScheduledEvent | Testbaarheid | Informatief | 1 |
| 50101 | ChaosRestartCodePackageFaultCompletedEvent | Testbaarheid | Informatief | 1 |

## <a name="service-events"></a>Gebeurtenissen van de service

**Gebeurtenissen van de levenscyclus van de service**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 18602 | ServiceCreatedOperational | FM | Informatief | 1 |
| 18658 | ServiceDeletedOperational | FM | Informatief | 1 |

**Gebeurtenissen van de service health rapport**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 54424 | ProcessServiceReportOperational | HM | Informatief | 1 |
| 54433 | ExpiredServiceEventOperational | HM | Informatief | 1 |

## <a name="partition-events"></a>Partitie-gebeurtenissen

**Gebeurtenissen van partitie verplaatsen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 18940 | ReconfigurationCompleted | RA | Informatief | 1 |

**Gebeurtenissen van partitie health rapport**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 54422 | ProcessPartitionReportOperational | HM | Informatief | 1 |
| 54431 | ExpiredPartitionEventOperational | HM | Informatief | 1 |

**Chaos partitie gebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 50069 | ChaosMovePrimaryFaultScheduledEvent | Testbaarheid | Informatief | 1 |
| 50077 | ChaosMoveSecondaryFaultScheduledEvent | Testbaarheid | Informatief | 1 |

**Partitie analysis gebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 65003 | PrimaryMoveAnalysisEvent | Testbaarheid | Informatief | 1 |

## <a name="replica-events"></a>Replica-gebeurtenissen

**Replica health rapport gebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 54429 | ProcessStatefulReplicaReportOperational | HM | Informatief | 1 |
| 54430 | ProcessStatelessInstanceReportOperational | HM | Informatief | 1 |
| 54438 | ExpiredStatefulReplicaEventOperational | HM | Informatief | 1 |
| 54439 | ExpiredStatelessInstanceEventOperational | HM | Informatief | 1 |

**Chaos replica gebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 50047 | ChaosRestartReplicaFaultScheduledEvent | Testbaarheid | Informatief | 1 |
| 50051 | ChaosRemoveReplicaFaultScheduledEvent | Testbaarheid | Informatief | 1 |
| 50093 | ChaosRemoveReplicaFaultCompletedEvent | Testbaarheid | Informatief | 1 |

## <a name="container-events"></a>Gebeurtenissen van de container

**Container lifecycle gebeurtenissen** 

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 23074 | ContainerActivatedOperational | Die als host fungeert | Informatief | 1 |
| 23075 | ContainerDeactivatedOperational | Die als host fungeert | Informatief | 1 |
| 23082 | ContainerExitedOperational | Die als host fungeert | Informatief | 1 |

## <a name="other-events"></a>Andere gebeurtenissen

**Correlatiegebeurtenissen**

| Gebeurtenis-id | Naam | Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- |
| 65011 | CorrelationOperational | Testbaarheid | Informatief | 1 |

## <a name="events-prior-to-version-62"></a>Gebeurtenissen voorafgaand aan versie 6.2

Hier volgt een uitgebreide lijst met gebeurtenissen die worden geleverd door de Service Fabric voorafgaand aan versie 6.2.

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

* Meer informatie over algemene [genereren van gebeurtenissen op het niveau van het platform](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric
* Wijzigen van uw [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) configuratie meer logboeken verzamelen
* [Instellen van Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) om te zien van uw operationele logboeken kanaal

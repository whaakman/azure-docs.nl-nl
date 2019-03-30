---
title: Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl-opdrachten.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: c195d0c4250022102e735cf584370278e354bc41
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666662"
---
# <a name="sfctl"></a>sfctl
Opdrachten voor het beheren van Service Fabric-clusters en entiteiten. Deze versie is compatibel met 6.4 van Service Fabric-runtime.

Opdrachten volgen het patroon zelfstandig naamwoord-bewerking. Zie subgroepen voor meer informatie.

## <a name="subgroups"></a>Subgroepen
|Subgroep|Description|
| --- | --- |
| [Toepassing](service-fabric-sfctl-application.md) | Maken, verwijderen en beheren van toepassingen en de soorten toepassingen. |
| [chaos](service-fabric-sfctl-chaos.md) | Starten, stoppen en rapporteren over de chaos-test-service. |
| [cluster](service-fabric-sfctl-cluster.md) | Selecteer, beheren en gebruiken van Service Fabric-clusters. |
| [Opstellen](service-fabric-sfctl-compose.md) | Maken, verwijderen en beheren van Docker Compose-toepassingen. |
| [container](service-fabric-sfctl-container.md) | Voer container gerelateerde opdrachten op een clusterknooppunt. |
| [is](service-fabric-sfctl-is.md) | Query's uitvoeren en opdrachten worden verzonden naar de infrastructuur-service. |
| [mesh](service-fabric-sfctl-mesh.md) | Verwijderen en beheren van Service Fabric-NET-toepassingen. |
| [node](service-fabric-sfctl-node.md) | Beheren van de knooppunten die een cluster wordt gevormd. |
| [partitie](service-fabric-sfctl-partition.md) | Query's uitvoeren en beheren van partities voor elke service. |
| [property](service-fabric-sfctl-property.md) | Eigenschappen van Store en query's uitvoeren onder de namen van de Service Fabric. |
| [replica](service-fabric-sfctl-replica.md) | De replica's die deel uitmaken van servicepartities beheren. |
| [rpm](service-fabric-sfctl-rpm.md) | Query's uitvoeren en opdrachten worden verzonden naar de manager-service voor herstel. |
| [sa-cluster](service-fabric-sfctl-sa-cluster.md) | Zelfstandige Service Fabric-clusters beheren. |
| [service](service-fabric-sfctl-service.md) | Maken, verwijderen en beheren van de service, servicetypen en service-pakketten. |
| [settings](service-fabric-sfctl-settings.md) | Lokale voor dit exemplaar van sfctl-instellingen configureren. |
| [Store](service-fabric-sfctl-store.md) | Basisopdrachten voor bestandsbeheer niveau bewerkingen uitvoeren op de installatiekopieopslag van het cluster. |

## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Meer informatie over het gebruik van de Service Fabric-CLI met behulp van de [voorbeelden van scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
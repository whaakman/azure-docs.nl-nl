---
title: Azure Service Fabric CLI-sfctl | Microsoft Docs
description: Hierin worden de Service Fabric CLI-sfctl-opdrachten beschreven.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 35b881268ca21a840836c96388a4562a54d17d3b
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035813"
---
# <a name="sfctl"></a>sfctl
Opdrachten voor het beheren van Service Fabric clusters en entiteiten. Deze versie is compatibel met Service Fabric 6,4-runtime.

Opdrachten volgen het patroon van de zelfstandig naam woord. Zie subgroepen voor meer informatie.

## <a name="subgroups"></a>Subgroepen
|Subgroep|Description|
| --- | --- |
| [modules](service-fabric-sfctl-application.md) | Toepassingen en toepassings typen maken, verwijderen en beheren. |
| [chaos](service-fabric-sfctl-chaos.md) | Starten, stoppen en rapporteren op de chaos-test service. |
| [cluster](service-fabric-sfctl-cluster.md) | Service Fabric clusters selecteren, beheren en uitvoeren. |
| [Compose](service-fabric-sfctl-compose.md) | Docker opstellen toepassingen maken, verwijderen en beheren. |
| [container](service-fabric-sfctl-container.md) | Container gerelateerde opdrachten uitvoeren op een cluster knooppunt. |
| [is](service-fabric-sfctl-is.md) | Query's uitvoeren en opdrachten verzenden naar de infrastructuur service. |
| [mesh](service-fabric-sfctl-mesh.md) | Service Fabric-mesh-toepassingen verwijderen en beheren. |
| [subknooppuntsleutels](service-fabric-sfctl-node.md) | De knoop punten beheren die een cluster vormen. |
| [partitie](service-fabric-sfctl-partition.md) | Zoek en beheer partities voor elke service. |
| [eigenschap](service-fabric-sfctl-property.md) | Eigenschappen voor opslaan en opvragen onder Service Fabric namen. |
| [replica](service-fabric-sfctl-replica.md) | De replica's beheren die deel uitmaken van service partities. |
| [rpm](service-fabric-sfctl-rpm.md) | Query's uitvoeren en opdrachten verzenden naar de service reparatie beheer. |
| [sa-cluster](service-fabric-sfctl-sa-cluster.md) | Zelfstandige Service Fabric-clusters beheren. |
| [service](service-fabric-sfctl-service.md) | Services, service typen en service pakketten maken, verwijderen en beheren. |
| [settings](service-fabric-sfctl-settings.md) | Instellingen lokaal configureren voor dit exemplaar van sfctl. |
| [Store](service-fabric-sfctl-store.md) | Voer elementaire bewerkingen op bestands niveau uit in het cluster installatie kopie archief. |

## <a name="next-steps"></a>Volgende stappen
- [Stel](service-fabric-cli.md) de service Fabric cli in.
- Meer informatie over het gebruik van de Service Fabric CLI met behulp van de [voorbeeld scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
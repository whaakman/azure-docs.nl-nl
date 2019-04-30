---
title: Controle en diagnostische gegevens in Azure Service Fabric NET-toepassingen | Microsoft Docs
description: Meer informatie over controle en diagnose van toepassing in Service Fabric net op Azure.
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2018
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 6166808c85bbee9465a8fa12332afe2163027982
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810613"
---
# <a name="monitoring-and-diagnostics"></a>Controle en diagnose
Azure Service Fabric Mesh is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen kunnen implementeren zonder virtuele machines, opslag of netwerken hoeven te beheren. Bewaking en diagnose voor Service Fabric-NET is onderverdeeld in drie belangrijkste typen diagnostische gegevens:

- Toepassingslogboeken - deze worden gedefinieerd als de logboeken van uw toepassingen in containers, op basis van hoe u uw toepassing hebt geïnstrumenteerd (bijvoorbeeld docker-Logboeken)
- Platform-gebeurtenissen - gebeurtenissen uit het Mesh-platform die relevant zijn voor uw containerbewerking, inclusief momenteel container activering en deactivering beëindiging.
- Metrische gegevens over container - resource-gebruik en prestatiemetrieken voor uw containers (docker statistieken)

Dit artikel worden de opties voor controle en diagnostische gegevens voor de meest recente preview-versie beschikbaar.

## <a name="application-logs"></a>Toepassingslogboeken

U kunt uw docker-logboeken van uw geïmplementeerde containers, op basis van per container bekijken. Elke container is in het model van de toepassing Service Fabric net een codepakket in uw toepassing. Als u wilt zien van de bijbehorende logboeken met een codepakket, gebruik de volgende opdracht:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> U kunt de opdracht "az NET service-replica" gebruiken om op te halen van de replicanaam van de. Replica-namen zijn getallen van 0.* verhogen

Hier ziet u hoe dit eruitziet voor het bekijken van de logboeken van de container VotingWeb.Code van de stemtoepassing:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="next-steps"></a>Volgende stappen
Lees het overzicht voor meer informatie over Service Fabric Mesh:
- [Service Fabric Mesh - overzicht](service-fabric-mesh-overview.md)

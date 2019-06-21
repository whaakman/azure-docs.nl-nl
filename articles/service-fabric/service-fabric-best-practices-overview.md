---
title: Aanbevolen procedures voor Azure Service Fabric-toepassing en cluster | Microsoft Docs
description: Aanbevolen procedures voor het beheren van Service Fabric-clusters en toepassingen.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/18/2019
ms.author: pepogors
ms.openlocfilehash: 5fdbd3f15b11e4c3975ca29627d5984382bcf049
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206791"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Aanbevolen procedures voor Azure Service Fabric-toepassing en cluster

In dit artikel bevat koppelingen naar aanbevolen procedures voor het beheren van Azure Service Fabric-toepassingen en -clusters. Het is raadzaam dat u deze procedures voor het optimaliseren van de betrouwbaarheid van uw productieomgeving implementeren. Gebruik een van de [Service Fabric-cluster sjablonen](https://github.com/Azure-Samples/service-fabric-cluster-templates) begint met het ontwerpen van uw oplossing voor productie of bijwerken van de bestaande sjabloon om deze procedures.

## <a name="security"></a>Beveiliging

* [Best practices voor beveiliging](service-fabric-best-practices-security.md)

## <a name="networking"></a>Netwerken

* [Aanbevolen procedures voor netwerken](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>Compute-planning en schalen

* [Aanbevolen procedures voor het schalen van compute](service-fabric-best-practices-capacity-scaling.md)
* [COMPUTE-capaciteitsplanning](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Infrastructure als code

* [Aanbevolen procedures voor het implementeren van infrastructuur als code](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose

* [Aanbevolen procedures voor het clustercontrole en diagnose](service-fabric-best-practices-monitoring.md)

## <a name="application-design"></a>Toepassingsontwerp

* [Aanbevolen procedures voor het ontwerp van toepassingen](service-fabric-best-practices-applications.md)

## <a name="checklist"></a>Controlelijst

Nadat u de procedures die wordt voorgesteld in de vorige secties hebt geïmplementeerd, zorg ervoor dat u de aanbevolen procedures in de controlelijst voor de gereedheid van de productie hebt geïntegreerd:
* [Controlelijst voor Azure Service Fabric productie-gereedheid](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md)
* Service Fabric oplossen: [Troubleshooting Guide](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
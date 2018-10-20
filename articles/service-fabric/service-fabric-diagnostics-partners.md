---
title: Azure Service Fabric-bewaking van Partners | Microsoft Docs
description: Informatie over het bewaken van Azure Service Fabric met oplossingen voor de controle-partner
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: 43b7b2e7b4f4da4939e08a68bc2ac7ea0a2c2a6b
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49459020"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Partners van Azure Service Fabric-bewaking

In dit artikel ziet u hoe een hun Service Fabric-toepassingen, clusters en infrastructuur met een handvol partneroplossingen kunt bewaken. We hebben gewerkt met elk van de onderstaande partners om te integreren aanbiedingen voor Service Fabric maken.

## <a name="dynatrace"></a>Dynatrace

De integratie met Dynatrace biedt veel buiten de box-functies voor het bewaken van uw Service Fabric-clusters. Het Dynatrace OneAgent installeren op uw VMSS-instanties biedt u prestatiemeteritems en een topologie van uw Service Fabric-implementatie op het niveau van de App. Dynatrace is ook een uitstekende keuze voor on-premises bewaking. Bekijk meer van de functies die worden vermeld in de [aankondiging](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) en [instructies](https://www.dynatrace.com/support/help/cloud-platforms/azure/how-do-i-monitor-azure-service-fabric-applications/) Dynatrace inschakelen voor uw cluster. 

## <a name="datadog"></a>Datadog

Datadog heeft een extensie voor VMSS voor zowel Windows als Linux-exemplaren. Met Datadog kunt u Windows-gebeurtenislogboeken verzamelen en waardoor het verzamelen van Service Fabric-platformgebeurtenissen op Windows. Bekijk de instructies voor het verzenden van diagnostische gegevens naar Datadog [hier](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric).

## <a name="appdynamics"></a>AppDynamics

De Service Fabric-integratie met AppDynamics is op het toepassingsniveau van de. U kunt de toepassingstelemetrie naar AppDynamics verzenden door te werken van omgevingsvariabelen en met behulp van de App Dynamics NuGets. Gebruikt u deze [instructies](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric) voor informatie over het integreren van uw .NET Service Fabric-toepassingen met AppDynamics.

## <a name="new-relic"></a>New Relic

New Relic is een andere Application Performance Management-hulpprogramma dat kan worden geïntegreerd met Service Fabric-toepassingen. U kunt de nieuwe Relic NuGet-pakketten installeren en specifieke omgevingsvariabelen in uw manifest-bestanden voor het verzenden van telemetriegegevens van uw toepassing op New Relic toevoegen. Lees deze [instructies](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric) om in te schakelen van New Relic-telemetrie voor uw .NET Service Fabric-toepassingen.

## <a name="elk"></a>ELK 

De ELK-stack is een verzameling van open source-technologieën: Elasticsearch, Logstash en Kibana. Met behulp van deze in combinatie, kunt u verzamelen, opslaan en analyseren van Service Fabric-bewaking en diagnostische gegevens. We hebben een zelfstudie voor hoe u dit doet met native Service Fabric-Java-toepassingen [hier](service-fabric-tutorial-java-elk.md). 


## <a name="next-steps"></a>Volgende stappen

* Krijgen een [overzicht van bewaking en diagnostiek](service-fabric-diagnostics-overview.md) in Service Fabric
* Meer informatie over het [algemene scenario's vaststellen](service-fabric-diagnostics-common-scenarios.md) met onze eerste hulpprogramma's van derden

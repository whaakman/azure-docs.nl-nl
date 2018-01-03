---
title: Aggregatie van Azure Service Fabric-gebeurtenis met Linux Azure Diagnostics | Microsoft Docs
description: Meer informatie over het aggregeren en het verzamelen van gebeurtenissen met LAD voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: b70780b2e1d169aced3412f6fe6d13ad4dab82be
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Gebeurtenis-aggregatie en verzameling op basis van Linux Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Wanneer u een Azure Service Fabric-cluster uitvoert, is het een goed idee de logboeken verzamelen van alle knooppunten in een centrale locatie. De logboeken die in een centrale locatie, helpt u bij het analyseren en oplossen van problemen in het cluster of problemen in de toepassingen en services die worden uitgevoerd in het cluster.

Een manier om te uploaden en verzamelen van Logboeken is het gebruik van de extensie Linux Azure Diagnostics (LAD), waardoor logboeken kunnen uploaden naar Azure Storage en heeft ook de optie om Logboeken te verzenden naar Azure Application Insights of Event Hubs. U kunt ook een extern proces gebruiken om te lezen van de gebeurtenissen uit de opslag en plaats deze in een analyse platform-product, zoals [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) of een andere oplossing voor het parseren van het logboek.

## <a name="log-and-event-sources"></a>Logboekbestand en de gebeurtenis

### <a name="service-fabric-platform-events"></a>Gebeurtenissen voor service Fabric-platform
Service Fabric verzendt enkele out-of-the-box-logboeken via [LTTng](http://lttng.org), met inbegrip van operationele gebeurtenissen of runtime-gebeurtenissen. Deze logboeken worden opgeslagen in de locatie waarin het cluster Resource Manager-sjabloon. Als u wilt ophalen of instellen van de accountdetails van de opslag, zoekt u de tag **AzureTableWinFabETWQueryable** en zoekt u **StoreConnectionString**.

### <a name="application-events"></a>Toepassingsgebeurtenissen
 Gebeurtenissen van uw toepassingen en services code zoals opgegeven door u worden verzonden wanneer het instrumenteren van de software. U kunt een oplossing voor logboekregistratie die op basis van tekst logboekbestanden--bijvoorbeeld LTTng schrijft. Zie voor meer informatie de documentatie van LTTng op de tracering van de toepassing.

[Controle en diagnose van services in de instellingen voor een lokale computer ontwikkeling](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).

## <a name="deploy-the-diagnostics-extension"></a>De extensie voor diagnostische gegevens implementeren
De eerste stap bij het verzamelen van Logboeken is voor het implementeren van de extensie voor diagnostische gegevens over elk van de virtuele machines in het Service Fabric-cluster. De extensie voor diagnostische gegevens verzamelt logboeken op elke virtuele machine en geüpload naar het opslagaccount dat u opgeeft. 

Instellen voor het implementeren van de extensie voor diagnostische gegevens naar de virtuele machines in het cluster als onderdeel van het maken van het cluster, **Diagnostics** naar **op**. Nadat u het cluster hebt gemaakt, kunt u deze instelling niet wijzigen met behulp van de portal, dus u moet de benodigde wijzigingen aanbrengen in de Resource Manager-sjabloon.

Hiermee configureert u de agent LAD voor het controleren van de opgegeven logboekbestanden. Wanneer een nieuwe regel wordt toegevoegd aan het bestand, maakt deze een syslog-vermelding die wordt verzonden naar de opslag (tabel) die u hebt opgegeven.


## <a name="next-steps"></a>Volgende stappen

1. Zie inzicht in meer detail welke gebeurtenissen u bij het oplossen van problemen te onderzoeken [LTTng documentatie](http://lttng.org/docs) en [LAD met behulp van](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
2. [Instellen van de OMS-agent](service-fabric-diagnostics-event-analysis-oms.md) om te helpen de metrische gegevens te verzamelen, Containers die zijn geïmplementeerd op het cluster controleren en visualiseren van uw logboeken 
---
title: Azure Service Fabric Event aggregatie met Linux Azure Diagnostics | Microsoft Docs
description: Meer informatie over aggregeren en het verzamelen van gebeurtenissen via LAD voor controle en diagnose van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: ca6be2b08c9a38813b2c98eef7e5696bd61fd074
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57893030"
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Aggregatie van de gebeurtenis en verzameling met Linux Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Wanneer u een Azure Service Fabric-cluster uitvoert, is het een goed idee om de logboeken te verzamelen van alle knooppunten in een centrale locatie. De logboeken die in een centrale locatie, helpt u bij het analyseren en oplossen van problemen in uw cluster, of problemen in de toepassingen en services die worden uitgevoerd in het cluster.

Eén manier om te uploaden en verzamelen van Logboeken is het gebruik van de Linux Azure Diagnostics (LAD)-extensie, die wordt logboeken geüpload naar Azure Storage en bevat ook een optie om Logboeken te verzenden naar Azure Application Insights of Eventhub. U kunt ook een extern proces gebruiken voor de gebeurtenissen kan lezen uit de opslag en plaats deze in een analyse platform product, zoals [logboeken van Azure Monitor](../log-analytics/log-analytics-service-fabric.md) of een andere oplossing voor het parseren van Logboeken.

## <a name="log-and-event-sources"></a>Logboek- en gebeurtenisgegevens bronnen

### <a name="service-fabric-platform-events"></a>Service Fabric-platformgebeurtenissen
Service Fabric verzendt een paar out-of-the-box-logboeken via [LTTng](https://lttng.org), met inbegrip van operationele gebeurtenissen of de runtime-gebeurtenissen. Deze logboeken worden opgeslagen in de locatie van het cluster Resource Manager-sjabloon bevat. Als u wilt ophalen of instellen van de details van het opslagaccount, zoek de tag **AzureTableWinFabETWQueryable** en zoek naar **StoreConnectionString**.

### <a name="application-events"></a>Toepassingsgebeurtenissen
 Gebeurtenissen die afkomstig zijn van uw toepassingen en services code zoals opgegeven door u bij het instrumenteren van uw software. U kunt een registratie-oplossing die tekstgebaseerde logboekbestanden--bijvoorbeeld LTTng schrijft. Zie voor meer informatie de documentatie LTTng op de tracering van de toepassing.

[Controle en diagnose van services in een lokale machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).

## <a name="deploy-the-diagnostics-extension"></a>Implementeren van de extensie voor diagnostische gegevens
De eerste stap bij het verzamelen van Logboeken wordt de extensie voor diagnostische gegevens op elk van de virtuele machines in de Service Fabric-cluster implementeren. De extensie voor diagnostische gegevens verzamelt logboeken op elke virtuele machine en geüpload naar het opslagaccount dat u opgeeft. 

Instellen voor het implementeren van de extensie voor diagnostische gegevens naar de virtuele machines in het cluster als onderdeel van het maken van clusters, **Diagnostics** naar **op**. Nadat u het cluster hebt gemaakt, kunt u deze instelling niet wijzigen met behulp van de portal, zodat u moet de benodigde wijzigingen aanbrengen in het Resource Manager-sjabloon.

Hiermee configureert u de agent LAD voor het controleren van de opgegeven logboekbestanden. Wanneer een nieuwe regel is toegevoegd aan het bestand, maakt deze een syslog-item dat wordt verzonden naar de opslag (tabel) die u hebt opgegeven.


## <a name="next-steps"></a>Volgende stappen

1. Zie voor meer informatie over in meer detail wat u controleren moet bij het oplossen van problemen met gebeurtenissen, [LTTng documentatie](https://lttng.org/docs) en [LAD met behulp van](https://docs.microsoft.com/azure/virtual-machines/extensions/diagnostics-linux).
2. [Instellen van de Log Analytics-agent](service-fabric-diagnostics-event-analysis-oms.md) om te verzamelen van metrische gegevens, Containers die zijn geïmplementeerd op het cluster controleren en visualiseren van uw logboeken 

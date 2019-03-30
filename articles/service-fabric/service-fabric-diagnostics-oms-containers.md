---
title: Containers in Azure Service Fabric met Azure Monitor-logboeken bewaken | Microsoft Docs
description: Logboeken van Azure Monitor gebruiken voor het bewaken van containers die worden uitgevoerd op Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: d03d68560502821b9c343be983d9f7b5a83ed977
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663181"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Bewaken van containers met Azure Monitor-Logboeken
 
In dit artikel bevat informatie over de stappen voor het instellen van de Azure Monitor logboeken containerbewakingsoplossing om containergebeurtenissen weer te geven. Als u uw cluster instelt voor het verzamelen van containergebeurtenissen, ziet deze [stapsgewijze zelfstudie](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>De containerbewakingsoplossing instellen

> [!NOTE]
> U moet Azure Monitor logboeken instellen voor uw cluster, evenals de Log Analytics-agent is geïmplementeerd op uw knooppunten hebben. Als u dit niet doet, volg de stappen in [instellen van Azure Monitor logboeken](service-fabric-diagnostics-oms-setup.md) en [de Log Analytics-agent toevoegen aan een cluster](service-fabric-diagnostics-oms-agent.md) eerste.

1. Zodra het cluster is ingesteld met Azure Monitor-logboeken en de Log Analytics-agent, moet u uw containers implementeren. Wachten op voor uw containers worden geïmplementeerd voordat u doorgaat met de volgende stap.

2. Zoek in Azure Marketplace, *Container Monitoring Solution* en klik op de **Container Monitoring Solution** resource die weergegeven onder de bewaking en beheer wordt categorie.

    ![Containers-oplossing toevoegen](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Maak de oplossing in dezelfde werkruimte die al is gemaakt voor het cluster. Deze wijziging wordt automatisch geactiveerd voor de agent voor het starten van docker-gegevens verzamelen voor de containers. In ongeveer 15 minuten of zo ziet u de oplossing belicht met binnenkomende logboeken en -statistieken, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Basic Log Analytics-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

De agent kunt u het verzamelen van verschillende container-specifieke logboeken die kunnen worden opgevraagd in Logboeken van Azure Monitor of gebruikt voor het visualiseren van prestatie-indicatoren. Typen logboeken die worden verzameld zijn:

* ContainerInventory: bevat informatie over de containerlocatie, naam en afbeeldingen
* ContainerImageInventory: informatie over geïmplementeerde installatiekopieën, met inbegrip van id's of -grootten
* ContainerLog: logboeken van de specifieke fout, docker-Logboeken (stdout, enzovoort) en andere items
* ContainerServiceLog: docker-daemon-opdrachten die zijn uitgevoerd
* Perf: prestatiemeteritems container zoals cpu, geheugen, netwerkverkeer, schijf-i/o en aangepaste metrische gegevens van de hostmachines



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Containers-oplossing van Azure Monitor-logboeken](../azure-monitor/insights/containers.md).
* Meer informatie over de indeling met containers in Service Fabric - [Service Fabric en containers](service-fabric-containers-overview.md)
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Azure Monitor-Logboeken
* Logboeken voor het instellen van Azure Monitor configureren [automatische waarschuwingen](../log-analytics/log-analytics-alerts.md) regels om te detecteren en diagnostiek
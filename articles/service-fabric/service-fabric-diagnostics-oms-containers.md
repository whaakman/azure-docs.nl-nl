---
title: Containers in Azure Service Fabric met Log Analytics controleren | Microsoft Docs
description: Log Analytics gebruiken voor het bewaken van containers die worden uitgevoerd op Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: d5fd55ec93ce07e30e4c6f123f9be8492581053c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972249"
---
# <a name="monitor-containers-with-log-analytics"></a>Containers bewaken met Log Analytics
 
In dit artikel bevat informatie over de stappen die nodig zijn om in te stellen de containerbewakingsoplossing voor Azure Log Analytics om containergebeurtenissen weer te geven. Als u uw cluster instelt voor het verzamelen van containergebeurtenissen, ziet deze [stapsgewijze zelfstudie](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

## <a name="set-up-the-container-monitoring-solution"></a>De containerbewakingsoplossing instellen

> [!NOTE]
> U moet de Log Analytics instellen voor uw cluster hebt, evenals de Log Analytics-agent is geïmplementeerd op de knooppunten. Als u dit niet doet, volg de stappen in [Log Analytics instellen](service-fabric-diagnostics-oms-setup.md) en [de Log Analytics-agent toevoegen aan een cluster](service-fabric-diagnostics-oms-agent.md) eerste.

1. Zodra het cluster met Log Analytics en de Log Analytics-agent is ingesteld, kunt u uw containers implementeren. Wachten op voor uw containers worden geïmplementeerd voordat u doorgaat met de volgende stap.

2. Zoek in Azure Marketplace, *Container Monitoring Solution* en klik op de **Container Monitoring Solution** resource die weergegeven onder de bewaking en beheer wordt categorie.

    ![Containers-oplossing toevoegen](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Maak de oplossing in dezelfde werkruimte die al is gemaakt voor het cluster. Deze wijziging wordt automatisch geactiveerd voor de agent voor het starten van docker-gegevens verzamelen voor de containers. In ongeveer 15 minuten of zo ziet u de oplossing belicht met binnenkomende logboeken en -statistieken, zoals wordt weergegeven in de onderstaande afbeelding.

    ![Basic Log Analytics-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

De agent kunt u het verzamelen van verschillende container-specifieke logboeken die kunnen worden opgevraagd in Log Analytics of gebruikt voor het visualiseren van prestatie-indicatoren. Typen logboeken die worden verzameld zijn:

* ContainerInventory: bevat informatie over de containerlocatie, naam en afbeeldingen
* ContainerImageInventory: informatie over geïmplementeerde installatiekopieën, met inbegrip van id's of -grootten
* ContainerLog: logboeken van de specifieke fout, docker-Logboeken (stdout, enzovoort) en andere items
* ContainerServiceLog: docker-daemon-opdrachten die zijn uitgevoerd
* Perf: prestatiemeteritems container zoals cpu, geheugen, netwerkverkeer, schijf-i/o en aangepaste metrische gegevens van de hostmachines



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [containerbewakingsoplossing voor Log Analytics](../azure-monitor/insights/containers.md).
* Meer informatie over de indeling met containers in Service Fabric - [Service Fabric en containers](service-fabric-containers-overview.md)
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Log Analytics
* Log Analytics configureren om het instellen van [automatische waarschuwingen](../log-analytics/log-analytics-alerts.md) regels om te detecteren en diagnostiek
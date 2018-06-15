---
title: Containers in Azure Service Fabric met logboekanalyse bewaken | Microsoft Docs
description: Log Analytics gebruiken voor het bewaken van de containers die worden uitgevoerd op Azure Service Fabric-clusters.
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
ms.openlocfilehash: 79d30a47b017379107b63b0006a35534f68c43b9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210773"
---
# <a name="monitor-containers-with-log-analytics"></a>Monitor containers met Log Analytics
 
In dit artikel bevat informatie over de stappen die nodig zijn voor het instellen van de OMS-logboekanalyse-container bewakingsoplossing container gebeurtenissen wilt weergeven. Als u uw cluster instelt voor het verzamelen van gebeurtenissen van de container, ziet u dit [stapsgewijze zelfstudie](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>De container bewakingsoplossing instellen

> [!NOTE]
> U moet logboekanalyse set up voor uw cluster hebben, evenals de OMS-agent die is geïmplementeerd op de knooppunten. Als u dit niet doet, volg de stappen in [logboekanalyse instellen](service-fabric-diagnostics-oms-setup.md) en [de OMS-Agent toevoegen aan een cluster](service-fabric-diagnostics-oms-agent.md) eerste.

1. Zodra het cluster met logboekanalyse en de OMS-Agent is ingesteld, moet u uw containers implementeren. Wachten op uw containers worden geïmplementeerd voordat u doorgaat met de volgende stap.

2. Zoek in Azure Marketplace voor *Container bewaking oplossing* en klik op de **Container bewaking oplossing** resource die wordt weergegeven onder de bewaking en beheer categorie.

    ![Containers-oplossing toevoegen](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Maak de oplossing binnen dezelfde werkruimte die al is gemaakt voor het cluster. Deze wijziging wordt automatisch geactiveerd voor de agent voor het verzamelen van docker-gegevens voor de containers. In ongeveer 15 minuten of doet ziet u de oplossing lichte up met binnenkomende logboeken en statistieken. zoals u in de onderstaande afbeelding.

    ![Basic OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

De agent kunt het verzamelen van verschillende container-specifieke logboeken die kan worden doorzocht in OMS of gebruikt voor het gevisualiseerde prestatie-indicatoren. De logboek-typen die zijn verzameld zijn:

* ContainerInventory: geeft informatie over de containerlocatie, naam en installatiekopieën
* ContainerImageInventory: informatie over geïmplementeerde installatiekopieën, met inbegrip van id's of grootten
* ContainerLog: specifieke foutenlogboeken, docker-Logboeken (stdout, enzovoort) en andere items
* ContainerServiceLog: docker-daemon opdrachten die zijn uitgevoerd
* Prestaties: prestatiemeteritems met inbegrip van container cpu, geheugen, netwerkverkeer, schijf-i/o en aangepaste metrische gegevens van de hostmachines



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [van OMS Containers oplossing](../log-analytics/log-analytics-containers.md).
* Lees meer over container orchestration op Service Fabric - [Service Fabric en containers](service-fabric-containers-overview.md)
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
* Configureren van logboekanalyse voor het instellen van [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) regels om te helpen detecteren en diagnostische gegevens
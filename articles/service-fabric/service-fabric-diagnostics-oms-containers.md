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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 7a775b6d23c144c81650bb3608ee6a117475a9ba
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="monitor-containers-with-log-analytics"></a>Monitor containers met Log Analytics
 
In dit artikel bevat informatie over de stappen die nodig zijn voor het instellen van de bewaking van de container voor uw cluster. Zie voor meer informatie over dit [Containers in Service Fabric Monitoring](service-fabric-diagnostics-event-analysis-oms.md#monitoring-containers). Een stapsgewijze zelfstudie hierover vindt u kunt ook volgen [Monitor Windows containers in Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>De container bewakingsoplossing instellen

> [!NOTE]
> U moet logboekanalyse set up voor uw cluster hebben, evenals de OMS-agent die is geïmplementeerd op de knooppunten. Als u dit niet doet, volg de stappen in [logboekanalyse instellen](service-fabric-diagnostics-oms-setup.md) en [de OMS-Agent toevoegen aan een cluster](service-fabric-diagnostics-oms-agent.md) eerste.

1. Zodra het cluster met logboekanalyse en de OMS-Agent is ingesteld, moet u uw containers implementeren. Wachten op uw containers worden geïmplementeerd voordat u doorgaat met de volgende stap.

2. Zoek in Azure Marketplace voor *Container bewaking oplossing* en klik op de **Container bewaking oplossing** resource die wordt weergegeven onder de bewaking en beheer categorie.

    ![Containers-oplossing toevoegen](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Maak de oplossing binnen dezelfde werkruimte die al is gemaakt voor het cluster. Deze wijziging wordt automatisch geactiveerd voor de agent voor het verzamelen van docker-gegevens voor de containers. In ongeveer 15 minuten of doet ziet u de oplossing lichte up met binnenkomende logboeken en statistieken.

## <a name="next-steps"></a>Volgende stappen
* Lees meer over container orchestration op Service Fabric - [Service Fabric en containers](service-fabric-containers-overview.md)
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
* Configureren van logboekanalyse voor het instellen van [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) regels om te helpen detecteren en diagnostische gegevens
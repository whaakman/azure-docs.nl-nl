---
title: Overzicht van Azure Containers bewaking | Microsoft Docs
description: Dit artikel bevat een overzicht van de verschillende methoden die beschikbaar zijn in Azure voor het bewaken van Containers in Azure sneller inzicht in een clusters status en beschikbaarheid.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2018
ms.author: magoedte
ms.openlocfilehash: 0d511c1f6dfd482e5754741da15b2852ee77c11e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Overzicht van de bewaking van Containers in Azure
Met Azure, kunt u effectief bewaken en beheren van uw werkbelastingen op Azure containers met Kubernetes of Docker geïmplementeerd. Het is belangrijk om te begrijpen hoe containers met meerdere microservice-toepassingen om te kunnen leveren een betrouwbare service op grote schaal en uw plan bewaking uitvoert. Dit artikel bevat een kort overzicht van het beheer en bewakingsmogelijkheden in Azure om u te helpen u elke begrijpen en die van toepassing zijn op basis van uw vereisten.

Met behulp van [Azure controleprogramma container status](monitoring-container-health.md), kunt u de prestaties en de status van de infrastructuur van uw Linux-container in één oogopslag bekijken en snel problemen onderzoeken. De telemetrie is opgeslagen in een werkruimte voor logboekanalyse en geïntegreerd in de Azure portal, waar u kunt verkennen filteren en segment cumulatieve gegevens met dashboards om te controleren op de belasting, prestaties en status.  

Voor containers die worden uitgevoerd buiten de gehoste service voor Azure Kubernetes, de logboekanalyse [oplossing voor Windows en Docker-Container](../log-analytics/log-analytics-containers.md) kunt u weergeven en beheren van uw Windows- en Docker-container-hosts. U kunt vanuit de werkruimte voor logboekanalyse inventarisgegevens, prestaties en gebeurtenissen van de knooppunten en containers weergeven in de omgeving. Vindt u gedetailleerde controle-informatie met opdrachten gebruikt met containers en containers oplossen door weer te geven en gecentraliseerd logboeken zoeken zonder te hoeven voor externe toegang tot Windows- of Docker-hosts.

Als u wilt bereiken holistische of end-to-end bewaking van de toepassing, moet eventuele afhankelijkheden of er een Azure is of on-premises resource, worden gecontroleerd met Azure-Monitor of logboekanalyse.  De toepassingslaag moet worden opgenomen om te kunnen toevoegen van een extra laag voor de health-awareness, zowel op het platform en toepassing niveau met behulp van Application Insights zijn. Op het niveau van het platform is Application Insights-SDK's voor [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/), en [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Voor microservice-toepassingen is ondersteuning voor [Java](../application-insights/app-insights-java-get-started.md), [Node.js](../application-insights/app-insights-nodejs-quick-start.md), [.Net](../application-insights/app-insights-asp-net.md), [.Net Core](../application-insights/app-insights-asp-net-core.md), evenals een aantal andere [talen/frameworks](../application-insights/app-insights-platforms.md). 

Anders problemen gaat niet geïdentificeerd die kan invloed hebben op de beschikbaarheid van de toepassing en serviceniveaudoelen niet voldaan aan.  

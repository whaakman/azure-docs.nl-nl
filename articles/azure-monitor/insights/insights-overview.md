---
title: Overzicht van inzichten in Azure Monitor | Microsoft Docs
description: Inzichten bieden een aangepaste controlemogelijkheden in Azure Monitor voor specifieke toepassingen en services. Dit artikel bevat een korte beschrijving van elk van de inzichten die momenteel beschikbaar zijn.
services: azure-monitor
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: bwren
ms.openlocfilehash: 8cb39a174c570b7019e872d731f49252a9505406
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247227"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Overzicht van inzichten in Azure Monitor
Inzichten bieden een aangepaste bewakingservaring voor specifieke toepassingen en services. Opslaan van gegevens in de [gegevensplatform in de Azure Monitor](../platform/data-platform.md) gebruikmaken van andere functies van Azure Monitor voor analyse en waarschuwingsdoeleinden kan verzamelen van aanvullende gegevens en bieden een unieke gebruikerservaring in Azure portal. Toegang tot inzichten op basis van de **Insights** gedeelte van het menu Azure Monitor in Azure portal.

De volgende secties bevatten een korte beschrijving van de inzichten die momenteel beschikbaar in Azure Monitor zijn. Zie de gedetailleerde documentatie voor meer informatie op elk.

## <a name="application-insights"></a>Application Insights
Application Insights is een uitbreidbare APM-service (Application Performance Management) voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Dit werkt voor toepassingen op een groot aantal verschillende platformen, waaronder .NET, Node.js en Java EE, die wordt gehost on-premises, hybride of openbare Clouds. Ook kan worden geïntegreerd met uw DevOps-proces en bevat verbindingspunten naar verschillende hulpmiddelen voor ontwikkelaars.

Zie [wat is Application Insights?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Azure Monitor voor Containers
Azure Monitor voor containers monitors de prestaties van containerwerkbelastingen geïmplementeerd in een Azure Container Instances of beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Controle van uw containers is kritiek, met name wanneer u een productiecluster op schaal, met meerdere toepassingen uitvoert.

Zie [Azure Monitor voor containers overzicht](../insights/container-insights-overview.md).

![Azure Monitor voor Containers](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Azure Monitor voor resourcegroepen (preview)
Azure Monitor voor resourcegroepen helpt bij het sorteren en onderzoeken van problemen optreden op uw afzonderlijke resources, terwijl context over de status en prestaties van de resourcegroep als geheel.

Zie [resourcegroepen bewaken met Azure Monitor (preview)](../insights/resource-group-insights.md).

![Azure Monitor voor resourcegroepen](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>Azure Monitor voor virtuele machines (preview)
Azure Monitor voor virtuele machines bewaakt uw Azure virtual machines (VM) en virtuele-machineschaalsets op schaal. De service analyseert de prestaties en status van uw Windows- en Linux-VM's en bewaakt hun processen en afhankelijkheden van andere resources en externe processen.

Zie [wat is Azure Monitor voor virtuele machines?](vminsights-overview.md)

![Azure Monitor voor virtuele machines](media/insights-overview/vm-insights.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [gegevensplatform in de Azure Monitor](../platform/data-platform.md) gebruikt door inzicht te krijgen.
* Meer informatie over de verschillende [gegevensbronnen die worden gebruikt door Azure Monitor](../platform/data-sources.md) en de verschillende soorten gegevens die zijn verzameld door elk van de inzichten.

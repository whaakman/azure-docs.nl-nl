---
title: Azure Monitor voor containers regio toewijzingen
description: Dit artikel beschrijft de regio toewijzingen ondersteund tussen Azure-Monitor voor containers, Log Analytics-werkruimte en aangepaste metrische gegevens.
services: azure-monitor
ms.service: azure-monitor
ms.workload: infrastructure-services
author: mgoedtel
ms.author: magoedte
ms.date: 06/26/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 481a2a400be4e983e0a2337a200324061494efa1
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518077"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Regio-toewijzingen ondersteund door Azure Monitor voor containers

 Bij het inschakelen van Azure Monitor voor containers, alleen bepaalde regio's worden ondersteund voor het koppelen van een Log Analytics-werkruimte en een AKS-cluster en het verzamelen van aangepaste metrische gegevens verzonden naar Azure Monitor.

## <a name="log-analytics-workspace-supported-mappings"></a>Log Analytics-werkruimte ondersteund toewijzingen

De AKS-clusterresources of Log Analytics-werkruimte kan zich bevinden in andere regio's, en de volgende tabel ziet u de toewijzingen.

|**AKS-Cluster regio** | **Log Analytics Workspace region** |
|-----------------------|------------------------------------|
|**Afrika** | |
|SouthAfricaNorth |West-Europa |
|SouthAfricaWest |West-Europa |
|**Australië** | |
|AustraliaEast |AustraliaEast |
|AustraliaCentral |AustraliaCentral |
|AustraliaCentral2 |AustraliaCentral |
|AustraliaEast |AustraliaEast |
|**Azië en Stille Oceaan** | |
|EastAsia |EastAsia |
|SoutheastAsia |SoutheastAsia |
|**Brazilië** | |
|BrazilSouth | SouthCentralUS |
|**Canada** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Europa** | |
|FranceCentral |FranceCentral |
|FranceSouth |FranceCentral |
|NorthEurope |NorthEurope |
|UKSouth |UKSouth |
|UKWest |UKSouth |
|West-Europa |West-Europa |
|**India** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japan** | |
|JapanEast |JapanEast |
|JapanWest |JapanEast |
|**Korea** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**VS** | |
|CentralUS |CentralUS|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|WestUS |WestUS |
|WestUS2 |WestUS2 |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|

<sup>1</sup> vanwege beperkingen van de capaciteit, de regio niet beschikbaar is bij het maken van nieuwe resources. Dit omvat een Log Analytics-werkruimte. Bestaande gekoppelde resources in de regio moeten echter nog steeds werken.

## <a name="custom-metrics-supported-regions"></a>Aangepaste metrische gegevens over ondersteunde regio 's

Verzamelen van metrische gegevens van Azure Kubernetes Services (AKS) clusters met knooppunten en schillen worden ondersteund voor het publiceren als aangepaste metrische gegevens in de volgende [Azure-regio's](../platform/metrics-custom-overview.md#supported-regions).

## <a name="next-steps"></a>Volgende stappen

Als u wilt bewaken van uw AKS-cluster, Controleer [het inschakelen van de Azure-Monitor voor containers](container-insights-onboard.md) om te begrijpen van de vereisten en de beschikbare methoden bewaking wilt inschakelen.  
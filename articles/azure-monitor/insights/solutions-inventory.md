---
title: Details van de verzameling gegevens voor de beheeroplossingen in Azure | Microsoft Docs
description: Management-oplossingen in Azure omvatten een verzameling van logica, visualisatie en gegevensverzameling gegevensverzamelingsregels waarmee metrische gegevens gedraaid rondom een specifiek probleemgebied.  In dit artikel bevat een lijst van beschikbare oplossingen van Microsoft en informatie over hun methode en de frequentie van verzamelen van gegevens.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: 38ed7a7fff08003563147d6b8eb9bbc33fb32bc9
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836148"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Details van de verzameling gegevens voor de beheeroplossingen in Azure
In dit artikel bevat een lijst met [beheeroplossingen](solutions.md) beschikbaar van Microsoft met koppelingen naar de gedetailleerde documentatie.  Het bevat ook informatie over hun methode en de frequentie van verzamelen van gegevens in Log Analytics.  Gebruik de informatie in dit artikel om te identificeren van de verschillende oplossingen beschikbaar en om te begrijpen van de stroom en verbinding eisen voor verschillende oplossingen. 

## <a name="list-of-management-solutions"></a>Lijst met oplossingen voor beheer

De volgende tabel bevat de [beheeroplossingen](solutions.md) in Azure die worden geleverd door Microsoft. Een vermelding in de kolom betekent dat de oplossing verzamelt gegevens in Log Analytics met behulp van deze methode.  Als een oplossing geen kolommen geselecteerd heeft, klikt u vervolgens schrijft het rechtstreeks naar Log Analytics van andere Azure-service. Volg de koppeling voor elk criterium de gedetailleerde documentatie voor meer informatie.

Uitleg van de kolommen zijn als volgt:

- **Microsoft monitoring agent** -Agent die wordt gebruikt in Windows en Linux om uit te voeren van Management pack van SCOM en beheer van oplossingen van Azure. In deze configuratie moet is de agent rechtstreeks verbonden met Log Analytics zonder verbinding met een Operations Manager-beheergroep. 
- **Operations Manager** -identieke agent als Microsoft monitoring agent. In deze configuratie heeft [verbonden met een Operations Manager-beheergroep](../../log-analytics/log-analytics-om-agents.md) die verbonden met Log Analytics. 
-  **Azure Storage** -oplossing verzamelt gegevens uit een Azure storage-account. 
- **Operations Manager vereist?** -Een verbonden beheergroep van Operations Manager is vereist voor het verzamelen van gegevens door de MDM-oplossing. 
- **Operations Manager-agent gegevens verzonden via de beheergroep** - als de agent [verbonden met een SCOM-beheergroep](../../log-analytics/log-analytics-om-agents.md), en vervolgens gegevens naar Log Analytics worden verzonden vanaf de beheerserver. In dit geval moet de agent niet rechtstreeks verbinding maken met Log Analytics. Als dit selectievakje niet is geselecteerd, wordt klikt u vervolgens gegevens verzonden vanaf de agent rechtstreeks met Log Analytics, zelfs als de agent is verbonden met een SCOM-beheergroep. Deze moet kunnen communiceren met Log Analytics gebruikt via de [Log Analytics gateway](../../azure-monitor/platform/gateway.md).
- **Verzamelingsfrequentie** -Hiermee geeft u de frequentie dat gegevens worden verzameld door de oplossing voor beheer. 



| **Oplossing voor het beheer** | **Platform** | **Microsoft monitoring agent** | **Operations Manager-agent** | **Azure Storage** | **Operations Manager vereist?** | **Operations Manager-agent gegevens verzonden via de beheergroep** | **Verzamelingsfrequentie** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Activity Log Analytics](../../azure-monitor/platform/collect-activity-logs.md) | Azure | | | | | | op de melding |
| [AD-evaluatie](../../azure-monitor/insights/ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 dagen |
| [AD-replicatiestatus](../../azure-monitor/insights/ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 dagen |
| [Status van agent](solution-agenthealth.md) | Windows en Linux | &#8226; | &#8226; | | | &#8226; | 1 minuut |
| [Ontvang een waarschuwing Management](../../azure-monitor/platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |bij ontvangst |
| [Ontvang een waarschuwing Management](../../azure-monitor/platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 minuut |
| [Ontvang een waarschuwing Management](../../azure-monitor/platform/alert-management-solution.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 minuten |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | N.v.t. |
| [Application Insights-Connector (Preview)](../../azure-monitor/platform/app-insights-connector.md) | Azure | | | |  |  | op de melding |
| [Hybrid Worker voor Automation](../../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | N.v.t. |
| [Azure Application Gateway Analytics](../../azure-monitor/insights/azure-networking-analytics.md) | Azure |  |  |  |  |  | op de melding |
| **Oplossing voor het beheer** | **Platform** | **Microsoft monitoring agent** | **Operations Manager-agent** | **Azure Storage** | **Operations Manager vereist?** | **Operations Manager-agent gegevens verzonden via de beheergroep** | **Verzamelingsfrequentie** |
| [Azure Network Security Group Analytics (afgeschaft)](../../azure-monitor/insights/azure-networking-analytics.md) | Azure |  |  |  |  |  | op de melding |
| [Azure SQL Analytics (Preview)](../../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 minuut |
| [Een back-up maken](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | op de melding |
| [Capaciteit en prestaties (Preview)](../../azure-monitor/insights/capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |bij ontvangst |
| [Tracering wijzigen](../../automation/automation-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |elk uur |
| [Tracering wijzigen](../../automation/automation-change-tracking.md) |Linux |&#8226; | | | | |elk uur |
| [Containers](../../log-analytics/log-analytics-containers.md) | Windows en Linux | &#8226; | &#8226; |  |  |  | 3 minuten |
| [Key Vault-analyse](../../azure-monitor/insights/azure-key-vault.md) |Windows | | | | | |op de melding |
| [Malware-evaluatie](../../security-center/security-center-install-endpoint-protection.md) |Windows |&#8226; |&#8226; | | |&#8226; |elk uur |
| [Netwerkprestatiemeter](../../azure-monitor/insights/network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | Aantal-TCP-handshakes om de vijf seconden, gegevens verzonden om de 3 minuten |
| [Office 365 Analytics (Preview)](solution-office-365.md) |Windows | | | | | |op de melding |
| **Oplossing voor het beheer** | **Platform** | **Microsoft monitoring agent** | **Operations Manager-agent** | **Azure Storage** | **Operations Manager vereist?** | **Operations Manager-agent gegevens verzonden via de beheergroep** | **Verzamelingsfrequentie** |
| [Service Fabric-analyse](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 minuten |
| [Serviceoverzicht](../../azure-monitor/insights/service-map.md) | Windows en Linux | &#8226; | &#8226; |  |  |  | 15 seconden |
| [SQL-evaluatie](../../azure-monitor/insights/sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 dagen |
| [SurfaceHub](../../azure-monitor/insights/surface-hubs.md) |Windows |&#8226; | | | | |bij ontvangst |
| [System Center Operations Manager-evaluatie (Preview)](../../azure-monitor/insights/scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | zeven dagen |
| [Updatebeheer](../../automation/automation-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |ten minste 2 keer per dag en 15 minuten na de installatie van een update |
| [Gereedheid voor upgrade](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 dagen |
| [VMware-bewaking (afgeschaft)](../../azure-monitor/insights/vmware.md) | Linux | &#8226; |  |  |  |  | 3 minuten |
| [Gegevens van wire Data 2.0 (Preview)](../../azure-monitor/insights/wire-data.md) |Windows (2012 R2 / 8.1 of hoger) |&#8226; |&#8226; | | | | 1 minuut |




## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [query's maken](../../azure-monitor/log-query/log-query-overview.md) voor het analyseren van gegevens die zijn verzameld door oplossingen voor beheer.

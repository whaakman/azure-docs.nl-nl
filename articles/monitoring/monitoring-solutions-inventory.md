---
title: Verzameling Gegevensdetails voor oplossingen in Azure | Microsoft Docs
description: Oplossingen in Azure is een verzameling van logica, visualiseren en gegevens overname regels waarmee de metrische gegevens om een bepaald probleemgebied gedraaid.  Dit artikel bevat een lijst met beschikbare oplossingen voor het beheer van Microsoft en informatie over de bijbehorende methode en de frequentie van verzamelen van gegevens.
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
ms.openlocfilehash: 887611f1293e276070df51ac06df51a994161ec1
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112186"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Verzameling Gegevensdetails voor oplossingen in Azure
In dit artikel bevat een lijst met [beheeroplossingen](monitoring-solutions.md) van Microsoft beschikbaar met koppelingen naar hun gedetailleerde documentatie.  Het bevat ook informatie van de methode en de frequentie van verzamelen van gegevens in logboekanalyse.  Gebruik de informatie in dit artikel voor het identificeren van de verschillende beschikbare oplossingen en de gegevens stroom en de verbinding met de vereisten voor andere beheeroplossingen te begrijpen. 

## <a name="list-of-management-solutions"></a>Lijst met oplossingen voor het beheer

De volgende tabel bevat de [beheeroplossingen](monitoring-solutions.md) in Azure door Microsoft geleverd. Een vermelding in de kolom betekent dat de oplossing worden gegevens verzameld in logboekanalyse met behulp van deze methode.  Als een oplossing geen kolommen geselecteerd heeft, klikt u vervolgens schrijft het rechtstreeks naar logboekanalyse van een andere Azure-service. Volg de koppeling voor elk criterium de gedetailleerde documentatie voor meer informatie.

Uitleg van de kolommen zijn als volgt:

- **Microsoft-bewakingsagent** -Agent gebruikt op Windows- en Linux voor het uitvoeren van Management pack van oplossingen voor SCOM en het beheer van Azure. In deze configuratie is de agent rechtstreeks verbonden met logboekanalyse zonder verbinding met een Operations Manager-beheergroep. 
- **Operations Manager** -identiek agent als Microsoft monitoring agent. In deze configuratie hieraan [verbonden met een Operations Manager-beheergroep](../log-analytics/log-analytics-om-agents.md) die verbonden met logboekanalyse. 
-  **Azure Storage** -oplossing verzamelt gegevens uit een Azure storage-account. 
- **Operations Manager is vereist?** -Een verbonden beheergroep van Operations Manager is vereist voor het verzamelen van gegevens door het beheersysteem. 
- **Operations Manager-agent gegevens verzonden via beheergroep** - als de agent is [verbonden met een SCOM-beheergroep](../log-analytics/log-analytics-om-agents.md), en vervolgens de gegevens worden verzonden met logboekanalyse van de beheerserver. In dit geval moet de agent niet rechtstreeks verbinding maken met logboekanalyse. Als dit selectievakje niet is geselecteerd, klikt u vervolgens gegevens verzonden van de agent rechtstreeks met logboekanalyse zelfs als de agent is verbonden met een SCOM-beheergroep. Deze moet kunnen communiceren met logboekanalyse via een [OMS gateway](../log-analytics/log-analytics-oms-gateway.md).
- **Verzameling frequentie** -de frequentie geeft aan dat gegevens worden verzameld door het beheersysteem. 



| **Oplossing voor het beheer** | **Platform** | **Microsoft monitoring agent** | **Operations Manager-agent** | **Azure Storage** | **Operations Manager is vereist?** | **Operations Manager-agent gegevens verzonden via de beheergroep** | **Frequentie van de verzameling** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Activity Log Analytics](../log-analytics/log-analytics-activity.md) | Azure | | | | | | op de melding |
| [AD-evaluatie](../log-analytics/log-analytics-ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 dagen |
| [AD-replicatiestatus](../log-analytics/log-analytics-ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |Er zijn vijf dagen |
| [Status van agent](../operations-management-suite/oms-solution-agenthealth.md) | Windows en Linux | &#8226; | &#8226; | | | &#8226; | 1 minuut |
| [Waarschuwing Management](../log-analytics/log-analytics-solution-alert-management.md) (Nagios) |Linux |&#8226; | | | | |bij ontvangst |
| [Waarschuwing Management](../log-analytics/log-analytics-solution-alert-management.md) (Zabbix) |Linux |&#8226; | | | | |1 minuut |
| [Waarschuwing Management](../log-analytics/log-analytics-solution-alert-management.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 minuten |
| [Azure Site Recovery](../site-recovery/site-recovery-overview.md) | Azure | | | | | | N.v.t. |
| [Application Insights-Connector (Preview)](../log-analytics/log-analytics-app-insights-connector.md) | Azure | | | |  |  | op de melding |
| [Automation Hybrid Worker](../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | N.v.t. |
| [Azure Application Gateway Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | op de melding |
| **Oplossing voor het beheer** | **Platform** | **Microsoft monitoring agent** | **Operations Manager-agent** | **Azure Storage** | **Operations Manager is vereist?** | **Operations Manager-agent gegevens verzonden via de beheergroep** | **Frequentie van de verzameling** |
| [Netwerkbeveiligingsgroep Azure Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | op de melding |
| [Azure SQL Analytics (Preview)](../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 minuut |
| [Een back-up maken](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | op de melding |
| [Capaciteit en prestaties (Preview)](../log-analytics/log-analytics-capacity.md) |Windows |&#8226; |&#8226; | | |&#8226; |bij ontvangst |
| [Tracering wijzigen](../log-analytics/log-analytics-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |elk uur |
| [Tracering wijzigen](../log-analytics/log-analytics-change-tracking.md) |Linux |&#8226; | | | | |elk uur |
| [Containers](../log-analytics/log-analytics-containers.md) | Windows en Linux | &#8226; | &#8226; |  |  |  | 3 minuten |
| [Key Vault-analyse](../log-analytics/log-analytics-azure-key-vault.md) |Windows | | | | | |op de melding |
| [Malware-evaluatie](../log-analytics/log-analytics-malware.md) |Windows |&#8226; |&#8226; | | |&#8226; |elk uur |
| [Netwerkprestatiemeter](../log-analytics/log-analytics-network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP-handshakes om de vijf seconden gegevens verzonden om de 3 minuten |
| [Office 365 Analytics (Preview)](../operations-management-suite/oms-solution-office-365.md) |Windows | | | | | |op de melding |
| **Oplossing voor het beheer** | **Platform** | **Microsoft monitoring agent** | **Operations Manager-agent** | **Azure Storage** | **Operations Manager is vereist?** | **Operations Manager-agent gegevens verzonden via de beheergroep** | **Frequentie van de verzameling** |
| [Service Fabric Analytics (Preview)](../log-analytics/log-analytics-service-fabric.md) |Windows | | |&#8226; | | |5 minuten |
| [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md) | Windows en Linux | &#8226; | &#8226; |  |  |  | 15 seconden |
| [SQL-evaluatie](../log-analytics/log-analytics-sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 dagen |
| [SurfaceHub](../log-analytics/log-analytics-surface-hubs.md) |Windows |&#8226; | | | | |bij ontvangst |
| [System Center Operations Manager Assessment (Preview)](../log-analytics/log-analytics-scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | zeven dagen |
| [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |ten minste 2 keer per dag en 15 minuten nadat u een update installeert |
| [Gereedheid voor upgrade](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 dagen |
| [VMware Monitoring (Preview)](../log-analytics/log-analytics-vmware.md) | Linux | &#8226; |  |  |  |  | 3 minuten |
| [Draadgegevens worden geleverd 2.0 (Preview)](../log-analytics/log-analytics-wire-data.md) |Windows (2012 R2 / 8.1 of hoger) |&#8226; |&#8226; | | | | 1 minuut |




## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [query's maken](../log-analytics/log-analytics-log-searches.md) om gegevens verzameld door oplossingen te analyseren.

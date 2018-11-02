---
title: Overzicht van de Azure monitoring-agents | Microsoft Docs
description: Dit artikel bevat een gedetailleerd overzicht van de Azure-agents beschikbaar die ondersteuning bieden voor bewaking van Azure VM's.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: magoedte
ms.openlocfilehash: 62edeb36fcd56733630edc6fa27c9963f20b0186
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913543"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Overzicht van de Azure-agents voor het bewaken van virtuele Machines van Azure
Microsoft Azure biedt verschillende manieren voor het verzamelen van verschillende typen gegevens van virtuele Machines die worden gehost in Azure of andere cloudproviders met Microsoft Windows en Linux.  Dit artikel voor het beschrijven van de verschillen en mogelijkheden beschikbaar voor elke agent in volgorde voor u om te bepalen welke sjabloon biedt ondersteuning voor het beheer van uw service of in het algemeen controle van vereisten.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Vergelijking van Azure Diagnostics- en Log Analytics-agent
Vandaag nog in Azure zijn er twee soorten beschikbaar voor het bewaken van een Azure-VM - agents de Azure Diagnostics-extensie en de Log Analytics-Agent voor Linux en Windows.  Fundamentele, worden deze agents zijn ontworpen voor metrische gegevens en logboeken verzamelen en doorsturen naar een opslagplaats. Dit is echter waar hun overeenkomsten beëindigen.  

De [Azure Diagnostics-extensie](../monitoring-and-diagnostics/azure-diagnostics.md), die is opgegeven voor Azure Cloud Services omdat deze is algemeen verkrijgbaar in 2010, is een agent die biedt eenvoudige verzamelen van diagnostische gegevens van een Azure IaaS-bron, zoals een virtuele machine, en bewaard deze in een Azure-opslag.  Wanneer in de opslag, u hebt gekozen om weer te geven met een van de verschillende beschikbare hulpprogramma's, zoals [Server Explorer in Visual Studio](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md) en [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).

U kunt kiezen voor het verzamelen van:

* Een vooraf gedefinieerde set besturingssysteem prestatiemeteritems en gebeurtenislogboeken, of u kunt aangeven welke te verzamelen. 
* Alle aanvragen en/of mislukte aanvragen op een IIS-webserver
* .NET-app voor het traceren van logboeken met uitvoergegevens
* Van Event tracing for Windows (ETW)-gebeurtenissen 
* Gebeurtenissen verzamelen van syslog  
* Crashdumps 

Gegevens kunnen ook worden doorgestuurd naar [Application Insights](../application-insights/app-insights-cloudservices.md), [Log Analytics](../log-analytics/log-analytics-overview.md), of op niet-Azure-services met behulp van [Event Hub](../event-hubs/event-hubs-about.md). 

Voor geavanceerde controle waarbij u meer dan verzameld metrische gegevens en een subset van Logboeken moet, hoeft de Log Analytics-agent voor Windows en Linux.  Met deze agent bent u kunt geen gebruik maken van Azure-services zoals Automation en Log Analytics, met inbegrip van de volledige set met functies die ze bieden, om te leveren, uitgebreide beheer van uw Azure-VM's gedurende hun levensduur. Dit omvat:

* [Azure Automation-updatebeheer](../automation/automation-update-management.md) van besturingssysteemupdates
* [Azure Automation Desired State Configuration](../automation/automation-dsc-overview.md) consistente configuratie toestand te handhaven
* Bijhouden van wijzigingen in de configuratie met [Azure Automation wijzigingen bijhouden en inventaris](../automation/automation-change-tracking.md)
* Verzameling van aangepaste logboeken van het besturingssysteem en gehoste toepassingen zoals [FluentD](../log-analytics/log-analytics-data-sources-json.md), [aangepaste logboeken](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL en Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) met Log Analytics
* Azure-services zoals [Application Insights](https://docs.microsoft.com/azure/application-insights/) en [Azure Security Center](https://docs.microsoft.com/azure/security-center/) systeemeigen slaan hun gegevens rechtstreeks in Log Analytics.  

## <a name="next-steps"></a>Volgende stappen

- Zie [gegevens verzamelen van computers in uw omgeving met Log Analytics](../log-analytics/log-analytics-concept-hybrid.md) vereisten en de beschikbare methoden voor het implementeren van de agent op computers in uw datacenter of andere cloudomgeving te bekijken.
- Zie [Gegevens verzamelen over Azure Virtual Machines](../log-analytics/log-analytics-quick-collect-azurevm.md) voor het configureren van het verzamelen van gegevens van Azure VM's. 

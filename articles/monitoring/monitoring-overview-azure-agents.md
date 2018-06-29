---
title: Overzicht van de Azure monitoring-agents | Microsoft Docs
description: Dit artikel bevat een gedetailleerd overzicht van de Azure-agents beschikbaar die ondersteuning bieden voor Azure Virtual machines bewaken.
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
ms.openlocfilehash: a399c3968e5ee1e2d1f6d623a68dbb1e15cef212
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088625"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Overzicht van de Azure-agents voor het bewaken van virtuele Machines in Azure
Microsoft Azure biedt verschillende manieren voor het verzamelen van verschillende soorten gegevens van virtuele Machines die worden gehost in Azure of andere cloudproviders met Microsoft Windows en Linux.  Dit artikel helpt beschrijven de verschillen en mogelijkheden beschikbaar bij elke agent om u te bepalen welke ondersteunt het beheer van uw service of de algemene controle van vereisten.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Vergelijking van diagnostische Azure- en Log Analytics-agent
Vandaag de dag in Azure zijn er twee typen beschikbaar zijn voor het bewaken van een Azure VM - agents de extensie voor diagnostische gegevens van Azure en Log Analytics-Agent voor Linux en Windows.  Fundamenteel, worden deze agents zijn ontworpen voor het verzamelen van Logboeken en metrische gegevens en door te sturen naar een opslagplaats. Dit is echter waar hun overeenkomsten eindigen.  

De [extensie voor diagnostische gegevens van Azure](../monitoring-and-diagnostics/azure-diagnostics.md), die is opgegeven voor Azure Cloud Services nadat werd algemeen beschikbaar is in 2010, is een agent die biedt eenvoudige verzamelen van diagnostische gegevens van een Azure IaaS-bron, zoals een virtuele machine, en deze naar Azure storage bewaard.  Wanneer in de opslag, u hebt gekozen om weer te geven met een van de verschillende beschikbare hulpprogramma's, zoals [Server Explorer in Visual Studio](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md) en [Azure Opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md).

U kunt kiezen om te verzamelen:

* Een vooraf gedefinieerde set prestatiemeteritems besturingssysteem en de gebeurtenislogboeken of u kunt aangeven welke te verzamelen. 
* Alle aanvragen en/of mislukte aanvragen voor een IIS-webserver
* .NET-app uitvoer traceringslogboek
* Gebeurtenistracering voor Windows (ETW)-gebeurtenissen 
* Gebeurtenissen verzamelen van syslog  
* Crashdumps 

Gegevens kunnen ook worden doorgestuurd naar [Application Insights](../application-insights/app-insights-cloudservices.md), [logboekanalyse](../log-analytics/log-analytics-overview.md), of op niet-Azure-services met behulp [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md). 

Voor geavanceerde controle hier u meer dan verzameld metrische gegevens en een subset van Logboeken moet, is de agent Log Analytics voor Windows en Linux vereist.  Met deze agent bent u gebruikmaken van Azure-services zoals Automation en Log Analytics, met inbegrip van de volledige set van functies die ze bieden, uitgebreid beheer van uw Azure VM's gedurende hun levensduur leveren. Dit omvat:

* [Azure Automation-updatebeheer](../automation/automation-update-management.md) van updates van besturingssysteem
* [Azure Automation gewenst State Configuration](../automation/automation-dsc-overview.md) consistente configuratie toestand te handhaven
* Bijhouden van wijzigingen in de configuratie met [bijhouden van Azure Automation en inventarisatie](../automation/automation-change-tracking.md)
* Verzameling van aangepaste logboeken van het besturingssysteem en gehoste toepassingen zoals [FluentD](../log-analytics/log-analytics-data-sources-json.md), [aangepaste logboeken](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL en Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) met Log Analytics
* Azure services, zoals [Application Insights](https://docs.microsoft.com/azure/application-insights/) en [Azure Security Center](https://docs.microsoft.com/azure/security-center/) systeemeigen hun gegevens rechtstreeks in logboekanalyse opslaan.  

## <a name="next-steps"></a>Volgende stappen

- Zie [verzamelen gegevens van computers in uw omgeving met logboekanalyse](../log-analytics/log-analytics-concept-hybrid.md) om te controleren van vereisten en de beschikbare methoden voor het implementeren van de agent op computers in uw datacenter of andere cloudomgeving.
- Zie [Gegevens verzamelen over Azure Virtual Machines](../log-analytics/log-analytics-quick-collect-azurevm.md) voor het configureren van het verzamelen van gegevens van Azure VM's. 
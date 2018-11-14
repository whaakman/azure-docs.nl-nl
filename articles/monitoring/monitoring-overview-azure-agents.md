---
title: Overzicht van de Azure monitoring-agents | Microsoft Docs
description: Dit artikel bevat een gedetailleerd overzicht van de Azure-agents beschikbaar die ondersteuning controleren van virtuele machines die worden gehost in Azure of in een hybride omgeving.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: ce6f40e580595e4f3fbf0519aa1ba8be0355ded1
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621902"
---
# <a name="overview-of-the-azure-monitoring-agents"></a>Overzicht van de Azure monitoring-agents 
Microsoft Azure biedt verschillende manieren voor het verzamelen van verschillende typen gegevens van virtuele Machines die worden gehost in Azure of andere cloudproviders met Microsoft Windows en Linux. Dit artikel voor het beschrijven van de verschillen en mogelijkheden beschikbaar voor elke agent in volgorde voor u om te bepalen welke sjabloon biedt ondersteuning voor uw IT-servicebeheer of algemene vereisten voor bewaking.  

## <a name="comparing-agents"></a>Vergelijking van agents
Vandaag nog in Azure zijn er drie soorten beschikbaar voor het bewaken van een Azure-VM - agents de Azure Diagnostics-extensie, de agent voor afhankelijkheden en de Log Analytics-Agent voor Linux en Windows.  Fundamentele, de afhankelijkheid van Azure-extensie en de Log Analytics-agents zijn ontworpen voor metrische gegevens en logboeken verzamelen en doorsturen naar een opslagplaats. Dit is echter waar hun overeenkomsten beëindigen.  

### <a name="azure-diagnostic-extension"></a>Diagnostische Azure-extensie
De [Azure Diagnostics-extensie](../monitoring-and-diagnostics/azure-diagnostics.md) (vaak aangeduid als de extensie Windows Azure diagnostische (WAD) of de Linux Azure diagnostische (LAD)), die is opgegeven voor Azure Cloud Services omdat deze algemeen beschikbaar in 2010 is is een agent die biedt eenvoudige verzamelen van diagnostische gegevens van een Azure compute-resource, zoals een virtuele machine en deze bewaard in Azure storage. Wanneer in de opslag, u hebt gekozen om weer te geven met een van de verschillende beschikbare hulpprogramma's, zoals [Server Explorer in Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) en [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).

U kunt kiezen voor het verzamelen van:

* Een vooraf gedefinieerde set besturingssysteem prestatiemeteritems en gebeurtenislogboeken, of u kunt aangeven welke te verzamelen. 
* Alle aanvragen en/of mislukte aanvragen op een IIS-webserver
* .NET-app voor het traceren van logboeken met uitvoergegevens
* Van Event tracing for Windows (ETW)-gebeurtenissen 
* Gebeurtenissen verzamelen van syslog  
* Crashdumps 

De Azure Diagnostics-agent moet worden gebruikt wanneer:

* U wilt archiveren, logboeken en metrische gegevens naar Azure storage
* Bewakingsgegevens worden geïntegreerd met hulpprogramma's van derden. Deze hulpprogramma's gebruiken een aantal methoden, waaronder het uitvoeren van query's de storage-account, doorgestuurd naar [Event Hubs](../event-hubs/event-hubs-about.md), of query's uitvoeren met de [REST-API voor Azure-bewaking](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md)
* Gegevens uploaden naar Azure Monitor om te grafieken met metrische gegevens maken in Azure portal of maak bijna-realtime [metrische waarschuwingen](../monitoring-and-diagnostics/alert-metric-overview.md). 
* De virtuele machine-schaalsets automatisch schalen en klassieke Cloudservices gebaseerd op OS metrische gegevens voor gasten.
* VM-opstartproblemen met onderzoeken [diagnostische gegevens over opstarten](../virtual-machines/troubleshooting/boot-diagnostics.md).
* Inzicht in hoe uw toepassingen worden uitgevoerd en identificeert u proactief problemen die betrekking hebben op deze met [Application Insights](../azure-monitor/overview.md).
* Log Analytics om te importeren van metrische gegevens en logboekgegevens die worden verzameld van Cloud Services, klassieke virtuele machines, configureren en Service Fabric-knooppunten die zijn opgeslagen in Azure storage-account.

### <a name="log-analytics-agent"></a>Log Analytics-agent
Voor geavanceerde controle waarbij u meer dan verzameld metrische gegevens en een subset van Logboeken moet, hoeft de Log Analytics-agent voor Windows en Linux. De Log Analytics-agent is ontwikkeld voor het beheer van uitgebreide zowel on-premises fysieke als virtuele machines, computers die worden bewaakt door System Center Operations Manager en VM's in die wordt gehost in andere clouds. De Windows- en Linux-agents verbinden met een werkruimte voor logboekanalyse voor het verzamelen van zowel oplossing op basis van gegevens als aangepaste gegevensbronnen die u configureert.

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

De Log Analytics-agent moet worden gebruikt wanneer:

* Hebt u de machines die on-premises of andere cloudomgeving worden uitgevoerd
* Met behulp van een van de Azure Monitor bewakingsoplossingen zoals [Azure Monitor voor virtuele machines](../monitoring/monitoring-vminsights-overview.md?toc=%2fazure%2fmonitoring%2ftoc.json), [Azure Monitor voor containers](../monitoring/monitoring-container-insights-overview.md?toc=%2fazure%2fmonitoring%2ftoc.json), enzovoort.  
* Met een van de Azure management-services zoals [Azure Security Center](../security-center/security-center-intro.md), [Azure Automation](../automation/automation-intro.md), enzovoort.
* Logboek-en/of metrische gegevens uploaden naar Azure Monitor.

Voorheen verschillende Azure-services zijn gebundeld in de *Operations Management Suite*, en als gevolg hiervan de Log Analytics-agent wordt gedeeld door services zoals Azure Security Center en Azure Automation.  Dit omvat de volledige set met functies die ze bieden, leveren van uitgebreide beheer van uw Azure-VM's gedurende hun levensduur. Dit omvat:

* [Azure Automation-updatebeheer](../automation/automation-update-management.md) van besturingssysteemupdates.
* [Azure Automation Desired State Configuration](../automation/automation-dsc-overview.md) consistente configuratie toestand te handhaven.
* Bijhouden van wijzigingen in de configuratie met [Azure Automation wijzigingen bijhouden en inventaris](../automation/automation-change-tracking.md).
* Verzameling van aangepaste logboeken van het besturingssysteem en gehoste toepassingen zoals [FluentD](../log-analytics/log-analytics-data-sources-json.md), [aangepaste logboeken](../log-analytics/log-analytics-data-sources-custom-logs.md), en [MySQL en Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) met Log Analytics.
* Azure-services zoals [Application Insights](https://docs.microsoft.com/azure/application-insights/) en [Azure Security Center](https://docs.microsoft.com/azure/security-center/) systeemeigen slaan hun gegevens rechtstreeks in Log Analytics.  

### <a name="dependency-agent"></a>Agent voor afhankelijkheden
De agent voor afhankelijkheden is ontwikkeld als onderdeel van de oplossing Serviceoverzicht die oorspronkelijk is ontwikkeld, extern van Microsoft. [Serviceoverzicht](../monitoring/monitoring-service-map.md) en [Azure Monitor voor virtuele machines](monitoring-vminsights-overview.md) vereist een Agent voor afhankelijkheden in Windows en Linux virtuele machines en deze kan worden geïntegreerd met de Log Analytics-agent voor gedetecteerde verzamelt gegevens over processen die worden uitgevoerd op de virtuele machine en extern Procesafhankelijkheden. Het deze gegevens worden opgeslagen in Log Analytics en visualiseert de gedetecteerde onderling verbonden onderdelen.

Mogelijk moet u een combinatie van deze agenten voor het bewaken van uw virtuele machine. De agents kunnen worden geïnstalleerd naast elkaar als Azure-extensies, maar op Linux, de Log Analytics-agent *moet* worden geïnstalleerd eerste anders de installatie mislukken. 

## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van de Log Analytics-agent](../log-analytics/log-analytics-agent-overview.md) vereisten en ondersteunde methoden voor het implementeren van de agent op de machines die worden gehost in Azure, in uw datacenter of andere cloudomgeving te bekijken.


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
ms.date: 11/14/2018
ms.author: magoedte
ms.openlocfilehash: 4fc38d033181b60f105fd016a3fa968acd9d9177
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51828353"
---
# <a name="overview-of-the-azure-monitoring-agents"></a>Overzicht van de Azure monitoring-agents 
Microsoft Azure biedt verschillende manieren voor het verzamelen van verschillende typen gegevens van virtuele machines met Microsoft Windows en Linux die worden gehost in Azure en uw datacenter of andere cloudproviders. De drie typen van de agents is beschikbaar voor het bewaken van een virtuele machine zijn:

* Azure Diagnostics-extensies
* Log Analytics-Agent voor Linux en Windows
* Agent voor afhankelijkheden

Dit artikel beschrijft de verschillen tussen deze en de bijbehorende mogelijkheden in volgorde voor u om te bepalen welke sjabloon biedt ondersteuning voor uw IT-servicebeheer of algemene controlevereisten.  

## <a name="azure-diagnostic-extension"></a>Diagnostische Azure-extensie
De [Azure Diagnostics-extensie](../../monitoring-and-diagnostics/azure-diagnostics.md) (vaak aangeduid als de extensie Windows Azure diagnostische (WAD) of de Linux Azure diagnostische (LAD)), die is opgegeven voor Azure Cloud Services omdat deze algemeen beschikbaar in 2010 is is een agent die biedt eenvoudige verzamelen van diagnostische gegevens van een Azure compute-resource, zoals een virtuele machine en deze bewaard in Azure storage. Wanneer in de opslag, u hebt gekozen om weer te geven met een van de verschillende beschikbare hulpprogramma's, zoals [Server Explorer in Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) en [Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md).

U kunt kiezen voor het verzamelen van:

* Een vooraf gedefinieerde set besturingssysteem prestatiemeteritems en gebeurtenislogboeken, of u kunt aangeven welke te verzamelen. 
* Alle aanvragen en/of mislukte aanvragen op een IIS-webserver
* .NET-app voor het traceren van logboeken met uitvoergegevens
* Van Event tracing for Windows (ETW)-gebeurtenissen 
* Gebeurtenissen verzamelen van syslog  
* Crashdumps 

De Azure Diagnostics-agent moet worden gebruikt wanneer u wilt:

* Archiveren van Logboeken en metrische gegevens naar Azure storage
* Bewakingsgegevens worden geïntegreerd met hulpprogramma's van derden. Deze hulpprogramma's gebruiken een aantal methoden, waaronder het uitvoeren van query's de storage-account, doorgestuurd naar [Event Hubs](../../event-hubs/event-hubs-about.md), of query's uitvoeren met de [REST-API voor Azure-bewaking](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md)
* Gegevens uploaden naar Azure Monitor om te grafieken met metrische gegevens maken in Azure portal of maak bijna-realtime [metrische waarschuwingen](../../monitoring-and-diagnostics/alert-metric-overview.md). 
* De virtuele machine-schaalsets automatisch schalen en klassieke Cloudservices gebaseerd op OS metrische gegevens voor gasten.
* VM-opstartproblemen met onderzoeken [diagnostische gegevens over opstarten](../../virtual-machines/troubleshooting/boot-diagnostics.md).
* Inzicht in hoe uw toepassingen worden uitgevoerd en identificeert u proactief problemen die betrekking hebben op deze met [Application Insights](../../azure-monitor/overview.md).
* Log Analytics om te importeren van metrische gegevens en logboekgegevens die worden verzameld van Cloud Services, klassieke virtuele machines, configureren en Service Fabric-knooppunten die zijn opgeslagen in Azure storage-account.

## <a name="log-analytics-agent"></a>Log Analytics-agent
Voor geavanceerde controle waar u wilt meer dan metrische gegevens worden verzameld en een subset van Logboeken, de Log Analytics-agent voor Windows en Linux is vereist. De Log Analytics-agent is ontwikkeld voor het beheer van uitgebreide zowel on-premises fysieke als virtuele machines, computers die worden bewaakt door System Center Operations Manager en VM's in die wordt gehost in andere clouds. De Windows- en Linux-agents verbinden met een werkruimte voor logboekanalyse voor het verzamelen van zowel oplossing op basis van gegevens als aangepaste gegevensbronnen die u configureert.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

De Log Analytics-agent moet worden gebruikt wanneer u wilt:

* Verzamelen van gegevens uit een groot aantal bronnen, zowel binnen Azure, andere cloudproviders en on-premises bronnen. 
* Met behulp van een van de Azure Monitor bewakingsoplossingen zoals [Azure Monitor voor virtuele machines](../insights/vminsights-overview.md), [Azure Monitor voor containers](../insights/container-insights-overview.md), enzovoort.  
* Gebruik een van de Azure management-services zoals [Azure Security Center](../../security-center/security-center-intro.md), [Azure Automation](../../automation/automation-intro.md), enzovoort.

Voorheen verschillende Azure-services zijn gebundeld in de *Operations Management Suite*, en als gevolg hiervan de Log Analytics-agent wordt gedeeld door services zoals Azure Security Center en Azure Automation.  Dit omvat de volledige set met functies die ze bieden, leveren van uitgebreide beheer van uw Azure-VM's gedurende hun levensduur.  Enkele voorbeelden hiervan zijn:

* [Azure Automation-updatebeheer](../../automation/automation-update-management.md) van besturingssysteemupdates.
* [Azure Automation Desired State Configuration](../../automation/automation-dsc-overview.md) consistente configuratie toestand te handhaven.
* Bijhouden van wijzigingen in de configuratie met [Azure Automation wijzigingen bijhouden en inventaris](../../automation/automation-change-tracking.md).
* Azure-services zoals [Application Insights](https://docs.microsoft.com/azure/application-insights/) en [Azure Security Center](https://docs.microsoft.com/azure/security-center/), welke systeemeigen slaan hun gegevens rechtstreeks in Log Analytics.  

## <a name="dependency-agent"></a>Agent voor afhankelijkheden
De agent voor afhankelijkheden is ontwikkeld als onderdeel van de oplossing Serviceoverzicht die oorspronkelijk is ontwikkeld, extern van Microsoft. [Serviceoverzicht](../insights/service-map.md) en [Azure Monitor voor virtuele machines](../insights/vminsights-overview.md) vereist een Agent voor afhankelijkheden in Windows en Linux virtuele machines en deze kan worden geïntegreerd met de Log Analytics-agent voor gedetecteerde verzamelt gegevens over processen die worden uitgevoerd op de virtuele machine en extern Procesafhankelijkheden. Het deze gegevens worden opgeslagen in Log Analytics en visualiseert de gedetecteerde onderling verbonden onderdelen.

Mogelijk moet u een combinatie van deze agenten voor het bewaken van uw virtuele machine. De agents kunnen worden geïnstalleerd naast elkaar als Azure-extensies, maar op Linux, de Log Analytics-agent *moet* worden geïnstalleerd eerste anders de installatie mislukken. 

## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van de Log Analytics-agent](../../log-analytics/log-analytics-agent-overview.md) vereisten en ondersteunde methoden voor het implementeren van de agent op de machines die worden gehost in Azure, in uw datacenter of andere cloudomgeving te bekijken.


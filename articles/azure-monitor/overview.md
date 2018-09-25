---
title: Overzicht van Azure Monitor | Microsoft Docs
description: Overzicht van Microsoft-services en -functionaliteiten die bijdragen aan een complete bewakingsstrategie voor uw Azure-services en -toepassingen.
author: bwren
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2018
ms.author: bwren
ms.openlocfilehash: 97cc6d6e922ab0bedd729eddca754ae016f041bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958013"
---
# <a name="azure-monitor-overview"></a>Overzicht van Azure Monitor

Azure Monitor maximaliseert de beschikbaarheid en prestaties van uw toepassingen door het leveren van een uitgebreide oplossing voor het verzamelen, analyseren en uitvoeren van telemetrie van uw cloud en on-premises omgevingen. Het helpt u begrijpen hoe uw toepassingen worden uitgevoerd en proactief identificeert die betrekking hebben op deze en de resources die ze afhankelijk van zijn problemen.

> [!VIDEO https://www.youtube.com/embed/_hGff5bVtkM]

## <a name="overview"></a>Overzicht
Het volgende diagram biedt een weergave op hoog niveau van Azure Monitor. In het midden van het diagram worden de gegevensarchieven voor metrische gegevens en logboeken die de twee fundamentele typen gegevens gebruikt door Azure Monitor zijn. Aan de linkerkant zijn de [bronnen die telemetrie van verschillende verzamelen resources bewaakt](../monitoring/monitoring-data-sources.md) in en vul de [gegevensarchieven](../monitoring/monitoring-data-collection.md). Aan de rechterkant worden de verschillende functies die Azure Monitor met deze verzamelde gegevens, zoals analyse, uitvoert waarschuwingen en streaming naar externe systemen.


![Overzicht van Azure Monitor](media/overview/overview.png)


## <a name="monitoring-data-platform"></a>Bewaking van data-platform
Alle gegevens die zijn verzameld door Azure Monitor in een van twee fundamentele typen, past [metrische gegevens en logboeken](../monitoring/monitoring-data-collection.md). [Metrische gegevens](../monitoring/monitoring-data-collection.md#metrics) zijn numerieke waarden die een bepaald aspect van een systeem op een bepaald punt in tijd beschrijven. Ze zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen. [Logboeken](../monitoring/monitoring-data-collection.md#logs) bevatten verschillende soorten gegevens die zijn ingedeeld in records met verschillende sets van eigenschappen voor elk type. Telemetrie, zoals gebeurtenissen en traceringen worden opgeslagen als Logboeken ook naar prestatiegegevens zodat deze kan allemaal worden gecombineerd voor analyse.

Voor veel Azure-resources ziet u de gegevens die zijn verzameld door met de rechtermuisknop op de pagina overzicht in Azure portal Azure Monitor. Een overzicht van elke virtuele machine bijvoorbeeld hebben en ziet u verschillende grafieken met metrische gegevens voor prestaties weergeven. Klik op een of meer grafieken om te openen van de gegevens in [Metric explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md) in Azure portal, kunt u de waarden van meerdere metrische gegevens gedurende een periode van grafiek.  U kunt de grafieken interactief weergeven of vastmaken aan een dashboard om deze met andere visualisaties weer te geven.

![Metrische gegevens](media/overview/metrics.png)

Logboekgegevens die zijn verzameld door Azure Monitor is opgeslagen in Log Analytics, waaronder een [uitgebreide querytaal](../log-analytics/log-analytics-queries.md) snel ophalen, samenvoegen en analyseren van verzamelde gegevens.  U kunt maken en query's testen met de [pagina logboekanalyse](../log-analytics/log-analytics-log-search-portals.md) in Azure portal en de gegevens met behulp van deze hulpprogramma's rechtstreeks analyseren of query's opslaan voor gebruik met [visualisaties](visualizations.md) of [ waarschuwingsregels](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

De querytaal van Log Analytics is geschikt voor eenvoudige logboeken-query's, maar bevat ook geavanceerde functies zoals aggregaties, joins en slimme analyse. U kunt snel leren over de query-taal met [meerdere lessen](../log-analytics/query-language/get-started-queries.md) die beschikbaar zijn.  Er worden specifieke richtlijnen gegeven voor gebruikers die al bekend zijn met [SQL](../log-analytics/query-language/sql-cheatsheet.md) en [Splunk](../log-analytics/query-language/splunk-cheatsheet.md).

![Logboeken](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>Welke gegevens worden verzameld door Azure Monitor?
Log Analytics kan gegevens verzamelen uit een groot aantal bronnen. U kunt zien van de bewakingsgegevens voor uw toepassingen in lagen, variërend van uw toepassing, alle besturingssysteem en services afhankelijk, omlaag naar het platform zelf wordt geboden. Azure Monitor verzamelt gegevens van elk van de volgende lagen:

- **Gegevens voor toepassingsbewaking**: gegevens over de prestaties en functionaliteit van de code u hebt geschreven, ongeacht het platform.
- **Gastbesturingssysteem bewakingsgegevens**: gegevens over het besturingssysteem waarop uw toepassing wordt uitgevoerd. Dit kan worden uitgevoerd in Azure, een andere cloud of on-premises. 
- **Azure-resource bewakingsgegevens**: gegevens over de werking van een Azure-resource.
- **Azure-abonnement bewakingsgegevens**: gegevens over de werking en het beheer van een Azure-abonnement, evenals gegevens over de status en de werking van Azure zelf. 
- **Azure-tenant bewakingsgegevens**: gegevens over de werking van op tenantniveau-Azure-services, zoals Azure Active Directory.

Als u een Azure-abonnement en begin toevoegen van resources, zoals virtuele machines en web-apps maakt, wordt Azure Monitor gestart verzamelen van gegevens.  [Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) opnemen wanneer resources worden gemaakt of gewijzigd. [Metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) uitgelegd hoe de resource wordt uitgevoerd en de resources die voor deze wordt gebruikt. 

Uitbreiden van de gegevens die u verzamelt in de huidige bewerking van de resources door [diagnostische gegevens inschakelen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) en [toevoegen van een agent](../log-analytics/log-analytics-agent-windows.md) om resources te berekenen. Hierdoor wordt het verzamelen van telemetrie voor de interne werking van de resource en kunt u het configureren van verschillende [gegevensbronnen](../log-analytics/log-analytics-data-sources.md) voor het verzamelen van Logboeken en metrische gegevens van Windows en Linux Gast-besturingssysteem. 

[Een instrumentatiepakket toevoegen aan uw toepassing](../application-insights/app-insights-azure-web-apps.md), Application Insights voor het verzamelen van gedetailleerde informatie over uw toepassing met inbegrip van paginaweergaven, aanvragen en uitzonderingen inschakelen. De beschikbaarheid van uw toepassing verder controleren door het configureren van een [beschikbaarheidstest](../application-insights/app-insights-monitor-web-app-availability.md) gebruikersverkeer simuleren.

### <a name="custom-sources"></a>Aangepaste bronnen
Azure Monitor kunt logboekgegevens verzamelen vanuit elke REST-client met behulp van de [Data Collector API](../log-analytics/log-analytics-data-collector-api.md). Hiermee kunt u aangepaste bewakingsscenario's maken en uitbreiden van de bewaking aan resources die niet beschikbaar voor telemetrie via andere bronnen maken.



## <a name="insights"></a>Inzichten
Door gegevens te controleren is alleen nuttig als uw inzicht in de werking van uw IT-omgeving kunt verhogen. Azure Monitor biedt verschillende functies en hulpprogramma's waarmee u inzicht in uw toepassingen en andere bronnen die ze afhankelijk zijn. [Bewakingsoplossingen](../monitoring/monitoring-solutions.md) en functies zoals [Application Insights](../application-insights/app-insights-overview.md) en Container-inzichten diep inzicht geven in verschillende aspecten van uw toepassing en de specifieke Azure-services. 

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) Hiermee wordt de beschikbaarheid, prestaties en gebruik van uw webtoepassingen gecontroleerd of ze in de cloud of on-premises worden gehost. Hierbij wordt gebruikgemaakt van het platform van de krachtige analyse in Log Analytics krijgt u diep inzicht in bewerkingen van uw toepassing en een diagnose van fouten zonder te wachten op een gebruiker ze heeft gerapporteerd. Application Insights bevat verbindingspunten naar verschillende hulpmiddelen voor ontwikkelaars en kan worden geïntegreerd met Visual Studio ter ondersteuning van uw DevOps-processen.

![App Insights](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>Azure Monitor voor containers
Azure Monitor voor containers is een functie die is ontworpen voor het bewaken van de prestaties van containerworkloads die zijn geïmplementeerd op beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Dit biedt u de zichtbaarheid van de prestaties door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers die beschikbaar in Kubernetes via de API voor metrische gegevens zijn. Er worden ook containerlogboeken verzameld.  Nadat u de bewaking van Kubernetes-clusters hebt ingeschakeld, worden deze metrische gegevens en logboeken automatisch verzameld voor u via een beperkte versie van de Log Analytics-agent voor Linux en opgeslagen in Log Analytics.

![Containerstatus](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>Azure Monitor voor virtuele machines
Azure Monitor VM insights bewaakt uw Azure virtual machines (VM) op schaal door het analyseren van de prestaties en status van uw Windows en Linux-VM's, met inbegrip van hun verschillende processen en de gekoppelde afhankelijkheden van andere bronnen en de externe processen. De oplossing biedt ondersteuning voor het bewaken van de prestaties en afhankelijkheden voor toepassingen voor virtuele machines die worden gehost on-premises of een andere cloudprovider.  


![VM Insights](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>Bewakingsoplossingen
[Bewakingsoplossingen](../monitoring/monitoring-solutions.md) zijn verpakt sets met logica die inzicht voor een bepaalde toepassing of service geven in Azure Monitor. Deze gegevens verzamelen in Log Analytics, samen met andere gegevens, met behulp van [query's](../log-analytics/log-analytics-queries.md) voor analyse en [weergaven](../log-analytics/log-analytics-view-designer.md) voor visualisatie. Bewakingsoplossingen zijn [verkrijgbaar bij Microsoft](../monitoring/monitoring-solutions-inventory.md) en partners om te voorzien in bewaking voor verschillende Azure-services en andere toepassingen.

![Bewakingsoplossingen](../monitoring/media/monitoring-solutions/overview.png)

## <a name="responding-to-critical-situations"></a>Reageren op kritieke situaties
Naast de mogelijkheid die u interactief controleren om gegevens te analyseren, moet een effectieve oplossing voor prestatiecontrole kunnen proactief reageren op kritieke situaties geïdentificeerd in de gegevens die worden verzameld. Dit kan het verzenden van een tekst- of e-mail naar een beheerder die verantwoordelijk is voor het onderzoeken van een probleem. Of u een geautomatiseerd proces dat u probeert te corrigeren van een fout kan starten.


### <a name="alerts"></a>Waarschuwingen
[Waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proactief te waarschuwen voor kritieke situaties en mogelijk proberen corrigerende actie te ondernemen. Regels voor waarschuwingen op basis van metrische gegevens beschikt u over bijna realtime waarschuwingen op basis van numerieke waarden, terwijl regels op basis van Logboeken toestaan voor complexe logica voor gegevens uit meerdere bronnen.

Waarschuwingsregels in Azure Monitor gebruiken [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md), die de unieke sets ontvangers en acties die kunnen worden gedeeld met meerdere regels bevatten. Op basis van uw behoeften, kunnen actiegroepen acties uitvoeren, zoals met behulp van webhooks om waarschuwingen externe acties te starten of om te integreren met uw ITSM-hulpprogramma's.

![Waarschuwingen](media/overview/alerts.png)

### <a name="autoscale"></a>Automatisch schalen
Automatisch schalen kunt u de juiste hoeveelheid resources die worden uitgevoerd voor het afhandelen van de belasting van uw toepassing. Hiermee kunt u regels maken waarmee metrische gegevens die worden verzameld door Azure Monitor gebruiken om te bepalen wanneer automatisch toevoegen van resources voor het verwerken van de belasting toeneemt en bespaart ook geld door het verwijderen van resources die zich niet actief. U een minimum en maximum aantal exemplaren en de logica voor het vergroten of verkleinen van resources.

![Automatisch schalen](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>Het visualiseren van gegevens te controleren
[Visualisaties](visualizations.md) zoals grafieken en tabellen zijn effectieve hulpmiddelen voor samengevat door gegevens te controleren en dit te presenteren aan verschillende doelgroepen. Azure Monitor heeft een eigen functies voor het visualiseren van gegevens te controleren en maakt gebruik van andere Azure-services voor het publiceren van het voor verschillende doelgroepen.

### <a name="dashboards"></a>Dashboards
[Azure-dashboards](../azure-portal/azure-portal-dashboards.md) kunt u verschillende soorten gegevens, gecombineerd met inbegrip van logboekbestanden en metrische gegevens in één venster inzicht in de [Azure-portal](https://portal.azure.com). U kunt eventueel het dashboard delen met andere Azure-gebruikers. Elementen in Azure Monitor kunnen worden toegevoegd aan een Azure-dashboard naast de uitvoer van een logboek query of metrische gegevens over de grafiek. U kunt bijvoorbeeld een dashboard met tegels die een grafiek met metrische gegevens, een tabel met activiteitenlogboeken, een grafiek van het gebruik van Application Insights en de uitvoer van een query van Log Analytics weergeven maken.

![Dashboard](media/overview/dashboard.png)

### <a name="views"></a>Weergaven
[Weergaven in Azure Monitor](../log-analytics/log-analytics-view-designer.md) visueel weergegeven logboekgegevens in Log Analytics.  Elke weergave bevat één tegel die ingezoomd op een combinatie van visualisaties, zoals staaf- en lijndiagrammen naast een lijst met essentiële gegevens samenvatten.  Bewakingsoplossingen bevatten weergaven waarin gegevens voor een bepaalde toepassing worden samengevat en kunt u uw eigen weergaven openen om te presenteren van gegevens vanuit een Log Analytics zoekopdracht in Logboeken. Net als andere elementen in Azure Monitor kunnen weergaven worden toegevoegd aan Azure-dashboards.

![Log Analytics-weergave](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) is een business analytics-service waarmee u interactieve visualisaties in een groot aantal gegevensbronnen en is een efficiënte manier om gegevens beschikbaar voor anderen binnen en buiten uw organisatie. U kunt Power BI om te configureren [importeren automatisch logboekgegevens van Azure Monitor](../log-analytics/log-analytics-powerbi.md) om te profiteren van deze extra visualisaties.


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>Integreren en gegevens exporteren
Vaak hebt u de vereiste Azure Monitor worden geïntegreerd met andere systemen en om aangepaste oplossingen die gebruikmaken van uw bewakingsgegevens te maken. Andere Azure-services werken met Azure Monitor om deze integratie te bieden.

### <a name="event-hub"></a>Event Hub
[Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs) is van een streaming-platform en een gebeurtenisopneemservice die kunt transformeren en opslaan van gegevens met behulp van een realtime-analyseprovider of batching/opslagadapters. Event Hubs te gebruiken [stream logboekgegevens van Azure Monitor](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) naar SIEM- en controlehulpprogramma's van partners.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

### <a name="logic-apps"></a>Logic Apps
[Logic Apps](https://azure.microsoft.com/services/logic-apps) is een service waarmee u kunt het automatiseren van taken en bedrijfsprocessen met behulp van werkstromen die kunnen worden geïntegreerd met andere systemen en services. Activiteiten zijn beschikbaar die lezen en schrijven van metrische gegevens en Logboeken in Azure Monitor, zodat u kunt het bouwen van werkstromen met tal van andere systemen integreren.

![Logische apps](../log-analytics/media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="api"></a>API
Meerdere API's zijn beschikbaar voor lezen en schrijven logboeken en metrische gegevens naar en van Azure Monitor naast de toegang tot gegenereerde waarschuwingen. U kunt ook configureren en waarschuwingen worden opgehaald. Dit biedt u in feite onbeperkte mogelijkheden om aangepaste oplossingen die kunnen worden geïntegreerd met Azure Monitor te maken.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:

* [Azure Monitor](https://azure.microsoft.com/services/monitor/) om aan de slag te gaan met metrische gegevens en waarschuwingen voor kernbewaking.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) als u problemen probeert te analyseren in uw App Service-web-app.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) voor het analyseren van verzamelde bewakingsgegevens en logboeken.




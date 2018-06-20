---
title: Azure-toepassingen en -resources bewaken
description: Overzicht van Microsoft-services en -functionaliteiten die bijdragen aan een complete bewakingsstrategie voor uw Azure-services en -toepassingen.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: overview
ms.date: 03/05/2018
ms.author: robb,bwren
ms.component: ''
ms.openlocfilehash: e6adcc136c273210cc40d23ed2cb177287654005
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265129"
---
# <a name="monitoring-azure-applications-and-resources"></a>Azure-toepassingen en -resources bewaken

Bewaken is het verzamelen en analyseren van gegevens om de prestaties, status en beschikbaarheid van uw zakelijke toepassingen te bepalen, evenals de resources waar deze toepassingen van afhankelijk zijn. Een effectieve bewakingsstrategie helpt u de gedetailleerde werking van de onderdelen van uw toepassing te begrijpen. Dit helpt u ook om uw uptime te vergroten door u proactief op de hoogte te stellen van kritieke kwesties, zodat u ze kunt oplossen voordat ze problemen worden.

Azure bevat meerdere services die elk een specifieke rol of taak uitvoeren in de bewakingsruimte. Samen bieden deze services een uitgebreide oplossing voor het verzamelen en analyseren van telemetrie en het reageren daarop vanuit uw toepassing en de Azure-resources die deze ondersteunen. Ze kunnen ook werken aan het bewaken van kritieke on-premises bronnen om een ​​hybride bewakingsomgeving te bieden. Begrip van de beschikbare hulpprogramma’s en gegevens is de eerste stap in het ontwikkelen van een complete bewakingsstrategie voor uw toepassing.

Het volgende diagram toont een conceptueel beeld van de onderdelen die samenwerken om bewaking van Azure-resources te bieden. De volgende secties beschrijven deze onderdelen en bieden links naar gedetailleerde technische informatie.

![Bewakingsoverzicht](media/monitoring-overview/monitoring-products-overview.png)


## <a name="shared-capabilities"></a>Gedeelde functionaliteit
De kern- en diepgaande bewakingsservices delen functionaliteit, wat de volgende mogelijkheden biedt.

### <a name="alerts"></a>Waarschuwingen
[Azure-waarschuwingen](../monitoring-and-diagnostics/monitoring-overview-alerts.md) stellen u proactief op de hoogte van kritieke omstandigheden en nemen mogelijk corrigerende maatregelen. Waarschuwingsregels kunnen gegevens uit meerdere bronnen gebruiken, waaronder metrische gegevens en logboeken. Ze gebruiken [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md) die unieke reeksen ontvangers en acties bevatten als reactie op een waarschuwing. Op basis van uw vereisten kunt u waarschuwingen externe acties laten starten door webhooks te gebruiken en te integreren met uw ITSM-hulpprogramma’s.

### <a name="dashboards"></a>Dashboards
U kunt [Azure-dashboards](../azure-portal/azure-portal-dashboards.md) gebruiken om verschillende soorten gegevens te combineren in één paneel in [Azure Portal](https://portal.azure.com). Vervolgens kunt u het dashboard delen met andere Azure-gebruikers.

U kunt bijvoorbeeld een dashboard maken waarin het volgende wordt gecombineerd:
- Tegels die een grafiek met metrische gegevens weergeven
- Een tabel met activiteitenlogboeken
- Een gebruiksgrafiek van Application Insights
- De uitvoer van het zoeken in logboeken in Log Analytics

U kunt ook Log Analytics-gegevens exporteren naar [Power BI](https://docs.microsoft.com/power-bi/). Daar kunt u profiteren van extra visualisaties. U kunt de gegevens ook beschikbaar stellen aan anderen binnen en buiten uw organisatie.

### <a name="metrics-explorer"></a>Metrics Explorer
[Metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) zijn numerieke waarden die door een Azure-resource worden gegenereerd om u te helpen de werking en prestaties van de resource te begrijpen. Door de Metrics Explorer te gebruiken, kunt u metrische gegevens naar Log Analytics verzenden voor analyse met gegevens uit andere bronnen.


## <a name="core-monitoring"></a>Kernbewaking
Kernbewaking biedt fundamentele, vereiste bewaking van Azure-resources. Deze services vereisen minimale configuratie en verzamelen kerntelemetrie die wordt gebruikt door de premium-bewakingsservices.    

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) maakt kernbewaking voor Azure-services mogelijk door de verzameling van [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)en [logboeken met diagnostische gegevens](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)toe te staan. Het activiteitenlogboek geeft bijvoorbeeld aan wanneer nieuwe bronnen worden gemaakt of gewijzigd.

Er zijn metrische gegevens beschikbaar met prestatiestatistieken voor verschillende resources en zelfs het besturingssysteem binnen een virtuele machine. U kunt deze gegevens bekijken met een van de verkenners in de Azure-portal en waarschuwingen maken op basis van deze metrische gegevens. Azure Monitor biedt de snelste pijplijn voor metrische gegevens (5 minuten tot 1 minuut); gebruik het dus voor waarschuwingen en meldingen waarbij de tijd van kritiek belang is.

U kunt deze statistieken en logboeken ook verzenden naar Azure Log Analytics voor trends en gedetailleerde analyses, of extra waarschuwingsregels maken om u proactief op de hoogte te stellen van kritieke problemen als resultaat van die analyse.  

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens naar Log Analytics via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Maar wanneer de metriek wordt geëxporteerd naar Log Analytics, wordt de metriek weergegeven als alle binnenkomende berichten in alle wachtrijen in de Event Hub.
>
>

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) bewaakt voortdurend uw resourceconfiguratie en gebruikstelemetrie. Vervolgens krijgt u gepersonaliseerde aanbevelingen op basis van best practices. Door deze aanbevelingen te volgen, kunt u de prestaties, beveiliging en beschikbaarheid van de resources verbeteren die uw toepassingen ondersteunen.

### <a name="service-health"></a>Service Health
De status van uw toepassing hangt af van de Azure-services waarvan deze afhankelijk is. [Azure Service Health](../service-health/service-health-overview.md) identificeert problemen met Azure-services die van invloed kunnen zijn op uw toepassing. Service Health helpt u ook bij het plannen van gepland onderhoud.

### <a name="activity-log"></a>Activiteitenlogboek
[Activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) biedt gegevens over de werking van een Azure-resource. Deze gegevens bevatten:
- Configuratiewijzigingen aan de resource.
- Servicestatusincidenten.
- Aanbevelingen voor beter gebruik van de resource.
- Informatie met betrekking tot bewerkingen voor automatisch schalen.

U kunt logboeken voor een bepaalde resource weergeven op de pagina ervan in de Azure-portal. U kunt ook logboeken van meerdere resources bekijken in Activiteitenlogboekverkenner.

U kunt ook activiteitenlogboekitems verzenden naar Log Analytics. Daar kunt u de logboeken analyseren door gegevens te gebruiken die zijn verzameld door beheeroplossingen, agents op virtuele machines en andere bronnen.

## <a name="deep-monitoring-services"></a>Diepgaande bewakingsservices
De volgende Azure-services bieden uitgebreide mogelijkheden voor het verzamelen en analyseren van bewakingsgegevens op een dieper niveau. Deze services bouwen voort op kernbewaking en profiteren van de algemene functionaliteit in Azure. Ze bieden krachtige analyses met verzamelde gegevens om u unieke inzichten te geven in uw toepassingen en infrastructuur. Ze presenteren gegevens in de context van scenario's die zijn gericht op verschillende doelgroepen.

## <a name="deep-application-monitoring"></a>Uitgebreide toepassingsbewaking
### <a name="application-insights"></a>Application Insights
U kunt [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) gebruiken om de beschikbaarheid, prestaties en gebruik van uw toepassing te bewaken, ongeacht of deze wordt gehost in de cloud of on-premises.

Door uw applicatie te instrumenteren om met Application Insights te werken, kunt u diepe inzichten bereiken en DevOps-scenario's implementeren. U kunt snel fouten identificeren en er een diagnose voor stellen, zonder te wachten tot een gebruiker ze meldt. Met de informatie die u verzamelt, kunt u weloverwogen keuzes maken over het onderhoud en de verbeteringen van uw toepassing.

Application Insights heeft uitgebreide hulpmiddelen voor interactie met de gegevens die worden verzameld. Application Insights slaat zijn gegevens op in een gemeenschappelijke opslagplaats. Het kan profiteren van gedeelde functionaliteit zoals waarschuwingen, dashboards en diepgaande analyse met de Log Analytics-querytaal.

## <a name="deep-infrastructure-monitoring"></a>Diepgaande infrastructuurbewaking
### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) speelt een centrale rol in Azure-bewaking door gegevens van verschillende resources (waaronder niet-Microsoft-tools) te verzamelen in één opslagplaats. Daar kunt u de gegevens analyseren met behulp van een krachtige querytaal.

Application Insights en Azure Security Center slaan hun gegevens op in de gegevensarchief van Log Analytics en maken gebruik van de analyse-engine daarvan. Er worden ook gegevens verzameld uit Azure Monitor, beheeroplossingen en agents die zijn geïnstalleerd op virtuele machines in de cloud of on-premises. Deze gedeelde functionaliteit helpt u een volledig overzicht van uw omgeving te vormen.

### <a name="management-solutions"></a>Beheeroplossingen
[Beheeroplossingen](../log-analytics/log-analytics-add-solutions.md) zijn verpakte logicasets die inzichten bieden voor een bepaalde toepassing of service. Ze zijn afhankelijk van Log Analytics voor het opslaan en analyseren van de bewakingsgegevens die ze verzamelen.

Bij Microsoft en partners zijn beheeroplossingen beschikbaar voor het bewaken van allerlei services van Azure en van derden. Voorbeelden van bewakingsoplossingen zijn:
* [Container Monitoring](../log-analytics/log-analytics-containers.md), dat u helpt uw containerhosts te bekijken en te beheren.
* [Azure SQL Analytics](../log-analytics/log-analytics-azure-sql.md), dat metrische gegevens over de prestaties van Azure SQL-databases verzamelt en visualiseert.

U kunt alle beschikbare beheeroplossingen bekijken in de Azure Portal onder het scherm *Monitor*.

### <a name="network-monitoring"></a>Netwerkbewaking
Er zijn verschillende hulpprogramma's die samenwerken om verschillende aspecten van uw netwerk te bewaken, zowel in Azure als on-premises.  

[Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) biedt op scenario's gebaseerde bewaking en diagnostiek voor verschillende netwerkscenario's in Azure. Het slaat gegevens op in metrische gegevens van Azure en diagnostische gegevens voor verdere analyse. Het werkt met de volgende oplossingen voor het bewaken van verschillende aspecten van uw netwerk.

[Netwerkprestatiemeter](../log-analytics/log-analytics-network-performance-monitor.md) (NPM) is een cloudnetwerkbewakingsoplossing die de connectiviteit bewaakt in openbare clouds, datacenters en on-premises omgevingen.

[ExpressRoute Monitor](../expressroute/how-to-npm.md) is een NPM-voorziening die de end-to-end connectiviteit en prestaties via Azure ExpressRoute-circuits bewaakt.

[DNS-analyse](../log-analytics/log-analytics-dns.md) is een oplossing die inzicht biedt in beveiliging, prestaties en bewerkingen, op basis van uw DNS-servers.

[Service Endpoint Monitor](../networking/network-monitoring-overview.md) test de bereikbaarheid van applicaties en detecteert knelpunten in de prestaties in on-premises, providernetwerken en cloud-/privédatacenters.


### <a name="service-map"></a>Serviceoverzicht
[Servicetoewijzing](../operations-management-suite/operations-management-suite-service-map.md) biedt inzicht in uw IaaS-omgeving door virtuele machines te analyseren met hun verschillende processen en afhankelijkheden van andere computers en externe processen. Het integreert evenementen, prestatiegegevens en beheeroplossingen in Log Analytics. U kunt deze gegevens vervolgens bekijken in de context van elke computer en de relatie ervan tot de rest van uw omgeving.

Servicetoewijzing is vergelijkbaar met [Applicatieoverzicht in Application Insights](../application-insights/app-insights-app-map.md). Het richt zich op de infrastructuuronderdelen die uw toepassingen ondersteunen.


## <a name="example-scenarios"></a>Voorbeeldscenario 's
Hieronder volgen voorbeelden op hoog niveau die illustreren hoe u verschillende bewakingshulpprogramma’s in Azure voor verschillende scenario's kunt gebruiken.

### <a name="monitoring-a-web-application"></a>Een webtoepassing bewaken
Neem bijvoorbeeld een webtoepassing die is geïmplementeerd in Azure via Azure App Service, Azure Storage en een SQL-database. U begint met het openen van [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en [activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) voor deze resources op hun pagina's in de Azure-portal. U zoekt naar kritieke informatie, zoals het aantal aanvragen voor de toepassing en de gemiddelde responstijd. U identificeert ook eventuele configuratiewijzigingen.

Vervolgens gaat u naar Controle in de portal om statistieken en logboeken voor de verschillende bronnen samen te bekijken. Terwijl u standaardparameters voor de metrische gegevens bepaalt, maakt u [meldingsregels](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md). Deze regels stellen u proactief op de hoogte wanneer bijvoorbeeld de gemiddelde reactietijd boven een drempelwaarde stijgt. Voor een snel overzicht van de dagelijkse prestaties van uw toepassing, maakt u een Azure-dashboard om grafieken weer te geven van metrische gegevens die kritieke KPI's vertegenwoordigen.

Voor meer diepgaande bewaking van uw toepassing kunt u deze [configureren voor Application Insights](../application-insights/quick-monitor-portal.md). U kunt nu aanvullende gegevens verzamelen die verder inzicht geven in de werking en prestaties van uw toepassing. Application Insights detecteert de onderliggende relaties tussen de onderdelen van uw app. Het maakt visuele weergave mogelijk via [Applicatieoverzicht](../application-insights/app-insights-app-map.md) in combinatie met [end-to-end tracering](../application-insights/app-insights-transaction-diagnostics.md) om exact het onderdeel, de afhankelijkheid of de uitzondering te diagnosticeren waar een probleem is opgetreden.

U maakt [Beschikbaarheidstests](../application-insights/app-insights-monitor-web-app-availability.md) om uw toepassing proactief te testen vanuit verschillende regio's. Om uw ontwikkelaars te helpen, schakelt u [de Profiler](../application-insights/enable-profiler-compute.md) in, zodat u aanvragen en uitzonderingen tot een specifieke regel code kunt volgen. Voor meer inzicht in de services die in uw toepassing worden gebruikt, voegt u de [SQL Analytics-oplossing](../log-analytics/log-analytics-azure-sql.md) toe om extra gegevens te verzamelen in Log Analytics.

Na enige tijd besluit u de hoofdoorzaak te onderzoeken voor perioden waarin de prestaties op de site onder een drempelwaarde zijn gedaald. U schrijft een query met behulp van Log Analytics. Dit helpt u de gebruiks- en prestatiegegevens die zijn verzameld door Application Insights, te correleren met configuratie- en prestatiegegevens in de Azure-bronnen die uw toepassing ondersteunen.


### <a name="monitoring-virtual-machines"></a>Virtuele machines bewaken
U hebt een combinatie van virtuele Windows- en Linux-machines in Azure. U gebruikt Azure Monitor om [activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) en [metrische gegevens op hostniveau](../monitoring-and-diagnostics/monitoring-overview-metrics.md)te bekijken. U voegt de [Azure Diagnostics-extensie](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) toe aan de virtuele machines om metrische gegevens te verzamelen van het gastbesturingssysteem. Vervolgens maakt u [waarschuwingsregels](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) om u proactief op de hoogte te stellen wanneer metrische basisgegevens zoals processorgebruik en geheugen bepaalde drempels overschrijden.

Als u meer gegevens wilt verzamelen over virtuele machines waarop een bedrijfstoepassing wordt uitgevoerd, [maakt u een Log Analytics–werkruimte en schakelt u de VM-extensie in](../log-analytics/log-analytics-quick-collect-azurevm.md) op elke computer. U configureert het [verzamelen van verschillende gegevensbronnen](../log-analytics/log-analytics-data-sources.md) voor uw toepassing en [maakt weergaven](../log-analytics/log-analytics-view-designer.md) om te rapporteren over de dagelijkse werking en prestaties. Vervolgens maakt u [waarschuwingsregels](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) om u op de hoogte te stellen wanneer bepaalde foutgebeurtenissen worden ontvangen.

Om de status van de geïnstalleerde agent voortdurend te bewaken, voegt u de [beheeroplossing Status van agent](../operations-management-suite/oms-solution-agenthealth.md)toe. Om meer inzicht te krijgen in de toepassing, voegt u [de afhankelijkheidsagent](../operations-management-suite/operations-management-suite-service-map-configure.md) toe aan de virtuele machines om ze toe te voegen aan [Servicetoewijzing](../operations-management-suite/operations-management-suite-service-map.md). Servicetoewijzing ontdekt kritieke processen en identificeert verbindingen tussen machines met andere services.

Na een gemelde uitval gebruikt u Servicetoewijzing om forensisch onderzoek uit te voeren om de specifieke machines te identificeren waar het probleem is opgetreden. Vervolgens maakt u een [query op de Log Analytics-gegevens](../log-analytics/log-analytics-log-search-new.md) om het probleem in de toekomst te identificeren. En u maakt een waarschuwingsregel om u proactief op de hoogte te stellen wanneer de toestand wordt gedetecteerd.



## <a name="next-steps"></a>Volgende stappen
Meer informatie over:

* [Azure Monitor](https://azure.microsoft.com/services/monitor/) om aan de slag te gaan met metrische gegevens en waarschuwingen voor kernbewaking.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) als u problemen probeert te analyseren in uw App Service-web-app.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) voor het analyseren van verzamelde bewakingsgegevens en logboeken.

---
title: Overzicht van Azure Monitor
description: Azure Monitor verzamelt statistieken die kunnen worden gebruikt in waarschuwingen, webhooks, automatisch schalen en automatisering. In het artikel worden ook andere bewakingsopties van Microsoft genoemd.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: overview
ms.date: 03/28/2018
ms.author: robb
ms.custom: mvc
ms.component: ''
ms.openlocfilehash: a96991c424b4709002d46b6b7abe1e884c3605dd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264582"
---
# <a name="overview-of-azure-monitor"></a>Overzicht van Azure Monitor
Dit artikel bevat een overzicht van de service Azure Monitor in Microsoft Azure. Het artikel beschrijft wat Azure Monitor doet en bevat koppelingen naar aanvullende informatie over het gebruik van Azure Monitor.  Als u de voorkeur geeft aan een video-inleiding, ga dan naar de koppelingen Volgende stappen onderin dit artikel. 

## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Azure Monitor en andere bewakingsproducten van Microsoft
Azure Monitor biedt metrische infrastructuurgegevens en logboeken op basisniveau voor de meeste services in Microsoft Azure. Azure-services die hun gegevens nog niet doorgeven aan Azure Monitor zullen dit in de toekomst wel gaan doen.

Microsoft levert aanvullende producten en services die extra bewakingsmogelijkheden bieden voor ontwikkelaars, DevOps of IT Ops, en ook on-premises kunnen worden geïnstalleerd. Zie [Monitoring in Microsoft Azure](monitoring-overview.md) (Azure-toepassingen en -resources bewaken) voor een overzicht van en meer informatie over hoe deze verschillende producten en services samenwerken .

## <a name="portal-overview-page"></a>Overzichtspagina van portal

Via de startpagina van Azure Monitor kunnen gebruikers: 
- Inzicht in de bewakingsmogelijkheden van Azure.
- Het platform en de premium bewakingsmogelijkheden van Azure ontdekken, configureren en gebruiken.

De pagina is een startpunt voor navigatie, met inbegrip van de ingebruikname. De pagina toont beheerde problemen van andere services die aandacht vereisen en geeft de gebruiker de mogelijkheid om deze in hun context te bekijken.
 
![Model voor bewaking en diagnostische gegevens voor niet-rekenresources](./media/monitoring-overview-azure-monitor/monitor-overview-ux2.png)

Wanneer u de pagina opent, kunt u een van de abonnementen selecteren waartoe u leestoegang hebt. Voor een geselecteerd abonnement, kunt u het volgende zien:

- **Geactiveerde waarschuwingen en waarschuwingsbronnen**: deze tabel toont totalen, waarschuwingsbronnen en hoe dikwijls waarschuwingen voor de geselecteerde duur zijn geactiveerd. De tabel is van toepassing op zowel oudere als nieuwere waarschuwingen. Meer informatie over de [nieuwere Azure-waarschuwingen](monitoring-overview-unified-alerts.md). 
- **Fouten in het activiteitenlogboek**: als een van uw Azure-resources gebeurtenissen registreren met een ernst op foutniveau, kunt u een telling op hoog niveau bekijken en de pagina van het activiteitenlogboek doorbladeren om elke gebeurtenis te onderzoeken.
- **Azure Service Health**: u kunt het aantal problemen met de Service Health-service, geplande onderhoudsgebeurtenissen en statusadviezen bekijken. Azure Service Health verstrekt gepersonaliseerde informatie wanneer problemen in de Azure-infrastructuur invloed hebben op uw services.  Zie [Azure Service Health](../service-health/service-health-overview.md) voor meer informatie.  
- **Application Insights**: zie KPI's voor elke AppInsights-resource in het huidige abonnement. De KPI's zijn geoptimaliseerd voor toepassingsbewaking aan serverzijde binnen ASP.NET-web-apps en toepassingen van het type Java, Node en Algemeen. De KPI's bevatten metrische gegevens voor aanvraagsnelheid, reactieduur, foutpercentage en beschikbaarheidspercentage. 

Als u Log Analytics of Application Insights niet hebt geïmplementeerd of als u in het huidige abonnement geen Azure-waarschuwingen hebt geconfigureerd, bevat de pagina koppelingen waarmee u het implementatieproces kunt starten.



## <a name="azure-monitor-sources---compute-subset"></a>Azure Monitor-bronnen - Compute-subset

![Model voor bewaking en diagnostische gegevens voor niet-rekenresources](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)


Deze Compute-services bevatten 
- Cloud Services 
- Virtuele machines 
- Schaalsets voor virtuele machines 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Toepassing - Diagnoselogboeken, Toepassingslogboeken en Metrische gegevens
Toepassingen kunnen worden uitgevoerd in het gastbesturingssysteem in het rekenmodel. Ze verzenden hun eigen set logboeken en metrische gegevens. Voor het verzamelen van de meeste metrische gegevens en logboeken op toepassingsniveau is Azure Monitor afhankelijk van de Azure-extensie voor diagnostische gegevens (Windows of Linux). De typen omvatten

* Prestatiemeteritems
* Toepassingslogboeken
* Windows-gebeurtenislogboeken
* .NET-gebeurtenisbron
* IIS-logboeken
* ETW op basis van manifest
* Crashdumps
* Foutenlogboeken van klant

Zonder de extensie voor diagnostische gegevens zijn slechts enkele metrische gegevens, zoals CPU-gebruik, beschikbaar. 

### <a name="host-and-guest-vm-metrics"></a>Metrische host- en gastgegevens
Aan de eerder vermelde rekenresources zijn een VM-host en gastbesturingssysteem toegewezen waarmee ze communiceren. De host-VM en het gastbesturingssysteem komen overeen met de basis-VM en gast-VM in het model van de Hyper-V-hypervisor. U kunt op beide metrische gegevens verzamelen. U kunt ook diagnoselogboeken verzamelen op het gastbesturingssysteem.   

### <a name="activity-log"></a>Activiteitenlogboek
U kunt in het activiteitenlogboek (eerder operationele of controlelogboeken genoemd) zoeken naar informatie over uw resource, zoals deze wordt gezien door de Azure-infrastructuur. Het logboek bevat informatie zoals de tijden waarop resources zijn gemaakt of vernietigd.  Zie [Overview of Activity Log](monitoring-overview-activity-logs.md) (Overzicht van Activiteitenlogboek) voor meer informatie. 

## <a name="azure-monitor-sources---everything-else"></a>Azure Monitor-bronnen - al het andere

![Model voor bewaking en diagnostische gegevens voor rekenresources](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Resource: metrische gegevens en diagnoselogboeken
Verzamelbare metrische gegevens en diagnoselogboeken variëren, afhankelijk van het resourcetype. Zo verstrekt Web Apps bijvoorbeeld statistieken over de schijf-I/O en het CPU-percentage. Deze metrische gegevens worden niet verstrekt voor een Service Bus-wachtrij; deze verstrekt weer metrische gegevens als de grootte van de wachtrij en de doorvoer van berichten. Een lijst met verzamelbare metrische gegevens voor elke resource is beschikbaar op [Ondersteunde metrische gegevens](monitoring-supported-metrics.md). 

### <a name="host-and-guest-vm-metrics"></a>Metrische host- en gastgegevens
Er is niet noodzakelijkerwijs een 1-op-1-toewijzing tussen uw resource en een bepaalde host- of gast-VM; metrische gegevens zijn dus niet altijd beschikbaar.

### <a name="activity-log"></a>Activiteitenlogboek
Het activiteitenlogboek is hetzelfde als voor rekenresources.  

## <a name="uses-for-monitoring-data"></a>Wordt gebruikt voor het bewaken van gegevens
Wanneer u uw gegevens verzamelt, kunt u in Azure Monitor het volgende met deze gegevens doen.

### <a name="route"></a>Route
U kunt bewakingsgegevens streamen naar andere locaties. 

Voorbeelden zijn:

- Verzenden naar Application Insights, zodat u de uitgebreidere visualisatie- en analysefuncties van deze service kunt gebruiken.
- Verzenden naar Event Hubs, zodat u ze kunt doorsturen naar hulpprogramma's van derden. 

### <a name="store-and-archive"></a>Opslaan en archiveren
Sommige bewakingsgegevens worden al opgeslagen en blijven gedurende een bepaalde tijd beschikbaar in Azure Monitor. 
- Metrische gegevens worden 90 dagen bewaard. 
- Vermeldingen in het activiteitenlogboek worden 90 dagen bewaard. 
- Diagnoselogboeken worden niet opgeslagen. 

Gebruik Azure-opslag als u gegevens langer dan de hierboven genoemde perioden wilt bewaren. Bewakingsgegevens worden in uw opslagaccount bewaard volgens een bewaarbeleid dat u hebt ingesteld. U betaalt voor de ruimte die de gegevens in Azure-opslag in beslag nemen. 

Een aantal manieren waarop u deze gegevens kunt gebruiken:

- Wanneer de gegevens zijn weggeschreven, kunnen ze worden gelezen en verwerkt door andere hulpprogramma's binnen en buiten Azure.
- Download de gegevens lokaal als u een lokaal archief wilt aanleggen of verander uw bewaarbeleid in de cloud als u gegevens langere tijd wilt behouden.  
- U kunt de gegevens voor onbepaalde tijd in Azure-opslag laten staan voor archiveringsdoeleinden. 

### <a name="query"></a>Query’s uitvoeren
U kunt de REST API van Azure Monitor, platformonafhankelijke CLI-opdrachten (Command-Line Interface), PowerShell-cmdlets of de .NET SDK gebruiken om toegang te krijgen tot de gegevens in het systeem of de Azure-opslag

Voorbeelden zijn:

* Gegevens ophalen voor een aangepaste bewakingstoepassing die u hebt geschreven
* Aangepaste query's maken en die gegevens verzenden naar een toepassing van derden.

### <a name="visualize"></a>Visualiseren
Door de bewakingsgegevens te visualiseren in afbeeldingen en grafieken kunt u gemakkelijker en sneller trends opsporen dan wanneer u de gegevens zelf bekijkt.  

Enkele visualisatie methoden zijn:

* Azure Portal gebruiken
* Gegevens doorsturen naar Azure Application Insights
* Gegevens doorsturen naar Microsoft PowerBI
* De gegevens met behulp van live streamen doorsturen naar een visualisatieprogramma van derden, of de gegevens door een hulpprogramma laten lezen uit een archief in Azure-opslag


### <a name="automate"></a>Automatiseren
> [!NOTE]
> Door de voortdurende ontwikkeling van waarschuwingen in Microsoft Azure is er nu een uniforme ervaring voor waarschuwingen beschikbaar. Meer informatie over [nieuwe Azure-waarschuwingen](monitoring-overview-unified-alerts.md)

In de Azure-waarschuwingen kunt u bewakingsgegevens gebruiken om waarschuwingen of zelfs hele processen te activeren. Voorbeelden zijn:

* Gegevens gebruiken om rekenexemplaren automatisch te schalen op basis van de belasting van toepassingen.
* E-mails verzenden op basis van voorwaarden van metrische gegevens of logboeken. 
* Een web-URL (webhook) aanroepen om een actie uit te voeren in een systeem buiten Azure
* Een runbook starten in Azure Automation om een scala aan taken uit te voeren

## <a name="methods-of-accessing-azure-monitor"></a>Methoden om toegang te krijgen tot Azure Monitor
In het algemeen kunt u het bijhouden, routeren en ophalen van gegevens manipuleren met een van de volgende methoden. Niet alle methoden zijn beschikbaar voor alle acties of gegevenstypen.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Platformonafhankelijke opdrachtregelinterface (CLI)](insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over
- Een video-overzicht van alleen Azure Monitor is beschikbaar in  
[Aan de slag met Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). 
- Een video waarin een scenario wordt uitgelegd waarin u Azure Monitor kunt gebruiken, is beschikbaar in [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) (Bewaking en diagnostische gegevens van Microsoft Azure verkennen) en [Azure Monitor in een video van Ignite 2016](https://myignite.microsoft.com/videos/4977).
- De Azure Monitor-interface bekijken in [Aan de slag met Azure Monitor](monitoring-get-started.md)
- De [Azure Diagnostics-extensies](../azure-diagnostics.md) instellen als u problemen wilt diagnosticeren in uw cloudservice, virtuele machine, VM-schaalsets of Service Fabric-toepassing.
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) als u problemen probeert te analyseren in uw App Service-web-app.
- [Problemen in Azure Storage oplossen](../storage/common/storage-e2e-troubleshooting.md) bij gebruik van Storage Blobs, Tables of Queues
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/)

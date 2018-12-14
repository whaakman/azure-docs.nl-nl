---
title: Gegevens weergeven vanuit Azure Monitor | Microsoft Docs
description: Geeft een overzicht van de beschikbare methoden voor het visualiseren van gegevens die zijn opgeslagen in Azure Monitor, met inbegrip van gegevens van de metrische gegevens opslaan en Log Analytics.
author: bwren
manager: carmonm
editor: ''
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/15/2018
ms.author: bwren
ms.openlocfilehash: a2fd26d110e7bf1ce7ac365b83659e5d33a037df
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53383541"
---
# <a name="visualizing-data-from-azure-monitor"></a>Gegevens weergeven vanuit Azure Monitor
In dit artikel bevat een samenvatting van de beschikbare methoden voor het visualiseren van gegevens die zijn opgeslagen in Azure Monitor. Dit omvat [metrische gegevens in de metrische gegevens van Azure store](../azure-monitor/platform/data-collection.md#metrics) en [gegevens vastleggen in Log Analytics](../azure-monitor/platform/data-collection.md#logs). 

Visualisaties, zoals diagrammen en grafieken kunt u bij het analyseren van uw gegevens om te zoomen op problemen en patronen te identificeren. Afhankelijk van het hulpprogramma die u gebruikt, kan u ook de optie voor visualisaties delen met andere gebruikers binnen en buiten uw organisatie hebt.

## <a name="azure-dashboards"></a>Azure-Dashboards
[Azure-dashboards](../azure-portal/azure-portal-dashboards.md) zijn van de primaire dashboarding-technologie voor Azure. Ze zijn vooral nuttig in een enkel glazen verstrekken over uw Azure-infrastructuur en services zodat u snel belangrijke problemen te identificeren.

![Dashboard](media/visualizations/dashboard.png)

### <a name="advantages"></a>Voordelen
- Diepe integratie in Azure. Visualisaties kunnen worden vastgemaakt aan dashboards vanuit meerdere Azure pagina's, met inbegrip van Metrics explorer, Log Analytics en Application Insights.
- Biedt ondersteuning voor metrische gegevens en Logboeken.
- Combineer gegevens uit meerdere bronnen, met inbegrip van de uitvoer van [Metrics explorer](../azure-monitor/platform/metrics-charts.md), [Log Analytics-query's](../azure-monitor/log-query/log-query-overview.md), en [toegewezen](../application-insights/app-insights-app-map.md) en [beschikbaarheid]()in Application Insights.
- De optie voor persoonlijke of gedeelde dashboards. Geïntegreerd met Azure [verificatie op basis van rollen (RBAC)](../role-based-access-control/overview.md).
- Automatische vernieuwing. Metrische gegevens over Gegevensvernieuwing is afhankelijk van tijdsbereik met ten minste vijf minuten. Logboeken vernieuwen op één minuut.
- Constructorreeks mag metrische gegevens over dashboards met tijdstempel en aangepaste parameters.
- Opties voor flexibele lay-out.
- Modus volledig scherm.


### <a name="limitations"></a>Beperkingen
- Beperkte controle over Log Analytics-visualisaties met biedt geen ondersteuning voor gegevenstabellen. Totaal aantal gegevensreeksen is beperkt tot 10 met verdere gegevensreeks gegroepeerd onder een _andere_ bucket.
- Er zijn geen aangepaste parameters ondersteuning voor Log Analytics-grafieken.
- Log Analytics grafieken zijn beperkt tot de afgelopen 30 dagen.
- Log Analytics grafieken kunnen alleen worden vastgemaakt aan gedeelde dashboards.
- Er is geen interactiviteit van de dashboardgegevens.
- Beperkte contextuele inzoomen.

## <a name="azure-monitor-views"></a>Azure Monitor-weergaven
[Weergaven in Azure Monitor](../azure-monitor/platform/view-designer.md) kunt u aangepaste visualisaties maken met de logboekgegevens die zijn opgeslagen in Log Analytics. Ze worden gebruikt door [bewakingsoplossingen](../azure-monitor/insights/solutions.md) om de gegevens die zij verzamelen te presenteren.

![Weergave](media/visualizations/view.png)

### <a name="advantages"></a>Voordelen
- Uitgebreide visualisaties voor Log Analytics-gegevens.
- Exporteren en importeren van weergaven deze overbrengen naar andere resourcegroepen en abonnementen.
- Kan worden geïntegreerd in Log Analytics management-model met werkruimten en bewakingsoplossingen.
- [Filters](../azure-monitor/platform/view-designer-filters.md) voor aangepaste parameters.
- Interactief, biedt ondersteuning voor meerdere niveaus drill-in (weergave die in een andere weergave ingezoomd)

### <a name="limitations"></a>Beperkingen
- Ondersteunt Logboeken, maar geen metrische gegevens.
- Er zijn geen persoonlijke weergaven. Beschikbaar voor alle gebruikers met toegang tot de werkruimte.
- Er is geen automatische vernieuwing.
- Beperkte outopties voor lay.
- Er is geen ondersteuning voor het uitvoeren van query's in Log Analytics-werkruimten en Application Insights-toepassingen.
- Query's worden in Antwoordgrootte beperkt tot 8MB en query uitvoeringstijd van 110 seconden.



## <a name="application-insights-workbooks"></a>Application Insights-werkmappen
[Werkmappen](../application-insights/app-insights-usage-workbooks.md) interactieve documenten die inzicht in uw gegevens, onderzoek en samenwerking binnen het team bieden. Specifieke voorbeelden waarin werkmappen nuttig zijn zijn voor probleemoplossing handleidingen en postmortem incident.

![Workbook](media/visualizations/workbook.png)

### <a name="advantages"></a>Voordelen
- Biedt ondersteuning voor metrische gegevens en Logboeken.
- Ondersteunt parameters het inschakelen van interactieve rapporten waar dynamisch selecteren van een element in een tabel wordt bijgewerkt gekoppeld grafieken en visualisaties.
- Document-achtige stroom.
- De optie voor persoonlijke of gedeelde werkmappen.
- Eenvoudige en gezamenlijke-vriendelijke ervaring.
- Sjablonen ondersteunen een galerie met openbare GitHub op basis van sjablonen.

### <a name="limitations"></a>Beperkingen
- Er is geen automatische vernieuwing.
- Er is geen compacte indeling, zoals dashboards, waardoor werkmappen minder nuttig zijn als een enkel glazen. Bedoeld informatie voor het leveren van meer inzicht te krijgen.


## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is vooral handig voor het maken van zakelijke dashboards en rapporten, evenals rapporten op de lange termijn KPI-trends analyseren. U kunt [importeren van de resultaten van een Log Analytics-query](../azure-monitor/platform/powerbi.md) naar een Power BI-gegevensset, zodat u van de functies profiteren kunt, zoals het combineren van gegevens uit verschillende bronnen en delen van rapporten op het web en mobiele apparaten.

![Power BI](media/visualizations/power-bi.png)

### <a name="advantages"></a>Voordelen
- Uitgebreide visualisaties.
- Uitgebreide interactieve inclusief in te zoomen en kruislings filteren.
- Eenvoudig te delen binnen uw organisatie.
- Integratie met andere gegevens van meerdere gegevensbronnen.
- Betere prestaties met resultaten in de cache opgeslagen in een kubus.


### <a name="limitations"></a>Beperkingen
- Ondersteunt Logboeken, maar geen metrische gegevens.
- Geen integratie van Azure. Dashboards en -modellen via Azure Resource Manager kan niet beheren.
- De resultaten van query moeten worden geïmporteerd in Power BI-model om te configureren. Beperking van de grootte en vernieuwen.
- Beperkte gegevens vernieuwen van acht keer per dag.


## <a name="grafana"></a>Grafana
[Grafana](https://grafana.com/) is een open platform waarmee werkt uitstekend in operationele dashboards. Dit is vooral handig voor het detecteren en isoleren en operationele incidenten beoordelen. U kunt toevoegen [Grafana Azure Monitor-gegevensbron invoegtoepassing](../monitoring-and-diagnostics/monitor-send-to-grafana.md) aan uw Azure-abonnement om uw metrische gegevens van Azure-gegevens visualiseren.

![Grafana](media/visualizations/grafana.png)

### <a name="advantages"></a>Voordelen
- Uitgebreide visualisaties.
- Rijk ecosysteem van gegevensbronnen.
- Gegevens met inbegrip van interactiviteit inzoomen.
- Biedt ondersteuning voor parameters.

### <a name="limitations"></a>Beperkingen
- Ondersteunt metrische gegevens, maar niet de logboeken.
- Geen integratie van Azure. Dashboards en -modellen via Azure Resource Manager kan niet beheren.
- Kosten voor de ondersteuning voor Grafana Cloud extra Grafana infrastructuur of extra kosten.


## <a name="build-your-own-custom-application"></a>Bouw uw eigen aangepaste toepassing
U kunt gegevens in de metrische gegevens van Azure en de Log Analytics openen via hun API met een willekeurige REST-client, waarmee u uw eigen aangepaste websites en toepassingen kunt maken.

### <a name="advantages"></a>Voordelen
- Volledige flexibiliteit in de gebruikersinterface, visualisatie, interactiviteit en functies.
- Combineer metrische gegevens en logboekgegevens aan andere gegevensbronnen.

### <a name="disadvantages"></a>Nadelen
- Belangrijke technische werk dat nodig is.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de [gegevens verzameld door Azure Monitor](../azure-monitor/platform/data-collection.md).
- Meer informatie over [Azure-dashboards](../azure-portal/azure-portal-dashboards.md).
- Meer informatie over [weergaven in Azure Monitor](../azure-monitor/platform/view-designer.md).
- Meer informatie over [werkmappen in Application Insights](../application-insights/app-insights-usage-workbooks.md).
- Meer informatie over [logboekgegevens importeren in Power BI](../azure-monitor/platform/powerbi.md).
- Meer informatie over de [Grafana Azure Monitor-gegevensbron invoegtoepassing](../monitoring-and-diagnostics/monitor-send-to-grafana.md).

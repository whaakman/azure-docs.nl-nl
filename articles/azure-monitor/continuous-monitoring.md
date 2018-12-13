---
title: Doorlopende bewaking met Azure Monitor | Microsoft Docs
description: Specifieke stappen voor het gebruik van Azure Monitor om in te schakelen voor continue bewaking in uw werkstromen beschreven.
author: bwren
manager: carmonm
editor: ''
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: bwren
ms.openlocfilehash: 88a4c6dbe9311a054c6ed93df0005d32e6d21672
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271790"
---
# <a name="continuous-monitoring-with-azure-monitor"></a>Doorlopende bewaking met Azure Monitor

Doorlopende bewaking verwijst naar het proces en de vereiste op te nemen controle voor elke fase van uw DevOps-technologie en IT operations levenscycli. Zo kunt u ervoor continu de status, prestaties en betrouwbaarheid van uw toepassing en de infrastructuur zoals wordt verplaatst van ontwikkeling naar productie. Doorlopende bewaking bouwt voort op de concepten van continue integratie en continue implementatie (CI/CD) waarmee u ontwikkel en lever software sneller en betrouwbaarder voor continu waarde aan uw gebruikers.

[Azure Monitor](overview.md) is de geïntegreerde oplossing voor controle in Azure met volledige stack observability voor toepassingen en infrastructuur in de cloud en on-premises. Werkt naadloos samen met [Visual Studio en Visual Studio Code](https://visualstudio.microsoft.com/) tijdens het ontwikkelen en testen en kan worden geïntegreerd met [Azure DevOps](/azure/devops/user-guide/index) voor releasebeheer en werkitembeheer tijdens de implementatie en bewerkingen. Het is ook geïntegreerd in de ITSM- en SIEM's van uw keuze om te helpen problemen bijhouden en incidenten binnen uw bestaande IT-processen.

Dit artikel beschrijft de specifieke stappen voor het gebruik van Azure Monitor om continue bewaking in uw werkstromen te schakelen. Het bevat koppelingen naar andere documentatie informatie biedt over het implementeren van verschillende functies.


## <a name="enable-monitoring-for-all-your-applications"></a>Schakel bewaking voor al uw toepassingen
Om te kunnen krijgen observability binnen uw gehele omgeving, moet u inschakelen bewaking op uw web-apps en services. Hiermee kunt u gemakkelijk visualiseren end-to-end-transacties en verbindingen voor alle onderdelen.

- [Met Azure DevOps Projects](../devops-project/overview.md) een vereenvoudigde ervaring met uw bestaande code en Git-opslagplaats, of kies een van de voorbeeldtoepassingen te maken van een pijplijn voor continue integratie (CI) en continue levering (CD) naar Azure.
- [Doorlopende bewaking in uw DevOps-release-pijplijn](../application-insights/app-insights-vsts-continuous-monitoring.md) kunt u poort of terugdraaien van de implementatie aanpassen aan de gegevens te controleren.
- [Statusmonitor](../application-insights/app-insights-monitor-performance-live-website-now.md) kunt u het instrumenteren van een live .NET-app op Windows met Azure Application Insights, zonder te hoeven wijzigen of opnieuw implementeren van uw code.
- Als u toegang tot de code voor uw toepassing hebt, klikt u vervolgens volledige controle inschakelen met [Application Insights](../application-insights/app-insights-overview.md) door het installeren van de Azure Monitor Application Insights SDK voor [.NET](../application-insights/quick-monitor-portal.md), [Java ](../application-insights/app-insights-java-quick-start.md), [Node.js](../application-insights/app-insights-nodejs-quick-start.md), of [andere programmeertalen](../application-insights/app-insights-platforms.md). Hiermee kunt u aangepaste gebeurtenissen, metrische gegevens of paginaweergaven die relevant voor uw toepassing en uw bedrijf zijn op te geven.



## <a name="enable-monitoring-for-your-entire-infrastructure"></a>Schakel bewaking voor uw volledige infrastructuur
Toepassingen zijn alleen zo betrouwbaar is als de onderliggende infrastructuur. Met bewaking ingeschakeld in uw hele infrastructuur, krijgt u volledige observability bereiken en maken het gemakkelijker voor het detecteren van een mogelijke hoofdoorzaak te achterhalen wanneer er iets fout gaat. Azure Monitor kunt u de status en prestaties van uw hele hybride infrastructuur met inbegrip van resources, zoals virtuele machines, containers, opslag en netwerk bijhouden.

- Krijgt u automatisch [platform metrische gegevens, logboeken en logboeken met diagnostische gegevens](platform/data-sources.md) vanaf de meeste van uw Azure-resources waarvoor geen configuratie.
- Inschakelen voor virtuele machines met een diepe bewaking [Azure Monitor voor virtuele machines](insights/vminsights-overview.md).
-  Schakel diepe bewaking voor AKS-clusters met [Azure Monitor voor containers](insights/container-insights-overview.md).
- Voeg [bewakingsoplossingen](insights/solutions-inventory.md) voor verschillende toepassingen en services in uw omgeving.


[Infrastructuur als code](/devops/learn/what-is-infrastructure-as-code) is het beheer van infrastructuur in een beschrijvend model, met behulp van de dezelfde versiebeheer DevOps-teams gebruiken voor de broncode. Betrouwbaarheid en schaalbaarheid toegevoegd aan uw omgeving en kunt u gebruikmaken van dezelfde processen die worden gebruikt voor het beheren van uw toepassingen.

-  Gebruik [Resource Manager-sjablonen](platform/template-workspace-configuration.md) inschakelen bewaking en waarschuwingen configureren via een groot aantal bronnen.
- Gebruik [Azure Policy](../governance/policy/overview.md) om af te dwingen van verschillende regels voor uw resources. Dit zorgt ervoor dat deze resources voldoen aan uw bedrijfsnormen en serviceovereenkomsten blijven. 


##  <a name="combine-resources-in-azure-resource-groups"></a>Resources in Azure-resourcegroepen combineren
Een typische toepassing op Azure vandaag bevat meerdere resources, zoals virtuele machines en Services van de App of microservices die worden gehost op Cloud Services, AKS clusters of Service Fabric. Deze toepassingen maken vaak gebruik van afhankelijkheden, zoals Event Hubs, opslag, SQL en Service Bus.

- Combineer resources inAzure resourcegroepen volledige om inzicht te krijgen in al uw resources die gezamenlijk uw verschillende toepassingen. [Azure Monitor voor resourcegroepen](../monitoring-and-diagnostics/resource-group-insights.md) biedt een eenvoudige manier om de status en prestaties van uw volledige reeks toepassingen met volledige stack en inzoomen op respectieve onderdelen voor onderzoeken of foutopsporing kunt bij te houden.

## <a name="ensure-quality-through-continuous-deployment"></a>Zorg ervoor dat de kwaliteit van continue implementatie
Continue integratie / continue implementatie kunt u automatisch integreren en wijzigingen in de code implementeren in uw toepassing op basis van de resultaten van de geautomatiseerde testen. Het stroomlijnt het implementatieproces en zorgt ervoor dat de kwaliteit van eventuele wijzigingen voordat ze in productie gaan.


- Gebruik [Azure pijplijnen](/azure/devops/pipelines) continue implementatie implementeren en automatiseren van uw hele proces van code doorvoeren naar productie op basis van uw CI/CD-tests.
- Gebruik [kwaliteit Gates](/devops/pipelines/release/approvals/gates) om te integreren in uw vóór de implementatie of na de implementatie controleren. Dit zorgt ervoor dat u voldoet aan de health/prestatie metrische gegevens (KPI's) als uw toepassingen van dev naar productie en eventuele verschillen in de infrastructuur-omgeving verplaatsen of schaal uw KPI's niet nadelig beïnvloedt.
- [Afzonderlijke exemplaren van de controle behouden](../application-insights/app-insights-separate-resources.md) tussen de verschillende omgevingen, zoals ontwikkelen, testen, Canarische en productie. Dit zorgt ervoor dat de verzamelde gegevens relevant voor de gekoppelde toepassingen en infrastructuur is. Als u nodig hebt voor het correleren van gegevens tussen omgevingen, kunt u [meerdere resource-grafieken in Metrics Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md) of maak [cross-resource-query's in Log Analytics](log-query/cross-workspace-query.md).


## <a name="create-actionable-alerts-with-actions"></a>Bruikbare waarschuwingen maken met acties
Een essentieel onderdeel van de bewaking wordt beheerders van actuele en voorspelde problemen proactief hoogte. 

- Maak [waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-alerts.md) op basis van Logboeken en metrische gegevens voor het identificeren van voorspelbare fout Staten. U hebt een doel van het maken van alle waarschuwingen bruikbare wat betekent dat ze werkelijke kritieke situaties vertegenwoordigen en gezocht tot fout-positieven. Gebruik [dynamische drempelwaarden](platform/alerts-dynamic-thresholds.md) automatisch wilt berekenen basislijnen op metrische gegevens in plaats van uw eigen statische drempelwaarden te definiëren. 
- Het definiëren van acties voor waarschuwingen gebruik van de meest efficiënte manier om uw beheerders te informeren. Beschikbare [acties voor melding](platform/action-groups.md#create-an-action-group-by-using-the-azure-portal) SMS, e-mail, pushmeldingen te verzenden of spraakoproepen zijn.
- Gebruik Geavanceerde bewerkingen voor het [verbinding maken met uw ITSM-hulpprogramma](platform/itsmc-overview.md) of andere systemen waarschuwingen te beheren via [webhooks](platform/activity-log-alerts-webhook.md).
- Herstellen van situaties geïdentificeerd in waarschuwingen ook met [Azure Automation-runbooks](../automation/automation-webhooks.md) of [Logic Apps](/connectors/custom-connectors/create-webhook-trigger) die kunnen worden gestart vanuit een waarschuwing met behulp van webhooks. 
- Gebruik [automatisch schalen](../monitoring-and-diagnostics/monitor-tutorial-autoscale-performance-schedule.md) dynamisch te verhogen en verlagen van de compute-resources op basis van verzamelde gegevens.

## <a name="prepare-dashboards-and-workbooks"></a>Werkmappen en dashboards voorbereiden
Ervoor te zorgen dat uw ontwikkel- en operations toegang tot de dezelfde Telemetrie en hulpprogramma's hebben, kunnen ze patronen in uw gehele omgeving weergeven en de gemiddelde tijd om te detecteren (MTTD) en de gemiddelde tijd om te herstellen (MTTR) kunt minimaliseren.

- Voorbereiden [aangepaste dashboards](../application-insights/app-insights-tutorial-dashboards.md) op basis van algemene metrische gegevens en logboeken voor de verschillende rollen in uw organisatie. Dashboards kunnen gegevens uit alle Azure-resources kunnen combineren.
- Voorbereiden [werkmappen](../application-insights/app-insights-usage-workbooks.md) om ervoor te zorgen kennis delen tussen ontwikkel- en bewerkingen. Deze kunnen worden voorbereid als dynamische rapporten met grafieken met metrische gegevens en logboeken-query's, of zelfs als het oplossen van problemen handleidingen voorbereid door ontwikkelaars helpen klantenondersteuning of bewerkingen voor het afhandelen van eenvoudige problemen.

## <a name="continuously-optimize"></a>Blijf optimaliseren
 Bewaking is een van de fundamentele aspecten van de populaire Build-meten-leren filosofie, dat wordt aanbevolen continu bijhouden van uw KPI's en het gedrag van metrische gegevens over gebruikers en vervolgens streven naar ze via planning iteraties te optimaliseren. Azure Monitor kunt u het verzamelen van metrische gegevens en logboeken die relevant zijn voor uw bedrijf en toe te voegen nieuwe gegevenspunten in de volgende implementatie zo nodig.

- Gebruik hulpprogramma's in Application Insights aan [gedrag van eindgebruikers en engagement bijhouden](../application-insights/app-insights-tutorial-users.md).
- Gebruik [impactanalyse](../application-insights/app-insights-usage-impact.md) kunt u welke onderdelen zich richten op station om belangrijke KPI's te prioriteren.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de onderdelen van het verschil van [Azure Monitor](overview.md).
- [Doorlopende bewaking toevoegen](../application-insights/app-insights-vsts-continuous-monitoring.md) aan uw release-pijplijn.
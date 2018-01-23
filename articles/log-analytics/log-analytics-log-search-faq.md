---
title: Log Analytics nieuwe logboek zoekopdracht Veelgestelde vragen | Microsoft Docs
description: Dit artikel bevat veelgestelde vragen met betrekking tot de upgrade van logboekanalyse op de nieuwe querytaal.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 6dfee26d7585c8ec295a1f0ea1bd0bc14a34cc5a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Meld u nieuwe logboekanalyse Veelgestelde vragen over het zoeken en bekende problemen

In dit artikel bevat veelgestelde vragen en bekende problemen met betrekking tot de upgrade van [logboekanalyse naar de nieuwe querytaal](log-analytics-log-search-upgrade.md).  U moet dit gehele artikel doorlezen voordat u de beslissing om bij te werken uw werkruimte.


## <a name="alerts"></a>Waarschuwingen

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Vraag: ik heb een groot aantal regels voor waarschuwingen. Moet ik ze opnieuw maken in de nieuwe taal nadat ik een upgrade uitvoeren?  
Nee, de regels voor waarschuwingen automatisch geconverteerd naar de nieuwe zoekopdracht taal tijdens de upgrade.  

### <a name="question-i-have-alert-rules-with-webhook-and-runbook-actions-will-these-continue-to-work-when-i-upgrade"></a>Vraag: ik heb waarschuwingsregels met webhook en runbook-acties. Blijft deze werken wanneer ik een upgrade?

Er zijn geen, enkele wijzigingen in de webhook en runbook acties waardoor u wijzigingen aanbrengen van hoe het verwerken van de nettolading van de. We hebt deze wijzigingen in de verschillende uitvoerindelingen standaardiseren en Reduceer de grootte van de nettolading van de aangebracht. Meer informatie over deze indelingen [acties toevoegen aan de regels voor waarschuwingen in logboekanalyse](log-analytics-alerts-actions.md).


## <a name="computer-groups"></a>Computergroepen

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Vraag: Ik krijg fouten bij het gebruik van computergroepen.  Is de syntaxis ervan gewijzigd?
Ja, de syntaxis voor het gebruik van de computer wijzigingen groups wanneer uw werkruimte wordt bijgewerkt.  Zie [computergroepen in logboekanalyse Meld zoekopdrachten](log-analytics-computer-groups.md) voor meer informatie.


## <a name="dashboards"></a>Dashboards

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Vraag: Kan ik nog steeds gebruiken dashboards in een bijgewerkte werkruimte?
Met de upgrade, begin van het proces van het depracating **mijn Dashboard**.  U kunt blijven gebruiken alle tegels die u hebt toegevoegd aan het dashboard voordat uw werkruimte is bijgewerkt, maar u niet kunt bewerken die tegels of Voeg nieuwe toe.  U kunt blijven maken en bewerken van weergaven met [ontwerper](log-analytics-view-designer.md), die beschikt over een rijkere functie ingesteld en ook dashboards maken in de Azure portal.


## <a name="log-searches"></a>Logboek zoekopdrachten

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Vraag: ik hebt opgeslagen zoekopdrachten buiten mijn bijgewerkte werkruimte. Kan ik converteren ze naar de nieuwe zoekopdracht taal automatisch?
Kunt u het conversiehulpprogramma taal in de zoekpagina logboek converteren van elkaar.  Er is geen methode om automatisch te converteren meerdere zoekacties zonder een upgrade van de werkruimte.

### <a name="question-why-are-my-query-results-not-sorted"></a>Vraag: Waarom wordt mijn queryresultaten niet is gesorteerd?
Resultaten worden niet standaard in de nieuwe querytaal gesorteerd.  Gebruik de [sorteren operator](https://go.microsoft.com/fwlink/?linkid=856079) uw resultaten sorteren op een of meer eigenschappen.

### <a name="question-where-did-the-metrics-view-go-after-i-upgraded"></a>Vraag: Waar is de weergave van de metrische gegevens gebleven na een upgrade
De weergave van de metrische gegevens heeft een grafische weergave van prestatiegegevens van een zoekopdracht logboek.  Deze weergave is niet meer beschikbaar na de upgrade.  U kunt de [operator renderen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) voor uitvoer van de indeling van een query in een timechart.

### <a name="question-where-did-minify-go-after-i-upgraded"></a>Vraag: Waar minify Ga nadat ik bijgewerkt?
Minify is een functie waarmee een samengevatte weergave van de zoekresultaten.  Na de upgrade, wordt de optie Minify niet meer weergegeven in de portal logboek zoeken.  U kunt vergelijkbare functionaliteit met de nieuwe zoekopdracht taal via ophalen [verminderen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/reduce-operator) of [autocluster_v2](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/evaluate-operator/autocluster). 

    Event
    | where TimeGenerated > ago(10h)
    | reduce by RenderedDescription

    Event
    | where TimeGenerated > ago(10h)
    | evaluate autocluster_v2()



## <a name="log-search-api"></a>API voor zoeken in logboeken

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Vraag: De Search-API van het logboek wordt bijgewerkt nadat ik een upgrade uitvoeren?
Het verouderde [Log-API van zoekservice](log-analytics-log-search-api.md) werkt niet meer bij de upgrade van uw werkruimte.  Zie [REST-API van Azure Log Analytics](https://dev.loganalytics.io/) voor meer informatie over de nieuwe API.


## <a name="portals"></a>Portals

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Vraag: Moet ik gebruiken de nieuwe Advanced Analytics-portal of houden via de portal logboek zoeken?
U ziet een vergelijking van de twee portals op [Portals voor het maken en bewerken van logboek-query's in Azure-logboekanalyse](log-analytics-log-search-portals.md).  Elk heeft verschillende voordelen, zodat u de beste is voor uw behoeften kunt kiezen.  Het is gebruikelijk om te schrijven van query's in de portal Advanced Analytics en plak ze in andere locaties zoals Designer bekijken.  Lees over [overwegingen](log-analytics-log-search-portals.md#advanced-analytics-portal) die bij het uitvoeren.


### <a name="question--after-upgrade-i-get-an-error-trying-to-run-queries-and-am-also-seeing-errors-in-my-views"></a>Vraag: Na de upgrade zijn er een fout opgetreden bij het uitvoeren van query's en krijg fouten ook zien in Mijn weergaven.

Uw browser vereist toegang tot de volgende adressen Log Analytics-query's uitvoeren na de upgrade.  Als uw browser de Azure-portal via een firewall benaderen is, moet u toegang tot deze adressen inschakelen.

| URI | IP | Poorten |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80,443 |
| api.loganalytics.io    | Dynamisch | 80,443 |
| docs.loganalytics.io   | Dynamisch | 80,443 |



## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Vraag: Verandert met Power BI-integratie
Ja.  Zodra uw werkruimte is bijgewerkt en vervolgens het proces voor het exporteren van logboekanalyse gegevens met Power BI niet meer werken.  Eventuele bestaande schema's die u hebt gemaakt voordat u de upgrade wordt uitgeschakeld.  

Na de upgrade, wordt Azure Log Analytics hetzelfde platform als Application Insights en u hetzelfde proces gebruiken om te exporteren Log Analytics-query's naar Power BI als [het proces voor het exporteren van Application Insights-query's naar Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Exporteren naar Power BI nu aanroepen rechtstreeks de API-eindpunt. Hiermee kunt u tot 500.000 rijen of 64,000,000 bytes aan gegevens ophalen, lange query's te exporteren en aanpassen van de time-out van de query (standaardtime-out is 3 minuten en de maximale time-out is 10 minuten).

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Vraag: De logboek zoeken PowerShell-cmdlet wordt bijgewerkt nadat ik een upgrade uitvoeren?
De [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) afgeschaft wanneer de upgrade van alle werkruimten voltooid is.  Gebruik de [cmdlet Invoke-LogAnalyticsQuery](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets) logboek zoekopdrachten uitvoeren in de bijgewerkte werkruimten.




## <a name="resource-manager-templates"></a>Resource Manager-sjablonen

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Vraag: Maak ik een bijgewerkte werkruimte met Resource Manager-sjabloon?
Ja.  U moet gebruiken een API-versie van 2017-03-15-preview en bevatten een **functies** sectie in uw sjabloon zoals in het volgende voorbeeld.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Oplossingen

### <a name="question-will-my-solutions-continue-to-work"></a>Vraag: Mijn oplossingen blijven werken?
Alle oplossingen blijven werken in een werkruimte bijgewerkte, hoewel de prestaties verbeteren als ze worden geconverteerd naar de nieuwe querytaal.  Er zijn bekende problemen met het aantal bestaande oplossingen die in deze sectie worden beschreven.

### <a name="known-issue-perspectives-in-application-insights-connector"></a>Bekende probleem: perspectieven in Application Insights-connector
Perspectieven in [Application Insights-Connector oplossing](log-analytics-app-insights-connector.md) worden niet meer ondersteund in de Application Insights-connector-oplossing.  Ontwerp weergeven kunt u aangepaste weergaven maken met Application Insights-gegevens.

### <a name="known-issue-backup-solution"></a>Bekende probleem:-back-upoplossing
De oplossing voor back-up kan niet worden verzameld als vóór de upgrade van een werkruimte is geïnstalleerd. Verwijder de oplossing en installeer vervolgens de nieuwste versie.  De nieuwe versie van de oplossing biedt geen ondersteuning voor klassieke Backup-kluizen, dus u ook naar de Recovery Services-kluizen upgraden moet wil blijven gebruiken van de oplossing.

## <a name="upgrade-process"></a>Het upgradeproces

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Vraag: ik heb meerdere werkruimten. Kan ik alle werkruimten op hetzelfde moment bijwerken?  
Nee.  Upgrade van toepassing op een enkele werkruimte telkens. Er is momenteel niet mogelijk van de upgrade van veel werkruimten tegelijk. Houd er rekening mee dat andere gebruikers van de bijgewerkte werkruimte ook worden beïnvloed.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Vraag: Bestaande logboekgegevens verzameld in mijn werkruimte worden aangepast en zullen als ik een upgrade uitvoeren?  
Nee. De logboekgegevens die beschikbaar zijn voor uw werkruimte-zoekopdrachten wordt niet beïnvloed door de upgrade. Opgeslagen zoekopdrachten, wordt waarschuwingen, weergaven en automatisch worden geconverteerd naar de nieuwe zoekopdracht taal.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Vraag: Wat gebeurt er als ik mijn werkruimte niet bijwerken?  
De verouderde logboek zoekopdracht wordt in de komende maanden afgeschaft. Werkruimten die niet zijn bijgewerkt op dat moment worden automatisch geüpgraded.

### <a name="question-can-i-revert-back-after-i-upgrade"></a>Vraag: Kan ik herstellen nadat ik een upgrade uitvoeren?
Vóór de algemene beschikbaarheid kan u uw werkruimte herstellen na een upgrade.  Nu dat de nieuwe taal algemene beschikbaarheid bereikt heeft, is deze mogelijkheid verwijderd omdat we beginnen met het buiten gebruik stellen van de verouderde platform.


## <a name="views"></a>Weergaven

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Vraag: Hoe maak ik een nieuwe weergave met Designer bekijken?
U kunt voorafgaande aan de upgrade een nieuwe weergave maken met ontwerper van een tegel op het hoofddashboard in de OMS-portal.  Wanneer uw werkruimte is geüpgraded, is deze tegel wordt verwijderd.  U kunt een nieuwe weergave maken met de Designer bekijken in de OMS-portal door te klikken op de groene + knop in het menu links.  U doorgaan met het maken van een nieuwe weergave met de Azure-portal door te klikken op de tegel Designer bekijken.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [uw werkruimte upgraden naar de nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md).

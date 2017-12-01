---
title: Wat er gewijzigd in Azure Log Analytics? | Microsoft Docs
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
ms.date: 11/29/2017
ms.author: bwren
ms.openlocfilehash: 017a1da233827f19489a99b234ee9009fd9f6fe3
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="whats-changed-in-azure-log-analytics"></a>Wat er gewijzigd in Azure Log Analytics?
Naast de querytaal zelf, zijn er verschillende verbeteringen en wijzigingen die u rekening moet houden wanneer uw werkruimte voor logboekanalyse is [bijgewerkt naar de nieuwe querytaal](log-analytics-log-search-new.md).  In dit artikel worden de wijzigingen tussen een verouderde en bijgewerkte werkruimte met koppelingen naar gedetailleerde informatie voor elke kort beschreven. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Whats-changed-in-Azure-Log-Analytics/player]

Zie [logboekanalyse nieuwe veelgestelde vragen over het zoeken en bekende problemen melden](log-analytics-log-search-faq.md) voor een beschrijving van de bekende problemen met de upgrade en antwoorden op veelgestelde vragen.  

## <a name="query-language"></a>Querytaal
De primaire wijziging in de Log Analytics-upgrade is de nieuwe querytaal die belangrijke verbeteringen via de vorige taal heeft.  

U krijgt een directe vergelijking van algemene bewerkingen tussen de verouderde taal en de nieuwe taal op [overstappen naar de nieuwe querytaal Azure Log Analytics](log-analytics-log-search-transition.md).  Volledige documentatie over de nieuwe taal is beschikbaar op [Azure Log Analytics Query Language](https://docs.loganalytics.io).


## <a name="computer-groups"></a>Computergroepen
De methode voor het maken van een computergroep is niet gewijzigd, maar u een unieke alias nu voor elk opgeven moet.  Computergroepen op basis van het logboek zoekacties moeten de syntaxis van de nieuwe taal gebruiken.

Er is een nieuwe syntaxis voor het gebruik van computergroepen in een logboek zoekopdracht.  In plaats van de functie $ComputerGroups, zijn computergroepen nu elk geïmplementeerd als een functie met een unieke alias.  U kunt de alias gebruiken in het logboek zoeken zoals een andere functie.  

Details zijn beschikbaar op [computergroepen in logboekanalyse Meld zoekopdrachten](log-analytics-computer-groups.md).


## <a name="log-search-portals"></a>Portals voor zoekopdrachten in logboeken
Naast de zoekfunctie portal maken en logboek zoekopdrachten uitvoeren, kunt u nu de Advanced Analytics-portal waardoor aanzienlijk verbeterde functies gebruiken.

Een korte beschrijving van de twee portals is beschikbaar op [Portals voor het maken en bewerken van logboek-query's in Azure-logboekanalyse](log-analytics-log-search-portals.md).  U kunt een zelfstudie doorlopen op de nieuwe portal op [aan de slag met de Portal Analytics](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal).

## <a name="alerts"></a>Waarschuwingen
Waarschuwingen werkt hetzelfde als in bijgewerkte werkruimten, maar de query waarmee ze worden uitgevoerd in de nieuwe taal moet worden geschreven.  Eventuele bestaande waarschuwingsregels die u vóór de upgrade had, automatisch worden geconverteerd naar de nieuwe taal.  U kunt meer informatie krijgen in [inzicht-waarschuwingen in logboekanalyse](log-analytics-alerts.md).

De indeling van de nettolading voor runbooks en webhooks verzonden van waarschuwingen is gewijzigd.  Informatie ophalen voor de nieuwe indeling van nettolading in [acties toevoegen aan de regels voor waarschuwingen in logboekanalyse](log-analytics-alerts-actions.md).

## <a name="dashboards"></a>Dashboards
[Dashboards](log-analytics-dashboards.md) zijn afgeschaft.  U kunt blijven gebruiken alle tegels die u hebt toegevoegd aan het dashboard voordat uw werkruimte is bijgewerkt, maar u niet kunt bewerken die tegels of Voeg nieuwe toe.  Weergave Designer gebruiken om aangepaste weergaven met een uitgebreidere set functies dan dashboards te maken.

Meer informatie over de ontwerper zijn beschikbaar op [ontwerper gebruiken voor het maken van aangepaste weergaven in logboekanalyse](log-analytics-view-designer.md).

## <a name="power-bi"></a>Power BI
Het proces voor het exporteren van logboekanalyse gegevens met Power BI is gewijzigd voor bijgewerkte werkruimten en eventuele bestaande schema's die u hebt gemaakt voordat u de upgrade zal worden uitgeschakeld.  

Details zijn beschikbaar op [exporteren Log Analytics-gegevens met Power BI](log-analytics-powerbi.md).

## <a name="powershell"></a>PowerShell
De Get-AzureRmOperationalInsightsSearchResults voor het logboek zoekopdrachten uitvoeren vanuit PowerShell werkt niet met een bijgewerkte werkruimte.  Gebruik Invoke-LogAnalyticsQuery voor deze functionaliteit aan een bijgewerkte werkruimte.

Details zijn beschikbaar op [Azure Log Analytics REST-API - PowerShell-Cmdlets](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets).

## <a name="log-search-api"></a>Meld u API van zoekservice
De zoekopdracht logboek REST-API is gewijzigd en oplossingen die gebruikmaken van de oudere versie moeten worden bijgewerkt voor het gebruik van de nieuwe versie van de API.   

Meer informatie over de nieuwe versie van de API zijn beschikbaar op [REST-API van Azure Log Analytics](https://dev.loganalytics.io/).

## <a name="next-steps"></a>Volgende stappen

- Zie [logboekanalyse nieuwe veelgestelde vragen over het zoeken en bekende problemen melden](log-analytics-log-search-faq.md) voor een beschrijving van de bekende problemen met de upgrade en antwoorden op veelgestelde vragen.
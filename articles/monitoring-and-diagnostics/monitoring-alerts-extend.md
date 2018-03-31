---
title: Waarschuwingen van OMS in Azure - overzicht uitbreiden | Microsoft Docs
description: Overzicht van proces voor het verlengen van waarschuwingen van OMS in Azure, waarschuwingen, informatie over algemene klant problemen.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: abab73a047fb923fa2cd57a1c7194e440697bf64
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="extend-alerts-from-oms-into-azure"></a>Waarschuwingen van OMS uitbreiden naar Azure
De nieuwe waarschuwingen is nu geïntegreerd ervaring voor de waarschuwingen over verschillende services en onderdelen in Microsoft Azure. De nieuwe ervaring beschikbaar als **waarschuwingen** onder Azure Monitor in de Portal, heeft samengebracht in een algemene plaats - activiteit logboek waarschuwingen, metrische waarschuwingen en logboek waarschuwingen op Log Analytics, evenals een Application Insights. 

Maar voor sommige gebruikers, het gebruik van logboekanalyse en allied functionaliteit, zoals waarschuwingen, is via [bewerking Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). En daarom zodat ze eenvoudig beheren hun andere Azure-resources samen met het gebruik van Log Analytics - systematischer Microsoft heeft is ervoor te zorgen dat de mogelijkheden van OMS ook beschikbaar in Azure-portal zijn. In dat opzicht waarschuwingen van Azure kunnen al gebruikers voor het beheren van waarschuwingen op basis van een query voor Log Analytics voor meer informatie raadpleegt u [Meld u waarschuwingen op waarschuwingen van Azure](monitor-alerts-unified-log.md). Waarschuwingen onder Azure Monitor worden waarschuwingen die zijn gemaakt in OMS al vermeld onder juiste log analytics-werkruimte. Maar een bewerken of wijzigen van deze waarschuwingen die zijn gemaakt in OMS, moet de gebruiker te laten Azure en gebruiken van OMS; keer vervolgens terug Terug naar Azure wanneer dit nodig is voor het beheren van een andere service. Microsoft is als u deze problemen, nu zodat gebruikers kunnen hun waarschuwingen van OMS uitbreiden naar Azure.

## <a name="benefits-of-extending-your-alerts"></a>Voordelen van het uitbreiden van uw waarschuwingen
Naast het voordeel dat is samengevoegd in dat u niet hoeft te navigeren buiten het Azure-portal, zijn er andere voordelen van de meest kenmerkende in waarschuwingen van OMS uit te breiden naar Azure

- In tegenstelling tot in OMS, waarbij alleen 250 waarschuwingen kunnen worden gemaakt en weergegeven; in Azure waarschuwingen is deze beperking niet aanwezig
- Van waarschuwingen van Azure, worden uw Waarschuwingstypen beheerd, opgesomd en weergegeven; niet alleen logboekanalyse waarschuwingen, zoals het geval met OMS
- Waarschuwingen van Azure gebruikmaken [actiegroepen](monitoring-action-groups.md), waarmee u kunt het hebben van meer dan één actie op voor elke waarschuwing inclusief SMS, telefoongesprek, Automation-Runbook, Webhook, ITSM Connector en meer. Terwijl de OMS-waarschuwingen zijn beperkt in zowel het nummer ook als acties mogelijk type

## <a name="process-of-extending-your-alerts"></a>Proces voor het verlengen van uw waarschuwingen
Het proces voor het uitbreiden van waarschuwingen van OMS in Azure, **niet** waarbij de meldingsdefinitie, een query of een configuratie op een manier wordt gewijzigd. De enige wijziging vereist is in Azure worden alle acties zoals het e-mailmeldingen worden webhook-aanroep automation-runbook wordt uitgevoerd of verbinding te maken met het hulpprogramma ITSM gedaan via de actie-groep. Daarom als groep passende maatregelen gekoppeld aan uw waarschuwing zijn - zal ze worden uitgebreid in Azure.

Omdat het proces voor het uitbreiden van niet-destructieve en niet interruptive, Microsoft waarschuwingen in Operations Management Suite (OMS) automatisch gemaakt voor waarschuwingen van Azure - vanaf wordt uitbreiden **23 April 2018**. Vanaf deze dag wordt Microsoft begint met het plannen van de waarschuwingen uit te breiden naar Azure en geleidelijk alle waarschuwingen aanwezig is in OMS beheerd via Azure portal. 

Wanneer de waarschuwingen in een OMS-werkruimte ophalen gepland voor de uit te breiden naar Azure, ze blijven werken en wordt **niet** inbreuk op een manier de bewaking. Bij het plannen, uw waarschuwingen mogelijk niet beschikbaar voor het bewerken van wijziging/tijdelijk. maar nieuwe waarschuwingen van Azure worden gemaakt in deze korte tijd kunnen worden voortgezet. In deze korte periode, als u geen bewerken of maken van waarschuwing van OMS doet, hebben gebruikers de optie om door te gaan naar de Azure Log Analytics of Azure-waarschuwingen.

 ![Tijdens de geplande periode, in te grijpen op waarschuwingen omgeleid naar Azure](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Waarschuwingen van Operations Management Suite (OMS) uitbreiden naar Azure is niet in rekening gebracht en informatie over het gebruik van Azure waarschuwingen voor de query op basis van logboekanalyse waarschuwingen wordt niet in rekening gebracht, wanneer gebruikt binnen de grenzen en voorwaarden die worden vermeld in de [prijsbeleid Azure-Monitor](https://azure.microsoft.com/en-us/pricing/details/monitor/)  

Gebruikers kunnen profiteren van de voordelen van het uitbreiden van waarschuwingen voor deze datum; door vrijwillig uitschakelen zodat hun waarschuwingen beheren in Azure.

### <a name="how-to-voluntarily-extending-your-alerts"></a>Hoe uw waarschuwingen vrijwillig uitbreiden
Microsoft heeft om OMS gebruikers een gemakkelijk op te geven in waarschuwingen van Azure, hulpprogramma's voor het uitbreiden van de waarschuwingen worden gemaakt. Klanten van Microsoft Operations Management Suite (OMS) kunnen hun waarschuwingen naar Azure uitbreiden van een wizard in de OMS-portal (of) door een programmatische aanpak met behulp van een nieuwe API. Zie voor meer informatie [waarschuwingen uit te breiden naar Azure met OMS-portal en API](monitoring-alerts-extend-tool.md).


## <a name="usage-after-extending-your-alerts"></a>Gebruik na het uitbreiden van uw waarschuwingen
Zoals gezegd, wordt waarschuwingen die zijn gemaakt in Microsoft bewerking Management Suite uitgebreid naar Azure waarschuwingen; na, waarmee u ze van Azure beheren kunt. Waarschuwingen blijven worden vermeld in de OMS-portal - waarschuwingsinstelling-sectie. Zoals hieronder weergegeven:

 ![Waarschuwingen voor vermeldingen na wordt uitgebreid naar Azure OMS-Portal](./media/monitor-alerts-extend/PostExtendList.png)

Voor een bewerking van waarschuwingen, zoals bewerken of maken in de OMS-portal uitgevoerd gebruikers transparant omgeleid naar Azure-waarschuwingen. Waarschuwing blijven maken van de bestaande [Log Analytics-API](../log-analytics/log-analytics-api-alerts.md) als eerdere, met slechts kleine wijziging is dat de nadat er waarschuwingen worden uitgebreid naar Azure - actiegroepen moet worden gekoppeld in de planning.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over van de hulpprogramma's om [initiëren waarschuwingen van OMS uit te breiden naar Azure](monitoring-alerts-extend-tool.md)
* Meer informatie over de nieuwe [waarschuwingen van Azure-ervaring](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen melden in Azure waarschuwingen](monitor-alerts-unified-log.md).

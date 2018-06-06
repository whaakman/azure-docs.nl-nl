---
title: Waarschuwingen (kopiëren) logboekanalyse uitbreiden naar Azure waarschuwingen - overzicht | Microsoft Docs
description: Overzicht van waarschuwingen van kopiëren logboekanalyse in OMS-portal naar Azure-waarschuwingen met details adressering algemene vragen van klanten.
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
ms.date: 05/24/2018
ms.author: vinagara
ms.openlocfilehash: 296bdc6939ef8ab67656e5805cd603b50bce211e
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763150"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Log Analytics waarschuwingen uitbreiden naar Azure-waarschuwingen
Tot voor kort opgenomen Azure Log Analytics eigen waarschuwing-functionaliteit kunt u voorwaarden op basis van gegevens van logboekanalyse proactief waarschuwen.  Beheer van regels voor waarschuwingen zijn uitgevoerd in [bewerking Management Suite (OMS) portal](../operations-management-suite/operations-management-suite-overview.md). De nieuwe waarschuwingen is nu geïntegreerd ervaring voor de waarschuwingen in verschillende services in Microsoft Azure. De nieuwe ervaring is beschikbaar als **waarschuwingen** onder Azure Monitor in de Azure portal en biedt ondersteuning voor waarschuwingen van activiteitenlogboeken, metrische gegevens, en registreert van zowel Log Analytics en Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Voordelen van het uitbreiden van uw waarschuwingen
Er zijn enkele voordelen van het maken en beheren van waarschuwingen in de Azure-poort, zoals:

- In tegenstelling tot in de OMS-portal, waarbij alleen 250 waarschuwingen kunnen worden gemaakt en weergegeven; in Azure waarschuwingen is deze beperking niet aanwezig
- Van Azure-waarschuwingen kunnen uw Waarschuwingstypen worden beheerd, opgesomd en weergegeven; niet alleen logboekanalyse waarschuwingen, zoals het geval eerder was
- Toegang tot gebruikers alleen bewaking en waarschuwingen, met behulp van [Azure Monitor rol](monitoring-roles-permissions-security.md)
- Waarschuwingen van Azure gebruikmaken [actiegroepen](monitoring-action-groups.md), welke beschikt u over meer dan één actie op voor elke waarschuwing SMS, inclusief een telefoongesprek te verzenden, een Automation-Runbook aanroepen, een Webhook aanroepen, configureren van een Connector ITSM en meer. 

## <a name="process-of-extending-your-alerts"></a>Proces voor het verlengen van uw waarschuwingen
Het proces van het verplaatsen van waarschuwingen van logboekanalyse in Azure waarschuwingen, biedt **niet** waarbij de meldingsdefinitie, een query of een configuratie op een manier wordt gewijzigd. De enige wijziging vereist is dat in Azure, alle acties zoals het e-mailmeldingen, een webhook-aanroep, een Automation-runbook wordt uitgevoerd of verbinding te maken met het hulpprogramma ITSM worden uitgevoerd met behulp van een groep in te grijpen. Als het actiegroepen zijn al gekoppeld aan de waarschuwing: ze worden opgenomen wanneer uitgebreid in Azure.

> [!NOTE]
> Waarschuwingen die zijn gemaakt in logboekanalyse te starten op waarschuwingen van Azure wordt automatisch uitbreiden door Microsoft **14 mei 2018** in een reeks terugkerende totdat voltooid. Als er problemen bij het maken van zijn [actiegroepen](monitoring-action-groups.md), kunnen gebruikers gebruiken [herstelstappen vermeld](monitoring-alerts-extend-tool.md#troubleshooting) tot **5 juli 2018** ophalen van actiegroepen automatisch gemaakt. 
> 

Wanneer de waarschuwingen in een werkruimte voor logboekanalyse voor het uitbreiden naar Azure zijn gepland, ze blijven werken en wordt **niet** inbreuk op een manier uw configuratie. Bij het plannen, uw waarschuwingen mogelijk niet beschikbaar voor het bewerken van wijziging/tijdelijk. maar nieuwe waarschuwingen van Azure kunnen blijven tijdens deze periode worden gemaakt. Als u probeert te bewerken of maken van waarschuwingen van de OMS-portal, hebt u de mogelijkheid om te blijven ze worden gemaakt vanuit de werkruimte voor logboekanalyse of vanuit Azure waarschuwingen in de Azure portal.

 ![Tijdens de geplande periode, in te grijpen op waarschuwingen omgeleid naar Azure](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> waarschuwingen van logboekanalyse uitbreiden naar Azure biedt niet worden kosten in rekening voor uw account en het gebruik van waarschuwingen van Azure voor query op basis van logboekanalyse waarschuwingen niet in rekening gebracht wordt bij gebruik binnen de grenzen en voorwaarden worden vermeld in de [prijsbeleid Azure-Monitor](https://azure.microsoft.com/pricing/details/monitor/)  

U kunt profiteren van de voordelen van het uitbreiden van waarschuwingen voor deze datum door vrijwillig te verplaatsen naar Azure-waarschuwingen uitschakelen.

### <a name="how-to-voluntarily-extend-your-alerts"></a>Het uitbreiden van uw waarschuwingen vrijwillig
Als u wilt uw waarschuwingen uitbreiden naar Azure-waarschuwingen, hebben we twee methoden voor het voltooien van deze taak in uw werkruimte opgenomen.  U kunt dit vanuit een wizard die beschikbaar zijn in de OMS-portal of programmatisch met behulp van een nieuwe API doen.  Zie voor meer informatie [waarschuwingen uit te breiden naar Azure met OMS-portal en API](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Ondervindt na het uitbreiden van uw waarschuwingen
Nadat uw waarschuwingen worden uitgebreid tot de Azure-waarschuwingen, blijven ze beschikbaar zijn in de OMS-portal voor het beheer niet anders dan voorheen.<br><br> ![Waarschuwingen voor vermeldingen na wordt uitgebreid naar Azure OMS-Portal](./media/monitor-alerts-extend/PostExtendList.png)

Wanneer u probeert een bestaande waarschuwing bewerken of maken van een nieuwe waarschuwing in de OMS-portal, wordt u automatisch omgeleid naar Azure-waarschuwingen.  

> [!NOTE]
> Het is nodig om te controleren of de machtigingen worden toegewezen aan personen die u wilt toevoegen of bewerken van waarschuwingen correct zijn toegewezen in Azure.  Overzicht, [machtigingen voor het gebruik van Azure bewaken en waarschuwingen](monitoring-roles-permissions-security.md) om te begrijpen welke machtigingen u moet geven.  
> 

Een waarschuwing gemaakt blijven werken vanuit de [Log Analytics-API](../log-analytics/log-analytics-api-alerts.md) en [Log Analytics Resource sjabloon](../monitoring/monitoring-solutions-resources-searches-alerts.md), met slechts één kleine wijzigen moet worden toegepast - actiegroepen moeten worden opgenomen.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de hulpprogramma's om [initiëren waarschuwingen van logboekanalyse uit te breiden naar Azure](monitoring-alerts-extend-tool.md)
* Meer informatie over de nieuwe [waarschuwingen van Azure-ervaring](monitoring-overview-unified-alerts.md).
* Meer informatie over het maken van [waarschuwingen melden in Azure waarschuwingen](monitor-alerts-unified-log.md).

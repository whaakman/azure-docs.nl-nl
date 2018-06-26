---
title: Waarschuwingen (kopiëren) logboekanalyse uitbreiden naar Azure waarschuwingen - overzicht
description: Overzicht van waarschuwingen van kopiëren logboekanalyse in OMS-portal naar Azure-waarschuwingen met details adressering algemene vragen van klanten.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6484142eafa8388117c1e96ab31eefeab188e488
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750269"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Log Analytics waarschuwingen uitbreiden naar Azure-waarschuwingen
Tot voor kort opgenomen Azure Log Analytics eigen waarschuwing-functionaliteit kunt u voorwaarden op basis van gegevens van logboekanalyse proactief waarschuwen. Regels voor waarschuwingen in de Microsoft Operations Management Suite-portal worden beheerd. De nieuwe waarschuwingen is nu geïntegreerd waarschuwingen in verschillende services in Microsoft Azure. Deze optie is beschikbaar als **waarschuwingen** onder Azure Monitor in de Azure portal en biedt ondersteuning voor waarschuwingen van activiteitenlogboeken, metrische gegevens, en registreert van logboekanalyse zowel Azure Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Voordelen van het uitbreiden van uw waarschuwingen
Er zijn enkele voordelen van het maken en het beheren van waarschuwingen in de Azure portal, zoals:

- In tegenstelling tot in de Operations Management Suite-portal, waarbij kunnen alleen 250 waarschuwingen worden gemaakt en weergegeven, heeft Azure waarschuwingen beperking bestaat niet.
- Van waarschuwingen van Azure, kunt u beheren, opsommen en uw Waarschuwingstypen weergeven. Eerder kon alleen doet u dat voor logboekanalyse waarschuwingen.
- U kunt toegang beperken tot gebruikers alleen bewaking en waarschuwingen, met behulp van de [Azure Monitor rol](monitoring-roles-permissions-security.md).
- In Azure-waarschuwingen kunt u [actiegroepen](monitoring-action-groups.md). Hiermee kunt u meer dan één actie op voor elke waarschuwing hebben. U kunt SMS, een telefoongesprek te verzenden, een Azure Automation-runbook aanroepen, een webhook aanroepen en configureren van een Connector IT Service Management (ITSM). 

## <a name="process-of-extending-your-alerts"></a>Proces voor het verlengen van uw waarschuwingen
Het wijzigen van de meldingsdefinitie, een query of een configuratie op een manier wordt geen gebruikgemaakt van het proces van het verplaatsen van waarschuwingen van logboekanalyse in Azure-waarschuwingen. De enige wijziging vereist is dat in Azure kunt u alle acties uitvoeren met behulp van een groep in te grijpen. Als het actiegroepen zijn al gekoppeld aan de waarschuwing, ze worden opgenomen wanneer uitgebreid in Azure.

> [!NOTE]
> Microsoft wordt automatisch gemaakt in logboekanalyse tot Azure-waarschuwingen, waarschuwingen uitbreiden van 14 mei 2018, gestart in een reeks terugkerende totdat voltooid. Als u eventuele problemen ondervindt bij het maken van [actiegroepen](monitoring-action-groups.md), gebruik [deze herstelstappen](monitoring-alerts-extend-tool.md#troubleshooting) ophalen van actiegroepen automatisch gemaakt. Tot 5 juli 2018 kunt u deze stappen. 
> 

Wanneer u waarschuwingen in een werkruimte voor logboekanalyse worden uitgebreid naar Azure plant, blijven ze werken, en niet in dat een manier gevaar brengen van uw configuratie. Bij het plannen, uw waarschuwingen mogelijk niet beschikbaar voor wijziging tijdelijk, maar u kunt doorgaan met het maken van nieuwe Azure-waarschuwingen gedurende deze tijd. Als u probeert te bewerken of maken van waarschuwingen van de Operations Management Suite-portal, hebt u de optie om door te gaan maken van deze van uw werkruimte voor logboekanalyse. U kunt ook om ze te maken van Azure-waarschuwingen in de Azure portal.

 ![Schermafbeelding van de optie voor het maken van waarschuwingen van Log Analytics of Azure-waarschuwingen](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Waarschuwingen van logboekanalyse uitbreiden naar Azure biedt niet worden kosten in rekening met uw account. Met behulp van Azure-waarschuwingen voor logboekanalyse waarschuwingen op basis van een query wordt niet in rekening gebracht bij gebruik binnen de grenzen en voorwaarden worden vermeld in de [Azure Monitor prijsbeleid](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Het uitbreiden van uw waarschuwingen vrijwillig
Als u wilt uw waarschuwingen uitbreiden naar Azure-waarschuwingen, kunt u een wizard beschikbaar in de Operations Management Suite-portal of u dit programmatisch kunt doen met behulp van een API. Zie voor meer informatie [waarschuwingen uit te breiden naar Azure met behulp van Operations Management Suite-portal en API](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Ondervindt na het uitbreiden van uw waarschuwingen
Nadat uw waarschuwingen worden uitgebreid tot de Azure-waarschuwingen, blijven ze beschikbaar in de Operations Management Suite-portal voor het beheer niet anders dan voorheen.

![Schermopname van Operations Management Suite-portal met waarschuwingen weergegeven](./media/monitor-alerts-extend/PostExtendList.png)

Wanneer u probeert te bewerken van een bestaande waarschuwing of maak een nieuwe waarschuwing in de Operations Management Suite-portal, wordt u automatisch omgeleid naar Azure-waarschuwingen.  

> [!NOTE]
> Zorg ervoor dat de machtigingen worden toegewezen aan personen die u wilt toevoegen of bewerken van waarschuwingen correct zijn toegewezen in Azure. Om te begrijpen welke machtigingen die u wilt verlenen, Zie [machtigingen voor het gebruik van Azure bewaken en waarschuwingen](monitoring-roles-permissions-security.md).  
> 

U kunt doorgaan met het maken van waarschuwingen van de [Log Analytics-API](../log-analytics/log-analytics-api-alerts.md) en [Log Analytics Resource sjabloon](../monitoring/monitoring-solutions-resources-searches-alerts.md). Als u doet dit, moet u actiegroepen opnemen.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de hulpprogramma's om [initiëren waarschuwingen van logboekanalyse uit te breiden naar Azure](monitoring-alerts-extend-tool.md).
* Meer informatie over de [Azure waarschuwingen optreden](monitoring-overview-unified-alerts.md).
* Meer informatie over het maken van [waarschuwingen melden in Azure waarschuwingen](monitor-alerts-unified-log.md).

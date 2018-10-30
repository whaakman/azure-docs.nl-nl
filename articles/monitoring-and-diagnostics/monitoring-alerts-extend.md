---
title: Waarschuwingen (kopie) Log Analytics uitbreiden naar Azure-waarschuwingen - overzicht
description: Overzicht van proces kopiëren waarschuwingen van Log Analytics in OMS-portal naar Azure-waarschuwingen met details adressering veelvoorkomende vragen van klanten.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 45682caedaaaac1c9d335a45db64f16d713dfd7f
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233494"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Waarschuwingen van Log Analytics uitbreiden naar Azure-waarschuwingen
Tot voor kort opgenomen Azure Log Analytics een eigen waarschuwing functionaliteit, die proactief te over voorwaarden op basis van Log Analytics-gegevens waarschuwen kan. U hebt beheerd waarschuwingsregels in de Microsoft Operations Management Suite-portal. Waarschuwingen voor verschillende services in Microsoft Azure is nu geïntegreerd in de nieuwe ervaring voor waarschuwingen. Deze optie is beschikbaar als **waarschuwingen** onder Azure Monitor in de Azure-portal en biedt ondersteuning voor waarschuwingen van activiteitenlogboeken, metrische gegevens en logboeken van Log Analytics en Azure Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Voordelen van uw waarschuwingen uitbreiden
Er zijn enkele voordelen van het maken en beheren van waarschuwingen in Azure portal, zoals:

- In tegenstelling tot in de Operations Management Suite-portal, waar kunnen alleen 250 waarschuwingen worden gemaakt en weergegeven, heeft Azure-waarschuwingen geen dergelijke beperking.
- Van Azure-waarschuwingen, kunt u beheren, opsommen en weergeven van alle Waarschuwingstypen. Eerder, u kunt alleen doen voor Log Analytics-waarschuwingen.
- U kunt toegang beperken tot gebruikers kunnen alleen voor bewaking en waarschuwingen, met behulp van de [rol van Azure Monitor](monitoring-roles-permissions-security.md).
- In de Azure-waarschuwingen, kunt u [actiegroepen](monitoring-action-groups.md). Hiermee kunt u meer dan één actie voor elke waarschuwing. U kunt SMS, verzenden van een telefoongesprek, een Azure Automation-runbook aanroepen, een webhook aanroepen en configureren van een Connector IT Service Management (ITSM). 

## <a name="process-of-extending-your-alerts"></a>Proces voor het uitbreiden van uw waarschuwingen
Het proces van het verplaatsen van waarschuwingen van Log Analytics in Azure-waarschuwingen heeft geen betrekking op de meldingsdefinitie, query of configuratie in geen enkele manier wijzigen. De enige wijziging vereist is dat in Azure kunt u alle acties uitvoeren met behulp van een actiegroep. Als actiegroepen al gekoppeld aan de waarschuwing zijn, worden ze opgenomen als uitgebreid naar Azure.

> [!NOTE]
> Microsoft zal automatisch waarschuwingen die zijn gemaakt in de openbare cloud exemplaren van Log Analytics voor Azure-waarschuwingen, uitgebreid vanaf 14 mei 2018 wordt in een reeks terugkerende tot voltooid. Als er geen problemen voor het maken van [actiegroepen](monitoring-action-groups.md), gebruikt u [deze herstelstappen](monitoring-alerts-extend-tool.md#troubleshooting) om op te halen actiegroepen automatisch gemaakt. U kunt deze stappen gebruiken tot 5 juli 2018. *Niet van toepassing voor Azure Government en onafhankelijke cloudgebruikers van Log Analytics*. 
> 

Bij het plannen van waarschuwingen in Log Analytics-werkruimte worden uitgebreid naar Azure blijven te werken, en niet in dat een configuratie van de manier waarop inbreuk. Bij het plannen, uw waarschuwingen mogelijk niet beschikbaar voor het wijzigen van tijdelijk, maar u kunt doorgaan met het maken van nieuwe Azure-waarschuwingen tijdens deze periode. Als u probeert te bewerken of maken van waarschuwingen van de Operations Management Suite-portal, hebt u de mogelijkheid om door te gaan ze worden gemaakt van uw Log Analytics-werkruimte. U kunt ook om ze te maken van Azure-waarschuwingen in Azure portal.

 ![Schermafbeelding van de optie voor het maken van waarschuwingen van Log Analytics of Azure-waarschuwingen](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Waarschuwingen van Log Analytics uitbreiden naar Azure, worden er kosten in rekening gebracht op uw account. Met behulp van Azure-waarschuwingen voor query's op basis van Log Analytics-waarschuwingen is niet in rekening gebracht bij gebruik binnen de grenzen en voorwaarden die zijn vermeld in de [Azure Monitor prijsbeleid](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Uw waarschuwingen vrijwillig uitbreiden
Als u wilt uw meldingen uitbreiden naar Azure-waarschuwingen, kunt u een wizard, die beschikbaar zijn in de Operations Management Suite-portal of u programmatisch kunt doen met behulp van een API. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure met behulp van Operations Management Suite-portal en API](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Ondervindt na het uitbreiden van uw waarschuwingen
Nadat uw waarschuwingen worden uitgebreid naar Azure-waarschuwingen, ze nog steeds beschikbaar in de Operations Management Suite-portal voor het beheer niet anders dan voorheen.

![Schermafbeelding van de Operations Management Suite-portal, met waarschuwingen die zijn vermeld](./media/monitor-alerts-extend/PostExtendList.png)

Wanneer u probeert te bewerken van een bestaande waarschuwing of een nieuwe waarschuwing maken in de Operations Management Suite-portal, wordt u automatisch omgeleid naar Azure-waarschuwingen.  

> [!NOTE]
> Zorg ervoor dat de machtigingen worden toegewezen aan de personen die u wilt toevoegen of bewerken van waarschuwingen correct zijn toegewezen in Azure. Zie voor meer informatie over welke machtigingen u wilt verlenen, [machtigingen voor het gebruik van Azure Monitor en waarschuwingen](monitoring-roles-permissions-security.md).  
> 

U kunt doorgaan met het maken van waarschuwingen van de [Log Analytics API](../log-analytics/log-analytics-api-alerts.md) en [Log Analytics-Resourcesjabloon](../monitoring/monitoring-solutions-resources-searches-alerts.md). Als u dit doet, moet u actiegroepen opnemen.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de hulpmiddelen voor het [initiëren waarschuwingen van Log Analytics uitbreiden naar Azure](monitoring-alerts-extend-tool.md).
* Meer informatie over de [ervaren Azure-waarschuwingen](monitoring-overview-unified-alerts.md).
* Meer informatie over het maken van [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](monitor-alerts-unified-log.md).

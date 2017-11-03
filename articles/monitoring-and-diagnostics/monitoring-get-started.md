---
title: Aan de slag met Azure Monitor | Microsoft Docs
description: Ga aan de slag met Azure Monitor om inzicht te krijgen in de werking van uw resources en maatregelen te nemen op basis van gegevens.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: johnkem
ms.openlocfilehash: ba4e8fe0d54deb4a980174ff7d0904854c794d3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-monitor"></a>Aan de slag met Azure Monitor
Azure Monitor is de platformservice die één bron biedt voor het bewaken van Azure-resources. Met Azure Monitor kunt u visualiseren, query's uitvoeren, routeren, archiveren en actie ondernemen op basis van de metrische gegevens en logboeken van resources in Azure. U kunt werken met deze gegevens via de blade van de Monitor-portal, [Monitor PowerShell-cmdlets](insights-powershell-samples.md), [platformoverschrijdende CLI](insights-cli-samples.md) of [Azure Monitor REST API's](https://msdn.microsoft.com/library/dn931943.aspx). In dit artikel gebruiken we de demonstratieportal om enkele belangrijke onderdelen van Azure Monitor te belichten.

## <a name="walkthrough"></a>Walkthrough
1. Navigeer in de portal naar **Meer services** en zoek de optie **Monitor**. Klik op het sterpictogram om deze optie toe te voegen aan uw lijst met favorieten, zodat deze altijd eenvoudig toegankelijk is vanaf de navigatiebalk aan de linkerkant.

    ![Monitor in de lijst met services](./media/monitoring-get-started/monitor-more-services.png)
2. Klik op de optie **Monitor** om de blade **Monitor** te openen. In deze blade zijn al uw controle-instellingen en -gegevens bijeengebracht in één geconsolideerde weergave. Als eerste wordt de sectie **Activiteitenlogboek** geopend.

    ![Navigatie in de blade Monitor](./media/monitoring-get-started/monitor-blade-nav.png)

    Azure Monitor omvat drie basiscategorieën controlegegevens: het **activiteitenlogboek**, **metrische gegevens** en **diagnoselogboeken**.
3. Klik op **Activiteitenlogboek** om de sectie Activiteitenlogboek weer te geven.

    ![Blade met activiteitenlogboek](./media/monitoring-get-started/monitor-act-log-blade.png)

    In [**Activiteitenlogboek**](monitoring-overview-activity-logs.md) worden alle bewerkingen beschreven die worden uitgevoerd voor resources binnen uw abonnement. Met behulp van het activiteitenlogboek kunt u nagaan wie resources heeft toegevoegd, bijgewerkt of verwijderd binnen uw abonnement, en wanneer dat is gebeurd en wat er is veranderd. In het activiteitenlogboek ziet u bijvoorbeeld wanneer een web-app is gestopt en wie deze heeft gestopt. Gebeurtenissen in het activiteitenlogboek worden opgeslagen op het platform en kunnen gedurende 90 dagen worden doorzocht.

    U kunt query's voor algemene filters maken en opslaan en de belangrijkste query's vervolgens vastmaken aan een portal dashboard, zodat u altijd op de hoogte bent als er zich gebeurtenissen voordoen die voldoen aan uw criteria.
4. Filter de weergave voor een bepaalde resourcegroep in de afgelopen week en klik vervolgens op de knop **Opslaan**.

    ![Query voor activiteitenlogboek opslaan](./media/monitoring-get-started/monitor-act-log-save.png)
5. Klik vervolgens op de knop **Vastmaken**.

    ![Klikken op Vastmaken voor het activiteitenlogboek](./media/monitoring-get-started/monitor-act-log-pin.png)

    De meeste weergaven in dit scenario kunnen worden vastgemaakt aan een dashboard. Hierdoor beschikt u over een enkele bron van informatie voor operationele gegevens over uw services.
6. Ga terug naar uw dashboard. Nu kunt u zien of de query (en het aantal resultaten) wordt weergegeven in het dashboard. Dit is handig als u snel zien hoogwaardig acties die onlangs zijn opgetreden in uw abonnement wilt, bijvoorbeeld een nieuwe rol is toegewezen of een virtuele machine is verwijderd.

    ![Activiteitenlogboeken vastgemaakt aan dashboard](./media/monitoring-get-started/monitor-act-log-db.png)
7. Ga terug naar de tegel **Monitor** en klik op de sectie **Metrische gegevens**. U moet eerst een resource selecteren door het filteren en te selecteren met behulp van de opties vervolgkeuzelijst bovenaan de blade.

    ![Resource voor metrische gegevens filteren](./media/monitoring-get-started/monitor-met-filter.png)

    [**Metrische gegevens**](monitoring-overview-metrics.md) zijn beschikbaar voor alle Azure-resources. In deze weergave ziet u alle metrische gegevens bij elkaar, zodat u gemakkelijk inzicht krijgt in de prestaties van uw resources. Bekijk ook onze merk [nieuwe metrische gegevens voor grafieken ervaring](https://aka.ms/azuremonitor/new-metrics-charts) door te klikken op de **metrische gegevens (preview)** tabblad.
8. Wanneer u een resource selecteert, worden alle beschikbare metrische gegevens weergegeven aan de linkerkant van de blade. U kunt meerdere metrische gegevens tegelijk in grafiekvorm weergeven door de metrische gegevens te selecteren en het grafiektype en tijdsbereik aan te passen. U kunt ook alle metrische waarschuwingen weergeven die zijn ingesteld voor deze resource.

    ![Blade met metrische gegevens](./media/monitoring-get-started/monitor-metric-blade.png)

   > [!NOTE]
   > Sommige metrische gegevens zijn alleen beschikbaar als u [Application Insights](../application-insights/app-insights-overview.md) en/of Windows Azure Diagnostics of Linux Azure Diagnostics inschakelt voor uw resource.
   >
   >
9. Wanneer u tevreden bent met de grafiek, kunt u deze vastmaken aan het dashboard met de knop **Vastmaken**.
10. Ga terug naar de blade **Monitor** blad en klik op **Diagnostische logboeken**.

    ![Blade met diagnostische logboeken](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Diagnostische logboeken**](monitoring-overview-of-diagnostic-logs.md) zijn logboeken *van* een resource met gegevens over de werking van die resource. Tellers van regels voor netwerkbeveiligingsgroepen en werkstroomlogboeken van logische apps zijn bijvoorbeeld allebei een type diagnostisch logboek. Deze logboeken kunnen worden opgeslagen in een opslagaccount, worden gestreamd naar een Event Hub, en/of worden verzonden naar [Log Analytics](../log-analytics/log-analytics-overview.md). Log Analytics is het operationele intelligence product van Microsoft voor geavanceerde zoekopdrachten en waarschuwingen.

    In de portal kunt u een lijst met alle resources binnen uw abonnement weergeven en filteren om te bepalen of diagnostische logboeken zijn ingeschakeld voor die resources.
11. Klik op een resource in de blade met diagnostische logboeken. Als diagnostische logboeken worden opgeslagen in een opslagaccount, ziet u een lijst met uurlogboeken die u rechtstreeks kunt downloaden.

    ![Diagnostische logboeken voor één resource](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    U kunt ook klikken op **Diagnostische instellingen** om uw instellingen te configureren of wijzigen. U kunt hier opgeven of u logboeken wilt archiveren in een opslagaccount, streamen naar Event Hubs of verzenden naar een Log Analytics-werkruimte.

    ![Diagnostische logboeken inschakelen](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Als u diagnostische logboeken hebt ingesteld op Log Analytics, dan kunt u de logboeken doorzoeken in de sectie **Zoeken in logboeken** op de portal.
12. Navigeer naar de sectie **Waarschuwingen** van de blade Monitor.

    ![blade met waarschuwingen voor openbaar gebruik](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Hier kunt u alle [**waarschuwingen**](monitoring-overview-alerts.md) voor uw Azure-resources beheren, Dit omvat waarschuwingen op de metrische gegevens, logboekgebeurtenissen activiteit, Application Insights-webtests (locaties) en Application Insights proactieve diagnostische gegevens. Waarschuwingen kunnen resulteren in een e-mailbericht dat wordt verzonden of in een HTTP POST naar een webhook-URL.
13. Klik op **Metrische waarschuwing toevoegen** om een waarschuwing te maken.

    ![metrische waarschuwing toevoegen](./media/monitoring-get-started/monitor-alerts-add.png)

    Vervolgens kunt u een waarschuwing vastmaken aan uw dashboard om gemakkelijk de status te kunnen zien op elk gewenst moment.

    Azure Monitor nu ook heeft [ **near-realtime metrische waarschuwingen**](https://aka.ms/azuremonitor/near-real-time-alerts)(preview) die kan worden geëvalueerd met een frequentie zo laag elke minuut!
    
14. De sectie Monitor bevat ook koppelingen naar [Application Insights](../application-insights/app-insights-overview.md)-toepassingen en [Log Analytics](../log-analytics/log-analytics-overview.md)-beheeroplossingen. Deze andere Microsoft-producten zijn vergaand geïntegreerd met Azure Monitor.
15. Als u Application Insights of Log Analytics niet gebruikt, dan werkt Azure Monitor mogelijk samen met de producten van derden die u gebruikt voor controle, logboekregistratie en waarschuwingen. Zie onze [partnerpagina](monitoring-partners.md) voor een volledige lijst en instructies voor de integratie van de betreffende producten.

Als u deze stappen volgt en alle relevante tegels vastmaakt aan een dashboard, dan kunt u uitgebreide overzichten van uw toepassing en -infrastructuur maken, zoals deze:

![Azure Monitor-dashboard](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Volgende stappen
* Lees [Overzicht van Azure Monitor](monitoring-overview.md)

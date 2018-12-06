---
title: Azure Monitor-resourcegroep Insights | Microsoft Docs
description: Informatie over de status en prestaties van uw gedistribueerde toepassingen en services op het niveau van de resourcegroep met Azure Monitor
services: azure-monitor
author: NumberByColors
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: 2b9aee39942562ec7f17c08c0fcf46143a7a25d3
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961804"
---
# <a name="monitor-resource-groups-with-azure-monitor-preview"></a>Resourcegroepen bewaken met Azure Monitor (preview)

Moderne toepassingen zijn vaak complexe en zeer gedistribueerde met veel afzonderlijke onderdelen werken samen met het oog op een service. Deze complexe herkennen, biedt Azure Monitor voor de bewaking inzicht voor resourcegroepen. Hierdoor kunt u gemakkelijk sorteren en onderzoeken van problemen optreden op uw afzonderlijke resources, terwijl context over de status en prestaties van de resourcegroep&mdash;en uw toepassing&mdash;als geheel.

## <a name="access-insights-for-resource-groups"></a>Toegang tot inzichten voor resourcegroepen

1. Selecteer **resourcegroepen** in de navigatiebalk aan de linkerkant.
2. Kies een van de resourcegroepen die u wilt verkennen. (Als u hebt een groot aantal resourcegroepen te filteren op abonnement kan soms nuttig zijn.)
3. Voor toegang tot inzichten voor een resourcegroep, klikt u op **Insights** in het menu aan de linkerkant van een resourcegroep.

![Schermafbeelding van de resource-groep insights-overzichtspagina](./media/resource-group-insights/0001-overview.png)

## <a name="resources-with-active-alerts-and-health-issues"></a>Resources met actieve waarschuwingen en statusproblemen

De overzichtspagina toont het aantal waarschuwingen is gestart en nog steeds actief is, samen met de huidige Azure Resource Health van elke resource. Samen deze informatie kunt u snel vinden alle resources die problemen ondervindt. Waarschuwingen helpen u problemen detecteren in uw code en hoe u uw infrastructuur hebt geconfigureerd. Azure Resource Health-oppervlakken probleem met de Azure-platform zelf, die niet specifiek zijn voor de afzonderlijke toepassingen.

![Schermopname van Azure Resource Health deelvenster](./media/resource-group-insights/0002-overview.png)

### <a name="azure-resource-health"></a>Azure Resource Health

Als u Azure Resource Health, Controleer de **weergeven Azure Resource Health** vak boven de tabel. Deze kolom is verborgen om u te helpen bij de pagina snel geladen.

![Schermafbeelding met resource health graph is toegevoegd](./media/resource-group-insights/0003-overview.png)

Standaard worden de resources zijn gegroepeerd op app-laag en het resourcetype. **App-laag** is een eenvoudige categorisatie van resourcetypen, die alleen in de context van de resource-groep insights-overzichtspagina bestaat. Er zijn met betrekking tot de toepassingscode resourcetypen, compute-infrastructuur, netwerken, opslag en databases. Beheerprogramma's krijgen hun eigen app-lagen en elke andere resource wordt gecategoriseerd als behorend tot de **andere** app-laag. Deze groepering kunt u in een oogopslag zien welke subsystemen van uw toepassing in orde is en niet in orde zijn.

## <a name="diagnose-issues-in-your-resource-group"></a>Problemen in uw resourcegroep

Pagina inzichten van de resourcegroep biedt verschillende andere hulpmiddelen binnen het bereik zodat u problemen identificeren

   |         |          |
   | ---------------- |:-----|
   | [**Waarschuwingen**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)      |  Weergeven, maken en beheren van uw waarschuwingen. |
   | [**Metrische gegevens**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) | Visualiseer en Verken uw metrische gegevens op basis van gegevens.    |
   | [**Activiteitenlogboeken**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) | Abonnement op gebeurtenissen die hebben plaatsgevonden in Azure.  |
   | [**Overzicht van de toepassing**](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) | De topologie van uw gedistribueerde toepassing voor het identificeren van knelpunten of fout hotspots navigeren. |

## <a name="failures-and-performance"></a>Fouten en prestaties

Wat gebeurt er als u hebt gemerkt dat uw toepassing is traag of gebruikers fouten hebben gerapporteerd? Het is tijd in beslag nemen om te zoeken via al uw resources voor het isoleren van problemen.

De **prestaties** en **fouten** tabbladen vereenvoudigen van dit proces door de prestaties en bijeenbrengt fout diagnostische weergaven voor veel algemene resourcetypen.

De meeste resourcetypen wordt een galerie van Azure Monitor werkmap-sjablonen geopend. Elke werkmap die u hebt gemaakt kan worden aangepast, opgeslagen, gedeeld met uw team en opnieuw gebruikte in de toekomst vergelijkbare om problemen te diagnosticeren.

### <a name="investigate-failures"></a>Mislukte pogingen onderzoeken

Voor het testen van het tabblad fouten select **fouten** onder **onderzoeken** in het linkermenu.

De aan de linkerkant in de menubalk verandert nadat uw selectie is gemaakt, biedt u de nieuwe opties.

![Schermafbeelding van de fout overzichtsvenster](./media/resource-group-insights/00004-failures.png)

Wanneer App Service is gekozen, krijgt u een galerie van Azure Monitor werkmap-sjablonen.

![Schermafbeelding van de werkmap-toepassingsgalerie](./media/resource-group-insights/0005-failure-insights-workbook.png)

De sjabloon voor fout Insights kiezen, wordt de werkmap geopend.

![Schermopname van het rapport over fouten](./media/resource-group-insights/0006-failure-visual.png)

U kunt een van de rijen selecteren. De selectie wordt vervolgens weergegeven in de weergave van een grafische details.

![Schermafbeelding van de foutdetails](./media/resource-group-insights/0007-failure-details.png)

Werkmappen abstracte onmiddellijk het moeilijk werk van het maken van aangepaste rapporten en visualisaties naar een eenvoudig worden gebruikt. Sommige gebruikers willen mogelijk alleen het vooraf gedefinieerde parameters aanpassen, zijn werkmappen volledig aanpasbaar.

Als u een beeld van hoe deze werkmap intern functioneert, selecteer **bewerken** in de bovenste balk.

![Schermafbeelding van de optie voor extra bewerken](./media/resource-group-insights/0008-failure-edit.png)

Een aantal **bewerken** vakken weergegeven in de buurt van de verschillende elementen van de werkmap. Selecteer de **bewerken** vak onder de tabel van bewerkingen.

![Schermafbeelding van de invoervakken](./media/resource-group-insights/0009-failure-edit-graph.png)

Dit geeft aan de onderliggende Log Analytics-query die vraagt om de tabelvisualisatie van de.

 ![Schermafbeelding van log analytics-query-venster](./media/resource-group-insights/0010-failure-edit-query.png)

U kunt de query rechtstreeks wijzigen. Of u kunt gebruiken als uitgangspunt en lenen uit bij het ontwerpen van uw eigen aangepaste geparameteriseerde werkmap.

### <a name="investigate-performance"></a>Prestaties onderzoeken

Prestaties biedt een eigen galerie van werkmappen. Voor App Service biedt de werkmap met vooraf gedefinieerde prestaties van toepassingen in de volgende weergave:

 ![Schermafbeelding van de weergave van agentprestaties](./media/resource-group-insights/0011-performance.png)

In dit geval, als u bewerken selecteert ziet u dat deze set visualisaties wordt aangestuurd door Azure Monitor metrische gegevens.

 ![Schermafbeelding van de Prestatieweergave met metrische gegevens over Azure](./media/resource-group-insights/0012-performance-metrics.png)

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="enabling-access-to-alerts"></a>Toegang tot waarschuwingen inschakelen

Om waarschuwingen te bekijken in Azure Monitor voor resourcegroepen, moet iemand met een eigenaar of de rol Inzender voor dit abonnement Azure Monitor openen voor resourcegroepen voor elke willekeurige resourcegroep in het abonnement. Hierdoor kan iedereen met toegang lezen om waarschuwingen te zien in Azure Monitor voor resourcegroepen voor alle van de resourcegroepen in het abonnement. Hebt u een rol eigenaar of bijdrager, vernieuw deze pagina in een paar minuten.

Azure Monitor voor resourcegroepen, is afhankelijk van het systeem beheer van Azure Monitor-waarschuwingen om op te halen van de status van waarschuwing. Waarschuwingen Management is niet standaard geconfigureerd voor elke resourcegroep en het abonnement en kan alleen worden ingeschakeld door iemand met een rol eigenaar of Bijdrager. Kan het zijn ingeschakeld door:
* Azure Monitor openen voor resourcegroepen voor elke willekeurige resourcegroep in het abonnement.
* Of door te gaan naar het abonnement, te klikken op **Resourceproviders**, vervolgens te klikken op **registreren voor Alerts.Management**.

## <a name="next-steps"></a>Volgende stappen

- [Azure Monitor-werkmappen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)
- [Azure Resource Health](https://docs.microsoft.com/azure/service-health/resource-health-overview)
- [Azure Monitor-waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)

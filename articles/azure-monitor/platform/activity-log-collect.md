---
title: Verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics-werkruimte | Microsoft Docs
description: De Azure-activiteitenlogboek in Azure controleren logboeken verzamelen en gebruiken van de oplossing voor controle te analyseren en te zoeken naar de Azure-activiteitenlogboek in al uw Azure abonnementen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.openlocfilehash: 5839fd40a128097e400f13acbe4fb6ef90c656b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248127"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics-werkruimte in Azure Monitor
De [Azure Activity Log](activity-logs-overview.md) biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in uw Azure-abonnement. Dit artikel wordt beschreven hoe u voor het verzamelen van het activiteitenlogboek in een Log Analytics-werkruimte en het gebruik van de Activity Log Analytics [bewakingsoplossing](../insights/solutions.md), waarmee u Logboeken-query's en weergaven voor het analyseren van deze gegevens. 

Het activiteitenlogboek verbinden met Log Analytics-werkruimte biedt de volgende voordelen:

- Het activiteitenlogboek van meerdere Azure-abonnementen op één locatie voor de analyse te consolideren.
- Store-activiteit logboekvermeldingen voor meer dan 90 dagen.
- Correleer activiteitenlogboek gegevens met andere gegevens die door Azure Monitor worden verzameld.
- Gebruik [query's bijgehouden](../log-query/log-query-overview.md) complexe analyses uitvoeren en inzicht op vermeldingen in het logboek van de activiteit.

## <a name="connect-to-log-analytics-workspace"></a>Verbinding maken met Log Analytics-werkruimte
Een activiteitenlogboek kunnen worden verbonden met slechts één werkruimte, maar één werkruimte kan worden verbonden met het activiteitenlogboek voor meerdere abonnementen in de dezelfde Azure-tenant. Zie voor de verzameling voor meerdere tenants, [Azure-activiteitenlogboeken verzamelen in Log Analytics-werkruimte meerdere abonnementen in verschillende Azure Active Directory-tenants](activity-log-collect-tenants.md).

Gebruik de volgende procedure het activiteitenlogboek verbinden met uw Log Analytics-werkruimte:

1. Uit de **Log Analytics-werkruimten** menu in de Azure-portal, selecteert u de werkruimte voor het verzamelen van het activiteitenlogboek.
1. In de **gegevensbronnen voor werkruimte** sectie van de werkruimte in het menu **Azure-activiteitenlogboek**.
1. Klik op het abonnement dat u verbinding wilt maken.

    ![Workspaces](media/activity-log-export/workspaces.png)

1. Klik op **Connect** verbinding maken met het activiteitenlogboek in het abonnement op de geselecteerde werkruimte. Als het abonnement al met een andere werkruimte verbonden is, klikt u op **verbinding verbreken** om deze verbinding verbreken.

    ![Verbinding maken met werkruimten](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Analyseren in Log Analytics-werkruimte
Wanneer u een activiteitenlogboek verbinding met een Log Analytics-werkruimte, vermeldingen worden geschreven naar de werkruimte in een tabel met de naam **AzureActivity** die u kunt ophalen met een [logboekquery](../log-query/log-query-overview.md). De structuur van deze tabel varieert, afhankelijk van de [categorie van logboekvermelding](activity-logs-overview.md#categories-in-the-activity-log). Zie [gebeurtenisschema in het Azure-activiteitenlogboek](activity-log-schema.md) voor een beschrijving van elke categorie.

## <a name="activity-logs-analytics-monitoring-solution"></a>Oplossing voor controle op activiteit logboeken Analytics
De Azure Log Analytics-oplossing voor controle omvat meerdere logboeken-query's en weergaven voor het analyseren van de records van het activiteitenlogboek in uw Log Analytics-werkruimte.

### <a name="install-the-solution"></a>De oplossing installeren
Gebruik de procedure in [installeren van een oplossing voor bewaking](../insights/solutions.md#install-a-monitoring-solution) voor het installeren van de **Activity Log Analytics** oplossing. Er is geen aanvullende configuratie vereist.

### <a name="use-the-solution"></a>De oplossing gebruiken
Bewakingsoplossingen worden geopend vanaf de **Monitor** in het menu in de Azure-portal. Selecteer **meer** in de **Insights** sectie om te openen de **overzicht** pagina met de tegels van de oplossingen. De **Azure-activiteitenlogboeken** tegel geeft een telling van het aantal weer **AzureActivity** records in uw werkruimte.

![Azure-activiteitenlogboeken tegel](media/collect-activity-logs/azure-activity-logs-tile.png)


Klik op de **Azure-activiteitenlogboeken** tegel om te openen de **Azure-activiteitenlogboeken** weergeven. In de volgende tabel bevat de weergave de visualisatie delen. Elk onderdeel bevat maximaal 10 items die voldoen aan de criteria van die onderdelen voor de opgegeven periode. U kunt een query voor die Hiermee worden alle overeenkomende records geretourneerd door te klikken op uitvoeren **alle** aan de onderkant van het onderdeel.

![Azure-activiteitenlogboeken-dashboard](media/collect-activity-logs/activity-log-dash.png)

| Visualisatie-onderdeel | Description |
| --- | --- |
| Vermeldingen in het Azure-activiteit | Ziet u een staafdiagram van de bovenste waarde van de Azure-activiteitenlogboek record totalen voor het datumbereik dat u hebt geselecteerd en bevat een overzicht van de bovenste 10 activiteit aanroepers. Klik op het staafdiagram voor het uitvoeren van een logboekzoekopdracht voor `AzureActivity`. Klik op een item oproepende functie voor het uitvoeren van een logboekzoekopdracht die alle activiteit logboekvermeldingen voor dat item geretourneerd. |
| Activiteitenlogboeken op Status | Een ringdiagram voor de status van de Azure-activiteitenlogboek voor het geselecteerde datumbereik en een lijst van de bovenste tien status records bevat. Klik op de grafiek voor het uitvoeren van een query voor voor `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Klik op een statusitem voor het uitvoeren van een logboekzoekopdracht die alle activiteit logboekvermeldingen voor die statusrecord retourneren. |
| Activiteitenlogboeken per Resource | Geeft het totale aantal resources met activiteitenlogboeken en geeft een lijst van de top tien resources met record aantallen voor elke resource. Klik op het totale aantal gebied voor het uitvoeren van een logboekzoekopdracht voor `AzureActivity | summarize AggregatedValue = count() by Resource`, waarin alle Azure-resources beschikbaar zijn voor de oplossing wordt getoond. Klik op een resource voor het uitvoeren van een query voor het retourneren van alle activiteitsrecords voor die bron. |
| Activiteitenlogboeken door de Resourceprovider | Geeft het totale aantal resourceproviders die activiteitenlogboeken produceren en geeft een lijst van de tien. Klik op het totale aantal gebied voor het uitvoeren van een query voor voor `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, waarin alle Azure-resource-providers. Klik op een resourceprovider voor het uitvoeren van een query voor het retourneren van alle activiteitsrecords voor de provider. |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [activiteitenlogboek](activity-logs-overview.md).
- Meer informatie over de [gegevensplatform in de Azure Monitor](data-platform.md).
- Gebruik [query's bijgehouden](../log-query/log-query-overview.md) om gedetailleerde gegevens uit uw activiteitenlogboek weer te geven.

---
title: Weergeven en analyseren van gegevens in Azure Log Analytics | Microsoft Docs
description: Ondersteuning voor gebruikers van zoeken in Logboeken in Azure Monitor log-query manier Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: bwren
ms.openlocfilehash: 89811e95ec24eb354020dd6384f6fdab6cee8c8f
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54392292"
---
# <a name="transition-from-log-analytics-log-search-to-azure-monitor-logs"></a>Verandert van Log Analytics zoeken in Logboeken in Logboeken van Azure Monitor
Zoeken in Logboeken in Log Analytics is onlangs vervangen door een nieuwe ervaring voor het analyseren van Azure Monitor-Logboeken. De zoekpagina Log is momenteel nog steeds toegankelijk via de **Logboeken (klassiek)** menu-item in de **Log Analytics-werkruimten** pagina in de Azure portal, maar wordt op 15 februari 2019 worden verwijderd. Dit artikel wordt beschreven verschillen tussen de twee ervaringen kunt u overstappen van zoeken in Logboeken. 

## <a name="extract-custom-fields"></a>Aangepaste velden ophalen 
In zoeken in Logboeken, u hebt uitgepakt [aangepaste velden](../platform/custom-fields.md) in de lijst, waarbij van een veld menu de actie bevat _velden extraheren uit tabel_.

![Meld u zoekvelden extraheren](media/log-search-transition/extract-fields-log-search.png)

In Azure Monitor-Logboeken, kunt u aangepaste velden ophalen uit de tabelweergave. Een record achtereenvolgens door te klikken op de pijl aan de linkerkant en klik op het weglatingsteken voor toegang tot de _velden extraheren_ actie.

![Logboeken ophalen velden](media/log-search-transition/extract-fields-logs.png)

## <a name="filter-results-of-a-query"></a>Resultaten van een query filteren
Een lijst met filters worden in zoeken in logboeken weergegeven als de zoekresultaten worden geleverd. Selecteer een filter en klik op **toepassen** om uit te voeren van de query met het geselecteerde filter.

![Meld u zoekfilter](media/log-search-transition/filter-log-search.png)

Selecteer in de logboeken van Azure Monitor, *Filter (preview)* om filters weer te geven. Klik op het filterpictogram om filters toevoeging weer te geven. Selecteer een filter en klik op **toepassen & uitvoeren** om uit te voeren van de query met het geselecteerde filter.

![Logboeken filteren](media/log-search-transition/filter-logs.png)

## <a name="functions-and-computer-groups"></a>Functies en computergroepen
Als u wilt een zoekopdracht opslaan in zoeken in Logboeken, selecteer **opgeslagen zoekopdrachten** en **toevoegen** een naam, categorie en querytekst opgeven. Maak een [computergroep](../platform/computer-groups.md) door een functiealias toe te voegen.

![Zoeken in Logboeken opslaan](media/log-search-transition/save-search-log-search.png)

Als u wilt de huidige query opslaan in Azure Monitor-Logboeken, selecteer **opslaan**. Wijziging **opslaan als** naar _functie_ en bieden een **Functiealias** maken een [functie](functions.md).

![Logboekquery opslaan](media/log-search-transition/save-query-logs.png)

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten
In zoeken in Logboeken, uw opgeslagen zoekopdrachten zijn beschikbaar via de actie-balkitem **opgeslagen zoekopdrachten**. Toegang tot opgeslagen query's uit in Azure Monitor-Logboeken, **Queryverkenner**.

![Queryverkenner](media/log-search-transition/query-explorer.png)

## <a name="take-action"></a>Actie ondernemen
In zoeken in Logboeken, kunt u [een runbook starten](take-action.md) uit een zoekresultaat door selectionselecting **actie ondernemen**.

![Actie ondernemen](media/log-search-transition/take-action-log-search.png)

In Azure Monitor-Logboeken, [maken met een waarschuwing van de logboekquery](../platform/alerts-log.md). Een actiegroep configureren met een of meer acties die worden uitgevoerd in reactie op de waarschuwing.

![Actiegroep](media/log-search-transition/action-group.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de nieuwe [Azure Monitor-logboeken ervaring](get-started-portal.md).
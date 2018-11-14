---
title: Een aangepast dashboard maken in Azure Log Analytics | Microsoft Docs
description: Deze handleiding helpt u begrijpen hoe Log Analytics-dashboards al uw opgeslagen logboekzoekopdrachten kunnen visualiseren zodat u een één lens naar uw omgeving.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 8afefa95e0a7b4f10f4a2bf6c7f099ba9eb4a957
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613200"
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Een aangepast dashboard voor gebruik in Log Analytics maken

Deze handleiding helpt u begrijpen hoe Log Analytics-dashboards al uw opgeslagen logboekzoekopdrachten kunnen visualiseren zodat u een één lens naar uw omgeving.

>[!NOTE]
> U kunt niet meer bewerken voor uw bestaande **mijn Dashboard**. Deze functie wordt momenteel wordt afgeschaft.

![Voorbeeld van Dashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

De aangepaste dashboards die u in de OMS-portal maakt zijn ook beschikbaar in de mobiele OMS-App. Zie de volgende pagina's voor meer informatie over de apps.

* [Mobiele OMS-app uit de Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Mobiele OMS-app vanuit iTunes van Apple](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobiele dashboard](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hoe maak ik mijn dashboard?
Als u wilt beginnen, gaat u naar de pagina overzicht van OMS. U ziet de **mijn Dashboard** tegel aan de linkerkant. Klik op het inzoomen op uw dashboard.

![Overzicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Een tegel wordt toegevoegd
In dashboards, worden tegels aangestuurd door uw opgeslagen logboekzoekopdrachten. OMS wordt geleverd met veel vooraf samengestelde opgeslagen zoekopdrachten, zodat u meteen kunt beginnen. Gebruik de volgende stappen uit die wordt beschreven hoe om te beginnen.

Klik in de weergave Mijn Dashboard **aanpassen** in te voeren modus aanpassen.

![Pictogrammen](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Het deelvenster dat wordt geopend aan de rechterkant van de pagina worden alle opgeslagen zoekopdrachten in Logboeken van uw werkruimte. Beweeg de muisaanwijzer over een opgeslagen zoekopdracht om te zoeken in een opgeslagen logboeken als een tegel visualiseren, en klik vervolgens op de **plus** symbool.

![Tegels 1 toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Wanneer u klikt op de **plus** een symbool, een nieuwe tegel wordt weergegeven in de weergave Mijn Dashboard.

![Tegels 2 toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Een tegel bewerken
Klik in de weergave Mijn Dashboard **aanpassen** in te voeren modus aanpassen. Klik op de tegel die u wilt bewerken. De wijzigingen deelvenster aan de rechterkant om te bewerken, en biedt een aantal opties:

![Tegel bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Tegel bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Tegelvisualisaties
Er zijn drie soorten tegelvisualisaties kiezen uit:

| grafiektype | Wat het doet |
| --- | --- |
| ![Staafdiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Weergegeven als een staafdiagram, of een lijst met resultaten op basis van een veld, afhankelijk van een tijdlijn van uw opgeslagen logboek zoekresultaten indien uw zoeken in Logboeken resultaten op basis van een veld of niet samenvoegt. |
| ![metrische waarde](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Geeft het totaal aantal log search resultaat treffers weer als een getal in een tegel. Metrische tegels kunnen u een drempelwaarde die de tegel wordt gemarkeerd als de drempelwaarde is bereikt. |
| ![Regel](./media/log-analytics-dashboards/oms-dashboards-line.png) |Geeft een tijdlijn van uw opgeslagen log search resultaat treffers met waarden weer als een lijndiagram. |

### <a name="threshold"></a>Drempelwaarde
U kunt een drempelwaarde maken op een tegel met behulp van de visualisatie van de metrische gegevens. Selecteer op het maken van een drempelwaarde op de tegel. Kies of u wilt markeren de tegel wanneer de waarde boven of onder de gekozen drempelwaarde is en stel vervolgens de onderstaande drempelwaarde.

## <a name="organizing-the-dashboard"></a>Het dashboard ordenen
Om te organiseren uw dashboard, gaat u naar de weergave van mijn Dashboard en klik op **aanpassen** in te voeren modus aanpassen. Klik en sleep de tegel die u wilt verplaatsen en verplaatsen naar waar u de tegel om te worden.

![Uw Dashboard delen](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Een tegel verwijderen
Als u wilt verwijderen van een tegel, gaat u naar de weergave van mijn Dashboard en klik op **aanpassen** in te voeren modus aanpassen. Selecteer de tegel die u verwijderen, in het rechterdeelvenster wilt en selecteer vervolgens **verwijderen tegel**.

![Een tegel verwijderen](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Volgende stappen
* Maak [waarschuwingen](../monitoring-and-diagnostics/monitoring-overview-alerts.md) in Log Analytics om meldingen te genereren en problemen kunt oplossen.

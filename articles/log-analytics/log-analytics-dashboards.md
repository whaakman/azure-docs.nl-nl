---
title: Een aangepaste dashboard maken in Azure Log Analytics | Microsoft Docs
description: Deze handleiding helpt u begrijpen hoe logboekanalyse dashboards Visualiseer al uw zoekopdrachten opgeslagen logboek zodat u een identiteitsonderdelen om weer te geven van uw omgeving.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7eaa74655b009e3154e2cc4f09d5599040ec4589
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Maken van een aangepast dashboard voor gebruik in Log Analytics

Deze handleiding helpt u begrijpen hoe logboekanalyse dashboards Visualiseer al uw zoekopdrachten opgeslagen logboek zodat u een identiteitsonderdelen om weer te geven van uw omgeving.

>[!NOTE]
> U kunt niet meer bewerken voor uw bestaande **mijn Dashboard**. Deze functie is momenteel wordt afgeschaft.

![Voorbeeld van Dashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

De aangepaste dashboards die u in de OMS-portal maakt zijn ook beschikbaar in de OMS mobiele App. Zie de volgende pagina's voor meer informatie over de apps.

* [OMS mobiele app van de Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Mobiele app van Apple iTunes OMS](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobiele dashboard](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hoe maak ik mijn dashboard
Ga naar de pagina overzicht van OMS. U ziet de **mijn Dashboard** tegel aan de linkerkant. Klik op het inzoomen op uw dashboard.

![Overzicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Toevoegen van een tegel
Dashboards, worden de tegels aangedreven door uw zoekopdrachten opgeslagen logboek. OMS wordt geleverd met veel en-klare opgeslagen logboek zoekopdrachten, zodat u meteen kunt beginnen. Gebruik de volgende stappen uit die een overzicht van hoe u om te beginnen.

Klik in de weergave Mijn Dashboard **aanpassen** invoeren modus aanpassen.

![Afbeeldingen](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Het deelvenster dat wordt geopend aan de rechterkant van de pagina geeft alle uw werkruimte opgeslagen logboek zoekopdrachten. Beweeg de muisaanwijzer over een opgeslagen zoekopdracht om te visualiseren een zoekopdracht opgeslagen logboek als tegel, en klik vervolgens op de **plus** symbool.

![1 tegels toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Wanneer u klikt op de **plus** een symbool, een nieuwe tegel wordt weergegeven in de weergave Mijn Dashboard.

![2 tegels toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Een tegel bewerken
Klik in de weergave Mijn Dashboard **aanpassen** invoeren modus aanpassen. Klik op de tegel die u wilt bewerken. De wijzigingen aan de rechterkant te bewerken, en biedt een aantal opties:

![Tegel bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Tegel bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Tegel visualisaties
Er zijn drie soorten tegel visualisaties kunt kiezen uit:

| grafiektype | wat het programma doet |
| --- | --- |
| ![Staafdiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Geeft een tijdlijn met uw opgeslagen logboek zoekresultaten weer als een staafdiagram of een lijst met resultaten in een veld afhankelijk van als uw zoekopdracht logboek resultaten door een veld of niet aggregeert. |
| ![metrische waarde](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Worden uw totale logboek zoeken resultaat treffers weergegeven als een getal in een tegel. Metrische tegels kunnen u een drempelwaarde die de tegel wordt gemarkeerd als de drempelwaarde is bereikt. |
| ![regel](./media/log-analytics-dashboards/oms-dashboards-line.png) |Hiermee geeft u een tijdlijn van uw opgeslagen logboek resultaat gevonden in de zoekopdracht met waarden weer als een lijndiagram. |

### <a name="threshold"></a>Drempelwaarde
U kunt een drempelwaarde maken op een tegel met behulp van de metrische visualisatie. Selecteer op het maken van een drempelwaarde in op de tegel. Kies of u de tegel markeren als de waarde boven of onder de gekozen drempelwaarde, wordt de onderstaande drempelwaarde ingesteld.

## <a name="organizing-the-dashboard"></a>Het dashboard ordenen
Voor het ordenen van uw dashboard, gaat u naar de weergave Mijn Dashboard en klik op **aanpassen** invoeren modus aanpassen. Klik en sleep de tegel die u wilt verplaatsen en verplaatsen naar waar u uw tegel te zijn.

![Uw Dashboard ordenen](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Een tegel verwijderen
Als u een tegel, gaat u naar de weergave Mijn Dashboard en op **aanpassen** invoeren modus aanpassen. Selecteer de tegel die u verwijderen wilt, selecteert in het rechterpaneel **verwijderen tegel**.

![Een tegel verwijderen](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Volgende stappen
* Maak [waarschuwingen](log-analytics-alerts.md) in Log Analytics om meldingen te genereren en oplossen van problemen.

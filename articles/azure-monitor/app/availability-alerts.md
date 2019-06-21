---
title: Stel waarschuwingen van de beschikbaarheid met Azure Application Insights | Microsoft Docs
description: Stel webtests in Application Insights in. Ontvang een waarschuwing wanneer een website niet meer beschikbaar is of traag reageert.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: cc022f91d4b4fec42929769df8c979320548a1f9
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304999"
---
# <a name="availability-alerts"></a>Beschikbaarheid van waarschuwingen

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) verzendt regelmatig webaanvragen naar uw toepassing vanaf verschillende punten over de hele wereld. Deze waarschuwt u als uw toepassing reageert niet of langzaam reageert.

## <a name="enable-alerts"></a>Waarschuwingen inschakelen

Waarschuwingen worden nu automatisch standaard ingeschakeld, maar om te kunnen volledig configureren de waarschuwing hebt u eerst uw beschikbaarheidstest in eerste instantie te maken.

![Ontwikkel-ervaring](./media/availability-alerts/create-test.png)

> [!NOTE]
>  Met de [nieuwe geïntegreerde waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), de voorkeuren waarschuwingsregel ernst en meldingen met [actiegroepen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **moet** geconfigureerd in de ervaring voor waarschuwingen. Zonder de volgende stappen maakt u alleen in de portal meldingen ontvangt.

1. Na het opslaan van de beschikbaarheidstest bij de details van tabblad Klik op het weglatingsteken door de test die u zojuist hebt gemaakt. Klik op 'waarschuwing bewerken'.

   ![Bewerken na het opslaan](./media/availability-alerts/edit-alert.png)

2. De gewenste ernst, de beschrijving van regel en bovenal - de actiegroep waarvoor de voorkeuren voor meldingen die u wilt gebruiken voor deze waarschuwingsregel instellen.

   ![Bewerken na het opslaan](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Waarschuwing bij X van Y-locaties die fouten rapporteren

De X van Y-locaties waarschuwingsregel is standaard ingeschakeld in de [nieuwe waarschuwingen van geïntegreerde ervaring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), wanneer u een nieuwe beschikbaarheidstest maken. U kunt zich afmelden op de optie 'klassieke' te selecteren of uitschakelen van de waarschuwingsregel te kiezen.

> [!NOTE]
> Configureer de actiegroepen voor het ontvangen van meldingen wanneer de waarschuwing wordt geactiveerd door de bovenstaande stappen te volgen. Zonder deze stap wordt u alleen in de portal-meldingen ontvangen wanneer de regel wordt geactiveerd.
>

### <a name="alert-on-availability-metrics"></a>Waarschuwing op basis van metrische gegevens over beschikbaarheid

Met behulp van de [nieuwe geïntegreerde waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), kunt u waarschuwingen over de beschikbaarheid van gesegmenteerde statistische en duur van de metrische gegevens ook testen:

1. Selecteer een Application Insights-resource in de ervaring voor metrische gegevens en een beschikbaarheid van metrische waarde selecteren:

    ![Selectie van de metrische gegevens over beschikbaarheid](./media/availability-alerts/select-metric.png)

2. Optie in het menu, u naar de nieuwe ervaring waarin u kunt selecteren specifieke tests of locaties gaat voor het instellen van de waarschuwingsregel op waarschuwingen configureren. U kunt ook de actiegroepen voor deze waarschuwingsregel hier configureren.

### <a name="alert-on-custom-analytics-queries"></a>Waarschuwing bij aangepaste analytics-query 's

Met behulp van de [nieuwe geïntegreerde waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), u kunt waarschuwingen op [aangepaste logboeken-query's](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). Met aangepaste query's, kunt u erop wijzen op een willekeurige voorwaarde die helpt u bij het ophalen van de meest betrouwbare signaal van problemen met de beschikbaarheid. Dit is ook vooral van toepassing, als u aangepaste beschikbaarheidsresultaten met behulp van de SDK TrackAvailability verzendt. 

> [!Tip]
> De metrische gegevens over de van beschikbaarheidsgegevens bevatten een aangepaste beschikbaarheidsresultaten die u door het aanroepen van onze SDK TrackAvailability kan indienen. U kunt de waarschuwingen voor de ondersteuning voor metrische gegevens op de waarschuwing op de van de aangepaste beschikbaarheidsresultaten.
>

## <a name="troubleshooting"></a>Problemen oplossen

Toegewezen [probleemoplossingsartikel](troubleshoot-availability.md).

## <a name="next-steps"></a>Volgende stappen

* [Webtests met meerdere stappen](availability-multistep.md)
* [URL-ping-webtests](monitor-web-app-availability.md)


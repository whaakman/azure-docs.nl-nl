---
title: Meldingen instellen voor Azure-services - Azure-portal | Microsoft Docs
description: Trigger e-mailberichten, meldingen, worden URL's van websites (webhooks) of automation aanroepen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: rboucher
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 4acf1f549a6c901fb0b772c4591f1f35d61365ad
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="create-classic-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Klassieke metrische waarschuwingen in de Azure-Monitor maken voor Azure-services - Azure-portal
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Overzicht

> [!NOTE]
> In dit artikel wordt beschreven hoe oudere metrische waarschuwingen maken. Azure Monitor ondersteunt nu nieuwere, voor betere metrische waarschuwingen. Deze waarschuwingen kunnen meerdere metrische gegevens controleren en waarschuwen voor dimensionale metrische gegevens mogelijk maken. Meer informatie over [bijna Real-Time metriek waarschuwingen](monitoring-near-real-time-metric-alerts.md).
>
>

In dit artikel leest u hoe Azure metrische waarschuwingen met de Azure portal instellen. 

U kunt een waarschuwing op basis van bewaking metrische gegevens voor of gebeurtenissen op uw Azure-services kunt ontvangen.

* **Metrische waarden** -de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en vervolgens later wanneer die voorwaarde wordt niet langer wordt voldaan.    
* **Activiteit logboekgebeurtenissen** -een waarschuwing kunt activeren voor *elke* gebeurtenis of alleen wanneer bepaalde gebeurtenis zich voordoet. Meer informatie over [activiteit logboek waarschuwingen](monitoring-activity-log-alerts.md).

U kunt het volgende te doen als er wordt een waarschuwing voor metrische configureren:

* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar andere e-mailberichten die u opgeeft.
* een webhook aanroepen
* starten van de uitvoering van een Azure-runbook (alleen van de Azure-portal)

U kunt configureren en ophalen van informatie over metrische waarschuwingsregels met

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
* [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Een waarschuwingsregel maken op een waarde met de Azure-portal
1. In de [portal](https://portal.azure.com/), zoekt de resource die u geïnteresseerd bent in de bewaking en selecteert u deze.

2. Selecteer **waarschuwingen (klassiek)** onder de sectie bewaking. De tekst en het pictogram mogelijk enigszins afwijken voor verschillende bronnen. Als u niet kunt vinden **waarschuwingen (klassiek)**, wellicht onder **waarschuwingen** of **waarschuwingsregels**

    ![Bewaking](./media/insights-alerts-portal/AlertRulesButton.png)

3. Selecteer de **metrische waarschuwing toevoegen** opdracht en vul de velden in.

    ![Waarschuwing toevoegen](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Naam** uw Waarschuwing regel en kies een **beschrijving**, die ook weergegeven in e-mailmeldingen.

5. Selecteer de **metriek** u wilt bewaken, en kies vervolgens een **voorwaarde** en **drempelwaarde** waarde voor de metriek. Ook voor kiezen de **periode** tijdsperiode waarin de meetwaarde regel moet worden voldaan voordat de waarschuwing triggers. Dus bijvoorbeeld, als u de periode 'in de afgelopen 5 minuten gebruiken' en de waarschuwing CPU dan 80 zoekt %, de waarschuwing wordt geactiveerd wanneer de CPU is al consistent bovenstaande 80% 5 minuten. Zodra de eerste trigger is plaatsvindt, deze opnieuw wordt geactiveerd wanneer de CPU onder 80% gedurende vijf minuten blijft. De meting van CPU-metrische doet zich voor elke één minuut.

6. Controleer **e-eigenaars...**  als u wilt dat beheerders en medebeheerders wilt ontvangen wanneer de waarschuwing wordt geactiveerd.

7. Als u aanvullende e-mailberichten een melding ontvangen wilt wanneer de waarschuwing wordt geactiveerd, deze toevoegen in de **aanvullende beheerder email(s)** veld. Scheid meerdere e-mailberichten met puntkomma's -  *email@contoso.com;email2@contoso.com*

8. Plaats in een geldige URI in de **Webhook** als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.

9. Als u Azure Automation gebruikt, kunt u een Runbook moet worden uitgevoerd wanneer de waarschuwing wordt geactiveerd.

10. Selecteer **OK** wanneer u klaar bent voor het maken van de waarschuwing.   

Binnen een paar minuten de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="managing-your-alerts"></a>Uw waarschuwingen beheren
Wanneer u een waarschuwing hebt gemaakt, kunt u deze selecteren en:

* Een grafiek weer met de metrische drempel en de werkelijke waarden van de vorige dag weergeven.
* Bewerk of verwijder deze.
* **Schakel** of **inschakelen** deze als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor deze waarschuwing.

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](monitoring-overview.md) met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Meer informatie over de [nieuwere metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md)
* Meer informatie over [configureren van webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [waarschuwingen configureren op activiteit logboekgebeurtenissen](monitoring-activity-log-alerts.md).
* Meer informatie over [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Ophalen van een [overzicht van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) en gedetailleerde hoge frequentie metrische gegevens verzamelen over uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.

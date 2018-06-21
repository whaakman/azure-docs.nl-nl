---
title: De Azure portal gebruiken voor het maken van klassieke waarschuwingen voor Azure-services | Microsoft Docs
description: E-mailberichten of meldingen activeren of website-URL's (webhooks) of automation aanroepen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/23/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: d0e5512eb3f963898ded6d7155f8b75cfb6ef911
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287426"
---
# <a name="use-the-azure-portal-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>De Azure portal gebruiken voor het maken van klassieke metrische waarschuwingen in de Azure-Monitor voor Azure-services 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>

> [!NOTE]
> In dit artikel wordt beschreven hoe oudere klassieke metrische waarschuwingen maken. Azure ondersteunt van de Monitor nu [nieuwere metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md). 


Dit artikel ziet u het instellen van de klassieke Azure metrische waarschuwingen met behulp van de Azure-portal. 

U kunt een waarschuwing op basis van de metrische gegevens voor uw Azure-services ontvangen of kunt u waarschuwingen voor gebeurtenissen die plaatsvinden ontvangen in Azure.

* **Metrische waarden**: de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en wanneer die voorwaarde niet langer wordt voldaan.    

* **Activiteit logboekgebeurtenissen**: een waarschuwing kunt activeren voor *elke* gebeurtenis of wanneer bepaalde gebeurtenissen optreden. Meer informatie over [activiteit logboek waarschuwingen](monitoring-activity-log-alerts.md).

U kunt het volgende te doen als er wordt een klassieke metrische-waarschuwing configureren:

* E-mailmeldingen verzenden naar de servicebeheerder en medebeheerders.
* E-mail verzenden naar andere e-mailadressen die u opgeeft.
* Een webhook aanroepen.
* Start de uitvoering van een Azure-runbook (alleen van de Azure-portal).

U kunt configureren en meer informatie over klassieke metrische waarschuwingsregels ophalen uit de volgende locaties: 

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure-CLI](insights-alerts-command-line-interface.md)
* [Monitor voor Azure REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Een waarschuwingsregel maken op een waarde met de Azure-portal
1. In de [portal](https://portal.azure.com/), zoek de resource die u wilt bewaken en selecteert u vervolgens.

2. In de **bewaking** sectie **waarschuwingen (klassiek)**. De tekst en het pictogram mogelijk verschillen voor verschillende bronnen. Als u niet kunt vinden **waarschuwingen (klassiek)** hier wellicht in **waarschuwingen** of **waarschuwingsregels**.

    ![Bewaking](./media/insights-alerts-portal/AlertRulesButton.png)

3. Selecteer de **metrische waarschuwing toevoegen (klassiek)** opdracht en vul de velden.

    ![Waarschuwing toevoegen](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Naam** de waarschuwingsregel. Kies een **beschrijving**, die ook wordt weergegeven in e-mailmeldingen.

5. Selecteer de **metriek** die u wilt bewaken. Kies een **voorwaarde** en **drempelwaarde** waarde voor de metriek. Ook voor kiezen de **periode** tijdsperiode waarin de meetwaarde regel moet worden voldaan voordat de waarschuwing triggers. Bijvoorbeeld, als u de periode 'in de afgelopen 5 minuten gebruiken' en de waarschuwing wordt gezocht naar een CPU dan 80%, de waarschuwing wordt geactiveerd wanneer de CPU is al consistent bovenstaande 80% 5 minuten. Na de eerste optreedt trigger, wordt opnieuw wordt geactiveerd wanneer de CPU onder 80% blijft gedurende vijf minuten. De meting van CPU-metrische gebeurt elke minuut.

6. Selecteer **e-eigenaars...**  als u wilt dat beheerders en medebeheerders e-mailmeldingen ontvangen wanneer de waarschuwing wordt geactiveerd.

7. Als u wilt om meldingen te verzenden naar andere e-mailadressen wanneer de waarschuwing wordt geactiveerd, deze toevoegen in de **aanvullende beheerder email(s)** veld. Scheid meerdere e-mailberichten met puntkomma's in de volgende indeling:  *email@contoso.com; email2@contoso.com*

8. Plaats in een geldige URI in de **Webhook** als u wilt dat deze moet worden aangeroepen wanneer de waarschuwing wordt geactiveerd.

9. Als u Azure Automation gebruikt, kunt u een runbook moet worden uitgevoerd wanneer de waarschuwing wordt geactiveerd.

10. Selecteer **OK** de waarschuwing wilt maken.   

Binnen een paar minuten de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="manage-your-alerts"></a>Waarschuwingen beheren
Nadat u een waarschuwing gemaakt, kunt u selecteren en voer een van de volgende taken:

* Een grafiek weergegeven waarin de meetwaarde drempelwaarde en de werkelijke waarden van de vorige dag weergeven.
* Bewerk of verwijder deze.
* **Schakel** of **inschakelen** deze als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor deze waarschuwing.

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](monitoring-overview.md), met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Meer informatie over de [nieuwere metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md).
* Meer informatie over [configureren van webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [waarschuwingen configureren op activiteit logboekgebeurtenissen](monitoring-activity-log-alerts.md).
* Meer informatie over [Azure Automation-runbooks](../automation/automation-starting-a-runbook.md).
* Ophalen van een [overzicht van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md), en gedetailleerde hoge frequentie metrische gegevens verzamelen over uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar en reageert is.

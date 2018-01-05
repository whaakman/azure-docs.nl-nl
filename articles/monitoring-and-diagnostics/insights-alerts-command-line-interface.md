---
title: Meldingen instellen voor Azure-services - platformoverschrijdende CLI | Microsoft Docs
description: Trigger e-mailberichten, meldingen, worden URL's van websites (webhooks) of automation aanroepen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: 92246a8da73a244a1c9a924bed55711d71a20fd8
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a>Metrische waarschuwingen maken in de Azure-Monitor voor Azure-services - platformoverschrijdende CLI
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Overzicht
In dit artikel leest u hoe Azure metrische waarschuwingen met behulp van de platformoverschrijdende opdrachtregelinterface (CLI) instellen.

> [!NOTE]
> Monitor voor Azure is de nieuwe naam voor wat 'Azure Insights' is aangeroepen tot 25 september 2016. Echter bevatten de naamruimten en dus de onderstaande opdrachten nog steeds de 'inzichten'.
>
>

U kunt een waarschuwing op basis van bewaking metrische gegevens voor of gebeurtenissen op uw Azure-services kunt ontvangen.

* **Metrische waarden** -de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en vervolgens later wanneer die voorwaarde wordt niet langer wordt voldaan.    
* **Activiteit logboekgebeurtenissen** -een waarschuwing kunt activeren voor *elke* gebeurtenis of alleen wanneer een bepaalde gebeurtenissen optreden. Voor meer informatie over waarschuwingen voor activiteit logboek [Klik hier](monitoring-activity-log-alerts.md)

U kunt het volgende te doen als er wordt een waarschuwing voor metrische configureren:

* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar andere e-mailberichten die u opgeeft.
* een webhook aanroepen
* uitvoering van een Azure-runbook (alleen van de Azure-portal op dit moment) starten

U kunt configureren en ophalen van informatie over metrische waarschuwingsregels met

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
* [Monitor voor Azure REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

U kunt altijd help voor opdrachten ontvangen door een opdracht te typen en stellen - help aan het einde. Bijvoorbeeld:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Met behulp van de CLI waarschuwingsregels maken
1. Het uitvoeren van de vereisten en aanmelden bij Azure. Zie [Azure Monitor CLI voorbeelden](insights-cli-samples.md). Kortom, CLI installeren en voer deze opdrachten. Ze u aangemeld, laten zien welke abonnement u gebruikt en voorbereiding op Azure Monitor opdrachten uit te voeren.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. Als u bestaande regels voor een resourcegroep wilt weergeven, gebruikt u de volgende indeling **inzicht van azure waarschuwingen regel lijst** *[opties] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Als u wilt een regel maakt, moet u eerst hebt van belangrijke stukjes informatie.
  * De **Resource-ID** voor de resource die u wilt een waarschuwing voor het instellen
  * De **metrische definities** beschikbaar voor die bron

     Een manier om op te halen van de Resource-ID is de Azure portal gebruiken. Ervan uitgaande dat de resource is al gemaakt, selecteert u deze in de portal. Selecteer in de volgende blade *eigenschappen* onder de *instellingen* sectie. De *RESOURCE-ID* is een veld in de volgende blade. Een andere manier is met de [Azure Resource Explorer](https://resources.azure.com/).

     Een voorbeeld van de resource-id voor een web-app is

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Als u een lijst van de beschikbare metrische gegevens en de eenheden voor deze metrische gegevens voor het vorige voorbeeld van de resource, gebruikt u de volgende CLI-opdracht:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* wordt de granulatie van de beschikbare meting (1 minuut intervallen). Met andere granulaties hebt u verschillende metrische opties.
4. Gebruik een opdracht van de volgende notatie voor het maken van een waarschuwingsregel op basis van metrische gegevens:

    **inzicht van Azure waarschuwingen metrische regelset** *[opties] &lt;ruleName&gt; &lt;locatie&gt; &lt;resourceGroup&gt; &lt;venstergrootte &gt; &lt;operator&gt; &lt;drempelwaarde&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt; timeAggregationOperator&gt;*

    Het volgende voorbeeld heeft een waarschuwing van een website resource ingesteld. De waarschuwing triggers wanneer het consistent verkeer ontvangt voor 5 minuten en opnieuw wanneer er geen verkeer ontvangen gedurende vijf minuten.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Als u webhook maken of e-mail verzenden wanneer een metrische waarschuwing wordt geactiveerd, moet u eerst de e-mailadres en/of webhooks maken. Vervolgens maakt u de regel onmiddellijk daarna. Kan geen koppelt u webhook of e-mailberichten met regels die gebruikmaken van de CLI al is gemaakt.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. U kunt controleren of uw waarschuwingen correct zijn gemaakt door te kijken op een afzonderlijke regel.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Als u wilt verwijderen van regels, een opdracht van het formulier te gebruiken:

    **Waarschuwingen regel verwijderen Insights** [opties] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Deze opdrachten verwijdert u de regels die eerder in dit artikel hebt gemaakt.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](monitoring-overview.md) met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Meer informatie over [configureren van webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [waarschuwingen configureren op activiteit logboekgebeurtenissen](monitoring-activity-log-alerts.md).
* Meer informatie over [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Ophalen van een [overzicht van het verzamelen van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) voor het verzamelen van gedetailleerde hoge frequentie metrische gegevens op uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.

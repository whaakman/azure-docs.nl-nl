---
title: Meldingen instellen voor Azure-services - PowerShell | Microsoft Docs
description: Trigger e-mailberichten, meldingen, worden URL's van websites (webhooks) of automation aanroepen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: rboucher
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: robb
ms.openlocfilehash: 8f7df424b27e6899821a9bdd2f1d8397a0de35a7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="create-classic-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a>Klassieke metrische waarschuwingen in de Azure-Monitor maken voor Azure-services - PowerShell
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Overzicht

> [!NOTE]
> In dit artikel wordt beschreven hoe oudere klassieke metrische waarschuwingen maken. Azure ondersteunt van de Monitor nu [nieuwere, metrische waarschuwingen voor een betere](monitoring-near-real-time-metric-alerts.md). Deze waarschuwingen kunnen meerdere metrische gegevens controleren en waarschuwen voor dimensionale metrische gegevens mogelijk maken. PowerShell-ondersteuning voor nieuwere metrische waarschuwingen is binnenkort beschikbaar.
>
>

In dit artikel leest u hoe Azure classic metrische waarschuwingen met behulp van PowerShell instellen.  

U kunt een waarschuwing op basis van bewaking metrische gegevens voor of gebeurtenissen op uw Azure-services kunt ontvangen.

* **Metrische waarden** -de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en vervolgens later wanneer die voorwaarde wordt niet langer wordt voldaan.    
* **Activiteit logboekgebeurtenissen** -een waarschuwing kunt activeren voor *elke* gebeurtenis of alleen wanneer een bepaalde gebeurtenis zich voordoet. Voor meer informatie over waarschuwingen voor activiteit logboek [Klik hier](monitoring-activity-log-alerts.md)

U kunt het volgende te doen als er wordt een klassieke metrische-waarschuwing configureren:

* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar andere e-mailberichten die u opgeeft.
* een webhook aanroepen
* starten van de uitvoering van een Azure-runbook (alleen van de Azure-portal)

U kunt configureren en informatie ophalen over met behulp van regels voor waarschuwingen

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
* [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Voor meer informatie, kunt u altijd typen ```Get-Help``` en vervolgens de PowerShell-opdracht op het gewenste help.

## <a name="create-alert-rules-in-powershell"></a>Maken van regels voor waarschuwingen in PowerShell
1. Aanmelden bij Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. Een lijst van de abonnementen er beschikbaar zijn. Controleer of u werkt met het juiste abonnement. Als dit niet het geval is, stel deze in op de juiste is met behulp van de uitvoer van `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. U kunt bestaande regels voor een resourcegroep gebruiken de volgende opdracht:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Als u wilt een regel maakt, moet u eerst hebt van belangrijke stukjes informatie.

  * De **Resource-ID** voor de resource die u wilt een waarschuwing voor het instellen
  * De **metrische definities** beschikbaar voor die bron

     Een manier om op te halen van de Resource-ID is de Azure portal gebruiken. Ervan uitgaande dat de resource is al gemaakt, selecteert u deze in de portal. Selecteer in de volgende blade *eigenschappen* onder de *instellingen* sectie. **De RESOURCE-ID** is een veld in de volgende blade. Een andere manier is met de [Azure Resource Explorer](https://resources.azure.com/).

     Een voorbeeld van de Resource-ID voor een web-app is

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     U kunt `Get-AzureRmMetricDefinition` om de lijst met alle metrische definities voor een specifieke bron weer te geven.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Het volgende voorbeeld wordt een tabel met de naam van de metriek en de eenheid voor deze metriek gegenereerd.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Een volledige lijst met beschikbare opties voor Get-AzureRmMetricDefinition is beschikbaar door het uitvoeren van `Get-Help Get-AzureRmMetricDefinition -Detailed`.
5. Het volgende voorbeeld heeft een waarschuwing van een website resource ingesteld. De waarschuwing triggers wanneer het consistent verkeer ontvangt voor 5 minuten en opnieuw wanneer er geen verkeer ontvangen gedurende vijf minuten.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Als u webhook maken of e-mail verzenden wanneer een waarschuwing wordt geactiveerd, moet u eerst de e-mailadres en/of webhooks maken. Vervolgens onmiddellijk de regel daarna met de - tag acties en maken zoals weergegeven in het volgende voorbeeld. Kan geen koppelt u webhook of e-mailberichten met regels via PowerShell al is gemaakt.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Om te controleren of uw waarschuwingen correct zijn gemaakt door te kijken naar de afzonderlijke regels.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Uw waarschuwingen verwijderen. Deze opdrachten verwijdert u de regels die eerder in dit artikel worden gemaakt.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](monitoring-overview.md) met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Informatie over het [webhooks configureren in waarschuwingen](insights-webhooks-alerts.md).
* Informatie over het [meldingen configureren voor de activiteit logboekgebeurtenissen](monitoring-activity-log-alerts.md).
* Meer informatie over [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Ophalen van een [overzicht van het verzamelen van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) voor het verzamelen van gedetailleerde hoge frequentie metrische gegevens op uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.

---
title: PowerShell gebruiken voor het maken van klassieke waarschuwingen voor Azure-services | Microsoft Docs
description: E-mailberichten of meldingen activeren of website-URL's (webhooks) of automation aanroepen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: b08a8f66add45d64085ac05605fe3c7d7f91b705
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286196"
---
# <a name="use-powershell-to-create-alerts-for-azure-services"></a>PowerShell gebruiken voor het maken van waarschuwingen voor Azure-services
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

> [!NOTE]
> In dit artikel wordt beschreven hoe oudere klassieke metrische waarschuwingen maken. Azure ondersteunt van de Monitor nu [nieuwere, metrische waarschuwingen voor een betere](monitoring-near-real-time-metric-alerts.md). Deze waarschuwingen kunnen meerdere metrische gegevens controleren en waarschuwen voor dimensionale metrische gegevens mogelijk maken. PowerShell-ondersteuning voor nieuwere metrische waarschuwingen is binnenkort beschikbaar.
>
>

Dit artikel laat zien hoe u waarschuwingen van Azure classic metrische instelt met behulp van PowerShell.  

U kunt waarschuwingen op basis van de metrische gegevens voor uw Azure-services kunt ontvangen of kunt u waarschuwingen voor gebeurtenissen die plaatsvinden ontvangen in Azure.

* **Metrische waarden**: de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en wanneer die voorwaarde niet langer wordt voldaan.    
* **Activiteit logboekgebeurtenissen**: een waarschuwing kunt activeren voor *elke* gebeurtenis of wanneer bepaalde gebeurtenissen optreden. Zie voor meer informatie over waarschuwingen voor activiteit logboek [activiteit logboek waarschuwingen (klassiek) maken](monitoring-activity-log-alerts.md).

U kunt hiervoor de volgende taken wanneer er wordt een klassieke metrische-waarschuwing configureren:

* E-mailmeldingen verzenden naar de servicebeheerder en medebeheerders.
* E-mail verzenden naar andere e-mailadressen die u opgeeft.
* Een webhook aanroepen.
* Start de uitvoering van een Azure-runbook (alleen van de Azure-portal).

U kunt configureren en ophalen van informatie over de regels voor waarschuwingen uit de volgende locaties: 

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure-opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
* [Monitor voor Azure REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Voor meer informatie, kunt u altijd typen ```Get-Help``` gevolgd door de PowerShell-opdracht die u hulp nodig.

## <a name="create-alert-rules-in-powershell"></a>Maken van regels voor waarschuwingen in PowerShell
1. Meld u aan bij Azure.   

    ```PowerShell
    Connect-AzureRmAccount

    ```
2. Haal een lijst van de abonnementen die voor u beschikbaar zijn. Controleer of u met het juiste abonnement werkt. Als dat niet ophalen voor het juiste abonnement met behulp van de uitvoer van `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Een overzicht van bestaande regels voor een resourcegroep met de volgende opdracht:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Als u wilt een regel maakt, moet u eerst hebt van belangrijke stukjes informatie.

    - De **resource-ID** voor de resource die u wilt een waarschuwing voor het instellen.
    - De **metrische definities** die beschikbaar zijn voor die bron.

     Een manier om op te halen van de resource-ID is de Azure portal gebruiken. Ervan uitgaande dat de resource al gemaakt is, selecteert u deze in de portal. Klik in de volgende blade in de **instellingen** sectie **eigenschappen**. **De RESOURCE-ID** is een veld in de volgende blade. 
     
     U kunt ook de resource-ID opvragen met behulp van [Azure Resource Explorer](https://resources.azure.com/).

     Hier volgt een voorbeeld van de resource-ID voor een web-app:

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     U kunt `Get-AzureRmMetricDefinition` om de lijst met alle metrische definities voor een specifieke bron weer te geven:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Het volgende voorbeeld wordt een tabel met de naam van de meetwaarde en eenheid voor deze metriek gegenereerd:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Uitvoeren als u een volledige lijst met beschikbare opties voor Get-AzureRmMetricDefinition, `Get-Help Get-AzureRmMetricDefinition -Detailed`.
5. Het volgende voorbeeld heeft een waarschuwing van een website resource ingesteld. De waarschuwing triggers wanneer het consistent verkeer ontvangt voor 5 minuten en opnieuw wanneer er geen verkeer ontvangen gedurende vijf minuten.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Voor het maken van een webhook of e-mail verzenden wanneer een waarschuwing wordt geactiveerd, moet u eerst de e-mailadres of webhook maken. Maak de regel onmiddellijk daarna met de - tag acties zoals weergegeven in het volgende voorbeeld. U kunt webhooks of e-mailberichten niet koppelen aan regels die al zijn gemaakt.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Bekijk de afzonderlijke regels om te controleren of uw waarschuwingen correct is gemaakt.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Uw waarschuwingen verwijderen. Deze opdrachten verwijdert u de regels die eerder in dit artikel zijn gemaakt.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](monitoring-overview.md), met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Informatie over het [webhooks configureren in waarschuwingen](insights-webhooks-alerts.md).
* Informatie over het [meldingen configureren voor de activiteit logboekgebeurtenissen](monitoring-activity-log-alerts.md).
* Meer informatie over [Azure Automation-runbooks](../automation/automation-starting-a-runbook.md).
* Ophalen van een [overzicht van het verzamelen van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) voor het verzamelen van gedetailleerde hoge frequentie metrische gegevens op uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.

---
title: Doorsturen van gegevens van Azure Automation-taak met OMS Log Analytics | Microsoft Docs
description: In dit artikel laat zien hoe u met het verzenden van taakstatus en runbook-taak streams Microsoft Operations Management Suite-logboekanalyse meer inzicht leveren en het beheer.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: 0319a7b9248dec9d7cdabba9c18a25463d94284b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>Doorsturen taakstatus en taak stromen van Automation voor logboekanalyse (OMS)
Automation kan runbook taak status en taak streams verzenden naar de werkruimte voor logboekanalyse voor Microsoft Operations Management Suite (OMS). Taak registreert en taak streams zichtbaar zijn in de Azure-portal of PowerShell zijn voor afzonderlijke taken en Hiermee kunt u eenvoudige onderzoeken uitvoeren. Met Log Analytics kunt u nu:

* Inzicht verkrijgen in uw Automation-taken.
* De trigger is een e-mailadres of de waarschuwing op basis van de status van de taak runbook (bijvoorbeeld is mislukt of onderbroken).
* Geavanceerde query's in uw taak stromen schrijven.
* Correleren taken via Automation-accounts.
* Visualiseer uw Taakgeschiedenis gedurende een bepaalde periode.

## <a name="prerequisites-and-deployment-considerations"></a>Vereisten en overwegingen voor de implementatie
Als u wilt beginnen met het verzenden van uw Automation-logboeken met Log Analytics, hebt u het volgende nodig:

* De November 2016 of hoger release van [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
* Een werkruimte voor logboekanalyse. Zie voor meer informatie [aan de slag met logboekanalyse](../log-analytics/log-analytics-get-started.md). 
* De ResourceId voor uw Azure Automation-account.


De ResourceId vinden voor uw Azure Automation-account:

```powershell-interactive
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Voer de volgende PowerShell ResourceId van de voor uw werkruimte voor logboekanalyse vindt:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Als er meer dan een Automation-accounts of werkruimten, in de uitvoer van de bovenstaande opdrachten, vinden de *naam* hoeft te configureren en kopieer de waarde voor *ResourceId*.

Als u wilt zoeken naar de *naam* van uw Automation-account in de Azure portal, selecteer uw Automation-account van de **Automation-account** blade en selecteer **alle instellingen** . Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**.  Noteer de waarden die op de blade **Eigenschappen** worden weergegeven.<br> ![Eigenschappen van Automation-Account](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Integratie met logboekanalyse instellen

1. Start op uw computer **Windows PowerShell** van de **Start** scherm.
2. Voer de volgende PowerShell- en bewerkt u de waarde voor de `[your resource id]` en `[resource id of the log analytics workspace]` met de waarden uit de vorige stap.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true
   ```

Na dit script uitvoert, ziet u records in logboekanalyse binnen 10 minuten van nieuwe JobLogs of JobStreams wordt geschreven.

Overzicht van de logboeken, voer de volgende query in logboekanalyse logboek zoeken:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION""`

### <a name="verify-configuration"></a>Configuratie controleren
Om te bevestigen dat uw Automation-account logboeken naar uw werkruimte voor logboekanalyse is verzendt, moet u controleren of diagnostische gegevens juist zijn geconfigureerd op het Automation-account met behulp van de volgende PowerShell:

```powershell-interactive
Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Zorg ervoor dat in de uitvoer:
+ Onder *logboeken*, de waarde voor *ingeschakeld* is *True*.
+ De waarde van *WorkspaceId* is ingesteld op de ResourceId van de werkruimte voor logboekanalyse.

## <a name="log-analytics-records"></a>Log Analytics-records

Diagnostische gegevens van Azure Automation maakt u twee soorten records in Log Analytics en zijn gelabeld als **AzureDiagnostics**. De volgende query's gebruiken de bijgewerkte querytaal met logboekanalyse. Ga naar voor informatie over algemene query's tussen verouderde querytaal en de nieuwe Azure Log Analytics query language [verouderde naar de nieuwe Azure Log Analytics Query Language-referentieoverzicht](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language)

### <a name="job-logs"></a>Taaklogboeken
| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| Tenant_g | GUID die de tenant voor de aanroeper identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| ResultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Nieuwe<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>-Voltooid |
| Category | Classificatie van het type gegevens. Voor Automation is de waarde JobLogs. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. De waarde is voor automatisering kunt taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Log Analytics verzameld hoe de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Hiermee wordt resultaatstatus van de runbooktaak beschreven. Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid |
| CorrelationId |De GUID die de correlatie-id van de runbooktaak is. |
| ResourceId |Hiermee geeft u de Azure Automation-account-id van het runbook. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | Naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT CORPORATION. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Taak stromen
| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| StreamType_s |Het type taakstroom. Mogelijke waarden zijn:<br>- Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid |
| Tenant_g | GUID die de tenant voor de aanroeper identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| ResultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-In voortgang |
| Category | Classificatie van het type gegevens. Voor Automation is de waarde JobStreams. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. De waarde is voor automatisering kunt taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Log Analytics verzameld hoe de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Bevat de uitvoerstroom van het runbook. |
| CorrelationId |De GUID die de correlatie-id van de runbooktaak is. |
| ResourceId |Hiermee geeft u de Azure Automation-account-id van het runbook. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | Naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT CORPORATION. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Log Analytics Automation weer aanmeldt
Nu u uw Automation-taaklogboeken verzenden met logboekanalyse gestart, gaan we kijken wat u kunt doen met deze logboeken in logboekanalyse.

Overzicht van de logboeken, voer de volgende query:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Een e-mailbericht verzenden wanneer een runbooktaak is mislukt of wordt onderbroken
Een van de bovenste klant vraagt is voor de mogelijkheid voor het verzenden van een e-mailbericht of een tekst wanneer er iets mis met een runbooktaak gaat.   

Als u wilt een waarschuwingsregel maakt, begint u met het maken van een zoekopdracht logboek voor de runbook-taak-records die de waarschuwing moet worden aangeroepen. Klik op de **waarschuwing** knop maken en configureren van de waarschuwingsregel.

1. Klik op de pagina overzicht van Log Analytics **logboek zoeken**.
2. Maken van een zoekquery logboek voor de waarschuwing door de volgende zoeken in het queryveld typen: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` u kunt de RunbookName groeperen met behulp van:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Als u logboeken van meer dan een Automation-account of -abonnement aan uw werkruimte, kunt u uw waarschuwingen per abonnement en de Automation-account kunt groeperen. Automation-accountnaam vindt u in het veld Resource in het doorzoeken van JobLogs.
1. Openen van de **waarschuwingsregel toevoegen** scherm, klikt u op **waarschuwing** boven aan de pagina. Zie voor meer informatie over de opties voor het configureren van de waarschuwing [waarschuwingen in logboekanalyse](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Alle taken die zijn voltooid met fouten vinden
Naast de waarschuwingen op fouten, vindt u wanneer een runbooktaak een fout niet wordt beëindigd heeft. In dergelijke gevallen PowerShell produceert een foutstroom, maar de fouten niet wordt beëindigd niet tot gevolg dat de taak onderbreken of als mislukt.    

1. Klik in de werkruimte voor logboekanalyse op **logboek zoeken**.
2. Typ in het queryveld `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` en klik vervolgens op de **Search** knop.

### <a name="view-job-streams-for-a-job"></a>Weergave taak stromen voor een taak
Wanneer u een taak foutopsporing, kunt u ook om te zoeken in de taak stromen. De volgende query geeft alle stromen voor een enkele taak met GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Status van de historische taak weergeven
U kunt ten slotte uw Taakgeschiedenis visualiseren gedurende een bepaalde periode. U kunt deze query gebruiken om te zoeken naar de status van de taken gedurende een bepaalde periode.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![OMS historische taak Statusgrafiek](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Samenvatting
Verzenden van uw Automation-taak status en stream-gegevens met Log Analytics, krijgt u inzicht in de status van uw Automation-taken op:
+ Waarschuwingen instellen om u te waarschuwen wanneer er een probleem.
+ Met behulp van aangepaste weergaven en zoekopdrachten voor het visualiseren van de runbookresultaten, gerelateerde de status van de runbook-taak en andere indicatoren of metrische gegevens.  

Log Analytics biedt meer operationeel inzicht voor uw Automation-taken en adres incidenten sneller kan helpen.  

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van verschillende zoekquery's en de Automation-taak logboeken met Log Analytics, [zoekopdrachten aanmelden met logboekanalyse](../log-analytics/log-analytics-log-searches.md).
* Zie voor informatie over het maken en ophalen van de uitvoer en foutberichten van runbooks, [Runbook uitvoer en berichten](automation-runbook-output-and-messages.md).
* Zie [Runbooktaken bijhouden](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details.
* Zie voor meer informatie over OMS Log Analytics en verzameling gegevensbronnen, [verzamelen van Azure storage-gegevens in het overzicht van logboekanalyse](../log-analytics/log-analytics-azure-storage.md).

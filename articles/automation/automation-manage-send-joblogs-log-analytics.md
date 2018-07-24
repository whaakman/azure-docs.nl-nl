---
title: Azure Automation-taakgegevens doorsturen naar Log Analytics
description: In dit artikel laat zien hoe u met het verzenden van taakstatus en runbook-taakstromen met Azure Log Analytics om meer inzicht en beheer te bieden.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/12/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 12628b5a552b864784d780e5f2adc00aac579911
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215030"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics"></a>Taakstatus en taakstromen van Automation doorsturen naar Log Analytics
Automation kunt runbook en taakstromen van een taak verzenden naar uw Log Analytics-werkruimte. Taaklogboeken en taakstromen zijn zichtbaar in de Azure portal of met PowerShell, voor afzonderlijke taken en deze kunt u eenvoudige onderzoek uitvoeren. Met Log Analytics kunt u nu:

* Krijg inzicht in uw Automation-taken.
* De trigger is een e-mailadres of de waarschuwing op basis van uw runbook job status (voor bijvoorbeeld mislukte of onderbroken).
* Geavanceerde query's schrijven over uw taakstromen.
* Combineer taken van Automation-accounts.
* Visualiseer uw Taakgeschiedenis na verloop van tijd.

## <a name="prerequisites-and-deployment-considerations"></a>Vereisten en overwegingen voor implementatie
Als u wilt beginnen met het verzenden van uw Automation-logboeken naar Log Analytics, hebt u het volgende nodig:

* De November 2016 of hoger van de release [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
* Een Log Analytics-werkruimte. Zie voor meer informatie, [aan de slag met Log Analytics](../log-analytics/log-analytics-get-started.md). 
* De ResourceId voor uw Azure Automation-account.


Zoek de ResourceId voor uw Azure Automation-account:

```powershell-interactive
# Find the ResourceId for the Automation Account
Get-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Als u zoekt de ResourceId voor uw Log Analytics-werkruimte, voer de volgende PowerShell:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Als u meer dan één Automation-accounts hebben, of als werkruimten in de uitvoer van de voorgaande opdrachten, vinden de *naam* u wilt configureren en kopieer de waarde voor *ResourceId*.

Als u wilt zoeken de *naam* van uw Automation-account, selecteert u in Azure portal uw Automation-account van de **Automation-account** blade en selecteer **alle instellingen** . Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**.  Noteer de waarden die op de blade **Eigenschappen** worden weergegeven.<br> ![Eigenschappen van Automation-Account](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Integratie met Log Analytics instellen

1. Start op uw computer **Windows PowerShell** uit de **Start** scherm.
2. Voer de volgende PowerShell uit en bewerkt u de waarde voor de `[your resource id]` en `[resource id of the log analytics workspace]` met de waarden uit de vorige stap.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true
   ```

Na dit script is uitgevoerd, ziet u records in Log Analytics binnen 10 minuten van nieuwe JobLogs of JobStreams wordt geschreven.

Als u wilt zien van de logboeken, moet u de volgende query uitvoeren in Log Analytics zoeken in Logboeken: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Configuratie controleren
Om te bevestigen dat uw Automation-account logboeken naar uw Log Analytics-werkruimte verzendt, controleert u of diagnostische gegevens correct zijn geconfigureerd op het Automation-account met behulp van de volgende PowerShell:

```powershell-interactive
Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Zorg ervoor dat in de uitvoer:
+ Onder *logboeken*, de waarde voor *ingeschakeld* is *waar*.
+ De waarde van *WorkspaceId* is ingesteld op de ResourceId van uw Log Analytics-werkruimte.

## <a name="log-analytics-records"></a>Log Analytics-records

Diagnostische gegevens van Azure Automation worden twee typen records gemaakt in Log Analytics en zijn gelabeld als **AzureDiagnostics**. De volgende query's gebruikt u de bijgewerkte querytaal Log Analytics. Bezoek voor meer informatie over algemene query's tussen de verouderde query-taal en de nieuwe Azure Log Analytics-querytaal [verouderde naar de nieuwe querytaal van Azure Log Analytics-referentiemateriaal](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language)

### <a name="job-logs"></a>Taaklogboeken
| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| Tenant_g | De GUID die de tenant voor de oproepende functie identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| resultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Nieuwe<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>-Voltooid |
| Categorie | Classificatie van het type gegevens. Voor Automation is de waarde JobLogs. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. Voor Automation is is de waarde van taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Hoe worden de gegevens verzameld door Log Analytics. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Hiermee wordt resultaatstatus van de runbooktaak beschreven. Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid |
| CorrelationId |De GUID die de correlatie-id van de runbooktaak is. |
| ResourceId |Hiermee geeft u de Azure Automation-account resource-id van het runbook. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | De naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Taakstromen
| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| StreamType_s |Het type taakstroom. Mogelijke waarden zijn:<br>- Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid |
| Tenant_g | De GUID die de tenant voor de oproepende functie identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| resultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Wordt uitgevoerd |
| Categorie | Classificatie van het type gegevens. Voor Automation is de waarde JobStreams. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. Voor Automation is is de waarde van taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Hoe worden de gegevens verzameld door Log Analytics. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Bevat de uitvoerstroom van het runbook. |
| CorrelationId |De GUID die de correlatie-id van de runbooktaak is. |
| ResourceId |Hiermee geeft u de Azure Automation-account resource-id van het runbook. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | De naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Weergeven van Automation-Logboeken in Log Analytics
Nu dat u begint met het verzenden van uw Automation-taaklogboeken met Log Analytics, laten we zien wat u kunt doen met deze logboeken in Log Analytics.

Als u wilt zien van de logboeken, voer de volgende query uit: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Een e-mailbericht verzenden wanneer een runbook-taak is mislukt of wordt onderbroken
Een van de grote klanten wordt gevraagd de mogelijkheid voor het verzenden van een e-mailadres of een SMS-bericht wanneer er iets met een runbook-taak misgaat.   

Voor het maken van een waarschuwingsregel, begint u met het maken van een zoeken in Logboeken voor de runbook-Taakrecords die de waarschuwing moet worden aangeroepen. Klik op de **waarschuwing** knop maken en configureren van de waarschuwingsregel.

1. Klik op de pagina overzicht van Log Analytics **zoeken in logboeken**.
2. Maken van een zoekquery logboek voor de waarschuwing door te zoeken in de volgende typen in het queryveld: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` u ook de RunbookName kunt groeperen met behulp van: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Als u logboeken van meer dan één Automation-account of abonnement aan uw werkruimte, kunt u uw waarschuwingen op basis van abonnement en de Automation-account kunt groeperen. Naam van het Automation-account kan worden gevonden in het veld Resource in het zoekvak van JobLogs.
1. Om te openen de **maken regel** scherm, klikt u op **+ nieuwe waarschuwingsregel** aan de bovenkant van de pagina. Zie voor meer informatie over de opties voor het configureren van de waarschuwing [waarschuwingen voor activiteitenlogboeken in Azure](../monitoring-and-diagnostics/monitor-alerts-unified-log.md).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Alle taken hebt voltooid met fouten zoeken
U kunt naast de waarschuwingen op fouten, wanneer een runbook-taak een niet-afsluitfout heeft vinden. In dergelijke gevallen PowerShell een foutstroom produceert, maar de niet-afsluitfouten niet tot gevolg dat de taak onderbreken of als mislukt.    

1. Klik in uw Log Analytics-werkruimte op **zoeken in logboeken**.
2. Typ in het queryveld `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` en klik vervolgens op de **zoeken** knop.

### <a name="view-job-streams-for-a-job"></a>Taakstromen weergeven voor een taak
Wanneer u een taak foutopsporing, kunt u ook de taakstromen kan bekijken. De volgende query geeft alle stromen voor een enkele taak met GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Status van de historische taak weergeven
Ten slotte kunt u voor het visualiseren van uw Taakgeschiedenis na verloop van tijd. U kunt deze query gebruiken om te zoeken naar de status van uw taken na verloop van tijd.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![Log Analytics historische taak Statusgrafiek](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Samenvatting
Uw Automation-taak de status en stream-gegevens verzenden naar Log Analytics, krijgt u inzicht in de status van uw Automation-taken op:
+ Instellen van waarschuwingen om u te waarschuwen wanneer er een probleem.
+ Met behulp van aangepaste weergaven en zoekopdrachten naar uw runbookresultaten te visualiseren, gerelateerde de status van de runbook-taak en andere KPI's of metrische gegevens.  

Log Analytics biedt tevens grotere operationele zichtbaarheid aan uw Automation-taken en kunt u incidenten sneller.  

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van verschillende zoekquery's en bekijk de Automation-taaklogboeken met Log Analytics, [zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md).
* Zie voor meer informatie over het maken en ophalen van de uitvoer en foutberichten van runbooks, [Runbook-uitvoer en berichten](automation-runbook-output-and-messages.md).
* Zie [Runbooktaken bijhouden](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details.
* Zie voor meer informatie over Log Analytics en gegevensverzamelingsbronnen [verzamelen van Azure storage-gegevens in Log Analytics-overzicht](../log-analytics/log-analytics-azure-storage.md).

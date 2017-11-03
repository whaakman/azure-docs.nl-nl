---
title: Doorsturen van gegevens van Azure Automation-taak met OMS Log Analytics | Microsoft Docs
description: In dit artikel laat zien hoe u met het verzenden van taakstatus en runbook-taak streams Microsoft Operations Management Suite-logboekanalyse meer inzicht leveren en het beheer.
services: automation
documentationcenter: 
author: eslesar
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
ms.openlocfilehash: 21923adaa8f8118995799319c1fd496a6e449faa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>Doorsturen taakstatus en taak stromen van Automation voor logboekanalyse (OMS)
Automation kan runbook taak status en taak streams verzenden naar de werkruimte voor logboekanalyse voor Microsoft Operations Management Suite (OMS).  Taak registreert en taak streams zichtbaar zijn in de Azure-portal of PowerShell zijn voor afzonderlijke taken en Hiermee kunt u eenvoudige onderzoeken uitvoeren. Met Log Analytics kunt u nu:

* Inzicht verkrijgen in uw Automation-taken
* Trigger een e-mailadres of de waarschuwing op basis van de status van de taak runbook (bijvoorbeeld is mislukt of onderbroken)
* Geavanceerde query's in uw taak stromen schrijven
* Taken correleren via Automation-accounts
* De taakgeschiedenis visualiseren gedurende een periode     

## <a name="prerequisites-and-deployment-considerations"></a>Vereisten en overwegingen voor de implementatie
Als u wilt beginnen met het verzenden van uw Automation-logboeken met Log Analytics, hebt u het volgende nodig:

1. De November 2016 of hoger release van [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. Een werkruimte voor logboekanalyse. Zie voor meer informatie [aan de slag met logboekanalyse](../log-analytics/log-analytics-get-started.md). 
3. De ResourceId voor uw Azure Automation-account

Voer de volgende PowerShell ResourceId van de voor uw Azure Automation-account en de werkruimte voor logboekanalyse vindt:

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Als er meerdere Automation-accounts of werkruimten, in de uitvoer van de bovenstaande opdrachten, vinden de *naam* hoeft te configureren en kopieer de waarde voor *ResourceId*.

Als u wilt zoeken naar de *naam* van uw Automation-account in de Azure portal, selecteer uw Automation-account van de **Automation-account** blade en selecteer **alle instellingen** .  Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**.  Noteer de waarden die op de blade **Eigenschappen** worden weergegeven.<br> ![Eigenschappen van Automation-Account](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Integratie met logboekanalyse instellen
1. Start op uw computer **Windows PowerShell** van de **Start** scherm.  
2. Kopieer en plak de volgende PowerShell en bewerken van de waarde voor de `$workspaceId` en `$automationAccountId`.  Voor de `-Environment` parameter, geldige waarden zijn *AzureCloud* of *AzureUSGovernment* , afhankelijk van de cloudomgeving die u in werkt.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

Na dit script uitvoert, ziet u records in logboekanalyse binnen 10 minuten van nieuwe JobLogs of JobStreams wordt geschreven.

Overzicht van de logboeken, voer de volgende query in logboekanalyse logboek zoeken:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Configuratie controleren
Om te bevestigen dat uw Automation-account logboeken naar uw werkruimte voor logboekanalyse is verzendt, moet u controleren of diagnostische gegevens correct zijn ingesteld op het Automation-account met behulp van de volgende PowerShell:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Zorg ervoor dat in de uitvoer:
+ Onder *logboeken*, de waarde voor *ingeschakeld* is *True*
+ De waarde van *WorkspaceId* is ingesteld op de ResourceId van de werkruimte voor logboekanalyse


## <a name="log-analytics-records"></a>Log Analytics-records
Diagnostische gegevens van Azure Automation maakt u twee soorten records in Log Analytics en zijn gelabeld als **Type = AzureDiagnostics**.

### <a name="job-logs"></a>Taaklogboeken
| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart.  Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| Tenant_g | GUID die de tenant voor de aanroeper identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| ResultType |De status van de runbooktaak.  Mogelijke waarden zijn:<br>-Nieuwe<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>-Voltooid |
| Category | Classificatie van het type gegevens.  Voor Automation is de waarde JobLogs. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd.  De waarde is voor automatisering kunt taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Log Analytics verzameld hoe de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Hiermee wordt resultaatstatus van de runbooktaak beschreven.  Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid |
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
| Caller_s |Wie de bewerking heeft gestart.  Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| StreamType_s |Het type taakstroom. Mogelijke waarden zijn:<br>- Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid |
| Tenant_g | GUID die de tenant voor de aanroeper identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| ResultType |De status van de runbooktaak.  Mogelijke waarden zijn:<br>-In voortgang |
| Category | Classificatie van het type gegevens.  Voor Automation is de waarde JobStreams. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd.  De waarde is voor automatisering kunt taak. |
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
Nu uw Automation-taaklogboeken verzenden met logboekanalyse hebt gestart, gaan we kijken wat u kunt doen met deze logboeken in logboekanalyse.

Overzicht van de logboeken, voer de volgende query:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Een e-mailbericht verzenden wanneer een runbooktaak is mislukt of wordt onderbroken
Een van onze belangrijkste klant vraagt is voor de mogelijkheid voor het verzenden van een e-mailbericht of een tekst wanneer er iets mis met een runbooktaak gaat.   

Als u wilt een waarschuwingsregel maakt, begint u met het maken van een zoekopdracht logboek voor de runbook-taak-records die de waarschuwing moet worden aangeroepen.  Klik op de **waarschuwing** knop maken en configureren van de waarschuwingsregel.

1. Klik op de pagina overzicht van Log Analytics **logboek zoeken**.
2. Maken van een zoekquery logboek voor de waarschuwing door de volgende zoeken in het queryveld typen: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` u kunt de RunbookName groeperen met behulp van:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Als u naar de werkruimte logboeken van meer dan een Automation-account of -abonnement hebt ingesteld, kunt u uw waarschuwingen per abonnement en de Automation-account kunt groeperen.  Automation-accountnaam kan worden afgeleid van het veld Resource in het doorzoeken van JobLogs.  
3. Openen van de **waarschuwingsregel toevoegen** scherm, klikt u op **waarschuwing** boven aan de pagina. Zie voor meer informatie over de opties voor het configureren van de waarschuwing [waarschuwingen in logboekanalyse](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Alle taken die zijn voltooid met fouten vinden
Naast de waarschuwingen op fouten, vindt u wanneer een runbooktaak een fout niet wordt beëindigd heeft. In dergelijke gevallen PowerShell produceert een foutstroom, maar de fouten niet wordt beëindigd zorgen niet voor de taak onderbreken of als mislukt.    

1. Klik in de werkruimte voor logboekanalyse op **logboek zoeken**.
2. Typ in het queryveld `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` en klik vervolgens op **Search**.

### <a name="view-job-streams-for-a-job"></a>Weergave taak stromen voor een taak
Wanneer u een taak foutopsporing, kunt u ook om te zoeken in de taak stromen.  De volgende query geeft alle stromen voor een enkele taak met GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Status van de historische taak weergeven
U kunt ten slotte uw Taakgeschiedenis visualiseren gedurende een bepaalde periode.  U kunt deze query gebruiken om te zoeken naar de status van de taken gedurende een bepaalde periode.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![OMS historische taak Statusgrafiek](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Samenvatting
Verzenden van uw Automation-taak status en stream-gegevens met Log Analytics, krijgt u inzicht in de status van uw Automation-taken op:
+ Instellen van waarschuwingen om u te waarschuwen wanneer er een probleem
+ Met behulp van aangepaste weergaven en zoekopdrachten voor het visualiseren van de runbookresultaten, gerelateerde de status van de runbook-taak en andere indicatoren of metrische gegevens.  

Log Analytics biedt meer operationeel inzicht voor uw Automation-taken en adres incidenten sneller kan helpen.  

## <a name="next-steps"></a>Volgende stappen
* Zie [Log searches in Log Analytics (Logboekzoekopdrachten in Log Analytics)](../log-analytics/log-analytics-log-searches.md) voor meer informatie over het maken van verschillende zoekquery's en het controleren van de Automation-taaklogboeken met Log Analytics
* Zie voor informatie over het maken en ophalen van de uitvoer en foutberichten van runbooks, [Runbook uitvoer en berichten](automation-runbook-output-and-messages.md)
* Zie [Track a runbook job (Runbooktaken bijhouden)](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details
* Zie [Collecting Azure storage data in Log Analytics overview (Overzicht Azure-opslaggegevens verzamelen in Log Analytics)](../log-analytics/log-analytics-azure-storage.md) voor meer informatie over OMS Log Analytics en gegevensverzamelingsbronnen

---
title: Uitvoeren van Runbook in Azure Automation
description: Hierin wordt beschreven in de details van hoe een runbook in Azure Automation wordt verwerkt.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2b1a6e2921fdaf9ede1184cfc02c3f61f63c60ac
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393759"
---
# <a name="runbook-execution-in-azure-automation"></a>Uitvoeren van Runbook in Azure Automation

Wanneer u een runbook in Azure Automation starten, wordt een taak gemaakt. Een taak is één uitvoeringsinstantie van een runbook. Een Azure Automation worker is toegewezen aan elke taak uitvoeren. Werknemers kunnen worden gedeeld door veel Azure-accounts, zijn-taken vanaf andere Automation-accounts geïsoleerd van elkaar. U hebt niet bepalen over welke worker-services de aanvraag voor uw taak. Een enkele runbook kan hebben veel taken in één keer worden uitgevoerd. De uitvoeringsomgeving voor taken van hetzelfde Automation-Account kan opnieuw worden gebruikt. Wanneer u de lijst met runbooks in Azure portal bekijkt, geeft de status van alle taken die zijn gestart voor elk runbook. U kunt de lijst met taken voor elk runbook voor het bijhouden van de status van elke weergeven. Voor een beschrijving van de status van een andere taak [taakstatus](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

Het volgende diagram toont de levenscyclus van een runbook-taak voor [grafische runbooks](automation-runbook-types.md#graphical-runbooks) en [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks).

![Status van een taak - PowerShell-werkstroom](./media/automation-runbook-execution/job-statuses.png)

Het volgende diagram toont de levenscyclus van een runbook-taak voor [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks).

![Status van een taak - PowerShell-Script](./media/automation-runbook-execution/job-statuses-script.png)

Uw taken hebben toegang tot uw Azure-resources door het maken van een verbinding met uw Azure-abonnement. Ze hebben alleen toegang tot resources in uw datacenter als deze resources toegankelijk via de openbare cloud zijn.

## <a name="job-statuses"></a>Status van een taak

De volgende tabel beschrijft de verschillende statussen die mogelijk voor een taak zijn. PowerShell heeft twee soorten fouten, afsluitfouten als niet-afsluitfouten. De runbookstatus van de beëindigen fouten instellen op **mislukt** als ze zich voordoen. Niet-afsluitfouten kunnen het script om door te gaan, zelfs nadat deze zich voordoen. Een voorbeeld van een niet-afsluitfout wordt met behulp van de `Get-ChildItem` cmdlet door een bestaand pad bestaat niet. PowerShell ziet dat het pad niet bestaat, treedt er een fout en de volgende map blijft. Deze fout wouldn't de runbookstatus van het niet instellen op **mislukt** en kunnen worden gemarkeerd als **voltooid**. Als u wilt afdwingen dat een runbook op een niet-afsluitfout te stoppen, kunt u `-ErrorAction Stop` over de cmdlet.

| Status | Beschrijving |
|:--- |:--- |
| Voltooid |De taak is voltooid. |
| Mislukt |Voor [grafisch en PowerShell Workflow-runbooks](automation-runbook-types.md), het runbook kan niet worden gecompileerd. Voor [runbooks voor PowerShell-Script](automation-runbook-types.md), het runbook kan niet worden gestart of de taak heeft een uitzondering. |
| Is mislukt, wachten op resources |De taak is mislukt omdat het bereikt de [evenredige deel](#fair-share) drie keer beperken en vanaf het dezelfde controlepunt of vanaf het begin van het runbook gestart elke keer. |
| In wachtrij |De taak is bronnen wacht op een Automation worker beschikbaar zodat het kan worden gestart. |
| Starten |De taak is toegewezen aan een werknemer en het systeem wordt gestart. |
| Hervatten |Het systeem is de taak wordt hervat nadat deze is onderbroken. |
| In uitvoering |De taak wordt uitgevoerd. |
| Wordt uitgevoerd, wachten op resources |De taak is verwijderd omdat het bereikt de [evenredige deel](#fair-share) limiet. Het wordt hervat over enkele ogenblikken vanaf de laatste controlepunt. |
| Gestopt |De taak is gestopt door de gebruiker voordat deze was voltooid. |
| Stoppen |Het systeem wordt de taak gestopt. |
| Uitgesteld |De taak is onderbroken door de gebruiker, door het systeem of door een opdracht in het runbook. Als een runbook niet beschikt over een controlepunt, begint deze vanaf het begin van het runbook. Als er een controlepunt, kan deze opnieuw starten en hervat vanaf het laatste controlepunt. Het runbook is alleen door het systeem worden onderbroken wanneer er een uitzondering optreedt. ErrorActionPreference is standaard ingesteld op **doorgaan**, wat inhoudt dat de taak wordt uitgevoerd op een fout. Als deze voorkeursvariabele is ingesteld op **stoppen**, en vervolgens de taak wordt onderbroken op een fout. Is van toepassing op [grafisch en PowerShell Workflow-runbooks](automation-runbook-types.md) alleen. |
| Onderbreken |Het systeem probeert te onderbreken van de taak op verzoek van de gebruiker. Het runbook moet het volgende controlepunt bereiken voordat deze kan worden onderbroken. Als deze de laatste controlepunt al doorgegeven en vervolgens het proces is voltooid voordat deze kan worden onderbroken. Is van toepassing op [grafisch en PowerShell Workflow-runbooks](automation-runbook-types.md) alleen. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Status van de Azure-portal weergeven

U kunt een samengevatte status van alle runbooktaken weergeven of Zoom in op details voor een specifiek runbook-taak in Azure portal. U kunt ook configureren integratie met uw Log Analytics-werkruimte om door te sturen runbook en taakstromen van een taak. Zie voor meer informatie over de integratie met Log Analytics [taakstatus en taakstromen van Automation doorsturen naar Log Analytics](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Automation-runbooktaken samenvatting

Aan de rechterkant van het geselecteerde Automation-account, ziet u een overzicht van alle van de runbooktaken onder **taakstatistieken** tegel.

![Taakstatistieken tegel](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Deze tegel toont een telling en grafische weergave van de taak de status voor alle taken die worden uitgevoerd.

Op de tegel te klikken geeft de **taken** pagina, waaronder een overzicht van alle taken die worden uitgevoerd. Deze pagina bevat de status, uitvoertijd en voltooiing tijden.

![Automation-accountpagina taken](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

U kunt de lijst met taken filteren door te selecteren **taken filteren** en filter op een specifiek runbook, taakstatus, of vanuit de vervolgkeuzelijst en het tijdsbereik moet worden gezocht.

![Filter de status van taak](./media/automation-runbook-execution/automation-account-jobs-filter.png)

U kunt ook overzichtsgegevens van taak voor een specifiek runbook weergeven door te selecteren dat runbook uit de **Runbooks** pagina in uw Automation-account en selecteer vervolgens de **taken** tegel. Deze actie geeft de **taken** pagina, en van daaruit kunt u klikken op de taakrecord om de details en de uitvoer weer te geven.

![Automation-accountpagina taken](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>Taakoverzicht

Hier vindt u een lijst van alle taken die zijn gemaakt voor een bepaald runbook en hun meest recente status. U kunt deze lijst op de taakstatus en het datumbereik voor de laatste wijziging aan de job filteren. Als u wilt de gedetailleerde informatie en de uitvoer bekijken, klikt u op de naam van een taak. De gedetailleerde weergave van de taak bevat de waarden voor de runbookparameters die zijn opgegeven voor die taak.

U kunt de volgende stappen uit om de taken voor een runbook weer te geven.

1. Selecteer in de Azure portal, **Automation** en selecteer vervolgens de naam van een Automation-account.
2. Selecteer in de hub, **Runbooks** en klik vervolgens op de **Runbooks** pagina selecteert u een runbook in de lijst.
3. Op de pagina voor het geselecteerde runbook, klikt u op de **taken** tegel.
4. Klik op een van de taken in de lijst en op de pagina met details van runbook-taak kunt u de details en de uitvoer bekijken.

## <a name="retrieving-job-status-using-windows-powershell"></a>Bij het ophalen van de status van taak met behulp van Windows PowerShell

U kunt de [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) om op te halen van de taken die voor een runbook en de details van een bepaalde taak is gemaakt. Als u een runbook te met Windows PowerShell starten [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook), en vervolgens wordt de resulterende taak geretourneerd. Gebruik [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) om een taak ophalen de uitvoer.

De volgende voorbeeldopdrachten de laatste taak voor een voorbeeldrunbook opgehaald en de status ervan, de waarden voor de runbookparameters en de uitvoer van de taak weergeven.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Het volgende voorbeeld wordt de uitvoer voor een specifieke taak opgehaald en elke record retourneert. In het geval is dat er een uitzondering voor een van de records, is de uitzondering geschreven in plaats van de waarde. Dit gedrag is handig als uitzonderingen krijgt u meer informatie, die niet tijdens de uitvoer normaal kan worden vastgelegd.

```azurepowershell-interactive
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>Details ophalen met het activiteitenlogboek

Andere gegevens, zoals de persoon die of het account dat het runbook startte kunnen worden opgehaald uit het activiteitenlogboek voor het automation-account. De volgende PowerShell-voorbeeld bevat de laatste gebruiker voor het uitvoeren van het desbetreffende runbook:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>Evenredige deel

Azure Automation wordt tijdelijk om resources kan delen met alle runbooks in de cloud, verwijderen of stoppen van elke taak die gedurende meer dan drie uur actief is geweest. Taken voor [op basis van een PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks) en [Python runbooks](automation-runbook-types.md#python-runbooks) worden gestopt en niet opnieuw opgestart, en de status van de taak wordt gestopt.

Voor langdurige taken wordt u aangeraden een [Hybrid Runbook Worker](automation-hrw-run-runbooks.md#job-behavior) te gebruiken. Hybrid Runbook Workers worden niet beperkt door evenredige deel en hoeft een beperking op hoe lang een runbook kunt uitvoeren. De andere taak [limieten](../azure-subscription-service-limits.md#automation-limits) gelden voor zowel Azure sandboxes als Hybrid Runbook Workers. Terwijl de Hybrid Runbook Workers worden niet beperkt door de limiet van de evenredige deel 3 uur, runbooks die worden uitgevoerd op deze moet nog steeds worden ontwikkeld ter ondersteuning van opnieuw opstarten gedrag van problemen met onverwachte lokale infrastructuur.

Een andere optie is het optimaliseren van het runbook met behulp van de onderliggende runbooks. Als uw runbook dezelfde functie op een aantal resources, zoals een databasebewerking op verschillende databases doorloopt, kunt u deze functie voor het verplaatsen een [onderliggend runbook](automation-child-runbooks.md) en roep deze aan met de [ Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet. Elk van deze onderliggende runbooks wordt parallel uitgevoerd in afzonderlijke processen, waardoor het minder lang duurt voordat het bovenliggende runbook is voltooid. U kunt de [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) cmdlet in uw runbook om te controleren van de taak de status van alle onderliggende als er bewerkingen die worden uitgevoerd moeten nadat het onderliggende runbook is voltooid.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de verschillende methoden die kunnen worden gebruikt om een runbook in Azure Automation starten, [een runbook starten in Azure Automation](automation-starting-a-runbook.md)

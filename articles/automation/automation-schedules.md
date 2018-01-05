---
title: Schema's in Azure Automation | Microsoft Docs
description: Automation-planningen worden gebruikt voor het plannen van runbooks in Azure Automation automatisch wordt gestart. Beschrijft het maken en beheren van een planning in, zodat u automatisch een runbook op een bepaald tijdstip of volgens een terugkerend schema starten kunt.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 1c2da639-ad20-4848-920b-88e471b2e1d9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 6ad70d736cd0a267ace3ade0a1ecfea38128ac72
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Een runbook in Azure Automation plannen
Als u een runbook in Azure Automation om te beginnen bij een bepaalde tijd plannen, koppelt u deze aan een of meer planningen. Geen schema kan worden geconfigureerd voor uitvoeren nadat de of op een opnieuw optreedt per uur of dagelijks schema voor runbooks in de Azure-portal. U kunt ook plannen ze voor wekelijks, maandelijks, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand.  Een runbook kan worden gekoppeld aan meerdere planningen kan, en een planning meerdere runbooks die zijn gekoppeld.

> [!NOTE]
> Schema's bieden momenteel geen ondersteuning voor Azure Automation DSC-configuraties.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets
De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van schema's met Windows PowerShell in Azure Automation. Ze worden verzonden als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview).

| Cmdlets | Beschrijving |
|:--- |:--- |
| **Azure Resource Manager-cmdlets** | |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Haalt een schema. |
| [Nieuwe AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Een nieuw schema maakt. |
| [Verwijder AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Hiermee verwijdert u een planning. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Hiermee stelt u de eigenschappen voor een bestaand schema. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Geplande runbooks opgehaald. |
| [Register AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Een runbook koppelt met een schema. |
| [Hef de registratie van AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Een runbook uit een planning dissociates. |
| **Azure Service Management-cmdlets** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |Haalt een schema. |
| [Nieuwe AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |Een nieuw schema maakt. |
| [Verwijder AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |Hiermee verwijdert u een planning. |
| [Set-AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |Hiermee stelt u de eigenschappen voor een bestaand schema. |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Geplande runbooks opgehaald. |
| [Register AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Een runbook koppelt met een schema. |
| [Hef de registratie van AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Een runbook uit een planning dissociates. |

## <a name="creating-a-schedule"></a>Een schema maken
U kunt een nieuw schema voor runbooks maken in de Azure portal of met Windows PowerShell. U hebt ook de optie voor het maken van een nieuw schema wanneer u een runbook koppelen aan een schema met de Azure classic of Azure-portal.

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.  Om te voorkomen die invloed hebben op uw runbooks en de processen die ze automatiseren, moet u eerst testen van runbooks die schema's met een speciaal ontworpen is voor het testen van Automation-account hebt gekoppeld.  Hiermee valideert u uw geplande runbooks correct blijven functioneren en als dat niet het geval is, kunt u verder oplossen en wijzigingen die zijn vereist voordat de bijgewerkte runbookversie migreren naar productie toepassen.  
>  Uw Automation-account, niet automatisch nieuwe versies van modules ophalen tenzij u ze handmatig hebt bijgewerkt door het selecteren van de [Azure-Modules WU](automation-update-azure-modules.md) optie van de **Modules** . 
>  

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Een nieuwe planning maken in de Azure portal
1. Selecteer in de Azure-portal van uw automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant. 
2. Klik op **toevoegen van een planning** boven aan de pagina.
4. Op de **nieuwe planning** deelvenster, typ een **naam** en optioneel een **beschrijving** voor het nieuwe schema.
5. Selecteer de planning wordt één keer uitgevoerd of volgens een terugkerende planning door te selecteren **eenmaal** of **terugkeerpatroon**.  Als u selecteert **eenmaal** Geef een **begintijd** en klik vervolgens op **maken**.  Als u selecteert **terugkeerpatroon**, Geef een **begintijd** en de frequentie voor hoe vaak het runbook moet worden herhaald - door **uur**, **dag**, **week**, of door **maand**.  Als u selecteert **week** of **maand** uit de vervolgkeuzelijst de **terugkeerpatroon optie** wordt weergegeven in het deelvenster en geselecteerde de **terugkeerpatroon optie** deelvenster wordt weergegeven en kunt u de dag van week selecteren als u hebt geselecteerd **week**.  Als u hebt geselecteerd **maand**, kunt u kiezen door **weekdagen** of specifieke dagen van de maand op de kalender en tot slot wilt u uitvoeren op de laatste dag van de maand of niet, en klik vervolgens op **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Een nieuw schema maken met Windows PowerShell
U kunt de [nieuw AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) cmdlet een nieuwe planning maken in Azure Automation voor klassieke runbooks of [nieuw AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet voor runbooks in de Azure-portal. De begintijd voor de planning en de frequentie die moet worden uitgevoerd, moet u opgeven.

De volgende voorbeeldopdrachten laten zien hoe maakt u een planning voor de 15e en 30e van elke maand met een cmdlet voor Azure Resource Manager.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

De volgende voorbeeldopdrachten laten zien hoe een nieuw schema dat elke dag om 3:30 PM op 20 januari 2015 beginnen met een cmdlet voor Azure Service Management wordt uitgevoerd.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Een planning koppelen aan een runbook
Een runbook kan worden gekoppeld aan meerdere planningen kan, en een planning meerdere runbooks die zijn gekoppeld. Als een runbook parameters heeft, kunt u waarden opgeven voor deze. U moet waarden opgeven voor de verplichte parameters en waarden kan opgeven voor optionele parameters.  Deze waarden worden telkens wanneer die het runbook wordt gestart door dit schema gebruikt.  U kunt hetzelfde runbook koppelen aan een ander schema en andere parameterwaarden opgeven.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een planning koppelen aan een runbook met de Azure-portal
1. Selecteer in de Azure-portal van uw automation-account **Runbooks** onder de sectie **procesautomatisering** aan de linkerkant.
2. Klik op de naam van het runbook te plannen.
3. Als het runbook is momenteel niet gekoppeld aan een schema, krijgt u de optie voor het maken van een nieuwe planning of een koppeling naar een bestaand schema.  
4. Als het runbook parameters heeft, kunt u de optie **instellingen (standaard: Azure) voor uitvoeren wijzigen** en de **Parameters** deelvenster wordt weergegeven waarin u overeenkomstig de gegevens kunt invoeren.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Een planning aan een runbook met Windows PowerShell koppelen
Kunt u de [registreren AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) een planning koppelen aan een klassieke runbook of [registreren AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet voor runbooks in de Azure-portal.  U kunt waarden voor het runbook parameters opgeven met de Parameters-parameter. Zie [een Runbook starten in Azure Automation](automation-starting-a-runbook.md) voor meer informatie over het opgeven van parameterwaarden.

De volgende voorbeeldopdrachten laten zien hoe een planning koppelen aan een runbook met een Azure Resource Manager-cmdlet met parameters.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
De volgende voorbeeldopdrachten laten zien hoe een planning met een Azure-servicebeheer-cmdlet parameters koppelen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Het uitschakelen van een planning
Wanneer u een planning uitschakelt, wordt alle runbooks die zijn gekoppeld aan deze niet langer uitgevoerd op die planning. U kunt handmatig taakplanning uitschakelen of stel een verlooptijd voor schema's met een frequentie wanneer u deze maakt. Wanneer de verlooptijd is bereikt, wordt het schema uitgeschakeld.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Uitschakelen van een planning van de Azure-portal
1. Selecteer in de Azure-portal van uw Automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
2. Klik op de naam van een planning te openen van het detaildeelvenster.
3. Wijziging **ingeschakeld** naar **Nee**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Een planning met Windows PowerShell uitschakelen
U kunt de [Set AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet om de eigenschappen van een bestaande planning voor een klassieke runbook wijzigen of [Set AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet voor runbooks in de Azure-portal. Schakel de planning opgeven **false** voor de **IsEnabled** parameter.

De volgende voorbeeldopdrachten laten zien hoe een runbook met een cmdlet voor Azure Resource Manager-taakplanning uitschakelen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

De volgende voorbeeldopdrachten laten zien hoe een schema met de cmdlet Azure Service Management uitschakelen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Volgende stappen
* Om te beginnen met runbooks in Azure Automation, Zie [een Runbook starten in Azure Automation](automation-starting-a-runbook.md) 


---
title: Schema's in Azure Automation
description: Automation-planningen worden gebruikt voor het plannen van runbooks in Azure Automation automatisch wordt gestart. Beschrijft het maken en beheren van een planning in, zodat u automatisch een runbook op een bepaald tijdstip of volgens een terugkerend schema starten kunt.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6c7bd4d4249d304ee7c1df4ae4b8fc0af476b99c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34192068"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Een runbook in Azure Automation plannen

Als u een runbook in Azure Automation om te beginnen bij een bepaalde tijd plannen, koppelt u deze aan een of meer planningen. Geen schema kan worden geconfigureerd voor uitvoeren nadat de of op een opnieuw optreedt per uur of dagelijks schema voor runbooks in de Azure-portal. U kunt ook plannen ze voor wekelijks, maandelijks, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand. Een runbook kan worden gekoppeld aan meerdere planningen kan, en een planning meerdere runbooks die zijn gekoppeld.

> [!NOTE]
> Schema's bieden momenteel geen ondersteuning voor Azure Automation DSC-configuraties.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van schema's met Windows PowerShell in Azure Automation. Ze worden verzonden als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview).

| Cmdlets | Beschrijving |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Haalt een schema. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Een nieuw schema maakt. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Hiermee verwijdert u een planning. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Hiermee stelt u de eigenschappen voor een bestaand schema. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Geplande runbooks opgehaald. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Een runbook koppelt met een schema. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Een runbook uit een planning dissociates. |

## <a name="creating-a-schedule"></a>Een schema maken

U kunt een nieuw schema voor runbooks maken in de Azure portal of met Windows PowerShell.

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.  Om te voorkomen die invloed hebben op uw runbooks en de processen die ze automatiseren, moet u eerst testen van runbooks die schema's met een speciaal ontworpen is voor het testen van Automation-account hebt gekoppeld.  Hiermee valideert u uw geplande runbooks correct blijven functioneren en als dat niet het geval is, kunt u verder oplossen en wijzigingen die zijn vereist voordat de bijgewerkte runbookversie migreren naar productie toepassen.
> Uw Automation-account, niet automatisch nieuwe versies van modules ophalen tenzij u ze handmatig hebt bijgewerkt door het selecteren van de [Azure-Modules WU](automation-update-azure-modules.md) optie van de **Modules**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Een nieuwe planning maken in de Azure portal

1. Selecteer in de Azure-portal van uw automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
1. Klik op **toevoegen van een planning** boven aan de pagina.
1. Op de **nieuwe planning** deelvenster, typ een **naam** en optioneel een **beschrijving** voor het nieuwe schema.
1. Selecteer de planning wordt één keer uitgevoerd of volgens een terugkerende planning door te selecteren **eenmaal** of **terugkeerpatroon**. Als u selecteert **eenmaal** Geef een **begintijd**, en klik vervolgens op **maken**. Als u selecteert **terugkeerpatroon**, Geef een **begintijd** en de frequentie voor hoe vaak het runbook moet worden herhaald - door **uur**, **dag**, **week**, of door **maand**. Als u selecteert **week** of **maand** uit de vervolgkeuzelijst de **terugkeerpatroon optie** wordt weergegeven in het deelvenster en geselecteerde de **terugkeerpatroon optie** deelvenster wordt weergegeven en kunt u de dag van week selecteren als u hebt geselecteerd **week**. Als u hebt geselecteerd **maand**, kunt u kiezen door **weekdagen** of specifieke dagen van de maand op de kalender en tot slot wilt u uitvoeren op de laatste dag van de maand of niet, en klik vervolgens op **OK**.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Een nieuw schema maken met Windows PowerShell

U gebruikt de [nieuw AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet maken van planningen. U de begintijd voor de planning en de frequentie die moet worden uitgevoerd.

De volgende voorbeeldopdrachten laten zien hoe maakt u een planning voor de 15e en 30e van elke maand met een cmdlet voor Azure Resource Manager.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
$scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
-DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
```

## <a name="linking-a-schedule-to-a-runbook"></a>Een planning koppelen aan een runbook

Een runbook kan worden gekoppeld aan meerdere planningen kan, en een planning meerdere runbooks die zijn gekoppeld. Als een runbook parameters heeft, kunt u waarden opgeven voor deze. U moet waarden opgeven voor de verplichte parameters en waarden kan opgeven voor optionele parameters. Deze waarden worden telkens wanneer die het runbook wordt gestart door dit schema gebruikt. U kunt hetzelfde runbook koppelen aan een ander schema en andere parameterwaarden opgeven.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een planning koppelen aan een runbook met de Azure-portal

1. Selecteer in de Azure-portal van uw automation-account **Runbooks** onder de sectie **procesautomatisering** aan de linkerkant.
1. Klik op de naam van het runbook te plannen.
1. Als het runbook is momenteel niet gekoppeld aan een schema, krijgt u de optie voor het maken van een nieuwe planning of een koppeling naar een bestaand schema.
1. Als het runbook parameters heeft, kunt u de optie **instellingen (standaard: Azure) voor uitvoeren wijzigen** en de **Parameters** deelvenster wordt weergegeven waarin u overeenkomstig de gegevens kunt invoeren.

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Een planning aan een runbook met Windows PowerShell koppelen

U kunt de [registreren AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet om een planning koppelen. U kunt waarden voor het runbook parameters opgeven met de Parameters-parameter. Zie voor meer informatie over het opgeven van parameterwaarden [een Runbook starten in Azure Automation](automation-starting-a-runbook.md).
De volgende voorbeeldopdrachten laten zien hoe een planning koppelen aan een runbook met een Azure Resource Manager-cmdlet met parameters.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Runbooks vaker plannen

De meest voorkomende interval voor die een schema in Azure Automation kan worden geconfigureerd voor is één uur. Als u schema's uit te voeren vaker dan dat vereist, zijn er twee opties:

* Maak een [webhook](automation-webhooks.md) voor het runbook en gebruik [Azure Scheduler](../scheduler/scheduler-get-started-portal.md) de webhook niet aanroepen. Azure Scheduler biedt meer gedetailleerd granulatie bij het definiëren van een planning.

* Vier schema's kunt maken die alle binnen 15 minuten van elkaar met één keer per uur wordt gestart. In dit scenario kunt het runbook uit te voeren om de 15 minuten met de schema's.

## <a name="disabling-a-schedule"></a>Het uitschakelen van een planning

Wanneer u een planning uitschakelt, wordt een willekeurig runbook dat is gekoppeld aan deze niet langer uitgevoerd op die planning. U kunt handmatig taakplanning uitschakelen of stel een verlooptijd voor schema's met een frequentie wanneer u deze maakt. Wanneer de verlooptijd is bereikt, wordt het schema uitgeschakeld.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Uitschakelen van een planning van de Azure-portal

1. Selecteer in de Azure-portal van uw Automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
1. Klik op de naam van een planning te openen van het detaildeelvenster.
1. Wijziging **ingeschakeld** naar **Nee**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Een planning met Windows PowerShell uitschakelen

U kunt de [Set AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet om de eigenschappen van een bestaand schema wijzigen. Schakel de planning opgeven **false** voor de **IsEnabled** parameter.

De volgende voorbeeldopdrachten laten zien hoe een runbook met een cmdlet voor Azure Resource Manager-taakplanning uitschakelen.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Volgende stappen

* Om te beginnen met runbooks in Azure Automation, Zie [een Runbook starten in Azure Automation](automation-starting-a-runbook.md)
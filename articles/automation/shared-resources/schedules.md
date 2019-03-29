---
title: Schema's in Azure Automation
description: Automation-planningen worden gebruikt voor het plannen van runbooks in Azure Automation om automatisch te starten. Beschrijft hoe u maken en beheren van een planning in, zodat u automatisch een runbook op een bepaald tijdstip of volgens een terugkerend schema starten kunt.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/22/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d99c6b419ee201be50e74849cd95a332845f5b73
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58623166"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Een runbook in Azure Automation plannen

Als u een runbook in Azure Automation om te beginnen bij een bepaalde tijd plannen, koppelt u deze aan een of meer planningen. Een schema kan worden geconfigureerd voor uitvoeren op een compressieversies of één keer per uur of dagelijks schema voor runbooks in Azure portal. U kunt ook plannen ze voor wekelijks, maandelijks, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand. Een runbook kan worden gekoppeld aan meerdere planningen en er kunnen meerdere runbooks aan een planning worden gekoppeld.

> [!NOTE]
> Schema's bieden momenteel geen ondersteuning voor Azure Automation DSC-configuraties.

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van schema's met PowerShell in Azure Automation. Ze geleverd als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview).

| Cmdlets | Description |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Hiermee haalt u een planning. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Hiermee maakt u een nieuw schema. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Hiermee verwijdert u een planning. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Hiermee stelt u de eigenschappen voor een bestaand schema. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Geplande runbooks worden opgehaald. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Een runbook koppelt aan een schema. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Een runbook uit een schema dissociates. |

## <a name="creating-a-schedule"></a>Het maken van een planning

U kunt een nieuw schema voor runbooks maken in Azure portal of met PowerShell.

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.  Om te voorkomen dat dit gevolgen heeft voor uw runbooks en de processen die ze automatiseren, moet u eerst alle runbooks met planningen met een Automation-account dat is bestemd gekoppelde voor het testen van testen.  Hiermee valideert u uw geplande runbooks correct blijven werken en als dat niet het geval is, kunt u verder oplossen en wijzigingen die vereist is voordat u de bijgewerkte runbookversie migreert naar productie.
> Uw Automation-account, niet automatisch nieuwe versies van modules ophalen, tenzij u ze handmatig hebt bijgewerkt door het selecteren van de [Update Azure-Modules](../automation-update-azure-modules.md) optie uit de **Modules**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Een nieuw schema maken in Azure portal

1. Selecteer in de Azure portal, in uw automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
2. Klik op **toevoegen van een schema** aan de bovenkant van de pagina.
3. Op de **nieuwe planning** deelvenster, typ een **naam** en eventueel een **beschrijving** voor het nieuwe schema.
4. Selecteer of het schema wordt één keer uitgevoerd of volgens een terugkerende planning door te schakelen **eenmaal** of **periodiek**. Als u selecteert **eenmaal** geven een **begintijd**, en klik vervolgens op **maken**. Als u selecteert **periodiek**, Geef een **begintijd** en voor **herhalen elke**, selecteert u de frequentie voor hoe vaak u wilt dat het runbook moet worden herhaald - door **uur**, **dag**, **week**, of door **maand**.
    1. Als u selecteert **week**, krijgt u een lijst van de dagen van de week om uit te kiezen. Aantal dagen als u wilt selecteren. De eerste uitvoering van het schema gebeurt op de eerste dag na de starttijd hebt geselecteerd. Bijvoorbeeld, als u een planning weekend, kiest u **zaterdag** en **zondag**.

       ![Instelling weekend terugkerende planning](../media/schedules/week-end-weekly-recurrence.png)

    2. Als u selecteert **maand**, krijgt u verschillende opties. Voor de **voorvallen in de maand** optie, selecteert u **maanddagen** of **weekdagen**. Als u ervoor kiest **maanddagen**, een agenda wordt weergegeven waarmee u zoveel dagen als u wilt kiezen. Als u ervoor een datum, bijvoorbeeld de 31e die niet wordt uitgevoerd in de huidige maand kiest, wordt het schema niet uitgevoerd. Als u wilt dat het schema uit te voeren op de laatste dag, kiest u **Ja** onder **worden uitgevoerd op de laatste dag van maand**. Als u ervoor kiest **weekdagen**, wordt de **herhalen elke** optie wordt weergegeven. Kies **eerste**, **tweede**, **derde**, **vierde**, of **laatste**. Kies ten slotte een dag moet worden herhaald op.

       ![Planning per maand op de eerste, de vijftiende en de laatste dag van de maand](../media/schedules/monthly-first-fifteenth-last.png)

5. Klik wanneer klaar **maken**.

### <a name="to-create-a-new-schedule-with-powershell"></a>Een nieuw schema maken met PowerShell

U gebruikt de [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet voor het maken van planningen. U de begintijd voor de planning en de frequentie die moet worden uitgevoerd. De volgende voorbeelden laten zien hoe veel verschillende schema's maken.

#### <a name="create-a-one-time-schedule"></a>Maak een tijdschema

De volgende voorbeeldopdrachten maakt u een schema.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Maken van een terugkerend schema

De volgende voorbeeldopdrachten laten zien over het maken van een terugkerend schema die elke dag om 1:00 uur voor een jaar wordt uitgevoerd.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Maakt u een wekelijks terugkerende planning

De volgende voorbeeldopdrachten laten zien hoe een wekelijkse planning die wordt uitgevoerd op weekdagen alleen maken.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Maakt u een wekelijks terugkerende planning voor weekends

De volgende voorbeeldopdrachten laten zien hoe een wekelijkse planning die wordt uitgevoerd in het weekend alleen maken.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-first-15th-and-last-days-of-the-month"></a>Maken van een terugkerend schema voor de eerste, 15, en de laatste dag van de maand

De volgende voorbeeldopdrachten laten zien over het maken van een terugkerend schema wordt uitgevoerd op de 1, 15 en laatste dag van een maand.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzureRmAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="linking-a-schedule-to-a-runbook"></a>Een planning koppelen aan een runbook

Een runbook kan worden gekoppeld aan meerdere planningen en er kunnen meerdere runbooks aan een planning worden gekoppeld. Als een runbook parameters heeft, kunt u waarden opgeven voor deze. U moet waarden opgeven voor de verplichte parameters en waarden voor eventuele optionele parameters kan opgeven. Deze waarden worden telkens wanneer die het runbook wordt gestart door dit schema gebruikt. U kunt hetzelfde runbook koppelen aan een ander schema en andere parameterwaarden opgeven.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een planning koppelen aan een runbook met de Azure-portal

1. Selecteer in de Azure portal, in uw automation-account **Runbooks** onder de sectie **procesautomatisering** aan de linkerkant.
2. Klik op de naam van het runbook om te plannen.
3. Als het runbook niet is momenteel gekoppeld aan een schema, bent u de optie voor het maken van een nieuw schema of een koppeling naar een bestaande planning schema aangeboden.
4. Als het runbook parameters heeft, kunt u de optie **instellingen (standaard: Azure) voor uitvoeren wijzigen** en de **Parameters** deelvenster wordt weergegeven waarin u de gegevens kunt invoeren.

### <a name="to-link-a-schedule-to-a-runbook-with-powershell"></a>Een planning koppelen aan een runbook met PowerShell

U kunt de [registreren AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet om een schema te koppelen. U kunt waarden voor parameters van het runbook opgeven met de parameter Parameters. Zie voor meer informatie over het opgeven van parameterwaarden [Starting a Runbook in Azure Automation](../automation-starting-a-runbook.md).
De volgende voorbeeldopdrachten laten zien hoe een planning koppelen aan een runbook met behulp van een Azure Resource Manager-cmdlet met parameters.

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

De meest voorkomende interval voor die een schema in Azure Automation kan worden geconfigureerd voor is één uur. Als u schema's uit te voeren vaker dan vereist, zijn er twee opties:

* Maak een [webhook](../automation-webhooks.md) voor het runbook en gebruik [Azure Scheduler](../../scheduler/scheduler-get-started-portal.md) voor het aanroepen van de webhook. Azure Scheduler biedt meer fijnmazige granulariteit bij het definiëren van een schema.

* Maken van vier planningen alle starten binnen 15 minuten van elkaar met één keer per uur. In dit scenario kunt het runbook om uit te voeren om de 15 minuten met de verschillende schema's.

## <a name="disabling-a-schedule"></a>Een schema uit te schakelen

Wanneer u een planning uitschakelt, wordt een willekeurig niet meer gekoppeld aan het runbook wordt uitgevoerd op die planning. U kunt handmatig een planning uitschakelen of een verlooptijd voor schema's met de frequency wordt ingesteld wanneer u deze maakt. Wanneer de vervaltijd is bereikt, wordt het schema uitgeschakeld.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Om uit te schakelen van een schema van de Azure-portal

1. Selecteer in de Azure portal, in uw Automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
2. Klik op de naam van een schema te openen van het deelvenster met details.
3. Wijziging **ingeschakeld** naar **Nee**.

### <a name="to-disable-a-schedule-with-powershell"></a>Om uit te schakelen van een schema met PowerShell

U kunt de [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet voor het wijzigen van de eigenschappen van een bestaand schema. Schakel de planning opgeven **false** voor de **IsEnabled** parameter.

De volgende voorbeeldopdrachten laten zien hoe een planning voor een runbook met behulp van een Azure Resource Manager-cmdlet uitschakelen.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Volgende stappen

* Om te beginnen met runbooks in Azure Automation, Zie [Starting a Runbook in Azure Automation](../automation-starting-a-runbook.md)


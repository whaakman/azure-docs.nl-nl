---
title: Schema's in Azure Automation
description: Automation-planningen worden gebruikt voor het plannen van runbooks in Azure Automation om automatisch te starten. Beschrijft hoe u maken en beheren van een planning in, zodat u automatisch een runbook op een bepaald tijdstip of volgens een terugkerend schema starten kunt.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3d8492d2a8982c9c85bfc91867f7eb6c2da04e58
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294761"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Een runbook in Azure Automation plannen

Als u een runbook in Azure Automation om te beginnen bij een bepaalde tijd plannen, koppelt u deze aan een of meer planningen. Een schema kan worden geconfigureerd voor uitvoeren op een compressieversies of één keer per uur of dagelijks schema voor runbooks in Azure portal. U kunt ook plannen ze voor wekelijks, maandelijks, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand. Een runbook kan worden gekoppeld aan meerdere planningen kan, en een schema meerdere runbooks aan.

> [!NOTE]
> Schema's bieden momenteel geen ondersteuning voor Azure Automation DSC-configuraties.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van schema's met Windows PowerShell in Azure Automation. Ze geleverd als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview).

| Cmdlets | Beschrijving |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Hiermee haalt u een planning. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Hiermee maakt u een nieuw schema. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Hiermee verwijdert u een planning. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Hiermee stelt u de eigenschappen voor een bestaand schema. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Geplande runbooks worden opgehaald. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Een runbook koppelt aan een schema. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Een runbook uit een schema dissociates. |

## <a name="creating-a-schedule"></a>Het maken van een planning

U kunt een nieuw schema voor runbooks maken in Azure portal of met Windows PowerShell.

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.  Om te voorkomen dat dit gevolgen heeft voor uw runbooks en de processen die ze automatiseren, moet u eerst alle runbooks met planningen met een Automation-account dat is bestemd gekoppelde voor het testen van testen.  Hiermee valideert u uw geplande runbooks correct blijven werken en als dat niet het geval is, kunt u verder oplossen en wijzigingen die vereist is voordat u de bijgewerkte runbookversie migreert naar productie.
> Uw Automation-account, niet automatisch nieuwe versies van modules ophalen, tenzij u ze handmatig hebt bijgewerkt door het selecteren van de [Update Azure-Modules](automation-update-azure-modules.md) optie uit de **Modules**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Een nieuw schema maken in Azure portal

1. Selecteer in de Azure portal, in uw automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
1. Klik op **toevoegen van een schema** aan de bovenkant van de pagina.
1. Op de **nieuwe planning** deelvenster, typ een **naam** en eventueel een **beschrijving** voor het nieuwe schema.
1. Selecteer of het schema wordt één keer uitgevoerd of volgens een terugkerende planning door te schakelen **eenmaal** of **periodiek**. Als u selecteert **eenmaal** geven een **begintijd**, en klik vervolgens op **maken**. Als u selecteert **periodiek**, Geef een **begintijd** en voor **herhalen elke**, selecteert u de frequentie voor hoe vaak u wilt dat het runbook moet worden herhaald - door **uur**, **dag**, **week**, of door **maand**.
    1. Als u selecteert **week**, vindt u een lijst van de dagen van de week om uit te kiezen. Aantal dagen als u wilt selecteren. De eerste uitvoering van het schema gebeurt op de eerste dag na de starttijd hebt geselecteerd.
    2. Als u selecteert **maand**, krijgt u verschillende opties. Voor de **maandelijkse als** optie, selecteert u **maanddagen** of **weekdagen**. Als u ervoor kiest **maanddagen** een agenda wordt weergegeven waarmee u zoveel dagen als u wilt kiezen. Als u ervoor een datum, bijvoorbeeld de 31e die niet wordt uitgevoerd in de huidige maand kiest, wordt de planning wordt niet uitgevoerd. Als u wilt dat het schema uit te voeren op de laatste dag, kiest u **Ja** onder **worden uitgevoerd op de laatste dag van maand**. Als u ervoor kiest **weekdagen**, wordt de **herhalen elke** optie wordt weergegeven. Kies **eerste**, **tweede**, **derde**, **vierde**, of **laatste**. Kies ten slotte een dag moet worden herhaald op.
1. Klik wanneer klaar **maken**.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Een nieuw schema maken met Windows PowerShell

U gebruikt de [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet voor het maken van planningen. U de begintijd voor de planning en de frequentie die moet worden uitgevoerd.

De volgende voorbeeldopdrachten laten zien hoe maakt u een planning voor de 15e en 30 van elke maand met een Azure Resource Manager-cmdlet.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
$scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
-DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
```

## <a name="linking-a-schedule-to-a-runbook"></a>Een planning koppelen aan een runbook

Een runbook kan worden gekoppeld aan meerdere planningen kan, en een schema meerdere runbooks aan. Als een runbook parameters heeft, kunt u waarden opgeven voor deze. U moet waarden opgeven voor de verplichte parameters en waarden voor eventuele optionele parameters kan opgeven. Deze waarden worden telkens wanneer die het runbook wordt gestart door dit schema gebruikt. U kunt hetzelfde runbook koppelen aan een ander schema en andere parameterwaarden opgeven.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een planning koppelen aan een runbook met de Azure-portal

1. Selecteer in de Azure portal, in uw automation-account **Runbooks** onder de sectie **procesautomatisering** aan de linkerkant.
2. Klik op de naam van het runbook om te plannen.
3. Als het runbook is momenteel niet gekoppeld aan een schema, krijgt u de optie voor het maken van een nieuw schema of een koppeling naar een bestaande planning schema.
4. Als het runbook parameters heeft, kunt u de optie **instellingen (standaard: Azure) voor uitvoeren wijzigen** en de **Parameters** deelvenster wordt weergegeven waarin u overeenkomstig de gegevens kunt invoeren.

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Een planning koppelen aan een runbook met Windows PowerShell

U kunt de [registreren AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet om een schema te koppelen. U kunt waarden voor parameters van het runbook opgeven met de parameter Parameters. Zie voor meer informatie over het opgeven van parameterwaarden [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).
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

* Maak een [webhook](automation-webhooks.md) voor het runbook en gebruik [Azure Scheduler](../scheduler/scheduler-get-started-portal.md) voor het aanroepen van de webhook. Azure Scheduler biedt meer fijnmazige granulariteit bij het definiëren van een schema.

* Maken van vier planningen alle starten binnen 15 minuten van elkaar met één keer per uur. In dit scenario kunt het runbook om uit te voeren om de 15 minuten met de verschillende schema's.

## <a name="disabling-a-schedule"></a>Een schema uit te schakelen

Wanneer u een planning uitschakelt, wordt een willekeurig niet meer gekoppeld aan het runbook wordt uitgevoerd op die planning. U kunt handmatig een planning uitschakelen of een verlooptijd voor schema's met de frequency wordt ingesteld wanneer u deze maakt. Wanneer de vervaltijd is bereikt, wordt het schema uitgeschakeld.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Om uit te schakelen van een schema van de Azure-portal

1. Selecteer in de Azure portal, in uw Automation-account **planningen** onder de sectie **gedeelde bronnen** aan de linkerkant.
1. Klik op de naam van een schema te openen van het deelvenster met details.
1. Wijziging **ingeschakeld** naar **Nee**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Om uit te schakelen van een planning met Windows PowerShell

U kunt de [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet voor het wijzigen van de eigenschappen van een bestaand schema. Schakel de planning opgeven **false** voor de **IsEnabled** parameter.

De volgende voorbeeldopdrachten laten zien hoe een planning voor een runbook met behulp van een Azure Resource Manager-cmdlet uitschakelen.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Volgende stappen

* Om te beginnen met runbooks in Azure Automation, Zie [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)

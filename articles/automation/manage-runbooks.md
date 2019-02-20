---
title: Runbooks in Azure Automation beheren
description: In dit artikel wordt beschreven hoe u runbooks in Azure Automation beheren.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: da3b09998d163ffcc16bfcbbf9f516467dd3311d
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418583"
---
# <a name="manage-runbooks-in-azure-automation"></a>Runbooks in Azure Automation beheren

U kunt een runbook toevoegen aan Azure Automation door [het maken van een nieuwe](#creating-a-new-runbook) of een bestaand runbook importeren uit een bestand of de [Runbookgalerie](automation-runbook-gallery.md). In dit artikel bevat informatie over het maken en runbooks importeren uit een bestand.  Krijgt u alle details over de toegang tot community runbooks en modules in [Runbook- en galerieën voor Azure Automation](automation-runbook-gallery.md).

## <a name="create-a-runbook"></a>Een runbook maken

U kunt een nieuw runbook maken in Azure Automation met behulp van een van de Azure Portal of de Windows PowerShell. Zodra het runbook is gemaakt, kunt u bewerken met behulp van informatie in [PowerShell Workflow leren kennen](automation-powershell-workflow.md) en [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).

### <a name="create-a-runbook-in-the-azure-portal"></a>Een runbook maken in Azure portal

1. Open uw Automation-account in Azure Portal.
2. Selecteer in de Hub, **Runbooks** om de lijst van runbooks te openen.
3. Klik op de **toevoegen van een runbook** knop en vervolgens **een nieuw runbook maken**.
4. Typ een **naam** voor het runbook en selecteer de [Type](automation-runbook-types.md). De runbooknaam moet beginnen met een letter en kan hebben letters, cijfers, onderstrepingstekens en streepjes bevatten.
5. Klik op **maken** te maken van het runbook en de editor te openen.

### <a name="create-a-runbook-with-powershell"></a>Een runbook maken met PowerShell

U kunt de [New-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/new-azurermautomationrunbook) cmdlet voor het maken van een lege [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks). Gebruik de **Type** parameter opgeven voor een van de vier runbooktypen.

De volgende voorbeeldopdrachten laten zien hoe u een nieuw, leeg runbook maken.

```azurepowershell-interactive
New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>Een runbook importeren

U kunt een nieuw runbook in Azure Automation maken door het importeren van een PowerShell-script of PowerShell-werkstroom (.ps1-extensie), een geëxporteerde een grafisch runbook (.graphrunbook) of een script Python 2 (met de extensie .py).  Moet u de [type runbook](automation-runbook-types.md) die is gemaakt tijdens het importeren, rekening houdend met de volgende overwegingen.

* Een `.graphrunbook` bestand kan alleen worden geïmporteerd in een nieuwe [grafisch runbook](automation-runbook-types.md#graphical-runbooks), en grafische runbooks kunnen alleen worden gemaakt op basis van een `.graphrunbook` bestand.
* Een `.ps1` -bestand met een PowerShell-werkstroom kan alleen worden geïmporteerd in een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als het bestand meerdere PowerShell-werkstromen bevat, klikt u vervolgens mislukt het importeren. U moet elke werkstroom opslaan in een eigen bestand en importeer elk afzonderlijk.
* Een `.ps1` -bestand dat een werkstroom bevat niet kan worden geïmporteerd in ofwel een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als deze wordt geïmporteerd in een PowerShell Workflow-runbook, klikt u vervolgens het wordt geconverteerd naar een werkstroom en opmerkingen zijn opgenomen in het runbook op te geven de wijzigingen die zijn aangebracht.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Een runbook importeren uit een bestand met de Azure-portal

De volgende procedure kunt u een scriptbestand importeren in Azure Automation.  

> [!NOTE]
> Houd er rekening mee dat u alleen een ps1-bestand kunt importeren in een PowerShell Workflow-runbook met behulp van de portal.

1. Open uw Automation-account in Azure Portal.
2. Selecteer in de Hub, **Runbooks** om de lijst van runbooks te openen.
3. Klik op de **toevoegen van een runbook** knop en vervolgens **importeren**.
4. Klik op **Runbook-bestand** om te selecteren van het bestand te importeren
5. Als de **naam** veld is ingeschakeld, wordt u hebt de mogelijkheid om deze te wijzigen.  De runbooknaam moet beginnen met een letter en kan hebben letters, cijfers, onderstrepingstekens en streepjes bevatten.
6. De [runbooktype](automation-runbook-types.md) wordt automatisch geselecteerd, maar u kunt het type wijzigen nadat de beperkingen van toepassing in aanmerking nemen. 
7. Het nieuwe runbook wordt weergegeven in de lijst met runbooks voor het Automation-Account.
8. U moet [het runbook publiceren](#publishing-a-runbook) voordat u deze kunt uitvoeren.

> [!NOTE]
> Nadat u een grafisch runbook of een grafische PowerShell workflow-runbook importeren, hebt u de optie moet worden geconverteerd naar het andere type als gewenst. U converteren niet naar een tekstuele runbook.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Een runbook importeren uit een scriptbestand met Windows PowerShell

U kunt de [Import-AzureRMAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/import-azurermautomationrunbook) cmdlet voor het importeren van een scriptbestand opgeven als een concept PowerShell Workflow-runbook. Als het runbook al bestaat, het importeren mislukt, tenzij u de *-Force* parameter.

De volgende voorbeeldopdrachten laten zien hoe een scriptbestand importeren in een runbook.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="test-a-runbook"></a>Een runbook testen

Wanneer u een runbook test de [conceptversie](#publishing-a-runbook) wordt uitgevoerd en de acties die worden uitgevoerd zijn voltooid. Er wordt geen Taakgeschiedenis wordt gemaakt, maar de [uitvoer](automation-runbook-output-and-messages.md#output-stream) en [waarschuwings- en](automation-runbook-output-and-messages.md#message-streams) stromen worden weergegeven in de Test deelvenster Uitvoer. Berichten naar de [uitgebreide Stream](automation-runbook-output-and-messages.md#message-streams) worden weergegeven in het deelvenster Uitvoer alleen als de [variabele $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) is ingesteld om verder te gaan.

Hoewel de conceptversie wordt uitgevoerd, wordt het runbook nog steeds normaal uitgevoerd en worden eventuele acties op basis van resources in de omgeving wordt uitgevoerd. Daarom moet u alleen runbooks op niet-productiebronnen testen.

De procedure voor het testen van elke [type runbook](automation-runbook-types.md) is hetzelfde, en er geen verschil in testen tussen de teksteditor en de grafische editor in Azure portal het.  

1. De conceptversie van het runbook te openen in een de [teksteditor](automation-edit-textual-runbook.md) of [grafische editor](automation-graphical-authoring-intro.md).
1. Klik op de **Test** knop om de Test-pagina te openen.
1. Als het runbook parameters heeft, wordt deze weergegeven in het linkerdeelvenster waarin u de waarden die moeten worden gebruikt voor de test kunt opgeven.
1. Als u de test uitvoeren wilt op een [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), wijzig de **instellingen uitvoeren** naar **Hybrid Worker** en selecteer de naam van de doelgroep.  Anders de standaardwaarde **Azure** om uit te voeren van de test in de cloud.
1. Klik op de **Start** knop om de test te starten.
1. Als het runbook is [PowerShell-werkstroom](automation-runbook-types.md#powershell-workflow-runbooks) of [grafisch](automation-runbook-types.md#graphical-runbooks), kunt u stoppen of deze onderbreken terwijl deze wordt getest met de knoppen onder het deelvenster Uitvoer. Wanneer u het runbook onderbreekt, wordt de huidige activiteit voltooid voordat het wordt onderbroken. Zodra het runbook wordt onderbroken, kunt u voorkomen dat deze of het programma opnieuw.
1. Inspecteer de uitvoer van het runbook in het deelvenster Uitvoer.

## <a name="publish-a-runbook"></a>Een runbook publiceren

Wanneer u maakt of een nieuw runbook importeren, moet u het publiceren voordat u deze kunt uitvoeren.  Elk runbook in Automation heeft een ontwerpversie en een gepubliceerde versie. Alleen de gepubliceerde versie is beschikbaar om te worden uitgevoerd, en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Wanneer de conceptversie beschikbaar moet worden gesteld, publiceert u deze, die wordt de gepubliceerde versie door de conceptversie overschreven.

### <a name="azure-portal"></a>Azure Portal

1. Open het runbook in Azure portal.
2. Klik op de **bewerken** knop.
3. Klik op de **publiceren** knop en vervolgens **Ja** in het bevestigingsbericht.

### <a name="powershell"></a>PowerShell

U kunt de [publiceren-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/publish-azurermautomationrunbook) cmdlet om een runbook met Windows PowerShell te publiceren. De volgende voorbeeldopdrachten laten zien hoe u een voorbeeldrunbook publiceert.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over hoe u van het Runbook en de galerie met PowerShell-Module profiteren kunt, [Runbook- en galerieën voor Azure Automation](automation-runbook-gallery.md)
* Zie voor meer informatie over het bewerken van PowerShell en PowerShell Workflow-runbooks met een teksteditor, [tekstrunbooks in Azure Automation bewerken](automation-edit-textual-runbook.md)
* Zie voor meer informatie over het ontwerpen van een grafisch runbook, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)

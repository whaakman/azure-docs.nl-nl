---
title: Een runbook in Azure Automation maken of importeren
description: In dit artikel wordt beschreven hoe u een nieuw runbook in Azure Automation maken of importeren uit een bestand.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2ccf9036d3701c710c6d3258f81390ed355c246c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578028"
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Een runbook in Azure Automation maken of importeren

U kunt een runbook toevoegen aan Azure Automation door [het maken van een nieuwe](#creating-a-new-runbook) of een bestaand runbook importeren uit een bestand of van de [Runbookgalerie](automation-runbook-gallery.md). In dit artikel bevat informatie over het maken en runbooks importeren uit een bestand.  Krijgt u alle details over de toegang tot community runbooks en modules in [Runbook- en galerieën voor Azure Automation](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Een nieuw runbook maken

U kunt een nieuw runbook maken in Azure Automation met behulp van een van de Azure Portal of de Windows PowerShell. Zodra het runbook is gemaakt, kunt u bewerken met behulp van informatie in [PowerShell Workflow leren kennen](automation-powershell-workflow.md) en [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Een nieuw Azure Automation-runbook maken met de Azure-portal

1. Open uw Automation-account in Azure Portal.
1. Selecteer in de Hub, **Runbooks** om de lijst van runbooks te openen.
1. Klik op de **toevoegen van een runbook** knop en vervolgens **een nieuw runbook maken**.
1. Typ een **naam** voor het runbook en selecteer de [Type](automation-runbook-types.md). De runbooknaam moet beginnen met een letter en kan hebben letters, cijfers, onderstrepingstekens en streepjes bevatten.
1. Klik op **maken** te maken van het runbook en de editor te openen.

### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Een nieuw Azure Automation-runbook maken met Windows PowerShell
U kunt de [New-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationrunbook) cmdlet voor het maken van een lege [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks). De **Type** parameter moet ook worden opgenomen om op te geven van een van de vier runbooktypen.

De volgende voorbeeldopdrachten laten zien hoe u een nieuw, leeg runbook maken.

```azurepowershell-interactive
New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Een runbook importeren uit een bestand in Azure Automation

U kunt een nieuw runbook in Azure Automation maken door het importeren van een PowerShell-script of PowerShell-werkstroom (.ps1-extensie), een geëxporteerde een grafisch runbook (.graphrunbook) of een script Python 2 (met de extensie .py).  Moet u de [type runbook](automation-runbook-types.md) die is gemaakt tijdens het importeren, rekening houdend met de volgende overwegingen.

* Een bestand .graphrunbook kan alleen worden geïmporteerd in een nieuwe [grafisch runbook](automation-runbook-types.md#graphical-runbooks), en grafische runbooks kunnen alleen worden gemaakt vanuit een bestand .graphrunbook.
* Een ps1-bestand met een PowerShell-werkstroom kan alleen worden geïmporteerd in een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als het bestand meerdere PowerShell-werkstromen bevat, klikt u vervolgens mislukt het importeren. U moet elke werkstroom opslaan in een eigen bestand en importeer elk afzonderlijk.
* Een .ps1-bestand een werkstroom bevat kan worden geïmporteerd in ofwel een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als deze wordt geïmporteerd in een PowerShell Workflow-runbook, klikt u vervolgens het wordt geconverteerd naar een werkstroom en opmerkingen zijn opgenomen in het runbook op te geven de wijzigingen die zijn aangebracht.

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
>  
> 

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

## <a name="publishing-a-runbook"></a>Een runbook publiceren

Wanneer u maakt of een nieuw runbook importeren, moet u het publiceren voordat u deze kunt uitvoeren.  Elk runbook in Automation heeft een ontwerpversie en een gepubliceerde versie. Alleen de gepubliceerde versie is beschikbaar om te worden uitgevoerd, en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Wanneer de conceptversie beschikbaar moet worden gesteld, publiceert u deze, die wordt de gepubliceerde versie door de conceptversie overschreven.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Voor het publiceren van een runbook met behulp van de Azure-portal

1. Open het runbook in Azure portal.
1. Klik op de **bewerken** knop.
1. Klik op de **publiceren** knop en vervolgens **Ja** in het bevestigingsbericht.

## <a name="to-publish-a-runbook-using-windows-powershell"></a>Een runbook met behulp van Windows PowerShell publiceren
U kunt de [publiceren-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/publish-azurermautomationrunbook) cmdlet om een runbook met Windows PowerShell te publiceren. De volgende voorbeeldopdrachten laten zien hoe u een voorbeeldrunbook publiceert.

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
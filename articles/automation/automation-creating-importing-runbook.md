---
title: Maken of importeren van een runbook in Azure Automation
description: In dit artikel wordt beschreven hoe een nieuw runbook maken in Azure Automation of een uit een bestand importeren.
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
ms.assetid: 24414362-b690-4474-8ca7-df18e30fc31d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte;bwren
ms.openlocfilehash: ad070333b85b70d911a492a35cc89c8c4a60e5c1
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Maken of importeren van een runbook in Azure Automation
U kunt een runbook toevoegen aan Azure Automation door [maken van een nieuwe](#creating-a-new-runbook) of een bestaand runbook importeren vanuit een bestand of vanuit de [Runbookgalerie](automation-runbook-gallery.md). In dit artikel bevat informatie over het maken en importeren van runbooks uit een bestand.  U kunt alle gegevens over de toegang tot de community-runbooks en modules in krijgen [galerieën Runbook en de module voor Azure Automation](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Een nieuw runbook maken
U kunt een nieuw runbook maken in Azure Automation via een van de Azure-portals of Windows PowerShell. Als het runbook is gemaakt, kunt u bewerken met behulp van informatie in [Learning PowerShell Workflow](automation-powershell-workflow.md) en [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Een nieuw Azure Automation-runbook maken met de klassieke Azure portal
U kunt alleen werken met [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks) in de Azure portal.

1. In de klassieke Azure portal, klikt u op, **nieuw**, **App Services**, **Automation**, **Runbook**, **snelle invoer**.
2. Voer de vereiste gegevens en klik vervolgens op **maken**. De runbooknaam moet beginnen met een letter en letters, cijfers, onderstrepingstekens en streepjes kan hebben.
3. Als u wilt dat het runbook nu bewerken, klikt u op **Runbook bewerken**. Klik anders op **OK**.
4. Uw nieuwe runbook wordt weergegeven op de **Runbooks** tabblad.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Een nieuw Azure Automation-runbook maken met de Azure-portal
1. Open uw Automation-account in Azure Portal.
2. Selecteer in de Hub **Runbooks** om de lijst van runbooks te openen.
3. Klik op de **een runbook toevoegen** knop en vervolgens **een nieuw runbook maken**.
4. Typ een **naam** voor het runbook en selecteer de [Type](automation-runbook-types.md). De runbooknaam moet beginnen met een letter en letters, cijfers, onderstrepingstekens en streepjes kan hebben.
5. Klik op **maken** voor het maken van het runbook en open de editor.

### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Een nieuw Azure Automation-runbook maken met Windows PowerShell
U kunt de [nieuw AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) cmdlet voor het maken van een lege [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks). U kunt opgeven de **naam** -parameter voor het maken van een leeg runbook dat u later kunt bewerken of u kunt opgeven de **pad** -parameter voor een runbook-bestand importeren. De **Type** parameter moet ook worden opgenomen in het opgeven van een van de vier runbooktypen.

De volgende voorbeeldopdrachten laten zien hoe een nieuw, leeg runbook maken.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Een runbook importeren uit een bestand in Azure Automation
U kunt een nieuw runbook maken in Azure Automation door het importeren van een PowerShell-script of PowerShell-werkstroom (.ps1 extensie), een geëxporteerde grafisch runbook (.graphrunbook) of een Python-2-script (.py extensie).  Geef de [type runbook](automation-runbook-types.md) dat is gemaakt tijdens het importeren, rekening houdend met de volgende overwegingen.

* Een bestand .graphrunbook kan alleen worden geïmporteerd in een nieuwe [grafisch runbook](automation-runbook-types.md#graphical-runbooks), en de grafische runbooks kunnen alleen worden gemaakt vanuit een bestand .graphrunbook.
* Een ps1-bestand met een PowerShell-werkstroom kan alleen worden geïmporteerd in een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als het bestand meerdere PowerShell-werkstromen bevat, klikt u vervolgens mislukt het importeren. U moet elke werkstroom opslaan in een eigen bestand en importeer elk afzonderlijk.
* Een .ps1-bestand bevat geen een werkstroom kan worden geïmporteerd in hetzij een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) of een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks).  Als het is geïmporteerd in een PowerShell Workflow-runbook, vervolgens wordt deze geconverteerd naar een werkstroom en opmerkingen zijn opgenomen in het runbook opgeven van de wijzigingen die zijn aangebracht.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Een runbook importeren uit een bestand met de klassieke Azure portal
De volgende procedure kunt u een scriptbestand importeren in Azure Automation.  Houd er rekening mee dat u alleen een ps1-bestand kunt importeren in een PowerShell Workflow-runbook met behulp van deze portal.  U moet de Azure portal gebruiken voor andere typen.

1. Selecteer in de klassieke Azure portal **Automation** en selecteer vervolgens een Automation-Account.
2. Klik op **Import**.
3. Klik op **bladeren naar bestand** en zoek het scriptbestand te importeren.
4. Als u wilt dat het runbook nu bewerken, klikt u op **Runbook bewerken**. Klik anders op OK.
5. Het nieuwe runbook wordt weergegeven op de **Runbooks** tabblad voor het Automation-Account.
6. U moet [het runbook gepubliceerd](#publishing-a-runbook) voordat u deze kunt uitvoeren.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Een runbook importeren uit een bestand met de Azure-portal
De volgende procedure kunt u een scriptbestand importeren in Azure Automation.  

> [!NOTE]
> Houd er rekening mee dat u alleen een ps1-bestand kunt importeren in een PowerShell Workflow-runbook met behulp van de portal.
> 
> 

1. Open uw Automation-account in Azure Portal.
2. Selecteer in de Hub **Runbooks** om de lijst van runbooks te openen.
3. Klik op de **een runbook toevoegen** knop en vervolgens **importeren**.
4. Klik op **Runbook bestand** het te importeren bestand selecteren
5. Als de **naam** veld is ingeschakeld, hebt u de optie om dit te wijzigen.  De runbooknaam moet beginnen met een letter en letters, cijfers, onderstrepingstekens en streepjes kan hebben.
6. De [runbooktype](automation-runbook-types.md) wordt automatisch geselecteerd, maar u kunt het type wijzigen nadat u de toepasselijke beperkingen rekening. 
7. Het nieuwe runbook wordt weergegeven in de lijst met runbooks voor het Automation-Account.
8. U moet [het runbook gepubliceerd](#publishing-a-runbook) voordat u deze kunt uitvoeren.

> [!NOTE]
> Als u een grafisch runbook of een grafische PowerShell workflow-runbook importeert, hebt u de optie om te converteren naar het andere type desgewenst. U kan niet converteren naar een tekstueel runbook.
>  
> 

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Een runbook importeren uit een scriptbestand met Windows PowerShell
U kunt de [importeren AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet om een scriptbestand importeren als een PowerShell Workflow-runbook-concept. Als het runbook al bestaat, het importeren mislukt tenzij u de *-Force* parameter. 

De volgende voorbeeldopdrachten laten zien hoe een scriptbestand importeren in een runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Een runbook publiceren
Wanneer u maakt of een nieuw runbook importeert, moet u het publiceren voordat u deze kunt uitvoeren.  Elk runbook in Automation heeft een ontwerpversie en een gepubliceerde versie. Alleen de gepubliceerde versie is beschikbaar om te worden uitgevoerd en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Wanneer de conceptversie beschikbaar moet worden gesteld, publiceert u deze waardoor de gepubliceerde versie met de conceptversie overschreven.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Voor het publiceren van een runbook met behulp van de klassieke Azure portal
1. Open het runbook in de klassieke Azure portal.
2. Klik boven aan het scherm op **auteur**.
3. Aan de onderkant van het scherm, klikt u op **publiceren** en vervolgens **Ja** in het bevestigingsbericht.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Een runbook met de Azure portal publiceren
1. Open het runbook in de Azure portal.
2. Klik op de **bewerken** knop.
3. Klik op de **publiceren** knop en vervolgens **Ja** in het bevestigingsbericht.

## <a name="to-publish-a-runbook-using-windows-powershell"></a>Een runbook met Windows PowerShell publiceren
U kunt de [publiceren AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) cmdlet voor het publiceren van een runbook met Windows PowerShell. De volgende voorbeeldopdrachten laten zien hoe een voorbeeldrunbook publiceren.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over hoe u van het Runbook en de PowerShell-Module-galerie profiteren kunt, [galerieën Runbook en de module voor Azure Automation](automation-runbook-gallery.md)
* Zie voor meer informatie over het bewerken van PowerShell en PowerShell Workflow-runbooks met een teksteditor, [tekstuele runbooks in Azure Automation bewerken](automation-edit-textual-runbook.md)
* Zie voor meer informatie over het ontwerpen van grafisch runbook, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)


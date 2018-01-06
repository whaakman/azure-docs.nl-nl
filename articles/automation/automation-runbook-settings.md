---
title: Runbookinstellingen | Microsoft Docs
description: Beschrijft de configuratie-instellingen voor een runbook in Azure Automation en het wijzigen van deze met de Azure-portal en de Windows PowerShell.
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 527c40c4a0c5c9a0eb4745f542a8afcc63281416
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="runbook-settings"></a>Runbook-instellingen
Elk runbook in Azure Automation heeft meerdere instellingen waarmee het kan worden geïdentificeerd en de logboekregistratie kan wijzigen. Elk van deze instellingen wordt hieronder beschreven en gevolgd door procedures voor het wijzigen van deze.

## <a name="settings"></a>Instellingen
### <a name="name-and-description"></a>Naam en beschrijving
U kunt de naam van een runbook niet wijzigen nadat deze is gemaakt. De beschrijving is optioneel en mag maximaal 512 tekens lang zijn.

### <a name="tags"></a>Tags
Labels kunt u verschillende woorden en woordgroepen om vast te stellen van een runbook toewijzen. Bijvoorbeeld, wanneer het indienen van een runbook de [PowerShell Gallery](https://www.powershellgallery.com/), u bepaalde labels om te identificeren welke categorieën die het runbook moet worden weergegeven in opgeven. U kunt meerdere labels voor een runbook opgeven door deze te scheiden met komma's.

### <a name="logging"></a>Logboekregistratie
Standaard worden uitgebreid en voortgang van de records niet naar Taakgeschiedenis geschreven. U kunt de instellingen voor een bepaald runbook voor logboekregistratie van deze records te wijzigen. Zie voor meer informatie over deze records [Runbookuitvoer en -berichten](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Runbookinstellingen

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Runbookinstellingen met de Azure-portal
Kunt u instellingen voor een runbook in de Azure portal van de **instellingen** blade voor het runbook.

1. Selecteer in de Azure-portal **Automation** en klik vervolgens op de naam van een automation-account.
2. Selecteer de **Runbooks** tabblad.
3. Klik op de naam van een runbook en wordt u omgeleid naar de instellingenblade voor het runbook. Hier kunt u opgeven of labels, de beschrijving van de runbook wijzigen, logboekregistratie en traceringsinstellingen configureren en toegang tot ondersteuning voor hulpprogramma's om u te helpen bij het oplossen van problemen.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Runbookinstellingen met Windows PowerShell
U kunt de [Set AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet om de instellingen voor een runbook wijzigen. Als u meerdere labels opgeven wilt, kunt u ofwel een matrix of een tekenreeks met door komma's gescheiden waarden naar de Tags-parameter opgeven. U kunt de huidige labels met krijgen de [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

De volgende voorbeeldopdrachten laten zien hoe de eigenschappen voor een runbook instellen. Dit voorbeeld worden drie tags toegevoegd aan de bestaande labels en geeft aan dat uitgebreide records moeten worden vastgelegd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken en ophalen van de uitvoer en foutberichten van runbooks, [Runbookuitvoer en -berichten](automation-runbook-output-and-messages.md) 
* Om te begrijpen hoe een runbook dat al is ontwikkeld door de community of een andere bron toevoegen of maken van uw eigen Zie runbook [een Runbook maken of importeren](automation-creating-importing-runbook.md) 


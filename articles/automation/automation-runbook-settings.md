---
title: Runbook-instellingen in Azure Automation
description: Beschrijving van de configuratie-instellingen voor een runbook in Azure Automation en wijzigen met behulp van de Azure portal en de Windows PowerShell.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 589df59e02a18629d5f405ff1ce8870333f2228e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397710"
---
# <a name="runbook-settings"></a>Runbook-instellingen
Elk runbook in Azure Automation heeft meerdere instellingen waarmee het kan worden geïdentificeerd en de logboekregistratie te wijzigen. Elk van deze instellingen wordt hieronder beschreven gevolgd door procedures voor het wijzigen van deze.

## <a name="settings"></a>Instellingen
### <a name="name-and-description"></a>Naam en beschrijving
U kunt de naam van een runbook niet wijzigen nadat deze is gemaakt. De beschrijving is optioneel en bevat maximaal 512 tekens.

### <a name="tags"></a>Tags
Tags kunnen u verschillende woorden en woordgroepen voor het identificeren van een runbook toewijzen. Bijvoorbeeld, wanneer u een runbook om te verzenden de [PowerShell Gallery](https://www.powershellgallery.com/), u bepaalde labels om te bepalen welke categorieën het runbook moet worden weergegeven in opgeven. U kunt meerdere labels voor een runbook opgeven door deze te scheiden met komma's.

### <a name="logging"></a>Logboekregistratie
Uitgebreide records en Voortgangsrecords worden standaard niet naar Taakgeschiedenis geschreven. U kunt de instellingen voor een bepaald runbook voor logboekregistratie van deze records wijzigen. Zie voor meer informatie over deze records [Runbook Output and Messages](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Runbookinstellingen wijzigen

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Runbookinstellingen wijzigen met de Azure-portal
U kunt instellingen voor een runbook in de Azure-portal wijzigen de **instellingen** blade voor het runbook.

1. Selecteer in de Azure portal, **Automation** en klik vervolgens op de naam van een automation-account.
2. Selecteer de **Runbooks** tabblad.
3. Klik op de naam van een runbook en u worden doorgestuurd naar de instellingenblade voor het runbook. Hier kunt u opgeven of wijzigen van de runbookbeschrijving van de-tags, logboekregistratie en traceringsinstellingen configureren en toegang tot ondersteuning voor hulpprogramma's om u te helpen bij het oplossen van problemen.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Runbookinstellingen wijzigen met Windows PowerShell
U kunt de [Set-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/set-azurermautomationrunbook) cmdlet om de instellingen voor een runbook te wijzigen. Als u opgeven van meerdere labels wilt, kunt u ofwel een matrix of een tekenreeks met door komma's gescheiden waarden naar de parameter Tags opgeven. Krijgt u de huidige labels met de [Get-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/get-azurermautomationrunbook).

De volgende voorbeeldopdrachten laten zien hoe de eigenschappen voor een runbook in te stellen. In dit voorbeeld wordt drie codes toegevoegd aan de bestaande tags en geeft aan dat uitgebreide records moeten worden geregistreerd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken en ophalen van de uitvoer en foutberichten van runbooks, [Runbook Output and Messages](automation-runbook-output-and-messages.md) 
* Om te begrijpen hoe u een runbook die al is ontwikkeld door de community of een andere bron toevoegen of maken van uw eigen runbook-Zie [een Runbook maken of importeren](automation-creating-importing-runbook.md) 


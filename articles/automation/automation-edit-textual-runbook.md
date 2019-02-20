---
title: Tekstrunbooks in Azure Automation bewerken
description: Dit artikel bevat verschillende procedures voor het werken met PowerShell en PowerShell-werkstroom runbooks in Azure Automation met behulp van de teksteditor.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9c684e9d1bf6cec12024cedfb5360d10e400e139
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416001"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Tekstrunbooks in Azure Automation bewerken

De teksteditor in Azure Automation kan worden gebruikt om te bewerken [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks) en [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks). Dit heeft de typische kenmerken van andere code-editors, zoals intellisense en kleurcodering met extra speciale functies om u te helpen bij het openen van bronnen voor runbooks. Dit artikel bevat gedetailleerde stappen voor het uitvoeren van verschillende functies met deze editor.

De teksteditor bevat een functie voor het invoegen van code voor cmdlets, middelen en onderliggende runbooks in een runbook. In plaats van te typen in de code zelf, kunt u selecteren uit een lijst van beschikbare resources en de juiste code in het runbook ingevoegd.

Elk runbook in Azure Automation heeft twee versies, ontwerp en gepubliceerd. U bewerkt de conceptversie van het runbook en publiceert deze zodat deze kan worden uitgevoerd. De gepubliceerde versie kan niet worden bewerkt. Zie voor meer informatie, [een runbook publiceren](manage-runbooks.md#publish-a-runbook).

Werken met [grafische Runbooks](automation-runbook-types.md#graphical-runbooks), Zie [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Een runbook bewerken met de Azure-portal

Gebruik de volgende procedure om te openen van een runbook bewerken in de teksteditor.

1. Selecteer in de Azure-portal uw automation-account.
2. Onder **PROCESAUTOMATISERING**, selecteer **Runbooks** om de lijst van runbooks te openen.
3. Selecteer het runbook dat u wilt bewerken en klik vervolgens op de **bewerken** knop.
4. Bewerk het runbook.
5. Klik op **opslaan** wanneer de bewerkingen zijn voltooid.
6. Klik op **publiceren** als u wilt dat de meest recente conceptversie van het runbook wordt gepubliceerd.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Een cmdlet in een runbook invoegen

1. In het Canvas van de teksteditor, plaats de cursor waar u wilt plaatsen van de cmdlet.
2. Vouw de **Cmdlets** knooppunt in het besturingselement bibliotheek.
3. Vouw de module met de cmdlet die u wilt gebruiken.
4. Met de rechtermuisknop op de cmdlet als u wilt invoegen en selecteer **toevoegen aan papier**. Als de cmdlet meer dan één parameter is ingesteld heeft, wordt de standaardset worden toegevoegd. U kunt ook de cmdlet om te selecteren van een andere parameterset uitbreiden.
5. De code voor de cmdlet wordt ingevoegd met de volledige lijst met parameters.
6. Geef een passende waarde in plaats van het gegevenstype omgeven door accolades <> voor eventueel vereiste parameters. Verwijder alle parameters die u niet nodig hebt.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor een onderliggend runbook in een runbook invoegen

1. In het Canvas van de teksteditor, plaats de cursor waar u de code voor de [onderliggend runbook](automation-child-runbooks.md).
2. Vouw de **Runbooks** knooppunt in het besturingselement bibliotheek.
3. Met de rechtermuisknop op het runbook wilt invoegen en selecteer **toevoegen aan papier**.
4. De code voor het onderliggende runbook wordt ingevoegd met een tijdelijke aanduidingen voor elke runbookparameters.
5. Vervang de tijdelijke aanduidingen met de juiste waarden voor elke parameter.

### <a name="to-insert-an-asset-into-a-runbook"></a>Een asset in een runbook invoegen

1. In het Canvas van de teksteditor, plaats de cursor waar u de code voor het onderliggende runbook.
2. Vouw de **activa** knooppunt in het besturingselement bibliotheek.
3. Vouw het knooppunt voor het type van de asset die u wilt.
4. Met de rechtermuisknop op de asset wilt invoegen en selecteer **toevoegen aan papier**. Voor [variabele assets](automation-variables.md), selecteert u **"variabele ophalen' naar het canvas toevoegen** of **"variabele instellen' op het canvas toevoegen** , afhankelijk van of u wilt ophalen of instellen van de variabele.
5. De code voor de asset wordt ingevoegd in het runbook.

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Een Azure Automation-runbook met behulp van Windows PowerShell bewerken

Als u wilt bewerken in een runbook met Windows PowerShell, u de editor van uw keuze gebruiken en sla deze op een `.ps1` bestand. U kunt de [Export-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Export-AzureRmAutomationRunbook) cmdlet voor het ophalen van de inhoud van het runbook en vervolgens [Import-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/import-azurermautomationrunbook) cmdlet om te vervangen door de bestaande ontwerp voor een runbook met de een gewijzigd.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>De inhoud van een Runbook met behulp van Windows PowerShell ophalen

De volgende voorbeeldopdrachten laten zien hoe het script voor een runbook ophalen en sla deze op een scriptbestand. In dit voorbeeld wordt de conceptversie opgehaald. Het is ook mogelijk om op te halen van de gepubliceerde versie van het runbook, hoewel deze versie kan niet worden gewijzigd.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>De inhoud van een Runbook met behulp van Windows PowerShell wijzigen

De volgende voorbeeldopdrachten laten zien hoe u de bestaande inhoud van een runbook vervangt door de inhoud van een scriptbestand. Dit is dezelfde voorbeeldprocedure als in [voor het importeren van een runbook uit een scriptbestand met Windows PowerShell](manage-runbooks.md#import-a-runbook).

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzureRmAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>Verwante artikelen:

* [Runbooks in Azure Automation beheren](manage-runbooks.md)
* [PowerShell workflow leren kennen](automation-powershell-workflow.md)
* [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* [Certificaten](automation-certificates.md)
* [Verbindingen](automation-connections.md)
* [Referenties](automation-credentials.md)
* [Schema's](automation-schedules.md)
* [Variabelen](automation-variables.md)


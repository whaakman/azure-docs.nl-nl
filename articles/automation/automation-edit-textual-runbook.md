---
title: Tekstuele runbooks in Azure Automation bewerken
description: Dit artikel bevat verschillende procedures voor het werken met PowerShell en PowerShell Workflow-runbooks in Azure Automation met behulp van de teksteditor.
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: 6f5b48fb-6f30-4e99-9e14-9061b5554b08
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: magoedte;bwren
ms.openlocfilehash: e166700be0ec6b32d40f34bd47f92a7cff1cc7bf
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Tekstuele runbooks in Azure Automation bewerken
De teksteditor in Azure Automation kan worden gebruikt om te bewerken [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks) en [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks). Dit heeft de typische functies van andere code-editor zoals intellisense en kleurcodering met aanvullende speciale functies om u te helpen bij het openen van bronnen die gemeenschappelijk zijn voor runbooks.  In dit artikel biedt gedetailleerde stappen voor het uitvoeren van verschillende functies met deze editor.

De teksteditor bevat een functie voor het invoegen van code voor activiteiten, assets en onderliggende runbooks in een runbook. U kunt in plaats van te typen in de code zelf selecteren uit een lijst met beschikbare resources en de juiste code is ingevoegd in het runbook.

Elk runbook in Azure Automation heeft twee versies, ontwerp- en gepubliceerd. U bewerkt de conceptversie van het runbook en publiceert deze zodat deze kan worden uitgevoerd. De gepubliceerde versie kan niet worden bewerkt. Zie [een runbook publiceren](automation-creating-importing-runbook.md#publishing-a-runbook) voor meer informatie.

Werken met [grafische Runbooks](automation-runbook-types.md#graphical-runbooks), Zie [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Een runbook bewerken met de Azure-portal
Gebruik de volgende procedure een runbook bewerken in de teksteditor te openen.

1. Selecteer uw automation-account in de Azure-portal.
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Klik op de naam van het runbook dat u wilt bewerken en klik vervolgens op de **bewerken** knop.
4. Voer de vereiste bewerkingen.
5. Klik op **opslaan** wanneer de bewerkingen zijn voltooid.
6. Klik op **publiceren** als u wilt dat de meest recente conceptversie van het runbook te publiceren.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Een cmdlet in een runbook invoegen
1. Het Canvas van de teksteditor zet de cursor in waar u de cmdlet plaatsen.
2. Vouw de **Cmdlets** knooppunt in het besturingselement bibliotheek.
3. Vouw in de module met de cmdlet die u wilt gebruiken.
4. Klik met de rechtermuisknop op de cmdlet invoegen en selecteer **toevoegen aan papier**.  Als de cmdlet meer dan één parameter is ingesteld heeft, wordt de standaardset worden toegevoegd.  U kunt ook de cmdlet om te selecteren van een andere parameterset uitbreiden.
5. De code voor de cmdlet worden opgenomen met de volledige lijst van parameters.
6. Kies een passende waarde in plaats van het gegevenstype omgeven door accolades <> voor alle vereiste parameters.  U hoeft geen parameters worden verwijderd.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor een onderliggend runbook in een runbook invoegen
1. Het Canvas van de teksteditor zet de cursor in waar u de code voor de [onderliggend runbook](automation-child-runbooks.md).
2. Vouw de **Runbooks** knooppunt in het besturingselement bibliotheek.
3. Klik met de rechtermuisknop op het runbook invoegen en selecteer **toevoegen aan papier**.
4. De code voor het onderliggende runbook wordt ingevoegd met een tijdelijke aanduidingen voor de runbookparameters.
5. Vervang de tijdelijke aanduidingen met de juiste waarden voor elke parameter.

### <a name="to-insert-an-asset-into-a-runbook"></a>Een asset in een runbook invoegen
1. Het Canvas van de teksteditor zet de cursor in waar u de code voor het onderliggende runbook.
2. Vouw de **activa** knooppunt in het besturingselement bibliotheek.
3. Vouw het knooppunt voor het type van de asset die u wilt.
4. Klik met de rechtermuisknop op de asset invoegen en selecteer **toevoegen aan papier**.  Voor [variabele assets](automation-variables.md), selecteert u **'Variabele ophalen' naar het canvas toevoegen** of **toevoegen '-variabele instellen' naar het canvas** afhankelijk van of u wilt ophalen of instellen van de variabele.
5. De code voor de activa wordt in het runbook ingevoegd.

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Een runbook bewerken met de Azure-portal
Gebruik de volgende procedure een runbook bewerken in de teksteditor te openen.

1. Selecteer in de Azure-portal **Automation** en klik vervolgens op de naam van een automation-account.
2. Selecteer de **Runbooks** tabblad.
3. Klik op de naam van het runbook dat u wilt bewerken en selecteer vervolgens de **auteur** tabblad.
4. Klik op de **bewerken** knop aan de onderkant van het scherm.
5. Voer de vereiste bewerkingen.
6. Klik op **opslaan** wanneer de bewerkingen zijn voltooid.
7. Klik op **publiceren** als u wilt dat de meest recente conceptversie van het runbook te publiceren.

### <a name="to-insert-an-activity-into-a-runbook"></a>Een activiteit in een Runbook invoegen
1. Het Canvas van de teksteditor zet de cursor in waar u de activiteit te plaatsen.
2. Aan de onderkant van het scherm, klikt u op **invoegen** en vervolgens **activiteit**.
3. In de **integratiemodule** kolom, selecteert u de module waarin de activiteit.
4. In de **activiteit** deelvenster, selecteert u een activiteit.
5. In de **beschrijving** kolom, Let op de beschrijving van de activiteit. Desgewenst kunt u weergeven gedetailleerde help help voor de activiteit in de browser te starten.
6. Klik op de pijl naar rechts.  Als de activiteit parameters bevat, wordt deze voor uw gegevens worden weergegeven.
7. Klik op de knop controleren.  Code voor het uitvoeren van de activiteit wordt ingevoegd in het runbook.
8. Als de activiteit parameters vereist, typt u een geschikte waarde in plaats van het gegevenstype omgeven door accolades <>.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Code voor een onderliggend runbook in een runbook invoegen
1. Het Canvas van de teksteditor zet de cursor in waar u wilt plaatsen de [onderliggend runbook](automation-child-runbooks.md).
2. Aan de onderkant van het scherm, klikt u op **invoegen** en vervolgens **Runbook**.
3. Selecteer het runbook invoegen in de middelste kolom en klik op de pijl naar rechts.
4. Als het runbook parameters heeft, wordt deze voor uw gegevens worden weergegeven.
5. Klik op de knop controleren.  Code voor het uitvoeren van het geselecteerde runbook wordt ingevoegd in het huidige runbook.
6. Als het runbook parameters vereist, typt u een geschikte waarde in plaats van het gegevenstype omgeven door accolades <>.

### <a name="to-insert-an-asset-into-a-runbook"></a>Een asset in een runbook invoegen
1. Het Canvas van de teksteditor zet de cursor in waar u de activiteit voor het ophalen van de asset plaatsen.
2. Aan de onderkant van het scherm, klikt u op **invoegen** en vervolgens **instelling**.
3. In de **Instellingsactie** kolom, selecteer de actie die u wilt.
4. Selecteer in de beschikbare elementen in de middelste kolom.
5. Klik op de knop controleren.  Code ophalen of instellen van de activa wordt ingevoegd in het runbook.

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Een Azure Automation-runbook met Windows PowerShell bewerken
Als u wilt een runbook bewerken met Windows PowerShell, gebruikt u editor van uw keuze en opslaan in een .ps1-bestand. U kunt de [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) cmdlet voor het ophalen van de inhoud van het runbook en vervolgens [Set AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) cmdlet het bestaande runbook concept door het gewijzigde.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>De inhoud van een Runbook met Windows PowerShell ophalen
De volgende voorbeeldopdrachten laten zien hoe het script voor een runbook ophalen en opslaan in een scriptbestand. In dit voorbeeld wordt de conceptversie opgehaald. Het is ook mogelijk om op te halen van de gepubliceerde versie van het runbook, hoewel deze versie kan niet worden gewijzigd.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>De inhoud van een Runbook met Windows PowerShell wijzigen
De volgende voorbeeldopdrachten laten zien hoe de bestaande inhoud van een runbook vervangt door de inhoud van een scriptbestand. Houd er rekening mee dat dit dezelfde voorbeeldprocedure als in is [een runbook importeren uit een scriptbestand met Windows PowerShell](automation-creating-importing-runbook.md).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Verwante artikelen:
* [Maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md)
* [Learning PowerShell workflow](automation-powershell-workflow.md)
* [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* [Certificaten](automation-certificates.md)
* [Verbindingen](automation-connections.md)
* [Referenties](automation-credentials.md)
* [Schema's](automation-schedules.md)
* [Variabelen](automation-variables.md)

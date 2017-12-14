---
title: Een runbook starten in Azure Automation | Microsoft Docs
description: Geeft een overzicht van de verschillende methoden die kunnen worden gebruikt voor het starten van een runbook in Azure Automation en biedt details over het gebruik van de Azure-portal en de Windows PowerShell.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 6ee756b4-9200-4eb2-9bda-ec156853803b
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 4387f3ed56f1adb562e29b9d152d113859c88b06
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a>Een runbook starten in Azure Automation
De volgende tabel kunt u bepalen welke methode voor het starten van een runbook in Azure Automation die het meest geschikt is voor uw specifieke scenario. Dit artikel bevat informatie over het starten van een runbook met de Azure-portal en met Windows PowerShell. Meer informatie over de andere methoden beschikbaar in andere documentatie die u vanaf de onderstaande koppelingen openen kunt.

| **METHODE** | **KENMERKEN** |
| --- | --- |
| [Azure Portal](#starting-a-runbook-with-the-azure-portal) |<li>Eenvoudigste methode met interactieve gebruikersinterface.<br> <li>Formulier eenvoudige parameterwaarden opgeven.<br> <li>Taakstatus op eenvoudige wijze volgen.<br> <li>De toegang wordt geverifieerd met aanmelding bij Azure. |
| [Windows PowerShell](https://msdn.microsoft.com/library/dn690259.aspx) |<li>Aanroepen vanuit de opdrachtregel met Windows PowerShell-cmdlets.<br> <li>Geautomatiseerde oplossing met meerdere stappen kunnen worden opgenomen.<br> <li>Aanvraag is geverifieerd met een certificaat of de OAuth-gebruiker principal / service principal.<br> <li>Eenvoudige en complexe parameterwaarden opgeven.<br> <li>Taakstatus bijhouden.<br> <li>De client vereist ter ondersteuning van PowerShell-cmdlets. |
| [Azure Automation-API](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li>Meest flexibele methode, maar ook de meeste complex.<br> <li>Aanroepen vanuit elke gewenste aangepaste code die HTTP-aanvragen kan maken.<br> <li>Aanvraag geverifieerd met het certificaat of de Oauth-gebruiker principal / service principal.<br> <li>Eenvoudige en complexe parameterwaarden opgeven.<br> <li>Taakstatus bijhouden. |
| [Webhooks.](automation-webhooks.md) |<li>Runbook starten vanuit één HTTP-aanvraag.<br> <li>Geverifieerd met beveiligingstoken in URL.<br> <li>Client overschrijven niet parameterwaarden die zijn opgegeven wanneer webhook is gemaakt. Runbook kunt definiëren één parameter die is gevuld met de details van de HTTP-aanvraag.<br> <li>Er is geen mogelijkheid om bij te houden taakstatus via webhook-URL. |
| [Reageren op Azure waarschuwing](../log-analytics/log-analytics-alerts.md) |<li>Een runbook starten in reactie op Azure waarschuwing.<br> <li>Webhook voor runbook en een koppeling naar een waarschuwing configureren.<br> <li>Geverifieerd met beveiligingstoken in URL. |
| [Planning](automation-schedules.md) |<li>Start runbook automatisch op elk uur, dagelijks, wekelijks of maandelijks schema.<br> <li>Manipuleren planning via Azure portal, PowerShell-cmdlets of Azure-API.<br> <li>Parameterwaarden moet worden gebruikt met een planning opgeven. |
| [Vanuit een ander Runbook](automation-child-runbooks.md) |<li>Een runbook gebruiken als een activiteit in een ander runbook.<br> <li>Dit is handig voor functionaliteit die wordt gebruikt door meerdere runbooks.<br> <li>Parameterwaarden voor onderliggend runbook opgeven en het gebruik van uitvoer in bovenliggende runbook. |

De volgende afbeelding illustreert de gedetailleerde stapsgewijze proces in de levenscyclus van een runbook. Dit omvat verschillende manieren die een runbook wordt gestart in Azure Automation onderdelen vereist zijn voor hybride Runbook Worker Azure Automation-runbooks en interacties tussen de verschillende onderdelen uit te voeren. Raadpleeg voor meer informatie over het uitvoeren van Automation-runbooks in uw datacenter, [hybride runbook workers](automation-hybrid-runbook-worker.md)

![Runbook-architectuur](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-the-azure-portal"></a>Een runbook starten met de Azure-portal
1. Selecteer in de Azure-portal **Automation** en klik vervolgens op de naam van een automation-account.
2. Selecteer in het menu Hub **Runbooks**.
3. Op de **Runbooks** blade, selecteert u een runbook en klik vervolgens op **Start**.
4. Als het runbook parameters heeft, wordt u gevraagd waarden met een tekstvak voor elke parameter opgeven. Zie [Runbookparameters](#Runbook-parameters) hieronder voor meer informatie over parameters.
5. Op de **taak** blade kunt u de status van de runbooktaak weergeven.

## <a name="starting-a-runbook-with-windows-powershell"></a>Een runbook starten met Windows PowerShell
U kunt de [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) een runbook starten met Windows PowerShell. De volgende voorbeeldcode wordt een runbook met de naam Test-Runbook gestart.

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Start AzureRmAutomationRunbook retourneert een taakobject dat u de status ervan bijhouden kunt zodra het runbook wordt gestart. U kunt dit object met [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) om te bepalen van de status van de taak en [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) ophalen van de uitvoer ervan weergegeven. De volgende voorbeeldcode wordt gestart van een runbook met de naam Test-Runbook, wordt er gewacht totdat deze is voltooid en vervolgens de uitvoer ervan weergegeven wordt.

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Als het runbook parameters vereist, dan u als opgeven moet een [hashtabel](http://technet.microsoft.com/library/hh847780.aspx) waar de sleutel van de hashtabel overeenkomt met de parameternaam van de en de waarde is de waarde van parameter. Het volgende voorbeeld ziet hoe u een runbook met twee Reeksparameters met de naam FirstName en LastName, een geheel getal met de naam RepeatCount en een Boole-parameter met de naam Show start. Zie voor meer informatie over parameters [Runbookparameters](#Runbook-parameters) hieronder.

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook-parameters
Wanneer u een runbook vanuit de Azure Portal of de Windows PowerShell start, wordt de instructie verzonden via de Azure Automation-webservice. Deze service biedt geen ondersteuning voor parameters met complexe gegevenstypen. Als u moet een waarde opgeven voor een complexe parameter, moet u deze inline vanuit een ander runbook aanroepen moet zoals beschreven in [onderliggende runbooks in Azure Automation](automation-child-runbooks.md).

De webservice Azure Automation biedt speciale functionaliteit voor parameters die bepaalde gegevenstypen zoals beschreven in de volgende secties gebruiken.

### <a name="named-values"></a>Naamwaarden
Als de parameter gegevenstype [object] is, dan kunt u de volgende JSON-indeling te verzenden in een lijst met naamwaarden: *{Name1: 'Value1', Naam2: 'Waarde2', Name3: 'Value3'}*. Deze waarden moeten eenvoudige typen. Het runbook ontvangt de parameter als een [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) met eigenschappen die met elke benoemde waarde overeenkomen.

Houd rekening met het volgende testrunbook dat een parameter met de naam van de gebruiker accepteert.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

De volgende tekst kan worden gebruikt voor de parameter user.

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Dit resulteert in de volgende uitvoer.

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Matrices
Als de parameter een matrix zoals [array is] of [string []], kunt u de volgende JSON-indeling gebruiken om te verzenden in een lijst met waarden: *[Value1, Value2, Value3]*. Deze waarden moeten eenvoudige typen.

Houd rekening met het volgende testrunbook dat een parameter met de naam accepteert *gebruiker*.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

De volgende tekst kan worden gebruikt voor de parameter user.

```
["Joe","Smith",2,true]
```

Dit resulteert in de volgende uitvoer.

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Referenties
Als de parameter gegevenstype **PSCredential**, kunt u de naam van een Azure Automation opgeven [referentieasset](automation-credentials.md). Het runbook haalt de referentie met de naam die u opgeeft.

Houd rekening met het volgende testrunbook dat een parameter met de naam van de referentie accepteert.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

De volgende tekst kan worden gebruikt voor de gebruiker parameter mits het referentie-element *mijn referentie*.

```
My Credential
```

Als de gebruikersnaam in de referentie is *jsmith*, resulteert dit in de volgende uitvoer.

```
jsmith
```

## <a name="next-steps"></a>Volgende stappen
* De runbook-architectuur in het huidige artikel biedt een overzicht van het beheer van bronnen van de runbooks in Azure en on-premises met de hybride Runbook Worker.  Raadpleeg voor meer informatie over het uitvoeren van Automation-runbooks in uw datacenter, [Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).
* Raadpleeg voor meer informatie over het maken, modulaire runbooks moet worden gebruikt door andere runbooks voor specifieke of algemene functies [onderliggende Runbooks](automation-child-runbooks.md).


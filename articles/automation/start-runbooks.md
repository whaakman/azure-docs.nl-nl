---
title: Een runbook in Azure Automation starten
description: Geeft een overzicht van de verschillende methoden die kunnen worden gebruikt om een runbook in Azure Automation starten en biedt details over het gebruik van de Azure-portal en de Windows PowerShell.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3fd2feb8bbd54e7aefab357b0b2ba4209aa29e09
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418587"
---
# <a name="start-a-runbook-in-azure-automation"></a>Een runbook in Azure Automation starten

De volgende tabel om te bepalen de methode voor het starten van een runbook in Azure Automation die het meest geschikt is voor uw specifieke scenario. Dit artikel bevat informatie over het starten van een runbook met de Azure-portal en met Windows PowerShell. Meer informatie over de andere methoden zijn beschikbaar in andere documentatie die toegankelijk is via de onderstaande koppelingen.

| **Methode** | **Kenmerken** |
| --- | --- |
| [Azure Portal](#starting-a-runbook-with-the-azure-portal) |<li>De eenvoudigste methode met interactieve gebruikersinterface.<br> <li>Formulier voor eenvoudige parameterwaarden.<br> <li>Taakstatus probleem gemakkelijk bijhouden.<br> <li>Toegang is geverifieerd met Azure sign in. |
| [Windows PowerShell](/powershell/module/azurerm.automation/start-azurermautomationrunbook) |<li>Aanroepen vanaf de opdrachtregel met Windows PowerShell-cmdlets.<br> <li>Kan worden opgenomen in geautomatiseerde oplossing met meerdere stappen.<br> <li>Aanvraag is geverifieerd met certificaat of de OAuth-gebruiker principal / service principal.<br> <li>Eenvoudige en complexe parameterwaarden opgeven.<br> <li>Taakstatus bijhouden.<br> <li>De client is vereist ter ondersteuning van PowerShell-cmdlets. |
| [Azure Automation API](/rest/api/automation/) |<li>Meest flexibele methode, maar ook de meeste complexe.<br> <li>Aanroepen van aangepaste code die de HTTP-aanvragen kunt maken.<br> <li>Aanvraag geverifieerd met certificaat of de Oauth-gebruiker principal / service principal.<br> <li>Eenvoudige en complexe parameterwaarden opgeven. *Als u een Python-runbook met behulp van de API aanroept bent, moet de JSON-nettolading worden geserialiseerd.*<br> <li>Taakstatus bijhouden. |
| [Webhooks](automation-webhooks.md) |<li>Runbook starten vanuit één HTTP-aanvraag.<br> <li>Geverifieerd met de security-token in URL.<br> <li>Client kan geen parameterwaarden opgegeven tijdens het webhook overschrijven. Runbook kunt definiëren één parameter die is gevuld met de details van de HTTP-aanvraag.<br> <li>Er is geen mogelijkheid om bij te houden van de taakstatus via webhook-URL. |
| [Reageren op Azure waarschuwing](../log-analytics/log-analytics-alerts.md) |<li>Een runbook starten in reactie op Azure-waarschuwing.<br> <li>Webhook voor het runbook en een koppeling naar de waarschuwing te configureren.<br> <li>Geverifieerd met de security-token in URL. |
| [Planning](automation-schedules.md) |<li>Runbook automatisch gestart op basis van per uur, dagelijks, wekelijks of maandelijks schema.<br> <li>Bewerken via Azure portal, PowerShell-cmdlets of -API van Azure.<br> <li>Geef parameterwaarden met planning moet worden gebruikt. |
| [Vanuit een ander Runbook](automation-child-runbooks.md) |<li>Gebruik van een runbook als een activiteit in een ander runbook.<br> <li>Dit is handig voor functionaliteit die wordt gebruikt door meerdere runbooks.<br> <li>Geef parameterwaarden op onderliggend runbook en uitvoer gebruiken in het bovenliggende runbook. |

De volgende afbeelding ziet u gedetailleerde stapsgewijze proces in de levenscyclus van een runbook. Het bevat verschillende manieren om een runbook wordt gestart in Azure Automation, welke onderdelen die vereist zijn voor Hybrid Runbook Worker voor Azure Automation-runbooks en interacties tussen de verschillende onderdelen uit te voeren. Raadpleeg voor meer informatie over het uitvoeren van Automation-runbooks in uw datacenter, [hybrid runbook workers](automation-hybrid-runbook-worker.md)

![Runbook-architectuur](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="start-a-runbook-with-the-azure-portal"></a>Een runbook te starten met Azure portal

1. Selecteer in de Azure portal, **Automation** en klik vervolgens op de naam van een automation-account.
2. Selecteer in het menu Hub **Runbooks**.
3. Op de **Runbooks** pagina, selecteert u een runbook en klik vervolgens op **Start**.
4. Als het runbook parameters heeft, wordt u gevraagd om te voorzien van waarden een tekstvak voor elke parameter. Zie voor meer informatie over parameters [Runbookparameters](#Runbook-parameters).
5. Op de **taak** pagina vindt u de status van de runbooktaak.

## <a name="start-a-runbook-with-powershell"></a>Een runbook te starten met PowerShell

U kunt de [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) een runbook starten met Windows PowerShell. De volgende voorbeeldcode wordt een runbook met de naam Test-Runbook gestart.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Start-AzureRmAutomationRunbook retourneert een taakobject dat u gebruiken kunt voor het bijhouden van de status ervan zodra het runbook wordt gestart. U kunt dit object met [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) om de status van de taak te bepalen en [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) om de uitvoer te verkrijgen. De volgende voorbeeldcode start een runbook met de naam Test-Runbook, wordt er gewacht totdat deze is voltooid en vervolgens de uitvoer weergegeven.

```azurepowershell-interactive
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

Als het runbook parameters vereist, wordt u deze als verstrekken moet een [hashtabel](https://technet.microsoft.com/library/hh847780.aspx). De sleutel van de hashtabel moet overeenkomen met de parameternaam en de waarde is de waarde van parameter. Het volgende voorbeeld ziet hoe u een runbook met twee Reeksparameters met de naam FirstName en LastName, een geheel getal met de naam RepeatCount en een Boole-parameter met de naam Show start. Zie voor meer informatie over parameters [Runbookparameters](#Runbook-parameters) hieronder.

```azurepowershell-interactive
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook-parameters

Wanneer u een runbook vanuit de Azure portal of de Windows PowerShell starten, wordt de instructie verzonden via de Azure Automation-webservice. Deze service biedt geen ondersteuning voor parameters met complexe gegevenstypen. Als u moet een waarde opgeven voor een complexe parameter, dan u deze inline vanuit een ander runbook aanroepen moet zoals beschreven in [onderliggende runbooks in Azure Automation](automation-child-runbooks.md).

De webservice Azure Automation biedt speciale functionaliteit voor parameters die bepaalde gegevenstypen, zoals beschreven in de volgende secties gebruiken:

### <a name="named-values"></a>Benoemde waarden

Als de parameter gegevenstype [object] is, dan kunt u de volgende JSON-indeling voor het verzenden van een lijst met naamwaarden: *{Name1: 'Value1', Name2: 'Value2', Naam3: 'Value3'}*. Deze waarden moeten eenvoudige typen. Het runbook ontvangt de parameter als een [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) met eigenschappen die overeenkomen met elke benoemde waarde.

Houd rekening met de volgende runbook-test die een parameter met de naam van gebruiker accepteert.

```powershell
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

```json
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Dit resulteert in de volgende uitvoer:

```output
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>matrices

Als de parameter een matrix, zoals [array is] of [string []], kunt u de volgende JSON-indeling gebruiken voor het verzenden van een lijst met waarden: *[Value1, Value2, Value3]*. Deze waarden moeten eenvoudige typen.

Houd rekening met de volgende runbook-test die een parameter met de naam accepteert *gebruiker*.

```powershell
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

```input
["Joe","Smith",2,true]
```

Dit resulteert in de volgende uitvoer:

```output
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Referenties

Als de parameter het gegevenstype is **PSCredential**, kunt u de naam van een Azure Automation verstrekken [referentie-element](automation-credentials.md). Het runbook haalt de referenties met de naam die u opgeeft.

Houd rekening met de volgende runbook-test die een parameter met de naam van referenties accepteert.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

De volgende tekst kan worden gebruikt voor de gebruiker parameter mits het referentie-element *mijn referentie*.

```input
My Credential
```

Als de gebruikersnaam in de referentie *jsmith*, resulteert dit in de volgende uitvoer:

```output
jsmith
```

## <a name="next-steps"></a>Volgende stappen

* De runbook-architectuur in het huidige artikel biedt een overzicht op hoog niveau van beheer van runbooks in Azure en on-premises met de Hybrid Runbook Worker. Raadpleeg voor meer informatie over het uitvoeren van Automation-runbooks in uw datacenter, [Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).
* Raadpleeg voor meer informatie over het maken, modulaire runbooks moet worden gebruikt door andere runbooks voor specifieke of algemene functies, [onderliggende Runbooks](automation-child-runbooks.md).
---
title: Onderliggende runbooks in Azure Automation | Microsoft Docs
description: Beschrijft de verschillende methoden voor het starten van een runbook in Azure Automation vanuit een ander runbook en delen van gegevens tussen deze.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 919887b9-43e2-4c16-883c-f81807fe37db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 5c18444b5a2767ccdd9a61a3bc9218fa4c0aac04
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="child-runbooks-in-azure-automation"></a>Onderliggende runbooks in Azure Automation
Het is een best practice in Azure Automation schrijven herbruikbare, modulaire runbooks met een aparte functie die kan worden gebruikt door andere runbooks. Een bovenliggend runbook roept vaak een of meer onderliggende runbooks om uit te voeren van de vereiste functionaliteit. Er zijn twee manieren om aan te roepen, een onderliggend runbook en elke manier kent duidelijke verschillen die u moet begrijpen zodat u kunt bepalen wat de beste optie voor uw verschillende scenario's.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Aanroepen van een onderliggend runbook met inline-uitvoering
Als u wilt een runbook inline vanuit een ander runbook aanroepen, gebruikt u de naam van het runbook en geef waarden op voor de parameters precies zoals wanneer u een activiteit of cmdlet.  Alle runbooks in dezelfde Automation-account zijn beschikbaar voor alle andere moet op deze manier worden gebruikt. Het bovenliggende runbook wacht tot het onderliggende runbook om te voltooien voordat u doorgaat naar de volgende regel, en eventuele uitvoer rechtstreeks aan het bovenliggende object wordt geretourneerd.

Als u een runbook inline aanroept, deze wordt uitgevoerd in dezelfde taak als het bovenliggende runbook. Er zijn geen vermelding in de taakgeschiedenis van het onderliggende runbook die is uitgevoerd. Eventuele uitzonderingen en eventuele Stroomuitvoer van het onderliggende runbook worden gekoppeld aan het bovenliggende item zijn. Dit resulteert in minder taken en zorgt ervoor dat ze gemakkelijker om bij te houden en op te lossen omdat eventuele uitzonderingen worden veroorzaakt door het onderliggende runbook en de Stroomuitvoer gekoppeld aan de bovenliggende taak zijn.

Wanneer een runbook wordt gepubliceerd, moet alle onderliggende runbooks die daardoor worden aangeroepen al zijn gepubliceerd. Dit is omdat Azure Automation een koppeling met onderliggende runbooks maakt wanneer een runbook wordt gecompileerd. Als ze niet, is het bovenliggende runbook correct wordt gepubliceerd wordt weergegeven, maar wordt een uitzondering gegenereerd wanneer deze wordt gestart. Als dit gebeurt, kunt u het bovenliggende runbook opnieuw publiceren om correct wordt verwezen naar de onderliggende runbooks. U hoeft niet naar het bovenliggende runbook opnieuw publiceren als een van de onderliggende runbooks worden gewijzigd omdat de koppeling wordt al zijn gemaakt.

Elk gegevenstype met inbegrip van complexe objecten kunnen worden door de parameters van een onderliggend runbook dat inline wordt aangeroepen en er is geen [JSON-serialisatie](automation-starting-a-runbook.md#runbook-parameters) omdat er bij het starten van het runbook met de Azure portal of met de De cmdlet start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Runbooktypen
Welke typen kunnen bellen elkaar:

* Een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) en [grafische runbooks](automation-runbook-types.md#graphical-runbooks) elke andere inline (beide zijn op basis van PowerShell) kunt aanroepen.
* Een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks) en grafische PowerShell Workflow-runbooks, kunnen elke andere inline (beide zijn op basis van PowerShell-werkstroom) aangeroepen.
* De PowerShell-typen en de typen PowerShell Workflow elkaar inline kunnen niet worden aangeroepen en Start AzureRmAutomationRunbook moeten gebruiken.

Wanneer volgorde zaak wordt gepubliceerd:

* De volgorde van het publiceren van runbooks is alleen van belang voor PowerShell-werkstromen en grafische PowerShell Workflow-runbooks.

Wanneer u een grafisch of PowerShell-werkstroom onderliggend runbook met inline-uitvoering aanroepen, gebruikt u alleen de naam van het runbook.  Wanneer u een PowerShell-onderliggend runbook aanroepen, moet u dezelfde naam hebben als voorafgegaan *.\\*  om op te geven dat het script in de lokale map bevinden zich. 

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld wordt een onderliggend testrunbook dat drie parameters, een complex object, een geheel getal en een Boolean-waarde accepteert. De uitvoer van het onderliggende runbook is toegewezen aan een variabele.  In dit geval wordt is het onderliggende runbook een PowerShell Workflow-runbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Hier volgt het hetzelfde voorbeeld met een PowerShell-runbook als het onderliggende object.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true


## <a name="starting-a-child-runbook-using-cmdlet"></a>Een onderliggend runbook met behulp van de cmdlet starten
U kunt de [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) cmdlet een runbook starten, zoals beschreven in [een runbook starten met Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Er zijn twee modi voor deze cmdlet.  De cmdlet retourneert de taak-id in één modus zodra de onderliggende taak is gemaakt voor het onderliggende runbook.  In de andere modus, waarbij u door het opgeven van inschakelen de **-wacht** parameter, de cmdlet gewacht tot de onderliggende taak is voltooid en de uitvoer van het onderliggende runbook wordt geretourneerd.

De taak van een onderliggend runbook dat is gestart met een cmdlet wordt uitgevoerd in een afzonderlijke taak van het bovenliggende runbook. Dit resulteert in meer taken dan het runbook inline wordt aangeroepen en maakt ze moeilijker te volgen. De bovenliggende kan meerdere onderliggende runbooks asynchroon starten zonder te wachten op elk daarvan is voltooid. Voor dezelfde soort parallelle uitvoering de onderliggende runbooks inline aanroepen, moet het bovenliggende runbook gebruiken de [parallelle sleutelwoord](automation-powershell-workflow.md#parallel-processing).

Parameters voor een onderliggend runbook dat is gestart met een cmdlet worden opgegeven als hashtabel, zoals beschreven in [Runbookparameters](automation-starting-a-runbook.md#runbook-parameters). Alleen eenvoudige gegevenstypen kunnen worden gebruikt. Als het runbook een parameter met een complex gegevenstype heeft, moet klikt u vervolgens het worden aangeroepen inline.

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld wordt een onderliggend runbook met parameters gestart en wordt er gewacht totdat deze is voltooid met behulp van de Start-AzureRmAutomationRunbook-parameter wacht. Zodra voltooid, wordt de uitvoer van het onderliggende runbook verzameld.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResourceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Vergelijking van methoden voor het aanroepen van een onderliggend runbook
De volgende tabel geeft een overzicht van de verschillen tussen de twee methoden voor het aanroepen van een runbook vanuit een ander runbook.

|  | Inline | Cmdlet |
|:--- |:--- |:--- |
| Job |Onderliggende runbooks worden uitgevoerd in dezelfde taak als het bovenliggende item. |Een afzonderlijke taak is voor het onderliggende runbook gemaakt. |
| Uitvoering |Bovenliggend runbook wacht tot het onderliggende runbook om te voltooien voordat u doorgaat. |Bovenliggend runbook gaat direct verder nadat onderliggend runbook is gestart *of* bovenliggend runbook wacht tot de onderliggende taak is voltooid. |
| Uitvoer |Bovenliggend runbook kan uitvoer rechtstreeks ophalen van onderliggend runbook. |Bovenliggend runbook moet uitvoer ophalen uit taak van onderliggend runbook *of* bovenliggend runbook kan uitvoer rechtstreeks ophalen van onderliggend runbook. |
| Parameters |Waarden voor de parameters van onderliggend runbook worden afzonderlijk opgegeven en elk gegevenstype kunnen gebruiken. |Waarden voor de parameters van onderliggend runbook moeten worden gecombineerd in één hashtabel en kunnen alleen bestaan uit eenvoudig, matrix en object-gegevenstypen die gebruiken voor de JSON-serialisatie. |
| Automation-account |Bovenliggend runbook kan alleen onderliggend runbook in de dezelfde automation-account gebruiken. |Bovenliggend runbook kunt onderliggend runbook vanuit een automation-account van de dezelfde Azure-abonnement en zelfs een ander abonnement gebruiken als u een verbinding mee hebt. |
| Publiceren |Onderliggend runbook moet worden gepubliceerd voordat bovenliggend runbook wordt gepubliceerd. |Onderliggend runbook moet worden gepubliceerd voordat bovenliggend runbook wordt gestart. |

## <a name="next-steps"></a>Volgende stappen
* [Een runbook starten in Azure Automation](automation-starting-a-runbook.md)
* [Runbookuitvoer en -berichten in Azure Automation](automation-runbook-output-and-messages.md)


---
title: Onderliggende runbooks in Azure Automation
description: Beschrijft de verschillende methoden voor het starten van een runbook in Azure Automation vanuit een ander runbook en delen van gegevens tussen deze.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 37cf44e2c9d28b1aac8f2ab80ba29d126fb8651f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54422965"
---
# <a name="child-runbooks-in-azure-automation"></a>Onderliggende runbooks in Azure Automation

Het wordt aanbevolen in Azure Automation schrijven herbruikbare, modulaire runbooks met een aparte functie die door andere runbooks. Een bovenliggend runbook roept vaak een of meer onderliggende runbooks om uit te voeren van de vereiste functionaliteit. Er zijn twee manieren om aan te roepen een onderliggend runbook en elke manier kent duidelijke verschillen die u moet begrijpen zodat u kunt bepalen welke wordt aanbevolen voor uw verschillende scenario's.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Aanroepen van een onderliggend runbook met inline-uitvoering

Als u wilt een runbook inline vanuit een ander runbook aanroepen, gebruikt u de naam van het runbook en geef waarden op voor de parameters ervan precies zoals wanneer u een activiteit of cmdlet.  Alle runbooks in het hetzelfde Automation-account zijn beschikbaar voor alle overige moet worden gebruikt op deze manier. Het bovenliggende runbook wacht tot het onderliggende runbook gereed is voordat het naar de volgende regel en eventuele uitvoer rechtstreeks naar het bovenliggende object wordt geretourneerd.

Als u een runbook inline aanroept, wordt deze uitgevoerd in dezelfde taak als het bovenliggende runbook. Er is geen aanwijzing in de taakgeschiedenis van het onderliggende runbook die is uitgevoerd. Eventuele uitzonderingen en eventuele Stroomuitvoer van het onderliggende runbook is gekoppeld aan het bovenliggende item. Dit gedrag resulteert in minder taken en maakt deze gemakkelijker om bij te houden en op te lossen omdat eventuele uitzonderingen die door het onderliggende runbook en een van de stream-uitvoer zijn gekoppeld aan de bovenliggende taak.

Wanneer een runbook wordt gepubliceerd, moeten alle onderliggende runbooks die daardoor worden aangeroepen al worden gepubliceerd. Dit is omdat Azure Automation een koppeling met onderliggende runbooks maakt wanneer een runbook wordt gecompileerd. Als ze niet, is het bovenliggende runbook publiceren correct wordt weergegeven, maar een uitzondering wordt gegenereerd wanneer deze wordt gestart. Als dit het geval is, kunt u het bovenliggende runbook voor het correct wordt verwezen naar de onderliggende runbooks opnieuw publiceren. U hoeft niet naar het bovenliggende runbook opnieuw publiceren als een van de onderliggende runbooks is gewijzigd, omdat de koppeling is al gemaakt.

De parameters van een onderliggend runbook dat inline wordt aangeroepen, kunnen elk gegevenstype met inbegrip van complexe objecten zijn. Er is geen [JSON-serialisatie](automation-starting-a-runbook.md#runbook-parameters) omdat er bij het starten van het runbook met behulp van de Azure portal of met de cmdlet Start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Runbooktypen

Welke typen kunnen elkaar aanroepen:

* Een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) en [grafische runbooks](automation-runbook-types.md#graphical-runbooks) elke andere inline (beide zijn op basis van PowerShell) kunt aanroepen.
* Een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks) en grafische PowerShell Workflow-runbooks kunnen elke andere inline (beide zijn op basis van PowerShell-werkstroom) aangeroepen
* De PowerShell-typen en de typen PowerShell-werkstroom elkaar inline kunnen niet worden aangeroepen en Start-AzureRmAutomationRunbook moeten gebruiken.

Wanneer volgorde zaak wordt gepubliceerd:

* De volgorde van het publiceren van runbooks is alleen belangrijk voor PowerShell-werkstromen en grafische PowerShell Workflow-runbooks.

Wanneer u een grafische of PowerShell-werkstroom onderliggend runbook met inline-uitvoering aanroepen, gebruikt u de naam van het runbook.  Wanneer u een PowerShell onderliggend runbook aanroepen, moet u beginnen met de naam ervan *.\\*  om op te geven dat het script in de lokale map bevindt zich.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een onderliggend testrunbook dat drie parameters, een complex object, een geheel getal zijn en een Boolean-waarde accepteert. De uitvoer van het onderliggende runbook is toegewezen aan een variabele.  In dit geval is het onderliggende runbook een PowerShell Workflow-runbook.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Hieronder volgt het hetzelfde voorbeeld met behulp van een PowerShell-runbook als het onderliggende object.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>Een onderliggend runbook met behulp van cmdlet starten

> [!IMPORTANT]
> Als u bij het aanroepen van een onderliggend runbook met de `Start-AzureRmAutomationRunbook` cmdlet met de `-Wait` switch en de resultaten van het onderliggende runbook is een object, kan er fouten optreden. Als tijdelijke oplossing voor de fout, Zie [onderliggende runbooks met uitvoer van object](troubleshoot/runbooks.md#child-runbook-object) voor informatie over het implementeren van de logica voor het peilen van de resultaten en gebruik de [Get-AzureRmAutomationJobOutputRecord](/powershell/module/azurerm.automation/get-azurermautomationjoboutputrecord)

U kunt de [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) cmdlet een runbook starten, zoals beschreven in [een runbook starten met Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Er zijn twee modi voor deze cmdlet.  De cmdlet retourneert de taak-id in de modus voor één, wanneer de onderliggende taak voor het onderliggende runbook is gemaakt.  In de andere modus, waarbij u door op te geven inschakelen de **-wacht** parameter, de cmdlet wordt er gewacht tot de onderliggende taak is voltooid en de uitvoer van het onderliggende runbook wordt.

De taak van een onderliggend runbook gestart met een cmdlet wordt uitgevoerd in een afzonderlijke taak van het bovenliggende runbook. Dit gedrag resulteert in meer taken dan het starten van de runbook-inline en zorgt ervoor dat ze moeilijker om bij te houden. De bovenliggende kunt meer dan één onderliggend runbook asynchroon starten zonder te wachten op elk om te voltooien. Voor dezelfde soort parallelle uitvoering de onderliggende runbooks inline aanroept, moet het bovenliggende runbook gebruiken de [parallelle sleutelwoord](automation-powershell-workflow.md#parallel-processing).

De uitvoer van de onderliggende runbooks worden niet geretourneerd naar het bovenliggende runbook betrouwbaar vanwege timing. Ook bepaalde variabelen, zoals $VerbosePreference, $WarningPreference, en anderen niet kunnen worden doorgegeven aan de onderliggende runbooks. Om te voorkomen dat deze problemen, kunt u de onderliggende runbooks starten als afzonderlijke Automation-taken met behulp van de `Start-AzureRmAutomationRunbook` cmdlet met de `-Wait` overschakelen. Het bovenliggende runbook geblokkeerd totdat het onderliggende runbook voltooid is.

Als u niet dat het bovenliggende runbook wordt geblokkeerd op wachten wilt, kunt u beginnen het onderliggende runbook met behulp van `Start-AzureRmAutomationRunbook` cmdlet zonder de `-Wait` overschakelen. U moet gebruiken `Get-AzureRmAutomationJob` na afloop van de taak is voltooid, en `Get-AzureRmAutomationJobOutput` en `Get-AzureRmAutomationJobOutputRecord` om op te halen van de resultaten.

Parameters voor een onderliggend runbook gestart met een cmdlet worden opgegeven als hashtabel, zoals beschreven in [Runbookparameters](automation-starting-a-runbook.md#runbook-parameters). Alleen eenvoudige gegevenstypen kunnen worden gebruikt. Als het runbook een parameter met een complex gegevenstype heeft, klikt u vervolgens het moet dit inline worden aangeroepen.

De context van het abonnement verloren zijn gegaan bij het starten van de onderliggende runbooks als afzonderlijke taken. In de volgorde voor het onderliggende runbook voor het uitvoeren van Azure RM-cmdlets op basis van een specifieke Azure-abonnement, moet het onderliggende runbook worden geverifieerd met dit abonnement onafhankelijk van het bovenliggende runbook.

Als de taken binnen hetzelfde Automation-account met meer dan één abonnement werkt, kan een abonnement te selecteren in één taak de context van het geselecteerde abonnement voor andere taken wijzigen. Gebruiken om te voorkomen dat dit probleem, `Disable-AzureRmContextAutosave –Scope Processsave` aan het begin van elke runbook. Deze actie wordt de context alleen opgeslagen die runbook-uitvoering.

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een onderliggend runbook met parameters gestart en wacht dan tot deze is voltooid met behulp van de Start-AzureRmAutomationRunbook-parameter wachten. Zodra het is voltooid, wordt de uitvoer van het onderliggende runbook verzameld. Gebruik `Start-AzureRmAutomationRunbook`, moet worden geverifieerd met uw Azure-abonnement.

```azurepowershell-interactive
# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -DefaultProfile $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Vergelijking van methoden voor het aanroepen van een onderliggend runbook

De volgende tabel geeft een overzicht van de verschillen tussen de twee methoden voor het aanroepen van een runbook vanuit een ander runbook.

|  | Inline | Cmdlet |
|:--- |:--- |:--- |
| Taak |Onderliggende runbooks worden uitgevoerd in dezelfde taak als het bovenliggende item. |Een afzonderlijke taak is voor het onderliggende runbook gemaakt. |
| Kan worden uitgevoerd |Bovenliggend runbook wacht tot het onderliggende runbook om te voltooien voordat u doorgaat. |Bovenliggend runbook gaat direct verder nadat onderliggend runbook is gestart *of* bovenliggend runbook wacht tot de onderliggende taak is voltooid. |
| Uitvoer |Bovenliggend runbook kan uitvoer rechtstreeks van onderliggend runbook ophalen. |Bovenliggend runbook moet uitvoer ophalen uit taak van onderliggend runbook *of* bovenliggend runbook kan uitvoer rechtstreeks van onderliggend runbook ophalen. |
| Parameters |Waarden voor de parameters van onderliggend runbook worden afzonderlijk opgegeven en elk gegevenstype kunnen gebruiken. |Waarden voor de parameters van onderliggend runbook moeten worden gecombineerd in één hashtabel. Deze hash-tabel kan alleen opnemen eenvoudig, matrix en gegevenstypen die gebruikmaken van JSON-serialisatie-object. |
| Automation-account |Bovenliggend runbook kunt onderliggend runbook alleen gebruiken in het hetzelfde automation-account. |Bovenliggende runbooks kunt een onderliggend runbook vanuit een automation-account uit hetzelfde Azure-abonnement en zelfs een ander abonnement dat u een verbinding met hebt gebruiken. |
| Publiceren |Onderliggend runbook moet worden gepubliceerd voordat bovenliggend runbook wordt gepubliceerd. |Onderliggend runbook moet worden gepubliceerd op elk moment voordat bovenliggend runbook wordt gestart. |

## <a name="next-steps"></a>Volgende stappen

* [Een runbook starten in Azure Automation](automation-starting-a-runbook.md)
* [Runbookuitvoer en -berichten in Azure Automation](automation-runbook-output-and-messages.md)


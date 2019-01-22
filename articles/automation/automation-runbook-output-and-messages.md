---
title: Runbookuitvoer en -berichten in Azure Automation
description: Beschrijving van het maken en uitvoer en fout bij ophalen van berichten van runbooks in Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 82382ecc3adf0d0621f51438a082f7807b031fc9
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431211"
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Runbookuitvoer en -berichten in Azure Automation
De meeste Azure Automation-runbooks hebben een vorm van uitvoer. Deze uitvoer mogelijk een foutbericht aan de gebruiker of een complex object dat u wilt gebruiken met een ander runbook. Windows PowerShell biedt [meerdere streams](/powershell/module/microsoft.powershell.core/about/about_redirection) voor het verzenden van uitvoer van een script of een werkstroom. Azure Automation werkt anders met elk van deze stromen. U moet volgen aanbevolen procedures voor het gebruik bij het maken van een runbook.

De volgende tabel bevat een korte beschrijving van elk van de stromen en hun gedrag in de Azure-portal voor gepubliceerde runbooks en wanneer [testen van een runbook](automation-testing-runbook.md). In latere secties vindt u meer informatie over elke stroom.

| Stream | Description | Gepubliceerd | Test |
|:--- |:--- |:--- |:--- |
| Uitvoer |Objecten die zijn bedoeld om te worden verbruikt door andere runbooks. |Naar de taakgeschiedenis geschreven. |In het deelvenster Testuitvoer weergegeven. |
| Waarschuwing |Waarschuwingsbericht bedoeld voor de gebruiker. |Naar de taakgeschiedenis geschreven. |In het deelvenster Testuitvoer weergegeven. |
| Fout |Foutbericht bedoeld voor de gebruiker. In tegenstelling tot een uitzondering blijft het runbook standaard na een foutbericht weergegeven. |Naar de taakgeschiedenis geschreven. |In het deelvenster Testuitvoer weergegeven. |
| Uitgebreid |Berichten die algemene of foutopsporing informatie geven. |Naar de taakgeschiedenis geschreven alleen als uitgebreide logboekregistratie is ingeschakeld voor het runbook. |In het deelvenster Testuitvoer weergegeven alleen als $VerbosePreference is ingesteld om verder te gaan in het runbook. |
| Voortgang |Records automatisch worden gegenereerd voor en na elke activiteit in het runbook. Het runbook mag niet probeert te maken van een eigen voortgangsrecords omdat ze zijn bedoeld voor een interactieve gebruiker. |Naar de taakgeschiedenis alleen geschreven als voortgangslogboekregistratie is ingeschakeld voor het runbook. |Niet weergegeven in het deelvenster Testuitvoer. |
| fouten opsporen |Berichten die zijn bedoeld voor een interactieve gebruiker. Mag niet worden gebruikt in runbooks. |Niet naar Taakgeschiedenis geschreven. |Niet naar het deelvenster Testuitvoer geschreven. |

## <a name="output-stream"></a>Uitvoerstroom
De uitvoerstroom is bedoeld voor de uitvoer van objecten die zijn gemaakt door een script of een werkstroom wanneer deze correct wordt uitgevoerd. Deze stroom wordt in Azure Automation wordt voornamelijk gebruikt voor objecten die zijn bedoeld om te worden verbruikt door [bovenliggende runbooks die het huidige runbook aanroepen](automation-child-runbooks.md). Wanneer u [een runbook inline aanroept](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) van een bovenliggend runbook, retourneert deze gegevens uit de uitvoerstroom naar de bovenliggende. Gebruik de uitvoerstroom alleen om te communiceren van algemene informatie terug naar de gebruiker als u bekend bent met dat het runbook wordt nooit aangeroepen door een ander runbook. Als een best practice, echter, moet u doorgaans gebruiken de [uitgebreide Stream](#verbose-stream) om algemene informatie voor de gebruiker te communiceren.

U kunt gegevens schrijven naar de uitvoer stream via [Write-Output](https://technet.microsoft.com/library/hh849921.aspx) of door het plaatsen van het object op een eigen regel in het runbook.

```PowerShell
#The following lines both write an object to the output stream.
Write-Output –InputObject $object
$object
```

### <a name="output-from-a-function"></a>Uitvoer van een functie
Bij het schrijven naar de uitvoerstroom in een functie die opgenomen in uw runbook, wordt de uitvoer wordt doorgegeven aan het runbook. Als het runbook die uitvoer aan een variabele toewijst, is het niet geschreven naar de uitvoerstroom. Schrijven naar een andere stromen uit binnen de functie worden geschreven naar de bijbehorende stroom voor het runbook.

Bekijk het volgende voorbeeld:

```PowerShell
Workflow Test-Runbook
{
  Write-Verbose "Verbose outside of function" -Verbose
  Write-Output "Output outside of function"
  $functionOutput = Test-Function
  $functionOutput

  Function Test-Function
  {
    Write-Verbose "Verbose inside of function" -Verbose
    Write-Output "Output inside of function"
  }
}
```

De uitvoerstroom voor de runbooktaak zou zijn:

```output
Output inside of function
Output outside of function
```

De uitgebreide stroom voor de runbooktaak zou zijn:

```output
Verbose outside of function
Verbose inside of function
```

Nadat u het runbook hebt gepubliceerd en voordat u start de service, moet u ook uitgebreide logboekregistratie in de runbook-instellingen om op te halen van de uitvoer uitgebreide stroom inschakelen.

### <a name="declaring-output-data-type"></a>Declarerende uitvoer-gegevenstype
Een werkstroom kan het gegevenstype van het gebruik van de uitvoer geeft de [OutputType kenmerk](https://technet.microsoft.com/library/hh847785.aspx). Dit kenmerk heeft geen effect tijdens runtime, maar biedt een indicatie voor de runbookauteur van het tijdens het ontwerpen van de verwachte uitvoer van het runbook. Als de toolset voor runbooks zich ontwikkelen blijft, steeds het belang van gegevenstypen uitvoer declareren tijdens de ontwerpfase belangrijker. Als gevolg hiervan is het een aanbevolen procedure om op te nemen in de verklaring in runbooks die u maakt.

Hier volgt een lijst van voorbeeld van de uitvoertypen:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

Het volgende voorbeeldrunbook voert een tekenreeksobject en bevat een declaratie van het uitvoertype. Als uw runbook een matrix van een bepaald type, moet u nog steeds het type in plaats van een matrix van het type opgeven.

```PowerShell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

U declareert u een uitvoertype in grafische of grafische PowerShell Workflow-runbooks, kunt u de **invoer en uitvoer** menu-optie en typ de naam van het uitvoertype. Het verdient aanbeveling om dat u de volledige naam van de .NET-klasse gebruiken om het gemakkelijk kan worden geïdentificeerd wanneer ernaar wordt verwezen vanuit een bovenliggend runbook. Hiermee wordt aangegeven dat de eigenschappen van die klasse met de gegevensbus in het runbook en biedt veel flexibiliteit bij het gebruik van deze voor de voorwaardelijke logica, logboekregistratie en verwijst naar een als waarden voor andere activiteiten in het runbook.<br> ![Runbook-invoer en uitvoer optie](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

In het volgende voorbeeld hebt u twee grafische runbooks ter illustratie van deze functie. Als u op basis van de modulaire runbook toepast, hebt u een runbook dat als fungeert de *verificatie runbooksjabloon* verificatie beheren met Azure met behulp van de uitvoeren als-account. Onze tweede runbook, die normaal de kernlogica voor het automatiseren van een bepaald scenario kunt uitvoeren, wordt in dit geval gaan om uit te voeren de *verificatie runbooksjabloon* en de resultaten weer te geven uw **Test** het deelvenster Uitvoer. Onder normale omstandigheden hoeft u dit doen op basis van een resource gebruik te maken van de uitvoer van het onderliggende runbook runbook.

Hier volgt de basislogica van de **AuthenticateTo Azure** runbook.<br> ![Voorbeeld van de Runbook-sjabloon verifiëren](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Het bevat het uitvoertype *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, waarvan de verificatie-profieleigenschappen retourneert.<br> ![Voorbeeld van het Type Runbook-uitvoer](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Dit runbook is vrij eenvoudig, maar er is één configuratie-item voor het aanroepen van hier. De laatste activiteit wordt uitgevoerd de **Write-Output** cmdlet en schrijft de profielgegevens naar een $_-variabele die met behulp van een PowerShell-expressie voor de **Inputobject** parameter, die vereist voor deze cmdlet is.  

Voor het tweede runbook in dit voorbeeld met de naam *Test ChildOutputType*, hoeft u alleen twee activiteiten.<br> ![Voorbeeld van de onderliggende uitvoeren Type Runbook](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

Het aanroepen van de eerste activiteit de **AuthenticateTo Azure** runbook en de tweede activiteit wordt uitgevoerd de **Write-Verbose** cmdlet met de **gegevensbron** van  **Uitvoer van activiteit** en de waarde voor **pad naar veld** is **Context.Subscription.SubscriptionName**, die is opgeven van de uitvoer van de context van de  **AuthenticateTo Azure** runbook.<br> ![Write-Verbose cmdlet Parameter-gegevensbron](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

De resulterende uitvoer is de naam van het abonnement.<br> ![Test-ChildOutputType Runbook Results](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

Een opmerking over het gedrag van het Type uitvoer-besturingselement. Wanneer u een waarde typt in het veld Type uitvoer op de eigenschappenblade voor invoer en uitvoer, hebt u op buiten het besturingselement klikken nadat u deze in volgorde van uw inzending worden herkend door het besturingselement typt.  

## <a name="message-streams"></a>Berichtstromen
In tegenstelling tot de uitvoerstroom zijn berichtstromen bedoeld om informatie voor de gebruiker te communiceren. Er zijn meerdere berichtstromen voor verschillende soorten informatie en elk wordt verwerkt door Azure Automation.

### <a name="warning-and-error-streams"></a>Waarschuwings- en foutstromen
De waarschuwings- en foutstromen zijn bedoeld om aan te melden van problemen die optreden in een runbook. Ze worden naar de taakgeschiedenis geschreven wanneer een runbook wordt uitgevoerd, en zijn opgenomen in het deelvenster Testuitvoer in Azure portal wanneer een runbook wordt getest. Standaard kan het runbook zal worden uitgevoerd na een waarschuwing of fout. U kunt opgeven dat het runbook op een waarschuwing of fout moet worden onderbroken door in te stellen een [voorkeursvariabele](#preference-variables) in het runbook voordat het bericht wordt gemaakt. Bijvoorbeeld, als een runbook op te schorten bij een fout als een uitzondering, ingesteld **$ErrorActionPreference** te stoppen.

Maken van een waarschuwing of een foutbericht met de [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) of [Write-Error](https://technet.microsoft.com/library/hh849962.aspx) cmdlet. Activiteiten mogen ook naar deze stromen schrijven.

```PowerShell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning –Message "This is a warning message."
Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="verbose-stream"></a>Uitgebreide stroom
De uitgebreide berichtenstroom is bedoeld voor algemene informatie over de runbookwerking. Omdat de [foutopsporing Stream](#debug-stream) is niet beschikbaar in een runbook uitgebreide berichten moeten worden gebruikt voor gegevens voor foutopsporing. Uitgebreide berichten uit gepubliceerde runbooks worden standaard niet opgeslagen in de taakgeschiedenis. Voor het opslaan van uitgebreide berichten, configureert u gepubliceerde runbooks voor uitgebreide Records registreren op het tabblad configureren van het runbook in Azure portal. In de meeste gevallen moet u de standaardinstelling niet registreren van uitgebreide records voor een runbook uit prestatieoverwegingen behouden. Schakel deze optie alleen voor probleemoplossing of foutopsporing van een runbook.

Wanneer [testen van een runbook](automation-testing-runbook.md), uitgebreide berichten worden niet weergegeven, zelfs als het runbook is geconfigureerd voor logboekregistratie van uitgebreide records. Om weer te geven van uitgebreide berichten tijdens [testen van een runbook](automation-testing-runbook.md), moet u de variabele $VerbosePreference instellen om verder te gaan. Met deze variabele is ingesteld, worden uitgebreide berichten weergegeven in het deelvenster Testuitvoer van de Azure-portal.

Maak een uitgebreid bericht met de [Write-Verbose](https://technet.microsoft.com/library/hh849951.aspx) cmdlet.

```PowerShell
#The following line creates a verbose message.

Write-Verbose –Message "This is a verbose message."
```

### <a name="debug-stream"></a>Foutopsporingsstroom
De foutopsporingsstroom is bedoeld voor gebruik met een interactieve gebruiker en mag niet worden gebruikt in runbooks.

## <a name="progress-records"></a>Voortgangsrecords
Als u een runbook aan te melden voortgang records (op het tabblad configureren van het runbook in Azure portal), wordt een record wordt naar de taakgeschiedenis geschreven voordat en nadat elke activiteit is uitgevoerd. In de meeste gevallen moet u de standaardinstelling niet registreren van voortgangsrecords voor een runbook om prestaties te maximaliseren. Schakel deze optie alleen voor probleemoplossing of foutopsporing van een runbook. Bij het testen van een runbook worden voortgangsberichten niet weergegeven, zelfs als het runbook is geconfigureerd voor logboekregistratie van voortgangsrecords.

De [Write-Progress](https://technet.microsoft.com/library/hh849902.aspx) cmdlet is niet geldig in een runbook, omdat deze cmdlet is bedoeld voor gebruik met een interactieve gebruiker.

## <a name="preference-variables"></a>Voorkeursvariabelen
Maakt gebruik van Windows PowerShell [voorkeursvariabelen](https://technet.microsoft.com/library/hh847796.aspx) om te bepalen hoe om te reageren op gegevens die worden verzonden naar verschillende uitvoerstromen. U kunt deze variabelen instellen in een runbook om te bepalen hoe ze reageren op gegevens die worden verzonden naar verschillende stromen.

De volgende tabel bevat de voorkeursvariabelen die kunnen worden gebruikt in runbooks met hun geldige en standaardwaarden. Deze tabel bevat alleen de waarden die geldig in een runbook zijn. Er zijn aanvullende waarden geldig zijn voor de voorkeursvariabelen bij gebruik in Windows PowerShell buiten Azure Automation.

| Variabele | Standaardwaarde | Geldige waarden |
|:--- |:--- |:--- |
| WarningPreference |Doorgaan |Stoppen<br>Doorgaan<br>SilentlyContinue |
| ErrorActionPreference |Doorgaan |Stoppen<br>Doorgaan<br>SilentlyContinue |
| VerbosePreference |SilentlyContinue |Stoppen<br>Doorgaan<br>SilentlyContinue |

De volgende tabel geeft een lijst van het gedrag van de voorkeursvariabelewaarden die geldig in runbooks zijn.

| Waarde | Gedrag |
|:--- |:--- |
| Doorgaan |Registreert het bericht en blijft het runbook uitvoeren. |
| SilentlyContinue |Blijft het runbook uitvoeren zonder de logboekregistratie van het bericht. Deze waarde heeft het effect van het bericht wordt genegeerd. |
| Stoppen |Registreert het bericht en het runbook wordt onderbroken. |

## <a name="runbook-output"></a>Bij het ophalen van runbookuitvoer en -berichten
### <a name="azure-portal"></a>Azure Portal
U kunt de details van een runbooktaak weergeven in Azure portal vanaf het tabblad taken van een runbook. De samenvatting van de taak geeft de invoerparameters en de [uitvoer Stream](#output-stream) naast algemene informatie over de taak en eventuele uitzonderingen als ze zijn opgetreden. De geschiedenis bevat berichten van de [uitvoer Stream](#output-stream) en [waarschuwings- en Foutstromen](#warning-and-error-streams) , evenals de [uitgebreide Stream](#verbose-stream) en [Voortgangsrecords](#progress-records) als het runbook is geconfigureerd voor logboekregistratie van uitgebreide records en voortgangsrecords.

### <a name="windows-powershell"></a>Windows Powershell
In Windows PowerShell, kunt u uitvoer en berichten ophalen vanuit een runbook met de [Get-AzureAutomationJobOutput](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjoboutput) cmdlet. Deze cmdlet is vereist voor de ID van de taak en heeft een parameter genaamd Stream waarin u opgeeft welke stroom moet worden geretourneerd. U kunt opgeven **eventuele** om te retourneren van alle stromen voor de taak.

Het volgende voorbeeld wordt een voorbeeldrunbook gestart en wacht dan tot deze is voltooid. Zodra het is voltooid, wordt de uitvoerstroom verzameld van de taak.

```PowerShell
$job = Start-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

$doLoop = $true
While ($doLoop) {
  $job = Get-AzureRmAutomationJob -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
  $status = $job.Status
  $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

# For more detailed job output, pipe the output of Get-AzureRmAutomationJobOutput to Get-AzureRmAutomationJobOutputRecord
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Any | Get-AzureRmAutomationJobOutputRecord
``` 

### <a name="graphical-authoring"></a>Grafisch ontwerpen
Voor grafische runbooks is extra logboekregistratie beschikbaar in de vorm van activiteiten op serverniveau tracering. Er zijn twee niveaus van tracering: Basic en gedetailleerde. In de tracering in Basic ziet u de begin- en -tijd van elke activiteit in het runbook plus informatie met betrekking tot elke activiteit nieuwe pogingen. Enkele voorbeelden zijn, het aantal pogingen en de starttijd van de activiteit. In de gedetailleerde tracering krijgt u eenvoudige tracering plus invoer en uitvoer voor elke activiteit. De trace-records worden momenteel geschreven met behulp van de uitgebreide stroom, zodat u uitgebreide logboekregistratie wanneer u tracering inschakelen moet inschakelen. Voor grafische runbooks met tracering ingeschakeld, is er niet nodig voor logboekregistratie van voortgangsrecords. Basic tracering heeft hetzelfde doel en meer informatieve is.

![Grafisch ontwerpen taak streamt weergeven](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

U kunt zien in de voorgaande schermafbeelding dat wanneer u uitgebreide logboekregistratie en tracering voor grafische runbooks inschakelt, veel meer informatie is beschikbaar in de productie Taakstromen weergeven. Deze extra informatie kan essentieel zijn voor het oplossen van productieproblemen met een runbook, en daarom moet u alleen dit inschakelen voor dit doel en niet als een algemene praktijk. De Trace-records kunnen worden met name veel. Met een grafisch runbook tracering krijgt u twee tot vier records per activiteit, afhankelijk van of u de basisindeling of gedetailleerde tracering hebt geconfigureerd. Tenzij u deze informatie voor het bijhouden van de voortgang van een runbook voor het oplossen van nodig hebt, kunt u blijven tracering uitgeschakeld.

**Activiteitenniveau tracering wilt inschakelen, moet u de volgende stappen uitvoeren:**

1. Open uw Automation-account in Azure Portal.
2. Onder **procesautomatisering**, selecteer **Runbooks** om de lijst van runbooks te openen.
3. Klik op de pagina Runbooks om te selecteren van een grafisch runbook uit de lijst met runbooks.
4. Onder **instellingen**, klikt u op **logboekregistratie en tracering**.
5. Klik op de logboekregistratie en tracering van de pagina, onder de uitgebreide records registreren, **op** uitgebreide logboekregistratie inschakelen en wijzig het traceringsniveau naar onder activiteitenniveau tracering, **Basic** of **gedetailleerd** op basis van het niveau van de tracering die u nodig hebt.<br>
   
   ![Grafisch ontwerpen logboekregistratie en tracering van pagina](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-azure-log-analytics"></a>Microsoft Azure Log Analytics
Automation kunt runbook en taakstromen van een taak verzenden naar uw Log Analytics-werkruimte. U kunt de met Log Analytics,

* Krijg inzicht in uw Automation-taken 
* Trigger een e-mailadres of de waarschuwing op basis van uw runbook job status (voor bijvoorbeeld mislukte of onderbroken) 
* Geavanceerde query's schrijven over uw taakstromen 
* Combineer taken van Automation-accounts 
* Visualiseer uw Taakgeschiedenis na verloop van tijd    

Zie voor meer informatie over het configureren van integratie met Log Analytics te verzamelen, correleren en reageren op Taakgegevens [taakstatus en taakstromen van Automation doorsturen naar Log Analytics](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Volgende stappen
* Zie [Track a runbook job (Runbooktaken bijhouden)](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details
* Zie voor meer informatie over het ontwerpen en gebruiken van onderliggende runbooks, [onderliggende runbooks in Azure Automation](automation-child-runbooks.md)



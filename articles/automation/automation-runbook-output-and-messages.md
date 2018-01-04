---
title: Runbookuitvoer en -berichten in Azure Automation | Microsoft Docs
description: Beschrijving van het maken en uitvoer en fout bij ophalen van berichten van runbooks in Azure Automation.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 13a414f5-0e2c-4be2-9558-a3e3ec84b6b2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 415eddaec9702a42ceee51858a39840fcd6a202b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Runbookuitvoer en -berichten in Azure Automation
De meeste Azure Automation-runbooks hebben een vorm van uitvoer, zoals een foutbericht voor de gebruiker of een complex object bedoeld om te worden verbruikt door een andere werkstroom. Windows PowerShell biedt [meerdere streams](http://blogs.technet.com/heyscriptingguy/archive/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell.aspx) voor het verzenden van uitvoer vanuit een script of een werkstroom. Azure Automation anders werkt met elk van deze stromen en u moet volgen aanbevolen procedures voor het gebruik van elk tijdens het maken van een runbook.

De volgende tabel bevat een korte beschrijving van elk van de stromen en hun gedrag in de Azure-portal bij het uitvoeren van een gepubliceerd runbook en wanneer [testen van een runbook](automation-testing-runbook.md). In de volgende secties vindt u meer informatie over elke stroom.

| Stream | Beschrijving | Gepubliceerd | Testen |
|:--- |:--- |:--- |:--- |
| Uitvoer |Objecten die zijn bedoeld om te worden verbruikt door andere runbooks. |Naar de taakgeschiedenis geschreven. |In het deelvenster Testuitvoer weergegeven. |
| Waarschuwing |Waarschuwingsbericht bedoeld voor de gebruiker. |Naar de taakgeschiedenis geschreven. |In het deelvenster Testuitvoer weergegeven. |
| Fout |Foutbericht bedoeld voor de gebruiker. In tegenstelling tot een uitzondering blijft het runbook standaard na een foutbericht weergegeven. |Naar de taakgeschiedenis geschreven. |In het deelvenster Testuitvoer weergegeven. |
| Uitgebreid |Berichten algemene of foutopsporing informatie opgeeft. |Naar Taakgeschiedenis geschreven als uitgebreide logboekregistratie is ingeschakeld voor het runbook. |In het deelvenster Testuitvoer weergegeven alleen als $VerbosePreference is ingesteld op Doorgaan in het runbook. |
| Voortgang |Records automatisch worden gegenereerd voor en na elke activiteit in het runbook. Het runbook moet niet proberen te maken van een eigen voortgangsrecords omdat deze zijn bedoeld voor een interactieve gebruiker. |Naar de taakgeschiedenis geschreven als voortgangslogboekregistratie is ingeschakeld voor het runbook. |Niet weergegeven in het deelvenster Testuitvoer. |
| Fouten opsporen |Berichten die zijn bedoeld voor een interactieve gebruiker. Mag niet worden gebruikt in runbooks. |Niet naar Taakgeschiedenis geschreven. |Niet naar het deelvenster Testuitvoer geschreven. |

## <a name="output-stream"></a>Uitvoerstroom
De uitvoerstroom is bedoeld voor de uitvoer van objecten die zijn gemaakt door een script of een werkstroom wanneer deze correct wordt uitgevoerd. In Azure Automation wordt deze stroom voornamelijk gebruikt voor objecten die zijn bedoeld om te worden verbruikt door [bovenliggende runbooks die het huidige runbook aanroepen](automation-child-runbooks.md). Wanneer u [een runbook inline aanroept](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) van een bovenliggend runbook, retourneert deze gegevens uit de uitvoerstroom naar de bovenliggende. Gebruik de uitvoerstroom alleen om te communiceren algemene informatie terug naar de gebruiker als u weet dat het runbook wordt nooit aangeroepen door een ander runbook. Als een best practice, echter, moet u doorgaans gebruiken de [uitgebreide stroom](#Verbose) om algemene informatie voor de gebruiker te communiceren.

U kunt gegevens schrijven naar de uitvoer-stream via [Write-Output](http://technet.microsoft.com/library/hh849921.aspx) of door het plaatsen van het object op een aparte regel in het runbook.

    #The following lines both write an object to the output stream.
    Write-Output –InputObject $object
    $object

### <a name="output-from-a-function"></a>Uitvoer van een functie
Wanneer u naar de uitvoerstroom in een functie die is opgenomen in uw runbook schrijft, wordt de uitvoer terug naar het runbook doorgegeven. Als het runbook die uitvoer aan een variabele toewijst, is het niet geschreven naar de uitvoerstroom. Schrijven naar andere stromen vanuit de functie schrijft naar de bijbehorende stroom voor het runbook.

Houd rekening met het volgende voorbeeldrunbook:

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


De uitvoerstroom voor de runbooktaak zou zijn:

    Output inside of function
    Output outside of function

De uitgebreide stroom voor de runbooktaak zou zijn:

    Verbose outside of function
    Verbose inside of function

Eenmaal hebt u het runbook gepubliceerd en voordat u deze, u moet ook uitgebreide logboekregistratie inschakelen in de runbookinstellingen om op te halen van de uitvoer van de uitgebreide stroom.

### <a name="declaring-output-data-type"></a>Declarerende uitvoer-gegevenstype
Een werkstroom kunt opgeven welk gegevenstype van het gebruik van de uitvoer de [OutputType kenmerk](http://technet.microsoft.com/library/hh847785.aspx). Dit kenmerk heeft geen effect tijdens runtime, maar het biedt een indicatie voor de runbookauteur van het tijdens het ontwerpen van de verwachte uitvoer van het runbook. Als de toolset voor runbooks zich ontwikkelen blijft, steeds het belang van uitvoergegevenstypen in de ontwerpfase belangrijker. Als gevolg hiervan wordt het aanbevolen deze declaratie opnemen in runbooks die u maakt.

Hier volgt een lijst van voorbeeld van uitvoer typen:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

Het volgende voorbeeldrunbook voert een tekenreeksobject en bevat een declaratie van het uitvoertype. Als uw runbook een matrix van een bepaald type, moet u nog steeds het type in plaats van een matrix van het type opgeven.

    Workflow Test-Runbook
    {
       [OutputType([string])]

       $output = "This is some string output."
       Write-Output $output
    }

Voor het declareren van een uitvoertype in grafisch of grafische PowerShell Workflow-runbooks, kunt u de **invoer en uitvoer** menuoptie en typ de naam van het uitvoertype. Het is raadzaam om dat u de volledige naam van de .NET-klasse gebruiken om het gemakkelijk te identificeren wanneer verwijzen vanaf een bovenliggend runbook. Dit beschrijft de eigenschappen van die klasse naar de gegevensbus in het runbook en biedt een grote flexibiliteit bij het gebruik ervan voor voorwaardelijke logica, logboekregistratie en het verwijzen naar als waarden voor andere activiteiten in het runbook.<br> ![Runbook-invoer en uitvoer-optie](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

In het volgende voorbeeld hebben we twee grafische runbooks ter illustratie van deze functie. Als we de ontwerpmodel modulaire runbook toepast, hebben we een runbook dat als fungeert de *verificatie runbooksjabloon* verificatie beheren met Azure met behulp van het Run As-account. Ons runbook tweede, die normaal de corelogica voor het automatiseren van een bepaald scenario kunt uitvoeren, wordt in dit geval gaan om uit te voeren de *verificatie runbooksjabloon* en de resultaten weer te geven uw **Test** deelvenster Uitvoer.  Onder normale omstandigheden hebben we dit runbook iets op basis van een resource die gebruik van de uitvoer van het onderliggende runbook doen.    

Hier volgt de basislogica van de **AuthenticateTo Azure** runbook.<br> ![Voorbeeld van de Runbook-sjabloon verifiëren](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Het omvat het uitvoertype *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, waarmee de verificatie profieleigenschappen wordt geretourneerd.<br> ![Voorbeeld van uitvoer van de Runbook-Type](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Dit runbook is rechtstreeks doorsturen, maar er is één configuratie-item aan te roepen hier. Het uitvoeren van de laatste activiteit het **Write-Output** cmdlet en schrijft de profielgegevens aan een $_ variabele met behulp van een PowerShell-expressie voor de **Inputobject** parameter, die vereist voor deze cmdlet is.  

Voor het tweede runbook in dit voorbeeld met de naam *Test ChildOutputType*, we gewoon twee activiteiten hebben.<br> ![Voorbeeld van de onderliggende uitvoer Type Runbook](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

Het aanroepen van de eerste activiteit de **AuthenticateTo Azure** runbook en de tweede activiteit wordt uitgevoerd de **Write-Verbose** cmdlet uit met de **gegevensbron** van **uitvoer van activiteit** en de waarde voor **pad naar veld** is **Context.Subscription.SubscriptionName**, die is opgeven van de uitvoer van de context van de **AuthenticateTo Azure** runbook.<br> ![Write-Verbose cmdlet Parameter-gegevensbron](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

De resulterende uitvoer is de naam van het abonnement.<br> ![Test ChildOutputType Runbookresultaten](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

Een opmerking over het gedrag van het Type uitvoer-besturingselement. Wanneer u een waarde in het veld Type van de uitvoer op de eigenschappenblade invoer en uitvoer typt, moet u op buiten het besturingselement, nadat u deze typen, zodat uw invoer worden herkend door het besturingselement.  

## <a name="message-streams"></a>Berichtstromen
In tegenstelling tot de uitvoerstroom zijn berichtstromen bedoeld om informatie naar de gebruiker te communiceren. Er zijn meerdere berichtstromen voor verschillende soorten informatie en elk wordt anders afgehandeld door Azure Automation.

### <a name="warning-and-error-streams"></a>Waarschuwings- en foutstromen
De waarschuwings- en foutstromen zijn bedoeld om de problemen die optreden in een runbook te melden. Ze worden naar de taakgeschiedenis geschreven wanneer een runbook wordt uitgevoerd en zijn opgenomen in het deelvenster Testuitvoer in de Azure portal wanneer een runbook wordt getest. Standaard blijft het runbook uitvoering na een waarschuwing of fout. U kunt opgeven dat het runbook op een waarschuwing of fout moet worden onderbroken door een [voorkeursvariabele](#PreferenceVariables) in het runbook voordat het maken van het bericht. Bijvoorbeeld, als een runbook onderbreken bij een fout als een uitzondering, stelt **$ErrorActionPreference** om te stoppen.

Maken van een waarschuwing of foutbericht met de [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) of [Write-Error](http://technet.microsoft.com/library/hh849962.aspx) cmdlet. Activiteiten mogen ook naar deze stromen schrijven.

    #The following lines create a warning message and then an error message that will suspend the runbook.

    $ErrorActionPreference = "Stop"
    Write-Warning –Message "This is a warning message."
    Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."

### <a name="verbose-stream"></a>Uitgebreide stroom
De uitgebreide berichtenstroom is bedoeld voor algemene informatie over de runbookwerking. Aangezien de [foutopsporingsstroom](#Debug) is niet beschikbaar in een runbook uitgebreide berichten moeten worden gebruikt voor gegevens voor foutopsporing. Standaard worden uitgebreide berichten uit gepubliceerde runbooks niet opgeslagen in de taakgeschiedenis. Voor het opslaan van uitgebreide berichten gepubliceerde runbooks voor uitgebreide Records in het logboek op het tabblad configureren van het runbook in de Azure-portal te configureren. In de meeste gevallen moet u de standaardinstelling van logboekregistratie van uitgebreide records voor een runbook uit prestatieoverwegingen niet behouden. Schakel deze optie alleen voor probleemoplossing of foutopsporing van een runbook.

Wanneer [testen van een runbook](automation-testing-runbook.md), uitgebreide berichten worden niet weergegeven, zelfs als het runbook is geconfigureerd voor logboekregistratie van uitgebreide records. Om weer te geven van uitgebreide berichten tijdens [testen van een runbook](automation-testing-runbook.md), moet u de variabele $VerbosePreference instellen om verder te gaan. Met deze variabele is ingesteld, worden uitgebreide berichten weergegeven in het deelvenster Testuitvoer van de Azure portal.

Maak een uitgebreid bericht met de [Write-Verbose](http://technet.microsoft.com/library/hh849951.aspx) cmdlet.

    #The following line creates a verbose message.

    Write-Verbose –Message "This is a verbose message."

### <a name="debug-stream"></a>Foutopsporingsstroom
De foutopsporingsstroom is bedoeld voor gebruik met een interactieve gebruiker en mag niet worden gebruikt in runbooks.

## <a name="progress-records"></a>Voortgangsrecords
Als u een runbook aan te melden voortgang registreert (op het tabblad configureren van het runbook in de Azure-portal), wordt een record naar de taakgeschiedenis geschreven voordat en nadat elke activiteit is uitgevoerd. In de meeste gevallen moet u de standaardinstelling van logboekregistratie van voortgangsrecords voor een runbook niet om de prestaties te behouden. Schakel deze optie alleen voor probleemoplossing of foutopsporing van een runbook. Bij het testen van een runbook worden voortgangsberichten niet weergegeven zelfs als het runbook is geconfigureerd voor logboekregistratie van voortgangsrecords.

De [Write-Progress](http://technet.microsoft.com/library/hh849902.aspx) cmdlet is niet geldig in een runbook, omdat deze is bedoeld voor gebruik met een interactieve gebruiker.

## <a name="preference-variables"></a>Voorkeursvariabelen
Maakt gebruik van Windows PowerShell [voorkeursvariabelen](http://technet.microsoft.com/library/hh847796.aspx) om te bepalen hoe gereageerd op gegevens die worden verzonden naar verschillende uitvoerstromen. U kunt deze variabelen instellen in een runbook om te bepalen hoe reageert op gegevens die worden verzonden naar verschillende stromen.

De volgende tabel bevat de voorkeursvariabelen die kunnen worden gebruikt in runbooks met hun geldige en standaardwaarden. Houd er rekening mee dat deze tabel alleen de waarden die geldig in een runbook zijn bevat. Er zijn aanvullende waarden geldig voor de voorkeursvariabelen bij gebruik in Windows PowerShell buiten Azure Automation.

| Variabele | Standaardwaarde | Geldige waarden |
|:--- |:--- |:--- |
| WarningPreference |Doorgaan |Stoppen<br>Doorgaan<br>SilentlyContinue |
| ErrorActionPreference |Doorgaan |Stoppen<br>Doorgaan<br>SilentlyContinue |
| VerbosePreference |SilentlyContinue |Stoppen<br>Doorgaan<br>SilentlyContinue |

De volgende tabel geeft een lijst van het gedrag van de voorkeursvariabelewaarden die geldig in runbooks zijn.

| Waarde | Gedrag |
|:--- |:--- |
| Doorgaan |Registreert het bericht en blijft het runbook uitvoeren. |
| SilentlyContinue |Blijft het runbook uitvoeren zonder de logboekregistratie van het bericht. Dit heeft het effect van het bericht wordt genegeerd. |
| Stoppen |Registreert het bericht en het runbook wordt onderbroken. |

## <a name="retrieving-runbook-output-and-messages"></a>Runbookuitvoer en -berichten ophalen
### <a name="azure-portal"></a>Azure Portal
U kunt de details van een runbooktaak weergeven in de Azure portal op het tabblad taken van een runbook. De samenvatting van de taak wordt weergegeven voor de invoerparameters en de [uitvoerstroom](#Output) naast algemene informatie over de taak en eventuele uitzonderingen als ze zich heeft voorgedaan. De geschiedenis bevat berichten van de [uitvoerstroom](#Output) en [waarschuwings- en Foutstromen](#WarningError) naast de [uitgebreide stroom](#Verbose) en [Voortgangsrecords](#Progress) als het runbook is geconfigureerd voor logboekregistratie van uitgebreide records en voortgangsrecords.

### <a name="windows-powershell"></a>Windows Powershell
In Windows PowerShell, kunt u uitvoer en berichten ophalen vanuit een runbook met de [Get-AzureAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) cmdlet. Deze cmdlet vereist de ID van de taak en heeft een parameter genaamd stroom waarin u opgeeft welke stroom moet worden geretourneerd. U kunt opgeven om te retourneren van alle stromen voor de taak.

Het volgende voorbeeld wordt een voorbeeldrunbook gestart en wordt er gewacht totdat deze is voltooid. Zodra het is voltooid, wordt de uitvoerstroom verzameld van de taak.

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
    

### <a name="graphical-authoring"></a>Grafisch ontwerpen
Voor grafische runbooks is extra logboekregistratie beschikbaar in de vorm van activiteitenniveau tracering.  Er zijn twee niveaus van tracering: Basic en gedetailleerde.  In Basic tracering ziet u de begintijd en eindtijd van elke activiteit in het runbook plus informatie met betrekking tot alle nieuwe pogingen van activiteit, zoals het aantal pogingen en de begintijd van de activiteit.  In de gedetailleerde tracering krijgt u Basic tracering plus invoer- en uitvoergegevens voor elke activiteit.  Houd er rekening mee dat momenteel de trace-records zijn geschreven met behulp van de uitgebreide stream, dus moet u uitgebreide logboekregistratie wanneer u tracering inschakelt inschakelen.  Er is logboekregistratie van voortgangsrecords, omdat de Basic tracering hetzelfde doel heeft en meer informatieve is niet nodig voor grafische runbooks met tracering is ingeschakeld.

![Grafisch ontwerpen taak Streams weergeven](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

U ziet van de bovenstaande schermafbeelding dat wanneer u uitgebreide logboekregistratie en tracering voor grafische runbooks inschakelt, veel meer informatie beschikbaar in de weergave van de taak stromen voor productie is.  Deze extra informatie kan essentieel zijn voor het oplossen van productieproblemen met een runbook, en daarom moet u alleen deze inschakelen voor dit doel en niet als over het algemeen. De Trace-records kunnen worden met name talrijke.  Met grafisch runbook tracering krijgt u twee tot vier records per activiteit, afhankelijk van of u de basisindeling of gedetailleerde tracering hebt geconfigureerd.  Tenzij u deze informatie om de voortgang van een runbook moet voor het oplossen van te volgen, u mogelijk wilt houden tracering uitgeschakeld.

**Activiteitenniveau tracering wilt inschakelen, moet u de volgende stappen uitvoeren.**

1. Open uw Automation-account in Azure Portal.
2. Klik op de tegel **Runbooks** om de lijst met runbooks te openen.
3. Klik op de blade Runbooks om te selecteren van een grafisch runbook uit de lijst met runbooks.
4. Klik op de blade instellingen voor het geselecteerde runbook **logboekregistratie en tracering**.
5. Klik op de logboekregistratie en tracering blade onder uitgebreide logboekrecords **op** uitgebreide logboekregistratie inschakelen en wijzig het traceerniveau naar onder activiteitenniveau tracering in, **Basic** of **gedetailleerd** op basis van het niveau van de tracering die u nodig hebt.<br>
   
   ![Grafisch ontwerpen logboekregistratie en tracering Blade](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-operations-management-suite-oms-log-analytics"></a>Microsoft Operations Management Suite (OMS) Log Analytics
Automation kan runbook taak status en taak streams verzenden naar de werkruimte voor logboekanalyse voor Microsoft Operations Management Suite (OMS).  U kunt met Log Analytics,

* Inzicht verkrijgen in uw Automation-taken 
* Trigger een e-mailadres of de waarschuwing op basis van de status van de taak runbook (bijvoorbeeld mislukte of onderbroken) 
* Geavanceerde query's in uw taak stromen schrijven 
* Taken correleren via Automation-accounts 
* De taakgeschiedenis visualiseren gedurende een periode    

Zie voor meer informatie over het configureren van integratie met logboekanalyse voor het verzamelen, correleren van en reageren op Taakgegevens [doorsturen taakstatus en taak stromen van Automation voor logboekanalyse (OMS)](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Volgende stappen
* Zie [Track a runbook job (Runbooktaken bijhouden)](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details
* Zie voor informatie over het ontwerpen en gebruiken van onderliggende runbooks, [onderliggende runbooks in Azure Automation](automation-child-runbooks.md)


---
title: Leren van PowerShell-werkstroom voor Azure Automation | Microsoft Docs
description: In dit artikel is bedoeld als een snelle les voor auteurs bekend zijn met PowerShell om te weten over de specifieke verschillen tussen PowerShell en PowerShell-werkstroom en concepten die van toepassing op de Automation-runbooks.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 84bf133e-5343-4e0e-8d6c-bb14304a70db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 90a8229b3d4974b8385039c7d85f916a168947d8
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Leren van de belangrijkste concepten voor Windows PowerShell-werkstroom voor Automation-runbooks 
Azure Automation-Runbooks worden geïmplementeerd als Windows PowerShell-werkstromen.  Een Windows PowerShell-werkstroom is vergelijkbaar met een Windows PowerShell-script, maar heeft enkele belangrijke verschillen die kunnen verwarrend zijn naar een nieuwe gebruiker.  Hoewel dit artikel is bedoeld om te schrijven met behulp van PowerShell-werkstroom runbooks, wordt u aangeraden dat u runbooks met behulp van PowerShell, tenzij u controlepunten moet schrijven.  Er zijn verschillende syntaxisverschillen bij het ontwerpen van PowerShell Workflow-runbooks en deze verschillen vereisen iets meer werk effectieve werkstromen te schrijven.  

Een werkstroom is een opeenvolging van geprogrammeerde, verbonden stappen waarmee langlopende taken uitvoeren of de coördinatie vereisen van meerdere stappen op meerdere apparaten of beheerde knooppunten. De voordelen van een werkstroom op een normaal script omvatten de mogelijkheid een actie tegen meerdere apparaten tegelijkertijd uitvoeren en de mogelijkheid om automatisch te herstellen van fouten. Een Windows PowerShell-werkstroom is een Windows PowerShell-script dat gebruikmaakt van Windows Workflow Foundation. Terwijl de werkstroom is geschreven met Windows PowerShell-syntaxis en gelanceerd door Windows PowerShell, wordt deze verwerkt door Windows Workflow Foundation.

Zie voor meer informatie over de onderwerpen in dit artikel [aan de slag met Windows PowerShell-werkstroom](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>Algemene structuur van een werkstroom
De eerste stap bij het converteren van een PowerShell-script naar een PowerShell-werkstroom is insluitende deze met de **werkstroom** sleutelwoord.  Een werkstroom begint met de **werkstroom** trefwoord gevolgd door de hoofdtekst van het script tussen accolades. De naam van de werkstroom volgt het **werkstroom** trefwoord zoals getoond in de volgende syntaxis:

    Workflow Test-Workflow
    {
       <Commands>
    }

De naam van de werkstroom moet overeenkomen met de naam van het Automation-runbook. Als het runbook wordt geïmporteerd, wordt de bestandsnaam moet overeenkomen met de Werkstroomnaam en moet eindigen op *.ps1*.

U kunt parameters toevoegen aan de werkstroom met de **Param** sleutelwoord net zoals een script.

## <a name="code-changes"></a>Codewijzigingen
PowerShell-werkstroom code ziet er bijna identiek aan de code van de PowerShell-script, met uitzondering van enkele belangrijke wijzigingen.  De volgende secties worden de wijzigingen die u moet aanbrengen in een PowerShell-script voor het in een werkstroom uit te voeren.

### <a name="activities"></a>Activiteiten
Een activiteit is een specifieke taak in een werkstroom. Net zoals een script uit een of meer opdrachten bestaat, bestaat een werkstroom uit een of meer activiteiten die worden uitgevoerd in een reeks. Windows PowerShell-werkstroom converteert automatisch veel van de Windows PowerShell-cmdlets voor activiteiten tijdens het uitvoeren van een werkstroom. Wanneer u een van deze cmdlets in uw runbook opgeeft, wordt de bijbehorende activiteit uitgevoerd door Windows Workflow Foundation. Voor die cmdlets zonder een bijbehorende activiteit Windows PowerShell Workflow voert automatisch de cmdlet uit binnen een [InlineScript](#inlinescript) activiteit. Er is een set cmdlets die zijn uitgesloten en kan niet worden gebruikt in een werkstroom, tenzij u ze expliciet in een InlineScript blok opneemt. Zie voor meer informatie over deze begrippen [met behulp van activiteiten in Script Workflows](http://technet.microsoft.com/library/jj574194.aspx).

Werkstroomactiviteiten delen een set met algemene parameters voor het configureren van hun werking. Zie voor meer informatie over de algemene werkstroomparameters [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Positionele parameters
U kunt positieparameters niet gebruiken met activiteiten en cmdlets in een werkstroom.  Dit betekent dat is dat u de namen van parameters moet gebruiken.

Neem bijvoorbeeld de volgende code waarmee alle actieve services opgehaald.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Als u deze code in een werkstroom uitvoert probeert, ontvangt u een bericht zoals "parameterset kan niet worden omgezet met behulp van de opgegeven benoemde parameters."  Geef de parameternaam zoals in het volgende om dit te corrigeren.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Gedeserialiseerde objecten
Objecten in werkstromen worden gedeserialiseerd.  Dit betekent dat de eigenschappen zijn nog steeds beschikbaar, maar niet de methoden.  Neem bijvoorbeeld de volgende PowerShell-code die een service met de methode stoppen van het object van de Service wordt gestopt.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Als u uitvoeren in een werkstroom wilt, ontvangt u een foutmelding verschijnt "de methodeaanroep wordt niet ondersteund in een Windows PowerShell-werkstroom."  

Een mogelijkheid is het inpakken van deze twee regels code in een [InlineScript](#inlinescript) blokkeren in dat geval $Service een serviceobject in het blok is.

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

Een andere optie is het gebruik van een andere cmdlet die dezelfde functionaliteit als de methode uitvoert als deze beschikbaar is.  In ons voorbeeld de cmdlet Stop-Service biedt dezelfde functionaliteit als de methode stoppen en u kunt het volgende gebruiken voor een werkstroom.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript
De **InlineScript** activiteit is handig wanneer u een of meer opdrachten uitvoeren als traditionele PowerShell-script in plaats van PowerShell-werkstroom.  Terwijl de opdrachten in een werkstroom voor verwerking naar de Windows Workflow Foundation worden verzonden, worden door Windows PowerShell-opdrachten in een InlineScript-blok verwerkt.

InlineScript gebruikt van de volgende syntaxis hieronder weergegeven.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Uitvoer kunt van een InlineScript u terugkeren door de uitvoer toe te wijzen aan een variabele. Het volgende voorbeeld een service stopt en vervolgens de naam van de service levert.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


U kunt waarden doorgeven in een InlineScript-blok, maar moet u **$Using** bereikaanpassingsfunctie.  Het volgende voorbeeld is identiek aan het vorige voorbeeld, behalve dat de servicenaam is opgegeven door een variabele.

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"

        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Tijdens het InlineScript-activiteiten mogelijk kritieke in bepaalde werkstromen, ze bieden geen ondersteuning voor werkstroom constructies en mag alleen worden gebruikt wanneer dat nodig is om de volgende redenen:

* U kunt geen gebruiken [controlepunten](#checkpoints) in een InlineScript-blok. Als er een fout optreedt in het blok, moet het worden hervat vanaf het begin van het blok.
* U kunt geen gebruiken [parallelle uitvoering](#parallel-processing) binnen een InlineScriptBlock.
* InlineScript dit beïnvloedt de schaalbaarheid van de werkstroom omdat deze de Windows PowerShell-sessie voor de hele lengte van het InlineScript-blok bevat.

Zie voor meer informatie over het gebruik van InlineScript [Windows PowerShell-opdrachten in een werkstroom](http://technet.microsoft.com/library/jj574197.aspx) en [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Parallelle verwerking
Een voordeel van het Windows PowerShell-werkstromen is de mogelijkheid voor het uitvoeren van een reeks opdrachten parallel in plaats van opeenvolgend net als bij een typische script.

U kunt de **parallelle** sleutelwoord voor het maken van een scriptblok is opgegeven met meerdere opdrachten die gelijktijdig worden uitgevoerd. Dit maakt gebruik van de volgende syntaxis hieronder weergegeven. In dit geval starten activiteit1 en activiteit2 op hetzelfde moment. Activiteit3 start pas als zowel activiteit1 en activiteit2 zijn voltooid.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Neem bijvoorbeeld het volgende PowerShell-opdrachten die meerdere bestanden naar een netwerkbestemming kopiëren.  Deze opdrachten worden opeenvolgend uitgevoerd zodanig dat één bestand met het kopiëren eindigen moet voordat de volgende wordt gestart.     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

De volgende werkstroom wordt deze opdrachten parallel uitgevoerd, zodat ze allemaal op hetzelfde moment kopiëren starten.  Pas nadat ze alle zijn wordt gekopieerd het voltooiingsbericht is weergegeven.

    Workflow Copy-Files
    {
        Parallel
        {
            Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


U kunt de **ForEach-Parallel** constructie om opdrachten proces voor elk item in een verzameling gelijktijdig te. De items in de verzameling worden parallel verwerkt, terwijl de opdrachten in het scriptblok worden opeenvolgend uitgevoerd. Dit maakt gebruik van de volgende syntaxis hieronder weergegeven. In dit geval starten activiteit1 op hetzelfde moment voor alle items in de verzameling. Voor elk item start activiteit2 nadat activiteit1 voltooid. Activiteit3 start pas als zowel activiteit1 en activiteit2 zijn voltooid voor alle items.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Het volgende voorbeeld is vergelijkbaar met het vorige voorbeeld parallel bestanden zijn gekopieerd.  In dit geval wordt een bericht weergegeven voor elk bestand nadat deze zijn gekopieerd.  Pas nadat ze alle zijn volledig gekopieerd is het uiteindelijke voltooiingsbericht weergegeven.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }

        Write-Output "All files copied."
    }

> [!NOTE]
> We raden niet onderliggende runbooks parallel uitgevoerd, omdat dit onbetrouwbare resultaten is weergegeven.  De uitvoer van het onderliggende runbook soms wordt niet weergegeven en instellingen in een onderliggend runbook kunnen invloed hebben op de andere parallelle onderliggende runbooks
>

## <a name="checkpoints"></a>Controlepunten
Een *controlepunt* is een momentopname van de huidige status van de werkstroom met de huidige waarde voor variabelen en eventuele gegenereerde uitvoer naar dat punt. Als een werkstroom in een fout eindigt of is onderbroken, wordt klikt u vervolgens de volgende keer dat deze wordt uitgevoerd gestart vanaf de laatste controlepunt in plaats van het begin van de werkstroom.  U kunt een controlepunt instellen in een werkstroom met de **Checkpoint-Workflow** activiteit.

In de volgende voorbeeldcode treedt een fout op na activiteit2 waardoor de werkstroom moet worden beëindigd. Wanneer de werkstroom opnieuw wordt uitgevoerd, wordt er door het uitvoeren van activiteit2 aangezien dit vlak nadat het laatste controlepunt instellen is gestart.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

U moet controlepunten in een werkstroom instellen na activiteiten die foutgevoelig uitzondering zijn en mag geen herhaalde als de werkstroom wordt hervat. De werkstroom kan bijvoorbeeld een virtuele machine maken. U kunt een controlepunt instellen, zowel voor en na de opdrachten voor het maken van de virtuele machine. Als het aanmaken mislukt, zou klikt u vervolgens de opdrachten worden herhaald als de werkstroom opnieuw wordt gestart. Als de werkstroom mislukt nadat de aanmaak slaagt, wordt klikt u vervolgens de virtuele machine niet opnieuw worden gemaakt wanneer de werkstroom wordt hervat.

Het volgende voorbeeld worden meerdere bestanden gekopieerd naar een netwerklocatie en stelt een controlepunt na elk bestand.  Als de netwerklocatie verbroken wordt, klikt u vervolgens beëindigd de werkstroom in fout.  Wanneer opnieuw wordt gestart, wordt deze hervat op het laatste controlepunt, wat betekent dat alleen de bestanden die al is gekopieerd worden overgeslagen.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }

        Write-Output "All files copied."
    }

Omdat de username-verwijzingen zijn niet permanent opgeslagen nadat u de [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activiteit of na het laatste controlepunt, moet u de referenties die null zijn en halen ze vervolgens opnieuw uit de store asset na instellen  **Suspend-Workflow** of controlepunt wordt aangeroepen.  Anders wordt het volgende foutbericht weergegeven: *de werkstroomtaak kan niet worden hervat, ofwel omdat persistentie gegevens kan niet worden opgeslagen volledig of opgeslagen gegevens persistentie is beschadigd. U moet de werkstroom opnieuw.*

De volgende dezelfde code laat zien hoe dit in uw PowerShell Workflow-runbooks te verwerken.

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)

          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


Dit is niet vereist als u zich verifiëren met behulp van een Run As-account geconfigureerd met een service-principal.  

Zie voor meer informatie over controlepunten [controlepunten toevoegen aan een Scriptwerkstroom](http://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks

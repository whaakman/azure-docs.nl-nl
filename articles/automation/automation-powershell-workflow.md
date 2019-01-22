---
title: PowerShell-werkstroom voor Azure Automation leren
description: In dit artikel is bedoeld als een snelle les voor auteurs die bekend zijn met PowerShell om te weten over de specifieke verschillen tussen PowerShell en PowerShell-werkstroom en concepten die van toepassing op Automation-runbooks.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 12/14/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7ab6b387a28df06758e5e0c1ce197781fc4be3c5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54436804"
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Leren van concepten van belangrijke Windows PowerShell-werkstroom voor Automation-runbooks

Azure Automation-Runbooks worden geïmplementeerd als Windows PowerShell-werkstromen.  Een Windows PowerShell-werkstroom is vergelijkbaar met een Windows PowerShell-script, maar heeft enkele belangrijke verschillen die kunnen verwarrend zijn voor een nieuwe gebruiker.  Hoewel dit artikel is bedoeld om te schrijven met behulp van PowerShell-werkstroom runbooks, adviseren we dat u runbooks met behulp van PowerShell, tenzij u controlepunten moet schrijven.  Er zijn enkele syntaxisverschillen bij het ontwerpen van PowerShell Workflow-runbooks en deze verschillen vereisen iets meer werk effectieve werkstromen te schrijven.

Een werkstroom is een opeenvolging van geprogrammeerde, verbonden stappen waarmee langlopende taken wordt uitgevoerd of de coördinatie vereisen van meerdere stappen op meerdere apparaten of beheerde knooppunten. De voordelen van een werkstroom op een normaal script omvatten de mogelijkheid een actie tegen meerdere apparaten tegelijkertijd uitvoeren en de mogelijkheid voor het automatisch herstellen van fouten. Een Windows PowerShell-werkstroom is een Windows PowerShell-script dat gebruikmaakt van Windows Workflow Foundation. Terwijl de werkstroom is geschreven met Windows PowerShell-syntaxis en gelanceerd door Windows PowerShell, wordt verwerkt door Windows Workflow Foundation.

Zie voor meer informatie over de onderwerpen in dit artikel, [aan de slag met Windows PowerShell-werkstroom](https://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>De basisstructuur van een werkstroom

De eerste stap bij het converteren van een PowerShell-script naar een PowerShell-werkstroom is insluitende met de **werkstroom** trefwoord.  Een werkstroom wordt gestart met de **werkstroom** trefwoord gevolgd door de hoofdtekst van het script tussen accolades. Gevolgd door de naam van de werkstroom de **werkstroom** trefwoord zoals getoond in de volgende syntaxis:

```powershell
Workflow Test-Workflow
{
    <Commands>
}
```

De naam van de werkstroom moet overeenkomen met de naam van het Automation-runbook. Als het runbook wordt geïmporteerd, wordt de bestandsnaam moet overeenkomen met de Werkstroomnaam en moet eindigen op *.ps1*.

U kunt parameters toevoegen aan de werkstroom met de **Param** sleutelwoord net zoals u zou voor een script doen.

## <a name="code-changes"></a>Codewijzigingen

PowerShell workflow-code ziet er bijna identiek zijn aan PowerShell-script-code, met uitzondering van enkele belangrijke wijzigingen.  De volgende secties worden wijzigingen die u aanbrengen in een PowerShell-script voor het moet uitvoeren in een werkstroom.

### <a name="activities"></a>Activiteiten

Een activiteit is een specifieke taak in een werkstroom. Net zoals een script van een of meer opdrachten bestaat, wordt een werkstroom bestaat uit een of meer activiteiten die worden uitgevoerd in een reeks. Windows PowerShell-werkstroom converteert automatisch veel van de Windows PowerShell-cmdlets voor activiteiten wanneer deze een werkstroom wordt uitgevoerd. Wanneer u een van deze cmdlets in uw runbook opgeeft, wordt de bijbehorende activiteit uitgevoerd door Windows Workflow Foundation. Voor die cmdlets zonder een bijbehorende activiteit, voert Windows PowerShell-werkstroom automatisch de cmdlet uit binnen een [InlineScript](#inlinescript) activiteit. Er is een set cmdlets die zijn uitgesloten en kan niet worden gebruikt in een werkstroom, tenzij u ze expliciet in een InlineScript-blok opneemt. Zie voor meer informatie over deze concepten [met behulp van activiteiten in Script Workflows](https://technet.microsoft.com/library/jj574194.aspx).

Werkstroomactiviteiten delen een aantal gemeenschappelijke parameters configureren hun werking. Zie voor meer informatie over de algemene werkstroomparameters [about_WorkflowCommonParameters](https://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Positionele parameters

U kunt positionele parameters niet gebruiken met cmdlets in een werkstroom en activiteiten.  Dit betekent dat is dat u de namen van parameters moet gebruiken.

Neem bijvoorbeeld de volgende code waarmee alle actieve services worden opgehaald.

```azurepowershell-interactive
Get-Service | Where-Object {$_.Status -eq "Running"}
```

Als u deze dezelfde code uitvoeren in een werkstroom probeert, ontvangt u een bericht zoals "parameterset kan niet worden omgezet met behulp van de opgegeven benoemde parameters."  Geef de naam van de parameter zoals in het volgende om dit te corrigeren.

```powershell
Workflow Get-RunningServices
{
    Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
}
```

### <a name="deserialized-objects"></a>Gedeserialiseerde objecten

Objecten in werkstromen worden gedeserialiseerd.  Dit betekent dat de eigenschappen zijn nog steeds beschikbaar, maar niet de methoden.  Neem bijvoorbeeld de volgende PowerShell-code waarmee een service met behulp van de Stop-methode van de Service-object wordt gestopt.

```azurepowershell-interactive
$Service = Get-Service -Name MyService
$Service.Stop()
```

Als u uitvoeren in een werkstroom wilt, ontvangt u een foutmelding met de mededeling "aanroepen van de methode wordt niet ondersteund in een Windows PowerShell-werkstroom."

Een optie is het inpakken van deze twee regels met code in een [InlineScript](#inlinescript) blokkeren in dat geval $Service een serviceobject in het blok is.

```powershell
Workflow Stop-Service
{
    InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
    }
}
```

Een andere optie is het gebruik van een andere cmdlet die wordt uitgevoerd dezelfde functionaliteit als de methode, als deze beschikbaar is.  De cmdlet Stop-Service biedt dezelfde functionaliteit als de methode Stop in ons voorbeeld, en u het volgende kunt gebruiken voor een werkstroom.

```powershell
Workflow Stop-MyService
{
    $Service = Get-Service -Name MyService
    Stop-Service -Name $Service.Name
}
```

## <a name="inlinescript"></a>InlineScript

De **InlineScript** activiteit is handig als u wilt uitvoeren van een of meer opdrachten als traditionele PowerShell-script in plaats van de PowerShell-werkstroom.  Terwijl de opdrachten in een werkstroom voor verwerking naar de Windows Workflow Foundation worden verzonden, worden door Windows PowerShell-opdrachten in een InlineScript-blok verwerkt.

InlineScript gebruikt van de volgende syntaxis hieronder weergegeven.

```powershell
InlineScript
{
    <Script Block>
} <Common Parameters>
```

Uitvoer kunt van een InlineScript u terugkeren door de uitvoer toe te wijzen aan een variabele. In het volgende voorbeeld wordt een service wordt gestopt en vervolgens weergeeft naam van de service.

```powershell
Workflow Stop-MyService
{
    $Output = InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
        $Service
    }

    $Output.Name
}
```

U kunt waarden doorgeven in een InlineScript-blok, maar moet u **$Using** bereikaanpassingsfunctie.  Het volgende voorbeeld is identiek aan het vorige voorbeeld, behalve dat de naam van de service wordt geleverd door een variabele.

```powershell
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
```

Terwijl de InlineScript-activiteiten kunnen essentieel zijn in bepaalde werkstromen, ze bieden geen ondersteuning voor werkstroom constructies en mag alleen worden gebruikt wanneer dat nodig is om de volgende redenen:

* U kunt geen gebruiken [controlepunten](#checkpoints) in een InlineScript-blok. Als er een fout optreedt in het blok, moet het worden hervat vanaf het begin van het blok.
* U kunt geen gebruiken [parallelle uitvoering](#parallel-processing) binnen een InlineScriptBlock.
* InlineScript dit beïnvloedt de schaalbaarheid van de werkstroom omdat deze de Windows PowerShell-sessie voor de hele lengte van het InlineScript-blok bevat.

Zie voor meer informatie over het gebruik van InlineScript [Windows PowerShell-opdrachten uitvoeren in een werkstroom](https://technet.microsoft.com/library/jj574197.aspx) en [about_InlineScript](https://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Parallelle verwerking

Een voordeel van Windows PowerShell-werkstromen is de mogelijkheid om te een reeks opdrachten parallel in plaats van opeenvolgend voeren net als bij een typische script.

U kunt de **parallelle** trefwoord dat u wilt een scriptblok te maken met meerdere opdrachten die gelijktijdig worden uitgevoerd. Dit maakt gebruik van de volgende syntaxis hieronder weergegeven. In dit geval starten activiteit1 en activiteit2 op hetzelfde moment. Activiteit3 start pas als zowel activiteit1 en activiteit2 zijn voltooid.

```powershell
Parallel
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Neem bijvoorbeeld de volgende PowerShell-opdrachten waarmee meerdere bestanden worden gekopieerd naar een netwerkbestemming.  Deze opdrachten worden na elkaar uitgevoerd zodanig dat één bestand moet zijn voltooid voordat het wordt gestart met de volgende kopiëren.

```azurepowershell-interactive
Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt
```

De volgende werkstroom wordt deze dezelfde opdrachten parallel uitgevoerd, zodat ze allemaal op hetzelfde moment kopiëren starten.  Alleen nadat ze alle zijn wordt gekopieerd het voltooiingsbericht is weergegeven.

```powershell
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
```

U kunt de **ForEach-Parallel** constructie verwerken van opdrachten voor elk item in een verzameling gelijktijdig. De items in de verzameling worden parallel verwerkt, terwijl de opdrachten in het scriptblok worden opeenvolgend uitgevoerd. Dit maakt gebruik van de volgende syntaxis hieronder weergegeven. In dit geval starten starten activiteit1 op hetzelfde moment voor alle items in de verzameling. Voor elk item start activiteit2 nadat activiteit1 voltooid. Activiteit3 start pas als zowel activiteit1 en activiteit2 zijn voltooid voor alle items. We gebruiken de `ThrottleLimit` parameter voor het beperken van de parallelle uitvoering. Te veel van een `ThrottleLimit` kan problemen veroorzaken. De ideale waarde voor de `ThrottleLimit` parameter, is afhankelijk van veel factoren in uw omgeving. U moet beginnen met een lage waarde en probeer verschillende waarden voor de toenemende totdat u een die geschikt is voor uw specifieke situatie.

```powershell
ForEach -Parallel -ThrottleLimit 10 ($<item> in $<collection>)
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Het volgende voorbeeld is vergelijkbaar met het vorige voorbeeld kopiëren van bestanden tegelijk.  In dit geval wordt een bericht weergegeven voor elk bestand nadat deze zijn gekopieerd.  Alleen nadat ze alle zijn volledig gekopieerd is het uiteindelijke voltooiingsbericht weergegeven.

```powershell
Workflow Copy-Files
{
    $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

    ForEach -Parallel -ThrottleLimit 10 ($File in $Files)
    {
        Copy-Item -Path $File -Destination \\NetworkPath
        Write-Output "$File copied."
    }

    Write-Output "All files copied."
}
```

> [!NOTE]
> We raden niet onderliggende runbooks gelijktijdig uitgevoerd omdat deze is gebleken onbetrouwbare resultaten. De uitvoer van het onderliggende runbook soms niet wordt weergegeven en instellingen in een onderliggend runbook kunnen invloed hebben op de andere parallelle onderliggende runbooks. Variabelen, zoals $VerbosePreference, $WarningPreference en andere kunnen niet worden doorgegeven aan de onderliggende runbooks. En als deze waarden wordt gewijzigd door het onderliggende runbook, ze mogelijk niet correct hersteld na de aanroep.

## <a name="checkpoints"></a>Controlepunten

Een *controlepunt* is een momentopname van de huidige status van de werkstroom die de huidige waarde van variabelen en eventuele gegenereerde uitvoer naar dat punt bevat. Als een werkstroom in fout eindigt of is onderbroken, wordt klikt u vervolgens de volgende keer dat deze wordt uitgevoerd gestart vanaf de laatste controlepunt in plaats van het begin van de werkstroom.  U kunt een controlepunt instellen in een werkstroom met de **Checkpoint-Workflow** activiteit.

In de volgende voorbeeldcode treedt een uitzondering op na activiteit2 waardoor de werkstroom te beëindigen. Wanneer de werkstroom wordt opnieuw uitgevoerd, begint deze door het uitvoeren van activiteit2 aangezien dit vlak nadat het laatst controlepunt ingestelde.

```powershell
<Activity1>
Checkpoint-Workflow
<Activity2>
<Exception>
<Activity3>
```

U moet controlepunten in een werkstroom instellen na activiteiten die foutgevoelig uitzondering zijn en mag geen herhaald als de werkstroom wordt hervat. De werkstroom kan bijvoorbeeld een virtuele machine maken. U kunt een controlepunt kan instellen, zowel vóór als na de opdrachten voor het maken van de virtuele machine. Als het maken is mislukt, zou klikt u vervolgens de opdrachten worden herhaald als de werkstroom opnieuw is gestart. Als de werkstroom mislukt nadat de aanmaak slaagt, wordt klikt u vervolgens de virtuele machine niet opnieuw worden gemaakt wanneer de werkstroom wordt hervat.

Het volgende voorbeeld meerdere bestanden worden gekopieerd naar een netwerklocatie en stelt een controlepunt na elk bestand.  Als de netwerklocatie of wordt eindigt de werkstroom deze fout.  Wanneer deze opnieuw wordt gestart, wordt deze hervat op het laatste controlepunt, wat betekent dat alleen de bestanden die al zijn gekopieerd worden overgeslagen.

```powershell
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
```

Omdat de referenties van de gebruikersnaam niet permanent nadat u de [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activiteit of nadat het laatste controlepunt, moet u de referenties moeten null zijn en halen ze vervolgens opnieuw uit de store asset na instellen  **Suspend-Workflow** of controlepunt wordt aangeroepen.  Anders wordt de volgende strekking weergegeven: *De werkstroomtaak kan niet worden hervat, ofwel omdat persistentie gegevens kan niet worden opgeslagen of opgeslagen persistentie gegevens zijn beschadigd. U moet de werkstroom opnieuw opstarten.*

De volgende dezelfde code ziet u hoe u dit in uw PowerShell Workflow-runbooks worden verwerkt.

```powershell
workflow CreateTestVms
{
    $Cred = Get-AzureAutomationCredential -Name "MyCredential"
    $null = Connect-AzureRmAccount -Credential $Cred

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
        $null = Connect-AzureRmAccount -Credential $Cred
        }
}
```

> [!IMPORTANT]
> **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

Dit is niet vereist als u tijdens de verificatie met behulp van een uitvoeren als-account geconfigureerd met een service-principal.

Zie voor meer informatie over controlepunten, [controlepunten toevoegen aan een Scriptwerkstroom](https://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Volgende stappen

* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks


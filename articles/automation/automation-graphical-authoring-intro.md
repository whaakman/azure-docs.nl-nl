---
title: Grafisch ontwerpen in Azure Automation | Microsoft Docs
description: Grafisch ontwerpen, kunt u runbooks voor Azure Automation maken zonder het werken met code. In dit artikel biedt een inleiding tot het grafisch ontwerpen en alle gegevens die nodig zijn voor het maken van een grafisch runbook starten.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 4b6f840c-e941-4293-a728-b33407317943
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 5cf9ef392a5a4e33f6413495e1c81e969d50dcad
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="graphical-authoring-in-azure-automation"></a>Grafisch ontwerpen in Azure Automation
## <a name="introduction"></a>Inleiding
Grafisch ontwerpen kunt u runbooks maken voor Azure Automation zonder de complexiteit van de onderliggende Windows PowerShell of PowerShell Workflow-code. U activiteiten toevoegen aan het papier vanuit een bibliotheek met de cmdlets en runbooks, ze aan elkaar koppelt en configureert om een werkstroom.  Als u ooit met System Center Orchestrator of Service Management Automation (SMA) hebt gewerkt, klikt u vervolgens ziet dit er waarschijnlijk vertrouwd voor u.   

In dit artikel biedt een inleiding tot het grafisch ontwerpen en de concepten die u nodig hebt om te beginnen bij het maken van een grafisch runbook.

## <a name="graphical-runbooks"></a>Grafische runbooks
Alle runbooks in Azure Automation zijn Windows PowerShell-werkstromen.  Grafisch en grafische PowerShell Workflow-runbooks PowerShell-code die wordt uitgevoerd door de werknemers Automation genereren, maar u niet kan worden weergegeven of rechtstreeks worden gewijzigd.  Een grafisch runbook kan worden geconverteerd naar een grafische PowerShell Workflow-runbook en vice versa, maar ze kunnen niet worden geconverteerd naar een tekstueel runbook. Een bestaande tekstueel runbook kan niet worden geïmporteerd in de grafische editor.  

## <a name="overview-of-graphical-editor"></a>Overzicht van de grafische editor
Open de grafische editor in Azure portal door maken of bewerken van een grafisch runbook.

![Grafische werkruimte](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

De volgende secties beschrijven de besturingselementen in de grafische editor.

### <a name="canvas"></a>Canvas
Het Canvas is waar u uw runbook te ontwerpen.  U activiteiten van de knooppunten in het besturingselement bibliotheek toevoegen aan het runbook en verbind ze met koppelingen naar de logica van het runbook definiëren.

U kunt de besturingselementen aan de onderkant van het canvas in-en uitzoomen.

![Grafische werkruimte](media/automation-graphical-authoring-intro/runbook-canvas-controls.png)

### <a name="library-control"></a>Besturingselement bibliotheek
Het besturingselement bibliotheek is waar u [activiteiten](#activities) om toe te voegen aan uw runbook.  U toevoegt ze aan het papier waar u ze met andere activiteiten verbindt.  Dit omvat vier secties in de volgende tabel beschreven.

| Sectie | Beschrijving |
|:--- |:--- |
| Cmdlets |Bevat alle cmdlets die kunnen worden gebruikt in uw runbook.  Cmdlets zijn ingedeeld in de module.  Alle van de modules die u hebt geïnstalleerd in uw automation-account zijn beschikbaar. |
| Runbooks |Bevat de runbooks in uw automation-account. Deze runbooks kunnen worden toegevoegd aan het papier moet worden gebruikt als onderliggende runbooks. Alleen runbooks van hetzelfde type als het runbook wordt bewerkt core worden weergegeven; Grafische worden runbooks alleen op basis van PowerShell-runbooks weergegeven, terwijl voor grafische PowerShell Workflow-runbooks alleen PowerShell-werkstroom runbooks worden weergegeven. |
| Assets |Bevat de [automation activa](http://msdn.microsoft.com/library/dn939988.aspx) in uw automation-account die kan worden gebruikt in uw runbook.  Wanneer u een asset aan een runbook toevoegt, wordt het toevoegen van een workflowactiviteit die de geselecteerde asset opgehaald.  In het geval van een variabele assets, kunt u selecteren of een activiteit als u de variabele ophalen of instellen van de variabele wilt toevoegen. |
| Runbookbesturing |Bevat besturingselement runbookactiviteiten die kunnen worden gebruikt in uw huidige runbook. Een *koppelingspunten* neemt meerdere invoer en wacht totdat alle hebben voltooid voordat u doorgaat met de werkstroom. Een *Code* activiteit wordt uitgevoerd voor een of meer coderegels PowerShell of PowerShell Workflow, afhankelijk van het type grafisch runbook.  U kunt deze activiteit kunt gebruiken voor aangepaste code of voor functionaliteit die moeilijk te bereiken met andere activiteiten. |

### <a name="configuration-control"></a>Configuratiebeheer
Het besturingselement configuratie is waarin u details opgeven voor een object dat is geselecteerd op het canvas. De eigenschappen die beschikbaar zijn in dit besturingselement is afhankelijk van het type object dat is geselecteerd.  Wanneer u een optie in het besturingselement configuratie selecteert, wordt deze extra blades geopend zodat u meer informatie.

### <a name="test-control"></a>Besturingselement testen
Het besturingselement van de Test wordt niet weergegeven wanneer de grafische editor voor het eerst wordt gestart. Dit bestand is geopend wanneer u interactief [grafisch runbook testen](#graphical-runbook-procedures).  

## <a name="graphical-runbook-procedures"></a>Grafisch runbook procedures
### <a name="exporting-and-importing-a-graphical-runbook"></a>Exporteren en importeren van een grafisch runbook
U kunt alleen de gepubliceerde versie van een grafisch runbook exporteren.  Als het runbook is nog niet gepubliceerd, wordt de **Export gepubliceerd** knop wordt uitgeschakeld.  Wanneer u klikt op de **Export gepubliceerd** knop, het runbook wordt gedownload op uw lokale computer.  De naam van het bestand overeenkomt met de naam van het runbook met een *graphrunbook* extensie.

![Gepubliceerde exporteren](media/automation-graphical-authoring-intro/runbook-export.png)

U kunt een grafisch of grafische PowerShell Workflow-runbook-bestand importeren door het selecteren van de **importeren** optie bij het toevoegen van een runbook.   Wanneer u het te importeren bestand selecteert, kunt u dezelfde **naam** of geef een nieuwe.  Nadat het evalueert het geselecteerde bestand en als u probeert een ander type dan is niet correct selecteren, een bericht verschijnt er mogelijke conflicten en tijdens de conversie, er zijn syntaxisfouten Let in het veld Runbooktype het type runbook weergegeven.  

![Runbook importeren](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Een grafisch runbook testen
Bij het verlaten van de gepubliceerde versie van het runbook niet worden gewijzigd of kunt u een nieuw runbook testen voordat deze is gepubliceerd, kunt u de conceptversie van een runbook testen in de Azure portal. Hiermee kunt u controleren of het runbook correct werkt voordat de gepubliceerde versie vervangen. Wanneer u een runbook test, wordt het conceptrunbook uitgevoerd en acties die worden uitgevoerd worden voltooid. Er wordt geen Taakgeschiedenis gemaakt, maar uitvoer wordt weergegeven in het deelvenster Testuitvoer. 

Het besturingselement testen voor een runbook te openen door het openen van het runbook om te bewerken en klik vervolgens op de **testvenster** knop.

![Deelvenster-knop testen](media/automation-graphical-authoring-intro/runbook-edit-test-pane.png)

Het besturingselement van de Test wordt gevraagd voor alle parameters voor invoer en u het runbook start door te klikken op de **Start** knop.

![Test knoppen](media/automation-graphical-authoring-intro/runbook-test-start.png)

### <a name="publishing-a-graphical-runbook"></a>Een grafisch runbook publiceren
Elk runbook in Azure Automation heeft een ontwerpversie en een gepubliceerde versie. Alleen de gepubliceerde versie is beschikbaar om te worden uitgevoerd en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Wanneer de conceptversie beschikbaar gereed is, publiceert u deze waardoor de gepubliceerde versie met de conceptversie overschreven.

U kunt een grafisch runbook publiceren door het openen van het runbook om te bewerken en vervolgens te klikken op de **publiceren** knop.

![De knop Publiceren](media/automation-graphical-authoring-intro/runbook-edit-publish.png)

Wanneer een runbook is nog niet gepubliceerd, heeft een status van **nieuw**.  Wanneer het wordt gepubliceerd, heeft deze een status **gepubliceerde**.  Als u het runbook te bewerken nadat deze is gepubliceerd en de conceptversie en een gepubliceerde versies verschillend zijn, het runbook heeft de status van **In bewerken**.

![Runbook-status](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png) 

U hebt ook de optie om de gepubliceerde versie van een runbook te herstellen.  Hiermee verwijdert u eventuele wijzigingen die zijn gemaakt sinds het runbook het laatst is gepubliceerd en de conceptversie van het runbook door de gepubliceerde versie vervangt.

![Terugkeren naar de gepubliceerde knop](media/automation-graphical-authoring-intro/runbook-edit-revert-published.png)

## <a name="activities"></a>Activiteiten
Activiteiten zijn de bouwstenen van een runbook.  Een activiteit kan dit een PowerShell-cmdlet, een onderliggend runbook of een workflow-activiteit.  Toevoegen van een activiteit aan het runbook door met de rechtermuisknop te klikken op het in het besturingselement bibliotheek en **toevoegen aan papier**.  U kunt Klik en sleep de activiteit om een willekeurige plaats op het canvas die u wilt plaatsen.  De locatie van de van de activiteit op het canvas heeft geen invloed op de werking van het runbook op elke manier.  U kunt lay-out uw runbook echter u het meest geschikt is vindt voor het visualiseren van de werking ervan. 

![Toevoegen aan canvas](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Selecteer de activiteit op het canvas voor het configureren van de eigenschappen en parameters in de configuratie-blade.  U kunt wijzigen de **Label** van de activiteit op een andere waarde die is beschrijvend voor u.  De oorspronkelijke cmdlet nog steeds wordt uitgevoerd, wijzigt u eenvoudigweg de weergavenaam die wordt gebruikt in de grafische editor.  Het label moet uniek zijn binnen het runbook. 

### <a name="parameter-sets"></a>Parametersets
Een parameterset definieert de verplichte en optionele parameters die waarden voor een bepaalde cmdlet accepteert.  Alle cmdlets hebben ten minste één parameter is ingesteld en sommige meerdere hebben.  Als een cmdlet meerdere parametersets bevat, moet vervolgens u selecteren welke methode u gebruiken wilt voordat u parameters kunt configureren.  De parameters die u kunt configureren, is afhankelijk van de parameterset die u kiest.  U kunt de parameter is ingesteld door een activiteit wordt gebruikt door het selecteren van wijzigen **parameterset** en het selecteren van een andere set.  In dit geval de parameterwaarden die u hebt geconfigureerd gaan verloren.

In het volgende voorbeeld heeft de cmdlet Get-AzureRmVM drie parametersets.  U kunt parameterwaarden niet configureren, totdat u een van de parametersets selecteren.  De parameter ListVirtualMachineInResourceGroupParamSet ingesteld voor het retourneren van alle virtuele machines in een resourcegroep is en één optionele parameter.  De GetVirtualMachineInResourceGroupParamSet is voor het opgeven van de virtuele machine u wilt retourneren en heeft twee verplicht en een optionele parameter.

![Parameterset](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parameterwaarden
Als u een waarde voor een parameter opgeven, selecteert u een gegevensbron om te bepalen hoe u de waarde wordt opgegeven.  De gegevensbronnen die beschikbaar voor een bepaalde parameter zijn is afhankelijk van de geldige waarden voor deze parameter.  Null worden bijvoorbeeld niet een beschikbare optie voor een parameter die null-waarden zijn niet toegestaan.

| Gegevensbron | Beschrijving |
|:--- |:--- |
| Constante waarde |Typ een waarde voor de parameter.  Dit is alleen beschikbaar voor de volgende gegevenstypen: Int32, Int64, String, Boolean, DateTime, Switch. |
| Uitvoer van activiteit |De uitvoer van een activiteit die voorafgaat aan de huidige activiteit in de werkstroom.  Kan alle geldige activiteiten wordt weergegeven.  Selecteer alleen de activiteit met de uitvoer voor de parameterwaarde.  Als een object met meerdere eigenschappen van de uitvoer van de activiteit, kunt u opgeven in de naam van de eigenschap na het selecteren van de activiteit. |
| Runbookinvoer |Selecteer een runbookinvoerparameter als invoer voor de parameter van de activiteit. |
| Variabelenactivum |Selecteer een Automation-variabele als invoer. |
| Referentie-element |Selecteer een Automation-referentie als invoer. |
| Certificaatasset |Selecteer een Automation-certificaat als invoer. |
| Verbindingstype-Asset |Selecteer een Automation-verbinding als invoer. |
| PowerShell-expressie |Geef een eenvoudige [PowerShell-expressie](#powershell-expressions).  De expressie wordt eerst geëvalueerd en de activiteit en het resultaat dat wordt gebruikt voor de parameterwaarde.  U kunt variabelen gebruiken om te verwijzen naar de uitvoer van een activiteit of een runbook-invoerparameter. |
| Niet geconfigureerd |Hiermee wist u elke waarde die eerder was geconfigureerd. |

#### <a name="optional-additional-parameters"></a>Optionele extra parameters
Alle cmdlets wordt de optie in als u extra parameters hebben.  Dit zijn algemene PowerShell-parameters of andere aangepaste parameters.  Krijgt u een tekstvak waarin u de parameters in met behulp van PowerShell-syntaxis kunt opgeven.  Bijvoorbeeld, gebruiken de **uitgebreid** algemene parameter geeft u **'-Verbose: $True '**.

### <a name="retry-activity"></a>Probeer de activiteit
**Opnieuw proberen** kunt een activiteit meerdere keren worden uitgevoerd totdat een bepaalde voorwaarde wordt voldaan, net als een lus.  U kunt deze functie gebruiken voor activiteiten die meerdere keren moet uitvoeren, foutgevoelig zijn en kan moet meer dan één proberen voor een correcte werking of testen van de uitvoerinformatie van de activiteit voor geldige gegevens.    

Wanneer u opnieuw proberen voor een activiteit inschakelt, kunt u een vertraging optreden en een voorwaarde instellen.  De vertraging is de tijd (gemeten in seconden of minuten) dat het runbook wacht voordat deze de activiteit opnieuw wordt uitgevoerd.  Als er geen vertraging is opgegeven, klikt u vervolgens de activiteit wordt uitgevoerd opnieuw onmiddellijk nadat deze is voltooid. 

![Activiteit-tijd tussen elke poging](media/automation-graphical-authoring-intro/retry-delay.png)

De voorwaarde opnieuw proberen is een PowerShell-expressie die wordt geëvalueerd na elke keer dat de activiteit wordt uitgevoerd.  Als de expressie op True is opgelost, klikt u vervolgens de activiteit opnieuw uitgevoerd.  De expressie wordt omgezet in False wordt de activiteit niet opnieuw uitgevoerd als het runbook op naar de volgende activiteit schakelt. 

![Activiteit-tijd tussen elke poging](media/automation-graphical-authoring-intro/retry-condition.png)

De voorwaarde opnieuw kunt gebruiken een variabele met de naam $RetryData die toegang tot informatie over de activiteit nieuwe pogingen biedt.  Deze variabele heeft de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| NumberOfAttempts |Het aantal keren dat de activiteit is uitgevoerd. |
| Uitvoer |De uitvoer van de laatste uitvoering van de activiteit. |
| TotalDuration |Time-out verstreken sinds de eerste keer werd gestart door de activiteit. |
| StartedAt |Tijd in UTC-notatie die de activiteit voor het eerst is gestart. |

Hieronder vindt u voorbeelden van de activiteit van de nieuwe poging voorwaarden.

    # Run the activity exactly 10 times.
    $RetryData.NumberOfAttempts -ge 10 

    # Run the activity repeatedly until it produces any output.
    $RetryData.Output.Count -ge 1 

    # Run the activity repeatedly until 2 minutes has elapsed. 
    $RetryData.TotalDuration.TotalMinutes -ge 2

Nadat u een nieuwe poging voorwaarde voor een activiteit configureert, bevat de activiteit twee visuele aanwijzingen om u te herinneren.  Een wordt weergegeven in de activiteit en de andere is wanneer u de configuratie van de activiteit controleren.

![Activiteit opnieuw Visual indicatoren](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>Script werkstroombeheer
Een Code-besturingselement is een speciale activiteit PowerShell of PowerShell Workflow-script afhankelijk van het type grafisch runbook worden opgesteld accepteert zodat de functionaliteit die anders mogelijk niet beschikbaar.  Er kan geen parameters accepteren, maar het kan variabelen gebruiken voor activiteit uitvoer en runbook-invoerparameters.  Eventuele uitvoer van de activiteit is toegevoegd aan de gegevensbus, tenzij er geen uitgaande koppelen in dat geval wordt deze toegevoegd aan de uitvoer van het runbook.

De volgende code voert bijvoorbeeld met behulp van een runbook invoer variabele met de naam $NumberOfDays datumberekeningen.  Vervolgens stuurt een berekende datum-tijd als uitvoer moet worden gebruikt door de volgende activiteiten in het runbook.

    $DateTimeNow = (Get-Date).ToUniversalTime()
    $DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
    $DateTimeStart


## <a name="links-and-workflow"></a>Werkstroom en koppelingen
Een **koppeling** verbindt twee activiteiten in een grafisch runbook.  Als een pijl vanuit de bronactiviteit naar de doelactiviteit wordt deze weergegeven op het canvas.  De activiteiten worden uitgevoerd in de richting van de pijl met de doelactiviteit wordt gestart nadat de bronactiviteit is voltooid.  

### <a name="create-a-link"></a>Een koppeling maken
Maak een koppeling tussen twee activiteiten door te klikken op de cirkel aan de onderkant van de vorm te selecteren van de bronactiviteit.  Sleep de pijl naar de doelactiviteit en release.

![Een koppeling maken](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Selecteer de koppeling voor het configureren van de eigenschappen in de configuratie-blade.  Hierbij wordt het type van de koppeling die wordt beschreven in de volgende tabel.

| Koppelingstype | Beschrijving |
|:--- |:--- |
| Pijplijn |De doelactiviteit wordt eenmaal uitvoeren voor elk objectuitvoer vanuit de bronactiviteit.  De doelactiviteit wordt niet uitgevoerd als de bronactiviteit in het geen uitvoer resulteert.  De uitvoer van de bronactiviteit is beschikbaar als een object. |
| Volgorde |De doelactiviteit wordt slechts één keer uitgevoerd.  Wordt een matrix met objecten ontvangen van de bronactiviteit.  Uitvoer van de bronactiviteit is beschikbaar als een matrix met objecten. |

### <a name="starting-activity"></a>Starten van de activiteit
Een grafisch runbook start met activiteiten die geen van de koppeling van een binnenkomende.  Dit is vaak slechts één activiteit die als de eerste activiteit voor het runbook optreden zou.  Als meerdere activiteiten een binnenkomende koppeling niet hebt, start het runbook door ze parallel worden uitgevoerd.  Deze wordt vervolgens volgt de koppelingen voor andere activiteiten worden uitgevoerd zoals elk is voltooid.

### <a name="conditions"></a>Voorwaarden
Wanneer u een voorwaarde op een koppeling opgeeft, wordt de doelactiviteit alleen uitgevoerd als de voorwaarde wordt omgezet in waar.  Gewoonlijk gebruikt u een variabele $ActivityOutput in een voorwaarde voor het ophalen van de uitvoer van de bronactiviteit.  

U een voorwaarde voor een enkel object opgeven voor een pipeline-koppeling, en de voorwaarde wordt geëvalueerd voor elke objectuitvoer door de bronactiviteit.  De doelactiviteit wordt vervolgens uitgevoerd voor elk object dat aan de voorwaarde voldoet.  Bijvoorbeeld, met een bronactiviteit van Get-AzureRmVm, de volgende syntaxis kan worden gebruikt voor een voorwaardelijke pijplijnkoppeling om op te halen, alleen virtuele machines in de resourcegroep met de naam *Group1*.  

    $ActivityOutput['Get Azure VMs'].Name -match "Group1"

Voor een reeks-koppeling, wordt de voorwaarde alleen geëvalueerd eenmaal omdat één array wordt geretourneerd met alle objecten uitvoer van de bronactiviteit.  Als gevolg hiervan is een reeks koppeling kan niet worden gebruikt voor het filteren van zoals een pipeline-koppeling, maar gewoon wordt bepaald of de volgende activiteit wordt uitgevoerd. Neem bijvoorbeeld de volgende set van activiteiten in ons runbook VM Start.<br> ![Voorwaardelijke koppeling met reeksen](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)<br>
Er zijn drie verschillende reeks koppelingen die waarden zijn opgegeven voor twee runbook invoerparameters die VM-naam en de naam van de resourcegroep vertegenwoordigt om te bepalen wat de juiste actie te ondernemen - controleren één VM starten, alle virtuele machines starten in de resource groep of alle virtuele machines in een abonnement.  Hier volgt de logica van de voorwaarde voor de koppeling volgorde tussen verbinding maken met Azure en één Get-VM:

    <# 
    Both VMName and ResourceGroupName runbook input parameters have values 
    #>
    (
    (($VMName -ne $null) -and ($VMName.Length -gt 0))
    ) -and (
    (($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
    )

Wanneer u een voorwaardelijke koppeling gebruikt, worden de gegevens beschikbaar zijn vanuit de bronactiviteit naar andere activiteiten in dat filiaal gefilterd door de voorwaarde.  Als een activiteit van de bron naar meerdere koppelingen, klikt u vervolgens afhankelijk de gegevens beschikbaar zijn voor activiteiten in elke vertakking van de voorwaarde in de koppeling die verbinding maken met dat filiaal.

Bijvoorbeeld, de **Start-AzureRmVm** activiteit in het onderstaande runbook wordt gestart voor alle virtuele machines.  Heeft twee voorwaardelijke koppelingen.  De expressie wordt gebruikt voor de eerste voorwaardelijke koppeling *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true* filteren als de Start-AzureRmVm-activiteit is voltooid.  De tweede maakt gebruik van de expressie *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true* filteren als de activiteit Start-AzureRmVm is de virtuele machine te starten is mislukt.  

![Voorbeeld van een voorwaardelijke koppeling](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

Elke activiteit die volgt op de eerste koppeling en maakt gebruik van de uitvoer van de activiteit van Get-AzureVM krijgt alleen de virtuele machines die zijn gestart op het moment dat de Get-AzureVM werd uitgevoerd.  Elke activiteit die volgt op de tweede koppeling krijgt alleen de virtuele machines die zijn gestopt op het moment dat de Get-AzureVM werd uitgevoerd.  Elke activiteit die de derde koppeling krijgen alle virtuele machines ongeacht hun actief is.

### <a name="junctions"></a>Koppelingen
Een verbinding is een speciale activiteit wacht totdat alle binnenkomende takken hebt voltooid.  Hiermee kunt u meerdere activiteiten parallel worden uitgevoerd en ervoor te zorgen dat alle hebt voltooid voordat u doorgaat.

Hoewel een verbinding een onbeperkt aantal inkomende koppelingen hebben kan, zijn niet meer dan één van deze koppelingen een pijplijn.  Het aantal inkomende koppelingen van de reeks wordt niet beperkt.  U mag de verbinding maken met meerdere inkomende pijplijn koppelingen en opslaan van het runbook, maar zal mislukken wanneer deze wordt uitgevoerd.

Het onderstaande voorbeeld is onderdeel van een runbook die een set van virtuele machines tijdens het downloaden van patches worden toegepast op deze machines tegelijkertijd begint.  Een verbinding wordt gebruikt om ervoor te zorgen dat beide processen zijn voltooid voordat het runbook wordt hervat.

![De verbinding](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Cycli
Een cyclus is wanneer een bestemming activiteit koppelingen terug naar de bronactiviteit of een andere activiteit die uiteindelijk teruggekoppeld naar de bron.  Cycli worden momenteel niet toegestaan in het grafisch ontwerpen.  Als uw runbook een cyclus is, correct worden opgeslagen, maar wordt er een foutbericht wanneer deze wordt uitgevoerd.

![Cyclus](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Delen van gegevens tussen activiteiten
Alle gegevens die wordt uitgevoerd door een activiteit met de koppeling van een uitgaande wordt geschreven naar de *gegevensbus* voor het runbook.  Elke activiteit in het runbook kunt gegevens gebruiken op de gegevensbus voor het invullen van parameterwaarden of opnemen in een script.  Een activiteit hebben toegang tot de uitvoer van een vorige activiteit in de werkstroom.     

Hoe de gegevens worden geschreven in de gegevensbus, is afhankelijk van het type koppeling op de activiteit.  Voor een **pijplijn**, de gegevens wordt weergegeven als veelvouden objecten.  Voor een **reeks** wordt uitgevoerd als een matrix van de gegevens te koppelen.  Als er slechts één waarde, zal de uitvoer zijn als een matrix met één element.

U kunt toegang tot gegevens op de gegevensbus met een van twee methoden.  Eerst wordt met behulp van een **uitvoer van activiteit** gegevensbron voor het vullen van een parameter van een andere activiteit.  Als de uitvoer een object is, kunt u één eigenschap opgeven.

![Uitvoer van activiteit](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

U kunt ook ophalen met de uitvoer van een activiteit in een **PowerShell-expressie** gegevensbron of vanuit een **Werkstroomscript** activiteit met een variabele ActivityOutput.  Als de uitvoer een object is, kunt u één eigenschap opgeven.  ActivityOutput variabelen gebruiken de volgende syntaxis.

    $ActivityOutput['Activity Label']
    $ActivityOutput['Activity Label'].PropertyName 

### <a name="checkpoints"></a>Controlepunten
U kunt instellen [controlepunten](automation-powershell-workflow.md#checkpoints) in een grafische PowerShell Workflow-runbook door te selecteren *controlepunt runbook* op elke activiteit.  Dit zorgt ervoor dat een controlepunt worden ingesteld nadat de activiteit is uitgevoerd.

![Controlepunt](media/automation-graphical-authoring-intro/set-checkpoint.png)

Controlepunten is alleen ingeschakeld in de grafische PowerShell Workflow-runbooks, is niet beschikbaar in de grafische runbooks.  Als het runbook Azure-cmdlets gebruikt, moet u een controlepunt activiteit met een Add-AzureRMAccount volgen als het runbook is onderbroken en opnieuw wordt opgestart vanaf dit controlepunt op een andere werknemer. 

## <a name="authenticating-to-azure-resources"></a>Verificatie bij de Azure-resources
Runbooks in Azure Automation die Azure-resources te beheren, moet verificatie met Azure.  De [Run As-account](automation-offering-get-started.md#creating-an-automation-account) (ook aangeduid als een service-principal) is de standaardmethode voor toegang tot Azure Resource Manager-resources in uw abonnement met Automation-runbooks.  U kunt deze functionaliteit toevoegen aan een grafisch runbook door toe te voegen de **AzureRunAsConnection** verbindingsasset die met behulp van de PowerShell [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) -cmdlet en [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet toe aan het papier. Dit wordt geïllustreerd in het volgende voorbeeld.<br>![Als verificatie activiteiten uitvoeren](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)<br>
De activiteit uitvoeren als-verbinding ophalen (dat wil zeggen Get-AutomationConnection) is geconfigureerd met een constante waarde gegevensbron met de naam AzureRunAsConnection.<br>![Run As-configuratie-verbinding](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)<br>
De volgende activiteit, Add-AzureRmAccount, voegt de geverifieerde Run As-account voor gebruik in het runbook.<br>
![Parameterset Add-AzureRmAccount](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)<br>
Voor de parameters **APPLICATIONID**, **CERTIFICATETHUMBPRINT**, en **TENANTID** moet u de naam van de eigenschap voor het veldpad opgeven omdat een object met meerdere eigenschappen van de uitvoer van de activiteit.  Anders tijdens het uitvoeren van het runbook mislukt dit probeert te verifiëren.  Dit is wat u moet ten minste uw runbook met het Run As-account te verifiëren.

Onderhouden achterwaartse compatibiliteit voor abonnees die u hebt gemaakt een Automation-account via een [Azure AD-gebruikersaccount](automation-create-aduser-account.md) voor het beheren van de klassieke Azure-implementatie of de methode om te verifiëren voor Azure Resource Manager-resources, de cmdlet Add-AzureAccount met een [referentieasset](automation-credentials.md) die staat voor een Active Directory-gebruiker met toegang op de Azure-account.

U kunt deze functionaliteit toevoegen aan een grafisch runbook door een referentie-element toe te voegen aan het canvas gevolgd door een Add-AzureAccount-activiteit.  -AzureAccount maakt gebruik van de activiteit van de referentie voor de invoer.  Dit wordt geïllustreerd in het volgende voorbeeld.

![Verificatie-activiteiten](media/automation-graphical-authoring-intro/authentication-activities.png)

U moet verifiëren aan het begin van het runbook en na elke controlepunt.  Dit betekent dat met het toevoegen van een activiteit toevoeging Add-AzureAccount na elke activiteit controlepunt-werkstroom. U hoeft niet een activiteit van de referentie toevoegen omdat u hetzelfde kunt gebruiken 

![Uitvoer van activiteit](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Runbook invoer en uitvoer
### <a name="runbook-input"></a>Runbookinvoer
Een runbook moet mogelijk invoer van een gebruiker wanneer het runbook via de Azure portal wordt gestart of vanuit een ander runbook als het huidige proces wordt gebruikt als een onderliggend element.
Bijvoorbeeld, hebt u een runbook dat een virtuele machine maakt, wellicht u waarmee informatie zoals de naam van de virtuele machine en andere eigenschappen telkens wanneer u het runbook start.  

U accepteert invoer voor een runbook door te definiëren een of meer parameters voor invoer.  U opgeven waarden voor deze parameters telkens wanneer die het runbook wordt gestart.  Wanneer u een runbook met de Azure portal start, wordt gevraagd u waarden opgeven voor elk van de invoerparameters van het runbook.

U toegang hebt tot invoerparameters voor een runbook door te klikken op de **invoer en uitvoer** op de werkbalk van runbook.  

![Runbookinvoer uitvoer](media/automation-graphical-authoring-intro/runbook-edit-input-output.png) 

Hiermee opent u de **invoer en uitvoer** besturingselement kunt u een bestaande invoerparameter bewerken of een nieuwe maken door te klikken op **invoer toevoegen**. 

![Invoer toevoegen](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Elke invoerparameter wordt gedefinieerd door de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Naam |De unieke naam van de parameter.  Dit mag alleen alfanumerieke tekens bevatten en mogen geen. |
| Beschrijving |Een optionele beschrijving voor de invoerparameter. |
| Type |Het gegevenstype voor de waarde van parameter verwacht.  De Azure-portal biedt een juiste besturingselement voor het gegevenstype voor elke parameter bij vragen om invoer. |
| Verplicht |Hiermee geeft u op of een waarde voor de parameter moet worden opgegeven.  Het runbook kan niet worden gestart als u een waarde niet opgeeft voor elke verplichte parameter waarmee geen standaardwaarde gedefinieerd heeft. |
| Standaardwaarde |Hiermee geeft u op welke waarde voor de parameter wordt gebruikt als deze niet is opgegeven.  Dit kan ofwel Null of een specifieke waarde zijn. |

### <a name="runbook-output"></a>Runbook-uitvoer
Gegevens die zijn gemaakt door elke activiteit die geen een uitgaande koppeling wordt toegevoegd aan de [uitvoer van het runbook](http://msdn.microsoft.com/library/azure/dn879148.aspx).  De uitvoer met de runbooktaak is opgeslagen en is beschikbaar voor een bovenliggend runbook wanneer het runbook wordt gebruikt als een onderliggend element.  

## <a name="powershell-expressions"></a>PowerShell-expressies
Een van de voordelen van het grafisch ontwerpen biedt u de mogelijkheid voor het bouwen van een runbook met minimale kennis van PowerShell.  Op dit moment kunt u moet weten een PowerShell-bits als voor het invullen van bepaalde [parameterwaarden](#activities) en voor de instelling [koppelen voorwaarden](#links-and-workflow).  Deze sectie bevat een korte inleiding in expressies voor gebruikers die mogelijk niet bekend bent met het PowerShell.  Volledige details van PowerShell zijn beschikbaar op [met Windows PowerShell-scripts](http://technet.microsoft.com/library/bb978526.aspx). 

### <a name="powershell-expression-data-source"></a>Gegevensbron voor PowerShell-expressie
U kunt een PowerShell-expressie gebruiken als gegevensbron voor het vullen van de waarde van een [activiteitsparameter](#activities) met de resultaten van sommige PowerShell-code.  Dit kan bijvoorbeeld één regel code die enkele eenvoudige functie of meerdere regels die bepaalde complexe logica uitvoeren wordt uitgevoerd.  Elke uitvoer van een opdracht die niet aan een variabele toegewezen is worden uitgevoerd in de parameterwaarde. 

De volgende opdracht zou bijvoorbeeld de uitvoer van de huidige datum. 

    Get-Date

De volgende opdrachten opbouwen van een tekenreeks van de huidige datum en deze toewijzen aan een variabele.  De inhoud van de variabele worden vervolgens naar de uitvoer verzonden 

    $string = "The current date is " + (Get-Date)
    $string

De volgende opdrachten Evalueer de huidige datum en -tekenreeks geretourneerd die aangeeft of de huidige dag een weekend of weekdag. 

    $date = Get-Date
    if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
    else { "Weekday" }


### <a name="activity-output"></a>Uitvoer van activiteit
Als u de uitvoer van een vorige activiteit in het runbook, gebruiken de $ActivityOutput variabele met de volgende syntaxis.

    $ActivityOutput['Activity Label'].PropertyName

Bijvoorbeeld wellicht hebt u een activiteit met een eigenschap waarvoor u de naam van een virtuele machine in dat geval kunt u de volgende expressie.

    $ActivityOutput['Get-AzureVm'].Name

Als de eigenschap die de virtuele machine vereist in plaats van alleen een eigenschap object, zou u het gehele object met de volgende syntaxis geretourneerd.

    $ActivityOutput['Get-AzureVm']

U kunt ook de uitvoer van een activiteit in een complexe expressie zoals de volgende tekst naar de naam van de virtuele machine worden aaneengeschakeld.

    "The computer name is " + $ActivityOutput['Get-AzureVm'].Name


### <a name="conditions"></a>Voorwaarden
Gebruik [vergelijkingsoperators](https://technet.microsoft.com/library/hh847759.aspx) waarden vergelijken of bepalen of een waarde overeenkomt met een opgegeven patroon.  Een vergelijking retourneert een waarde van $true en $false.

Bijvoorbeeld de volgende voorwaarde bepaalt of de virtuele machine vanuit een activiteit genaamd *Get-AzureVM* is momenteel *gestopt*. 

    $ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"

De volgende voorwaarden controles of dezelfde virtuele machine anders dan in een staat is *gestopt*.

    $ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"

U kunt deelnemen aan meerdere voorwaarden met behulp van een [logische operator](https://technet.microsoft.com/library/hh847789.aspx) zoals **- en** of **- of**.  De volgende voorwaarden bijvoorbeeld controles of dezelfde virtuele machine in het vorige voorbeeld in een status van is *gestopt* of *stoppen*.

    ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping") 


### <a name="hashtables"></a>Hash-tabellen
[Hash-tabellen](http://technet.microsoft.com/library/hh847780.aspx) zijn naam/waarde-paren die handig zijn voor een set waarden retourneren.  Eigenschappen voor bepaalde activiteiten kunnen een hashtabel in plaats van een eenvoudige waarde verwacht.  U ziet mogelijk ook als hashtabel aangeduid als een woordenlijst. 

U kunt een hashtabel maken met de volgende syntaxis.  Een hashtabel mag een onbeperkt aantal vermeldingen bevatten, maar elk wordt gedefinieerd door een naam en waarde.

    @{ <name> = <value>; [<name> = <value> ] ...}

De volgende expressie wordt bijvoorbeeld een hashtabel moet worden gebruikt in de gegevensbron voor de parameter van een activiteit die een hashtabel met waarden voor een zoekopdracht internet verwacht.

    $query = "Azure Automation"
    $count = 10
    $h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
    $h

Het volgende voorbeeld wordt de uitvoer van een activiteit aangeroepen *Twitter-verbinding ophalen* een hashtabel te vullen.

    @{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
      'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
      'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
      'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}



## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks 
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Om te begrijpen hoe u verifieert met behulp van de automatisering Run As-account, Zie [configureren Azure uitvoeren als-Account](automation-sec-configure-azure-runas-account.md)


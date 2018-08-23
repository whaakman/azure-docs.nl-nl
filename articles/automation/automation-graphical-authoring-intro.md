---
title: Grafisch ontwerpen in Azure Automation
description: Grafisch ontwerpen, kunt u runbooks voor Azure Automation maken zonder het werken met code. Dit artikel bevat een inleiding tot het grafisch ontwerpen en alle informatie die nodig zijn om te beginnen met het maken van een grafisch runbook.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: afc1ec8e171bc602f2698b4a36f249bc454cbed9
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059438"
---
# <a name="graphical-authoring-in-azure-automation"></a>Grafisch ontwerpen in Azure Automation

Grafisch ontwerpen kunt u runbooks voor Azure Automation maken zonder de complexiteit van de onderliggende code van de Windows PowerShell of PowerShell-werkstroom. U activiteiten toevoegen aan het canvas uit een bibliotheek met cmdlets en runbooks, ze aan elkaar koppelen en configureren om een werkstroom. Als u ooit met System Center Orchestrator of Service Management Automation (SMA) hebt gewerkt, klikt u vervolgens ziet dit er waarschijnlijk vertrouwd voor u

Dit artikel bevat een inleiding tot het grafisch ontwerpen en de concepten die u nodig hebt om te beginnen bij het maken van een grafisch runbook.

## <a name="graphical-runbooks"></a>Grafische runbooks

Alle runbooks in Azure Automation zijn Windows PowerShell-werkstromen. Grafisch en grafische PowerShell Workflow-runbooks PowerShell-code die wordt uitgevoerd door de werknemers Automation genereren, maar u bent niet kunnen worden weergegeven of rechtstreeks wijzigen. Een grafisch runbook kan worden geconverteerd naar een grafische PowerShell Workflow-runbook en vice versa, maar ze kunnen niet worden geconverteerd naar een tekstuele runbook. Een bestaand tekstuele runbook kan niet worden geïmporteerd in de grafische editor.

## <a name="overview-of-graphical-editor"></a>Overzicht van de grafische editor

U kunt de grafische editor in Azure portal openen door te maken of bewerken van een grafisch runbook.

![Grafische werkruimte](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

De volgende secties beschrijven de besturingselementen in de grafische editor.

### <a name="canvas"></a>Canvas

Het Canvas is waar u uw runbook te ontwerpen. U activiteiten van de knooppunten in het besturingselement bibliotheek toevoegen aan het runbook en verbind deze met koppelingen voor het definiëren van de logica van het runbook.

U kunt de besturingselementen aan de onderkant van het canvas in-en uitzoomen.

### <a name="library-control"></a>Besturingselement bibliotheek

Het besturingselement bibliotheek is waar u het selecteert [activiteiten](#activities) om toe te voegen aan uw runbook. U toevoegen ze aan het papier waar u deze aan andere activiteiten verbinden. Het bevat vier secties wordt beschreven in de volgende tabel:

| Sectie | Beschrijving |
|:--- |:--- |
| Cmdlets |Bevat alle cmdlets die kunnen worden gebruikt in uw runbook. Cmdlets zijn ingedeeld door de module. Alle modules die u hebt geïnstalleerd in uw automation-account zijn beschikbaar. |
| Runbooks |Bevat de runbooks in uw automation-account. Deze runbooks kunnen worden toegevoegd aan het canvas om te worden gebruikt als onderliggende runbooks. Alleen runbooks van hetzelfde type als het runbook wordt bewerkt core worden weergegeven; Grafische worden runbooks alleen op PowerShell gebaseerde runbooks weergegeven, terwijl voor grafische PowerShell Workflow-runbooks alleen PowerShell-werkstroom runbooks worden weergegeven. |
| Assets |Bevat de [automation-assets](http://msdn.microsoft.com/library/dn939988.aspx) in uw automation-account die kan worden gebruikt in uw runbook. Wanneer u een asset aan een runbook toevoegt, wordt een workflow-activiteit die de geselecteerde asset wordt toegevoegd. In het geval van een variabele assets, kunt u selecteren of toevoegen van een activiteit als u wilt ophalen van de variabele of stelt u de variabele. |
| Runbookbesturing |Runbook-controleactiviteiten die kunnen worden gebruikt bevat in uw huidige runbook. Een *koppelingspunten* neemt van meerdere invoergegevens en wacht totdat alle hebt voltooid voordat de werkstroom. Een *Code* activiteit wordt uitgevoerd voor een of meer regels met code van PowerShell of PowerShell-werkstroom, afhankelijk van het type grafisch runbook. U kunt deze activiteit gebruiken voor aangepaste code of functionaliteit die moeilijk te bereiken met andere activiteiten. |

### <a name="configuration-control"></a>Configuratiebeheer

De configuratie-besturingselement is waar u details opgeven voor een object dat is geselecteerd op het canvas. De eigenschappen die beschikbaar zijn in dit besturingselement is afhankelijk van het type object dat is geselecteerd. Wanneer u een optie in het configuratie-besturingselement selecteert, wordt extra blades geopend zodat u meer informatie.

### <a name="test-control"></a>Besturingselement testen

Het besturingselement van de Test wordt niet weergegeven wanneer de grafische editor voor het eerst wordt gestart. Deze wordt geopend wanneer u interactief [testen van een grafisch runbook](#graphical-runbook-procedures).

## <a name="graphical-runbook-procedures"></a>Grafisch runbook procedures

### <a name="exporting-and-importing-a-graphical-runbook"></a>Exporteren en importeren van een grafisch runbook

U kunt alleen de gepubliceerde versie van een grafisch runbook exporteren. Als het runbook is nog niet gepubliceerd, dan zal de **exporteren** knop is uitgeschakeld. Wanneer u klikt op de **exporteren** knop, het runbook wordt gedownload naar uw lokale computer. De naam van het bestand overeenkomt met de naam van het runbook met een *graphrunbook* extensie.

U kunt een grafisch of grafische PowerShell Workflow-runbook-bestand importeren met het selecteren van de **importeren** optie bij het toevoegen van een runbook. Wanneer u het te importeren bestand selecteert, kunt u dezelfde bewaren **naam** of geef een nieuwe. Het veld Type van Runbook wordt het type van runbook weergegeven nadat deze het geselecteerde bestand beoordeelt en als u probeert om te selecteren van een ander type dan niet juist is, een bericht wordt weergegeven waarbij er mogelijke conflicten en tijdens de conversie, kunnen er syntaxis Er zijn fouten.

![Runbook importeren](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Een grafisch runbook testen

Terwijl u de gepubliceerde versie van het runbook ongewijzigd, of u een nieuw runbook testen kunt voordat deze is gepubliceerd, kunt u de conceptversie van een runbook testen in Azure portal. Hiermee kunt u controleren of het runbook correct werkt voordat u de gepubliceerde versie vervangen. Wanneer u een runbook test, wordt het concept runbook wordt uitgevoerd en de acties die worden uitgevoerd worden voltooid. Er wordt geen Taakgeschiedenis wordt gemaakt, maar uitvoer wordt weergegeven in het deelvenster Testuitvoer.

Open het Test-besturingselement voor een runbook door het openen van het runbook om te bewerken en klik vervolgens op de **testvenster** knop.

Het besturingselement van de Test wordt gevraagd voor alle invoerparameters en u het runbook start door te klikken op de **Start** knop.

### <a name="publishing-a-graphical-runbook"></a>Een grafisch runbook publiceren

Elk runbook in Azure Automation heeft een ontwerpversie en een gepubliceerde versie. Alleen de gepubliceerde versie is beschikbaar om te worden uitgevoerd, en alleen de conceptversie kan worden bewerkt. De gepubliceerde versie wordt niet beïnvloed door wijzigingen in de conceptversie. Wanneer de conceptversie nu is beschikbaar is, publiceert u deze, die wordt de gepubliceerde versie door de conceptversie overschreven.

U kunt een grafisch runbook publiceren door het openen van het runbook voor het bewerken en vervolgens te klikken op de **publiceren** knop.

Wanneer een runbook is nog niet gepubliceerd, heeft de status van **nieuw**. Wanneer het wordt gepubliceerd, heeft deze een status **gepubliceerd**. Als u het runbook bewerken nadat deze is gepubliceerd, en de conceptversie en een gepubliceerde versies verschillend zijn, heeft het runbook de status van **bewerken**.

![Runbook-statussen](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png)

U hebt ook de mogelijkheid om terug te keren naar de gepubliceerde versie van een runbook. Dit verwijdert u alle wijzigingen omdat het runbook het laatst is gepubliceerd en de conceptversie van het runbook door de gepubliceerde versie vervangt.

## <a name="activities"></a>Activiteiten

Activiteiten zijn de bouwstenen van een runbook. Een activiteit kan bestaan uit een PowerShell-cmdlet, een onderliggend runbook of een workflow-activiteit. Toevoegen van een activiteit aan het runbook door te selecteren met de rechtermuisknop op in het besturingselement bibliotheek **toevoegen aan papier**. U kunt Klik en sleep de activiteit overal op het canvas dat u wilt plaatsen. De locatie van de activiteit op het canvas heeft geen invloed op de werking van het runbook op geen enkele manier. U kunt indelen uw runbook echter vindt u het meest geschikt is voor het visualiseren van de werking ervan.

![Toevoegen aan papier](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Selecteer de activiteit op het canvas voor het configureren van de eigenschappen en parameters in de blade van de configuratie. U kunt wijzigen de **Label** van de activiteit in op iets dat beschrijvende aan u is. De oorspronkelijke cmdlet nog steeds wordt uitgevoerd, wijzigt u gewoon de weergavenaam die wordt gebruikt in de grafische editor. Het label moet uniek zijn binnen het runbook.

### <a name="parameter-sets"></a>Parametersets

Een parameterset definieert de verplichte en optionele parameters die waarden voor een bepaalde cmdlet te accepteren. Alle cmdlets hebben ten minste één parameter is ingesteld, en soms meerdere. Als een cmdlet meerdere parametersets bevat, moet klikt u vervolgens u selecteren welke methode u gebruikt voordat u parameters kunt configureren. De parameters die u kunt configureren, is afhankelijk van de parameter is ingesteld dat u kiest. Kunt u de parameterset gebruikt door een activiteit door het selecteren van **parameterset** en een andere verzameling te selecteren. In dit geval gaan alle parameterwaarden die u hebt geconfigureerd verloren.

In het volgende voorbeeld bevat de cmdlet Get-AzureRmVM drie parametersets. U kunt parameterwaarden niet configureren, totdat u een van de parametersets selecteren. De parameterset ListVirtualMachineInResourceGroupParamSet is voor het retourneren van alle virtuele machines in een resourcegroep en heeft één optionele parameter. De **GetVirtualMachineInResourceGroupParamSet** is voor het opgeven van de virtuele machine dat u wilt retourneren en heeft twee verplicht en een optionele parameter.

![Parameterset](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parameterwaarden

Als u een waarde voor een parameter opgeeft, selecteert u een gegevensbron om te bepalen hoe de waarde is opgegeven. De gegevensbronnen die beschikbaar zijn voor een bepaalde parameter, is afhankelijk van de geldige waarden voor deze parameter. Bijvoorbeeld: Null is niet een beschikbare optie voor een parameter die null-waarden zijn niet toegestaan.

| Gegevensbron | Beschrijving |
|:--- |:--- |
| Constante waarde |Typ een waarde voor de parameter. Dit is alleen beschikbaar voor de volgende gegevenstypen: Int32, Int64, tekenreeks, Boole-waarde, DateTime, Switch. |
| Uitvoer van activiteit |De uitvoer van een activiteit die voorafgaat aan de huidige activiteit in de werkstroom. Alle geldige activiteiten worden weergegeven. Selecteer alleen de activiteit met de uitvoer voor de waarde van parameter. Als de uitvoer van de activiteit een object met meerdere eigenschappen, kunt u opgeven de naam van de eigenschap na het selecteren van de activiteit. |
| Runbookinvoer |Selecteer een runbook-invoerparameter als invoer voor de parameter van de activiteit. |
| Variabeleasset |Selecteer een Automation-variabele als invoer. |
| Referentie-element |Selecteer een Automation-referentie als invoer. |
| Certificaatasset |Selecteer een Automation-certificaat als invoer. |
| Verbindingsasset |Selecteer een Automation-verbinding als invoer. |
| PowerShell-expressie |Geef een eenvoudige [PowerShell-expressie](#powershell-expressions). De expressie wordt geëvalueerd vóór de activiteit en het resultaat dat wordt gebruikt voor de parameterwaarde. U kunt variabelen gebruiken om te verwijzen naar de uitvoer van een activiteit of een runbook-invoerparameter. |
| Niet geconfigureerd |Hiermee schakelt u elke waarde die eerder was geconfigureerd. |

#### <a name="optional-additional-parameters"></a>Optionele extra parameters

Alle cmdlets hebben de optie voor het bieden van aanvullende parameters. Dit zijn algemene PowerShell-parameters of andere aangepaste parameters. Krijgt u een tekstvak waarin u de parameters met behulp van PowerShell-syntaxis kunt opgeven. Bijvoorbeeld, om te gebruiken de **uitgebreid** algemene parameter geeft u **'-Verbose: $True "**.

### <a name="retry-activity"></a>Opnieuw proberen van activiteit

**Gedrag voor opnieuw proberen** kunt u een activiteit meerdere keren worden uitgevoerd tot aan een bepaalde voorwaarde wordt voldaan, net als een lus. U kunt deze functie gebruiken voor activiteiten die meerdere keren moet worden uitgevoerd, is dit foutgevoelig zijn, en mogelijk moet meer dan één poging voor het slagen of testen van de informatie over de uitvoer van de activiteit voor geldige gegevens.

Wanneer u opnieuw proberen voor een activiteit inschakelt, kunt u een vertraging en een voorwaarde instellen. De vertraging is de tijd (gemeten in seconden of minuten) dat het runbook wacht voordat deze de activiteit opnieuw wordt uitgevoerd. Als er geen vertraging is opgegeven, worden klikt u vervolgens de activiteit opnieuw uitgevoerd onmiddellijk nadat deze is voltooid.

![Poging van de activiteit](media/automation-graphical-authoring-intro/retry-delay.png)

De voorwaarde voor opnieuw proberen is een PowerShell-expressie die wordt geëvalueerd na elke keer dat de activiteit wordt uitgevoerd. Als de expressie op ' True ' is opgelost, klikt u vervolgens de activiteit opnieuw uitgevoerd. De expressie wordt omgezet naar ONWAAR, wordt de activiteit niet opnieuw uitgevoerd als het runbook gaat verder met de volgende activiteit.

![Poging van de activiteit](media/automation-graphical-authoring-intro/retry-condition.png)

De voorwaarde opnieuw kunt gebruiken een variabele met de naam $RetryData die toegang tot informatie over de activiteit nieuwe pogingen biedt. Deze variabele heeft de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| NumberOfAttempts |Het aantal keren dat de activiteit is uitgevoerd. |
| Uitvoer |De uitvoer van de laatste uitvoering van de activiteit. |
| TotalDuration |Time-out verstreken sinds de eerste keer door de activiteit is gestart. |
| StartedAt |Tijd in UTC-notatie die de activiteit voor het eerst is gestart. |

Hieronder vindt u voorbeelden van de activiteit van de voorwaarden opnieuw.

```powershell-interactive
# Run the activity exactly 10 times.
$RetryData.NumberOfAttempts -ge 10
```

```powershell-interactive
# Run the activity repeatedly until it produces any output.
$RetryData.Output.Count -ge 1
```

```powershell-interactive
# Run the activity repeatedly until 2 minutes has elapsed.
$RetryData.TotalDuration.TotalMinutes -ge 2
```

Nadat u een voorwaarde voor opnieuw proberen voor een activiteit hebt geconfigureerd, bevat de activiteit twee visuele aanwijzingen om u te herinneren. Een wordt weergegeven in de activiteit en de andere is bij het controleren van de configuratie van de activiteit.

![Activiteit opnieuw proberen Visual indicatoren](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>Script werkstroombeheer

Een Code-besturingselement is een speciale activiteit PowerShell of PowerShell-werkstroom script afhankelijk van het type grafisch runbook wordt geschreven accepteert zodat de functionaliteit die anders mogelijk niet beschikbaar. Er kan geen parameters accepteren, maar deze variabelen kunt gebruiken voor activiteit uitvoer en runbook-invoerparameters. Uitvoer van de activiteit is toegevoegd aan de gegevensbus, tenzij er geen uitgaande koppelen in dat geval wordt deze toegevoegd aan de uitvoer van het runbook.

Bijvoorbeeld, berekeningen de volgende code datum met behulp van een runbook invoer variabele $NumberOfDays genoemd. Vervolgens stuurt een berekende tijd als uitvoer moet worden gebruikt door volgende activiteiten in het runbook.

```powershell-interactive
$DateTimeNow = (Get-Date).ToUniversalTime()
$DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
$DateTimeStart
```

## <a name="links-and-workflow"></a>Koppelingen en werkstroom

Een **koppeling** verbinding maakt twee activiteiten in een grafisch runbook. Als een pijl van de bronactiviteit naar de doelactiviteit wordt dit weergegeven op het canvas. De activiteiten worden uitgevoerd in de richting van de pijl met de doelactiviteit starten nadat de bronactiviteit is voltooid.

### <a name="create-a-link"></a>Een koppeling maken

Maak een koppeling tussen twee activiteiten op basis van de bronactiviteit selecteren en te klikken op de cirkel aan de onderkant van de vorm. Sleep de pijl naar de doelactiviteit en release.

![Een koppeling maken](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Selecteer de koppeling naar de eigenschappen ervan configureren in de blade van de configuratie. Dit omvat het koppelingstype, die wordt beschreven in de volgende tabel:

| Koppelingstype | Beschrijving |
|:--- |:--- |
| Pijplijn |De doelactiviteit wordt eenmaal uitgevoerd voor elk objectuitvoer van de bronactiviteit. De doelactiviteit wordt niet uitgevoerd als de bronactiviteit in geen uitvoer resulteert. Uitvoer van de bronactiviteit is beschikbaar als een object. |
| Volgorde |De doelactiviteit wordt slechts één keer uitgevoerd. Ontvangt deze een matrix met objecten uit de bronactiviteit. Uitvoer van de bronactiviteit is beschikbaar als een matrix met objecten. |

### <a name="starting-activity"></a>Activiteit vanaf

Een grafisch runbook begint met alle activiteiten die nog geen een binnenkomende verbinding. Dit is vaak slechts één activiteit, die als de eerste activiteit voor het runbook fungeren kan. Als meerdere activiteiten nog geen koppeling van een binnenkomende, vervolgens het runbook wordt gestart door kunnen parallel worden uitgevoerd. Volgt de koppelingen voor het uitvoeren van andere activiteiten zoals elk is voltooid.

### <a name="conditions"></a>Voorwaarden

Wanneer u een voorwaarde op een koppeling opgeeft, de doelactiviteit alleen wordt uitgevoerd als de voorwaarde true. Doorgaans gebruikt u een variabele $ActivityOutput in een voorwaarde om op te halen van de uitvoer van de bronactiviteit

Voor een pijplijn-koppeling, geeft u een voorwaarde voor een enkel object en de voorwaarde wordt geëvalueerd voor elke uitvoer van object door de bronactiviteit. De doelactiviteit wordt vervolgens uitgevoerd voor elk object dat voldoet aan de voorwaarde. Bijvoorbeeld, met de bronactiviteit van een van Get-AzureRmVm, de syntaxis van de volgende kan worden gebruikt voor een koppeling voorwaardelijke pijplijn om op te halen, alleen virtuele machines in de resourcegroep met de naam *Group1*.

```powershell-interactive
$ActivityOutput['Get Azure VMs'].Name -match "Group1"
```

Voor de koppeling van een reeks wordt de voorwaarde slechts één keer geëvalueerd omdat een matrix wordt geretourneerd met de uitvoer van alle objecten van de bronactiviteit. Als gevolg hiervan is de koppeling van een reeks kan niet worden gebruikt voor het filteren van als een pijplijn-koppeling, maar gewoon wordt bepaald of de volgende activiteit wordt uitgevoerd. Neem bijvoorbeeld de volgende set van activiteiten in ons runbook VM starten.

![Voorwaardelijke koppeling bij reeksen](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)

Er zijn drie verschillende reeks koppelingen die zijn deze waarden zijn opgegeven voor twee invoerparameters voor runbook voor VM-naam en de naam van de resourcegroep om te bepalen wat de juiste actie te ondernemen - verifiëren één virtuele machine starten, start alle virtuele machines in de resource groep, of alle virtuele machines in een abonnement. Dit is de voorwaardelijke logica voor de koppeling volgorde tussen verbinding maken met Azure en één Get-VM:

```powershell-interactive
<#
Both VMName and ResourceGroupName runbook input parameters have values
#>
(
(($VMName -ne $null) -and ($VMName.Length -gt 0))
) -and (
(($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
)
```

Wanneer u een voorwaardelijke koppeling gebruikt, worden de gegevens die beschikbaar zijn vanuit de bronactiviteit aan andere activiteiten in dat filiaal wordt gefilterd op de voorwaarde. Als een activiteit van de bron naar meerdere koppelingen is, klikt u vervolgens de gegevens beschikbaar zijn voor activiteiten in elke vertakking afhankelijk zijn van de voorwaarde in de koppeling die verbinding maken met dat filiaal.

Bijvoorbeeld, de **Start-AzureRmVm** activiteit in het onderstaande runbook wordt gestart voor alle virtuele machines. Heeft twee voorwaardelijke koppelingen. De expressie wordt gebruikt voor de eerste voorwaardelijke koppeling *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true* om te filteren als de Start-AzureRmVm-activiteit is voltooid. De tweede maakt gebruik van de expressie *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true* om te filteren als de Start-AzureRmVm-activiteit is de virtuele machine te starten is mislukt.

![Voorbeeld van een voorwaardelijke koppeling](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

Elke activiteit die volgt op de eerste koppeling en maakt gebruik van de activiteit uitvoer van Get-AzureVM krijgt alleen de virtuele machines die zijn gestart op het moment dat de Get-AzureVM werd uitgevoerd. Elke activiteit die volgt op de tweede koppeling wordt alleen de virtuele machines die zijn gestopt op het moment dat de Get-AzureVM werd uitgevoerd. Elke activiteit die de derde koppeling worden alle virtuele machines opgehaald, ongeacht hun actief is.

### <a name="junctions"></a>Koppelingen

Een verbinding is een speciale activiteit moet wachten totdat alle binnenkomende vertakkingen hebt voltooid. Hiermee kunt u meerdere activiteiten parallel uit te voeren en ervoor zorgen dat alle hebt voltooid voordat u doorgaat.

Terwijl een knooppunt een onbeperkt aantal inkomende koppelingen hebben kan, mag niet meer dan één van deze koppelingen zijn een pijplijn. Het aantal inkomende koppelingen van de takenreeks wordt niet beperkt. U mag het verbinding maken met meerdere inkomende koppelingen in de pijplijn en slaat u het runbook, maar deze mislukt wanneer deze wordt uitgevoerd.

Het onderstaande voorbeeld is onderdeel van een runbook die een set van virtuele machines tijdens het downloaden van patches worden toegepast op deze machines tegelijkertijd begint. Een verbinding wordt gebruikt om ervoor te zorgen dat beide processen zijn voltooid voordat het runbook wordt hervat.

![De verbinding](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Cycli

Een cyclus is als een doel activiteit bevat een koppeling terug naar de bronactiviteit of naar een andere activiteit die uiteindelijk teruggekoppeld naar de bron. Cycli worden momenteel niet toegestaan in het grafisch ontwerpen. Als uw runbook een cyclus heeft, juist opgeslagen maar ontvangt een foutbericht wanneer deze wordt uitgevoerd.

![Cyclus](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Delen van gegevens tussen activiteiten

Alle gegevens die wordt uitgevoerd door een activiteit met een uitgaande koppeling is geschreven naar de *gegevensbus* voor het runbook. Elke activiteit in het runbook kunt gegevens op de gegevensbus parameterwaarden vullen of opnemen in een script gebruiken. Een activiteit hebben toegang tot de uitvoer van een vorige activiteit in de werkstroom.

Hoe de gegevens worden geschreven in de gegevensbus, is afhankelijk van het type koppeling op de activiteit. Voor een **pijplijn**, de gegevens wordt uitgevoerd als veelvouden objecten. Voor een **reeks** koppeling, de gegevens wordt uitgevoerd als een matrix. Als er slechts één waarde, wordt deze uitgevoerd als een matrix van één element.

U kunt toegang tot gegevens op de gegevensbus met behulp van een van twee methoden. Eerst wordt met behulp van een **uitvoer van activiteit** gegevensbron voor het vullen van een parameter van een andere activiteit. Als de uitvoer een object is, kunt u één eigenschap opgeven.

![Uitvoer van activiteit](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

U kunt ook ophalen met de uitvoer van een activiteit in een **PowerShell-expressie** gegevensbron of vanuit een **Werkstroomscript** activiteit met een variabele ActivityOutput. Als de uitvoer een object is, kunt u één eigenschap opgeven. ActivityOutput variabelen gebruikt u de volgende syntaxis.

```powershell-interactive
$ActivityOutput['Activity Label']
$ActivityOutput['Activity Label'].PropertyName
```

### <a name="checkpoints"></a>Controlepunten

U kunt instellen [controlepunten](automation-powershell-workflow.md#checkpoints) in een grafische PowerShell Workflow-runbook door te selecteren *runbookcontrolepunt* voor elke activiteit. Dit zorgt ervoor dat een controlepunt worden ingesteld nadat de activiteit is uitgevoerd.

![Controlepunt](media/automation-graphical-authoring-intro/set-checkpoint.png)

Controlepunten worden alleen ingeschakeld in grafische PowerShell Workflow-runbooks, is niet beschikbaar in grafische runbooks. Als het runbook maakt gebruik van Azure-cmdlets, moet u een controlepunt activiteit met een Connect-AzureRmAccount volgen in het geval het runbook wordt onderbroken en opnieuw wordt opgestart vanaf dit controlepunt op een andere werknemer.

## <a name="authenticating-to-azure-resources"></a>Verificatie bij Azure-resources

Runbooks in Azure Automation die Azure-resources beheren vereist verificatie voor Azure. De [uitvoeren als-account](automation-create-runas-account.md) (ook wel een service-principal genoemd) is de standaardmethode voor toegang tot Azure Resource Manager-resources in uw abonnement met Automation-runbooks. U kunt deze functionaliteit toevoegen aan een grafisch runbook door toe te voegen de **AzureRunAsConnection** verbindingsasset, die met behulp van PowerShell [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) -cmdlet en [ Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet toe aan het papier. Dit wordt geïllustreerd in het volgende voorbeeld:

![Als verificatie activiteiten uitvoeren](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)

De activiteit uitvoeren als-verbinding ophalen (dat wil zeggen, Get-AutomationConnection) is geconfigureerd met een constante waarde gegevensbron met de naam AzureRunAsConnection.

![Run As-verbindingsconfiguratie](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)

De volgende activiteit, Connect-AzureRmAccount, voegt de geverifieerde uitvoeren als-account voor gebruik in het runbook.

![Connect-AzureRmAccount-parameterset](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)

> [!IMPORTANT]
> **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

Voor de parameters **APPLICATIONID**, **CERTIFICATETHUMBPRINT**, en **TENANTID** moet u de naam van de eigenschap voor het veldpad opgeven, omdat de activiteit de uitvoer van een object met meerdere eigenschappen. Anders wanneer u het runbook uit te voeren, mislukt dit proberen te verifiëren. Dit is wat u moet ten minste uw runbook met de uitvoeren als-account te verifiëren.

Onderhouden achterwaartse compatibiliteit voor abonnees die hebben gemaakt een Automation-account met een [Azure AD-gebruikersaccount](automation-create-aduser-account.md) voor het beheren van de klassieke Azure-implementatie of voor Azure Resource Manager-resources, de methode om te verifiëren, is de De cmdlet toevoegen-Azure-account met een [referentie-element](automation-credentials.md) dat een Active Directory-gebruiker met toegang op het Azure-account vertegenwoordigt.

U kunt deze functionaliteit toevoegen aan een grafisch runbook door een referentie-element toe te voegen aan het papier gevolgd door een Add-AzureAccount-activiteit. Toevoegen-Azure-account maakt gebruik van de activiteit van de referentie voor de invoer. Dit wordt geïllustreerd in het volgende voorbeeld:

![Verificatie-activiteiten](media/automation-graphical-authoring-intro/authentication-activities.png)

U moet verifiëren aan het begin van het runbook en na elk controlepunt. Dit betekent dat met het toevoegen van een activiteit toevoeging Add-AzureAccount na elke activiteit controlepunt-werkstroom. U hoeft niet een activiteit van de referentie toevoegen omdat u hetzelfde kunt gebruiken

![Uitvoer van activiteit](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Runbook invoer en uitvoer

### <a name="runbook-input"></a>Runbookinvoer

Een runbook mogelijk invoer van een gebruiker wanneer het runbook via Azure portal wordt gestart of vanuit een ander runbook als de huidige sleutel wordt gebruikt als een onderliggend element.
Als u een runbook dat wordt gemaakt van een virtuele machine hebt, moet u er bijvoorbeeld voor kiezen om informatie te geven, zoals de naam van de virtuele machine en andere eigenschappen telkens wanneer u het runbook start.

Gaat u akkoord met de invoer voor een runbook door te definiëren een of meer-invoerparameters. U opgeven waarden voor deze parameters telkens wanneer die het runbook wordt gestart. Wanneer u een runbook met de Azure-portal start, wordt u gevraagd waarden op te geven voor elk van de invoerparameters van het runbook.

Invoerparameters voor een runbook toegankelijk is door te klikken op de **invoer en uitvoer** op de werkbalk van het runbook.

Hiermee opent u de **invoer en uitvoer** besturingselement kunt u een bestaande invoerparameter bewerken of een nieuwe maken door te klikken op **invoer toevoegen**.

![Invoer toevoegen](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Elke invoerparameter wordt gedefinieerd door de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| Naam |De unieke naam van de parameter. Dit mag alleen alfanumerieke tekens en mag geen spatie bevatten. |
| Beschrijving |Een optionele beschrijving voor de invoerparameter. |
| Type |Het gegevenstype voor de parameterwaarde verwacht. De Azure portal biedt een juiste besturingselement voor het gegevenstype voor elke parameter tijdens het vragen om invoer. |
| Verplicht |Hiermee geeft u op of moet een waarde worden opgegeven voor de parameter. Het runbook kan niet worden gestart als u geen waarde opgeeft voor de verplichte parameter waarmee de heeft geen standaardwaarde gedefinieerd. |
| Standaardwaarde |Hiermee geeft u op welke waarde voor de parameter wordt gebruikt als niet is opgegeven. Dit kan ofwel Null of een specifieke waarde zijn. |

### <a name="runbook-output"></a>Runbook-uitvoer

Gemaakt door elke activiteit die een uitgaande verbinding heeft geen gegevens worden opgeslagen in de [uitvoer van het runbook](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages). De uitvoer wordt opgeslagen met de runbooktaak en is beschikbaar voor een bovenliggend runbook wanneer het runbook wordt gebruikt als een onderliggend element.

## <a name="powershell-expressions"></a>PowerShell-expressies

Een van de voordelen van het grafisch ontwerpen biedt u de mogelijkheid om het bouwen van een runbook met minimale kennis van PowerShell. Op dit moment u hoeft te weten een deel van de PowerShell al voor het invullen van bepaalde [parameterwaarden](#activities) en voor de instelling [koppelen voorwaarden](#links-and-workflow). Deze sectie bevat een korte inleiding in expressies voor gebruikers die mogelijk niet bekend bent met het PowerShell. Volledige details van PowerShell zijn beschikbaar op [met Windows PowerShell-scripts](http://technet.microsoft.com/library/bb978526.aspx).

### <a name="powershell-expression-data-source"></a>Gegevensbron voor PowerShell-expressie
U kunt een PowerShell-expressie gebruiken als een gegevensbron voor het vullen van de waarde van een [activiteitsparameter](#activities) met de resultaten van enige PowerShell-code. Dit kan een enkele regel code waarmee u een eenvoudige functie of meerdere regels die bepaalde complexe logica uitvoeren. Elke uitvoer van een opdracht dat niet is toegewezen aan een variabele worden uitgevoerd in de waarde van parameter.

De volgende opdracht zou bijvoorbeeld uitvoer van de huidige datum.

De volgende opdracht zou bijvoorbeeld uitvoer van de huidige datum.

```powershell-interactive
Get-Date
```

De volgende opdrachten opbouwen van een tekenreeks van de huidige datum en deze toewijzen aan een variabele. De inhoud van de variabele worden vervolgens naar de uitvoer verzonden

```powershell-interactive
$string = "The current date is " + (Get-Date)
$string
```

De volgende opdrachten de huidige datum geëvalueerd en geretourneerd van een tekenreeks die aangeeft of de huidige dag een weekend of weekdag.

```powershell-interactive
$date = Get-Date
if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
else { "Weekday" }
```

### <a name="activity-output"></a>Uitvoer van activiteit

Gebruik de variabele $ActivityOutput met de volgende syntaxis voor het gebruik van de uitvoer van een voorgaande activiteit in het runbook.

```powershell-interactive
$ActivityOutput['Activity Label'].PropertyName
```

Bijvoorbeeld, wellicht u een activiteit met een eigenschap die u de naam van een virtuele machine moet in dat geval kunt u de volgende expressie:

```powershell-interactive
$ActivityOutput['Get-AzureVm'].Name
```

Als de eigenschap die de virtuele machine vereist in plaats van alleen een eigenschap object, zou u het gehele object met de volgende syntaxis retourneren.

```powershell-interactive
$ActivityOutput['Get-AzureVm']
```

U kunt de uitvoer van een activiteit ook gebruiken in een complexe expressie, zoals de volgende tekst naar de naam van de virtuele machine worden samengevoegd.

```powershell-interactive
"The computer name is " + $ActivityOutput['Get-AzureVm'].Name
```

### <a name="conditions"></a>Voorwaarden

Gebruik [vergelijkingsoperators](https://technet.microsoft.com/library/hh847759.aspx) waarden vergelijken of kunt vaststellen of een waarde overeenkomt met een opgegeven patroon. Een vergelijking retourneert een waarde van $true en $false.

Bijvoorbeeld, de volgende voorwaarde wordt bepaald of de virtuele machine van een activiteit met de naam *Get-AzureVM* is momenteel *gestopt*.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"
```

De volgende voorwaarden controles of dezelfde virtuele machine anders dan in een staat is *gestopt*.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"
```

U kunt deelnemen aan meerdere voorwaarden met behulp van een [logische operator](https://technet.microsoft.com/library/hh847789.aspx) zoals **- en** of **- of**. Bijvoorbeeld, de volgende controles voorwaarde of dezelfde virtuele machine in het vorige voorbeeld heeft de status van *gestopt* of *stoppen*.

```powershell-interactive
($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping")
```

### <a name="hashtables"></a>Hashtabellen

[Hashtabellen](http://technet.microsoft.com/library/hh847780.aspx) zijn naam/waarde-paren die nuttig zijn voor een set waarden geretourneerd. Eigenschappen voor bepaalde activiteiten kunnen een hash-tabel in plaats van een eenvoudige waarde verwacht. U ziet ook als hash-tabel aangeduid als een woordenlijst.

U maakt een hash-tabel met de volgende syntaxis. Een hash-tabel kan een onbeperkt aantal vermeldingen bevatten, maar elk wordt gedefinieerd door een naam en waarde.

```powershell-interactive
@{ <name> = <value>; [<name> = <value> ] ...}
```

De volgende expressie maakt bijvoorbeeld een hash-tabel moet worden gebruikt in de gegevensbron voor de activiteitsparameter van een die een hash-tabel met de waarden voor een zoekopdracht internet verwacht.

```powershell-interactive
$query = "Azure Automation"
$count = 10
$h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
$h
```

Het volgende voorbeeld wordt de uitvoer van een activiteit met de naam *Twitter-verbinding ophalen* voor het vullen van een hash-tabel.

```powershell-interactive
@{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
    'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
    'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
    'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}
```

## <a name="next-steps"></a>Volgende stappen

* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie voor meer informatie over hoe u verifieert met behulp van het Automation uitvoeren als-account, [configureren Azure uitvoeren als-Account](automation-sec-configure-azure-runas-account.md)
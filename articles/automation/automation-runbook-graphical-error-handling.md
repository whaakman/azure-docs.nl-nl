---
title: Foutafhandeling in grafische Azure Automation-runbooks | Microsoft Docs
description: In dit artikel wordt beschreven hoe u foutafhandelingslogica kunt implementeren in grafische Azure Automation-runbooks.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: f9b359691122da9e5d93e51f3085cad51e55d8f2
ms.openlocfilehash: 13c3e1693badcf4148738cb63666f34546d1696c

---

# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Foutafhandeling in grafische Azure Automation-runbooks

Houd bij het ontwerpen van runbooks rekening met het volgende: ze identificeren verschillende zaken die zich kunnen voordoen in runbooks, zoals geslaagde pogingen, verwachte foutstatussen en onverwachte foutvoorwaarden.  Runbooks moeten foutafhandeling bevatten om fouten naar behoren te kunnen detecteren.  Als u de uitvoer van een activiteit wilt valideren of een fout op de juiste manier wilt verwerken, gebruikt u in het geval van grafische runbooks waarschijnlijk een PowerShell-codeactiviteit, definieert u de voorwaardelijke logica voor de uitvoerkoppeling van de activiteit of past u een andere methode toe.          

Wanneer er bij de uitvoering van runbooks een niet-afsluitfout optreedt tijdens een activiteit, wordt de activiteit die hierop volgt vaak toch verwerkt.  Natuurlijk wordt er waarschijnlijk een uitzondering gegenereerd, maar het punt is dat de volgende activiteit wel mag worden uitgevoerd. Dit komt door de manier waarop de PowerShell-taal is ontworpen voor het verwerken van fouten.    

De typen PowerShell-fouten die zich kunnen voordoen tijdens de uitvoering, zijn afsluitfouten of niet-afsluitfouten.  Het verschil bestaat hierin:

* Afsluitfout: een serieuze fout tijdens het uitvoeren waardoor de opdracht (of de uitvoering van het script) volledig wordt gestopt. Voorbeelden zijn onder andere niet-bestaande cmdlets, syntaxisfouten waardoor een cmdlet niet kan worden uitgevoerd of andere fatale fouten.

* Niet-afsluitfout: een niet-ernstige fout waarbij de uitvoering ondanks de fout toch wordt voortgezet. Voorbeelden zijn onder andere operationele fouten zoals niet-gevonden bestanden, machtigingsproblemen, enzovoort.

Grafische Azure Automation-runbooks zijn verbeterd. Ze bieden nu de mogelijkheid om foutafhandeling op te nemen, zodat u van uitzonderingen niet-afsluitfouten kunt maken en foutkoppelingen kunt maken tussen activiteiten. Hierdoor kunnen met een runbookauteur fouten worden gedetecteerd en kan er een pad worden gemaakt om de gerealiseerde of onverwachte voorwaarde te beheren.  

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Het is belangrijk om de werkstroom te controleren zodat u, wanneer tijdens een kritieke activiteit een fout of uitzondering optreedt, kunt voorkomen dat de volgende activiteit in het runbook wordt uitgevoerd en de fout naar behoren kan worden afgehandeld.  Dit is vooral noodzakelijk wanneer uw runbooks ondersteuning bieden voor een bedrijfsproces of servicebewerking.

Voor elke activiteit die een fout kan veroorzaken, kan de runbookauteur een foutkoppeling toevoegen die verwijst naar een willekeurige andere activiteit.  De doelactiviteit kan elk type zijn: codeactiviteit, aanroepen van een cmdlet, aanroepen van een ander runbook of iets anders. 

Bovendien kan de doelactiviteit ook uitgaande koppelingen hebben. Dit kunnen gewone koppelingen zijn of foutkoppelingen. De runbookauteur kan dus complexe foutafhandelingslogica implementeren zonder dat een codeactiviteit hoeft te worden opgenomen.  De aanbevolen procedure is om een toegewezen runbook voor foutafhandeling te maken met algemene functionaliteit. Dit is echter niet verplicht en foutafhandelingslogica in een PowerShell-codeactiviteit is niet het enige alternatief.  

Voorbeeld: als een runbook een VM probeert te starten en hier een toepassing op wil installeren, maar de VM niet juist start, worden er twee acties uitgevoerd: 

1. Er wordt een melding over dit probleem verzonden.
2. Er wordt een ander runbook gestart dat in plaats hiervan automatisch een nieuwe VM inricht. 

Een mogelijke oplossing is om een foutkoppeling die wijst naar een activiteit om stap 1 af te handelen, zoals de **Write-Warning**-cmdlet, te koppelen aan een activiteit voor stap 2, zoals de **Start-AzureRmAutomationRunbook**-cmdlet. 

U kunt dit gedrag ook generaliseren voor gebruik in meerdere runbooks en deze twee activiteiten in afzonderlijke runbooks voor foutafhandeling plaatsen, zoals eerder is voorgesteld.  Voordat u dit runbook voor foutafhandeling aanroept, kunt u een aangepast bericht opstellen uit de gegevens van het oorspronkelijke runbook en dit bericht vervolgens als een parameter doorgeven aan het runbook voor foutafhandeling. 

## <a name="how-to-use"></a>Gebruiksinstructies

Elke activiteit is geconfigureerd om van uitzonderingen niet-afsluitfouten te maken. Deze optie is standaard uitgeschakeld.  U moet dit inschakelen voor elke activiteit waarvoor u fouten wilt verwerken.  Als deze configuratie wordt ingeschakeld, worden zowel afsluitfouten als niet-afsluitfouten in de activiteit verwerkt als niet-afsluitfouten. Deze kunnen vervolgens worden verwerkt met een foutkoppeling.  Na het configureren van deze instelling maakt u een activiteit om deze fout af te handelen.  Als een activiteit een fout veroorzaakt, worden de uitgaande foutkoppelingen gevolgd en de reguliere koppelingen niet, zelfs als een activiteit ook reguliere uitvoer heeft geproduceerd.<br><br> ![Voorbeeld van foutkoppeling voor Automation-runbook](media/automation-runbook-graphical-error-handling/error-link-example.png)

In het volgende eenvoudige voorbeeld wordt met een runbook een variabele opgehaald die de computernaam bevat van een virtuele machine. Vervolgens wordt geprobeerd de virtuele machine te starten met de volgende activiteit.<br><br> ![Voorbeeld van foutafhandeling in Automation-runbook](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

De activiteiten **Get-AutomationVariable** en **Start AzureRmVm** zijn geconfigureerd om uitzonderingen te converteren naar fouten.  Als zich problemen voordoen bij het ophalen van de variabele of bij het starten van de virtuele machine, worden er fouten gegenereerd.<br><br> ![Activiteitsinstellingen voor foutafhandeling in Automation-runbook](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Foutkoppelingen stromen van deze activiteiten naar een enkele activiteit voor **Foutbeheer** (een codeactiviteit), die is geconfigureerd met een eenvoudige PowerShell-expressie met behulp van het trefwoord *Throw* om het verwerken te stoppen samen met *$Error.Exception.Message* om het bericht op te halen waarin de huidige uitzondering wordt beschreven.<br><br> ![Codevoorbeeld voor foutafhandeling in Automation-runbook](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Volgende stappen

* Zie [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow) voor meer informatie over koppelingen en voor meer inzicht in koppelingstypen in grafische runbooks.

* Zie [Track a runbook job (Runbooktaken bijhouden)](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details 


<!--HONumber=Jan17_HO1-->



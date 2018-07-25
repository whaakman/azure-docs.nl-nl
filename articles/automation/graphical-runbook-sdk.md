---
title: Overzicht van het runbook in Azure Automation Grpahical SDK
description: In dit artikel wordt beschreven hoe u de SDK voor Azure Automation Runbook grafische
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 07/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 702af8311887afc94e7127704d3377e944503324
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239883"
---
# <a name="use-the-azure-automation-graphical-runbook-sdk"></a>Gebruik de SDK een Azure Automation grafisch runbook

[Grafische runbooks](automation-graphical-authoring-intro.md) zijn die het beheer van de complexiteit van de onderliggende code van de Windows PowerShell of PowerShell Workflow-runbooks. De Microsoft Azure Automation-SDK voor grafisch ontwerpen kunnen ontwikkelaars maken en bewerken van grafische Runbooks voor gebruik met Azure Automation-service. De volgende codefragmenten laten de basisprincipes van het maken van een grafisch runbook vanuit uw code.

## <a name="pre-requisites"></a>Vereisten

Als u wilt starten, importeert de `Microsoft.Azure.Automation.GraphicalRunbook.Model` pakket in uw project.

## <a name="create-a-runbook-object-instance"></a>Een exemplaar van het object runbook maken

Naslaginformatie over de `Orchestrator.GraphRunbook.Model` assembly en maak een instantie van de `Orchestrator.GraphRunbook.Model.GraphRunbook` klasse:

```csharp
using Orchestrator.GraphRunbook.Model;
using Orchestrator.GraphRunbook.Model.ExecutableView;

var runbook = new GraphRunbook();
```

## <a name="add-runbook-parameters"></a>Runbookparameters toevoegen

Instantiëren `Orchestrator.GraphRunbook.Model.Parameter` objecten en toe te voegen aan het runbook:

```csharp
runbook.AddParameter(
 new Parameter("YourName") {
  TypeName = typeof(string).FullName,
   DefaultValue = "World",
   Optional = true
 });

runbook.AddParameter(
 new Parameter("AnotherParameter") {
  TypeName = typeof(int).FullName, ...
 });
```

## <a name="add-activities-and-links"></a>Activiteiten en koppelingen toevoegen

Activiteiten van de juiste typen instantiëren en toe te voegen aan het runbook:

```csharp
var writeOutputActivityType = new CommandActivityType {
 CommandName = "Write-Output",
  ModuleName = "Microsoft.PowerShell.Utility",
 InputParameterSets = new [] {
  new ParameterSet {
   Name = "Default",
    new [] {
     new Parameter("InputObject"), ...
    }
  },
  ...
 }
};

var outputName = runbook.AddActivity(
 new CommandActivity("Output name", writeOutputActivityType) {
  ParameterSetName = "Default",
   Parameters = new ActivityParameters {
    {
     "InputObject",
     new RunbookParameterValueDescriptor("YourName")
    }
   },
   PositionX = 0,
   PositionY = 0
 });

var initializeRunbookVariable = runbook.AddActivity(
 new WorkflowScriptActivity("Initialize variable") {
  Process = "$a = 0",
   PositionX = 0,
   PositionY = 100
 });
```

Activiteiten zijn geïmplementeerd door de volgende klassen in de `Orchestrator.GraphRunbook.Model` naamruimte:

|Klasse  |Activiteit  |
|---------|---------|
|CommandActivity     | Hiermee wordt een PowerShell-opdracht (cmdlet, functie, enz.).        |
|InvokeRunbookActivity     | Een ander runbook inline aanroept.        |
|JunctionActivity     | Wacht tot alle binnenkomende vertakkingen te voltooien.        |
|WorkflowScriptActivity     | Voert een blok van code van PowerShell of PowerShell-werkstroom (afhankelijk van het runbooktype) in de context van het runbook. Dit is een krachtig hulpprogramma, maar niet overmatig gebruik deze: de gebruikersinterface ziet u deze scriptblok als tekst. de engine voor het uitvoeren wordt het opgegeven blok behandelen als een zwart vak en doen geen pogingen om de inhoud, met uitzondering van een selectievakje basissyntaxis analyseren. Als u slechts één PowerShell-opdracht aanroepen moet, krijgt u CommandActivity voorkeur.        |

> [!NOTE]
> Voer niet uw eigen activiteiten afgeleid van de opgegeven klassen: Azure Automation niet mogelijk het gebruik van runbooks met aangepaste activiteitstypen.

Parameters CommandActivity en InvokeRunbookActivity moeten worden opgegeven als waarde descriptors, waarden niet direct. Waarde descriptors opgeven hoe de waarden van de feitelijke parameter moeten worden gemaakt. De volgende waarde descriptors zijn momenteel beschikbaar:


|Descriptor  |Definitie  |
|---------|---------|
|ConstantValueDescriptor     | Verwijst naar een vastgelegde constante waarde.        |
|RunbookParameterValueDescriptor     | Verwijst naar een runbook-parameter met de naam.        |
|ActivityOutputValueDescriptor     | Verwijst naar een upstream-activiteit uitvoer, zodat één activiteit voor een 'abonnement' op gegevens die door een andere activiteit worden geproduceerd.        |
|AutomationVariableValueDescriptor     | Verwijst naar een Automation-variabele asset met de naam.         |
|AutomationCredentialValueDescriptor     | Verwijst naar een Automation-certificaatasset met de naam.        |
|AutomationConnectionValueDescriptor     | Verwijst naar een Automation-verbindingsasset met de naam.        |
|PowerShellExpressionValueDescriptor     | Hiermee geeft u een PowerShell-expressie voor vrije tekst die wordt geëvalueerd vlak voor het aanroepen van de activiteit.  <br/>Dit is een krachtig hulpprogramma, maar niet overmatig gebruik deze: de gebruikersinterface wordt weergegeven in deze expressie als tekst. de engine voor het uitvoeren wordt het opgegeven blok behandelen als een zwart vak en doen geen pogingen om de inhoud, met uitzondering van een selectievakje basissyntaxis analyseren. Indien mogelijk meer specifieke waarde descriptors de voorkeur.      |

> [!NOTE]
> Voer uw eigen waarde descriptors niet afgeleid van de opgegeven klassen: Azure Automation niet mogelijk het gebruik van runbooks met aangepaste waarde descriptor typen.

Exemplaar maken van koppelingen activiteiten verbinding te maken en toe te voegen aan het runbook:

```csharp
runbook.AddLink(new Link(activityA, activityB, LinkType.Sequence));

runbook.AddLink(
 new Link(activityB, activityC, LinkType.Pipeline) {
  Condition = string.Format("$ActivityOutput['{0}'] -gt 0", activityB.Name)
 });
```

## <a name="save-the-runbook-to-a-file"></a>Het runbook in een bestand opslaan

Gebruik `Orchestrator.GraphRunbook.Model.Serialization.RunbookSerializer` het serialiseren van een runbook naar een tekenreeks:

```csharp
var serialized = RunbookSerializer.Serialize(runbook);
```

Deze tekenreeks kan worden opgeslagen als een bestand met de **.graphrunbook** extensie en dit bestand kan worden geïmporteerd in Azure Automation.
De geserialiseerde indeling kan worden gewijzigd in de toekomst versies van `Orchestrator.GraphRunbook.Model.dll`. We beloven compatibiliteit met eerdere versies: een runbook dat is geserialiseerd met een oudere versie van `Orchestrator.GraphRunbook.Model.dll` met een nieuwere versie kunnen worden gedeserialiseerd. Voorwaartse compatibiliteit kan niet worden gegarandeerd: een runbook dat is geserialiseerd met een nieuwere versie is mogelijk niet opnieuw worden geserialiseerd met oudere versies.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over grafische Runbooks in Azure Automation, [inleiding grafisch ontwerpen](automation-graphical-authoring-intro.md)

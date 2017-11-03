---
title: Begin met het ontwikkelen met Visual Studio-projectsjablonen - Azure Batch-oplossingen | Microsoft Docs
description: Meer informatie over hoe sjablonen voor Visual Studio-project kunt implementeren en uitvoeren van uw rekenintensieve workloads op Azure Batch.
services: batch
documentationcenter: .net
author: fayora
manager: timlt
editor: 
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: da77ce827c65deb18d9d84ce5cf768d89788e205
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-visual-studio-project-templates-to-jump-start-batch-solutions"></a>Gebruik Visual Studio-projectsjablonen Batch-oplossingen snel op

De **Job Manager** en **taak Processor Visual Studio sjablonen** bieden code om te implementeren en uitvoeren van uw rekenintensieve workloads op Batch met zo min mogelijk moeite voor Batch. Dit document beschrijft deze sjablonen en biedt richtlijnen voor het gebruik ervan.

> [!IMPORTANT]
> In dit artikel vindt u alleen informatie die van toepassing op deze twee sjablonen en wordt ervan uitgegaan dat u bekend met de Batch-service en de belangrijkste concepten die betrekking hebben op het bent: pools, rekenknooppunten, taken en taken, jobbeheertaken, omgevingsvariabelen en andere relevante informatie. U vindt meer informatie in [basisbeginselen van Azure Batch](batch-technical-overview.md), [overzicht van de Batch-functies voor ontwikkelaars](batch-api-basics.md), en [aan de slag met de Azure Batch-bibliotheek voor .NET](batch-dotnet-get-started.md).
> 
> 

## <a name="high-level-overview"></a>Overzicht
De Job Manager en de processorsnelheid van de taak sjablonen kunnen worden gebruikt voor het maken van twee nuttig onderdelen:

* Een jobbeheertaak waarmee de splitser van een taak die een taak verbreken kunt omlaag in meerdere taken die onafhankelijk, parallel kunnen worden uitgevoerd.
* Een taak processor dat voorverwerking en naverwerking rond een toepassing vanaf de opdrachtregel uit te voeren kan worden gebruikt.

Bijvoorbeeld in een scenario film rendering van zou de splitser taak zet een taak één film in honderden of duizenden afzonderlijke taken die afzonderlijke frames afzonderlijk wilt verwerken. : De taak processor de van renderingtoepassing zou aanroepen en alle afhankelijke processen die nodig zijn om weer te geven elk frame, evenals aanvullende acties (bijvoorbeeld het gerenderde frame kopiëren naar een opslaglocatie) uitvoeren.

> [!NOTE]
> De Job Manager en de processorsnelheid van de taak sjablonen zijn onafhankelijk van elkaar, zodat u kiezen kunt om beide of slechts één van beide, afhankelijk van de vereisten van uw compute-taak en op uw voorkeuren te gebruiken.
> 
> 

Zoals u in het onderstaande diagram, doorlopen een compute-taak die gebruikmaakt van deze sjablonen drie fasen:

1. De clientcode (bijvoorbeeld toepassingen, web-service, enzovoort) verzendt een taak met de Batch-service op Azure, zoals de manager van de taak van de taak programma voor de taak opgeven.
2. De jobbeheertaak wordt in de Batch-service op een rekenknooppunt uitgevoerd en de splitser taak start het opgegeven aantal taak processor taken op als veel waar nodig rekenknooppunten, op basis van de parameters en -specificaties in de code van de splitser taak.
3. De taken van de processor taak uitgevoerd onafhankelijk, parallel voor de invoergegevens verwerkt en het genereren van de uitvoergegevens.

![Diagram die weergeeft hoe clientcode samenwerkt met de Batch-service][diagram01]

## <a name="prerequisites"></a>Vereisten
De Batch als sjablonen wilt gebruiken, moet u het volgende:

* Een computer met Visual Studio 2015 geïnstalleerd. Batch-sjablonen zijn momenteel alleen ondersteund voor Visual Studio 2015.
* De Batch-sjablonen, die beschikbaar zijn vanuit de [Visual Studio-galerie] [ vs_gallery] als Visual Studio-extensies. Er zijn twee manieren om de sjablonen te verkrijgen:
  
  * Installeren van de sjablonen met behulp van de **uitbreidingen en Updates** dialoogvenster in Visual Studio (Zie voor meer informatie [zoeken en met behulp van Visual Studio Extensions][vs_find_use_ext]). In de **uitbreidingen en Updates** dialoogvenster vak, zoeken en downloaden van de volgende twee extensies:
    
    * Azure Batch-Job Manager met de taak splitsen
    * Processor van Azure Batch-taak
  * De sjablonen gedownload vanuit de online-galerie voor Visual Studio: [projectsjablonen voor Microsoft Azure-Batch][vs_gallery_templates]
* Als u wilt gebruiken de [toepassingspakketten](batch-application-packages.md) functie voor het implementeren van de manager van de taak en taak processor naar de Batch-rekenknooppunten, moet u een opslagaccount aan uw Batch-account koppelen.

## <a name="preparation"></a>Voorbereiding
Het is raadzaam om een oplossing met uw job manager, evenals de processor taak maken omdat hierdoor kunt u gemakkelijker code tussen uw job manager en de taak processor-programma's te delen. Volg deze stappen voor het maken van deze oplossing:

1. Open Visual Studio en selecteer **bestand** > **nieuw** > **Project**.
2. Onder **sjablonen**, vouw **andere projecttypen**, klikt u op **Visual Studio-oplossingen**, en selecteer vervolgens **blanco-oplossing**.
3. Typ een naam voor uw toepassing en het doel van deze oplossing (bijvoorbeeld ' LitwareBatchTaskPrograms').
4. Klik op om de nieuwe oplossing **OK**.

## <a name="job-manager-template"></a>Taak Manager-sjabloon
De sjabloon Job Manager helpt u bij het implementeren van een jobbeheertaak die u kunt de volgende acties uitvoeren:

* Een job onderverdeeld in meerdere taken.
* Deze taken uit te voeren op Batch verzenden.

> [!NOTE]
> Zie voor meer informatie over jobbeheertaken [overzicht van de Batch-functies voor ontwikkelaars](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-the-template"></a>Een Job Manager met behulp van de sjabloon maken
Als u wilt een taak toevoegen aan de oplossing die u eerder hebt gemaakt, de volgende stappen uit:

1. Open uw bestaande oplossing in Visual Studio.
2. Klik in Solution Explorer met de rechtermuisknop op de oplossing, klikt u op **toevoegen** > **nieuw Project**.
3. Onder **Visual C#**, klikt u op **Cloud**, en klik vervolgens op **Azure Batch-Job Manager met taak splitser**.
4. Typ een naam waarmee u een beschrijving van uw toepassing en dit project (bijvoorbeeld als de beheerder van de taak identificeert 'LitwareJobManager').
5. Klik op om het project **OK**.
6. Bouw ten slotte het project om af te dwingen van Visual Studio laden alle waarnaar wordt verwezen NuGet-pakketten en om te controleren of het project geldig is voordat u begint met het wijzigen van deze.

### <a name="job-manager-template-files-and-their-purpose"></a>De sjabloonbestanden Job Manager en hun doel
Wanneer u een project met behulp van de Job Manager-sjabloon maakt, genereert het codebestanden drie groepen:

* Het belangrijkste programmabestand (Program.cs). Dit document bevat de programma-ingangspunt en uitzonderingsverwerking op het hoogste niveau. U mag niet normaal gesproken moet dit wijzigen.
* De Framework-map. Dit bevat de bestanden die verantwoordelijk is voor het 'standaard'-werk dat door het programma job manager – uitpakken van de parameters, taken toe te voegen aan de Batch-job, enzovoort. U mag niet normaal gesproken moet deze bestanden wijzigen.
* De taak splitser-bestand (JobSplitter.cs). Dit is waar u uw toepassingsspecifieke logica voor het splitsen van een taak in taken wordt geplaatst.

Natuurlijk kunt u aanvullende bestanden toevoegen als vereist ter ondersteuning van uw taak splitser code, afhankelijk van de complexiteit van de taak logica splitsen.

De sjabloon genereert ook standaard .NET projectbestanden zoals een .csproj-bestand, app.config, packages.config, enz.

De rest van deze sectie beschrijft de verschillende bestanden en de codestructuur en wordt uitgelegd wat elke klasse doet.

![Visual Studio Solution Explorer toont de Job Manager sjabloonoplossing][solution_explorer01]

**Framework-bestanden**

* `Configuration.cs`: Het laden van gegevens van de taak configuratie zoals details van de Batch-account, gekoppelde opslagaccountreferenties taak en taakgegevens en taakparameters ingekapseld. Bovendien wordt toegang tot Batch gedefinieerde omgevingsvariabelen (Zie omgevingsinstellingen voor taken in de documentatie van de Batch) via de Configuration.EnvironmentVariable-klasse.
* `IConfiguration.cs`: Hiermee isoleert de implementatie van de Configuration-klasse, zodat u kunt testen van eenheden uw taak splitser met behulp van een valse of mock configuration-object.
* `JobManager.cs`: De onderdelen van het programma job manager ingedeeld. Het is verantwoordelijk voor het initialiseren van de splitser taak, het aanroepen van de splitser taak en tijdens het verzenden van de taken die zijn geretourneerd door de taak splitser naar de aanvrager taak.
* `JobManagerException.cs`: Hiermee geeft u een fout is van de manager van de taak is beëindigd. JobManagerException wordt laten teruglopen 'verwachte' fouten waarin specifieke diagnostische informatie kan worden opgegeven als onderdeel van de beëindiging gebruikt.
* `TaskSubmitter.cs`: Deze klasse is die verantwoordelijk zijn voor het toevoegen van taken die zijn geretourneerd door de splitser taak op de batchverwerking. De aggregaties van de klasse Taakbeheer heeft de volgorde van de taken in batches efficiënt, maar tijdig toegevoegd aan de taak vervolgens roept TaskSubmitter.SubmitTasks in een achtergrondthread voor elke batch.

**Taak splitsen**

`JobSplitter.cs`: Met deze klasse bevat toepassingsspecifieke logica voor het verdelen van de taak in de taken. Het framework roept de methode JobSplitter.Split voor het verkrijgen van een reeks van taken, die wordt toegevoegd aan het project als de methode ze retourneert. Dit is de klasse waarop u de logica van de taak wordt invoeren. De Split-methode om een reeks CloudTask-exemplaren die de taken in die u wilt dat voor het partitioneren van de taak vertegenwoordigt terug te implementeren.

**Standaard projectbestanden voor .NET-opdrachtregel**

* `App.config`: De standaard .NET toepassingsconfiguratiebestand.
* `Packages.config`: Standaard bestand NuGet-pakket voor afhankelijkheid.
* `Program.cs`: De programma-toegangspunt en op het hoogste niveau uitzonderingsverwerking bevat.

### <a name="implementing-the-job-splitter"></a>Implementatie van de splitser taak
Wanneer u het sjabloonproject Taakbeheer opent, wordt in het project het standaard geopend JobSplitter.cs-bestand hebben. U kunt de split-logica voor de taken in uw werkbelasting implementeren met behulp van het onderstaande Split() methode weergeven:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

> [!NOTE]
> De sectie aantekeningen in de `Split()` methode is het alleen-lezen gedeelte van de sjablooncode van de Job Manager-die is bedoeld voor u om te wijzigen door de logica toe te voegen aan uw taken splitsen in verschillende taken. Als u een andere sectie van de sjabloon wijzigt wilt, zorg ervoor dat u met Batch werking, zijn familiarized en probeer uit slechts enkele van de [Batch-codevoorbeelden][github_samples].
> 
> 

Uw implementatie Split() heeft toegang tot:

* De taakparameters via de `_parameters` veld.
* Het CloudJob-object dat de taak vertegenwoordigt de `_job` veld.
* Het CloudTask-object dat de jobbeheertaak vertegenwoordigt de `_jobManagerTask` veld.

Uw `Split()` implementatie hoeft niet rechtstreeks taken toevoegen aan de taak. In plaats daarvan een reeks CloudTask objecten in uw code moet worden geretourneerd en worden deze toegevoegd aan de job automatisch door de framework-klassen die gebruikmaken van de splitser taak. Wordt meestal gebruik van C# iterator (`yield return`) functie taak splitsbalken implementeren, zoals Hierdoor kan de taken te starten in zo snel mogelijk worden uitgevoerd in plaats van wachten tot alle taken moet worden berekend.

**Taak splitser is mislukt**

Als de splitser taak er een fout optreedt, moet deze ofwel het volgende doen:

* Beëindigen van de reeks met de C# `yield break` -instructie waarin geval de manager van de taak wordt beschouwd als geslaagd; of
* Veroorzaak een uitzondering, waarin de taak manager case wordt beschouwd als mislukt en mogelijk opnieuw worden geprobeerd, afhankelijk van hoe de client deze heeft geconfigureerd).

In beide gevallen worden alle taken die al worden geretourneerd door de splitser taak en toegevoegd aan de batchverwerking kunnen worden uitgevoerd. Als u niet wilt dat dit gebeurt, klikt u vervolgens kunt u:

* De taak beëindigen voordat u terugkeert uit de splitser taak
* De verzameling gehele taak voordat deze wordt teruggezonden formuleren (dat wil zeggen, retourneren een `ICollection<CloudTask>` of `IList<CloudTask>` in plaats van de implementatie van uw taak splitser met behulp van een iterator C#)
* Taakafhankelijkheden zouden gebruiken om alle taken die afhankelijk zijn van de voltooiing van de job manager

**Nieuwe pogingen van Job manager**

Als de manager van de taak is mislukt, mogelijk opnieuw door de Batch-service, afhankelijk van de instellingen van de client opnieuw worden uitgevoerd. Dit is in het algemeen veilig, omdat wanneer het framework taken aan de taak toevoegt, worden alle taken die al bestaan genegeerd. Echter, als het berekenen van taken dure is, mogelijk niet wilt u de kosten van de taken die al zijn toegevoegd aan de taak; herberekenen in rekening gebracht Als u daarentegen als het opnieuw uitvoeren kan niet worden gegarandeerd voor het genereren van de dezelfde taak-id's wordt vervolgens het negeren van duplicaten gedrag niet starten. In dergelijke gevallen moet u uw splitser taak voor het detecteren van het werk dat al is uitgevoerd en wordt niet herhaald, bijvoorbeeld door het uitvoeren van een CloudJob.ListTasks voordat u begint met het rendement van taken ontwerpen.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Afsluitcodes en uitzonderingen in de sjabloon Job Manager
Afsluitcodes en uitzonderingen bieden een mechanisme om te bepalen van het resultaat van het programma uitvoeren en ze kunnen helpen bij het identificeren van problemen met de uitvoering van het programma. De sjabloon Job Manager implementeert de afsluitcodes en uitzonderingen in deze sectie beschreven.

Een jobbeheertaak die wordt geïmplementeerd met de Job Manager-sjabloon kunt drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
| --- | --- |
| 0 |De taak manager is voltooid. De code van de splitser taak uitgevoerd worden voltooid en alle taken zijn toegevoegd aan de taak. |
| 1 |De jobbeheertaak is mislukt met een uitzondering in een 'verwachte' deel van het programma. De uitzondering is vertaald naar een JobManagerException met diagnostische gegevens en, indien mogelijk, suggesties voor het oplossen van de fout. |
| 2 |De jobbeheertaak is mislukt met een 'onverwachte'-uitzondering. De uitzondering is vastgelegd in het logboek voor standaarduitvoer, maar de manager van de taak is niet toegevoegd eventuele aanvullende informatie voor diagnose of herstel. |

In het geval van een taak manager taak is mislukt, kunnen sommige taken nog steeds zijn toegevoegd aan de service voordat de fout is opgetreden. Deze taken worden uitgevoerd die normaal werken. Zie 'Splitser taakfout' hierboven voor bespreking van dit codepad.

Alle informatie die wordt geretourneerd door uitzonderingen is geschreven in stdout.txt en stderr.txt-bestanden. Zie voor meer informatie [fouten afhandelen](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Overwegingen voor clients
Deze sectie beschrijft de vereisten voor een implementatie van de client wanneer een job manager op basis van deze sjabloon wordt aangeroepen. Zie [parameters en variabelen doorgeven van de clientcode](#pass-environment-settings) voor meer informatie over parameters en omgevingsinstellingen wordt doorgegeven.

**Verplichte referenties**

De jobbeheertaak vereist om de taken toevoegen aan de Azure Batch-taak, de URL van de Azure Batch-account en -sleutel. U moet deze in de omgevingsvariabelen met de naam YOUR_BATCH_URL en YOUR_BATCH_KEY doorgeven. U kunt deze Job Manager instellen omgevingsinstellingen voor taken. Bijvoorbeeld in een C#-client:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Storage-referenties**

De client hoeft normaal gesproken niet te bieden de referenties van gekoppelde opslagaccount naar de jobbeheertaak omdat (a) meeste taak managers niet hoeven te expliciet toegang tot het gekoppelde opslagaccount en (b) het gekoppelde opslagaccount vaak is opgegeven voor alle taken als een algemene omgevingsinstelling voor de taak. Als u de gekoppelde storage-account via de algemene omgevingsinstellingen voor de niet opgeeft, en de manager van de taak toegang tot de gekoppelde opslag vereist, moet u opgeven de referenties van de gekoppelde opslag als volgt:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Instellingen voor de taken van Job manager**

De client moet de manager van de taak ingesteld *killJobOnCompletion* markering **false**.

Het is doorgaans veilig voor de client in te stellen *runExclusive* naar **false**.

De client moet gebruiken de *resourceFiles* of *applicationPackageReferences* verzameling om te zorgen dat de taak manager uitvoerbare (en de vereiste DLL's) naar het rekenknooppunt wordt geïmplementeerd.

Standaard de manager van de taak wordt niet opnieuw worden uitgevoerd als deze uitvalt. Afhankelijk van uw job manager logica, de client mogelijk wilt inschakelen pogingen via *beperkingen*/*maxTaskRetryCount*.

**Instellingen van de taak**

Als de splitser taak taken met afhankelijkheden verzendt, moet de client van de taak usesTaskDependencies ingesteld op true.

In het model van de splitser taak is het ongebruikelijk voor clients wilt taken toevoegen aan taken dan wat de splitser taak maakt. De taak moet daarom normaal ingesteld door de client *onAllTasksComplete* naar **terminatejob**.

## <a name="task-processor-template"></a>Sjabloon voor taak-Processor
Een sjabloon taak Processor helpt u bij het implementeren van een taak processor die u kunt de volgende acties uitvoeren:

* Instellen van de informatie die door elke Batch-taak moet uitvoeren.
* Alle acties die zijn vereist voor elke Batch-taak uitgevoerd.
* Sla taak uitvoer naar de permanente opslag.

Hoewel een processor van de taak niet vereist is voor het uitvoeren van taken in Batch, is het belangrijkste voordeel van een processor van de taak dat deze een wrapper biedt voor het implementeren van alle acties van de taak kan worden uitgevoerd op één locatie. Bijvoorbeeld, moet u meerdere toepassingen in de context van de afzonderlijke taken uitvoeren of als u wilt kopiëren van gegevens naar de permanente opslag na het voltooien van elke taak.

De acties die worden uitgevoerd door de processor van de taak kunnen worden als eenvoudig of complex zijn, en zo veel of weinig, zoals vereist door uw workload. Bovendien door het implementeren van alle acties in de taak in één taak processor, kunt u direct bijwerken of toevoegen acties op basis van wijzigingen naar toepassingen of werklastvereisten. Echter, in sommige gevallen een processor van de taak mogelijk niet de optimale oplossing voor uw implementatie als deze onnodige complexiteit, bijvoorbeeld wanneer de uitvoering van taken die snel kunnen worden gestart vanaf een opdrachtregel eenvoudig kunt toevoegen.

### <a name="create-a-task-processor-using-the-template"></a>De Processor van een taak met de sjabloon maken
Als de processor van een taak toevoegen aan de oplossing die u eerder hebt gemaakt, de volgende stappen uit:

1. Open uw bestaande oplossing in Visual Studio.
2. Klik in Solution Explorer met de rechtermuisknop op de oplossing, klikt u op **toevoegen**, en klik vervolgens op **nieuw Project**.
3. Onder **Visual C#**, klikt u op **Cloud**, en klik vervolgens op **Azure Batch-taak Processor**.
4. Typ een naam waarmee u een beschrijving van uw toepassing en geeft aan dit project (bijvoorbeeld als de taak-processor 'LitwareTaskProcessor').
5. Klik op om het project **OK**.
6. Bouw ten slotte het project om af te dwingen van Visual Studio laden alle waarnaar wordt verwezen NuGet-pakketten en om te controleren of het project geldig is voordat u begint met het wijzigen van deze.

### <a name="task-processor-template-files-and-their-purpose"></a>Taak Processor sjabloonbestanden en hun doel
Wanneer u een project met de taak processor-sjabloon maakt, genereert het codebestanden drie groepen:

* Het belangrijkste programmabestand (Program.cs). Dit document bevat de programma-ingangspunt en uitzonderingsverwerking op het hoogste niveau. U mag niet normaal gesproken moet dit wijzigen.
* De Framework-map. Dit bevat de bestanden die verantwoordelijk is voor het 'standaard'-werk dat door het programma job manager – uitpakken van de parameters, taken toe te voegen aan de Batch-job, enzovoort. U mag niet normaal gesproken moet deze bestanden wijzigen.
* De taak processor-bestand (TaskProcessor.cs). Dit is waar u uw toepassingsspecifieke logica voor het uitvoeren van een taak (doorgaans door bellen naar een bestaand uitvoerbaar bestand) wordt geplaatst. Vóór en na verwerking komt, zoals aanvullende gegevens te downloaden of uploaden van bestanden met resultaten, ook hier code.

Natuurlijk kunt u aanvullende bestanden toevoegen als vereist ter ondersteuning van uw taak processor code, afhankelijk van de complexiteit van de taak logica splitsen.

De sjabloon genereert ook standaard .NET projectbestanden zoals een .csproj-bestand, app.config, packages.config, enz.

De rest van deze sectie beschrijft de verschillende bestanden en de codestructuur en wordt uitgelegd wat elke klasse doet.

![Visual Studio Solution Explorer met de sjabloon-oplossing taak-Processor][solution_explorer02]

**Framework-bestanden**

* `Configuration.cs`: Het laden van gegevens van de taak configuratie zoals details van de Batch-account, gekoppelde opslagaccountreferenties taak en taakgegevens en taakparameters ingekapseld. Bovendien wordt toegang tot Batch gedefinieerde omgevingsvariabelen (Zie omgevingsinstellingen voor taken in de documentatie van de Batch) via de Configuration.EnvironmentVariable-klasse.
* `IConfiguration.cs`: Hiermee isoleert de implementatie van de Configuration-klasse, zodat u kunt testen van eenheden uw taak splitser met behulp van een valse of mock configuration-object.
* `TaskProcessorException.cs`: Hiermee geeft u een fout is van de manager van de taak is beëindigd. TaskProcessorException wordt laten teruglopen 'verwachte' fouten waarin specifieke diagnostische informatie kan worden opgegeven als onderdeel van de beëindiging gebruikt.

**Taak-Processor**

* `TaskProcessor.cs`: De taak wordt uitgevoerd. Het framework roept de methode TaskProcessor.Run. Dit is de klasse waarop u de toepassingsspecifieke logica van de taak wordt invoeren. De methode uitvoeren om te implementeren:
  * Parseren en valideer de taakparameters van een
  * De opdrachtregel voor een extern programma dat u wilt aanroepen opstellen
  * Meld u diagnostische informatie die hebt u mogelijk nodig voor foutopsporing
  * Een proces starten met deze opdrachtregel
  * Wacht totdat het proces om af te sluiten
  * Vastleggen van de afsluitcode van het proces om te bepalen of deze is geslaagd of mislukt
  * Sla alle uitvoerbestanden die u wilt behouden voor permanente opslag

**Standaard projectbestanden voor .NET-opdrachtregel**

* `App.config`: De standaard .NET toepassingsconfiguratiebestand.
* `Packages.config`: Standaard bestand NuGet-pakket voor afhankelijkheid.
* `Program.cs`: De programma-toegangspunt en op het hoogste niveau uitzonderingsverwerking bevat.

## <a name="implementing-the-task-processor"></a>Implementatie van de taak-processor
Wanneer u de taak Processor sjabloonproject opent, wordt in het project het standaard geopend TaskProcessor.cs-bestand hebben. De uitvoering logica voor de taken kunt u implementeren in uw werkbelasting met behulp van de methode Run() hieronder wordt weergegeven:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
> [!NOTE]
> De sectie aantekeningen in de methode Run() is het alleen-lezen gedeelte van de taak Processor sjabloon-code die is bedoeld voor u door het toevoegen van de uitgevoerde instructies voor de taken in uw werkbelasting te wijzigen. Als u wijzigen van een andere sectie van de sjabloon wilt, moet eerst raken met de werking van Batch door de documentatie van de Batch controleren en enkele van de Batch-codevoorbeelden uitprobeert.
> 
> 

De methode Run() is verantwoordelijk voor het starten van de opdrachtregel voor het starten van een of meer processen, wacht tot alle proces te voltooien, de resultaten worden opgeslagen en ten slotte te retourneren met de afsluitcode. De methode Run() is waar u de verwerking van logica implementeren voor uw taken. De taak processor framework roept de methode Run() voor u; u hoeft niet zelf aanroepen.

Uw implementatie Run() heeft toegang tot:

* De taakparameters via de `_parameters` veld.
* De taak en taak-id's, via de `_jobId` en `_taskId` velden.
* De taakconfiguratie via de `_configuration` veld.

**Taak mislukt**

In geval van storing, kunt u de methode Run() afsluiten door er een uitzondering is opgetreden, maar dit laat het bovenste niveau uitzonderings-handler de controle over de afsluitcode van de taak. Als u de afsluitcode beheren wilt zodat u verschillende soorten mislukt, bijvoorbeeld voor diagnostische doeleinden onderscheiden kunt is of omdat de taak moeten worden beëindigd door een aantal fout-modi en anderen mag niet, moet u sluit de methode Run() door te retourneren van een niet-nul afsluitcode. Dit wordt de afsluitcode van de taak.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Afsluitcodes en uitzonderingen in de sjabloon taak Processor
Afsluitcodes en uitzonderingen bieden een mechanisme om te bepalen van het resultaat van het programma uitvoeren en deze kunt identificeren van problemen met de uitvoering van het programma. De taak Processor sjabloon implementeert de afsluitcodes en uitzonderingen in deze sectie beschreven.

Processor met een taak die is geïmplementeerd met de taak Processor-sjabloon kunt drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
| --- | --- |
| [Process.ExitCode][process_exitcode] |De processor van de taak uitgevoerd worden voltooid. Let op: dit niet impliceert dat het programma dat u aangeroepen geslaagd – alleen dat de processor van de taak met succes gestart en uitgevoerd na verwerking zonder uitzonderingen. De betekenis van de afsluitcode is afhankelijk van het aangeroepen programma – doorgaans afsluitcode 0 betekent dat het programma is voltooid en andere afsluitcode betekent dat het programma is mislukt. |
| 1 |De processor van de taak is mislukt met een uitzondering in een 'verwachte' deel van het programma. De uitzondering is vertaald naar een `TaskProcessorException` met diagnostische gegevens en, indien mogelijk, suggesties voor het oplossen van de fout. |
| 2 |De processor van de taak is mislukt met een 'onverwachte'-uitzondering. De uitzondering is vastgelegd in het logboek voor standaarduitvoer, maar de processor van de taak is niet toegevoegd eventuele aanvullende informatie voor diagnose of herstel. |

> [!NOTE]
> Als het programma dat u aanroept afsluitcodes 1 en 2 gebruikt om aan te geven van de specifieke fout modi, is vervolgens met afsluitcodes 1 en 2 voor taak processor fouten dubbelzinnig. U kunt deze taak processor-foutcodes onderscheidende afsluitcodes wijzigen door de uitzondering aanvragen in het bestand Program.cs te bewerken.
> 
> 

Alle informatie die wordt geretourneerd door uitzonderingen is geschreven in stdout.txt en stderr.txt-bestanden. Zie de afhandeling van de fout in de Batch-documentatie voor meer informatie.

### <a name="client-considerations"></a>Overwegingen voor clients
**Storage-referenties**

Als uw taak processor Azure blob storage gebruikt om vast te leggen uitvoer, bijvoorbeeld met behulp van het bestand conventies hulpbibliotheek, dan toegang tot moet *beide* de cloud opslagaccountreferenties *of* een blob container-URL met een shared access signature (SAS). De sjabloon biedt ondersteuning voor het opgeven van de referenties via algemene omgevingsvariabelen. De client kan doorgeven de storage-referenties als volgt:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Het opslagaccount is beschikbaar in de klasse TaskProcessor via de `_configuration.StorageAccount` eigenschap.

Als u liever een container-URL met SAS gebruiken, kunt u dit ook doorgeven via een gemeenschappelijke omgevingsinstelling van taak, maar de taak processor sjabloon omvat ingebouwde ondersteuning voor dit niet op dit moment.

**Setup van opslag**

Het wordt aanbevolen dat de taak van de manager-client of de taak geen containers die voor de taken vereist voordat de taken toe te voegen aan de taak maken. Dit is verplicht dat als u de URL van een container met SAS, als zodanig een URL bevat geen machtiging voor het maken van de container. Het verdient aanbeveling zelfs als u opslagaccountreferenties, zoals bespaart u elke taak hoeven CloudBlobContainer.CreateIfNotExistsAsync aanroepen voor de container.

## <a name="pass-parameters-and-environment-variables"></a>Pass-parameters en variabelen
### <a name="pass-environment-settings"></a>Omgevingsinstellingen doorgeven
Een client kan informatie doorgeven aan de jobbeheertaak in de vorm van omgevingsinstellingen. Deze informatie kan vervolgens worden gebruikt door de jobbeheertaak bij het genereren van de taak processor-taken die wordt uitgevoerd als onderdeel van de compute-taak. Voorbeelden van de informatie die u als omgevingsinstellingen doorgeven kunt zijn:

* Naam en het account toegangscodes voor opslag
* URL van de batch-account
* De sleutel van de batch-account

De Batch-service heeft een eenvoudig mechanisme omgevingsinstellingen doorgeven aan een jobbeheertaak met behulp van de `EnvironmentSettings` eigenschap in [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Bijvoorbeeld, om op te halen de `BatchClient` exemplaar voor een Batch-account, kunt u als omgevingsvariabelen van de client de URL en de gedeelde sleutel referenties voor het Batch-account code doorgeven. Evenzo voor toegang tot het opslagaccount dat is gekoppeld aan het Batch-account, kunt u de opslagaccountnaam en doorgeven de opslagaccountsleutel als omgevingsvariabelen.

### <a name="pass-parameters-to-the-job-manager-template"></a>Parameters doorgeven aan de Job Manager-sjabloon
In veel gevallen is het handig voor de per taak parameters doorgeven aan de jobbeheertaak om te bepalen van de taak proces splitsen of voor het configureren van de taken voor de taak. U kunt dit doen door het uploaden van een JSON-bestand parameters.json met de naam als een bronbestand voor de jobbeheertaak. De parameters kunnen vervolgens beschikbaar gesteld in de `JobSplitter._parameters` veld in de Job Manager-sjabloon.

> [!NOTE]
> De ingebouwde parameter handler ondersteunt alleen string-naar-tekenreeks woordenlijsten. Als u complexe JSON-waarden als parameterwaarden doorgeven wilt, moet u deze poorten als tekenreeksen doorgeven en ze in de splitser taak parseert of te wijzigen van het framework `Configuration.GetJobParameters` methode.
> 
> 

### <a name="pass-parameters-to-the-task-processor-template"></a>Parameters doorgeven aan de sjabloon taak Processor
U kunt ook parameters doorgeven aan de afzonderlijke taken die zijn geïmplementeerd met behulp van de taak Processor-sjabloon. Net zoals met de taak manager-sjabloon de sjabloon voor de taak-processor wordt gezocht naar een resource-bestand met de naam

parameters.JSON, en, indien deze gevonden geladen als de parameters-woordenlijst. Er zijn een aantal opties voor het parameters doorgeven aan de taak processor taken:

* De taakparameters JSON hergebruiken. Dit werkt goed als de enige parameters taak wide (voor bijvoorbeeld een render hoogte en breedte zijn). U doet dit door bij het maken van een CloudTask in de splitser taak, Voeg een verwijzing naar het object parameters.json resource-bestand van de jobbeheertaak ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) aan de CloudTask ResourceFiles verzameling.
* Genereren en uploaden van een document taakspecifieke parameters.json als onderdeel van de splitser taakuitvoering en verwijzen naar blob in verzameling voor de taak resource files. Dit is nodig als u verschillende taken verschillende parameters hebben. Een voorbeeld is mogelijk een 3D-rendering scenario waar de frame-index wordt doorgegeven aan de taak als parameter.

> [!NOTE]
> De ingebouwde parameter handler ondersteunt alleen string-naar-tekenreeks woordenlijsten. Als u complexe JSON-waarden als parameterwaarden doorgeven wilt, moet u deze poorten als tekenreeksen doorgeven en in de processor van de taak worden geparseerd of te wijzigen van het framework `Configuration.GetTaskParameters` methode.
> 
> 

## <a name="next-steps"></a>Volgende stappen
### <a name="persist-job-and-task-output-to-azure-storage"></a>Behouden van de taak en uitvoer naar Azure Storage
Een ander handig hulpmiddel in ontwikkeling van de Batch-oplossingen is [Azure Batch-bestand conventies][nuget_package]. Gebruik deze bibliotheek van de .NET-klasse (momenteel in preview) in uw Batch .NET-toepassingen eenvoudig opslaan en ophalen van de taak uitvoer en naar Azure Storage. [Azure Batch-taak en uitvoer behouden](batch-task-output.md) bevat een volledige beschrijving van de tapewisselaar en het gebruik ervan.

### <a name="batch-forum"></a>Batch-Forum
De [Azure Batch-Forum] [ forum] is een goede plaats om te bespreken Batch en vragen over de service op MSDN. Kop op via voor nuttige 'een tijdelijke' berichten, en stel uw vragen wanneer deze zich voordoen tijdens het bouwen van uw Batch-oplossingen.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png

---
title: Met Visual Studio-sjablonen - Azure Batch-oplossingen bouwen | Microsoft Docs
description: Leer hoe Visual Studio-projectsjablonen helpen u bij het implementeren en uitvoeren van uw rekenintensieve workloads op Azure Batch.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5a44c249a957050afb500decd094183c71d6ca5e
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114093"
---
# <a name="use-visual-studio-project-templates-to-jump-start-batch-solutions"></a>Visual Studio-projectsjablonen voor Batch-oplossingen snel gebruiken

De **Job Manager** en **taak Processor Visual Studio-sjablonen** voor Batch bieden code waarmee u kunt implementeren en uw rekenintensieve workloads uitvoert op Batch met zo min mogelijk inspanningen. Dit document beschrijft deze sjablonen en richtlijnen voor het gebruik ervan.

> [!IMPORTANT]
> In dit artikel vindt u alleen informatie die van toepassing zijn op deze twee sjablonen en wordt ervan uitgegaan dat u bekend met de Batch-service en de belangrijkste concepten met betrekking tot het bent: pools, rekenknooppunten, taken en taken, jobbeheertaken, omgevingsvariabelen en andere relevante informatie. U vindt meer informatie in [basisbeginselen van Azure Batch](batch-technical-overview.md) en [overzicht van de Batch-functies voor ontwikkelaars](batch-api-basics.md).
> 
> 

## <a name="high-level-overview"></a>Overzicht
De Job Manager en taak Processor-sjablonen kunnen worden gebruikt om te maken van twee nuttig onderdelen:

* Een jobbeheertaak waarmee het splitsen van een taak die kunt uitsplitsen een taak in meerdere taken die onafhankelijk van elkaar, parallel kunnen worden uitgevoerd.
* Een taak-processor die kan worden gebruikt om uit te voeren vooraf verwerken en nabewerking rond een toepassing vanaf de opdrachtregel.

Bijvoorbeeld in een film-rendering-scenario, zou het splitsen van de taak zet een enkele film-taak in honderden of duizenden afzonderlijke taken die afzonderlijke frames afzonderlijk wilt verwerken. Dienovereenkomstig, de processor van de taak zou aanroepen van de renderingtoepassing en alle afhankelijke processen die nodig zijn om weer te geven van elk frame, evenals aanvullende acties (bijvoorbeeld het gerenderde frame kopiëren naar een opslaglocatie) uitvoeren.

> [!NOTE]
> De Job Manager en taak Processor sjablonen zijn onafhankelijk van elkaar worden verbonden, zodat u kiezen kunt om beide of slechts één provider, afhankelijk van de vereisten van uw taak compute en uw voorkeuren te gebruiken.
> 
> 

Zoals u in het onderstaande diagram, wordt een compute-taak die gebruikmaakt van deze sjablonen drie fasen doorlopen:

1. De clientcode (bijvoorbeeld, toepassing, webservice, enz.) dient een taak voor de Batch-service op Azure, op te geven als de jobbeheer taak programma voor de taak.
2. De Batch-service wordt de jobbeheertaak uitgevoerd op een rekenknooppunt en het splitsen van de taak wordt het opgegeven aantal taak processor taken gestart op als veel zo nodig op basis van de parameters en specificaties in de code van de splitsen taak rekenknooppunten.
3. De taak processor taken parallel uit te voeren onafhankelijk van elkaar, voor het verwerken van de ingevoerde gegevens en de uitvoergegevens die worden gegenereerd.

![Diagram waarin wordt getoond hoe clientcode communiceert met de Batch-service][diagram01]

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van de Batch-sjablonen, moet u het volgende:

* Een computer met Visual Studio 2015 geïnstalleerd. Batch-sjablonen zijn momenteel alleen ondersteund voor Visual Studio 2015.
* De Batch-sjablonen, die beschikbaar zijn vanuit de [Visual Studio-galerie] [ vs_gallery] als Visual Studio-extensies. Er zijn twee manieren om de sjablonen te verkrijgen:
  
  * Installeren van de sjablonen met behulp van de **extensies en Updates** in het dialoogvenster in Visual Studio (Zie voor meer informatie, [zoeken en met behulp van Visual Studio-extensies][vs_find_use_ext]). In de **extensies en Updates** dialoogvenster vak, zoeken en downloaden van de volgende twee extensies:
    
    * Azure Batch-Job Manager met taak splitsen
    * Processor van Azure Batch-taak
  * De sjablonen voor Visual Studio downloaden uit de galerie met online: [projectsjablonen voor Microsoft Azure Batch][vs_gallery_templates]
* Als u van plan bent te gebruiken de [toepassingspakketten](batch-application-packages.md) functie voor het implementeren van job manager en processor-taak naar de Batch-rekenknooppunten, moet u een opslagaccount koppelen aan uw Batch-account.

## <a name="preparation"></a>Voorbereiding
Het is raadzaam om het maken van een oplossing die de manager van uw taak, evenals de processor van uw taak bevatten kan, omdat dit kan het eenvoudiger voor het delen van code tussen de manager van de taak en de taak processor-programma's. Volg deze stappen voor het maken van deze oplossing:

1. Open Visual Studio en selecteer **bestand** > **nieuw** > **Project**.
2. Onder **sjablonen**, vouw **andere projecttypen**, klikt u op **Visual Studio-oplossingen**, en selecteer vervolgens **leeg oplossing**.
3. Typ een naam voor uw toepassing en het doel van deze oplossing (bijvoorbeeld ' LitwareBatchTaskPrograms').
4. Klik op om de nieuwe oplossing **OK**.

## <a name="job-manager-template"></a>Taak Manager-sjabloon
De Job Manager-sjabloon helpt u bij het implementeren van een jobbeheertaak die u kunt de volgende acties uitvoeren:

* Splits een taak in meerdere taken.
* Deze taken uit te voeren op Batch verzenden.

> [!NOTE]
> Zie voor meer informatie over jobbeheertaken [overzicht van de Batch-functies voor ontwikkelaars](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-the-template"></a>Een Job Manager met behulp van de sjabloon maken
Als u wilt een taak toevoegen aan de oplossing die u eerder hebt gemaakt, de volgende stappen uit:

1. Open uw bestaande oplossing in Visual Studio.
2. Klik in Solution Explorer met de rechtermuisknop op de oplossing, klikt u op **toevoegen** > **nieuw Project**.
3. Onder **Visual C#**, klikt u op **Cloud**, en klik vervolgens op **Azure Batch-Job Manager met taak splitsen**.
4. Typ een naam die uw toepassing wordt beschreven en wordt dit project (bijvoorbeeld als de beheerder van de taak geïdentificeerd "LitwareJobManager").
5. Klik op om het project **OK**.
6. Bouw ten slotte het project om af te dwingen van Visual Studio laden van alle waarnaar wordt verwezen, NuGet-pakketten en om te verifiëren dat het project geldig is voordat u begint dat u deze wijzigt.

### <a name="job-manager-template-files-and-their-purpose"></a>Sjabloonbestanden Job Manager en hun doel
Wanneer u een project met behulp van de Job Manager-sjabloon maakt, genereert deze drie groepen van codebestanden:

* Het belangrijkste programmabestand (Program.cs). Dit bevat de programma-toegangspunt en de afhandeling van uitzonderingen op het hoogste niveau. U mag niet normaal gesproken moet deze wijzigen.
* De map Frameworkmap. Dit bevat de bestanden die verantwoordelijk is voor de 'standaard'-werk dat door het programma job manager – uitpakken van de parameters, taken toe te voegen aan de Batch-taak, enzovoort. U mag niet normaal gesproken moet deze bestanden wijzigt.
* Het bestand taak splitsen (JobSplitter.cs). Dit is waar u uw toepassing-specifieke logica voor het splitsen van een taak in taken wordt geplaatst.

Natuurlijk kunt u extra bestanden toevoegen als vereist ter ondersteuning van uw taak splitsen code, afhankelijk van de complexiteit van de taak logische splitsing.

De sjabloon wordt ook gegenereerd standaard .NET-project-bestanden, zoals een .csproj-bestand, app.config, packages.config, enzovoort.

De rest van deze sectie beschrijft de verschillende bestanden en de codestructuur van hun en wordt uitgelegd wat elke klasse doet.

![Visual Studio Solution Explorer toont de Job Manager sjabloonoplossing][solution_explorer01]

**Framework-bestanden**

* `Configuration.cs`: Het laden van gegevens van de configuratie, zoals de details van de Batch-account, referenties voor het gekoppelde opslagaccount, job en taakgegevens en taakparameters ingekapseld. Het biedt ook toegang tot Batch gedefinieerde omgevingsvariabelen (Zie omgevingsinstellingen voor taken, in de Batch-documentatie) via de klasse Configuration.EnvironmentVariable.
* `IConfiguration.cs`: Hiermee isoleert de implementatie van de configuratieklasse zodat u uw taak splitsen met behulp van een valse of mock configuratieobject eenheidstest kunt.
* `JobManager.cs`: De onderdelen van het programma job manager deelt. Het is verantwoordelijk voor het initialiseren van de taak splitser, aanroepen van de taak splitser, en het verzenden van de taken die zijn geretourneerd door de taak splitser de indiener van de taak.
* `JobManagerException.cs`: Hiermee geeft u een fout waarvoor de manager van de taak is beëindigd. JobManagerException wordt gebruikt om u te laten teruglopen 'verwachte' fouten waarbij specifieke diagnostische informatie kan worden opgegeven als onderdeel van de beëindiging.
* `TaskSubmitter.cs`: Met deze klasse is verantwoordelijk voor het toevoegen van taken die zijn geretourneerd door de taak splitser voor de Batch-taak. De aggregaties van de klasse Taakbeheer heeft de volgorde van de taken in batches voor efficiënt, maar tijdige toevoegen aan de taak, roept vervolgens TaskSubmitter.SubmitTasks op een achtergrond-thread voor elke batch.

**Taak splitsen**

`JobSplitter.cs`: Met deze klasse bevat toepassingsspecifieke logica voor het splitsen van de taak in taken. Het framework roept de methode JobSplitter.Split voor het verkrijgen van een reeks taken, die wordt toegevoegd aan de taak als de methode deze retourneert. Dit is de klasse waar voert u de logica van de taak. Implementeer de Split-methode voor het retourneren van een reeks CloudTask exemplaren die de taken in die u wilt voor het partitioneren van de taak vertegenwoordigt.

**Standaard .NET projectbestanden in de opdrachtregel**

* `App.config`: De standaard .NET-toepassing-configuratiebestand.
* `Packages.config`: Standaard NuGet-pakket afhankelijkheid-bestand.
* `Program.cs`: Bevat de programma-toegangspunt en de afhandeling van uitzonderingen op het hoogste niveau.

### <a name="implementing-the-job-splitter"></a>Implementatie van de taak splitsen
Wanneer u het sjabloonproject Job Manager opent, wordt het project het open standaard JobSplitter.cs-bestand hebt. U kunt de split-logica voor de taken in uw werkbelasting implementeren met behulp van de onderstaande Split() methode weergeven:

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
> De sectie met aantekeningen in de `Split()` methode is het alleen-lezen gedeelte van de sjablooncode van de Job Manager-die is bedoeld voor u om te wijzigen door de logica toe te voegen aan uw taken onderverdeeld in verschillende taken. Als u wijzigen van een andere sectie van de sjabloon wilt, zorg ervoor dat u met Batch werking, zijn familiarized en uitproberen van enkele van de [Batch-codevoorbeelden][github_samples].
> 
> 

Uw implementatie Split() heeft toegang tot:

* De taakparameters, via de `_parameters` veld.
* Het CloudJob-object dat de taak vertegenwoordigt de `_job` veld.
* Het CloudTask-object vertegenwoordigt de jobbeheertaak de `_jobManagerTask` veld.

Uw `Split()` implementatie hoeft niet te taken rechtstreeks toevoegen aan de taak. In plaats daarvan een reeks CloudTask objecten in uw code moet worden geretourneerd, en worden deze toegevoegd aan de job automatisch door de framework-klassen die de taak splitser aanroepen. Is het gebruikelijk dat gebruik van C# iterator (`yield return`) functie voor het implementeren van taak splitsbalken omdat hiermee de taken te starten in zo snel mogelijk worden uitgevoerd in plaats van wachten tot alle taken moet worden berekend.

**Taakfout splitsen**

Als het splitsen van uw taak is een fout optreedt, moet deze een:

* Beëindigen van de reeks met de C# `yield break` -instructie in dat geval job manager zullen worden behandeld als geslaagd; of
* Genereren van een uitzondering waarin job manager zullen worden behandeld als aanvraag is mislukt en opnieuw kan worden uitgevoerd, afhankelijk van hoe de client deze heeft geconfigureerd).

In beide gevallen wordt komen alle taken die al zijn geretourneerd door de taak splitsen en toegevoegd aan de Batch-job in aanmerking om uit te voeren. Als u niet dat dit wilt mag gebeuren, klikt u vervolgens kunt u:

* De taak beëindigen voordat het retourneren van de taak splitsen
* De gehele taak in de verzameling voordat deze worden teruggezonden formuleren (dat wil zeggen, retourneren een `ICollection<CloudTask>` of `IList<CloudTask>` in plaats van de implementatie van uw taak splitsen met behulp van een C#-iterator)
* Taakafhankelijkheden gebruiken om te maken van alle taken zijn afhankelijk van de voltooiing van de manager van de taak

**Taak manager nieuwe pogingen**

Als de manager van de taak is mislukt, mogelijk opnieuw door de Batch-service, afhankelijk van de clientinstellingen voor nieuwe pogingen worden uitgevoerd. Dit is in het algemeen veilig, omdat wanneer het framework taken aan de taak toevoegt, worden alle taken die al bestaan genegeerd. Echter, als het berekenen van de taken is duur, niet kunt u naar de kosten van het berekenen van de taken die al zijn toegevoegd aan de taak. daarentegen als het opnieuw uitvoeren is niet noodzakelijkerwijs voor het genereren van de dezelfde taak-id's wordt vervolgens het gedrag 'dubbele negeren' niet gestart. In dergelijke gevallen moet u uw taak splitser voor het detecteren van het werk dat al is uitgevoerd en niet herhalen, bijvoorbeeld door het uitvoeren van een CloudJob.ListTasks voordat u begint met het rendement van taken ontwerpen.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Afsluitcodes en uitzonderingen in de Job Manager-sjabloon
Afsluitcodes en uitzonderingen bieden een mechanisme om te bepalen van het resultaat van het uitvoeren van een programma, en ze helpen bij het identificeren van problemen met de uitvoering van het programma. De Job Manager-sjabloon implementeert de afsluitcodes en uitzonderingen die in deze sectie beschreven.

Een jobbeheertaak die is geïmplementeerd met behulp van de Job Manager-sjabloon kunt drie mogelijke afsluitcodes geretourneerd:

| Code | Beschrijving |
| --- | --- |
| 0 |De taak manager is voltooid. Uw taak splitsen code uitgevoerd worden voltooid en alle taken zijn toegevoegd aan de job. |
| 1 |De jobbeheertaak is mislukt met een uitzondering in een 'verwachte' deel van het programma. De uitzondering is vertaald naar een JobManagerException met diagnostische gegevens en, indien mogelijk, suggesties voor het oplossen van de fout. |
| 2 |De jobbeheertaak is mislukt met een 'onverwachte' uitzondering. De uitzondering naar de standaarduitvoer is vastgelegd, maar job manager kon om toe te voegen van eventuele aanvullende informatie voor diagnose of herstel. |

In het geval van job manager taak mislukt, sommige taken kunnen nog steeds zijn toegevoegd aan de service voordat de fout is opgetreden. Deze taken worden uitgevoerd die normaal werken. Zie 'Taak splitsen-fout' hierboven voor beschrijving van dit codepad.

Alle informatie die wordt geretourneerd door uitzonderingen is geschreven in stdout.txt en stderr.txt-bestanden. Zie voor meer informatie, [vorm van foutafhandeling](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Overwegingen voor de client
Deze sectie beschrijft enkele clientvereisten voor de implementatie bij het aanroepen van de manager van een taak op basis van deze sjabloon. Zie [hoe worden doorgegeven parameters en variabelen van de clientcode](#pass-environment-settings) voor meer informatie over het doorgeven van parameters en instellingen voor omgevingsvariabelen.

**Verplichte referenties**

De jobbeheertaak vereist om te kunnen taken toevoegen aan de Azure Batch-taak, uw Azure Batch-account-URL en de sleutel. U moet de volgende omgevingsvariabelen met de naam YOUR_BATCH_URL en YOUR_BATCH_KEY doorgeven. U kunt deze in de Job Manager instellen omgevingsinstellingen voor taken. Bijvoorbeeld, in een C#-client:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Storage-referenties**

Normaal gesproken de client niet nodig voor de gekoppelde storage-accountreferenties op de jobbeheertaak omdat (a) de meeste taak managers niet hoeft expliciet toegang tot het gekoppelde storage-account en (b) het gekoppelde opslagaccount vaak wordt verstrekt aan alle taken als een algemene omgevingsinstelling voor de taak. Als u de gekoppelde storage-account via de algemene omgevingsinstellingen voor de niet opgeeft, en job manager toegang tot de gekoppelde opslag vereist, moet klikt u vervolgens u de gekoppelde opslag gegevens opgeven als volgt:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Instellingen voor de taken van Job manager**

De client moet de manager van de taak ingesteld *killJobOnCompletion* markering **false**.

Het is doorgaans veilig is voor de client om in te stellen *runExclusive* naar **false**.

De client moet gebruiken de *resourceFiles* of *applicationPackageReferences* verzameling om te zorgen dat de taak manager uitvoerbaar bestand (en de vereiste DLL-bestanden) geïmplementeerd op het rekenknooppunt.

Standaard een job manager wordt niet herhaald als het mislukt. Afhankelijk van uw taak manager logica, de client wilt inschakelen, nieuwe pogingen via *beperkingen*/*maxTaskRetryCount*.

**Taakinstellingen**

Als de taak splitser taken met afhankelijkheden verzendt, moet de client van de taak usesTaskDependencies ingesteld op true.

In het model van de splitsen taak is het ongebruikelijk voor clients wilt taken toevoegen aan taken cloudniveau wat het splitsen van de taak wordt gemaakt. Van de taak moet daarom gewoonlijk ingesteld door de client *onAllTasksComplete* naar **terminatejob**.

## <a name="task-processor-template"></a>Sjabloon voor taak-Processor
Een taak Processor sjabloon helpt u bij het implementeren van een taak processor en dat de volgende acties kunt uitvoeren:

* Instellen van de informatie die is vereist voor elke Batch-taak om uit te voeren.
* Voer alle acties die zijn vereist voor elke Batch-taak.
* Taak-uitvoer opslaan naar de permanente opslag.

Hoewel een processor van de taak niet vereist is om uit te voeren taken in Batch, is het belangrijkste voordeel van het gebruik van de processor van een taak die het biedt een wrapper voor het implementeren van alle acties van de taak kan worden uitgevoerd op één locatie. Bijvoorbeeld, taak als u nodig hebt voor het uitvoeren van meerdere toepassingen in de context van elke taak, of als u wilt kopiëren gegevens naar permanente opslag na het voltooien van elke.

De acties die worden uitgevoerd door de taak-processor kunnen zijn als eenvoudig of complex, en veel of weinig, zoals vereist door uw workload. Bovendien door het implementeren van alle acties van de taak in één taak processor, kunt u direct bijwerken of toevoegen acties op basis van wijzigingen aan toepassingen of vereisten van workloads. Echter, in sommige gevallen een processor van de taak mogelijk niet de optimale oplossing voor uw implementatie als het onnodige complexiteit, bijvoorbeeld wanneer de uitvoering van taken die snel kunnen worden gestart vanaf de opdrachtregel van een eenvoudige opdracht kunt toevoegen.

### <a name="create-a-task-processor-using-the-template"></a>De Processor van een taak met behulp van de sjabloon maken
De processor van een taak toevoegen aan de oplossing die u eerder hebt gemaakt, de volgende stappen uit:

1. Open uw bestaande oplossing in Visual Studio.
2. Klik in Solution Explorer met de rechtermuisknop op de oplossing, klikt u op **toevoegen**, en klik vervolgens op **nieuw Project**.
3. Onder **Visual C#**, klikt u op **Cloud**, en klik vervolgens op **Azure Batch-taak Processor**.
4. Typ een naam die wordt beschreven voor uw en dit project (bijvoorbeeld) identificeert als de taak-processor "LitwareTaskProcessor").
5. Klik op om het project **OK**.
6. Bouw ten slotte het project om af te dwingen van Visual Studio laden van alle waarnaar wordt verwezen, NuGet-pakketten en om te verifiëren dat het project geldig is voordat u begint dat u deze wijzigt.

### <a name="task-processor-template-files-and-their-purpose"></a>Taak Processor-sjabloonbestanden en hun doel
Wanneer u een project met behulp van de taak processor-sjabloon maakt, genereert deze drie groepen van codebestanden:

* Het belangrijkste programmabestand (Program.cs). Dit bevat de programma-toegangspunt en de afhandeling van uitzonderingen op het hoogste niveau. U mag niet normaal gesproken moet deze wijzigen.
* De map Frameworkmap. Dit bevat de bestanden die verantwoordelijk is voor de 'standaard'-werk dat door het programma job manager – uitpakken van de parameters, taken toe te voegen aan de Batch-taak, enzovoort. U mag niet normaal gesproken moet deze bestanden wijzigt.
* Het bestand taak processor (TaskProcessor.cs). Dit is waar plaatst u uw toepassing-specifieke logica voor het uitvoeren van een taak (meestal door het aanroepen van aan een bestaand uitvoerbaar bestand). Vóór en na verwerking code, zoals aanvullende gegevens te downloaden of uploaden van bestanden met resultaten, is ook hier gaat.

Natuurlijk kunt u extra bestanden toevoegen als vereist ter ondersteuning van uw taak processor code, afhankelijk van de complexiteit van de taak logische splitsing.

De sjabloon wordt ook gegenereerd standaard .NET-project-bestanden, zoals een .csproj-bestand, app.config, packages.config, enzovoort.

De rest van deze sectie beschrijft de verschillende bestanden en de codestructuur van hun en wordt uitgelegd wat elke klasse doet.

![Visual Studio Solution Explorer met de sjabloon-oplossing taak-Processor][solution_explorer02]

**Framework-bestanden**

* `Configuration.cs`: Het laden van gegevens van de configuratie, zoals de details van de Batch-account, referenties voor het gekoppelde opslagaccount, job en taakgegevens en taakparameters ingekapseld. Het biedt ook toegang tot Batch gedefinieerde omgevingsvariabelen (Zie omgevingsinstellingen voor taken, in de Batch-documentatie) via de klasse Configuration.EnvironmentVariable.
* `IConfiguration.cs`: Hiermee isoleert de implementatie van de configuratieklasse zodat u uw taak splitsen met behulp van een valse of mock configuratieobject eenheidstest kunt.
* `TaskProcessorException.cs`: Hiermee geeft u een fout waarvoor de manager van de taak is beëindigd. TaskProcessorException wordt gebruikt om u te laten teruglopen 'verwachte' fouten waarbij specifieke diagnostische informatie kan worden opgegeven als onderdeel van de beëindiging.

**Taak-Processor**

* `TaskProcessor.cs`: De taak wordt uitgevoerd. Het framework roept de methode TaskProcessor.Run. Dit is de klasse waar voert u de toepassing-specifieke logica van de taak. De Run-methode om te implementeren:
  * Parseren en de taakparameters van elke valideren
  * Opstellen van de opdrachtregel voor alle externe programma's die u wilt aanroepen
  * Meld eventuele diagnostische gegevens die u hebt mogelijk voor foutopsporing
  * Een proces starten met die vanaf de opdrachtregel
  * Wacht totdat het proces wordt afgebroken
  * Vastleggen van de afsluitcode van het proces om te bepalen of deze is geslaagd of mislukt
  * Sla alle uitvoerbestanden die u wilt behouden naar de permanente opslag

**Standaard .NET projectbestanden in de opdrachtregel**

* `App.config`: De standaard .NET-toepassing-configuratiebestand.
* `Packages.config`: Standaard NuGet-pakket afhankelijkheid-bestand.
* `Program.cs`: Bevat de programma-toegangspunt en de afhandeling van uitzonderingen op het hoogste niveau.

## <a name="implementing-the-task-processor"></a>Implementatie van de taak-processor
Wanneer u de taak Processor-sjabloonproject opent, wordt het project het open standaard TaskProcessor.cs-bestand hebt. U kunt de run logica voor de taken in uw werkbelasting implementeren met behulp van de methode Run() hieronder wordt weergegeven:

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
> De sectie met aantekeningen in de methode Run() is de alleen-lezen gedeelte van de code van de taak Processor-sjabloon die is bedoeld voor u door het toevoegen van de run logica voor de taken in uw werkbelasting te wijzigen. Als u wijzigen van een andere sectie van de sjabloon wilt, Geef eerst raken met de werking van Batch door de Batch-documentatie bekijken en uit een aantal van de Batch-codevoorbeelden te proberen.
> 
> 

De methode Run() is verantwoordelijk voor het starten van de opdrachtregel, starten van een of meer processen, alle proces is voltooid, wacht de resultaten worden opgeslagen en ten slotte te retourneren met de afsluitcode. De methode Run() is waar u de verwerkingslogica implementeren voor uw taken. Het framework van de processor taak roept de methode Run(). u hoeft niet te deze zelf aanroepen.

Uw implementatie Run() heeft toegang tot:

* De taakparameters, via de `_parameters` veld.
* De taak en de taak-id's, via de `_jobId` en `_taskId` velden.
* De taakconfiguratie van de, via de `_configuration` veld.

**Taak mislukt**

In geval van storing, kunt u de methode Run() afsluiten door er een uitzondering optreedt, maar in dat geval het bovenste niveau uitzonderingshandler controle over de afsluitcode van de taak. Als u nodig hebt voor het beheren van de afsluitcode zodat u verschillende typen fouten, bijvoorbeeld voor diagnostische doeleinden onderscheiden kunt of omdat de taak moeten worden beëindigd door sommige foutmodi en anderen mag niet, moet u sluit de methode Run() door te retourneren van een niet-nul afsluitcode. Hiermee wordt de afsluitcode van de taak.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Afsluitcodes en uitzonderingen in de taak Processor-sjabloon
Afsluitcodes en uitzonderingen bieden een mechanisme om te bepalen van het resultaat van het uitvoeren van een programma, en ze te kunnen met het identificeren van problemen met de uitvoering van het programma. De taak Processor-sjabloon implementeert de afsluitcodes en uitzonderingen die in deze sectie beschreven.

Een taak van de processor die wordt geïmplementeerd met behulp van de sjabloon van de Processor van de taak kan drie mogelijke afsluitcodes retourneren:

| Code | Beschrijving |
| --- | --- |
| [Process.ExitCode][process_exitcode] |De processor van de taak is voltooid. Let op: dit niet betekent dat het programma dat u aangeroepen, alleen geslaagd is: dat de processor van de taak is aangeroepen en uitgevoerd na verwerking zonder uitzonderingen. De betekenis van de afsluitcode is afhankelijk van de aangeroepen programma – doorgaans afsluitcode 0 betekent dat het programma is voltooid en een andere afsluitcode betekent dat het programma is mislukt. |
| 1 |De processor van de taak is mislukt met een uitzondering in een 'verwachte' deel van het programma. De uitzondering is vertaald naar een `TaskProcessorException` met diagnostische gegevens en, indien mogelijk, suggesties voor het oplossen van de fout. |
| 2 |De processor van de taak is mislukt met een 'onverwachte' uitzondering. De uitzondering naar de standaarduitvoer is vastgelegd, maar de processor van de taak kan niet toe te voegen van eventuele aanvullende informatie voor diagnose of herstel. |

> [!NOTE]
> Als het programma dat u aanroepen maakt gebruik van afsluitcodes 1 en 2 om aan te geven specifieke foutmodi, is klikt u vervolgens met behulp van afsluitcodes 1 en 2 voor taak processor fouten niet eenduidig. U kunt deze taak processor-foutcodes onderscheidend afsluitcodes wijzigen door te bewerken van de uitzondering aanvragen in het bestand Program.cs.
> 
> 

Alle informatie die wordt geretourneerd door uitzonderingen is geschreven in stdout.txt en stderr.txt-bestanden. Zie de vorm van foutafhandeling, in de Batch-documentatie voor meer informatie.

### <a name="client-considerations"></a>Overwegingen voor de client
**Storage-referenties**

Als de processor van uw taak maakt gebruik van Azure blob-opslag om vast te leggen van de uitvoer, bijvoorbeeld met behulp van het bestand conventions helper-bibliotheek en deze toegang hebben tot moet *beide* de opslagaccountreferenties cloud *of* een blob container-URL met een shared access signature (SAS). De sjabloon biedt ondersteuning voor referenties via algemene omgevingsvariabelen op te geven. De client kan de referenties voor opslag als volgt doorgeven:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Het opslagaccount is beschikbaar in de klasse TaskProcessor via de `_configuration.StorageAccount` eigenschap.

Als u liever het gebruik van de URL van een container met SAS, u kunt dit ook doorgeven via een gemeenschappelijke omgevingsinstelling van taak, maar de taak processor-sjabloon bevat momenteel geen ingebouwde ondersteuning voor deze.

**Setup van opslag**

Het wordt aanbevolen dat de taak van de manager-client of de taak containers vereist door taken voordat u de taken toevoegt aan de taak te maken. Dit is verplicht dat als u de URL van een container met SAS, als zodanig een URL bevat geen machtiging voor het maken van de container. Het verdient aanbeveling, zelfs als u de referenties van het opslagaccount, doorgeven als elke taak hoeven in te roepen CloudBlobContainer.CreateIfNotExistsAsync voor de container wordt opgeslagen.

## <a name="pass-parameters-and-environment-variables"></a>Geef parameters en variabelen
### <a name="pass-environment-settings"></a>Pass-omgevingsinstellingen
Een client kan gegevens doorgeven aan de jobbeheertaak in de vorm van omgevingsinstellingen. Deze informatie kan vervolgens worden gebruikt door de jobbeheertaak bij het genereren van de taak processor-taken die worden uitgevoerd als onderdeel van de compute-taak. Voorbeelden van de informatie die u als omgevingsinstellingen doorgeven kunt zijn:

* De naam en sleutels van opslagaccount
* Batch-account-URL
* Batch-accountsleutel

De Batch-service heeft een eenvoudig mechanisme omgevingsinstellingen doorgeven aan een jobbeheertaak met behulp van de `EnvironmentSettings` eigenschap in [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Bijvoorbeeld, om op te halen de `BatchClient` exemplaar voor een Batch-account, kunt u als omgevingsvariabelen van de client de URL en de gedeelde sleutel referenties voor het Batch-account code doorgeven. Evenzo, voor toegang tot het opslagaccount dat is gekoppeld aan het Batch-account, u kunt doorgeven naam van het opslagaccount en de opslagaccountsleutel als omgevingsvariabelen.

### <a name="pass-parameters-to-the-job-manager-template"></a>Parameters doorgeven aan de Job Manager-sjabloon
In veel gevallen is het handig per taak parameters doorgeven aan de jobbeheertaak, voor het beheren van de taak proces splitsen of de taken voor de taak configureren. U kunt dit doen door het uploaden van een JSON-bestand met de naam parameters.json als bronbestand voor de jobbeheertaak. De parameters kunnen vervolgens worden beschikbaar in de `JobSplitter._parameters` veld in de Job Manager-sjabloon.

> [!NOTE]
> De handler ingebouwde parameter ondersteunt alleen de woordenboeken tekenreeks-naar-tekenreeks. Als u complexe JSON-waarden als parameterwaarden doorgeven wilt, moet u deze als tekenreeksen doorgeven en in het splitsen van de taak kan parseren of wijzigen van het framework `Configuration.GetJobParameters` methode.
> 
> 

### <a name="pass-parameters-to-the-task-processor-template"></a>Parameters doorgeven aan de taak Processor-sjabloon
U kunt ook parameters doorgeven aan de afzonderlijke taken die zijn geïmplementeerd met behulp van de taak Processor-sjabloon. Net als met de taak manager-sjabloon, de sjabloon van de processor taak zoekt naar een resourcebestand met de naam

parameters.JSON, en als deze gevonden als de parameters-woordenlijst wordt geladen. Er zijn een aantal opties voor informatie over parameters doorgeven aan de taak processor-taken:

* De taakparameters JSON hergebruiken. Dit werkt ook als de enige parameters gehele taak (voor bijvoorbeeld een render hoogte en breedte zijn). Om dit te doen, bij het maken van een CloudTask in het splitsen van de taak, Voeg een verwijzing naar de gehydrateerd parameters.json-bestand van de jobbeheertaak ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) op van de CloudTask ResourceFiles verzameling.
* Genereren en uploaden van een document taakspecifieke parameters.json als onderdeel van het uitvoeren van taak splitsen en verwijzen naar die blob in de resourceverzameling bestanden van de taak. Dit is nodig als andere taken verschillende parameters hebben. Een voorbeeld is mogelijk een 3D-rendering-scenario waarbij de frame-index wordt doorgegeven aan de taak als een parameter.

> [!NOTE]
> De handler ingebouwde parameter ondersteunt alleen de woordenboeken tekenreeks-naar-tekenreeks. Als u complexe JSON-waarden als parameterwaarden doorgeven wilt, moet u deze als tekenreeksen doorgeven en in de taak-processor kan parseren of wijzigen van het framework `Configuration.GetTaskParameters` methode.
> 
> 

## <a name="next-steps"></a>Volgende stappen
### <a name="persist-job-and-task-output-to-azure-storage"></a>Behouden van taken kunt uitvoeren naar Azure Storage
Een andere handige hulpprogramma in de ontwikkeling van de Batch-oplossing is [Azure Batch File Conventions][nuget_package]. Gebruik deze .NET-klassebibliotheek (momenteel in preview) in de Batch .NET-toepassingen eenvoudig opslaan en ophalen van de taak uitvoer en naar Azure Storage. [Azure Batch-taak en de taak uitvoer behouden](batch-task-output.md) bevat een volledige beschrijving van de bibliotheek en het gebruik ervan.


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

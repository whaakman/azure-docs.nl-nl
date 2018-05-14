---
title: Bewaken van Batch met Azure Application Insights | Microsoft-documenten
description: Informatie over het instrumenteren van een Azure Batch .NET-toepassing met de Azure Application Insights-bibliotheek.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: .NET
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: 9f989ada01a2ffced509b42df9e46aa001386ab6
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="monitor-and-debug-an-azure-batch-net-application-with-application-insights"></a>Bewaken en fouten opsporen in een Azure Batch .NET-toepassing met Application Insights

[Application Insights](../application-insights/app-insights-overview.md) biedt een elegante en krachtige manier voor ontwikkelaars voor monitor en foutopsporing geïmplementeerde toepassingen op Azure-services. Gebruik Application Insights voor monitor prestatiemeteritems en uitzonderingen en instrument uw code met aangepaste metrische gegevens en tracering. Application Insights integratie met uw Azure Batch-toepassing, kunt u grondige inzicht in gedrag en onderzoeken van problemen in near-realtime.

In dit artikel laat zien hoe toevoegen en configureren van de Application Insights-bibliotheek in uw Azure Batch .NET-oplossing en het instrumenteren van uw toepassingscode. U ziet ook manieren om uw toepassingen via de Azure-portal bewaken en aangepaste dashboards bouwen. Voor Application Insights in andere talen ondersteunen, bekijk de [talen, platforms en integraties documentatie](../application-insights/app-insights-platforms.md).

Een voorbeeld C#-oplossing met code ter ondersteuning van dit artikel is beschikbaar op [GitHub](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights). Application Insights instrumentation code naar het volgende voorbeeld wordt de [TopNWords](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords) voorbeeld. Als u niet bekend met in dat geval, probeer te bouwen en uitvoeren van TopNWords eerst bent. Hierdoor krijgt u inzicht van Batch basiswerkstroom van de verwerking van een reeks invoer blobs parallel op meerdere rekenknooppunten. 

## <a name="prerequisites"></a>Vereisten
* [Visual Studio IDE](https://www.visualstudio.com/vs) (Visual Studio 2015 of een recentere versie)

* [Batch-account en gekoppelde storage-account](batch-account-create-portal.md)

* [Application Insights-resource](../application-insights/app-insights-create-new-resource.md)
  
   * De Azure portal gebruiken voor het maken van een Application Insights *resource*. Selecteer de *algemene* **toepassingstype**.

   * Kopieer de [instrumentatiesleutel](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key) vanuit de portal. Verderop in dit artikel is verplicht.
  
  > [!NOTE]
  > Hebt u mogelijk [in rekening gebracht](https://azure.microsoft.com/pricing/details/application-insights/) voor de gegevens die zijn opgeslagen in Application Insights. Dit omvat de diagnose en het bewaken van gegevens die in dit artikel wordt besproken.
  > 

## <a name="add-application-insights-to-your-project"></a>Application Insights toevoegen aan uw project

De **Microsoft.ApplicationInsights.WindowsServer** NuGet-pakket en de bijbehorende afhankelijkheden vereist zijn voor uw project. Toevoegen of deze terugzetten naar project van uw toepassing. U kunt het pakket installeren met de `Install-Package` opdracht of NuGet Package Manager.

```powershell
Install-Package Microsoft.ApplicationInsights.WindowsServer
```
Naar Application Insights verwijzen vanuit een .NET-toepassing met behulp van de **Microsoft.ApplicationInsights** naamruimte.

## <a name="instrument-your-code"></a>Uw code softwareontwikkelaars

Als u wilt uw code softwareontwikkelaars, uw oplossing moet maken van een Application Insights [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient). In het voorbeeld wordt de TelemetryClient laadt de configuratie van de [ApplicationInsights.config](../application-insights/app-insights-configuration-with-applicationinsights-config.md) bestand. Zorg ervoor dat ApplicationInsights.config bijwerken in de volgende projecten met uw Application Insights-instrumentatiesleutel: Microsoft.Azure.Batch.Samples.TelemetryStartTask en TopNWordsSample.

```xml
<InstrumentationKey>YOUR-IKEY-GOES-HERE</InstrumentationKey>
```
De instrumentatiesleutel ook toevoegen in het bestand TopNWords.cs.

Het voorbeeld in TopNWords.cs gebruikgemaakt van de volgende [instrumentation aanroepen](../application-insights/app-insights-api-custom-events-metrics.md) uit de Application Insights-API:
* `TrackMetric()` -Houdt hoe lang, gemiddeld een rekenknooppunt nodig is voor het downloaden van het tekstbestand vereist.
* `TrackTrace()` -Foutopsporing aanroepen naar uw code toegevoegd.
* `TrackEvent()` -Nummers interessante gebeurtenissen vast te leggen.

In dit voorbeeld uitzonderingsverwerking opzettelijk weggelaten. In plaats daarvan rapporteert Application Insights automatisch niet-verwerkte uitzonderingen die aanzienlijk verbetert de ervaring van de foutopsporing. 

Het volgende fragment laat zien hoe deze methoden te gebruiken.

```csharp
public void CountWords(string blobName, int numTopN, string storageAccountName, string storageAccountKey)
{
    // simulate exception for some set of tasks
    Random rand = new Random();
    if (rand.Next(0, 10) % 10 == 0)
    {
        blobName += ".badUrl";
    }

    // log the url we are downloading the file from
    insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Download file from: {1}", this.taskId, blobName), SeverityLevel.Verbose));

    // open the cloud blob that contains the book
    var storageCred = new StorageCredentials(storageAccountName, storageAccountKey);
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName), storageCred);
    using (Stream memoryStream = new MemoryStream())
    {
        // calculate blob download time
        DateTime start = DateTime.Now;
        blob.DownloadToStream(memoryStream);
        TimeSpan downloadTime = DateTime.Now.Subtract(start);

        // track how long the blob takes to download on this node
        // this will help debug timing issues or identify poorly performing nodes
        insightsClient.TrackMetric("Blob download in seconds", downloadTime.TotalSeconds, this.CommonProperties);

        memoryStream.Position = 0; //Reset the stream
        var sr = new StreamReader(memoryStream);
        var myStr = sr.ReadToEnd();
        string[] words = myStr.Split(' ');

        // log how many words were found in the text file
        insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Found {1} words", this.taskId, words.Length), SeverityLevel.Verbose));
        var topNWords =
            words.
                Where(word => word.Length > 0).
                GroupBy(word => word, (key, group) => new KeyValuePair<String, long>(key, group.LongCount())).
                OrderByDescending(x => x.Value).
                Take(numTopN).
                ToList();
        foreach (var pair in topNWords)
        {
            Console.WriteLine("{0} {1}", pair.Key, pair.Value);
        }

        // emit an event to track the completion of the task
        insightsClient.TrackEvent("Done counting words");
    }
}
```

### <a name="azure-batch-telemetry-initializer-helper"></a>Azure Batch telemetrie initialisatiefunctie helper
Tijdens het rapporteren van telemetrie voor een bepaalde server en het exemplaar, Application Insights maakt gebruik van de Azure VM-rol en de VM-naam voor de standaardwaarden. Het voorbeeld ziet in de context van Azure Batch, het gebruik van de naam van de toepassingen en in plaats daarvan knooppuntnaam berekenen. Gebruik een [telemetrie initialisatiefunctie](../application-insights/app-insights-api-filtering-sampling.md#add-properties) naar de standaardwaarden overschrijven. 

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;
using System;
using System.Threading;

namespace Microsoft.Azure.Batch.Samples.TelemetryInitializer
{
    public class AzureBatchNodeTelemetryInitializer : ITelemetryInitializer
    {
        // Azure Batch environment variables
        private const string PoolIdEnvironmentVariable = "AZ_BATCH_POOL_ID";
        private const string NodeIdEnvironmentVariable = "AZ_BATCH_NODE_ID";

        private string roleInstanceName;
        private string roleName;

        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                // override the role name with the Azure Batch Pool name
                string name = LazyInitializer.EnsureInitialized(ref this.roleName, this.GetPoolName);
                telemetry.Context.Cloud.RoleName = name;
            }

            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleInstance))
            {
                // override the role instance with the Azure Batch Compute Node name
                string name = LazyInitializer.EnsureInitialized(ref this.roleInstanceName, this.GetNodeName);
                telemetry.Context.Cloud.RoleInstance = name;
            }
        }

        private string GetPoolName()
        {
            return Environment.GetEnvironmentVariable(PoolIdEnvironmentVariable) ?? string.Empty;
        }

        private string GetNodeName()
        {
            return Environment.GetEnvironmentVariable(NodeIdEnvironmentVariable) ?? string.Empty;
        }
    }
}
```

Het bestand ApplicationInsights.config is in het project TopNWordsSample omvat om de initialisatiefunctie telemetrie, het volgende:

```xml
<TelemetryInitializers>
    <Add Type="Microsoft.Azure.Batch.Samples.TelemetryInitializer.AzureBatchNodeTelemetryInitializer, Microsoft.Azure.Batch.Samples.TelemetryInitializer"/>
</TelemetryInitializers>
``` 

## <a name="update-the-job-and-tasks-to-include-application-insights-binaries"></a>Bijwerken van de job en taken te nemen van de binaire bestanden voor Application Insights

Zorg dat de binaire bestanden juist worden geplaatst in de volgorde voor Application Insights correct uit te voeren op uw rekenknooppunten. Toevoegen aan uw taak resource bestanden verzameling binaire bestanden van de vereiste zodat ze ophalen gedownload op het moment dat de taak wordt uitgevoerd. De volgende codefragmenten zijn vergelijkbaar met de code in Job.cs.

Maak eerst een statische lijst met Application Insights-bestanden te uploaden.

```csharp
private static readonly List<string> AIFilesToUpload = new List<string>()
{
    // Application Insights config and assemblies
    "ApplicationInsights.config",
    "Microsoft.ApplicationInsights.dll",
    "Microsoft.AI.Agent.Intercept.dll",
    "Microsoft.AI.DependencyCollector.dll",
    "Microsoft.AI.PerfCounterCollector.dll",
    "Microsoft.AI.ServerTelemetryChannel.dll",
    "Microsoft.AI.WindowsServer.dll",
    
    // custom telemetry initializer assemblies
    "Microsoft.Azure.Batch.Samples.TelemetryInitializer.dll",
 };
...
```

Maak vervolgens de staging-bestanden die worden gebruikt door de taak.
```csharp
...
// create file staging objects that represent the executable and its dependent assembly to run as the task.
// These files are copied to every node before the corresponding task is scheduled to run on that node.
FileToStage topNWordExe = new FileToStage(TopNWordsExeName, stagingStorageAccount);
FileToStage storageDll = new FileToStage(StorageClientDllName, stagingStorageAccount);

// Upload Application Insights assemblies
List<FileToStage> aiStagedFiles = new List<FileToStage>();
foreach (string aiFile in AIFilesToUpload)
{
    aiStagedFiles.Add(new FileToStage(aiFile, stagingStorageAccount));
}
...
```

De `FileToStage` methode is een Help-functie in de voorbeeldcode die u kunt gemakkelijk een bestand van de lokale schijf naar een Azure Storage-blob te uploaden. Elk bestand wordt vervolgens gedownload naar een rekenknooppunt en waarnaar wordt verwezen door een taak.

Ten slotte de taken toevoegen aan de job en de benodigde Application Insights-binaire bestanden bevatten.
```csharp
...
// initialize a collection to hold the tasks that will be submitted in their entirety
List<CloudTask> tasksToRun = new List<CloudTask>(topNWordsConfiguration.NumberOfTasks);
for (int i = 1; i <= topNWordsConfiguration.NumberOfTasks; i++)
{
    CloudTask task = new CloudTask("task_no_" + i, String.Format("{0} --Task {1} {2} {3} {4}",
        TopNWordsExeName,
        string.Format("https://{0}.blob.core.windows.net/{1}",
            accountSettings.StorageAccountName,
            documents[i]),
        topNWordsConfiguration.TopWordCount,
        accountSettings.StorageAccountName,
        accountSettings.StorageAccountKey));

    //This is the list of files to stage to a container -- for each job, one container is created and 
    //files all resolve to Azure Blobs by their name (so two tasks with the same named file will create just 1 blob in
    //the container).
    task.FilesToStage = new List<IFileStagingProvider>
                        {
                            // required application binaries
                            topNWordExe,
                            storageDll,
                        };
    foreach (FileToStage stagedFile in aiStagedFiles)
   {
        task.FilesToStage.Add(stagedFile);
   }    
    task.RunElevated = false;
    tasksToRun.Add(task);
}
```

## <a name="view-data-in-the-azure-portal"></a>Gegevens weergeven in de Azure portal

Nu dat u de taak en taken voor het gebruik van Application Insights hebt geconfigureerd, moet u de voorbeeld-taak uitvoeren in uw groep. Navigeer naar de Azure-portal en open de Application Insights-resource die u hebt ingericht. Nadat de groep van toepassingen is geconfigureerd, moet u eerst om te zien waarbij de gegevens en ophalen van het logboek geregistreerd. De rest van dit artikel raakt op slechts een paar Application Insights-functies, maar de volledige functieset verkennen gerust.

### <a name="view-live-stream-data"></a>Live stream-gegevens weergeven

Als u wilt de traceringslogboeken weergeven in uw toepassingen Insights-resource **Live Stream**. De volgende schermafbeelding ziet het weergeven van dynamische gegevens die afkomstig zijn van de rekenknooppunten in de groep, bijvoorbeeld het CPU-gebruik per rekenknooppunt.

![Live stream compute-knooppuntgegevens](./media/monitor-application-insights/applicationinsightslivestream.png)

### <a name="view-trace-logs"></a>Traceringslogboeken weergeven

Als u wilt de traceringslogboeken weergeven in uw toepassingen Insights-resource **Search**. Deze weergave toont een lijst met diagnostische gegevens die zijn vastgelegd door Application Insights, met inbegrip van traceringen, gebeurtenissen en uitzonderingen. 

De volgende schermafbeelding ziet u hoe een Eén tracering voor een taak is geregistreerd en later voor foutopsporing worden opgevraagd.

![Afbeelding van Trace-Logboeken](./media/monitor-application-insights/tracelogsfortask.png)

### <a name="view-unhandled-exceptions"></a>Niet-verwerkte uitzonderingen weergeven

De volgende schermafbeeldingen ziet u hoe Application Insights uitzonderingen vanuit uw App registreert. In dit geval binnen enkele seconden van de toepassing die de uitzondering veroorzaakt, kunt u inzoomen op een specifieke uitzondering en vaststellen van het probleem.

![Niet-verwerkte uitzonderingen](./media/monitor-application-insights/exception.png)

### <a name="measure-blob-download-time"></a>Meting blob downloadtijd

Aangepaste metrische gegevens zijn ook een waardevol hulpmiddel in de portal. U kunt bijvoorbeeld de gemiddelde tijd die nodig elk rekenknooppunt was voor het downloaden van het vereiste tekstbestand dat het verwerken van weergeven.

Een voorbeeldgrafiek maken:
1. Klik in de Application Insights-resource op **Metrics Explorer** > **grafiek toevoegen**.
2. Klik op **bewerken** op de grafiek die is toegevoegd.
2. De grafiekdetails van de als volgt bijwerken:
   * Stel **grafiektype** naar **raster**.
   * Stel **aggregatie** naar **gemiddelde**.
   * Stel **groeperen op** naar **NodeId**.
   * In **metrische gegevens**, selecteer **aangepaste** > **Blob gedownload in seconden**.
   * Weergave aanpassen **kleurenpalet** voor uw keuze. 

![Downloadtijd BLOB per knooppunt](./media/monitor-application-insights/blobdownloadtime.png)


## <a name="monitor-compute-nodes-continuously"></a>Monitor rekenknooppunten continu

U mogelijk opgevallen dat alle metrische gegevens, met inbegrip van prestatiemeteritems, alleen worden geregistreerd wanneer de taken worden uitgevoerd. Dit gedrag is nuttig omdat deze wordt beperkt door de hoeveelheid gegevens die Application Insights-Logboeken. Er zijn echter gevallen wanneer u altijd wilt bewaken van de rekenknooppunten. Bijvoorbeeld, deze wordt mogelijk uitgevoerd achtergrond work. Dit is niet gepland via de Batch-service. In dit geval instellen een controleproces gedurende de levensduur van het rekenknooppunt worden uitgevoerd. 

Er is een manier om dit gedrag bereiken worden geproduceerd van een proces dat wordt geladen van de Application Insights-bibliotheek en op de achtergrond uitgevoerd. In het voorbeeld wordt de begintaak wordt geladen van de binaire bestanden op de machine en houdt een proces voor onbepaalde tijd wordt uitgevoerd. Configureer de Application Insights-configuratiebestand voor het verzenden van extra gegevens die u geïnteresseerd in, zoals prestatiemeteritems bent.

```csharp
...
 // Batch start task telemetry runner
private const string BatchStartTaskFolderName = "StartTask";
private const string BatchStartTaskTelemetryRunnerName = "Microsoft.Azure.Batch.Samples.TelemetryStartTask.exe";
private const string BatchStartTaskTelemetryRunnerAIConfig = "ApplicationInsights.config";
...
CloudPool pool = client.PoolOperations.CreatePool(
    topNWordsConfiguration.PoolId,
    targetDedicated: topNWordsConfiguration.PoolNodeCount,
    virtualMachineSize: "small",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));
...

// Create a start task which will run a dummy exe in background that simply emits performance
// counter data as defined in the relevant ApplicationInsights.config.
// Note that the waitForSuccess on the start task was not set so the Compute Node will be
// available immediately after this command is run.
pool.StartTask = new StartTask()
{
    CommandLine = string.Format("cmd /c {0}", BatchStartTaskTelemetryRunnerName),
    ResourceFiles = resourceFiles
};
...
```

> [!TIP]
> Voor een verhoging van de beheerbaarheid van uw oplossing, kunt u de assembly in bundelen een [toepassingspakket](./batch-application-packages.md). Vervolgens om automatisch het toepassingspakket implementeren op uw pools, voegt u een verwijzing application-pakket voor de configuratie van toepassingen.
>

## <a name="throttle-and-sample-data"></a>Vertraging en voorbeeld-gegevens 

Vanwege de grootschalige aard van Azure Batch-toepassingen die worden uitgevoerd in de productieomgeving, wilt u mogelijk de hoeveelheid gegevens die worden verzameld door Application Insights voor het beheren van de kosten beperken. Zie [meting in Application Insights](../application-insights/app-insights-sampling.md) voor sommige mechanismen om dit te bereiken.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Application Insights](../application-insights/app-insights-overview.md).

* Voor Application Insights in andere talen ondersteunen, bekijk de [talen, platforms en integraties documentatie](../application-insights/app-insights-platforms.md).



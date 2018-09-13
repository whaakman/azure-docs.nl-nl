---
title: Batch met Azure Application Insights bewaken | Microsoft Docs
description: Informatie over het instrumenteren van een Azure Batch .NET-toepassing met behulp van de Azure Application Insights-bibliotheek.
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
ms.openlocfilehash: 5e0358ebf525c39c09df4268971fa71c02457821
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35907488"
---
# <a name="monitor-and-debug-an-azure-batch-net-application-with-application-insights"></a>Bewaken en fouten opsporen in een Azure Batch .NET-toepassing met Application Insights

[Application Insights](../application-insights/app-insights-overview.md) biedt een elegante en krachtige manier voor ontwikkelaars om te controleren en fouten opsporen toepassingen die zijn geïmplementeerd met Azure-services. Gebruik Application Insights te bewaken van prestatiemeteritems en uitzonderingen, alsmede instrument uw code met aangepaste metrische gegevens en tracering. Integratie van Application Insights met uw Azure Batch-toepassing, kunt u meer inzicht in problemen en onderzoeken van problemen in near-real-time.

In dit artikel laat zien hoe toevoegen en configureren van de Application Insights-bibliotheek in uw Azure Batch .NET-oplossing en code van uw toepassing instrumenteren. U ziet ook manieren voor het bewaken van uw toepassing via Azure portal en aangepaste dashboards kunt bouwen. Voor Application Insights-ondersteuning in andere talen, bekijk de [programmeertalen, platforms en integraties documentatie](../application-insights/app-insights-platforms.md).

Een voorbeeld van C#-oplossing met code ter ondersteuning van dit artikel vindt u op [GitHub](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights). In dit voorbeeld instrumentation code Application Insights wordt de [TopNWords](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords) voorbeeld. Als u niet bekend met dit voorbeeld, probeer het bouwen en uitvoeren van TopNWords eerst bent. Dit doet, krijgt u inzicht in een algemene Batch-werkstroom van de verwerking van een reeks blobs voor invoer parallel op meerdere rekenknooppunten. 

## <a name="prerequisites"></a>Vereisten
* [Visual Studio 2017](https://www.visualstudio.com/vs)

* [Batch-account en gekoppelde storage-account](batch-account-create-portal.md)

* [Application Insights-resource](../application-insights/app-insights-create-new-resource.md)
  
   * De Azure portal gebruiken voor het maken van een Application Insights *resource*. Selecteer de *algemene* **toepassingstype**.

   * Kopieer de [instrumentatiesleutel](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key) vanuit de portal. Verderop in dit artikel is verplicht.
  
  > [!NOTE]
  > U kunt mogelijk [in rekening gebracht](https://azure.microsoft.com/pricing/details/application-insights/) voor de gegevens die zijn opgeslagen in Application Insights. Dit omvat de diagnose en controle van gegevens die in dit artikel worden besproken.
  > 

## <a name="add-application-insights-to-your-project"></a>Application Insights toevoegen aan uw project

De **Microsoft.ApplicationInsights.WindowsServer** NuGet-pakket en de bijbehorende afhankelijkheden zijn vereist voor uw project. Toevoegen of deze terugzetten naar project van uw toepassing. Voor het installeren van het pakket, gebruikt u de `Install-Package` opdracht of het NuGet-Pakketbeheer.

```powershell
Install-Package Microsoft.ApplicationInsights.WindowsServer
```
Naar Application Insights verwijzen vanuit uw .NET-toepassing met behulp van de **Microsoft.ApplicationInsights** naamruimte.

## <a name="instrument-your-code"></a>Instrumenteer uw code

Als u wilt instrumenteren van uw code, uw oplossing moet maken van een Application Insights [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient). In het voorbeeld wordt de TelemetryClient laadt de configuratie van de [ApplicationInsights.config](../application-insights/app-insights-configuration-with-applicationinsights-config.md) bestand. Zorg ervoor dat u ApplicationInsights.config in de volgende projecten bijwerken met de Application Insights-instrumentatiesleutel: Microsoft.Azure.Batch.Samples.TelemetryStartTask en TopNWordsSample.

```xml
<InstrumentationKey>YOUR-IKEY-GOES-HERE</InstrumentationKey>
```
Ook de instrumentatiesleutel in het bestand TopNWords.cs toevoegen.

Het voorbeeld in TopNWords.cs gebruikgemaakt van de volgende [instrumentation aanroepen](../application-insights/app-insights-api-custom-events-metrics.md) uit de Application Insights-API:
* `TrackMetric()` -Wordt bijgehouden hoe lang, Gemiddeld, een rekenknooppunt duurt om de vereiste tekst-bestand te downloaden.
* `TrackTrace()` -Toegevoegd foutopsporing aanroepen naar uw code.
* `TrackEvent()` -Nummers interessante gebeurtenissen vast te leggen.

In dit voorbeeld opzettelijk weggelaten afhandeling van uitzonderingen. In plaats daarvan rapporteert Application Insights automatisch niet-verwerkte uitzonderingen die de probleemoplossingservaring aanzienlijk wordt verbeterd. 

Het volgende codefragment laat zien hoe deze methoden te gebruiken.

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

### <a name="azure-batch-telemetry-initializer-helper"></a>Azure Batch telemetrie-initializer helper
Tijdens het rapporteren van telemetrie voor een bepaalde server en het exemplaar, Application Insights maakt gebruik van de Azure VM-rol en de VM-naam voor de standaardwaarden. In de context van Azure Batch ziet het voorbeeld u hoe u de naam van de toepassingen en compute knooppuntnaam in plaats daarvan. Gebruik een [telemetrische initializer](../application-insights/app-insights-api-filtering-sampling.md#add-properties) voor de onderdrukking van de standaardwaarden. 

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

Als u wilt de initialisatiefunctie voor telemetrie inschakelen, moet aan het bestand ApplicationInsights.config in het project TopNWordsSample omvat het volgende:

```xml
<TelemetryInitializers>
    <Add Type="Microsoft.Azure.Batch.Samples.TelemetryInitializer.AzureBatchNodeTelemetryInitializer, Microsoft.Azure.Batch.Samples.TelemetryInitializer"/>
</TelemetryInitializers>
``` 

## <a name="update-the-job-and-tasks-to-include-application-insights-binaries"></a>De taak en taken om op te nemen van Application Insights-binaire bestanden bijwerken

Controleer of dat de binaire bestanden correct worden geplaatst in de volgorde voor Application Insights correct uit te voeren op uw rekenknooppunten. Binaire bestanden voor de vereiste toevoegen aan resourceverzameling voor bestanden van uw taak, zodat ze gedownload op het moment dat de taak wordt uitgevoerd. De volgende codefragmenten zijn vergelijkbaar met de code in Job.cs.

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

De `FileToStage` methode is een Help-functie in het codevoorbeeld waarmee u eenvoudig een bestand van de lokale schijf naar een Azure Storage-blob te uploaden. Elk bestand wordt later naar een rekenknooppunt wordt gedownload en waarnaar wordt verwezen door een taak.

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

## <a name="view-data-in-the-azure-portal"></a>Gegevens weergeven in Azure portal

Nu dat u de taak en taken met Application Insights hebt geconfigureerd, moet u de voorbeeld-taak uitvoeren in uw pool. Navigeer naar de Azure-portal en open de Application Insights-resource die u hebt ingericht. Nadat de groep is ingericht, moet u gaan zien gegevens doorgestuurd en ophalen in het logboek geregistreerd. De rest van dit artikel is van toepassing op slechts een paar Application Insights-functies, maar u kunt de volledige functieset worden besproken.

### <a name="view-live-stream-data"></a>Live stream-gegevens weergeven

Als u wilt weergeven van logboeken met traceringen in uw toepassingen Insights-resource, klikt u op **Live Stream**. De volgende schermafbeelding ziet u hoe u live-gegevens die afkomstig zijn van de rekenknooppunten in de groep, bijvoorbeeld het CPU-gebruik per knooppunt weergeven.

![Live stream compute-knooppuntgegevens](./media/monitor-application-insights/applicationinsightslivestream.png)

### <a name="view-trace-logs"></a>Traceringslogboeken weergeven

Als u wilt weergeven van logboeken met traceringen in uw toepassingen Insights-resource, klikt u op **zoeken**. Deze weergave toont een lijst met diagnostische gegevens die zijn vastgelegd door Application Insights, met inbegrip van traceringen, gebeurtenissen en uitzonderingen. 

De volgende schermafbeelding ziet u hoe een enkele tracering voor een taak is geregistreerd en later een query uitgevoerd voor foutopsporing.

![Afbeelding van Trace-Logboeken](./media/monitor-application-insights/tracelogsfortask.png)

### <a name="view-unhandled-exceptions"></a>Niet-verwerkte uitzonderingen weergeven

De volgende schermafbeeldingen laat zien hoe Application Insights-logboeken uitzonderingen van uw toepassing. In dit geval binnen enkele seconden van de toepassing die de uitzondering veroorzaakt, kunt u inzoomen op een specifieke uitzondering en vaststellen van het probleem.

![Niet-verwerkte uitzonderingen](./media/monitor-application-insights/exception.png)

### <a name="measure-blob-download-time"></a>Meting blob-downloadtijd

Aangepaste metrische gegevens zijn ook een waardevol hulpmiddel in de portal. U kunt bijvoorbeeld de gemiddelde tijd die het heeft elk knooppunt voor het downloaden van het vereiste bestand dat het verwerken van weergeven.

Een voorbeeldgrafiek maken:
1. Klik in uw Application Insights-resource op **Metrics Explorer** > **toevoegen grafiek**.
2. Klik op **bewerken** in de grafiek die is toegevoegd.
2. De grafiekdetails van de als volgt bijwerken:
   * Stel **grafiektype** naar **raster**.
   * Stel **aggregatie** naar **gemiddelde**.
   * Stel **groeperen op** naar **NodeId**.
   * In **metrische gegevens**, selecteer **aangepaste** > **Blob download in een paar seconden**.
   * Weergave aanpassen **kleurenpalet** naar keuze. 

![Downloadtijd voor BLOB per knooppunt](./media/monitor-application-insights/blobdownloadtime.png)


## <a name="monitor-compute-nodes-continuously"></a>Rekenknooppunten continu bewaken

U wellicht hebt opgemerkt dat alle metrische gegevens, met inbegrip van prestatiemeteritems, alleen worden geregistreerd wanneer de taken worden uitgevoerd. Dit gedrag is nuttig omdat deze wordt beperkt door de hoeveelheid gegevens die Application Insights-Logboeken. Er zijn echter gevallen wanneer u graag altijd voor het bewaken van de compute-knooppunten. Bijvoorbeeld, ook ze worden uitgevoerd achtergrond work. Dit is niet gepland via de Batch-service. In dit geval, instellen van een controleproces voor de levensduur van het rekenknooppunt uit te voeren. 

Een manier om dit gedrag bewerkstelligen is voor het produceren van een proces dat de Application Insights-bibliotheek geladen en uitgevoerd op de achtergrond. In het voorbeeld wordt de begintaak laadt de binaire bestanden op de machine en een proces wordt uitgevoerd voor onbepaalde tijd blijft. Configureer het Application Insights-configuratiebestand voor dit proces om te verzenden als u meer gegevens die u geïnteresseerd in, zoals prestatiemeteritems bent.

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
    virtualMachineSize: "standard_d1_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));
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
> Als u wilt de beheerbaarheid van uw oplossing verhogen, kunt u de assembly in het bundelen een [toepassingspakket](./batch-application-packages.md). Vervolgens voor het implementeren van het toepassingspakket automatisch naar uw groepen, voegt u een verwijzing naar het toepassingspakket voor configuratie van de pool.
>

## <a name="throttle-and-sample-data"></a>Vertraging en voorbeeldgegevens 

Vanwege de grootschalige aard van Azure Batch-toepassingen in productie, is het raadzaam om de hoeveelheid gegevens die zijn verzameld door Application Insights voor het beheren van kosten te beperken. Zie [steekproeven in Application Insights](../application-insights/app-insights-sampling.md) voor sommige methoden om dit te bereiken.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Application Insights](../application-insights/app-insights-overview.md).

* Voor Application Insights-ondersteuning in andere talen, bekijk de [programmeertalen, platforms en integraties documentatie](../application-insights/app-insights-platforms.md).



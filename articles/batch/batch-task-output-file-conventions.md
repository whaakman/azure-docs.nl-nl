---
title: Taak en uitvoer naar Azure Storage met de bestand Conventions-bibliotheek voor .NET - Azure Batch behouden | Microsoft Docs
description: Informatie over het bestand conventies van Azure Batch-bibliotheek voor .NET gebruiken voor het behouden van de Batch-taak en de taak uitvoer naar Azure Storage en de persistente uitvoer weergeven in de Azure portal.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a9de327c20463469bc91d9720aa17333a36f919e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net-to-persist"></a>Behouden van de taak en gegevens naar Azure Storage met de Batch-bestand Conventions-bibliotheek voor .NET voor het persistent maken 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Een manier om taakgegevens is met de [bestand conventies van Azure Batch-bibliotheek voor .NET][nuget_package]. De bibliotheek bestand conventies vereenvoudigt het proces voor het opslaan van taak uitvoergegevens naar Azure Storage en het ophalen van het. U kunt de bibliotheek bestanden in de taak en de client &mdash; in taakcode voor persistent maken van bestanden en in clientcode weergeven en ophalen van deze. Uw taakcode kunt ook de bibliotheek gebruiken voor het ophalen van de uitvoer van de upstream-taken, zoals een [taakafhankelijkheden](batch-task-dependencies.md) scenario. 

Voor het ophalen van uitvoerbestanden met de bibliotheek bestand verdragen, kunt u de bestanden voor een bepaalde taak of de taak vinden door deze door de ID en het doel. U hoeft niet te weten de namen of locaties van de bestanden. U kunt bijvoorbeeld gebruikmaken van de tapewisselaar bestand conventies voor een lijst met alle tussenliggende bestanden voor een bepaalde taak of een preview-bestand voor een bepaalde taak ophalen.

> [!TIP]
> Vanaf versie 2017-05-01 ondersteunt de Batch-service-API persistent maken uitvoergegevens naar Azure Storage voor taken en jobbeheertaken die worden uitgevoerd op de groepen die zijn gemaakt met de configuratie van de virtuele machine. De API van Batch-service biedt een eenvoudige manier om vast te leggen van de uitvoer van de code die een taak maakt en fungeert als een alternatief voor het bestand Conventions-bibliotheek. Uw Batch-clienttoepassingen om vast te leggen uitvoer zonder bijwerken van de toepassing die de taak wordt uitgevoerd, kunt u wijzigen. Zie voor meer informatie [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md).
> 
> 

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a>Wanneer gebruik de bibliotheek bestand verdragen om vast te leggen van de uitvoer van de taak?

Azure Batch biedt meer dan één manier om vast te leggen van de uitvoer van de taak. De conventies bestand is het meest geschikt is voor deze scenario's:

- U kunt de code voor de toepassing die de taak wordt uitgevoerd om te blijven behouden bestanden met behulp van de bibliotheek bestand conventies eenvoudig wijzigen.
- Wilt u van stroomgegevens naar Azure Storage terwijl de taak nog actief.
- Wilt u gegevens uit toepassingen die zijn gemaakt met de configuratie van de cloud-service of de configuratie van de virtuele machine persistent maken.
- Uw clienttoepassing of andere taken in de taak moet om te zoeken en downloaden van de taak uitvoerbestanden ID of met het doel. 
- U wilt weergeven van uitvoer van de taak in de Azure portal.

Als uw scenario van die verschilt hierboven zijn vermeld, moet u wellicht een andere benadering overwegen. Zie voor meer informatie over andere opties voor het persistent maken van de taakuitvoer [behouden van de taak en uitvoer naar Azure Storage](batch-task-output.md). 

## <a name="what-is-the-batch-file-conventions-standard"></a>Wat is de standaard Batch bestand conventies?

De [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) biedt een schematische naam voor de bestemming containers en blobs paden waarnaar de uitvoerbestanden worden geschreven. Bestanden die zijn opgeslagen in Azure Storage die voldoen aan de bestanden standaard automatisch beschikbaar zijn voor weergave in de Azure portal. De portal is op de hoogte van de naamconventie en dus kan bestanden die aan deze voldoen worden weergegeven.

Het bestand Conventions-bibliotheek voor .NET worden automatisch weergegeven in de naam van een uw storage-containers en uitvoerbestanden taak volgens de conventies bestand standaard. De bibliotheek bestand conventies biedt ook methoden voor het opvragen van de uitvoerbestanden in Azure Storage volgens de taak-ID, taak-ID of doel.   

Als u met een andere taal dan .NET ontwikkelt, kunt u implementeren de bestanden standaard uzelf in uw toepassing. Zie voor meer informatie [de conventies voor Batch-bestand standaard](batch-task-output.md#about-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-to-your-batch-account"></a>Een Azure Storage-account koppelen aan uw Batch-account

Om te blijven behouden uitvoergegevens naar Azure Storage met behulp van het bestand Conventions-bibliotheek, moet u eerst een Azure Storage-account koppelen aan uw Batch-account. Als u dit nog niet hebt gedaan, koppelt u een opslagaccount aan uw Batch-account met behulp van de [Azure-portal](https://portal.azure.com):

1. Ga in Azure Portal naar uw Batch-account. 
2. Onder **instellingen**, selecteer **Opslagaccount**.
3. Als u nog geen een opslagaccount die is gekoppeld aan uw Batch-account, klikt u op **Storage-Account (geen)**.
4. Selecteer een opslagaccount in de lijst voor uw abonnement. Gebruik een Azure Storage-account dat zich in dezelfde regio bevinden als het Batch-account waarop uw taken worden uitgevoerd voor de beste prestaties.

## <a name="persist-output-data"></a>Uitvoergegevens behouden

Om te blijven behouden taak en uitvoergegevens met de bibliotheek bestand verdragen, een container maken in Azure Storage en sla vervolgens de uitvoer naar de container. Gebruik de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in uw taakcode voor het uploaden van de uitvoer van de taak voor de container. 

Zie voor meer informatie over het werken met containers en blobs in Azure Storage [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Alle uitvoerwaarden van taak en persistent is gemaakt met het bestand conventies bibliotheek worden opgeslagen in dezelfde container. Als een groot aantal taken voor het persistent maken van bestanden op hetzelfde moment probeert [opslag beperking limieten](../storage/common/storage-performance-checklist.md#blobs) kan worden afgedwongen.
> 
> 

### <a name="create-storage-container"></a>Opslagcontainer maken

Om te blijven behouden taakuitvoer naar Azure Storage, eerst een container maken door het aanroepen van [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. Deze uitbreidingsmethode heeft een [CloudStorageAccount] [ net_cloudstorageaccount] -object als parameter. Wordt gemaakt van een container met de naam volgens de standaard bestand verdragen, zodat de inhoud ervan kunnen gevonden door de Azure-portal worden en de ophalen-methoden die later in dit artikel wordt besproken.

U doorgaans de code voor het maken van een container in uw clienttoepassing plaatsen &mdash; de toepassing die wordt gemaakt van uw pools, jobs en taken.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Taakuitvoer Store

Nu dat u een container in Azure Storage hebt voorbereid, taken uitvoer naar de container kunnen opslaan met behulp van de [TaskOutputStorage] [ net_taskoutputstorage] klasse gevonden in de bibliotheek bestand verdragen.

In uw taakcode maakt u eerst een [TaskOutputStorage] [ net_taskoutputstorage] object wanneer de taak is voltooid zijn werk, en roep vervolgens de [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] methode voor het opslaan van de uitvoer naar Azure Storage.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

De `kind` parameter van de [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) methode categoriseert de persistente bestanden. Er zijn vier vooraf gedefinieerde [TaskOutputKind] [ net_taskoutputkind] typen: `TaskOutput`, `TaskPreview`, `TaskLog`, en `TaskIntermediate.` ook kunt u aangepaste categorieën van uitvoer.

Deze uitvoer-typen kunnen u opgeven welk type uitvoer om weer te geven als u later Batch een query voor het persistent gemaakte uitvoerwaarden van een bepaalde taak. Met andere woorden, wanneer u de uitvoer voor een taak wilt weergeven, kunt u de lijst op een van de typen uitvoer filteren. Bijvoorbeeld: ' Ik wil de *preview* uitvoer voor de taak *109*. " Meer informatie over het weergeven en ophalen van de uitvoer wordt weergegeven in [uitvoer ophalen](#retrieve-output) verderop in het artikel.

> [!TIP]
> Het type uitvoer bepaalt ook waar in de Azure portal een bepaald bestand wordt weergegeven: *TaskOutput*-gecategoriseerde bestanden worden weergegeven onder **taak uitvoerbestanden**, en *TaskLog* bestanden worden weergegeven onder **logboeken van de taak**.
> 
> 

### <a name="store-job-outputs"></a>Opslaan van de taak uitvoer

Naast de uitvoer van de taak op te slaan, kunt u de uitvoer die zijn gekoppeld aan een volledige taak opslaan. In de taak samenvoegen van een taak van de rendering film, kunt u de volledig gerenderde film kan behouden als de taakuitvoer van een. Wanneer de taak is voltooid, uw clienttoepassing kunt weergeven en ophalen van de uitvoer voor de taak en heeft geen query uitvoeren op de afzonderlijke taken nodig.

Taakuitvoer slaan door het aanroepen van de [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] methode, en geef de [JobOutputKind] [ net_joboutputkind] en bestandsnaam:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Net als bij de **TaskOutputKind** type voor de taak uitvoer, u de [JobOutputKind] [ net_joboutputkind] type om een taak te categoriseren de opgeslagen bestanden. Deze parameter kunt u later query voor (lijst) voor een specifiek type uitvoer. De **JobOutputKind** type bevat zowel uitvoer als preview-categorieën en biedt ondersteuning voor het maken van aangepaste categorieën.

### <a name="store-task-logs"></a>Taak logboeken worden opgeslagen

Naast het behouden blijven van een bestand naar een duurzame opslag wanneer een taak of de taak is voltooid, moet u mogelijk persistent maken van bestanden die zijn bijgewerkt tijdens het uitvoeren van een taak &mdash; logboekbestanden of `stdout.txt` en `stderr.txt`, bijvoorbeeld. Voor dit doel het bestand conventies van Azure Batch-bibliotheek biedt de [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] methode. Met [SaveTrackedAsync][net_savetrackedasync], kunt u updates naar een bestand op het knooppunt (met een interval dat u opgeeft) bijhouden en persistent maken die updates naar Azure Storage.

In het volgende codefragment gebruiken we [SaveTrackedAsync] [ net_savetrackedasync] bijwerken `stdout.txt` in Azure Storage elke 15 seconden tijdens de uitvoering van de taak:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
     await Task.Delay(stdoutFlushDelay);
}
```

De sectie met opmerkingen `Code to process data and produce output file(s)` is een tijdelijke aanduiding voor de code die uw taak normaal kunt uitvoeren. Bijvoorbeeld wellicht code die gegevens uit Azure Storage downloadt en transformatie of berekening op deze uitvoert. De belangrijk onderdeel van dit fragment is te demonstreren hoe u kunt laten teruglopen deze code in een `using` blok regelmatig bijwerken van een bestand met [SaveTrackedAsync][net_savetrackedasync].

De knooppunt-agent is een programma dat wordt uitgevoerd op elk knooppunt in de groep en de opdracht en controle interface vormt tussen het knooppunt en de Batch-service. De `Task.Delay` aanroep is vereist bij het einde van dit `using` blok om ervoor te zorgen dat de agent knooppunt tijd de inhoud van de standard out naar het bestand stdout.txt op het knooppunt leegmaken. Zonder deze vertraging is het mogelijk de laatste paar seconden van uitvoer mogen worden. Deze vertraging is mogelijk niet vereist voor alle bestanden.

> [!NOTE]
> Wanneer het inschakelen van bestand bijhouden met **SaveTrackedAsync**, alleen *voegt* naar het bestand bijgehouden zijn opgeslagen in Azure Storage. Gebruik deze methode alleen voor het bijhouden van de logboekbestanden niet draaien of andere bestanden die moeten worden toegevoegd met bewerkingen aan het einde van het bestand worden geschreven.
> 
> 

## <a name="retrieve-output-data"></a>Uitvoergegevens ophalen

Wanneer u de persistente uitvoer met de Azure Batch-bestand conventies bibliotheek ophaalt, kunt u dat doen op een taak en taak-gericht manier. U kunt de uitvoer van de aanvragen voor de gegeven taak of taak zonder te weten het pad in Azure Storage of zelfs de naam van het bestand. U kunt in plaats daarvan uitvoerbestanden aanvragen door de taak of taak-ID.

Het volgende codefragment taken van een taak doorloopt, wordt bepaalde informatie over de uitvoerbestanden voor de taak afgedrukt en downloadt u de bestanden uit de opslag.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-the-azure-portal"></a>De uitvoerbestanden weergeven in de Azure portal

De Azure-portal weergegeven taak uitvoerbestanden en logboeken die zijn opgeslagen in een gekoppelde Azure Storage-account met de [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). U kunt ook zelf deze conventies implementeren in de een taal van uw keuze of u kunt de bibliotheek bestanden in uw .NET-toepassingen gebruiken.

Om de weergave van de uitvoerbestanden in de portal, moet u voldoen aan de volgende vereisten:

1. [Een Azure Storage-account koppelen](#requirement-linked-storage-account) aan uw Batch-account.
2. Voldoen aan de vooraf gedefinieerde naamconventies voor Storage-containers en bestanden als uitvoer persistent maken. U kunt de definitie van deze overeenkomsten vinden in de bibliotheek bestand conventies [Leesmij][github_file_conventions_readme]. Als u de [Azure Batch-bestand conventies] [ nuget_package] bibliotheek om vast te leggen van de uitvoer uw bestanden blijven aanwezig op basis van de bestanden standaard.

Als u wilt weergeven logboeken en uitvoerbestanden taak in de Azure portal, gaat u naar de taak waarvan de uitvoer u geïnteresseerd bent in, klik dan ofwel **opgeslagen uitvoerbestanden** of **opgeslagen logboeken**. Deze afbeelding ziet u de **uitvoerbestanden opgeslagen** voor de taak met ID '007':

![Taakuitvoer blade in de Azure portal][2]

## <a name="code-sample"></a>Codevoorbeeld

De [PersistOutputs] [ github_persistoutputs] voorbeeldproject is een van de [Azure Batch-codevoorbeelden] [ github_samples] op GitHub. Deze Visual Studio-oplossing laat zien hoe de Azure Batch-bestand conventies-bibliotheek gebruiken om vast te leggen van de uitvoer van de taak naar duurzame opslag. Als u wilt uitvoeren in het voorbeeld, de volgende stappen uit:

1. Open het project in **Visual Studio 2015 of hoger**.
2. Toevoegen van uw Batch- en Storage **accountreferenties** naar **AccountSettings.settings** in het project Microsoft.Azure.Batch.Samples.Common.
3. **Bouwen** (maar niet worden uitgevoerd) de oplossing. Herstel alle NuGet-pakketten als daarom wordt gevraagd.
4. De Azure portal gebruiken voor het uploaden van een [toepassingspakket](batch-application-packages.md) voor **PersistOutputsTask**. Bevatten de `PersistOutputsTask.exe` en afhankelijke assembly's in het ZIP-pakket, de toepassings-ID aan 'PersistOutputsTask' en de versie van het toepassingspakket naar "1.0".
5. **Start** (uitvoeren) de **PersistOutputs** project.
6. Als u wordt gevraagd om te kiezen de persistentie-technologie gebruiken voor het uitvoeren van de steekproef, geeft u **1** om uit te voeren van het voorbeeld met de bibliotheek bestand verdragen om vast te leggen van de uitvoer van de taak. 

## <a name="next-steps"></a>Volgende stappen

### <a name="get-the-batch-file-conventions-library-for-net"></a>Ophalen van de Batch-bestand Conventions-bibliotheek voor .NET

De Batch-bestand Conventions-bibliotheek voor .NET is beschikbaar op [NuGet][nuget_package]. De bibliotheek breidt de [CloudJob] [ net_cloudjob] en [CloudTask] [ net_cloudtask] klassen met nieuwe methoden. Zie ook de [documentatie verwijst naar](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) voor de bibliotheek bestand verdragen.

De [broncode] [ github_file_conventions] voor de conventies bestand bibliotheek is beschikbaar op GitHub in de Microsoft Azure SDK voor .NET-opslagplaats. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Andere benaderingen voor persistent maken uitvoergegevens verkennen

- Zie [behouden van de taak en uitvoer naar Azure Storage](batch-task-output.md) voor een overzicht van de persistente gegevens van de taak en taak.
- Zie [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md) voor informatie over het gebruik van de API van Batch-service om uitvoergegevens.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "De uitvoerbestanden opgeslagen en opgeslagen logboeken selectoren in portal"
[2]: ./media/batch-task-output/task-output-02.png "Taakuitvoer blade in de Azure portal"

---
title: Taak en uitvoer behouden met Azure Storage met File Conventions-bibliotheek voor .NET - Azure Batch | Microsoft Docs
description: Informatie over het gebruik van Azure Batch File Conventions-bibliotheek voor .NET voor Batch-taak en de taak uitvoer naar Azure Storage behouden en de persistente uitvoer weergeven in Azure portal.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4d86a8fcd1dc85ccacea91afe36cb39dabe10464
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117593"
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net"></a>Behoud van de taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Een manier om vast te leggen van de taakgegevens is met de [Azure Batch File Conventions-bibliotheek voor .NET][nuget_package]. De File Conventions-bibliotheek vereenvoudigt het proces van het opslaan van gegevens van de taak uitvoer naar Azure Storage en het ophalen van het. U kunt de File Conventions-bibliotheek in de taak- en client code &mdash; in taakcode voor het behoud van bestanden en clientcode te lijst en deze op te halen. De taakcode van uw kunt ook de clientbibliotheek gebruiken om op te halen van de uitvoer van de upstream-taken, zoals een [taakafhankelijkheden](batch-task-dependencies.md) scenario. 

U kunt de bestanden voor een bepaalde taak of een taak om op te halen uitvoerbestanden met File Conventions-bibliotheek, vinden door ze door de ID en het doel weer te geven. U hoeft niet te weten wat de namen of locaties van de bestanden. U kunt bijvoorbeeld de File Conventions-bibliotheek gebruiken om alle tussenliggende bestanden voor een bepaalde taak weer te geven, of een preview-bestand voor een bepaalde taak.

> [!TIP]
> Vanaf versie 2017-05-01, ondersteunt de API voor Batch-service vastleggen van uitvoergegevens naar Azure Storage voor taken en taken van een job manager die worden uitgevoerd op pools die zijn gemaakt met de configuratie van de virtuele machine. De Batch-service-API biedt een eenvoudige manier om vast te leggen van de uitvoer van de code die een taak gemaakt en die fungeert als een alternatief voor de File Conventions-bibliotheek. Uw Batch-client-toepassingen om vast te leggen uitvoer zonder te hoeven bijwerken van de toepassing die de taak wordt uitgevoerd, kunt u wijzigen. Zie voor meer informatie, [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md).
> 
> 

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a>Wanneer gebruik ik de File Conventions-bibliotheek om vast te leggen van de uitvoer van de taak?

Azure Batch biedt meer dan één manier om vast te leggen van de uitvoer van de taak. De File Conventions is het meest geschikt is voor deze scenario's:

- U kunt de code voor de toepassing die de taak wordt uitgevoerd om te blijven behouden bestanden met behulp van de File Conventions-bibliotheek eenvoudig wijzigen.
- Wilt u streaminggegevens naar Azure Storage terwijl de taak is nog steeds uitgevoerd.
- U wilt dat het behoud van gegevens van pools die zijn gemaakt met de cloudserviceconfiguratie of de configuratie van de virtuele machine.
- Uw clienttoepassing of andere taken in de taak moet om te zoeken en downloaden van de uitvoerbestanden van taak-ID of doel. 
- U wilt weergeven van uitvoer van de taak in Azure portal.

Als uw scenario van die verschilt hierboven zijn vermeld, moet u mogelijk rekening houden met een andere benadering. Zie voor meer informatie over andere opties voor permanente taakuitvoer [behouden taken kunt uitvoeren naar Azure Storage](batch-task-output.md). 

## <a name="what-is-the-batch-file-conventions-standard"></a>Wat is de standaard Batch File Conventions?

De [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) biedt een schematische naam voor de doel-containers en de blob-paden waarnaar de uitvoerbestanden worden geschreven. Bestanden die zijn opgeslagen in een Azure-opslag die voldoen aan de standaard File Conventions zijn automatisch beschikbaar voor weergave in de Azure-portal. De portal op de hoogte van de naamconventie en bestanden die aan deze voldoen dus kunt weergeven.

De File Conventions-bibliotheek voor .NET worden automatisch weergegeven in de naam van een uw storage-containers en de taak uitvoerbestanden op basis van de standaard File Conventions. De File Conventions-bibliotheek biedt ook methoden om op te vragen van de uitvoerbestanden in Azure Storage op basis van taak-ID, taak-ID of doel.   

Als u met een andere taal dan .NET ontwikkelt, kunt u implementeren de standaard File Conventions zelf in uw toepassing. Zie voor meer informatie, [over de Batch File Conventions standard](batch-task-output.md#about-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-to-your-batch-account"></a>Een Azure Storage-account koppelt aan uw Batch-account

Als u wilt zich blijven voordoen uitvoergegevens naar Azure Storage met behulp van de File Conventions-bibliotheek, moet u eerst een Azure Storage-account aan uw Batch-account koppelen. Als u dit nog niet hebt gedaan, koppelt u een opslagaccount aan uw Batch-account met behulp van de [Azure-portal](https://portal.azure.com):

1. Ga in Azure Portal naar uw Batch-account. 
2. Onder **instellingen**, selecteer **Opslagaccount**.
3. Als u nog geen een Storage-account dat is gekoppeld aan uw Batch-account, klikt u op **Storage-Account (geen)**.
4. Selecteer een opslagaccount in de lijst voor uw abonnement. Gebruik een Azure Storage-account dat in dezelfde regio als het Batch-account waar uw taken worden uitgevoerd voor de beste prestaties.

## <a name="persist-output-data"></a>Uitvoergegevens die worden behouden

Een container maken in Azure Storage voor het behoud van gegevens in de taak en de uitvoer met de File Conventions-bibliotheek, en sla de uitvoer naar de container. Gebruik de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in uw taakcode voor het uploaden van de uitvoer van de taak naar de container. 

Zie voor meer informatie over het werken met containers en blobs in Azure Storage [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Alle uitvoer van taken persistent gemaakt met de File Conventions bibliotheek worden opgeslagen in dezelfde container. Als een groot aantal taken probeert om vast te leggen van bestanden op hetzelfde moment, [opslag beperkingslimieten](../storage/common/storage-performance-checklist.md#blobs) kan worden afgedwongen.
> 
> 

### <a name="create-storage-container"></a>Opslagcontainer maken

Als u wilt zich blijven voordoen taakuitvoer naar Azure Storage, eerst een container maken door het aanroepen van [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. Deze methode extensie heeft een [CloudStorageAccount] [ net_cloudstorageaccount] object als parameter. Het maakt een container met de naam op basis van de standaard File Conventions zo in dat de inhoud ervan kunnen worden gedetecteerd door de Azure-portal en het ophalen van methoden die later in dit artikel worden besproken.

U plaatst u doorgaans de code voor het maken van een container in uw clienttoepassing &mdash; de toepassing die wordt gemaakt van uw pools, jobs en taken.

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

### <a name="store-task-outputs"></a>Store-taakuitvoer

Nu dat u een container in Azure Storage hebt voorbereid, taken uitvoer naar de container kunnen opslaan met behulp van de [TaskOutputStorage] [ net_taskoutputstorage] klasse gevonden in de File Conventions-bibliotheek.

In de taakcode van uw, maakt u eerst een [TaskOutputStorage] [ net_taskoutputstorage] object en klik vervolgens wanneer de taak zijn werk heeft voltooid, roept de [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] methode voor het opslaan van de uitvoer naar Azure Storage.

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

De `kind` parameter van de [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) methode de persistente bestanden worden gecategoriseerd. Er zijn vier vooraf gedefinieerde [TaskOutputKind] [ net_taskoutputkind] typen: `TaskOutput`, `TaskPreview`, `TaskLog`, en `TaskIntermediate.` ook kunt u aangepaste categorieën van uitvoer.

Deze uitvoertypen kunnen u opgeven welk type uitvoer om wanneer u later Batch een query voor de persistente uitvoer van een bepaalde taak weer te geven. Wanneer u de uitvoer voor een taak, kunt u met andere woorden, de lijst op een van de uitvoertypen filteren. Bijvoorbeeld, "Ik wil de *preview* uitvoer voor de taak *109*." Meer informatie over de aanbieding en het ophalen van de uitvoer wordt weergegeven in [uitvoer ophalen](#retrieve-output) verderop in het artikel.

> [!TIP]
> Het type uitvoer bepaalt ook waar in de Azure-portal een bepaald bestand wordt weergegeven: *TaskOutput*-gecategoriseerde bestanden worden weergegeven onder **taak uitvoerbestanden**, en *TaskLog* bestanden worden weergegeven onder **taak logboeken**.
> 
> 

### <a name="store-job-outputs"></a>Store-taakuitvoer

Naast de taak uitvoer op te slaan, kunt u de uitvoer die zijn gekoppeld aan een volledige taak opslaan. In de taak samenvoegen van een renderingtaak film, kunt u de volledig gerenderde film kan behouden als de taakuitvoer van een. Wanneer de taak is voltooid, uw clienttoepassing kunt weergeven en ophalen van de uitvoer voor de taak, en hoeft niet te query uitvoeren op de afzonderlijke taken.

Taakuitvoer Store door het aanroepen van de [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] methode, en geef de [JobOutputKind] [ net_joboutputkind] en de bestandsnaam:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Net als bij de **TaskOutputKind** type voor de taak uitvoer, gebruikt u de [JobOutputKind] [ net_joboutputkind] type voor het categoriseren van een taak de opgeslagen bestanden. Deze parameter kunt u later query voor een specifiek type uitvoer-(lijst). De **JobOutputKind** type omvat zowel uitvoer als preview-categorieën en biedt ondersteuning voor het maken van aangepaste categorieën.

### <a name="store-task-logs"></a>Logboeken voor Store-taak

Naast het opslaan van een bestand naar een duurzame opslag wanneer een taak of de taak is voltooid, moet u mogelijk behouden bestanden die zijn bijgewerkt tijdens het uitvoeren van een taak &mdash; logboekbestanden of `stdout.txt` en `stderr.txt`, bijvoorbeeld. Voor dit doel, de Azure Batch File Conventions-bibliotheek biedt de [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] methode. Met [SaveTrackedAsync][net_savetrackedasync], kunt u updates naar een bestand op het knooppunt (met een interval dat u opgeeft) bijhouden en behouden die updates naar Azure Storage.

In het volgende codefragment gebruiken we [SaveTrackedAsync] [ net_savetrackedasync] om bij te werken `stdout.txt` in Azure Storage elke 15 seconden tijdens het uitvoeren van de taak:

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

De sectie met opmerkingen `Code to process data and produce output file(s)` is een tijdelijke aanduiding voor de code die uw taak normaal kunt uitvoeren. Bijvoorbeeld, mogelijk code die gegevens uit Azure Storage downloadt en transformatie of berekening op deze uitvoert. De belangrijk onderdeel van dit fragment is om aan te tonen hoe u kunt laten lopen deze code in een `using` blok regelmatig bijwerken van een bestand met [SaveTrackedAsync][net_savetrackedasync].

De knooppuntagent is een programma dat wordt uitgevoerd op elk knooppunt in de groep van toepassingen en biedt de opdracht en controle-interface tussen het knooppunt en de Batch-service. De `Task.Delay` aanroep is vereist bij het einde van dit `using` blokkeren om ervoor te zorgen dat de knooppuntagent tijd leegmaken van de inhoud van de standard out naar het bestand stdout.txt op het knooppunt is. Zonder deze vertraging is het mogelijk is de afgelopen paar seconden van uitvoer missen. Deze vertraging is mogelijk niet vereist voor alle bestanden.

> [!NOTE]
> Wanneer u een bestand met inschakelt **SaveTrackedAsync**, alleen *voegt* naar het bestand bijgehouden worden persistent gemaakt met Azure Storage. Gebruik deze methode alleen voor het bijhouden van logboekbestanden niet draaien of andere bestanden die zijn geschreven om toe te voegen met bewerkingen aan het einde van het bestand.
> 
> 

## <a name="retrieve-output-data"></a>Uitvoergegevens ophalen

Bij het ophalen van de persistente uitvoer met behulp van de Azure Batch File Conventions-bibliotheek, kunt u dat doen op een taak - en taak-georiënteerde manier. U kunt de uitvoer aanvragen voor de opgegeven taak of taak zonder te hoeven te kennen van het pad in Azure Storage, of zelfs de naam van het bestand. U kunt in plaats daarvan uitvoerbestanden aanvragen door de taak of taak-ID.

Het volgende codefragment doorloopt de taken van een job, worden enkele gegevens over de uitvoerbestanden van de taak afgedrukt en vervolgens de bestanden van Storage downloadt.

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

## <a name="view-output-files-in-the-azure-portal"></a>De uitvoerbestanden weergeven in Azure portal

De Azure-portal geeft uitvoerbestanden van de taak en logboeken die zijn opgeslagen op een gekoppelde Azure Storage-account met de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). U kunt ook zelf deze conventies implementeren de een taal van uw keuze, of u kunt de File Conventions-bibliotheek in uw .NET-toepassingen.

Om in te schakelen in de weergave van de uitvoerbestanden in de portal, moet u de volgende vereisten voldoen:

1. [Een Azure Storage-account koppelt](#requirement-linked-storage-account) aan uw Batch-account.
2. Voldoen aan de vooraf gedefinieerde naamgevingsconventies voor Storage-containers en uitvoerbare bestanden bij het opslaan van de uitvoer. U kunt de definitie van deze overeenkomsten vinden in de File Conventions-bibliotheek [Leesmij-bestand][github_file_conventions_readme]. Als u de [Azure Batch File Conventions] [ nuget_package] bibliotheek om vast te leggen van de uitvoer van uw bestanden blijven aanwezig op basis van de File Conventions-standaard.

Als u wilt weergeven en logboeken van de uitvoerbestanden taak in Azure portal, gaat u naar de taak waarvan uitvoer die u geïnteresseerd bent in, klik dan ofwel **opgeslagen uitvoerbestanden** of **opgeslagen logboeken**. Deze afbeelding ziet u de **opgeslagen uitvoerbestanden** voor de taak met ID '007':

![Taakuitvoer blade in Azure portal][2]

## <a name="code-sample"></a>Codevoorbeeld

De [PersistOutputs] [ github_persistoutputs] voorbeeldproject is een van de [Azure Batch-codevoorbeelden] [ github_samples] op GitHub. Deze Visual Studio-oplossing laat zien hoe u de Azure Batch File Conventions-bibliotheek om vast te leggen van de taakuitvoer naar duurzame opslag gebruiken. De als voorbeeld wilt uitvoeren, de volgende stappen uit:

1. Open het project in **Visual Studio 2017**.
2. Toevoegen van uw Batch- en Storage **accountreferenties** naar **AccountSettings.settings** in het project Microsoft.Azure.Batch.Samples.Common.
3. **Bouw** (maar niet wordt uitgevoerd) de oplossing. Alle NuGet-pakketten herstellen als u hierom wordt gevraagd.
4. De Azure portal gebruiken voor het uploaden van een [toepassingspakket](batch-application-packages.md) voor **PersistOutputsTask**. Bevatten de `PersistOutputsTask.exe` en afhankelijke assembly's in het ZIP-pakket, stel de toepassings-ID aan 'PersistOutputsTask' en de versie van de toepassing-pakket naar "1.0".
5. **Start** (uitvoeren) de **PersistOutputs** project.
6. Wanneer u wordt gevraagd om te kiezen van de persistentie-technologie gebruiken voor het uitvoeren van het voorbeeld, vul dan **1** om uit te voeren van het voorbeeld met behulp van de File Conventions-bibliotheek om vast te leggen van de uitvoer van de taak. 

## <a name="next-steps"></a>Volgende stappen

### <a name="get-the-batch-file-conventions-library-for-net"></a>De Batch File Conventions-bibliotheek voor .NET ophalen

De Batch File Conventions-bibliotheek voor .NET is beschikbaar op [NuGet][nuget_package]. De bibliotheek breidt de [CloudJob] [ net_cloudjob] en [CloudTask] [ net_cloudtask] klassen met nieuwe methoden. Zie ook de [referentiedocumentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) voor de File Conventions-bibliotheek.

De [broncode] [ github_file_conventions] voor de File Conventions bibliotheek is beschikbaar op GitHub in de Microsoft Azure SDK voor .NET-opslagplaats. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Andere methoden voor het vastleggen van uitvoergegevens verkennen

- Zie [behouden taken kunt uitvoeren naar Azure Storage](batch-task-output.md) voor een overzicht van het vastleggen van gegevens taak en taakplanning.
- Zie [Persist taakgegevens naar Azure Storage met de Batch-service API](batch-task-output-files.md) voor informatie over het gebruik van de Batch-service-API om vast te leggen van de uitvoergegevens.

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

[1]: ./media/batch-task-output/task-output-01.png "Opgeslagen uitvoerbestanden en opgeslagen logboeken selectors in portal"
[2]: ./media/batch-task-output/task-output-02.png "Taakuitvoer blade in Azure portal"

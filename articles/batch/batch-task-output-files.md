---
title: Behouden van de taak en uitvoer naar Azure Storage met de Azure Batch-service-API | Microsoft Docs
description: Informatie over het gebruik van de API van Batch-service om Batch-taak en de taak uitvoer naar Azure Storage.
services: batch
author: tamram
manager: timlt
editor: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.openlocfilehash: 2530b7c20347b9fb58aee4dfe693847cf3911741
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="persist-task-data-to-azure-storage-with-the-batch-service-api"></a>Taak gegevens naar Azure Storage met de API van Batch-service

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

De API van Batch-service ondersteunt vanaf 2017-05-01-versie persistent maken uitvoergegevens naar Azure Storage voor taken en jobbeheertaken die worden uitgevoerd op de groepen met de configuratie van de virtuele machine. Wanneer u een taak toevoegt, kunt u een container in Azure Storage als bestemming voor de uitvoer van de taak. De Batch-service schrijft vervolgens uitvoergegevens op die container, wanneer de taak voltooid is.

Een voordeel voor het gebruik van de API van Batch-service om vast te leggen van de uitvoer van de taak is dat u niet wilt wijzigen van de toepassing die de taak wordt uitgevoerd. In plaats daarvan met enkele eenvoudige wijzigingen aan de clienttoepassing, u kunt deze persistent maken van de taak-uitvoer van de code die wordt gemaakt van de taak.   

## <a name="when-do-i-use-the-batch-service-api-to-persist-task-output"></a>Wanneer kan ik de API van Batch-service gebruiken om vast te leggen van de uitvoer van de taak?

Azure Batch biedt meer dan één manier om vast te leggen van de uitvoer van de taak. Met behulp van de API van Batch-service is een handige benadering die wordt het meest geschikt voor deze scenario's:

- Wilt u code schrijven om te blijven behouden taakuitvoer uit vanuit de clienttoepassing zonder te wijzigen van de toepassing die de taak wordt uitgevoerd.
- Wilt u de uitvoer van de Batch-taken en jobbeheertaken in groepen die zijn gemaakt met de configuratie van de virtuele machine persistent maken.
- Wilt u uitvoer naar een Azure Storage-container met een willekeurige naam behouden.
- U wilt behouden uitvoer naar een Azure Storage-container met de naam volgens de [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

Als uw scenario van die verschilt hierboven zijn vermeld, moet u wellicht een andere benadering overwegen. Bijvoorbeeld: de API van Batch-service biedt geen momenteel ondersteuning streaming uitvoer naar Azure Storage terwijl de taak wordt uitgevoerd. Naar uitvoer stream: overweeg het gebruik van de bibliotheek Batch bestand conventies voor .NET. Voor andere talen moet u om uw eigen oplossing te implementeren. Zie voor meer informatie over andere opties voor het persistent maken van de taakuitvoer [behouden van de taak en uitvoer naar Azure Storage](batch-task-output.md). 

## <a name="create-a-container-in-azure-storage"></a>Een container maken in Azure Storage

Om te blijven behouden taakuitvoer naar Azure Storage, moet u een container die als de bestemming voor de uitvoerbestanden fungeert maken. De container maken voordat u uw taak, bij voorkeur uitvoeren voordat het verzenden van de taak. Gebruik de juiste Azure Storage-clientbibliotheek of de SDK voor het maken van de container. Zie voor meer informatie over Azure Storage-API's, de [documentatie bij Azure Storage](https://docs.microsoft.com/azure/storage/).

Bijvoorbeeld, als u uw toepassing in C# schrijft, gebruikt de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Het volgende voorbeeld ziet u hoe een container maken:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await conainer.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-the-container"></a>Een shared access signature ophalen voor de container

Nadat u de container maken, ophalen van een shared access signature (SAS) die schrijftoegang hebben tot de container. Een SAS biedt gedelegeerde toegang tot de container. De SAS verleent toegang met een opgegeven set machtigingen en via een opgegeven tijdsinterval. De Batch-service moet een SAS met schrijfmachtigingen in voor de uitvoer van de taak schrijven naar de container. Zie voor meer informatie over SAS [gedeelde handtekeningen voor toegang \(SAS\) in Azure Storage](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Als u een SAS met de Azure Storage-API's krijgt, retourneert de API een tekenreeks van SAS-token. Deze tekenreeks token bevat alle parameters van de SA's, met inbegrip van de machtigingen en het interval gedurende welke de SAS. Als u wilt de SAS gebruiken voor toegang tot een container in Azure Storage, moet u de tekenreeks van SAS-token toevoegen aan de bron-URI. De resource-URI, samen met het toegevoegde SAS-token biedt geverifieerde toegang tot Azure Storage.

Het volgende voorbeeld laat zien hoe u een alleen-schrijven SAS-token tekenreeks voor de container ophalen en vervolgens voegt de SAS met de URI van de container:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken; 
```

## <a name="specify-output-files-for-task-output"></a>Uitvoerbestanden voor de uitvoer van de taak opgeven

Als u de uitvoerbestanden voor een taak, maakt u een verzameling van [uitvoerbestand](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) objecten en wijst deze toe aan de [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) eigenschap bij het maken van de taak. 

Het volgende voorbeeld van de .NET-code maakt een taak die u willekeurige getallen naar een bestand met de naam schrijft `output.txt`. Het voorbeeld wordt een bestand voor uitvoer voor `output.txt` naar de container te worden geschreven. In het voorbeeld maakt ook uitvoerbestanden voor alle logboekbestanden die overeenkomen met het bestandspatroon `std*.txt` (_bijvoorbeeld_, `stdout.txt` en `stderr.txt`). De URL van de container moet eerder de SA's dat is gemaakt voor de container. De Batch-service gebruikt de SAS om te verifiëren van toegang tot de container: 

```csharp
new CloudTask(taskId, "cmd /v:ON /c \"echo off && set && (FOR /L %i IN (1,1,100000) DO (ECHO !RANDOM!)) > output.txt\"")
{
    OutputFiles = new List<OutputFile>
    {
        new OutputFile(
            filePattern: @"..\std*.txt",
            destination: new OutputFileDestination(
         new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId)),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
        new OutputFile(
            filePattern: @"output.txt",
            destination: 
         new OutputFileDestination(new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId + @"\output.txt")),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
}
```

### <a name="specify-a-file-pattern-for-matching"></a>Geef een patroon voor overeenkomende

Wanneer u een uitvoerbestand opgeeft, kunt u de [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) om op te geven van een patroon voor het afstemmen van de eigenschap. Het bestandspatroon mogelijk overeen met nul bestanden, één bestand of een set bestanden die zijn gemaakt door de taak.

De **FilePattern** eigenschap ondersteunt standaard bestandssysteem jokertekens zoals `*` (voor niet-recursieve komt overeen met) en `**` (voor recursieve komt overeen met). De bovenstaande voorbeeldcode geeft bijvoorbeeld het op hetzelfde bestandspatroon `std*.txt` niet-recursief: 

`filePattern: @"..\std*.txt"`

Geef een patroon met geen jokertekens om één bestand te uploaden. De bovenstaande voorbeeldcode geeft bijvoorbeeld het op hetzelfde bestandspatroon `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Geef een voorwaarde uploaden

De [OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) eigenschap toestaat uitvoerbestanden voorwaardelijke worden geüpload. Een gebruikelijk scenario is een set bestanden als de taak is voltooid en een andere set bestanden uploaden als deze uitvalt. U wilt bijvoorbeeld uitgebreide logboekbestanden uploaden wanneer de taak mislukt en wordt afgesloten met de afsluitcode van een andere waarde dan nul. U kunt op deze manier voor het uploaden van bestanden met resultaten alleen als de taak is voltooid, omdat deze bestanden mogelijk ontbreken of zijn onvolledig als de taak mislukt.

Het bovenstaande sets codevoorbeeld de **UploadCondition** eigenschap **TaskCompletion**. Deze instelling geeft aan dat het bestand te uploaden nadat de taken is voltooid, ongeacht de waarde van de afsluitcode. 

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Zie voor andere instellingen, de [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) enum.

### <a name="disambiguate-files-with-the-same-name"></a>Bestanden met dezelfde naam heffen

De taken in een taak kunnen leiden tot bestanden die dezelfde naam hebben. Bijvoorbeeld: `stdout.txt` en `stderr.txt` worden gemaakt voor elke taak die in een taak wordt uitgevoerd. Omdat elke taak wordt uitgevoerd in een eigen context, zijn deze bestanden niet in conflict op het bestandssysteem van het knooppunt. Echter tijdens het uploaden van bestanden van meerdere taken naar een gedeelde container, moet u heffen bestanden met dezelfde naam.

De [OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) eigenschap geeft u de bestemmings-blob of virtuele map voor uitvoerbestanden. U kunt de **pad** eigenschap als naam voor de virtuele map zodanig dat uitvoerbestanden met dezelfde naam een unieke naam op basis van Azure Storage of blob. Met behulp van de taak-ID in het pad is een prima manier om te controleren van unieke namen en eenvoudig bestanden te identificeren.

Als de **FilePattern** eigenschap is ingesteld op een expressie met jokertekens en vervolgens alle bestanden die overeenkomen met het patroon worden geüpload naar de virtuele map die is opgegeven door de **pad** eigenschap. Bijvoorbeeld, als de container is `mycontainer`, de taak-ID is `mytask`, en het bestandspatroon `..\std*.txt`, wordt de absolute URI's naar de uitvoerbestanden in Azure Storage vergelijkbaar met:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Als de **FilePattern** eigenschap is ingesteld op overeenkomen met de naam van een enkel bestand, wat betekent dat het bevat geen geen jokertekens, wordt de waarde van de **pad** eigenschap geeft u de volledig gekwalificeerde blob-naam. Als u naamconflicten met één bestand uit meerdere taken verwacht, moet u vervolgens de naam van de virtuele map als onderdeel van de bestandsnaam om op te heffen die bestanden bevatten. Bijvoorbeeld, stelt de **pad** op te nemen van de taak-ID, het scheidingsteken (meestal een slash) en de naam eigenschap:

`path: taskId + @"/output.txt"`

De absolute URI's naar de uitvoerbestanden voor een aantal taken zijn vergelijkbaar met:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Zie voor meer informatie over virtuele mappen in Azure Storage [lijst van de blobs in een container](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container).


## <a name="diagnose-file-upload-errors"></a>Bestand uploadfouten opsporen

Als uitvoerbestanden uploaden naar Azure Storage is mislukt, wordt de taak wordt verplaatst naar de **voltooid** status en de [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) eigenschap is ingesteld. Bekijk de **FailureInformation** eigenschap om te bepalen welke fout is opgetreden. Dit is bijvoorbeeld een fout die bij het uploaden van het bestand optreedt als de container kan niet worden gevonden: 

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of the specified Azure container(s) was not found while attempting to upload an output file
```

Op elke uploaden bestand Batch schrijft twee logboekbestanden naar het rekenknooppunt `fileuploadout.txt` en `fileuploaderr.txt`. U kunt deze logboekbestanden voor meer informatie over een specifieke fout onderzoeken. In gevallen waarin het bestand te uploaden is nooit uitgevoerd, bijvoorbeeld omdat de taak zelf kan niet worden uitgevoerd, klikt u vervolgens bestaat deze logboekbestanden niet.

## <a name="diagnose-file-upload-performance"></a>Bestand uploaden van prestaties vaststellen

De `fileuploadout.txt` bestand uploadvoortgang registreert. U kunt dit bestand voor meer informatie over hoe lang duurt het bestandsuploads onderzoeken. Houd er rekening mee dat er veel factoren voor het uploaden van prestaties, met inbegrip van de grootte van het knooppunt, andere activiteiten op het knooppunt op het moment van het uploaden of de doelcontainer bevindt zich in dezelfde regio bevinden als de Batch-pool, hoeveel knooppunten uploadt naar de stora ge-account op het moment, enzovoort.

## <a name="use-the-batch-service-api-with-the-batch-file-conventions-standard"></a>Gebruik de API van Batch-service met de Batch-bestand Conventions-standaard

Wanneer u de uitvoer van de taak met de API van Batch-service zich blijven voordoen, kunt u uw doelcontainer kunt naam en gewenste echter blobs. U kunt ook deze volgens de naam de [Batch bestand conventies standaard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). De bestanden standaard bepaalt de namen van de doelcontainer en blob in Azure Storage voor een bepaald op basis van de namen van de taak en -uitvoerbestand. Als u de bestanden standaard gebruikt voor het benoemen van uitvoerbestanden, wordt de uitvoerbestanden zijn beschikbaar voor weergave in de [Azure-portal](https://portal.azure.com).

Als u in C# ontwikkelt, kunt u de methoden die zijn ingebouwd in de [bestand conventies van Batch-bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). Deze bibliotheek de goed benoemde containers en blobs paden voor u gemaakt. U kunt bijvoorbeeld de API om op te halen van de juiste naam voor de container, op basis van de taaknaam aanroepen:

```csharp
string containerName = job.OutputStorageContainerName();
```

U kunt de [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) methode om te retourneren van een shared access signature (SAS)-URL die wordt gebruikt om te schrijven naar de container. U kunt deze SAS naar geeft de [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) constructor.

Als u in een andere taal dan C# ontwikkelt, moet u voor het implementeren van de bestanden standaard.

## <a name="code-sample"></a>Codevoorbeeld

De [PersistOutputs] [ github_persistoutputs] voorbeeldproject is een van de [Azure Batch-codevoorbeelden] [ github_samples] op GitHub. Deze Visual Studio-oplossing laat zien hoe u de Batch-clientbibliotheek voor .NET gebruikt om vast te leggen van de uitvoer van de taak naar duurzame opslag. Als u wilt uitvoeren in het voorbeeld, de volgende stappen uit:

1. Open het project in **Visual Studio 2015 of hoger**.
2. Toevoegen van uw Batch- en Storage **accountreferenties** naar **AccountSettings.settings** in het project Microsoft.Azure.Batch.Samples.Common.
3. **Bouwen** (maar niet worden uitgevoerd) de oplossing. Herstel alle NuGet-pakketten als daarom wordt gevraagd.
4. De Azure portal gebruiken voor het uploaden van een [toepassingspakket](batch-application-packages.md) voor **PersistOutputsTask**. Bevatten de `PersistOutputsTask.exe` en afhankelijke assembly's in het ZIP-pakket, de toepassings-ID aan 'PersistOutputsTask' en de versie van het toepassingspakket naar "1.0".
5. **Start** (uitvoeren) de **PersistOutputs** project.
6. Als u wordt gevraagd om te kiezen de persistentie-technologie gebruiken voor het uitvoeren van de steekproef, geeft u **2** om uit te voeren van het voorbeeld met de API van Batch-service om vast te leggen van de uitvoer van de taak.
7. Indien gewenst, het voorbeeld opnieuw uitvoeren, voeren **3** om vast te leggen van uitvoer met de API van Batch-service en naam van het doelpad voor blob-container en op basis van de bestanden standaard.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over persistent maken uitvoer van de taak met de bestand Conventions-bibliotheek voor .NET [taak en gegevens naar Azure Storage met de Batch-bestand Conventions-bibliotheek voor .NET om vast te leggen ](batch-task-output-file-conventions.md).
- Zie voor informatie over andere benaderingen voor persistent maken uitvoergegevens in Azure Batch, [behouden van de taak en uitvoer naar Azure Storage](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples

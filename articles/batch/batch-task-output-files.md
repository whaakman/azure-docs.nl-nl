---
title: Taak en uitvoer behouden met Azure Storage met de Batch-service-API - Azure Batch | Microsoft Docs
description: Informatie over het gebruik van Batch-service-API om vast te leggen van de Batch-taak en de taak uitvoer naar Azure Storage.
services: batch
author: laurenhughes
manager: jeconnoc
editor: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 11/14/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 685f034dfa3b312c461f7313c0f85a256838a8d2
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539548"
---
# <a name="persist-task-data-to-azure-storage-with-the-batch-service-api"></a>Taakgegevens naar Azure Storage met de Batch-service-API behouden

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

De Batch-service-API biedt ondersteuning voor permanente uitvoergegevens naar Azure Storage voor taken en taken van een job manager die worden uitgevoerd voor groepen met de configuratie van de virtuele machine. Wanneer u een taak toevoegt, kunt u een container in Azure Storage opgeven als bestemming voor uitvoer van de taak. De Batch-service schrijft vervolgens uitvoergegevens naar die container wanneer de taak voltooid is.

Een voordeel voor het gebruik van de Batch-service-API om vast te leggen van de uitvoer van de taak is dat u niet wilt wijzigen van de toepassing die de taak wordt uitgevoerd. U kunt in plaats daarvan de uitvoer van de taak van binnen de dezelfde code die wordt gemaakt van de taak behouden met enkele wijzigingen in uw clienttoepassing.

## <a name="when-do-i-use-the-batch-service-api-to-persist-task-output"></a>Wanneer gebruik ik de API voor Batch-service om vast te leggen van de uitvoer van de taak?

Azure Batch biedt meer dan één manier om vast te leggen van de uitvoer van de taak. Met behulp van de Batch-service-API is een handige benadering die het beste geschikt voor deze scenario's:

- Wilt u code schrijven voor de taakuitvoer van binnen uw clienttoepassing behouden zonder te wijzigen van de toepassing die de taak wordt uitgevoerd.
- Wilt u de uitvoer van de Batch-taken en jobbeheertaken in pools die zijn gemaakt met de configuratie van de virtuele machine behouden.
- Wilt u de uitvoer naar een Azure Storage-container met een willekeurige naam behouden.
- U wilt behouden van de uitvoer naar een Azure Storage-container met de naam volgens de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> De API voor Batch-service biedt geen ondersteuning voor vastleggen van gegevens van de taken die worden uitgevoerd in pools die zijn gemaakt met de configuratie van de cloud-service. Zie voor meer informatie over permanente taak uitvoer van groepen met de cloud services-configuratie [taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET om vast te leggen persistent ](batch-task-output-file-conventions.md).

Als uw scenario van die verschilt hierboven zijn vermeld, moet u mogelijk rekening houden met een andere benadering. Bijvoorbeeld, ondersteunt de API voor Batch-service momenteel geen streaming uitvoer naar Azure Storage terwijl de taak wordt uitgevoerd. Om te streamen uitvoer, overweeg het gebruik van de Batch File Conventions-bibliotheek, beschikbaar voor .NET. Voor andere talen moet u om uw eigen oplossing te implementeren. Zie voor meer informatie over andere opties voor permanente taakuitvoer [behouden taken kunt uitvoeren naar Azure Storage](batch-task-output.md).

## <a name="create-a-container-in-azure-storage"></a>Maak een container in Azure Storage

Als u wilt zich blijven voordoen taakuitvoer naar Azure Storage, moet u een container die als de bestemming voor de uitvoerbestanden fungeert maken. De container maken voordat u uw taak, bij voorkeur uitgevoerd voordat u uw taak indienen. Gebruik de juiste Azure Storage-clientbibliotheek of -SDK voor het maken van de container. Zie voor meer informatie over Azure Storage-API's, de [documentatie voor Azure Storage](https://docs.microsoft.com/azure/storage/).

Bijvoorbeeld: als u uw toepassing in C#, gebruiken de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Het volgende voorbeeld laat zien hoe om een container te maken:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await conainer.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-the-container"></a>Een shared access signature voor de container ophalen

Nadat u de container maken, krijgt u een shared access signature (SAS) met toegang voor schrijven naar de container. Een SAS biedt gedelegeerde toegang tot de container. De SAS verleent toegang met een opgegeven set machtigingen en gedurende een opgegeven tijdsinterval. De Batch-service moet een SAS met schrijfmachtigingen voor de taakuitvoer schrijven naar de container. Zie voor meer informatie over SAS [voor gedeelde toegangshandtekeningen \(SAS\) in Azure Storage](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Wanneer u een SAS met de Azure Storage-API's, retourneert de API een tekenreeks van SAS-token. Dit token tekenreeks bevat alle parameters van de SAS, met inbegrip van de machtigingen en het interval op waarover de SAS geldig is. Voor het gebruik van de SAS voor toegang tot een container in Azure Storage, moet u de tekenreeks van SAS-token toevoegen aan de bron-URI. De resource-URI, samen met de toegevoegde SAS-token, biedt geverifieerde toegang tot Azure Storage.

Het volgende voorbeeld laat zien hoe u een alleen-schrijven SAS-token tekenreeks voor de container ophalen en vervolgens wordt de SAS toegevoegd aan de URI van de container:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken;
```

## <a name="specify-output-files-for-task-output"></a>Uitvoerbestanden voor uitvoer van de taak opgeven

Als u de uitvoerbestanden van een taak, maakt u een verzameling van [uitvoerbestand](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) objecten en wijs deze toe aan de [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) eigenschap bij het maken van de taak.

De volgende C# codevoorbeeld maakt u een taak die willekeurige getallen naar een bestand met de naam schrijft `output.txt`. Het voorbeeld wordt een bestand voor uitvoer voor `output.txt` worden geschreven naar de container. Het voorbeeld maakt ook uitvoerbestanden voor alle logboekbestanden die overeenkomen met het bestandspatroon `std*.txt` (_bijvoorbeeld_, `stdout.txt` en `stderr.txt`). De URL van de container moet eerder de SAS die is gemaakt voor de container. De Batch-service gebruikt de SAS voor het verifiëren van toegang tot de container:

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

### <a name="specify-a-file-pattern-for-matching"></a>Geef een bestandspatroon om te vergelijken met

Wanneer u een uitvoerbestand opgeeft, kunt u de [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) om op te geven van een bestandspatroon voor het afstemmen van de eigenschap. Het bestandspatroon mogelijk overeen met nul bestanden, een enkel bestand of een set bestanden die zijn gemaakt door de taak.

De **FilePattern** eigenschap ondersteunt standaard bestandssysteem jokertekens zoals `*` (voor niet-recursieve komt overeen met) en `**` (voor recursieve komt overeen met). Het bovenstaande codevoorbeeld geeft bijvoorbeeld het bestandspatroon dat moet overeenkomen met `std*.txt` niet-recursief:

`filePattern: @"..\std*.txt"`

Als u wilt uploaden een enkel bestand, geeft u een bestandspatroon met geen jokertekens. Het bovenstaande codevoorbeeld geeft bijvoorbeeld het bestandspatroon dat moet overeenkomen met `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Een voorwaarde voor uploaden opgeven

De [OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) eigenschap toestaat uitvoerbestanden voorwaardelijke worden geüpload. Een veelvoorkomend scenario is een set bestanden als de taak is voltooid en een andere set bestanden uploaden als deze is mislukt. Bijvoorbeeld, kunt u met het uploaden van bestanden van het uitgebreide logboek wanneer de taak mislukt en wordt afgesloten met een andere afsluitcode. Op deze manier kunt u voor het uploaden van bestanden met resultaten alleen als de taak is voltooid, omdat deze bestanden mogelijk ontbreken of zijn onvolledig als de taak mislukt.

In het voorbeeld hierboven stelt de **UploadCondition** eigenschap **TaskCompletion**. Deze instelling geeft aan dat het bestand moet worden geüpload nadat de taken is voltooid, ongeacht de waarde van de afsluitcode.

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Zie voor andere instellingen, de [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) enum.

### <a name="disambiguate-files-with-the-same-name"></a>Bestanden met dezelfde naam dubbelzinnigheid

De taken in een taak kunnen leiden tot bestanden die dezelfde naam hebben. Bijvoorbeeld, `stdout.txt` en `stderr.txt` worden gemaakt voor elke taak die in een taak wordt uitgevoerd. Omdat elke taak wordt uitgevoerd in een eigen context, worden deze bestanden geen conflict op het bestandssysteem van het knooppunt. Echter tijdens het uploaden van bestanden van meerdere taken naar een gedeelde container, moet u dubbelzinnigheid van bestanden met dezelfde naam.

De [OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) eigenschap geeft u de bestemmings-blob of virtuele map voor uitvoerbestanden. U kunt de **pad** eigenschap om de naam van de blob of virtuele map zodanig dat uitvoerbestanden met dezelfde naam een unieke naam op basis van Azure Storage. Met behulp van de taak-ID in het pad is een goede manier om te controleren van unieke namen en bestanden eenvoudig kunt identificeren.

Als de **FilePattern** eigenschap is ingesteld op een expressie met jokertekens, en vervolgens alle bestanden die overeenkomen met het patroon worden geüpload naar de virtuele map die is opgegeven door de **pad** eigenschap. Bijvoorbeeld, als de container is `mycontainer`, de taak-ID is `mytask`, en is het bestandspatroon `..\std*.txt`, en vervolgens de absolute URI's voor de uitvoerbestanden in Azure Storage vergelijkbaar met is:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Als de **FilePattern** eigenschap is ingesteld op overeenkomen met de naam van een enkel bestand, wat betekent dat deze geen jokertekens, wordt de waarde van de **pad** eigenschap geeft u de volledig gekwalificeerde blobnaam. Als u verwacht naamconflicten met één bestand uit meerdere taken dat, neemt u de naam van de virtuele map als onderdeel van de naam van het bestand te onderscheiden van die bestanden. Bijvoorbeeld, stel de **pad** eigenschap om op te nemen van de taak-ID, het scheidingsteken (meestal een slash) en de bestandsnaam:

`path: taskId + @"/output.txt"`

De absolute URI's voor de uitvoerbestanden voor een set taken zijn vergelijkbaar met:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Zie voor meer informatie over virtuele mappen in Azure Storage, [de blobs in een container te vermelden](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container).

## <a name="diagnose-file-upload-errors"></a>Bestand uploadfouten vaststellen

Als het uploaden van uitvoerbestanden naar Azure Storage mislukt, wordt de taak wordt verplaatst naar de **voltooid** status en de [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) eigenschap is ingesteld. Bekijk de **FailureInformation** eigenschap om te bepalen welke fout is opgetreden. Bijvoorbeeld, is hier een fout die wordt uitgevoerd op het uploaden van het bestand als de container kan niet worden gevonden:

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of the specified Azure container(s) was not found while attempting to upload an output file
```

Op elke bestandsupload Batch schrijft twee logboekbestanden naar het rekenknooppunt `fileuploadout.txt` en `fileuploaderr.txt`. U kunt deze logboekbestanden voor meer informatie over een specifieke fout onderzoeken. In gevallen waar het bestand uploadt is nooit uitgevoerd, bijvoorbeeld omdat de taak zelf kan niet worden uitgevoerd, klikt u vervolgens bestaat deze logboekbestanden niet.

## <a name="diagnose-file-upload-performance"></a>Bestand uploaden van prestaties vaststellen

De `fileuploadout.txt` bestand logboeken uploaden wordt uitgevoerd. U kunt dit bestand voor meer informatie over hoe lang de bestandsuploads zijn te onderzoeken. Houd er rekening mee dat er veel factoren prestaties, met inbegrip van de grootte van het knooppunt, andere activiteiten op het knooppunt op het moment van het uploaden, ongeacht of de doelcontainer in dezelfde regio als de Batch-pool, hoeveel knooppunten uploadt naar de stora uploaden ge-account op hetzelfde moment en enzovoort.

## <a name="use-the-batch-service-api-with-the-batch-file-conventions-standard"></a>De Batch-service-API gebruiken met de Batch File Conventions-standaard

Wanneer u de uitvoer van de taak met de API voor Batch-service zich blijven voordoen, u kunt een naam geven de doelcontainer en gewenste echter blobs. U kunt er ook voor kiezen om de naam van deze volgens de [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions). De standaard File Conventions bepaalt de namen van de doelcontainer en blob in Azure Storage voor een bepaalde uitvoerbestand op basis van de namen van de taak en de taak. Als u de standaard File Conventions gebruikt voor het benoemen van uitvoerbestanden, wordt de uitvoerbestanden zijn beschikbaar voor weergave in de [Azure-portal](https://portal.azure.com).

Als u in C# ontwikkelt, kunt u de methoden die zijn ingebouwd in de [Batch File Conventions-bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). Deze bibliotheek goed benoemde containers en blob-paden voor u gemaakt. Bijvoorbeeld, kunt u de API om op te halen van de juiste naam voor de container, op basis van de taaknaam aanroept:

```csharp
string containerName = job.OutputStorageContainerName();
```

U kunt de [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) methode om te retourneren van een shared access signature (SAS)-URL die wordt gebruikt om te schrijven naar de container. U kunt deze SAS geeft de [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) constructor.

Als u in een andere taal dan C# ontwikkelt, moet u voor het implementeren van de File Conventions-standaard.

## <a name="code-sample"></a>Codevoorbeeld

De [PersistOutputs] [ github_persistoutputs] voorbeeldproject is een van de [Azure Batch-codevoorbeelden] [ github_samples] op GitHub. Deze Visual Studio-oplossing laat zien hoe u kunt de Batch-clientbibliotheek voor .NET gebruiken om vast te leggen van de taakuitvoer naar duurzame opslag. De als voorbeeld wilt uitvoeren, de volgende stappen uit:

1. Open het project in **Visual Studio 2017**.
2. Toevoegen van uw Batch- en Storage **accountreferenties** naar **AccountSettings.settings** in het project Microsoft.Azure.Batch.Samples.Common.
3. **Bouw** (maar niet wordt uitgevoerd) de oplossing. Alle NuGet-pakketten herstellen als u hierom wordt gevraagd.
4. De Azure portal gebruiken voor het uploaden van een [toepassingspakket](batch-application-packages.md) voor **PersistOutputsTask**. Bevatten de `PersistOutputsTask.exe` en afhankelijke assembly's in het ZIP-pakket, stel de toepassings-ID aan 'PersistOutputsTask' en de versie van de toepassing-pakket naar "1.0".
5. **Start** (uitvoeren) de **PersistOutputs** project.
6. Wanneer u wordt gevraagd om te kiezen van de persistentie-technologie gebruiken voor het uitvoeren van het voorbeeld, vul dan **2** om uit te voeren van het voorbeeld met behulp van de Batch-service-API om vast te leggen van de uitvoer van de taak.
7. Indien gewenst, het voorbeeld opnieuw uitvoeren, voeren **3** om vast te leggen van de uitvoer met de Batch-service-API, en ook om de naam van het doelpad voor container- en blobnaam op basis van de File Conventions-standaard.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over permanente uitvoer van de taak met de File Conventions-bibliotheek voor .NET, [behoud van de taak en gegevens naar Azure Storage met de Batch File Conventions-bibliotheek voor .NET](batch-task-output-file-conventions.md).
- Zie voor informatie over andere manieren voor permanente uitvoergegevens in de Azure Batch, [behouden taken kunt uitvoeren naar Azure Storage](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples

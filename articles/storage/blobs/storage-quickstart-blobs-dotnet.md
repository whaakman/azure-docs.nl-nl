---
title: Azure-snelstart - Blobs uploaden, downloaden en vermelden in Azure Storage met behulp van .NET | Microsoft Docs
description: In deze snelstart maakt u een opslagaccount en een container. Vervolgens gebruikt u de opslagclientbibliotheek voor .NET om een blob in Azure Storage te uploaden, een blob te downloaden en de blobs in een container te vermelden.
services: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 03/01/2018
ms.author: tamram
ms.openlocfilehash: 8d1f09a39e865500aa8e4d093473d4989f134c3d
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-net"></a>Snelstart: blobs downloaden, uploaden en vermelden met behulp van .NET

In deze snelstart leert u hoe u de .NET-clientbibliotheek voor Azure Storage gebruikt om blok-bobs in een container te uploaden, te downloaden en weer te geven.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Maak om deze snelstart te volgen eerst een Azure opslagaccount in [Azure Portal](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM). Zie voor meer informatie over het maken van het account [Een opslagaccount maken](../common/storage-quickstart-create-account.md).

Download en installeer vervolgens .NET Core 2.0 voor uw besturingssysteem. U kunt ook een editor installeren die u wilt gebruiken bij uw besturingssysteem.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

- Installeer [.NET Core voor Windows](https://www.microsoft.com/net/download/windows/build) 
- Installeer desgewenst [Visual Studio voor Windows](https://www.visualstudio.com/) 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

- Installeer [.NET Core voor Linux](https://www.microsoft.com/net/download/linux/build)
- Installeer desgewenst [Visual Studio Code](https://www.visualstudio.com/) en de [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp&dotnetid=963890049.1518206068)

# <a name="macostabmacos"></a>[MacOS](#tab/macos)

- Installeer [.NET Core voor macOS](https://www.microsoft.com/net/download/macos/build).
- Installeer desgewenst [Visual Studio voor Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

---

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De voorbeeldtoepassing die in deze snelstart wordt gebruikt, is een basisconsoletoepassing. U kunt de voorbeeldtoepassing verkennen op [GitHub](https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart).

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Zoek voor het openen van de Visual Studio-oplossing naar de map storage-blobs-dotnet-quickstart, open deze en dubbelklik op storage-blobs-dotnet-quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

U moet de verbindingsreeks voor uw opslagaccount opgeven om de toepassing uit te voeren. U kunt deze verbindingsreeks opslaan in een omgevingsvariabele op de lokale computer waarop de toepassing wordt uitgevoerd. U kunt de omgevingsvariabele maken met behulp van een van de volgende opdrachten, afhankelijk van uw besturingssysteem. Vervang `<yourconnectionstring>` door de werkelijke verbindingsreeks.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export storageconnectionstring=<yourconnectionstring>
```

# <a name="macostabmacos"></a>[MacOS](#tab/macos)

Bewerk uw .bash_profile en voeg de omgevingsvariabele toe:

```
export STORAGE_CONNECTION_STRING=
```

Nadat u de omgevingsvariabele toevoegt, meldt u zich af en weer aan om de wijzigingen door te voeren. U kunt ook in uw "source .bash_profile" typen in uw terminal.

---

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met dit voorbeeld wordt een testbestand gemaakt in uw lokale map **Mijn documenten** en geüpload naar de Blob-opslag. Vervolgens wordt een lijst gemaakt van de blobs in de container en wordt het bestand gedownload met een nieuwe naam, zodat u het oude en nieuwe bestand kunt vergelijken. 

Navigeer naar de toepassingsmap en voer de toepassing uit met de opdracht `dotnet run`.

```
dotnet run
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
Azure Blob storage quick start sample
Temp file = /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Uploading to Blob storage as blob 'QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt'
List blobs in container.
https://mystorageaccount.blob.core.windows.net/quickstartblobs/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374.txt
Downloading blob to /home/admin/QuickStart_b73f2550-bf20-4b3b-92ec-b9b31c56b374_DOWNLOADED.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

Wanneer u op **Enter** drukt, worden de opslagcontainer en de bestanden verwijderd. Controleer voordat u doorgaat of de map **Mijn documenten** de twee bestanden bevat. Als u ze opent, ziet u dat ze identiek zijn. Kopieer de URL van de blob in het consolevenster en plak deze in een browser om de inhoud van de blob weer te geven.

Nadat u de bestanden hebt gecontroleerd, drukt u op een willekeurige toets om de demo te voltooien en de testbestanden te verwijderen. Nu u weet wat het voorbeeld doet, opent u het bestand Program.cs om de code te bekijken. 

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

We gaan nu de voorbeeldcode verkennen, zodat u begrijpt hoe deze werkt.

### <a name="try-parsing-the-connection-string"></a>Probeer de verbindingsreeks te parseren

Het eerste wat het voorbeeld doet, is controleren of de omgevingsvariabele een verbindingsreeks bevat die kan worden geparseerd om een [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount)-object te maken dat naar het opslagaccount wijst. Gebruik de [TryParse](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.tryparse)-methode om te controleren of de verbindingsreeks geldig is. Als **TryParse** lukt, wordt de variabele *storageAccount* geïnitialiseerd en **true** geretourneerd.

```csharp
// Retrieve the connection string for use with the application. The storage connection string is stored
// in an environment variable on the machine running the application called storageconnectionstring.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring", EnvironmentVariableTarget.User);

// Check whether the connection string can be parsed.
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // If the connection string is valid, proceed with operations against Blob storage here.
    ...
}
else
{
    // Otherwise, let the user know that they need to define the environment variable.
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add a environment variable named 'storageconnectionstring' with your storage " +
        "connection string as a value.");
    Console.WriteLine("Press any key to exit the sample application.");
    Console.ReadLine();
}
```

### <a name="create-the-container-and-set-permissions"></a>De container maken en machtigingen instellen

Vervolgens wordt een container gemaakt, en de machtigingen ervan worden zo ingesteld dat alle blobs in de container openbaar zijn. Als een blob openbaar is, is deze anoniem toegankelijk voor elke client.

Maak om de container te maken eerst een instantie van het object [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient), dat naar de Blob-opslag in uw opslagaccount wijst. Maak vervolgens een instantie van het object [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer) en maak de container. 

In dit geval roept het voorbeeld de methode [CreateAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createasync) aan om de container te maken. Er wordt een GUID-waarde toegevoegd aan de containernaam om ervoor te zorgen dat deze uniek is. In een productieomgeving is het vaak beter de methode [CreateIfNotExistsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexistsasync) te gebruiken om alleen een container te maken als deze nog niet bestaat en naamconflicten voorkomen.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [Containers, blobs en metagegevens een naam geven en hiernaar verwijderen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) voor meer informatie over de naamgeving van containers en blobs.


```csharp
// Create the CloudBlobClient that represents the Blob storage endpoint for the storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs' and append a GUID value to it to make the name unique. 
cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Vervolgens wordt een lokaal bestand geüpload naar een blok-blob. Het codevoorbeeld haalt een verwijzing naar een **CloudBlockBlob**-object op door de [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference)-methode aan te roepen op de container die in de vorige sectie is gemaakt. Vervolgens wordt het geselecteerde bestand naar de blob geüpload door de [UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync)-methode aan te roepen. Met deze methode wordt de blob gemaakt als deze nog niet bestaat, of overschreven als dat wel het geval is. 

```csharp
// Create a file in your local MyDocuments folder to upload to a blob.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

Console.WriteLine("Temp file = {0}", sourceFile);
Console.WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the blob address, then upload the file to the blob.
// Use the value of localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);
```

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Het voorbeeld maakt een lijst de blobs in de container met behulp van de [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync)-methode. In het geval van het voorbeeld is slechts één blob aan de container toegevoegd, zodat de lijstbewerking alleen die ene blob retourneert.

Als er te veel blobs zijn om met één aanroep te retourneren (standaard meer dan 5000), retourneert de **ListBlobsSegmentedAsync**-methode een segment van de totale resultatenset en een vervolgtoken. Om het volgende segment met blobs op te halen, geeft u het vervolgtoken op dat door de vorige aanroep is geretourneerd, enzovoort, totdat het vervolgtoken null is. Een null-vervolgtoken geeft aan dat alle blobs zijn opgehaald. De voorbeeldcode laat aanbevolen procedures zien voor het op de beste manier gebruiken van het vervolgtoken.

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    // Get the value of the continuation token returned by the listing call.
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
    blobContinuationToken = results.ContinuationToken;
} while (blobContinuationToken != null); // Loop while the continuation token is not null. 

```

### <a name="download-blobs"></a>Blobs downloaden

Vervolgens downloadt het voorbeeld de eerder gemaakte blob naar uw lokale bestandssysteem met behulp van de [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync)-methode. De voorbeeldcode voegt het achtervoegsel "_DOWNLOADED" toe aan de naam van de blob, zodat u beide bestanden in het lokale bestandssysteem kunt zien.

```csharp
// Download the blob to a local file, using the reference created earlier. 
// Append the string "_DOWNLOADED" before the .txt extension so that you can see both files in MyDocuments.
destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);  
```

### <a name="clean-up-resources"></a>Resources opschonen

Het voorbeeld ruimt de gemaakte resources op door de hele container te verwijderen met behulp van[CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). U kunt ook de lokale bestanden verwijderen als u dat wilt.

```csharp
Console.WriteLine("Press the 'Enter' key to delete the sample files, example container, and exit the application.");
Console.ReadLine();
// Clean up resources. This includes the container and the two temp files.
Console.WriteLine("Deleting the container");
if (cloudBlobContainer != null)
{
    await cloudBlobContainer.DeleteIfExistsAsync();
}
Console.WriteLine("Deleting the source, and downloaded files");
File.Delete(sourceFile);
File.Delete(destinationFile);
```

## <a name="resources-for-developing-net-applications-with-blobs"></a>Resources voor het ontwikkelen van .NET-toepassingen met blobs

Zie de volgende aanvullende bronnen voor .NET-ontwikkeling met Blob-opslag:

### <a name="binaries-and-source-code"></a>Binaire bestanden en broncode

- Download het NuGet-pakket voor de nieuwste versie van de [Storage .NET-clientbibliotheek](https://www.nuget.org/packages/WindowsAzure.Storage/). 
- Bekijk de [broncode van de Storage .NET-clientbibliotheek](https://github.com/Azure/azure-storage-net) op GitHub.

### <a name="client-library-reference-and-samples"></a>Naslaginformatie en voorbeelden voor de .NET-clientbibliotheek

- Zie het [Storage .NET API-referentiemateriaal](https://docs.microsoft.com/dotnet/api/overview/azure/storage) voor meer informatie over de clientbibliotheek.
- Verken met behulp van de Storage .NET-clientbibliotheek geschreven [Blob-opslagvoorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob).

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u blobs kunt uploaden, downloaden en er een lijst van maken met behulp van .NET. 

Ga door naar [Afbeeldingsgegevens uploaden in de cloud met Azure Storage](storage-upload-process-images.md) om te leren hoe u een web-app kunt maken waarmee u een afbeelding kunt uploaden naar Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](storage-dotnet-how-to-use-blobs.md)

- Zie voor meer informatie over .NET Core [Aan de slag met .NET in 10 minuten](https://www.microsoft.com/net/learn/get-started/).
- Zie [.NET Photo Gallery Web Application Sample with Azure Blob Storage](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/) voor meer informatie over een voorbeeld van een toepassing die u kunt implementeren vanuit Visual Studio voor Windows.
 
---
title: 'Quickstart: .NET gebruiken om te een blob maken in objectopslag - Azure Storage'
description: In deze snelstart leert u hoe u de Azure Storage-clientbibliotheek voor .NET kunt gebruiken om een container te maken en een blob-in-blob-opslag (object). Hierna leert u hoe u de blob naar uw lokale computer downloadt en hoe u alle blobs in een container kunt weergeven.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: tamram
ms.openlocfilehash: 50bb13ecaa9e6076f00749d54b492a1e6663a93e
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287108"
---
# <a name="quickstart-use-net-to-create-a-blob-in-object-storage"></a>Quickstart: .NET gebruiken om te een blob maken in objectopslag

In deze snelstart leert u hoe u de Azure Storage-clientbibliotheek voor .NET kunt gebruiken om een container te maken en een blob-in-blob-opslag (object). Hierna leert u hoe u de blob naar uw lokale computer downloadt en hoe u alle blobs in een container kunt weergeven.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Download en installeer vervolgens .NET Core 2.0 voor uw besturingssysteem. Als u Windows uitvoert, kunt u desgewenst Visual Studio installeren en .NET Framework gebruiken. U kunt ook een editor installeren die u wilt gebruiken bij uw besturingssysteem.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

- Installeer [.NET Core voor Windows](https://www.microsoft.com/net/download/windows) of [.NET Framework](https://www.microsoft.com/net/download/windows) (opgenomen in Visual Studio voor Windows)
- Installeer [Visual Studio voor Windows](https://www.visualstudio.com/). Als u van .NET Core gebruikmaakt, is het installeren van Visual Studio optioneel.  

Zie [Choose between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) (Kiezen tussen .NET Core en .NET Framework voor server-apps) voor informatie over de keuze tussen .NET Core en .NET Framework.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

- Installeer [.NET Core voor Linux](https://www.microsoft.com/net/download/linux)
- Installeer desgewenst [Visual Studio Code](https://www.visualstudio.com/) en de [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp&dotnetid=963890049.1518206068)

# <a name="macostabmacos"></a>[MacOS](#tab/macos)

- Installeer [.NET Core voor macOS](https://www.microsoft.com/net/download/macos).
- Installeer desgewenst [Visual Studio voor Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

---

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De voorbeeldtoepassing die in deze quickstart wordt gebruikt, is een basisconsoletoepassing. U kunt de voorbeeldtoepassing verkennen op [GitHub](https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart).

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Zoek voor het openen van de Visual Studio-oplossing naar de map *storage-blobs-dotnet-quickstart*, open deze en dubbelklik op *storage-blobs-dotnet-quickstart.sln*. 

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

U moet de verbindingsreeks voor uw opslagaccount opgeven om de toepassing uit te voeren. In de voorbeeldtoepassing wordt de verbindingsreeks uit een omgevingsvariabele gelezen en gebruikt om aanvragen voor Azure Storage te autoriseren.

Nadat u de verbindingsreeks hebt gekopieerd, schrijft u deze naar een nieuwe omgevingsvariabele op de lokale computer waarop de toepassing wordt uitgevoerd. Als u de omgevingsvariabele wilt instellen, opent u een consolevenster en volgt u de aanwijzingen voor uw besturingssysteem. Vervang `<yourconnectionstring>` door de feitelijke verbindingsreeks:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

Nadat u de omgevingsvariabele hebt toegevoegd, moet u actieve programma's die de omgevingsvariabele moeten lezen, opnieuw starten. Start ook het consolevenster opnieuw. Als u bijvoorbeeld Visual Studio als editor gebruikt, start u Visual Studio opnieuw voordat u het voorbeeld uitvoert. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export storageconnectionstring=<yourconnectionstring>
```

Nadat u de omgevingsvariabele toevoegt, voert u `source ~/.bashrc` uit vanuit het consolevenster om de wijzigingen van kracht te laten worden.

# <a name="macostabmacos"></a>[MacOS](#tab/macos)

Bewerk uw .bash_profile en voeg de omgevingsvariabele toe:

```bash
export STORAGE_CONNECTION_STRING=<yourconnectionstring>
```

Nadat u de omgevingsvariabele toevoegt, voert u `source .bash_profile` uit vanuit het consolevenster om de wijzigingen van kracht te laten worden.

---

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met dit voorbeeld wordt een testbestand gemaakt in uw lokale map **Mijn documenten** en geüpload naar de Blob-opslag. Vervolgens wordt een lijst gemaakt van de blobs in de container en wordt het bestand gedownload met een nieuwe naam, zodat u het oude en nieuwe bestand kunt vergelijken. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Als u Visual Studio als editor gebruikt, drukt u op **F5** om het uit te voeren. 

Navigeer anders naar de toepassingsmap en voer de toepassing uit met de opdracht `dotnet run`.

```
dotnet run
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Navigeer naar de toepassingsmap en voer de toepassing uit met de opdracht `dotnet run`.

```
dotnet run
```

# <a name="macostabmacos"></a>[MacOS](#tab/macos)

Navigeer naar de toepassingsmap en voer de toepassing uit met de opdracht `dotnet run`.

```
dotnet run
```

---

De uitvoer van de voorbeeldtoepassing is vergelijkbaar met het volgende voorbeeld:

```
Azure Blob storage - .NET Quickstart sample

Created container 'quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f'

Temp file = C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt
Uploading to Blob storage as blob 'QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt'

Listing blobs in container.
https://storagesamples.blob.core.windows.net/quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f/QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt

Downloading blob to C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db_DOWNLOADED.txt

Press any key to delete the sample files and example container.
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
// Studio, the shell or application needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");

// Check whether the connection string can be parsed.
CloudStorageAccount storageAccount;
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
        "Add an environment variable named 'storageconnectionstring' with your storage " +
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
CloudBlobContainer cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
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

- Download het NuGet-pakket voor de nieuwste versie van de [.NET-clientbibliotheek](https://www.nuget.org/packages/WindowsAzure.Storage/) voor Azure Storage. 
- Bekijk de [broncode van de .NET-clientbibliotheek](https://github.com/Azure/azure-storage-net) op GitHub.

### <a name="client-library-reference-and-samples"></a>Naslaginformatie en voorbeelden voor de .NET-clientbibliotheek

- Zie het [.NET API-referentiemateriaal](https://docs.microsoft.com/dotnet/api/overview/azure/storage) voor meer informatie over de .NET-clientbibliotheek.
- Verken [Blob-opslagvoorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob) die zijn geschreven met de .NET-clientbibliotheek.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u blobs kunt uploaden, downloaden en er een lijst van maken met behulp van .NET. 

Ga door naar [Afbeeldingsgegevens uploaden in de cloud met Azure Storage](storage-upload-process-images.md) om te leren hoe u een web-app kunt maken waarmee u een afbeelding kunt uploaden naar Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](storage-dotnet-how-to-use-blobs.md)

- Zie voor meer informatie over .NET Core [Aan de slag met .NET in 10 minuten](https://www.microsoft.com/net/learn/get-started/).
- Zie [.NET Photo Gallery Web Application Sample with Azure Blob Storage](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/) voor meer informatie over een voorbeeld van een toepassing die u kunt implementeren vanuit Visual Studio voor Windows.
 

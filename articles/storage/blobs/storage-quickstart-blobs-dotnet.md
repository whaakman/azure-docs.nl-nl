---
title: Azure-snelstartgids - Objecten overdragen naar/van Azure Blob-opslag met .NET | Microsoft Docs
description: Snel leren om objecten over te dragen naar/van Azure Blob-opslag met .NET
services: storage
documentationcenter: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 12/04/2017
ms.author: tamram
ms.openlocfilehash: 5020f070a8eb9215f175fc3ff3a905cff28ce37f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Objecten overdragen naar/van Azure Blob-opslag met .NET

In deze snelstartgids leert u hoe u de .NET-clientbibliotheek voor Azure Storage gebruikt om blok-bobs in een container te uploaden, te downloaden en weer te geven.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze snelstartgids:

* .NET core 2.0 voor [Linux](/dotnet/core/linux-prerequisites?tabs=netcore2x) of [Windows](/dotnet/core/windows-prerequisites?tabs=netcore2x) installeren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De voorbeeldtoepassing die in deze snelstartgids wordt gebruikt, is een basisconsoletoepassing. 

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Zoek voor het openen van de Visual Studio-oplossing naar de map storage-blobs-dotnet-quickstart, open deze en dubbelklik op storage-blobs-dotnet-quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

In de toepassing moet u de verbindingsreeks voor uw opslagaccount opgeven. U wordt aangeraden deze verbindingsreeks binnen een omgevingsvariabele op te slaan op de lokale computer waarop de toepassing wordt uitgevoerd. Volg een van de onderstaande voorbeelden afhankelijk van uw besturingssysteem voor het maken van de omgevingsvariabele.  Vervang \<yourconnectionstring\> door uw werkelijke verbindingsreeks.

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```
### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met dit voorbeeld wordt een testbestand gemaakt in Mijn documenten, wordt dit bestand geüpload naar Blob-opslag, worden de blobs vermeld in de container en wordt het bestand vervolgens met een nieuwe naam gedownload, zodat u het oude en het nieuwe bestand met elkaar kunt vergelijken. 

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

Wanneer u op een willekeurige toets drukt om door te gaan, worden de opslagcontainer en de bestanden verwijderd. Controleer de map Mijn documenten op de twee bestanden voordat u doorgaat. Als u ze opent, ziet u dat ze identiek zijn. Kopieer de URL voor de blob in het consolevenster en plak deze in een browser om de inhoud van het bestand in de Blob-opslag weer te geven.

U kunt ook een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gebruiken om de bestanden in de Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang hebt tot de gegevens van uw opslagaccount. 

Nadat u de bestanden hebt gecontroleerd, drukt u op een willekeurige toets om de demo te voltooien en de testbestanden te verwijderen. Nu u weet wat het voorbeeld doet, opent u het bestand Program.cs om de code te bekijken. 

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

We gaan nu de voorbeeldcode bespreken, zodat u begrijpt hoe deze werkt.

### <a name="get-references-to-the-storage-objects"></a>Verwijzingen naar de opslagobjecten ophalen

Als eerste moeten verwijzingen worden gemaakt naar objecten die worden gebruikt voor het verkrijgen van toegang tot de Blob-opslag en voor het beheren ervan. Deze objecten zijn afhankelijk van elkaar en elk object vormt de basis voor het volgende object in de lijst.

* Maak een exemplaar van het [CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)-object, dat naar het opslagaccount wijst.

* Maak een exemplaar van het [CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)-object, dat naar de Blob-service in uw opslagaccount wijst.

* Maak een exemplaar van het [CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)-object, dat de container die u opent vertegenwoordigt. Containers worden gebruikt om uw blobs te organiseren op een wijze die lijkt op het gebruik van mappen op uw computer om uw bestanden te organiseren.

Wanneer u de [Cloud Blob-container](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet) hebt, kunt u een exemplaar maken van het object [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet) dat verwijst naar de specifieke blob waarin u geïnteresseerd bent, en kunt u bewerkingen uitvoeren zoals uploaden, downloaden en kopiëren.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de namen van containers en blobs.

In deze sectie gaat u een exemplaar maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn en via slechts een URL kunnen worden geopend. De container heeft de naam **quickstartblobs**.

In dit voorbeeld wordt [CreateIfNotExists](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.createifnotexists?view=azure-dotnet) gebruikt omdat we bij elke nieuwe uitvoering van het voorbeeld een nieuwe container willen maken. In een productieomgeving waarin u overal in een toepassing dezelfde container gebruikt, is het beter om **CreateIfNotExists** maar één keer aan te roepen. U kunt de container ook vooraf maken, zodat u deze niet in de code hoeft te maken.

```csharp
// Load the connection string for use with the application. The storage connection string is stored
// in an environment variable on the machine running the application called storageconnectionstring.
// If the environment variable is created after the application is launched in a console or with Visual
// Studio, the shell needs to be closed and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("storageconnectionstring");
if (storageConnectionString == null)
{
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add a environment variable name 'storageconnectionstring' with the actual storage " +
        "connection string as a value.");
}
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConnectionString);

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
cloudBlobContainer = cloudBlobClient.GetContainerReference("quickstartblobs" + Guid.NewGuid().ToString());
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs worden het meest gebruikt, en dat is wat er wordt gebruikt in deze snelstartgids.

Als u een bestand naar een blob wilt uploaden, haalt u een verwijzing naar de blob op in de doelcontainer. Wanneer u de blobverwijzing hebt, kunt u er gegevens naar uploaden met behulp van [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als deze wel al bestaat.

Met de voorbeeldcode wordt een lokaal bestand gemaakt voor het uploaden en downloaden, waarmee het bestand dat moet worden geüpload als **fileAndPath** en de naam van de blob in **localFileName** worden opgeslagen. In het volgende voorbeeld wordt het bestand geüpload naar de container met de naam **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

Console.WriteLine("Temp file = {0}", sourceFile);
Console.WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the location where the blob is going to go, then upload the file.
// Upload the file you created, use localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);
```

Er zijn meerdere uploadmethoden die u kunt gebruiken met Blob-opslag. Als u bijvoorbeeld een geheugenstream hebt, kunt u de UploadFromStreamAsync-methode gebruiken in plaats van UploadFromFileAsync.

Blok-bobs kunnen een willekeurig soort tekst- of binair bestand zijn. Pagina-blobs worden hoofdzakelijk gebruikt voor de VHD-bestanden die worden gebruikt als back-up voor IaaS-VM‘s. Toevoeg-blobs worden gebruikt voor logboekregistratie, bijvoorbeeld wanneer u gegevens wilt wegschrijven naar een bestand en vervolgens gegevens wilt blijven toevoegen. De meeste objecten die worden opgeslagen in Blob-opslag, zijn blok-blobs.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

U kunt een lijst met bestanden in de container ophalen met behulp van [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). Met de volgende code wordt de lijst met blobs opgehaald en doorlopen, waarbij de URL‘s van de gevonden blobs worden weergegeven. U kunt de URI uit het opdrachtvenster kopiëren en in een browser plakken om het bestand weer te geven.

Als u 5.000 of minder blobs in de container hebt, worden alle blobnamen in één aanroep naar ListBlobsSegmentedAsync opgehaald. Als u meer dan 5.000 blobs in de container hebt, wordt de lijst door de service opgehaald in sets van 5.000 totdat alle blobnamen zijn opgehaald. De eerste keer dat deze API wordt aangeroepen, worden dus de eerste 5000 namen en een vervolgtoken geretourneerd. De tweede keer geeft u het token op en haalt de service de volgende set blobnamen op, enzovoort, totdat het vervolgtoken null is, wat aangeeft dat alle blobnamen zijn opgehaald.

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
    blobContinuationToken = results.ContinuationToken;
} while (blobContinuationToken != null);

```

### <a name="download-blobs"></a>Blobs downloaden

Download blobs naar uw lokale schijf met behulp van [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

Met de volgende code wordt de blob gedownload die in een eerdere sectie is geüpload, waarbij het achtervoegsel '_DOWNLOADED' wordt toegevoegd aan de blobnaam, zodat u beide bestanden op de lokale schijf kunt zien.

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);  
```

### <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs die in deze snelstartgids zijn geüpload niet langer nodig hebt, kunt u de volledige container verwijderen met behulp van [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). Verwijder ook de gemaakte bestanden als u deze niet langer nodig hebt.

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

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag met behulp van .NET. Voor meer informatie over het werken met Blob-opslag, gaat u naar de instructies voor Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](storage-dotnet-how-to-use-blobs.md)

Zie de lijst met [Azure Storage-voorbeelden met behulp van .NET](../common/storage-samples-dotnet.md) voor aanvullende Azure Storage-codevoorbeelden die u kunt downloaden en uitvoeren.

Zie [Azure Blob-opslagresources beheren met Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over de Storage Explorer en blobs.

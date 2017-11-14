---
title: Azure Quickstart - objecten voor overdracht naar/van Azure Blob storage met .NET | Microsoft Docs
description: Snel informatie over het overdragen van objecten uit Azure Blob storage met .NET
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
ms.date: 11/10/2017
ms.author: tamram
ms.openlocfilehash: 1eac4165c35cb116a359c074bd629c918b58097c
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Objecten voor overdracht naar/van Azure Blob storage met .NET

In deze snelstartgids leert u hoe u met de .NET-clientbibliotheek voor Azure Storage uploadt, downloaden en weergeven van blok-blobs in een container.

## <a name="prerequisites"></a>Vereisten

Installeren voor het voltooien van deze snelstartgids [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) met de volgende werkbelasting:
    
    - **Azure-ontwikkeling**

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De voorbeeldtoepassing gebruikt in deze snelstartgids is een eenvoudige consoletoepassing. 

Gebruik [git](https://git-scm.com/) een kopie van de toepassing op uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Met deze opdracht kloont de opslagplaats naar uw lokale git-map. Open de Visual Studio-oplossing, zoekt u naar de map storage-blobs-dotnet-Quick Start, opent u het en dubbelklik op de storage-blobs-dotnet-quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

In de toepassing, moet u de verbindingsreeks opgeven voor uw opslagaccount. Open de `app.config` bestand vanuit Solution Explorer in Visual Studio. Zoek de `StorageConnectionString` vermelding. Voor **waarde**, de volledige waarde van de verbindingsreeks vervangen door het u vanuit de Azure-portal hebt opgeslagen. Uw `storageConnectionString` moet er ongeveer als volgt uitzien:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;
    AccountName=<NameHere>;
    AccountKey=<KeyHere>" />
  </appSettings>
</configuration>
```

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

Dit voorbeeld maakt een testbestand in Mijn documenten, geüpload naar Blob storage, geeft een lijst van de blobs in de container en downloadt het bestand met een nieuwe naam zodat u de oude en nieuwe bestanden kunt vergelijken. 

Voer het voorbeeld door op F5 te drukken. Deze ziet u uitvoer in een consolevenster die vergelijkbaar is met het volgende: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Wanneer u op een willekeurige toets om door te gaan, worden de storage-container en de bestanden verwijderd. Controleer voordat u doorgaat, Mijn documenten voor de twee bestanden. U kunt deze openen en ze identiek zijn. Kopieer de URL voor de blob buiten het consolevenster en plak deze in een browser om te bekijken van de inhoud van het bestand in Blob-opslag.

U kunt ook een hulpprogramma zoals gebruiken de [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) de bestanden in Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang tot de gegevens van uw storage-account. 

Nadat u de bestanden hebt gecontroleerd, druk op een willekeurige toets om de demo voltooien en verwijderen van de testbestanden. Nu dat u wat het voorbeeld doet weet, opent u het bestand Program.cs om te kijken naar de code. 

## <a name="understand-the-sample-code"></a>Inzicht in de voorbeeldcode

Daarna bekijken we de voorbeeldcode zodat u kunt begrijpen hoe het werkt.

### <a name="get-references-to-the-storage-objects"></a>Het ophalen van verwijzingen naar de opslagobjecten

Het eerste wat te doen is de verwijzingen naar objecten die worden gebruikt voor toegang tot en beheren van Blob-opslag maken. Deze objecten bouwen op elkaar--elk wordt gebruikt door de volgende gateway in de lijst.

* Maak een instantie van de **CloudStorageAccount** object verwijst naar het opslagaccount. 

* Maak een instantie van de **CloudBlobClient** -object, dat naar de Blob-service in uw opslagaccount verwijst. 

* Maak een instantie van de **CloudBlobContainer** -object, dat staat voor de container die u wilt openen. Containers worden gebruikt om uw blobs te organiseren dat u de mappen op uw computer gebruiken om uw bestanden te organiseren.

Zodra u hebt de **CloudBlobContainer**, kunt u een exemplaar van de **CloudBlockBlob** -object dat verwijst naar de specifieke blob waarin u geïnteresseerd bent en uitvoeren van een uploaden, downloaden, kopiëren, enzovoort. de bewerking.

> [!IMPORTANT]
> Containernamen moeten kleine letters. Zie [Naming en verwijzen naar Containers, Blobs en metagegevens](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) voor meer informatie over de container en de blob-namen.

In deze sectie geen exemplaar maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn en kunnen worden geopend met alleen een URL. De container wordt aangeroepen **quickstartblobs**. 

In dit voorbeeld wordt **CreateIfNotExists** omdat we willen maken van een nieuwe container telkens wanneer de steekproef wordt uitgevoerd. In een productieomgeving waarbij u gebruikmaakt van de container in een toepassing, is het beter idee om alleen aanroepen **CreateIfNotExists** zodra. U kunt ook moeten voor het maken van de container tevoren zodat u niet in de code te maken.

```csharp
// Create a CloudStorageAccount instance pointing to your storage account.
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs");
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions();
permissions.PublicAccess = BlobContainerPublicAccessType.Blob;
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>BLOB's uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs zijn het meest gebruikt, en dat is wat wordt gebruikt in deze snelstartgids. 

Als u wilt uploaden naar een blob, geen verwijzing ophalen naar de blob in de doelcontainer. Zodra u de blobverwijzing hebt, kunt u gegevens uploaden naar deze met behulp van [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of deze wordt overschreven als deze al bestaat.

De voorbeeldcode maakt een lokaal bestand moet worden gebruikt voor het uploaden en downloaden, het opslaan van het bestand moet worden geüpload als **fileAndPath** en de naam van de blob in **Lokale_bestandsnaam**. Het volgende voorbeeld wordt het bestand geüpload naar uw zogenaamd **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string fileAndPath = Path.Combine(localPath, localFileName);
File.WriteAllText(fileAndPath, "Hello, World!");

//Upload the file.
CloudBlockBlob blockBlob = container.GetBlockBlobReference(localFileName);
await blockBlob.UploadFromFileAsync(fileAndPath);
```

Er zijn verschillende methoden voor het uploaden die u met Blob storage gebruiken kunt. Bijvoorbeeld, als u een geheugenstream hebt, kunt u de methode UploadFromStreamAsync in plaats van de UploadFromFileAsync. 

Blok-blobs kunnen elk type tekst of binair bestand zijn. Pagina-blobs zijn voornamelijk gebruikt voor de VHD-bestanden gebruikt voor back-IaaS VM's. Toevoeg-BLOB's worden gebruikt voor logboekregistratie, zoals indien u wenst te schrijven naar een bestand en klik vervolgens houden meer gegevens toe te voegen. De meeste objecten die zijn opgeslagen in Blob storage zijn blok-blobs.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

U krijgt een lijst met bestanden in de container met behulp [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). De volgende code haalt de lijst met blobs en doorlopen, met de URI van de blobs. U kunt de URI in het opdrachtvenster kopieert en plakt u deze in een browser om het bestand weer te geven.

Als u 5.000 of minder blobs in de container hebt, worden alle blobnamen van de met één aanroep ListBlobsSegmentedAsync opgehaald. Als u meer dan 5000 blobs in de container hebt, haalt de service de lijst in sets van 5.000 totdat alle van de blob-namen zijn opgehaald. Zodat de eerste keer dat deze API wordt aangeroepen, wordt de eerste 5000 blobnamen en een vervolgtoken. De tweede keer opgeven van het token, haalt de service de volgende set van blob-namen en enzovoort, totdat het vervolgtoken is null, waarmee wordt aangegeven dat alle van de blob-namen zijn opgehaald. 

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    foreach(IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null);
```

### <a name="download-blobs"></a>Blobs downloaden

Blobs downloaden naar uw lokale schijf met [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

De volgende code downloadt de blob geüpload in een eerder gedeelte een achtervoegsel van '_DOWNLOADED' toevoegen aan de blob-naam, zodat u beide bestanden op lokale schijf kunt zien. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

### <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs in deze snelstartgids geüpload niet meer nodig hebt, kunt u de volledige container via te verwijderen [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). De bestanden die zijn gemaakt als ze niet meer nodig zijn ook verwijderd.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u bestanden overbrengen tussen een lokale schijf en Azure Blob storage met .NET. Voor meer informatie over het werken met Blob storage, blijven de instructies van de Blob-opslag.

> [!div class="nextstepaction"]
> [Procedures voor de bewerkingen van de BLOB-opslag](storage-dotnet-how-to-use-blobs.md)

Voor extra Azure Storage-codevoorbeelden die u kunt downloaden en uitvoeren, raadpleegt u de lijst met [voorbeelden van Azure Storage met .NET](../common/storage-samples-dotnet.md).

Zie voor meer informatie over de Storage Explorer en Blobs [beheren Azure Blob storage-resources met Opslagverkenner](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

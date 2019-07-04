---
title: 'Quickstart: Azure Blob storage-clientbibliotheek voor .NET'
description: In deze snelstartgids leert u hoe u de Azure Blob storage-clientbibliotheek voor .NET gebruiken om te maken van een container en een blob in blobopslag (object). Hierna leert u hoe u de blob naar uw lokale computer downloadt en hoe u alle blobs in een container kunt weergeven.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: mhopkins
ms.subservice: blobs
ms.openlocfilehash: c5e9981c6854ff778775631f1d671189830e564b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67435754"
---
# <a name="quickstart-azure-blob-storage-client-library-for-net"></a>Quickstart: Azure Blob storage-clientbibliotheek voor .NET

Aan de slag met de Azure Blob Storage-clientbibliotheek voor .NET. Azure Blob Storage is Microsoft de object-oplossing voor de cloud. Volg de stappen voor het installeren van het pakket en voorbeeldcode voor basistaken uitproberen. Blob Storage is geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde gegevens.

De Azure Blob Storage-clientbibliotheek voor .NET te gebruiken:

* Een container maken
* Machtigingen instellen voor een container
* Een blob maken in Azure Storage
* De blob te downloaden naar uw lokale computer
* Overzicht van de blobs in een container
* Een container verwijderen

[API-referentiedocumentatie](https://docs.microsoft.com/dotnet/api/overview/azure/storage?view=azure-dotnet) | [broncode clientbibliotheek](https://github.com/Azure/azure-storage-net/tree/master/Blob) | [pakket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/) | [voorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=dotnet&term=blob)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [maakt u er een gratis](https://azure.microsoft.com/free/)
* Azure Storage-account - [een opslagaccount maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* [NET Core-SDK 2.1](https://dotnet.microsoft.com/download/dotnet-core), of hoger, voor uw besturingssysteem

## <a name="setting-up"></a>Instellen

In deze sectie helpt u bij het voorbereiden van een project om te werken met de Azure Blob Storage-clientbibliotheek voor .NET.

### <a name="create-the-project"></a>Het project maken

Maak eerst een .NET Core-toepassing met de naam **blob-quickstart**.

1. In een consolevenster (zoals cmd, PowerShell of Bash), gebruikt u de `dotnet new` opdracht voor het maken van een nieuwe console-app met de naam van de **blob-quickstart**. Deze opdracht maakt u een eenvoudige 'Hallo wereld" C# project met een één-bronbestand: **Program.cs**.

   ```console
   dotnet new console -n blob-quickstart
   ```

2. Schakel over naar de zojuist gemaakte **blob-quickstart** map en bouwen de app om te controleren of alles goed.

   ```console
   cd blob-quickstart
   ```

   ```console
   dotnet build
   ```

De verwachte uitvoer van de build moet er ongeveer als volgt uit:

```output
C:\QuickStarts\blob-quickstart> dotnet build
Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 44.31 ms for C:\QuickStarts\blob-quickstart\blob-quickstart.csproj.
  blob-quickstart -> C:\QuickStarts\blob-quickstart\bin\Debug\netcoreapp2.1\blob-quickstart.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:03.08
```

### <a name="install-the-package"></a>Installeer het pakket

Terwijl u nog in de toepassingsmap, door de Azure Blob Storage-clientbibliotheek voor .NET-pakket te installeren met behulp van de `dotnet add package` opdracht.

```console
dotnet add package Microsoft.Azure.Storage.Blob
```

### <a name="set-up-the-app-framework"></a>Instellen van de app-framework

In de projectmap:

1. Open het bestand Program.cs in de editor
2. Verwijder de **Console.WriteLine** instructie
3. Voeg **met behulp van** richtlijnen
4. Maak een **ProcessAsync** methode waar de belangrijkste code voor het voorbeeld worden geplaatst
5. Asynchroon aanroepen de **ProcessAsync** methode **Main**

Dit is de code:

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;

namespace blob_quickstart
{
    class Program
    {
        public static void Main()
        {
            Console.WriteLine("Azure Blob Storage - .NET quickstart sample\n");

            // Run the examples asynchronously, wait for the results before proceeding
            ProcessAsync().GetAwaiter().GetResult();

            Console.WriteLine("Press any key to exit the sample application.");
            Console.ReadLine();
        }

        private static async Task ProcessAsync()
        {
        }
    }
}
```

### <a name="copy-your-credentials-from-the-azure-portal"></a>Kopieer uw referenties van de Azure Portal

De voorbeeldtoepassing moet de toegang tot uw opslagaccount verifiëren. Om te verifiëren voegt u de referenties van uw opslagaccount als een verbindingsreeks aan de toepassing toe. U kunt de referenties van het opslagaccount weergeven door de volgende stappen te volgen:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Zoek uw opslagaccount.
3. In de sectie **Instellingen** van het overzicht met opslagaccounts selecteert u **Toegangssleutels**. Hier worden de toegangssleutels van uw account weergegeven, evenals de volledige verbindingsreeks voor elke sleutel.
4. Zoek de waarde van de **Verbindingsreeks** onder **key1** en selecteer de knop **Kopiëren** om de verbindingsreeks te kopiëren. U gaat in de volgende stap de waarde voor de verbinding toevoegen aan een omgevingsvariabele.

    ![Schermopname waarin een verbindingsreeks vanuit de Azure-portal wordt gekopieerd](../../../includes/media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

Nadat u de verbindingsreeks hebt gekopieerd, schrijft u deze naar een nieuwe omgevingsvariabele op de lokale computer waarop de toepassing wordt uitgevoerd. Als u de omgevingsvariabele wilt instellen, opent u een consolevenster en volgt u de aanwijzingen voor uw besturingssysteem. Vervang `<yourconnectionstring>` door uw werkelijke verbindingsreeks.

Nadat u de omgevingsvariabele toevoegt, moet u mogelijk opnieuw opstarten van alle actieve programma's die nodig hebt om te lezen van de omgevingsvariabele. Bijvoorbeeld, als u Visual Studio als editor gebruikt, Visual Studio opnieuw voordat u het voorbeeld uitvoert.

#### <a name="windows"></a>Windows

```cmd
setx STORAGE_CONNECTION_STRING "<yourconnectionstring>"
```

#### <a name="linux"></a>Linux

```bash
export STORAGE_CONNECTION_STRING="<yourconnectionstring>"
```

#### <a name="macos"></a>MacOS

```bash
export STORAGE_CONNECTION_STRING="<yourconnectionstring>"
```

## <a name="object-model"></a>Objectmodel

Azure Blob storage is geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde gegevens. Ongestructureerde gegevens zijn gegevens die niet voldoen aan een bepaald gegevensmodel of bepaalde definitie, zoals tekst of binaire gegevens. Er zijn drie typen resources voor blobopslag:

* Het opslagaccount.
* Een container in het storage-account
* Een blob in een container

Het volgende diagram geeft de relatie tussen deze resources weer.

![Diagram van de blobopslagarchitectuur](./media/storage-quickstart-blobs-dotnet/blob1.png)

De volgende .NET-klassen gebruiken om te communiceren met deze resources:

* [CloudStorageAccount](/dotnet/api/microsoft.azure.storage.cloudstorageaccount): De **CloudStorageAccount** klasse vertegenwoordigt uw Azure storage-account. Deze klasse gebruiken toegang verlenen aan Blob-opslag met behulp van de toegangssleutels van uw account.
* [CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient): De **CloudBlobClient** klasse biedt een toegangspunt voor de Blob-service in uw code.
* [CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer): De **CloudBlobContainer** klasse vertegenwoordigt een blobcontainer in uw code.
* [CloudBlockBlob](//dotnet/api/microsoft.azure.storage.blob.cloudblockblob): De **CloudBlockBlob** object vertegenwoordigt een blok-blob in uw code. Blok-blobs bestaan uit blokken met gegevens die afzonderlijk kunnen worden beheerd.

## <a name="code-examples"></a>Voorbeelden van code

Deze voorbeelden van codefragmenten laten zien hoe u het volgende bij de Azure Blob storage-clientbibliotheek voor .NET uitvoeren:

   * [Verifiëren van de client](#authenticate-the-client)
   * [Een container maken](#create-a-container)
   * [Machtigingen instellen voor een container](#set-permissions-on-a-container)
   * [Blobs uploaden naar een container](#upload-blobs-to-a-container)
   * [De blobs in een container te vermelden](#list-the-blobs-in-a-container)
   * [Downloaden van blobs](#download-blobs)
   * [Container verwijderen](#delete-a-container)

### <a name="authenticate-the-client"></a>Verifiëren van de client

De onderstaande code gecontroleerd of de omgevingsvariabele een verbindingsreeks die kan worden geparseerd bevat voor het maken van een [CloudStorageAccount](/dotnet/api/microsoft.azure.storage.cloudstorageaccount?view=azure-dotnet) object dat verwijst naar het opslagaccount. Gebruik de [TryParse](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.tryparse?view=azure-dotnet)-methode om te controleren of de verbindingsreeks geldig is. Als **TryParse** lukt, wordt de variabele *storageAccount* geïnitialiseerd en **true** geretourneerd.

Voeg deze code binnen de **ProcessAsync** methode:

```csharp
// Retrieve the connection string for use with the application. The storage 
// connection string is stored in an environment variable on the machine 
// running the application called STORAGE_CONNECTION_STRING. If the 
// environment variable is created after the application is launched in a 
// console or with Visual Studio, the shell or application needs to be closed
// and reloaded to take the environment variable into account.
string storageConnectionString = Environment.GetEnvironmentVariable("STORAGE_CONNECTION_STRING");

// Check whether the connection string can be parsed.
CloudStorageAccount storageAccount;
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // If the connection string is valid, proceed with operations against Blob
    // storage here.
    // ADD OTHER OPERATIONS HERE
}
else
{
    // Otherwise, let the user know that they need to define the environment variable.
    Console.WriteLine(
        "A connection string has not been defined in the system environment variables. " +
        "Add an environment variable named 'STORAGE_CONNECTION_STRING' with your storage " +
        "connection string as a value.");
    Console.WriteLine("Press any key to exit the application.");
    Console.ReadLine();
}
```

> [!NOTE]
> Als u wilt de rest van de bewerkingen in dit artikel uitvoert, Vervang **/ / andere bewerkingen hier toevoegen** in de bovenstaande code met de codefragmenten in de volgende secties.

### <a name="create-a-container"></a>Een container maken

Maak om de container te maken eerst een instantie van het object [CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient), dat naar de Blob-opslag in uw opslagaccount wijst. Maak vervolgens een instantie van het object [CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer) en maak de container.

In dit geval wordt de code roept de [CreateAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createasync) methode om de container te maken. Er wordt een GUID-waarde toegevoegd aan de containernaam om ervoor te zorgen dat deze uniek is. In een productieomgeving is het vaak beter gebruik van de [CreateIfNotExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createifnotexistsasync) methode voor het maken van een container alleen als deze niet al bestaat.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [Containers, blobs en metagegevens een naam geven en hiernaar verwijderen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) voor meer informatie over de naamgeving van containers en blobs.

```csharp
// Create the CloudBlobClient that represents the 
// Blob storage endpoint for the storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs' and 
// append a GUID value to it to make the name unique.
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs" + 
        Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();
```

### <a name="set-permissions-on-a-container"></a>Machtigingen instellen voor een container

Machtigingen instellen voor de container, zodat alle blobs in de container openbaar zijn. Als een blob openbaar is, is deze anoniem toegankelijk voor elke client.

```csharp
// Set the permissions so the blobs are public.
BlobContainerPermissions permissions = new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
};
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-a-container"></a>Blobs uploaden naar een container

Het volgende codefragment wordt een verwijzing naar een **CloudBlockBlob** object door het aanroepen van de [GetBlockBlobReference](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getblockblobreference) methode voor de container in de vorige sectie hebt gemaakt. Vervolgens het geselecteerde lokale bestand uploadt naar de blob door het aanroepen van de [UploadFromFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.uploadfromfileasync) methode. Met deze methode wordt de blob gemaakt als deze nog niet bestaat, of overschreven als dat wel het geval is.

```csharp
// Create a file in your local MyDocuments folder to upload to a blob.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string sourceFile = Path.Combine(localPath, localFileName);
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

De blobs in de container te vermelden met behulp van de [ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync) methode. In dit geval is slechts één blob toegevoegd aan de container, zodat de lijstbewerking alleen die ene blob retourneert.

Als er te veel blobs zijn om met één aanroep te retourneren (standaard meer dan 5000), retourneert de **ListBlobsSegmentedAsync**-methode een segment van de totale resultatenset en een vervolgtoken. Om het volgende segment met blobs op te halen, geeft u het vervolgtoken op dat door de vorige aanroep is geretourneerd, enzovoort, totdat het vervolgtoken null is. Een null-vervolgtoken geeft aan dat alle blobs zijn opgehaald. De code toont hoe u de voortzetting van aanbevolen procedures zien.

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

Downloaden van de eerder gemaakte blob naar uw lokale bestandssysteem met behulp van de [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync) methode. De voorbeeldcode voegt het achtervoegsel "_downloaded"toe aan de naam van de blob zodat u beide bestanden in het lokale bestandssysteem kunt zien.

```csharp
// Download the blob to a local file, using the reference created earlier.
// Append the string "_DOWNLOADED" before the .txt extension so that you 
// can see both files in MyDocuments.
string destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);
```

### <a name="delete-a-container"></a>Een container verwijderen

De volgende code opschonen van de resources de app gemaakt met behulp van de volledige container verwijderen [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteasync). U kunt ook de lokale bestanden verwijderen als u dat wilt.

```csharp
Console.WriteLine("Press the 'Enter' key to delete the example files, " +
    "example container, and exit the application.");
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

## <a name="run-the-code"></a>De code uitvoeren

Deze app wordt een testbestand gemaakt in uw lokale **Mijn documenten** map en geüpload naar Blob-opslag. Vervolgens worden de blobs in de container en wordt het bestand met een nieuwe naam gedownload, zodat u het oude en nieuwe bestand kunt vergelijken.

Als u Visual Studio als editor gebruikt, drukt u op **F5** om het uit te voeren.

Anders gaat u naar de toepassingsmap en vervolgens bouwen en uitvoeren van de toepassing.

```console
dotnet build
```

```console
dotnet run
```

De uitvoer van de voorbeeldtoepassing is vergelijkbaar met het volgende voorbeeld:

```output
Azure Blob storage - .NET Quickstart example

Created container 'quickstartblobs33c90d2a-eabd-4236-958b-5cc5949e731f'

Temp file = C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926-a9da-96
97c748f4db.txt
Uploading to Blob storage as blob 'QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f
4db.txt'

Listing blobs in container.
https://storagesamples.blob.core.windows.net/quickstartblobs33c90d2a-eabd-4236-
958b-5cc5949e731f/QuickStart_c5e7f24f-a7f8-4926-a9da-9697c748f4db.txt

Downloading blob to C:\Users\myusername\Documents\QuickStart_c5e7f24f-a7f8-4926
-a9da-9697c748f4db_DOWNLOADED.txt

Press any key to delete the example files and example container.
```

Wanneer u op **Enter** drukt, worden de opslagcontainer en de bestanden verwijderd. Controleer voordat u doorgaat of de map **Mijn documenten** de twee bestanden bevat. Als u ze opent, ziet u dat ze identiek zijn. Kopieer de URL van de blob in het consolevenster en plak deze in een browser om de inhoud van de blob weer te geven.

Nadat u de bestanden hebt gecontroleerd, drukt u op een willekeurige toets om de demo te voltooien en de testbestanden te verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u blobs kunt uploaden, downloaden en er een lijst van maken met behulp van .NET.

Voor meer informatie over het maken van een web-app die een afbeelding kunt naar Blob-opslag uploaden, gaat u verder naar:

> [!div class="nextstepaction"]
> [Uploaden en verwerken van een installatiekopie](storage-upload-process-images.md)

* Zie voor meer informatie over .NET Core [Aan de slag met .NET in 10 minuten](https://www.microsoft.com/net/learn/get-started/).
* Zie [.NET Photo Gallery Web Application Sample with Azure Blob Storage](https://azure.microsoft.com/resources/samples/storage-blobs-dotnet-webapp/) voor meer informatie over een voorbeeld van een toepassing die u kunt implementeren vanuit Visual Studio voor Windows.

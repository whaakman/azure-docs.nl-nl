---
title: Azure Quickstart - objecten voor overdracht naar/van Azure Blob storage met Java | Microsoft Docs
description: Snel informatie over het overdragen van objecten uit Azure Blob storage met Java
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: b096b9d79c049d8659a4171a0cbb42a99e245776
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Objecten voor overdracht naar/van Azure Blob storage met Java

In deze snelstartgids leert u hoe u Java gebruikt om te uploaden, downloaden en weergeven van blok-blobs in een container in Azure Blob-opslag.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart:

* Installeren van een IDE met Maven-integratie

* U kunt ook installeren en configureren van Maven te laten werken vanuit de opdrachtregel

Deze zelfstudie wordt gebruikgemaakt van [Eclipse](http://www.eclipse.org/downloads/) met de configuratie 'Eclipse IDE voor Java-ontwikkelaars'.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-java-quickstart) gebruikt in deze snelstartgids is een eenvoudige consoletoepassing. 

Gebruik [git](https://git-scm.com/) een kopie van de toepassing op uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Met deze opdracht kloont de opslagplaats naar uw lokale git-map. Open het project, start Eclipse en sluit het welkomstscherm. Selecteer **bestand** vervolgens **projecten van bestandssysteem openen...** . Zorg ervoor dat **detecteren en configureren van project aard** is ingeschakeld. Selecteer **Directory** navigeer vervolgens naar waar u de gekloonde opslagplaats, erin opgeslagen selecteert u de **javaBlobsQuickstart** map. Zorg ervoor dat de **javaBlobsQuickstarts** project wordt weergegeven als een Eclipse-project, vervolgens selecteert **voltooien**.

Zodra het project is voltooid importeren, opent u **AzureApp.java** (zich in **blobQuickstart.blobAzureApp** binnen van **src/main/java**), en vervang de `accountname`en `accountkey` binnen de `storageConnectionString` tekenreeks. Voer vervolgens de toepassing.
     

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren
    
In de toepassing, moet u de verbindingsreeks opgeven voor uw opslagaccount. Open de **AzureApp.Java** bestand. Zoek de `storageConnectionString` variabele. Vervang de `AccountName` en `AccountKey` waarden in de verbindingstekenreeks met de waarden die u hebt genoteerd in de Azure-portal. Uw `storageConnectionString` moet er ongeveer als volgt uitzien:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

Dit voorbeeld maakt een testbestand in de standaardmap (voor windows-gebruikers Mijn documenten), geüpload naar Blob storage, geeft een lijst van de blobs in de container en downloadt het bestand met een nieuwe naam zodat u de oude en nieuwe bestanden kunt vergelijken. 

Het voorbeeld uitvoert door te drukken **Ctrl + F11** in Eclipse.

Als u uitvoeren van het voorbeeld op de opdrachtregel met behulp van Maven wilt, opent u een shell en navigeer naar **blobAzureApp** binnen uw gekloonde directory. Voer `mvn compile exec:java`.

Hier volgt een voorbeeld van uitvoer als u de toepassing op Windows worden uitgevoerd.

```
Location of file: C:\Users\<user>\Documents\results.txt
File has been written
URI of blob is: http://myexamplesacct.blob.core.windows.net/quickstartblobs/results.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

 Voordat u doorgaat, Controleer de standaardmap (voor windows-gebruikers Mijn documenten) voor de twee bestanden. U kunt deze openen en ze identiek zijn. Kopieer de URL voor de blob buiten het consolevenster en plak deze in een browser om te bekijken van de inhoud van het bestand in Blob-opslag. Wanneer u op enter drukt, wordt de storage-container en de bestanden verwijderd.

U kunt ook een hulpprogramma zoals gebruiken de [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) de bestanden in Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang tot de gegevens van uw storage-account. 

Nadat u de bestanden hebt gecontroleerd, drukt u op enter drukken om de demo voltooien en verwijderen van de testbestanden. Nu dat u wat het voorbeeld doet weet, opent u de **AzureApp.java** bestand om te kijken naar de code. 

## <a name="understand-the-sample-code"></a>Inzicht in de voorbeeldcode

Het eerste wat te doen is de verwijzingen naar objecten die worden gebruikt voor toegang tot en beheren van Blob-opslag maken. Deze objecten bouwen op elkaar--elk wordt gebruikt door de volgende gateway in de lijst.

* Maak een instantie van de **CloudStorageAccount** object verwijst naar de [opslagaccount](/java/api/com.microsoft.azure.management.storage._storage_account).

De **CloudStorageAccount** object is een weergave van uw opslagaccount en kunt u instellen en eigenschappen van het opslagaccount programmatisch toegang. Met behulp van de **CloudStorageAccount** object kunt u een exemplaar van de **CloudBlobClient**, die nodig zijn voor toegang tot de blob-service is.

* Maak een instantie van de **CloudBlobClient** -object, dat naar verwijst de [Blob-service](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) in uw opslagaccount.

De **CloudBlobClient** biedt u een punt voor toegang tot de blob-service, zodat u kunt instellen en programmatisch toegang krijgen tot blob storage-eigenschappen. Met behulp van de **CloudBlobClient** kunt u een exemplaar van de **CloudBlobContainer** -object, dat nodig is om containers te maken.

* Geen exemplaar maken van de **CloudBlobContainer** -object, dat staat voor de [container](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) u opent. Containers worden gebruikt om uw blobs te organiseren dat u de mappen op uw computer gebruiken om uw bestanden te organiseren.    

Zodra u hebt de **CloudBlobContainer**, kunt u een exemplaar van de **CloudBlockBlob** -object dat naar de specifieke verwijst [blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) in die u vinden wilt, en een upload, downloaden, kopiëren, enzovoort-bewerking uitvoeren.

> [!IMPORTANT]
> Containernamen moeten kleine letters. Zie [Naming en verwijzen naar Containers, Blobs en metagegevens](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) voor meer informatie over de container en de blob-namen.

### <a name="create-a-container"></a>Een container maken 

In deze sectie geen exemplaar maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn en kunnen worden geopend met alleen een URL. De container wordt aangeroepen **quickstartblobs**. 

In dit voorbeeld wordt **CreateIfNotExists** omdat we willen maken van een nieuwe container telkens wanneer de steekproef wordt uitgevoerd. In een productieomgeving waarbij u gebruikmaakt van de container in een toepassing, is het beter idee om alleen aanroepen **CreateIfNotExists** zodra. U kunt ook de container tevoren maken, dus u hoeft niet in de code te maken.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>BLOB's uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs zijn het meest gebruikt, en dat is wat wordt gebruikt in deze snelstartgids. 

Als u wilt uploaden naar een blob, geen verwijzing ophalen naar de blob in de doelcontainer. Zodra u de blobverwijzing hebt, kunt u gegevens uploaden naar deze met behulp van [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of deze wordt overschreven als deze al bestaat.

De voorbeeldcode maakt een lokaal bestand om te worden gebruikt voor het uploaden en downloaden. opslaan van het bestand moet worden geüpload als **bron** en de naam van de blob in **blob**. Het volgende voorbeeld wordt het bestand geüpload naar uw zogenaamd **quickstartblobs**.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Er zijn verschillende [uploaden methoden](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) die u kunt gebruiken met Blob storage. Bijvoorbeeld, als er een tekenreeks, kunt u de methode UploadText in plaats van de methode uploaden. 

Blok-blobs kunnen elk type tekst of binair bestand zijn. Pagina-blobs zijn voornamelijk gebruikt voor de VHD-bestanden gebruikt voor back-IaaS VM's. Toevoeg-BLOB's worden gebruikt voor logboekregistratie, zoals indien u wenst te schrijven naar een bestand en klik vervolgens houden meer gegevens toe te voegen. De meeste objecten die zijn opgeslagen in Blob storage zijn blok-blobs.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

U krijgt een lijst met bestanden in de container met behulp [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). De volgende code haalt de lijst met blobs en doorlopen, met de URI van de blobs. U kunt de URI in het opdrachtvenster kopieert en plakt u deze in een browser om het bestand weer te geven.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Blobs downloaden

Blobs downloaden naar uw lokale schijf met [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

De volgende code downloadt de blob geüpload in een eerder gedeelte een achtervoegsel van '_DOWNLOADED' toevoegen aan de blob-naam, zodat u beide bestanden op lokale schijf kunt zien. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs in deze snelstartgids geüpload niet meer nodig hebt, kunt u de volledige container via te verwijderen [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Hiermee verwijdert u ook de bestanden in de container.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u bestanden overbrengen tussen een lokale schijf en Azure Blob storage met Java. Voor meer informatie over het werken met Blob storage, blijven de instructies van de Blob-opslag.

> [!div class="nextstepaction"]
> [Procedures voor de bewerkingen van de BLOB-opslag](storage-java-how-to-use-blob-storage.md)

Zie voor meer informatie over de Storage Explorer en Blobs [beheren Azure Blob storage-resources met Opslagverkenner](../../vs-azure-tools-storage-explorer-blobs.md).
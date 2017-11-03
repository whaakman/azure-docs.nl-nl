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
ms.openlocfilehash: 0ed27077f649df2d0673351cda708b9a358ff92b
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Objecten voor overdracht naar/van Azure Blob storage met Java

In deze snelstartgids leert u hoe u Java gebruikt om te uploaden, downloaden en weergeven van blok-blobs in een container in Azure Blob-opslag.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart:

* Installeren van een IDE dat geïntegreerd met Maven is of installeren en configureren van Maven te laten werken vanuit de opdrachtregel
    * Deze zelfstudie wordt gebruikgemaakt van [Eclipse](http://www.eclipse.org/downloads/) met de configuratie 'Eclipse IDE voor Java-ontwikkelaars'
    


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-storage-account-using-the-azure-portal"></a>Een opslagaccount met de Azure portal maken

Maak eerst een nieuwe, algemeen opslagaccount moet worden gebruikt voor deze snelstartgids. 

1. Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account. 
2. Selecteer in het menu Hub **nieuw** > **opslag** > **opslagaccount - blob, bestand, tabel, wachtrij**. 
3. Voer een naam in voor het opslagaccount. De naam moet tussen 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten. Ook moet het uniek.
4. Stel `Deployment model` naar **resourcemanager**.
5. Stel `Account kind` naar **algemeen**.
6. Stel `Performance` naar **standaard**. 
7. Stel `Replication` naar **lokaal redundante opslag (LRS)**.
8. Stel `Storage service encryption` naar **uitgeschakelde**.
9. Stel `Secure transfer required` naar **uitgeschakelde**.
10. Selecteer uw abonnement. 
11. Voor `resource group`, een nieuwe maken en een unieke naam geven. 
12. Selecteer de `Location` moet worden gebruikt voor uw opslagaccount.
13. Controleer **vastmaken aan dashboard** en klik op **maken** om uw opslagaccount te maken. 

Nadat uw storage-account is gemaakt, wordt deze is vastgemaakt aan het dashboard. Klik op deze te openen. Klik op instellingen **toegangssleutels**. Een sleutel selecteren en kopiëren naar het Klembord en plak deze in een teksteditor voor later gebruik. De naam van uw storage-account Kopieer en plak deze in een teksteditor voor later gebruik.

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
    public static final String storageConnectionString ="DefaultEndpointsProtocol=https;" +
     "AccountName=<Namehere>;" +
    "AccountKey=<Keyhere>";
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

## <a name="get-references-to-the-storage-objects"></a>Het ophalen van verwijzingen naar de opslagobjecten

Het eerste wat te doen is de verwijzingen naar objecten die worden gebruikt voor toegang tot en beheren van Blob-opslag maken. Deze objecten bouwen op elkaar--elk wordt gebruikt door de volgende gateway in de lijst.

* Maak een instantie van de **CloudStorageAccount** object verwijst naar de [opslagaccount](/java/api/com.microsoft.azure.management.storage._storage_account). 

* Maak een instantie van de **CloudBlobClient** -object, dat naar verwijst de [Blob-service](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) in uw opslagaccount. 

* Geen exemplaar maken van de **CloudBlobContainer** -object, dat staat voor de [container](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) u opent. Containers worden gebruikt om uw blobs te organiseren dat u de mappen op uw computer gebruiken om uw bestanden te organiseren.

Zodra u hebt de **CloudBlobContainer**, kunt u een exemplaar van de **CloudBlockBlob** -object dat naar de specifieke verwijst [blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) in die u vinden wilt, en een upload, downloaden, kopiëren, enzovoort-bewerking uitvoeren.

In deze sectie geen exemplaar maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn en kunnen worden geopend met alleen een URL. De container wordt aangeroepen **quickstartblobs**. 

In dit voorbeeld wordt **CreateIfNotExists** omdat we willen maken van een nieuwe container telkens wanneer de steekproef wordt uitgevoerd. In een productieomgeving waarbij u gebruikmaakt van de container in een toepassing, is het beter idee om alleen aanroepen **CreateIfNotExists** zodra. U kunt ook de container tevoren maken, dus u hoeft niet in de code te maken.

```java
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
        
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

// Get a reference to a container.
// The container name must be lower case
CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

// Create the container if it does not exist.
container.createIfNotExists();

// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-blobs-to-the-container"></a>BLOB's uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs zijn het meest gebruikt, en dat is wat wordt gebruikt in deze snelstartgids. 

Als u wilt uploaden naar een blob, geen verwijzing ophalen naar de blob in de doelcontainer. Zodra u de blobverwijzing hebt, kunt u gegevens uploaden naar deze met behulp van [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of deze wordt overschreven als deze al bestaat.

De voorbeeldcode maakt een lokaal bestand moet worden gebruikt voor het uploaden en downloaden, het opslaan van het bestand moet worden geüpload als **bron** en de naam van de blob in **blob**. Het volgende voorbeeld wordt het bestand geüpload naar uw zogenaamd **quickstartblobs**.

```java
//Getting the path to user's myDocuments folder
String myDocs = FileSystemView.getFileSystemView().getDefaultDirectory().getPath();

//Creating a file in the myDocuments folder
File source = new File(myDocs + File.separator + "results.txt");
try( Writer output = new BufferedWriter(new FileWriter(source)))
{
    System.out.println("Location of file: " + source.toString());

    output.write("Hello Azure!");
    output.close();
    
    System.out.println("File has been written");
}
catch(IOException x) 
{
    System.err.println(x);
}

//Getting blob reference
CloudBlockBlob blob = container.getBlockBlobReference("results.txt");

//Creating a FileInputStream as it is necessary for the upload
FileInputStream myFile = new FileInputStream(source);

//Creating blob and uploading file to it
blob.upload( myFile, source.length());

//Closing FileInputStream
myFile.close();
```

Er zijn verschillende methoden voor het uploaden die u met Blob storage gebruiken kunt. Bijvoorbeeld, als u een geheugenstream hebt, kunt u de methode UploadFromStreamAsync in plaats van de UploadFromFileAsync. 

Blok-blobs kunnen elk type tekst of binair bestand zijn. Pagina-blobs zijn voornamelijk gebruikt voor de VHD-bestanden gebruikt voor back-IaaS VM's. Toevoeg-BLOB's worden gebruikt voor logboekregistratie, zoals indien u wenst te schrijven naar een bestand en klik vervolgens houden meer gegevens toe te voegen. De meeste objecten die zijn opgeslagen in Blob storage zijn blok-blobs.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

U krijgt een lijst met bestanden in de container met behulp [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). De volgende code haalt de lijst met blobs en doorlopen, met de URI van de blobs. U kunt de URI in het opdrachtvenster kopieert en plakt u deze in een browser om het bestand weer te geven.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

## <a name="download-blobs"></a>Blobs downloaden

Blobs downloaden naar uw lokale schijf met [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

De volgende code downloadt de blob geüpload in een eerder gedeelte een achtervoegsel van '_DOWNLOADED' toevoegen aan de blob-naam, zodat u beide bestanden op lokale schijf kunt zien. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in your default directory.
blob.downloadToFile(myDocs + File.separator + "results_DOWNLOADED.txt");
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs in deze snelstartgids geüpload niet meer nodig hebt, kunt u de volledige container via te verwijderen [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Hiermee verwijdert u ook de bestanden in de container.

```java
//Deletes container if it exists then deletes files created locally
container.deleteIfExists();
downloadedFile.deleteOnExit();
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u bestanden overbrengen tussen een lokale schijf en Azure Blob storage met Java. Voor meer informatie over het werken met Blob storage, blijven de instructies van de Blob-opslag.

> [!div class="nextstepaction"]
> [Procedures voor de bewerkingen van de BLOB-opslag](storage-java-how-to-use-blob-storage.md)

Zie voor meer informatie over de Storage Explorer en Blobs [beheren Azure Blob storage-resources met Opslagverkenner](../../vs-azure-tools-storage-explorer-blobs.md).
---
title: 'Azure-snelstart: Een blob maken in objectopslag via de Java Storage SDK V10 | Microsoft Docs'
description: In deze snelstart gaat u een container maken in objectopslag (Blob), een bestand uploaden, objecten vermelden en downloaden met behulp van de Java Storage SDK.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: rogarana
ms.openlocfilehash: a789269e73e1817f6a45e1e5948dbfaa21efd283
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704467"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-java-sdk-v10-preview"></a>Snelstart: Blobs uploaden, downloaden en vermelden met behulp van de Java SDK V10 (preview)

In deze snelstart leert u hoe u de nieuwe Java Storage SDK kunt gebruiken om blok-bobs te uploaden, te downloaden en weer te geven in een container in Azure Blob-opslag. De nieuwe Java SDK maakt gebruik van het reactieve programmeermodel met RxJava om u asynchrone bewerkingen te bieden. Krijg [hier](https://github.com/ReactiveX/RxJava) meer informatie over RxJava. 

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart:

* U kunt [Maven](http://maven.apache.org/download.cgi) installeren en configureren voor gebruik vanaf de opdrachtregel, of elke gewenste Java IDE
* Installeer en configureer een [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) die in deze quickstart wordt gebruikt, is een eenvoudige consoletoepassing. 

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond.

Zodra het importeren in het project is voltooid, opent u **Quickstart.java** (locatie: **src/main/java/quickstart**).

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren
Deze oplossing vereist dat uw opslagaccountnaam en -sleutel veilig zijn opgeslagen in omgevingsvariabelen die lokaal op de computer staan waarop u het voorbeeld uitvoert. Volg afhankelijk van uw besturingssysteem een van de onderstaande voorbeelden voor het maken van de omgevingsvariabelen.

### <a name="for-linux"></a>Voor Linux

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="for-windows"></a>Voor Windows

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met dit voorbeeld wordt er een testbestand gemaakt in de standaardmap (AppData\Local\Temp voor Windows-gebruikers). Vervolgens wordt u gevraagd om opdrachten in te voeren voor het uploaden van het testbestand naar Blob-opslag, worden de blobs vermeld in de container en wordt het bestand gedownload met een nieuwe naam, zodat u het oude en het nieuwe bestand met elkaar kunt vergelijken. 

Als u het voorbeeld met behulp van Maven wilt uitvoeren op de opdrachtregel, opent u een shell en navigeert u naar **storage-blobs-java-v10-quickstart** binnen de gekloonde map. Voer vervolgens `mvn compile exec:java` uit.

Hieronder ziet u een voorbeeld van uitvoer als u de toepassing zou uitvoeren in Windows.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

U beheert het voorbeeld. Voer daarom opdrachten in om de code te laten uitvoeren. Let op: invoer is hoofdlettergevoelig.

U kunt ook een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gebruiken om de bestanden in de Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang hebt tot de gegevens van uw opslagaccount. 

Nadat u de bestanden hebt gecontroleerd, **E** en drukt u op Enter om de demo te voltooien en de testbestanden te verwijderen. Nu u weet wat het voorbeeld doet, opent u het bestand **Quickstart.java** om de code te bekijken. 

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

We gaan nu de voorbeeldcode bespreken, zodat u begrijpt hoe deze werkt.

### <a name="get-references-to-the-storage-objects"></a>Verwijzingen naar de opslagobjecten ophalen

Als eerste moeten verwijzingen worden gemaakt naar objecten die worden gebruikt voor het verkrijgen van toegang tot de Blob-opslag en voor het beheren ervan. Deze objecten zijn afhankelijk van elkaar en elk object vormt de basis voor het volgende object in de lijst.

* Maak een exemplaar van het object StorageURL dat naar het opslagaccount wijst.

    Het object [**StorageURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-preview) is een weergave van het opslagaccount en stelt u in staat om een nieuwe pijplijn te genereren. Een pijplijn ([Http-pijplijn](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline)) is een set beleidsregels die worden gebruikt om aanvragen en antwoorden te bewerken met mechanismen voor autorisatie, logboekregistratie en opnieuw uitvoeren. U kunt met de pijplijn een exemplaar van het object [**ServiceURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-preview) maken, wat u in staat stelt om een exemplaar van de [**ContainerURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview) te maken. Dit laatste is nodig om bewerkingen uit te voeren in Blob-containers.

* Maak een exemplaar van het object ContainerURL, dat de container voorstelt waartoe u toegang wilt. Containers worden gebruikt om uw blobs te organiseren op een wijze die lijkt op het gebruik van mappen op uw computer om uw bestanden te organiseren.

    De **ContainerURL** biedt u een toegangspunt tot de containerservice. Met behulp van de **ContainerURL** kunt u een exemplaar van de [**BlobURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview) maken, wat nodig is voor het maken van blobs.

* Maak een exemplaar van het object [BlobURL](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-preview), dat verwijst naar een specifieke blob waarin u geïnteresseerd bent.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de namen van containers en blobs.

### <a name="create-a-container"></a>Een container maken 

In deze sectie maakt u een exemplaar van de ContainerURL en maakt u hiermee een nieuwe container. De container in het voorbeeld heet **quickstart**. 

In dit voorbeeld wordt [containerURL.create](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-preview) gebruikt omdat we bij elke nieuwe uitvoering van het voorbeeld een nieuwe container willen maken. U kunt de container ook vooraf maken, zodat u dit niet in de code hoeft te doen.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs worden het meest gebruikt, en dat is wat er wordt gebruikt in deze quickstart. 

Als u een bestand naar een blob wilt uploaden, haalt u een verwijzing naar de blob op in de doelcontainer. Zodra u beschikt over een blobverwijzing, kunt u een bestand naar deze blob uploaden. Dit doet u met behulp van de API’s op laag niveau, zoals [Upload](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-preview) (ook wel PutBlob) genoemd, [StageBlock](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-preview#com_microsoft_azure_storage_blob__block_blob_u_r_l_stageBlock_String_Flowable_ByteBuffer__long_LeaseAccessConditions_) (ook wel PutBLock genoemd) in het geval van BlockBlobURL, of de API’s op hoog niveau die worden geboden in de klasse [TransferManager](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-preview), zoals de methode [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-preview). Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als deze wel al bestaat.

Met de voorbeeldcode wordt een lokaal bestand gemaakt dat kan worden gebruikt voor uploaden en downloaden, waarbij het bestand dat moet worden geüpload, wordt opgeslagen als **sourceFile** en de naam wordt opgeslagen in **blob**. In het volgende voorbeeld wordt het bestand geüpload naar de container met de naam **quickstart**.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {
    
    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());
            
    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Blok-bobs kunnen bestaan uit elk type tekstbestand of binair bestand. Pagina-blobs worden hoofdzakelijk gebruikt voor de VHD-bestanden die worden gebruikt als back-up voor IaaS-VM‘s. Toevoeg-blobs zijn bedoeld om gegevens toe te voegen aan het eind en worden vaak gebruikt voor aanmelden. De meeste objecten die worden opgeslagen in Blob-opslag, zijn blok-blobs.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Met behulp van [containerURL.listBlobsFlatSegment](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-preview) kunt u een lijst met objecten in een container ophalen. Met deze methode worden maximaal 5000 objecten tegelijk geretourneerd samen met een vervolgmarkering (markering Volgende) als de container meer objecten bevat om weer te geven. Hiervoor maken we een Helper-functie die zichzelf herhaaldelijk aanroept, zolang er een markering Volgende is in het vorige listBlobsFlatSegment-antwoord.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs, 
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse -> 
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))    
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {                
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }
    
    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */
            
        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */
            
    return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Blobs downloaden

Download blobs naar de lokale schijf met [blobURL.download](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-preview).

Met de volgende code wordt de blob gedownload die in een eerdere sectie is geüpload, waarbij het achtervoegsel '_DOWNLOADED' wordt toegevoegd aan de blobnaam, zodat u beide bestanden op de lokale schijf kunt zien. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs die in deze snelstart zijn geüpload, niet meer nodig hebt, kunt u de hele container verwijderen met behulp van [containerURL.delete](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-preview). Met deze methode worden ook de bestanden in de container verwijderd.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag met behulp van Java. 

> [!div class="nextstepaction"]
> [Storage SDK V10 voor Java-broncode](https://github.com/Azure/azure-storage-java/tree/New-Storage-SDK-V10-Preview)
> [API-verwijzing](https://docs.microsoft.com/en-us/java/api/storage/client?view=azure-java-preview)
> [Meer informatie over RxJava](https://github.com/ReactiveX/RxJava)
---
title: Azure-quickstart - Blobs uploaden, downloaden en vermelden in Azure Storage met behulp van Go | Microsoft Docs
description: In deze quickstart maakt u een opslagaccount en een container. Vervolgens gebruikt u de opslagclientbibliotheek voor Go om een blob in Azure Storage te uploaden, een blob te downloaden en de blobs in een container te vermelden.
services: storage
author: seguler
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 02/22/2018
ms.author: seguler
ms.openlocfilehash: 5572eaafa49200be0b2a8156dbc96bffd48be925
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-go"></a>Quickstart: blobs downloaden, uploaden en vermelden met behulp van Go

In deze quickstart leert u hoe u de programmeertaal Go kunt gebruiken om blok-bobs te uploaden, te downloaden en weer te geven in een container in Azure Blob-opslag. 

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart: 
* Installeer [Go 1.8 of hoger](https://golang.org/dl/)
* Download en installeer [Azure Storage Blob-SDK voor Go](https://github.com/azure/azure-storage-blob-go/) met behulp van `go get -u github.com/azure/azure-storage-blob-go/2016-05-31/azblob`. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden
De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-go-quickstart.git) die in deze quickstart wordt gebruikt, is een Go-basistoepassing.  

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-go-quickstart 
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Om het Go-voorbeeld voor Blob-opslag te openen, zoekt u het bestand storage-quickstart.go op.  

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren
Deze oplossing vereist dat uw opslagaccountnaam en -sleutel veilig zijn opgeslagen in omgevingsvariabelen die lokaal op de computer staan waarop u het voorbeeld uitvoert. Volg afhankelijk van uw besturingssysteem een van de onderstaande voorbeelden voor het maken van de omgevingsvariabelen.

# <a name="linuxtablinux"></a>[Linux](#tab/Linux)

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

# <a name="windowstabwindows"></a>[Windows](#tab/Windows)

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren
Met dit voorbeeld wordt een testbestand gemaakt in de huidige map, het testbestand geüpload naar Blob-opslag, worden de blobs in the container vermeld en wordt het bestand met een nieuwe naam gedownload naar een buffer. 

Geef de volgende opdracht om het voorbeeld uit te voeren: 

```go run storage-quickstart.go```

De volgende uitvoer is een voorbeeld van de uitvoer die wordt geretourneerd wanneer de toepassing wordt uitgevoerd:
  
```
Azure Blob storage quick start sample
Creating a container named quickstart-5568059279520899415
Creating a dummy file to test the upload and download
Uploading the file with blob name: 630910657703031215
Blob name: 630910657703031215
Downloaded the blob: hello world
this is a blob
Press the enter key to delete the sample files, example container, and exit the application.
```
Wanneer u op een willekeurige toets drukt om door te gaan, worden de opslagcontainer en de bestanden door het voorbeeldprogramma verwijderd. 

> [!TIP]
> U kunt ook een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com) gebruiken om de bestanden in de Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang hebt tot de gegevens van uw opslagaccount. 
>

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

We gaan nu de voorbeeldcode bespreken, zodat u begrijpt hoe deze werkt.

### <a name="create-containerurl-and-bloburl-objects"></a>ContainerURL- en BlobURL-objecten maken
Als eerste moeten verwijzingen worden gemaakt naar de ContainerURL- en BlobURL-objecten die worden gebruikt voor het verkrijgen van toegang tot de Blob-opslag en voor het beheren ervan. Deze objecten bieden API's op laag niveau zoals Create, PutBlob en GetBlob om REST-API's uit te geven.

* Gebruik de struct **SharedKeyCredential** voor het opslaan van uw referenties. 

* Maak een **pijplijn** met behulp van de referenties en opties. De pijplijn bevat items zoals het beleid voor nieuwe pogingen, logboekregistratie en deserialisatie van payloads voor HTTP-antwoorden.  

* Maak een nieuw ContainerURL- en een nieuw BlobURL-object om bewerkingen op de container (Maken) en blobs (PutBlob en GetBlob) uit te voeren.


Zodra u de ContainerURL hebt, kunt u het **BlobURL**-object maken dat verwijst naar een blob en bewerkingen uitvoeren zoals uploaden, downloaden en kopiëren.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de namen van containers en blobs.

In deze sectie maakt u een nieuwe container. De container heeft de naam **quickstartblobs-[random string]**. 

```go 
// From the Azure portal, get your storage account name and key and set environment variables.
accountName, accountKey := os.Getenv("AZURE_STORAGE_ACCOUNT"), os.Getenv("AZURE_STORAGE_ACCESS_KEY")
if len(accountName) == 0 || len(accountKey) == 0 {
    log.Fatal("Either the AZURE_STORAGE_ACCOUNT or AZURE_STORAGE_ACCESS_KEY environment variable is not set")
}

// Create a default request pipeline using your storage account name and account key.
credential := azblob.NewSharedKeyCredential(accountName, accountKey)
p := azblob.NewPipeline(credential, azblob.PipelineOptions{})

// Create a random string for the quick start container
containerName := fmt.Sprintf("quickstart-%s", randomString())

// From the Azure portal, get your storage account blob service URL endpoint.
URL, _ := url.Parse(
    fmt.Sprintf("https://%s.blob.core.windows.net/%s", accountName, containerName))

// Create a ContainerURL object that wraps the container URL and a request
// pipeline to make requests.
containerURL := azblob.NewContainerURL(*URL, p)

// Create the container
fmt.Printf("Creating a container named %s\n", containerName)
ctx := context.Background() // This example uses a never-expiring context
_, err := containerURL.Create(ctx, azblob.Metadata{}, azblob.PublicAccessNone)
handleErrors(err)
```
### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Omdat blok-blobs het meest worden gebruikt, worden ze in deze Quick Start gebruikt.  

Als u een bestand wilt uploaden naar een blob, opent u het bestand met **os. Open**. U kunt het bestand vervolgens naar het opgegeven pad uploaden met een van de REST-API's: PutBlob, PutBlock/PutBlockList. 

De SDK biedt [API's op hoog niveau](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go) die zijn gebaseerd op de REST-API's op laag niveau. De functie ***UploadFileToBlockBlob*** maakt bijvoorbeeld gebruik van PutBlock-bewerkingen om gelijktijdig een bestand in segmenten te uploaden om de doorvoer te optimaliseren. Als het bestand minder dan 256 MB is, wordt PutBlob gebruikt om de overdracht in één transactie te voltooien.

In het volgende voorbeeld wordt het bestand geüpload naar de container met de naam **quickstartblobs-[randomstring]**.

```go
// Here's how to upload a blob.
blobURL := containerURL.NewBlockBlobURL(fileName)
file, err := os.Open(fileName)
handleErrors(err)

// You can use the low-level PutBlob API to upload files. Low-level APIs are simple wrappers for the Azure Storage REST APIs.
// Note that PutBlob can upload up to 256MB data in one shot. Details: https://docs.microsoft.com/en-us/rest/api/storageservices/put-blob
// Following is commented out intentionally because we will instead use UploadFileToBlockBlob API to upload the blob
// _, err = blobURL.PutBlob(ctx, file, azblob.BlobHTTPHeaders{}, azblob.Metadata{}, azblob.BlobAccessConditions{})
// handleErrors(err)

// The high-level API UploadFileToBlockBlob function uploads blocks in parallel for optimal performance, and can handle large files as well.
// This function calls PutBlock/PutBlockList for files larger 256 MBs, and calls PutBlob for any file smaller
fmt.Printf("Uploading the file with blob name: %s\n", fileName)
_, err = azblob.UploadFileToBlockBlob(ctx, file, blobURL, azblob.UploadToBlockBlobOptions{
    BlockSize:   4 * 1024 * 1024,
    Parallelism: 16})
handleErrors(err)
```

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Haal een lijst met bestanden in de container op met behulp van de methode **ListBlobs** in een **ContainerURL**. ListBlobs retourneert een enkel segment met blobs (maximaal 5000) vanaf de opgegeven **markering**. Gebruik een lege markering om de opsomming vanaf het begin starten. Blob-namen worden in lexografische volgorde geretourneerd. Nadat u een segment hebt opgehaald, verwerkt u het en roept u vervolgens ListBlobs opnieuw aan waarmee de eerder geretourneerde markering wordt doorgegeven.  

```go
// List the blobs in the container
for marker := (azblob.Marker{}); marker.NotDone(); {
    // Get a result segment starting with the blob indicated by the current Marker.
    listBlob, err := containerURL.ListBlobs(ctx, marker, azblob.ListBlobsOptions{})
    handleErrors(err)

    // ListBlobs returns the start of the next segment; you MUST use this to get
    // the next segment (after processing the current result segment).
    marker = listBlob.NextMarker

    // Process the blobs returned in this result segment (if the segment is empty, the loop body won't execute)
    for _, blobInfo := range listBlob.Blobs.Blob {
        fmt.Print("Blob name: " + blobInfo.Name + "\n")
    }
}
```

### <a name="download-the-blob"></a>De blob downloaden

Download blobs met de **GetBlob**-methode op laag niveau in een BlobURL. U kunt ook een stream maken en er bereiken uit lezen met behulp van de **NewDownloadStream**-API op hoog niveau die is opgegeven in [highlevel.go](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go). De functie NewDownloadStream doet een nieuwe poging in het geval van een verbindingsfout, terwijl de API 'Blob ophalen' alleen een nieuwe poging doet bij HTTP-statuscodes zoals 503 (Server bezet). Met de volgende code wordt de blob gedownload met behulp van de functie **NewDownloadStream**. De inhoud van de blob wordt naar een buffer geschreven en weergegeven in de console.

```go
// Here's how to download the blob. NOTE: This method automatically retries if the connection fails
// during download (the low-level GetBlob function does NOT retry errors when reading from its stream).
stream := azblob.NewDownloadStream(ctx, blobURL.GetBlob, azblob.DownloadStreamOptions{})
downloadedData := &bytes.Buffer{}
_, err = downloadedData.ReadFrom(stream)
handleErrors(err)
```

### <a name="clean-up-resources"></a>Resources opschonen
Als u de blobs die in deze quickstart zijn geüpload niet langer nodig hebt, kunt u de volledige container verwijderen met behulp van de methode **Delete**. 

```go
// Cleaning up the quick start by deleting the container and the file created locally
fmt.Printf("Press the enter key to delete the sample files, example container, and exit the application.\n")
bufio.NewReader(os.Stdin).ReadBytes('\n')
fmt.Printf("Cleaning up.\n")
containerURL.Delete(ctx, azblob.ContainerAccessConditions{})
file.Close()
os.Remove(fileName)
```

## <a name="resources-for-developing-go-applications-with-blobs"></a>Resources voor het ontwikkelen van Go-toepassingen met blobs

Zie de volgende aanvullende bronnen voor Go-ontwikkeling met Blob-opslag:

- Bekijk en installeer de [broncode voor de Go-clientbibliotheek](https://github.com/Azure/azure-storage-blob-go) voor Azure Storage op GitHub.
- Verken [Blob-opslagvoorbeelden](https://godoc.org/github.com/Azure/azure-storage-blob-go/2016-05-31/azblob#pkg-examples) die zijn geschreven met de Go-clientbibliotheek.

## <a name="next-steps"></a>Volgende stappen
 
In deze quickstart hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag met behulp van Go. Raadpleeg de [broncode](https://github.com/Azure/azure-storage-blob-go/) en [API-verwijzing](https://godoc.org/github.com/Azure/azure-storage-blob-go/2016-05-31/azblob) voor meer informatie over de SDK van Azure Storage Blob.

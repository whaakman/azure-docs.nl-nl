---
title: Azure Quickstart - objecten voor overdracht naar/van Azure Blob storage met Node.js| Microsoft Docs
description: Snel informatie over het overdragen van objecten uit Azure Blob storage met Node.js
services: storage
documentationcenter: storage
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: gwallace
ms.openlocfilehash: dd4d3abf082767c40760d020c0997b365452e769
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Objecten voor overdracht naar/van Azure Blob storage met Node.js

In deze snelstartgids leert u hoe u met Node.js uploaden, downloaden en weergeven van blok-blobs in een container in Azure Blob-opslag.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart:

* [Node.js](https://nodejs.org/en/) installeren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) gebruikt in deze snelstartgids is een eenvoudige consoletoepassing. 

Gebruik [git](https://git-scm.com/) een kopie van de toepassing op uw ontwikkelomgeving te downloaden.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Met deze opdracht kloont de opslagplaats naar uw lokale git-map. Als u wilt het uiterlijk van de toepassing voor de opslag-blobs-knooppunt-Quick Start-map openen, opent u het en dubbelklik op index.js.

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

In de toepassing, moet u de verbindingsreeks opgeven voor uw opslagaccount. Open de `index.js` bestand, zoek de `connectionString` variabele. De waarde ervan vervangen door de volledige waarde van de verbindingsreeks met de categorie die u hebt genoteerd in de Azure-portal. De opslagverbindingsreeks moet er ongeveer als volgt uitzien:

```javascript
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Installeert de vereiste pakketten

In de toepassingsmap uitvoeren `npm install` om de installatie van een vereiste pakketten die worden vermeld in de `package.json` bestand.

```javascript
npm install
```

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

Dit voorbeeld maakt een testbestand in Mijn documenten, geüpload naar Blob storage, geeft een lijst van de blobs in de container en downloadt het bestand met een nieuwe naam zodat u de oude en nieuwe bestanden kunt vergelijken.

Het voorbeeld uitvoert door te typen `node index.js`. De volgende uitvoer is van een Windows-systeem.  Een vergelijkbare uitvoer met de juiste bestandspaden kan worden verwacht als met Linux.

```
Azure Storage Node.js Client Library Blobs Quick Start

1. Creating a container with public access: quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d

2. Creating a file in ~/Documents folder to test the upload and download

   Local File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

3. Uploading BlockBlob: quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

   Uploaded Blob URL: https://mystorageaccount.blob.core.windows.net/quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d/quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

4. Listing blobs in container

   - quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt (type: BlockBlob)


5. Downloading blob

   Downloaded File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d_DOWNLOADED.txt

Sample finished running. When you hit <ENTER> key, the temporary files will be deleted and the sample application will exit.
```

Controleer voordat u doorgaat, Mijn documenten voor de twee bestanden. U kunt deze openen en ze identiek zijn.

U kunt ook een hulpprogramma zoals gebruiken de [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) de bestanden in Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang tot de gegevens van uw storage-account.

Nadat u de bestanden hebt gecontroleerd, druk op een willekeurige toets om de demo voltooien en verwijderen van de testbestanden. Nu dat u wat het voorbeeld doet weet, open het bestand index.js om te kijken naar de code. 

## <a name="understand-the-sample-code"></a>Inzicht in de voorbeeldcode

Daarna bekijken we de voorbeeldcode zodat u kunt begrijpen hoe het werkt.

### <a name="get-references-to-the-storage-objects"></a>Het ophalen van verwijzingen naar de opslagobjecten

Het eerste wat te doen is het maken van de verwijzing naar de `BlobService` gebruikt voor toegang tot en beheren van Blob-opslag. Deze objecten bouwen op elkaar--elk wordt gebruikt door de volgende gateway in de lijst.

* Maak een instantie van de  **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)**  -object, dat naar de Blob-service in uw opslagaccount verwijst.

* Maak een nieuwe container en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn en kunnen worden geopend met alleen een URL. De container begint met **quickstartcontainer -**.

In dit voorbeeld wordt [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) omdat we willen maken van een nieuwe container telkens wanneer de steekproef wordt uitgevoerd. Het is beter om aan te roepen CreateIfNotExists slechts één keer in een productie-omgeving waarin u de container in een toepassing gebruiken. U kunt ook de container tevoren maken, dus u hoeft niet in de code te maken.

```javascript
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

### <a name="upload-blobs-to-the-container"></a>BLOB's uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs zijn het meest gebruikt. Ze zijn ideaal voor het opslaan van tekst en binaire gegevens, dit is de reden dat ze worden gebruikt in deze snelstartgids.

Als u wilt uploaden naar een blob, gebruikt u de [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) methode. Deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of deze wordt overschreven als deze al bestaat.

De voorbeeldcode maakt een lokaal bestand moet worden gebruikt voor het uploaden en downloaden, het opslaan van het bestand moet worden geüpload als **localPath** en de naam van de blob in **localFileToUpload**. Het volgende voorbeeld wordt het bestand geüpload naar de container die beginnen met **quickstartcontainer -**.

```javascript
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Er zijn verschillende methoden voor het uploaden die u met Blob storage gebruiken kunt. Bijvoorbeeld, als u een geheugenstream hebt, kunt u de [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) methode plaats [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Vervolgens wordt de toepassing krijgt voor een lijst met bestanden in de container met behulp [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). De volgende code haalt de lijst met blobs en doorlopen, met de URI van de blobs. U kunt de URI in het opdrachtvenster kopieert en plakt u deze in een browser om het bestand weer te geven.

Als u 5.000 of minder blobs in de container hebt, alle van de blob-namen zijn opgehaald in één aanroep van [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Als u meer dan 5000 blobs in de container hebt, haalt de service de lijst in sets van 5.000 totdat alle van de blob-namen zijn opgehaald. Zodat de eerste keer dat deze API wordt aangeroepen, wordt de eerste 5000 blobnamen en een vervolgtoken. De tweede keer opgeven van het token, haalt de service de volgende set van blob-namen en enzovoort, totdat het vervolgtoken is null, waarmee wordt aangegeven dat alle van de blob-namen zijn opgehaald.

```javascript
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

### <a name="download-blobs"></a>Blobs downloaden

Blobs downloaden naar uw lokale schijf met [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

De volgende code downloadt de blob geüpload in een eerder gedeelte een achtervoegsel van '_DOWNLOADED' toevoegen aan de blob-naam, zodat u beide bestanden op lokale schijf kunt zien. 

```javascript
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

### <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs in deze snelstartgids geüpload niet meer nodig hebt, kunt u de volledige container via te verwijderen [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) en [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). De bestanden die zijn gemaakt als ze niet meer nodig zijn ook verwijderd. Dit is afgehandeld in de toepassing wanneer u op enter om af te sluiten van de toepassing.

```javascript
console.log('6. Deleting block Blob\n');
    blobService.deleteBlobIfExists(CONTAINER_NAME, BLOCK_BLOB_NAME, function (error) {
        handleError(error);

    console.log('7. Deleting container\n');
    blobService.deleteContainerIfExists(CONTAINER_NAME, function (error) {
        handleError(error);
            
        fs.unlinkSync(LOCAL_FILE_PATH);
        fs.unlinkSync(DOWNLOADED_FILE_PATH);
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u bestanden overbrengen tussen een lokale schijf en Azure Blob storage met Node.js. Voor meer informatie over het werken met Blob storage, blijven de instructies van de Blob-opslag.

> [!div class="nextstepaction"]
> [Procedures voor de bewerkingen van de BLOB-opslag](storage-nodejs-how-to-use-blob-storage.md)

Zie voor meer informatie over de Storage Explorer en Blobs [beheren Azure Blob storage-resources met Opslagverkenner](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

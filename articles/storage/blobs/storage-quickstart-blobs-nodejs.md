---
title: Azure Quick Start - Objecten overdragen naar/van Azure Blob-opslag met Node.js | Microsoft Docs
description: Snel leren om objecten over te dragen naar/van Azure Blob-opslag met Node.js
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
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Objecten overdragen naar/van Azure Blob-opslag met Node.js

In deze zelfstudie leert u hoe u Node.js kunt gebruiken om blok-bobs te uploaden, te downloaden en weer te geven in een container in Azure Blob-opslag.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze quickstart:

* [Node.js](https://nodejs.org/en/) installeren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) die in deze Quick Start wordt gebruikt, is een basisconsoletoepassing. 

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. Als u wilt de toepassing wilt gaan gebruiken, gaat u naar de map storage-blobs-node-quickstart, opent u de toepassing en dubbelklikt u op index.js.

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

In de toepassing moet u de verbindingsreeks voor uw opslagaccount opgeven. Open het bestand `index.js` en zoek naar de variabele `connectionString`. Vervang de gehele waarde van de verbindingsreeks door de reeks die u uit Azure Portal hebt opgeslagen. De opslagverbindingsreeks moet er ongeveer als volgt uitzien:

```javascript
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>De vereiste pakketten installeren

Voer in de map van de toepassing `npm install` uit om alle vereiste pakketten die worden vermeld in het bestand `package.json` te installeren.

```javascript
npm install
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Met dit voorbeeld wordt een testbestand gemaakt in Mijn documenten, wordt dit bestand geüpload naar Blob-opslag, worden de blobs vermeld in de container en wordt het bestand vervolgens met een nieuwe naam gedownload, zodat u het oude en het nieuwe bestand met elkaar kunt vergelijken.

Voer het voorbeeld uit door `node index.js` te typen. De volgende uitvoer is van een Windows-systeem.  Een vergelijkbare uitvoer met de juiste bestandspaden kan worden verwacht als Linux wordt gebruikt.

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

Voordat u doorgaat moet u controleren of de map 'Mijn documenten' de twee bestanden bevat. Als u ze opent, ziet u dat ze identiek zijn.

U kunt ook een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gebruiken om de bestanden in de Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang hebt tot de gegevens van uw opslagaccount.

Nadat u de bestanden hebt gecontroleerd, drukt u op een willekeurige toets om de demo te voltooien en de testbestanden te verwijderen. Nu u weet wat het voorbeeld doet, opent u het bestand index.js om de code te bekijken. 

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

Nu lopen we door de voorbeeldcode zodat u begrijpt hoe deze werkt.

### <a name="get-references-to-the-storage-objects"></a>Verwijzingen naar de opslagobjecten ophalen

Als eerste moet de verwijzing worden gemaakt naar de `BlobService` die wordt gebruikt voor het verkrijgen van toegang tot de Blob-opslag en voor het beheren ervan. Deze objecten worden boven op elkaar gebouwd en elk ervan wordt door de volgende in de lijst gebruikt.

* Maak een exemplaar van het object **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)** dat naar de Blob-service in uw opslagaccount verwijst.

* Maak een nieuwe container en stel vervolgens machtigingen voor de container in, zodat de blobs openbaar zijn en via slechts een URL kunnen worden geopend. De container begint met **quickstartcontainer -**.

In dit voorbeeld wordt gebruikgemaakt van [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) omdat we telkens als het voorbeeld wordt uitgevoerd een nieuwe container willen maken. In een productieomgeving waarin u overal in een toepassing dezelfde container gebruikt, kunt u CreateIfNotExists het beste maar één keer aanroepen. U kunt de container ook vooraf maken, zodat u deze niet in de code hoeft te maken.

```javascript
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs worden het meest gebruikt. Ze zijn ideaal voor het opslaan van tekst en binaire gegevens. Dat is de reden waarom ze worden gebruikt in deze Quick Start.

Gebruik de methode [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) om een bestand naar een blob te uploaden. Met deze bewerking wordt de blob gemaakt als deze nog niet bestaat, of overschreven als deze wel al bestaat.

Met de voorbeeldcode wordt een lokaal bestand gemaakt voor het uploaden en downloaden, waarmee het bestand dat moet worden geüpload als **localPath** en de naam van de blob in **localFileToUpload** worden opgeslagen. In het volgende voorbeeld wordt het bestand geüpload naar uw container met de naam **quickstartcontainer-**.

```javascript
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Er zijn meerdere uploadmethoden die u kunt gebruiken met Blob-opslag. Als u bijvoorbeeld een geheugenstroom hebt, kunt u de methode [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) gebruiken in plaats van [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Vervolgens ontvangt de toepassing een lijst met bestanden in de container met behulp van [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Met de volgende code wordt de lijst met blobs opgehaald en doorlopen, waarbij de URL‘s van de gevonden blobs worden weergegeven. U kunt de URL uit het opdrachtvenster kopiëren en in een browser plakken om het bestand weer te geven.

Als u 5000 of minder blobs in de container hebt, worden alle blobnamen in één aanroep naar [ListBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented) opgehaald. Als u meer dan 5000 blobs in de container hebt, wordt de lijst door de service opgehaald in sets van 5000 totdat alle blobnamen zijn opgehaald. De eerste keer dat deze API wordt aangeroepen, worden dus de eerste 5000 namen en een vervolgtoken geretourneerd. De tweede keer geeft u het token op en haalt de service de volgende set blobnamen op, enzovoort, totdat het vervolgtoken null is, wat aangeeft dat alle blobnamen zijn opgehaald.

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

Download blobs naar uw lokale schijf met [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

Met de volgende code wordt de blob gedownload die in een eerdere sectie is geüpload, waarbij de suffix '_DOWNLOADED' wordt toegevoegd aan de blobnaam, zodat u beide bestanden op de lokale schijf kunt zien. 

```javascript
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

### <a name="clean-up-resources"></a>Resources opschonen

Als u de blobs die in deze Quick Start zijn geüpload niet langer nodig hebt, kunt u de volledige container verwijderen met behulp van [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) en [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). Verwijder ook de gemaakte bestanden als u deze niet langer nodig hebt. Dit wordt afgehandeld in de toepassing wanneer u op Enter drukt om de toepassing af te sluiten.

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

In deze Quick Start hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag met behulp van Node.js. Voor meer informatie over het werken met Blob-opslag, gaat u naar de instructies voor Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](storage-nodejs-how-to-use-blob-storage.md)

Zie [Azure Blob-opslagresources beheren met Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over de Storage Explorer en blobs.

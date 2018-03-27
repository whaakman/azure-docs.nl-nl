---
title: 'Azure-snelstart: blobs uploaden, downloaden en vermelden in Azure Storage met behulp van Node.js | Microsoft Docs'
description: In deze snelstart maakt u een opslagaccount en een container. Vervolgens gebruikt u de opslagclientbibliotheek voor Node.js om een blob in Azure Storage te uploaden, een blob te downloaden en de blobs in een container te vermelden.
services: storage
author: craigshoemaker
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 03/15/2018
ms.author: cshoe
ms.openlocfilehash: 28f9936c297b6f641810e0c7783f4d84be108286
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-nodejs"></a>Snelstart: blobs downloaden, uploaden en vermelden met behulp van Node.js

In deze zelfstudie leert u hoe u Node.js kunt gebruiken om blok-bobs te uploaden, te downloaden en weer te geven in een container die Azure Blob Storage gebruikt.

U hebt een [Azure-abonnement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) nodig om deze quickstart te voltooien.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) in deze quickstart is een eenvoudige Node.js-consoletoepassing. Kloon om te beginnen de opslagplaats naar uw computer met de volgende opdracht:

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Als u de toepassing wilt openen, gaat u naar de map *storage-blobs-node-quickstart* en opent u deze in uw favoriete omgeving voor het bewerken van code.

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

U moet eerst de verbindingsreeks voor uw opslagaccount opgeven om de toepassing te kunnen uitvoeren. Het voorbeeld van de opslagplaats bevat een bestand met de naam *.env.example*. U kunt de naam van dit bestand wijzigen door de *.example*-extensie te verwijderen, wat resulteert in een bestand met de naam *.env*. In het bestand *.env* voegt u de waarde van de verbindingsreeks toe na de *AZURE_STORAGE_CONNECTION_STRING*-sleutel.

## <a name="install-required-packages"></a>De vereiste pakketten installeren

Voer in de toepassingsmap de opdracht *npm install* uit om de vereiste pakketten voor de toepassing te installeren.

```bash
npm install
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren
Nu de afhankelijkheden zijn geïnstalleerd, kunt u het voorbeeld uitvoeren door opdrachten door te geven aan het script. Als u bijvoorbeeld een blob-container wilt maken, voert u de volgende opdracht uit:

```bash
node index.js --command createContainer
```

Dit zijn de beschikbare opdrachten:

| Opdracht | Beschrijving |
|---------|---------|
|*createContainer* | Hiermee maakt u een container met de naam *test-container* (wordt ook uitgevoerd als de container al bestaat) |
|*upload*          | Hiermee uploadt u het bestand *example.txt* naar de container *test-container* |
|*download*        | Hiermee downloadt u de inhoud van de *example*-blob naar *example.downloaded.txt* |
|*delete*          | Hiermee verwijdert u de *example*-blob |
|*list*            | Hiermee geeft u de inhoud van de container *test-container* weer voor de console |


## <a name="understanding-the-sample-code"></a>De voorbeeldcode begrijpen
Deze voorbeeldcode maakt gebruik van enkele modules die aan het bestandssysteem en de opdrachtregel worden gekoppeld. 

```javascript
if (process.env.NODE_ENV !== 'production') {
    require('dotenv').load();
}
const path = require('path');
const args = require('yargs').argv;
const storage = require('azure-storage');
```

Dit is het doel van de modules: 

- *dotenv* laadt omgevingsvariabelen die zijn gedefinieerd in een bestand met de naam *.env* in de huidige uitvoeringscontext
- *path* is vereist om het absolute pad van het bestand te bepalen voor uploaden naar blob-opslag
- *yargs* genereert een eenvoudige interface voor toegang tot opdrachtregelargumenten
- *azure-storage* is de [Azure Storage SDK](/nodejs/api/azure-storage)-module voor Node.js

Vervolgens wordt een reeks variabelen geïnitialiseerd:

```javascript
const blobService = storage.createBlobService();
const containerName = 'test-container';
const sourceFilePath = path.resolve('./example.txt');
const blobName = path.basename(sourceFilePath, path.extname(sourceFilePath));
```

De variabelen worden ingesteld op de volgende waarden:

- *blobService* wordt ingesteld op een nieuwe instantie van de Azure Blob-service
- *containerName* wordt ingesteld op de naam van de container
- *sourceFilePath* wordt ingesteld op het absolute pad van het te uploaden bestand
- *blobName* wordt gemaakt door de bestandsnaam te nemen en de bestandsextensie te verwijderen

In de volgende implementatie wordt elk van de *blobService*-functies verpakt in een *Promise* voor toegang tot de JavaScript-functie *async* en de *await*-operator voor het stroomlijnen van de callback van de [Azure Storage API](/nodejs/api/azure-storage/blobservice). Wanneer een geslaagde reactie voor elke functie wordt gegenereerd, wordt de promise omgezet met relevante gegevens, samen met een bericht dat specifiek bij die actie hoort.

### <a name="create-a-blob-container"></a>Een blob-container maken

De *createContainer*-functie roept [createContainerIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) aan en stelt het juiste toegangsniveau voor de blob in.

```javascript
const createContainer = () => {
    return new Promise((resolve, reject) => {
        blobService.createContainerIfNotExists(containerName, { publicAccessLevel: 'blob' }, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Container '${containerName}' created` });
            }
        });
    });
};
```

De tweede parameter (*options*) voor **createContainerIfNotExists** accepteert een waarde voor [publicAccessLevel](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists). De waarde *blob* voor *publicAccessLevel* geeft aan dat specifieke blob-gegevens worden weergegeven aan het publiek. Deze instelling is het tegenovergestelde van toegang op *container*niveau, waarmee de inhoud van de container kan worden weergegeven.

Door **createContainerIfNotExists** kan de toepassing de opdracht *createContainer* meerdere keren uitvoeren zonder dat er fouten worden geretourneerd wanneer de container al bestaat. In een productieomgeving hoeft u **createContainerIfNotExists** vaak maar eenmaal aan te roepen wanneer dezelfde container overal in de toepassing wordt gebruikt. In dergelijke gevallen kunt u de container vooraf via de portal of via de Azure CLI maken.

### <a name="upload-a-blob-to-the-container"></a>Een blob uploaden naar de container

De *upload*-functie maakt gebruik van de [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromLocalFile)-functie om een bestand te uploaden en te schrijven of overschrijven vanuit het bestandssysteem naar de blob-opslag. 

```javascript
const upload = () => {
    return new Promise((resolve, reject) => {
        blobService.createBlockBlobFromLocalFile(containerName, blobName, sourceFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Upload of '${blobName}' complete` });
            }
        });
    });
};
```
In de context van de voorbeeldtoepassing wordt het bestand met de naam *example.txt* geüpload naar een blob met de naam *example* binnen een container met de naam *test-container*. Andere methoden voor het uploaden van inhoud naar blobs zijn werken met [tekst](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromText) en [streams](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromStream).

Om te controleren of het bestand is geüpload naar de blob-opslag, kunt u met de [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) de gegevens in uw account bekijken.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

De *list*-functie roept de [listBlobsSegmented](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createBlockBlobFromText)-methode aan om een lijst met blob-metagegevens in een container te retourneren. 

```javascript
const list = () => {
    return new Promise((resolve, reject) => {
        blobService.listBlobsSegmented(containerName, null, (err, data) => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Items in container '${containerName}':`, data: data });
            }
        });
    });
};
```

Het aanroepen van *listBlobsSegmented* retourneert blob-metagegevens als een matrix met [BlobResult](/nodejs/api/azure-storage/blobresult)-instanties. Resultaten worden geretourneerd in batches van 5000 blobs (segmenten). Als er in een container meer dan 5000 blobs zijn, bevatten de resultaten een waarde voor **continuationToken**. Als u volgende segmenten uit de blob-container wilt weergeven, kunt u het vervolgtoken als tweede argument doorgeven aan **listBlobSegmented**.

### <a name="download-a-blob-from-the-container"></a>Een blob downloaden uit de container

De *download*-functie maakt gebruik van [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_getBlobToLocalFile) om de inhoud van de blob te downloaden naar het opgegeven absolute bestandspad.

```javascript
const download = () => {
    const dowloadFilePath = sourceFilePath.replace('.txt', '.downloaded.txt');
    return new Promise((resolve, reject) => {
        blobService.getBlobToLocalFile(containerName, blobName, dowloadFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Download of '${blobName}' complete` });
            }
        });
    });
};
```
De hier weergegeven implementatie verandert het bronbestandspad door *.downloaded.txt* toe te voegen aan de bestandsnaam. In een praktijkscenario kunt u zowel de locatie als de bestandsnaam wijzigen bij het selecteren van een downloadbestemming.

### <a name="delete-blobs-in-the-container"></a>Blobs in de container verwijderen

De *deleteBlock*-functie (als alias van de consoleopdracht *delete*) roept de [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists)-functie aan. Zoals de naam al aangeeft, retourneert deze functie geen fout als de blob al is verwijderd.

```javascript
const deleteBlock = () => {
    return new Promise((resolve, reject) => {
        blobService.deleteBlobIfExists(containerName, blobName, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Block blob '${blobName}' deleted` });
            }
        });
    });
};
```

### <a name="upload-and-list"></a>Uploaden en weergeven

Een van de voordelen van het gebruik van promises is de mogelijkheid om opdrachten aan elkaar te koppelen. De **uploadAndList**-functie laat zien hoe eenvoudig het is om de inhoud van een blob direct na het uploaden van een bestand weer te geven.

```javascript
const uploadAndList = () => {
    return _module.upload().then(_module.list);
};
```

### <a name="calling-code"></a>Code aanroepen

Als u de functies wilt weergeven die bij de opdrachtregel zijn geïmplementeerd, wordt elk van die functies toegewezen aan de letterlijke waarde van een object.

```javascript
const _module = {
    "createContainer": createContainer,
    "upload": upload,
    "download": download,
    "delete": deleteBlock,
    "list": list,
    "uploadAndList": uploadAndList
};
```

Nu *_module* aanwezig is, is elk van de opdrachten beschikbaar vanaf de opdrachtregel.

```javascript
const commandExists = () => exists = !!_module[args.command];
```

Als een bepaalde opdracht niet bestaat, worden de eigenschappen van *_module* weergegeven in de console als help-tekst voor de gebruiker. 

De functie *executeCommand* is een *asynchrone* functie die de gegeven opdracht aanroept met de *await*-operator en alle berichten over gegevens in de console registreert.

```javascript
const executeCommand = async () => {
    const response = await _module[args.command]();

    console.log(response.message);

    if (response.data) {
        response.data.entries.forEach(entry => {
            console.log('Name:', entry.name, ' Type:', entry.blobType)
        });
    }
};
```

Ten slotte roept de uitvoerende code als eerste *commandExists* aan om te controleren of een bekende opdracht aan het script wordt doorgegeven. Als een bestaande opdracht wordt geselecteerd, wordt de opdracht uitgevoerd en worden eventuele fouten geregistreerd in de console.

```javascript
try {
    const cmd = args.command;

    console.log(`Executing '${cmd}'...`);

    if (commandExists()) {
        executeCommand();
    } else {
        console.log(`The '${cmd}' command does not exist. Try one of these:`);
        Object.keys(_module).forEach(key => console.log(` - ${key}`));
    }
} catch (e) {
    console.log(e);
}
```

## <a name="clean-up-resources"></a>Resources opschonen
Als u niet van plan bent de in dit artikel gemaakte gegevens of accounts te gebruiken, kunt u die verwijderen om ongewenste facturering te voorkomen. Als u de blob en containers wilt verwijderen, kunt u de methoden [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice?view=azure-node-latest#deleteBlobIfExists_container__blob__options__callback_) en [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice?view=azure-node-latest#deleteContainerIfExists_container__options__callback_) gebruiken. U kunt de opslagaccount ook verwijderen [via de portal](../common/storage-create-storage-account.md).

## <a name="resources-for-developing-nodejs-applications-with-blobs"></a>Resources voor het ontwikkelen van Node.js-toepassingen met blobs

Zie de volgende aanvullende bronnen voor Node.js-ontwikkeling met Blob-opslag:

### <a name="binaries-and-source-code"></a>Binaire bestanden en broncode

- Bekijk en installeer de [broncode voor de Node.js-clientbibliotheek](https://github.com/Azure/azure-storage-node) voor Azure Storage op GitHub.

### <a name="client-library-reference-and-samples"></a>Naslaginformatie en voorbeelden voor de .NET-clientbibliotheek

- Zie het [Node.js API-referentiemateriaal](https://docs.microsoft.com/en-us/javascript/api/overview/azure/storage) voor meer informatie over de Node.js-clientbibliotheek.
- Verken [Blob-opslagvoorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=nodejs&term=blob) die zijn geschreven met de Node.js-clientbibliotheek.

## <a name="next-steps"></a>Volgende stappen

Deze quickstart laat zien hoe u een bestand uploadt tussen een lokale schijf en Azure Blob-opslag met behulp van Node.js. Voor meer informatie over het werken met Blob-opslag, gaat u naar de instructies voor Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](storage-nodejs-how-to-use-blob-storage.md)

Bekijk [azure-storage package](https://docs.microsoft.com/javascript/api/azure-storage/?view=azure-node-latest) voor de Node.js-verwijzing voor Azure Storage.
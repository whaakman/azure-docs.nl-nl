---
title: Het Blob storage gebruiken met Node.js | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
documentationcenter: nodejs
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: e52f38d5fb3c100e4275032f9a2a1234961c672b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-nodejs"></a>Blob Storage gebruiken met Node.js
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Overzicht
Dit artikel laat zien hoe u veelvoorkomende scenario's met behulp van Blob-opslag. De voorbeelden zijn geschreven via de Node.js-API. De scenario's worden behandeld bevatten informatie over het uploaden, weergeven, downloaden en verwijderen van blobs.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
Zie voor instructies over het maken van een Node.js-toepassing [Maak een Node.js-web-app in Azure App Service] [bouwen en implementeren van een Node.js-toepassing naar een Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) --met Windows PowerShell of [bouwen en een Node.js-web-app implementeren naar Azure met behulp van Web Matrix](https://www.microsoft.com/web/webmatrix/).

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag
Voor het gebruik van Azure-opslag, moet u de Azure-opslag-SDK voor Node.js, waaronder een set van gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) om te navigeren naar de map waar u uw voorbeeldtoepassing gemaakt.
2. Type **npm Installeer azure-opslag** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende codevoorbeeld.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. U kunt handmatig uitvoeren de **ls** opdracht om te controleren of een **knooppunt\_modules** map is gemaakt. In deze map te zoeken naar de **azure-opslag** , dit pakket de bibliotheken die u nodig hebt bevat voor toegang tot opslag.

### <a name="import-the-package"></a>Het pakket importeren
Het volgende in Kladblok of een andere teksteditor toevoegen aan de bovenkant van de **server.js** -bestand van de toepassing waarin u van plan bent opslag gebruiken:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen
De Azure-module leest de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_ACCESS_KEY`, of `AZURE_STORAGE_CONNECTION_STRING`, voor de benodigde informatie om te verbinden met uw Azure storage-account. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createBlobService**.

## <a name="create-a-container"></a>Een container maken
De **BlobService** object kunt u samenwerken met containers en blobs. De volgende code maakt een **BlobService** object. Voeg de volgende boven aan **server.js**:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> U kunt toegang tot een blob anoniem met **createBlobServiceAnonymous** en het adres van de host te geven. Bijvoorbeeld: `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Gebruik voor het maken van een nieuwe container **createContainerIfNotExists**. Het volgende codevoorbeeld maakt een nieuwe container met de naam 'mycontainer':

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Als de container pas is gemaakt, `result.created` is ingesteld op true. Als de container al bestaat, `result.created` is ingesteld op false. `response`bevat informatie over de bewerking, inclusief de ETag-informatie voor de container.

### <a name="container-security"></a>Beveiliging van de container
Standaard worden nieuwe containers privé zijn en kunnen niet anoniem worden gebruikt. Als u de container openbaar, zodat u toegang hebt tot het anoniem, kunt u de container access instellen op **blob** of **container**.

* **BLOB** -kunnen anonieme leestoegang tot blob-inhoud en metagegevens in deze container, maar niet aan de container metagegevens zoals het weergeven van alle blobs in een container
* **container** -kunnen anonieme leestoegang tot de blob-inhoud en metagegevens, evenals de metagegevens van de container

Het volgende codevoorbeeld ziet u de toegang instellen voor **blob**:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access to blob
      // content and metadata within this container
    }
});
```

U kunt ook het toegangsniveau van een container wijzigen met behulp van **setContainerAcl** het toegangsniveau opgeven. Het volgende codevoorbeeld verandert het toegangsniveau in container:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set to 'container'
  }
});
```

Het resultaat bevat informatie over de bewerking, waaronder de huidige **ETag** voor de container.

### <a name="filters"></a>Filters
U kunt optioneel filteren bewerkingen toepassen op bewerkingen die worden uitgevoerd met behulp van **BlobService**. Bewerkingen voor het filteren kunt opnemen logboekregistratie, automatisch opnieuw, enzovoort. Filters zijn objecten die een methode met de handtekening implementeren:

```nodejs
function handle (requestOptions, next)
```

Hierna moet u de voorverwerking van de aanvraag-opties, de methode moet 'next', niet aanroepen voor het doorgeven van een retouraanroep met de volgende handtekening:

```nodejs
function (returnObject, finalCallback, next)
```

In deze retouraanroep en na het verwerken van de returnObject (de reactie van de aanvraag naar de server), moet de callback volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon aanroepen finalCallback om te beëindigen van het service-aanroepen.

Twee filters die Pogingslogica implementeren zijn opgenomen in de Azure SDK voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende code maakt een **BlobService** -object dat gebruikmaakt van de **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Er zijn drie typen blobs: blok-blobs, pagina-blobs en toevoeg-blobs. Blok-blobs, kunt dat u grote hoeveelheden gegevens efficiënt te uploaden. Toevoeg-blobs zijn geoptimaliseerd voor toevoegbewerkingen. Pagina-blobs zijn geoptimaliseerd voor lees-en schrijfbewerkingen. Zie voor meer informatie [blok-Blobs, toevoeg-Blobs en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blok-blobs
Als u wilt gegevens uploaden naar een blok-blob, gebruikt u het volgende:

* **createBlockBlobFromLocalFile** - maakt een nieuw blok-blob en uploadt u de inhoud van een bestand
* **createBlockBlobFromStream** - maakt een nieuw blok-blob en uploadt u de inhoud van een stroom
* **createBlockBlobFromText** - maakt een nieuw blok-blob en uploadt u de inhoud van een tekenreeks
* **createWriteStreamToBlockBlob** -biedt een stroom schrijven naar een blok-blob

Het volgende codevoorbeeld uploadt de inhoud van de **test.txt** bestand naar **myblob**.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

De `result` geretourneerd door deze methoden bevat informatie over de bewerking, zoals de **ETag** van de blob.

### <a name="append-blobs"></a>Toevoeg-blobs
Als u wilt gegevens uploaden naar een nieuwe toevoeg-blob, gebruikt u het volgende:

* **createAppendBlobFromLocalFile** - maakt een nieuwe toevoeg-blob en uploadt u de inhoud van een bestand
* **createAppendBlobFromStream** - maakt een nieuwe toevoeg-blob en uploadt u de inhoud van een stroom
* **createAppendBlobFromText** - maakt een nieuwe toevoeg-blob en uploadt u de inhoud van een tekenreeks
* **createWriteStreamToNewAppendBlob** - maakt een nieuwe toevoeg-blob en geeft vervolgens een stroom om ernaar te schrijven

Het volgende codevoorbeeld uploadt de inhoud van de **test.txt** bestand naar **myappendblob**.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Als u wilt een blok toevoegen aan een bestaande toevoeg-blob, gebruikt u het volgende:

* **appendFromLocalFile** -de inhoud van een bestand toevoegen aan een bestaande toevoeg-blob
* **appendFromStream** -de inhoud van een stroom toevoegen aan een bestaande toevoeg-blob
* **appendFromText** -toevoegen van de inhoud van een tekenreeks aan een bestaande toevoeg-blob
* **appendBlockFromStream** -de inhoud van een stroom toevoegen aan een bestaande toevoeg-blob
* **appendBlockFromText** -toevoegen van de inhoud van een tekenreeks aan een bestaande toevoeg-blob

> [!NOTE]
> appendFromXXX API's doet clientzijde validatie doen mislukken snel onnodige server om aanroepen te vermijden. appendBlockFromXXX niet het geval.
>
>

Het volgende codevoorbeeld uploadt de inhoud van de **test.txt** bestand naar **myappendblob**.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Pagina-blobs
Als u wilt gegevens uploaden naar een pagina-blob, gebruikt u het volgende:

* **createPageBlob** -maakt een nieuwe pagina-blob met een specifieke lengte
* **createPageBlobFromLocalFile** - maakt een nieuwe pagina-blob en uploadt u de inhoud van een bestand
* **createPageBlobFromStream** - maakt een nieuwe pagina-blob en uploadt u de inhoud van een stroom
* **createWriteStreamToExistingPageBlob** -biedt een stroom schrijven naar een bestaande paginablob
* **createWriteStreamToNewPageBlob** - maakt een nieuwe pagina-blob en geeft vervolgens een stroom om ernaar te schrijven

Het volgende codevoorbeeld uploadt de inhoud van de **test.txt** bestand naar **mypageblob**.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Pagina-blobs bestaan uit 512-byte 'pages'. U ontvangt een fout opgetreden tijdens het uploaden van gegevens met een grootte die is geen veelvoud van 512 bytes.
>
>

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Als de blobs in een container wilt weergeven, gebruikt de **listBlobsSegmented** methode. Als u retourneren BLOB's met een specifieke voorvoegsel wilt, gebruikt u **listBlobsSegmentedWithPrefix**.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains the entries
      // If not all blobs were returned, result.continuationToken has the continuation token.
  }
});
```

De `result` bevat een `entries` verzameling, die is een matrix met objecten die een beschrijving van elke blob. Als u alle blobs kunnen niet worden geretourneerd, de `result` biedt ook een `continuationToken`, die u kunt als de tweede parameter voor het ophalen van extra vermeldingen.

## <a name="download-blobs"></a>Blobs downloaden
Voor het downloaden van gegevens van een blob, gebruikt u het volgende:

* **getBlobToLocalFile** -schrijft de blobinhoud naar het bestand
* **getBlobToStream** -schrijft de blobinhoud naar een stream
* **getBlobToText** -schrijft de blobinhoud naar een tekenreeks
* **createReadStream** -biedt een stroom te lezen van de blob

De volgende voorbeeldcode laat zien met **getBlobToStream** voor het downloaden van de inhoud van de **myblob** blob en op te slaan op de **uitvoer.txt** bestand met behulp van een stream:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

De `result` bevat informatie over de blob, met inbegrip van **ETag** informatie.

## <a name="delete-a-blob"></a>Een blob verwijderen
Tenslotte voor het verwijderen van een blob, roept **deleteBlob**. De volgende voorbeeldcode wordt verwijderd van de blob met de naam **myblob**.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Gelijktijdige toegang
Om gelijktijdige toegang tot een blob van meerdere clients of meerdere procesexemplaren, kunt u **ETags** of **leases**.

* **ETag** -biedt een manier om te detecteren dat het blob of de container is gewijzigd door een ander proces
* **Lease** -biedt een manier om te verkrijgen van exclusieve, verlengd, schrijven of verwijderen van toegang tot een blob voor een bepaalde periode

### <a name="etag"></a>ETag
Gebruik ETags als u wilt toestaan dat meerdere clients of exemplaren om te schrijven naar het blok-Blob of pagina-Blob tegelijkertijd. De ETag kunt u bepalen of de container of blob is gewijzigd sinds u in eerste instantie lezen of deze is gemaakt, zodat u om te voorkomen dat wijzigingen die zijn doorgevoerd door een andere client of een ander proces wordt overschreven.

U kunt voorwaarden ETag instellen met behulp van de optionele `options.accessConditions` parameter. De volgende code voorbeeld alleen uploads de **test.txt** bestand als de blob bestaat al en ETag-waarde is opgenomen door `etagToMatch`.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

Wanneer u ETags gebruikt, is het algemene patroon:

1. De ETag ophalen als gevolg van een create, lijst of get-bewerking.
2. Een actie uitgevoerd, controleren of de ETag-waarde niet is gewijzigd.

Als de waarde is gewijzigd, betekent dit dat een andere client of exemplaar gewijzigd, het blob of de container omdat verkregen van de ETag-waarde.

### <a name="lease"></a>Lease
U kunt een nieuwe lease verkrijgen met behulp van de **acquireLease** methode, het blob of de container die u een lease wilt op te geven. Bijvoorbeeld, een lease op de volgende code verkrijgt **myblob**.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

Verdere bewerkingen op **myblob** moet opgeven de `options.leaseId` parameter. De ID wordt geretourneerd als lease `result.id` van **acquireLease**.

> [!NOTE]
> Standaard is de duur van de lease oneindig. U kunt een duur niet oneindig (tussen 15 en 60 seconden) opgeven door de `options.leaseDuration` parameter.
>
>

U kunt een lease verwijderen **releaseLease**. Als u wilt opsplitsen een lease te voorkomen dat anderen die een nieuwe lease totdat de oorspronkelijke duur is verlopen, gebruik **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Werken met handtekeningen voor gedeelde toegang
Shared access signatures (SAS) zijn geen veilige manier toegang te bieden gedetailleerde tot blobs en containers zonder dat de naam van het opslagaccount of sleutels. Handtekeningen voor gedeelde toegang worden vaak gebruikt voor beperkte toegang tot uw gegevens, zoals het toestaan van een mobiele app voor toegang tot blobs.

> [!NOTE]
> Terwijl u ook toestaan voor anonieme toegang tot blobs dat kunt, wordt er handtekeningen voor gedeelde toegang kunnen u meer beperkte toegang, moet u de SAS genereren opgeven.
>
>

Een vertrouwde toepassing zoals een cloud-gebaseerde service genereert handtekeningen voor gedeelde toegang met behulp van de **generateSharedAccessSignature** van de **BlobService**, en biedt dit aan een niet-vertrouwde of semi vertrouwde toepassing zoals een mobiele app. Gedeelde toegang handtekeningen worden gegenereerd op basis van beleid, waarin wordt beschreven van het begin en einddatums gedurende welke de handtekeningen voor gedeelde toegang geldig zijn, evenals het toegangsniveau verleend aan de houder van de handtekeningen voor gedeelde toegang.

Het volgende codevoorbeeld genereert een nieuw beleid voor gedeelde toegang waarmee de houder van de handtekeningen voor gedeelde toegang leesbewerkingen uitvoeren op de **myblob** blob en 100 minuten na het tijdstip waarop deze is gemaakt is verlopen.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Houd er rekening mee dat de informatie over de host moet ook worden opgegeven als deze vereist is wanneer de houder van de handtekeningen voor gedeelde toegang probeert te krijgen tot de container.

Vervolgens kunt u de clienttoepassing maakt gebruik van handtekeningen voor gedeelde toegang met **BlobServiceWithSAS** bewerkingen op basis van de blob uit te voeren. De volgende informatie opgehaald over **myblob**.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Omdat de handtekeningen voor gedeelde toegang zijn gegenereerd met alleen-lezen toegang als wordt geprobeerd om te wijzigen van de blob, wordt er een fout geretourneerd.

### <a name="access-control-lists"></a>Toegangsbeheerlijsten
U kunt ook een toegangsbeheerlijst (ACL) gebruiken voor het instellen van het toegangsbeleid voor SAS. Dit is handig als u wilt toestaan dat meerdere clients toegang tot een container, maar bieden verschillende toegangsbeleid voor elke client.

Een ACL die is geïmplementeerd met behulp van een matrix van toegangsbeleid, met een ID die is gekoppeld aan elk beleid. Het volgende codevoorbeeld definieert twee beleidsregels, één voor 'gebruiker1' en één voor 'gebruiker2':

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Het volgende codevoorbeeld wordt de huidige ACL voor **mycontainer**, en voegt vervolgens het nieuwe beleid met behulp van **setBlobAcl**. Met deze aanpak kunt:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Zodra de ACL is ingesteld, kunt u vervolgens de handtekeningen voor gedeelde toegang op basis van de ID voor een beleid maken. Het volgende codevoorbeeld maakt nieuwe handtekeningen voor gedeelde toegang voor 'gebruiker2':

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie.

* [Azure-opslag-SDK voor knooppunt API Reference] [Azure-opslag-SDK voor knooppunt API Reference]  
* [Blog van azure Storage-Team] [Blog van azure Storage-Team]  
* [Azure-opslag-SDK voor knooppunt] [ Azure Storage SDK for Node] opslagplaats op GitHub  
* [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)  
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)  

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node  

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/  
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx  
[Azure portal]: https://portal.azure.com  
[Bouw en implementeer een Node.js-toepassing naar een Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)  
[Blog van azure Storage-Team]: http://blogs.msdn.com/b/windowsazurestorage/  
[Azure-opslag-SDK voor knooppunt API-referentiemateriaal]: http://dl.windowsazure.com/nodestoragedocs/index.html  

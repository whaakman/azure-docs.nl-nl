---
title: Het gebruik van blob storage (objectopslag) met PHP | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
documentationcenter: php
services: storage
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 01/11/2018
ms.author: tamram
ms.openlocfilehash: 6bc7fd799eddca14e728f965601acd244d88870c
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-use-blob-storage-from-php"></a>Het blob storage gebruiken met PHP
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Azure Blob Storage is een service waarmee ongestructureerde gegevens als objecten/blobs worden opgeslagen in de cloud. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

Deze handleiding wordt getoond hoe u veelvoorkomende scenario's met behulp van de Azure blob-service uitvoert. De voorbeelden zijn geschreven in PHP en gebruik de [Azure Storage-Blob-clientbibliotheek voor PHP][download]. De scenario's worden behandeld: **uploaden**, **aanbieding**, **downloaden**, en **verwijderen** blobs. Zie voor meer informatie over blobs de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken
De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de Azure blob-service is de verwijzende klassen in de [Azure Storage-clientbibliotheek voor PHP] [ download] vanuit uw code. Alle ontwikkelingsprogramma's kunt u uw toepassing, met inbegrip van Kladblok maken.

In deze handleiding gebruikt u de Blob storage-servicefuncties, die kunnen worden aangeroepen binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een Azure-Webrol, werkrol of website.

## <a name="get-the-azure-client-libraries"></a>De clientbibliotheken van Azure ophalen
### <a name="install-via-composer"></a>Installeren via de Composer
1. Maak een bestand met de naam **composer.json** in de hoofdmap van uw project en voeg de volgende code toe:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage-blob": "*"
      }
    }
    ```
2. Download  **[composer.phar] [ composer-phar]**  in de hoofdmap van uw project.
3. Open een opdrachtprompt en voer de volgende opdracht in de hoofdmap van uw project
   
    ```
    php composer.phar install
    ```

U kunt ook gaat u naar de [Azure Storage-clientbibliotheek met PHP] [ download] op GitHub voor het klonen van de broncode.

## <a name="configure-your-application-to-access-the-blob-service"></a>Uw toepassing configureren voor toegang tot de blob-service
Voor het gebruik van de Azure blob-service API's, moet u:

1. Verwijst naar de autoloader-bestand met de [require_once] -instructie en
2. Verwijst naar alle klassen die u kunt gebruiken.

Het volgende voorbeeld laat zien hoe de autoloader-bestand en de verwijzing naar de **BlobRestProxy** klasse.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Blob\BlobRestProxy;
```

In de onderstaande voorbeelden de `require_once` instructie altijd wordt weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-an-azure-storage-connection"></a>Een Azure-opslag-verbinding instellen
U moet een geldige verbindingsreeks hebben voor het concretiseren van een Azure blob-service-client. De indeling voor de verbindingsreeks van de blob-service is:

Voor toegang tot een service voor live:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Voor toegang tot de opslagemulator:

```php
UseDevelopmentStorage=true
```

Voor het maken van een Azure Blob-serviceclient die u wilt gebruiken, de **BlobRestProxy** klasse. U kunt:

* de verbindingsreeks rechtstreeks aan deze doorgeven of
* Omgevingsvariabelen in uw Web-App gebruiken voor het opslaan van de verbindingsreeks. Zie [configuratie-instellingen van Azure-web-app](../../app-service/web-sites-configure.md) document voor het configureren van verbindingsreeksen.

Voor de voorbeelden die hier wordt beschreven, wordt de verbindingsreeks rechtstreeks doorgegeven.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

$blobClient = BlobRestProxy::createBlobService($connectionString);
```

## <a name="create-a-container"></a>Een container maken
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Een **BlobRestProxy** object maakt u een blob-container met de **createContainer** methode. Bij het maken van een container, kunt u opties voor de container instellen, maar dit is dus niet vereist. (Het volgende voorbeeld ziet u het instellen van de container toegangsbeheerlijst (ACL) en de metagegevens van de container.)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob client.
$blobClient = BlobRestProxy::createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobClient->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Het aanroepen van **setPublicAccess (PublicAccessType::CONTAINER\_en\_BLOBS)** toegankelijk via anonieme aanvragen maakt van de container en de blob-gegevens. Het aanroepen van **setPublicAccess(PublicAccessType::BLOBS_ONLY)** zorgt ervoor dat alleen blob-gegevens toegankelijk via anonieme aanvragen. Zie voor meer informatie over de container-ACL's, [Set container-ACL (REST-API)][container-acl].

Zie voor meer informatie over foutcodes voor Blob-service [foutcodes voor Blob-Service][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Als u wilt een bestand als een blob uploadt, gebruiken de **BlobRestProxy -> createBlockBlob** methode. Deze bewerking wordt de blob gemaakt als deze niet bestaat, of deze wordt overschreven als dit het geval is. Het volgende voorbeeld wordt ervan uitgegaan dat de container al gemaakt is en maakt gebruik van [fopen] [ fopen] naar het bestand openen als een stroom.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobClient->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Houd er rekening mee dat het vorige voorbeeld een blob als een stream uploadt. Echter, een blob kan ook worden geüpload als een tekenreeks met bijvoorbeeld de [bestand\_ophalen\_inhoud] [ file_get_contents] functie. U doet dit met behulp van het vorige voorbeeld door wijzigen `$content = fopen("c:\myfile.txt", "r");` naar `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Als de blobs in een container wilt weergeven, gebruikt de **BlobRestProxy -> listBlobs** methode met een **foreach** lus via het resultaat. De volgende code wordt de naam van elke blob als uitvoer in een container en wordt de URI voor de browser.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

try    {
    // List blobs.
    $blob_list = $blobClient->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Een blob downloaden
Aanroepen voor het downloaden van een blob, de **BlobRestProxy -> getBlob** methode, roept u vervolgens de **getContentStream** methode op de resulterende **GetBlobResult** object.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobClient->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Houd er rekening mee dat het bovenstaande voorbeeld een blob opgehaald als de bron van een stroom (de standaardinstelling). U kunt echter de [stroom\_ophalen\_inhoud] [ stream-get-contents] functioneren op de geretourneerde stroom niet converteren naar een tekenreeks.

## <a name="delete-a-blob"></a>Een blob verwijderen
Geeft de containernaam en blob in voor het verwijderen van een blob **BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

try    {
    // Delete blob.
    $blobClient->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>Verwijderen van een blob-container
Ten slotte voor het verwijderen van een blob-container, geeft de containernaam van de aan **BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

try    {
    // Delete container.
    $blobClient->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van de Azure blob-service hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* Ga naar de [API-referentiemateriaal voor Azure Storage-clientbibliotheek voor PHP](http://azure.github.io/azure-storage-php/)
* Zie de [Blob Geavanceerd voorbeeld](https://github.com/Azure/azure-storage-php/blob/master/samples/BlobSamples.php).

[download]: https://github.com/Azure/azure-storage-php
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[composer-phar]: http://getcomposer.org/composer.phar

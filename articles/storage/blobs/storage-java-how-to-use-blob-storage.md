---
title: Het gebruik van Azure Blob storage (objectopslag) met Java | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
documentationcenter: java
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 2e223b38-92de-4c2f-9254-346374545d32
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 91ef09916dbb587305572ea640fb4408ea9aebb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-java"></a>Blob Storage gebruiken met Java
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Overzicht
Azure Blob Storage is een service waarmee ongestructureerde gegevens als objecten/blobs worden opgeslagen in de cloud. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

In dit artikel wordt beschreven hoe u veelvoorkomende scenario's met behulp van de Microsoft Azure Blob-opslag uitvoeren. De voorbeelden zijn geschreven in Java en gebruik de [Azure-opslag-SDK voor Java][Azure Storage SDK for Java]. De scenario's worden behandeld: **uploaden**, **aanbieding**, **downloaden**, en **verwijderen** blobs. Zie voor meer informatie over blobs de [Vervolgstappen](#Next-Steps) sectie.

> [!NOTE]
> Een SDK is beschikbaar voor ontwikkelaars die werken met Azure Storage op Android-apparaten. Zie voor meer informatie de [Azure-opslag-SDK voor Android][Azure Storage SDK for Android].
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken
In dit artikel gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in de code die wordt uitgevoerd binnen een Webrol of worker-rol in Azure.

Om dit te doen, moet u Java Development Kit (JDK) installeren en een Azure Storage-account maken in uw Azure-abonnement. Als u dit hebt gedaan, moet u controleren of uw ontwikkelsysteem voldoet aan de minimale vereisten en afhankelijkheden die worden vermeld in de [Azure-opslag-SDK voor Java] [ Azure Storage SDK for Java] opslagplaats op GitHub. Als uw systeem aan deze vereisten voldoet, kunt u de instructies voor het downloaden en installeren van de Azure Storage-bibliotheken voor Java op uw systeem vanuit die opslagplaats. Nadat u deze taken hebt voltooid, kunt u zich kunt maken van een Java-toepassing die gebruikmaakt van de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-blob-storage"></a>Uw toepassing configureren voor toegang tot Blob-opslag
De volgende importinstructies toevoegen aan het begin van de Java-bestand waar u de Azure Storage-API's gebruiken voor toegang tot blobs.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Een Azure Storage-verbindingsreeks instellen
Een Azure Storage-client gebruikt een verbindingsreeks voor opslag voor het opslaan van eindpunten en referenties voor toegang tot gegevens beheerservices. Wanneer u in een clienttoepassing uitvoert, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw opslagaccount en de primaire toegangssleutel voor het opslagaccount vermeld in de [Azure-portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Het volgende voorbeeld ziet hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren.

```java
// Define the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

In een toepassing in een rol in Microsoft Azure wordt uitgevoerd, kan deze tekenreeks worden opgeslagen in het serviceconfiguratiebestand *ServiceConfiguration.cscfg*, en kunnen worden geopend met een aanroep naar de **RoleEnvironment.getConfigurationSettings** methode. Het volgende voorbeeld wordt de verbindingsreeks uit een **instelling** element met de naam *StorageConnectionString* in het configuratiebestand van de service.

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om op te halen van de verbindingsreeks voor opslag.

## <a name="create-a-container"></a>Een container maken
Een **CloudBlobClient** object kunt u profiteren van reference-objecten voor containers en blobs. De volgende code maakt een **CloudBlobClient** object.

> [!NOTE]
> Er zijn aanvullende manieren maken **CloudStorageAccount** objecten; voor meer informatie Zie **CloudStorageAccount** in de [naslaginformatie over Azure Storage Client SDK].
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Gebruik de **CloudBlobClient** object een verwijzing naar de container die u wilt gebruiken. U kunt de container maken als deze niet met bestaat de **createIfNotExists** methode, waarmee de bestaande container anders retourneert. Standaard is de nieuwe container privé, dus u uw toegangssleutel voor opslag opgeven moet (zoals u eerder hebt gedaan) om blobs te downloaden uit deze container.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference to a container.
    // The container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create the container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a>Optioneel: Een container voor openbare toegang configureren
Een container machtigingen zijn standaard geconfigureerd voor persoonlijke toegang, maar u kunt gemakkelijk een container machtigingen configureren voor openbare, alleen-lezen toegang toestaan voor alle gebruikers op Internet:

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Als u wilt een bestand naar een blob uploadt, een containerverwijzing ophalen en deze gebruiken een blobverwijzing ophalen. Zodra u een blobverwijzing hebt, kunt u elke gewenste gegevensstroom uploaden door het aanroepen van het uploaden van de blobverwijzing. Deze bewerking wordt de blob gemaakt als deze niet bestaat of overschrijven als dit het geval is. Het volgende codevoorbeeld ziet u dit en wordt ervan uitgegaan dat de container al is gemaakt.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define the path to a local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite the "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Als de blobs in een container wilt weergeven, moet u eerst een containerverwijzing net zoals een blob uploaden ophalen. U kunt de container **listBlobs** methode met een **voor** lus. De volgende code wordt de uitvoer de Uri van elke blob in een container met de console.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within the container and output the URI to each of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Houd er rekening mee dat kunt u de naam blobs met informatie over het pad in hun naam. Hiermee maakt u een virtuele mapstructuur die u kunt ordenen en kunt doorlopen als een traditioneel bestandssysteem. Houd er rekening mee dat de mapstructuur alleen virtueel is: de enige beschikbare resources in Blob Storage zijn containers en blobs. De clientbibliotheek biedt echter een **CloudBlobDirectory** -object om te verwijzen naar een virtuele map en het proces van het werken met blobs die op deze manier zijn ingedeeld vereenvoudigen.

U kan bijvoorbeeld een container met de naam 'foto's ', waarin u uploaden mogelijk blobs met de naam 'rootphoto1', ' 2010/photo1', ' 2010/photo2' en ' 2011/photo1' hebben. Dit maakt de virtuele mappen '2010' en '2011' in de container 'foto's '. Als u aanroept **listBlobs** op de container 'foto's ', de verzameling geretourneerd bevat **CloudBlobDirectory** en **CloudBlob** -objecten die de mappen en blobs die zijn opgenomen op het hoogste niveau. In dit geval zou mappen '2010' en '2011', evenals photo 'rootphoto1' worden geretourneerd. U kunt de **instanceof** operator om te onderscheiden van deze objecten.

U kunt eventueel doorgeven in parameters voor de **listBlobs** methode met de **useFlatBlobListing** parameter ingesteld op true. Dit leidt ertoe dat elke blob wordt geretourneerd, ongeacht de directory. Zie voor meer informatie **CloudBlobContainer.listBlobs** in de [naslaginformatie over Azure Storage Client SDK].

## <a name="download-a-blob"></a>Een blob downloaden
Om blobs te downloaden, volgt u dezelfde stappen als u dit hebt gedaan voor het uploaden van een blob om een blobverwijzing ophalen. In het voorbeeld uploaden u uploaden voor de blob-object aangeroepen. In het volgende voorbeeld roept downloaden om over te dragen van de blobinhoud naar een stroomobject, zoals een **FileOutputStream** dat u gebruiken kunt om vast te leggen van de blob naar een lokaal bestand.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in the container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If the item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download the item and save it to a file with the same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a>Een blob verwijderen
Verwijderen van een blob, een blobverwijzing ophalen en aanroep **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference to a blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete the blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a>Verwijderen van een blob-container
Ten slotte voor het verwijderen van een blob-container ophalen van een blob containerverwijzing en aanroep **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete the blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Blob storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* [Azure-opslag-SDK voor Java][Azure Storage SDK for Java]
* [naslaginformatie over Azure Storage Client SDK][naslaginformatie over Azure Storage Client SDK]
* [REST API van Azure Storage][Azure Storage REST API]
* [Azure Storage-teamblog][Azure Storage Team Blog]

Zie voor meer informatie ook [Azure voor Java-ontwikkelaars](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[naslaginformatie over Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/

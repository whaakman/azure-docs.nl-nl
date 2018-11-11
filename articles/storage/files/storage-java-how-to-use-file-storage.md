---
title: Ontwikkelen voor Azure Files met Java | Microsoft Docs
description: Meer informatie over het ontwikkelen van Java-toepassingen en services die Azure Files gebruiken voor het opslaan van gegevens uit een bestand.
services: storage
author: wmgries
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 09/19/2017
ms.author: renash
ms.component: files
ms.openlocfilehash: 8a5a71fada190939abf638cd1b681adff7e80970
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244539"
---
# <a name="develop-for-azure-files-with-java"></a>Ontwikkelen voor Azure Files met Java
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Over deze zelfstudie
In deze zelfstudie worden de basisbeginselen van het gebruik van Java voor het ontwikkelen van toepassingen of services die gebruikmaken van Azure Files voor het opslaan van gegevens uit een bestand. In deze zelfstudie wordt deze gemaakt een consoletoepassing en laten zien hoe u basisbewerkingen uitvoert met Java en Azure Files:

* Maken en verwijderen van Azure-bestandsshares
* Maken en verwijderen van mappen
* Het inventariseren van bestanden en mappen in een Azure-bestandsshare
* Uploaden, downloaden en een bestand verwijderen

> [!Note]  
> Omdat Azure Files toegankelijk is via SMB, is het mogelijk om toepassingen die toegang hebben tot de Azure-bestandsshare met behulp van de standaard Java i/o-klassen te schrijven. In dit artikel wordt beschreven hoe u toepassingen schrijft die gebruikmaken van de Azure Storage Java SDK, die gebruikmaakt van de [Azure Files REST API](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) om te communiceren met Azure Files.

## <a name="create-a-java-application"></a>Een Java-toepassing maken
Als u wilt de voorbeelden bouwt, moet u de Java Development Kit (JDK) en de [Azure Storage SDK voor Java](https://github.com/Azure/azure-storage-java). U moet hebt een Azure storage-account ook gemaakt.

## <a name="set-up-your-application-to-use-azure-files"></a>Instellen van uw toepassing in Azure Files gebruiken
Voor het gebruik van de Azure storage-API's, toevoegen de volgende instructie aan het begin van de Java-bestand waarin u van plan bent voor toegang tot de opslagservice.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Instellen van een Azure storage-verbindingsreeks
Azure-bestanden wilt gebruiken, moet u verbinding maken met uw Azure storage-account. De eerste stap is het configureren van een verbindingsreeks, die we gebruiken om verbinding maken met uw opslagaccount. We definiëren een statische variabele om dit te doen.

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Vervang your_storage_account_name en your_storage_account_key door de werkelijke waarden voor uw opslagaccount.
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>Verbinding maken met een Azure storage-account
Voor verbinding met uw storage-account, moet u gebruiken de **CloudStorageAccount** -object doorgeven van een verbindingsreeks voor de **parseren** methode.

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** genereert een InvalidKeyException, dus u moet deze in een try/catch-blok te plaatsen.

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken
Alle bestanden en mappen in Azure-bestanden bevinden zich in een container met de naam een **Share**. Uw storage-account kunt zoveel shares als de accountcapaciteit van uw hebben. Als u wilt verkrijgen van toegang tot een share en de inhoud ervan, moet u een Azure Files-client gebruiken.

```java
// Create the Azure Files client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

U kunt vervolgens een een verwijzing naar een share verkrijgen met behulp van de Azure Files-client.

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

U kunt de share daadwerkelijk te maken met de **createIfNotExists** methode van het object CloudFileShare.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

Op dit moment **delen** bevat een verwijzing naar een share met de naam **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Een Azure-bestandsshare verwijderen
Verwijderen van een share wordt gedaan door het aanroepen van de **deleteIfExists** methode voor het object CloudFileShare. Hier volgt een voorbeeldcode dat dit doet.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a>Een map maken
U kunt ook storage organiseren door het opslaan van bestanden in submappen in plaats van dat ze allemaal in de hoofdmap. Azure Files kunt u zo veel mappen als uw account kunt maken. De code hieronder maakt u een submap met de naam **sampledir** onder de hoofddirectory.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a>Een map verwijderen
Verwijderen van een directory is een eenvoudige taak, maar moet worden opgemerkt dat u een map waarin zich nog steeds bestanden of andere directory's niet verwijderen.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Het inventariseren van bestanden en mappen in een Azure-bestandsshare
Het ophalen van een lijst met bestanden en mappen in een share is eenvoudig uitgevoerd door het aanroepen van **listFilesAndDirectories** op een CloudFileDirectory-verwijzing. De methode retourneert een lijst met ListFileItem-objecten die u kunt herhalen. Als u bijvoorbeeld de volgende code worden bestanden en mappen in de hoofdmap.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Bestand uploaden
In deze sectie leert u hoe u een bestand uit de lokale opslag naar de hoofdmap van een share te uploaden.

De eerste stap bij het uploaden van een bestand is op te halen van een verwijzing naar de map waarin dit zich moet bevinden. U dit doen door het aanroepen van de **getRootDirectoryReference** methode van de shareobject.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Nu dat u een verwijzing naar de hoofdmap van de share hebt, kunt u een bestand op met behulp van de volgende code kunt uploaden.

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Bestand downloaden
Een van de vaker bewerkingen die u op basis van Azure Files uitvoeren wilt is om bestanden te downloaden. In het volgende voorbeeld wordt de code SampleFile.txt gedownload en wordt de inhoud weergegeven.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a>Een bestand verwijderen
Een andere algemene Azure Files-bewerking wordt de bestanden hebt verwijderd. De volgende code verwijdert een bestand met de naam SampleFile.txt opgeslagen in een map met de naam **sampledir**.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a>Volgende stappen
Als u wilt meer informatie over andere Azure storage-API's, volgt u deze koppelingen.

* [Azure voor Java-ontwikkelaars](/java/azure)/)
* [Azure Storage SDK voor Java](https://github.com/azure/azure-storage-java)
* [Azure Storage SDK voor Android](https://github.com/azure/azure-storage-android)
* [Azure Storage Client SDK-referentie](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](https://blogs.msdn.com/b/windowsazurestorage/)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../common/storage-use-azcopy.md)
* [Problemen met betrekking tot Azure Files oplossen - Windows](storage-troubleshoot-windows-file-connection-problems.md)
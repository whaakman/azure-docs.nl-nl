---
title: Het gebruik van blob storage (objectopslag) van C++ | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
documentationcenter: .net
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 53844120-1c48-4e2f-8f77-5359ed0147a4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: daf480b7be78dc001712010eac6386d4744c3c1d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-blob-storage-from-c"></a>Het Blob Storage gebruiken met C++
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
Azure Blob Storage is een service waarmee ongestructureerde gegevens als objecten/blobs worden opgeslagen in de cloud. In Blob Storage kan elk type tekst of binaire gegevens, zoals een document, mediabestand of toepassingsinstallatieprogramma, worden opgeslagen. U kunt Blob Storage zien als een vorm van objectopslag.

Deze handleiding wordt gedemonstreerd hoe u veelvoorkomende scenario's met behulp van de Azure Blob storage-service uitvoert. De voorbeelden zijn geschreven in C++ en gebruik de [Azure Storage-clientbibliotheek voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's worden behandeld: **uploaden**, **aanbieding**, **downloaden**, en **verwijderen** blobs.  

> [!NOTE]
> Deze handleiding is bedoeld voor de Azure Storage-clientbibliotheek voor C++ versie 1.0.0 en hoger. De aanbevolen versie is Storage-clientbibliotheek 2.2.0, die beschikbaar is via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing.  

Om dit te doen, moet u voor het installeren van de Azure Storage-clientbibliotheek voor C++ en een Azure storage-account maken in uw Azure-abonnement.   

Voor het installeren van de Azure Storage-clientbibliotheek voor C++, kunt u de volgende methoden:

* **Linux:** Volg de instructies in de [Azure Storage-clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.  
* **Windows:** In Visual Studio, klikt u op **Extra > NuGet Package Manager > Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.  
  
     Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Uw toepassing configureren voor toegang tot Blob-opslag
Toevoegen aan dat de volgende instructies toe aan de bovenkant van het bestand C++ is waar u de Azure storage-API's gebruiken voor toegang tot blobs instructies bevatten:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a>Een Azure-opslag-verbindingsreeks instellen
Een Azure-opslag-client gebruikt een verbindingsreeks voor opslag voor het opslaan van eindpunten en referenties voor toegang tot gegevens beheerservices. Wanneer u in een clienttoepassing uitvoert, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw opslagaccount en de toegangssleutel voor opslag voor de storage-account wordt vermeld in de [Azure Portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Zie voor informatie over de storage-accounts en toegangstoetsen, [over Azure Storage-Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Dit voorbeeld ziet hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Testen van uw toepassing in uw lokale Windows-computer, kunt u de Microsoft Azure [opslagemulator](../storage-use-emulator.md) die is geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/). De opslagemulator is een hulpprogramma dat de services Blob, wachtrijen en tabellen beschikbaar zijn in Azure op uw lokale ontwikkelcomputer simuleert. Het volgende voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks naar uw lokale opslagemulator kunt declareren:

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Voor het starten van de Azure-opslagemulator, selecteer de **Start** of drukt u op de **Windows** sleutel. Begint te typen **Azure-Opslagemulator**, en selecteer **Microsoft Azure-Opslagemulator** uit de lijst met toepassingen.  

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om op te halen van de verbindingsreeks voor opslag.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de **cloud_storage_account** klasse vertegenwoordigt de informatie van uw Opslagaccount. Voor het ophalen van gegevens over uw storage-account van de verbindingsreeks voor opslag, kunt u de **parseren** methode.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Vervolgens maakt geen verwijzing ophalen naar een **cloud_blob_client** klasse omdat u objecten die vertegenwoordigen containers en blobs die zijn opgeslagen in de Blob Storage-Service ophalen. De volgende code maakt een **cloud_blob_client** object met het account opslagobject we hierboven opgehaald:  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Procedure: een container maken
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

In dit voorbeeld ziet u hoe u een container kunt maken als deze nog niet bestaat:  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create the container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

Standaard is de nieuwe container privé en moet u uw toegangssleutel voor opslag om blobs te downloaden uit deze container. Als u wilt dat de bestanden (BLOB's) in de container beschikbaar maken voor iedereen, kunt u de container openbaar met de volgende code instellen:  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Iedereen op Internet kan blobs in een openbare container zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste sleutel hebt.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Procedure: een blob uploaden naar een container
Azure Blob Storage ondersteunt blok-blobs en pagina-blobs. In de meeste gevallen is een blok-blob het aangewezen type om te gebruiken.  

Om een bestand naar een blok-blob te uploaden, haalt u een containerverwijzing op en gebruikt u deze om een blok-blobverwijzing op te halen. Zodra u een blobverwijzing hebt, kunt u een stream met gegevens uploaden naar het door het aanroepen van de **upload_from_stream** methode. Met deze bewerking wordt de blob gemaakt (als deze nog niet bestaat) of overschreven (als deze wel al bestaat). Het volgende voorbeeld laat zien hoe u een blob uploadt naar een container. Hierbij wordt ervan uitgegaan dat de container al is gemaakt.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite the "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference to a blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

U kunt ook de **upload_from_file** methode voor het uploaden van een bestand naar een blok-blob.

## <a name="how-to-list-the-blobs-in-a-container"></a>Procedure: lijst van de blobs in een container
Als u een lijst van de blobs in een container wilt weergeven, moet u eerst een containerverwijzing ophalen. Vervolgens kunt u de container **list_blobs** methode voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **list_blob_item**, moet u aanroepen de **list_blob_item.as_blob** methode om op te halen een **cloud_blob** object, of de **list_blob.as_directory** methode een cloud_blob_directory-object niet ophalen. De volgende code laat zien hoe ophalen en de uitvoer van de URI van elk item in de **mijn-voorbeeld-container** container:

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Output URI of each item.
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
    }
    else
    {
        std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
}
```

Zie voor meer informatie over het weergeven van bewerkingen [lijst Azure Storage-Resources in C++](../storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Hoe: blobs downloaden
Om blobs te downloaden, eerst een blobverwijzing ophalen en roept u vervolgens de **download_to_stream** methode. Het volgende voorbeeld wordt de **download_to_stream** methode voor het overdragen van de blobinhoud naar een stroomobject dat u vervolgens blijven in een lokaal bestand behouden kunt.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents to a file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

U kunt ook de **download_to_file** methode voor het downloaden van de inhoud van een blob naar een bestand.
Bovendien kunt u ook kunt u de **download_text** methode voor het downloaden van de inhoud van een blob als een tekenreeks.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download the contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a>Hoe: blobs verwijderen
U verwijdert een blob door eerst een blobverwijzing ophalen en roept u vervolgens de **delete_blob** methode erop.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete the blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van blob storage hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Storage.  

* [Hoe Queue Storage gebruiken met C++](../storage-c-plus-plus-how-to-use-queues.md)
* [Hoe Table Storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Lijst met Azure Storage-Resources in C++](../storage-c-plus-plus-enumeration.md)
* [Opslagclientbibliotheek voor C++-verwijzing](http://azure.github.io/azure-storage-cpp)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../storage-use-azcopy.md)


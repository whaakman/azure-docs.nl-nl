---
title: Het gebruik van object (Blob)-opslag via C++ - Azure | Microsoft Docs
description: Store niet-gestructureerde gegevens in de cloud met Azure-blobopslag (object).
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: michaelhauss
ms.subservice: blobs
ms.openlocfilehash: d86b2c71515900405b0e7714d2c36cd8e4cbc7fc
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993216"
---
# <a name="how-to-use-blob-storage-from-c"></a>Het Blob storage gebruiken met C++

Deze handleiding laat zien hoe u algemene scenario's met behulp van Azure Blob-opslag uitvoert. De voorbeelden laten zien hoe om te uploaden, vermelden, downloaden en verwijderen van blobs. De voorbeelden zijn geschreven in C++ en maken gebruik van de [Azure Storage-clientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).   

Zie voor meer informatie over Blob-opslag, [Inleiding tot Azure Blob-opslag](storage-blobs-introduction.md).

> [!NOTE]
> Deze handleiding is gericht op de Azure-opslagclientbibliotheek voor C++ versie 1.0.0 en hoger. Microsoft raadt het gebruik van de meest recente versie van de Storage-clientbibliotheek voor C++, beschikbaar via [NuGet](https://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing.  

Hiervoor moet u de Azure-opslagclientbibliotheek voor C++ installeren en een Azure-opslagaccount in uw Azure-abonnement maken.   

Voor het installeren van de Azure-opslagclientbibliotheek voor C++ kunt u de volgende methoden gebruiken:

* **Linux:** Volg de instructies de [Azure Storage-clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.  
* **Windows:** Klik in Visual Studio op **Tools > NuGet Package Manager > Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.  
  
     Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Uw toepassing configureren voor toegang tot Blob storage
Voeg dat de volgende instructies toe aan het begin van de C++-bestand dat is waar u de Azure storage-API's gebruiken voor toegang tot blobs bevatten:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
#include <cpprest/filestream.h>  
#include <cpprest/containerstream.h> 
```

## <a name="setup-an-azure-storage-connection-string"></a>Een Azure storage-verbindingsreeks instellen
Een Azure-opslagclient gebruikt een opslagverbindingstekenreeks om eindpunten en referenties voor toegang tot gegevensbeheerservices op te slaan. Wanneer in een clienttoepassing wordt uitgevoerd, moet u de verbindingsreeks voor opslag in de volgende indeling, met de naam van uw opslagaccount en de toegangssleutel voor het opslagaccount dat is vermeld in de [Azure Portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Zie voor meer informatie over storage-accounts en toegangssleutels [over Azure Storage-Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). In dit voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de verbindingstekenreeks:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Als u wilt uw toepassing testen op uw lokale Windows-computer, kunt u de Microsoft Azure [opslagemulator](../storage-use-emulator.md) die is geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/). De opslagemulator is een hulpprogramma waarmee de services Blob, Queue en Table beschikbaar zijn in Azure op uw lokale ontwikkelcomputer simuleert. In het volgende voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de verbindingstekenreeks naar uw lokale opslagemulator:

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Selecteer eerst de Azure-opslagemulator de **Start** of drukt u op de **Windows** sleutel. Begin met typen **Azure-Opslagemulator**, en selecteer **Microsoft Azure-Opslagemulator** uit de lijst met toepassingen.  

In de volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de opslagverbindingstekenreeks op te halen.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de **cloud_storage_account** klasse om weer te geven van de gegevens van uw Opslagaccount. Voor het ophalen van uw opslagaccountgegevens uit de opslagverbindingstekenreeks kunt u de methode **parse** gebruiken.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Vervolgens krijgt u een verwijzing naar een **cloud_blob_client** klasse aangezien kunt u objecten die staan voor containers en blobs die zijn opgeslagen in Blob-opslag ophalen. De volgende code maakt een **cloud_blob_client** object met behulp van het opslagobject account we hierboven opgehaald:  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Procedure: Een container maken
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

Standaard is de nieuwe container privé en moet u uw toegangssleutel voor het downloaden van blobs uit deze container opgeven. Als u de bestanden (BLOB's) in de container beschikbaar te maken voor iedereen wilt, kunt u de container openbaar met de volgende code instellen:  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Iedereen op Internet kan blobs in een openbare container zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste toegangssleutel hebben.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Procedure: Een blob uploaden naar een container
Azure Blob storage ondersteunt blokkeren blobs en pagina-blobs. In de meeste gevallen is een blok-blob het aangewezen type om te gebruiken.  

Om een bestand naar een blok-blob te uploaden, haalt u een containerverwijzing op en gebruikt u deze om een blok-blobverwijzing op te halen. Zodra u een blobverwijzing hebt, kunt u elke gewenste gegevensstroom van gegevens naar uploaden door het aanroepen van de **upload_from_stream** methode. Met deze bewerking wordt de blob gemaakt (als deze nog niet bestaat) of overschreven (als deze wel al bestaat). Het volgende voorbeeld laat zien hoe u een blob uploadt naar een container. Hierbij wordt ervan uitgegaan dat de container al is gemaakt.  

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

U kunt ook kunt u de **upload_from_file** methode voor het uploaden van een bestand naar een blok-blob.

## <a name="how-to-list-the-blobs-in-a-container"></a>Procedure: De blobs in een container in een lijst weergeven
Als u een lijst van de blobs in een container wilt weergeven, moet u eerst een containerverwijzing ophalen. Vervolgens kunt u van de container **list_blobs** methode voor het ophalen van de blobs en/of de mappen hierin. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **list_blob_item**, moet u aanroepen de **list_blob_item.as_blob** methode om op te halen een **cloud_blob** -object, of de **list_blob.as_directory** methode voor het ophalen van een cloud_blob_directory-object. De volgende code toont hoe u kunt ophalen en de uitvoer van de URI van elk item in de **my-sample-container** container:

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

Zie voor meer informatie over het weergeven van bewerkingen [lijst met Azure Storage-Resources in C++](../storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Procedure: Blobs downloaden
Blobs downloaden, eerst een blobverwijzing ophalen en roep vervolgens de **download_to_stream** methode. Het volgende voorbeeld wordt de **download_to_stream** methode voor het overdragen van de blob-inhoud naar een stroomobject, dat u vervolgens persistent naar een lokaal bestand maken kunt.  

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

U kunt ook kunt u de **download_to_file** methode voor het downloaden van de inhoud van een blob naar een bestand.
Bovendien kunt u ook kunt u de **download_text** methode voor het downloaden van de inhoud van een blob als een tekenreeks met tekst.  

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

## <a name="how-to-delete-blobs"></a>Procedure: Blobs verwijderen
Als u wilt verwijderen van een blob, eerst een blobverwijzing ophalen en vervolgens roept de **delete_blob** methode.  

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
Nu dat u de basisprincipes van blob storage hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Storage.  

* [Hoe u Queue Storage gebruiken met C++](../storage-c-plus-plus-how-to-use-queues.md)
* [Hoe u kunt Table Storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Lijst met Azure Storage-Resources in C++](../storage-c-plus-plus-enumeration.md)
* [Storage-clientbibliotheek voor C++-referentie](https://azure.github.io/azure-storage-cpp)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../storage-use-azcopy.md)


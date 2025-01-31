---
title: Ontwikkelen voor Azure Files met C++ | Microsoft Docs
description: Meer informatie over het ontwikkelen van C++-toepassingen en services die Azure Files gebruiken voor het opslaan van gegevens uit een bestand.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 424db4435c569475f9463894b5364fc22190689a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717920"
---
# <a name="develop-for-azure-files-with-c"></a>Ontwikkelen voor Azure Files met C++

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Over deze zelfstudie

In deze zelfstudie leert u hoe u eenvoudige bewerkingen op Azure-bestanden uit te voeren. Via de voorbeelden die zijn geschreven in C++, leert u hoe u Maak shares en mappen, uploaden, vermelden en verwijderen van bestanden. Als u niet bekend bent met Azure Files, is via de concepten in de volgende secties is handig bij het begrijpen van de voorbeelden.

* Maken en verwijderen van Azure-bestandsshares
* Maken en verwijderen van mappen
* Het inventariseren van bestanden en mappen in een Azure-bestandsshare
* Uploaden, downloaden en een bestand verwijderen
* Het quotum (maximumgrootte) voor een Azure-bestandsshare instellen
* Een Shared Access Signature (SAS-sleutel) maken voor een bestand dat gebruikmaakt van een gedeeld toegangsbeleid dat voor de share is gedefinieerd.

> [!Note]  
> Omdat Azure Files toegankelijk is via SMB, is het mogelijk om eenvoudige toepassingen die toegang hebben tot de Azure-bestandsshare met behulp van de standaard C++ i/o-klassen en -functies te schrijven. In dit artikel wordt beschreven hoe u toepassingen schrijft die gebruikmaken van de Azure Storage C++ SDK, die gebruikmaakt van de [File REST-API](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) om te communiceren met Azure Files.

## <a name="create-a-c-application"></a>Een C++-toepassing maken

Voor het bouwen van de voorbeelden, moet u de 2.4.0 Azure Storage-clientbibliotheek voor C++ installeren. U moet hebt een Azure storage-account ook gemaakt.

Voor het installeren van de Azure Storage Client 2.4.0 voor C++, kunt u een van de volgende methoden gebruiken:

* **Linux:** Volg de instructies de [Azure Storage-clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.
* **Windows:** Klik in Visual Studio **extra &gt; NuGet Package Manager &gt; Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.
  

```powershell
Install-Package wastorage
```

## <a name="set-up-your-application-to-use-azure-files"></a>Instellen van uw toepassing in Azure Files gebruiken

Voeg dat hieronder vindt u instructies toe aan het begin van de C++-bronbestand waar u wilt bewerken, Azure Files:

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Instellen van een Azure storage-verbindingsreeks

Voor het gebruik van File storage, moet u verbinding maken met uw Azure storage-account. De eerste stap is het configureren van een verbindingsreeks, die we gebruiken om verbinding maken met uw opslagaccount. We definiëren een statische variabele om dit te doen.

```cpp
// Define the connection-string with your values.
const utility::string_t
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-to-an-azure-storage-account"></a>Verbinding maken met een Azure storage-account

U kunt de **cloud_storage_account** klasse om weer te geven van de gegevens van uw Opslagaccount. Voor het ophalen van uw opslagaccountgegevens uit de opslagverbindingstekenreeks kunt u de methode **parse** gebruiken.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account =
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Alle bestanden en mappen in een Azure-bestandsshare bevinden zich in een container met de naam een **delen**. Uw storage-account kunt zoveel shares als de accountcapaciteit van uw hebben. Als u wilt verkrijgen van toegang tot een share en de inhoud ervan, moet u een Azure Files-client gebruiken.

```cpp
// Create the Azure Files client.
azure::storage::cloud_file_client file_client =
  storage_account.create_cloud_file_client();
```

U kunt vervolgens een een verwijzing naar een share verkrijgen met behulp van de Azure Files-client.

```cpp
// Get a reference to the file share
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

Gebruik voor het maken van de share de **create_if_not_exists** -methode van de **cloud_file_share** object.

```cpp
if (share.create_if_not_exists()) {
    std::wcout << U("New share created") << std::endl;
}
```

Op dit moment **delen** bevat een verwijzing naar een share met de naam **my-sample-share**.

## <a name="delete-an-azure-file-share"></a>Een Azure-bestandsshare verwijderen

Verwijderen van een share wordt gedaan door het aanroepen van de **delete_if_exists** methode voor het object cloud_file_share. Hier volgt een voorbeeldcode dat dit doet.

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete the share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a>Een map maken

U kunt opslag organiseren door het opslaan van bestanden in submappen in plaats van dat ze allemaal in de hoofdmap. Azure Files kunt u zo veel mappen als uw account kunt maken. De code hieronder maakt u een map met de naam **my-sample-directory** onder de hoofdmap, evenals een submap met de naam **my-sample-submap**.

```cpp
// Retrieve a reference to a directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if the share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a>Een map verwijderen

Verwijderen van een directory is een eenvoudige taak, maar moet worden opgemerkt dat u een map waarin zich nog steeds bestanden of andere directory's niet verwijderen.

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference to the directory.
azure::storage::cloud_file_directory directory =
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference to the subdirectory you want to delete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete the subdirectory and the sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Het inventariseren van bestanden en mappen in een Azure-bestandsshare

Het ophalen van een lijst met bestanden en mappen in een share is eenvoudig uitgevoerd door het aanroepen van **list_files_and_directories** op een **cloud_file_directory** verwijzing. Voor toegang tot de uitgebreide set eigenschappen en methoden voor een geretourneerde **list_file_and_directory_item**, moet u aanroepen de **list_file_and_directory_item.as_file** methode om een **cloud_file**  -object, of de **list_file_and_directory_item.as_directory** methode om een **cloud_file_directory** object.

De volgende code toont hoe u kunt ophalen en de uitvoer van de URI van elk item in de hoofdmap van de share.

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir =
  share.get_root_directory_reference();

// Output URI of each item.
azure::storage::list_file_and_directory_result_iterator end_of_results;

for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
{
    if(it->is_directory())
    {
        ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
    else if (it->is_file())
    {
        ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
    }
}
```

## <a name="upload-a-file"></a>Bestand uploaden

Een Azure-bestandsshare bevat op zijn minst een hoofdmap waarin de bestanden kunnen zich bevinden. In deze sectie leert u hoe u een bestand uit de lokale opslag naar de hoofdmap van een share te uploaden.

De eerste stap bij het uploaden van een bestand is op te halen van een verwijzing naar de map waarin dit zich moet bevinden. U dit doen door het aanroepen van de **get_root_directory_reference** methode van de shareobject.

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

Nu dat u een verwijzing naar de hoofdmap van de share hebt, kunt u een bestand naar het uploaden. In dit voorbeeld wordt geüpload uit een bestand, tekst en een stroom.

```cpp
// Upload a file from a stream.
concurrency::streams::istream input_stream =
  concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

azure::storage::cloud_file file1 =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
file1.upload_from_stream(input_stream);

// Upload some files from text.
azure::storage::cloud_file file2 =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
file2.upload_text(_XPLATSTR("more text"));

// Upload a file from a file.
azure::storage::cloud_file file4 =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
file4.upload_from_file(_XPLATSTR("DataFile.txt"));
```

## <a name="download-a-file"></a>Bestand downloaden

Om bestanden te downloaden, eerst een verwijzing naar een bestand opgehaald en vervolgens roept de **download_to_stream** methode om over te dragen van inhoud van het bestand naar een stroomobject, dat u vervolgens persistent naar een lokaal bestand maken kunt. U kunt ook kunt u de **download_to_file** methode voor het downloaden van de inhoud van een bestand naar een lokaal bestand. U kunt de **download_text** methode voor het downloaden van de inhoud van een bestand op als een tekenreeks met tekst.

Het volgende voorbeeld wordt de **download_to_stream** en **download_text** methoden om te demonstreren downloaden van de bestanden die zijn gemaakt in de vorige secties.

```cpp
// Download as text
azure::storage::cloud_file text_file =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
utility::string_t text = text_file.download_text();
ucout << "File Text: " << text << std::endl;

// Download as a stream.
azure::storage::cloud_file stream_file =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
stream_file.download_to_stream(output_stream);
std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();
outfile.write((char *)&data[0], buffer.size());
outfile.close();
```

## <a name="delete-a-file"></a>Een bestand verwijderen

Een andere algemene Azure Files-bewerking wordt de bestanden hebt verwijderd. De volgende code verwijdert een bestand met de naam my-sample-bestand-3 opgeslagen onder de hoofddirectory.

```cpp
// Get a reference to the root directory for the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir =
  share.get_root_directory_reference();

azure::storage::cloud_file file =
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-the-quota-maximum-size-for-an-azure-file-share"></a>Het quotum (maximumgrootte) voor een Azure-bestandsshare instellen

U kunt het quotum (of de maximale grootte) instellen voor een bestandsshare, in gigabytes. U kunt ook controleren hoeveel gegevens er momenteel zijn opgeslagen in de share.

Door het quotum voor een share in te stellen, kunt u de totale grootte instellen van de bestanden die zijn opgeslagen in de share. Als de totale grootte van de bestanden in de share het quotum voor de share overschrijdt, kunnen clients bestaande bestanden niet meer vergroten en geen nieuwe bestanden meer maken, tenzij deze bestanden leeg zijn.

In onderstaand voorbeeld ziet u hoe u het huidige gebruik voor een share controleert en een quotum voor de share instelt.

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account =
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client.
azure::storage::cloud_file_client file_client =
  storage_account.create_cloud_file_client();

// Get a reference to the share.
azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets the quota to 2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Een Shared Access Signature genereren voor een bestand of bestandsshare

U kunt een shared access signature (SAS) voor een bestandsshare of voor een afzonderlijk bestand genereren. U kunt op een bestandsshare ook een beleid voor gedeelde toegang maken om handtekeningen voor gedeelde toegang te beheren. Het maken van een beleid voor gedeelde toegang wordt aanbevolen omdat dit een manier biedt om de SAS in te trekken als deze verdacht is.

In het volgende voorbeeld wordt een beleid voor gedeelde toegang gemaakt op een share en wordt dat beleid vervolgens gebruikt om de beperkingen te verstrekken voor een SAS voor een bestand in de share.

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account =
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client and get a reference to the share
azure::storage::cloud_file_client file_client =
  storage_account.create_cloud_file_client();

azure::storage::cloud_file_share share =
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

if (share.exists())
{
    // Create and assign a policy
    utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

    azure::storage::file_shared_access_policy sharedPolicy =
      azure::storage::file_shared_access_policy();

    //set permissions to expire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() +
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for the share
    azure::storage::file_share_permissions permissions;

    //retrieve the current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add the new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save the updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve the root directory and file references
    azure::storage::cloud_file_directory root_dir =
        share.get_root_directory_reference();
    azure::storage::cloud_file file =
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in the share
    //  and associate this access policy with it.
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");
    file_with_sas.upload_text(text);

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();
    ucout << downloaded_text << std::endl;
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure Storage:

* [Opslagclientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp)
* [Voorbeelden voor de Service van Azure Storage-bestand in C++](https://github.com/Azure-Samples/storage-file-cpp-getting-started)
* [Azure-opslagverkenner](https://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
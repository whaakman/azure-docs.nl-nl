---
title: Openbare leestoegang voor containers en blobs in Azure Blob-opslag inschakelen | Microsoft Docs
description: Leer hoe u containers en blobs beschikbaar voor anonieme toegang, en hoe u ze via een programma te openen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: e0f93b0a95a228b26fae266129aea4b595b05e0f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148353"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Anonieme leestoegang tot containers en blobs beheren

U kunt anoniem, openbare leestoegang tot een container en de blobs in Azure Blob-opslag inschakelen. Op deze manier kunt u alleen-lezen toegang tot deze resources verlenen zonder het delen van uw accountsleutel en zonder een shared access signature (SAS).

Openbare leestoegang wordt aanbevolen voor scenario's waar u bepaalde blobs moet altijd beschikbaar voor anonieme toegang voor lezen. Voor preciezere beheermogelijkheden kunt u een shared access signature maken. Handtekeningen voor gedeelde toegang kunnen u beperkte toegang met behulp van verschillende machtigingen voor een bepaalde periode te bieden. Voor meer informatie over het maken van gedeelde toegangshandtekeningen, Zie [Using shared access signatures (SAS) in Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonieme gebruikersmachtigingen verlenen voor containers en blobs

Standaard kunnen een container en alle bestaande blobs erin alleen worden geopend door een gebruiker die de juiste machtigingen heeft gekregen. Anonieme gebruikers lezen om toegang te verlenen tot een container en de blobs, kunt u de container openbaar toegangsniveau instellen. Wanneer u openbare toegang tot een container verlenen, kunnen anonieme gebruikers kunnen blobs in een openbaar toegankelijke container lezen zonder dat de aanvraag machtigen.

U kunt een container configureren met de volgende machtigingen:

* **Er zijn geen openbare leestoegang:** De container en de blobs kunnen alleen worden geopend door de eigenaar van het storage-account. Dit is de standaardinstelling voor alle nieuwe containers.
* **Openbare leestoegang alleen voor blobs:** BLOBs in de container kunnen worden gelezen door anonieme aanvraag, maar de containergegevens is niet beschikbaar. Anonieme clients kunnen de blobs in de container niet inventariseren.
* **Openbare leestoegang voor containers en de blobs:** Container- en blob-gegevens kunnen worden gelezen door anonieme aanvraag. Clients kunnen opsommen en blobs in de container door anonieme aanvraag, maar kunnen de containers in het opslagaccount niet opsommen.

U kunt de volgende containermachtigingen in te stellen:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure-CLI](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Via een programma, met behulp van een van de storage-clientbibliotheken of de REST-API

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Container openbaar toegangsniveau instellen in Azure portal

Uit de [Azure-portal](https://portal.azure.com), kunt u het niveau openbare toegang voor een of meer containers bijwerken:

1. Ga in Azure Portal naar uw opslagaccount.
1. Onder **Blob-service** Selecteer op de blade menu **Blobs**.
1. Selecteer de containers waarvoor u wilt instellen van het niveau van de openbare toegang.
1. Gebruik de **toegangsniveau wijzigen** knop om de instellingen van de openbare toegang te geven.
1. Selecteer de gewenste openbaar toegangsniveau van de **openbaar toegangsniveau** vervolgkeuzelijst en klik op de knop OK om de wijziging toepassen op de geselecteerde containers.

De volgende schermafbeelding ziet u hoe u het niveau openbare toegang voor de geselecteerde containers wijzigen.

![Schermopname die laat zien hoe niveau openbare toegang instelt in de portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> U kunt het niveau openbare toegang voor een afzonderlijke blob niet wijzigen. Niveau openbare toegang is ingesteld op het niveau van de container.

### <a name="set-container-public-access-level-with-net"></a>Instellen van de container openbaar toegangsniveau met .NET

Voor het instellen van machtigingen voor een container met behulp van C# en de Storage-clientbibliotheek voor .NET, eerst ophalen van de bestaande machtigingen van de container door het aanroepen van de **GetPermissions** methode. Stel vervolgens de **PublicAccess** eigenschap voor de **BlobContainerPermissions** object dat wordt geretourneerd door de **GetPermissions** methode. Roep ten slotte de **SetPermissions** methode met de bijgewerkte machtigingen.

Het volgende voorbeeld wordt de machtigingen van de container naar volledige openbare leestoegang. Om in te stellen van machtigingen voor openbare leestoegang voor blobs alleen de **PublicAccess** eigenschap **BlobContainerPublicAccessType.Blob**. Als u wilt verwijderen van alle machtigingen voor anonieme gebruikers, kunt u de eigenschap instellen op **BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Toegang tot containers en blobs anoniem

Een client die toegang heeft tot containers en blobs anoniem kan constructors waarvoor geen referenties gebruiken. De volgende voorbeelden ziet een aantal verschillende manieren om te verwijzen naar containers en blobs anoniem.

### <a name="create-an-anonymous-client-object"></a>Een anonieme clientobject maken

U kunt een nieuwe service-clientobject voor anonieme toegang maken door op te geven van het eindpunt van Blob storage voor het account. U moet echter ook de naam van een container in het account dat beschikbaar is voor anonieme toegang weten.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Een container anoniem verwijzen naar

Als u de URL naar een container die anoniem beschikbaar hebt, kunt u deze rechtstreeks verwijzen naar de container.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Een blob anoniem verwijzen naar

Als u de URL naar een blob die beschikbaar is voor anonieme toegang hebt, kunt u verwijzen naar de blob die URL rechtstreeks gebruiken:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Functies die beschikbaar zijn voor anonieme gebruikers

De volgende tabel ziet u welke bewerkingen kunnen worden aangeroepen anoniem wanneer een container is geconfigureerd voor openbare toegang.

| REST-bewerking | Openbare leestoegang tot de container | Openbare leestoegang alleen voor blobs |
| --- | --- | --- |
| Lijst met Containers | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container maken | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Eigenschappen van de Container ophalen | Anonieme aanvragen dat is toegestaan | Alleen geautoriseerde aanvragen |
| De Containermetagegevens van de ophalen | Anonieme aanvragen dat is toegestaan | Alleen geautoriseerde aanvragen |
| De Containermetagegevens van de instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container ACL ophalen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container ACL instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container verwijderen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Lijst met Blobs | Anonieme aanvragen dat is toegestaan | Alleen geautoriseerde aanvragen |
| Blob plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blob ophalen | Anonieme aanvragen dat is toegestaan | Anonieme aanvragen dat is toegestaan |
| Blobeigenschappen ophalen | Anonieme aanvragen dat is toegestaan | Anonieme aanvragen dat is toegestaan |
| Blobeigenschappen instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| De metagegevens van de blob ophalen | Anonieme aanvragen dat is toegestaan | Anonieme aanvragen dat is toegestaan |
| Instellen van de metagegevens van de Blob | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blok plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Lijst met geblokkeerde websites (alleen toegezegde blokken) ophalen | Anonieme aanvragen dat is toegestaan | Anonieme aanvragen dat is toegestaan |
| Lijst met geblokkeerde websites (alleen niet-doorgevoerde blokken of alle blokken) ophalen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Lijst met geblokkeerde websites plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blob verwijderen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blob kopiëren | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Momentopname maken van Blob | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Lease-Blob | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Pagina plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Get Page-bereiken | Anonieme aanvragen dat is toegestaan | Anonieme aanvragen dat is toegestaan |
| Blob toevoegen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |

## <a name="next-steps"></a>Volgende stappen

* [Autorisatie voor de Azure Storage-Services](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-azure-storage-services)
* [Shared Access Signatures (SAS) gebruiken](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
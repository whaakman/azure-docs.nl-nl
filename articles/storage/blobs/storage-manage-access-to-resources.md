---
title: Openbare leestoegang voor containers en blobs in Azure Blob-opslag inschakelen | Microsoft Docs
description: Leer hoe u containers en blobs beschikbaar voor anonieme toegang, en hoe u ze via een programma te openen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/26/2017
ms.author: tamram
ms.openlocfilehash: bf7dabc1c3765d86e7a0f87acaa6f06a68d3d530
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782005"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Anonieme leestoegang tot containers en blobs beheren
U kunt anoniem, openbare leestoegang tot een container en de blobs in Azure Blob-opslag inschakelen. Op deze manier kunt u alleen-lezen toegang tot deze resources verlenen zonder het delen van uw accountsleutel en zonder een shared access signature (SAS).

Openbare leestoegang wordt aanbevolen voor scenario's waar u bepaalde blobs moet altijd beschikbaar voor anonieme toegang voor lezen. Voor preciezere beheermogelijkheden kunt u een shared access signature maken. Handtekeningen voor gedeelde toegang kunnen u beperkte toegang met behulp van verschillende machtigingen voor een bepaalde periode te bieden. Voor meer informatie over het maken van gedeelde toegangshandtekeningen, Zie [Using shared access signatures (SAS) in Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonieme gebruikersmachtigingen verlenen voor containers en blobs
Standaard kunnen een container en alle bestaande blobs erin alleen worden geopend door de eigenaar van het storage-account. U kunt de containermachtigingen voor openbare toegang tot instellen zodat anonieme gebruikers leesmachtigingen heeft voor een container en de blobs. Anonieme gebruikers kunnen blobs in een openbaar toegankelijke container lezen zonder verificatie van de aanvraag.

U kunt een container configureren met de volgende machtigingen:

* **Er zijn geen openbare leestoegang:** de container en de blobs kunnen alleen worden geopend door de eigenaar van het storage-account. Dit is de standaardinstelling voor alle nieuwe containers.
* **Openbare leestoegang alleen voor blobs:** Blobs in de container kunnen worden gelezen door anonieme aanvraag, maar de containergegevens is niet beschikbaar. Anonieme clients kunnen de blobs in de container niet inventariseren.
* **Openbare leestoegang volledige:** alle container en blob-gegevens kunnen worden gelezen door anonieme aanvraag. Clients kunnen opsommen en blobs in de container door anonieme aanvraag, maar kunnen de containers in het opslagaccount niet opsommen.

U kunt de volgende containermachtigingen in te stellen:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure CLI 2.0](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Via een programma, met behulp van een van de storage-clientbibliotheken of de REST-API

### <a name="set-container-permissions-in-the-azure-portal"></a>Container-machtigingen instellen in Azure portal
Containermachtigingen instellen de [Azure-portal](https://portal.azure.com), als volgt te werk:

1. Open uw **opslagaccount** -blade in de portal. U vindt uw storage-account door te selecteren **opslagaccounts** in de blade van de portal in het hoofdmenu.
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **Blobs**.
1. Met de rechtermuisknop op de rij van de container of Selecteer het weglatingsteken te openen van de container **contextmenu**.
1. Selecteer **toegangsbeleid** in het contextmenu.
1. Selecteer een **toegangstype** in de vervolgkeuzelijst.

    ![Dialoogvenster Containermetagegevens bewerken](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>Container-machtigingen instellen met .NET
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
Een client die toegang heeft tot containers en blobs anoniem kan constructors waarvoor geen referenties gebruiken. De volgende voorbeelden ziet een aantal verschillende manieren om te verwijzen anoniem naar Blob-service-resources.

### <a name="create-an-anonymous-client-object"></a>Een anonieme clientobject maken
U kunt een nieuwe service-clientobject voor anonieme toegang maken door op te geven van het eindpunt van Blob service voor het account. U moet echter ook de naam van een container in het account dat beschikbaar is voor anonieme toegang weten.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
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
De volgende tabel ziet u welke bewerkingen door anonieme gebruikers kunnen worden aangeroepen wanneer de ACL van de container is ingesteld voor openbare toegang tot.

| REST-bewerking | Machtiging met de volledige openbare leestoegang | Machtiging met de openbare leestoegang alleen voor blobs |
| --- | --- | --- |
| Lijst met Containers |Alleen de eigenaar |Alleen de eigenaar |
| Container maken |Alleen de eigenaar |Alleen de eigenaar |
| Eigenschappen van de Container ophalen |Alle |Alleen de eigenaar |
| De Containermetagegevens van de ophalen |Alle |Alleen de eigenaar |
| De Containermetagegevens van de instellen |Alleen de eigenaar |Alleen de eigenaar |
| Container ACL ophalen |Alleen de eigenaar |Alleen de eigenaar |
| Container ACL instellen |Alleen de eigenaar |Alleen de eigenaar |
| Container verwijderen |Alleen de eigenaar |Alleen de eigenaar |
| Lijst met Blobs |Alle |Alleen de eigenaar |
| Blob plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Blob ophalen |Alle |Alle |
| Blobeigenschappen ophalen |Alle |Alle |
| Blobeigenschappen instellen |Alleen de eigenaar |Alleen de eigenaar |
| De metagegevens van de blob ophalen |Alle |Alle |
| Instellen van de metagegevens van de Blob |Alleen de eigenaar |Alleen de eigenaar |
| Blok plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Lijst met geblokkeerde websites (alleen toegezegde blokken) ophalen |Alle |Alle |
| Lijst met geblokkeerde websites (alleen niet-doorgevoerde blokken of alle blokken) ophalen |Alleen de eigenaar |Alleen de eigenaar |
| Lijst met geblokkeerde websites plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Blob verwijderen |Alleen de eigenaar |Alleen de eigenaar |
| Blob kopiëren |Alleen de eigenaar |Alleen de eigenaar |
| Momentopname maken van Blob |Alleen de eigenaar |Alleen de eigenaar |
| Lease-Blob |Alleen de eigenaar |Alleen de eigenaar |
| Pagina plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Get Page-bereiken |Alle |Alle |
| Toevoeg-Blob |Alleen de eigenaar |Alleen de eigenaar |

## <a name="next-steps"></a>Volgende stappen

* [Verificatie voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Shared Access Signatures (SAS) gebruiken](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Toegang delegeren met een Shared Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx)

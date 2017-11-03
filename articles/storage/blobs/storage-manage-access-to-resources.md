---
title: Openbare leestoegang voor containers en blobs in Azure Blob-opslag inschakelen | Microsoft Docs
description: Informatie over hoe u containers en blobs beschikbaar voor anonieme toegang en hoe programmatisch toegang toe hebben.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: tamram
ms.openlocfilehash: f52079c72be298daaa45074e516f911022780392
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Anonieme leestoegang tot containers en blobs beheren
U kunt anonieme, openbare leestoegang tot een container en de blobs in Azure Blob storage inschakelen. Op deze manier kunt u alleen-lezen toegang tot deze bronnen verlenen zonder het delen van de accountsleutel en zonder een shared access signature (SAS).

Openbare leestoegang wordt aanbevolen voor scenario's waar u wilt dat bepaalde blobs altijd alleen beschikbaar voor anonieme toegang voor lezen. Voor nauwkeuriger beheer, kunt u een shared access signature maken. Handtekeningen voor gedeelde toegang kunnen u beperkte toegang met andere machtigingen voor een bepaalde periode. Voor meer informatie over het maken van gedeelde handtekeningen krijgen, Zie [Using shared access signatures (SAS) in Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonieme gebruikers machtigen tot containers en blobs
Standaard kunnen een container en alle bestaande blobs erin alleen worden geopend door de eigenaar van het opslagaccount. Als u anonieme gebruikers leesmachtigingen voor een container en de blobs geven, kunt u de container-machtigingen voor openbare toegang instellen. Anonieme gebruikers kunnen BLOB's binnen een container openbaar toegankelijk lezen zonder verificatie van de aanvraag.

U kunt een container configureren met de volgende machtigingen:

* **Geen enkele openbare leestoegang:** de container en de blobs kunnen alleen worden geopend door de eigenaar van het opslagaccount. Dit is de standaardwaarde voor alle nieuwe containers.
* **Openbare leestoegang voor blobs alleen:** Blobs in de container kunnen worden gelezen door anonieme aanvraag, maar de containergegevens is niet beschikbaar. Anonieme clients kunnen de blobs in de container niet opsommen.
* **Volledige openbare leestoegang:** alle container en de blob-gegevens kunnen worden gelezen door anonieme aanvraag. Clients anonieme aanvraag kunnen opsommen blobs in de container, maar kunnen de containers in het opslagaccount niet inventariseren.

U kunt de volgende container machtigingen worden ingesteld:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure CLI 2.0](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Programmatisch met behulp van een van de opslag-clientbibliotheken of de REST-API

### <a name="set-container-permissions-in-the-azure-portal"></a>Container machtigingen instellen in de Azure portal
Container machtigingen instellen de [Azure-portal](https://portal.azure.com), als volgt te werk:

1. Open uw **opslagaccount** blade in de portal. U kunt uw storage-account vinden door te selecteren **opslagaccounts** in de portal hoofdmenu-blade.
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **Containers**.
1. Met de rechtermuisknop op de rij container of Selecteer het weglatingsteken openen van de container **contextmenu**.
1. Selecteer **toegangsbeleid** in het contextmenu.
1. Selecteer een **toegangstype** in de vervolgkeuzelijst.

    ![Dialoogvenster metagegevens bewerken](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>Container machtigingen instellen met .NET
Als machtigingen wilt instellen voor een container met C# en de Storage-clientbibliotheek voor .NET, eerst ophalen van de container bestaande machtigingen door het aanroepen van de **GetPermissions** methode. Stel de **PublicAccess** eigenschap voor de **BlobContainerPermissions** -object dat wordt geretourneerd door de **GetPermissions** methode. Tenslotte roept de **SetPermissions** methode met de bijgewerkte machtigingen.

Het volgende voorbeeld wordt de container machtigingen voor volledige openbare leestoegang. Om in te stellen van machtigingen voor openbare leestoegang voor blobs alleen de **PublicAccess** eigenschap **BlobContainerPublicAccessType.Blob**. De eigenschap voor het verwijderen van alle machtigingen voor anonieme gebruikers instellen op **BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Toegang tot containers en blobs anoniem
Een client die toegang heeft tot containers en blobs anoniem kan constructors waarvoor geen referenties gebruiken. De volgende voorbeelden ziet diverse manieren om te verwijzen naar van Blob-serviceresources anoniem.

### <a name="create-an-anonymous-client-object"></a>Een clientobject anonieme maken
U kunt een nieuw serviceobject voor een client voor anonieme toegang maken door het eindpunt van de Blob-service voor het account. U moet echter ook de naam van een container in het account dat beschikbaar is voor anonieme toegang kennen.

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

### <a name="reference-a-container-anonymously"></a>Een container anoniem verwijst naar
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

### <a name="reference-a-blob-anonymously"></a>Een blob anoniem verwijst naar
Als u de URL naar een blob die beschikbaar is voor anonieme toegang hebt, kunt u verwijzen naar de blob die URL direct met:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Functies die beschikbaar voor anonieme gebruikers
De volgende tabel ziet u welke bewerkingen door anonieme gebruikers kunnen worden aangeroepen wanneer een container-ACL is ingesteld op het toestaan van openbare toegang.

| REST-bewerking | Machtiging met volledige openbare leestoegang | Machtiging met openbare leestoegang voor blobs alleen |
| --- | --- | --- |
| Lijst Containers |Alleen de eigenaar |Alleen de eigenaar |
| Container maken |Alleen de eigenaar |Alleen de eigenaar |
| Eigenschappen van Container ophalen |Alle |Alleen de eigenaar |
| Container metagegevens ophalen |Alle |Alleen de eigenaar |
| Metagegevens van de Container instellen |Alleen de eigenaar |Alleen de eigenaar |
| Container-ACL ophalen |Alleen de eigenaar |Alleen de eigenaar |
| Container-ACL instellen |Alleen de eigenaar |Alleen de eigenaar |
| Verwijderen van Container |Alleen de eigenaar |Alleen de eigenaar |
| Lijst met BLOB 's |Alle |Alleen de eigenaar |
| Blob plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Ophalen van Blob |Alle |Alle |
| Blob-eigenschappen ophalen |Alle |Alle |
| Blob-eigenschappen instellen |Alleen de eigenaar |Alleen de eigenaar |
| Blobmetagegevens ophalen |Alle |Alle |
| Blobmetagegevens instellen |Alleen de eigenaar |Alleen de eigenaar |
| Blok plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Ophalen van lijst met geblokkeerde websites (alleen doorgevoerde blokken) |Alle |Alle |
| Ophalen van lijst met geblokkeerde websites (alleen niet-doorgevoerde blokken of alle blokken) |Alleen de eigenaar |Alleen de eigenaar |
| Lijst met geblokkeerde websites plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Verwijderen van Blob |Alleen de eigenaar |Alleen de eigenaar |
| Blob kopiëren |Alleen de eigenaar |Alleen de eigenaar |
| Momentopname Blob |Alleen de eigenaar |Alleen de eigenaar |
| Lease Blob |Alleen de eigenaar |Alleen de eigenaar |
| Pagina plaatsen |Alleen de eigenaar |Alleen de eigenaar |
| Get-paginabereiken |Alle |Alle |
| Toevoeg-Blob |Alleen de eigenaar |Alleen de eigenaar |

## <a name="next-steps"></a>Volgende stappen

* [Verificatie voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Met behulp van Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Toegang delegeren met een Shared Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx)

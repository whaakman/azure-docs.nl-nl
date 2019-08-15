---
title: Open bare Lees toegang voor containers en blobs in Azure Blob-opslag inschakelen | Microsoft Docs
description: Meer informatie over het maken van containers en blobs beschikbaar voor anonieme toegang en hoe u deze via een programma kunt openen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 6293fc84969c4e246c05da4482f76142263db230
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985547"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Anonieme leestoegang tot containers en blobs beheren

U kunt anonieme, open bare Lees toegang inschakelen voor een container en de bijbehorende blobs in Azure Blob-opslag. Door dit te doen, kunt u alleen-lezen toegang verlenen aan deze resources zonder uw account sleutel te delen en zonder een Shared Access Signature (SAS).

Open bare Lees toegang is het beste voor scenario's waarin u wilt dat bepaalde blobs altijd beschikbaar zijn voor anonieme lees toegang. Voor meer nauw keurige controle kunt u een gedeelde toegangs handtekening maken. Met hand tekeningen voor gedeelde toegang kunt u beperkte toegang bieden met behulp van verschillende machtigingen, voor een specifieke periode. Zie [using Shared Access signatures (SAS) (Engelstalig) in azure Storage](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)voor meer informatie over het maken van hand tekeningen voor gedeelde toegang.

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonieme gebruikers machtigingen verlenen aan containers en blobs

Standaard kan een container en alle blobs in deze alleen worden geopend door een gebruiker die de juiste machtigingen heeft gekregen. Als u anonieme gebruikers lees toegang wilt verlenen voor een container en de bijbehorende blobs, kunt u het open bare toegangs niveau van de container instellen. Wanneer u open bare toegang verleent aan een container, kunnen anonieme gebruikers blobs lezen binnen een openbaar toegankelijke container zonder de aanvraag te autoriseren.

U kunt een container met de volgende machtigingen configureren:

* **Geen open bare Lees toegang:** De container en de blobs kunnen alleen worden geopend door de eigenaar van het opslag account. Dit is de standaard waarde voor alle nieuwe containers.
* **Open bare Lees toegang alleen voor blobs:** Blobs in de container kunnen worden gelezen door anonieme aanvragen, maar er zijn geen container gegevens beschikbaar. Anonieme clients kunnen de blobs in de container niet inventariseren.
* **Open bare Lees toegang voor container en de bijbehorende blobs:** Alle container-en BLOB-gegevens kunnen door anonieme aanvragen worden gelezen. Clients kunnen blobs in de container inventariseren door anonieme aanvragen, maar kunnen containers niet inventariseren binnen het opslag account.

U kunt het volgende gebruiken om container machtigingen in te stellen:

* [Azure-portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure-CLI](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Programmatisch, met behulp van een van de opslag-client bibliotheken of de REST API

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Stel een openbaar toegangs niveau in voor de container in de Azure Portal

Vanuit het [Azure Portal](https://portal.azure.com)kunt u het niveau van open bare toegang voor een of meer containers bijwerken:

1. Ga in Azure Portal naar uw opslagaccount.
1. Selecteer onder **BLOB service** op de menu Blade de optie **blobs**.
1. Selecteer de containers waarvoor u het niveau van open bare toegang wilt instellen.
1. Gebruik de knop **toegangs niveau wijzigen** om de instellingen voor open bare toegang weer te geven.
1. Selecteer het gewenste open bare toegangs niveau van de vervolg keuzelijst **openbaar toegangs niveau** en klik op de knop OK om de wijziging toe te passen op de geselecteerde containers.

De volgende scherm afbeelding laat zien hoe u het niveau van open bare toegang voor de geselecteerde containers kunt wijzigen.

![Scherm afbeelding die laat zien hoe u het niveau van open bare toegang instelt in de portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> U kunt het niveau van de open bare toegang voor een afzonderlijke BLOB niet wijzigen. Niveau van open bare toegang wordt alleen op container niveau ingesteld.

### <a name="set-container-public-access-level-with-net"></a>Het niveau van open bare toegang tot de container instellen met .NET

Als u machtigingen wilt instellen voor een C# container met en de Storage-client bibliotheek voor .net, moet u eerst de bestaande machtigingen van de container ophalen door de methode **GetPermissions** aan te roepen. Stel vervolgens de eigenschap **PublicAccess** in voor het **BlobContainerPermissions** -object dat wordt geretourneerd door de methode **GetPermissions** . Roep ten slotte de **SetPermissions** -methode aan met de bijgewerkte machtigingen.

In het volgende voor beeld worden de machtigingen van de container ingesteld op volledige open bare Lees toegang. Als u machtigingen wilt instellen op open bare Lees toegang voor blobs, stelt u de eigenschap **PublicAccess** in op **BlobContainerPublicAccessType. blob**. Als u alle machtigingen voor anonieme gebruikers wilt verwijderen, stelt u de eigenschap in op **BlobContainerPublicAccessType. off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Anoniem toegang krijgen tot containers en blobs

Een client die toegang heeft tot containers en blobs anoniem kan gebruikmaken van constructors waarvoor geen referenties zijn vereist. In de volgende voor beelden ziet u een aantal verschillende manieren om te verwijzen naar containers en blobs anoniem.

### <a name="create-an-anonymous-client-object"></a>Een anoniem client object maken

U kunt een nieuw service-client object voor anonieme toegang maken door het eind punt van de Blob-opslag voor het account op te geven. U moet echter ook de naam weten van een container in dat account die beschikbaar is voor anonieme toegang.

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

### <a name="reference-a-container-anonymously"></a>Anoniem verwijzen naar een container

Als u de URL naar een container hebt die anoniem beschikbaar is, kunt u deze gebruiken om rechtstreeks naar de container te verwijzen.

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

### <a name="reference-a-blob-anonymously"></a>Anoniem verwijzen naar een BLOB

Als u de URL naar een BLOB hebt die beschikbaar is voor anonieme toegang, kunt u rechtstreeks naar de BLOB verwijzen met behulp van die URL:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Functies die beschikbaar zijn voor anonieme gebruikers

In de volgende tabel ziet u welke bewerkingen anoniem kunnen worden aangeroepen wanneer een container wordt geconfigureerd voor open bare toegang.

| REST-bewerking | Open bare Lees toegang tot container | Openbare leestoegang alleen voor blobs |
| --- | --- | --- |
| Containers weer geven | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container maken | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container eigenschappen ophalen | Anonieme aanvragen toegestaan | Alleen geautoriseerde aanvragen |
| Meta gegevens van container ophalen | Anonieme aanvragen toegestaan | Alleen geautoriseerde aanvragen |
| Meta gegevens van container instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container-ACL ophalen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container-ACL instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Container verwijderen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blobs weer geven | Anonieme aanvragen toegestaan | Alleen geautoriseerde aanvragen |
| BLOB plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| BLOB ophalen | Anonieme aanvragen toegestaan | Anonieme aanvragen toegestaan |
| BLOB-eigenschappen ophalen | Anonieme aanvragen toegestaan | Anonieme aanvragen toegestaan |
| BLOB-eigenschappen instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| De metagegevens van de blob ophalen | Anonieme aanvragen toegestaan | Anonieme aanvragen toegestaan |
| BLOB-meta gegevens instellen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blok keren | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blokkerings lijst ophalen (alleen toegewezen blokken) | Anonieme aanvragen toegestaan | Anonieme aanvragen toegestaan |
| Blokkerings lijst ophalen (alleen niet-toegewezen blokken of alle blokken) | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blokkerings lijst plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blob verwijderen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Blob kopiëren | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Moment opname-BLOB | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Lease-BLOB | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Pagina plaatsen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |
| Paginabereiken ophalen | Anonieme aanvragen toegestaan | Anonieme aanvragen toegestaan |
| Blob toevoegen | Alleen geautoriseerde aanvragen | Alleen geautoriseerde aanvragen |

## <a name="next-steps"></a>Volgende stappen

* [Autorisatie voor de Azure Storage services](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-azure-storage-services)
* [Shared Access signatures (SAS) gebruiken](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
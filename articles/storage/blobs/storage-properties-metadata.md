---
title: Instellen en ophalen van metagegevens in Azure Storage en eigenschappen van het object | Microsoft Docs
description: Aangepaste metagegevens voor objecten in Azure Storage, opslaan en instelt en ophaalt Systeemeigenschappen.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: tamram
ms.openlocfilehash: a3eb598b2dabd4986c72b8814926eb0944707050
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Eigenschappen en metagegevens instellen en ophalen

Objecten in Azure Storage ondersteuning Systeemeigenschappen en de gebruiker gedefinieerde metagegevens, naast de gegevens die ze bevatten. Dit artikel worden de eigenschappen van het beheren en de gebruiker gedefinieerde metagegevens met de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **Systeemeigenschappen**: Systeemeigenschappen op elke opslagresource bestaan. Enkele van deze kunnen worden gelezen of ingesteld, terwijl andere alleen-lezen. Achter de correspondeert sommige Systeemeigenschappen met bepaalde standaard HTTP-headers. De Azure storage-clientbibliotheek houdt deze voor u.

* **Gebruiker gedefinieerde metagegevens**: gebruiker gedefinieerde metagegevens zijn metagegevens die u op een bepaalde bron in de vorm van een naam / waarde-paar opgeeft. U kunt metagegevens voor het opslaan van aanvullende waarden met een opslagresource gebruiken. Deze aanvullende metagegevens-waarden zijn voor uw eigen doeleinden alleen en hebben geen invloed op de werking van de resource.

Bij het ophalen van eigenschap en metagegevens waarden voor een opslagresource is een proces. Voordat u deze waarden lezen kunt, u moet expliciet halen ze door het aanroepen van de **FetchAttributesAsync** methode.

> [!IMPORTANT]
> Eigenschap en metagegevens waarden voor een opslagresource zijn niet gevuld als u een van de **FetchAttributesAsync** methoden.
>
> U ontvangt een `400 Bad Request` als een naam/waarde-paren niet-ASCII-tekens bevatten. Naam/waarde-paren metagegevens geldig HTTP-headers zijn en dus moeten voldoen aan alle beperkingen voor HTTP-headers. Het is daarom raadzaam dat u URL-codering of Base64-codering voor namen en waarden die niet-ASCII-tekens bevat.
>

## <a name="setting-and-retrieving-properties"></a>Bijwerken en het ophalen van eigenschappen
Aanroepen voor het ophalen van eigenschapswaarden, de **FetchAttributesAsync** methode op uw blob of een container voor het vullen van de eigenschappen, leest u de waarden.

Eigenschappen worden ingesteld op een object, geef dan de eigenschap value, roep vervolgens de **SetProperties** methode.

Het volgende codevoorbeeld maakt een container en sommige van de eigenschapswaarden schrijft naar een consolevenster.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
await container.FetchAttributesAsync();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Instellen en ophalen van metagegevens
U kunt metagegevens opgeven als een of meer naam / waarde-paren van een blob of container resource. Metagegevens stelt toevoegen naam-waardeparen voor de **metagegevens** verzameling voor de bron, roept u vervolgens de **SetMetadata** methode voor het opslaan van de waarden in de service.

> [!NOTE]
> De naam van uw metagegevens moet voldoen aan de naamgeving van C#-id's.
>
>

Het volgende codevoorbeeld stelt metagegevens in een container. Een waarde is ingesteld met behulp van de verzameling **toevoegen** methode. De andere waarde is ingesteld met impliciete sleutel/waarde-syntaxis. Beide zijn geldig.

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    // Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    // Set the container's metadata.
    await container.SetMetadataAsync();
}
```

Aanroepen voor het ophalen van metagegevens van de **FetchAttributes** methode op uw blob of een container voor het vullen van de **metagegevens** verzameling, lees de waarden, zoals wordt weergegeven in het onderstaande voorbeeld.

```csharp
public static async Task ListContainerMetadataAsync(CloudBlobContainer container)
{
    // Fetch container attributes in order to populate the container's properties and metadata.
    await container.FetchAttributesAsync();

    // Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a>Volgende stappen
* [Azure Storage-clientbibliotheek voor .NET-verwijzing](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [Azure Storage-clientbibliotheek voor .NET-NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.Storage/)

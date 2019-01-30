---
title: Een momentopname van een alleen-lezen van een blob maken in Azure Storage | Microsoft Docs
description: Informatie over het maken van een momentopname van een blob naar de back-up van blob-gegevens op een bepaald tijdstip in de tijd. Begrijp hoe momentopnamen worden gefactureerd en hoe u kunt gebruiken voor het minimaliseren van capaciteit, kosten in rekening gebracht.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: ac13b40ae58054b091963de198213c1a68fcdc05
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244841"
---
# <a name="create-a-blob-snapshot"></a>Een blob-momentopname maken

Een momentopname is een alleen-lezen versie van een blob die moet worden uitgevoerd op een bepaald tijdstip. Momentopnamen zijn handig voor het back-ups van blobs. Nadat u een momentopname gemaakt, kunt u lezen, kopiëren of verwijderen, maar u kunt dit niet wijzigen.

Een momentopname van een blob is identiek aan de basis-blob, behalve dat de blob-URI is een **datum-/** waarde toegevoegd aan de blob-URI om aan te geven van de tijd waarop de momentopname werd gemaakt. Bijvoorbeeld, als een pagina blob-URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, de momentopname-URI vergelijkbaar met is `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Alle momentopnamen delen van de basis blob-URI. De enige verschil tussen de base blob en de momentopname is de toegevoegde **datum-/** waarde.
>

Een blob kan een onbeperkt aantal momentopnamen hebben. Momentopnamen blijft actief totdat ze expliciet worden verwijderd. Een momentopname kan niet de base blob outlive. U kunt de momentopnamen die zijn gekoppeld aan de basis-blob voor het bijhouden van uw huidige momentopnamen opsommen.

Wanneer u een momentopname van een blob maakt, worden de eigenschappen van de blob gekopieerd naar de momentopname met dezelfde waarden. De base blobmetagegevens wordt ook gekopieerd naar de momentopname, tenzij u afzonderlijke metagegevens voor de momentopname opgeeft wanneer u deze maakt.

De momentopname niet van invloed op alle leases die zijn gekoppeld aan de basis-blob. U kunt een lease op een momentopname kan niet verkrijgen.

Een VHD-bestand wordt gebruikt voor het opslaan van de huidige informatie en status voor een VM-schijf. U kunt een schijf loskoppelen van binnen de virtuele machine of de virtuele machine af en vervolgens maakt u een momentopname van de VHD-bestand. U kunt dat momentopnamebestand later gebruiken voor het ophalen van het VHD-bestand op dat punt in tijd en opnieuw maken van de virtuele machine.

## <a name="create-a-snapshot"></a>Een momentopname maken
Het volgende codevoorbeeld laat zien hoe u een momentopname maakt met behulp van de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). In dit voorbeeld bevat aanvullende metagegevens voor de momentopname wanneer deze wordt gemaakt.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="copy-snapshots"></a>Momentopnamen kopiëren
Kopieerbewerkingen met betrekking tot blobs en momentopnamen volgt u deze regels:

* U kunt een momentopname van een via de base blob kopiëren. Door het promoveren van een momentopname met de positie van de basis-blob, kunt u een eerdere versie van een blob herstellen. De momentopname-blijven, maar de base blob wordt overschreven met een beschrijfbare kopie van de momentopname.
* U kunt een momentopname van een kopiëren naar een bestemmings-blob met een andere naam. De resulterende bestemmings-blob is een beschrijfbare blob en niet een momentopname.
* Als een bron-blob is gekopieerd, wordt alle momentopnamen van de bron-blob worden niet gekopieerd naar de bestemming. Wanneer een bestemmings-blob wordt overschreven met behulp van een kopie, blijven alle momentopnamen die zijn gekoppeld aan het oorspronkelijke bestemmings-blob intact.
* Wanneer u een momentopname van een blok-blob maakt, wordt ook vastgelegd van de blob-blokkeringslijst gekopieerd naar de momentopname. Een niet-doorgevoerde blokken zijn niet gekopieerd.

## <a name="specify-an-access-condition"></a>Geef een voorwaarde voor toegang
Als u aanroept [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], kunt u een voorwaarde toegang opgeven, zodat de momentopname alleen gemaakt wordt als een voorwaarde wordt voldaan. Als u een voorwaarde toegang, gebruik de [AccessCondition] [ dotnet_AccessCondition] parameter. Als niet aan de opgegeven voorwaarde wordt voldaan, de momentopname niet is gemaakt en de Blob-service statuscode retourneert [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Momentopnamen verwijderen
U kunt een blob met momentopnamen niet verwijderen, tenzij de momentopnamen worden ook verwijderd. U kunt een momentopname van een afzonderlijk verwijderen of opgeven dat alle momentopnamen worden verwijderd wanneer de bron-blob wordt verwijderd. Als u probeert te verwijderen van een blob die nog steeds momentopnamen bevat, wordt er een fout resulteert.

Het volgende codevoorbeeld laat zien hoe een blob en de momentopnamen in .NET, te verwijderen waar `blockBlob` is een object van het type [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Momentopnamen met Azure Premium Storage
Als u momentopnamen voor Premium-opslag, gelden de volgende regels:

* Het maximum aantal momentopnamen per pagina-blobs in een premium storage-account is 100. Als deze limiet wordt overschreden, de momentopname maken van Blob-bewerking retourneert foutcode 409 (`SnapshotCountExceeded`).
* U kunt een momentopname van een pagina-blob in een premium storage-account om de tien minuten. Als dit percentage wordt overschreden, de momentopname maken van Blob-bewerking retourneert foutcode 409 (`SnapshotOperationRateExceeded`).
* Om te lezen van een momentopname, kunt u de bewerking kopiëren van de Blob te kopiëren van een momentopname naar een andere paginablob in het account. De bestemmings-blob voor de kopieerbewerking moet alle bestaande momentopnamen niet hebben. Als de bestemmings-blob momentopnamen hebt, wordt het kopiëren van de Blob-bewerking foutcode 409 retourneert (`SnapshotsPresent`).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Retourneert de absolute URI zijn die naar een momentopname
Dit C#-codevoorbeeld wordt een momentopname gemaakt en schrijft de absolute URI zijn die voor de primaire locatie.

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Begrijpen hoe de kosten in rekening gebracht voor het samenvoegen van momentopnamen
Het maken van een momentopname een alleen-lezen kopie van een blob is, kan leiden tot extra kosten voor opslag aan uw account. Bij het ontwerpen van uw toepassing, is het belangrijk dat u zich bewust zijn van hoe deze kosten toenemen kunnen, zodat u kosten kunt minimaliseren.

### <a name="important-billing-considerations"></a>Belangrijke overwegingen voor facturering
De volgende lijst bevat belangrijke punten om te overwegen bij het maken van een momentopname.

* Uw storage-account leidt tot kosten voor voor unieke blokken of pagina's, ongeacht of deze in de blob of in de momentopname. Uw account heeft geen extra kosten voor momentopnamen die zijn gekoppeld aan een blob, totdat u de blob op waarop ze zijn gebaseerd bijwerken. Nadat u de basis blob bijgewerkt, wordt het afwijkt van de momentopnamen ervan verwijderd. Als dit gebeurt, worden in rekening gebracht voor de unieke blokken of pagina's in elke blob of een momentopname.
* Wanneer u een blok in een blok-blob vervangt, dat blok wordt later in rekening gebracht als een unieke blok. Dit geldt zelfs als het blok heeft dezelfde blok-ID en de dezelfde gegevens er in de momentopname. Nadat het blok is het belangrijk opnieuw het afwijkt van het bijbehorende equivalent van een momentopname, en u wordt gefactureerd voor de gegevens. Hetzelfde geldt voor een pagina in een pagina-blob die wordt bijgewerkt met identieke gegevens.
* Een blok-blob vervangen door het aanroepen van de [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], of [UploadFromByteArray] [ dotnet_UploadFromByteArray] methode vervangt alle blokken in de blob. Alle blokken in de basis blob en momentopname luidsprekers nu hebt u een momentopname die is gekoppeld aan die blob, en u wordt gefactureerd voor de blokken in beide blobs. Dit geldt zelfs als de gegevens in de basis-blob en de momentopname identiek.
* De Azure Blob-service heeft geen een manier om te bepalen of twee blokken identieke gegevens bevatten. Elk blok dat is geüpload en verwerkt wordt beschouwd als uniek is, zelfs als deze dezelfde gegevens en de dezelfde blok-ID heeft. Omdat kosten voor unieke blokken toenemen, is het belangrijk te bedenken dat een blob die de resultaten van een momentopname in als u meer unieke blokken en extra kosten in rekening is bijgewerkt.

### <a name="minimize-cost-with-snapshot-management"></a>Kosten met beheer van momentopnamen minimaliseren

Het is raadzaam om voor het beheren van uw momentopnamen zorgvuldig om te voorkomen dat extra kosten in rekening gebracht. U kunt deze aanbevolen procedures voor het minimaliseren van de kosten van de opslag van uw momentopnamen volgen:

* Verwijderen en opnieuw maken van momentopnamen die zijn gekoppeld aan een blob wanneer u de blob bijwerken, zelfs als u met identieke gegevens, bijwerken wilt tenzij het ontwerp van uw toepassing vereist dat u momentopnamen behoudt. Door te verwijderen en opnieuw te maken van de blob-momentopnamen, kunt u ervoor zorgen dat de blob en de momentopnamen niet luidsprekers.
* Als u momentopnamen voor een blob, te voorkomen dat aanroepen [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], of [UploadFromByteArray] [ dotnet_UploadFromByteArray] om bij te werken van de blob. Deze methoden vervangen van alle de blokken in de blob, waardoor uw basis-blob en de momentopnamen sterk afwijkt. In plaats daarvan het minst mogelijke aantal blokken bijwerken met behulp van de [PutBlock] [ dotnet_PutBlock] en [PutBlockList] [ dotnet_PutBlockList] methoden.

### <a name="snapshot-billing-scenarios"></a>Momentopname van facturering van scenario 's
De volgende scenario's laten zien hoe de kosten voor een blok-blob en de momentopnamen ervan verwijderd toenemen.

**Scenario 1**

In scenario 1, is de base blob niet bijgewerkt nadat de momentopname is gemaakt, zodat kosten in voor unieke blokken 1, 2 en 3 gebracht rekening.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Scenario 2**

In scenario 2, de base blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is bijgewerkt en zelfs als deze de dezelfde gegevens en dezelfde ID bevat, het is niet hetzelfde als 3 in de momentopname geblokkeerd. Als gevolg hiervan het account wordt in rekening gebracht voor vier blokken.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Scenario 3**

In scenario 3, de base blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is vervangen door blok 4 in de basis-blob, maar de momentopname weerspiegelt nog steeds block 3. Als gevolg hiervan het account wordt in rekening gebracht voor vier blokken.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Scenario 4**

In scenario 4, de base blob volledig is bijgewerkt en geen van de oorspronkelijke blokken bevat. Als gevolg hiervan het account wordt in rekening gebracht voor alle acht unieke blokken. Dit kan gebeuren als u een update-methode, zoals [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], of [UploadFromByteArray][dotnet_UploadFromByteArray], omdat deze methoden vervangen van alle van de inhoud van een blob.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Volgende stappen

* U vindt meer informatie over het werken met momentopnamen van de virtuele machine (VM)-schijf in [maakt u een Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen](../../virtual-machines/windows/incremental-snapshots.md)

* Zie voor voorbeelden van aanvullende code met behulp van Blob-opslag, [Azure-codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). U kunt een voorbeeld-App downloaden en uitvoeren, of de code bekijken op GitHub.

[dotnet_AccessCondition]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.accesscondition.aspx
[dotnet_CloudBlockBlob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx
[dotnet_CreateSnapshotAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.createsnapshotasync.aspx
[dotnet_HTTPStatusCode]: https://msdn.microsoft.com/library/system.net.httpstatuscode(v=vs.110).aspx
[dotnet_PutBlockList]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblocklist.aspx
[dotnet_PutBlock]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblock.aspx
[dotnet_UploadFromByteArray]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfrombytearray.aspx
[dotnet_UploadFromFile]: https://msdn.microsoft.com/library/azure/mt705654.aspx
[dotnet_UploadFromStream]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstream.aspx
[dotnet_UploadText]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadtext.aspx

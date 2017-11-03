---
title: Maken van een momentopname van een alleen-lezen van een blob in Azure Storage | Microsoft Docs
description: Informatie over het maken van een momentopname van een blob naar de back-up van blob-gegevens op een gegeven moment. Begrijpen hoe momentopnamen worden gefactureerd en het gebruik ervan capaciteit, kosten te minimaliseren.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 3710705d-e127-4b01-8d0f-29853fb06d0d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: tamram
ms.openlocfilehash: 7e891018ab110e7506601cd5b9b0460bf61711b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-blob-snapshot"></a>Een blob-momentopname maken

Een momentopname is een alleen-lezen-versie van een blob die wordt uitgevoerd op een punt in tijd. Momentopnamen zijn nuttig voor back-up blobs. Nadat u een momentopname gemaakt, lezen, kopiëren of verwijderen, maar u kunt dit niet wijzigen.

Een momentopname van een blob is identiek aan de base blob, behalve dat de blob-URI bevat een **DateTime** waarde toegevoegd aan de blob-URI om aan te geven van de tijd waarop de momentopname werd gemaakt. Bijvoorbeeld, als een pagina blob-URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, de momentopname van de URI vergelijkbaar met is `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Alle momentopnamen delen de base blob-URI. Het enige verschil tussen de base blob en de momentopname is de toegevoegde **DateTime** waarde.
>

Een blob kan een onbeperkt aantal momentopnamen hebben. Momentopnamen bewaard totdat ze expliciet worden verwijderd. Een momentopname kan niet de base blob outlive. U kunt de momentopnamen die zijn gekoppeld aan de basis blob om bij te houden van uw huidige momentopnamen opsommen.

Wanneer u een momentopname van een blob maakt, worden de blob-Systeemeigenschappen gekopieerd naar de momentopname met dezelfde waarden. De base blob metagegevens wordt ook gekopieerd naar de momentopname, tenzij u afzonderlijke metagegevens voor de momentopname opgeven wanneer u dit hebt gemaakt.

Alle leases die zijn gekoppeld aan de basis blob hebben geen invloed op de momentopname. U kunt een lease op een momentopname kan niet verkrijgen.

Een VHD-bestand wordt gebruikt voor het opslaan van de huidige informatie en status voor een VM-schijf. U kunt loskoppelen van een schijf uit vanuit de virtuele machine of sluit de virtuele machine en vervolgens een momentopname van de VHD-bestand. U kunt dat momentopnamebestand later gebruiken voor het ophalen van het VHD-bestand op dat moment en de virtuele machine opnieuw.

Versleuteling voor opslag-Service (SSE) is ingeschakeld voor het opslagaccount waarin de blob zich bevindt, wordt alle momentopnamen gehouden met de blob worden versleuteld in rust.

## <a name="create-a-snapshot"></a>Een momentopname maken
De volgende voorbeeldcode laat zien hoe een momentopname te maken met behulp van de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). In dit voorbeeld bevat aanvullende metagegevens voor de momentopname wanneer deze wordt gemaakt.

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

## <a name="copy-snapshots"></a>Kopiëren van momentopnamen
Kopieerbewerkingen met betrekking tot de blobs en momentopnamen van voldoen deze regels:

* U kunt een momentopname via de base blob kopiëren. Door een momentopname met de positie van de basis blob te promoveren, kunt u een eerdere versie van een blob herstellen. De momentopname blijft, maar de base blob wordt overschreven met een beschrijfbare kopie van de momentopname.
* U kunt een momentopname kopiëren naar een bestemmings-blob met een andere naam. De resulterende bestemmings-blob is een beschrijfbare blob en niet een momentopname.
* Als een bron-blob zijn gekopieerd, worden alle momentopnamen van de bron-blob niet gekopieerd naar de bestemming. Wanneer een bestemmings-blob is overschreven door een exemplaar, zijn de momentopnamen die zijn gekoppeld aan de oorspronkelijke bestemmings-blob blijven behouden.
* Bij het maken van een momentopname van een blok-blob is ook de blob doorgevoerd blokkeringslijst gekopieerd naar de momentopname. Een niet-doorgevoerde blokken worden niet gekopieerd.

## <a name="specify-an-access-condition"></a>Geef een voorwaarde voor toegang
Als u aanroept [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], kunt u een voorwaarde voor toegang opgeven, zodat de momentopname alleen gemaakt wordt als een voorwaarde wordt voldaan. Als u een voorwaarde voor toegang, gebruik de [AccessCondition] [ dotnet_AccessCondition] parameter. Als niet aan de opgegeven voorwaarde wordt voldaan, wordt de momentopname niet gemaakt is en de Blob-service statuscode retourneert [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Verwijder momentopnamen
U kunt een blob met de momentopnamen niet verwijderen, tenzij de momentopnamen worden ook verwijderd. U kunt een momentopname van een afzonderlijk verwijderen of opgeven dat alle momentopnamen worden verwijderd wanneer de bron-blob is verwijderd. Als u probeert te verwijderen van een blob die nog steeds momentopnamen bevat, wordt een fout resulteert.

De volgende voorbeeldcode laat zien hoe een blob en bijbehorende momentopnamen in .NET, te verwijderen waar `blockBlob` is een object van het type [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Momentopnamen met Azure Premium-opslag
Als u momentopnamen met Premium-opslag, gelden de volgende regels:

* Het maximum aantal momentopnamen per pagina-blob in een premium storage-account is 100. Als deze limiet wordt overschreden, de momentopname Blob-bewerking retourneert foutcode 409 (`SnapshotCountExceeded`).
* U kunt een momentopname van een pagina-blob nemen in een premium storage-account om de 10 minuten. Als dit percentage wordt overschreden, de momentopname Blob-bewerking retourneert foutcode 409 (`SnapshotOperationRateExceeded`).
* Om te lezen van een momentopname, kunt u de bewerking Blob kopiëren naar een momentopname kopiëren naar een andere paginablob in het account. De bestemmings-blob voor de kopieerbewerking mag geen eventuele bestaande momentopnamen. Als de bestemmings-blob momentopnamen heeft, wordt de kopie Blob-bewerking foutcode 409 retourneert (`SnapshotsPresent`).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Retourneert de absolute URI naar een momentopname
In dit voorbeeld C#-code maakt een momentopname en schrijft de absolute URI voor de primaire locatie.

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

## <a name="understand-how-snapshots-accrue-charges"></a>Begrijpen hoe de kosten voor het samenvoegen van momentopnamen
Maken van een momentopname een alleen-lezen kopie van een blob is, kan leiden tot aanvullende gegevens opslagkosten aan uw account. Bij het ontwerpen van uw toepassing, is het belangrijk te weten van hoe deze kosten doorlopen kunnen, zodat u de kosten kunt minimaliseren.

### <a name="important-billing-considerations"></a>Belangrijke overwegingen voor facturering
De volgende lijst bevat de belangrijkste punten in overweging moet nemen bij het maken van een momentopname.

* Uw opslagaccount leidt ertoe dat de kosten voor unieke blokken of pagina's, ongeacht of deze in de blob of in de momentopname. Uw account heeft geen gevolgen voor de extra kosten voor momentopnamen die zijn gekoppeld met een blob, totdat u de blob waarop ze zijn gebaseerd bijwerkt. Na het bijwerken van de basis blob, wordt deze afwijkt van de momentopnamen. Als dit gebeurt, wordt u in rekening gebracht voor de unieke blokken of pagina's in elke blob of een momentopname.
* Wanneer u een blok binnen een blok-blob vervangt, dat blok wordt vervolgens in rekening gebracht als unieke blok. Dit geldt zelfs als het blok heeft dezelfde blok-ID en dezelfde gegevens er in de momentopname. Nadat het blok doorgevoerd is opnieuw het afwijkt van het bijbehorende equivalent in een momentopname en wordt u gefactureerd voor de gegevens. Hetzelfde geldt voor een pagina in een pagina-blob bijgewerkt met identieke gegevens.
* Een blok-blob vervangen door het aanroepen van de [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], of [UploadFromByteArray] [ dotnet_UploadFromByteArray] vervangt alle blokken in de blob-methode. Als er een momentopname die is gekoppeld aan blob, alle blokken in de basis-blob en momentopname nu luidsprekers en wordt u gefactureerd voor alle blokken in beide blobs. Dit geldt zelfs als de gegevens in de base blob en de momentopname identiek.
* De Azure Blob-service heeft geen een manier om te bepalen of twee blokken identieke gegevens bevatten. Elk blok dat wordt geüpload en doorgevoerd wordt behandeld als uniek zijn, zelfs als deze dezelfde gegevens en de dezelfde blok-ID heeft. Omdat kosten voor unieke blokken doorlopen, is het belangrijk in die voor het bijwerken van een blob met een momentopname resulteert in extra unieke blokken en extra kosten.

### <a name="minimize-cost-with-snapshot-management"></a>Kosten met beheer van momentopnamen beperken

Het is raadzaam om het beheer van uw momentopnamen zorgvuldig om te voorkomen dat extra kosten. U kunt deze best practices te helpen de kosten van de opslag van uw momentopnamen minimaliseren volgen:

* Verwijderen en opnieuw maken van momentopnamen die zijn gekoppeld met een blob telkens wanneer u de blob bijwerkt, zelfs als u met identieke gegevens bijwerken wilt tenzij ontwerp van uw toepassing vereist is voor het onderhouden van momentopnamen. Door te verwijderen en opnieuw maken van de blob-momentopnamen, kunt u ervoor zorgen dat de blob en momentopnamen niet luidsprekers.
* Als u momentopnamen voor een blob en onderhoudt voorkomen dat de aanroepen [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], of [UploadFromByteArray] [ dotnet_UploadFromByteArray] bijwerken van de blob. Deze methoden vervangen van alle van de gegevensblokken die zich in de blob, waardoor uw base blob en bijbehorende momentopnamen aanzienlijk afwijkt. In plaats daarvan het minste aantal blokken bijwerken met behulp van de [PutBlock] [ dotnet_PutBlock] en [PutBlockList] [ dotnet_PutBlockList] methoden.

### <a name="snapshot-billing-scenarios"></a>Momentopname facturering scenario 's
De volgende scenario's laten zien hoe de kosten voor een blok-blob en bijbehorende momentopnamen doorlopen.

**Scenario 1**

In scenario 1, is de base blob niet bijgewerkt nadat de momentopname werd gemaakt, zodat de kosten verbonden zijn alleen voor unieke blokken 1, 2 en 3.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Scenario 2**

In scenario 2 de base blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is bijgewerkt en hoewel het bevat dezelfde gegevens en dezelfde ID, het is niet hetzelfde als 3 in de momentopname geblokkeerd. Als gevolg hiervan het account wordt in rekening gebracht voor vier blokken.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Scenario 3**

In scenario 3 de base blob is bijgewerkt, maar de momentopname niet heeft. Blok 3 is vervangen door blok 4 in de base blob, maar de momentopname weerspiegelt nog steeds blok 3. Als gevolg hiervan het account wordt in rekening gebracht voor vier blokken.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Scenario 4**

In scenario 4 de base blob volledig is bijgewerkt en geen van de oorspronkelijke blokken bevat. Het account wordt hierdoor alle acht unieke blokken belast. Dit kan gebeuren als u een update-methode, zoals [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], of [UploadFromByteArray][dotnet_UploadFromByteArray], omdat deze methoden vervangen van alle van de inhoud van een blob.

![Azure Storage-resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Volgende stappen

* U vindt meer informatie over het werken met momentopnamen van virtuele machine (VM)-schijf in [Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen](../../virtual-machines/windows/incremental-snapshots.md)

* Zie voor aanvullende codevoorbeelden met behulp van Blob-opslag, [Azure codevoorbeelden](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Een voorbeeld van een toepassing te downloaden en uitvoeren, of de code op GitHub bladeren.

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
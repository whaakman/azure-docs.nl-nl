---
title: De unieke functies van Azure-pagina-blobs | Microsoft Docs
description: Een overzicht van Azure-pagina-blobs en hun voordelen, waaronder gebruiksvoorbeelden met voorbeelden van scripts.
services: storage
author: anasouma
ms.service: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: wielriac
ms.component: blobs
ms.openlocfilehash: a215771b0126e9048b7d9da4ed1d6073c8e960a4
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265731"
---
# <a name="unique-features-of-azure-page-blobs"></a>De unieke functies van Azure-pagina-blobs

Azure Storage biedt drie typen van blob-opslag: blok-Blobs, toevoeg-Blobs en pagina-blobs. Blok-blobs bestaan uit blokken en zijn ideaal voor het opslaan van tekst of binaire bestanden en voor het uploaden van grote bestanden efficiënt. Toevoeg-blobs ook bestaan uit blokken, maar ze zijn geoptimaliseerd voor toevoegbewerkingen, waardoor ze ideaal voor scenario's voor logboekregistratie. Pagina-blobs bestaan uit 512-byte-pagina's omhoog naar 8 TB in totale grootte en zijn ontworpen voor frequente willekeurige lees-/ schrijfbewerkingen. Pagina-blobs vormen de basis van Azure IaaS-schijven. In dit artikel is gericht op uitleg van de functies en voordelen van pagina-blobs.

Pagina-blobs zijn een verzameling van 512-byte-pagina's, waardoor de mogelijkheid om willekeurige bereiken in bytes voor lezen/schrijven. Daarom kan de pagina-blobs zijn ideaal voor het opslaan van gegevens op basis van een index en sparse-structuren, zoals besturingssysteem en gegevensschijven voor virtuele Machines en Databases. Bijvoorbeeld, Azure SQL-database maakt gebruik van pagina-blobs als de onderliggende permanente opslag voor de databases. Pagina-blobs worden bovendien ook vaak gebruikt voor bestanden die zijn bijgewerkt op basis van een bereik.  

Belangrijke functies van Azure-pagina-blobs zijn de REST-interface, de duurzaamheid van de onderliggende opslag en de mogelijkheden voor naadloze migratie naar Azure. Deze functies worden gedetailleerd besproken in de volgende sectie beschreven. Bovendien Azure pagina-blobs die momenteel worden ondersteund op twee soorten opslag: Premium-opslag- en Standard-opslag. Premium Storage is speciaal ontworpen voor workloads waarvoor consistent hoge prestaties en lage latentie, waardoor premium-pagina-blobs ideaal voor databases voor opslag van krachtige gegevens vereist.  Standard-opslag is kostenefficiënter voor het uitvoeren van niet gevoelig werkbelastingen.

## <a name="sample-use-cases"></a>Voorbeelden van use cases

Laten we bespreken enkele gebruiksvoorbeelden voor pagina-blobs die beginnen met Azure IaaS-schijven. Azure-pagina-blobs vormen de ruggengraat van de virtuele schijven platform voor Azure IaaS. Azure-besturingssysteem en de gegevensschijven worden geïmplementeerd als virtuele schijven waaruit gegevens worden blijvend opgeslagen in het Azure Storage-platform en vervolgens geleverd aan de virtuele machines voor maximale prestaties. Azure-schijven zijn opgeslagen in Hyper-V [VHD-indeling](https://technet.microsoft.com/library/dd979539.aspx) en opgeslagen als een [pagina-blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) in Azure Storage. Naast het gebruik van virtuele schijven voor Azure IaaS VM's, inschakelen pagina-blobs ook voor PaaS en DBaaS scenario's zoals Azure SQL DB-service, die momenteel pagina-blobs gebruikt voor het opslaan van SQL-gegevens, zodat snel, willekeurig lees-/ schrijfbewerkingen voor de database. Een ander voorbeeld zou zijn als u een PaaS-service voor toegang tot gedeelde media voor samenwerkingstoepassingen video bewerken hebt, pagina-blobs snelle toegang tot willekeurige locaties in de media inschakelen. Ook kunt met snelle en efficiënte bewerken en samenvoegen van de dezelfde media door meerdere gebruikers. 

Eerste partij Microsoft-services zoals Azure Site Recovery, Azure Backup, evenals veel ontwikkelaars van derden hebben toonaangevende innovaties met behulp van pagina-blob REST-interface geïmplementeerd. Hier volgen enkele van de unieke scenario's geïmplementeerd op Azure: 
* Beheer van incrementele momentopnamen toepassing omgeleid: toepassingen kunnen gebruikmaken van pagina-blob-momentopnamen en REST-API's voor het opslaan van de controlepunten van de toepassing zonder kostbare duplicatie van gegevens. Azure Storage biedt ondersteuning voor lokale momentopnamen voor pagina-blobs, die geen kopiëren van de gehele blob is vereist. Deze momentopname van de openbare API's kunnen ook toegang tot en het kopiëren van de verschillen tussen momentopnamen.
* Livemigratie van toepassingen en gegevens van on-premises naar de cloud: de on-premises gegevens kopiëren en gebruiken van REST-API's om te schrijven rechtstreeks naar een Azure-pagina-blob, terwijl de on-premises virtuele machine blijft om uit te voeren. Wanneer het doel is bijgewerkt, kunt u snel een failover voor Azure-VM met behulp van die gegevens. Op deze manier kunt u uw virtuele machines migreren en virtuele schijven van on-premises naar de cloud met minimale downtime, omdat de migratie van gegevens op de achtergrond treedt op wanneer u echter ook doorgaan met de virtuele machine en de downtime die nodig is voor failover zijn kort (in minuten).
* [Op basis van een SAS](../common/storage-dotnet-shared-access-signature-part-1.md) gedeelde toegang, waardoor scenario's met meerdere lezers en één-schrijver met ondersteuning voor gelijktijdigheidsbeheer.

## <a name="page-blob-features"></a>Functies voor pagina-blobs

### <a name="rest-api"></a>REST-API
Raadpleeg het volgende document aan de slag met [ontwikkelen met behulp van pagina-blobs](storage-dotnet-how-to-use-blobs.md). Bekijk bijvoorbeeld over toegang tot pagina-blobs met behulp van Storage-clientbibliotheek voor .NET. 

Het volgende overzicht beschrijft de algehele relaties tussen de account, containers en pagina-blobs.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-specified-size"></a>Het maken van een lege pagina-blob van een opgegeven grootte
Voor het maken van een pagina-blob, maken we eerst een **CloudBlobClient** object met de basis-URI voor de toegang tot de blob-opslag voor uw storage-account (*pbaccount* in afbeelding 1) samen met de  **StorageCredentialsAccountAndKey** -object, zoals wordt weergegeven in het volgende voorbeeld. In het voorbeeld wordt vervolgens het maken van een verwijzing naar een **CloudBlobContainer** object en klikt u vervolgens het maken van de container (*testvhds*) als deze nog niet bestaat. Vervolgens met behulp van de **CloudBlobContainer** object, maakt u een verwijzing naar een **CloudPageBlob** object met de naam van de pagina-blob (os4.vhd) om toegang te geven. Aanroepen voor het maken van de pagina-blob, [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) doorgeven in de maximale grootte voor de blob te maken. De *blobSize* moet een meervoud van 512 bytes.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Formaat van een pagina-blob
Als u wilt het formaat van een pagina-blob na het maken, gebruikt u de [vergroten of verkleinen](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. De aangevraagde grootte moet een meervoud zijn van 512 bytes.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Pagina's schrijven naar een pagina-blob
Voor het schrijven van pagina's, gebruikt u de [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) methode.  Hiermee kunt u een opeenvolgende reeks's tot 4MBs schrijven. De verschuiving naar worden geschreven moet starten op een grens van een 512-byte (startingOffset % 512 == 0), en eindigen op een 512 grens - 1.  Het volgende codevoorbeeld laat zien hoe u aan te roepen **WritePages** voor een blob:

```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Als u een aanvraag voor een opeenvolgende reeks pagina's in de blob-service is voltooid en wordt gerepliceerd voor duurzaamheid en meer flexibiliteit, de schrijfbewerking is doorgevoerd en succes terug naar de client wordt geretourneerd.  

Het onderstaande diagram laat zien 2 afzonderlijke schrijfbewerkingen:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Een schrijfbewerking vanaf offset 0 met een lengte van 1024 bytes 
2.  Een schrijfbewerking beginnen bij verschuiving 4096 van de lengte van 1024 

#### <a name="reading-pages-from-a-page-blob"></a>Gelezen pagina's van een pagina-blob
Gelezen pagina's, gebruikt u de [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) methode voor het lezen van een bereik van bytes van de pagina-blob. Hiermee kunt u de volledige blob of een bereik van bytes vanaf de verschuiving in de blob te downloaden. Bij het lezen, heeft de verschuiving niet starten op een veelvoud van 512 bytes. Bij het lezen van bytes van een pagina met NUL, retourneert de service nul bytes.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
De volgende afbeelding toont een leesbewerking met BlobOffSet van 256 en rangeSize van 4352. Gegevens die worden geretourneerd is in oranje gemarkeerd. Nullen worden geretourneerd voor pagina's met NUL

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Als u een facturatiemodel ingevuld blob hebt, kunt u alleen de regio geldige pagina's om te voorkomen dat betaalt voor egressing van nul bytes en verminder de latentie van de download te downloaden.  Gebruiken om te bepalen welke pagina's worden ondersteund door de gegevens, [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). U kunt vervolgens het inventariseren van de geretourneerde bereiken en downloaden van de gegevens in elk bereik. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### <a name="leasing-a-page-blob"></a>Overdracht van een pagina-blob
De Lease-Blob-bewerking wordt tot stand gebracht en een vergrendeling voor een blob beheert voor schrijven en verwijderen van bewerkingen. Met deze bewerking is handig in situaties waar een pagina-blob van meerdere clients om ervoor te zorgen slechts één client tegelijk kunt schrijven naar de blob wordt geopend. Azure-schijven, bijvoorbeeld: maakt gebruik van deze lease-mechanisme om te controleren of de schijf alleen wordt beheerd door een enkele virtuele machine. De vergrendelingsduur van de kan 15 tot 60 seconden, of kunt oneindige. Zie de documentatie [hier](/rest/api/storageservices/lease-blob) voor meer informatie.

> Gebruik de volgende koppeling om op te halen [codevoorbeelden](/resources/samples/?service=storage&term=blob&sort=0 ) voor veel andere scenario's de toepassing. 

Naast uitgebreide REST-API's en bieden pagina-blobs ook gedeelde toegang, duurzaamheid en verbeterde beveiliging. We komen aan bod die voordelen in de volgende alinea's in meer detail. 

### <a name="concurrent-access"></a>Gelijktijdige toegang
De pagina-blobs REST-API en de leasemechanisme zorgt ervoor dat toepassingen toegang hebben tot de pagina-blob vanaf meerdere clients. Stel dat u wilt maken van een gedistribueerde cloud-service die opslagobjecten met meerdere gebruikers deelt. Het kan een webtoepassing die een grote verzameling van installatiekopieën voor meerdere gebruikers zijn. Een optie voor het implementeren van dit is het gebruik van een virtuele machine met gekoppelde schijven. De nadelen van deze toevoegen (i) de beperking van een schijf kan alleen worden gekoppeld aan een enkele virtuele machine zo beperken van de schaalbaarheid, flexibiliteit en risico's te vergroten. Als er een probleem met de virtuele machine of de service wordt uitgevoerd op de virtuele machine, vanwege de lease is de installatiekopie niet toegankelijk totdat de lease is verlopen of beschadigd is; en (ii) extra kosten van een IaaS-VM met. 

Een alternatief is het gebruik van de pagina-blobs rechtstreeks via Azure Storage REST-API's. Deze optie elimineert de noodzaak voor kostbare IaaS-VM's, biedt volledige flexibiliteit van directe toegang vanaf meerdere clients, vereenvoudigt het klassieke implementatiemodel door het elimineren van de noodzaak om te koppelen en loskoppelen van schijven en wordt voorkomen dat de kans op problemen op de virtuele machine. En het biedt dezelfde mate van prestaties voor willekeurige lees-/ schrijfbewerkingen als een schijf

### <a name="durability-and-high-availability"></a>Duurzaamheid en hoge beschikbaarheid
Standard en premium storage zijn duurzame opslag waar-de pagina-blob-gegevens worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Zie voor meer informatie over Azure-Opslagredundantie [documentatie](../common/storage-redundancy.md). Azure heeft geavanceerde duurzaamheid consistent geleverd voor IaaS-schijven en pagina-blobs, met een toonaangevende nul % [foutpercentage op jaarbasis](https://en.wikipedia.org/wiki/Annualized_failure_rate). Dat wil zeggen, heeft Azure nooit van een klant pagina-blob-gegevens verloren. 

### <a name="seamless-migration-to-azure"></a>Naadloze migratie naar azure
Voor de klanten en ontwikkelaars die geïnteresseerd zijn in de implementatie van hun eigen aangepaste back-upoplossing, biedt Azure ook incrementele momentopnamen die alleen de delta's bevatten. Deze functie wordt voorkomen dat de kosten van de eerste volledige kopie, die aanzienlijk de kosten voor back-up verlaagt. Naast de mogelijkheid om efficiënt lezen en differentiële gegevens kopiëren is dit een andere krachtige functie waarmee u nog meer innovaties van ontwikkelaars die leiden tot een best mogelijke back-ups en disaster recovery (DR)-ervaring op Azure. U kunt instellen van uw eigen back-up of DR-oplossing voor uw virtuele machines in Azure met behulp [Blob-momentopname](/rest/api/storageservices/snapshot-blob) samen met de [Get Page-bereiken](/rest/api/storageservices/get-page-ranges) API en de [incrementeel kopiëren van de Blob](/rest/api/storageservices/incremental-copy-blob) API, die u kunt Gebruik voor het snel kopiëren van de incrementele gegevens voor herstel na Noodgevallen. 

Bovendien hebben veel ondernemingen die essentiële workloads die al wordt uitgevoerd in de on-premises datacenters. Voor het migreren van de werkbelasting naar de cloud, zou een van de belangrijkste betrekking heeft op de hoeveelheid uitvaltijd beperkt die nodig zijn voor het kopiëren van de gegevens en het risico van onvoorziene problemen na de overschakeling zijn. In veel gevallen kan de downtime een verbluffend veel bekijks voor migratie naar de cloud zijn. REST-API met behulp van de pagina-blobs, Azure pakken dit probleem door het inschakelen van de cloudmigratie met minimale verstoring voor essentiële workloads. 

Voor voorbeelden van hoe u een momentopname te maken en herstellen van een pagina-blob van een momentopname, raadpleegt u de [instellen van een back-upproces met incrementele momentopnamen](../../virtual-machines/windows/incremental-snapshots.md) artikel.

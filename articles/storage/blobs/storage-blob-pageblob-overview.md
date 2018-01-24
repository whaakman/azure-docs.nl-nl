---
title: Unieke functie van Azure-pagina-blobs | Microsoft Docs
description: Overzicht van de Azure pagina-blobs, voordelen en gebruiksvoorbeelden met voorbeelden van scripts.
services: storage
documentationcenter: 
author: anasouma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: wielriac
ms.openlocfilehash: 019b793f6d2b4cb70514d867b78c9501240baeda
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="unique-features-of-azure-page-blobs"></a>Unieke kenmerken van de Azure-pagina-Blobs

Azure Storage biedt drie typen van blob storage: blok-Blobs, toevoeg-Blobs en pagina-Blobs. Blok-blobs zijn samengesteld uit blokken en zijn ideaal voor het opslaan van tekst of binaire bestanden en voor het uploaden van grote bestanden efficiënt. Toevoeg-blobs ook bestaan uit blokken, maar ze zijn geoptimaliseerd voor toevoegbewerkingen, waardoor ze ideaal voor logboekscenario's. Pagina-blobs zijn opgebouwd uit 512-byte-pagina's tot maximaal 8 TB in de totale grootte en zijn efficiënter voor regelmatige willekeurige lees-en schrijfbewerkingen. Pagina-blobs vormen de basis van Azure IaaS-schijven. Dit artikel is gericht op uitleg van de functies en voordelen van pagina-Blobs.

## <a name="overview"></a>Overzicht
Pagina-Blobs zijn een verzameling van 512-byte pagina's de mogelijkheid om te lezen bieden/schrijven willekeurige bereiken in bytes. Pagina-Blobs zijn daarom ideaal voor het opslaan van de index en sparse gegevensstructuren zoals besturingssysteem en gegevensschijven voor virtuele Machines en Databases. Bijvoorbeeld: Azure SQL-database maakt gebruik van pagina-blobs als de onderliggende permanente opslag voor de databases. Pagina-Blobs zijn bovendien ook vaak gebruikt voor bestanden die zijn bijgewerkt op basis van het bereik.  

Belangrijke functies van Azure-pagina-Blobs zijn de REST-interface, de duurzaamheid van de onderliggende opslag en de mogelijkheden voor naadloze migratie naar Azure. Bespreken we deze functies in de volgende sectie voor meer informatie. Bovendien Azure-pagina-Blobs die momenteel worden ondersteund op twee soorten opslag: Premium-opslag- en Standard-opslag. Premium-opslag is speciaal ontworpen voor werkbelastingen vereisen consistent hoge prestaties en lage latentie premium-pagina-blobs ideaal voor hoge zodat gegevens storage-databases te maken.  Standard-opslag is rendabeler om latentie hoofdlettergevoelig werkbelastingen uit te voeren.

## <a name="sample-use-cases"></a>Voorbeeld gebruiksvoorbeelden

Bespreken we enkele gebruiksvoorbeelden voor pagina-Blobs die beginnen met Azure IaaS-schijven. Azure-pagina-Blobs vormen de ruggengraat van de virtuele schijven platform voor Azure IaaS. Azure-besturingssysteem en de gegevensschijven worden geïmplementeerd als virtuele schijven, waarbij gegevens worden blijvend persistent in de Azure Storage-platform en vervolgens aan de virtuele machines voor maximale prestaties geleverd. Azure-schijven worden doorgevoerd in Hyper-V [VHD-indeling](https://technet.microsoft.com/library/dd979539.aspx) en opgeslagen als een [pagina-Blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) in Azure Storage. Naast het gebruik van virtuele schijven voor Azure IaaS VM's, inschakelen pagina-Blobs ook PaaS en DBaaS scenario's zoals Azure SQL DB-service, die momenteel pagina-Blobs gebruikt voor het opslaan van de SQL-gegevens, snel, willekeurig lees-/ schrijfbewerkingen voor de database inschakelen. Een ander voorbeeld zou zijn als er een PaaS-service voor gedeelde media-toegang voor toepassingen voor het samenwerken video bewerken, pagina-blobs snel toegang tot willekeurige locaties in de media inschakelen. Ook kunt met snelle en efficiënte bewerken en samenvoegen van de dezelfde media door meerdere gebruikers. 

Eerste partij Microsoft-services zoals Azure Site Recovery, back-up van Azure, evenals veel ontwikkelaars van de derde partij hebben toonaangevende innovaties met behulp van pagina-blob REST-interface geïmplementeerd. Hieronder volgen enkele van de unieke scenario's in Azure worden geïmplementeerd: 
* Beheer van incrementele momentopnamen toepassing omgeleid: toepassingen kunnen gebruikmaken van pagina-Blob momentopnamen en REST-API's voor het opslaan van de controlepunten van de toepassing zonder kostbare duplicatie van gegevens. Dit is mogelijk omdat er een lokale momentopnamen ondersteuning voor pagina-blobs, die niet vereist voor het kopiëren van de hele gegevens. Deze openbare momentopname-API's kunnen ook toegang tot en het kopiëren van de delta's tussen momentopnamen.
* Livemigratie van de toepassing en gegevens van on-premises naar cloud: de on-premises gegevens kopiëren en REST-API's gebruiken om te schrijven rechtstreeks naar de pagina-Blob van Azure terwijl de on-premises virtuele machine nog wordt uitgevoerd. Wanneer het doel heeft de achterstand bij, kunt u snel failover naar de virtuele machine in Azure met behulp van die gegevens. Hierdoor is de migratie van uw virtuele machines en virtuele schijven van on-premises naar cloud met minimale downtime, omdat de gegevensmigratie terwijl u blijven gebruiken van de virtuele machine en de uitvaltijd nodig is voor failover plaatsvindt op de achtergrond worden korte (in minuten).
* [Op basis van een SAS](../common/storage-dotnet-shared-access-signature-part-1.md) gedeelde toegang, waardoor scenario's mogelijk, zoals meerdere lezers en één writer met ondersteuning voor gelijktijdigheidbeheer.

## <a name="page-blob-features"></a>Pagina-Blob-functies

### <a name="rest-api"></a>REST-API
Raadpleeg het volgende document aan de slag met [ontwikkelen met behulp van pagina-Blobs](storage-dotnet-how-to-use-blobs.md). Als u bijvoorbeeld laat het ons bekijken over het openen van pagina-Blobs met Storage-clientbibliotheek voor .NET. 

Het volgende overzicht beschrijft de algehele relaties tussen account, containers en pagina-blobs.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-certain-size"></a>Maken van een lege pagina-Blob van een bepaalde grootte
Voor het maken van een pagina-Blob maken we eerst een CloudBlobClient-object met de basis-Uri voor toegang tot de blobopslag voor uw opslagaccount (pbaccount in afbeelding 1) samen met het object StorageCredentialsAccountAndKey zoals hieronder wordt weergegeven.  Voorbeeld ziet u vervolgens een verwijzing naar een object CloudBlobContainer maakt en vervolgens de container (testvhds) te maken als deze niet al bestaat.  Vervolgens wordt met het object CloudBlobContainer, maken we een verwijzing naar een object CloudPageBlob door te geven van de pagina blob-naam (os4.vhd) we willen openen. Vervolgens te maken van de paginablob we noemen [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) doorgegeven in de maximale grootte voor de blob die we willen maken.  De blobSize moet een veelvoud van 512 bytes zijn.

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

#### <a name="resizing-a-page-blob"></a>Een pagina-Blob vergroten of verkleinen
Als u een pagina-Blob na het maken, gebruikt u de [vergroten of verkleinen](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. De aangevraagde grootte moet een veelvoud van 512 bytes zijn.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Schrijven van pagina's naar een pagina-Blob
Voor het schrijven van pagina's, gebruiken de [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) methode.  Hiermee kunt u een opeenvolgende reeks's tot 4MBs schrijven. De verschuiving naar worden geschreven moet starten op de grens van een 512-byte (startingOffset % 512 == 0), en eindigen op een 512 grens - 1.  De volgende code toont een voorbeeld van het aanroepen van WritePages voor een blob-object dat we toegang hebben tot:
```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Als u een aanvraag voor een opeenvolgende reeks's erin slaagt de blob-service en is gerepliceerd voor duurzaamheid en tolerantie, de voor schrijven is toegewezen en geslaagd terug naar de client wordt geretourneerd.  

Het onderstaande diagram laat zien welke 2 afzonderlijke schrijfbewerkingen:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Een schrijfbewerking beginnen bij offset 0 met een lengte van 1024 bytes 
2.  Een schrijfbewerking beginnen bij verschuiving 4096 van de lengte van 1024 

#### <a name="reading-pages-from-a-page-blob"></a>Pagina's lezen van een pagina-Blob
Gelezen pagina's, gebruiken de [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) methode een reeks bytes lezen uit de paginablob. Hiermee kunt u de volledige blob of een bytebereik vanaf de verschuiving die in de blob te downloaden. Bij het lezen, heeft de verschuiving niet starten op een veelvoud van 512. Bij het lezen bytes van een pagina NUL, stuurt de service nul bytes.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
De volgende afbeelding toont een leesbewerking met BlobOffSet van 256 en rangeSize van 4352. Gegevens die worden geretourneerd is in oranje gemarkeerd. Nullen worden geretourneerd voor NUL 's

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Als u een zeer ingevuld blob u alleen de regio geldige pagina's kunt om te voorkomen dat betaalt voor egressing van nul bytes te verlagen en kan downloaden latentie te downloaden.  Gebruiken om te bepalen welke pagina's worden ondersteund door de gegevens, [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). U kunt de geretourneerde bereiken inventariseren en downloaden van de gegevens in elk bereik. 
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

#### <a name="leasing-a-page-blob"></a>Overdracht van een pagina-Blob
De Lease Blob-bewerking wordt tot stand gebracht en een vergrendeling voor een blob beheert voor schrijven en verwijderen van bewerkingen. Deze bewerking is wel handig in gevallen waarin een pagina-blob van meerdere clients om ervoor te zorgen slechts één client tegelijk kunt schrijven naar de blob wordt geopend. Azure-schijven, bijvoorbeeld: maakt gebruik van dit lease-mechanisme om te controleren of de schijf wordt alleen beheerd door een enkele virtuele machine. De duur van de vergrendeling kan 15 tot 60 seconden, of kunt oneindige. Zie de documentatie van [hier](/rest/api/storageservices/lease-blob) voor meer informatie.

> Gebruik de volgende koppeling om op te halen [codevoorbeelden](/resources/samples/?service=storage&term=blob&sort=0) voor veel toepassingsscenario's. 

Naast uitgebreide REST-API's bieden pagina-blobs ook gedeelde toegang, duurzaamheid en verbeterde beveiliging. Deze voordelen nader beschreven in de volgende alinea's wordt uitgelegd. 

### <a name="concurrent-access"></a>Gelijktijdige toegang
De REST-API van de pagina-Blobs en least mechanisme zorgt ervoor dat toepassingen toegang krijgen tot de paginablob van meerdere clients. Stel dat u nodig hebt voor het bouwen van een gedistribueerde cloudservice die opslagobjecten met meerdere gebruikers deelt. Een webtoepassing die voor een grote verzameling afbeeldingen aan meerdere gebruikers kan zijn. Een optie voor de implementatie hiervan is het gebruik van een virtuele machine met de gekoppelde schijven. De nadelen van deze toevoegen (i) de beperking een schijf kan alleen worden gekoppeld aan een enkele VM waardoor de schaalbaarheid, flexibiliteit, beperken en risico's te verhogen. Als er een probleem met de virtuele machine of de service die op de virtuele machine wordt uitgevoerd, klikt u vervolgens zijn als gevolg van de lease, de installatiekopie niet toegankelijk totdat de lease is verlopen of beschadigd is; en (ii) extra kosten van een IaaS VM hebben. 

Een alternatief is het gebruik van de pagina-Blobs rechtstreeks via de Azure Storage REST-API's. Deze optie niet meer nodig kostbare IaaS VM's, biedt volledige flexibiliteit van rechtstreekse toegang van meerdere clients, vereenvoudigt u het servicebeheer door niet hoeft te koppelen/loskoppelen van schijven en elimineert het risico van problemen op de virtuele machine. En biedt hetzelfde niveau van de prestaties voor willekeurige lees-/ schrijfbewerkingen als een schijf

### <a name="durability-and-high-availability"></a>Duurzaamheid en hoge beschikbaarheid
Standard en premium storage zijn duurzame opslag waar-de pagina-blob-gegevens worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid. Zie voor meer informatie over Azure Storage redundantie [documentatie](../common/storage-redundancy.md). Azure heeft consistent bedrijfsniveau duurzaamheid uitgebracht voor IaaS-schijven en pagina-blobs, met een toonaangevende nul % [op basis van Faalpercentage](https://en.wikipedia.org/wiki/Annualized_failure_rate). Dat wil zeggen, heeft Azure nooit een klant pagina-blob-gegevens verloren. 

### <a name="seamless-migration-to-azure"></a>Naadloze migratie naar Azure
Voor de klanten en ontwikkelaars die geïnteresseerd zijn in de implementatie van hun eigen aangepaste back-upoplossing, biedt Azure ook incrementele momentopnamen die alleen de delta's bevatten. Deze functie wordt voorkomen dat de kosten van de eerste volledige kopie, die aanzienlijk de kosten van de back-up verlaagt. Naast de mogelijkheid om efficiënt te lezen en differentiële gegevens kopiëren is dit een andere krachtige functie waarmee nog meer vernieuwingen van ontwikkelaars, wat leidt tot een beste eersteklas back-up en noodherstel (DR) herstelmogelijkheden op Azure. U kunt uw eigen back-up of DR-oplossing instellen voor uw virtuele machines over het gebruik van Azure [Blob momentopname](/rest/api/storageservices/snapshot-blob) samen met de [Get-paginabereiken](/rest/api/storageservices/get-page-ranges) API en de [incrementele kopie Blob](/rest/api/storageservices/incremental-copy-blob) API die u kunt Gebruik voor het kopiëren van de incrementele gegevens gemakkelijk voor herstel na Noodgevallen. 

Veel bedrijven hebben bovendien kritieke werkbelastingen die al wordt uitgevoerd in de lokale datacenters. Voor het migreren van de werkbelasting naar de cloud, zou een van de belangrijkste betrekking heeft op de hoeveelheid uitvaltijd nodig voor het kopiëren van de gegevens en het risico van onvoorziene problemen na de overschakeling zijn. In veel gevallen kan de uitvaltijd een verbluffend veel bekijks voor migratie naar de cloud zijn. Azure lost met behulp van de REST-API van de pagina-Blobs, dit probleem doordat cloudmigratie met een minimale overlast aan kritieke werkbelastingen. 

Voor voorbeelden van hoe u een momentopname en het terugzetten van een pagina-blob van een momentopname, raadpleegt u de [instellen van een back-upproces met incrementele momentopnamen](../../virtual-machines/windows/incremental-snapshots.md) artikel.


## <a name="summary"></a>Samenvatting
Azure wil het beste in klasse gebruikerservaring voor onze klanten. We het opslag-platform gebouwd met bedrijfsniveau duurzaamheid van gegevens, zodat onze klanten Azure met hun essentiële gegevens vertrouwen kunnen. Azure biedt de unieke API-ondersteuning en ontwikkelaars ervaring voor pagina-Blobs, wat mogelijk is er geen andere openbare cloud-platform. Dit heeft geleid tot veel 1e van derden en 3rd derden innovaties zoals naadloze cloud migraties, bovenliggende back-up-/ DR-ervaring, ondersteuning voor PaaS en DBaaS, gedistribueerde opslagoplossingen en andere innovaties voor IaaS VM's / schijven. Over het algemeen Azure opvalt tussen openbare cloudplatforms, met deze unieke mogelijkheden en voordelen van deze Azure-klanten-meerwaarde of andere cloudplatforms kunnen bieden.

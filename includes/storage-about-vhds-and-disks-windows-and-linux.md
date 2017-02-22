## <a name="about-vhds"></a>Over VHD's

De VHD's die worden gebruikt in Azure zijn .vhd-bestanden opgeslagen als pagina-blobs in een Standard Storage- of Premium Storage-account in Azure. Zie [Understanding block blobs and page blobs](/rest/api/storageservices/fileservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/) (Inzicht in blok-blobs en pagina-blobs) voor meer informatie over pagina-blobs. Zie [High-performance premium storage and Azure VMs](../articles/storage/storage-premium-storage.md) (Hoogwaardige Premium Storage en virtuele Azure-machines) voor meer informatie over Premium Storage.

Azure biedt ondersteuning voor de VHD-indeling met vaste schijf. Bij een vaste indeling wordt de logische schijf lineair ingedeeld binnen het bestand, zodat de schijf-offset X wordt opgeslagen op blob-offset X. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Bij een vaste indeling wordt vaak te veel ruimte gebruikt omdat de meeste schijven grote ongebruikte bereiken bevatten. Azure slaat VHD-bestanden echter in een sparse-indeling op, zodat u profiteert van de voordelen van zowel vaste als dynamische schijven. Zie [Getting started with virtual hard disks](https://technet.microsoft.com/library/dd979539.aspx) (Aan de slag met virtuele harde schijven) voor meer informatie.

Alle VHD-bestanden in Azure die u wilt gebruiken als bron voor het maken van schijven of installatiekopieën zijn alleen-lezen. Wanneer u een schijf of installatiekopie maakt, maakt Azure kopieën van de VHD-bestanden. Deze kopieën kunnen alleen-lezen of lezen/schrijven zijn, afhankelijk van hoe u de VHD gebruikt.

Wanneer u een virtuele machine van een installatiekopie maakt, maakt Azure een schijf voor de virtuele machine die een kopie is van het VHD-bronbestand. Als bescherming tegen onbedoeld verwijderen, plaatst Azure een lease op elk VHD-bronbestand dat wordt gebruikt voor het maken van een installatiekopie, een besturingssysteemschijf of een gegevensschijf.

Voordat u een VHD-bronbestand kunt verwijderen, dient u de lease te verwijderen door de schijf of installatiekopie te verwijderen. Als u een VHD-bestand wilt verwijderen dat door een virtuele machine wordt gebruikt als besturingssysteemschijf, kunt u de virtuele machine, de besturingssysteemschijf en het VHD-bronbestand in één keer verwijderen door de virtuele machine en alle gekoppelde schijven te verwijderen. Het verwijderen van een VHD-bronbestand voor een gegevensschijf vereist echter een aantal stappen in een vaste volgorde. Koppel eerst de schijf los van de virtuele machine, verwijder daarna de schijf en verwijder vervolgens het VHD-bestand.

> [!WARNING]
> Als u een VHD-bronbestand uit de opslag verwijdert of uw opslagaccount verwijdert, kan Microsoft die gegevens niet voor u herstellen.
> 

## <a name="types-of-disks"></a>Typen schijven 

U kunt kiezen uit twee prestatielagen voor opslag wanneer u uw schijven maakt: Standard Storage en Premium Storage. Daarnaast zijn er twee soorten schijven, niet-beheerd en beheerd, die zich in elk van de twee prestatielagen kunnen bevinden.  

### <a name="standard-storage"></a>Standard Storage 

Standard Storage wordt ondersteund door HDD's en biedt voordelige en hoogwaardige opslag. Standard Storage kan lokaal worden gerepliceerd in één datacenter of kan geografisch redundant zijn met primaire en secundaire datacenters. Zie [Azure Storage-replicatie](../articles/storage/storage-redundancy.md) voor meer informatie over opslagreplicatie. 

Raadpleeg [Standard Storage and Disks](../articles/storage/storage-standard-storage.md) (Standard Storage en schijven) voor meer informatie over het gebruik van Standard Storage met VM-schijven.

### <a name="premium-storage"></a>Premium Storage 

Premium Storage maakt gebruik van SSD's en voorziet in hoogwaardige schijfondersteuning met lage latentie voor virtuele machines die I/O-intensieve workloads uitvoeren. U kunt Premium Storage gebruiken met virtuele Azure-machines van de DS-, DSv2-, GS- of FS-reeks. Ga voor meer informatie naar [Premium Storage](../articles/storage/storage-premium-storage.md).

### <a name="unmanaged-disks"></a>Niet-beheerde schijven

Oorspronkelijk werden niet-beheerde schijven gebruikt door virtuele machines. Met deze schijven maakt u uw eigen opslagaccount en specificeert u dit account wanneer u de schijf maakt. U dient te controleren of u niet te veel schijven in hetzelfde opslagaccount plaatst, omdat u anders de [schaalbaarheidsdoelen](../articles/storage/storage-scalability-targets.md) van het opslagaccount kunt overschrijden (bijvoorbeeld&20;.000 IOP's), waardoor de virtuele machines worden vertraagd. Met niet-beheerde schijven moet u bepalen hoe u het beste gebruik kunt maken van een of meer opslagaccounts om de beste prestaties voor uw virtuele machines te realiseren.

### <a name="managed-disks"></a>Managed Disks 

Managed Disks beheert het maken/beheren van het opslagaccount op de achtergrond en zorgt ervoor dat u zich geen zorgen hoeft te maken over de schaalbaarheidslimieten van het opslagaccount. U hoeft alleen maar de schijfgrootte en prestatielaag (Standard/Premium) op te geven en Azure maakt en beheert de schijf voor u. Zelfs als u schijven toevoegt of de virtuele machine omhoog of omlaag schaalt, hoeft u zich geen zorgen te maken over de gebruikte opslag. 

U kunt ook uw aangepaste installatiekopieën in één opslagaccount per Azure-regio beheren en deze gebruiken om honderden virtuele machines onder hetzelfde abonnement te maken. Zie [Overzicht van Managed Disks](../articles/storage/storage-managed-disks-overview.md) voor meer informatie over Managed Disks.

We raden u aan Azure Managed Disks te gebruiken voor nieuwe virtuele machines en uw vorige niet-beheerde schijven te converteren naar beheerde schijven, zodat u kunt profiteren van de functies in Managed Disks.

### <a name="disk-comparison"></a>Vergelijking van schijven

De volgende tabel bevat een vergelijking van Premium en Standard voor niet-beheerde en beheerde schijven om u te helpen bepalen welke u het beste kunt gebruiken.

|    | Azure Premium Disk | Azure Standard Disk |
|--- | ------------------ | ------------------- |
| Schijftype | Solid-state drives (SSD) | Hardeschijfstation (HDD)  |
| Overzicht  | Hoogwaardige schijfondersteuning met lage latentie op basis van SSD's voor virtuele machines die IO-intensieve workloads uitvoeren of een bedrijfskritieke productieomgeving hosten | Voordelige schijfondersteuning op basis van HDD's voor het ontwikkelen/testen van virtuele machines |
| Scenario  | Productie- en prestatiegevoelige workloads | Ontwikkelen/testen, niet-kritiek, <br>Incidentele toegang |
| Schijfgrootte | P10: 128 GB<br>P20: 512 GB<br>P30: 1024 GB | Niet-beheerde schijven: 1 GB – 1TB <br><br>Beheerde schijven:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1024 GB |
| Max. doorvoer per schijf | 200 MB/s | 60 MB/s |
| Max. IOP's per schijf | 5000 IOP's | 500 IOP's |



<!--HONumber=Feb17_HO2-->



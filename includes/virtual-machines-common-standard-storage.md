# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Rendabele standaardopslag en niet-beheerde en beheerde Azure VM-schijven

Azure Standard-opslag biedt ondersteuning voor betrouwbare, goedkope schijven voor virtuele machines waarop niet latentie gevoelig werkbelastingen worden uitgevoerd. Het ondersteunt ook blobs, tabellen, wachtrijen en -bestanden. De gegevens worden opgeslagen met de Standard-opslag op de harde schijven (HDD's). Als u werkt met virtuele machines, kunt u standaardopslag schijven voor ontwikkel-/ Testscenario's en minder kritieke werkbelastingen en premium-schijven voor opslag voor essentiële productietoepassingen. Standard-opslag is beschikbaar in alle Azure-regio's. 

Dit artikel is gericht op het gebruik van de standard-opslag voor VM-schijven. Raadpleeg voor meer informatie over het gebruik van opslag met blobs, tabellen, wachtrijen en -bestanden, de [Inleiding tot Storage](../articles/storage/common/storage-introduction.md).

## <a name="disk-types"></a>Schijftypen

Er zijn twee manieren standaardschijven maken voor Azure virtuele machines:

**Schijven zonder begeleiding**: dit is de oorspronkelijke methode waar u de storage-accounts gebruikt voor het opslaan van de VHD-bestanden die met de VM-schijven overeenkomen kunt beheren. VHD-bestanden worden opgeslagen als pagina-blobs in opslagaccounts. Niet-beheerde schijven kunnen worden gekoppeld aan een Azure VM-grootte, met inbegrip van de virtuele machines die voornamelijk Premium-opslag, zoals de DSv2 en GS-serie gebruikt. Virtuele machines in Azure ondersteuning voor het koppelen van verschillende standaardschijven toestaan maximaal 256 TB aan opslag per VM.

[**Azure-beheerde schijven**](../articles/virtual-machines/windows/managed-disks-overview.md): deze functie de storage-accounts gebruikt voor de VM-schijven voor u beheert. U geeft het type (Premium of standaard) en de grootte van de schijf moet u en Azure maken en beheren van de schijf voor u. U hoeft te hoeven maken over de schijven tussen meerdere opslagaccounts plaatsen om ervoor te zorgen binnen de limieten voor schaalbaarheid voor de storage-accounts te blijven--Azure verwerkt die voor u.

Hoewel beide typen schijven beschikbaar zijn, wordt u aangeraden beheerd schijven om te profiteren van veel functies.

Om aan de slag met Azure Standard-opslag, gaat u naar [gratis aan de slag](https://azure.microsoft.com/pricing/free-trial/). 

Raadpleeg een van de volgende artikelen voor meer informatie over het maken van een virtuele machine met schijven beheerd.

* [Een virtuele machine maken met behulp van Resource Manager en PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)
* [Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0](../articles/virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Standard-opslag-functies 

U gaat nu een overzicht van enkele van de functies van de Standard-opslag. Zie voor meer informatie [Inleiding tot Azure Storage](../articles/storage/common/storage-introduction.md).

**Standard-opslag**: Azure Standard-opslag biedt ondersteuning voor schijven van Azure, Azure Blobs, Azure-bestanden, Azure-tabellen en wachtrijen in Azure. Standard-opslag om services te gebruiken, beginnen met [maken van een Azure Storage-account](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).

**Standard-opslag-schijven:** standaardopslag schijven kunnen worden gekoppeld aan alle Azure-machines, met inbegrip van grootte-serie virtuele machines gebruikt met Premium-opslag, zoals de DSv2 en GS-serie. Een standard-opslag-schijf kan alleen worden gekoppeld aan één virtuele machine. U kunt echter een of meer van deze schijven koppelen aan een VM, tot aan het maximale aantal schijven voor de gedefinieerd voor deze VM-grootte. De specificaties in meer detail worden beschreven in de volgende sectie op standaard Storage Scalability and Performance Targets. 

**Standaard pagina-blob**: standaard pagina-blobs worden gebruikt om permanente schijven voor virtuele machines en kan ook rechtstreeks via REST net als andere soorten Azure Blobs worden geopend. [Pagina-blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) zijn van een verzameling 512-byte-'s die zijn geoptimaliseerd voor willekeurige lees- en schrijfbewerkingen. 

**Storage-replicatie:** In de meeste regio's, gegevens in een standaard opslagaccount gerepliceerde lokaal of geogerepliceerde in meerdere datacentrums kunnen worden. De vier typen replicatie beschikbaar zijn lokaal redundante opslag (LRS), Zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geografisch redundante opslag met leestoegang (RA-GRS). Beheerde schijven in de Standard-opslag ondersteunt momenteel lokaal redundante opslag (LRS) alleen. Zie voor meer informatie [Opslagreplicatie](../articles/storage/common/storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen

In deze sectie beschrijven we de schaalbaarheid en prestaties doelen die u overwegen moet wanneer u de standard-opslag.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Limieten: niet van toepassing op beheerde schijven

| **Resource** | **Standaardlimiet** |
|--------------|-------------------|
| TB per storage-account  | 500 TB |
| Maximale inkomende<sup>1</sup> per storage-account (ons regio's) | 10 Gbps als GRS/ZRS is ingeschakeld, 20 Gbps voor LRS |
| Maximum aantal uitgaande<sup>1</sup> per storage-account (ons regio's) | 20 Gbps als RA-GRS/GRS/ZRS is ingeschakeld, 30 Gbps voor LRS |
| Maximale inkomende<sup>1</sup> per storage-account (Europese en Aziatische regio's) | 5 Gbps als GRS/ZRS is ingeschakeld, 10 Gbps voor LRS |
| Maximum aantal uitgaande<sup>1</sup> per storage-account (Europese en Aziatische regio's) | 10 Gbps als RA-GRS/GRS/ZRS is ingeschakeld, 15 Gbps voor LRS |
| Totale snelheid van aanvragen voor (ervan uitgaande dat de grootte van 1 KB object) per storage-account | Maximaal 20.000 IOP's, entiteiten per seconde of berichten per seconde |

<sup>1</sup> inkomend verwijst naar alle gegevens (aanvragen) verstuurd naar een opslagaccount. Uitgaande gegevens verwijst naar alle gegevens (antwoorden) ontvangen van een opslagaccount.

Zie voor meer informatie [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md).

Als de behoeften van uw toepassing de schaalbaarheidsdoelen van een enkele storage-account overschrijdt, moet u uw toepassing te gebruiken van meerdere opslagaccounts en partitioneren van uw gegevens over de storage-accounts maken. Ook kunt u beheerde Azure-schijven en Azure beheert het partitioneren en de plaatsing van uw gegevens voor u.

### <a name="standard-disks-limits"></a>Standaardschijven limieten

In tegenstelling tot de Premium-schijven, worden de i/o-bewerkingen per seconde (IOPS) en de doorvoer (bandbreedte) van standaardschijven niet ingericht. De prestaties van standaardschijven afhankelijk van de VM-grootte op de schijf is gekoppeld, niet op de grootte van de schijf. U kan verwacht te verkrijgen tot de prestaties is bereikt die worden vermeld in de onderstaande tabel.

**Standaardschijven limieten (beheerde en onbeheerde)**

| **VM-laag**            | **Basis VM-laag** | **Standaard VM-laag** |
|------------------------|-------------------|----------------------|
| De grootte van de maximale diskette          | 4095 GB           | 4095 GB              |
| Maximaal 8 KB IOP's per schijf | 300         | Maximaal 500            |
| Maximale Bandwidth per schijf | Maximaal 60 MB/s     | Maximaal 60 MB/s        |

Als uw werkbelasting schijfondersteuning met hoge prestaties, lage latentie is vereist, moet u rekening houden met behulp van Premium-opslag. Als u wilt weten meer voordelen van Premium-opslag, gaat u naar [High-Performance Premium-opslag- en Azure VM-schijven](../articles/virtual-machines/windows/premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>Momentopnamen en blob kopiëren

Het VHD-bestand is met de Storage-service een pagina-blob. U kunt momentopnamen van pagina-blobs en kopieer deze naar een andere locatie, zoals een ander opslagaccount.

### <a name="unmanaged-disks"></a>Niet-beheerde schijven

U kunt maken [incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md) voor niet-beheerde standaardschijven op dezelfde manier als u momentopnamen met Standard-opslag gebruiken. U wordt aangeraden dat u momentopnamen maken en kopieer deze momentopnamen naar een standaard geografisch redundante opslagaccount als de bronschijf in een account met lokaal redundante opslag is. Zie voor meer informatie [Azure redundantie opslagopties](../articles/storage/common/storage-redundancy.md).

Als een schijf aan een virtuele machine is gekoppeld, worden bepaalde API-bewerkingen zijn niet toegestaan op de schijven. Bijvoorbeeld, u kunt niet uitvoeren een [Blob kopiëren](/rest/api/storageservices/Copy-Blob) bewerking op de blob zolang de schijf is gekoppeld aan een virtuele machine. In plaats daarvan eerst een momentopname maken van blob met behulp van de [momentopname Blob](/rest/api/storageservices/Snapshot-Blob) REST-API-methode en voert u de [Blob kopiëren](/rest/api/storageservices/Copy-Blob) van de momentopname naar de gekoppelde schijf kopiëren. U kunt ook Ontkoppel de schijf en vervolgens alle noodzakelijke bewerkingen uitvoeren.

Om te blijven geografisch redundante exemplaren van de momentopnamen, kunt u momentopnamen van een account met lokaal redundante opslag met geografisch redundante standard-opslagaccount met behulp van AzCopy of Blob kopiëren. Zie voor meer informatie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../articles/storage/common/storage-use-azcopy.md) en [Blob kopiëren](/rest/api/storageservices/Copy-Blob).

Zie voor gedetailleerde informatie voor het uitvoeren van de REST-bewerkingen op de pagina-blobs in opslagaccounts standaard [REST-API van Azure Storage-Services](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Managed Disks

Een momentopname voor een beheerde schijf is een alleen-lezen kopie van de beheerde schijf die wordt opgeslagen als een standard-beheerde schijven. Incrementele momentopnamen worden momenteel niet ondersteund voor schijven die worden beheerd, maar in de toekomst worden ondersteund.

Als een beheerde schijf is gekoppeld aan een VM, worden bepaalde API-bewerkingen zijn niet toegestaan op de schijven. U kunt bijvoorbeeld een shared access signature (SAS) naar een kopieerbewerking niet uitvoeren terwijl de schijf is gekoppeld aan een virtuele machine niet genereren. In plaats daarvan maakt eerst een momentopname van de schijf en voer de kopie van de momentopname. U kunt ook Ontkoppel de schijf en vervolgens een shared access signature (SAS) om uit te voeren de kopieerbewerking te genereren.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u de Standard-opslag, zijn de volgende factureringsvoorwaarden van toepassing:

* Standard-opslag zonder begeleiding schijven/gegevensgrootte 
* Beheerde standaardschijven
* Standard-opslag-momentopnamen
* Uitgaande gegevensoverdracht
* Transacties

**De grootte van de schijf en gegevens van de opslagruimte zonder begeleiding:** voor niet-beheerde schijven en andere gegevens (blobs, tabellen, wachtrijen en -bestanden), u in rekening worden gebracht alleen voor de hoeveelheid ruimte u gebruikt. Bijvoorbeeld, als u een VM hebt waarvoor pagina-blob is ingericht als 127 GB, maar de virtuele machine is in feite alleen met behulp van 10 GB aan ruimte u wordt gefactureerd voor 10 GB aan ruimte. We bieden ondersteuning voor Standard-opslag maximaal 8191 GB en standaard zonder begeleiding schijven 4095 GB. 

**Schijven die worden beheerd:** beheerde schijven worden gefactureerd voor de ingerichte grootte. Als de schijf is ingericht als een schijf van 10 GB en u alleen met behulp van 5 GB, wordt u in rekening gebracht voor de grootte van het inrichten van 10 GB.

**Momentopnamen**: momentopnamen van standaardschijven wordt gefactureerd voor de extra capaciteit die wordt gebruikt door de momentopnamen. Zie voor informatie over momentopnamen [maken van een momentopname van een Blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Uitgaande gegevensoverdracht**: [uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) (gegevens uit de Azure-datacenters gaan) worden gefactureerd voor bandbreedtegebruik.

**Transactie**: Azure rekent $0.0036 per 100.000 transacties voor standard-opslag. Transacties bestaan zowel uit lees- als schrijfbewerkingen voor opslag.

Zie voor gedetailleerde informatie over prijzen voor Standard-opslag, virtuele Machines en schijven beheerd:

* [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Beheerde schijven prijzen](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Ondersteuning voor Azure Backup-service 

Virtuele machines met niet-beheerde schijven kunt een back-up maken met Azure Backup. [Meer informatie](../articles/backup/backup-azure-vms-first-look-arm.md).

U kunt ook de Azure Backup-service met beheerd schijven gebruiken een back-uptaak met back-ups op basis van tijd, eenvoudig herstel van de virtuele machine en bewaarbeleid voor back-up te maken. U vindt meer informatie over dit op [met behulp van Azure Backup-service voor virtuele machines met schijven beheerd](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Storage](../articles/storage/common/storage-introduction.md)

* [Een opslagaccount maken](../articles/storage/common/storage-create-storage-account.md)

* [Overzicht Managed Disks](../articles/virtual-machines/linux/managed-disks-overview.md)

* [Een virtuele machine maken met behulp van Resource Manager en PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)

* [Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0](../articles/virtual-machines/linux/quick-create-cli.md)

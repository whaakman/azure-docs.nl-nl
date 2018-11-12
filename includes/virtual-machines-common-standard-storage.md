---
title: bestand opnemen
description: bestand opnemen
services: storage
author: yuemlu
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: yuemlu
ms.custom: include file
ms.openlocfilehash: e266b239a44907e8e38e60cfc217aa21e46ab17e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264424"
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Rendabele standaardopslag en niet-beheerde en beheerde Azure-VM-schijven

Azure Standard-opslag biedt een betrouwbare, voordelige schijfondersteuning voor virtuele machines waarop niet gevoelig werkbelastingen worden uitgevoerd. Het ondersteunt ook blobs, tabellen, wachtrijen en bestanden. Met de Standard-opslag, worden de gegevens worden opgeslagen op hardeschijfstations (HDD's). Als u werkt met virtuele machines, kunt u standaard SSD en HDD-schijven voor Dev/Test-scenario's en minder kritieke werkbelastingen en premium-SSD-schijven voor essentiële productietoepassingen. Standard-opslag is beschikbaar in alle Azure-regio's. 

In dit artikel richt zich op het gebruik van standard-SSD en HDD-schijven. Zie voor meer informatie over het gebruik van opslag-blobs, tabellen, wachtrijen en bestanden [Inleiding tot Storage](../articles/storage/common/storage-introduction.md).

## <a name="disk-types"></a>Schijftypen

Er zijn twee manieren om te maken van standard-schijven voor virtuele Azure-machines:

**Niet-beheerde schijven**: dit type schijf is de oorspronkelijke methode waarbij u de storage-accounts gebruikt voor het opslaan van de VHD-bestanden die met de VM-schijven overeenkomen beheren. VHD-bestanden worden opgeslagen als pagina-blobs in opslagaccounts. Niet-beheerde schijven kunnen worden gekoppeld aan elke Azure-VM-grootte, met inbegrip van de virtuele machines die voornamelijk gebruikmaken van Premium-opslag, zoals de DSv2 en GS-serie. Azure VM's ondersteunen verschillende standaardschijven koppelen zodat maximaal 256 TiB opslagruimte per virtuele machine. Als u de grootte van de preview-schijven gebruikt, kunt u maximaal ongeveer 2 gaat dit opslagruimte per virtuele machine hebben.

[**Azure Managed Disks**](../articles/virtual-machines/windows/managed-disks-overview.md): deze functie wordt de storage-accounts die voor u gebruikt voor de VM-schijven beheerd. U geeft het type (Premium-SSD-, Standard-SSD- of Standard HDD) en de grootte van de schijf die u nodig hebt en Azure maakt en beheert de schijf voor u. U hoeft te bekommeren plaatsen van de schijven voor meerdere opslagaccounts om ervoor te zorgen, u binnen de grenzen van de schaalbaarheid voor de storage-accounts blijven--Azure regelt die voor u.

Hoewel beide typen schijven beschikbaar zijn, kunt u het beste Managed Disks gebruiken om te profiteren van de vele functies.

Als u wilt aan de slag met Azure Standard-opslag, gaat u naar [voor gratis aan de slag](https://azure.microsoft.com/pricing/free-trial/). 

Zie een van de volgende artikelen voor informatie over het maken van een virtuele machine met Managed Disks.

* [Een virtuele machine maken met behulp van Resource Manager en PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)
* [Een virtuele Linux-machine maken met behulp van de Azure CLI](../articles/virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Standard-opslag-functies

Laten we eens enkele van de functies van Standard-opslag. Zie voor meer informatie, [Inleiding tot Azure Storage](../articles/storage/common/storage-introduction.md).

**Standard-opslag**: Azure Standard-opslag biedt ondersteuning voor Azure Disks, Azure-Blobs, Azure Files, Azure-tabellen en wachtrijen van Azure. Voor het gebruik van Standard Storage-services, beginnen met [maken van een Azure Storage-account](../articles/storage/common/storage-quickstart-create-account.md).

**Standard-SSD-schijven:** Standard-SSD-schijven bieden meer betrouwbare prestaties dan standaard harde schijven en op dit moment beschikbaar zijn. Zie voor meer informatie over beschikbaarheid in regio's van de Standard-SSD-schijven, [beschikbaarheid in regio's van de Standard-SSD-schijven](../articles/virtual-machines/windows/faq-for-disks.md#standard-ssds-azure-regions).

**Standaardschijven HDD:** standaard harde schijven kunnen worden gekoppeld aan alle Azure-VM's, inclusief grootte-serie VM's met Premium Storage gebruikt, zoals de DSv2 en GS-serie. Een diskette standaard harde schijven kan alleen worden gekoppeld aan een virtuele machine. U kunt echter een of meer van deze schijven koppelen aan een virtuele machine, tot het maximum aantal op schijf aantal gedefinieerd voor deze VM-grootte. In de volgende sectie op Standard Storage Scalability and Performance Targets, wordt de specificaties in meer detail beschreven.

**Standard-pagina-blob**: standaard pagina-blobs worden gebruikt voor het opslaan van permanente schijven voor virtuele machines en ook rechtstreeks via REST net als andere soorten Azure-Blobs kunnen worden geopend. [Pagina-blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) zijn een verzameling van 512-byte-'s die zijn geoptimaliseerd voor willekeurige lees- en schrijfbewerkingen. 

**Storage-replicatie:** In de meeste regio's, gegevens in een standaardopslagaccount mag gerepliceerde lokaal of geo-replicatie over meerdere datacenters. De vier typen replicatie beschikbaar zijn voor lokaal redundante opslag (LRS), Zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) en geografisch redundante opslag met leestoegang (RA-GRS). Beheerde schijven in standaardopslag ondersteuning momenteel voor lokaal redundante opslag (LRS) alleen. Zie voor meer informatie, [Opslagreplicatie](../articles/storage/common/storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen

In deze sectie beschrijven we de schaalbaarheids- en prestatiedoelen die u overwegen moet bij het gebruik van standard-opslag.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Limieten voor account: niet van toepassing op beheerde schijven

| **Resource** | **Standaardlimiet** |
|--------------|-------------------|
| TB per opslagaccount  | 500 TB |
| Maximum aantal inkomende<sup>1</sup> per opslagaccount (VS-regio's) | 10 Gbps als GRS/ZRS ingeschakeld, 20 Gbps voor LRS |
| Maximum aantal uitgaande<sup>1</sup> per opslagaccount (VS-regio's) | 20 Gbps als ingeschakeld RA-GRS/GRS/ZRS, 30 Gbps voor LRS |
| Maximum aantal inkomende<sup>1</sup> per opslagaccount (Europese en Aziatische regio's) | 5 Gbps als GRS/ZRS ingeschakeld, 10 Gbps voor LRS |
| Maximum aantal uitgaande<sup>1</sup> per opslagaccount (Europese en Aziatische regio's) | 10 Gbps als ingeschakeld RA-GRS/GRS/ZRS, 15 Gbps voor LRS |
| Totale snelheid van aanvragen (ervan uitgaande dat de grootte van 1 KB-object) per opslagaccount | Maximaal 20.000 IOP's, entiteiten per seconde of berichten per seconde |

<sup>1</sup> inkomend verwijst naar alle gegevens (aanvragen) die worden verzonden naar een opslagaccount. Uitgaande gegevens verwijst naar alle gegevens (reacties) worden ontvangen van een storage-account.

Zie voor meer informatie, [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md).

Als de behoeften van uw toepassing de schaalbaarheidsdoelen van een enkel opslagaccount overschrijdt, moet u uw toepassing gebruik meerdere opslagaccounts en partities voor uw gegevens voor deze opslagaccounts maken. U kunt ook kunt u Azure Managed Disks en Azure beheert de partitionerings- en de plaatsing van uw gegevens voor u.

### <a name="standard-disks-limits"></a>Limieten voor Standard-schijven

In tegenstelling tot Premium-schijven, zijn de i/o-bewerkingen per seconde (IOPS) en doorvoer (bandbreedte) van de Standard-schijven niet ingericht. De prestaties van de standard-schijven is afhankelijk van de VM-grootte voor de schijf is gekoppeld, niet voor de grootte van de schijf. U kunt verwacht te verkrijgen tot de prestatielimiet die worden vermeld in de onderstaande tabel.

**Limieten voor Standard-schijven (beheerd en niet-beheerd)**

| **VM-laag**            | **Basic-laag virtuele machine** | **Standard-laag virtuele machine** |
|------------------------|-------------------|----------------------|
| Disk maximale grootte          | 32.767 giB           | 32.767 giB        |
| Max 8 KB IOPS per schijf | Maximaal 2000         | Maximaal 2000        |
| Maximale Bandwidth per schijf | Maximaal 500 MB/s     | Maximaal 500 MB/s      |

Als uw workload schijfondersteuning met hoge prestaties en lage latentie, moet u overwegen Premium-opslag. Als u wilt weten meer voordelen van Premium-opslag, gaat u naar [High-Performance Premium Storage en Azure VM-schijven](../articles/virtual-machines/windows/premium-storage.md).

## <a name="snapshots-and-copy-blob"></a>Momentopnamen en kopiëren van de blob

Met de Storage-service is het VHD-bestand een pagina-blob. U kunt momentopnamen van pagina-blobs en kopieer deze naar een andere locatie, zoals een ander opslagaccount.

### <a name="unmanaged-disks"></a>Niet-beheerde schijven

U kunt maken [incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md) voor niet-beheerde standaardschijven op dezelfde manier als u momentopnamen gebruiken met de Standard-opslag. U wordt aangeraden dat u momentopnamen maken en kopieert u deze momentopnamen naar een standaard geografisch redundante opslagaccount als de bronschijf in een account voor lokaal redundante opslag is. Zie voor meer informatie, [opslagopties van Azure voor redundantie](../articles/storage/common/storage-redundancy.md).

Als een schijf is gekoppeld aan een virtuele machine, worden bepaalde API-bewerkingen zijn niet toegestaan op de schijven. Bijvoorbeeld, u kunt niet uitvoeren een [kopiëren van de Blob](/rest/api/storageservices/Copy-Blob) bewerking op die blob als de schijf is gekoppeld aan een virtuele machine. In plaats daarvan eerst een momentopname maken van die blob met behulp van de [momentopname maken van Blob](/rest/api/storageservices/Snapshot-Blob) REST-API-methode en vervolgens voert u de [kopiëren van de Blob](/rest/api/storageservices/Copy-Blob) van de momentopname naar de gekoppelde schijf kopiëren. U kunt ook de schijf loskoppelen en vervolgens voert geen bewerkingen uit die nodig zijn.

Als u wilt behouden geografisch redundante kopieën van uw momentopnamen, kunt u momentopnamen van een account voor lokaal redundante opslag naar een standaard geografisch redundante opslagaccount kopiëren met behulp van AzCopy of kopiëren van de Blob. Zie voor meer informatie, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../articles/storage/common/storage-use-azcopy.md) en [kopiëren van de Blob](/rest/api/storageservices/Copy-Blob).

Zie voor gedetailleerde informatie over het uitvoeren van REST-bewerkingen voor pagina-blobs in standard storage-accounts [REST-API voor Azure Storage-Services](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Managed Disks

Een momentopname van een beheerde schijf is een alleen-lezen kopie van de beheerde schijf die is opgeslagen als een standard-beheerde schijven. Incrementele momentopnamen worden momenteel niet ondersteund voor Managed Disks, maar in de toekomst worden ondersteund.

Als een beheerde schijf is gekoppeld aan een virtuele machine, worden bepaalde API-bewerkingen zijn niet toegestaan op de schijven. U kunt bijvoorbeeld een shared access signature (SAS) om uit te voeren van een kopieerbewerking terwijl de schijf is gekoppeld aan een virtuele machine niet genereren. In plaats daarvan maakt u eerst een momentopname van de schijf en voert u de kopie van de momentopname. U kunt ook de schijf loskoppelen en vervolgens een shared access signature (SAS) om uit te voeren van de kopieerbewerking te genereren.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u de Standard-opslag, zijn de volgende factureringsvoorwaarden van toepassing:

* Standard-opslag niet-beheerde schijven/gegevensgrootte
* Standaard beheerde schijven
* Standard storage-momentopnamen
* Uitgaande gegevensoverdracht
* Transacties

**Niet-beheerde gegevens en schijf opslaggrootte:** voor niet-beheerde schijven en andere gegevens (blobs, tabellen, wachtrijen en bestanden), u betaalt alleen voor de hoeveelheid ruimte u gebruikt. Bijvoorbeeld, als u een virtuele machine hebt waarvan pagina-blob is ingericht als 127 GB, maar de virtuele machine is in feite alleen met behulp van 10 GB aan schijfruimte, in rekening gebracht voor 10 GB aan ruimte. We bieden ondersteuning voor Standard-opslag tot 8191 GB en standaard niet-beheerde schijven tot 4095 GB. 

**Beheerde schijven:** facturering voor standaard beheerde schijven, is afhankelijk van de ingerichte grootte van de schijf. Azure wijst de ingerichte grootte (afgerond) naar de dichtstbijzijnde Managed Disks-optie die zijn opgegeven in de onderstaande tabellen. Elke beheerde schijf wordt toegewezen aan een van de ondersteunde ingerichte grootte en dienovereenkomstig wordt gefactureerd. Bijvoorbeeld, als u een standaard beheerde schijf maken en een ingerichte grootte van 200 GiB opgeven, in rekening gebracht volgens de prijzen van het type S15 schijf.

Grootte die is gemarkeerd met een sterretje zijn momenteel in preview.

| **Standaard harde schijven die worden beheerd <br>schijftype** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** | **S60*** | **S70*** | **S80*** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Schijfgrootte        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1024 giB (1 TiB) | 2048 giB (2 TiB) | 4.095 giB (4 TiB) | 8192 giB (8 TiB) | 16,385 giB (16 TiB) | 32.767 giB (32 TiB) |


**Momentopnamen**: momentopnamen van schijven die standaard worden in rekening gebracht voor de extra capaciteit gebruikt door de momentopnamen. Zie voor meer informatie over momentopnamen [het maken van een momentopname van een Blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Uitgaande gegevensoverdracht**: [uitgaande gegevensoverdrachten](https://azure.microsoft.com/pricing/details/data-transfers/) (gegevens die vanuit Azure-datacenters) worden gefactureerd voor bandbreedtegebruik.

**Transactie**: Azure rekent $0.0036 per 100.000 transacties voor standard-opslag. Transacties bestaan zowel uit lees- als schrijfbewerkingen voor opslag.

Zie voor gedetailleerde informatie over prijzen voor Standard-opslag, virtuele Machines en beheerde schijven:

* [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Prijzen voor Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Prijzen van Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Ondersteuning van Azure Backup-service

Virtuele machines met niet-beheerde schijven kunnen worden reservekopie maken met Azure Backup. [Meer informatie](../articles/backup/backup-azure-vms-first-look-arm.md).

U kunt ook de Azure Backup-service met Managed Disks gebruiken om te maken van een back-uptaak met back-ups op basis van tijd, eenvoudig herstel van de virtuele machine en voor het bewaren van back-up. U kunt meer lezen over deze [met behulp van Azure Backup-service voor virtuele machines met Managed Disks](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Volgende stappen

* [Kennismaking met Azure Storage](../articles/storage/common/storage-introduction.md)

* [Een opslagaccount maken](../articles/storage/common/storage-quickstart-create-account.md)

* [Overzicht Managed Disks](../articles/virtual-machines/linux/managed-disks-overview.md)

* [Een virtuele machine maken met behulp van Resource Manager en PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)

* [Een virtuele Linux-machine maken met behulp van de Azure CLI](../articles/virtual-machines/linux/quick-create-cli.md)

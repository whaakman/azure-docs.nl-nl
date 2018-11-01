---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3e660dbcbfecd57c43ecec6006581364660adb44
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50737226"
---
# <a name="azure-managed-disks-overview"></a>Overzicht Azure Managed Disks

Azure Managed Disks vereenvoudigt u Schijfbeheer voor Azure IaaS VM's door het beheer van de [opslagaccounts](../articles/storage/common/storage-introduction.md) die zijn gekoppeld aan de VM-schijven. U hoeft alleen te geven van het type ([Standard HDD](../articles/virtual-machines/windows/standard-storage.md), [Standard-SSD](../articles/virtual-machines/windows/disks-standard-ssd.md), of [Premium SSD](../articles/virtual-machines/windows/premium-storage.md)) en de grootte van de schijf moet u, en Azure maakt en beheert de schijf voor u.

## <a name="benefits-of-managed-disks"></a>Voordelen van beheerde schijven

Laten we eens enkele van de voordelen die u toegang met behulp van beheerde schijven, beginnen met deze video van Channel 9 [betere Azure VM-tolerantie met Managed Disks](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Eenvoudige en schaalbare VM-implementatie

Managed Disks verwerken de opslag voor u achter de schermen. Voorheen moest u opslagaccounts voor het opslaan van de schijven (VHD-bestanden) voor uw Azure-VM's maken. Wanneer u omhoog, moest u om ervoor te zorgen dat u extra storage-accounts hebt gemaakt, zodat u niet langer zijn dan de IOPS-limiet voor opslag met een van uw schijven. Met Managed Disks verwerken de opslag, u bent niet langer beperkt door de opslaglimieten voor account (zoals 20.000 IOP's / account). U moet ook niet meer uw aangepaste installatiekopieën (VHD-bestanden) kopiëren naar meerdere opslagaccounts. U kunt deze op een centrale locatie – één opslagaccount per Azure-regio – beheren en gebruiken voor het maken van honderden virtuele machines in een abonnement.

Beheerde schijven kunt u maximaal 50.000 virtuele machine maken **schijven** van een type in een abonnement per regio, wordt die kunt u duizenden maken **VMs** in één abonnement. Deze functie ook verder verhoogt de schaalbaarheid van [Virtual Machine Scale Sets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) doordat u maximaal duizend virtuele machines in een schaalset van de virtuele machine met behulp van een Marketplace-installatiekopie maken.

### <a name="better-reliability-for-availability-sets"></a>Betere betrouwbaarheid voor Beschikbaarheidssets

Beheerde schijven bieden een betere betrouwbaarheid voor Beschikbaarheidssets door ervoor te zorgen dat de schijven van [VM's in een Beschikbaarheidsset](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) voldoende zijn geïsoleerd van elkaar om te voorkomen dat single point of failure. Schijven worden automatisch in verschillende kasten (stempels) geplaatst. Als een stempel is mislukt vanwege fout bij de hardware of software, wordt alleen de VM-exemplaren met schijven op deze stempels mislukt. Bijvoorbeeld: Stel dat u hebt een toepassing die wordt uitgevoerd op vijf virtuele machines en de virtuele machines zich in een Beschikbaarheidsset. De schijven voor deze virtuele machines wordt niet allemaal worden opgeslagen in de dezelfde stempel, dus als één stamp is uitvalt, worden de andere exemplaren van de toepassing worden uitgevoerd.

### <a name="highly-durable-and-available"></a>Zeer duurzaam en hoge beschikbaarheid

Azure-schijven zijn ontworpen om 99,999% van de tijd beschikbaar te zijn. Geniet van de wetenschap dat u hebt drie replica's van uw gegevens die grote duurzaamheid mogelijk maakt. Als er zich problemen voordoen met een of zelfs twee van de replica’s, kunnen de resterende replica’s ervoor zorgen dat uw gegevens bewaard blijven en storingen weinig kwaad kunnen. Dankzij deze architectuur heeft Azure consistente duurzaamheid op ondernemingsniveau kunnen leveren voor IaaS-schijven, met een foutpercentage van NUL% op jaarbasis, het beste in de bedrijfstak. 

### <a name="granular-access-control"></a>Gebruik nauwkeurig toegangsbeheer

U kunt [op rollen gebaseerd toegangsbeheer (RBAC)](../articles/role-based-access-control/overview.md) specifieke machtigingen voor een beheerde schijf aan een of meer gebruikers toewijzen. Beheerde schijven toont een aantal bewerkingen, zoals lezen, schrijven (maken/bijwerken), verwijderen en het ophalen van een [handtekening voor gedeelde toegang (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) voor de schijf. U kunt toegang verlenen tot alleen de bewerkingen die iemand nodig heeft om uit te voeren hun taak. Bijvoorbeeld, als u niet dat een persoon in een beheerde schijf kopiëren naar een opslagaccount wilt, kunt u geen toegang naar de export-actie voor de beheerde schijf te verlenen. Op dezelfde manier als u niet dat een persoon in een SAS-URI gebruiken om te kopiëren van een beheerde schijf wilt, kunt u geen toestemming te verlenen die voor de beheerde schijf.

### <a name="azure-backup-service-support"></a>Ondersteuning van Azure Backup-service

Gebruik Azure Backup-service met Managed Disks te maken van een back-uptaak met back-ups op basis van tijd eenvoudig herstel van de virtuele machine en voor het bewaren van back-up. Beheerde schijven ondersteunen alleen lokaal redundante opslag (LRS) als de replicatie-optie. Drie kopieën van de gegevens worden bewaard in een enkele regio. Voor regionaal herstel na noodgevallen, u moet back-up van de VM-schijven in een andere regio met [Azure Backup-service](../articles/backup/backup-introduction-to-azure-backup.md) en een GRS-opslagaccount als back-upkluis. Azure Backup ondersteunt momenteel de schijfgrootten tot 4 TB-schijven. U moet [upgrade VM-back-upstack naar V2](../articles/backup/backup-upgrade-to-vm-backup-stack-v2.md) voor ondersteuning van 4 TB-schijven. Zie voor meer informatie, [met behulp van Azure Backup-service voor virtuele machines met Managed Disks](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u Managed Disks gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* Opslagtype

* Schijfgrootte

* Aantal transacties

* Uitgaande gegevensoverdracht

* Beheerde momentopnamen van de schijf (volledige schijf kopiëren)

We gaan nog eens kijken deze opties.

**Opslagtype:** Managed Disks biedt 3 prestatielagen: [Standard HDD](../articles/virtual-machines/windows/standard-storage.md), [Standard-SSD](../articles/virtual-machines/windows/disks-standard-ssd.md), en [Premium](../articles/virtual-machines/windows/premium-storage.md). De facturering van een beheerde schijf, is afhankelijk van welk type opslag die u hebt geselecteerd voor de schijf.

**Schijfgrootte**: facturering voor beheerde schijven, is afhankelijk van de ingerichte grootte van de schijf. Azure wijst de ingerichte grootte (afgerond) naar de dichtstbijzijnde Managed Disks-optie die zijn opgegeven in de onderstaande tabellen. Elke beheerde schijf wordt toegewezen aan een van de ondersteunde ingerichte grootte en dienovereenkomstig wordt gefactureerd. Bijvoorbeeld, als u een standaard beheerde schijf maken en geef een ingerichte grootte van 200 GB, in rekening gebracht volgens de prijzen van het type S15 schijf.

Hier volgen de schijfgrootten beschikbaar voor een premium-beheerde schijf:

| **Premium SDD beheerd <br>schijftype** | **P4** | **P6** | **P10** | **P15** | **P20** | **P30** | **P40** | **P50** | **P60** | **P70** | **P80** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Schijfgrootte        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1024 giB (1 TiB) | 2048 giB (2 TiB) | 4.095 giB (4 TiB) | 8192 giB (8 TiB) | 16.384 giB (16 TiB) | 32.767 giB (TiB) |

Hier volgen de schijfgrootten beschikbaar voor een standard-SSD beheerde schijven:

| **Standard-SSD beheerd <br>schijftype** | **E10** | **E15** | **E20** | **E30** | **E40** | **E50** | **E60** | **E70** | **E80** |
|------------------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Schijfgrootte        | 128 GiB | 256 GiB | 512 GiB | 1024 giB (1 TiB) | 2048 giB (2 TiB) | 4.095 giB (4 TiB) | 8192 giB (8 TiB) | 16.384 giB (16 TiB) | 32.767 giB (TiB) |

Hier volgen de schijfgrootten beschikbaar voor een standaard beheerde schijf van HDD:

| **Standaard harde schijven die worden beheerd <br>schijftype** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** | **S60** | **S70** | **S80** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Schijfgrootte        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1024 giB (1 TiB) | 2048 giB (2 TiB) | 4.095 giB (4 TiB) | 8192 giB (8 TiB) | 16.384 giB (16 TiB) | 32.767 giB (TiB) |

**Aantal transacties**: U wordt gefactureerd voor het aantal transacties die u op een standard-beheerde schijven uitvoert.

Standard-SSD-schijven gebruiken i/o-grootte van 256KB. Als de gegevens worden overgebracht minder dan 256 KB is, wordt deze beschouwd als 1 i/o-eenheid. I/o-grotere worden geteld als meerdere i/o's van de grootte van 256 KB. Bijvoorbeeld, wordt een 1100 KB i/o geteld als vijf i/o-eenheden.

Er zijn geen kosten verbonden voor transacties voor een premium-beheerde schijf.

**Uitgaande gegevensoverdracht**: [uitgaande gegevensoverdrachten](https://azure.microsoft.com/pricing/details/data-transfers/) (gegevens die vanuit Azure-datacenters) worden gefactureerd voor bandbreedtegebruik.

Zie voor gedetailleerde informatie over prijzen voor Managed Disks [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Momentopnamen van de beheerde schijf

Een Managed Snapshot is een alleen-lezen kopie van een beheerde schijf die wordt opgeslagen als een standaard beheerde schijf standaard. Met momentopnamen, kunt u back-up uw beheerde schijven op elk gewenst moment in-time. Deze momentopnamen onafhankelijk van de bronschijf bestaan en kunnen worden gebruikt om nieuwe beheerde schijven te maken. Ze worden in rekening gebracht op basis van de gebruikte grootte. Bijvoorbeeld, als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GiB en de werkelijke gebruikte gegevensgrootte van 10 GiB maken, wordt momentopname alleen gefactureerd voor de gebruikte gegevensgrootte van 10 GiB.  

[Incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md) worden momenteel niet ondersteund voor Managed Disks.

Zie de volgende bronnen voor meer informatie over het maken van momentopnamen met Managed Disks:

* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

## <a name="images"></a>Installatiekopieën

Beheerde schijven bieden ook ondersteuning voor het maken van een beheerde aangepaste installatiekopie. U kunt een installatiekopie maken van uw aangepaste VHD in een opslagaccount of rechtstreeks vanaf een gegeneraliseerde VM voor (sys voorbereid). Dit proces worden vastgelegd in een één installatiekopie van alle schijven die zijn gekoppeld aan een virtuele machine beheerde, met inbegrip van zowel het besturingssysteem en gegevensschijven. Deze beheerde aangepaste installatiekopie kunt maken honderden VM's met behulp van uw aangepaste installatiekopie zonder de noodzaak om te kopiëren of een storage-accounts beheren.

Zie de volgende artikelen voor meer informatie over het maken van installatiekopieën:

* [Hoe u een beheerde installatiekopie maken van een gegeneraliseerde VM in Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Het generaliseren en vastleggen van een virtuele Linux-machine met de Azure CLI](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Installatiekopieën met momentopnamen

Vaak ziet u het woord 'afbeelding' gebruikt in combinatie met virtuele machines en u ziet nu 'momentopnamen' ook. Het is belangrijk om te begrijpen van het verschil tussen deze voorwaarden. Met Managed Disks, kunt u een installatiekopie van een gegeneraliseerde virtuele machine die is opgeheven uitvoeren. Deze installatiekopie bevat alle schijven die zijn gekoppeld aan de virtuele machine. U kunt deze installatiekopie gebruiken om te maken van een nieuwe virtuele machine en bevat alle schijven.

Een momentopname is een kopie van een schijf op het punt in tijd, die wordt het opgevat. Dit is alleen van toepassing op één schijf. Als u een virtuele machine met slechts één schijf (OS) hebt, kunt u een momentopname of een afbeelding van het ondernemen en een virtuele machine maken van de momentopname of de afbeelding.

Wat gebeurt er als een virtuele machine heeft vijf schijven en ze worden striped opgeslagen? U kunt een momentopname van elk van de schijven, maar er is geen awareness binnen de virtuele machine van de status van de schijven – de momentopnamen kennen alleen één schijf. In dit geval moeten de momentopnamen worden gecoördineerd met elkaar en momenteel niet ondersteund.

## <a name="managed-disks-and-encryption"></a>Beheerde schijven en versleuteling

Er zijn twee typen versleuteling om te bespreken betrekking tot beheerde schijven. De eerste gebeurtenis wordt Storage Service Encryption (SSE), die wordt uitgevoerd door de storage-service. De tweede waarde is Azure Disk Encryption, die u op het besturingssysteem en gegevensschijven voor uw VM's inschakelen kunt.

### <a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

[Azure Storage-Serviceversleuteling](../articles/storage/common/storage-service-encryption.md) biedt versleuteling-at-rest en Beveilig uw gegevens om te voldoen aan de beveiligings- en nalevingsverplichtingen van de organisatie. SSE is standaard ingeschakeld voor alle beheerde schijven, momentopnamen en installatiekopieën in alle regio's waar beheerde schijven beschikbaar zijn. Vanaf 10 juni 2017, alle nieuwe beheerde schijven/momentopnamen/images en nieuwe gegevens geschreven naar bestaande beheerde schijven worden automatisch versleuteld in rust met sleutels die worden beheerd door Microsoft standaard. Ga naar de [pagina met veelgestelde vragen voor schijven beheerd](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) voor meer informatie.

### <a name="azure-disk-encryption-ade"></a>Azure Disk Encryption (ADE)

Azure Disk Encryption kunt u voor het versleutelen van het besturingssysteem en gegevensschijven die wordt gebruikt door een virtuele IaaS-Machine. Deze versleuteling omvat beheerde schijven. De stations zijn versleuteld met versleutelingstechnologie van industriestandaard BitLocker voor Windows. De schijven worden versleuteld met behulp van de technologie DM-Crypt voor Linux. Het versleutelingsproces is geïntegreerd met Azure Key Vault kunt u controleren en beheren van de versleutelingssleutels voor de schijf. Zie voor meer informatie, [Azure Disk Encryption voor Windows en Linux IaaS-VM's](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Managed Disks.

### <a name="get-started-with-managed-disks"></a>Aan de slag met beheerde schijven

* [Een virtuele machine maken met behulp van Resource Manager en PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Een virtuele Linux-machine maken met behulp van de Azure CLI](../articles/virtual-machines/linux/quick-create-cli.md)

* [Een beheerde gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Een beheerde schijf toevoegen aan een virtuele Linux-machine](../articles/virtual-machines/linux/add-disk.md)

* [Beheerde schijven PowerShell-voorbeeldscripts](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Beheerde schijven gebruiken in Azure Resource Manager-sjablonen](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Managed Disks-opslag-opties vergelijken

* [Premium-SSD-schijven](../articles/virtual-machines/windows/premium-storage.md)

* [Standard-SSD en HDD-schijven](../articles/virtual-machines/windows/standard-storage.md)

### <a name="operational-guidance"></a>Gebruiksaanwijzing

* [Migreren van AWS en andere platforms naar Managed Disks in Azure](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Azure-VM's converteren naar beheerde schijven in Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md)

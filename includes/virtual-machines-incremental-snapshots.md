---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 88a9348ea7d6282b7410d5a323fd482dc82416c6
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45979593"
---
# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen
## <a name="overview"></a>Overzicht
Azure Storage biedt de mogelijkheid voor momentopnamen van blobs. Momentopnamen kunt u de status van de blob op dat punt in tijd vastleggen. In dit artikel wordt beschreven een scenario waarin u back-ups van virtuele-machineschijven met momentopnamen kunt onderhouden. U kunt deze methode gebruiken wanneer u niet wilt gebruiken van Azure Backup en Recovery-Service en wilt maken van een aangepaste back-upstrategie voor uw virtuele-machineschijven.

Virtuele machine van Azure-schijven zijn opgeslagen als pagina-blobs in Azure Storage. Omdat we zijn met een beschrijving van een back-upstrategie voor schijven van virtuele machines in dit artikel, verwijzen we naar momentopnamen in de context van pagina-blobs. Raadpleeg voor meer informatie over momentopnamen [het maken van een momentopname van een Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Wat is een momentopname van een?
Een blob-momentopname is een alleen-lezen versie van een blob die is opgenomen op een bepaald tijdstip. Nadat een momentopname is gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. Momentopnamen bieden een manier om back-up van een blob zoals deze wordt weergegeven op een tijdstip. Tot en met REST versie 2015-04-05 moest u de mogelijkheid om te kopiëren van de volledige momentopnamen. Met de REST-versie 2015-07-08 en hoger, u kunt ook kopiëren voor incrementele momentopnamen.

## <a name="full-snapshot-copy"></a>Exemplaar van de volledige momentopname
Momentopnamen kunnen worden gekopieerd naar een ander opslagaccount als een blob te bewaren van back-ups van de basis-blob. U kunt ook een momentopname van de basis-BLOB, zoals u dat de blob herstellen naar een eerdere versie kopiëren. Wanneer een momentopname van een storage-account wordt gekopieerd naar een andere, neemt het dezelfde ruimte als de basis-pagina-blob. Daarom wordt het hele momentopnamen van één opslagaccount kopiëren naar de andere traag en veel ruimte in het doelopslagaccount verbruikt.

> [!NOTE]
> Als u de basis-blob naar een andere bestemming kopieert, worden de momentopnamen van de blob niet samen met het gekopieerd. Op dezelfde manier als u een basis blob met een kopie overschrijven, momentopnamen die zijn gekoppeld aan de basis blob worden niet beïnvloed en onder de blobnaam base intact blijven.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Back-up van schijven met momentopnamen
Als een back-upstrategie voor de schijven van uw virtuele machines, u kunt periodieke momentopnamen van de schijf of pagina-blob, en kopieer ze naar een andere storage-met account hulpprogramma's zoals [kopiëren van de Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) bewerking of [AzCopy](../articles/storage/common/storage-use-azcopy.md). U kunt een momentopname van een kopiëren naar een bestemming pagina-blob met een andere naam. De resulterende bestemming pagina-blob is een beschrijfbare pagina-blob en niet een momentopname. Verderop in dit artikel wordt beschreven stappen voor het back-ups maken van virtuele-machineschijven met behulp van momentopnamen.

### <a name="restore-disks-using-snapshots"></a>Herstel de schijven met momentopnamen
Wanneer is het tijd om te herstellen van de schijf naar een stabiele versie die eerder zijn vastgelegd in een van de Backup-momentopnamen, kunt u een momentopname van een via de basispagina-blob kopiëren. Nadat de momentopname wordt gepromoveerd tot de basispagina blob, blijft van de momentopname, maar de bron wordt overschreven met behulp van een kopie die kan worden gelezen en geschreven. Verderop in dit artikel wordt beschreven stappen voor het herstellen van een eerdere versie van de schijf van de momentopname.

### <a name="implementing-full-snapshot-copy"></a>Implementatie van exemplaar van de volledige momentopname
U kunt een exemplaar van de volledige momentopname implementeren door het volgende te doen

* Eerst maakt u een momentopname van het gebruik van de basis blob de [momentopname maken van Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) bewerking.
* Kopieer vervolgens de momentopname naar een doel-opslagaccount door middel [kopiëren van de Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Herhaal dit proces voor het onderhouden van back-ups van uw basis-blob.

## <a name="incremental-snapshot-copy"></a>Momentopname van de incrementele kopie
De nieuwe functie in de [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API biedt een veel betere manier om back-up van de momentopnamen van de pagina-blobs of de schijven. De API retourneert de lijst met wijzigingen tussen de base blob en de momentopnamen, waardoor de hoeveelheid opslagruimte die wordt gebruikt voor de back-account. De API biedt ondersteuning voor pagina-blobs op Premium Storage, evenals de Standard-opslag. Met deze API kunt u sneller en efficiënter back-oplossingen bouwen voor Azure VM's. Deze API is beschikbaar in de REST-versie 2015-07-08 en hoger.

Incrementele kopie van de momentopname kunt u om te kopiëren van één opslagaccount naar de andere het verschil tussen

* Basis-blob en de momentopname of
* De twee momentopnamen van de basis-blob

De volgende voorwaarden wordt voldaan, opgegeven

* De blob is gemaakt op Jan-1-2016 of later.
* De blob is niet overschreven met [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) of [kopiëren van de Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) tussen twee momentopnamen.

**Houd er rekening mee**: deze functie is beschikbaar voor Premium en Standard Azure-pagina-Blobs.

Wanneer u een aangepaste back-upstrategie met momentopnamen hebt, de momentopnamen van één opslagaccount kopiëren naar een andere kan traag zijn en hoeveel opslagruimte kan worden gebruikt. In plaats van de momentopname van het gehele kopiëren naar een back-upopslag-account, kunt u het verschil tussen opeenvolgende momentopnamen om een back-pagina-blob te schrijven. Op deze manier wordt de tijd voor het kopiëren en de ruimte voor het opslaan van back-ups aanzienlijk verminderd.

### <a name="implementing-incremental-snapshot-copy"></a>Implementatie van de momentopname van de incrementele kopie
U kunt de momentopname van de incrementele kopie implementeren door het volgende te doen

* Een momentopname van het gebruik van de basis blob [momentopname maken van Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Kopiëren van de momentopname naar de doel-back-upopslag-account met behulp [kopiëren van de Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Dit is de back-pagina-blob. Een momentopname van de back-pagina-blob en op te slaan in de back-account.
* Maak nog een momentopname van de basis-blob met behulp van de Blob-momentopname.
* Het verschil tussen de eerste en tweede momentopnamen van het gebruik van de basis blob ophalen [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Gebruik de nieuwe parameter **prevsnapshot**, de datum/tijd-waarde van de momentopname die u wilt ophalen van het verschil met op te geven. Wanneer deze parameter aanwezig is, betekent dit dat het REST-antwoord alleen op de pagina's die zijn gewijzigd tussen doel momentopname en eerdere momentopname, inclusief wissen's bevat.
* Gebruik [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) deze wijzigingen toepassen op de back-pagina-blob.
* Ten slotte maakt u een momentopname van de back-pagina-blob en sla deze op in de back-upopslag-account.

In de volgende sectie wordt beschreven in meer detail hoe kunt u back-ups van schijven met incrementele kopie van de momentopname beheren

## <a name="scenario"></a>Scenario
In deze sectie wordt een scenario dat bestaat uit een aangepaste back-upstrategie voor VM-schijven met momentopnamen beschreven.

Houd rekening met een Azure-VM uit de DS-serie met een premium storage P30-schijf die is gekoppeld. De P30-schijf met de naam *mypremiumdisk* wordt opgeslagen in een premium storage-account met de naam *mypremiumaccount*. Een standard storage-account met de naam *mybackupstdaccount* wordt gebruikt voor het opslaan van de back-up van *mypremiumdisk*. We willen graag houden van een momentopname van *mypremiumdisk* elke 12 uur.

Zie voor meer informatie over het maken van een storage-account, [een opslagaccount maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).

Raadpleeg voor meer informatie over het back-ups van virtuele Azure-machines, [back-ups van virtuele machine van Azure van plan bent](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Stappen voor het beheren van back-ups van een schijf met incrementele momentopnamen
De volgende stappen wordt beschreven hoe u momentopnamen maken van *mypremiumdisk* en onderhouden van de back-ups in *mybackupstdaccount*. De back-up is een standaard pagina-blob met de naam *mybackupstdpageblob*. De back-pagina-blob geeft altijd dezelfde toestand als de laatste momentopname van *mypremiumdisk*.

1. De back-pagina-blob voor de premium-opslag-schijf maken door het maken van een momentopname van *mypremiumdisk* met de naam *mypremiumdisk_ss1*.
2. Deze momentopname als een pagina-blob met de naam kopiëren naar mybackupstdaccount *mybackupstdpageblob*.
3. Een momentopname van *mybackupstdpageblob* met de naam *mybackupstdpageblob_ss1*, met [momentopname maken van Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) en op te slaan in *mybackupstdaccount*.
4. Tijdens het back-upvenster maken nog een momentopname van *mypremiumdisk*, bijvoorbeeld *mypremiumdisk_ss2*, en op te slaan in *mypremiumaccount*.
5. De incrementele wijzigingen tussen de twee momentopnamen ophalen *mypremiumdisk_ss2* en *mypremiumdisk_ss1*, met [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) op *mypremiumdisk_ ss2* met de **prevsnapshot** parameter ingesteld op de tijdstempel van *mypremiumdisk_ss1*. Deze incrementele wijzigingen schrijven naar de back-pagina-blob *mybackupstdpageblob* in *mybackupstdaccount*. Als er in de incrementele wijzigingen verwijderde bereiken, moeten ze worden gewist uit de back-pagina-blob. Gebruik [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) incrementele wijzigingen schrijven naar de back-pagina-blob.
6. Een momentopname van de back-pagina-blob *mybackupstdpageblob*, met de naam *mybackupstdpageblob_ss2*. Verwijderen van de vorige momentopname *mypremiumdisk_ss1* van premium storage-account.
7. Herhaal stap 4-6 elke back-upvenster. Op deze manier kunt u back-ups van onderhouden *mypremiumdisk* in een standard storage-account.

![Back-up van schijf met incrementele momentopnamen](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Stappen voor het herstellen van een schijf van momentopnamen
De volgende stappen wordt beschreven hoe u de premium-schijf herstellen *mypremiumdisk* naar een eerdere momentopname van de back-upopslagaccount *mybackupstdaccount*.

1. Het punt in tijd die u wilt herstellen van de premium-schijf om te identificeren. Laten we zeggen dat deze momentopname *mybackupstdpageblob_ss2*, die is opgeslagen in de back-upopslagaccount *mybackupstdaccount*.
2. In mybackupstdaccount, bevordering van de momentopname *mybackupstdpageblob_ss2* als de nieuwe back-up basispagina blob *mybackupstdpageblobrestored*.
3. Een momentopname van deze pagina met een herstelde back-up-blob, met de naam *mybackupstdpageblobrestored_ss1*.
4. De herstelde pagina-blob kopiëren *mybackupstdpageblobrestored* van *mybackupstdaccount* naar *mypremiumaccount* als de nieuwe premium-schijf  *mypremiumdiskrestored*.
5. Een momentopname van *mypremiumdiskrestored*, met de naam *mypremiumdiskrestored_ss1* voor het maken van toekomstige incrementele back-ups.
6. De virtuele machine uit de DS-serie verwijzen naar de herstelde schijf *mypremiumdiskrestored* en loskoppelen van de oude *mypremiumdisk* van de virtuele machine.
7. Beginnen met de back-up wordt beschreven in de vorige sectie voor de herstelde schijf *mypremiumdiskrestored*, met de *mybackupstdpageblobrestored* als de back-pagina-blob.

![Schijf terugzetten van momentopnamen](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende koppelingen voor meer informatie over het maken van momentopnamen van een blob en het plannen van uw VM-back-upinfrastructuur.

* [Het maken van een momentopname van een Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [Uw VM-back-upinfrastructuur plannen](../articles/backup/backup-azure-vms-introduction.md)


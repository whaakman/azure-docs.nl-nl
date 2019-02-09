---
title: Informatie over niet-beheerde (pagina-blobs) en managed disks-opslag voor Microsoft Azure Windows VM's | Microsoft Docs
description: Meer informatie over de basisprincipes van niet-beheerde (pagina-blobs) en managed disks-opslag voor Windows-machines in Azure.
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.tgt_pltfrm: windows
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0caa88939e263aa5d18460144893cdbce72f10ec
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961530"
---
# <a name="about-disks-storage-for-azure-windows-vms"></a>Over schijfopslag voor Azure Windows VM 's

Net als elke andere computer gebruik virtuele machines in Azure schijven als een plaats voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle virtuele machines van Azure hebt ten minste twee schijven: de schijf van een Windows-besturingssysteem en een tijdelijke schijf. De besturingssysteemschijf is gemaakt op basis van een afbeelding, en zowel de besturingssysteemschijf en de installatiekopie van het virtuele harde schijven (VHD's) die zijn opgeslagen in Azure storage-account zijn. Virtuele machines hebben ook een of meer gegevensschijven die ook als virtuele harde schijven zijn opgeslagen. 

In dit artikel hebben we praten over de verschillende manieren worden gebruikt voor de schijven en vervolgens bespreken de verschillende soorten schijven kunt u maken en gebruiken. In dit artikel is ook beschikbaar voor [virtuele Linux-machines](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Schijven die worden gebruikt door virtuele machines

We gaan kijken hoe de schijven worden gebruikt door de virtuele machines.

### <a name="operating-system-disk"></a>Besturingssysteemschijf

Elke virtuele machine heeft een gekoppelde besturingssysteemschijf. Het is geregistreerd als een SATA-schijf en gelabeld als het station C: standaard. Deze schijf heeft een maximale capaciteit van 2048 GB (Gigabyte).

### <a name="temporary-disk"></a>Tijdelijke schijf

Elke virtuele machine bevat een tijdelijke schijf. De tijdelijke schijf opslag op korte termijn biedt voor toepassingen en processen en is bedoeld voor het opslaan van gegevens, zoals pagina-of wisselbestanden alleen. Op de tijdelijke schijf mogelijk gegevens verloren tijdens een [onderhoudsgebeurtenis](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) of wanneer u [een virtuele machine opnieuw implementeren](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tijdens een geslaagde standaard opnieuw opstarten van de virtuele machine behouden de gegevens op de tijdelijke schijf. Er zijn echter gevallen waar de gegevens niet zich blijven voordoen kunnen, zoals het verplaatsen naar een nieuwe host. Alle gegevens op het tijdelijke station mag daarom geen gegevens die essentieel is voor het systeem. Bij het ontwerpen van een toepassing die gebruikmaakt van een tijdelijke schijf als gegevenscache voor betere toepassingsprestaties, wordt ervan uitgegaan dat in uw ontwerp dat de gegevens in cache op de tijdelijke schijf verloren tijdens opnieuw opstarten en dat de toepassing moet tijd opnieuw opbouwen van de gegevens in cache voordat u een soortgelijke prestaties is bereikt.

De tijdelijke schijf wordt aangeduid als het station D: standaard en het wordt gebruikt voor het opslaan van pagefile.sys. Als u wilt deze schijf aan een andere stationsletter toewijzen, Zie [wijzigen van de stationsletter van de tijdelijke schijf Windows](change-drive-letter.md). De grootte van de tijdelijke schijf varieert, afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie, [grootten voor Windows virtual machines](sizes.md).

Zie voor meer informatie over hoe Azure gebruikt voor de tijdelijke schijf [inzicht krijgen in het tijdelijke station op Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

### <a name="data-disk"></a>Gegevensschijf

Een gegevensschijf is een VHD die gekoppeld aan een virtuele machine voor het opslaan van toepassingsgegevens, of andere gegevens die u wilt houden. Gegevensschijven worden geregistreerd als SCSI-stations en zijn gelabeld met een letter die u kiest. Elke gegevensschijf heeft een maximale capaciteit van 4095 GB, beheerde schijven hebben een maximale capaciteit van 32.767 GiB. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u aan het en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

> [!NOTE]
> Zie voor meer informatie over de capaciteit van virtuele machines, [grootten voor Windows virtual machines](sizes.md).

Azure maakt een besturingssysteemschijf, wanneer u een virtuele machine van een installatiekopie maken. Als u een installatiekopie met gegevensschijven, maakt Azure ook de gegevensschijven bij het maken van de virtuele machine. Anders toevoegen u gegevensschijven nadat u de virtuele machine hebt gemaakt.

U kunt gegevensschijven toevoegen aan een virtuele machine op elk gewenst moment door **koppelen** de schijf met de virtuele machine. U kunt een VHD die u hebt geüpload of gekopieerd naar uw storage-account gebruiken of een lege VHD die Azure voor u gemaakt. Een gegevensschijf koppelen wordt gekoppeld aan het VHD-bestand met de virtuele machine door een 'lease' op de VHD zodat deze kan niet worden verwijderd uit de opslag terwijl nog steeds gekoppeld.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

Voor de preview-grootten, Zie onze [Veelgestelde vragen over](faq-for-disks.md#new-disk-sizes-managed-and-unmanaged) voor meer informatie over welke regio's ze zijn beschikbaar in.

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Een laatste aanbeveling: TRIM gebruiken met niet-beheerde standaardschijven

Als u niet-beheerde standaardschijven (HDD), moet u TRIM inschakelen. TRIM verwijdert niet-gebruikte blokken op de schijf, zodat u alleen worden in rekening gebracht voor opslag die u daadwerkelijk gebruikt. Deze kunt kosten besparen als u grote bestanden maakt en ze vervolgens te verwijderen.

U kunt deze opdracht om te controleren of de beperkende instelling uitvoeren. Open een opdrachtprompt op uw Windows-VM en het type:

```
fsutil behavior query DisableDeleteNotify
```

Als de opdracht 0 retourneert, moet u TRIM is ingeschakeld. Als deze 1 retourneert, voert u de volgende opdracht uit om TRIM uit:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Opmerking: Ondersteuning wordt gestart met Windows Server 2012 / Windows 8 en hoger, Zie [nieuwe API kunt u apps voor het verzenden van "TRIM en toewijzing" hints op opslagmedium](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Volgende stappen
* [Een schijf koppelen](attach-disk-portal.md) extra opslag toevoegen voor uw virtuele machine.
* [Maken van een momentopname van een](snapshot-copy-managed-disk.md).
* [Converteren naar managed disks](convert-unmanaged-to-managed-disks.md).



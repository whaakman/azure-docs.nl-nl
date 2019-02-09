---
title: Informatie over niet-beheerde (pagina-blobs) en managed disks-opslag voor Microsoft Azure Linux VM's | Microsoft Docs
description: Meer informatie over de basisprincipes van niet-beheerde (pagina-blobs) en managed disks-opslag voor virtuele Linux-machines in Azure.
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 3bc7853ea306a5872e34c7e90f2bd7d6c334eafd
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55958963"
---
# <a name="about-disks-storage-for-azure-linux-vms"></a>Over de opslag van schijven voor virtuele Azure Linux-machines
Net als elke andere computer gebruik virtuele machines in Azure schijven als een plaats voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle virtuele machines van Azure hebt ten minste twee schijven: de schijf van een Linux-besturingssysteem en een tijdelijke schijf. De besturingssysteemschijf is gemaakt op basis van een afbeelding, en zowel de besturingssysteemschijf en de installatiekopie van het virtuele harde schijven (VHD's) die zijn opgeslagen in Azure storage-account zijn. Virtuele machines hebben ook een of meer gegevensschijven die ook als virtuele harde schijven zijn opgeslagen.

In dit artikel hebben we praten over de verschillende manieren worden gebruikt voor de schijven en vervolgens bespreken de verschillende soorten schijven kunt u maken en gebruiken. In dit artikel is ook beschikbaar voor [Windows virtuele machines](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Schijven die worden gebruikt door virtuele machines

We gaan kijken hoe de schijven worden gebruikt door de virtuele machines.

## <a name="operating-system-disk"></a>Besturingssysteemschijf

Elke virtuele machine heeft een gekoppelde besturingssysteemschijf. Het geregistreerd als een SATA-schijf en /dev/sda heet standaard. Deze schijf heeft een maximale capaciteit van 2048 GB (Gigabyte).

## <a name="temporary-disk"></a>Tijdelijke schijf

Elke virtuele machine bevat een tijdelijke schijf. De tijdelijke schijf opslag op korte termijn biedt voor toepassingen en processen en is bedoeld voor het opslaan van gegevens, zoals pagina-of wisselbestanden alleen. Op de tijdelijke schijf mogelijk gegevens verloren tijdens een [onderhoudsgebeurtenis](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) of wanneer u [een virtuele machine opnieuw implementeren](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tijdens een standaard opnieuw opstarten van de virtuele machine behouden de gegevens op de tijdelijke schijf. Er zijn echter gevallen waar de gegevens niet zich blijven voordoen kunnen, zoals het verplaatsen naar een nieuwe host. Alle gegevens op het tijdelijke station mag daarom geen gegevens die essentieel is voor het systeem. Bij het ontwerpen van een toepassing die gebruikmaakt van een tijdelijke schijf als gegevenscache voor betere toepassingsprestaties, wordt ervan uitgegaan dat in uw ontwerp dat de gegevens in cache op de tijdelijke schijf verloren tijdens opnieuw opstarten en dat de toepassing moet tijd opnieuw opbouwen van de gegevens in cache voordat u een soortgelijke prestaties is bereikt.

Op Linux virtuele machines, de schijf is doorgaans **/dev/sdb** en is geformatteerd en gekoppeld aan **mnt** door de Azure Linux Agent. De grootte van de tijdelijke schijf varieert, afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie, [grootten voor virtuele Linux-machines](../windows/sizes.md).

## <a name="data-disk"></a>Gegevensschijf

Een gegevensschijf is een VHD die gekoppeld aan een virtuele machine voor het opslaan van toepassingsgegevens, of andere gegevens die u wilt houden. Gegevensschijven worden geregistreerd als SCSI-stations en zijn gelabeld met een letter die u kiest. Elke gegevensschijf heeft een maximale capaciteit van 4095 GB. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u aan het en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

> [!NOTE]
> Zie voor meer informatie over de capaciteit van virtuele machines, [grootten voor virtuele Linux-machines](./sizes.md).

Azure maakt een besturingssysteemschijf, wanneer u een virtuele machine van een installatiekopie maken. Als u een installatiekopie met gegevensschijven, maakt Azure ook de gegevensschijven bij het maken van de virtuele machine. Anders toevoegen u gegevensschijven nadat u de virtuele machine hebt gemaakt.

U kunt gegevensschijven toevoegen aan een virtuele machine op elk gewenst moment door **koppelen** de schijf met de virtuele machine. U kunt een VHD die u hebt geüpload of gekopieerd naar uw storage-account of een die Azure voor u gemaakt. Een gegevensschijf koppelen wordt gekoppeld aan het VHD-bestand met de virtuele machine, door een 'lease' op de VHD zodat deze kan niet worden verwijderd uit de opslag terwijl nog steeds gekoppeld.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

Voor de preview-grootten, Zie onze [Veelgestelde vragen over](faq-for-disks.md#new-disk-sizes-managed-and-unmanaged) voor meer informatie over welke regio's ze zijn beschikbaar in.

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen

* [Een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) extra opslag toevoegen voor uw virtuele machine.
* [Maken van een momentopname van een](snapshot-copy-managed-disk.md).
* [Converteren naar managed disks](convert-unmanaged-to-managed-disks.md).

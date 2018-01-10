---
title: Over zonder begeleiding (pagina-BLOB's) en beheerde schijven opslag voor Microsoft Azure Linux VM's | Microsoft Docs
description: Meer informatie over de basisprincipes van zonder begeleiding (pagina-BLOB's) en opslag van de schijven voor virtuele Linux-machines in Azure worden beheerd.
services: virtual-machines
author: iainfoulds
manager: jeconnoc
ms.service: virtual-machines
ms.workload: storage
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 11/15/2017
ms.author: iainfou
ms.openlocfilehash: 107e332a0f8c9d5a84a74de685ca458fb29caa8b
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="about-disks-storage-for-azure-linux-vms"></a>Over schijven opslag voor Azure Linux VM 's
Net als elke andere computer gebruiken virtuele machines in Azure schijven als een plaats voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle virtuele machines in Azure hebt ten minste twee schijven: een Linux-besturingssysteem en een tijdelijke schijf. De besturingssysteemschijf is gemaakt van een installatiekopie en zowel de besturingssysteemschijf en de installatiekopie zijn daadwerkelijk opgeslagen virtuele harde schijven (VHD's) in Azure storage-account. Virtuele machines hebben ook een of meer gegevensschijven die ook als virtuele harde schijven zijn opgeslagen. 

In dit artikel wordt hebben over de verschillende manieren worden gebruikt voor de schijven, en vervolgens bespreken de verschillende typen schijven kunt u maken en gebruiken. In dit artikel is ook beschikbaar voor [Windows virtuele machines](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Schijven die worden gebruikt door virtuele machines

Eens kijken hoe de schijven worden gebruikt door de virtuele machines.

## <a name="operating-system-disk"></a>Besturingssysteemschijf
Elke virtuele machine heeft een gekoppelde besturingssysteemschijf. Het geregistreerd als een SATA harde schijf en /dev/sda heet standaard. Deze schijf heeft een maximale capaciteit van 2048 gigabyte (GB). 

## <a name="temporary-disk"></a>Tijdelijke schijf
Elke virtuele machine bevat een tijdelijke schijf. De tijdelijke schijf opslag op korte termijn biedt voor toepassingen en processen en voor het opslaan van gegevens, zoals pagina of het swap-bestanden alleen is bedoeld. Gegevens op de tijdelijke schijf zijn mogelijk verloren gegaan tijdens een [onderhoud](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) of wanneer u [opnieuw implementeren van een virtuele machine](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tijdens een standaard opnieuw opstarten van de virtuele machine, moet de gegevens op de tijdelijke schijf handhaven.

Op Linux virtuele machines, de schijf is doorgaans **/dev/sdb** en geformatteerd en is gekoppeld aan **mnt** door de Azure Linux Agent. De grootte van de tijdelijke schijf varieert, afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie [grootten voor virtuele Linux-machines](../windows/sizes.md).

Zie voor meer informatie over hoe Azure gebruikt voor de tijdelijke schijf [inzicht in de tijdelijke schijf op Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Gegevensschijf
Een gegevensschijf is een VHD die gekoppeld aan een virtuele machine voor het opslaan van toepassingsgegevens of andere gegevens die u wilt bewaren. Gegevensschijven worden geregistreerd als SCSI-stations en zijn gelabeld met een letter die u kiest. Elke gegevensschijf heeft een maximale capaciteit van 4095 GB. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u aan deze en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

> [!NOTE]
> Zie voor meer informatie over virtuele machines capaciteiten [grootten voor virtuele Linux-machines](./sizes.md).
> 

Azure maakt een besturingssysteemschijf wanneer u een virtuele machine van een installatiekopie maakt. Als u een afbeelding met gegevensschijven gebruikt, maakt Azure ook de gegevensschijven bij het maken van de virtuele machine. Anders toevoegen u gegevensschijven nadat u de virtuele machine hebt gemaakt.

U kunt gegevensschijven toevoegen aan een virtuele machine op elk gewenst moment door **koppelen** de schijf met de virtuele machine. U kunt een VHD die u hebt geüpload of gekopieerd naar uw storage-account of een Azure voor u maakt. Een gegevensschijf koppelen koppelt het VHD-bestand aan de virtuele machine door het plaatsen van een 'lease' op de VHD zodat deze kan niet worden verwijderd uit de opslag terwijl er nog steeds gekoppeld.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen
* [Een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toevoegen van extra opslagruimte voor uw virtuele machine.
* [Momentopname maken van een](snapshot-copy-managed-disk.md).
* [Converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md).


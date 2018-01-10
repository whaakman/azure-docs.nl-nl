---
title: Over zonder begeleiding (pagina-BLOB's) en beheerde schijven opslag voor Microsoft Azure VM's van Windows | Microsoft Docs
description: Meer informatie over de basisprincipes van zonder begeleiding (pagina-BLOB's) en opslag van de schijven voor Windows virtuele machines in Azure worden beheerd.
services: virtual-machines
author: iainfoulds
manager: jeconnoc
ms.service: virtual-machines
ms.workload: storage
ms.tgt_pltfrm: windows
ms.topic: article
ms.date: 11/15/2017
ms.author: iainfou
ms.openlocfilehash: bf5c5cc0637b9a515bf567ff8933170d7fc1a8ba
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="about-disks-storage-for-azure-windows-vms"></a>Over schijven storage voor VM's van Windows Azure
Net als elke andere computer gebruiken virtuele machines in Azure schijven als een plaats voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle virtuele machines in Azure hebt ten minste twee schijven: de schijf van een Windows-besturingssysteem en een tijdelijke schijf. De besturingssysteemschijf wordt gemaakt van een installatiekopie en zowel de besturingssysteemschijf en de installatiekopie zijn virtuele harde schijven (VHD's) opgeslagen in Azure storage-account. Virtuele machines hebben ook een of meer gegevensschijven die ook als virtuele harde schijven zijn opgeslagen. 

In dit artikel wordt hebben over de verschillende manieren worden gebruikt voor de schijven, en vervolgens bespreken de verschillende typen schijven kunt u maken en gebruiken. In dit artikel is ook beschikbaar voor [virtuele Linux-machines](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Schijven die worden gebruikt door virtuele machines

Eens kijken hoe de schijven worden gebruikt door de virtuele machines.

### <a name="operating-system-disk"></a>Besturingssysteemschijf
Elke virtuele machine heeft een gekoppelde besturingssysteemschijf. Het is geregistreerd als een SATA harde schijf en aangeduid als het station C: standaard. Deze schijf heeft een maximale capaciteit van 2048 gigabyte (GB). 

### <a name="temporary-disk"></a>Tijdelijke schijf
Elke virtuele machine bevat een tijdelijke schijf. De tijdelijke schijf opslag op korte termijn biedt voor toepassingen en processen en voor het opslaan van gegevens, zoals pagina of het swap-bestanden alleen is bedoeld. Gegevens op de tijdelijke schijf zijn mogelijk verloren gegaan tijdens een [onderhoud](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) of wanneer u [opnieuw implementeren van een virtuele machine](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tijdens een standaard opnieuw opstarten van de virtuele machine, moet de gegevens op de tijdelijke schijf handhaven.

De tijdelijke schijf wordt aangeduid als het station D: standaard en het wordt gebruikt voor het opslaan van pagefile.sys. Als u wilt deze schijf aan een andere stationsletter toewijzen, Zie [wijzigen van de stationsletter van de tijdelijke schijf Windows](change-drive-letter.md). De grootte van de tijdelijke schijf varieert, afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie [grootten voor Windows virtuele machines](sizes.md).

Zie voor meer informatie over hoe Azure gebruikt voor de tijdelijke schijf [inzicht in de tijdelijke schijf op Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Gegevensschijf
Een gegevensschijf is een VHD die gekoppeld aan een virtuele machine voor het opslaan van toepassingsgegevens of andere gegevens die u wilt bewaren. Gegevensschijven worden geregistreerd als SCSI-stations en zijn gelabeld met een letter die u kiest. Elke gegevensschijf heeft een maximale capaciteit van 4095 GB. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u aan deze en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

> [!NOTE]
> Zie voor meer informatie over de capaciteit van virtuele machines, [grootten voor Windows virtuele machines](sizes.md).
> 

Azure maakt een besturingssysteemschijf wanneer u een virtuele machine van een installatiekopie maakt. Als u een afbeelding met gegevensschijven gebruikt, maakt Azure ook de gegevensschijven bij het maken van de virtuele machine. Anders toevoegen u gegevensschijven nadat u de virtuele machine hebt gemaakt.

U kunt gegevensschijven toevoegen aan een virtuele machine op elk gewenst moment door **koppelen** de schijf met de virtuele machine. U kunt een VHD die u hebt geüpload of gekopieerd naar uw opslagaccount gebruiken of een lege VHD die Azure voor u maakt. Een gegevensschijf koppelen koppelt het VHD-bestand aan de virtuele machine door het plaatsen van een 'lease' op de VHD zodat deze kan niet worden verwijderd uit de opslag terwijl er nog steeds gekoppeld.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Een laatste aanbeveling: gebruik TRIM met niet-beheerde standaardschijven 

Als u niet-beheerde standaard schijven (HDD) gebruikt, moet u TRIM inschakelen. TRIM worden niet-gebruikte blokken op de schijf verwijderd, zodat u wordt alleen gefactureerd voor opslag die u daadwerkelijk gebruikt. Dit kunt besparen op kosten als u grote bestanden maken en deze vervolgens te verwijderen. 

U kunt deze opdracht om te controleren van de beperkende instelling uitvoeren. Open een opdrachtprompt op de virtuele machine van Windows en typ:


```
fsutil behavior query DisableDeleteNotify
```

Als de opdracht 0 retourneert, wordt ' trim ' correct ingeschakeld. Als deze 1 retourneert, voert u de volgende opdracht voor het vrijmaken van opslagruimte inschakelen:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Opmerking: Ondersteuning begint met Windows Server 2012 / Windows 8 en hoger, Zie Zie [nieuwe API apps hints 'TRIM en ontkoppelen' verzenden naar opslagmedia toestaan](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Volgende stappen
* [Een schijf koppelen](attach-disk-portal.md) toevoegen van extra opslagruimte voor uw virtuele machine.
* [Momentopname maken van een](snapshot-copy-managed-disk.md).
* [Converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md).



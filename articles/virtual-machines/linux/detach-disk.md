---
title: Loskoppelen van een gegevensschijf van een Linux-VM - Azure | Microsoft Docs
description: Leer hoe u een gegevensschijf van een virtuele machine in Azure met behulp van Azure CLI of Azure portal ontkoppelen.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 07/18/2018
ms.author: cynthn
ms.component: disks
ms.openlocfilehash: 5a0c2f91c5290e679cc2a3ac03cbf226ceb902da
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477699"
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Loskoppelen van een gegevensschijf van een virtuele Linux-machine

Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Hiermee wordt de schijf van de virtuele machine verwijderd, maar niet verwijderd uit de opslag. In dit artikel werken we met een Ubuntu-16.04 LTS-verdeling. Als u een ander distributiepunt gebruikt, worden de instructies voor het ontkoppelen van de schijf kunnen afwijken.

> [!WARNING]
> Als u een schijf loskoppelen wordt deze niet automatisch verwijderd. Als u bent geabonneerd op Premium-opslag, blijft u opslag in rekening gebracht voor de schijf. Zie voor meer informatie, [prijzen en facturering bij het gebruik van Premium Storage](../windows/premium-storage.md#pricing-and-billing). 
> 
> 

Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.  


## <a name="connect-to-the-vm-to-unmount-the-disk"></a>Verbinding maken met de virtuele machine de schijf ontkoppelen

Voordat u de schijf met de CLI of de portal loskoppelen kunt, moet u moet worden ontkoppeld van de schijf en verwijzingen naar verwijderd uit het fstab-bestand.

Maak verbinding met de VM. In dit voorbeeld wordt het openbare IP-adres van de virtuele machine is *10.0.1.4* met de gebruikersnaam *azureuser*: 

```bash
ssh azureuser@10.0.1.4
```

Zoek eerst de gegevensschijf die u wilt loskoppelen. Het volgende voorbeeld wordt dmesg om te filteren op SCSI-schijven:

```bash
dmesg | grep SCSI
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Hier *sdc* is de schijf die we willen loskoppelen. U moet ook de UUID van de schijf ophalen.

```bash
sudo -i blkid
```

De uitvoer lijkt op het volgende voorbeeld:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```


Bewerk de */etc/fstab* bestand verwijzingen naar de schijf verwijderen. 

> [!NOTE]
> Onjuist bewerken van de **/etc/fstab** bestand kan leiden tot een systeem opgestart. Als u niet zeker, verwijzen naar de distributie van documentatie voor meer informatie over hoe u dit bestand goed te bewerken. Het verdient ook dat er een back-up van het bestand/etc/fstab voordat u bewerkt wordt gemaakt.

Open de */etc/fstab* bestand in een teksteditor als volgt:

```bash
sudo vi /etc/fstab
```

In dit voorbeeld de volgende regel moet worden verwijderd uit de */etc/fstab* bestand:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

Gebruik `umount` ontkoppelen van de schijf. Het volgende voorbeeld wordt de */dev/sdc1* partitioneren van de */datadrive* koppelpunt:

```bash
sudo umount /dev/sdc1 /datadrive
```


## <a name="detach-a-data-disk-using-azure-cli"></a>Een gegevensschijf met behulp van Azure CLI ontkoppelen 

In dit voorbeeld losgekoppeld de *myDataDisk* schijf van VM met de naam *myVM* in *myResourceGroup*.

```azurecli
az vm disk detach \
    -g myResourceGroup \
    --vm-name myVm \
    -n myDataDisk
```

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.


## <a name="detach-a-data-disk-using-the-portal"></a>Een gegevensschijf ontkoppelen via de portal

1. Selecteer in het menu links **virtuele Machines**.
2. Selecteer de virtuele machine waarvoor de gegevensschijf die u wilt loskoppelen en klikt u op **stoppen** toewijzing van de virtuele machine.
3. Selecteer in het deelvenster met de virtuele machine **schijven**.
4. Aan de bovenkant van de **schijven** venster **bewerken**.
5. In de **schijven** in het deelvenster aan de rechterkant van de gegevensschijf die u wilt loskoppelen, klikt u op de ![afbeelding van de knop ontkoppelen](./media/detach-disk/detach.png) knop loskoppelen.
5. Nadat de schijf is verwijderd, klikt u op opslaan boven aan het deelvenster.
6. Klik in het deelvenster virtuele machine **overzicht** en klik vervolgens op de **Start** knop aan de bovenkant van het deelvenster met de virtuele machine opnieuw opstarten.

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.



## <a name="next-steps"></a>Volgende stappen
Als u de gegevensschijf opnieuw gebruiken wilt, kunt u zojuist hebt [koppelen aan een andere virtuele machine](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


---
title: Een gegevensschijf toevoegen aan de Linux-VM met de Azure CLI | Microsoft Docs
description: Leer hoe u een permanente gegevensschijf toevoegen aan uw Linux-VM met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 06/13/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.subservice: disks
ms.openlocfilehash: 1f545747b883ab70b597b4e598a86b192f89b027
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892758"
---
# <a name="add-a-disk-to-a-linux-vm"></a>Een schijf toevoegen aan een virtuele Linux-machine
Dit artikel ziet u hoe u een permanente schijf koppelen aan uw virtuele machine zodat u kunt uw gegevens - behouden, zelfs als uw virtuele machine is ingericht vanwege onderhoud vergroten of verkleinen.

## <a name="attach-a-new-disk-to-a-vm"></a>Een nieuwe schijf koppelen aan een virtuele machine

Als u een nieuwe, lege gegevensschijf toevoegen op de virtuele machine wilt, gebruikt u de [az vm disk attach](/cli/azure/vm/disk?view=azure-cli-latest) opdracht met de `--new` parameter. Als uw virtuele machine in een Beschikbaarheidszone is, wordt de schijf wordt automatisch gemaakt in dezelfde regio bevindt als de virtuele machine. Zie voor meer informatie, [overzicht van Beschikbaarheidszones](../../availability-zones/az-overview.md). Het volgende voorbeeld wordt een schijf met de naam *myDataDisk* dat wil zeggen 50 Gb in grootte:

```azurecli
az vm disk attach \
   -g myResourceGroup \
   --vm-name myVM \
   --disk myDataDisk \
   --new \
   --size-gb 50
```

## <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen

Als u wilt een bestaande schijf koppelen, de schijf-ID zoeken en geef de ID op door de [az vm disk attach](/cli/azure/vm/disk?view=azure-cli-latest) opdracht. De volgende voorbeeldquery's voor een schijf met de naam *myDataDisk* in *myResourceGroup*, koppelt u deze aan de virtuele machine met de naam *myVM*:

```azurecli
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)

az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Verbinding maken met de Linux-VM naar de nieuwe schijf koppelen

Om te partitioneren, formatteren en koppel de nieuwe schijf, zodat uw Linux-VM kan worden gebruikt, SSH in uw virtuele machine. Zie voor meer informatie [SSH gebruiken met Linux op Azure](mac-create-ssh-keys.md). Het volgende voorbeeld maakt verbinding met een virtuele machine met de openbare DNS-vermelding van *mypublicdns.westus.cloudapp.azure.com* met de gebruikersnaam *azureuser*:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Eenmaal verbinding hebben met uw virtuele machine, u kunt een schijf koppelen. Zoek eerst de schijf met `dmesg` (de methode die u gebruikt voor het detecteren van de nieuwe schijf kan variëren). Het volgende voorbeeld wordt dmesg om te filteren op *SCSI* schijven:

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

Hier *sdc* is de schijf die we willen. Partitioneer de schijf met `parted`, als de schijfgrootte 2 tebibytes (TiB is) of groter en vervolgens moet u GPT-partitionering, als deze onder 2TiB, kunt u partitioneren MBR of GPT gebruiken. Als u partitioneren MBR, kunt u `fdisk`. Een primaire schijf op partitie 1 maken en de andere standaardwaarden te accepteren. Het volgende voorbeeld wordt de `fdisk` op */dev/sdc*:

```bash
sudo fdisk /dev/sdc
```

Gebruik de `n` opdracht voor het toevoegen van een nieuwe partitie. In dit voorbeeld wordt er ook voor kiezen `p` voor een primaire partitie en de rest van de standaardwaarden accepteren. De uitvoer is vergelijkbaar met het volgende voorbeeld:

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

De partitietabel afdrukken door te typen `p` en gebruik vervolgens `w` te schrijven in de tabel schijf en af te sluiten. De uitvoer moet eruitzien zoals in het volgende voorbeeld:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Schrijf u nu een bestandssysteem naar de partitie met de `mkfs` opdracht. Geef het type bestandssysteem en de naam van het apparaat. Het volgende voorbeeld wordt een *ext4* bestandssysteem op het */dev/sdc1* partitie die is gemaakt in de voorgaande stappen:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

De uitvoer lijkt op die in het volgende voorbeeld:

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Maak nu een map voor het koppelen van het bestandssysteem via `mkdir`. Het volgende voorbeeld wordt een map op */datadrive*:

```bash
sudo mkdir /datadrive
```

Gebruik `mount` vervolgens koppelen van het bestandssysteem. Hiermee koppelt u het volgende voorbeeld de */dev/sdc1* partitie op de */datadrive* koppelpunt:

```bash
sudo mount /dev/sdc1 /datadrive
```

Om ervoor te zorgen dat het station na het opnieuw opstarten automatisch wordt gekoppeld, moet deze worden toegevoegd aan de */etc/fstab* bestand. Het is ook ten zeerste aanbevolen dat de UUID (Universally Unique IDentifier) wordt gebruikt in */etc/fstab* om te verwijzen naar de schijf in plaats van alleen de naam van het apparaat (zoals */dev/sdc1*). Als het besturingssysteem wordt een fout gedetecteerd tijdens het opstarten, voorkomt met behulp van de UUID de onjuiste schijf is gekoppeld aan een bepaalde locatie. Resterende gegevensschijven kan vervolgens worden toegewezen die dezelfde apparaat-id's. Als u zoekt de UUID van het nieuwe station, gebruiken de `blkid` hulpprogramma:

```bash
sudo blkid
```

De uitvoer lijkt op het volgende voorbeeld:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> Onjuist bewerken van de **/etc/fstab** bestand kan leiden tot een systeem opgestart. Als u niet zeker, verwijzen naar de distributie van documentatie voor meer informatie over hoe u dit bestand goed te bewerken. Het verdient ook dat er een back-up van het bestand/etc/fstab voordat u bewerkt wordt gemaakt.

Open vervolgens de */etc/fstab* bestand in een teksteditor als volgt:

```bash
sudo vi /etc/fstab
```

In dit voorbeeld gebruikt u de UUID-waarde voor de */dev/sdc1* apparaat dat is gemaakt in de vorige stappen, en het koppelpunt van */datadrive*. Voeg de volgende regel toe aan het einde van de */etc/fstab* bestand:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Later een gegevensschijf verwijderen zonder te bewerken van fstab kan ervoor zorgen dat de virtuele machine niet kunnen worden opgestart. De meeste distributies bieden ofwel de *nofail* en/of *nobootwait* fstab-opties. Deze opties kunt een systeem om op te starten, zelfs als de schijf koppelen bij het opstarten is mislukt. De documentatie van uw distributie voor meer informatie over deze parameters.
> 
> De *nofail* optie zorgt ervoor dat de virtuele machine wordt gestart, zelfs als het bestandssysteem beschadigd is of de schijf niet bij het opstarten bestaat. Zonder deze optie kunnen optreden gedrag zoals beschreven in [niet kan SSH naar Linux-VM vanwege FSTAB-fouten](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP ondersteuning voor Linux in Azure
Sommige Linux kernels ondersteund TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze functie is vooral handig in de standard-opslag om te informeren over Azure die pagina's verwijderd niet langer geldig zijn en kunnen worden verwijderd en geld kunt besparen als u grote bestanden maakt en ze vervolgens te verwijderen.

Er zijn twee manieren om in te schakelen TRIM ondersteuning in uw Linux-VM. Raadpleeg uw distributie zoals gewoonlijk voor de aanbevolen aanpak:

* Gebruik de `discard` koppelen in de optie */etc/fstab*, bijvoorbeeld:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* In sommige gevallen kan de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` handmatig vanaf de opdrachtregel de opdracht of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Problemen oplossen

[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen

* Om ervoor te zorgen voor uw Linux-VM correct is geconfigureerd, Controleer de [optimaliseren de prestaties van uw Linux-machine](optimization.md) aanbevelingen.
* Uw opslagcapaciteit uitbreiden door extra schijven toe te voegen en [RAID configureren](configure-raid.md) voor extra prestaties.
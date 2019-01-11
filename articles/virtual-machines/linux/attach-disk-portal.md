---
title: Een gegevensschijf koppelen aan een Linux-VM | Microsoft Docs
description: Nieuwe of bestaande gegevensschijf koppelen aan een Linux-VM via de portal.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: 519fd063e52d1e202ea76db0fd4be15ebd117cd0
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214926"
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>Een gegevensschijf koppelen aan een Linux-VM via de portal 
In dit artikel wordt beschreven hoe u zowel nieuwe als bestaande schijven koppelt aan een virtuele Linux-machine via de Azure-portal. U kunt ook [een gegevensschijf koppelen aan een Windows-VM in Azure portal](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Voordat u schijven aan uw virtuele machine koppelt, controleert u de volgende tips:

* De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie, [grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Voor het gebruik van Premium-opslag, moet u een virtuele machine uit de DS- of GS-serie. U kunt Premium en Standard disks gebruiken met deze virtuele machines. Premium storage is beschikbaar in bepaalde regio's. Zie voor meer informatie, [Premium Storage: Opslag met hoge prestaties voor workloads van virtuele Azure-machines](../windows/premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Schijven die zijn gekoppeld aan virtuele machines zijn daadwerkelijk .vhd-bestanden zoals opgeslagen in Azure. Zie voor meer informatie, [over schijven en VHD's voor virtuele machines](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Na de schijf koppelen, moet u [verbinding maken met de Linux-VM om te koppelen van de nieuwe schijf](#connect-to-the-linux-vm-to-mount-the-new-disk).


## <a name="find-the-virtual-machine"></a>De virtuele machine gevonden
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links op **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
4. Aan de virtuele machines van pagina, in **Essentials**, klikt u op **schijven**.
   
    ![De Schijfinstellingen van de openen](./media/attach-disk-portal/find-disk-settings.png)


## <a name="attach-a-new-disk"></a>Een nieuwe schijf koppelen

1. Op de **schijven** deelvenster, klikt u op **+ gegevensschijf toevoegen**.
2. Klik op de vervolgkeuzelijst voor **naam** en selecteer **maken-schijf**:

    ![Maak Azure beheerde schijf](./media/attach-disk-portal/create-new-md.png)

3. Voer een naam voor uw beheerde schijf. Controleer de standaardinstellingen, indien nodig bijwerken en klik vervolgens op **maken**.
   
   ![Controleer de Schijfinstellingen](./media/attach-disk-portal/create-new-md-settings.png)

4. Klik op **opslaan** de beheerde schijf maken en bijwerken van de VM-configuratie:

   ![Nieuwe Azure beheerde schijf opslaan](./media/attach-disk-portal/confirm-create-new-md.png)

5. Nadat Azure de schijf wordt gemaakt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen van de schijven van de virtuele machine onder **gegevensschijven**. Als u beheerde schijven worden een resource op het hoogste niveau, de schijf wordt weergegeven in de hoofdmap van de resourcegroep:

   ![Azure Managed Disk in resourcegroep](./media/attach-disk-portal/view-md-resource-group.png)

## <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen
1. Op de **schijven** deelvenster, klikt u op **+ gegevensschijf toevoegen**.
2. Klik op de vervolgkeuzelijst voor **naam** om een lijst met bestaande beheerde schijven toegankelijk voor uw Azure-abonnement weer te geven. Selecteer de beheerde schijf te koppelen:

   ![Bestaande Azure beheerde schijf koppelen](./media/attach-disk-portal/select-existing-md.png)

3. Klik op **opslaan** voor het koppelen van de bestaande beheerde schijf en bijwerken van de VM-configuratie:
   
   ![Door Azure beheerde schijf updates opslaan](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Nadat Azure de schijf is gekoppeld aan de virtuele machine, wordt deze weergegeven in de instellingen van de schijven van de virtuele machine onder **gegevensschijven**.

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

Hier *sdc* is de schijf die we willen. 

### <a name="partion-a-new-disk"></a>Een nieuwe schijf Partion
Als u van een bestaande schijf die gegevens bevat gebruikmaakt, gaat u naar de schijf koppelen. Als u een nieuwe schijf toevoegen wilt, moet u voor het partitioneren van de schijf.

Gebruik `fdisk` als u wilt de schijf partitioneren, dat een primaire schijf op partitie 1, en de andere standaardwaarden te accepteren. Het volgende voorbeeld wordt de `fdisk` op */dev/sdc*:

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
### <a name="mount-the-disk"></a>De schijf koppelen
Maak een map voor het koppelen van het bestandssysteem via `mkdir`. Het volgende voorbeeld wordt een map op */datadrive*:

```bash
sudo mkdir /datadrive
```

Gebruik `mount` vervolgens koppelen van het bestandssysteem. Hiermee koppelt u het volgende voorbeeld de */dev/sdc1* partitie op de */datadrive* koppelpunt:

```bash
sudo mount /dev/sdc1 /datadrive
```

Om ervoor te zorgen dat het station na het opnieuw opstarten automatisch wordt gekoppeld, moet deze worden toegevoegd aan de */etc/fstab* bestand. Het is ook ten zeerste aanbevolen dat de UUID (Universally Unique IDentifier) wordt gebruikt in */etc/fstab* om te verwijzen naar de schijf in plaats van alleen de naam van het apparaat (zoals */dev/sdc1*). Als het besturingssysteem wordt een fout gedetecteerd tijdens het opstarten, voorkomt met behulp van de UUID de onjuiste schijf is gekoppeld aan een bepaalde locatie. Resterende gegevensschijven kan vervolgens worden toegewezen die dezelfde apparaat-id's. Als u zoekt de UUID van het nieuwe station, gebruiken de `blkid` hulpprogramma:

```bash
sudo -i blkid
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

## <a name="next-steps"></a>Volgende stappen
U kunt ook [een gegevensschijf koppelen](add-disk.md) met de Azure CLI.

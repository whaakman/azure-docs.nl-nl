---
title: Een schijf toevoegen aan de Linux-VM met de Azure CLI | Microsoft Docs
description: Meer informatie over een permanente schijf toevoegen aan uw Linux-VM met de Azure CLI 1.0 en 2.0.
keywords: virtuele Linux-machine, resource-schijf toevoegen
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 02/02/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3d3e3468b491f366473899f5d073704ea9a95ea
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30837003"
---
# <a name="add-a-disk-to-a-linux-vm"></a>Een schijf toevoegen aan een virtuele Linux-machine
In dit artikel laat zien hoe een permanente schijf koppelen met uw virtuele machine zodat u kunt uw gegevens - zelfs als uw virtuele machine is ingericht vanwege onderhoud vergroten of verkleinen. 


## <a name="use-managed-disks"></a>Gebruik de schijven van de beheerde
Met Azure Managed Disks wordt het beheren van schrijven voor virtuele Azure-machines eenvoudiger. Hierbij worden namelijk de opslagaccounts die zijn gekoppeld aan de VM-schijven beheerd. U hoeft alleen het type (Premium of Standard) en de grootte op te geven van de schijf die u nodig hebt. Azure maakt en beheert de schijf dan voor u. Zie voor meer informatie [schijven beheerd overzicht](managed-disks-overview.md).


### <a name="attach-a-new-disk-to-a-vm"></a>Een nieuwe schijf koppelen aan een virtuele machine
Als u alleen een nieuwe schijf nodig op de virtuele machine, gebruikt u de [az vm schijf koppelen](/cli/azure/vm/disk?view=azure-cli-latest#az_vm_disk_attach) opdracht met de `--new` parameter. Als uw virtuele machine zich in een Zone beschikbaarheid, wordt de schijf automatisch gemaakt in dezelfde regio bevindt als de virtuele machine. Zie voor meer informatie [overzicht van de Zones van de beschikbaarheid](../../availability-zones/az-overview.md). Het volgende voorbeeld wordt een schijf met de naam *myDataDisk* die *50*Gb in grootte:

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a>Een bestaande schijf koppelen 
In veel gevallen. u koppelt schijven die al zijn gemaakt. De schijf-ID vinden voor een bestaande schijf koppelen, en de ID op te geven de [az vm schijf koppelen](/cli/azure/vm/disk?view=azure-cli-latest#az_vm_disk_attach) opdracht. De volgende voorbeeldquery's voor een schijf met de naam *myDataDisk* in *myResourceGroup*, koppelt u deze vervolgens aan met de virtuele machine met de naam *myVM*:

```azurecli
# find the disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

De uitvoer ziet er ongeveer als volgt (u kunt de `-o table` optie als u wilt willekeurige opdracht van de uitvoer in opmaken):

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="use-unmanaged-disks"></a>Niet-beheerde schijven gebruiken
Niet-beheerde schijven is extra overhead maken en beheren van de onderliggende storage-accounts vereist. Niet-beheerde schijven worden gemaakt in hetzelfde opslagaccount als de besturingssysteemschijf. Gebruik wilt maken en koppelen van een niet-beheerde schijf, de [zonder begeleiding az vm-schijf koppelen](/cli/azure/vm/unmanaged-disk?view=azure-cli-latest#az_vm_unmanaged_disk_attach) opdracht. Het volgende voorbeeld wordt een *50*GB onbeheerde schijf naar de virtuele machine met de naam *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```


## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Verbinding maken met de Linux-VM naar de nieuwe schijf koppelen
Aan de partitie formatteren, en de nieuwe schijf koppelen zodat uw Linux-VM kan worden gebruikt, SSH in uw Azure-virtuele machine. Zie voor meer informatie [SSH gebruiken met Linux op Azure](mac-create-ssh-keys.md). Het volgende voorbeeld maakt verbinding met een virtuele machine met de openbare DNS-vermelding van *mypublicdns.westus.cloudapp.azure.com* met de gebruikersnaam *azureuser*: 

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Eenmaal zijn verbonden met uw virtuele machine, kunt u kunt een schijf koppelen. Zoek eerst de schijf met behulp van `dmesg` (de methode die u gebruikt voor het detecteren van de nieuwe schijf kan variëren). Het volgende voorbeeld wordt dmesg kunt u filteren op *SCSI* schijven:

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

Hier *sdc* is de schijf die we willen. De schijf met partitioneren `fdisk`, een primaire schijf op partitie 1 maken en de andere standaardinstellingen accepteren. Het volgende voorbeeld wordt de `fdisk` op */dev/sdc*:

```bash
sudo fdisk /dev/sdc
```

De uitvoer lijkt op die in het volgende voorbeeld:

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

De partitie maken door te typen `p` bij de opdrachtprompt als volgt:

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

Nu een bestandssysteem schrijven naar de partitie met de `mkfs` opdracht. Geef het type van uw bestandssysteem en de naam van het apparaat. Het volgende voorbeeld wordt een *ext4* bestandssysteem op het */dev/sdc1* partitie die is gemaakt in de voorgaande stappen:

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

Maak nu een directory te koppelen van het bestandssysteem via `mkdir`. Het volgende voorbeeld maakt u een map op */datadrive*:

```bash
sudo mkdir /datadrive
```

Gebruik `mount` vervolgens koppelen van het bestandssysteem. Het volgende voorbeeld koppelt de */dev/sdc1* partitie voor de */datadrive* koppelpunt:

```bash
sudo mount /dev/sdc1 /datadrive
```

Om ervoor te zorgen dat de schijf automatisch na opnieuw opstarten van opnieuw is gekoppeld, moet deze worden toegevoegd aan de */etc/fstab* bestand. Het is ook raadzaam dat de UUID (Universally Unique IDentifier) wordt gebruikt in */etc/fstab* om te verwijzen naar het station in plaats van alleen de naam van het apparaat (zoals */dev/sdc1*). Als het besturingssysteem een schijffout tijdens het opstarten detecteert, voorkomt met behulp van de UUID de onjuiste schijf wordt gekoppeld aan een bepaalde locatie. Resterende gegevensschijven zou worden toegewezen die dezelfde apparaat-id. Als de UUID van het nieuwe station zoekt, volgt u de `blkid` hulpprogramma:

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
> Onjuist bewerken van de **/etc/fstab** bestand kan leiden tot een systeem opgestart. Als u niet zeker, Raadpleeg de distributie-documentatie voor informatie over het correct dit bestand te bewerken. Het is ook raadzaam dat een back-up van het bestand /etc/fstab is gemaakt voordat u bewerkt.

Open vervolgens de */etc/fstab* bestand in een teksteditor als volgt:

```bash
sudo vi /etc/fstab
```

In dit voorbeeld gebruiken we de UUID-waarde voor de */dev/sdc1* apparaat dat is gemaakt in de vorige stappen en het koppelpunt van */datadrive*. Voeg de volgende regel toe aan het einde van de */etc/fstab* bestand:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Later een gegevensschijf verwijderen zonder te bewerken fstab kan ervoor zorgen dat de virtuele machine niet kunnen worden opgestart. De meeste distributies Geef ofwel de *nofail* en/of *nobootwait* fstab-opties. Deze opties kunt een systeem op te starten, zelfs als de schijf niet koppelen tijdens het opstarten. Raadpleeg de distributie-documentatie voor meer informatie over deze parameters.
> 
> De *nofail* optie zorgt ervoor dat de VM start zelfs als het bestandssysteem beschadigd is of de schijf tijdens het opstarten niet bestaat. Zonder deze optie kunnen optreden gedrag zoals beschreven in [niet kan SSH voor Linux VM vanwege FSTAB-fouten](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP ondersteuning voor Linux in Azure
Sommige kernels Linux ondersteuning TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze functie is vooral handig in standard-opslag om te informeren over Azure die pagina's verwijderd zijn niet langer geldig en kan worden genegeerd en geld kunt besparen als u grote bestanden maken en deze vervolgens te verwijderen.

Er zijn twee manieren om in te schakelen TRIM ondersteunen in uw Linux-VM. Raadpleeg uw distributiepunt gebruikelijke voor de aanbevolen aanpak:

* Gebruik de `discard` koppelen optie in */etc/fstab*, bijvoorbeeld:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* In sommige gevallen kan de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` opdracht handmatig vanaf de opdrachtregel of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:
  
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
* Denk eraan dat de nieuwe schijf is niet beschikbaar voor de virtuele machine als deze opnieuw is opgestart tenzij u deze informatie om te schrijven uw [fstab](http://en.wikipedia.org/wiki/Fstab) bestand.
* Bekijk uw Linux-VM juist is geconfigureerd, zodat de [optimaliseren van de prestaties van uw Linux-machine](optimization.md) aanbevelingen.
* Uw opslagcapaciteit uitbreiden door extra schijven toe te voegen en [configureren RAID](configure-raid.md) voor extra prestaties.


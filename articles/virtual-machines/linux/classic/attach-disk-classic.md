---
title: Een schijf koppelen aan een Linux-VM in Azure | Microsoft Docs
description: Lees hoe u een gegevensschijf koppelen aan een Linux-VM met behulp van het klassieke implementatiemodel en de schijf, zodat deze gereed voor gebruik initialiseren
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: cynthn
ms.openlocfilehash: b5bb3a9353f83cb569988a068f3ca02da85f739c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929148"
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Een gegevensschijf koppelen aan een virtuele Linux-Machine
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie hoe u [een gegevensschijf koppelen via het Resource Manager-implementatiemodel](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

U kunt zowel lege schijven als schijven met gegevens naar uw Azure-VM's koppelen. Beide typen schijven zijn .vhd-bestanden die zich in een Azure storage-account bevinden. Als met een schijf toevoegen aan een virtuele Linux machine, moet nadat u de schijf koppelen u voor het initialiseren en formatteren, zodat deze gereed voor gebruik. Dit artikel vindt u zowel de lege schijven en de schijven al die gegevens naar uw virtuele machines, evenals hoe u vervolgens initialiseren en formatteren van een nieuwe schijf koppelen.

> [!NOTE]
> Het is een aanbevolen procedure voor het gebruik van een of meer afzonderlijke schijven voor het opslaan van gegevens van een virtuele machine. Wanneer u een virtuele Azure-machine maakt, bevat een besturingssysteemschijf en een tijdelijke schijf. **Gebruik niet de tijdelijke schijf voor het opslaan van uw permanente gegevens.** Als de naam al aangeeft, biedt deze alleen tijdelijke opslag. Het biedt geen redundantie of back-up omdat deze zich niet in Azure storage.
> De tijdelijke schijf wordt meestal beheerd door de Azure Linux Agent en automatisch worden gekoppeld aan **/mnt/resource** (of **mnt** op Ubuntu-installatiekopieën). Aan de andere kant een gegevensschijf kan worden met de naam van de Linux-kernel er ongeveer als `/dev/sdc`, en u wilt partitioneren, formatteren en koppelen van deze resource. Zie de [gebruikershandleiding voor Azure Linux Agent] [ Agent] voor meer informatie.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Initialiseren van een nieuwe gegevensschijf in Linux
1. SSH naar uw virtuele machine. Zie voor meer informatie, [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het][Logon].
2. Vervolgens moet u de apparaat-id voor de gegevensschijf initialiseren vinden. Er zijn twee manieren om dat te doen:
   
    (a) Grep voor SCSI-apparaten in de logboeken, zoals in de volgende opdracht uit:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Voor recente Ubuntu-distributies, moet u mogelijk gebruiken `sudo grep SCSI /var/log/syslog` omdat aanmelding `/var/log/messages` standaard kan worden uitgeschakeld.
   
    Hier vindt u de id van de laatste gegevensschijf die is toegevoegd in de berichten die worden weergegeven.
   
    ![De schijf-berichten ophalen](./media/attach-disk/scsidisklog.png)
   
    OF
   
    (b) Gebruik de `lsscsi` opdracht om de apparaat-id erachter te komen. `lsscsi` kan worden geïnstalleerd door een van beide `yum install lsscsi` (Red Hat-distributies) of `apt-get install lsscsi` (Debian-distributies). U vindt de schijf die u zoekt door de *lun* of **logische eenheidnummer**. Bijvoorbeeld, de *lun* voor de schijven kunnen eenvoudig worden weergegeven uit `azure vm disk list <virtual-machine>` als:

    ```azurecli
    azure vm disk list myVM
    ```

    De uitvoer lijkt op het volgende:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Deze gegevens met de uitvoer van vergelijken `lsscsi` voor hetzelfde voorbeeld van een virtuele machine:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    Het laatste getal in de tuple in elke rij is de *lun*. Zie `man lsscsi` voor meer informatie.
3. Typ bij de prompt de volgende opdracht om uw apparaat te maken:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. Wanneer u hierom wordt gevraagd, typt u **n** te maken van een partitie.

    ![Apparaat maken](./media/attach-disk/fdisknewpartition.png)

5. Wanneer u hierom wordt gevraagd, typt u **p** naar de primaire partitie voor de partitie maken. Type **1** gemakkelijk de eerste partitie en typ vervolgens voor het accepteren van de standaardwaarde voor de 3D-cilinder invoeren. In sommige systemen, kan deze de standaardwaarden van de eerste en de laatste sectoren, in plaats van de 3D-cilinder weergeven. U kunt deze standaardinstellingen accepteren.

    ![Partitie maken](./media/attach-disk/fdisknewpartdetails.png)


6. Type **p** voor de details van de schijf die is worden gepartitioneerd.

    ![Lijst met schijfgegevens](./media/attach-disk/fdiskpartitiondetails.png)


7. Type **w** de instellingen voor de schijf schrijven.

    ![De schijfwijzigingen geschreven](./media/attach-disk/fdiskwritedisk.png)

8. U kunt nu het bestandssysteem op de nieuwe partitie maken. Het partitienummer toevoegen aan de apparaat-ID (in het volgende voorbeeld `/dev/sdc1`). Het volgende voorbeeld wordt een partitie ext4 op /dev/sdc1:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Bestandssysteem maken](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SuSE Linux Enterprise 11 systemen ondersteunen alleen alleen-lezentoegang voor ext4 bestandssystemen. Voor deze systemen is het raadzaam om het nieuwe bestandssysteem als ext3 in plaats van ext4.

9. Moet u een directory voor het koppelen van het nieuwe bestandssysteem als volgt:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Ten slotte kunt u het station, als volgt koppelen:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    De gegevensschijf is nu klaar voor gebruik als **/datadrive**.
   
    ![Maak de map en de schijf koppelen](./media/attach-disk/mkdirandmount.png)

11. Het nieuwe station aan/etc/fstab toevoegen:
   
    Om te controleren of dat het station wordt automatisch gekoppeld na het opnieuw opstarten moet deze worden toegevoegd aan het bestand/etc/fstab. Het is bovendien raadzaam een dat de UUID (Universally Unique IDentifier) om te verwijzen naar de schijf in plaats van alleen de naam van het apparaat (dat wil zeggen /dev/sdc1) wordt gebruikt in/etc/fstab. Met behulp van de UUID voorkomt de onjuiste schijf is gekoppeld aan een bepaalde locatie als het besturingssysteem wordt een fout gedetecteerd tijdens het opstarten en eventuele resterende gegevensschijven vervolgens deze apparaat-id's die worden toegewezen. Als u de UUID van het nieuwe station zoekt, kunt u de **blkid** hulpprogramma:
   
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

    Open vervolgens de **/etc/fstab** bestand in een teksteditor:

    ```bash
    sudo vi /etc/fstab
    ```

    In dit voorbeeld gebruiken we de UUID-waarde voor de nieuwe **/dev/sdc1** apparaat dat is gemaakt in de vorige stappen, en het koppelpunt **/datadrive**. Voeg de volgende regel toe aan het einde van de **/etc/fstab** bestand:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Of op systemen op basis van SuSE Linux moet u wellicht een iets andere notatie:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > De `nofail` optie zorgt ervoor dat de virtuele machine wordt gestart, zelfs als het bestandssysteem beschadigd is of de schijf niet bij het opstarten bestaat. Zonder deze optie kunnen optreden gedrag zoals beschreven in [niet kan SSH naar Linux-VM vanwege FSTAB-fouten](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    U kunt nu testen dat het bestandssysteem is gekoppeld goed door ontkoppelen en vervolgens stationseigendom het bestandssysteem, dat wil zeggen met behulp van het voorbeeld koppelpunt `/datadrive` in de vorige stappen hebt gemaakt:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Als de `mount` opdracht wordt een foutmelding weergegeven, controleert u de/etc/fstab-bestand voor de juiste syntaxis. Als extra schijven of partities zijn gemaakt, / etc/fstab afzonderlijk ook aangaan.

    Het station beschrijfbare maken door met de volgende opdracht:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Vervolgens een gegevensschijf verwijderen zonder te bewerken van fstab kan ervoor zorgen dat de virtuele machine niet kunnen worden opgestart. Als dit vaak het geval is, bieden de meeste distributies ofwel de `nofail` en/of `nobootwait` fstab-opties waarmee u een systeem om op te starten, zelfs als de schijf niet koppelen aan het opstarten. De documentatie van uw distributie voor meer informatie over deze parameters.

### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/UNMAP ondersteuning voor Linux in Azure
Sommige Linux kernels ondersteund TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze bewerkingen zijn vooral nuttig in de standard-opslag om te informeren over Azure die pagina's verwijderd niet langer geldig zijn en kunnen worden verwijderd. 'S te verwijderen kunt kosten besparen als u grote bestanden maken en ze vervolgens te verwijderen.

Er zijn twee manieren om in te schakelen TRIM ondersteuning in uw Linux-VM. Raadpleeg uw distributie zoals gewoonlijk voor de aanbevolen aanpak:

* Gebruik de `discard` koppelen in de optie `/etc/fstab`, bijvoorbeeld:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* In sommige gevallen de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` handmatig vanaf de opdrachtregel de opdracht of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:
  
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
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Volgende stappen
U kunt meer lezen over het gebruik van uw Linux-VM in de volgende artikelen:

* [Hoe u aan te melden met een virtuele machine waarop Linux wordt uitgevoerd][Logon]
* [Hoe u een schijf loskoppelen van een virtuele Linux-machine](detach-disk-classic.md)
* [De Azure CLI gebruiken met het klassieke implementatiemodel](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [RAID op een virtuele Linux-machine in Azure configureren](../configure-raid.md)
* [LVM configureren op een virtuele Linux-machine in Azure](../configure-lvm.md)

<!--Link references-->
[Agent]:../../extensions/agent-linux.md
[Logon]:../mac-create-ssh-keys.md

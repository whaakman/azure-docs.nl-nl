---
title: LVM configureren op een virtuele machine met Linux | Microsoft Docs
description: Informatie over het configureren van LVM op Linux in Azure.
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 7926627aaa3f0da935131f491d927ab5cb4b35c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>LVM configureren op een virtuele Linux-machine in Azure
Dit document wordt uitgelegd hoe het configureren van logische Volume Manager (LVM) in uw virtuele machine van Azure. Het is mogelijk LVM configureren op elke schijf die is gekoppeld aan de virtuele machine, standaard de meeste cloud afbeeldingen geen LVM geconfigureerd op de schijf met het besturingssysteem. Dit is om problemen te voorkomen met groepen dubbele volume als de besturingssysteemschijf is ooit gekoppeld aan een andere virtuele machine van de dezelfde distributie en het type, dat wil zeggen tijdens een scenario voor herstel. Daarom is het aanbevolen alleen voor LVM gebruiken op de gegevensschijven.

## <a name="linear-vs-striped-logical-volumes"></a>Lineaire versus striped logische volumes
LVM kan worden gebruikt om een aantal fysieke schijven samenvoegen tot één opslagvolume. Standaard wordt LVM meestal gemaakt lineaire logische volumes, wat betekent dat de fysieke opslag samengevoegd. In dit geval wordt lees-/ schrijfbewerkingen doorgaans alleen worden verzonden voor één schijf. We kunnen daarentegen ook striped logische volumes waar de lees- en schrijfbewerkingen worden gedistribueerd naar meerdere schijven die zijn opgenomen in de groep volume (dat wil zeggen vergelijkbaar met RAID 0) maken. Voor betere prestaties is het waarschijnlijk wilt u uw logische volumes stripe zodat lees- en schrijfbewerkingen gebruikmaken van alle schijven in de bijgesloten gegevens.

Dit document wordt beschreven hoe u verschillende gegevensschijven combineren in een groep één volume en maak vervolgens logische striped volumes. De onderstaande stappen zijn enigszins gegeneraliseerd om te werken met de meeste distributies. In de meeste gevallen zijn de hulpprogramma's en werkstromen voor het beheren van LVM in Azure niet fundamenteel anders dan andere omgevingen. Gebruikelijke ook Raadpleeg de leverancier van uw Linux voor documentatie en aanbevolen procedures voor het gebruik van LVM met uw bepaalde distributiepunt.

## <a name="attaching-data-disks"></a>Gegevensschijven koppelen
Een wilt meestal beginnen met twee of meer gegevensschijven zijn leeg bij gebruik van LVM. Op basis van uw i/o-behoeften, kunt u schijven die zijn opgeslagen in de Standard-opslag, met maximaal 500 i/o/ps per schijf of onze Premium-opslag met maximaal 5000 i/o/ps per schijf koppelen. In dit artikel gaat niet informatie over het inrichten en gegevensschijven koppelen aan een virtuele Linux-machine. Zie het artikel van Microsoft Azure [een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor gedetailleerde instructies over hoe u een lege gegevensschijf koppelen aan een virtuele Linux-machine in Azure.

## <a name="install-the-lvm-utilities"></a>De hulpprogramma's voor LVM installeren
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS & Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 en openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    Op SLES11 moet u ook bewerken `/etc/sysconfig/lvm` en stel `LVM_ACTIVATED_ON_DISCOVERED` naar 'inschakelen':

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM configureren
In deze handleiding wordt ervan uitgegaan hebt u drie gegevensschijven die we naar als verwijzen gekoppeld `/dev/sdc`, `/dev/sdd` en `/dev/sde`. Houd er rekening mee dat deze altijd mogelijk niet hetzelfde padnamen in uw virtuele machine. U kunt uitvoeren '`sudo fdisk -l`' of een vergelijkbare opdracht om een lijst van uw beschikbare schijven.

1. Bereid de fysieke volumes:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Een volume-groep maken. In dit voorbeeld wordt de groep volume belt `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. De logische volumes maken. De onderstaande opdracht we één logische volume aangeroepen maakt `data-lv01` span van de groep van het gehele volume, maar houd er rekening mee dat het is ook mogelijk meerdere logische om volumes te maken in de groep volume.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Het logische volume formatteren

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Bij gebruik van SLES11 `-t ext3` in plaats van ext4. SLES11 ondersteunt alleen alleen-lezen toegang tot ext4 bestandssystemen.

## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestandssysteem aan /etc/fstab toevoegen
> [!IMPORTANT]
> Onjuist bewerken van de `/etc/fstab` bestand kan leiden tot een systeem opgestart. Als u niet zeker, raadpleeg dan de distributie-documentatie voor informatie over het correct dit bestand te bewerken. Het is ook aanbevolen een back-up van de `/etc/fstab` bestand is gemaakt voordat u kunt bewerken.

1. Maak de gewenste koppelpunt voor het nieuwe bestandssysteem, bijvoorbeeld:

    ```bash  
    sudo mkdir /data
    ```

2. Zoek het logische pad naar het

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Open `/etc/fstab` in een teksteditor en voeg een vermelding voor het nieuwe bestandssysteem, bijvoorbeeld:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Vervolgens opslaan en sluiten `/etc/fstab`.

4. Testen of de `/etc/fstab` invoer correct is:

    ```bash    
    sudo mount -a
    ```

    Als u deze opdracht resulteert in een foutbericht Controleer de syntaxis de `/etc/fstab` bestand.
   
    Voer vervolgens de `mount` opdracht om te controleren of het bestandssysteem is gekoppeld:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (Optioneel) Opstartparameters failsafe in`/etc/fstab`
   
    Groot aantal distributies bevat de `nobootwait` of `nofail` koppelen van de parameters die kunnen worden toegevoegd aan de `/etc/fstab` bestand. Deze parameters toestaan voor fouten bij het koppelen van een bepaald bestandssysteem en dat het Linux-systeem om door te gaan om op te starten, zelfs als deze niet correct koppelen het RAID-bestandssysteem. Raadpleeg de distributie-documentatie voor meer informatie over deze parameters.
   
    Voorbeeld (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>TRIM/UNMAP-ondersteuning
Sommige kernels Linux ondersteuning TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze bewerkingen zijn voornamelijk nuttig in standard-opslag om te informeren over Azure die verwijderde pagina's zijn niet langer geldig en kan worden verwijderd. 'S te verwijderen kunt kosten besparen, als u grote bestanden maken en deze vervolgens te verwijderen.

Er zijn twee manieren om in te schakelen TRIM ondersteunen in uw Linux-VM. Raadpleeg uw distributiepunt gebruikelijke voor de aanbevolen aanpak:

- Gebruik de `discard` koppelen optie in `/etc/fstab`, bijvoorbeeld:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- In sommige gevallen de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` opdracht handmatig vanaf de opdrachtregel of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL/CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```

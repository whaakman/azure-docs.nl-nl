---
title: LVM configureren op een virtuele machine waarop Linux wordt uitgevoerd | Microsoft Docs
description: Informatie over het configureren van LVM op Linux in Azure.
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: jeconnoc
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: szark
ms.subservice: disks
ms.openlocfilehash: 08f98775360b8c0a82f68f322053cb71f0e79af3
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457323"
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>LVM configureren op een virtuele Linux-machine in Azure
Dit document wordt uitgelegd hoe u kunt logische Volume Manager (LVM) configureren in uw Azure virtual machine. LVM kan worden gebruikt op de besturingssysteemschijf en gegevensschijven in Azure VM's, maar standaard de meeste cloud-afbeeldingen niet LVM geconfigureerd op de besturingssysteemschijf. De onderstaande stappen wordt de nadruk gelegd op het LVM configureren voor uw data-schijven.

## <a name="linear-vs-striped-logical-volumes"></a>Lineaire versus striped logische volumes
LVM kan worden gebruikt om een aantal fysieke schijven samenvoegen tot één opslagvolume. Standaard LVM wordt meestal lineaire logische volumes maken, wat betekent dat de fysieke opslag is samengevoegd. In dit geval wordt bewerkingen voor lezen/schrijven doorgaans alleen worden verzonden naar één schijf. Daarentegen, kunnen we ook striped logische volumes waar lees- en schrijfbewerkingen worden gedistribueerd naar meerdere schijven die zijn opgenomen in de volumegroep (vergelijkbaar met RAID 0) maken. Voor betere prestaties is het waarschijnlijk wilt u uw logische volumes stripe zodat lees- en schrijfbewerkingen gebruikmaken van uw gekoppelde gegevensschijven.

Dit document wordt beschreven hoe u verschillende gegevensschijven combineren in een groep één volume en maak vervolgens logische striped volumes. De volgende stappen uit zijn om te werken met de meeste distributies gegeneraliseerd. In de meeste gevallen zijn de hulpprogramma's en werkstromen voor het beheren van LVM in Azure niet fundamenteel anders dan andere omgevingen. Zoals gewoonlijk ook raadpleegt u uw Linux-leverancier voor documentatie en aanbevolen procedures voor het gebruik van LVM met uw specifieke distributie.

## <a name="attaching-data-disks"></a>Gekoppelde gegevensschijven
Een wilt meestal beginnen met twee of meer lege gegevensschijven bij het gebruik van LVM. Op basis van uw i/o-behoeften, kunt u schijven die zijn opgeslagen in de Standard-opslag, met maximaal 500 i/o/ps per schijf of onze Premium-opslag met maximaal 5000 i/o/ps per schijf koppelt. In dit artikel gaat niet informatie over het inrichten en gegevensschijven koppelen aan een virtuele Linux-machine. Zie het artikel van Microsoft Azure [een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor gedetailleerde instructies over hoe u een lege gegevensschijf koppelen aan een virtuele Linux-machine in Azure.

## <a name="install-the-lvm-utilities"></a>De LVM-hulpprogramma's installeren
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

    Op SLES11, moet u ook bewerken `/etc/sysconfig/lvm` en stel `LVM_ACTIVATED_ON_DISCOVERED` ' inschakelen ':

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM configureren
In deze handleiding wordt ervan uitgegaan hebt u drie gegevensschijven, die we naar als verwijzen gekoppeld `/dev/sdc`, `/dev/sdd` en `/dev/sde`. Deze paden mogelijk niet overeenkomt met de namen van de schijf-pad in uw virtuele machine. U kunt uitvoeren '`sudo fdisk -l`' of vergelijkbare-opdracht uit om de beschikbare schijven weer te geven.

1. Het voorbereiden van de fysieke volumes:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Maak een volumegroep. In dit voorbeeld zijn we aanroepen van de volumegroep `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. De logische volumes maken. De onderstaande opdracht we maakt u één logische volume met de naam `data-lv01` omvatten het gehele volume-groep, maar houd er rekening mee dat het is ook mogelijk meerdere logische om volumes te maken in de volumegroep.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. De logische volume formatteren

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Met SLES11 gebruik `-t ext3` in plaats van ext4. SLES11 biedt alleen ondersteuning voor alleen-lezen toegang tot ext4 bestandssystemen.

## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestandssysteem aan/etc/fstab toevoegen
> [!IMPORTANT]
> Onjuist bewerken van de `/etc/fstab` bestand kan leiden tot een systeem opgestart. Als u niet zeker, verwijzen naar de distributie van documentatie voor meer informatie over hoe u dit bestand goed te bewerken. Het is ook raadzaam een back-up van de `/etc/fstab` bestand is gemaakt voordat u bewerkt.

1. Maak de gewenste koppelpunt voor uw nieuwe file system, bijvoorbeeld:

    ```bash  
    sudo mkdir /data
    ```

2. Ga naar het Volumepad van de logische

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

4. Testen of de `/etc/fstab` invoer juist is:

    ```bash    
    sudo mount -a
    ```

    Als deze opdracht in een foutbericht resulteert Controleer de syntaxis van de in de `/etc/fstab` bestand.
   
    Voer vervolgens de `mount` opdracht om te controleren of het bestandssysteem is gekoppeld:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (Optioneel) Failsafe boot parameters in `/etc/fstab`
   
    Groot aantal distributies bevat de `nobootwait` of `nofail` koppelen van de parameters die kunnen worden toegevoegd aan de `/etc/fstab` bestand. Deze parameters toestaan voor fouten bij het koppelen van een bepaalde bestandssysteem en de Linux-systeem om door te gaan om op te starten, zelfs als dit is niet juist de RAID-bestandssysteem te koppelen. Raadpleeg de documentatie van uw distributie voor meer informatie over deze parameters.
   
    Voorbeeld (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>TRIM/UNMAP ondersteuning
Sommige Linux kernels ondersteund TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze bewerkingen zijn vooral nuttig in de standard-opslag om te informeren over Azure die pagina's verwijderd niet langer geldig zijn en kunnen worden verwijderd. 'S te verwijderen kunt kosten besparen als u grote bestanden maken en ze vervolgens te verwijderen.

Er zijn twee manieren om in te schakelen TRIM ondersteuning in uw Linux-VM. Raadpleeg uw distributie zoals gewoonlijk voor de aanbevolen aanpak:

- Gebruik de `discard` koppelen in de optie `/etc/fstab`, bijvoorbeeld:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- In sommige gevallen de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` handmatig vanaf de opdrachtregel de opdracht of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:

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

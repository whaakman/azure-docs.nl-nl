---
title: Configureren van software-RAID op een virtuele machine waarop Linux wordt uitgevoerd | Microsoft Docs
description: Informatie over het gebruik van mdadm RAID op Linux configureren in Azure.
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: jeconnoc
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: f3cb2786-bda6-4d2c-9aaf-2db80f490feb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: 2bced71a7211907e0e517e47c4f5d301d4a56f56
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457408"
---
# <a name="configure-software-raid-on-linux"></a>Software-RAID configureren onder Linux
Het is een veelvoorkomend scenario voor het gebruik van software-RAID op virtuele Linux-machines in Azure meerdere gekoppelde gegevensschijven presenteren als één RAID-apparaat. Dit kan doorgaans worden gebruikt voor betere prestaties en toestaan voor een verbeterde doorvoer in vergelijking met behulp van één schijf.

## <a name="attaching-data-disks"></a>Gekoppelde gegevensschijven
Twee of meer lege gegevensschijven zijn nodig voor het configureren van een RAID-apparaat.  De belangrijkste reden voor het maken van een RAID-apparaat is om de prestaties van de schijf-i/o te verbeteren.  Op basis van uw i/o-behoeften, kunt u schijven die zijn opgeslagen in de Standard-opslag, met maximaal 500 i/o/ps per schijf of onze Premium-opslag met maximaal 5000 i/o/ps per schijf koppelt. In dit artikel gaat niet informatie over het inrichten en gegevensschijven koppelen aan een virtuele Linux-machine.  Zie het artikel van Microsoft Azure [een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor gedetailleerde instructies over hoe u een lege gegevensschijf koppelen aan een virtuele Linux-machine in Azure.

## <a name="install-the-mdadm-utility"></a>Installeer het hulpprogramma mdadm
* **Ubuntu**
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* **CentOS & Oracle Linux**
```bash
sudo yum install mdadm
```

* **SLES en openSUSE**
```bash  
zypper install mdadm
```

## <a name="create-the-disk-partitions"></a>De partities op schijf maken
In dit voorbeeld maken we een partitie van één schijf op /dev/sdc. De partitie van de nieuwe schijf wordt /dev/sdc1 worden aangeroepen.

1. Start `fdisk` om te beginnen met het maken van partities

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off the mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

1. Druk op n bij de prompt te maken van een **n**ieuw partitie:

    ```bash
    Command (m for help): n
    ```

1. Vervolgens drukt u op 'p' maken een **p**rimaire partitie:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

1. Druk op '1' partitienummer 1 selecteren:

    ```bash
    Partition number (1-4): 1
    ```

1. Selecteer het startpunt van de nieuwe partitie of druk op `<enter>` de standaardoptie om de partitie aan het begin van de vrije ruimte op het station:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

1. Selecteer de grootte van de partitie, typt u bijvoorbeeld '+10G' voor het maken van een 10 GB-partitie. Of druk op `<enter>` maken van een enkele partitie die het hele station omvat:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

1. Wijzig vervolgens de ID en **t**ype van de partitie van de standaard-ID '83' (Linux) naar ID 'fd' (Linux raid automatisch):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

1. Ten slotte de partitietabel schrijven naar het station en sluit fdisk:

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a>De RAID-matrix maken
1. Het volgende voorbeeld wordt 'stripe' (RAID-niveau 0) drie partities zich op drie afzonderlijke schijven (sdc1, sdd1, sde1).  Nadat een nieuw RAID-apparaat voor deze opdracht uitvoert met de naam **/dev/md127** wordt gemaakt. Let ook op dat als deze gegevensschijven we eerder deel uit van een andere uitgeschakelde RAID-matrix het nodig zijn kan om toe te voegen de `--force` parameter voor de `mdadm` opdracht:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

1. Maken van het bestandssysteem op het nieuwe RAID-apparaat
   
    a. **CentOS, Oracle Linux, SLES 12, openSUSE en Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    b. **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    c. **SLES 11** : Schakel boot.md en mdadm.conf maken

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > Opnieuw opstarten nadat u deze wijzigingen aanbrengt op SUSE systemen mogelijk vereist zijn. Deze stap is *niet* vereist op SLES 12.
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestandssysteem aan/etc/fstab toevoegen
> [!IMPORTANT]
> Onjuist bewerken van het bestand/etc/fstab kan leiden tot een systeem opgestart. Als u niet zeker, verwijzen naar de distributie van documentatie voor meer informatie over hoe u dit bestand goed te bewerken. Het verdient ook dat er een back-up van het bestand/etc/fstab voordat u bewerkt wordt gemaakt.

1. Maak de gewenste koppelpunt voor uw nieuwe file system, bijvoorbeeld:

    ```bash
    sudo mkdir /data
    ```
1. Wanneer u bewerkt/etc/fstab, de **UUID** moet worden gebruikt om te verwijzen naar het bestandssysteem in plaats van de naam van het apparaat.  Gebruik de `blkid` hulpprogramma voor het bepalen van de UUID voor het nieuwe bestandssysteem:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

1. / Etc/fstab openen in een teksteditor en voeg een vermelding voor het nieuwe bestandssysteem, bijvoorbeeld:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Of op **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Vervolgens opslaan en sluiten/etc/fstab.

1. Test of de vermelding/etc/fstab juist is:

    ```bash  
    sudo mount -a
    ```

    Als deze opdracht in een foutbericht weergegeven resulteert, controleert u de syntaxis van de in het bestand/etc/fstab.
   
    Voer vervolgens de `mount` opdracht om te controleren of het bestandssysteem is gekoppeld:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

1. (Optioneel) Failsafe Boot Parameters
   
    **configuratie van fstab**
   
    Groot aantal distributies bevat de `nobootwait` of `nofail` koppelen van de parameters die kunnen worden toegevoegd aan het bestand/etc/fstab. Deze parameters toestaan voor fouten bij het koppelen van een bepaalde bestandssysteem en de Linux-systeem om door te gaan om op te starten, zelfs als dit is niet juist de RAID-bestandssysteem te koppelen. Raadpleeg de documentatie van uw distributie voor meer informatie over deze parameters.
   
    Voorbeeld (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Opstartparameters van Linux**
   
    Naast de bovenstaande parameters, de kernel-parameter '`bootdegraded=true`"kunt toestaan dat het systeem om op te starten, zelfs als de RAID wordt beschouwd als beschadigd of gedegradeerd, voor voorbeeld als een gegevensstation per ongeluk wordt verwijderd uit de virtuele machine. Standaard kan dit ook resulteren in een niet-opstartbare systeem.
   
    Raadpleeg de documentatie van uw distributie over het kernel-parameters juist te bewerken. Bijvoorbeeld in een groot aantal distributies (CentOS, Oracle Linux, SLES-11) deze parameters kunnen worden toegevoegd handmatig aan de '`/boot/grub/menu.lst`' bestand.  Op Ubuntu deze parameter kan worden toegevoegd aan de `GRUB_CMDLINE_LINUX_DEFAULT` variabele op '/ standaard/etc/wormgaten'.


## <a name="trimunmap-support"></a>TRIM/UNMAP ondersteuning
Sommige Linux kernels ondersteund TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze bewerkingen zijn vooral nuttig in de standard-opslag om te informeren over Azure die pagina's verwijderd niet langer geldig zijn en kunnen worden verwijderd. 'S te verwijderen kunt kosten besparen als u grote bestanden maken en ze vervolgens te verwijderen.

> [!NOTE]
> RAID kan niet verwijderen-opdrachten uitgeven als de chunkgrootte voor de matrix is ingesteld op lager is dan de standaardwaarde (512KB). Dit is omdat de granulatie unmap op de Host ook 512KB is. Als u de chunkgrootte van de matrix via de mdadm gewijzigd `--chunk=` parameter en klik vervolgens TRIM/ontkoppelen aanvragen kunnen worden genegeerd door de kernel.

Er zijn twee manieren om in te schakelen TRIM ondersteuning in uw Linux-VM. Raadpleeg uw distributie zoals gewoonlijk voor de aanbevolen aanpak:

- Gebruik de `discard` koppelen in de optie `/etc/fstab`, bijvoorbeeld:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- In sommige gevallen de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` handmatig vanaf de opdrachtregel de opdracht of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:

    **Ubuntu**

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    **RHEL/CentOS**
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```

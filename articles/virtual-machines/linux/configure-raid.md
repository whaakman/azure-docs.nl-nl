---
title: Configureren van software-RAID op een virtuele machine met Linux | Microsoft Docs
description: Informatie over het gebruik van mdadm RAID op Linux configureren in Azure.
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: timlt
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
ms.openlocfilehash: 12f540a700fbf85e579e8aadc9f6def039299ff7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-software-raid-on-linux"></a>Software-RAID configureren onder Linux
Het is een veelvoorkomend scenario gebruik van software-RAID op Linux virtuele machines in Azure aan meerdere gegevensschijven van de gekoppelde worden weergegeven als een enkel RAID-apparaat. Dit kan doorgaans worden gebruikt voor betere prestaties en toestaan voor een verbeterde doorvoer vergeleken met het gebruik van één schijf.

## <a name="attaching-data-disks"></a>Gegevensschijven koppelen
Twee of meer leeg gegevensschijven zijn nodig om een RAID-apparaat te configureren.  De belangrijkste reden voor het maken van een RAID-apparaat is voor het verbeteren van de prestaties van de schijf-i/o.  Op basis van uw i/o-behoeften, kunt u schijven die zijn opgeslagen in de Standard-opslag, met maximaal 500 i/o/ps per schijf of onze Premium-opslag met maximaal 5000 i/o/ps per schijf koppelen. In dit artikel gaat niet informatie over het inrichten en gegevensschijven koppelen aan een virtuele Linux-machine.  Zie het artikel van Microsoft Azure [een schijf koppelen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor gedetailleerde instructies over hoe u een lege gegevensschijf koppelen aan een virtuele Linux-machine in Azure.

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
In dit voorbeeld maken we een enkele schijfpartitie op /dev/sdc. De partitie van de nieuwe schijf wordt /dev/sdc1 worden aangeroepen.

1. Start `fdisk` om te beginnen met partities maken

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

2. Tik op "n" bij de prompt maken een  **n** euwe partitie:

    ```bash
    Command (m for help): n
    ```

3. Druk op 'p' maken een **p**rimaire partitie:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. Druk op '1' partitienummer 1 selecteren:

    ```bash
    Partition number (1-4): 1
    ```

5. Selecteer het beginpunt van de nieuwe partitie of druk op `<enter>` de standaardoptie voor het plaatsen van de partitie aan het begin van de vrije ruimte op de schijf:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. Selecteer de grootte van de partitie, typt u bijvoorbeeld '+10G' voor het maken van een 10 GB-partitie. Of druk op `<enter>` één partitie die het hele station omvat maken:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. Wijzig vervolgens de ID en **t**ype van de partitie van de standaard-ID '83' (Linux)-ID 'fd' (Linux raid automatisch):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

8. Ten slotte de partitietabel schrijven naar het station en fdisk af te sluiten:

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a>De RAID-matrix maken
1. Het volgende voorbeeld wordt 'stripe' (RAID-niveau 0) drie partities zich op drie aparte gegevensschijven (sdc1, sdd1, sde1).  Nadat u deze opdracht uitvoert op een nieuw RAID-apparaat aangeroepen **/dev/md127** wordt gemaakt. Let ook op dat als deze gegevens schijven we eerder onderdeel van een andere uitgeschakelde RAID-matrix het mogelijk nodig om toe te voegen de `--force` -parameter voor de `mdadm` opdracht:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. Maken van het bestandssysteem op het nieuwe RAID-apparaat
   
    a. **CentOS, Oracle Linux SLES 12, openSUSE en Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    b. **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    c. **SLES 11** - boot.md inschakelen en mdadm.conf maken

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > Opnieuw opstarten nadat u deze wijzigingen aanbrengt op SUSE systemen mogelijk vereist zijn. Deze stap is *niet* vereist voor SLES 12.
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a>Het nieuwe bestandssysteem aan /etc/fstab toevoegen
> [!IMPORTANT]
> Onjuist bewerken van het bestand /etc/fstab kan leiden tot een systeem opgestart. Als u niet zeker, Raadpleeg de distributie-documentatie voor informatie over het correct dit bestand te bewerken. Het is ook raadzaam dat een back-up van het bestand /etc/fstab is gemaakt voordat u bewerkt.

1. Maak de gewenste koppelpunt voor het nieuwe bestandssysteem, bijvoorbeeld:

    ```bash
    sudo mkdir /data
    ```
2. Bij het bewerken van /etc/fstab, de **UUID** moet worden gebruikt om te verwijzen naar het bestandssysteem in plaats van op naam van het apparaat.  Gebruik de `blkid` hulpprogramma voor het bepalen van de UUID voor het nieuwe bestandssysteem:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. Open /etc/fstab in een teksteditor en voeg een vermelding voor het nieuwe bestandssysteem, bijvoorbeeld:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Of op **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Vervolgens opslaan en sluiten /etc/fstab.

4. Test of de vermelding/etc/fstab juist is:

    ```bash  
    sudo mount -a
    ```

    Als deze opdracht in een foutbericht weergegeven resulteert, controleert u de syntaxis in het bestand /etc/fstab.
   
    Voer vervolgens de `mount` opdracht om te controleren of het bestandssysteem is gekoppeld:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. (Optioneel) Opstartparameters failsafe
   
    **fstab-configuratie**
   
    Groot aantal distributies bevat de `nobootwait` of `nofail` parameters die kunnen worden toegevoegd aan het bestand/etc/fstab koppelen. Deze parameters toestaan voor fouten bij het koppelen van een bepaald bestandssysteem en dat het Linux-systeem om door te gaan om op te starten, zelfs als deze niet correct koppelen het RAID-bestandssysteem. Raadpleeg de distributie-documentatie voor meer informatie over deze parameters.
   
    Voorbeeld (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Linux-opstartparameters**
   
    Naast de bovenstaande parameters, de kernel-parameter '`bootdegraded=true`' toestaan dat het systeem om op te starten, zelfs als de RAID wordt beschouwd als beschadigd of verslechterde voor voorbeeld als een gegevensstation per ongeluk is verwijderd van de virtuele machine. Standaard kan dit ook leiden tot een niet-opstartbare systeem.
   
    Raadpleeg de documentatie van uw distributiepunt voor het kernel-parameters juist te bewerken. Bijvoorbeeld in een groot aantal distributies (CentOS, Oracle Linux, SLES 11) deze parameters kunnen worden toegevoegd handmatig de '`/boot/grub/menu.lst`' bestand.  Deze parameter op Ubuntu kan worden toegevoegd aan de `GRUB_CMDLINE_LINUX_DEFAULT` variabele op '/ etc/standaard/grub'.


## <a name="trimunmap-support"></a>TRIM/UNMAP-ondersteuning
Sommige kernels Linux ondersteuning TRIM/UNMAP bewerkingen voor het negeren van niet-gebruikte blokken op de schijf. Deze bewerkingen zijn voornamelijk nuttig in standard-opslag om te informeren over Azure die verwijderde pagina's zijn niet langer geldig en kan worden verwijderd. 'S te verwijderen kunt kosten besparen, als u grote bestanden maken en deze vervolgens te verwijderen.

> [!NOTE]
> RAID kan geen verwijderde opdrachten uitgeven als de chunkgrootte voor de matrix is ingesteld op lager is dan de standaardwaarde (512KB). Dit is omdat de granulatie unmap op de Host ook 512KB is. Als u de chunkgrootte van de matrix via de mdadm gewijzigd `--chunk=` parameter en klik vervolgens TRIM/ontkoppelen aanvragen kunnen worden genegeerd door de kernel.

Er zijn twee manieren om in te schakelen TRIM ondersteunen in uw Linux-VM. Raadpleeg uw distributiepunt gebruikelijke voor de aanbevolen aanpak:

- Gebruik de `discard` koppelen optie in `/etc/fstab`, bijvoorbeeld:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- In sommige gevallen de `discard` optie prestaties gevolgen kan hebben. U kunt ook uitvoeren de `fstrim` opdracht handmatig vanaf de opdrachtregel of toe te voegen aan uw crontab regelmatig wordt uitgevoerd:

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

---
title: Een Linux-VHD in Azure maken en uploaden
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: szark
ms.openlocfilehash: e032f9a9772232d3a57a9672dc6c601354ecad43
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58105519"
---
# <a name="information-for-non-endorsed-distributions"></a>Informatie over niet-goedgekeurde distributies
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Het Azure-platform SLA van toepassing op virtuele machines met het Linux-besturingssysteem alleen wanneer een van de [distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) wordt gebruikt. Voor deze onderschreven distributies vindt u vooraf geconfigureerde Linux-installatiekopieën in de Azure Marketplace.

* [Linux op Azure - distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ondersteuning voor Linux-installatiekopieën in Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alle distributies die worden uitgevoerd op Azure hebt een aantal vereisten. In dit artikel kan niet worden uitgebreid, als elke distributie verschilt. Zelfs als u de onderstaande criteria voldoen, moet u mogelijk aanzienlijk aanpassen uw Linux-systeem voor het correct kunnen worden uitgevoerd.

Het is raadzaam dat u met een van begint de [Linux op Azure aanbevolen distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De volgende artikelen laten zien hoe u de verschillende Distributies voor het maken van onderschreven Linux die worden ondersteund op Azure voorbereiden:

* **[Op basis van centOS-distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

In dit artikel richt zich op de algemene richtlijnen voor het uitvoeren van de Linux-distributie op Azure.

## <a name="general-linux-installation-notes"></a>Opmerkingen bij de installatie van de algemene Linux
* De indeling van Hyper-V virtuele harde schijf (VHDX) wordt niet ondersteund in Azure, alleen *vaste VHD*.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de [Convert-VHD](https://docs.microsoft.com/powershell/module/hyper-v/convert-vhd) cmdlet. Als u van VirtualBox gebruikmaakt, selecteert u **vaste grootte** in plaats van de standaardwaarde (dynamisch toegewezen) bij het maken van de schijf.
* Azure biedt alleen ondersteuning voor virtuele machines van generatie 1. U kunt een virtuele machine generatie 1 converteren van VHDX, zodat de VHD-indeling en van het dynamisch uitbreiden naar een vaste grootte schijf. U kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie, [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* De maximale grootte van de VHD is 1023 GB.
* Bij het installeren van de Linux-systeem wordt u aangeraden dat u gebruikt standaard partities in plaats van logische Volume Manager (LVM) Dit is de standaardinstelling voor vele installaties. Met behulp van standaard partities voorkomen LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf is ooit gekoppeld aan een andere identieke virtuele machine voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt.
* Kernel-ondersteuning voor het koppelen van bestandssystemen UDF is nodig. De configuratie van de inrichting wordt op de eerste keer opstarten op Azure met behulp van media UDF-indeling die is gekoppeld aan de Gast doorgegeven aan de Linux-VM. De Azure Linux-agent moet de UDF-bestandssysteem om te lezen van de configuratie en het inrichten van de virtuele machine koppelen.
* Linux-kernel-versies ondersteunen ouder dan 2.6.37 geen NUMA op Hyper-V met grotere VM-grootten. Dit probleem voornamelijk gevolgen oudere distributies met behulp van de upstream Red Hat 2.6.32 kernel, en Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504) kon worden opgelost. Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of op basis van RHEL kernels die ouder zijn dan 2.6.32-504 moet de parameter opstarten `numa=off` op de opdrachtregel van de kernel in grub.conf. Zie voor meer informatie, [Red Hat KB 436883](https://access.redhat.com/solutions/436883).
* Configureer een swap-partitie niet op de besturingssysteemschijf. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf, zoals beschreven in de volgende stappen uit.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1 MB. Bij het converteren van een onbewerkte schijf naar VHD u moet ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1 MB vóór de conversie is, zoals beschreven in de volgende stappen uit.

### <a name="installing-kernel-modules-without-hyper-v"></a>Kernelmodules zonder Hyper-V installeren
Azure wordt uitgevoerd op de Hyper-V-hypervisor, zodat Linux is vereist voor bepaalde kernelmodules mogen worden uitgevoerd in Azure. Hebt u een virtuele machine die buiten Hyper-V is gemaakt, het Linux-installatieprogramma's bevatten mogelijk niet de stuurprogramma's voor Hyper-V in de eerste ramdisk (initrd of initramfs), tenzij de virtuele machine vaststelt dat deze wordt uitgevoerd op een Hyper-V-omgeving. Wanneer u verschillende virtualisatiesysteem (zoals Virtualbox, KVM, enzovoort) gebruikt voor het voorbereiden van uw Linux-installatiekopie, moet u mogelijk opnieuw opbouwen van de initrd dus op minimaal de hv_vmbus en hv_storvsc kernel-modules zijn beschikbaar op de eerste ramdisk.  Deze bekend probleem is bedoeld voor systemen gebaseerd op de upstream Red Hat-distributie, en mogelijk andere.

Het mechanisme voor het opnieuw opbouwen van de initrd of initramfs afbeelding kan variëren, afhankelijk van de distributie. Raadpleeg de documentatie of de ondersteuning van uw distributie voor de juiste procedure.  Hier volgt een voorbeeld voor het opnieuw opbouwen van de initrd met behulp van de `mkinitrd` hulpprogramma:

1. Back-up van de bestaande initrd-installatiekopie:

    ```
    cd /boot
    sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak
    ```

2. Opnieuw met het maken van de initrd met de kernel hv_vmbus en hv_storvsc modules:

    ```
    sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`
    ```

### <a name="resizing-vhds"></a>Wijzigen van de grootte VHD 's
VHD-installatiekopieën op Azure beschikken over een virtuele grootte die is afgestemd op 1 MB.  Normaal gesproken zijn VHD's die zijn gemaakt met behulp van Hyper-V correct uitgelijnd.  Als de VHD is niet correct uitgelijnd, ontvangt u mogelijk een vergelijkbaar met het volgende foutbericht weergegeven wanneer u probeert te maken van een installatiekopie van uw VHD.

* De VHD- http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd heeft een niet-ondersteunde virtuele grootte van 21475270656 bytes. De grootte moet een geheel getal (in MB/s).

In dit geval vergroten of verkleinen van de virtuele machine met behulp van de Hyper-V Manager-console of de [grootte VHD](https://technet.microsoft.com/library/hh848535.aspx) PowerShell-cmdlet.  Als u niet worden uitgevoerd in een Windows-omgeving, wordt u aangeraden `qemu-img` te converteren (indien nodig) en het formaat van de VHD.

> [!NOTE]
> Er is een [bekend probleem in qemu img](https://bugs.launchpad.net/qemu/+bug/1490611) versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Wordt u aangeraden hetzij `qemu-img` 2.2.0 of minder of 2.6 of hoger.
> 

1. Formaat van de VHD die rechtstreeks met behulp van hulpprogramma's zoals `qemu-img` of `vbox-manage` kan leiden tot een opgestart VHD.  Het is raadzaam om eerst de VHD te converteren naar een installatiekopie van de ONBEWERKTE schijf.  Als de VM-installatiekopie is gemaakt als de installatiekopie van een ONBEWERKTE schijf (de standaardinstelling voor bepaalde hypervisors zoals KVM), kunt u deze stap overslaan.
 
    ```
    qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw
    ```

1. De vereiste grootte van de installatiekopie van de schijf berekenen zodat de grootte van de virtuele 1 MB is uitgelijnd.  Het volgende bash-shell-script gebruikt `qemu-img info` om te bepalen van de virtuele grootte van de installatiekopie van de schijf en berekent vervolgens de grootte van de volgende 1 MB.

    ```bash
    rawdisk="MyLinuxVM.raw"
    vhddisk="MyLinuxVM.vhd"

    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "$rawdisk" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    rounded_size=$((($size/$MB + 1)*$MB))
    
    echo "Rounded Size = $rounded_size"
    ```

3. Grootte van de onbewerkte schijf met `$rounded_size` als hierboven.

    ```bash
    qemu-img resize MyLinuxVM.raw $rounded_size
    ```

4. Nu de ONBEWERKTE schijf converteren naar een VHD met vaste grootte.

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

   Of qemu versie 2.6 +, bevatten de `force_size` optie.

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

## <a name="linux-kernel-requirements"></a>Linux-Kernel-vereisten

De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure zijn bijgedragen rechtstreeks naar de upstream-Linux-kernel. Groot aantal distributies die een recente versie van de kernel Linux (zoals 3.x bevatten) al deze stuurprogramma's die beschikbaar zijn, of Geef anders backported versies van deze stuurprogramma's met hun kernels.  Deze stuurprogramma's worden voortdurend wordt bijgewerkt in de upstream-kernel met nieuwe oplossingen en functies, waar mogelijk raden wij aan die wordt uitgevoerd een [distributie die zijn goedgekeurd](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die deze oplossingen en updates bevat.

Als u een variant van Red Hat Enterprise Linux-versies 6.0-6.3 uitvoert, dan u moet voor het installeren van de [meest recente LIS-stuurprogramma's voor Hyper-V](https://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Beginnen met RHEL 6.4 + (en afleidingen van) de LIS-stuurprogramma's zijn al opgenomen in de kernel en dus geen aanvullende installatie-pakketten zijn vereist.

Als u een aangepaste kernel is vereist, raden wij een recente kernelversie (zoals 3.8 +). Voor distributies of leveranciers die hun eigen kernel onderhouden, moet u regelmatig backport de LIS-stuurprogramma's van de upstream-kernel voor uw aangepaste kernel.  Zelfs als u al een relatief recente kernelversie uitvoert, raden wij het bijhouden van een upstream-correcties in de LIS-stuurprogramma's en backport ze indien nodig. De locaties van de bronbestanden van de LIS-stuurprogramma zijn opgegeven in de [instaan](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) bestand in de structuur van Linux-kernel bron:
```
    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/
```
De volgende patches moeten worden opgenomen in de kernel. Deze lijst kan niet worden voltooid voor alle distributies.

* [ata_piix: schijven aan de Hyper-V-stuurprogramma's standaard uitstellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: Account voor de pakketten in transit in het pad opnieuw instellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: gebruik van WRITE_SAME voorkomen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: DEZELFDE schrijven uitschakelen voor RAID en stuurprogramma's voor virtuele host-netwerkadapters](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL-aanwijzer fix voor referentie ongedaan maken](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: ring buffer storingen kunnen leiden tot i/o-bevriezing](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: bescherming tegen een dubbele uitvoering van __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>De Azure Linux Agent
De [Azure Linux Agent](../extensions/agent-linux.md) `waagent` richt een virtuele Linux-machine in Azure. U kunt de nieuwste versie, de problemen met bestanden ophalen of indienen van pull-aanvragen op de [Linux Agent GitHub-opslagplaats](https://github.com/Azure/WALinuxAgent).

* De Linux-agent wordt de licentie Apache 2.0 uitgebracht. Groot aantal distributies al bieden RPM- of deb pakketten voor de agent, en deze pakketten kunnen gemakkelijk worden geïnstalleerd en bijgewerkt.
* De Azure Linux Agent vereist Python v2.6 +.
* De agent is ook vereist voor de python pyasn1-module. De meeste distributies bieden deze module als een apart pakket moet worden geïnstalleerd.
* In sommige gevallen kan de Azure Linux Agent mogelijk niet compatibel is met NetworkManager. Veel van de RPM/Deb-pakketten dat is geleverd door distributies configureren NetworkManager als een conflict voor het pakket waagent. In dergelijke gevallen wordt het NetworkManager verwijderd wanneer u de Linux-agent-pakket installeert.
* De Azure Linux Agent moet worden bij of boven de [minimaal ondersteunde versie](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Algemene Linux-systeemvereisten

1. Wijzig de kernel boot line in WORMGATEN of GRUB2 om op te nemen van de volgende parameters, zodat alle consoleberichten worden verzonden naar de eerste seriële poort. Deze berichten Azure kunnen helpen ondersteuning bij foutopsporing in eventuele problemen.
    ```  
    console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
    ```
    We raden ook aan *verwijderen* de volgende parameters als deze bestaan.
    ```  
    rhgb quiet crashkernel=auto
    ```
    Grafische en stil opstarten is niet nuttig in een cloudomgeving, zoals we willen dat alle logboeken die worden verzonden naar de seriële poort. De `crashkernel` mogelijk links die zijn geconfigureerd, indien nodig, maar houd er rekening mee dat deze parameter wordt de hoeveelheid beschikbaar geheugen in de virtuele machine met ten minste 128 MB, die mogelijk geschikt voor kleinere VM-grootten.

1. Installeer de Azure Linux Agent.
  
    De Azure Linux Agent is vereist voor het inrichten van een installatiekopie van Linux op Azure.  Groot aantal distributies bieden de agent als een RPM- of Deb-pakket (het pakket wordt doorgaans genoemd WALinuxAgent of walinuxagent).  De agent kan ook handmatig geïnstalleerd met de volgende stappen in de [Linux Agent handleiding](../extensions/agent-linux.md).

1. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Deze configuratie is doorgaans de standaardinstelling.

1. Maak geen wisselruimte op de besturingssysteemschijf.
  
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. De lokale bronschijf wordt een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (stap 2 hierboven), de volgende parameters in /etc/waagent.conf zo nodig wijzigen.
    ```  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: Set this to your desired size.
    ```
1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken.
  
     ```
     sudo waagent -force -deprovision
     export HISTSIZE=0
     logout
     ```  
   > [!NOTE]
   > Op Virtualbox mag u de volgende fout ziet nadat het is uitgevoerd `waagent -force -deprovision` met de tekst `[Errno 5] Input/output error`. Dit foutbericht is niet kritiek en kan worden genegeerd.

* Sluit de virtuele machine en de VHD uploaden naar Azure.


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
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 17b4df83b141d5365a8d6244c4ab73b0eba5ed73
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972346"
---
# <a name="information-for-non-endorsed-distributions"></a>Informatie voor niet-aanbevolen distributies
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Het Azure-platform SLA van toepassing op virtuele machines met het Linux-besturingssysteem alleen wanneer een van de [distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) wordt gebruikt. Voor deze onderschreven distributies vindt u Linux-installatiekopieën in de Azure Marketplace en de vereiste configuratie.

* [Linux op Azure - distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ondersteuning voor Linux-installatiekopieën in Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alle distributies die worden uitgevoerd op Azure nodig om te voldoen aan een aantal vereisten hebben een kans om correct wordt uitgevoerd op het platform.  In dit artikel is geen uitgebreide als elke distributie anders is, en het is mogelijk, zelfs als u voldoet aan de onderstaande criteria u uw Linux-systeem om ervoor te zorgen moet dat deze correct wordt uitgevoerd op het platform aanzienlijk aanpassen.

Het is om deze reden dat het is raadzaam dat u met begint een [Linux op Azure aanbevolen distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) indien mogelijk. Het volgende artikel begeleidt u bij het voorbereiden van de verschillende Distributies voor het maken van onderschreven Linux die worden ondersteund op Azure:

* **[Op basis van centOS-distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

De rest van dit artikel richt zich op de algemene richtlijnen voor het uitvoeren van de Linux-distributie op Azure.

## <a name="general-linux-installation-notes"></a>Opmerkingen bij de installatie van de algemene Linux
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd. Als u met behulp van VirtualBox betekent dit dat selecteren **vaste grootte** in plaats van de standaard dynamisch toegewezen bij het maken van de schijf.
* Azure biedt alleen ondersteuning voor virtuele machines van generatie 1. U kunt een virtuele machine generatie 1 converteren van VHDX, zodat de VHD-indeling en dynamisch uitbreiden naar een vaste grootte schijf. Maar u kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie, [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* De maximale grootte van de VHD is 1023 GB.
* Bij het installeren van de Linux-systeem is *aanbevolen* dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit zal LVM naam conflicteert met de gekloonde virtuele machines, voorkomen dat met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere identieke virtuele machine moet voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt.
* Kernel-ondersteuning voor het koppelen van bestandssystemen UDF is vereist. De configuratie van de inrichting wordt op de eerste keer opstarten op Azure via media UDF-indeling die is gekoppeld aan de Gast doorgegeven aan de Linux-VM. De Azure Linux-agent moet u mogelijk de UDF-bestandssysteem installeren om te lezen van de configuratie en de virtuele machine inricht.
* Linux-kernel-versies hieronder 2.6.37 ondersteunen geen NUMA op Hyper-V met grotere VM-grootten. Dit probleem voornamelijk gevolgen oudere distributies met behulp van de upstream Red Hat 2.6.32 kernel, en is vastgesteld in RHEL 6.6 (kernel-2.6.32-504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of op basis van RHEL kernels die ouder zijn dan 2.6.32-504 moet de parameter opstarten `numa=off` op de opdrachtregel in grub.conf kernel. Zie voor meer informatie Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt in de onderstaande stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Meer informatie kunt vinden in de onderstaande stappen.

### <a name="installing-kernel-modules-without-hyper-v"></a>Kernelmodules zonder Hyper-V installeren
Azure wordt uitgevoerd op de Hyper-V-hypervisor, zodat Linux is vereist dat bepaalde kernelmodules zijn geïnstalleerd om uit te voeren in Azure. Hebt u een virtuele machine die buiten Hyper-V is gemaakt, het Linux-installatieprogramma's bevatten mogelijk niet de stuurprogramma's voor Hyper-V in de eerste ramdisk (initrd of initramfs), tenzij er wordt gedetecteerd dat deze wordt uitgevoerd op een Hyper-V-omgeving. Wanneer u een andere virtualisatiesysteem (dat wil zeggen, Virtualbox, KVM, enzovoort) om voor te bereiden uw Linux-installatiekopie, moet u mogelijk opnieuw opbouwen van de initrd om ervoor te zorgen dat ten minste de `hv_vmbus` en `hv_storvsc` kernelmodules zijn beschikbaar op de eerste ramdisk.  Dit is een bekend probleem ten minste op systemen op basis van de upstream Red Hat-distributie.

Het mechanisme voor het opnieuw opbouwen van de initrd of initramfs afbeelding kan variëren, afhankelijk van de distributie. Raadpleeg de documentatie of de ondersteuning van uw distributie voor de juiste procedure.  Hier volgt een voorbeeld voor het opnieuw opbouwen van de initrd met behulp van de `mkinitrd` hulpprogramma:

Eerste back-up van de bestaande initrd-installatiekopie:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Vervolgens opnieuw de initrd met de `hv_vmbus` en `hv_storvsc` kernelmodules:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Wijzigen van de grootte VHD 's
VHD-installatiekopieën op Azure beschikken over een virtuele grootte die is afgestemd op 1 MB.  VHD's die zijn gemaakt met behulp van Hyper-V moeten normaal gesproken al goed zijn uitgelijnd.  Als de VHD is niet correct uitgelijnd, mogelijk ontvangt u een vergelijkbaar met het volgende foutbericht weergegeven wanneer u probeert te maken van een *installatiekopie* van uw VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

U lost dit probleem, vergroten of verkleinen van de virtuele machine met behulp van de Hyper-V Manager-console of de [grootte VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-cmdlet.  Als u niet wordt uitgevoerd in een Windows-omgeving, wordt aanbevolen qemu img gebruiken om te converteren van (indien nodig) en grootte van de VHD.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Formaat van de VHD die rechtstreeks met behulp van hulpprogramma's zoals `qemu-img` of `vbox-manage` kan leiden tot een opgestart VHD.  Daarom wordt het aanbevolen eerst converteren de VHD met een schijfinstallatiekopie van ONBEWERKTE.  Als de VM-installatiekopie al als ONBEWERKTE schijfimage (de standaardinstelling voor bepaalde Hypervisors zoals KVM gemaakt is) kunt u deze stap overslaan:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Berekent de vereiste grootte van de installatiekopie van de schijf om ervoor te zorgen dat de grootte van de virtuele 1 MB is uitgelijnd.  Het volgende bash-shell-script kan u helpen bij dit.  Het script maakt gebruik van '`qemu-img info`' om te bepalen van de virtuele grootte van de installatiekopie van de schijf en berekent vervolgens de grootte van de volgende 1 MB:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Grootte van de onbewerkte schijf met behulp van $rounded_size zoals ingesteld in het bovenstaande script wijzigen:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Nu de ONBEWERKTE schijf converteren naar een VHD met vaste grootte:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Of, voor qemu versie **2.6 +** bevatten de `force_size` optie:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Linux-Kernel-vereisten
De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure zijn bijgedragen rechtstreeks naar de upstream-Linux-kernel. Groot aantal distributies die een recente versie van Linux-kernel (dat wil zeggen 3.x bevatten) al deze stuurprogramma's die beschikbaar zijn, of Geef anders backported versies van deze stuurprogramma's met hun kernels.  Deze stuurprogramma's worden voortdurend wordt bijgewerkt in de upstream-kernel met nieuwe oplossingen en functies, zodat indien mogelijk het wordt aanbevolen om uit te voeren een [distributie die zijn goedgekeurd](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die deze oplossingen en updates bevat.

Als u een variant van Red Hat Enterprise Linux-versies worden uitgevoerd **6.0 6.3**, moet u de meest recente LIS-stuurprogramma's installeren voor Hyper-V. De stuurprogramma's kunnen worden gevonden [op deze locatie](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Vanaf RHEL **6.4 +** (en afleidingen van) de LIS-stuurprogramma's zijn al opgenomen in de kernel en dus geen aanvullende installatie-pakketten zijn vereist om uit te voeren van deze systemen in Azure.

Als een aangepaste kernel vereist is, is het raadzaam om te gebruiken een recentere kernelversie (dat wil zeggen **3.8 +**). Voor deze distributies of leveranciers die hun eigen kernel onderhouden moeite is vereist voor het regelmatig backport de LIS-stuurprogramma's van de upstream-kernel voor uw aangepaste kernel.  Zelfs als u al een relatief recente kernelversie worden uitgevoerd, moet het is raadzaam om bij te houden eventuele upstream verbeteringen in de LIS-stuurprogramma's en backport die indien nodig. De locatie van de bronbestanden van de LIS-stuurprogramma is beschikbaar in de [instaan](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) bestand in de structuur van Linux-kernel bron:

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

Ten minste de afwezigheid van de volgende patches is bekend dat problemen veroorzaken in Azure en dus deze moeten worden opgenomen in de kernel. Deze lijst is geen volledige of voltooid voor alle distributies:

* [ata_piix: schijven aan de Hyper-V-stuurprogramma's standaard uitstellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: Account voor de pakketten in transit in het pad opnieuw instellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: gebruik van WRITE_SAME voorkomen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: dezelfde schrijven uitschakelen voor RAID en stuurprogramma's voor virtuele host-netwerkadapters](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL-aanwijzer fix voor referentie ongedaan maken](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: ring buffer storingen kunnen leiden tot i/o-bevriezing](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: bescherming tegen een dubbele uitvoering van __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>De Azure Linux Agent
De [Azure Linux Agent](../extensions/agent-linux.md) (waagent) is vereist voor het juist een Linux-machine inrichten in Azure. U kunt de nieuwste versie, de problemen met bestanden ophalen of indienen van pull-aanvragen op de [Linux Agent GitHub-opslagplaats](https://github.com/Azure/WALinuxAgent).

* De Linux-agent wordt de licentie Apache 2.0 uitgebracht. Groot aantal distributies al bieden RPM- of deb pakketten voor de agent, en dus in sommige gevallen kan dit kan worden geïnstalleerd en bijgewerkt zonder veel moeite.
* De Azure Linux Agent vereist Python v2.6 +.
* De agent is ook vereist voor de python pyasn1-module. De meeste distributies bieden dit als een afzonderlijk pakket dat kan worden geïnstalleerd.
* In sommige gevallen de Azure Linux Agent mogelijk niet compatibel is met NetworkManager. Veel van de RPM/Deb-pakketten dat is geleverd door distributies NetworkManager configureren als een conflict voor het pakket waagent en dus worden verwijderd NetworkManager wanneer u de Linux-agent-pakket installeert.
* De Azure Linux Agent moet worden boven de minimaal ondersteunde versie, Raadpleeg dit artikel voor [details](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Algemene Linux-systeemvereisten

* Wijzig de kernel boot line in WORMGATEN of GRUB2 om op te nemen van de volgende parameters. Dit zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Dit zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.
  
    Naast de bovenstaande, verdient het *verwijderen* de volgende parameters als deze bestaan:
  
        rhgb quiet crashkernel=auto
  
    Grafische en stil opstarten is niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op de kleinere VM-grootten.

* De Azure Linux-Agent installeren
  
    De Azure Linux Agent is vereist voor het inrichten van een installatiekopie van Linux op Azure.  Groot aantal distributies bieden de agent als een RPM- of Deb-pakket (het pakket wordt doorgaans genoemd 'WALinuxAgent' of 'walinuxagent').  De agent kan ook handmatig geïnstalleerd met de volgende stappen in de [Linux Agent handleiding](../extensions/agent-linux.md).

* Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.

* Maak geen wisselruimte op de besturingssysteemschijf
  
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. De lokale bronschijf wordt een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

* Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken als laatste stap:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Op Virtualbox mag u de volgende fout ziet nadat het is uitgevoerd ' waagent-force - inrichting ': `[Errno 5] Input/output error`. Dit foutbericht is niet kritiek en kan worden genegeerd.
  > 
  > 

* Sluit de virtuele machine en de VHD uploaden naar Azure.


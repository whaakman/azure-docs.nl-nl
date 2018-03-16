---
title: Maken en uploaden van een VHD Linux in Azure
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) die een Linux-besturingssysteem bevat.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
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
ms.openlocfilehash: b06144e6ad3df1626022edd856e14d6c47494336
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="information-for-non-endorsed-distributions"></a>Informatie voor niet-aanbevolen distributies
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

De Azure-platform SLA van toepassing op virtuele machines met Linux-besturingssysteem alleen wanneer een van de [goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) wordt gebruikt. Voor deze aangebracht distributies Linux installatiekopieën beschikbaar in Azure Marketplace met de vereiste configuratie.

* [Linux op Azure - goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ondersteuning voor Linux-afbeeldingen in Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alle verdelingen uitgevoerd op Azure moet voldoen aan een aantal vereisten voor een kans voor een correcte werking van het platform.  In dit artikel is geen uitgebreide omdat elk distributiepunt verschilt; en het is wel mogelijk dat zelfs als u voldoet aan de onderstaande criteria nog steeds u aanzienlijk aanpassen van uw Linux-systeem moet wordt om ervoor te zorgen dat deze correct wordt uitgevoerd op het platform.

Het is daarom raadzaam dat u met een van begint onze [Linux op Azure goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) indien mogelijk. De volgende artikelen begeleidt u bij het voorbereiden van de verschillende aangebracht Linux distributies die worden ondersteund op Azure:

* **[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

De rest van dit artikel wordt de nadruk gelegd op algemene richtlijnen voor het uitvoeren van uw Linux-distributie op Azure.

## <a name="general-linux-installation-notes"></a>Opmerkingen bij de installatie van de algemene Linux
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd. Als u van VirtualBox gebruikmaakt betekent dit dat selecteren **een vaste grootte** in plaats van de standaard dynamisch toegewezen bij het maken van de schijf.
* Azure biedt alleen ondersteuning voor virtuele machines van generatie 1. U kunt virtuele machines van generatie 1 converteren uit VHDX voor de VHD-indeling en dynamisch uitbreidbare naar een vaste grootte schijf. Maar u een virtuele machine generatie niet wijzigen. Zie voor meer informatie [moet ik een virtuele machine van generatie 1 of 2 maken in Hyper-V?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* De maximale grootte van de VHD is 1023 GB.
* Bij het installeren van de Linux-systeem is *aanbevolen* dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere identieke virtuele machine moet voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mag worden gebruikt voor gegevensschijven.
* Kernel-ondersteuning voor het koppelen van UDF-bestandssysteem is vereist. De configuratie van de inrichting wordt op de eerste keer opstarten op Azure via media UDF-indeling die is gekoppeld aan de Gast doorgegeven aan de Linux-VM. De Azure Linux-agent moet kunnen de UDF-bestandssysteem voor het lezen van de configuratie en inrichten van de virtuele machine te koppelen.
* Kernel-versies van Linux onder 2.6.37 ondersteunen geen NUMA op Hyper-V met grotere virtuele machine. Dit probleem voornamelijk effecten oudere distributies met behulp van de upstream Red Hat 2.6.32 kernel, en is vastgesteld in RHEL 6.6 (kernel 2.6.32 504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of op basis van RHEL kernels die ouder zijn dan 2.6.32-504 moet de parameter opstarten ingesteld `numa=off` op de opdrachtregel in grub.conf kernel. Zie voor meer informatie Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Configureer een partitie van de wisseling niet op de schijf met het besturingssysteem. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle VHD's in Azure, moeten een virtuele grootte die is afgestemd op 1MB hebben. Bij het converteren van een onbewerkte schijf naar VHD moet u ervoor zorgen dat de grootte voor onbewerkte schijven een veelvoud van 1MB vóór de conversie is. In de onderstaande stappen vindt meer informatie.

### <a name="installing-kernel-modules-without-hyper-v"></a>Kernelmodules zonder Hyper-V installeren
Azure wordt uitgevoerd op de Hyper-V-hypervisor, dus Linux vereist dat bepaalde kernelmodules zijn geïnstalleerd om te kunnen uitvoeren in Azure. Als u een virtuele machine die is gemaakt buiten de Hyper-V hebt, installatieprogramma's voor de Linux mag bevatten de stuurprogramma's voor Hyper-V in de eerste ramdisk (initrd of initramfs) tenzij er wordt gedetecteerd dat deze wordt uitgevoerd een een Hyper-V-omgeving. Als u een andere virtualisatie-systeem (dat wil zeggen Virtualbox, KVM, enz.) voor het voorbereiden van uw Linux-installatiekopie, mogelijk moet u opnieuw opbouwen van de initrd om ervoor te zorgen dat ten minste de `hv_vmbus` en `hv_storvsc` kernelmodules zijn beschikbaar op de eerste ramdisk.  Dit is een bekend probleem ten minste op systemen die op basis van de upstream Red Hat distributie.

Het mechanisme voor het opnieuw opbouwen van de installatiekopie van het initrd of initramfs kan variëren, afhankelijk van het distributiepunt. Raadpleeg de documentatie bij uw distributiepunt of ondersteuning voor de juiste procedure.  Hier volgt een voorbeeld voor het opnieuw opbouwen van de initrd met behulp van de `mkinitrd` hulpprogramma:

Eerst een back-up van de installatiekopie van het bestaande initrd:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Vervolgens opnieuw de initrd met de `hv_vmbus` en `hv_storvsc` kernelmodules:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>VHD's vergroten of verkleinen
VHD-installatiekopieën in Azure, moeten een virtuele grootte die is afgestemd op 1MB hebben.  VHD's die zijn gemaakt met behulp van Hyper-V moeten normaal gesproken al goed worden uitgelijnd.  Als de VHD niet correct wordt uitgelijnd wordt u een vergelijkbaar met het volgende foutbericht mogelijk wanneer u probeert te maken van een *installatiekopie* van uw VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

U verhelpt dit u kunt de grootte van de virtuele machine met de Hyper-V Manager-console of de [formaat VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-cmdlet.  Als u niet worden uitgevoerd in een Windows-omgeving wordt aanbevolen qemu img om te converteren (indien nodig) en het formaat van de VHD.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is verholpen in QEMU 2.6. Het verdient aanbeveling gebruik qemu-img 2.2.0 of kleine of bijwerken van op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Grootte wijzigen van de VHD met rechtstreeks de hulpprogramma's zoals `qemu-img` of `vbox-manage` kan leiden tot een opgestart VHD.  Daarom wordt het aanbevolen eerste converteren de VHD op een installatiekopie van het ONBEWERKTE schijf.  Als de VM-installatiekopie al als ONBEWERKTE schijfimage (de standaardinstelling voor bepaalde Hypervisors zoals KVM gemaakt is) kunt u deze stap overslaan:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. De vereiste grootte van de installatiekopie van de schijf om ervoor te zorgen dat de grootte van de virtuele wordt uitgelijnd op 1MB berekenen.  De volgende bash-shell-script kan u helpen bij dit.  Het script gebruikt '`qemu-img info`' om te bepalen van de virtuele grootte van de installatiekopie van de schijf en berekent vervolgens de grootte van de volgende 1 MB:
   
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

   Of, met qemu versie **2.6 +** omvatten de `force_size` optie:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Linux-Kernel-vereisten
De Linux Integration Services (LIS) stuurprogramma's voor Hyper-V en Azure worden aangeleverd rechtstreeks aan de upstream Linux-kernel. Groot aantal distributies met een recente versie van Linux kernel (dat wil zeggen 3.x) al deze stuurprogramma's beschikbaar zijn, of anders backported versies van deze stuurprogramma's voorzien van hun kernels.  Deze stuurprogramma's worden voortdurend wordt bijgewerkt in de upstream-kernel met nieuwe oplossingen en functies, indien mogelijk het wordt daarom aanbevolen om uit te voeren een [distributie goedgekeurde](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die deze oplossingen en -updates zal bevatten.

Als u een variant van de Red Hat Enterprise Linux-versies uitvoert **6.0 6.3**, moet u de nieuwste stuurprogramma's van LIS voor Hyper-V installeren. De stuurprogramma's kunnen worden gevonden [op deze locatie](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Vanaf RHEL **6.4 +** (en afleidingen van) de LIS stuurprogramma's zijn al opgenomen in de kernel en dus geen aanvullende installatie-pakketten zijn vereist voor het uitvoeren van deze systemen op Azure.

Als een aangepaste kernel vereist is, is het raadzaam een meer recente kernelversie gebruiken (dat wil zeggen **3.8 +**). Voor deze distributies of leveranciers die hun eigen kernel onderhouden moeite zijn vereist voor regelmatig backport LIS stuurprogramma's van de upstream-kernel aan uw aangepaste kernel.  Zelfs als u al een relatief recente kernelversie wordt uitgevoerd, moet het is raadzaam om bij te houden in de LIS stuurprogramma's en backport upstream fixes die indien nodig. De locatie van de bronbestanden van de LIS-stuurprogramma is beschikbaar in de [instaan](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) bestand in de boomstructuur van Linux kernel bron:

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

Bij een zeer minimum, het ontbreken van de volgende patches bekend is dat deze problemen veroorzaken in Azure en dus dit moeten zijn opgenomen in de kernel. Deze lijst is geen volledig of voltooid voor alle verdelingen:

* [ata_piix: schijven aan de Hyper-V-stuurprogramma's standaard uitgesteld](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: Account voor de pakketten in transit in het pad opnieuw instellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: informatie over het gebruik van WRITE_SAME voorkomen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: dezelfde schrijven uitschakelen voor RAID en stuurprogramma's voor netwerkadapters virtuele host](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL-aanwijzer fix verwijzing](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: ring buffer fouten kunnen leiden tot i/o-blokkeren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: beschermen tegen een dubbele uitvoering van __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>De Azure Linux-Agent
De [Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (waagent) is vereist voor het inrichten goed een virtuele Linux-machine in Azure. U kunt de nieuwste versie, problemen met de bestanden ophalen of pull-aanvragen op verzenden de [Linux Agent GitHub-repo-](https://github.com/Azure/WALinuxAgent).

* De Linux-agent wordt uitgebracht onder de licentie Apache 2.0. Groot aantal distributies al Geef RPM of deb pakketten voor de agent, en dus in sommige gevallen dit kan worden geïnstalleerd en bijgewerkt met weinig moeite.
* De Azure Linux Agent vereist Python v2.6 +.
* De agent is ook vereist voor de module python pyasn1. De meeste distributies biedt dit als een afzonderlijk pakket dat kan worden geïnstalleerd.
* In sommige gevallen de Azure Linux Agent mogelijk niet compatibel met NetworkManager. Veel van de RPM/Deb-pakketten dat is geleverd door distributies NetworkManager configureren als een conflict met het pakket waagent en dus verwijdert NetworkManager wanneer u de Linux-agent-pakket installeert.
* De Azure Linux Agent moet hoger dan de minimaal ondersteunde versie Raadpleeg dit artikel voor [details](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Linux-systeem voor algemene vereisten

* De regel voor het opstarten van kernel in GRUB of GRUB2 om op te nemen van de volgende parameters wijzigen. Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Naast de bovenstaande, wordt u aangeraden te *verwijderen* de volgende parameters als deze bestaan:
  
        rhgb quiet crashkernel=auto
  
    Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort. De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar dat deze parameter wordt Verklein de hoeveelheid beschikbaar geheugen in de virtuele machine 128 MB of meer, die mogelijk problemen op de VM-kleinere opmerking.

* De Azure Linux-Agent installeren
  
    De Azure Linux Agent is vereist voor het inrichten van een Linux-installatiekopie in Azure.  Groot aantal distributies bieden de agent als een RPM of Deb-pakket (het pakket wordt doorgaans genoemd 'WALinuxAgent' of 'walinuxagent').  De agent kan ook worden geïnstalleerd handmatig door de stappen in de [Linux Agent handleiding](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.

* Maak geen wisselruimte op de schijf met het besturingssysteem
  
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

* Voer de volgende opdrachten voor de inrichting ervan ongedaan maakt de virtuele machine als een laatste stap:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Op Virtualbox mogelijk ziet u de volgende fout na het uitvoeren ' waagent-force - deprovision': `[Errno 5] Input/output error`. Dit foutbericht is niet kritiek en kan worden genegeerd.
  > 
  > 

* U moet de virtuele machine afsluiten en de VHD te uploaden naar Azure.


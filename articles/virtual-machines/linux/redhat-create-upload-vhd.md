---
title: Maken en uploaden van een Red Hat Enterprise Linux-VHD voor gebruik in Azure | Microsoft Docs
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een Red Hat Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: szark
ms.openlocfilehash: 01acdf23c3113c3c4d185263b5cab75f3efd34a2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58001658"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Een op Red Hat gebaseerde virtuele machine voor Azure voorbereiden
In dit artikel leert u hoe u een virtuele machine van Red Hat Enterprise Linux (RHEL) voorbereidt voor gebruik in Azure. De versies van RHEL die worden beschreven in dit artikel zijn 6.7 + en 7.1 +. De hypervisors voor voorbereiding die worden beschreven in dit artikel zijn Hyper-V, op basis van een kernel virtuele machine (KVM) en VMware. Zie voor meer informatie over vereisten voor geschiktheid voor uw deelname aan het Red Hat Cloud Access programma [van Red Hat Cloud Access website](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) en [RHEL uitgevoerd op Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Een Red Hat gebaseerde virtuele machine op basis van Hyper-V Manager voorbereiden

### <a name="prerequisites"></a>Vereisten
In deze sectie wordt ervan uitgegaan dat u hebt al een ISO-bestand vanaf de website van Red Hat verkregen en de RHEL-installatiekopie hebt geïnstalleerd op een virtuele harde schijf (VHD). Zie voor meer informatie over het gebruik van Hyper-V-beheer voor het installeren van een besturingssysteeminstallatiekopie [de Hyper-V-rol installeren en configureren van een virtuele Machine](https://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie van de RHEL**

* Azure biedt geen ondersteuning voor VHDX-indeling. Azure ondersteunt alleen vaste VHD. U kunt Hyper-V Manager gebruiken voor de schijf converteren naar VHD-indeling of kunt u de cmdlet convert-vhd. Als u VirtualBox gebruiken, selecteert u **vaste grootte** in plaats van de standaard dynamisch toegewezen optie bij het maken van de schijf.
* Azure ondersteunt alleen virtuele machines van generatie 1. U kunt een virtuele machine generatie 1 converteren van VHDX, zodat de VHD-indeling en dynamisch uit te breiden naar een schijf met vaste grootte. U kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie, [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* De maximale grootte die toegestaan voor de VHD is 1023 GB.
* Logische Volume Manager (LVM) wordt ondersteund en kan worden gebruikt op de besturingssysteemschijf en gegevensschijven in virtuele machines van Azure. In het algemeen wordt het echter aanbevolen standaard partities op de besturingssysteemschijf in plaats van LVM gebruiken. Met deze procedure vermijdt LVM naam conflicteert met de gekloonde virtuele machines, met name als u ooit een besturingssysteemschijf koppelen aan een andere identieke virtuele machine moet voor probleemoplossing. Zie ook [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) documentatie.
* Kernel-ondersteuning voor het koppelen van bestandssystemen Universal Disk Format (UDF's) is vereist. Op de eerste keer opstarten op Azure geeft de UDF-indeling media die is gekoppeld aan de Gast de inrichtingsconfiguratie aan de virtuele Linux-machine. De Azure Linux Agent moet u mogelijk de UDF-bestandssysteem om te lezen van de configuratie en het inrichten van de virtuele machine koppelen.
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-Agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Meer informatie vindt u in de onderstaande stappen. Zie ook [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer informatie.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Een RHEL 6 virtuele machine op basis van Hyper-V Manager voorbereiden

1. Selecteer de virtuele machine in Hyper-V-beheer.

1. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

1. In de RHEL-6, NetworkManager kan leiden tot problemen met de Azure Linux agent. Dit pakket verwijderen door de volgende opdracht uit:
   
        # sudo rpm -e --nodeps NetworkManager

1. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Verplaats de udev-regels om te voorkomen dat statische regels voor de Ethernet-interface voor het genereren (of verwijderen). Deze regels veroorzaken problemen bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # sudo chkconfig network on

1. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze wijziging `/boot/grub/menu.lst` in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Dit ook zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.
    
    Bovendien wordt aangeraden dat u de volgende parameters:
    
        rhgb quiet crashkernel=auto
    
    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort.  U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer wordt. Deze configuratie mogelijk problemen op kleinere virtuele machine.


1. Zorg ervoor dat de secure shell (SSH)-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten, dit doorgaans de standaardinstelling is. Wijzig /etc/ssh/sshd_config om op te nemen in de volgende regel:

        ClientAliveInterval 180

1. De Azure Linux-Agent installeren met de volgende opdracht:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    Installatie van het pakket WALinuxAgent verwijdert de NetworkManager en NetworkManager gnome pakketten als ze zijn niet verwijderd in stap 3.

1. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en dat deze kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, op de juiste wijze de volgende parameters in /etc/waagent.conf wijzigen:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Het abonnement (indien nodig) registratie met de volgende opdracht:

        # sudo subscription-manager unregister

1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Klik op **actie** > **afsluiten** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Een RHEL 7 virtuele machine op basis van Hyper-V Manager voorbereiden

1. Selecteer de virtuele machine in Hyper-V-beheer.

1. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

1. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

1. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # sudo systemctl enable network

1. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze wijziging `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Dit ook zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze configuratie worden ook uitgeschakeld de nieuwe naamgeving van RHEL 7 NIC's. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

1. Als u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht om op te bouwen de grub-configuratie:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

1. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten, dit doorgaans de standaardinstelling is. Wijzigen `/etc/ssh/sshd_config` om op te nemen in de volgende regel:

        ClientAliveInterval 180

1. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. De Azure Linux-Agent installeren met de volgende opdracht:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

1. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Als u wilt de registratie van het abonnement, moet u de volgende opdracht uitvoeren:

        # sudo subscription-manager unregister

1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Klik op **actie** > **afsluiten** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Een Red Hat gebaseerde virtuele machine op basis van KVM voorbereiden
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>Een RHEL 6 virtuele machine op basis van KVM voorbereiden

1. Download het KVM-image van RHEL 6 van de Red Hat-website.

1. Een root-wachtwoord instellen.

    Een versleuteld wachtwoord genereren en kopieer de uitvoer van de opdracht:

        # openssl passwd -1 changeme

    Stel een hoofdwachtwoord met guestfish:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Wijzigen van de tweede veld van de hoofdgebruiker van "." in het versleutelde wachtwoord.

1. Maak een virtuele machine in KVM vanuit de installatiekopie van het qcow2. Het schijftype instellen op **qcow2**, en stelt u het virtuele netwerkinterface-Apparaatmodel op **virtio**. Vervolgens start u de virtuele machine en meld u aan als hoofdgebruiker.

1. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Verplaats de udev-regels om te voorkomen dat statische regels voor de Ethernet-interface voor het genereren (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Azure of Hyper-V: klonen

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # chkconfig network on

1. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

1. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze configuratie `/boot/grub/menu.lst` in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Dit ook zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.
    
    Bovendien is het raadzaam dat u de volgende parameters:
    
        rhgb quiet crashkernel=auto
    
    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.


1. Hyper-V-modules aan initramfs toevoegen:  

    Bewerken `/etc/dracut.conf`, en voeg de volgende inhoud:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Opnieuw opbouwen initramfs:

        # dracut -f -v

1. Cloud-init verwijderen:

        # yum remove cloud-init

1. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten:

        # chkconfig sshd on

    Wijzig /etc/ssh/sshd_config om op te nemen van de volgende regels:

        PasswordAuthentication yes
        ClientAliveInterval 180

1. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. De Azure Linux-Agent installeren met de volgende opdracht:

        # yum install WALinuxAgent

        # chkconfig waagent on

1. De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in **/etc/waagent.conf** op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Het abonnement (indien nodig) registratie met de volgende opdracht:

        # subscription-manager unregister

1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. De virtuele machine in KVM afgesloten.

1. De afbeelding qcow2 naar de VHD-indeling converteren.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f qcow2 -O raw rhel-6.9.qcow2 rhel-6.9.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.9.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd

        
### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>Een RHEL 7 virtuele machine op basis van KVM voorbereiden

1. Download de KVM-afbeelding van RHEL 7 van de Red Hat-website. Deze procedure maakt gebruik van RHEL 7 als voorbeeld.

1. Een root-wachtwoord instellen.

    Een versleuteld wachtwoord genereren en kopieer de uitvoer van de opdracht:

        # openssl passwd -1 changeme

    Stel een hoofdwachtwoord met guestfish:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Wijzigen van de tweede veld van de hoofdgebruiker van "." in het versleutelde wachtwoord.

1. Maak een virtuele machine in KVM vanuit de installatiekopie van het qcow2. Het schijftype instellen op **qcow2**, en stelt u het virtuele netwerkinterface-Apparaatmodel op **virtio**. Vervolgens start u de virtuele machine en meld u aan als hoofdgebruiker.

1. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

1. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # sudo systemctl enable network

1. Registreer uw Red Hat-abonnement om in te schakelen van installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

1. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze configuratie `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Met deze opdracht zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. De opdracht worden ook uitgeschakeld de nieuwe naamgeving van RHEL 7 NIC's. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

1. Als u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht om op te bouwen de grub-configuratie:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

1. Hyper-V-modules in initramfs toevoegen.

    Bewerken `/etc/dracut.conf` en voeg inhoud toe:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Opnieuw opbouwen initramfs:

        # dracut -f -v

1. Cloud-init verwijderen:

        # yum remove cloud-init

1. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten:

        # systemctl enable sshd

    Wijzig /etc/ssh/sshd_config om op te nemen van de volgende regels:

        PasswordAuthentication yes
        ClientAliveInterval 180

1. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. De Azure Linux-Agent installeren met de volgende opdracht:

        # yum install WALinuxAgent

    De service waagent inschakelen:

        # systemctl enable waagent.service

1. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Het abonnement (indien nodig) registratie met de volgende opdracht:

        # subscription-manager unregister

1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. De virtuele machine in KVM afgesloten.

1. De afbeelding qcow2 naar de VHD-indeling converteren.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.4.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Een Red Hat gebaseerde virtuele machine van VMware voorbereiden
### <a name="prerequisites"></a>Vereisten
In deze sectie wordt ervan uitgegaan dat u een virtuele RHEL-machine in VMware al hebt geïnstalleerd. Zie voor meer informatie over het installeren van een besturingssysteem in VMware [installatiehandleiding voor VMware Gast-besturingssysteem](https://partnerweb.vmware.com/GOSIG/home.html).

* Wanneer u de Linux-besturingssysteem installeert, raden wij aan dat u standaard partities in plaats van LVM, dit is vaak de standaardwaarde voor vele installaties. Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machine, dat met name als de schijf van een besturingssysteem ooit worden gekoppeld aan een andere virtuele machine moet voor probleemoplossing. LVM of RAID kan op gegevensschijven worden gebruikt als de voorkeur.
* Stel een swap-partitie niet op de besturingssysteemschijf. U kunt de Linux-agent voor het maken van een wisselbestand op de tijdelijke schijf configureren. Meer informatie hierover vindt u in de volgende stappen.
* Wanneer u de virtuele harde schijf maakt, selecteert u **Store virtuele schijf als een enkel bestand**.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>Een RHEL 6 virtuele machine van VMware voorbereiden
1. In de RHEL-6, NetworkManager kan leiden tot problemen met de Azure Linux agent. Dit pakket verwijderen door de volgende opdracht uit:
   
        # sudo rpm -e --nodeps NetworkManager

1. Maak een bestand met de naam **netwerk** in de map/etc/sysconfig/met de volgende tekst:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Verplaats de udev-regels om te voorkomen dat statische regels voor de Ethernet-interface voor het genereren (of verwijderen). Deze regels veroorzaken problemen wanneer u een virtuele machine in Azure of Hyper-V: klonen

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # sudo chkconfig network on

1. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. U doet dit door open `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Dit ook zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

1. Hyper-V-modules aan initramfs toevoegen:

    Bewerken `/etc/dracut.conf`, en voeg de volgende inhoud:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Opnieuw opbouwen initramfs:

        # dracut -f -v

1. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten, dit doorgaans de standaardinstelling is. Wijzigen `/etc/ssh/sshd_config` om op te nemen in de volgende regel:

    ClientAliveInterval 180

1. De Azure Linux-Agent installeren met de volgende opdracht:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

1. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Het abonnement (indien nodig) registratie met de volgende opdracht:

        # sudo subscription-manager unregister

1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Sluit de virtuele machine en het VMDK-bestand converteren naar een VHD-bestand.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f vmdk -O raw rhel-6.9.vmdk rhel-6.9.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.9.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Een RHEL 7 virtuele machine van VMware voorbereiden
1. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

1. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # sudo systemctl enable network

1. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze wijziging `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Deze configuratie zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe naamgeving van RHEL 7 voor NIC's. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

1. Als u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht om op te bouwen de grub-configuratie:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

1. Hyper-V-modules aan initramfs toevoegen.

    Bewerken `/etc/dracut.conf`, Voeg inhoud toe:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Opnieuw opbouwen initramfs:

        # dracut -f -v

1. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is doorgaans de standaardinstelling. Wijzigen `/etc/ssh/sshd_config` om op te nemen in de volgende regel:

        ClientAliveInterval 180

1. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. De Azure Linux-Agent installeren met de volgende opdracht:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

1. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Als u wilt de registratie van het abonnement, moet u de volgende opdracht uitvoeren:

        # sudo subscription-manager unregister

1. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Sluit de virtuele machine en het VMDK-bestand converteren naar de VHD-indeling.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f vmdk -O raw rhel-7.4.vmdk rhel-7.4.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.4.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Een Red Hat gebaseerde virtuele machine op basis van een ISO met behulp van een bestand kickstart automatisch voorbereiden
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Een RHEL 7 virtuele machine uit een bestand kickstart voorbereiden

1.  Maak een bestand kickstart met de volgende inhoud en sla het bestand. Zie voor meer informatie over de installatie van kickstart de [Kickstart installatiehandleiding](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

1. Plaats het bestand kickstart waar het installatie-systeem kunt openen.

1. Maak een nieuwe virtuele machine in Hyper-V-beheer. Op de **virtuele hardeschijf aansluiten** weergeeft, schakelt **later een virtuele harde schijf koppelen**, en voltooi de Wizard Nieuwe virtuele Machine.

1. Open de instellingen van de virtuele machine:

    a.  Een nieuwe virtuele harde schijf koppelen aan de virtuele machine. Zorg ervoor dat u selecteert **VHD-indeling** en **vaste grootte**.

    b.  De ISO-installatie aan het DVD-station koppelen.

    c.  Stel het BIOS om op te starten vanaf de CD.

1. Hiermee wordt de virtuele machine gestart. Wanneer de installatiehandleiding wordt weergegeven, drukt u op **tabblad** het configureren van de opstartopties.

1. ENTER `inst.ks=<the location of the kickstart file>` aan het einde van de opstartopties en druk op **Enter**.

1. Wacht tot de installatie is voltooid. Wanneer deze voltooid, wordt de virtuele machine automatisch afgesloten. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="known-issues"></a>Bekende problemen
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>De Hyper-V-stuurprogramma kan niet worden opgenomen in de eerste RAM-schijf bij het gebruik van een niet-Hyper-V-hypervisor

In sommige gevallen, Linux-installatieprogramma's mogelijk niet bevatten de stuurprogramma's voor Hyper-V in de eerste RAM-schijf (initrd of initramfs) tenzij Linux wordt gedetecteerd dat deze wordt uitgevoerd in een omgeving met Hyper-V.

Wanneer u een andere virtualisatiesysteem (dat wil zeggen, Virtualbox, Xen, enzovoort) gebruikt voor het voorbereiden van uw Linux-installatiekopie, moet u mogelijk opnieuw opbouwen initrd om ervoor te zorgen dat ten minste de hv_vmbus en hv_storvsc kernel-modules zijn beschikbaar op de eerste RAM-schijf. Dit is een bekend probleem ten minste op systemen die zijn gebaseerd op de upstream Red Hat-distributie.

U lost dit probleem, Hyper-V-modules toevoegen aan initramfs en deze opnieuw bouwen:

Bewerken `/etc/dracut.conf`, en voeg de volgende inhoud:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

Opnieuw opbouwen initramfs:

        # dracut -f -v

Zie de informatie voor meer informatie over [opnieuw opbouwen van initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw Red Hat Enterprise Linux virtuele harde schijf gebruiken om te maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).

Zie voor meer informatie over de hypervisors die zijn gecertificeerd voor Red Hat Enterprise Linux [de Red Hat-website](https://access.redhat.com/certified-hypervisors).

---
title: Maken en uploaden van een Red Hat Enterprise Linux VHD voor gebruik in Azure-Stack | Microsoft Docs
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) waarop een Red Hat Linux-besturingssysteem.
services: azure-stack
documentationcenter: ''
author: JeffGoldner
manager: BradleyB
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: jeffgo
ms.openlocfilehash: d7b79962822dcfec96c7087100b1537d8046ab46
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure-stack"></a>Een Red Hat-virtuele machine voorbereiden voor Azure-Stack
In dit artikel leert u hoe u een virtuele machine van Red Hat Enterprise Linux (RHEL) voorbereidt voor gebruik in Azure-Stack. De versies van RHEL die worden beschreven in dit artikel zijn 7.1 +. De hypervisors voor voorbereiding die worden besproken in dit artikel zijn Hyper-V, op basis van de kernel virtuele machine (KVM) en VMware.

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Voorbereiden van een Red Hat-virtuele machine vanuit Hyper-V-beheer

### <a name="prerequisites"></a>Vereisten
Deze sectie wordt ervan uitgegaan dat u hebt al een ISO-bestand van de website van Red Hat verkregen en de installatiekopie van het RHEL geïnstalleerd op een virtuele harde schijf (VHD). Zie voor meer informatie over het gebruik van Hyper-V-beheer voor het installeren van een besturingssysteeminstallatiekopie [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie van de RHEL**

* Azure-Stack biedt geen ondersteuning voor VHDX-indeling. Azure ondersteunt alleen vaste VHD. U kunt Hyper-V-beheer de schijf converteren naar VHD-indeling of kunt u de cmdlet convert-vhd. Als u VirtualBox gebruikt, selecteert u **een vaste grootte** in plaats van de standaard dynamisch toegewezen optie wanneer u de schijf maken.
* Azure Stack ondersteunt alleen virtuele machines van generatie 1. U kunt virtuele machines van generatie 1 converteren uit VHDX voor de VHD-indeling en dynamisch uitbreidbare naar een schijf met vaste grootte. U kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie [moet ik een virtuele machine van generatie 1 of 2 maken in Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* De maximale grootte die toegestaan voor de VHD is 1023 GB.
* Wanneer u de Linux-besturingssysteem installeert, wordt u aangeraden dat u standaard partities in plaats van logische Volume Manager (LVM) Dit is vaak de standaardwaarde voor vele installaties. Hierdoor wordt voorkomen LVM naam conflicteert met de gekloonde virtuele machines, met name als u ooit een schijf koppelen aan een andere identieke virtuele machine moet voor het oplossen van problemen.
* Kernel-ondersteuning voor het koppelen van bestandssystemen Universal UDF (Disk Format) is vereist. Op de eerste keer opstarten op Azure geeft de UDF-indeling media die is gekoppeld aan de Gast de configuratie van de inrichting aan de virtuele Linux-machine. De Azure Linux Agent moet kunnen de UDF-bestandssysteem voor het lezen van de configuratie en inrichten van de virtuele machine te koppelen.
* Versies van de kernel Linux die ouder dan 2.6.37 zijn ondersteunen geen niet-uniforme geheugentoegang (NUMA) op Hyper-V met grotere virtuele machine. Dit probleem voornamelijk effecten oudere distributies die gebruikmaken van de upstream-kernel Red Hat 2.6.32 en is vastgesteld in RHEL 6.6 (kernel 2.6.32 504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of RHEL-kernels die ouder dan 2.6.32-504 zijn moeten ingesteld de `numa=off` parameter op de opdrachtregel van de kernel in grub.conf opstart. Zie voor meer informatie, Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Configureer een partitie van de wisseling niet op de schijf van het besturingssysteem. De Linux-Agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle VHD's in Azure, moeten een virtuele grootte die is afgestemd op 1MB hebben. Bij het converteren van een onbewerkte schijf naar VHD moet u ervoor zorgen dat de grootte voor onbewerkte schijven een veelvoud van 1MB vóór de conversie is. In de onderstaande stappen vindt u meer informatie.
* Azure-Stack biedt geen ondersteuning voor initialisatie van de cloud. U virtuele machine moet worden geconfigureerd met een ondersteunde versie van Windows Azure Linux Agent (WALA).

### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Voorbereiden van een RHEL 7 virtuele machine vanuit Hyper-V-beheer

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

3. Creëer of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Creëer of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
    NM_CONTROLLED = Nee

5. Zorg ervoor dat de netwerkservice tijdens het opstarten wordt gestart met de volgende opdracht:

        # sudo systemctl enable network

6. Registreer uw abonnement Red Hat zodat de installatie van pakketten uit de opslagplaats voor RHEL met de volgende opdracht:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Open hiertoe deze wijziging, `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Dit ook zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze configuratie schakelt ook uit de nieuwe RHEL 7 naamconventies voor NIC's. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` optie desgewenst geconfigureerd. Houd er rekening mee dat deze parameter wordt gereduceerd de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

8. Nadat u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht voor het opnieuw samenstellen van de configuratie van de grub:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten, doorgaans de standaardinstelling is. Wijzig `/etc/ssh/sshd_config` om op te nemen van de volgende regel:

        ClientAliveInterval 180

10. Het pakket WALinuxAgent `WALinuxAgent-<version>`, is naar de opslagplaats voor Red Hat extra's is gepusht. De opslagplaats voor extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Installeer de Azure Linux Agent met de volgende opdracht:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. Maak geen wisselruimte op de schijf van het besturingssysteem.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de schijf van de lokale resource een tijdelijke schijf is en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Nadat u de Azure Linux Agent hebt geïnstalleerd in de vorige stap, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Als u wilt de registratie van het abonnement, voert u de volgende opdracht:

        # sudo subscription-manager unregister

14. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Klik op **actie** > **afgesloten** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Een Red Hat-virtuele machine uit KVM voorbereiden

### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>Een virtuele machine voor RHEL 7 van KVM voorbereiden

1. Download de installatiekopie van het KVM RHEL 7 van de website van Red Hat. Deze procedure maakt gebruik van RHEL 7 als voorbeeld.

2. Een root-wachtwoord instellen.

    Een versleutelde wachtwoord genereren en kopieer de uitvoer van de opdracht:

        # openssl passwd -1 changeme

    Stel het wachtwoord voor een hoofdaccount met guestfish:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Het tweede veld wijzigen voor de hoofdgebruiker van '. ' in het versleutelde wachtwoord.

3. Een virtuele machine van de installatiekopie van het qcow2 in KVM maken. Het schijftype ingesteld op **qcow2**, en stel het virtuele netwerk interface model op **virtio**. Vervolgens de virtuele machine starten en meld u aan als hoofdmap.

4. Creëer of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Creëer of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. Zorg ervoor dat de netwerkservice tijdens het opstarten wordt gestart met de volgende opdracht:

        # sudo systemctl enable network

7. Registreer uw abonnement Red Hat zodat de installatie van pakketten uit de opslagplaats voor RHEL met de volgende opdracht:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Open hiertoe deze configuratie, `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Met deze opdracht zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. De opdracht schakelt ook uit de nieuwe RHEL 7 naamconventies voor NIC's. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` optie desgewenst geconfigureerd. Houd er rekening mee dat deze parameter wordt gereduceerd de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

9. Nadat u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht voor het opnieuw samenstellen van de configuratie van de grub:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Hyper-V-modules toevoegen aan initramfs.

    Bewerken `/etc/dracut.conf` en inhoud toevoegen:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Opnieuw samenstellen initramfs:

        # dracut -f -v

11. Cloud-init verwijderen:

        # yum remove cloud-init

12. Controleer of de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten:

        # systemctl enable sshd

    Wijzig /etc/ssh/sshd_config zodanig dat de volgende regels:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. Het pakket WALinuxAgent `WALinuxAgent-<version>`, is naar de opslagplaats voor Red Hat extra's is gepusht. De opslagplaats voor extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Installeer de Azure Linux Agent met de volgende opdracht:

        # yum install WALinuxAgent

    Schakel de waagent-service:

        # systemctl enable waagent.service

15. Maak geen wisselruimte op de schijf van het besturingssysteem.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de schijf van de lokale resource een tijdelijke schijf is en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Nadat u de Azure Linux Agent hebt geïnstalleerd in de vorige stap, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Hef de registratie van het abonnement (indien nodig) met de volgende opdracht:

        # subscription-manager unregister

17. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. De virtuele machine in KVM afgesloten.

19. De installatiekopie van het qcow2 converteren naar het VHD-indeling.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is verholpen in QEMU 2.6. Het verdient aanbeveling gebruik qemu-img 2.2.0 of kleine of bijwerken van op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
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


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Een Red Hat-virtuele machine van VMware voorbereiden
### <a name="prerequisites"></a>Vereisten
Deze sectie wordt ervan uitgegaan dat u al een RHEL virtuele machine in VMware hebt geïnstalleerd. Zie voor meer informatie over het installeren van een besturingssysteem in VMware [VMware Guest Operating System installatiehandleiding](http://partnerweb.vmware.com/GOSIG/home.html).

* Wanneer u de Linux-besturingssysteem installeert, wordt u aangeraden dat u standaard partities in plaats van LVM, dit is vaak de standaardwaarde voor vele installaties. Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machine, met name als de schijf van een besturingssysteem ooit worden gekoppeld aan een andere virtuele machine moet voor het oplossen van problemen. LVM of RAID kan worden gebruikt op gegevensschijven als voorkeur.
* Configureer een partitie van de wisseling niet op de schijf van het besturingssysteem. U kunt de Linux-agent voor het maken van een wisselbestand op de tijdelijke schijf configureren. U vindt meer informatie over deze in de stappen volgen.
* Wanneer u de virtuele harde schijf maakt, selecteert u **Store virtuele schijf als een enkel bestand**.


### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Een RHEL 7 virtuele machine van VMware voorbereiden
1. Creëer of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. Creëer of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. Zorg ervoor dat de netwerkservice tijdens het opstarten wordt gestart met de volgende opdracht:

        # sudo chkconfig network on

4. Registreer uw abonnement Red Hat zodat de installatie van pakketten uit de opslagplaats voor RHEL met de volgende opdracht:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Open hiertoe deze wijziging, `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Deze configuratie zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe RHEL 7 naamconventies voor NIC's. Bovendien is het raadzaam dat u de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` optie desgewenst geconfigureerd. Houd er rekening mee dat deze parameter wordt gereduceerd de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

6. Nadat u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht voor het opnieuw samenstellen van de configuratie van de grub:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. Hyper-V-modules toevoegen aan initramfs.

    Bewerken `/etc/dracut.conf`, inhoud toevoegen:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Opnieuw samenstellen initramfs:

        # dracut -f -v

8. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is doorgaans de standaardinstelling. Wijzig `/etc/ssh/sshd_config` om op te nemen van de volgende regel:

        ClientAliveInterval 180

9. Het pakket WALinuxAgent `WALinuxAgent-<version>`, is naar de opslagplaats voor Red Hat extra's is gepusht. De opslagplaats voor extra's inschakelen met de volgende opdracht:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Installeer de Azure Linux Agent met de volgende opdracht:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. Maak geen wisselruimte op de schijf van het besturingssysteem.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de schijf van de lokale resource een tijdelijke schijf is en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Nadat u de Azure Linux Agent hebt geïnstalleerd in de vorige stap, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Als u wilt de registratie van het abonnement, voert u de volgende opdracht:

        # sudo subscription-manager unregister

13. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. Sluit de virtuele machine en het VMDK-bestand naar de VHD-indeling converteren.

> [!NOTE]
> Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is verholpen in QEMU 2.6. Het verdient aanbeveling gebruik qemu-img 2.2.0 of kleine of bijwerken van op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.
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


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Een Red Hat-virtuele machine van een ISO met behulp van een bestand kickstart automatisch voorbereiden
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Een RHEL 7 virtuele machine uit een bestand kickstart voorbereiden

1.  Maak een kickstart-bestand met de volgende inhoud en sla het bestand. Zie voor meer informatie over installatie kickstart de [Kickstart installatiehandleiding](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

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

2. Plaats het kickstart bestand waartoe het installatie-systeem toegang heeft.

3. Maak een nieuwe virtuele machine in Hyper-V-beheer. Op de **virtuele hardeschijf aansluiten** pagina **later een virtuele harde schijf koppelen**, en voltooi de Wizard Nieuwe virtuele Machine.

4. Open de instellingen van de virtuele machine:

    a.  Een nieuwe virtuele harde schijf koppelen aan de virtuele machine. Zorg ervoor dat u selecteert **VHD-indeling** en **vaste grootte**.

    b.  De installatie van de ISO koppelen aan het DVD-station.

    c.  Stel het BIOS om op te starten vanaf de CD.

5. Hiermee wordt de virtuele machine gestart. Wanneer de installatiehandleiding wordt weergegeven, drukt u op **tabblad** de opstartopties te configureren.

6. Voer `inst.ks=<the location of the kickstart file>` aan het einde van de opstartopties en druk op **Enter**.

7. Wacht totdat de installatie is voltooid. Wanneer deze voltooid, wordt de virtuele machine automatisch afgesloten. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

## <a name="known-issues"></a>Bekende problemen
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Het Hyper-V-stuurprogramma kan niet worden opgenomen in de eerste RAM-schijf wanneer u een niet-Hyper-V-hypervisor

In sommige gevallen Linux installatieprogramma's mogelijk niet opnemen de stuurprogramma's voor Hyper-V in de eerste RAM-schijf (initrd of initramfs) tenzij Linux vaststelt dat deze wordt uitgevoerd in een Hyper-V-omgeving.

Wanneer u een andere virtualisatie-systeem (dat wil zeggen, Virtualbox, Xen, enz.) voor het voorbereiden van uw Linux-installatiekopie, moet u mogelijk opnieuw samenstellen initrd om ervoor te zorgen dat ten minste de hv_vmbus en hv_storvsc kernel-modules zijn beschikbaar op de eerste RAM-schijf. Dit is een bekend probleem ten minste op systemen die zijn gebaseerd op de upstream Red Hat distributie.

U lost dit probleem, Hyper-V-modules toevoegen aan initramfs en opnieuw maken:

Bewerken `/etc/dracut.conf`, en voeg de volgende inhoud:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

Opnieuw samenstellen initramfs:

        # dracut -f -v

Zie voor meer informatie, de informatie over [opnieuw opbouwen van initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de Red Hat Enterprise Linux virtuele harde schijf voor het maken van nieuwe virtuele machines in Azure-Stack. Als dit de eerste keer dat u de VHD-bestand naar Azure-Stack uploadt, Zie [gebruik van de toolkit Marketplace maken en publiceren van marketplace-items](azure-stack-marketplace-publisher.md).

Zie voor meer informatie over de hypervisors die zijn gecertificeerd voor het uitvoeren van Red Hat Enterprise Linux [de website van Red Hat](https://access.redhat.com/certified-hypervisors).

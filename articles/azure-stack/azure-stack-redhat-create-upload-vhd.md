---
title: Maken en uploaden van een Red Hat Enterprise Linux-VHD voor gebruik in Azure Stack | Microsoft Docs
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een Red Hat Linux-besturingssysteem.
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
ms.date: 05/11/2018
ms.author: jeffgo
ms.openlocfilehash: 82a8da5897d811f80dd18cc199cb31f810a5a438
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399784"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure-stack"></a>Een Red Hat gebaseerde virtuele machine voorbereiden voor Azure Stack

In dit artikel leert u hoe u een virtuele machine van Red Hat Enterprise Linux (RHEL) voorbereidt voor gebruik in Azure Stack. De versies van RHEL die worden beschreven in dit artikel zijn 7.1 +. De hypervisors voor voorbereiding die worden beschreven in dit artikel zijn Hyper-V, op basis van een kernel virtuele machine (KVM) en VMware.

Raadpleeg voor informatie over Red Hat Enterprise Linux, [Red Hat en Azure Stack: veelgestelde vragen over](https://access.redhat.com/articles/3413531).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Een Red Hat gebaseerde virtuele machine op basis van Hyper-V Manager voorbereiden

In deze sectie wordt ervan uitgegaan dat u al hebt een ISO-bestand van de Red Hat-website en de RHEL-installatiekopie hebt geïnstalleerd op een virtuele harde schijf (VHD). Zie voor meer informatie over het gebruik van Hyper-V-beheer voor het installeren van een besturingssysteeminstallatiekopie [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="rhel-installation-notes"></a>Opmerkingen bij de installatie van de RHEL

* Azure Stack biedt geen ondersteuning voor VHDX-indeling. Azure ondersteunt alleen vaste VHD. U kunt Hyper-V Manager gebruiken voor de schijf converteren naar VHD-indeling of kunt u de cmdlet convert-vhd. Als u VirtualBox gebruiken, selecteert u **vaste grootte** in plaats van de standaard dynamisch toegewezen optie bij het maken van de schijf.
* Azure Stack ondersteunt alleen virtuele machines van generatie 1. U kunt een virtuele machine generatie 1 converteren van VHDX, zodat de VHD-indeling en dynamisch uit te breiden naar een schijf met vaste grootte. U kunt een virtuele machine generatie niet wijzigen. Zie voor meer informatie, [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* De maximale grootte die toegestaan voor de VHD is 1023 GB.
* Wanneer u de Linux-besturingssysteem installeert, raden wij aan dat u standaard partities in plaats van logische Volume Manager (LVM), dit is vaak de standaardwaarde voor vele installaties. Met deze procedure voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als u ooit een besturingssysteemschijf koppelen aan een andere identieke virtuele machine moet voor probleemoplossing.
* Kernel-ondersteuning voor het koppelen van bestandssystemen Universal Disk Format (UDF's) is vereist. Op de eerste keer opstarten geeft de UDF-indeling media die is gekoppeld aan de Gast de inrichtingsconfiguratie aan de virtuele Linux-machine. De Azure Linux Agent moeten de UDF-bestandssysteem om te lezen van de configuratie en het inrichten van de virtuele machine te koppelen.
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-Agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf. Meer informatie over kan worden gevonden in de volgende stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1 MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1 MB vóór de conversie is. Meer informatie vindt u in de onderstaande stappen.
* Azure Stack biedt geen ondersteuning voor cloud-init. Uw virtuele machine moet worden geconfigureerd met een ondersteunde versie van de Windows Azure Linux Agent (WALA).

### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Een RHEL 7 virtuele machine op basis van Hyper-V Manager voorbereiden

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

3. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

4. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand, en voeg de volgende tekst toe indien nodig:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

5. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

    ```sh
    # sudo systemctl enable network
    ```

6. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

    ```sh
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

7. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze wijziging `/etc/default/grub` in een teksteditor en wijzig de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Dit zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze configuratie worden ook uitgeschakeld de nieuwe naamgeving van RHEL 7 NIC's. 

   Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine. U wordt aangeraden dat u de volgende parameters:

    ```sh
    rhgb quiet crashkernel=auto
    ```

8. Als u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht om op te bouwen de grub-configuratie:

    ```sh
    # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

9. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten, dit doorgaans de standaardinstelling is. Wijzigen `/etc/ssh/sshd_config` om op te nemen in de volgende regel:

    ```sh
    ClientAliveInterval 180
    ```

10. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

    ```sh
    # subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

11. De Azure Linux-Agent installeren met de volgende opdracht:

    ```sh
    # sudo yum install WALinuxAgent
    # sudo systemctl enable waagent.service
    ```

12. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. De lokale bronschijf is een tijdelijke schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    # NOTE: set this to whatever you need it to be.
    ```

13. Als u wilt de registratie van het abonnement, moet u de volgende opdracht uitvoeren:

    ```sh
    # sudo subscription-manager unregister
    ```

14. Als u van een systeem dat is geïmplementeerd met behulp van de certificeringsinstantie van een bedrijf gebruikmaakt, vertrouwt de RHEL-virtuele machine niet de Azure Stack-basiscertificaat. U moet die in het archief Vertrouwde basiscertificeringsinstanties plaatsen. Zie [toe te voegen vertrouwde basiscertificaten op de server](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

15. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

    ```sh
    # sudo waagent -force -deprovision
    # export HISTSIZE=0
    # logout
    ```

16. Klik op **actie** > **afsluiten** in Hyper-V-beheer.

17. De VHD converteren naar een vaste grootte VHD met behulp van de 'Schijf bewerken'-functie van Hyper-V-beheer of de Convert-VHD PowerShell-opdracht. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Een Red Hat gebaseerde virtuele machine op basis van KVM voorbereiden

1. Download de KVM-afbeelding van RHEL 7 van de Red Hat-website. Deze procedure maakt gebruik van RHEL 7 als voorbeeld.

2. Een root-wachtwoord instellen.

    Een versleuteld wachtwoord genereren en kopieer de uitvoer van de opdracht:

    ```sh
    # openssl passwd -1 changeme
    ```

   Stel een hoofdwachtwoord met guestfish:

    ```sh
    # guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Wijzigen van de tweede veld van de hoofdgebruiker van "." in het versleutelde wachtwoord.

3. Maak een virtuele machine in KVM vanuit de installatiekopie van het qcow2. Het schijftype instellen op **qcow2**, en stelt u het virtuele netwerkinterface-Apparaatmodel op **virtio**. Vervolgens start u de virtuele machine en meld u aan als hoofdgebruiker.

4. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

5. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

6. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

    ```sh
    # sudo systemctl enable network
    ```

7. Registreer uw Red Hat-abonnement om in te schakelen van installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

    ```sh
    # subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

8. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze configuratie `/etc/default/grub` in een teksteditor en wijzig de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Met deze opdracht zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. De opdracht ook schakelt de nieuwe naamgeving van RHEL 7 voor NIC 's

   Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Deze parameter wordt de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine. U wordt aangeraden dat u de volgende parameters:

    ```sh
    rhgb quiet crashkernel=auto
    ```

9. Als u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht om op te bouwen de grub-configuratie:

    ```sh
    # grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

10. Hyper-V-modules in initramfs toevoegen.

    Bewerken `/etc/dracut.conf` en voeg inhoud toe:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Opnieuw opbouwen initramfs:

    ```sh
    # dracut -f -v
    ```

11. Cloud-init verwijderen:

    ```sh
    # yum remove cloud-init
    ```

12. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten:

    ```sh
    # systemctl enable sshd
    ```

    Wijzig /etc/ssh/sshd_config om op te nemen van de volgende regels:

    ```sh
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

13. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

    ```sh
    # subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

14. De Azure Linux-Agent installeren met de volgende opdracht:

    ```sh
    # yum install WALinuxAgent
    ```

    De service waagent inschakelen:

    ```sh
    # systemctl enable waagent.service
    ```

15. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. De lokale bronschijf is een tijdelijke schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    # NOTE: set this to whatever you need it to be.
    ```

16. Het abonnement (indien nodig) registratie met de volgende opdracht:

    ```sh
    # subscription-manager unregister
    ```

17. Als u van een systeem dat is geïmplementeerd met behulp van de certificeringsinstantie van een bedrijf gebruikmaakt, vertrouwt de RHEL-virtuele machine niet de Azure Stack-basiscertificaat. U moet die in het archief Vertrouwde basiscertificeringsinstanties plaatsen. Zie [toe te voegen vertrouwde basiscertificaten op de server](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

18. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

    ```sh
    # sudo waagent -force -deprovision
    # export HISTSIZE=0
    # logout
    ```

19. De virtuele machine in KVM afgesloten.

20. De afbeelding qcow2 naar de VHD-indeling converteren.

    > [!NOTE]
    > Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie: https://bugs.launchpad.net/qemu/+bug/1490611.

    De installatiekopie eerst converteren naar onbewerkte indeling:

    ```sh
    # qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Zorg ervoor dat de grootte van de installatiekopie van het onbewerkte wordt uitgelijnd met 1 MB. Anders ronden de grootte aan te passen aan 1 MB:

    ```sh
    # MB=$((1024*1024))
    # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    # rounded_size=$((($size/$MB + 1)*$MB))
    # qemu-img resize rhel-7.4.raw $rounded_size
    ```

    De onbewerkte schijf converteren naar een VHD met vaste grootte en:

    ```sh
    # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Of, voor qemu versie **2.6 +** bevatten de `force_size` optie:

    ```sh
    # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Een Red Hat gebaseerde virtuele machine van VMware voorbereiden

In deze sectie wordt ervan uitgegaan dat u een virtuele RHEL-machine in VMware al hebt geïnstalleerd. Zie voor meer informatie over het installeren van een besturingssysteem in VMware [installatiehandleiding voor VMware Gast-besturingssysteem](http://partnerweb.vmware.com/GOSIG/home.html).

* Wanneer u de Linux-besturingssysteem installeert, raden wij aan dat u standaard partities in plaats van LVM, dit is vaak de standaardwaarde voor vele installaties. Hiermee voorkomt u LVM naam conflicteert met de gekloonde virtuele machine, met name als de schijf van een besturingssysteem ooit worden gekoppeld aan een andere virtuele machine moet voor probleemoplossing. LVM of RAID kan op gegevensschijven worden gebruikt als de voorkeur.
* Stel een swap-partitie niet op de besturingssysteemschijf. U kunt de Linux-agent voor het maken van een wisselbestand op de tijdelijke schijf configureren. Meer informatie hierover vindt u in de volgende stappen.
* Wanneer u de virtuele harde schijf maakt, selecteert u **Store virtuele schijf als een enkel bestand**.

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Een RHEL 7 virtuele machine van VMware voorbereiden

1. Maak of bewerk de `/etc/sysconfig/network` bestand en voeg de volgende tekst toe:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

2. Maak of bewerk de `/etc/sysconfig/network-scripts/ifcfg-eth0` bestand en voeg de volgende tekst toe:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

3. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

    ```sh
    # sudo chkconfig network on
    ```

4. Registreer uw Red Hat-abonnement om in te schakelen van de installatie van pakketten van de RHEL-opslagplaats met de volgende opdracht:

    ```sh
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

5. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Open hiervoor deze wijziging `/etc/default/grub` in een teksteditor en wijzig de `GRUB_CMDLINE_LINUX` parameter. Bijvoorbeeld:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

    Deze configuratie zorgt er ook voor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe naamgeving van RHEL 7 voor NIC's. Bovendien is het raadzaam dat u de volgende parameters:

    ```sh
    rhgb quiet crashkernel=auto
    ```

    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. U kunt laten de `crashkernel` geconfigureerd als de gewenste optie. Houd er rekening mee dat deze parameter vermindert de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op kleinere virtuele machine.

6. Als u klaar bent bewerken `/etc/default/grub`, voer de volgende opdracht om op te bouwen de grub-configuratie:

    ```sh
    # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

7. Hyper-V-modules aan initramfs toevoegen.

    Bewerken `/etc/dracut.conf`, Voeg inhoud toe:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Opnieuw opbouwen initramfs:

    ```sh
    # dracut -f -v
    ```

8. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is doorgaans de standaardinstelling. Wijzigen `/etc/ssh/sshd_config` om op te nemen in de volgende regel:

    ```sh
    ClientAliveInterval 180
    ```

9. Het pakket WALinuxAgent `WALinuxAgent-<version>`, naar de opslagplaats van Red Hat extra's is gepusht. De opslagplaats extra's inschakelen met de volgende opdracht:

    ```sh
    # subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

10. De Azure Linux-Agent installeren met de volgende opdracht:

    ```sh
    # sudo yum install WALinuxAgent
    # sudo systemctl enable waagent.service
    ```

11. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte automatisch configureren met behulp van de schijf van de lokale resource die is gekoppeld aan de virtuele machine nadat de virtuele machine is ingericht op Azure. Houd er rekening mee dat de lokale bronschijf een tijdelijke schijf is en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Nadat u de Azure Linux Agent in de vorige stap hebt geïnstalleerd, wijzigt u de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    # NOTE: set this to whatever you need it to be.
    ```

12. Als u wilt de registratie van het abonnement, moet u de volgende opdracht uitvoeren:

    ```sh
    # sudo subscription-manager unregister
    ```

13. Als u van een systeem dat is geïmplementeerd met behulp van de certificeringsinstantie van een bedrijf gebruikmaakt, vertrouwt de RHEL-virtuele machine niet de Azure Stack-basiscertificaat. U moet die in het archief Vertrouwde basiscertificeringsinstanties plaatsen. Zie [toe te voegen vertrouwde basiscertificaten op de server](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

14. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

    ```sh
    # sudo waagent -force -deprovision
    # export HISTSIZE=0
    # logout
    ```

15. Sluit de virtuele machine en het VMDK-bestand converteren naar de VHD-indeling.

    > [!NOTE]
    > Er is een bekend probleem in qemu img versies > = 2.2.1 die in een onjuiste indeling VHD resulteert. Het probleem is opgelost in QEMU 2.6. Het verdient aanbeveling qemu-img 2.2.0 of lager gebruiken, of bijwerken op 2.6 of hoger. Naslaginformatie over: <https://bugs.launchpad.net/qemu/+bug/1490611.>

    De installatiekopie eerst converteren naar onbewerkte indeling:

    ```sh
    # qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Zorg ervoor dat de grootte van de installatiekopie van het onbewerkte wordt uitgelijnd met 1 MB. Anders ronden de grootte aan te passen aan 1 MB:

    ```sh
    # MB=$((1024*1024))
    # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    # rounded_size=$((($size/$MB + 1)*$MB))
    # qemu-img resize rhel-7.4.raw $rounded_size
    ```

    De onbewerkte schijf converteren naar een VHD met vaste grootte en:

    ```sh
    # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Of, voor qemu versie **2.6 +** bevatten de `force_size` optie:

    ```sh
    # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Een Red Hat gebaseerde virtuele machine op basis van een ISO met behulp van een bestand kickstart automatisch voorbereiden

1. Maak een bestand kickstart met de volgende inhoud en sla het bestand. Zie voor meer informatie over de installatie van kickstart de [Kickstart installatiehandleiding](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

    ```sh
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
    ```

2. Plaats het bestand kickstart waar het installatie-systeem kunt openen.

3. Maak een nieuwe virtuele machine in Hyper-V-beheer. Op de **virtuele hardeschijf aansluiten** weergeeft, schakelt **later een virtuele harde schijf koppelen**, en voltooi de Wizard Nieuwe virtuele Machine.

4. Open de instellingen van de virtuele machine:

    a. Een nieuwe virtuele harde schijf koppelen aan de virtuele machine. Zorg ervoor dat u selecteert **VHD-indeling** en **vaste grootte**.

    b. De ISO-installatie aan het DVD-station koppelen.

    c. Stel het BIOS om op te starten vanaf de CD.

5. Hiermee wordt de virtuele machine gestart. Wanneer de installatiehandleiding wordt weergegeven, drukt u op **tabblad** het configureren van de opstartopties.

6. ENTER `inst.ks=<the location of the kickstart file>` aan het einde van de opstartopties en druk op **Enter**.

7. Wacht tot de installatie is voltooid. Wanneer deze voltooid, wordt de virtuele machine automatisch afgesloten. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="known-issues"></a>Bekende problemen

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>De Hyper-V-stuurprogramma kan niet worden opgenomen in de eerste RAM-schijf bij het gebruik van een niet-Hyper-V-hypervisor

In sommige gevallen, Linux-installatieprogramma's mogelijk niet bevatten de stuurprogramma's voor Hyper-V in de eerste RAM-schijf (initrd of initramfs) tenzij Linux wordt gedetecteerd dat deze wordt uitgevoerd in een omgeving met Hyper-V.

Wanneer u een andere virtualisatiesysteem (dat wil zeggen, Virtualbox, Xen, enzovoort) gebruikt voor het voorbereiden van uw Linux-installatiekopie, moet u mogelijk opnieuw opbouwen initrd om ervoor te zorgen dat ten minste de hv_vmbus en hv_storvsc kernel-modules zijn beschikbaar op de eerste RAM-schijf. Dit is een bekend probleem ten minste op systemen die zijn gebaseerd op de upstream Red Hat-distributie.

U lost dit probleem, Hyper-V-modules toevoegen aan initramfs en deze opnieuw bouwen:

Bewerken `/etc/dracut.conf`, en voeg de volgende inhoud:

```sh
add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
```

Opnieuw opbouwen initramfs:

```sh
# dracut -f -v
```

Zie voor meer informatie, [opnieuw opbouwen van initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Volgende stappen

U kunt nu uw Red Hat Enterprise Linux virtuele harde schijf gebruiken om te maken van nieuwe virtuele machines in Azure Stack. Als dit de eerste keer dat u de VHD-bestand naar Azure Stack uploadt, Zie [gebruik van de toolkit Marketplace maken en publiceren van marketplace-items](azure-stack-marketplace-publisher.md).

Zie voor meer informatie over de hypervisors die zijn gecertificeerd voor Red Hat Enterprise Linux, [de Red Hat-website](https://access.redhat.com/certified-hypervisors).

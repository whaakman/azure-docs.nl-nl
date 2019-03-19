---
title: Een Linux CentOS gebaseerde VHD in Azure maken en uploaden
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een op basis van CentOS Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: szark
ms.openlocfilehash: 4e32d2357636cb488d3a58b78b025860da3f74c4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091355"
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Een op CentOS gebaseerde virtuele Azure-machine voorbereiden

* [Een CentOS 6.x virtuele machine voor Azure voorbereiden](#centos-6x)
* [Een CentOS 7.0 + virtuele machine voor Azure voorbereiden](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al een CentOS hebt geïnstalleerd (of vergelijkbaar afgeleide) Linux-besturingssysteem op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van VHD-bestanden, bijvoorbeeld een oplossing voor netwerkvirtualisatie zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](https://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie van centOS**

* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips voor Linux voorbereiden voor Azure.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd. Als u met behulp van VirtualBox betekent dit dat selecteren **vaste grootte** in plaats van de standaard dynamisch toegewezen bij het maken van de schijf.
* Bij het installeren van de Linux-systeem is *aanbevolen* dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit zal LVM naam conflicteert met de gekloonde virtuele machines, voorkomen dat met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere identieke virtuele machine moet voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt.
* Kernel-ondersteuning voor het koppelen van bestandssystemen UDF is vereist. De configuratie van de inrichting wordt op de eerste keer opstarten op Azure via media UDF-indeling die is gekoppeld aan de Gast doorgegeven aan de Linux-VM. De Azure Linux-agent moet u mogelijk de UDF-bestandssysteem installeren om te lezen van de configuratie en de virtuele machine inricht.
* Linux-kernel-versies hieronder 2.6.37 ondersteunen geen NUMA op Hyper-V met grotere VM-grootten. Dit probleem voornamelijk gevolgen oudere distributies met behulp van de upstream Red Hat 2.6.32 kernel, en is vastgesteld in RHEL 6.6 (kernel-2.6.32-504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of op basis van RHEL kernels die ouder zijn dan 2.6.32-504 moet de parameter opstarten `numa=off` op de opdrachtregel in grub.conf kernel. Zie voor meer informatie Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt in de onderstaande stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Zie [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer informatie.

## <a name="centos-6x"></a>CentOS 6.x

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

3. CentOS 6, NetworkManager kan leiden tot problemen met de Azure Linux agent. Dit pakket verwijderen door de volgende opdracht uit:

        # sudo rpm -e --nodeps NetworkManager

4. Maak of bewerk het bestand `/etc/sysconfig/network` en voeg de volgende tekst toe:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Maak of bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende tekst toe:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:

        # sudo chkconfig network on

8. Als u wilt gebruiken de OpenLogic mirrors die worden gehost in de Azure-datacenters en vervangt u vervolgens de `/etc/yum.repos.d/CentOS-Base.repo` -bestand met de volgende opslagplaatsen.  Hierdoor wordt ook toegevoegd de **[openlogic]** opslagplaats met extra pakketten, zoals de Azure Linux-agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    > [!Note]
    > De rest van deze handleiding wordt ervan uitgegaan u ten minste de `[openlogic]` opslagplaats, die wordt gebruikt om de onderstaande Azure Linux-agent te installeren.

9. Voeg de volgende regel toe /etc/yum.conf:

        http_caching=packages

10. Voer de volgende opdracht uit om te wissen van de huidige yum-metagegevens en bijwerken van het systeem met de meest recente pakketten:

        # yum clean all

    Tenzij u een installatiekopie voor een oudere versie van CentOS maakt, wordt het aanbevolen om bij te werken van alle pakketten naar de nieuwste versie:

        # sudo yum -y update

    Mogelijk opnieuw worden opgestart na het uitvoeren van deze opdracht zijn vereist.

11. (Optioneel) De stuurprogramma's voor de Linux Integration Services (LIS) installeren.

    > [!IMPORTANT]
    > De stap is het **vereist** voor CentOS 6.3 en lager en optioneel voor latere versies.

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    U kunt ook de handmatige installatie-instructies volgen op de [LIS downloadpagina](https://go.microsoft.com/fwlink/?linkid=403033) voor het installeren van de RPM naar uw virtuele machine.

12. Installeer de Azure Linux Agent en de afhankelijkheden:

        # sudo yum install python-pyasn1 WALinuxAgent

    Het pakket WALinuxAgent verwijdert u de NetworkManager en NetworkManager gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 3.

13. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. U doet dit door open `/boot/grub/menu.lst` in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.

    Naast de bovenstaande, verdient het *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort.  De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar houd er rekening mee dat deze parameter wordt verminderen de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op de kleinere VM-grootten.

    > [!Important]
    > CentOS 6.5 en eerdere moeten ook instellen voor de kernel-parameter `numa=off`. Zie Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

14. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.

15. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Houd er rekening mee dat de lokale bronschijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), wijzigen van de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klik op **actie-Afsluiten omlaag >** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

- - -

## <a name="centos-70"></a>CentOS 7.0+

**Wijzigingen in CentOS 7 (en vergelijkbare producten)**

Een CentOS 7 virtuele machine voorbereiden voor Azure is heel vergelijkbaar met CentOS 6, maar er zijn enkele belangrijke verschillen vermelden waard:

* Het pakket NetworkManager wordt niet meer conflicteert met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en het is raadzaam dat het niet is verwijderd.
* GRUB2 wordt nu gebruikt als de bootloader standaard, zodat de procedure voor het bewerken van parameters van de kernel is gewijzigd (Zie hieronder).
* XFS is nu het standaardbestandssysteem. Het bestandssysteem ext4 kan nog steeds worden gebruikt, indien gewenst.

**Configuratiestappen**

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

3. Maak of bewerk het bestand `/etc/sysconfig/network` en voeg de volgende tekst toe:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Maak of bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende tekst toe:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Als u wilt gebruiken de OpenLogic mirrors die worden gehost in de Azure-datacenters en vervangt u vervolgens de `/etc/yum.repos.d/CentOS-Base.repo` -bestand met de volgende opslagplaatsen.  Hierdoor wordt ook toegevoegd de **[openlogic]** opslagplaats met inbegrip van pakketten voor de Azure Linux-agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    > [!Note]
    > De rest van deze handleiding wordt ervan uitgegaan u ten minste de `[openlogic]` opslagplaats, die wordt gebruikt om de onderstaande Azure Linux-agent te installeren.

7. Voer de volgende opdracht uit om te wissen van de huidige yum-metagegevens en installeert u de updates:

        # sudo yum clean all

    Tenzij u een installatiekopie voor een oudere versie van CentOS maakt, wordt het aanbevolen om bij te werken van alle pakketten naar de nieuwste versie:

        # sudo yum -y update

    Een opnieuw opstarten is mogelijk vereist na het uitvoeren van deze opdracht.

8. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. U doet dit door open `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

   Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe naamgeving van CentOS 7 voor NIC's. Naast de bovenstaande, verdient het *verwijderen* de volgende parameters:

        rhgb quiet crashkernel=auto

    Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort. De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar houd er rekening mee dat deze parameter wordt verminderen de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op de kleinere VM-grootten.

9. Wanneer u klaar bent bewerken `/etc/default/grub` per hierboven, voer de volgende opdracht om op te bouwen de grub-configuratie:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. Als het opbouwen van de installatiekopie van **VMware, VirtualBox of KVM:** Zorg ervoor dat de Hyper-V-stuurprogramma's zijn opgenomen in de initramfs:

    Bewerken `/etc/dracut.conf`, Voeg inhoud toe:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    De initramfs opnieuw:

        # sudo dracut -f -v

11. Installeer de Azure Linux Agent en de afhankelijkheden:

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. Maak geen wisselruimte op de besturingssysteemschijf.

    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Houd er rekening mee dat de lokale bronschijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), wijzigen van de volgende parameters in `/etc/waagent.conf` op de juiste wijze:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klik op **actie-Afsluiten omlaag >** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar voor gebruik van de virtuele harde schijf van CentOS Linux te maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).

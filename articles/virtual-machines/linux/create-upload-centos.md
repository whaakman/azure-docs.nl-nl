---
title: Maken en uploaden van een op basis van CentOS Linux VHD in Azure
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) die een besturingssysteem op basis van CentOS Linux bevat.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: f649067590dc990c962aa0c9df8c76080fc2a0b8
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Een op CentOS gebaseerde virtuele Azure-machine voorbereiden
* [Een CentOS 6.x virtuele machine voorbereiden voor Azure](#centos-6x)
* [Een CentOS 7.0 + virtuele machine voorbereiden voor Azure](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al een CentOS hebt geïnstalleerd (of vergelijkbaar afgeleide) Linux-besturingssysteem naar een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van de VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie van centOS**

* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips over Linux voorbereiden voor Azure.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd. Als u van VirtualBox gebruikmaakt betekent dit dat selecteren **een vaste grootte** in plaats van de standaard dynamisch toegewezen bij het maken van de schijf.
* Bij het installeren van de Linux-systeem is *aanbevolen* dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere identieke virtuele machine moet voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mag worden gebruikt voor gegevensschijven.
* Kernel-ondersteuning voor het koppelen van UDF-bestandssysteem is vereist. De configuratie van de inrichting wordt op de eerste keer opstarten op Azure via media UDF-indeling die is gekoppeld aan de Gast doorgegeven aan de Linux-VM. De Azure Linux-agent moet kunnen de UDF-bestandssysteem voor het lezen van de configuratie en inrichten van de virtuele machine te koppelen.
* Kernel-versies van Linux onder 2.6.37 ondersteunen geen NUMA op Hyper-V met grotere virtuele machine. Dit probleem voornamelijk effecten oudere distributies met behulp van de upstream Red Hat 2.6.32 kernel, en is vastgesteld in RHEL 6.6 (kernel 2.6.32 504). Systemen met aangepaste kernels die ouder zijn dan 2.6.37 of op basis van RHEL kernels die ouder zijn dan 2.6.32-504 moet de parameter opstarten ingesteld `numa=off` op de opdrachtregel in grub.conf kernel. Zie voor meer informatie Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Configureer een partitie van de wisseling niet op de schijf met het besturingssysteem. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle van de VHD's moeten grootten die veelvouden van 1 MB hebben.

## <a name="centos-6x"></a>CentOS 6.x

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

3. In CentOS 6, NetworkManager kan leiden tot problemen met de Azure Linux-agent. Dit pakket verwijderen met de volgende opdracht:
   
        # sudo rpm -e --nodeps NetworkManager

4. Maak of bewerk het bestand `/etc/sysconfig/network` en voeg de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Maak of bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure- of Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Zorg ervoor dat de netwerkservice tijdens het opstarten wordt gestart met de volgende opdracht:
   
        # sudo chkconfig network on

8. Als u gebruiken wilt aan de OpenLogic mirrors die worden gehost in de Azure-datacenters en vervangt u vervolgens de `/etc/yum.repos.d/CentOS-Base.repo` bestand met de volgende opslagplaatsen.  Hierdoor wordt ook toegevoegd de **[openlogic]** opslagplaats met extra pakketten, zoals de Azure Linux-agent:

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

    >[!Note]
    De rest van deze handleiding wordt wordt ervan uitgegaan dat u ten minste de `[openlogic]` opslagplaats die wordt gebruikt voor het installeren van de onderstaande Azure Linux-agent.


9. Voeg de volgende regel toe /etc/yum.conf:
    
        http_caching=packages

10. Voer de volgende opdracht om te wissen van de huidige yum metagegevens en bijwerken van het systeem met de meest recente pakketten:
    
        # yum clean all

    Tenzij u een afbeelding voor een oudere versie van CentOS maakt, wordt u aangeraden om bij te werken van alle pakketten naar de meest recente:

        # sudo yum -y update

    Mogelijk opnieuw worden opgestart na het uitvoeren van deze opdracht zijn vereist.

11. (Optioneel) De stuurprogramma's voor de Linux Integration Services (LIS) installeren.
   
    >[!IMPORTANT]
    De stap is het **vereist** voor CentOS 6.3 en lager en optioneel voor latere versies.

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    U kunt ook de handmatige installatie-instructies volgen op de [LIS downloadpagina](https://go.microsoft.com/fwlink/?linkid=403033) voor het installeren van de RPM naar uw virtuele machine.
 
12. Installeer de Azure Linux Agent en de afhankelijkheden:
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    Het pakket WALinuxAgent verwijdert de NetworkManager en NetworkManager gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 3.


13. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Open hiervoor `/boot/grub/menu.lst` in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.
    
    Naast de bovenstaande, wordt u aangeraden te *verwijderen* de volgende parameters:
    
        rhgb quiet crashkernel=auto
    
    Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort.  De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar dat deze parameter wordt Verklein de hoeveelheid beschikbaar geheugen in de virtuele machine 128 MB of meer, die mogelijk problemen op de VM-kleinere opmerking.

    >[!Important]
    CentOS 6.5 en eerdere moeten ook de parameter kernel instellen `numa=off`. Red Hat Zie [KB 436883](https://access.redhat.com/solutions/436883).

14. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.

15. Maak geen wisselruimte op de schijf met het besturingssysteem.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), wijzig de volgende parameters in `/etc/waagent.conf` op de juiste wijze:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klik op **actie-Afsluiten > omlaag** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.


- - -
## <a name="centos-70"></a>CentOS 7.0+
**Wijzigingen in CentOS 7 (en vergelijkbare afgeleiden)**

Een CentOS 7 virtuele machine voorbereiden voor Azure is vergelijkbaar met CentOS 6, maar er zijn echter enkele belangrijke verschillen opgemerkt:

* Het pakket NetworkManager wordt niet langer strijdig met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en wordt aangeraden deze niet is verwijderd.
* GRUB2 wordt nu gebruikt als de standaard bootloader, zodat de procedure voor het bewerken van de kernel parameters is gewijzigd (Zie hieronder).
* XFS is nu het standaardbestandssysteem. Het bestandssysteem ext4 kan nog steeds worden gebruikt, indien gewenst.

**Configuratiestappen**

1. Selecteer de virtuele machine in Hyper-V-beheer.

2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.

3. Maak of bewerk het bestand `/etc/sysconfig/network` en voeg de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Maak of bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure- of Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Als u gebruiken wilt aan de OpenLogic mirrors die worden gehost in de Azure-datacenters en vervangt u vervolgens de `/etc/yum.repos.d/CentOS-Base.repo` bestand met de volgende opslagplaatsen.  Hierdoor wordt ook toegevoegd de **[openlogic]** opslagplaats met pakketten voor de Azure Linux-agent:
   
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

    >[!Note]
    De rest van deze handleiding wordt wordt ervan uitgegaan dat u ten minste de `[openlogic]` opslagplaats die wordt gebruikt voor het installeren van de onderstaande Azure Linux-agent.

7. Voer de volgende opdracht om te wissen van de metagegevens van de huidige yum en geen updates installeren:
   
        # sudo yum clean all

    Tenzij u een afbeelding voor een oudere versie van CentOS maakt, wordt u aangeraden om bij te werken van alle pakketten naar de meest recente:

        # sudo yum -y update

    Een opnieuw opstarten is mogelijk vereist na het uitvoeren van deze opdracht.

8. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Open hiervoor `/etc/default/grub` in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe CentOS 7 naamconventies voor NIC's. Naast de bovenstaande, wordt u aangeraden te *verwijderen* de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
    Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort. De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar dat deze parameter wordt Verklein de hoeveelheid beschikbaar geheugen in de virtuele machine 128 MB of meer, die mogelijk problemen op de VM-kleinere opmerking.

9. Wanneer u klaar bent bewerken `/etc/default/grub` per hierboven, voer de volgende opdracht voor het opnieuw samenstellen van de configuratie van de grub:
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. Als het bouwen van een installatiekopie van **VMWare, VirtualBox of KVM:** Zorg ervoor dat de Hyper-V-stuurprogramma's zijn opgenomen in de initramfs:
   
   Bewerken `/etc/dracut.conf`, inhoud toevoegen:
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   De initramfs bouwen:
   
        # sudo dracut –f -v

11. Installeer de Azure Linux Agent en de afhankelijkheden:

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. Maak geen wisselruimte op de schijf met het besturingssysteem.
   
   De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), wijzig de volgende parameters in `/etc/waagent.conf` op de juiste wijze:
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klik op **actie-Afsluiten > omlaag** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de virtuele harde schijf van CentOS Linux maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM te maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).


---
title: Maken en uploaden van een VHD van Oracle Linux | Microsoft Docs
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een Oracle Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: szark
ms.openlocfilehash: c631ddf3acf6df7364c03eb4691b78be0493e0d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Een virtuele Oracle Linux-machine voor Azure voorbereiden
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al een Oracle Linux-besturingssysteem hebt geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van de VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Opmerkingen bij de installatie van de Oracle Linux
* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips over Linux voorbereiden voor Azure.
* Oracle van Red Hat compatibel kernel en hun UEK3 (Unbreakable Enterprise Kernel) worden beide ondersteund op Hyper-V en Azure. Voor de beste resultaten u wordt aangeraden om bij te werken naar de meest recente kernel tijdens het voorbereiden van uw Oracle Linux-VHD.
* UEK2 van Oracle wordt niet ondersteund op Hyper-V en Azure als u beschikt niet over de vereiste stuurprogramma's.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd.
* Bij het installeren van de Linux-systeem wordt het aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mag worden gebruikt voor gegevensschijven als voorkeur.
* NUMA wordt niet ondersteund voor grotere virtuele machine vanwege een fout in de kernel-versies van Linux onder 2.6.37. Dit probleem hoofdzakelijk van invloed is op distributies met behulp van de upstream Red Hat 2.6.32 kernel. Handmatige installatie van de Azure Linux-agent (waagent) wordt automatisch NUMA uitgeschakeld in de configuratie GRUB voor de Linux-kernel. Meer informatie hierover vindt u in de volgende stappen uit.
* Configureer een partitie van de wisseling niet op de schijf met het besturingssysteem. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle van de VHD's moeten grootten die veelvouden van 1 MB hebben.
* Zorg ervoor dat de `Addons` opslagplaats is ingeschakeld. Bewerk het bestand `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), en wijzigt u de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
U moet specifieke configuratiestappen in het besturingssysteem voor de virtuele machine in Azure uit te voeren.

1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.
2. Klik op **Connect** om het venster voor de virtuele machine te openen.
3. NetworkManager verwijderen met de volgende opdracht:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Opmerking:** als het pakket nog niet is geïnstalleerd, deze opdracht mislukt met een foutbericht weergegeven. Dit is normaal.
4. Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` map waarin zich de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Maak een bestand met de naam **ifcfg eth0** in de `/etc/sysconfig/network-scripts/` map waarin zich de volgende tekst:
   
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
   
        # chkconfig network on
8. Python-pyasn1 installeren met de volgende opdracht:
   
        # sudo yum install python-pyasn1
9. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Deze open doen ' / boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Hiermee wordt het NUMA uitgeschakeld vanwege een fout in Oracle van Red Hat compatibel kernel.
   
   Naast de bovenstaande, wordt u aangeraden te *verwijderen* de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
   Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort.
   
   De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar dat deze parameter wordt Verklein de hoeveelheid beschikbaar geheugen in de virtuele machine 128 MB of meer, die mogelijk problemen op de VM-kleinere opmerking.
10. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
11. Installeer de Azure Linux Agent door de volgende opdracht uit te voeren. De meest recente versie is 2.0.15.
    
        # sudo yum install WALinuxAgent
    
    Houd er rekening mee dat de installatie van het pakket WALinuxAgent de NetworkManager verwijdert en NetworkManager gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 2.
12. Maak geen wisselruimte op de schijf met het besturingssysteem.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
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

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Wijzigingen in Oracle Linux 7**

Een virtuele machine van de Oracle Linux 7 voorbereiden voor Azure is vergelijkbaar met Oracle Linux 6, maar er zijn echter enkele belangrijke verschillen opgemerkt:

* Zowel de Red Hat compatibel kernel van Oracle UEK3 worden ondersteund in Azure.  De kernel UEK3 wordt aanbevolen.
* Het pakket NetworkManager wordt niet langer strijdig met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en wordt aangeraden deze niet is verwijderd.
* GRUB2 wordt nu gebruikt als de standaard bootloader, zodat de procedure voor het bewerken van de kernel parameters is gewijzigd (Zie hieronder).
* XFS is nu het standaardbestandssysteem. Het bestandssysteem ext4 kan nog steeds worden gebruikt, indien gewenst.

**Configuratiestappen**

1. Selecteer de virtuele machine in Hyper-V-beheer.
2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.
3. Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` map waarin zich de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Maak een bestand met de naam **ifcfg eth0** in de `/etc/sysconfig/network-scripts/` map waarin zich de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure- of Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Zorg ervoor dat de netwerkservice tijdens het opstarten wordt gestart met de volgende opdracht:
   
        # sudo chkconfig network on
7. Installeer het pakket python pyasn1 met de volgende opdracht:
   
        # sudo yum install python-pyasn1
8. Voer de volgende opdracht om te wissen van de metagegevens van de huidige yum en geen updates installeren:
   
        # sudo yum clean all
        # sudo yum -y update
9. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Om dit te doen open '/ standaard/etc/grub' in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe indicatie 7 naamconventies voor NIC's. Naast de bovenstaande, wordt u aangeraden te *verwijderen* de volgende parameters:
   
       rhgb quiet crashkernel=auto
   
   Grafische en stil opstarten zijn niet handig in een omgeving waar we de logboeken worden verzonden naar de seriële poort.
   
   De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar dat deze parameter wordt Verklein de hoeveelheid beschikbaar geheugen in de virtuele machine 128 MB of meer, die mogelijk problemen op de VM-kleinere opmerking.
10. Wanneer u klaar bent editing '/ standaard/etc/grub' per hierboven, voer de volgende opdracht voor het opnieuw samenstellen van de configuratie van de grub:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
12. Installeer de Azure Linux Agent met de volgende opdracht:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. Maak geen wisselruimte op de schijf met het besturingssysteem.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Klik op **actie-Afsluiten > omlaag** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van uw VHD Oracle Linux maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie de stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).


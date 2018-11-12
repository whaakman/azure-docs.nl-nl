---
title: Een Oracle Linux-VHD maken en uploaden | Microsoft Docs
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een Oracle Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: c2c02283518bab0723b7bc815f034c4324c944e1
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232879"
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Een virtuele Oracle Linux-machine voor Azure voorbereiden
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt een Oracle Linux-besturingssysteem geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van VHD-bestanden, bijvoorbeeld een oplossing voor netwerkvirtualisatie zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Opmerkingen bij de installatie van de Oracle Linux
* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips voor Linux voorbereiden voor Azure.
* Het Oracle Red Hat compatibele kernel en hun UEK3 (Unbreakable Enterprise Kernel) worden beide ondersteund op Hyper-V en Azure. Voor optimale resultaten, moet u ervoor dat u bijwerkt naar de meest recente kernel tijdens het voorbereiden van uw Oracle Linux-VHD.
* UEK2 van Oracle wordt niet ondersteund op Hyper-V en Azure, omdat het omvat niet de vereiste stuurprogramma's.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd.
* Bij het installeren van de Linux-systeem is het raadzaam dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit zal LVM naam conflicteert met de gekloonde virtuele machines, voorkomen dat met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt als de voorkeur.
* NUMA wordt niet ondersteund voor grotere VM-grootten vanwege een fout in Linux-kernel-versies hieronder 2.6.37. Dit probleem voornamelijk gevolgen heeft voor distributies die gebruikmaken van de upstream Red Hat 2.6.32 kernel. Handmatige installatie van de Azure Linux-agent (waagent) wordt automatisch NUMA uitgeschakeld in de GRUB-configuratie voor de Linux-kernel. Meer informatie hierover vindt in de onderstaande stappen.
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt in de onderstaande stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Zie [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer informatie.
* Zorg ervoor dat de `Addons` opslagplaats is ingeschakeld. Bewerk het bestand `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), en wijzigt u de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in dit bestand.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
U moet specifieke configuratiestappen in het besturingssysteem voor de virtuele machine om uit te voeren in Azure uitvoeren.

1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.
2. Klik op **Connect** om het venster voor de virtuele machine te openen.
3. NetworkManager verwijderen door de volgende opdracht uit:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Opmerking:** als het pakket niet al is geïnstalleerd, met deze opdracht mislukt met een foutbericht weergegeven. Dit is normaal gedrag.
4. Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` map met de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Maak een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` map met de volgende tekst:
   
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
   
        # chkconfig network on
8. Python pyasn1 installeren met de volgende opdracht:
   
        # sudo yum install python-pyasn1
9. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Deze open doen "/ boot/grub/menu.lst" in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Hiermee wordt het NUMA uitgeschakeld vanwege een fout in Oracle van Red Hat compatibele kernel.
   
   Naast de bovenstaande, verdient het *verwijderen* de volgende parameters:
   
        rhgb quiet crashkernel=auto
   
   Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort.
   
   De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar houd er rekening mee dat deze parameter wordt verminderen de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op de kleinere VM-grootten.
10. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
11. De Azure Linux-Agent installeren met de volgende opdracht. De meest recente versie is 2.0.15.
    
        # sudo yum install WALinuxAgent
    
    Houd er rekening mee dat u het pakket WALinuxAgent installeert de NetworkManager wordt verwijderd en NetworkManager gnome pakketten als ze zijn niet al verwijderd zoals beschreven in stap 2.
12. Maak geen wisselruimte op de besturingssysteemschijf.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Houd er rekening mee dat de lokale bronschijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
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

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Wijzigingen in Oracle Linux 7**

Een virtuele Oracle Linux 7-machine voorbereiden voor Azure is heel vergelijkbaar met Oracle Linux 6, maar er zijn enkele belangrijke verschillen vermelden waard:

* Zowel de Red Hat compatibele kernel en UEK3 van Oracle worden ondersteund in Azure.  De kernel UEK3 wordt aanbevolen.
* Het pakket NetworkManager wordt niet meer conflicteert met de Azure Linux-agent. Dit pakket wordt standaard geïnstalleerd en het is raadzaam dat het niet is verwijderd.
* GRUB2 wordt nu gebruikt als de bootloader standaard, zodat de procedure voor het bewerken van parameters van de kernel is gewijzigd (Zie hieronder).
* XFS is nu het standaardbestandssysteem. Het bestandssysteem ext4 kan nog steeds worden gebruikt, indien gewenst.

**Configuratiestappen**

1. Selecteer de virtuele machine in Hyper-V-beheer.
2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.
3. Maak een bestand met de naam **netwerk** in de `/etc/sysconfig/` map met de volgende tekst:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Maak een bestand met de naam **ifcfg-eth0** in de `/etc/sysconfig/network-scripts/` map met de volgende tekst:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Zorg ervoor dat de netwerkservice wordt gestart bij het opstarten met de volgende opdracht:
   
        # sudo chkconfig network on
7. Installeer de python pyasn1-pakket met de volgende opdracht:
   
        # sudo yum install python-pyasn1
8. Voer de volgende opdracht uit om te wissen van de huidige yum-metagegevens en installeert u de updates:
   
        # sudo yum clean all
        # sudo yum -y update
9. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Om dit te doen open '/ standaard/etc/wormgaten' in een teksteditor en bewerk de `GRUB_CMDLINE_LINUX` parameter, bijvoorbeeld:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Hiermee zorgt u ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Deze ook schakelt u de nieuwe naamgeving van de indicatie 7 voor NIC's. Naast de bovenstaande, verdient het *verwijderen* de volgende parameters:
   
       rhgb quiet crashkernel=auto
   
   Grafische en stil opstarten zijn niet nuttig in een cloudomgeving waar we alle logboeken worden verzonden naar de seriële poort.
   
   De `crashkernel` mogelijk links geconfigureerd indien gewenst, maar houd er rekening mee dat deze parameter wordt verminderen de hoeveelheid beschikbaar geheugen in de virtuele machine door 128 MB of meer, die mogelijk problemen op de kleinere VM-grootten.
10. Wanneer u klaar bent bewerken '/ standaard/etc/wormgaten' per hierboven, voer de volgende opdracht om op te bouwen de grub-configuratie:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
12. De Azure Linux-Agent installeren met de volgende opdracht:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. Maak geen wisselruimte op de besturingssysteemschijf.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Houd er rekening mee dat de lokale bronschijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Klik op **actie-Afsluiten omlaag >** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van uw Oracle Linux-VHD te maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).


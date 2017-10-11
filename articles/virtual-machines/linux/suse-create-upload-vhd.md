---
title: Maken en uploaden van een VHD SUSE Linux in Azure
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) waarop een SUSE Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: c829f5d9a90b4260c6f41b2d9e511a0c6cb48f18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Een op SLES of openSUSE gebaseerde virtuele machine voor Azure voorbereiden
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd een SUSE of openSUSE Linux-besturingssysteem naar een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van de VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE opmerkingen bij de installatie
* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips over Linux voorbereiden voor Azure.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd.
* Bij het installeren van de Linux-systeem wordt het aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mag worden gebruikt voor gegevensschijven als voorkeur.
* Configureer een partitie van de wisseling niet op de schijf met het besturingssysteem. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle van de VHD's moeten grootten die veelvouden van 1 MB hebben.

## <a name="use-suse-studio"></a>SUSE Studio gebruiken
[SUSE Studio](http://www.susestudio.com) eenvoudig kunt maken en beheren van uw afbeeldingen SLES en openSUSE voor Azure en Hyper-V. Dit is de aanbevolen aanpak voor het aanpassen van uw eigen installatiekopieën SLES en openSUSE.

Als alternatief voor het bouwen van uw eigen VHD SUSE publiceert ook BYOS (uw eigen abonnement Bring) installatiekopieën voor SLES op [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 voorbereiden
1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.
2. Klik op **Connect** om het venster voor de virtuele machine te openen.
3. Registreren van uw systeem SUSE Linux Enterprise zodat deze kan updates downloaden en installeren van pakketten.
4. Het bijwerken van het systeem met de meest recente patches:
   
        # sudo zypper update
5. Installeer de Azure Linux Agent uit de opslagplaats voor SLES:
   
        # sudo zypper install WALinuxAgent
6. Controleer als waagent is ingesteld op 'aan' in chkconfig en als dat niet inschakelen voor automatisch starten:
   
        # sudo chkconfig waagent on
7. Controleer of waagent-service wordt uitgevoerd en als dat niet starten: 
   
        # sudo service waagent start
8. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Deze open doen ' / boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.
9. Bevestig dat /boot/grub/menu.lst en /etc/fstab beide verwijzen naar de schijf met de UUID (door uuid) in plaats van de schijf-ID (door-id). 
   
    Schijf UUID ophalen
   
        # ls /dev/disk/by-uuid/
   
    Als /dev/disk/by-id-is gebruikt, zowel /boot/grub/menu.lst als/etc/fstab bijwerken met de juiste door uuid-waarde
   
    Voordat de wijziging
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Na wijziging
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure- of Hyper-V:
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Het wordt aanbevolen het bestand bewerken '/ etc/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` -parameter voor het volgende:
    
     DHCLIENT_SET_HOSTNAME = "Nee"
12. Uitcommentariëren '/ etc/sudoers', of verwijder de volgende regels, indien aanwezig:
    
     Standaard targetpw # vragen het wachtwoord van de doelgebruiker dat wil zeggen hoofdmap alle ALL=(ALL) alle # waarschuwing! Gebruik alleen deze samen met 'Standaardwaarden targetpw'!
13. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
14. Maak geen wisselruimte op de schijf met het besturingssysteem.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Opmerking: Stel dit in op wat u ook nodig om te worden.
15. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - deprovision
    # <a name="export-histsize0"></a>exporteren van HISTSIZE = 0
    # <a name="logout"></a>afmelden
16. Klik op **actie-Afsluiten > omlaag** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

- - -
## <a name="prepare-opensuse-131"></a>OpenSUSE 13,1 + voorbereiden
1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.
2. Klik op **Connect** om het venster voor de virtuele machine te openen.
3. Voer de opdracht op de shell '`zypper lr`'. Als u deze opdracht uitvoer lijkt op de volgende retourneert, wordt de opslagplaatsen zijn geconfigureerd zoals verwacht--zonder aanpassingen nodig zijn (Houd er rekening mee dat versienummers kan verschillen):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Als de opdracht 'Geen opslagplaatsen gedefinieerd...' retourneert gebruikt u de volgende opdrachten om toe te voegen deze repo's:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    U kunt vervolgens controleren of de opslagplaatsen zijn toegevoegd met de opdracht '`zypper lr`' opnieuw. Als een van de relevante update-opslagplaatsen niet is ingeschakeld, wordt dit inschakelen met de volgende opdracht:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. De kernel bijwerken naar de meest recente versie:
   
        # sudo zypper up kernel-default
   
    Of het systeem bijwerken met de meest recente patches:
   
        # sudo zypper update
5. Installeer de Azure Linux Agent.
   
   # <a name="sudo-zypper-install-walinuxagent"></a>sudo zypper installeren WALinuxAgent
6. De regel voor het opstarten van kernel in uw configuratie wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Open hiervoor ' / boot/grub/menu.lst ' in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
   
     console = ttyS0 earlyprintk ttyS0 rootdelay = 300 =
   
   Hierdoor worden alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Bovendien de volgende parameters uit de kernel boot-regel verwijderen als deze bestaan:
   
     libata.atapi_enabled=0 reserve = 0x1f0, 0x8
7. Het wordt aanbevolen het bestand bewerken '/ etc/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` -parameter voor het volgende:
   
     DHCLIENT_SET_HOSTNAME = "Nee"
8. **Belangrijk:** uitcommentariëren In '/ etc/sudoers', of verwijder de volgende regels, indien aanwezig:
   
     Standaard targetpw # vragen het wachtwoord van de doelgebruiker dat wil zeggen hoofdmap alle ALL=(ALL) alle # waarschuwing! Gebruik alleen deze samen met 'Standaardwaarden targetpw'!
9. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
10. Maak geen wisselruimte op de schijf met het besturingssysteem.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource die is gekoppeld aan de virtuele machine na het inrichten op Azure automatisch configureren. Let op de lokale resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Opmerking: Stel dit in op wat u ook nodig om te worden.
11. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - deprovision
    # <a name="export-histsize0"></a>exporteren van HISTSIZE = 0
    # <a name="logout"></a>afmelden
12. Zorg ervoor dat de Azure Linux Agent wordt uitgevoerd bij het opstarten:
    
        # sudo systemctl enable waagent.service
13. Klik op **actie-Afsluiten > omlaag** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de virtuele harde schijf van SUSE Linux maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie de stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).


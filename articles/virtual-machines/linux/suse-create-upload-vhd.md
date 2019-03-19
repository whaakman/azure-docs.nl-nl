---
title: Een SUSE Linux-VHD in Azure maken en uploaden
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een SUSE Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 2b0c01ee4b1d1bc5ce83fc0afc309abfcf25f33e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57996685"
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Een op SLES of openSUSE gebaseerde virtuele machine voor Azure voorbereiden
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd een SUSE of openSUSE Linux-besturingssysteem op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van VHD-bestanden, bijvoorbeeld een oplossing voor netwerkvirtualisatie zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / opmerkingen bij de installatie van openSUSE
* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips voor Linux voorbereiden voor Azure.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd.
* Bij het installeren van de Linux-systeem is het raadzaam dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit zal LVM naam conflicteert met de gekloonde virtuele machines, voorkomen dat met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt als de voorkeur.
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt in de onderstaande stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Zie [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer informatie.

## <a name="use-suse-studio"></a>SUSE Studio gebruiken
[SUSE Studio](http://www.susestudio.com) eenvoudig kunt maken en beheren van uw installatiekopieën voor SLES en openSUSE voor Azure en Hyper-V. Dit is de aanbevolen aanpak voor het aanpassen van uw eigen installatiekopieën voor SLES en openSUSE.

Als alternatief voor het bouwen van uw eigen VHD SUSE publiceert ook BYOS (uw eigen abonnement Bring) installatiekopieën voor SLES op [VMDepot](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/04/using-and-contributing-vms-to-vm-depot.pdf).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 voorbereiden
1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.
2. Klik op **Connect** om het venster voor de virtuele machine te openen.
3. Registreer uw systeem SUSE Linux Enterprise zodat deze updates downloaden en installeren van pakketten.
4. Het systeem bijwerkt met de meest recente patches:
   
        # sudo zypper update
5. De Azure Linux Agent installeren vanaf de SLES-opslagplaats:
   
        # sudo zypper install python-azure-agent
6. Chkconfig inchecken als waagent is ingesteld op 'aan', en als dat niet inschakelen voor automatisch starten:
   
        # sudo chkconfig waagent on
7. Controleer of de service in waagent wordt uitgevoerd, en als dat niet het geval is, start de service: 
   
        # sudo service waagent start
8. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Deze open doen "/ boot/grub/menu.lst" in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Hierdoor kunnen alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen.
9. Bevestig dat /boot/grub/menu.lst en/etc/fstab beide verwijzen naar de schijf met behulp van de UUID (door uuid) in plaats van de schijf-ID (door-id). 
   
    Schijf UUID ophalen
   
        # ls /dev/disk/by-uuid/
   
    Als /dev/disk/by-id / is /boot/grub/menu.lst zowel/etc/fstab gebruikt, werkt met de juiste door uuid-waarde
   
    Vóór de wijziging
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Na wijziging
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Udev-regels om te voorkomen dat statische regels voor de Ethernet-interface (s) wijzigen. Deze regels kunnen problemen veroorzaken bij het klonen van een virtuele machine in Microsoft Azure of Hyper-V:
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Het wordt aanbevolen om het bestand te bewerken '/ etc/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` parameter in het volgende:
    
     DHCLIENT_SET_HOSTNAME="no"
12. In "/ etc/sudoers", commentaar of verwijder de volgende regels als deze bestaan:
    
     Standaard targetpw # gevraagd het wachtwoord van de doelgebruiker dat wil zeggen hoofdmap alle ALL=(ALL) alle # waarschuwing! Gebruik alleen deze samen met 'Standaardinstellingen targetpw'!
13. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
14. Maak geen wisselruimte op de besturingssysteemschijf.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Houd er rekening mee dat de lokale bronschijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Opmerking: Stel dit in op alles wat u nodig om te worden.
15. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
16. Klik op **actie-Afsluiten omlaag >** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

- - -
## <a name="prepare-opensuse-131"></a>OpenSUSE 13.1 + voorbereiden
1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.
2. Klik op **Connect** om het venster voor de virtuele machine te openen.
3. Voer op de shell de opdracht '`zypper lr`'. Als deze opdracht uitvoer die vergelijkbaar is met het volgende retourneert en vervolgens de opslagplaatsen zijn geconfigureerd zoals verwacht--zonder aanpassingen nodig zijn (Houd er rekening mee dat de versienummers kan verschillen):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Als de opdracht retourneert 'Er zijn geen bibliotheken gedefinieerd...', gebruik vervolgens de volgende opdrachten om toe te voegen deze opslagplaatsen:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f https://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Vervolgens kunt u controleren de opslagplaatsen zijn toegevoegd met de opdracht '`zypper lr`' opnieuw. Als een van de betreffende update-opslagplaatsen niet is ingeschakeld, wordt dit inschakelen met de volgende opdracht uit:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. De kernel bijwerken naar de meest recente beschikbare versie:
   
        # sudo zypper up kernel-default
   
    Of het systeem bijwerken met de meest recente patches:
   
        # sudo zypper update
5. Installeer de Azure Linux Agent.
   
        # sudo zypper install WALinuxAgent
6. Wijzig de kernel boot line in de grub-configuratie om op te nemen van aanvullende kernel-parameters voor Azure. Om dit te doen, opent u het volgende: "/ boot/grub/menu.lst" in een teksteditor en zorg ervoor dat de kernel standaard de volgende parameters bevat:
   
     console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
   Hierdoor kunnen alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure helpen kan ondersteuning bij het opsporen van problemen. Bovendien de volgende parameters uit de kernel boot line verwijderen als deze bestaan:
   
     libata.atapi_enabled=0 reserve=0x1f0,0x8
7. Het wordt aanbevolen om het bestand te bewerken '/ etc/sysconfig/netwerk/dhcp' en wijzig de `DHCLIENT_SET_HOSTNAME` parameter in het volgende:
   
     DHCLIENT_SET_HOSTNAME="no"
8. **Belangrijk:** In "/ etc/sudoers", commentaar of verwijder de volgende regels als deze bestaan:
   
     Standaard targetpw # gevraagd het wachtwoord van de doelgebruiker dat wil zeggen hoofdmap alle ALL=(ALL) alle # waarschuwing! Gebruik alleen deze samen met 'Standaardinstellingen targetpw'!
9. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.
10. Maak geen wisselruimte op de besturingssysteemschijf.
    
    De Azure Linux Agent kunt wisselruimte met behulp van de lokale resource-schijf die is gekoppeld aan de VM na het inrichten op Azure automatisch configureren. Houd er rekening mee dat de lokale bronschijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd. Na de installatie van de Azure Linux Agent (Zie de vorige stap), de volgende parameters in /etc/waagent.conf op de juiste wijze te wijzigen:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Opmerking: Stel dit in op alles wat u nodig om te worden.
11. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
12. Zorg ervoor dat de Azure Linux Agent wordt uitgevoerd bij het opstarten:
    
        # sudo systemctl enable waagent.service
13. Klik op **actie-Afsluiten omlaag >** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de virtuele harde schijf van SUSE Linux te maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).

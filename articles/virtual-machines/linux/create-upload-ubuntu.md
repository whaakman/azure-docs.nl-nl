---
title: Maken en uploaden van een VHD Ubuntu Linux in Azure
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) die een Ubuntu Linux-besturingssysteem bevat.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 4496b34ff88ca1e08cc74788ae09d787d4399eaf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Een virtuele Ubuntu-machine voor Azure voorbereiden
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Officiële Ubuntu cloud installatiekopieën
Ubuntu publiceert nu officiële Azure VHD's worden gedownload op [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Als u nodig hebt om uw eigen gespecialiseerde Ubuntu installatiekopie ontwikkelen voor Azure, in plaats daarvan dan de handmatige procedure hieronder gebruiken het verdient aanbeveling om te starten met deze bekende VHD's werkt en indien nodig aanpassen. De nieuwste versies van de installatiekopie kunnen altijd worden gevonden op de volgende locaties:

* Ubuntu 12.04/nauwkeurig: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/betrouwbare: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al een Ubuntu Linux-besturingssysteem hebt geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van de VHD-bestanden, bijvoorbeeld een virtualisatieoplossing zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie Ubuntu**

* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips over Linux voorbereiden voor Azure.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd.
* Bij het installeren van de Linux-systeem wordt het aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mag worden gebruikt voor gegevensschijven als voorkeur.
* Configureer een partitie van de wisseling niet op de schijf met het besturingssysteem. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt u in de volgende stappen uit.
* Alle van de VHD's moeten grootten die veelvouden van 1 MB hebben.

## <a name="manual-steps"></a>Handmatige stappen
> [!NOTE]
> Voordat u uw eigen aangepaste Ubuntu-afbeelding voor Azure maken, u kunt overwegen de installatiekopieën van het vooraf samengestelde en geteste van [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) in plaats daarvan.
> 
> 

1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.

2. Klik op **Connect** om het venster voor de virtuele machine te openen.

3. Vervang de huidige opslagplaatsen in de installatiekopie van Ubuntu Azure repo's gebruiken. De stappen verschillen, afhankelijk van de Ubuntu-versie.
   
    Voordat u bewerkt `/etc/apt/sources.list`, wordt u aangeraden te maken van een back-up:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. De afbeeldingen Ubuntu Azure volgt nu de *hardware stuk* kernel (HWE). Werk het besturingssysteem naar de meest recente kernel met de volgende opdrachten:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    **Zie ook:**
    - [https://Wiki.Ubuntu.com/kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://Wiki.Ubuntu.com/kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. De regel voor het opstarten van kernel voor wormgaten aanvullende kernel-parameters voor Azure opnemen wijzigen. Deze open doen `/etc/default/grub` in een teksteditor, ziet u de variabele met de naam `GRUB_CMDLINE_LINUX_DEFAULT` (of toe te voegen indien nodig) en het bewerken om op te nemen van de volgende parameters:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Opslaan en sluiten van dit bestand en voer `sudo update-grub`. Dit zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure technische ondersteuning helpen kan bij het opsporen van problemen.

6. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.

7. Installeer de Azure Linux Agent:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    De `walinuxagent` pakket kan verwijderen de `NetworkManager` en `NetworkManager-gnome` pakketten als ze zijn geïnstalleerd.

8. Voer de volgende opdrachten inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klik op **actie-Afsluiten > omlaag** in Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de Ubuntu Linux virtuele harde schijf maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie de stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="references"></a>Verwijzingen
Ubuntu hardware inschakelen (HWE) kernel:

* [http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [http://blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-Using-Hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)


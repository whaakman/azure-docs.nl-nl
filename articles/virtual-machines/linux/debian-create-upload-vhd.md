---
title: Voorbereiden van een Debian Linux-VHD in Azure | Microsoft Docs
description: Informatie over het maken van Debian 7 en 8 VHD-bestanden voor implementatie in Azure.
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 63970d162c12984d6476bf0b9fc4ab70160eccdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Een Debian VHD voor Azure voorbereiden
## <a name="prerequisites"></a>Vereisten
Deze sectie wordt ervan uitgegaan dat u al een Debian Linux-besturingssysteem hebt geïnstalleerd vanuit een .iso-bestand dat is gedownload van de [Debian website](https://www.debian.org/distrib/) op een virtuele harde schijf. Bestaan meerdere hulpprogramma's voor het maken van de VHD-bestanden; Hyper-V is slechts één voorbeeld. Zie voor instructies over het gebruik van Hyper-V [de Hyper-V-rol installeren en configureren van een virtuele Machine](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Opmerkingen bij de installatie
* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips over Linux voorbereiden voor Azure.
* De nieuwe VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de **convert-vhd** cmdlet.
* Bij het installeren van de Linux-systeem wordt het aanbevolen dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit voorkomt LVM naam conflicteert met de gekloonde virtuele machines, met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor het oplossen van problemen. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mag worden gebruikt voor gegevensschijven als voorkeur.
* Configureer een partitie van de wisseling niet op de schijf met het besturingssysteem. De Azure Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf. Meer informatie hierover vindt u in de volgende stappen uit.
* Alle van de VHD's moeten grootten die veelvouden van 1 MB hebben.

## <a name="use-azure-manage-to-create-debian-vhds"></a>Gebruik Azure beheren voor het maken van Debian VHD 's
Er zijn extra beschikbaar voor het genereren van Debian VHD's voor Azure, zoals de [azure-beheren](https://github.com/credativ/azure-manage) scripts van [credativ](http://www.credativ.com/). Dit is de aanbevolen aanpak ten opzichte van een installatiekopie van het maken van nieuw. Bijvoorbeeld, te maken van een Debian 8 VHD Voer de volgende opdrachten voor het downloaden van azure beheren (en de afhankelijkheden) en voer het script azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Handmatig een Debian VHD voorbereiden
1. Selecteer de virtuele machine in Hyper-V-beheer.
2. Klik op **Connect** om een consolevenster voor de virtuele machine te openen.
3. De regel voor commentaar **deb cdrom** in `/etc/apt/source.list` als u de virtuele machine op basis van een ISO-bestand instelt.
4. Bewerk de `/etc/default/grub` bestand en wijzig de **GRUB_CMDLINE_LINUX** parameter de als volgt aanvullende kernel-parameters voor Azure.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. De grub bouwen en uitvoeren:
   
        # sudo update-grub
6. Azure van Debian-opslagplaatsen aan /etc/apt/sources.list voor Debian 7 of 8 toevoegen:
   
    **Debian 7.x 'Wheezy'**
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    **Debian 8.x 'Jessie'**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. Installeer de Azure Linux Agent:
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. Voor Debian 7 is vereist voor het uitvoeren van de kernel 3,16 gebaseerde vanuit de opslagplaats wheezy backports. Maak eerst een bestand met de naam /etc/apt/preferences.d/linux.pref met de volgende inhoud:
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    Voer 'sudo apt get-installatie linux-installatiekopie-amd64' voor het installeren van de nieuwe kernel.
3. Inrichting ervan ongedaan maakt de virtuele machine en deze voorbereiden voor het inrichten op Azure en uitvoeren:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. Klik op **actie** -afsluiten > omlaag in de Hyper-V-beheer. Uw Linux VHD is nu gereed om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de Debian virtuele harde schijf maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie de stappen 2 en 3 in [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).


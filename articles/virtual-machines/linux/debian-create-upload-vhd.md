---
title: Voorbereiden van een Debian VHD met Linux in Azure | Microsoft Docs
description: Informatie over het maken van Debian VHD-installatiekopieën voor implementatie in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: szark
ms.openlocfilehash: ce2b9811baffea85cfa9a542fb5f93652daf39c8
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976447"
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Een Debian VHD voor Azure voorbereiden
## <a name="prerequisites"></a>Vereisten
Deze sectie wordt ervan uitgegaan dat u hebt een Debian Linux-besturingssysteem geïnstalleerd vanaf een .iso-bestand gedownload van de [Debian website](https://www.debian.org/distrib/) op een virtuele harde schijf. Bestaan meerdere hulpprogramma's voor het maken van VHD-bestanden. Hyper-V is slechts één voorbeeld. Zie voor instructies over het gebruik van Hyper-V [de Hyper-V-rol installeren en configureren van een virtuele Machine](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Opmerkingen bij de installatie
* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips voor Linux voorbereiden voor Azure.
* De nieuwe VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de **convert-vhd** cmdlet.
* Bij het installeren van de Linux-systeem, is het raadzaam dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit zal LVM naam conflicteert met de gekloonde virtuele machines, voorkomen dat met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt als de voorkeur.
* Stel een swap-partitie niet op de besturingssysteemschijf. De Azure Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf. Meer informatie kunt vinden in de onderstaande stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar VHD, moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Zie voor meer informatie, [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes).

## <a name="use-azure-manage-to-create-debian-vhds"></a>Gebruik Azure beheren om te maken van Debian VHD 's
Er zijn hulpprogramma's beschikbaar voor het genereren van Debian VHD's voor Azure, zoals de [azure-beheren](https://github.com/credativ/azure-manage) scripts via [Credativ](http://www.credativ.com/). Dit is de aanbevolen aanpak ten opzichte van het maken van een installatiekopie van een nieuwe. Bijvoorbeeld, het uitvoeren van een VHD voor Debian 8 de volgende maken-opdrachten voor het downloaden van de `azure-manage` hulpprogramma (en afhankelijkheden) en voer de `azure_build_image` script:

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
3. Als u het besturingssysteem met behulp van een ISO die is geïnstalleerd, klikt u vervolgens als commentaar voor een regel met betrekking tot '`deb cdrom`"in `/etc/apt/source.list`.

4. Bewerk de `/etc/default/grub` -bestand en wijzig de **GRUB_CMDLINE_LINUX** parameter als volgt om op te nemen van aanvullende kernel-parameters voor Azure.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8 earlyprintk=ttyS0,115200"

5. De grub bouwen en uitvoeren:

        # sudo update-grub

6. Toevoegen van Debian Azure opslagplaatsen aan /etc/apt/sources.list voor Debian 8 en 9:

    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie main
        deb-src http://debian-archive.trafficmanager.net/debian jessie main
        deb http://debian-archive.trafficmanager.net/debian-security jessie/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security jessie/updates
        deb http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main

    **Debian 9.x "Stretch"**

        deb http://debian-archive.trafficmanager.net/debian stretch main
        deb-src http://debian-archive.trafficmanager.net/debian stretch main
        deb http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-backports main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-backports main


7. De Azure Linux-Agent installeren:
   
        # sudo apt-get update
        # sudo apt-get install waagent

8. Voor Debian 9 +, is het aanbevolen gebruik van de nieuwe Debian Cloud-kernel voor gebruik met virtuele machines in Azure. Voor het installeren van deze nieuwe kernel, maakt u eerst een bestand met de naam /etc/apt/preferences.d/linux.pref met de volgende inhoud:
   
        Package: linux-* initramfs-tools
        Pin: release n=stretch-backports
        Pin-Priority: 500
   
    Voer 'sudo apt-get install linux-installatiekopie-cloud-amd64' voor het installeren van de nieuwe Debian Cloud-kernel.

9. Inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure en uitvoeren:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

10. Klik op **actie** afsluiten omlaag -> in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="next-steps"></a>Volgende stappen
U kunt nu uw Debian virtuele harde schijf gebruiken om te maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).


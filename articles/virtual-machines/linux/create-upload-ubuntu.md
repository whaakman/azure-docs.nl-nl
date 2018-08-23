---
title: Een Ubuntu Linux-VHD in Azure maken en uploaden
description: Informatie over het maken en uploaden van een Azure virtuele harde schijf (VHD) met een Ubuntu Linux-besturingssysteem.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 5b2f902389b96136e0ea0c4c58f5e8be8144a248
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42055140"
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Een virtuele Ubuntu-machine voor Azure voorbereiden
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Officiële cloud Ubuntu-installatiekopieën
Nu publiceren officiële Azure-VHD's voor downloaden op Ubuntu [ http://cloud-images.ubuntu.com/ ](http://cloud-images.ubuntu.com/). Als u uw eigen gespecialiseerde Ubuntu-installatiekopie bouwen voor Azure wilt, in plaats daarvan dan de handmatige procedure hieronder gebruiken het verdient aanbeveling om te starten met deze bekende VHD's werkt en indien nodig aanpassen. De meest recente versies van de installatiekopie kunnen altijd worden gevonden op de volgende locaties:

* Ubuntu 12.04/nauwkeurige: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/betrouwbare: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt een Ubuntu Linux-besturingssysteem geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het maken van VHD-bestanden, bijvoorbeeld een oplossing voor netwerkvirtualisatie zoals Hyper-V. Zie voor instructies [de Hyper-V-rol installeren en configureren van een virtuele Machine](http://technet.microsoft.com/library/hh846766.aspx).

**Opmerkingen bij de installatie van Ubuntu**

* Zie ook [algemene opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer tips voor Linux voorbereiden voor Azure.
* De VHDX-indeling wordt niet ondersteund in Azure, alleen **vaste VHD**.  U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet convert-vhd.
* Bij het installeren van de Linux-systeem is het raadzaam dat u standaard partities in plaats van LVM (vaak de standaardinstelling voor vele installaties gebruikt). Dit zal LVM naam conflicteert met de gekloonde virtuele machines, voorkomen dat met name als een besturingssysteemschijf ooit worden gekoppeld aan een andere virtuele machine moet voor probleemoplossing. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) op gegevensschijven kunnen worden gebruikt als de voorkeur.
* Stel een swap-partitie niet op de besturingssysteemschijf. De Linux-agent kan worden geconfigureerd voor het maken van een wisselbestand op de tijdelijke schijf.  Meer informatie hierover vindt in de onderstaande stappen.
* Alle VHD's op Azure beschikken over een virtuele grootte die is afgestemd op 1MB. Bij het converteren van een onbewerkte schijf naar de VHD moet u ervoor zorgen dat de onbewerkte schijfgrootte een veelvoud van 1MB vóór de conversie is. Zie [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer informatie.

## <a name="manual-steps"></a>Handmatige stappen
> [!NOTE]
> Voordat u probeert te maken van uw eigen aangepaste Ubuntu-installatiekopie voor Azure, overweeg dan de afbeeldingen vooraf gebouwd en getest in [ http://cloud-images.ubuntu.com/ ](http://cloud-images.ubuntu.com/) in plaats daarvan.
> 
> 

1. Selecteer in het middelste deelvenster van de Hyper-V-beheer, de virtuele machine.

2. Klik op **Connect** om het venster voor de virtuele machine te openen.

3. Vervang de huidige opslagplaatsen in de afbeelding moet worden gebruikt Azure de Ubuntu-opslagplaatsen. De stappen verschillen, afhankelijk van de Ubuntu-versie.
   
    Voordat u bewerkt `/etc/apt/sources.list`, het is raadzaam om een back-up:
   
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

4. De Ubuntu Azure-installatiekopieën volgt nu het *hardware, stuk* (HWE) kernel. Werk het besturingssysteem naar de meest recente kernel door het uitvoeren van de volgende opdrachten:

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
    - [https://wiki.ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Wijzig de kernel boot line voor wormgaten om op te nemen van aanvullende kernel-parameters voor Azure. Deze open doen `/etc/default/grub` in een teksteditor en zoek de variabele met de naam `GRUB_CMDLINE_LINUX_DEFAULT` (of toe te voegen indien nodig) en bewerk deze zodanig dat de volgende parameters:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Opslaan en sluiten van dit bestand en voer `sudo update-grub`. Dit zorgt ervoor dat alle consoleberichten worden verzonden naar de eerste seriële poort, die Azure technische ondersteuning helpen kan bij het opsporen van problemen.

6. Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten.  Dit is doorgaans de standaardinstelling.

7. De Azure Linux-Agent installeren:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    De `walinuxagent` pakket kan verwijderen de `NetworkManager` en `NetworkManager-gnome` pakketten, als ze zijn geïnstalleerd.

8. Voer de volgende opdrachten voor de inrichting van de virtuele machine ongedaan maken en voorbereiden voor het inrichten op Azure:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klik op **actie-Afsluiten omlaag >** in Hyper-V-beheer. De VHD met Linux is nu klaar om te worden geüpload naar Azure.

## <a name="references"></a>Verwijzingen
[Ubuntu hardware activering (HWE) kernel](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar voor gebruik van de Ubuntu Linux virtuele harde schijf te maken van nieuwe virtuele machines in Azure. Als dit de eerste keer dat u de VHD-bestand naar Azure uploadt, Zie [een Linux-VM maken van een aangepaste schijf](upload-vhd.md#option-1-upload-a-vhd).





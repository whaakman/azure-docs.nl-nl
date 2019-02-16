---
title: Optimaliseren van uw virtuele Linux-machine in Azure | Microsoft Docs
description: Meer optimalisatietips voor om ervoor te zorgen dat u uw Linux-VM voor optimale prestaties op Azure hebt ingesteld
keywords: virtuele Linux-machine, virtuele machine linux, virtuele ubuntu-machine
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 8baa30c8-d40e-41ac-93d0-74e96fe18d4c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: 30d153863a20dcdddc702ee5a37c34a2938d7446
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327363"
---
# <a name="optimize-your-linux-vm-on-azure"></a>Uw Linux VM optimaliseren voor Azure
Het maken van een Linux virtuele machine (VM) is heel gemakkelijk vanaf de opdrachtregel of vanuit de portal. Deze zelfstudie leert u hoe u om te controleren of u dit hebt ingesteld om de prestaties van de Microsoft Azure-platform te optimaliseren. In dit onderwerp wordt gebruikgemaakt van een Ubuntu-Server-VM, maar u kunt ook maken gebruik van Linux virtuele machine [uw eigen installatiekopieën als sjablonen](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Vereisten
In dit onderwerp wordt ervan uitgegaan dat u al een werkende Azure-abonnement hebt ([registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)) en een virtuele machine al in uw Azure-abonnement hebt ingericht. Zorg ervoor dat u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij uw Azure-abonnement met [az login](/cli/azure/reference-index) voordat u [maken van een virtuele machine](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Azure OS Disk
Als u een Linux-VM in Azure maakt, heeft twee schijven zijn gekoppeld. **/ dev/sda** is van de besturingssysteemschijf **/dev/sdb** is de tijdelijke schijf.  Gebruik niet de belangrijkste OS-schijf (**/dev/sda**) voor alles wat met uitzondering van het besturingssysteem die is geoptimaliseerd voor snelle VM boot-tijd en biedt geen goede prestaties voor uw workloads. U wilt een of meer schijven aansluiten op uw virtuele machine om op te halen permanente en geoptimaliseerd voor opslag voor uw gegevens. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Schijven toe te voegen voor grootte- en prestatiedoelen
Op basis van de VM-grootte, kunt u maximaal 16 extra schijven op een A-serie, 32 schijven op een D-serie koppelen en 64 schijven op een G-serie-machine - elke tot 1 TB in grootte. U kunt extra schijven toevoegen per ruimte en IOps-vereisten van uw behoefte. Elke schijf heeft een prestatiedoel van 500 IOps voor Standard-opslag en maximaal 5000 IOP's per schijf voor Premium-opslag.

Om te kunnen de hoogste IOps op waar de cache-instellingen zijn ingesteld op Premium Storage-schijven **ReadOnly** of **geen**, moet u uitschakelen **barrières** tijdens het koppelen het bestandssysteem in Linux. U hoeft niet barrières omdat de schrijfbewerkingen naar Premium-opslag ondersteund schijven duurzame voor deze cache-instellingen.

* Als u **reiserFS**, uitschakelen hindernissen bij met de optie koppelpunt `barrier=none` (gebruikt voor het inschakelen van barrières `barrier=flush`)
* Als u **ext3/ext4**, uitschakelen hindernissen bij met de optie koppelpunt `barrier=0` (gebruikt voor het inschakelen van barrières `barrier=1`)
* Als u **XFS**, uitschakelen hindernissen bij met de optie koppelpunt `nobarrier` (Gebruik de optie voor het inschakelen van barrières `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Aandachtspunten voor gebruikersaccounts van niet-beheerde opslag
De standaardactie wanneer u een virtuele machine met de Azure CLI maken is het gebruik van Azure Managed Disks.  Deze schijven worden verwerkt door het Azure-platform en vereisen geen geen voorbereiding of locatie op te slaan.  Niet-beheerde schijven vereisen een storage-account en hebben enkele aanvullende prestatie-overwegingen.  Zie voor meer informatie over beheerde schijven [overzicht Azure Managed Disks](../windows/managed-disks-overview.md).  De volgende sectie bevat een overzicht van prestatie-overwegingen wanneer u het niet-beheerde schijven gebruikt.  Nogmaals, de standaardinstelling en aanbevolen oplossing is het gebruik van beheerde schijven.

Als u een virtuele machine met niet-beheerde schijven maken, zorg er dan voor dat u schijven koppelen van storage-accounts die zich bevinden in dezelfde regio als uw virtuele machine om te controleren dicht en minimale netwerklatentie.  Elke Standard-opslagaccount heeft maximaal 20 k IOP's en een capaciteit van de grootte van 500 TB.  Deze limiet werkt op ongeveer 40 intensief gebruikte schijven, waaronder zowel schijf met het besturingssysteem en eventuele gegevensschijven die u maakt. Er is geen maximale IOps-limiet voor Premium Storage-accounts, maar er is een limiet van 32 TB. 

Bij het afhandelen van hoge IOps werkbelastingen en u ervoor hebt gekozen voor Standard-opslag voor uw schijven, moet u mogelijk de schijven verdeeld over meerdere opslagaccounts om ervoor te zorgen dat u hebt niet bereikt voor de 20.000 IOps-limiet voor Standard Storage-accounts. Uw virtuele machine kan een combinatie van schijven uit tussen verschillende opslagaccounts en storage-accounttypen om de optimale configuratie bevatten.
 

## <a name="your-vm-temporary-drive"></a>Uw virtuele machine tijdelijk station
Standaard bij het maken van een virtuele machine, biedt Azure u een besturingssysteemschijf (**/dev/sda**) en een tijdelijke schijf (**/dev/sdb**).  Alle extra schijven u, weergeven als toevoegen **/dev/sdc**, **/dev/sdd**, **/dev/sde** enzovoort. Alle gegevens op de tijdelijke schijf (**/dev/sdb**) is niet duurzame, en kunnen verloren gaan als specifieke gebeurtenissen, zoals VM-formaat, opnieuw te implementeren of onderhoud zorgt ervoor dat uw virtuele machine opnieuw worden opgestart.  De grootte en het type van de tijdelijke schijf is gekoppeld aan de VM-grootte die u hebt gekozen tijdens de implementatie. Alle van de premium-grootte van virtuele machines (DS, G en DS_V2-serie) het tijdelijke station worden ondersteund door een lokale SSD voor extra prestaties van maximaal 48 kB IOps. 

## <a name="linux-swap-file"></a>Wisselbestand voor Linux
Als uw Azure-VM van een installatiekopie van een Ubuntu of CoreOS is, kunt u CustomData gebruiken voor het verzenden van een cloud-configuratie voor cloud-init. Als u [geüpload van een aangepaste installatiekopie van Linux](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die gebruikmaakt van cloud-init, u swap-partities cloud-init gebruiken voor het ook configureren.

Op Ubuntu Cloud-installatiekopieën, moet u cloud-init gebruiken voor het configureren van de swap-partitie. Zie voor meer informatie, [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Voor afbeeldingen zonder ondersteuning voor cloud-init hebt geïmplementeerd op Azure Marketplace VM-installatiekopieën een virtuele machine Linux-Agent geïntegreerd met het besturingssysteem. Deze agent kan de virtuele machine om te communiceren met verschillende Azure-services. Ervan uitgaande dat u een standard-installatiekopie uit de Azure Marketplace hebt geïmplementeerd, moet u voor het correct configureren van instellingen voor het wisselbestand Linux als volgt:

Zoek en wijzigen van twee vermeldingen in de **/etc/waagent.conf** bestand. Ze bepalen het bestaan van een toegewezen wisselbestand en de grootte van het wisselbestand. De parameters die u wilt wijzigen zijn `ResourceDisk.EnableSwap=N` en `ResourceDisk.SwapSizeMB=0` 

Wijzig de parameters voor de volgende instellingen:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size in MB om te voldoen aan uw behoeften} 

Nadat u de wijziging hebt aangebracht, moet u moet de waagent of opnieuw opstarten van uw Linux-VM om de wijzigingen weer te geven.  U weet dat de wijzigingen die zijn geïmplementeerd en er is een wisselbestand gemaakt wanneer u de `free` opdracht om beschikbare ruimte weer te geven. Het volgende voorbeeld heeft een wisselbestand voor 512MB gemaakt als gevolg van het wijzigen van de **waagent.conf** bestand:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Planning algoritme voor Premium-opslag i/o
Met de 2.6.18 Linux-kernel, de standaard i/o planning algoritme is gewijzigd van de Deadline in CFQ (volledig geoorloofd queuing algoritme). Er is voor willekeurige toegang i/o-patronen, te verwaarlozen verschil in prestatieverschillen tussen CFQ en Deadline.  Overschakelen naar het algoritme Nooperation of Deadline bereiken betere i/o-prestaties voor SSD-schijven op basis van waar de schijf-i/o-patroon voornamelijk sequentieel is.

### <a name="view-the-current-io-scheduler"></a>De huidige i/o-planner weergeven
Gebruik de volgende opdracht:  

```bash
cat /sys/block/sda/queue/scheduler
```

U ziet de volgende uitvoer geeft de huidige Taakplanner.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Het huidige apparaat (/ dev/sda) van i/o algoritme planning wijzigen
Gebruik de volgende opdrachten:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Toepassen van deze instelling voor **/dev/sda** alleen is niet nuttig. Ingesteld op alle gegevensschijven waar opeenvolgende i/o bepaalt de i/o-patroon.  

U ziet de uitvoer van de volgende die aangeeft dat **grub.cfg** met succes is opnieuw opgebouwd en dat de scheduler standaard is bijgewerkt naar Nooperation.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Voor de distributie-familie van Red Hat hoeft u alleen de volgende opdracht uit:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-to-achieve-higher-iops"></a>Met behulp van Software-RAID voor een hoger ik / Ops
Als uw werkbelastingen meer IOps vereisen dan één schijf kan bieden, moet u een software-RAID-configuratie van meerdere schijven gebruiken. Omdat Azure al tolerantie van de schijf op het niveau van de lokale infrastructuur voert, kunt u het hoogste niveau van de prestaties van een configuratie van RAID-0 striping van bereiken.  Inrichten en maken van schijven in de Azure-omgeving en koppelt u ze aan uw Linux-VM voordat u partitioneren, formatteren en het koppelen van de stations.  Meer informatie over het configureren van een software-RAID-instellingen op uw Linux-VM in azure vindt u de **[Software-RAID configureren onder Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** document.

## <a name="next-steps"></a>Volgende stappen
Denk eraan dat als met alle optimalisatie discussies, u moet het uitvoeren van tests voor en na elke wijziging voor het meten van de impact is van de wijziging.  Optimalisatie is een stapsgewijze proces dat bestaat uit verschillende resultaten op verschillende computers in uw omgeving.  Wat werkt voor één configuratie werkt niet voor andere gebruikers.

Sommige handige koppelingen naar aanvullende bronnen:

* [Gebruikershandleiding voor Azure Linux Agent](../extensions/agent-linux.md)
* [MySQL-prestaties op virtuele Azure Linux-machines optimaliseren](classic/optimize-mysql.md)
* [Software-RAID op Linux configureren](configure-raid.md)

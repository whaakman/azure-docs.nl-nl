---
title: Optimaliseren van uw virtuele Linux-machine in Azure | Microsoft Docs
description: Meer informatie over een aantal optimalisatietips om ervoor te zorgen dat u uw Linux-VM voor optimale prestaties in Azure hebt ingesteld
keywords: virtuele Linux-machine, linux virtuele machine, ubuntu virtuele machine
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
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
ms.openlocfilehash: eb79d574fd4dddfb986660cc338bc8748f2082c2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-your-linux-vm-on-azure"></a>Uw Linux VM optimaliseren voor Azure
Maken van virtuele Linux-machine (VM) is eenvoudig doen vanaf de opdrachtregel of in de portal. Deze zelfstudie laat zien hoe u om te controleren of u dit hebt ingesteld om de prestaties van het Microsoft Azure-platform te optimaliseren. In dit onderwerp maakt gebruik van een virtuele Ubuntu Server-machine, maar u kunt ook maken Linux virtuele machine met [uw eigen installatiekopieën die u als sjabloon](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Vereisten
In dit onderwerp wordt ervan uitgegaan dat u hebt al een werkende Azure-abonnement ([gratis proefversie te registreren](https://azure.microsoft.com/pricing/free-trial/)) en u hebt al een virtuele machine ingericht in uw Azure-abonnement. Zorg ervoor dat u de meest recente hebt [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij uw Azure-abonnement met [az aanmelding](/cli/azure/#login) voordat u [een virtuele machine maken](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Azure Besturingssysteemschijf
Wanneer u een Linux-VM in Azure maakt, heeft deze twee schijven zijn gekoppeld. **/ dev/sda** staat voor de schijf OS **/dev/sdb** staat voor de tijdelijke schijf.  Gebruik niet de belangrijkste besturingssysteemschijf (**/dev/sda**) voor alles behalve het besturingssysteem die is geoptimaliseerd voor snelle VM-opstarten en biedt geen goede prestaties voor uw werkbelastingen. U wilt een of meer schijven toevoegen aan uw virtuele machine persistent ophalen en opslag van uw gegevens geoptimaliseerd. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Het toevoegen van schijven voor grootte- en prestatiedoelen
Op basis van de VM-grootte, kunt u maximaal 16 extra schijven op een A-serie, 32 schijven op een D-reeks koppelen en 64 schijven op een G-serie machine - elke maximaal 1 TB groot. U kunt extra schijven toevoegen naar behoefte per uw ruimte en de vereisten voor IOps. Elke schijf heeft een prestatiedoel van 500 IOps voor Standard-opslag en maximaal 5000 IOP's per schijf voor Premium-opslag.  Zie voor meer informatie over Premium-opslag schijven [Premium-opslag: krachtige opslag voor Azure Virtual machines](../../storage/common/storage-premium-storage.md)

Als u de hoogste IOps op schijven met Premium-opslag waar de cache-instellingen zijn ingesteld op **ReadOnly** of **geen**, moet u uitschakelen **barrières** tijdens het koppelen van het bestandssysteem in Linux. U hoeft niet barrières omdat schrijfbewerkingen naar de Premium-opslag ondersteund schijven duurzame voor deze cache-instellingen.

* Als u **reiserFS**, barrières van uitschakelen met de optie koppelpunt `barrier=none` (gebruikt voor het inschakelen van barrières `barrier=flush`)
* Als u **ext3/ext4**, barrières van uitschakelen met de optie koppelpunt `barrier=0` (gebruikt voor het inschakelen van barrières `barrier=1`)
* Als u **XFS**, barrières van uitschakelen met de optie koppelpunt `nobarrier` (Gebruik de optie voor het inschakelen van barrières `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Aandachtspunten voor gebruikersaccounts onbeheerde opslag
De standaardactie wanneer u een virtuele machine met de Azure CLI 2.0 maken is het gebruik van beheerde Azure-schijven.  Deze schijven worden verwerkt door de Azure-platform en hoeven niet de voorbereidings- of locatie om op te slaan.  Niet-beheerde schijven vereisen een opslagaccount en hebben enkele aanvullende prestatie-overwegingen.  Zie voor meer informatie over beheerde schijven [overzicht Azure Managed Disks](../windows/managed-disks-overview.md).  De volgende sectie bevat een overzicht van prestatie-overwegingen wanneer u het niet-beheerde schijven gebruikt.  Opnieuw, de standaardwaarde en aanbevolen opslagoplossing beheerde schijven te gebruiken.

Als u een virtuele machine met niet-beheerde schijven maakt, zorg er dan voor dat u Koppel de schijven van de storage-accounts die zich in dezelfde regio bevinden als uw virtuele machine om te controleren dicht en netwerklatentie minimaliseren.  Elk Standard-opslag-account is een maximum van 20 k IOps en een capaciteit van de grootte van 500 TB.  Deze limiet werkt ongeveer 40 intensief gebruikte schijven, inclusief de besturingssysteemschijf en eventuele gegevensschijven die u maakt. Er is geen limiet voor de maximale IOps voor Premium Storage-accounts, maar er is een limiet van 32 TB. 

Wanneer het omgaan met een hoge IOps werkbelastingen en u hebt ervoor gekozen Standard-opslag voor uw schijven, moet u mogelijk de schijven verdelen over meerdere opslagaccounts om ervoor te zorgen dat u hebt niet de 20.000 IOps-limiet voor Standard-opslag-accounts bereikt. Uw virtuele machine kan een combinatie van schijven uit tussen verschillende opslagaccounts en opslagaccounttypen bereiken van de optimale configuratie bevatten.
 

## <a name="your-vm-temporary-drive"></a>De VM tijdelijke schijf
Wanneer u een virtuele machine, maakt Azure biedt standaard u met een besturingssysteemschijf (**/dev/sda**) en een tijdelijke schijf (**/dev/sdb**).  Alle aanvullende schijven u, weergeven als toevoegen **/dev/sdc**, **/dev/sdd**, **/dev/sde** enzovoort. Alle gegevens op de tijdelijke schijf (**/dev/sdb**) is niet duurzame, en kunnen verloren gaan als specifieke gebeurtenissen, zoals VM-grootte, opnieuw installeren, of onderhoud zorgt ervoor uw virtuele machine opnieuw wordt opgestart dat.  De grootte en het type van de tijdelijke schijf is gekoppeld aan de VM-grootte die u hebt gekozen tijdens de implementatie. Alle premium het formaat van virtuele machines (DS, G en DS_V2-serie) het tijdelijke station worden ondersteund door een lokale SSD voor extra prestaties van maximaal 48k IOps. 

## <a name="linux-swap-file"></a>Wisselbestand van Linux
Als uw Azure VM van een installatiekopie van een Ubuntu of virtuele CoreOS is, kunt u CustomData gebruiken om te verzenden van een cloud-config naar cloud init. Als u [geüpload een aangepaste installatiekopie van Linux](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) die gebruikmaakt van cloud-init, u ook configureren met behulp van cloud-init swap-partities.

Ubuntu Cloud afbeeldingen, moet u cloud-init gebruiken voor het configureren van de wisseling partitie. Zie voor meer informatie [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Voor installatiekopieën zonder ondersteuning voor cloud-init zijn VM-installatiekopieën die zijn geïmplementeerd vanuit Azure Marketplace een VM-Linux-Agent geïntegreerd met het besturingssysteem. Deze agent kan de virtuele machine om te communiceren met verschillende Azure-services. Ervan uitgaande dat u een standaardinstallatiekopie vanuit Azure Marketplace hebt geïmplementeerd, moet u volgt te werk om instellingen voor het wisselbestand Linux correct te configureren:

Zoek en wijzigen van twee vermeldingen in de **/etc/waagent.conf** bestand. Ze bepalen de aanwezigheid van een speciale wisselbestand en grootte van het wisselbestand. De parameters die u wilt wijzigen zijn `ResourceDisk.EnableSwap=N` en`ResourceDisk.SwapSizeMB=0` 

Wijzig de parameters voor de volgende instellingen:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size in MB voor uw behoeften} 

Zodra u de wijziging hebt aangebracht, moet u moet de waagent of opnieuw opstarten van uw Linux-VM om deze wijzigingen weer te geven.  U weet dat de wijzigingen zijn uitgevoerd en er is een wisselbestand gemaakt wanneer u de `free` opdracht voor het weergeven van de vrije ruimte. Het volgende voorbeeld heeft een wisselbestand van 512MB gemaakt naar aanleiding van het wijzigen van de **waagent.conf** bestand:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/o-planning algoritme voor Premium-opslag
Met de 2.6.18 Linux kernel, de standaard i/o planning algoritme is gewijzigd van de Deadline in CFQ (volledig evenredige queuing algoritme). Er is te verwaarlozen verschil in prestatieverschillen tussen CFQ en de Deadline voor willekeurige toegang i/o-patronen.  Overschakelen naar het algoritme Nooperation of Deadline leveren betere i/o-prestaties voor SSD-schijven op basis van waar de schijf-i/o-patroon hoofdzakelijk sequentieel is.

### <a name="view-the-current-io-scheduler"></a>De huidige i/o-planner weergeven
Gebruik de volgende opdracht:  

```bash
cat /sys/block/sda/queue/scheduler
```

U ziet na uitvoer, waarmee wordt aangegeven van de huidige Taakplanner.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Wijzigen van het huidige apparaat (/ dev/sda) van i/o algoritme plannen
Gebruik de volgende opdrachten:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Toepassen van deze instelling voor **/dev/sda** alleen is niet handig. Ingesteld op alle gegevensschijven waar opeenvolgende i/o zomaar het i/o-patroon.  

U ziet de volgende uitvoer, die aangeeft dat **grub.cfg** met succes is opnieuw opgebouwd en dat de planner standaard is bijgewerkt naar Nooperation.  

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

Voor de distributie-familie van Redhat hoeft u alleen de volgende opdracht:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-to-achieve-higher-iops"></a>Met behulp van Software-RAID bereiken hoger ik / Ops
Als uw werkbelastingen meer IOps vereisen dan één schijf kan bieden, moet u een software-RAID-configuratie van meerdere schijven gebruiken. Omdat Azure al tolerantie van de schijf op de lokale fabric-laag voert, kunt u het hoogste niveau van de prestaties van een configuratie van RAID-0 striping van bereiken.  Inrichten en schijven te maken in de Azure-omgeving en deze koppelt aan uw Linux-VM voordat partitioneren, formatteren en koppelen van de stations.  Meer informatie over het configureren van een software-RAID-instellingen op uw Linux-VM in azure vindt u in de  **[configureren van Software-RAID op Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**  document.

## <a name="next-steps"></a>Volgende stappen
Denk eraan dat als met alle optimalisatie discussies die u nodig hebt voor het uitvoeren van de tests voor en na elke wijziging voor het meten van de gevolgen heeft voor de wijziging.  Optimalisatie wordt stap voor stap processen met verschillende resultaten op verschillende computers in uw omgeving.  Wat werkt voor één configuratie werkt niet voor anderen.

Sommige nuttig koppelingen naar aanvullende bronnen: 

* [Premium Storage: opslag met hoge prestaties voor de werkbelasting van virtuele Azure-machines](../../storage/common/storage-premium-storage.md)
* [Gebruikershandleiding voor Azure Linux-Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [MySQL-prestaties op Azure Linux virtuele machines optimaliseren](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Configureren van Software-RAID op Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


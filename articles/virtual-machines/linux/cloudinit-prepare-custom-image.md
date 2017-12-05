---
title: De installatiekopie van de virtuele machine van Azure voor gebruik met cloud-init voorbereiden | Microsoft Docs
description: Een installatiekopie van een bestaande virtuele machine van Azure voorbereiden voor implementatie met cloud-init
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 2eb7510d4e76e4996e83f351a62c0b025b487df2
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Een installatiekopie van een bestaande virtuele machine van Azure Linux voor gebruik met cloud-init voorbereiden
In dit artikel leest u hoe moet worden overgenomen van een bestaande virtuele machine van Azure en deze worden opnieuw gedistribueerde en klaar voor gebruik van cloud-init voorbereiden. De afbeelding kan worden gebruikt om een nieuwe virtuele machine of virtuele-machineschaalsets - waaruit kan vervolgens verder worden aangepast door cloud init tijdens de implementatie te implementeren.  Deze cloud init-scripts worden uitgevoerd op de eerste keer wordt opgestart nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud init-overzicht](using-cloud-init.md)

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u hebt al een actieve virtuele machine van Azure met een ondersteunde versie van het Linux-besturingssysteem. U de machine aan uw behoeften al hebt geconfigureerd de vereiste modules geïnstalleerd, wordt de vereiste updates verwerkt en getest om te controleren of deze voldoet aan uw vereisten. 

## <a name="preparing-rhel-74--centos-74"></a>RHEL 7.4 voorbereiden / CentOS 7.4
U moet SSH in Linux-VM en voer de volgende opdrachten om te kunnen installeren cloud init.

```bash
sudo yum install -y cloud-init gdisk
sudo yum check-update cloud-init -y
sudo yum install cloud-init -y
```

Update de `cloud_init_modules` in sectie `/etc/cloud/cloud.cfg` om op te nemen van de volgende modules:
```bash
- disk_setup
- mounts
```

Hier volgt een voorbeeld van wat een algemeen `cloud_init_modules` sectie eruit ziet.
```bash
 cloud_config_modules:
 - mounts
 - locale
 - set-passwords
 - rh_subscription
 - yum-add-repo
 - package-update-upgrade-install
 - timezone
 - puppet
 - chef
 - salt-minion
 - mcollective
 - disable-ec2-metadata
 - runcmd
 - disk_setup
 - mounts
```
Een aantal taken met betrekking tot inrichting en afhandeling van tijdelijke schijven moet worden bijgewerkt `/etc/waagent.conf`. Voer de volgende opdrachten om bij te werken van de toepasselijke instellingen. 
```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
```
Alleen Azure als een gegevensbron voor de Azure Linux Agent toestaan door het maken van een nieuw bestand `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` met een editor naar keuze met de volgende regels:

```bash
# This configuration file is provided by the WALinuxAgent package.
datasource_list: [ Azure ]
```

Als uw bestaande Azure-installatiekopie een wisselbestand geconfigureerd heeft en u wilt wijzigen van de configuratie van de wisseling bestand voor installatiekopieën van het nieuwe met behulp van cloud-init, moet u het bestaande wisselbestand verwijderen.

Voor RedHat op basis van installatiekopieën - Volg de instructies in de volgende RedHat document waarin wordt uitgelegd hoe u [verwijderen van het wisselbestand](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/s2-swap-removing-file.html).

Voor CentOS afbeeldingen met een wisselbestand ingeschakeld, kunt u de volgende opdracht voor het uitschakelen van het wisselbestand uitvoeren:
```bash
sudo swapoff /mnt/resource/swapfile
```

Zorg ervoor dat de verwijzing van het wisselbestand wordt verwijderd uit `/etc/fstab` -deze ziet er ongeveer als de volgende uitvoer:
```text
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

U kunt ruimte besparen en verwijderen van het wisselbestand van de volgende opdracht uitvoeren:
```bash
rm /mnt/resource/swapfile
```
## <a name="extra-step-for-cloud-init-prepared-image"></a>Extra stap voor cloud-init voorbereide installatiekopie
> [!NOTE]
> Als uw installatiekopie eerder is een **cloud init** installatiekopie voorbereid en geconfigureerd, moet u de volgende stappen uit te voeren.

De volgende drie opdrachten worden alleen gebruikt als de virtuele machine aanpassen om te worden van een nieuwe gespecialiseerde bronafbeelding is eerder zijn ingericht door cloud init.  U hoeft niet te deze uitvoeren als uw installatiekopie is geconfigureerd met behulp van de Azure Linux Agent.

```bash
sudo rm -rf /var/lib/cloud/instances/* 
sudo rm -rf /var/log/cloud-init*
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Linux-Agent voltooien instelling 
Alle installatiekopieën van het Azure-platform hebt de Azure Linux Agent is geïnstalleerd, ongeacht als deze is geconfigureerd door cloud init of niet.  Voer de volgende opdracht voor het voltooien van de gebruiker van de Linux-machine opheffen van inrichting. 

```bash
sudo waagent -deprovision+user -force
```

Zie voor meer informatie over de Azure Linux Agent identiteitsgegevens opdrachten de [Azure Linux Agent](agent-user-guide.md) voor meer informatie.

De SSH-sessie te sluiten en vervolgens vanuit uw bash-shell de volgende opdrachten AzureCLI ongedaan gemaakt, generaliseren en maken van een nieuwe Azure VM-installatiekopie.  Vervang `myResourceGroup` en `sourceVmName` met de juiste informatie uw sourceVM opgetreden bij het weergeven.

```bash
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer cloud-init voorbeelden van wijzigingen in de configuratie, de volgende:
 
- [Een extra Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Voer een Pakketbeheer voor het bijwerken van bestaande pakketten op de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale hostnaam VM](cloudinit-update-vm-hostname.md) 
- [Installeren van een toepassingspakket, het bijwerken van de configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
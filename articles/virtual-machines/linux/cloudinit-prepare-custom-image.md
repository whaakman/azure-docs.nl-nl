---
title: Azure VM-installatiekopie voorbereiden voor gebruik met cloud-init | Microsoft Docs
description: De installatiekopie van een bestaande virtuele Azure-machine voorbereiden voor implementatie met cloud-init
services: virtual-machines-linux
documentationcenter: ''
author: danis
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/27/2019
ms.author: danis
ms.openlocfilehash: da539a5bebc1613115f89a7b47c513ce486b5e3a
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337308"
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Een bestaande installatiekopie van Linux virtuele Azure-machine voorbereiden voor gebruik met cloud-init
Dit artikel ziet u hoe u een bestaande virtuele Azure-machine voorbereiden voor het opnieuw geïmplementeerde zijn en klaar voor gebruik van cloud-init. De afbeelding kan worden gebruikt om een nieuwe virtuele machine of virtuele-machineschaalsets - die allebei kan vervolgens verder worden aangepast door cloud-init tijdens de implementatie te implementeren.  Deze cloud-init-scripts uitvoeren op de eerste keer opstarten nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud-init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud-init-overzicht](using-cloud-init.md)

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u al een actieve Azure virtuele machine waarop een ondersteunde versie van de Linux-besturingssysteem wordt uitgevoerd. U hebt al geconfigureerd met de machine aan de behoeften van uw behoeften, de vereiste modules geïnstalleerd, de vereiste updates verwerkt en getest om te controleren of deze voldoet aan uw vereisten. 

## <a name="preparing-rhel-76--centos-76"></a>Voorbereiden 7.6 RHEL / CentOS 7,6
U moet SSH aan bij uw Linux-VM en voer de volgende opdrachten om te kunnen installeren van cloud-init.

```bash
sudo yum makecache fast
sudo yum install -y gdisk cloud-utils-growpart
sudo yum install - y cloud-init 
```

Update de `cloud_init_modules` in sectie `/etc/cloud/cloud.cfg` om op te nemen van de volgende modules:
```bash
- disk_setup
- mounts
```

Hier volgt een voorbeeld van wat een algemene `cloud_init_modules` sectie ziet eruit als.
```bash
cloud_init_modules:
 - migrator
 - bootcmd
 - write-files
 - growpart
 - resizefs
 - disk_setup
 - mounts
 - set_hostname
 - update_hostname
 - update_etc_hosts
 - rsyslog
 - users-groups
 - ssh
```
Een aantal taken met betrekking tot inrichting en de afhandeling van tijdelijke schijven moet worden bijgewerkt `/etc/waagent.conf`. Voer de volgende opdrachten om bij te werken van de toepasselijke instellingen. 
```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
cp /lib/systemd/system/waagent.service /etc/systemd/system/waagent.service
sed -i 's/After=network-online.target/WantedBy=cloud-init.service\\nAfter=network.service systemd-networkd-wait-online.service/g' /etc/systemd/system/waagent.service
systemctl daemon-reload
cloud-init clean
```
Alleen Azure als een gegevensbron voor de Azure Linux Agent toestaan door het maken van een nieuw bestand `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` met behulp van een editor naar keuze met de volgende regels:

```bash
# Azure Data Source config
datasource_list: [ Azure ]
datasource:
   Azure:
     agent_command: [systemctl, start, waagent, --no-block]
```

Als uw bestaande Azure-installatiekopie een wisselbestand geconfigureerd heeft en u wilt wijzigen van de configuratie van het bestand wisselen voor nieuwe installatiekopieën van cloud-init gebruiken, moet u het bestaande swap-bestand te verwijderen.

Voor Red Hat op basis van installatiekopieën - Volg de instructies in de volgende Red Hat document waarin wordt uitgelegd hoe u [verwijderen van het wisselbestand](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/swap-removing-file).

CentOS-installatiekopieën met wisselbestand ingeschakeld, kunt u de volgende opdracht om het uitschakelen van het wisselbestand uitvoeren:
```bash
sudo swapoff /mnt/resource/swapfile
```

Zorg ervoor dat de verwijzing van het wisselbestand is verwijderd uit het `/etc/fstab` -dit ziet er ongeveer als in het volgende:
```text
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

U kunt de volgende opdracht uitvoeren om ruimte te maken en verwijderen van het wisselbestand:
```bash
rm /mnt/resource/swapfile
```
## <a name="extra-step-for-cloud-init-prepared-image"></a>Een extra stap voor cloud-init voorbereide installatiekopie
> [!NOTE]
> Als uw installatiekopie eerder is een **cloud-init** afbeelding voorbereid en geconfigureerd, moet u de volgende stappen uit.

De volgende drie opdrachten worden alleen gebruikt als de virtuele machine aanpassen om te worden van een nieuwe gespecialiseerde broninstallatiekopie eerder is ingericht door cloud-init.  U hoeft niet te deze uitvoeren als uw installatiekopie is geconfigureerd met behulp van de Azure Linux Agent.

```bash
sudo cloud-init clean --logs
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Linux-Agent wordt voltooid instellen 
Alle Azure-platform-installatiekopieën hebben de Azure Linux Agent is geïnstalleerd, ongeacht als deze is geconfigureerd met cloud-init of niet.  Voer de volgende opdracht om te voltooien van de inrichting van de gebruiker van de Linux-machine. 

```bash
sudo waagent -deprovision+user -force
```

Zie voor meer informatie over de opdrachten van Azure Linux Agent inrichting, het [Azure Linux Agent](../extensions/agent-linux.md) voor meer informatie.

Sluit de SSH-sessie af en klik vanuit de bash-shell en voer de volgende Azure CLI-opdrachten toewijzing ongedaan maken, het generaliseren en het maken van een nieuwe Azure-VM-installatiekopie.  Vervang `myResourceGroup` en `sourceVmName` met de juiste informatie zetten op basis van uw sourceVM.

```bash
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Volgende stappen
Voor extra cloud-init-voorbeelden van wijzigingen in de configuratie, Zie de volgende:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)

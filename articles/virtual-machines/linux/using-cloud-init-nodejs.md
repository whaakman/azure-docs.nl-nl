---
title: Gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken in Azure | Microsoft Docs
description: Het gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van de Azure CLI 1.0
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: 0b6150bca333188666935b3c9aa02c4b33690db9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation-with-the-azure-cli-10"></a>Cloud-init gebruiken voor het aanpassen van een Linux-VM tijdens het maken van de Azure CLI 1.0
Dit artikel laat zien hoe u een cloud-init-script de hostnaam, geïnstalleerd updatepakketten instellen en beheren van gebruikersaccounts.  De cloud init-scripts worden aangeroepen tijdens het maken van de virtuele machine van Azure CLI.  Het artikel schrijft het volgende als vereiste voor:

* een Azure-account ([probeer een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure-CLI](../../cli-install-nodejs.md) die is aangemeld bij `azure login`.
* de Azure-CLI *moet in de* Azure Resource Manager-modus`azure config mode arm` staan.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="quick-commands"></a>Snelle opdrachten
Een cloud-init.txt-script dat Hiermee stelt u de hostnaam, updates van alle pakketten en voegt een sudo-gebruiker toe aan Linux maken.

```sh
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Maak een resourcegroep om te starten van virtuele machines in.

```azurecli
azure group create myResourceGroup westus
```

Maak een Linux-VM met behulp van cloud-init te configureren tijdens opstarten.

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
### <a name="introduction"></a>Inleiding
Wanneer u een nieuwe Linux VM start, kunt u een standaard Linux-VM met niets wel aangepaste of gereed voor uw behoeften. [Cloud-init](https://cloudinit.readthedocs.org) is een standaard manier om een script of configuratie-instellingen in die Linux VM invoeren als deze voor de eerste keer wordt opgestart.

In Azure, er zijn drie manieren om wijzigingen te brengen naar een Linux-VM als dit wordt geïmplementeerd of opgestart.

* Scripts met cloud-init invoeren.
* Scripts met de Azure injecteren [VMAccess-extensie](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Een Azure-sjabloon met behulp van cloud-init.
* Een Azure-sjabloon met [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Scripts op elk gewenst moment na opstarten invoeren:

* SSH opdrachten rechtstreeks uitvoeren
* Scripts met de Azure injecteren [VMAccess-extensie](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively of in een Azure-sjabloon
* Configuration management-hulpprogramma's zoals Ansible, Salt of Chef of Puppet.

> [!NOTE]
> : Een script wordt uitgevoerd als hoofdmap op dezelfde manier via SSH in VMAccess-extensie.  Echter, met behulp van de VM-extensie kan verschillende functies die Azure biedt die nuttig is afhankelijk van uw scenario kunnen zijn.
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Cloud-init beschikbaarheid op Azure VM snelle invoer installatiekopie aliassen:
| Alias | Uitgever | Aanbieding | SKU | Versie | cloud-init |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |meest recente |nee |
| CoreOS |CoreOS |CoreOS |Stabiel |meest recente |ja |
| Debian |credativ |Debian |8 |meest recente |nee |
| openSUSE |SUSE |openSUSE |13.2 |meest recente |nee |
| RHEL |RedHat |RHEL |7.2 |meest recente |nee |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |meest recente |ja |

Microsoft werkt samen met onze partners ophalen van cloud-init opgenomen en in de afbeeldingen die ze naar Azure bieden werkt.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Toevoegen van een cloud-init-script voor het maken van de virtuele machine met de Azure CLI
Als u wilt een cloud-init-script wordt gestart bij het maken van een virtuele machine in Azure, geeft u het cloud-init-bestand met de Azure CLI `--custom-data` overschakelen.

Maak een resourcegroep om te starten van virtuele machines in.

```azurecli
azure group create myResourceGroup westus
```

Maak een Linux-VM met behulp van cloud-init te configureren tijdens opstarten.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Maken van een cloud-init-script voor het instellen van de hostnaam van een Linux-VM
Een van de eenvoudigste en meest belangrijke instellingen voor een Linux-VM is de hostnaam. We kunnen eenvoudig Stel dit met behulp van cloud-init met dit script.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Voorbeeld van de cloud init script met de naam `cloud_config_hostname.txt`.
```sh
#cloud-config
hostname: myservername
```

Dit script cloud init wordt tijdens het opstarten van de virtuele machine, de hostnaam ingesteld op `myservername`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

Aanmelding en controleer of de hostnaam van de nieuwe virtuele machine.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Maken van een cloud-init-script voor het bijwerken van Linux
Voor beveiliging moet uw Ubuntu VM bijwerken voor het eerst wordt opgestart.  Met behulp van cloud-init die we doen dat met het script volgen, afhankelijk van de Linux-distributie die u gebruikt.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Cloud-init voorbeeldscript `cloud_config_apt_upgrade.txt` voor de Debian-familie
```sh
#cloud-config
apt_upgrade: true
```

Nadat Linux is opgestart, de geïnstalleerde pakketten worden bijgewerkt via `apt-get`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_apt_upgrade.txt
```

Aanmelding en controleer of alle pakketten worden bijgewerkt.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Maken van een cloud-init-script om een gebruiker toevoegen aan Linux
Een van de eerste taken op een nieuwe Linux-VM is een gebruiker toevoegen voor uzelf of te vermijden met behulp van `root`. SSH-sleutels zijn best practice voor beveiliging en bruikbaarheid en ze worden toegevoegd aan de `~/.ssh/authorized_keys` bestand met dit script cloud init.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Cloud-init voorbeeldscript `cloud_config_add_users.txt` voor Debian-familie
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Nadat Linux is opgestart, worden alle vermelde gebruikers gemaakt en toegevoegd aan de sudo-groep.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_add_users.txt
```

Aanmelding en controleer of de nieuwe gebruiker.

```bash
ssh myVM
cat /etc/group
```

Uitvoer

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Volgende stappen
Cloud-init steeds één standaardmethode voor het wijzigen van uw Linux VM op opstarten. Azure heeft ook een VM-extensies wijzigen van uw LinuxVM op opstarten of kunnen terwijl deze wordt uitgevoerd. Bijvoorbeeld, kunt u de Azure-VMAccessExtension opnieuw instellen van SSH of gebruikersgegevens terwijl de virtuele machine wordt uitgevoerd. Met cloud-init moet u opnieuw het wachtwoord opnieuw wilt opstarten.

[Over functies en uitbreidingen van de virtuele machine](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Beheren van gebruikers, SSH en controleer of het herstellen van schijven op Azure Linux VM's met behulp van de VMAccess-extensie](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


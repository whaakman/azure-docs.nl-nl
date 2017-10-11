---
title: Opnieuw instellen op Azure Linux VM's met behulp van de VMAccess-extensie | Microsoft Docs
description: Opnieuw instellen op Azure Linux VM's met behulp van de VMAccess-extensie.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 278bf1785aac71068ab94cf9916af69a204c44be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-10"></a>Beheren van gebruikers, SSH en controleer of het herstellen van schijven op Azure Linux VM's met behulp van de VMAccess-extensie met de Azure CLI 1.0
In dit artikel laat zien hoe de uitbreiding van de VMAcesss Azure gebruiken om te controleren of herstellen van een schijf, gebruikerstoegang opnieuw instellen, beheren van gebruikersaccounts of instellen van de configuratie SSHD op Linux. Het artikel schrijft het volgende als vereiste voor:

* een Azure-account ([probeer een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure-CLI](../../cli-install-nodejs.md) die is aangemeld bij `azure login`.
* de Azure-CLI *moet in de* Azure Resource Manager-modus`azure config mode arm` staan.


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands)– onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten
Er zijn twee manieren VMAccess op uw virtuele Linux-machines gebruiken:

* Met behulp van de Azure CLI 1.0 en de vereiste parameters.
* Met behulp van onbewerkte JSON-bestanden die VMAccess verwerkt en klik vervolgens op te volgen.

Voor het gedeelte met snelle opdrachten gaan we gebruik van de Azure CLI 1.0 `azure vm reset-access` methode. Vervang in de volgende opdrachtvoorbeelden de waarden die "voorbeeld" met de waarden van uw eigen omgeving bevatten.

## <a name="create-a-resource-group-and-linux-vm"></a>Een resourcegroep en een virtuele Linux-machine maken
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Een Debian virtuele machine maken
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a>Root-wachtwoord opnieuw instellen
Het root-wachtwoord opnieuw instellen:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH-sleutel opnieuw instellen
Opnieuw instellen van een gebruiker niet de hoofdmap van de SSH-sleutel:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Een gebruiker maken
Een gebruiker maken:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a>Een gebruiker verwijderen
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD opnieuw instellen
De SSHD-configuratie opnieuw instellen:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
### <a name="vmaccess-defined"></a>VMAccess gedefinieerd:
De schijf op uw Linux-VM worden fouten weergegeven. U enigszins het root-wachtwoord opnieuw instellen voor uw Linux-VM of uw persoonlijke SSH-sleutel per ongeluk worden verwijderd. Als dat is gebeurd terug in de dagen van het datacenter, zou u moet er station en open vervolgens de KVM ophalen via de serverconsole. De Azure-VMAccess-extensie beschouwen als die KVM-switch waarmee u toegang tot de console opnieuw instellen naar Linux of voer schijfonderhoud niveau.

We gaan de lang VMAccess dat gebruikmaakt van onbewerkte JSON-bestanden gebruikt voor de gedetailleerde walktrough.  Deze VMAccess JSON-bestanden kunnen ook worden aangeroepen vanuit de Azure-sjablonen.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>U vmaccess gebruikt om te controleren of herstellen van de schijf van een Linux-VM
U vmaccess gebruikt kunt u een fsck doen uitvoeren op de schijf onder uw Linux-VM.  U kunt ook een schijf controleren en de herstellen van een schijf met behulp van een VMAccess doen.

Om te controleren en herstel daarna de schijf gebruikmaken van dit script vmaccess gebruikt:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Voer het script VMAccess met:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>U vmaccess gebruikt gebruikerstoegang opnieuw wordt ingesteld op Linux
Als u hebt geen toegang meer tot hoofdmap op uw Linux-VM, kunt u een script vmaccess gebruikt als de root-wachtwoord opnieuw wilt starten.

Voor het root-wachtwoord opnieuw instellen, gebruikt u dit script VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

Voer het script VMAccess met:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

Om in te stellen de SSH-sleutel van een niet-hoofdgebruiker, gebruikt u dit script vmaccess gebruikt:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

Voer het script VMAccess met:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>U vmaccess gebruikt voor het beheren van gebruikersaccounts op Linux
VMAccess is een pythonscript dat kan worden gebruikt voor het beheren van gebruikers op uw Linux-VM zonder aanmelden en het gebruik van sudo of het root-account.

Gebruik dit script vmaccess gebruikt voor het maken van een gebruiker:

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Voer het script VMAccess met:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

Gebruik dit script vmaccess gebruikt voor het verwijderen van een gebruiker:

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

Voer het script VMAccess met:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>U vmaccess gebruikt de SSHD-configuratie opnieuw instellen
Als u wijzigingen in de configuratie van de Linux-machines SSHD aanbrengen en de SSH-verbinding sluit voordat de wijzigingen te controleren, u kan worden voorkomen dat SSH'ing weer in.  VMAccess kan worden gebruikt om de SSHD-configuratie opnieuw instellen naar een bekende juiste configuratie zonder worden geregistreerd in via SSH.

De configuratie van de SSHD gebruiken om in te stellen dit script vmaccess gebruikt:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Voer het script VMAccess met:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Volgende stappen
Bijwerken van Linux, met behulp van Azure VMAccess extensies één methode te wijzigen op een actieve Linux VM is.  U kunt ook hulpprogramma's zoals cloud init en Azure-sjablonen gebruiken om te wijzigen van uw Linux VM op opstarten.

[Over functies en uitbreidingen van de virtuele machine](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure Resource Manager-sjablonen met Linux VM-extensies](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


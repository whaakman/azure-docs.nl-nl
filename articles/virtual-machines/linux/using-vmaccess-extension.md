---
title: Toegang instellen voor een Azure Linux VM | Microsoft Docs
description: Het beheren van gebruikers met beheerdersrechten en -toegang op de virtuele Linux-machines met behulp van de VMAccess-extensie en de Azure CLI 2.0 opnieuw instellen
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.openlocfilehash: 15fab2b4f83b86227a3a2327669861b5adc8d3c5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915140"
---
# <a name="manage-administrative-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a>Beheren van gebruikers met beheerdersrechten, SSH en controleer of het herstellen van schijven op virtuele Linux-machines met behulp van de VMAccess-extensie met de Azure CLI 2.0
De schijf op uw Linux-VM worden fouten weergegeven. U enigszins het root-wachtwoord opnieuw instellen voor uw Linux-VM of uw persoonlijke SSH-sleutel per ongeluk worden verwijderd. Als dat is gebeurd terug in de dagen van het datacenter, zou u moet er station en open vervolgens de KVM ophalen via de serverconsole. De Azure-VMAccess-extensie beschouwen als die KVM-switch waarmee u toegang tot de console opnieuw instellen naar Linux of voer schijfonderhoud niveau.

In dit artikel laat zien hoe de VMAccess-extensie van Azure gebruiken om te controleren of herstellen van een schijf, gebruikerstoegang opnieuw instellen, beheren van accounts voor gebruikers met beheerdersrechten of bijwerken van de SSH-configuratie op Linux. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="ways-to-use-the-vmaccess-extension"></a>Manieren om te gebruiken de VMAccess-extensie
Er zijn twee manieren waarop u de VMAccess-extensie op uw virtuele Linux-machines kan gebruiken:

* Gebruik de Azure CLI 2.0 en de vereiste parameters.
* [Gebruik onbewerkte JSON-bestanden die de VMAccess-extensie verwerken](#use-json-files-and-the-vmaccess-extension) en klik vervolgens op te volgen.

Gebruik de volgende voorbeelden [az vm gebruiker](/cli/azure/vm/user) opdrachten. Als u wilt deze stappen uitvoert, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login).

## <a name="update-ssh-key"></a>SSH-sleutel bijwerken
Het volgende voorbeeld de SSH-sleutel voor de gebruiker bijgewerkt `azureuser` op de virtuele machine met de naam `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

> **Opmerking:** de `az vm user update` opdracht wordt de nieuwe openbare sleutel tekst naar de `~/.ssh/authorized_keys` -bestand voor de gebruiker met beheerdersrechten op de virtuele machine. Hierdoor niet vervangen of verwijder bestaande SSH-sleutels. Hiermee wordt voorafgaande sleutels die zijn ingesteld op moment van implementatie- of daaropvolgende updates via de VMAccess-extensie niet verwijderd.

## <a name="reset-password"></a>Wachtwoord opnieuw instellen
Het wachtwoord voor de gebruiker opnieuw instellen van het volgende voorbeeld `azureuser` op de virtuele machine met de naam `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>SSH opnieuw starten
Het volgende voorbeeld de SSH-daemon opnieuw is opgestart en de SSH-configuratie opnieuw instellen naar standaardwaarden op een virtuele machine met de naam `myVM`:

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-an-administrativesudo-user"></a>Maken van een gebruiker met beheerdersrechten/sudo
Het volgende voorbeeld wordt een gebruiker met de naam `myNewUser` met **sudo** machtigingen. Het account een SSH-sleutel gebruikt voor verificatie op de virtuele machine met de naam `myVM`. Deze methode is ontworpen om u te helpen u weer toegang tot een virtuele machine in het geval dat de referenties van huidige zijn kwijtraakt of vergeet. Als een best practice van accounts met **sudo** machtigingen moeten worden beperkt.

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```



## <a name="delete-a-user"></a>Een gebruiker verwijderen
Het volgende voorbeeld wordt een gebruiker met de naam `myNewUser` op de virtuele machine met de naam `myVM`:

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a>Gebruik JSON-bestanden en de VMAccess-extensie
De volgende voorbeelden onbewerkte JSON-bestanden gebruiken. Gebruik [az vm-extensie set](/cli/azure/vm/extension#az_vm_extension_set) aan te roepen vervolgens uw JSON-bestanden. Deze JSON-bestanden kunnen ook worden aangeroepen vanuit de Azure-sjablonen. 

### <a name="reset-user-access"></a>Toegang van gebruikers opnieuw instellen
Als u hebt geen toegang meer tot hoofdmap op uw Linux-VM, kunt u een script vmaccess gebruikt voor het bijwerken van de SSH-sleutel van een gebruiker of het wachtwoord te starten.

Maak een bestand met de naam voor het bijwerken van de openbare SSH-sleutel van een gebruiker, `update_ssh_key.json` en -instellingen toevoegen in de volgende indeling. Vervang uw eigen waarden voor de `username` en `ssh_key` parameters:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Voer het script VMAccess met:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings update_ssh_key.json
```

Als u wilt een gebruikerswachtwoord opnieuw instelt, maakt u een bestand met de naam `reset_user_password.json` en -instellingen toevoegen in de volgende indeling. Vervang uw eigen waarden voor de `username` en `password` parameters:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Voer het script VMAccess met:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>SSH opnieuw starten
Als u wilt de SSH-daemon starten en de SSH-configuratie opnieuw instellen naar standaardwaarden, maakt u een bestand met de naam `reset_sshd.json`. Voeg de volgende inhoud:

```json
{
  "reset_ssh": true
}
```

Voer het script VMAccess met:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-administrative-users"></a>Beheren van gebruikers met beheerdersrechten

Maken van een gebruiker met **sudo** machtigingen die gebruikmaakt van een SSH-sleutel voor verificatie, maak een bestand met de naam `create_new_user.json` en -instellingen toevoegen in de volgende indeling. Vervang uw eigen waarden voor de `username` en `ssh_key` parameters. Deze methode is ontworpen om u te helpen u weer toegang tot een virtuele machine in het geval dat de referenties van huidige zijn kwijtraakt of vergeet. Als een best practice van accounts met **sudo** machtigingen moeten worden beperkt.

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Voer het script VMAccess met:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Als u wilt een gebruiker verwijdert, maakt u een bestand met de naam `delete_user.json` en voegt u de volgende inhoud. Vervangen door uw eigen waarde voor de `remove_user` parameter:

```json
{
  "remove_user":"myNewUser"
}
```

Voer het script VMAccess met:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a>Controleer of het herstellen van de schijf
U vmaccess gebruikt kunt u ook controleren en herstellen van een schijf die u hebt toegevoegd aan de Linux-VM.

Om te controleren en herstel daarna de schijf, maakt u een bestand met de naam `disk_check_repair.json` en -instellingen toevoegen in de volgende indeling. Vervangen door uw eigen waarde voor de naam van `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Voer het script VMAccess met:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a>Volgende stappen
Bijwerken van Linux via de Azure-VMAccess-extensie is een methode wijzigingen aanbrengen in een actieve Linux VM. U kunt ook hulpprogramma's zoals cloud init en Azure Resource Manager-sjablonen gebruiken om te wijzigen van uw Linux VM op opstarten.

[Uitbreidingen van de virtuele machine en functies voor Linux](extensions-features.md)

[Azure Resource Manager-sjablonen met Linux VM-extensies](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van](using-cloud-init.md)


---
title: Toegang opnieuw instellen met een Azure Linux-VM | Microsoft Docs
description: Over het beheren van gebruikers met beheerdersrechten en op virtuele Linux-machines met behulp van de VMAccess-extensie en de Azure CLI-toegang opnieuw instellen
services: virtual-machines-linux
documentationcenter: ''
author: zroiy
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2018
ms.author: roiyz
ms.openlocfilehash: e878f5c9f923b55a1eb94cefb1ecf021c81e884e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998624"
---
# <a name="manage-administrative-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli"></a>Beheren van gebruikers met beheerdersrechten, SSH en controleer of het herstellen van schijven op virtuele Linux-machines met behulp van de VMAccess-extensie met de Azure CLI
## <a name="overview"></a>Overzicht
De schijf op uw Linux-VM worden fouten weergegeven. U loopt de toepassing het root-wachtwoord opnieuw instellen voor uw Linux-VM of uw persoonlijke SSH-sleutel per ongeluk worden verwijderd. Als dat is gebeurd terug in de dagen van het datacenter, moet u zou er station en open vervolgens de KVM om op te halen op de serverconsole. De Azure VMAccess-extensie beschouwen als die KVM-switch waarmee u toegang tot de console voor het opnieuw instellen van toegang tot Linux of schijf niveau onderhoud uit te voeren.

Dit artikel ziet u hoe u de Azure VMAccess-extensie gebruiken om te controleren of herstellen van een schijf, gebruikerstoegang opnieuw instellen, beheren van de gebruiker met beheerdersrechten accounts of de SSH-configuratie op Linux bijwerken wanneer ze worden uitgevoerd als virtuele machines van Azure Resource Manager. Als u nodig hebt voor het beheren van klassieke virtuele machines - kunt u Volg de instructies in de [documentatie voor klassieke VM's](../linux/classic/reset-access-classic.md). 

## <a name="prerequisites"></a>Vereisten
### <a name="operating-system"></a>Besturingssysteem

De extensie voor VM-toegang kan voor deze Linux-distributies worden uitgevoerd:

| Distributie | Versie |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS en 12.04 LTS |
| Debian | Debian 7,9 +, 8.2 + |
| Red Hat | RHEL 6.7 +, 7.1 + |
| Oracle Linux | 6.4+, 7.0+ |
| SUSE | 11 en 12 |
| OpenSuse | openSUSE Leap 42.2 + |
| CentOS | CentOS 6.3+, 7.0+ |
| CoreOS | 494.4.0+ |

## <a name="ways-to-use-the-vmaccess-extension"></a>Manieren waarop u met de VMAccess-extensie
Er zijn twee manieren waarop u de VMAccess-extensie op uw Linux-VM's gebruiken kunt:

* Gebruik de Azure CLI en de vereiste parameters.
* [Gebruik van onbewerkte JSON-bestanden die de VMAccess-extensie verwerken](#use-json-files-and-the-vmaccess-extension) en vervolgens actie ondernemen op.

De volgende voorbeelden gebruiken [az vm gebruiker](/cli/azure/vm/user) opdrachten. Als u wilt deze stappen uitvoert, moet u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

## <a name="update-ssh-key"></a>SSH-sleutel bijwerken
Het volgende voorbeeld wordt de SSH-sleutel voor de gebruiker bijgewerkt `azureuser` op de virtuele machine met de naam `myVM`:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

> **Opmerking:** de `az vm user update` opdracht wordt de nieuwe openbare sleutel tekst naar de `~/.ssh/authorized_keys` -bestand voor de gebruiker met beheerdersrechten op de virtuele machine. Dit niet vervangen of verwijderen van bestaande SSH-sleutels. Hiermee wordt eerdere sleutels die zijn ingesteld op implementatie of een latere updates via de VMAccess-extensie niet verwijderd.

## <a name="reset-password"></a>Wachtwoord opnieuw instellen
Het volgende voorbeeld wordt het wachtwoord voor de gebruiker `azureuser` op de virtuele machine met de naam `myVM`:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>SSH opnieuw starten
Het volgende voorbeeld wordt de SSH-daemon opnieuw opgestart en de SSH-configuratie opnieuw instellen naar standaardwaarden op een virtuele machine met de naam `myVM`:

```azurecli-interactive
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-an-administrativesudo-user"></a>Maakt u een gebruiker met beheerdersrechten/sudo
Het volgende voorbeeld wordt een gebruiker met de naam `myNewUser` met **sudo** machtigingen. Het account een SSH-sleutel gebruikt voor verificatie op de virtuele machine met de naam `myVM`. Deze methode is ontworpen om u te helpen u weer toegang krijgen tot een virtuele machine in het geval dat huidige referenties zijn kwijtraakt of vergeet. Als een best practice-accounts met **sudo** machtigingen moeten worden beperkt.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Een gebruiker verwijderen
Het volgende voorbeeld wordt een gebruiker met de naam `myNewUser` op de virtuele machine met de naam `myVM`:

```azurecli-interactive
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```

## <a name="use-json-files-and-the-vmaccess-extension"></a>JSON-bestanden en de VMAccess-extensie gebruiken
De volgende voorbeelden gebruiken de onbewerkte JSON-bestanden. Gebruik [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set) om aan te roepen vervolgens uw JSON-bestanden. Deze JSON-bestanden kunnen ook worden aangeroepen vanuit de Azure-sjablonen. 

### <a name="reset-user-access"></a>Gebruikerstoegang opnieuw instellen
Als u hebt niet langer toegang tot de hoofdmap van uw Linux-VM, kunt u een VMAccess-script voor het bijwerken van de SSH-sleutel of het wachtwoord van een gebruiker kunt starten.

Voor het bijwerken van de openbare SSH-sleutel van een gebruiker, maakt u een bestand met de naam `update_ssh_key.json` en -instellingen toevoegen in de volgende indeling. Vervangt door uw eigen waarden voor de `username` en `ssh_key` parameters:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Voer het script VMAccess met:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings update_ssh_key.json
```

Als u wilt een gebruikerswachtwoord opnieuw instelt, maakt u een bestand met de naam `reset_user_password.json` en -instellingen toevoegen in de volgende indeling. Vervangt door uw eigen waarden voor de `username` en `password` parameters:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Voer het script VMAccess met:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>SSH opnieuw starten
Voor het opnieuw opstarten van de SSH-daemon en de SSH-configuratie opnieuw instellen naar standaardwaarden, maakt u een bestand met de naam `reset_sshd.json`. Voeg de volgende inhoud:

```json
{
  "reset_ssh": true
}
```

Voer het script VMAccess met:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-administrative-users"></a>Beheren van gebruikers met beheerdersrechten

Het maken van een gebruiker met **sudo** machtigingen die gebruikmaakt van een SSH-sleutel voor verificatie, maak een bestand met de naam `create_new_user.json` en -instellingen toevoegen in de volgende indeling. Vervangt door uw eigen waarden voor de `username` en `ssh_key` parameters. Deze methode is ontworpen om u te helpen u weer toegang krijgen tot een virtuele machine in het geval dat huidige referenties zijn kwijtraakt of vergeet. Als een best practice-accounts met **sudo** machtigingen moeten worden beperkt.

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Voer het script VMAccess met:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Als u wilt verwijderen van een gebruiker, maakt u een bestand met de naam `delete_user.json` en voeg de volgende inhoud toe. Vervangen door uw eigen waarde voor de `remove_user` parameter:

```json
{
  "remove_user":"myNewUser"
}
```

Voer het script VMAccess met:

```azurecli-interactive
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

Als u wilt controleren en vervolgens de schijf te herstellen, maakt u een bestand met de naam `disk_check_repair.json` en -instellingen toevoegen in de volgende indeling. Vervangen door uw eigen waarde voor de naam van `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Voer het script VMAccess met:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```
## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

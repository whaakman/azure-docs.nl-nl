---
title: In Azure een SSH-sleutelpaar maken en gebruiken voor virtuele Linux-machines | Microsoft Docs
description: Het maken en gebruiken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure om de beveiliging van het verificatieproces te verbeteren.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: cynthn
ms.openlocfilehash: 2e3d86d776f44c47a33bf075cf7f2140a3940e5e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928451"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Snelle stappen: maken en gebruiken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure
Met een SSH-sleutelpaar (secure shell) kunt u virtuele machines (VM's) in Azure maken die voor verificatie gebruikmaken van SSH-sleutels, waardoor aanmelding met een wachtwoord niet meer nodig is. Dit artikel ziet u hoe u snel genereren en het gebruik van een sleutelpaar openbare en persoonlijke-sleutelbestand met SSH voor virtuele Linux-machines. U kunt deze stappen voltooien met de Azure Cloud Shell, een macOS of Linux-host, de Windows-subsysteem voor Linux en andere hulpprogramma's die ondersteuning bieden voor OpenSSH. 

Zie voor meer achtergrondinformatie en voorbeelden, [gedetailleerde stappen voor het maken van SSH-sleutelparen](create-ssh-keys-detailed.md).

Zie voor aanvullende manieren om te genereren en het gebruik van SSH-sleutels op een Windows-computer, [over het gebruik van SSH-sleutels met Windows op Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
Gebruik de `ssh-keygen` opdracht voor het genereren van SSH openbare en persoonlijke sleutelbestanden die standaard gemaakt in de `~/.ssh` directory. Kunt u een andere locatie en een extra wachtwoordzin (een wachtwoord voor toegang tot het bestand met persoonlijke sleutel) als u hierom wordt gevraagd. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven.

```bash
ssh-keygen -t rsa -b 2048
```

Als u de [Azure CLI 2.0](/cli/azure) voor het maken van uw virtuele machine, kunt u eventueel SSH openbare en persoonlijke sleutelbestanden genereren door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--generate-ssh-keys` optie. De sleutels worden opgeslagen in de map ~/.ssh. Houd er rekening mee dat deze optie niet sleutels overschreven als deze al bestaan op die locatie.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Openbare SSH-sleutel opgeven bij het implementeren van een virtuele machine
Geef uw openbare SSH-sleutel voor het maken van een Linux-VM die gebruikmaakt van SSH-sleutels voor verificatie bij het maken van de virtuele machine met behulp van Azure portal, CLI, Resource Manager-sjablonen of andere methoden:

* [Een virtuele Linux-machine maken met de Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-machine maken met de Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-VM met behulp van een Azure-sjabloon maken](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Als u niet bekend met de indeling van een openbare SSH-sleutel bent, kunt u de openbare sleutel bekijken door te voeren `cat` als volgt vervangen `~/.ssh/id_rsa.pub` met de locatie van uw eigen openbare-sleutelbestand:

```bash
cat ~/.ssh/id_rsa.pub
```

Als u de inhoud van het te gebruiken openbare-sleutelbestand kopieert en plakt in Azure Portal of een Resource Manager-sjabloon, moet u ervoor zorgen dat u geen extra witruimte kopieert. Als u Mac OS, kunt u bijvoorbeeld een bestand met de openbare sleutel doorgeven (standaard `~/.ssh/id_rsa.pub`) naar **pbcopy** om de inhoud te kopiëren (Er zijn andere Linux-programma's die hetzelfde, zoals doen **xclip**).

De openbare sleutel die u op uw Linux-VM in Azure plaatst, wordt standaard opgeslagen `~/.ssh/id_rsa.pub`, tenzij u de locatie gewijzigd tijdens het maken van de sleutels. Als u de [Azure CLI 2.0](/cli/azure) opgeven voor het maken van uw virtuele machine met een bestaande openbare sleutel, de waarde of de locatie van deze openbare sleutel door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--ssh-key-value` optie. 

## <a name="ssh-to-your-vm"></a>SSH naar uw virtuele machine
Met de openbare sleutel die is geïmplementeerd op uw Azure-VM en de persoonlijke sleutel op uw lokale systeem, SSH verbinding met uw virtuele machine via het IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht uit met de naam van de beheerder-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt opgegeven, moet u deze tijdens het aanmeldingsproces desgevraagd invoeren. (De server is toegevoegd aan uw map `~/.ssh/known_hosts` en u wordt pas weer gevraagd om verbinding te maken nadat de openbare sleutel op uw virtuele Azure-machine is gewijzigd of de naam van de server is verwijderd uit `~/.ssh/known_hosts`.)

Virtuele machines die zijn gemaakt met behulp van SSH-sleutels, zijn standaard geconfigureerd met uitgeschakelde wachtwoorden, waardoor brute force-aanvallen om wachtwoorden in handen te krijgen veel duurder en dus ook lastiger worden. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven in het maken van een eenvoudige SSH-sleutelpaar voor snel gebruik. 

* Als u meer hulp nodig bij het werken met uw SSH-sleutelpaar, Zie [gedetailleerde stappen voor het maken en beheren van SSH-sleutelparen](create-ssh-keys-detailed.md).

* Als u problemen met SSH-verbindingen met een Azure-VM hebt, raadpleegt u [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md).



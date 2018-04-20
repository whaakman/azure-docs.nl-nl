---
title: In Azure een SSH-sleutelpaar maken en gebruiken voor virtuele Linux-machines | Microsoft Docs
description: Informatie over het maken en gebruiken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure om de beveiliging van het verificatieproces te verbeteren.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: 137fb13ff286e5284b8e8910834913ec9f1d48a9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Snelle stappen: maken en gebruiken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure
Met een SSH-sleutelpaar (secure shell) kunt u virtuele machines (VM's) in Azure maken die voor verificatie gebruikmaken van SSH-sleutels, waardoor aanmelding met een wachtwoord niet meer nodig is. In dit artikel leest u hoe snel genereren en gebruiken van een SSH-sleutelbestand openbaar / persoonlijk sleutelpaar voor virtuele Linux-machines. U kunt deze stappen voltooien met de Azure-Cloud-Shell, een Mac OS of Linux-host, de Windows-subsysteem voor Linux en andere hulpprogramma's die ondersteuning bieden voor OpenSSH. 

Zie voor meer informatie over de achtergrond en voorbeelden [gedetailleerde stappen voor het maken van SSH-sleutel paren](create-ssh-keys-detailed.md).

Zie voor meer manieren om te genereren en gebruiken van SSH-sleutels op een Windows-computer [het gebruik van SSH-sleutels met Windows in Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
Gebruik de `ssh-keygen` opdracht voor het genereren van SSH openbare en persoonlijke sleutelbestanden die gemaakt zijn in standaard de `~/.ssh` directory. Kunt u een andere locatie en een extra wachtwoordzin (een wachtwoord voor toegang tot het bestand met de persoonlijke sleutel) wanneer u wordt gevraagd. Als een SSH-sleutelpaar in de huidige locatie bestaat, worden deze bestanden overschreven.

```bash
ssh-keygen -t rsa -b 2048
```

Als u de [Azure CLI 2.0](/cli/azure) voor het maken van uw virtuele machine, kunt u eventueel SSH openbare en persoonlijke sleutelbestanden genereren door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--generate-ssh-keys` optie. De sleutels worden opgeslagen in de map ~/.ssh. Houd er rekening mee dat deze optie niet sleutels overschreven als ze al aanwezig zijn op die locatie.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>Openbare SSH-sleutel bieden bij het implementeren van een virtuele machine
Geef uw openbare SSH-sleutel voor het maken van een Linux VM die gebruikmaakt van SSH-sleutels voor verificatie bij het maken van de virtuele machine met behulp van de Azure CLI-portal Resource Manager-sjablonen of andere methoden:

* [Een virtuele Linux-machine maken met de Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een virtuele Linux-machine maken met de Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Maak een Linux-VM met een Azure-sjabloon](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Als u niet bekend met de indeling van een openbare SSH-sleutel bent, ziet u de openbare sleutel door te voeren `cat` als volgt vervangen `~/.ssh/id_rsa.pub` met uw eigen locatie openbaar-sleutelbestand:

```bash
cat ~/.ssh/id_rsa.pub
```

Als u de inhoud van het te gebruiken openbare-sleutelbestand kopieert en plakt in Azure Portal of een Resource Manager-sjabloon, moet u ervoor zorgen dat u geen extra witruimte kopieert. Als u Mac OS gebruikt, kunt u bijvoorbeeld een bestand met de openbare sleutel doorsluizen (standaard `~/.ssh/id_rsa.pub`) naar **pbcopy** om de inhoud te kopiëren (Er zijn andere Linux-programma's die dezelfde dingen, zoals doen **xclip**).

De openbare sleutel die u op uw Linux-VM in Azure plaatst, wordt standaard opgeslagen in `~/.ssh/id_rsa.pub`, tenzij u de locatie gewijzigd tijdens het maken van de sleutels. Als u de [Azure CLI 2.0](/cli/azure) als uw virtuele machine maken met een bestaande openbare sleutel, geeft u de waarde of de locatie van deze openbare sleutel door het uitvoeren van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht met de `--ssh-key-value` optie. 

## <a name="ssh-to-your-vm"></a>SSH met uw virtuele machine
Met de openbare sleutel geïmplementeerd op de virtuele machine in Azure, en de persoonlijke sleutel op het lokale systeem, SSH met uw virtuele machine met behulp van de IP-adres of de DNS-naam van uw virtuele machine. Vervang *azureuser* en *myvm.westus.cloudapp.azure.com* in de volgende opdracht met de naam van de administrator-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt opgegeven, moet u deze tijdens het aanmeldingsproces desgevraagd invoeren. (De server is toegevoegd aan uw map `~/.ssh/known_hosts` en u wordt pas weer gevraagd om verbinding te maken nadat de openbare sleutel op uw virtuele Azure-machine is gewijzigd of de naam van de server is verwijderd uit `~/.ssh/known_hosts`.)

Virtuele machines die zijn gemaakt met behulp van SSH-sleutels, zijn standaard geconfigureerd met uitgeschakelde wachtwoorden, waardoor brute force-aanvallen om wachtwoorden in handen te krijgen veel duurder en dus ook lastiger worden. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt beschreven voor het maken van een eenvoudige SSH-sleutelpaar voor snelle gebruik. 

* Als u meer hulp nodig bij het werken met uw SSH-sleutelpaar, Zie [gedetailleerde stappen voor het maken en beheren van de SSH-sleutel paren](create-ssh-keys-detailed.md).

* Als u problemen met SSH-verbindingen met een virtuele machine in Azure hebt, raadpleegt u [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md).



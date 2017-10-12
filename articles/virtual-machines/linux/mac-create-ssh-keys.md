---
title: In Azure een SSH-sleutelpaar maken en gebruiken voor virtuele Linux-machines | Microsoft Docs
description: Een openbaar en persoonlijk SSH-sleutelpaar voor virtuele Linux-machines in Azure maken en gebruiken om de beveiliging van het verificatieproces te verbeteren.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 6fcdcc96c7762e2362aebf909ef25f4a5ab62f99
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Een sleutelpaar met een openbare en persoonlijke SSH-sleutel voor virtuele Linux-machines maken en gebruiken
Met een SSH-sleutelpaar (secure shell) kunt u virtuele machines (VM's) in Azure maken die voor verificatie gebruikmaken van SSH-sleutels, waardoor aanmelding met een wachtwoord niet meer nodig is. In dit artikel wordt beschreven hoe u snel een sleutelpaar met een openbare en een persoonlijke sleutel met SSH-protocol versie 2 RSA maakt en gebruikt voor virtuele Linux-machines. Zie voor meer stappen en extra voorbeelden [Gedetailleerde stappen voor het maken van SSH-sleutelparen en certificaten](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken
Gebruik de opdracht `ssh-keygen` om openbare en persoonlijke SSH-sleutelbestanden te maken die standaard gemaakt worden in de map `~/.ssh`. U kunt een andere locatie en een extra wachtwoordzin (een wachtwoord voor toegang tot het bestand met de persoonlijke sleutel) opgeven wanneer u hierom wordt gevraagd. Voer de volgende opdracht uit vanuit een Bash-shell en beantwoord de prompts met uw eigen gegevens.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a>Het SSH-sleutelpaar gebruiken
De openbare sleutel die u op de virtuele Linux-machine in Azure plaatst, wordt standaard opgeslagen in `~/.ssh/id_rsa.pub`, tenzij u de locatie hebt gewijzigd toen u ze maakte. Als u de [Azure CLI 2.0](/cli/azure) gebruikt om uw virtuele machine te maken, geeft u de locatie van deze openbare sleutel op wanneer u [az vm create](/cli/azure/vm#create) gebruikt met de optie `--ssh-key-path`. Als u de inhoud van het te gebruiken openbare-sleutelbestand kopieert en plakt in Azure Portal of een Resource Manager-sjabloon, moet u ervoor zorgen dat u geen extra witruimte kopieert. Als u OS X gebruikt, kunt u bijvoorbeeld een bestand met de openbare sleutel doorgeven (standaard **~/.ssh/id_rsa.pub**) naar **pbcopy** om de inhoud te kopiëren (er zijn andere Linux-programma's die hetzelfde doen, zoals `xclip`).

Als u niet vertrouwd bent met openbare SSH-sleutels, kunt u de openbare sleutel bekijken door `cat` als volgt uit te voeren. Vervang `~/.ssh/id_rsa.pub` door de locatie van uw eigen openbare-sleutelbestand:

```bash
cat ~/.ssh/id_rsa.pub
```

Met de openbare sleutel op uw virtuele Azure-machine voert u SSH uit op uw virtuele machine met het IP-adres of de DNS-naam van de virtuele machine (vervang `azureuser` en `myvm.westus.cloudapp.azure.com` hieronder door de gebruikersnaam van de beheerder en de volledig gekwalificeerde domeinnaam, of het IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt opgegeven, moet u deze tijdens het aanmeldingsproces desgevraagd invoeren. (De server is toegevoegd aan uw map `~/.ssh/known_hosts` en u wordt pas weer gevraagd om verbinding te maken nadat de openbare sleutel op uw virtuele Azure-machine is gewijzigd of de naam van de server is verwijderd uit `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Volgende stappen

Virtuele machines die zijn gemaakt met behulp van SSH-sleutels, zijn standaard geconfigureerd met uitgeschakelde wachtwoorden, waardoor brute force-aanvallen om wachtwoorden in handen te krijgen veel duurder en dus ook lastiger worden. Dit onderwerp beschrijft het maken van een eenvoudig SSH-sleutelpaar voor snel gebruik. Als u meer hulp nodig hebt bij het maken van het SSH-sleutelpaar of meer certificaten nodig hebt, raadpleegt u [Gedetailleerde stappen voor het maken van SSH-sleutelparen en certificaten](create-ssh-keys-detailed.md).

U kunt virtuele machines maken die gebruikmaken van het SSH-sleutelpaar, met Azure Portal, CLI en sjablonen:

* [Een beveiligde virtuele Linux-machine maken met Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een beveiligde virtuele Linux-machine maken met de Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een beveiligde virtuele Linux-machine maken met een Azure-sjabloon](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

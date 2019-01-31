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
ms.date: 09/11/2018
ms.author: cynthn
ms.openlocfilehash: d442d09c8c8ded3aa50faf74e28c8d95ded24a5e
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300197"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Snelle stappen: Maken en gebruiken van een openbare en persoonlijke SSH-sleutelpaar voor virtuele Linux-machines in Azure

Met de combinatie van een secure shell (SSH), kunt u virtuele machines (VM's) maken in Azure die SSH-sleutels gebruiken voor verificatie, hoeft u de wachtwoorden aan te melden. Dit artikel ziet u hoe u snel genereren en het gebruik van een sleutelpaar openbare en persoonlijke-sleutelbestand met SSH voor virtuele Linux-machines. U kunt deze stappen voltooien met de Azure Cloud Shell, een macOS of Linux-host, de Windows-subsysteem voor Linux en andere hulpprogramma's die ondersteuning bieden voor OpenSSH. 

> [!NOTE]
> Virtuele machines die zijn gemaakt met behulp van SSH-sleutels zijn standaard geconfigureerd met wachtwoorden uitgeschakeld, die aanzienlijk verbeterd, evenals de moeite van raden brute-force-aanvallen. 

Zie voor meer achtergrondinformatie en voorbeelden, [gedetailleerde stappen voor het maken van SSH-sleutelparen](create-ssh-keys-detailed.md).

Zie voor aanvullende manieren om te genereren en het gebruik van SSH-sleutels op een Windows-computer, [over het gebruik van SSH-sleutels met Windows op Azure](ssh-from-windows.md).

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>Een SSH-sleutelpaar maken

Gebruik de `ssh-keygen` opdracht voor het genereren van SSH openbare en persoonlijke sleutelbestanden. Deze bestanden worden standaard gemaakt in de map ~/.ssh. U kunt een andere locatie en een optionele wachtwoord opgeven (*wachtwoordzin*) voor toegang tot het persoonlijke sleutelbestand. Als een SSH-sleutelpaar met dezelfde naam in de opgegeven locatie bestaat, worden deze bestanden overschreven.

De volgende opdracht maakt u een SSH-sleutelpaar, met behulp van RSA-versleuteling en een bitlengte van 2048:

```bash
ssh-keygen -t rsa -b 2048
```

Als u de [Azure CLI](/cli/azure) te maken van uw virtuele machine met de [az vm maken](/cli/azure/vm#az-vm-create) opdracht, u kunt eventueel genereren met SSH openbare en persoonlijke sleutelbestanden met behulp van de `--generate-ssh-keys` optie. De belangrijkste bestanden worden opgeslagen in de map ~/.ssh, tenzij anders aangegeven met de `--ssh-dest-key-path` optie. De `--generate-ssh-keys` optie overschrijft geen bestaande sleutelbestanden, in plaats daarvan een fout geretourneerd. In de volgende opdracht, vervangt u *VMname* en *RGname* door uw eigen waarden:

```azurecli
az vm create --name VMname --resource-group RGname --generate-ssh-keys 
```

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>Geef een openbare SSH-sleutel bij het implementeren van een virtuele machine

Voor het maken van een Linux-VM die gebruikmaakt van SSH-sleutels voor verificatie, Geef uw openbare SSH-sleutel bij het maken van de virtuele machine met behulp van de Azure-portal, Azure CLI, Azure Resource Manager-sjablonen of andere methoden:

* [Een virtuele Linux-machine maken met de Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-machine maken met de Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een Linux-VM met behulp van een Azure-sjabloon maken](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Als u niet bekend met de indeling van een openbare SSH-sleutel bent, kunt u uw openbare sleutel met de volgende weergeven `cat` opdracht, vervangt `~/.ssh/id_rsa.pub` met het pad en bestandsnaam van uw eigen openbare-sleutelbestand indien nodig:

```bash
cat ~/.ssh/id_rsa.pub
```

Een typische waarde voor de openbare sleutel ziet eruit zoals in dit voorbeeld:

```
ssh-rsa AAAAB3NzaC1yc2EAABADAQABAAACAQC1/KanayNr+Q7ogR5mKnGpKWRBQU7F3Jjhn7utdf7Z2iUFykaYx+MInSnT3XdnBRS8KhC0IP8ptbngIaNOWd6zM8hB6UrcRTlTpwk/SuGMw1Vb40xlEFphBkVEUgBolOoANIEXriAMvlDMZsgvnMFiQ12tD/u14cxy1WNEMAftey/vX3Fgp2vEq4zHXEliY/sFZLJUJzcRUI0MOfHXAuCjg/qyqqbIuTDFyfg8k0JTtyGFEMQhbXKcuP2yGx1uw0ice62LRzr8w0mszftXyMik1PnshRXbmE2xgINYg5xo/ra3mq2imwtOKJpfdtFoMiKhJmSNHBSkK7vFTeYgg0v2cQ2+vL38lcIFX4Oh+QCzvNF/AXoDVlQtVtSqfQxRVG79Zqio5p12gHFktlfV7reCBvVIhyxc2LlYUkrq4DHzkxNY5c9OGSHXSle9YsO3F1J5ip18f6gPq4xFmo6dVoJodZm9N0YMKCkZ4k1qJDESsJBk2ujDPmQQeMjJX3FnDXYYB182ZCGQzXfzlPDC29cWVgDZEXNHuYrOLmJTmYtLZ4WkdUhLLlt5XsdoKWqlWpbegyYtGZgeZNRtOOdN6ybOPJqmYFd2qRtb4sYPniGJDOGhx4VodXAjT09omhQJpE6wlZbRWDvKC55R2d/CSPHJscEiuudb+1SG2uA/oik/WQ== username@domainname
```

Als u kopieert en plakt u de inhoud van het openbare sleutelbestand om het te gebruiken in Azure portal of een Resource Manager-sjabloon, zorg er dan voor dat u alle volgspaties niet kopiëren. Als u wilt kopiëren van een openbare sleutel in Mac OS, kunt u het openbare sleutelbestand dat u wilt doorgeven **pbcopy**. Op dezelfde manier in Linux, u kunt doorgeven het openbare sleutelbestand voor programma's zoals **xclip**.

De openbare sleutel die u op uw Linux-VM in Azure plaatst, wordt standaard opgeslagen ~ /.ssh/id_rsa.pub, tenzij u een andere locatie hebt opgegeven tijdens het maken van het sleutelpaar. Gebruik de [Azure CLI 2.0](/cli/azure) voor het maken van uw virtuele machine met een bestaande openbare sleutel, geeft u de waarde en (optioneel) de locatie van deze openbare sleutel met de [az vm maken](/cli/azure/vm#az-vm-create) opdracht met de `--ssh-key-value` optie. In de volgende opdracht, vervangt u *VMname*, *RGname*, en *keyFile* door uw eigen waarden:

```azurecli
az vm create --name VMname --resource-group RGname --ssh-key-value @keyFile
```

## <a name="ssh-into-your-vm"></a>SSH in uw virtuele machine

Met de openbare sleutel die is geïmplementeerd op uw Azure-VM en de persoonlijke sleutel op uw lokale systeem, voeg SSH toe aan uw virtuele machine via het IP-adres of de DNS-naam van uw virtuele machine. In de volgende opdracht, vervangt u *azureuser* en *myvm.westus.cloudapp.azure.com* met de naam van de beheerder-gebruiker en de volledig gekwalificeerde domeinnaam (of IP-adres):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Als u tijdens het maken van uw sleutelpaar een wachtwoordzin hebt opgegeven, voert u deze wachtwoordzin wanneer hierom wordt gevraagd tijdens het aanmelden. De virtuele machine wordt toegevoegd aan uw bestand ~/.ssh/known_hosts en u wordt niet gevraagd verbinding opnieuw tot de openbare sleutel van uw Azure-VM-wijzigingen of naam van de server wordt verwijderd uit ~/.ssh/known_hosts.

Als de virtuele machine wordt met behulp van het beleid voor just-in-time-toegang, moet u om toegang te vragen voordat u verbinding met de virtuele machine maken kunt. Zie voor meer informatie over het beleid voor just-in-time [beheer van de virtuele machine toegang met just in time-beleid](../../security-center/security-center-just-in-time.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het werken met SSH-sleutelparen [gedetailleerde stappen voor het maken en beheren van SSH-sleutelparen](create-ssh-keys-detailed.md).

* Als u problemen met SSH-verbindingen met virtuele Azure-machines hebt, raadpleegt u [oplossen SSH-verbindingen met een Azure Linux VM](troubleshoot-ssh-connection.md).

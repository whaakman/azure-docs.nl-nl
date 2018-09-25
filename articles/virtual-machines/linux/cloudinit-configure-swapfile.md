---
title: Cloud-init gebruiken voor een wisselbestand configureren op een Linux-VM | Microsoft Docs
description: Hoe u cloud-init gebruiken voor het configureren van een wisselbestand in een Linux-VM tijdens het maken van met de Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 2a5a878b7c8c3b6126d90b978241fbcb237d8db7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946303"
---
# <a name="use-cloud-init-to-configure-a-swapfile-on-a-linux-vm"></a>Cloud-init gebruiken voor een wisselbestand configureren op een Linux VM
In dit artikel leest u hoe u [cloud-init](https://cloudinit.readthedocs.io) voor het configureren van het wisselbestand op verschillende Linux-distributies. Het wisselbestand is door de Linux Agent (WALA) op basis van welke distributies een vereist traditioneel geconfigureerd.  Dit document wordt een overzicht van het proces voor het bouwen van het wisselbestand op de vraag tijdens inrichtingstijd cloud-init gebruiken.  Zie voor meer informatie over hoe cloud-init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud-init-overzicht](using-cloud-init.md)

## <a name="create-swapfile-for-ubuntu-based-images"></a>Wisselbestand voor Ubuntu op basis van installatiekopieën maken
Standaard op Azure Maak Ubuntu galerie met installatiekopieën niet wisselbestanden. Om te wisselen bestandsconfiguratie tijdens VM inrichten met behulp van cloud-init inschakelen - Raadpleeg de [AzureSwapPartitions document](https://wiki.ubuntu.com/AzureSwapPartitions) op de wiki Ubuntu.

## <a name="create-swapfile-for-red-hat-and-centos-based-images"></a>Wisselbestand voor Red Hat en op basis van CentOS-installatiekopieën maken

Maak een bestand in uw huidige shell met de naam *cloud_init_swapfile.txt* en plak de volgende configuratie. In dit voorbeeld maakt u het bestand in de Cloud Shell niet op uw lokale computer. U kunt elke editor die u wilt gebruiken. Voer `sensible-editor cloud_init_swapfile.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Zorg ervoor dat het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel.  

```yaml
#cloud-config
disk_setup:
  ephemeral0:
    table_type: gpt
    layout: [66, [33,82]]
    overwrite: true
fs_setup:
  - device: ephemeral0.1
    filesystem: ext4
  - device: ephemeral0.2
    filesystem: swap
mounts:
  - ["ephemeral0.1", "/mnt"]
  - ["ephemeral0.2", "none", "swap", "sw", "0", "0"]
```

Voordat u deze installatiekopie implementeert, moet u een resourcegroep maken met de [az-groep maken](/cli/azure/group#az_group_create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) en geef het cloud-init-bestand met `--custom-data cloud_init_swapfile.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_swapfile.txt \
  --generate-ssh-keys 
```

## <a name="verify-swapfile-was-created"></a>Controleer of wisselbestand is gemaakt
SSH naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

Beschikt u SSH'ed in de virtuele machine, moet u controleren of het wisselbestand is gemaakt

```bash
swapon -s
```

De uitvoer van deze opdracht moet er als volgt:

```text
Filename                Type        Size    Used    Priority
/dev/sdb2  partition   2494440 0   -1
```

> [!NOTE] 
> Als u een bestaande Azure-installatiekopie met een wisselbestand geconfigureerd hebt en u wilt dat de configuratie van het bestand wisselen voor nieuwe afbeeldingen te wijzigen, moet u de bestaande wisselbestand verwijderen. Raadpleeg het document 'Aanpassen installatiekopieën om in te richten van cloud-init' voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Voor extra cloud-init-voorbeelden van wijzigingen in de configuratie, Zie de volgende:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)

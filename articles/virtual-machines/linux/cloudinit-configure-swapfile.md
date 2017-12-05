---
title: Cloud-init gebruiken voor het configureren van een wisselbestand op een Linux-VM | Microsoft Docs
description: Het gebruik van cloud-init voor het configureren van een wisselbestand in een Linux-VM tijdens het maken van met de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: a8ccec0dc8ff100c5d067cd50f2a6fa8cb4871fb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-configure-a-swapfile-on-a-linux-vm"></a>Cloud-init gebruiken voor het configureren van een wisselbestand op een Linux-VM
Dit artikel laat zien hoe u [cloud init](https://cloudinit.readthedocs.io) het wisselbestand configureren op verschillende Linux-distributies. Het wisselbestand is door de Linux Agent (WALA) op basis van welke distributies een vereist oudsher geconfigureerd.  Dit document wordt een overzicht van het proces voor het bouwen van het wisselbestand op aanvraag tijdens inrichting met behulp van cloud-init.  Zie voor meer informatie over hoe cloud init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud init-overzicht](using-cloud-init.md)

## <a name="create-swapfile-for-ubuntu-based-images"></a>Wisselbestand voor Ubuntu op basis van installatiekopieën maken
Ubuntu-afbeeldingen maken standaard op Azure, geen wisselbestanden. Configuratie van de wisseling bestand tijdens de VM-inrichting met behulp van cloud-init inschakelen-Zie de [AzureSwapPartitions document](https://wiki.ubuntu.com/AzureSwapPartitions) op de wiki Ubuntu.

## <a name="create-swapfile-for-redhat-and-centos-based-images"></a>Wisselbestand voor RedHat maken en CentOS op basis van installatiekopieën

Maak een bestand in uw huidige shell met de naam *cloud_init_swapfile.txt* en plak de volgende configuratie. Bijvoorbeeld, het bestand te maken in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor cloud_init_swapfile.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Kies #1 gebruiken de **nano** editor. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel.  

```yaml
#cloud-config
disk_setup:
ephemeral0:
table_type: gpt
layout: [66, [33,82]]
overwrite: True
fs_setup:
- device: ephemeral0.1
filesystem: ext4
- device: ephemeral0.2
filesystem: swap
mounts:
- ["ephemeral0.1", "/mnt"]
- ["ephemeral0.2", "none", "swap", "sw", "0", "0"]
```

Voordat u deze installatiekopie implementeert, moet u maken van een resourcegroep met de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geef het cloud-init-bestand met `--custom-data cloud_init_swapfile.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_swapfile.txt \
  --generate-ssh-keys 
```

## <a name="verify-swapfile-was-created"></a>Controleer of het wisselbestand is gemaakt
SSH kunt uitvoeren naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

Zodra u SSH'ed in de virtuele machine hebt, moet u controleren of het wisselbestand is gemaakt

```bash
swapon -s
```

De uitvoer van deze opdracht moet er als volgt uitzien:

```text
Filename                Type        Size    Used    Priority
/dev/sdb2  partition   2494440 0   -1
```

> [!NOTE] 
> Als u een bestaande Azure-installatiekopie die is van een wisselbestand geconfigureerd hebt en u wilt wijzigen van de configuratie van de wisseling-bestand voor installatiekopieën van het nieuwe, moet u het bestaande wisselbestand verwijderen. Zie 'Aanpassen afbeeldingen inrichten door cloud init' document voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer cloud-init voorbeelden van wijzigingen in de configuratie, de volgende:
 
- [Een extra Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Voer een Pakketbeheer voor het bijwerken van bestaande pakketten op de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale hostnaam VM](cloudinit-update-vm-hostname.md) 
- [Installeren van een toepassingspakket, het bijwerken van de configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
---
title: Cloud-init gebruiken voor de hostnaam ingesteld voor een Linux-VM op Azure | Microsoft Docs
description: Hoe u cloud-init gebruiken voor het aanpassen van een Linux-VM tijdens het maken van met de Azure CLI
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
ms.openlocfilehash: da20c4b30e2708bf7754d025cfbd2c269c3b5c7c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164186"
---
# <a name="use-cloud-init-to-set-hostname-for-a-linux-vm-in-azure"></a>Cloud-init gebruiken voor de hostnaam voor een Linux-VM in Azure instellen
In dit artikel leest u hoe u [cloud-init](https://cloudinit.readthedocs.io) voor het configureren van een specifieke hostnaam op een virtuele machine (VM) of de virtuele machine-schaalsets (VMSS) inrichtingstijd in Azure. Deze cloud-init-scripts uitvoeren op de eerste keer opstarten nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud-init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud-init-overzicht](using-cloud-init.md)

## <a name="set-the-hostname-with-cloud-init"></a>Stel de hostnaam met cloud-init
De hostnaam is standaard hetzelfde als de VM-naam wanneer u een nieuwe virtuele machine in Azure maakt.  Het uitvoeren van een cloud-init-script als u wilt deze standaardhostnaam wijzigen wanneer u een virtuele machine in Azure met maken [az vm maken](/cli/azure/vm), geeft u het cloud-init-bestand met de `--custom-data` overschakelen.  

Als u wilt het upgradeproces in actie zien, maakt u een bestand in uw huidige shell met de naam *cloud_init_hostname.txt* en plak de volgende configuratie. In dit voorbeeld maakt u het bestand in de Cloud Shell niet op uw lokale computer. U kunt elke editor die u wilt gebruiken. Voer `sensible-editor cloud_init_hostname.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Zorg ervoor dat het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel.  

```yaml
#cloud-config
hostname: myhostname
```

Voordat u deze installatiekopie implementeert, moet u een resourcegroep maken met de [az-groep maken](/cli/azure/group) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm) en geef het cloud-init-bestand met `--custom-data cloud_init_hostname.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_hostname.txt \
  --generate-ssh-keys 
```

Nadat u hebt gemaakt, toont de Azure CLI informatie over de virtuele machine. Gebruik de `publicIpAddress` voor SSH naar uw virtuele machine. Voer uw eigen adres als volgt in:

```bash
ssh <publicIpAddress>
```

Als de naam van de virtuele machine wilt weergeven, gebruikt de `hostname` opdracht als volgt:

```bash
hostname
```

De hostnaam van de virtuele machine wilt laten rapporteren als die waarde instellen in de cloud-init-bestand, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```bash
myhostname
```

## <a name="next-steps"></a>Volgende stappen
Voor extra cloud-init-voorbeelden van wijzigingen in de configuratie, Zie de volgende:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)

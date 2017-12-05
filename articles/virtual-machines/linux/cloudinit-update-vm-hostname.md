---
title: Gebruik van cloud-init hostnaam instellen voor een Linux-VM op Azure | Microsoft Docs
description: Het gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van met de Azure CLI 2.0
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
ms.openlocfilehash: 7455748b2e08104dadfed8cfe41581a400539d4f
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-set-hostname-for-a-linux-vm-in-azure"></a>Gebruik van cloud-init hostnaam instellen voor een Linux VM in Azure
Dit artikel laat zien hoe u [cloud init](https://cloudinit.readthedocs.io) scale ingesteld voor het configureren van een specifieke hostnaam op een virtuele machine (VM) of de virtuele machine (VMSS) bij het inrichten van de tijd in Azure. Deze cloud init-scripts worden uitgevoerd op de eerste keer wordt opgestart nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud init-overzicht](using-cloud-init.md)

## <a name="set-the-hostname-with-cloud-init"></a>De hostnaam met cloud-init instellen
De hostnaam is standaard hetzelfde zijn als de naam van de VM wanneer u een nieuwe virtuele machine in Azure maakt.  Uitvoeren van een script cloud init om te wijzigen van deze hostnaam standaard wanneer u een virtuele machine in Azure met maken [az vm maken](/cli/azure/vm#create), geeft u het cloud-init-bestand met de `--custom-data` overschakelen.  

Om het upgradeproces in actie zien, kunt u een bestand maken in uw huidige shell met de naam *cloud_init_hostname.txt* en plak de volgende configuratie. Bijvoorbeeld, het bestand te maken in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor cloud_init_hostname.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Kies #1 gebruiken de **nano** editor. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel.  

```yaml
#cloud-config
hostname: myhostname
```

Voordat u deze installatiekopie implementeert, moet u maken van een resourcegroep met de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geef het cloud-init-bestand met `--custom-data cloud_init_hostname.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_hostname.txt \
  --generate-ssh-keys 
```

Zodra gemaakt, bevat de Azure CLI informatie over de virtuele machine. Gebruik de `publicIpAddress` naar SSH met uw virtuele machine. Voer uw eigen adres als volgt:

```bash
ssh <publicIpAddress>
```

Als de naam van de virtuele machine wilt weergeven, gebruikt de `hostname` opdracht als volgt:

```bash
hostname
```

De virtuele machine moet de hostnaam als deze waarde is ingesteld in de cloud init-bestand, zoals wordt weergegeven in de volgende voorbeelduitvoer report:

```bash
myhostname
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer cloud-init voorbeelden van wijzigingen in de configuratie, de volgende:
 
- [Een extra Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Voer een Pakketbeheer voor het bijwerken van bestaande pakketten op de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale hostnaam VM](cloudinit-update-vm-hostname.md) 
- [Installeren van een toepassingspakket, het bijwerken van de configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
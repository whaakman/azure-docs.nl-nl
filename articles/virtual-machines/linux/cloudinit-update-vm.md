---
title: Cloud-init gebruiken om te werken en -pakketten installeren op een Linux-VM in Azure | Microsoft Docs
description: Hoe u cloud-init gebruiken om te werken en pakketten worden geïnstalleerd in een Linux-VM tijdens het maken van met de Azure CLI
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
ms.date: 04/20/2018
ms.author: rclaus
ms.openlocfilehash: d5f4dc7f4abc13f253a206a63e65faf1106f9c7c
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766177"
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Cloud-init gebruiken om te werken en te installeren pakketten in een Linux-VM in Azure
In dit artikel leest u hoe u [cloud-init](https://cloudinit.readthedocs.io) om bij te werken van pakketten op een Linux virtuele machine (VM) of virtuele-machineschaalset ingesteld (VMSS) bij het inrichten van de tijd in Azure. Deze cloud-init-scripts uitvoeren op de eerste keer opstarten nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud-init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud-init-overzicht](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Bijwerken van een virtuele machine met cloud-init
Uit veiligheidsoverwegingen kunt u een virtuele machine om toe te passen van de meest recente updates op de eerste keer opstarten configureren. Als u cloud-init werkt met verschillende Linux-distributies, is het niet nodig om op te geven `apt` of `yum` voor de package manager. In plaats daarvan, definieert u `package_upgrade` en laat het cloud-init-proces bepalen van het juiste mechanisme voor de distro die in gebruik. Deze werkstroom kunt u de dezelfde cloud-init-scripts gebruiken voor distributies.

Als u wilt het upgradeproces in actie zien, maakt u een bestand in uw huidige shell met de naam *cloud_init_upgrade.txt* en plak de volgende configuratie. In dit voorbeeld maakt u het bestand in de Cloud Shell niet op uw lokale computer. U kunt elke editor die u wilt gebruiken. Voer `sensible-editor cloud_init_upgrade.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Zorg ervoor dat het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Voordat u deze installatiekopie implementeert, moet u een resourcegroep maken met de [az-groep maken](/cli/azure/group) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm) en geef het cloud-init-bestand met `--custom-data cloud_init_upgrade.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

Het pakket met beheerprogramma en controleren op updates uitgevoerd.

```bash
sudo yum update
```

Cloud-init gecontroleerd en updates worden geïnstalleerd bij het opstarten, mogen er geen aanvullende updates worden toegepast.  U ziet het updateproces, aantal gewijzigde pakketten, evenals de installatie van `httpd` door uit te voeren `yum history` en controleer de uitvoer die vergelijkbaar is met de onderstaande.

```bash
Loaded plugins: fastestmirror, langpacks
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     3 | -t -y install httpd      | 2018-04-20 22:42 | Install        |    5
     2 | -t -y upgrade            | 2018-04-20 22:38 | I, U           |   65
     1 |                          | 2017-12-12 20:32 | Install        |  522
```

## <a name="next-steps"></a>Volgende stappen
Voor extra cloud-init-voorbeelden van wijzigingen in de configuratie, Zie de volgende:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)

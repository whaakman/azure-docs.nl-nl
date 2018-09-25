---
title: Cloud-init gebruiken voor een gebruiker toevoegen aan een Linux-VM op Azure | Microsoft Docs
description: Hoe u cloud-init gebruiken voor een gebruiker toevoegen aan een Linux-VM tijdens het maken van met de Azure CLI
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
ms.openlocfilehash: 905d701437b1b580c019c800d13b18f725580fdd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972943"
---
# <a name="use-cloud-init-to-add-a-user-to-a-linux-vm-in-azure"></a>Cloud-init gebruiken voor een gebruiker toevoegen aan een Linux-VM in Azure
In dit artikel leest u hoe u [cloud-init](https://cloudinit.readthedocs.io) toevoegen van een gebruiker op een virtuele machine (VM) of de virtuele machine-schaalsets (VMSS) inrichtingstijd in Azure. Deze cloud-init-script wordt uitgevoerd bij de eerste keer opstarten nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud-init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [overzicht van cloud-init](using-cloud-init.md).

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Een gebruiker toevoegen aan een virtuele machine met cloud-init
Een van de eerste taken op elke nieuwe Linux-VM is het toevoegen van een andere gebruiker zelf om te voorkomen dat het gebruik van *hoofdmap*. SSH-sleutels zijn best practice voor beveiliging en gebruiksgemak. Sleutels worden toegevoegd aan de *~/.ssh/authorized_keys* -bestand met dit script cloud-init.

Als u wilt een gebruiker toevoegen aan een Linux-VM, maakt u een bestand in uw huidige shell met de naam *cloud_init_add_user.txt* en plak de volgende configuratie. In dit voorbeeld maakt u het bestand in de Cloud Shell niet op uw lokale computer. U kunt elke editor die u wilt gebruiken. Voer `sensible-editor cloud_init_add_user.txt` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Zorg ervoor dat het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel.  U moet uw eigen openbare sleutel opgeven (zoals de inhoud van *~/.ssh/id_rsa.pub*) voor de waarde van `ssh-authorized-keys:` -heeft hier ter vereenvoudiging van het voorbeeld zijn ingekort.

```yaml
#cloud-config
users:
  - default
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```
> [!NOTE] 
> Het #cloud-config-bestand bevat de `- default` parameter opgenomen. Hiermee worden de gebruiker toegevoegd aan de bestaande gebruiker met beheerdersrechten gemaakt tijdens het inrichten. Als u een gebruiker zonder de `- default` parameter - de automatisch gegenereerde admin-gebruiker die zijn gemaakt door het Azure-platform zou worden overschreven. 

Voordat u deze installatiekopie implementeert, moet u een resourcegroep maken met de [az-groep maken](/cli/azure/group#az_group_create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) en geef het cloud-init-bestand met `--custom-data cloud_init_add_user.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_add_user.txt \
  --generate-ssh-keys 
```

SSH naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

Weergeven om te bevestigen dat de gebruiker is toegevoegd aan de virtuele machine en de opgegeven groepen, de inhoud van de */etc/groep* bestand als volgt:

```bash
cat /etc/group
```

De volgende voorbeelduitvoer ziet u de gebruiker de *cloud_init_add_user.txt* bestand is toegevoegd aan de virtuele machine en de juiste groep:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Volgende stappen
Voor extra cloud-init-voorbeelden van wijzigingen in de configuratie, Zie de volgende:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
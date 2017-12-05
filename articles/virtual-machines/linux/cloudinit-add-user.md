---
title: Cloud-init gebruiken een gebruiker toevoegen aan een Linux-VM op Azure | Microsoft Docs
description: Het gebruik van cloud-init voor een gebruiker toevoegen aan een Linux-VM tijdens het maken van met de Azure CLI 2.0
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
ms.openlocfilehash: 728ffed27747cb298d5da312014a3c9e98b44f1e
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-add-a-user-to-a-linux-vm-in-azure"></a>Cloud-init gebruiken een gebruiker toevoegen aan een Linux VM in Azure
Dit artikel laat zien hoe u [cloud init](https://cloudinit.readthedocs.io) schaal wordt een gebruiker op een virtuele machine (VM) of de virtuele machine toevoegen (VMSS) ingesteld op de inrichting van de tijd in Azure. Dit script cloud init wordt uitgevoerd op de eerste keer wordt opgestart nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud init overzicht](using-cloud-init.md).

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Een gebruiker toevoegen aan een virtuele machine met cloud-init
Een van de eerste taken op een nieuwe Linux-VM is het toevoegen van een gebruiker met aanvullende voor uzelf om te voorkomen dat het gebruik van *hoofdmap*. SSH-sleutels zijn best practice voor beveiliging en gebruiksgemak. Sleutels worden toegevoegd aan de *~/.ssh/authorized_keys* bestand met dit script cloud init.

Als u wilt een gebruiker toevoegt aan een Linux-VM, maakt u een bestand in uw huidige shell met de naam *cloud_init_add_user.txt* en plak de volgende configuratie. Bijvoorbeeld, het bestand te maken in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor cloud_init_add_user.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Kies #1 gebruiken de **nano** editor. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel.  U moet uw eigen openbare sleutel opgeven (zoals de inhoud van *~/.ssh/id_rsa.pub*) voor de waarde van `ssh-authorized-keys:` -heeft hier om te vereenvoudigen in het voorbeeld is ingekort.

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
> Het #cloud-config-bestand bevat de `- default` parameter opgenomen. Hiermee wordt de gebruiker toevoegen aan de bestaande gebruiker met beheerdersrechten gemaakt tijdens het inrichten. Als u een gebruiker zonder maakt de `- default` parameter - de automatisch gegenereerde admin gebruiker gemaakt door de Azure-platform zou worden overschreven. 

Voordat u deze installatiekopie implementeert, moet u maken van een resourcegroep met de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geef het cloud-init-bestand met `--custom-data cloud_init_add_user.txt` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_add_user.txt \
  --generate-ssh-keys 
```

SSH kunt uitvoeren naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

De inhoud van bekijken om te bevestigen op de gebruiker is toegevoegd aan de virtuele machine en de opgegeven groepen, de */etc/groep* als volgt:

```bash
cat /etc/group
```

De volgende voorbeelduitvoer wordt weergegeven voor de gebruiker uit de *cloud_init_add_user.txt* bestand is toegevoegd aan de virtuele machine en de juiste groep:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer cloud-init voorbeelden van wijzigingen in de configuratie, de volgende:
 
- [Een extra Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Voer een Pakketbeheer voor het bijwerken van bestaande pakketten op de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale hostnaam VM](cloudinit-update-vm-hostname.md) 
- [Installeren van een toepassingspakket, het bijwerken van de configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
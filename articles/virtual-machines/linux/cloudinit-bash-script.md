---
title: Cloud-init gebruiken voor een bash-script uitvoeren in een Linux-VM op Azure | Microsoft Docs
description: Hoe u cloud-init gebruiken voor een bash-script uitvoeren in een Linux-VM tijdens het maken van met de Azure CLI
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
ms.openlocfilehash: 8e8950cbd7927cb6b0543866ab976b550c9ec043
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959543"
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Cloud-init gebruiken voor het uitvoeren van een bash-script in een Linux-VM in Azure
In dit artikel leest u hoe u [cloud-init](https://cloudinit.readthedocs.io) om uit te voeren een bestaand bash-script op een Linux-machine (VM) of virtuele-machineschaalsets (VMSS) bij het inrichten van de tijd in Azure. Deze cloud-init-scripts uitvoeren op de eerste keer opstarten nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud-init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud-init-overzicht](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Een bash-script uitvoeren met cloud-init
Met cloud-init die u niet wilt dat uw bestaande scripts omzetten in een cloudconfiguratie, accepteert cloud-init meerdere invoertypen, waarbij een van een bash-script is.

Als u de Azure Linux Custom Script Extension hebt met uw scripts uit te voeren, kunt u ze voor het gebruik van cloud-init kunt migreren. Echter-extensies van Azure hebt geïntegreerd met rapporteren aan de waarschuwing op fouten van het script, mislukt de implementatie van een cloud-init-installatiekopie niet als het script is mislukt.

Als u wilt deze functionaliteit in actie zien, maakt u een eenvoudige bash-script voor het testen van. Als de cloud-init `#cloud-config` bestand, met dit script moet zich lokaal op waarop u de Azure CLI-opdrachten voor het inrichten van uw virtuele machine wordt uitgevoerd.  In dit voorbeeld maakt u het bestand in de Cloud Shell niet op uw lokale computer. U kunt elke editor die u wilt gebruiken. Voer `sensible-editor simple_bash.sh` in voor het maken van het bestand en om een overzicht van beschikbare editors te zien. Kies #1 gebruiken de **nano** editor. Zorg ervoor dat het hele cloud-init-bestand correct is gekopieerd, met name de eerste regel.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Voordat u deze installatiekopie implementeert, moet u een resourcegroep maken met de [az-groep maken](/cli/azure/group#az_group_create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) en geef de bash-script-bestand met `--custom-data simple_bash.sh` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Controleer of de bash-script is uitgevoerd
SSH naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

Wijzig in het **map/Tmp** directory en controleer of dat myScript.txt-bestand bestaat en de juiste tekst binnen het heeft.  Als dit niet het geval is, kunt u controleren de **/var/log/cloud-init.log** voor meer informatie.  Zoeken naar de volgende vermelding:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Volgende stappen
Voor extra cloud-init-voorbeelden van wijzigingen in de configuratie, Zie de volgende:
 
- [Een aanvullende Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Uitvoeren van een pakketbeheerder voor het bijwerken van bestaande pakketten bij de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale VM-hostnaam](cloudinit-update-vm-hostname.md) 
- [Het installatiepakket voor een toepassing, het bijwerken van configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
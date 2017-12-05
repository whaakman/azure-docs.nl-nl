---
title: Cloud-init gebruiken een bash-script uitvoeren op een Linux-VM op Azure | Microsoft Docs
description: Het gebruik van cloud-init een bash-script uitvoeren op een Linux-VM tijdens het maken van met de Azure CLI 2.0
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
ms.openlocfilehash: 1fcc432e8437a7fd284a75aa40454848a2af3006
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Cloud-init gebruiken een bash-script uitvoeren op een Linux-VM in Azure
Dit artikel laat zien hoe u [cloud init](https://cloudinit.readthedocs.io) om uit te voeren een bestaand script op basis van virtuele Linux-machine (VM) bash of virtuele-machineschaalset (VMSS) ingesteld op de inrichting van de tijd in Azure. Deze cloud init-scripts worden uitgevoerd op de eerste keer wordt opgestart nadat de resources zijn ingericht met Azure. Zie voor meer informatie over hoe cloud init systeemeigen in Azure en de ondersteunde Linux-distributies werkt [cloud init-overzicht](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Een bash-script uitvoeren met cloud-init
Met cloud-init niet hoeft uw bestaande scripts converteren naar een cloud-config accepteert cloud init meerdere invoertypen, waarvan er één een bash-script is.

Als u de extensie van het Linux aangepast Script Azure hebt met uw scripts uitvoeren, kunt u ze voor het gebruik van cloud-init kunt migreren. Echter Azure Extensions rapporteren aan de waarschuwing op fouten van het script hebt geïntegreerd, de implementatie van een cloud-init-installatiekopie niet mislukken als het script is mislukt.

Overzicht van deze functionaliteit in actie een eenvoudige bash-script voor het testen van te maken. De initialisatie van de cloud, zoals `#cloud-config` bestand, dit script moet lokaal op waarop u de AzureCLI-opdrachten voor het inrichten van de virtuele machine wordt uitgevoerd.  Bijvoorbeeld, het bestand te maken in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor simple_bash.sh` voor het maken van het bestand en een overzicht van beschikbare editors. Kies #1 gebruiken de **nano** editor. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Voordat u deze installatiekopie implementeert, moet u maken van een resourcegroep met de [az groep maken](/cli/azure/group#create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geeft u het scriptbestand bash met `--custom-data simple_bash.sh` als volgt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Controleer of bash-script is uitgevoerd
SSH kunt uitvoeren naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen **publicIpAddress** als volgt:

```bash
ssh <publicIpAddress>
```

Wijzig in de **map** directory en controleer of dat myScript.txt-bestand bestaat en de juiste tekst heeft.  Als dit niet het geval is, kunt u controleren de **/var/log/cloud-init.log** voor meer informatie.  Zoeken naar de volgende vermelding:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer cloud-init voorbeelden van wijzigingen in de configuratie, de volgende:
 
- [Een extra Linux-gebruiker toevoegen aan een virtuele machine](cloudinit-add-user.md)
- [Voer een Pakketbeheer voor het bijwerken van bestaande pakketten op de eerste keer opstarten](cloudinit-update-vm.md)
- [Wijzigen van de lokale hostnaam VM](cloudinit-update-vm-hostname.md) 
- [Installeren van een toepassingspakket, het bijwerken van de configuratiebestanden en het invoeren van sleutels](tutorial-automate-vm-deployment.md)
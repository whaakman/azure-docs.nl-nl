---
title: Cloud-init gebruiken voor het aanpassen van een Linux-VM | Microsoft Docs
description: Het gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van met de Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/03/2017
ms.author: iainfou
ms.openlocfilehash: 5559f258f5c29b07edb5e61be4755d67173019e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Cloud-init gebruiken voor het aanpassen van een Linux VM in Azure
Dit artikel laat zien hoe u [cloud init](https://cloudinit.readthedocs.io) om in te stellen de hostnaam, pakketten bijwerken en beheren van gebruikersaccounts op een virtuele machine (VM) in Azure. Deze cloud init-scripts uitgevoerd op opstarten als u een virtuele machine met de Azure CLI 2.0 maken. Zie voor een meer gedetailleerd overzicht over het installeren van toepassingen, schrijven configuratiebestanden en sleutels van de Sleutelkluis invoeren, [in deze zelfstudie](tutorial-automate-vm-deployment.md). U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](using-cloud-init-nodejs.md).


## <a name="cloud-init-overview"></a>Cloud-init-overzicht
[Cloud-init](https://cloudinit.readthedocs.io) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud init gebruiken voor het installeren van pakketten en bestanden schrijven of om gebruikers en beveiliging te configureren. Als de initialisatie van de cloud wordt uitgevoerd tijdens het opstartproces, zijn er geen extra stappen of vereist agents naar uw configuratie toe te passen.

Cloud-init werkt ook via distributies. Bijvoorbeeld, u niet gebruikt **apt get-installatie** of **yum installeren** om een pakket te installeren. In plaats daarvan kunt u een lijst met pakketten te installeren. Het hulpprogramma voor systeemeigen pakket cloud init automatisch gebruikt voor de distro die u selecteert.

We werken met onze partners ophalen van cloud-init opgenomen en in de afbeeldingen die ze naar Azure bieden werkt. De volgende tabel geeft een overzicht van de huidige beschikbaarheid van de cloud init op Azure-platform installatiekopieën:

| Alias | Uitgever | Aanbieding | SKU | Versie |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 TNS |meest recente |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |meest recente |
| CoreOS |CoreOS |CoreOS |Stabiel |meest recente |


## <a name="set-the-hostname-with-cloud-init"></a>De hostnaam met cloud-init instellen
Cloud-init-bestanden zijn geschreven in [YAML](http://www.yaml.org). Een cloud-init-script uitvoeren bij het maken van een virtuele machine in Azure met [az vm maken](/cli/azure/vm#create), geeft u het cloud-init-bestand met de `--custom-data` overschakelen. Bekijk enkele voorbeelden van wat u met een cloud-init-bestand configureren kunt. Een veelvoorkomend scenario is het instellen van de hostnaam van een virtuele machine. De hostnaam is standaard hetzelfde zijn als de naam van de VM. 

Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt de resourcegroep met de naam *myResourceGroup* in de *eastus* locatie:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak een bestand met de naam in uw huidige shell *cloud_init_hostname.txt* en plak de volgende configuratie. Maak bijvoorbeeld het bestand in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer in de Cloud-Shell `sensible-editor cloud_init_hostname.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel:

```yaml
#cloud-config
hostname: myhostname
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geef het cloud-init-bestand met `--custom-data cloud_init_hostname.txt` als volgt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMHostname \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_hostname.txt
```

Zodra gemaakt, bevat de Azure CLI informatie over de virtuele machine. Gebruik de `publicIpAddress` naar SSH met uw virtuele machine. Voer uw eigen adres als volgt:

```bash
ssh azureuser@publicIpAddress
```

Als de naam van de virtuele machine wilt weergeven, gebruikt de `hostname` opdracht als volgt:

```bash
hostname
```

De virtuele machine moet de hostnaam als deze waarde is ingesteld in de cloud init-bestand, zoals wordt weergegeven in de volgende voorbeelduitvoer report:

```bash
myhostname
```

## <a name="update-a-vm-with-cloud-init"></a>Bijwerken van een virtuele machine met cloud-init
Om veiligheidsredenen wilt u mogelijk een VM voor het toepassen van de meest recente updates op de eerste keer opstarten configureren. Als u cloud-init voor verschillende Linux-distributies werkt, is niet nodig om op te geven `apt` of `yum` voor de package manager. In plaats daarvan u definiëren `package_upgrade` en laat het cloud-init-proces bepalen van het juiste mechanisme voor het distro in gebruik. Deze werkstroom kunt u de dezelfde cloud init-scripts gebruiken via distributies.

Om het upgradeproces in actie zien, maakt u een cloud-init-bestand met de naam *cloud_init_upgrade.txt* en plak de volgende configuratie:

```yaml
#cloud-config
package_upgrade: true
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geef het cloud-init-bestand met `--custom-data cloud_init_upgrade.txt` als volgt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUpgrade \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_upgrade.txt
```

SSH kunt uitvoeren naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen openbare IP-adres:

```bash
ssh azureuser@publicIpAddress
```

De pakket-beheerprogramma en controleren op updates uitvoeren. Het volgende voorbeeld wordt `apt-get` op een Ubuntu-VM:

```bash
sudo apt-get upgrade
```

Omdat cloud init gecontroleerd en updates geïnstalleerd op opstarten, zijn er geen updates toe te passen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Een gebruiker toevoegen aan een virtuele machine met cloud-init
Een van de eerste taken op een nieuwe Linux-VM is het toevoegen van een gebruiker voor uzelf om te voorkomen dat het gebruik van *hoofdmap*. SSH-sleutels zijn best practice voor beveiliging en gebruiksgemak. Sleutels worden toegevoegd aan de *~/.ssh/authorized_keys* bestand met dit script cloud init.

Als u wilt een gebruiker toevoegt aan een Linux-VM, maak een cloud-init-bestand met de naam *cloud_init_add_user.txt* en plak de volgende configuratie. Uw eigen openbare sleutel opgeven (zoals de inhoud van *~/.ssh/id_rsa.pub*) voor *ssh-geautoriseerd-sleutels*:

```yaml
#cloud-config
users:
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```

Maak nu een virtuele machine met [az vm maken](/cli/azure/vm#create) en geef het cloud-init-bestand met `--custom-data cloud_init_add_user.txt` als volgt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUser \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_add_user.txt
```

SSH kunt uitvoeren naar het openbare IP-adres van uw virtuele machine wordt weergegeven in de uitvoer van de voorgaande opdracht. Voer uw eigen openbare IP-adres:

```bash
ssh myadminuser@publicIpAddress
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
Cloud-initialisatie is een van de standaard manieren om te wijzigen van uw virtuele Linux-machines op opstarten. In Azure, kunt u VM-extensies ook gebruiken om te wijzigen van uw Linux-VM op opstarten of wanneer deze wordt uitgevoerd. Bijvoorbeeld, kunt u de Azure VM-extensie voor een script uitvoeren op een actieve virtuele machine, niet alleen op eerst op te starten. Zie voor meer informatie over de VM-extensies [VM-extensies en functies](extensions-features.md), of Zie voor voorbeelden voor het gebruik van de extensie [gebruikers beheren, SSH, en controleer of het herstellen van schijven op Azure Linux VM's met behulp van de VMAccess-extensie](using-vmaccess-extension.md).
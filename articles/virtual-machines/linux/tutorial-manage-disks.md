---
title: 'Zelfstudie: Azure-schijven beheren met de Azure CLI | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Freeman Azure CLI 2.0 gebruikt voor het maken en beheren van Azure-schijven voor virtuele machines
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dff6af6a68dcc454877532c3d6f06cb86e6fe897
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial---manage-azure-disks-with-the-azure-cli-20"></a>Zelfstudie: Azure-schijven beheren met de Azure CLI 2.0

Virtuele machines in Azure gebruiken schijven voor het opslaan van het besturingssysteem, toepassingen en gegevens van virtuele machines. Bij het maken van een virtuele machine is het van belang dat u een schijfgrootte en configuratie kiest die geschikt zijn voor de verwachte werkbelasting. Deze zelfstudie bevat informatie over het implementeren en beheren van VM-schijven. U krijgt informatie over:

> [!div class="checklist"]
> * Besturingssysteemschijven en tijdelijke schijven
> * Gegevensschijven
> * Standard- en Premium-schijven
> * Schijfprestaties
> * Het koppelen en voorbereiden van gegevensschijven
> * De grootte van schijven wijzigen
> * Momentopnamen van schijven

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.30 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="default-azure-disks"></a>Standaard Azure-schijven

Wanneer een virtuele Azure-machine wordt gemaakt, worden automatisch twee schijven aan de virtuele machine gekoppeld. 

**Besturingssysteemschijf**: de grootte van besturingssysteemschijven kan worden gewijzigd in maximaal 1 terabyte en besturingssysteemschijven fungeren als host voor het besturingssysteem van de virtuele machine. De besturingssysteemschijf is standaard gelabeld met */dev/sda*. De schijfcacheconfiguratie van de besturingssysteemschijf is geoptimaliseerd voor besturingssysteemprestaties. Vanwege deze configuratie kan de besturingssysteemschijf **beter geen** toepassingen of gegevens hosten. Gebruik voor toepassingen en gegevens gegevensschijven, die verderop in dit artikel worden beschreven. 

**Tijdelijke schijf**: tijdelijke schijven gebruiken een SSD-schijf die zich op dezelfde Azure-host bevindt als de virtuele machine. Tijdelijke schijven leveren zeer goede prestaties en kunnen worden gebruikt voor bewerkingen als tijdelijke gegevensverwerking. Als de virtuele machine wordt verplaatst naar een nieuwe host, worden gegevens die zijn opgeslagen op een tijdelijke schijf echter verwijderd. De grootte van de tijdelijke schijf wordt bepaald door de VM-grootte. Tijdelijke schijven zijn gelabeld als */dev/sdb* en hebben een koppelpunt van */mnt*.

### <a name="temporary-disk-sizes"></a>Groottes van tijdelijke schijven

| Type | VM-grootte | Maximumgrootte van tijdelijke schijf (GB) |
|----|----|----|
| [Algemeen doel](sizes-general.md) | A- en D-serie | 800 |
| [Geoptimaliseerde rekenkracht](sizes-compute.md) | F-serie | 800 |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md) | D- en G-serie | 6144 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md) | L-serie | 5630 |
| [GPU](sizes-gpu.md) | N-serie | 1440 |
| [Hoge prestaties](sizes-hpc.md) | A- en H-serie | 2000 |

## <a name="azure-data-disks"></a>Azure-gegevensschijven

Er kunnen extra gegevensschijven worden toegevoegd voor het installeren van toepassingen en opslaan van gegevens. Gegevensschijven moeten worden gebruikt in situaties waarin duurzame en responsieve gegevensopslag gewenst is. Elke gegevensschijf heeft een maximale capaciteit van 1 terabyte. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven aan een virtuele machine kunnen worden gekoppeld. Voor elke VM-vCPU kunnen twee schijven worden gekoppeld. 

### <a name="max-data-disks-per-vm"></a>Max. aantal gegevensschijven per VM

| Type | VM-grootte | Max. aantal gegevensschijven per VM |
|----|----|----|
| [Algemeen doel](sizes-general.md) | A- en D-serie | 32 |
| [Geoptimaliseerde rekenkracht](sizes-compute.md) | F-serie | 32 |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md) | D- en G-serie | 64 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md) | L-serie | 64 |
| [GPU](sizes-gpu.md) | N-serie | 48 |
| [Hoge prestaties](sizes-hpc.md) | A- en H-serie | 32 |

## <a name="vm-disk-types"></a>Typen VM-schijven

Azure biedt twee typen schijven.

### <a name="standard-disk"></a>Standard-schijf

Standard Storage wordt ondersteund door HDD's en biedt voordelige en hoogwaardige opslag. Standard-schijven zijn ideaal voor een kostenefficiënte werkbelasting voor ontwikkelen en testen.

### <a name="premium-disk"></a>Premium-schijf

Premium-schijven worden ondersteund door hoogwaardige schijven met een lage latentie op basis van SSD. Ideaal voor virtuele machines met een productiewerkbelasting. Premium Storage ondersteunt virtuele machines uit de DS-serie, DSv2-serie GS-serie en FS-serie. Premium-schijven worden geleverd in drie typen (P10, P20, P30); de grootte van de schijf bepaalt het schijftype. Wanneer u een selectie maakt, wordt de waarde van de schijfgrootte afgerond naar het volgende type. Als de schijfgrootte bijvoorbeeld kleiner dan 128 GB is, is het schijftype P10. Als de schijfgrootte tussen 129 en 512 GB is, is het type een P20. Van alles dat meer dan 512 GB is, is het type een P30.

### <a name="premium-disk-performance"></a>Prestaties Premium-schijf

|Schijftype voor Premium Storage | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Schijfgrootte (afronden) | 128 GB | 512 GB | 1.024 GB (1 TB) |
| Max. aantal IOP's per schijf | 500 | 2.300 | 5.000 |
Doorvoer per schijf | 100 MB/s | 150 MB/s | 200 MB/s |

In de bovenstaande tabel wordt het max. IOP's per schijf aangegeven, maar er kan een hoger prestatieniveau worden bereikt door striping van meerdere gegevensschijven. Een virtuele machine van het type Standard_GS5 kan bijvoorbeeld maximaal 80.000 IOPS bereiken. Zie [Linux VM-grootten](sizes.md) voor gedetailleerde informatie over het maximum aantal IOP's per VM.

## <a name="create-and-attach-disks"></a>Schijven maken en koppelen

Gegevensschijven kunnen worden gemaakt en aan een VM worden gekoppeld tijdens het maken of aan een bestaande virtuele machine worden gekoppeld.

### <a name="attach-disk-at-vm-creation"></a>Schijf koppelen tijdens het maken van de virtuele machine

Een resourcegroep maken met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Maak een VM met de opdracht [az vm create]( /cli/azure/vm#az_vm_create). In het volgende voorbeeld wordt de VM *myVM* gemaakt, wordt het gebruikersaccount *azureuser* toegevoegd en worden SSH-sleutels gemaakt als deze nog niet bestaan. Het argument `--datadisk-sizes-gb` wordt gebruikt om op te geven dat een extra schijf moet worden gemaakt en gekoppeld aan de virtuele machine. Als u meer dan één schijf wilt maken en koppelen, gebruikt u een door spaties gescheiden lijst met waarden voor schijfgroottes. In het volgende voorbeeld wordt een virtuele machine gemaakt met twee gegevensschijven, beide van 128 GB. Omdat de schijfgrootte 128 GB is, zijn deze schijven beide geconfigureerd als P10, dat maximaal 500 IOP's per schijf biedt.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 128 128
```

### <a name="attach-disk-to-existing-vm"></a>Een schijf koppelen aan een bestaande virtuele machine

Als u een nieuwe schijf wilt maken en koppelen aan een bestaande virtuele machine wilt maken, gebruikt u de opdracht [az vm disk attach](/cli/azure/vm/disk#az_vm_disk_attach). In het volgende voorbeeld wordt een Premium-schijf gemaakt met een grootte van 128 GB en gekoppeld aan de virtuele machine die u in de vorige stap hebt gemaakt.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Gegevensschijven voorbereiden

Wanneer een schijf is gekoppeld aan de virtuele machine, moet het besturingssysteem worden geconfigureerd voor gebruik van de schijf. Het volgende voorbeeld laat zien hoe u een schijf handmatig configureert. Dit proces kan ook worden geautomatiseerd met behulp van cloud-init, wat wordt beschreven in een [latere zelfstudie](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Handmatige configuratie

Maak een SSH-verbinding met de virtuele machine. Vervang het voorbeeld van een IP-adres door het openbare IP-adres van de virtuele machine.

```azurecli-interactive 
ssh 52.174.34.95
```

Partitioneer de schijf met `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Schrijf een bestandssysteem naar de partitie met behulp van de opdracht `mkfs`.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Koppel de nieuwe schijf, zodat deze toegankelijk is in het besturingssysteem.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

De schijf kan nu worden geopend via het *datadrive*-koppelpunt. Dit kan worden gecontroleerd door de opdracht `df -h` uit te voeren. 

```bash
df -h
```

De uitvoer geeft het nieuwe station weer dat is gekoppeld aan */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Om ervoor te zorgen dat het station na het opnieuw opstarten opnieuw wordt gekoppeld, moet het worden toegevoegd aan het bestand */etc/fstab*. Haal hiervoor de UUID van de schijf op met het hulpprogramma `blkid`.

```bash
sudo -i blkid
```

De uitvoer geeft de UUID van het station weer, in dit geval `/dev/sdc1`.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Voeg een regel toe aan het bestand */etc/fstab* die vergelijkbaar is met de volgende.

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail   1  2
```

Nu de schijf is geconfigureerd, sluit u de SSH-sessie.

```bash
exit
```

## <a name="resize-vm-disk"></a>Grootte van VM-schijf wijzigen

Wanneer een virtuele machine is geïmplementeerd, kunnen het besturingssysteem of aangesloten gegevensschijven worden vergroot. Het vergroten van een schijf is handig wanneer u meer opslagruimte of hogere prestaties (P10, P20, P30) nodig hebt. De grootte van schijven kan niet worden verkleind.

U hebt de naam of id van de schijf nodig als u de grootte ervan wilt wijzigen. Gebruik de opdracht [az disk list](/cli/azure/disk#az_disk_list) om alle schijven in een resourcegroep te retourneren. Noteer de naam van de schijf die u wilt vergroten of verkleinen.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

Ook moet de toewijzing van de virtuele machine ongedaan worden gemaakt. Gebruik de opdracht [az vm deallocate]( /cli/azure/vm#az_vm_deallocate) om de virtuele machine te stoppen en de toewijzing van de virtuele machine ongedaan te maken.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Gebruik de opdracht [az disk update](/cli/azure/vm/disk#az_vm_disk_update) om de grootte van de schijf te wijzigen. In dit voorbeeld wordt de grootte van een schijf met de naam *myDataDisk* gewijzigd in 1 terabyte.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Nadat de grootte is gewijzigd, start u de virtuele machine.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Als u de grootte van de besturingssysteemschijf hebt gewijzigd, wordt de partitie automatisch uitgebreid. Als u de grootte van een gegevensschijf hebt gewijzigd, moeten alle huidige partities worden uitgebreid in het besturingssysteem van de virtuele machine.

## <a name="snapshot-azure-disks"></a>Momentopname maken van Azure-schijven

Met het maken van een momentopname van de schijf, wordt een alleen-lezen, tijdgebonden kopie van de schijf gemaakt. Azure VM-momentopnamen zijn handig om snel de status van een virtuele machine op te slaan voordat u configuratiewijzigingen aanbrengt. Als configuratiewijzigingen ongewenst blijken te zijn, kan de status van de virtuele machine worden hersteld met behulp van de momentopname. Wanneer een virtuele machine meer dan één schijf heeft, wordt van elke schijf een momentopname gemaakt, onafhankelijk van de andere schijven. Overweeg de virtuele machine te stoppen voordat u momentopnamen van de schijf maakt, zodat u toepassingsconsistente back-ups maakt. U kunt ook de [Azure Backup-service](/azure/backup/) gebruiken, waarmee u automatische back-ups kunt maken terwijl de virtuele machine wordt uitgevoerd.

### <a name="create-snapshot"></a>Momentopname maken

Voor het maken van een momentopname van de schijf van een virtuele machine hebt u de id of naam van de schijf nodig. Gebruik de opdracht [az vm show](https://docs.microsoft.com/cli/azure/vm#az_vm_show) om de schijf-id op te halen. In dit voorbeeld wordt de schijf-id opgeslagen in een variabele, zodat deze in een later stadium kan worden gebruikt.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Nu u de id van de schijf van de virtuele machine hebt, kunt u met de volgende opdracht een momentopname van de schijf maken.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Schijf maken op basis van een momentopname

Deze momentopname kan vervolgens worden geconverteerd naar een schijf die kan worden gebruikt om de virtuele machine opnieuw te maken.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Virtuele machine herstellen op basis van een momentopname

Voor het herstellen van de virtuele machine dient u de bestaande virtuele machine te verwijderen. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Maak een nieuwe virtuele machine op basis van de momentopname van de schijf.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Gegevensschijf opnieuw koppelen

Alle gegevensschijven moeten opnieuw worden gekoppeld aan de virtuele machine.

Zoek eerst de naam van de gegevensschijf op met de opdracht [az disk list](https://docs.microsoft.com/cli/azure/disk#az_disk_list). In dit voorbeeld wordt de naam van de schijf in een variabele met de naam *datadisk* geplaatst, die in de volgende stap wordt gebruikt.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Gebruik de opdracht [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) om de schijf te koppelen.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u meer geleerd over onderwerpen over VM-schijven, zoals:

> [!div class="checklist"]
> * Besturingssysteemschijven en tijdelijke schijven
> * Gegevensschijven
> * Standard- en Premium-schijven
> * Schijfprestaties
> * Het koppelen en voorbereiden van gegevensschijven
> * De grootte van schijven wijzigen
> * Momentopnamen van schijven

In de volgende zelfstudie leert u hoe u de configuratie van virtuele machines automatiseert.

> [!div class="nextstepaction"]
> [VM-configuratie automatiseren](./tutorial-automate-vm-deployment.md)

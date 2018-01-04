---
title: Beheren van Azure-schijven met de Azure CLI | Microsoft Docs
description: Zelfstudie - Azure-schijven met de Azure CLI beheren
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 16cc0c5e38eb273fc2504a39497d00c76d666316
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="manage-azure-disks-with-the-azure-cli"></a>Azure-schijven met de Azure CLI beheren

Virtuele machines in Azure schijven gebruiken voor het opslaan van het besturingssysteem, toepassingen en gegevens van virtuele machines. Bij het maken van een virtuele machine is het van belang dat u kiest een grootte van de schijf en de configuratie geschikt is voor de verwachte werkbelasting. Deze zelfstudie bevat informatie over het implementeren en beheren van VM-schijven. U meer informatie over:

> [!div class="checklist"]
> * OS-schijven en tijdelijke schijven
> * Gegevensschijven
> * Standard en Premium-schijven
> * Prestaties van de schijf
> * Koppelen en gegevensschijven voorbereiden
> * De schijfgrootte
> * Schijf momentopnamen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Standaard Azure-schijven

Wanneer een virtuele machine van Azure is gemaakt, worden twee schijven automatisch gekoppeld aan de virtuele machine. 

**Besturingssysteemschijf** -systeemschijven kan worden verkleind tot 1 terabyte is, en fungeert als host voor het besturingssysteem van de virtuele machines. De besturingssysteemschijf is gelabeld */dev/sda* standaard. De configuratie van de besturingssysteemschijf van de schijfcache is geoptimaliseerd voor OS-prestaties. Vanwege deze configuratie de besturingssysteemschijf **beter niet** toepassingen of gegevens hosten. Gebruik voor toepassingen en gegevens gegevensschijven, verderop in dit artikel worden beschreven. 

**Tijdelijke schijf** -tijdelijke schijven gebruiken een SSD-schijf die zich op dezelfde Azure host als de virtuele machine. Tijdelijke schijven zijn maximaal zodat en kunnen worden gebruikt voor bewerkingen, zoals tijdelijke gegevensverwerking. Als de virtuele machine wordt verplaatst naar een nieuwe host, wordt echter gegevens die zijn opgeslagen op een tijdelijke schijf verwijderd. De grootte van de tijdelijke schijf wordt bepaald door de VM-grootte. Tijdelijke schijven zijn gelabeld */dev/sdb* en hebben een koppelpunt van *mnt*.

### <a name="temporary-disk-sizes"></a>Tijdelijke schijfgrootten

| Type | VM-grootte | Maximumgrootte van tijdelijke schijf (GB) |
|----|----|----|
| [Algemeen doel](sizes-general.md) | A en D-reeks | 800 |
| [Geoptimaliseerde rekenkracht](sizes-compute.md) | F-serie | 800 |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md) | D en G-serie | 6144 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md) | L-reeks | 5630 |
| [GPU](sizes-gpu.md) | N reeks | 1440 |
| [Hoge prestaties](sizes-hpc.md) | A en H reeks | 2000 |

## <a name="azure-data-disks"></a>Azure gegevensschijven

Extra gegevensschijven kunnen worden toegevoegd voor het installeren van toepassingen en gegevens op te slaan. Gegevensschijven moeten worden gebruikt in een situatie waarin de opslag van gegevens duurzaam en responsief gewenst is. Elke gegevensschijf heeft een maximale capaciteit van 1 terabyte. De grootte van de virtuele machine bepaalt hoeveel gegevensschijven kunnen worden gekoppeld aan een virtuele machine. Voor elke vCPU VM kunnen twee schijven worden gekoppeld. 

### <a name="max-data-disks-per-vm"></a>Maximum aantal gegevensschijven per VM

| Type | VM-grootte | Maximum aantal gegevensschijven per VM |
|----|----|----|
| [Algemeen doel](sizes-general.md) | A en D-reeks | 32 |
| [Geoptimaliseerde rekenkracht](sizes-compute.md) | F-serie | 32 |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md) | D en G-serie | 64 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md) | L-reeks | 64 |
| [GPU](sizes-gpu.md) | N reeks | 48 |
| [Hoge prestaties](sizes-hpc.md) | A en H reeks | 32 |

## <a name="vm-disk-types"></a>VM-schijftypen

Azure biedt twee typen van de schijf.

### <a name="standard-disk"></a>Standard-schijven

Standard Storage wordt ondersteund door HDD's en biedt voordelige en hoogwaardige opslag. Standaardschijven zijn ideaal voor een voordelige ontwikkelen en testen werkbelasting.

### <a name="premium-disk"></a>Premium-schijf

Premium-schijven worden ondersteund door de hoge prestaties, lage latentie SSD-schijf. Ideaal voor virtuele machines met productie werkbelasting. Premium-opslag ondersteunt DS-serie, DSv2-serie GS-serie en virtuele machines FS-serie. Premium-schijven zijn drie typen (P10, P20, P30), de grootte van de schijf bepaalt het schijftype. Wanneer u selecteert, wordt de een de waarde van de schijfgrootte afgerond naar het volgende type. Als de schijfgrootte minder dan 128 GB is, is het schijftype P10. Als de schijfgrootte tussen 129 en 512 GB, is de grootte een P20. Meer dan 512 GB, de grootte van een P30 is.

### <a name="premium-disk-performance"></a>Premium-schijfprestaties

|Premium-opslag schijftype | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Grootte van de schijf (afronden) | 128 GB | 512 GB | 1.024 GB (1 TB) |
| Max. aantal IOP's per schijf | 500 | 2,300 | 5,000 |
Doorvoer per schijf | 100 MB/s | 150 MB/s | 200 MB/s |

Terwijl de bovenstaande tabel max. IOP's per schijf identificeert, kan een hoger niveau van de prestaties worden bereikt door meerdere gegevensschijven te verwijderen. Een VM Standard_GS5 kunt bijvoorbeeld een maximumaantal IOPS 80.000 bereiken. Zie voor gedetailleerde informatie over max. IOP's per VM [Linux VM-grootten](sizes.md).

## <a name="create-and-attach-disks"></a>Maken en koppelen van schijven

Gegevensschijven worden gemaakt en gekoppeld tijdens de aanmaak van de VM of naar een bestaande virtuele machine.

### <a name="attach-disk-at-vm-creation"></a>Schijf bij het maken van de virtuele machine koppelen

Een resourcegroep maken met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Maak een virtuele machine met de [az vm maken]( /cli/azure/vm#create) opdracht. De `--datadisk-sizes-gb` argument wordt gebruikt om op te geven dat een extra schijf moet worden gemaakt en gekoppeld aan de virtuele machine. Gebruik wilt maken en koppelen van meer dan één schijf, een door spaties gescheiden lijst met grootten van de schijf. In het volgende voorbeeld wordt een virtuele machine gemaakt met twee gegevensschijven, beide 128 GB. Omdat de schijfgrootte 128 GB, zijn deze schijven geconfigureerd als P10s waarmee maximaal 500 IOP's per schijf.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a>Schijf koppelen aan bestaande virtuele machine

Als u een nieuwe schijf koppelen aan een bestaande virtuele machine wilt maken, gebruikt u de [az vm schijf koppelen](/cli/azure/vm/disk#attach) opdracht. Het volgende voorbeeld maakt een premium-schijf 128 GB groot, en deze is gekoppeld aan de virtuele machine in de vorige stap hebt gemaakt.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Gegevensschijven voorbereiden

Zodra een schijf is gekoppeld aan de virtuele machine, moet het besturingssysteem worden geconfigureerd voor gebruik van de schijf. Het volgende voorbeeld laat zien hoe handmatig configureren van een schijf. Dit proces kan ook worden geautomatiseerd met behulp van cloud-init, die wordt beschreven in een [hoger zelfstudie](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Handmatige configuratie

Maak een SSH-verbinding met de virtuele machine. De voorbeeld-IP-adres vervangen door het openbare IP-adres van de virtuele machine.

```azurecli-interactive 
ssh 52.174.34.95
```

De schijf met partitioneren `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Een bestandssysteem schrijven naar de partitie met behulp van de `mkfs` opdracht.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

De nieuwe schijf koppelen zodat deze toegankelijk is in het besturingssysteem.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

De schijf kan nu worden geopend via de *datadrive* koppelpunt die kan worden gecontroleerd door het uitvoeren van de `df -h` opdracht. 

```bash
df -h
```

De uitvoer ziet u het nieuwe station gekoppeld aan */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Om ervoor te zorgen dat het station na opnieuw opstarten opnieuw is gekoppeld, moet deze worden toegevoegd aan de */etc/fstab* bestand. Om dit te doen, krijgen de UUID van de schijf met de `blkid` hulpprogramma.

```bash
sudo -i blkid
```

De uitvoer geeft de UUID van het station `/dev/sdc1` in dit geval.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Toevoegen van een regel vergelijkbaar met het volgende wanneer u de */etc/fstab* bestand. Ook dat barrières schrijven kan worden uitgeschakeld met *blokkade = 0*, deze configuratie kan de schijf worden verbeterd. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Nu dat de schijf is geconfigureerd, sluit u de SSH-sessie.

```bash
exit
```

## <a name="resize-vm-disk"></a>Formaat van VM-schijf

Wanneer een virtuele machine is geïmplementeerd, kunnen het besturingssysteem of een aangesloten gegevensschijven worden vergroot. Het vergroten van een schijf is handig wanneer hoeven meer opslagruimte of een hoger niveau van de prestaties (P10, P20, P30). Houd er rekening mee schijven in grootte kunnen niet worden verhoogd.

Voordat u voor de grootte van de schijf, is de Id of naam van de schijf nodig. Gebruik de [az Schijflijst](/cli/azure/disk#az_disk_list) opdracht voor het retourneren van alle schijven in een resourcegroep. Noteer de naam van de schijf die u wilt vergroten of verkleinen.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

De virtuele machine moet ook ongedaan. Gebruik de [az vm ongedaan]( /cli/azure/vm#deallocate) opdracht om te stoppen en de VM ongedaan gemaakt.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Gebruik de [az schijf update](/cli/azure/vm/disk#update) opdracht het formaat van de schijf. In dit voorbeeld wordt een schijf met de naam het formaat *myDataDisk* naar 1 terabyte.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Nadat de bewerking formaat is voltooid, start u de virtuele machine.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Als u het formaat van de schijf van het besturingssysteem hebt gewijzigd, wordt de partitie automatisch uitgebreid. Als u het formaat van een gegevensschijf hebt gewijzigd, moeten alle huidige partities worden uitgebreid in het besturingssysteem van de virtuele machines.

## <a name="snapshot-azure-disks"></a>Momentopname maken van Azure-schijven

Maken van een momentopname van de schijf, maakt een lezen alleen, punt in tijd kopie van de schijf. Azure VM-momentopnamen zijn handig voor het opslaan van snel de status van een virtuele machine voordat u configuratiewijzigingen in. In het geval van wijzigingen in de configuratie blijken ongewenste, kan status virtuele machine worden hersteld met behulp van de momentopname. Wanneer een virtuele machine meer dan één schijf heeft, wordt een momentopname gemaakt van elke schijf onafhankelijk van de andere. Houd rekening met de virtuele machine stoppen voordat het maken van momentopnamen van de schijf voor het maken van de toepassing consistente back-ups. U kunt ook de [Azure Backup-service](/azure/backup/), waardoor u automatische back-ups uitvoeren terwijl de virtuele machine wordt uitgevoerd.

### <a name="create-snapshot"></a>Momentopname maken

Voordat u een momentopname van de virtuele machine schijf maakt, is de Id of naam van de schijf nodig. Gebruik de [az vm weergeven](https://docs.microsoft.com/cli/azure/vm#az_vm_show) opdracht voor het retourneren van de schijf-id. In dit voorbeeld wordt de schijf-id opgeslagen in een variabele, zodat deze kan worden gebruikt in een later stadium.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Nu dat u de id van de schijf van de virtuele machine hebt, maakt de volgende opdracht u een momentopname van de schijf.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Schijf maken vanuit een momentopname.

Deze momentopname kan vervolgens worden geconverteerd naar een schijf die kan worden gebruikt om de virtuele machine opnieuw te maken.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Virtuele machine herstellen vanuit een momentopname.

Om te demonstreren herstel van virtuele machine, verwijder de bestaande virtuele machine. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Maak een nieuwe virtuele machine van de momentopname-schijf.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Gegevensschijf opnieuw koppelen

Alle gegevensschijven moeten opnieuw worden gekoppeld aan de virtuele machine.

Zoek eerst de gegevens schijf naam met de [az Schijflijst](https://docs.microsoft.com/cli/azure/disk#az_disk_list) opdracht. In dit voorbeeld wordt de naam van de schijf in een variabele met de naam *datadisk*, die wordt gebruikt in de volgende stap.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Gebruik de [az vm schijf koppelen](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) opdracht om de schijf te koppelen.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over VM schijven onderwerpen, zoals:

> [!div class="checklist"]
> * OS-schijven en tijdelijke schijven
> * Gegevensschijven
> * Standard en Premium-schijven
> * Prestaties van de schijf
> * Koppelen en gegevensschijven voorbereiden
> * De schijfgrootte
> * Schijf momentopnamen

Ga naar de volgende zelfstudie voor meer informatie over het automatiseren van VM-configuratie.

> [!div class="nextstepaction"]
> [VM-configuratie automatiseren](./tutorial-automate-vm-deployment.md)

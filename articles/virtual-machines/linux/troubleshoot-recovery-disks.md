---
title: Gebruik een op Linux VM met de Azure CLI 2.0 probleemoplossing | Microsoft Docs
description: Meer informatie over het oplossen van problemen van Linux VM door verbinding te maken van de besturingssysteemschijf voor een herstel-VM met de Azure CLI 2.0
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 7a28accce1bd328b2b486b588c44d91b03e42122
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli-20"></a>Problemen oplossen van een Linux-VM met de OS-schijf koppelen aan een herstel-VM met de Azure CLI 2.0
Als uw virtuele Linux-machine (VM) een opstart- of -fout optreedt, moet u wellicht de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab` die verhindert dat de virtuele machine kunnen opstarten is. Dit artikel wordt uitgelegd hoe u met de Azure CLI 2.0 verbinding maken met de virtuele harde schijf aan een andere Linux VM eventuele fouten te corrigeren en vervolgens opnieuw maken van de oorspronkelijke VM. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine zonder problemen, om de virtuele harde schijven te houden.
2. Koppelen en koppel de virtuele harde schijf aan een andere Linux VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerken van bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine maken met de oorspronkelijke virtuele harde schijf.

Voor deze stappen voor probleemoplossing, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

In de volgende voorbeelden kunt u parameternamen vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `mystorageaccount`, en `myVM`.


## <a name="determine-boot-issues"></a>Opstartproblemen bepalen
Bekijk de uitvoer seriële om te bepalen waarom de virtuele machine kan niet correct worden opgestart. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab`, of de onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

De logboeken opstarten met [az vm diagnostische gegevens over opstarten get-boot-logboek](/cli/azure/vm/boot-diagnostics#get-boot-log). Het volgende voorbeeld wordt de uitvoer van de seriële van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

Bekijk de uitvoer van de seriële om te bepalen waarom de virtuele machine kan niet worden opgestart. Als de uitvoer van de seriële is niet een aanwijzing biedt, moet u mogelijk Raadpleeg logboekbestanden in `/var/log` zodra u de virtuele harde schijf is verbonden met een VM voor het oplossen van problemen.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijf details weergeven
Voordat u de virtuele harde schijf (VHD) aan een andere virtuele machine koppelen kunt, moet u de URI van de besturingssysteemschijf te identificeren. 

Informatie weergeven over uw virtuele machine met [az vm weergeven](/cli/azure/vm#show). Gebruik de `--query` vlag uitpakken van de URI moet een schijf met het besturingssysteem. Het volgende voorbeeld wordt informatie over de schijven voor de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

De URI is vergelijkbaar met **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.

## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf zijn gewoon metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de OS-schijf koppelen aan een virtuele machine, zelfs wanneer die VM een status gestopt en de toewijzing ongedaan is gemaakt heeft.

De eerste stap voor het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine wordt verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine kunt u de fouten oplossen.

Verwijderen van de virtuele machine met [az vm verwijderen](/cli/azure/vm#delete). Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` uit de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

Wacht totdat de virtuele machine verwijderen voordat u de virtuele harde schijf aan een andere virtuele machine koppelen is voltooid. De lease op de virtuele harde schijf waarin deze zijn gekoppeld aan de virtuele machine moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelen de bestaande virtuele harde schijf aan deze VM voor probleemoplossing om te bladeren en de inhoud van de schijf bewerken. Dit proces kunt u eventuele configuratiefouten te corrigeren of extra toepassing of een systeem-logboekbestanden, bijvoorbeeld controleren. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

Koppel de bestaande virtuele harde schijf met [zonder begeleiding az vm-schijf koppelen](/cli/azure/vm/unmanaged-disk#attach). Wanneer u de bestaande virtuele harde schijf koppelt, geeft u de URI naar de schijf die is verkregen in de voorgaande `az vm show` opdracht. Het volgende voorbeeld wordt een bestaande virtuele harde schijf voor het oplossen van problemen met de naam VM `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a>Koppel de gekoppelde gegevensschijf

> [!NOTE]
> De volgende voorbeelden worden de stappen die nodig is op een VM Ubuntu in detail beschreven. Als u gebruikmaakt van een andere Linux distro zoals Red Hat Enterprise Linux of SUSE, de locaties van het logboekbestand en `mount` opdrachten mogelijk enigszins anders. Raadpleeg de documentatie voor uw specifieke distro voor de benodigde wijzigingen in de opdrachten.

1. SSH met uw probleemoplossing virtuele machine met de juiste referenties. Als u deze schijf is de eerste gegevensschijf gekoppeld aan uw VM voor het oplossen van problemen, de schijf waarschijnlijk is verbonden met `/dev/sdc`. Gebruik `dmseg` om gekoppelde schijven weer te geven:

    ```bash
    dmesg | grep SCSI
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    In het voorgaande voorbeeld de OS-schijf is op `/dev/sda` en de tijdelijke schijf die is opgegeven voor elke virtuele machine is op `/dev/sdb`. Als u meerdere gegevensschijven had, moeten ze op `/dev/sdd`, `/dev/sde`, enzovoort.

2. Maak een map voor het koppelen van uw bestaande virtuele harde schijf. Het volgende voorbeeld wordt een map met de naam `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Als u meerdere partities op de bestaande virtuele harde schijf hebt, koppelt u de vereiste partitie. Het volgende voorbeeld koppelt de eerste primaire partitie op `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Er is een aanbevolen procedure gegevensschijven koppelen aan virtuele machines in Azure met behulp van de UUID universally unique identifier () van de virtuele harde schijf. In dit scenario voor korte voor probleemoplossing voor is het koppelen van de virtuele harde schijf met de UUID niet nodig. Echter, bij normaal gebruik bewerken `/etc/fstab` koppelen van virtuele harde schijven met de apparaatnaam van het in plaats van UUID kan ertoe leiden dat de virtuele machine niet kunnen worden opgestart.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Los problemen op de oorspronkelijke virtuele harde schijf
Met de bestaande virtuele harde schijf dat wordt gekoppeld, kunt u nu onderhoud en stappen voor probleemoplossing naar behoefte uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Ontkoppel en loskoppelen van de oorspronkelijke virtuele harde schijf
Zodra de fouten opgelost zijn, kunt u ontkoppelen en loskoppelen van de bestaande virtuele harde schijf van uw VM voor het oplossen van problemen. U kunt de virtuele harde schijf niet gebruiken met eventuele andere virtuele machine totdat de lease koppelen van de virtuele harde schijf voor het oplossen van problemen VM is uitgebracht.

1. Ontkoppel de bestaande virtuele harde schijf van de SSH-sessie voor uw VM voor het oplossen van problemen. Eerst buiten de bovenliggende map voor uw koppelpunt wijzigen:

    ```bash
    cd /
    ```

    Nu Ontkoppel de bestaande virtuele harde schijf. Het volgende voorbeeld het apparaat op ontkoppelt `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nu loskoppelen van de virtuele harde schijf van de virtuele machine. De SSH-sessie voor de probleemoplossing VM afsluiten. Lijst van de schijven bijgesloten gegevens voor uw VM het oplossen van problemen met [az vm zonder begeleiding schijf lijst](/cli/azure/vm/unmanaged-disk#list). Het volgende voorbeeld worden de gegevensschijven gekoppeld aan de virtuele machine met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    Noteer de naam van uw bestaande virtuele harde schijf. Bijvoorbeeld, de naam van een schijf met de URI van **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**. 

    Ontkoppel de gegevensschijf van uw VM [zonder begeleiding az vm-schijf loskoppelen](/cli/azure/vm/unmanaged-disk#detach). Het volgende voorbeeld wordt losgekoppeld van de schijf met de naam `myVHD` van de virtuele machine met de naam `myVMRecovery` in de `myResourceGroup` resourcegroep:

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine van de oorspronkelijke harde schijf maken
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). De werkelijke JSON-sjabloon is op de volgende koppeling:

- https://RAW.githubusercontent.com/Azure/Azure-QuickStart-templates/master/201-VM-Specialized-VHD/azuredeploy.JSON

De sjabloon implementeert u een virtuele machine met behulp van de VHD-URI van de vorige opdracht. Implementeren van de sjabloon met [az implementatie maken](/cli/azure/group/deployment#create). De URI voor uw oorspronkelijke VHD bieden en geef het type besturingssysteem, de VM-grootte en de naam van de VM als volgt:

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten weer inschakelen
Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, kan boot diagnostics niet automatisch worden ingeschakeld. Schakel diagnostische gegevens over opstarten met [az vm-diagnostische gegevens over opstarten inschakelen](/cli/azure/vm/boot-diagnostics#enable). Het volgende voorbeeld wordt de diagnostische uitbreiding op de virtuele machine met de naam `myDeployedVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, raadpleegt u [oplossen SSH-verbindingen met een Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zie voor problemen met de toegang tot toepassingen die worden uitgevoerd op de virtuele machine [oplossen verbindingsproblemen van toepassing op een Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
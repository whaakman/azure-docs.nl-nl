---
title: Gebruik van een Linux-VM met de Azure CLI 2.0 probleemoplossing | Microsoft Docs
description: Informatie over het oplossen van problemen met Linux-VM door verbinding te maken van de besturingssysteemschijf aan een virtuele machine met behulp van de Azure CLI 2.0 voor herstel
services: virtual-machines-linux
documentationCenter: ''
authors: iainfoulds
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 91c00f8ae933cc071f18c8e24634fafb4d64be79
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902204"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli-20"></a>Een Linux-VM oplossen door de besturingssysteemschijf koppelen aan een virtuele machine met de Azure CLI 2.0 voor herstel
Als uw Linux-machine (VM) een fout opstart- of schijffout optreedt, moet u mogelijk de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab` dat voorkomt dat de virtuele machine wordt het opstarten. Dit artikel wordt uitgelegd hoe u verbinding maken met de virtuele harde schijf met een andere Linux-VM op eventuele fouten te corrigeren en vervolgens de oorspronkelijke virtuele machine opnieuw te maken met de Azure CLI 2.0. 


## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine problemen worden aangetroffen, de virtuele harde schijven te houden.
2. Toevoegen en koppelen van de virtuele harde schijf met een andere Linux-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerk bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine met behulp van de oorspronkelijke virtuele harde schijf maken.

Voor de virtuele machine die gebruikmaakt van beheerde schijf, Zie [een beheerde schijf-VM oplossen door het koppelen van een nieuwe schijf met besturingssysteem](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).

Als u deze stappen voor probleemoplossing, moet u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u namen van parameters vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten `myResourceGroup`, `mystorageaccount`, en `myVM`.


## <a name="determine-boot-issues"></a>Opstartproblemen met bepalen
Bekijk de seriële uitvoer om te bepalen waarom de virtuele machine niet staat zijn om correct is. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab`, of de onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

Ophalen van de opstartlogboeken met [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log). Het volgende voorbeeld wordt de seriële uitvoer van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

Controleer de seriële uitvoer om te bepalen waarom de virtuele machine is mislukt om op te starten. Als de seriële uitvoer is niet een aanwijzing bieden, moet u mogelijk om te controleren van de logboekbestanden in `/var/log` beschikt u over de virtuele harde schijf die zijn verbonden met een virtuele machine voor probleemoplossing.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijfdetails weergeven
Voordat u de virtuele harde schijf (VHD) aan een andere virtuele machine koppelen kunt, moet u voor het identificeren van de URI van de besturingssysteemschijf. 

Informatie weergeven over uw virtuele machine met [az vm show](/cli/azure/vm#az_vm_show). Gebruik de `--query` vlag om op te halen van de URI voor de besturingssysteemschijf. Het volgende voorbeeld wordt informatie over de schijf voor de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

De URI is vergelijkbaar met **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.

## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf bestaat uit metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer die zijn gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de besturingssysteemschijf koppelen aan een virtuele machine, zelfs wanneer die virtuele machine in een status gestopt en toewijzing ongedaan gemaakt.

De eerste stap bij het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine is verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine oplossen van problemen en los de fouten.

Verwijderen van de virtuele machine met [az vm verwijderen](/cli/azure/vm#az_vm_delete). Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` uit de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

Wacht totdat de virtuele machine verwijderd is voordat u de virtuele harde schijf aan een andere virtuele machine koppelen. De lease op de virtuele harde schijf die het aan de virtuele machine koppelt moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U kunt de bestaande virtuele harde schijf koppelen aan deze VM voor probleemoplossing om te bladeren en de inhoud van de schijf bewerken. Dit proces kunt u Corrigeer eventuele fouten in de configuratie of extra toepassing of Systeemlogboekbestanden, bijvoorbeeld kunnen beoordelen. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

Koppel de bestaande virtuele harde schijf met [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk#az_vm_unmanaged_disk_attach). Wanneer u de bestaande virtuele harde schijf koppelt, geeft u de URI naar de schijf hebt verkregen in de voorgaande `az vm show` opdracht. Het volgende voorbeeld wordt een bestaande virtuele harde schijf gekoppeld aan de VM voor probleemoplossing met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a>De gekoppelde gegevensschijf koppelen

> [!NOTE]
> De volgende voorbeelden worden de stappen die nodig zijn op een Ubuntu-VM beschreven. Als u een andere Linux-distributie, zoals Red Hat Enterprise Linux of SUSE, de locatie van het logboekbestand en `mount` opdrachten mogelijk iets anders. Raadpleeg de documentatie voor uw specifieke distributie voor de benodigde wijzigingen in opdrachten.

1. SSH naar uw VM voor probleemoplossing met behulp van de juiste referenties. Als deze schijf de eerste gegevensschijf die is gekoppeld aan uw virtuele machine voor probleemoplossing is, de schijf waarschijnlijk is verbonden met `/dev/sdc`. Gebruik `dmseg` om gekoppelde schijven weer te geven:

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

    In het voorgaande voorbeeld, de besturingssysteemschijf is op `/dev/sda` en de tijdelijke schijf die is opgegeven voor elke virtuele machine is op `/dev/sdb`. Als u meerdere gegevensschijven had, moeten ze op `/dev/sdd`, `/dev/sde`, enzovoort.

2. Maak een map voor het koppelen van uw bestaande virtuele harde schijf. Het volgende voorbeeld wordt een map met de naam `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Als u meerdere partities op uw bestaande virtuele harde schijf hebt, koppelt u de vereiste partitie. Het volgende voorbeeld koppelt de eerste primaire partitie op `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Aanbevolen procedure is om te koppelen van gegevensschijven op virtuele machines in Azure met behulp van de universele, unieke id (UUID) van de virtuele harde schijf. Voor dit scenario voor de korte oplossen van problemen met is koppelen van de virtuele harde schijf met behulp van de UUID niet nodig. Echter bij normaal gebruik bewerken `/etc/fstab` virtuele harde schijven koppelen met behulp van de apparaatnaam in plaats van UUID kan ertoe leiden dat de virtuele machine niet kunnen worden opgestart.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Problemen oplossen op oorspronkelijke virtuele harde schijf
Met de bestaande virtuele harde schijf dat wordt gekoppeld, kunt u nu onderhouds- en stappen voor probleemoplossing naar behoefte uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Oorspronkelijke virtuele harde schijf loskoppelen
Zodra de fouten opgelost zijn, kunt u ontkoppelen en de bestaande virtuele harde schijf loskoppelen van uw virtuele machine voor probleemoplossing. U kunt de virtuele harde schijf met andere VM's niet gebruiken totdat de lease die de virtuele harde schijf koppelen aan de virtuele machine voor probleemoplossing is vrijgegeven.

1. Ontkoppel de bestaande virtuele harde schijf van de SSH-sessie aan uw VM voor probleemoplossing. Eerst uit de bovenliggende map voor uw koppelpunt wijzigen:

    ```bash
    cd /
    ```

    Nu de bestaande virtuele harde schijf ontkoppelen. Het volgende voorbeeld wordt het apparaat op `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nu de virtuele harde schijf van de virtuele machine loskoppelen. Sluit de SSH-sessie af op uw virtuele machine voor probleemoplossing. Lijst van de gekoppelde gegevensschijven aan uw VM voor probleemoplossing met [az vm unmanaged-disk list](/cli/azure/vm/unmanaged-disk#az_vm_unmanaged_disk_list). Het volgende voorbeeld worden de gegevensschijven die zijn gekoppeld aan de virtuele machine met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    Noteer de naam voor uw bestaande virtuele harde schijf. Bijvoorbeeld, de naam van een schijf met de URI van **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**. 

    De gegevensschijf van uw virtuele machine loskoppelen [az vm unmanaged-disk detach](/cli/azure/vm/unmanaged-disk#az_vm_unmanaged_disk_detach). Het volgende voorbeeld wordt losgekoppeld van de schijf met de naam `myVHD` van de virtuele machine met de naam `myVMRecovery` in de `myResourceGroup` resourcegroep:

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine maken vanaf de oorspronkelijke harde schijf
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). De daadwerkelijke JSON-sjabloon is op de volgende koppeling:

- https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json

De sjabloon implementeert een virtuele machine met behulp van de VHD-URI van de vorige opdracht. Implementeer de sjabloon met [az group deployment maken](/cli/azure/group/deployment#az_group_deployment_create). Hiermee geeft u de URI naar de oorspronkelijke VHD en geef vervolgens het type besturingssysteem, VM-grootte en de naam van de virtuele machine als volgt:

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten opnieuw inschakelen
Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, diagnostische gegevens over opstarten mogelijk niet automatisch ingeschakeld. Inschakelen van diagnostische gegevens over opstarten met [az vm boot-diagnostics inschakelen](/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). Het volgende voorbeeld wordt de diagnostische extensie op de virtuele machine met de naam `myDeployedVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Een beheerde schijf-VM oplossen door een nieuwe besturingssysteemschijf te koppelen
1. De betrokken beheerde schijf Windows virtuele machine stoppen.
2. [Maken van een momentopname van een beheerde schijf](../windows/snapshot-copy-managed-disk.md) van de Besturingssysteemschijf van de virtuele machine van beheerde schijf.
3. [Een beheerde schijf maken op basis van de momentopname](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [De beheerde schijf koppelen als een gegevensschijf van de virtuele machine](../windows/attach-disk-ps.md).
5. [Wijzigen van de gegevensschijf van stap 4 in besturingssysteemschijf](../windows/os-disk-swap.md).

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, Zie [oplossen SSH-verbindingen met een Azure-VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zie voor problemen met toegang tot toepassingen die worden uitgevoerd op de virtuele machine, [verbindingsproblemen van toepassing op een Linux-VM oplossen](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
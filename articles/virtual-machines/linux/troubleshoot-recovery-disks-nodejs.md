---
title: Gebruik een op Linux VM met de Azure CLI 1.0 probleemoplossing | Microsoft Docs
description: Meer informatie over het oplossen van problemen van Linux VM door verbinding te maken van de besturingssysteemschijf voor een herstel-VM met behulp van de Azure CLI 1.0
services: virtual-machines-linux
documentationCenter: ''
authors: iainfoulds
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 047d0041fa89fa480de0744e594b8ac4f974973a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915198"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-cli-10"></a>Problemen oplossen van een Linux-VM met de OS-schijf koppelen aan een herstel-VM met behulp van de Azure CLI 1.0
Als uw virtuele Linux-machine (VM) een opstart- of -fout optreedt, moet u wellicht de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab` die verhindert dat de virtuele machine kunnen opstarten is. Dit artikel wordt uitgelegd hoe u met de Azure CLI 1.0 verbinding maken met de virtuele harde schijf aan een andere Linux VM eventuele fouten te corrigeren en vervolgens opnieuw maken van de oorspronkelijke VM.


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#recovery-process-overview) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine zonder problemen, om de virtuele harde schijven te houden.
2. Koppelen en koppel de virtuele harde schijf aan een andere Linux VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerken van bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine maken met de oorspronkelijke virtuele harde schijf.

Zorg ervoor dat u hebt [de nieuwste Azure CLI 1.0](../../cli-install-nodejs.md) geïnstalleerd en geregistreerd in en gebruikt de modus Resource Manager:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u parameternamen vervangen door uw eigen waarden. De namen van de voorbeeld-parameter `myResourceGroup`, `mystorageaccount`, en `myVM`.


## <a name="determine-boot-issues"></a>Opstartproblemen bepalen
Bekijk de uitvoer seriële om te bepalen waarom de virtuele machine kan niet correct worden opgestart. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab`, of de onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

Het volgende voorbeeld wordt de uitvoer van de seriële van de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

Bekijk de uitvoer van de seriële om te bepalen waarom de virtuele machine kan niet worden opgestart. Als de uitvoer van de seriële is niet een aanwijzing biedt, moet u mogelijk Raadpleeg logboekbestanden in `/var/log` zodra u de virtuele harde schijf is verbonden met een VM voor het oplossen van problemen.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijf details weergeven
Voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt, moet u de naam van de virtuele harde schijf (VHD). 

Het volgende voorbeeld wordt de informatie voor de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

Zoek naar `Vhd URI` in de uitvoer van de voorgaande opdracht. Het volgende voorbeeld ziet u uitvoer afgekapt de `Vhd URI` op de laatste regel:

```azurecli
info:    Executing command vm show
+ Looking up the VM "myVM"
+ Looking up the NIC "myNic"
+ Looking up the public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf zijn gewoon metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de OS-schijf koppelen aan een virtuele machine, zelfs wanneer die VM een status gestopt en de toewijzing ongedaan is gemaakt heeft.

De eerste stap voor het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine wordt verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine kunt u de fouten oplossen.

Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` uit de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

Wacht totdat de virtuele machine verwijderen voordat u de virtuele harde schijf aan een andere virtuele machine koppelen is voltooid. De lease op de virtuele harde schijf waarin deze zijn gekoppeld aan de virtuele machine moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelen de bestaande virtuele harde schijf aan deze VM voor probleemoplossing om te bladeren en de inhoud van de schijf bewerken. Dit proces kunt u eventuele configuratiefouten te corrigeren of extra toepassing of een systeem-logboekbestanden, bijvoorbeeld controleren. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

Wanneer u de bestaande virtuele harde schijf koppelt, geef de URL naar de schijf die is verkregen in de voorgaande `azure vm show` opdracht. Het volgende voorbeeld wordt een bestaande virtuele harde schijf voor het oplossen van problemen met de naam VM `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
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

2. Nu loskoppelen van de virtuele harde schijf van de virtuele machine. De SSH-sessie voor de probleemoplossing VM afsluiten. In de Azure CLI, moet u eerst de bijgesloten gegevensschijven voor uw VM voor het oplossen van problemen weergeven. Het volgende voorbeeld worden de gegevensschijven gekoppeld aan de virtuele machine met de naam `myVMRecovery` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    Opmerking de `Lun` waarde voor de bestaande virtuele harde schijf. De uitvoer van de volgende voorbeeld ziet u de bestaande virtuele schijf voor LUN 0 is gekoppeld:

    ```azurecli
    info:    Executing command vm disk list
    + Looking up the VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    De gegevensschijf loskoppelen van uw virtuele machine met behulp van de toepasselijke `Lun` waarde:

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine van de oorspronkelijke harde schijf maken
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). De werkelijke JSON-sjabloon is op de volgende koppeling:

- https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json

De sjabloon implementeert een virtuele machine in een bestaand virtueel netwerk met behulp van de VHD-URL van de vorige opdracht. Het volgende voorbeeld implementeert u de sjabloon in de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

Beantwoord de aanwijzingen voor de sjabloon zoals VM-naam (`myDeployedVM` in het volgende voorbeeld), type besturingssysteem (`Linux`), en VM-grootte (`Standard_DS1_v2`). De `osDiskVhdUri` is hetzelfde als eerder gebruikt als de bestaande virtuele harde schijf koppelen aan de VM voor het oplossen van problemen. Een voorbeeld van de uitvoer van de opdracht en de prompts is als volgt:

```azurecli
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment to complete
+
```


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten weer inschakelen

Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, kan boot diagnostics niet automatisch worden ingeschakeld. Het volgende voorbeeld wordt de diagnostische uitbreiding op de virtuele machine met de naam `myDeployedVM` in de resourcegroep met de naam `myResourceGroup`:

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, raadpleegt u [oplossen SSH-verbindingen met een Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zie voor problemen met de toegang tot toepassingen die worden uitgevoerd op de virtuele machine [oplossen verbindingsproblemen van toepassing op een Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
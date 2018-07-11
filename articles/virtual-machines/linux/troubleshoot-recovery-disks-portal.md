---
title: Gebruik van een Linux VM in de Azure portal voor probleemoplossing | Microsoft Docs
description: Informatie over het oplossen van problemen met Linux virtuele machine door verbinding te maken van de besturingssysteemschijf aan een virtuele machine met behulp van de Azure-portal voor herstel
services: virtual-machines-linux
documentationCenter: ''
authors: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: cynthn
ms.openlocfilehash: efa001a24be3fb646a2a10afe72cb9b4ebfbf836
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932005"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Een Linux-VM oplossen door de besturingssysteemschijf koppelen aan een virtuele machine met behulp van de Azure-portal voor herstel
Als uw Linux-machine (VM) een fout opstart- of schijffout optreedt, moet u mogelijk de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab` dat voorkomt dat de virtuele machine wordt het opstarten. Dit artikel wordt uitgelegd hoe u verbinding maken met de virtuele harde schijf met een andere Linux-VM op eventuele fouten te corrigeren en vervolgens de oorspronkelijke virtuele machine opnieuw te maken met de Azure-portal.

## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine problemen worden aangetroffen, de virtuele harde schijven te houden.
2. Toevoegen en koppelen van de virtuele harde schijf met een andere Linux-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerk bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine met behulp van de oorspronkelijke virtuele harde schijf maken.

Voor de virtuele machine die gebruikmaakt van beheerde schijf, Zie [een beheerde schijf-VM oplossen door het koppelen van een nieuwe schijf met besturingssysteem](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).

## <a name="determine-boot-issues"></a>Opstartproblemen met bepalen
Controleer de diagnostische gegevens over opstarten en de VM-schermopname om te bepalen waarom de virtuele machine niet staat zijn om correct is. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab`, of een onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

Selecteer de virtuele machine in de portal en schuif vervolgens omlaag naar de **ondersteuning + probleemoplossing** sectie. Klik op **diagnostische gegevens over opstarten** om de consoleberichten gestreamd vanaf uw virtuele machine weer te geven. Bekijk de logboeken van de console om te zien als kunt u bepalen waarom de virtuele machine een probleem is gestuit. Het volgende voorbeeld ziet u dat een virtuele machine in de onderhoudsmodus bevinden waarvoor handmatige tussenkomst van de vastgelopen:

![Diagnostische gegevens over opstarten console weergeven VM Logboeken](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

U kunt ook klikken op **schermafbeelding** aan de bovenkant van het logboek van de diagnostische gegevens over opstarten voor het downloaden van een opname van de VM-schermopname.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijfdetails weergeven
Voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt, moet u de naam van de virtuele harde schijf (VHD). 

Selecteer de resourcegroep van de portal en selecteer vervolgens uw opslagaccount. Klik op **Blobs**, zoals in het volgende voorbeeld:

![Selecteer de storage-blobs](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

Normaal gesproken hebt u een container met de naam **VHD's** die de virtuele harde schijven worden opgeslagen. Selecteer de container om een lijst met virtuele harde schijven weer te geven. Houd rekening met de naam van uw VHD (het voorvoegsel is gewoonlijk de naam van uw virtuele machine):

![Identificeren van de VHD in storage-container](./media/troubleshoot-recovery-disks-portal/storage-container.png)

Selecteer uw bestaande virtuele harde schijf in de lijst en kopieer de URL voor gebruik in de volgende stappen uit:

![Bestaande virtuele harde schijf-URL kopiëren](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf bestaat uit metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer die zijn gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de besturingssysteemschijf koppelen aan een virtuele machine, zelfs wanneer die virtuele machine in een status gestopt en toewijzing ongedaan gemaakt.

De eerste stap bij het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine is verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine oplossen van problemen en los de fouten.

Selecteer de virtuele machine in de portal en klik vervolgens op **verwijderen**:

![VM boot diagnostics schermafbeelding fout bij het opstarten](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

Wacht totdat de virtuele machine verwijderd is voordat u de virtuele harde schijf aan een andere virtuele machine koppelen. De lease op de virtuele harde schijf die het aan de virtuele machine koppelt moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelen de bestaande virtuele harde schijf aan deze VM voor probleemoplossing kunnen bladeren en bewerken van de inhoud van de schijf. Dit proces kunt u Corrigeer eventuele fouten in de configuratie of extra toepassing of Systeemlogboekbestanden, bijvoorbeeld kunnen beoordelen. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

1. Selecteer de resourcegroep van de portal en selecteer vervolgens uw virtuele machine voor probleemoplossing. Selecteer **schijven** en klik vervolgens op **koppelen aan bestaande**:

    ![Het koppelen van bestaande schijf in de portal](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. Klik om uw bestaande virtuele harde schijf te selecteren op **VHD-bestand**:

    ![Zoeken naar bestaande VHD](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. Selecteer uw opslagaccount en container en klik vervolgens op uw bestaande VHD. Klik op de **Selecteer** knop om uw keuze te bevestigen:

    ![Uw bestaande VHD selecteren](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. Met de VH geselecteerd, klikt u op **OK** aan de bestaande virtuele harde schijf koppelen:

    ![Controleer of de bestaande virtuele harde schijf koppelen](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. Na enkele seconden, de **schijven** deelvenster voor uw virtuele machine een lijst met uw bestaande virtuele harde schijf als een gegevensschijf verbonden:

    ![Bestaande virtuele harde schijf gekoppeld als een gegevensschijf](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a>De gekoppelde gegevensschijf koppelen

> [!NOTE]
> De volgende voorbeelden worden de stappen die nodig zijn op een Ubuntu-VM beschreven. Als u een andere Linux-distributie, zoals Red Hat Enterprise Linux of SUSE, de locatie van het logboekbestand en `mount` opdrachten mogelijk iets anders. Raadpleeg de documentatie voor uw specifieke distributie voor de benodigde wijzigingen in opdrachten.

1. SSH naar uw VM voor probleemoplossing met behulp van de juiste referenties. Als deze schijf de eerste gegevensschijf die is gekoppeld aan uw virtuele machine voor probleemoplossing is, waarschijnlijk is verbonden met `/dev/sdc`. Gebruik `dmseg` om gekoppelde schijven weer te geven:

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
Zodra de fouten opgelost zijn, loskoppelen van de bestaande virtuele harde schijf van uw VM voor probleemoplossing. U kunt de virtuele harde schijf met andere VM's niet gebruiken totdat de lease die de virtuele harde schijf koppelen aan de virtuele machine voor probleemoplossing is vrijgegeven.

1. Ontkoppel de bestaande virtuele harde schijf van de SSH-sessie aan uw VM voor probleemoplossing. Eerst uit de bovenliggende map voor uw koppelpunt wijzigen:

    ```bash
    cd /
    ```

    Nu de bestaande virtuele harde schijf ontkoppelen. Het volgende voorbeeld wordt het apparaat op `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nu de virtuele harde schijf van de virtuele machine loskoppelen. Selecteer de virtuele machine in de portal en klik op **schijven**. Selecteer uw bestaande virtuele harde schijf en klik vervolgens op **loskoppelen**:

    ![Bestaande virtuele harde schijf loskoppelen](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Wacht totdat de virtuele machine heeft de gegevensschijf losgekoppeld voordat u doorgaat.

## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine maken vanaf de oorspronkelijke harde schijf
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). De sjabloon implementeert een virtuele machine in een bestaand virtueel netwerk, met behulp van de VHD-URL van de vorige opdracht. Klik op de **implementeren in Azure** knop als volgt:

![Virtuele machine implementeren vanuit GitHub-sjabloon](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

De sjabloon is geladen in de Azure-portal voor implementatie. Geef de naam op voor uw nieuwe virtuele machine en de bestaande Azure-resources en plak de URL van uw bestaande virtuele harde schijf. Om te beginnen met de implementatie, klikt u op **aankoop**:

![Virtuele machine implementeren met sjabloon](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten opnieuw inschakelen
Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, diagnostische gegevens over opstarten mogelijk niet automatisch ingeschakeld. Controleer de status van diagnostische gegevens over opstarten en schakel indien nodig, selecteert u uw virtuele machine in de portal. Onder **bewaking**, klikt u op **diagnostische instellingen**. Zorg ervoor dat de status is **op**, en het selectievakje naast **diagnostische gegevens over opstarten** is geselecteerd. Als u geen wijzigingen aanbrengt, klikt u op **opslaan**:

![De instellingen voor diagnostische gegevens over opstarten bijwerken](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Een beheerde schijf-VM oplossen door een nieuwe besturingssysteemschijf te koppelen
1. De betrokken beheerde schijf Windows virtuele machine stoppen.
2. [Maken van een momentopname van een beheerde schijf](../windows/snapshot-copy-managed-disk.md) van de Besturingssysteemschijf van de virtuele machine van beheerde schijf.
3. [Een beheerde schijf maken op basis van de momentopname](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [De beheerde schijf koppelen als een gegevensschijf van de virtuele machine](../windows/attach-disk-ps.md).
5. [Wijzigen van de gegevensschijf van stap 4 in besturingssysteemschijf](../windows/os-disk-swap.md).

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, Zie [oplossen SSH-verbindingen met een Azure-VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zie voor problemen met toegang tot toepassingen die worden uitgevoerd op de virtuele machine, [verbindingsproblemen van toepassing op een Linux-VM oplossen](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

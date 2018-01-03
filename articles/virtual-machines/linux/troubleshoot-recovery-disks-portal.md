---
title: Gebruik een op Linux VM probleemoplossing in de Azure portal | Microsoft Docs
description: Meer informatie over het oplossen van problemen voor Linux-virtuele machine door verbinding te maken van de besturingssysteemschijf voor een herstel-VM met de Azure portal
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: c96ff625c3e83f6fc9057f1163c877e8e0aed5e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Problemen oplossen van een Linux-VM met de OS-schijf koppelen aan een herstel-VM met de Azure portal
Als uw virtuele Linux-machine (VM) een opstart- of -fout optreedt, moet u wellicht de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab` die verhindert dat de virtuele machine kunnen opstarten is. Dit artikel wordt uitgelegd hoe u met de Azure-portal verbinding maken met de virtuele harde schijf aan een andere Linux VM eventuele fouten te corrigeren en vervolgens opnieuw maken van de oorspronkelijke VM.

## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine zonder problemen, om de virtuele harde schijven te houden.
2. Koppelen en koppel de virtuele harde schijf aan een andere Linux VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerken van bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine maken met de oorspronkelijke virtuele harde schijf.


## <a name="determine-boot-issues"></a>Opstartproblemen bepalen
Raadpleeg de diagnostische gegevens over opstarten en schermopname van de VM om te bepalen waarom de virtuele machine kan niet correct worden opgestart. Een veelvoorkomend voorbeeld is een ongeldige waarde in `/etc/fstab`, of een onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

Selecteer uw virtuele machine in de portal en schuif vervolgens omlaag naar de **ondersteuning + probleemoplossing** sectie. Klik op **opstarten diagnostics** om de consoleberichten gestreamd vanaf uw virtuele machine weer te geven. Bekijk de logboeken van de console om te zien als kunt u bepalen waarom de virtuele machine wordt uitgevoerd als een probleem. Het volgende voorbeeld ziet u dat een virtuele machine is vastgelopen in de onderhoudsmodus die handmatige interactie vereist:

![Diagnostische gegevens over opstarten console weer te geven VM Logboeken](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

U kunt ook klikken op **schermafbeelding** aan de bovenkant van het logboek boot diagnostics voor het downloaden van een vastleggen van de VM-schermafbeelding.


## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijf details weergeven
Voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt, moet u de naam van de virtuele harde schijf (VHD). 

Selecteer uw resourcegroep via de portal en selecteer vervolgens uw storage-account. Klik op **Blobs**, zoals in het volgende voorbeeld:

![Selecteer de storage-blobs](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

Hebben doorgaans een container met de naam **VHD's** die de virtuele harde schijven worden opgeslagen. Selecteer de container voor een lijst van virtuele harde schijven. Houd rekening met de naam van uw VHD (het voorvoegsel is doorgaans de naam van uw virtuele machine):

![Identificeren van de VHD in de storage-container](./media/troubleshoot-recovery-disks-portal/storage-container.png)

Selecteer uw bestaande virtuele harde schijf in de lijst en kopieer de URL voor gebruik in de volgende stappen:

![Kopieer de URL van bestaande virtuele harde schijf](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf zijn gewoon metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de OS-schijf koppelen aan een virtuele machine, zelfs wanneer die VM een status gestopt en de toewijzing ongedaan is gemaakt heeft.

De eerste stap voor het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine wordt verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine kunt u de fouten oplossen.

Selecteer uw virtuele machine in de portal en klik vervolgens op **verwijderen**:

![VM boot diagnostics schermafbeelding opstartfout](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

Wacht totdat de virtuele machine verwijderen voordat u de virtuele harde schijf aan een andere virtuele machine koppelen is voltooid. De lease op de virtuele harde schijf waarin deze zijn gekoppeld aan de virtuele machine moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelen de bestaande virtuele harde schijf aan deze VM voor probleemoplossing kunnen bladeren en de inhoud van de schijf bewerken. Dit proces kunt u eventuele configuratiefouten te corrigeren of extra toepassing of een systeem-logboekbestanden, bijvoorbeeld controleren. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

1. Selecteer uw resourcegroep via de portal en selecteer vervolgens uw VM voor het oplossen van problemen. Selecteer **schijven** en klik vervolgens op **Attach bestaande**:

    ![Het koppelen van bestaande schijf in de portal](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. Klik om uw bestaande virtuele harde schijf te selecteren op **VHD-bestand**:

    ![Zoeken naar bestaande VHD](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. Uw opslagaccount en container selecteren en klik vervolgens op uw bestaande VHD. Klik op de **Selecteer** knop om te bevestigen van uw keuze:

    ![Uw bestaande VHD selecteren](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. Met de VHD die is geselecteerd, klikt u op **OK** koppelen van de bestaande virtuele harde schijf:

    ![Bevestig de bestaande virtuele harde schijf koppelen](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. Na enkele seconden, de **schijven** deelvenster voor uw virtuele machine een lijst met uw bestaande virtuele harde schijf als een gegevensschijf verbonden:

    ![Bestaande virtuele harde schijf gekoppeld als een gegevensschijf](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a>Koppel de gekoppelde gegevensschijf

> [!NOTE]
> De volgende voorbeelden worden de stappen die nodig is op een VM Ubuntu in detail beschreven. Als u gebruikmaakt van een andere Linux distro zoals Red Hat Enterprise Linux of SUSE, de locaties van het logboekbestand en `mount` opdrachten mogelijk enigszins anders. Raadpleeg de documentatie voor uw specifieke distro voor de benodigde wijzigingen in de opdrachten.

1. SSH met uw probleemoplossing virtuele machine met de juiste referenties. Als u deze schijf is de eerste gegevensschijf gekoppeld aan uw VM voor het oplossen van problemen, waarschijnlijk is verbonden met `/dev/sdc`. Gebruik `dmseg` voor een lijst met gekoppelde schijven:

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
Zodra de fouten opgelost zijn, loskoppelen de bestaande virtuele harde schijf van uw VM voor het oplossen van problemen. U kunt de virtuele harde schijf niet gebruiken met eventuele andere virtuele machine totdat de lease koppelen van de virtuele harde schijf voor het oplossen van problemen VM is uitgebracht.

1. Ontkoppel de bestaande virtuele harde schijf van de SSH-sessie voor uw VM voor het oplossen van problemen. Eerst buiten de bovenliggende map voor uw koppelpunt wijzigen:

    ```bash
    cd /
    ```

    Nu Ontkoppel de bestaande virtuele harde schijf. Het volgende voorbeeld het apparaat op ontkoppelt `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nu loskoppelen van de virtuele harde schijf van de virtuele machine. Selecteer uw virtuele machine in de portal en klik op **schijven**. Selecteer uw bestaande virtuele harde schijf en klik vervolgens op **Detach**:

    ![Loskoppelen van de bestaande virtuele harde schijf](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Wacht totdat de virtuele machine heeft de de gegevensschijf is losgekoppeld voordat u doorgaat.

## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine van de oorspronkelijke harde schijf maken
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). De sjabloon implementeert een virtuele machine in een bestaand virtueel netwerk met behulp van de VHD-URL van de vorige opdracht. Klik op de **implementeren in Azure** knop als volgt:

![Implementeer de virtuele machine van de sjabloon vanuit GitHub](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

De sjabloon is geladen in de Azure-portal voor implementatie. Geef de naam op voor uw nieuwe virtuele machine en de bestaande Azure-resources en plak de URL naar uw bestaande virtuele harde schijf. Om te beginnen met de implementatie, klikt u op **aankoop**:

![VM van sjabloon implementeren](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten weer inschakelen
Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, kan boot diagnostics niet automatisch worden ingeschakeld. Controleer de status van diagnostische gegevens over opstarten en schakel indien nodig, selecteert u de virtuele machine in de portal. Onder **bewaking**, klikt u op **diagnostische instellingen**. Zorg ervoor dat de status **op**, en het selectievakje naast **opstarten diagnostics** is geselecteerd. Als u geen wijzigingen aanbrengen, klikt u op **opslaan**:

![Bijwerken van diagnostische gegevens van de instellingen voor opstarten](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, raadpleegt u [oplossen SSH-verbindingen met een Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zie voor problemen met de toegang tot toepassingen die worden uitgevoerd op de virtuele machine [oplossen verbindingsproblemen van toepassing op een Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

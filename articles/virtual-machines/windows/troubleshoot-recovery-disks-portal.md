---
title: Gebruik een Windows VM probleemoplossing in de Azure portal | Microsoft Docs
description: Meer informatie over het oplossen van problemen voor Windows-virtuele machine in Azure door verbinding te maken van de besturingssysteemschijf voor een herstel-VM met de Azure portal
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/07/2018
ms.author: genli
ms.openlocfilehash: 818e4ca5c4985d1740c477bf4a5aa198e64b506d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Een virtuele machine van Windows oplossen door de OS-schijf koppelen aan een herstel-VM met de Azure portal
Als uw Windows-machine (VM) in Azure een opstart- of -fout optreedt, moet u wellicht de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een mislukte toepassingsupdate die verhindert dat de virtuele machine kunnen opstarten is. Dit artikel wordt uitgelegd hoe u met de Azure-portal verbinding van de virtuele harde schijf met een andere virtuele machine van Windows op eventuele fouten te corrigeren en vervolgens opnieuw maken van de oorspronkelijke VM.

## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine zonder problemen, om de virtuele harde schijven te houden.
2. Koppelen en koppel de virtuele harde schijf aan een andere Windows-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerken van bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine maken met de oorspronkelijke virtuele harde schijf.


## <a name="determine-boot-issues"></a>Opstartproblemen bepalen
Om te bepalen waarom de virtuele machine kan niet correct worden opgestart, controleert u de diagnostische gegevens over opstarten schermopname VM. Een veelvoorkomend voorbeeld zou worden het bijwerken van een mislukte toepassing of een onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

Selecteer uw virtuele machine in de portal en schuif vervolgens omlaag naar de **ondersteuning + probleemoplossing** sectie. Klik op **opstarten diagnostics** om weer te geven in de schermafbeelding. Houd er rekening mee specifieke foutberichten of foutcodes om te bepalen waarom de virtuele machine wordt uitgevoerd als een probleem. Het volgende voorbeeld ziet u een virtuele machine die wachten op het stoppen van services:

![Diagnostische gegevens over opstarten console weer te geven VM Logboeken](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

U kunt ook klikken op **schermafbeelding** voor het downloaden van een vastleggen van de VM-schermafbeelding.


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

1. Open een extern bureaublad-verbinding met uw virtuele machine. Selecteer uw virtuele machine in de portal en klik op **Connect**. Download en open het RDP-verbinding-bestand. Voer uw referenties aan te melden bij uw virtuele machine als volgt:

    ![Aanmelden bij uw virtuele machine via Extern bureaublad](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. Open **Serverbeheer**, selecteer daarna **File and Storage Services**. 

    ![Selecteer bestands- en opslagservices in Serverbeheer](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. De gegevensschijf wordt automatisch gedetecteerd en gekoppeld. Selecteer een overzicht van de schijven die zijn verbonden **schijven**. U kunt uw gegevensschijf selecteren voor volume-informatie, inclusief de stationsletter weergeven. Het volgende voorbeeld ziet u de gegevensschijf gekoppeld en met behulp van **F:**:

    ![Schijf is gekoppeld aan- en volumegegevens in Serverbeheer](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Los problemen op de oorspronkelijke virtuele harde schijf
Met de bestaande virtuele harde schijf dat wordt gekoppeld, kunt u nu onderhoud en stappen voor probleemoplossing naar behoefte uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Ontkoppel en loskoppelen van de oorspronkelijke virtuele harde schijf
Zodra de fouten opgelost zijn, loskoppelen de bestaande virtuele harde schijf van uw VM voor het oplossen van problemen. U kunt de virtuele harde schijf niet gebruiken met eventuele andere virtuele machine totdat de lease koppelen van de virtuele harde schijf voor het oplossen van problemen VM is uitgebracht.

1. Open in de RDP-sessie met uw virtuele machine, **Serverbeheer**, selecteer daarna **File and Storage Services**:

    ![Selecteer bestands- en opslagservices in Serverbeheer](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. Selecteer **schijven** en selecteer vervolgens de gegevensschijf van uw. Met de rechtermuisknop op de gegevensschijf van uw en selecteer **offline zetten**:

    ![De gegevensschijf ingesteld als offline in Serverbeheer](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. Nu loskoppelen van de virtuele harde schijf van de virtuele machine. Selecteer uw virtuele machine in de Azure portal en klik op **schijven**. Selecteer uw bestaande virtuele harde schijf en klik vervolgens op **Detach**:

    ![Loskoppelen van de bestaande virtuele harde schijf](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Wacht totdat de virtuele machine heeft de de gegevensschijf is losgekoppeld voordat u doorgaat.

## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine van de oorspronkelijke harde schijf maken
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). De sjabloon implementeert een virtuele machine in een bestaand virtueel netwerk met behulp van de VHD-URL van de vorige opdracht. Klik op de **implementeren in Azure** knop als volgt:

![Implementeer de virtuele machine van de sjabloon vanuit Github](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

De sjabloon is geladen in de Azure-portal voor implementatie. Geef de naam op voor uw nieuwe virtuele machine en de bestaande Azure-resources en plak de URL naar uw bestaande virtuele harde schijf. Om te beginnen met de implementatie, klikt u op **aankoop**:

![VM van sjabloon implementeren](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten weer inschakelen
Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, kan boot diagnostics niet automatisch worden ingeschakeld. Controleer de status van diagnostische gegevens over opstarten en schakel indien nodig, selecteert u de virtuele machine in de portal. Onder **bewaking**, klikt u op **diagnostische instellingen**. Zorg ervoor dat de status **op**, en het selectievakje naast **opstarten diagnostics** is geselecteerd. Als u geen wijzigingen aanbrengen, klikt u op **opslaan**:

![Bijwerken van diagnostische gegevens van de instellingen voor opstarten](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, raadpleegt u [problemen met RDP-verbindingen naar een Azure-virtuele machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Zie voor problemen met de toegang tot toepassingen die worden uitgevoerd op de virtuele machine [oplossen verbindingsproblemen van toepassing op een Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

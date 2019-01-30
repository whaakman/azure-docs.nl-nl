---
title: Gebruik een Windows VM in de Azure portal voor probleemoplossing | Microsoft Docs
description: Informatie over het oplossen van problemen met Windows virtuele machine in Azure door verbinding te maken van de besturingssysteemschijf aan een virtuele machine met behulp van de Azure-portal voor herstel
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/013/2018
ms.author: genli
ms.openlocfilehash: 2c5fac377dfab4b4c85991dcb8f4e15f4e3cb61a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225926"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine met behulp van de Azure-portal voor herstel
Als uw Windows virtuele machine (VM) in Azure een opstart- of schijffout-fout optreedt, moet u mogelijk de stappen voor probleemoplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voorbeeld is een mislukte toepassingsupdate die verhindert de virtuele machine dat wordt het opstarten. Dit artikel wordt uitgelegd hoe u verbinding maken met de virtuele harde schijf naar een andere Windows-virtuele machine om eventuele fouten te corrigeren en vervolgens de oorspronkelijke virtuele machine opnieuw te maken met Azure portal.

## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de virtuele machine problemen worden aangetroffen, de virtuele harde schijven te houden.
2. Toevoegen en koppelen van de virtuele harde schijf met een andere Windows-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerk bestanden of alle hulpmiddelen voor het oplossen van problemen op de oorspronkelijke virtuele harde schijf worden uitgevoerd.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Een virtuele machine met behulp van de oorspronkelijke virtuele harde schijf maken.

Voor de virtuele machine die beheerde schijf gebruikt, kunnen we nu Azure PowerShell gebruiken voor het wijzigen van de besturingssysteemschijf voor een virtuele machine. We hoeft niet meer te verwijderen en opnieuw maken van de virtuele machine. Zie voor meer informatie, [een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine met behulp van Azure PowerShell voor herstel](troubleshoot-recovery-disks-windows.md).

## <a name="determine-boot-issues"></a>Opstartproblemen met bepalen
Om te bepalen waarom de virtuele machine kan niet correct worden opgestart, controleert u de diagnostische gegevens over opstarten VM-schermopname. Een veelvoorkomend voorbeeld zou worden van een mislukte toepassingsupdate, of een onderliggende virtuele harde schijf wordt verwijderd of verplaatst.

Selecteer de virtuele machine in de portal en schuif vervolgens omlaag naar de **ondersteuning + probleemoplossing** sectie. Klik op **diagnostische gegevens over opstarten** om weer te geven in de schermafbeelding. Houd er rekening mee om te bepalen waarom de virtuele machine een probleem is gestuit specifieke foutberichten of foutcodes. 

![Diagnostische gegevens over opstarten console weergeven VM Logboeken](./media/troubleshoot-recovery-disks-portal-windows/screenshot-error.png)

U kunt ook klikken op **schermopname downloaden** voor het downloaden van een opname van de VM-schermopname.

## <a name="view-existing-virtual-hard-disk-details"></a>Bestaande virtuele harde schijfdetails weergeven
Voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt, moet u de naam van de virtuele harde schijf (VHD). 

Selecteer de resourcegroep van de portal en selecteer vervolgens uw opslagaccount. Klik op **Blobs**, zoals in het volgende voorbeeld:

![Selecteer de storage-blobs](./media/troubleshoot-recovery-disks-portal-windows/storage-account-overview.png)

Normaal gesproken hebt u een container met de naam **VHD's** die de virtuele harde schijven worden opgeslagen. Selecteer de container om een lijst met virtuele harde schijven weer te geven. Houd rekening met de naam van uw VHD (het voorvoegsel is gewoonlijk de naam van uw virtuele machine):

![Identificeren van de VHD in storage-container](./media/troubleshoot-recovery-disks-portal-windows/storage-container.png)

Selecteer uw bestaande virtuele harde schijf in de lijst en kopieer de URL voor gebruik in de volgende stappen uit:

![Bestaande virtuele harde schijf-URL kopiëren](./media/troubleshoot-recovery-disks-portal-windows/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Bestaande virtuele machine verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is waar het besturingssysteem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf bestaat uit metagegevens die definieert de grootte of locatie en verwijst naar resources, zoals een virtuele harde schijf of virtuele netwerkinterfacekaart (NIC). Elke virtuele harde schijf heeft een lease toegewezen wanneer die zijn gekoppeld aan een virtuele machine. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de besturingssysteemschijf koppelen aan een virtuele machine, zelfs wanneer die virtuele machine in een status gestopt en toewijzing ongedaan gemaakt.

De eerste stap bij het herstellen van uw virtuele machine is de VM-resource zelf verwijderen. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de virtuele machine is verwijderd, kunt u de virtuele harde schijf koppelen aan een andere virtuele machine oplossen van problemen en los de fouten.

Selecteer de virtuele machine in de portal en klik vervolgens op **verwijderen**:

![VM boot diagnostics schermafbeelding fout bij het opstarten](./media/troubleshoot-recovery-disks-portal-windows/stop-delete-vm.png)

Wacht totdat de virtuele machine verwijderd is voordat u de virtuele harde schijf aan een andere virtuele machine koppelen. De lease op de virtuele harde schijf die het aan de virtuele machine koppelt moet worden vrijgegeven voordat u de virtuele harde schijf aan een andere virtuele machine koppelen kunt.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelen de bestaande virtuele harde schijf aan deze VM voor probleemoplossing kunnen bladeren en bewerken van de inhoud van de schijf. Dit proces kunt u Corrigeer eventuele fouten in de configuratie of extra toepassing of Systeemlogboekbestanden, bijvoorbeeld kunnen beoordelen. Kies of maak een andere virtuele machine moet worden gebruikt voor het oplossen van problemen.

1. Selecteer de resourcegroep van de portal en selecteer vervolgens uw virtuele machine voor probleemoplossing. Selecteer **schijven** en klik vervolgens op **koppelen aan bestaande**:

    ![Het koppelen van bestaande schijf in de portal](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Klik om uw bestaande virtuele harde schijf te selecteren op **VHD-bestand**:

    ![Zoeken naar bestaande VHD](./media/troubleshoot-recovery-disks-portal-windows/select-vhd-location.png)

3. Selecteer uw opslagaccount en container en klik vervolgens op uw bestaande VHD. Klik op de **Selecteer** knop om uw keuze te bevestigen:

    ![Uw bestaande VHD selecteren](./media/troubleshoot-recovery-disks-portal-windows/select-vhd.png)

4. Met de VH geselecteerd, klikt u op **OK** aan de bestaande virtuele harde schijf koppelen:

    ![Controleer of de bestaande virtuele harde schijf koppelen](./media/troubleshoot-recovery-disks-portal-windows/attach-disk-confirm.png)

5. Na enkele seconden, de **schijven** deelvenster voor uw virtuele machine een lijst met uw bestaande virtuele harde schijf als een gegevensschijf verbonden:

    ![Bestaande virtuele harde schijf gekoppeld als een gegevensschijf](./media/troubleshoot-recovery-disks-portal-windows/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a>De gekoppelde gegevensschijf koppelen

1. Open een extern bureaublad-verbinding met uw virtuele machine. Selecteer de virtuele machine in de portal en klik op **Connect**. Download en open het bestand RDP-verbinding. Voer uw referenties aan te melden met uw virtuele machine als volgt:

    ![Meld u aan uw virtuele machine via Extern bureaublad](./media/troubleshoot-recovery-disks-portal-windows/open-remote-desktop.png)

2. Open **Serverbeheer**en selecteer vervolgens **File and Storage Services**. 

    ![Schakel Bestands- en opslagservices in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. De gegevensschijf is automatisch gedetecteerd en die is gekoppeld. Een lijst van de gekoppelde schijven wilt bekijken, selecteert u **schijven**. U kunt de gegevensschijf voor volume-informatie, inclusief de stationsletter. Het volgende voorbeeld ziet u de gegevensschijf die is gekoppeld en het gebruik van **F:**:

    ![Schijf die is gekoppeld en volumegegevens in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Problemen oplossen op oorspronkelijke virtuele harde schijf
Met de bestaande virtuele harde schijf dat wordt gekoppeld, kunt u nu onderhouds- en stappen voor probleemoplossing naar behoefte uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Oorspronkelijke virtuele harde schijf loskoppelen
Zodra de fouten opgelost zijn, loskoppelen van de bestaande virtuele harde schijf van uw VM voor probleemoplossing. U kunt de virtuele harde schijf met andere VM's niet gebruiken totdat de lease die de virtuele harde schijf koppelen aan de virtuele machine voor probleemoplossing is vrijgegeven.

1. Open in het RDP-sessie met uw virtuele machine, **Serverbeheer**en selecteer vervolgens **File and Storage Services**:

    ![Schakel Bestands- en opslagservices in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Selecteer **schijven** en selecteer vervolgens de gegevensschijf van uw. Met de rechtermuisknop op de gegevensschijf van uw en selecteer **Offline nemen**:

    ![De gegevensschijf instellen als offline in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Nu de virtuele harde schijf van de virtuele machine loskoppelen. Selecteer de virtuele machine in Azure portal en klikt u op **schijven**. Selecteer uw bestaande virtuele harde schijf en klik vervolgens op **loskoppelen**:

    ![Bestaande virtuele harde schijf loskoppelen](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Wacht totdat de virtuele machine heeft de gegevensschijf losgekoppeld voordat u doorgaat.

## <a name="create-vm-from-original-hard-disk"></a>Virtuele machine maken vanaf de oorspronkelijke harde schijf
Gebruik voor het maken van een virtuele machine van de oorspronkelijke virtuele harde schijf [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet). De sjabloon implementeert een virtuele machine in een bestaand of nieuw virtueel netwerk, met behulp van de VHD-URL van de vorige opdracht. Klik op de **implementeren in Azure** knop als volgt:

![Virtuele machine implementeren vanuit GitHub-sjabloon](./media/troubleshoot-recovery-disks-portal-windows/deploy-template-from-github.png)

De sjabloon is geladen in de Azure-portal voor implementatie. Geef de naam op voor uw nieuwe virtuele machine en de bestaande Azure-resources en plak de URL van uw bestaande virtuele harde schijf. Om te beginnen met de implementatie, klikt u op **aankoop**:

![Virtuele machine implementeren met sjabloon](./media/troubleshoot-recovery-disks-portal-windows/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten opnieuw inschakelen
Wanneer u uw virtuele machine van de bestaande virtuele harde schijf maakt, diagnostische gegevens over opstarten mogelijk niet automatisch ingeschakeld. Controleer de status van diagnostische gegevens over opstarten en schakel indien nodig, selecteert u uw virtuele machine in de portal. Onder **bewaking**, klikt u op **diagnostische instellingen**. Zorg ervoor dat de status is **op**, en het selectievakje naast **diagnostische gegevens over opstarten** is geselecteerd. Als u geen wijzigingen aanbrengt, klikt u op **opslaan**:

![De instellingen voor diagnostische gegevens over opstarten bijwerken](./media/troubleshoot-recovery-disks-portal-windows/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, Zie [oplossen RDP-verbindingen met een Azure VM](troubleshoot-rdp-connection.md). Zie voor problemen met toegang tot toepassingen die worden uitgevoerd op de virtuele machine, [verbindingsproblemen van toepassing op een Windows-VM oplossen](troubleshoot-app-connection.md).

Zie voor meer informatie over het gebruik van Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).


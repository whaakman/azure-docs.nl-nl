---
title: Gebruik een Windows-probleemoplossings-VM in de Azure Portal | Microsoft Docs
description: Informatie over het oplossen van problemen met virtuele Windows-machines in azure door de besturingssysteem schijf te koppelen aan een herstel-VM met behulp van de Azure Portal
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/13/2018
ms.author: genli
ms.openlocfilehash: 8ab6fc75475cd99e3d803450476880175f12d2b6
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881155"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Problemen met een Windows-VM oplossen door de besturingssysteem schijf te koppelen aan een herstel-VM met behulp van de Azure Portal
Als op uw virtuele Windows-machine (VM) in azure een opstart-of schijf fout optreedt, moet u mogelijk de stappen voor probleem oplossing uitvoeren op de virtuele harde schijf zelf. Een veelvoorkomend voor beeld hiervan is een mislukte toepassings update waarmee wordt voor komen dat de virtuele machine kan worden opgestart. In dit artikel wordt beschreven hoe u met behulp van de Azure Portal de virtuele harde schijf verbindt met een andere Windows-VM om eventuele fouten op te lossen en vervolgens de oorspronkelijke VM opnieuw te maken. 

## <a name="recovery-process-overview"></a>Overzicht van het herstelproces
Het probleemoplossingsproces is als volgt:

1. Verwijder de VM die problemen ondervindt en behoud de virtuele harde schijven.
2. Koppel en koppel de virtuele harde schijf aan een andere Windows-VM voor het oplossen van problemen.
3. Maak verbinding met de VM voor probleemoplossing. Bewerk bestanden of voer hulpprogram ma's uit om problemen op de oorspronkelijke virtuele harde schijf op te lossen.
4. Koppel de virtuele harde schijf van de VM voor probleemoplossing los.
5. Maak een virtuele machine met behulp van de oorspronkelijke virtuele harde schijf.

Voor de virtuele machine die gebruikmaakt van beheerde schijven, kunnen we nu Azure PowerShell gebruiken om de besturingssysteem schijf voor een virtuele machine te wijzigen. Het is niet meer nodig om de virtuele machine te verwijderen en opnieuw te maken. Zie [problemen met een Windows-VM oplossen door de besturingssysteem schijf te koppelen aan een herstel-VM met behulp van Azure PowerShell](troubleshoot-recovery-disks-windows.md)voor meer informatie.

> [!NOTE]
> Dit artikel is niet van toepassing op de virtuele machine met een niet-beheerde schijf.

## <a name="determine-boot-issues"></a>Opstart problemen vaststellen
Als u wilt weten waarom de virtuele machine niet correct kan worden opgestart, controleert u de scherm opname van de VM voor diagnostische gegevens over opstarten. Een veelvoorkomend voor beeld is een mislukte toepassings update of een onderliggende virtuele harde schijf die wordt verwijderd of verplaatst.

Selecteer uw virtuele machine in de portal en schuif omlaag naar de sectie **ondersteuning en probleem oplossing** . Klik op **Diagnostische gegevens over opstarten** om de scherm opname weer te geven. Noteer alle specifieke fout berichten of fout codes om te bepalen waarom de virtuele machine een probleem ondervindt.

![De console logboeken voor diagnostische gegevens over opstarten van VM weer geven](./media/troubleshoot-recovery-disks-portal-windows/screenshot-error.png)

U kunt ook klikken op **scherm opname downloaden** om een opname van de VM-scherm afbeelding te downloaden.

## <a name="view-existing-virtual-hard-disk-details"></a>Details van bestaande virtuele harde schijf weer geven
Voordat u de virtuele harde schijf aan een andere virtuele machine kunt koppelen, moet u de naam van de virtuele harde schijf (VHD) identificeren.

Selecteer de virtuele machine met een probleem en selecteer vervolgens **schijven**. Noteer de naam van de besturingssysteem schijf, zoals in het volgende voor beeld:

![Opslag-blobs selecteren](./media/troubleshoot-recovery-disks-portal-windows/view-disk.png)

## <a name="delete-existing-vm"></a>Bestaande VM verwijderen
Virtuele harde schijven en virtuele machines zijn twee verschillende resources in Azure. Een virtuele harde schijf is de locatie waar het besturings systeem zelf, toepassingen en configuraties worden opgeslagen. De virtuele machine zelf bevat alleen meta gegevens die de grootte of locatie bepalen, en verwijst naar bronnen zoals een virtuele harde schijf of een virtuele netwerk interface kaart (NIC). Aan elke virtuele harde schijf is een lease toegewezen wanneer deze aan een virtuele machine is gekoppeld. Hoewel gegevensschijven zelfs wanneer de virtuele machine wordt uitgevoerd, kunnen worden gekoppeld en losgekoppeld, kan de besturingssysteemschijf niet worden losgekoppeld tenzij de VM-resource wordt verwijderd. De lease blijft de besturingssysteem schijf koppelen aan een virtuele machine, zelfs wanneer de virtuele machine zich in de status gestopt en ontoegewezen bevindt.

De eerste stap voor het herstellen van uw virtuele machine is het verwijderen van de VM-resource zelf. Wanneer de virtuele machine wordt verwijderd, blijven de virtuele harde schijven aanwezig in uw opslagaccount. Nadat de VM is verwijderd, koppelt u de virtuele harde schijf aan een andere virtuele machine om problemen op te lossen en de fouten te verhelpen.

Selecteer uw virtuele machine in de portal en klik vervolgens op **verwijderen**:

![Scherm opname van diagnostische gegevens over opstarten van VM met opstart fout](./media/troubleshoot-recovery-disks-portal-windows/stop-delete-vm.png)

Wacht tot de VM is verwijderd voordat u de virtuele harde schijf aan een andere virtuele machine koppelt. De lease op de virtuele harde schijf die deze koppelt aan de VM moet worden vrijgegeven voordat u de virtuele harde schijf kunt koppelen aan een andere virtuele machine.

## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Bestaande virtuele harde schijf koppelen aan een andere virtuele machine
Voor de volgende stappen gebruikt u een andere virtuele machine voor het oplossen van problemen. U koppelt de bestaande virtuele harde schijf aan deze oplossing voor het oplossen van problemen met VM om de inhoud van de schijf te kunnen door bladeren en te bewerken. Met dit proces kunt u eventuele configuratie fouten corrigeren of aanvullende toepassings-of systeem logboek bestanden controleren, bijvoorbeeld. Kies of maak een andere virtuele machine die moet worden gebruikt voor het oplossen van problemen.

1. Selecteer uw resource groep in de portal en selecteer vervolgens de virtuele machine voor probleem oplossing. Selecteer **schijven**, selecteer **bewerken**en klik vervolgens op **gegevens schijf toevoegen**:

    ![Een bestaande schijf koppelen in de portal](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Selecteer in de lijst **gegevens schijven** de besturingssysteem schijf van de virtuele machine die u hebt geïdentificeerd. Als u de besturingssysteem schijf niet ziet, zorg er dan voor dat u problemen met de virtuele machine oplost en de besturingssysteem schijf zich in dezelfde regio (locatie) bevindt.
3. Selecteer **Opslaan** om de wijzigingen toe te passen.

## <a name="mount-the-attached-data-disk"></a>De gekoppelde gegevens schijf koppelen

1. Open een Extern bureaublad verbinding met uw VM. Selecteer uw virtuele machine in de portal en klik op **verbinden**. Down load en open het RDP-verbindings bestand. Voer als volgt uw referenties in om u aan te melden bij uw virtuele machine:

    ![Meld u aan bij uw VM met Extern bureaublad](./media/troubleshoot-recovery-disks-portal-windows/open-remote-desktop.png)

2. Open **Serverbeheer**en selecteer vervolgens **Bestands-en opslag Services**. 

    ![Bestands-en opslag Services in Serverbeheer selecteren](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. De gegevens schijf wordt automatisch gedetecteerd en gekoppeld. Selecteer **schijven**als u een lijst met de verbonden schijven wilt weer geven. U kunt uw gegevens schijf selecteren om volume gegevens weer te geven, inclusief de stationsletter. In het volgende voor beeld ziet u de gegevens schijf die is gekoppeld aan en met **F:** :

    ![De schijf is gekoppeld en de volume gegevens in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Problemen op de oorspronkelijke virtuele harde schijf oplossen
Als de bestaande virtuele harde schijf is gekoppeld, kunt u nu eventuele onderhouds-en probleemoplossings stappen uitvoeren. Zodra u de problemen hebt opgelost, kunt u doorgaan met de volgende stappen.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>De oorspronkelijke virtuele harde schijf ontkoppelen en loskoppelen
Wanneer de fouten zijn opgelost, koppelt u de bestaande virtuele harde schijf los van uw probleemoplossings-VM. U kunt de virtuele harde schijf niet met andere virtuele machines gebruiken totdat de lease die de virtuele harde schijf koppelt aan de VM voor probleem oplossing is vrijgegeven.

1. Open **Serverbeheer**van de RDP-sessie naar uw VM en selecteer vervolgens **Bestands-en opslag Services**:

    ![Selecteer Bestands-en opslag Services in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Selecteer **schijven** en selecteer vervolgens uw gegevens schijf. Klik met de rechter muisknop op uw gegevens schijf en selecteer **offline halen**:

    ![De gegevens schijf als offline instellen in Serverbeheer](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Ontkoppel nu de virtuele harde schijf van de VM. Selecteer uw virtuele machine in de Azure Portal en klik op **schijven**. 
4. Selecteer **bewerken**, selecteer de besturingssysteem schijf die u hebt toegevoegd en klik vervolgens op ontkoppelen:

    ![Bestaande virtuele harde schijf ontkoppelen](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Wacht tot de virtuele machine de gegevens schijf heeft losgekoppeld voordat u doorgaat.

## <a name="create-vm-from-original-hard-disk"></a>Een VM maken op basis van de oorspronkelijke harde schijf

### <a name="method-1-use-azure-resource-manager-template"></a>Methode 1 Azure Resource Manager sjabloon gebruiken
Gebruik [deze Azure Resource Manager sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet)om een virtuele machine te maken op basis van de oorspronkelijke virtuele harde schijf. Met de sjabloon implementeert u een virtuele machine in een bestaand of nieuw virtueel netwerk met behulp van de VHD-URL uit de vorige opdracht. Klik op de knop **implementeren naar Azure** als volgt:

![VM implementeren vanuit een sjabloon vanaf GitHub](./media/troubleshoot-recovery-disks-portal-windows/deploy-template-from-github.png)

De sjabloon wordt in de Azure Portal geladen voor implementatie. Voer de namen in voor de nieuwe virtuele machine en de bestaande Azure-resources en plak de URL naar uw bestaande harde schijf. Klik op **kopen**om te beginnen met de implementatie:

![VM implementeren op basis van een sjabloon](./media/troubleshoot-recovery-disks-portal-windows/deploy-from-image.png)

### <a name="method-2-create-a-vm-from-the-disk"></a>Methode 2 een virtuele machine maken op basis van de schijf

1. Selecteer in de Azure Portal uw resource groep in de portal en zoek vervolgens de besturingssysteem schijf. U kunt ook zoeken naar de schijf met behulp van de schijf naam:

    ![Schijf zoeken op basis van Azure Portal](./media/troubleshoot-recovery-disks-portal-windows/search-disk.png)
1. Selecteer **overzicht**en selecteer vervolgens **VM maken**.
    ![Een virtuele machine maken op basis van een schijf van Azure Portal](./media/troubleshoot-recovery-disks-portal-windows/create-vm-from-disk.png)
1. Volg de wizard om de virtuele machine te maken.

## <a name="re-enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten opnieuw inschakelen
Wanneer u uw virtuele machine maakt op basis van de bestaande virtuele harde schijf, worden diagnostische gegevens over opstarten mogelijk niet automatisch ingeschakeld. Als u de status van diagnostische gegevens over opstarten wilt controleren en indien nodig wilt inschakelen, selecteert u uw virtuele machine in de portal. Klik onder **bewaking**op **Diagnostische instellingen**. Controleer of de status is ingeschakeld en of het selectie vakje **Diagnostische gegevens** **over**opstarten is geselecteerd. Als u wijzigingen aanbrengt, klikt u op **Opslaan**:

![Instellingen voor diagnostische gegevens over opstarten bijwerken](./media/troubleshoot-recovery-disks-portal-windows/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Volgende stappen
Zie [problemen met RDP-verbindingen met een Azure VM oplossen](troubleshoot-rdp-connection.md)als u problemen ondervindt bij het maken van verbinding met uw virtuele machine. Zie problemen met [toepassings connectiviteit oplossen op een Windows-VM](troubleshoot-app-connection.md)voor problemen met het openen van toepassingen die op uw virtuele machine worden uitgevoerd.

Zie [Azure Resource Manager Overview](../../azure-resource-manager/resource-group-overview.md)voor meer informatie over het gebruik van Resource Manager.


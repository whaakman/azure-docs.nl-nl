---
title: Een testfailover voor Hyper-V-replicatie (zonder de System Center VMM) uitvoeren naar Azure | Microsoft Docs
description: Geeft een overzicht van de stappen die u nodig hebt voor een testfailover voor het Hyper-V-machines repliceren naar Azure met behulp van de Azure Site Recovery-service wordt uitgevoerd.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c9a4c3ca-84a0-4668-b700-9b0046202299
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 0974b9eda2cb7e3ba54a4a0fad0a768db644caf9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-to-azure"></a>Stap 11: Een testfailover voor Hyper-V-replicatie uitvoeren naar Azure

Dit artikel wordt beschreven hoe u een testfailover uitvoeren van on-premises Hyper-V virtuele machines (wordt niet beheerd door System Center VMM) naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Voordat u begint

Voordat u een testfailover die wordt aangeraden Controleer of de VM-eigenschappen en breng de gewenste wijzigingen uitvoert moet u. u kunt toegang tot de VM-eigenschappen in **gerepliceerde items**. De **Essentials** blade ziet u informatie over machines instellingen en status.

## <a name="managed-disk-considerations"></a>Beheerde schijf-overwegingen

[Schijven die worden beheerd](../virtual-machines/windows/managed-disks-overview.md) Schijfbeheer voor Azure VM's vereenvoudigen door het beheer van de storage-accounts die zijn gekoppeld aan de VM-schijven. 

- Beheerde schijven zijn gemaakt en gekoppeld aan de virtuele machine alleen wanneer er een failover naar Azure optreedt. Als u beveiliging inschakelt, worden gegevens uit de lokale virtuele machines repliceert naar storage-accounts.
- Beheerde schijven kunnen alleen voor virtuele machines die zijn geïmplementeerd met het implementatiemodel van Resource manager worden gemaakt.
- Failback vanuit Azure naar een on-premises Hyper-V-omgeving is momenteel niet ondersteund voor computers met beheerde-schijven. Mag alleen worden ingesteld **beheerde schijven gebruiken** naar **Ja** als u alleen een migratie alleen (failover naar Azure zonder failback)
- Met deze instelling is ingeschakeld, alleen beschikbaarheidssets in resourcegroepen die beschikken over **beheerde schijven gebruiken** ingeschakeld kan worden geselecteerd. Virtuele machines met beheerde schijven moeten zich in beschikbaarheidssets met **beheerde schijven gebruiken** ingesteld op **Ja**. Als de instelling is niet ingeschakeld voor virtuele machines, kunnen alleen in beschikbaarheidssets voor resourcegroepen zonder beheerde schijven ingeschakeld worden geselecteerd. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).
- - Als het opslagaccount dat u voor replicatie gebruikt zijn versleuteld met versleuteling van opslag-Service, kunnen beheerde schijven kunnen niet worden gemaakt tijdens de failover. In dit geval niet een gebruik van beheerde schijven, inschakelen of schakel de beveiliging voor de virtuele machine en opnieuw inschakelen voor gebruik van een opslagaccount dat geen versleuteling ingeschakeld. [Meer informatie](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Aandachtspunten voor netwerken
    
- U kunt instellen dat het doel-IP-adres moet worden gebruikt voor de Azure VM na een failover. Als u geen adres opgeeft, maakt de machine waarvoor een failover is uitgevoerd, gebruik van DHCP. Als u een adres instelt dat tijdens het uitvoeren van de failover niet beschikbaar is, mislukt de failover. Hetzelfde doel-IP-adres kan worden gebruikt voor een testfailover als het adres beschikbaar is in het testfailovernetwerk.
- Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele doelmachine opgeeft. Dat werkt als volgt:
    - Als het aantal netwerkadapters op de bronmachine kleiner is dan of gelijk is aan het aantal adapters dat is toegestaan voor de grootte van de doelmachine, heeft het doel hetzelfde aantal adapters als de bron.
    - Als het aantal adapters op de virtuele bronmachine groter is dan voor de doelgrootte is toegestaan, wordt het maximum voor de doelgrootte gebruikt.
    - Als de bronmachine bijvoorbeeld twee netwerkadapters heeft en de grootte van de doelmachine vier netwerkadapters ondersteunt, heeft de doelmachine twee adapters. Als de bronmachine twee adapters heeft, maar de grootte van de doelmachine slechts één adapter ondersteunt, heeft de doelmachine één adapter.     
- Als de virtuele machine meerdere netwerkadapters heeft, worden deze allemaal verbonden met hetzelfde netwerk.
- Als de virtuele machine meerdere netwerkadapters heeft dan het eerste beheerpunt dat wordt weergegeven in de lijst wordt de *standaard* netwerkadapter in de virtuele machine van Azure.


## <a name="view-and-manage-vm-settings"></a>Weergeven en beheren van instellingen voor VM

U wordt aangeraden de eigenschappen van de bronmachine te controleren voordat u een failover uitvoert.

1. In **beveiligde Items**, klikt u op **gerepliceerde Items**, en klik op de virtuele machine.

    ![Replicatie inschakelen](./media/hyper-v-site-walkthrough-test-failover/test-failover1.png)
2. In de **gerepliceerde item** deelvenster ziet u een overzicht van de VM-informatie, de status en de meest recente beschikbare herstelpunten. Klik op **eigenschappen** om meer details te bekijken.

    ![Replicatie inschakelen](./media/hyper-v-site-walkthrough-test-failover/test-failover2.png)
3. In **berekening en netwerk**, kunt u:
    - Wijzig de naam van de virtuele machine van Azure. De naam moet voldoen aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Geef een na een failover [resourcegroep].
    - Specificeer de doelgrootte van een voor de Azure VM
    - Selecteer een [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md).
    - Geef op of gebruik [schijven die worden beheerd](#managed-disk-considerations). Selecteer **Ja**als u wilt koppelen beheerde schijven aan de computer over de migratie naar Azure.
    - Netwerkinstellingen weergeven of wijzigen, met inbegrip van de netwerk-en het subnet waarin de virtuele machine in Azure worden opgeslagen na de failover- en het IP-adres dat wordt toegewezen aan deze.

    ![Replicatie inschakelen](./media/hyper-v-site-walkthrough-test-failover/test-failover4.png)
4. In **schijven**, vindt u informatie over het besturingssysteem en gegevensschijven op de virtuele machine.


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Voer nu een testfailover om te controleren of dat alles werkt zoals verwacht.

- Als u wilt verbinding maken met virtuele Azure-machines met RDP na een failover [voorbereiden om verbinding te](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Als u een volledige test wilt uitvoeren, moet u Active Directory en DNS naar uw testomgeving kopiëren. [Meer informatie](site-recovery-active-directory.md#test-failover-considerations).
 - Lees voor volledige informatie over de testfailover [in dit artikel](site-recovery-test-failover-to-azure.md) artikel.
 
 Nu u een failover uitvoeren:

1. Een enkele machine failover **gerepliceerde Items**, klikt u op de virtuele machine > **+ Testfailover** pictogram.
2. Als u een failover wilt uitvoeren op basis van een herstelplan, klikt u in **Herstelplannen** met de rechtermuisknop op het plan > **Testfailover**. Volg [deze instructies](site-recovery-create-recovery-plans.md) om een herstelplan te maken.
3. In **Testfailover**, selecteer het Azure-netwerk welke virtuele Azure-machines moet worden verbonden nadat er failover plaatsvindt.
4. Klik op **OK** om te beginnen met de failover. U kunt de voortgang volgen door te klikken op de virtuele machine om de eigenschappen te openen of op de **Testfailover** taak in de kluisnaam > **taken** > **Site Recovery-taken**.
5. Wanneer de failover is voltooid, moet u de gerepliceerde Azure-machine ook kunnen zien in Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is.
6. Als u verbindingen hebt voorbereid na een failover, kunt u verbinding maken met de Azure-VM.
7. Als u klaar bent, klikt u op **testfailover opschonen** op het herstelplan. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op. Hiermee verwijdert u de virtuele machines die tijdens de testfailover zijn gemaakt.



## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](site-recovery-failover.md) over verschillende soorten failovers en hoe ze uit te voeren.
- [Meer informatie over failback](site-recovery-failback-from-azure-to-hyper-v.md), zodat een failback- en virtuele Azure-machines repliceren naar de primaire lokale Hyper-V-site.


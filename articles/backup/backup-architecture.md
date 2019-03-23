---
title: Azure Backup-architectuur
description: Biedt een overzicht van de architectuur, onderdelen en processen die worden gebruikt door de Azure Backup-service.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: 98ffe145103b4be04014627ed04d04dcf7542015
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368949"
---
# <a name="azure-backup-architecture"></a>Azure Backup-architectuur

U kunt de [Azure Backup-service](backup-overview.md) naar back-ups naar de Microsoft Azure-cloud-platform. In dit artikel bevat een overzicht van Azure Backup-architectuur, onderdelen en processen. 

## <a name="what-does-azure-backup-do"></a>Wat is Azure Backup?

Azure Backup een back-up van de gegevens, de status van de machine en de workloads die worden uitgevoerd op on-premises machines en Azure virtual machines (VM). Er zijn een aantal scenario's voor Azure Backup.

## <a name="how-does-azure-backup-work"></a>Hoe werkt Azure Backup?

U kunt back-up van machines en gegevens met behulp van een aantal methoden:

- **Maak een back-up van on-premises computers**:
    - U kunt back-up on-premises Windows-computers rechtstreeks naar Azure met behulp van de Azure Backup Microsoft Azure Recovery Services agent (MARS). Linux-machines worden niet ondersteund.
    - U kunt een back-up on-premises virtuele machines maken met een back-server (System Center Data Protection Manager (DPM) of Microsoft Azure Backup Server (MABS)). U kunt vervolgens back-up van de back-upserver naar een Recovery Services-kluis in Azure.

- **Maak een back-up van virtuele Azure-machines**:
    - U kunt back-up van virtuele machines van Azure direct. Azure Backup een back-upextensie in de Azure VM-agent die wordt uitgevoerd op de virtuele machine geïnstalleerd. Deze extensie een back-up van de hele virtuele machine.
    - U kunt back-up van bepaalde bestanden en mappen op de virtuele machine van Azure door het uitvoeren van de MARS-agent.
    - U kunt back-up van virtuele Azure-machines naar de MABS die wordt uitgevoerd in Azure en u kunt vervolgens back-up van de MAB naar een Recovery Services-kluis.

Meer informatie over [wat u kunt back-up](backup-overview.md) en [back-upscenario's ondersteund](backup-support-matrix.md).

## <a name="where-is-data-backed-up"></a>Waar gegevens een back-up?

Een back-up gegevens van de Azure Backup worden opgeslagen in een Recovery Services-kluis. Een kluis is een online-opslagentiteit in Azure die wordt gebruikt voor het opslaan van gegevens, zoals back-ups, herstelpunten en back-upbeleid.

Recovery Services-kluizen hebben de volgende kenmerken:

- Kluizen maken het eenvoudig is om te organiseren van uw back-upgegevens, terwijl de beheer-overhead worden geminimaliseerd.
- In elk Azure-abonnement, kunt u maximaal 500 kluizen maken.
- U kunt een back-up items in een kluis, met inbegrip van virtuele machines van Azure en on-premises machines bewaken.
- U kunt de toegang van de kluis met Azure beheren [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
- U opgeven hoe de gegevens in de kluis worden gerepliceerd voor redundantie:
    - **Lokaal redundante opslag (LRS)**: Als u wilt beveiligen tegen fouten in een datacenter, kunt u LRS. LRS worden gegevens gerepliceerd naar een opslagschaaleenheid. [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
    - **Geografisch redundante opslag (GRS)**: Ter bescherming tegen regiobrede storingen, kunt u GRS. GRS worden uw gegevens gerepliceerd naar een secundaire regio. [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). 
    - Recovery Services-kluizen gebruiken standaard GRS. 

## <a name="backup-agents"></a>Backup-agents

Azure Backup biedt verschillende Backup-agents, afhankelijk van het type machine back-up:

**Agent** | **Details** 
--- | --- 
**MARS-agent** | <ul><li>Wordt uitgevoerd op afzonderlijke on-premises Windows Server-machines naar back-up van bestanden, mappen en de systeemstatus.</li> <li>Wordt uitgevoerd op virtuele Azure-machines naar back-up van bestanden, mappen en de systeemstatus.</li> <li>Wordt uitgevoerd op DPM/MABS-servers voor back-up van de lokale DPM/MABS-opslagschijf in Azure.</li></ul> 
**Azure VM-extensie** | Wordt uitgevoerd op virtuele Azure-machines naar back-up op een kluis.

## <a name="backup-types"></a>Back-uptypen

De volgende tabel beschrijft de verschillende typen van back-ups en wanneer ze worden gebruikt:

**Type back-up** | **Details** | **Gebruik**
--- | --- | ---
**Volledige** | Een volledige back-up bevat de hele gegevensbron. Neemt meer netwerkbandbreedte dan differentiële of incrementele back-ups. | Gebruikt voor de eerste back-up.
**Differentiële** |  Een differentiële back-up slaat de blokken die zijn gewijzigd sinds de eerste volledige back-up. Een kleinere hoeveelheid netwerk en opslag gebruikt en redundante exemplaren met ongewijzigde gegevens niet behouden.<br/><br/> Inefficiënt omdat de gegevensblokken die ongewijzigd tussen latere back-ups zijn overgebracht en opgeslagen. | Niet gebruikt door Azure Backup.
**Incrementele** | Een incrementele back-up bevat alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up. Hoge efficiëntie van opslag en netwerk. <br/><br/> Met incrementele back-up is niet nodig om aan te vullen met volledige back-ups. | Gebruikt door DPM/MABS voor back-ups van schijf, en gebruikt in alle back-ups naar Azure.

## <a name="sql-server-backup-types"></a>SQL Server-back-uptypen

De volgende tabel beschrijft de verschillende typen back-ups die worden gebruikt voor SQL Server-databases en hoe vaak ze worden gebruikt:

**Type back-up** | **Details** | **Gebruik**
--- | --- | ---
**Volledige back-up** | Bij een volledige back-up wordt er een back-up van de hele database gemaakt. Deze bevat alle gegevens in een specifieke database of in een set bestanden of bestandsgroepen. Een volledige back-up bevat ook voldoende Logboeken om die gegevens te herstellen. | U kunt maximaal één volledige back-up per dag activeren.<br/><br/> U kunt een volledige back-up maken van een dagelijks of wekelijks interval.
**Differentiële back-up** | Een differentiële back-up is gebaseerd op de meest recente vorige volledige gegevens back-ups.<br/><br/> Wordt alleen de gegevens die zijn gewijzigd nadat de volledige back-up vastgelegd. |  U kunt maximaal één differentiële back-up per dag activeren.<br/><br/> U kunt niet zowel een volledige back-up als een differentiële back-up configureren op dezelfde dag.
**Transactielogboekback-up** | Met een logboekback-up kunt u herstel naar een bepaald tijdstip uitvoeren, tot op een specifieke seconde. | U kunt maximaal elke 15 minuten een back-up van het transactielogboek configureren.

### <a name="comparison-of-backup-types"></a>Vergelijking van de back-uptypen

Opslagverbruik, beoogde hersteltijd (RTO) en netwerkverbruik varieert voor elk type back-up. De volgende afbeelding ziet u een vergelijking van de back-typen:

- Gegevensbron A bestaat uit 10 opslag blokken, A1-A10, waarvan een back-up maandelijks worden gemaakt.
- Blokken A2, A3, A4 en A9 zijn in de eerste maand gewijzigd, en blok A5 is de maand erna gewijzigd.
- Voor differentiële back-ups in de tweede maand gewijzigde blokken A2, A3, A4 en A9 back-ups. In de derde maand wordt er opnieuw een back-up gemaakt van dezelfde blokken en van het gewijzigde blok A5. Van de gewijzigde blokken worden back-ups gemaakt totdat de volgende volledige back-up wordt uitgevoerd.
- Voor incrementele back-ups, in de tweede maand blokken A2, A3, A4 en A9 gemarkeerd als gewijzigd en overgedragen. In het derde maand wordt alleen gewijzigd blok A5 gemarkeerd als gemarkeerd en overgedragen. 

![afbeelding met vergelijkingen van back-upmethoden](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Back-functies

De volgende tabel geeft een overzicht van de ondersteunde functies voor de verschillende typen back-up:

**Functie** | **On-premises Windows Server-machines (direct)** | **Azure VM's** | **Computers of apps met DPM/MABS**
--- | --- | --- | ---
Back-up naar de kluis | ![Ja][green] | ![Ja][green] | ![Ja][green] 
Back-up naar schijf voor DPM/MABS, klikt u vervolgens naar Azure | | | ![Ja][green] 
Gegevens die worden verzonden voor back-up comprimeren | ![Ja][green] | Er wordt geen compressie wordt gebruikt bij de overdracht van gegevens. Opslag is enigszins schaalfactor, maar de herstelbewerking is sneller.  | ![Ja][green] 
Incrementele back-up uitvoeren |![Ja][green] |![Ja][green] |![Ja][green] 
Back-up van ontdubbelde schijven | | | ![Gedeeltelijk][yellow]<br/><br/> Voor DPM/MABS-servers geïmplementeerd alleen on-premises. 

![tabelsleutel](./media/backup-architecture/table-key.png)

## <a name="architecture-direct-backup-of-azure-vms"></a>Architectuur: Directe back-ups van virtuele Azure-machines

1. Wanneer u back-up voor een Azure-VM inschakelt, wordt een back-up wordt uitgevoerd volgens het schema dat u opgeeft.
1. Een Backup-extensie is geïnstalleerd op de virtuele machine tijdens de eerste back-up, als de virtuele machine wordt uitgevoerd.
    - Voor Windows-VM's, is de VMSnapshot-extensie geïnstalleerd.
    - Voor virtuele Linux-machines, is de VMSnapshot-Linux-extensie geïnstalleerd.
1. De extensie van een opslagniveau momentopname. 
    - Back-up-coördinaten voor Windows-VM's die worden uitgevoerd, met de Windows Volume Shadow Copy Service (VSS) naar een app-consistente momentopname te maken van de virtuele machine. Back-up wordt standaard volledige VSS-back-ups. Als back-up niet kan een app-consistente momentopname te maken, maakt deze een bestand-consistente momentopname.
    - Voor Linux-VM's momentopname back-up van een bestandsconsistente. Voor app-consistente momentopnamen moet u handmatig aanpassen voor/na-scripts.
    - Back-up is geoptimaliseerd door de back-ups van elke VM-schijf parallel. Voor elke schijf een back-wordt gemaakt, wordt met Azure Backup leest de blokken op schijf en alleen de gewijzigde gegevens worden opgeslagen. 
1. Nadat de momentopname wordt gemaakt, worden de gegevens overgedragen naar de kluis. 
    - Alleen blokken met gegevens die gewijzigd sinds de laatste back-up worden gekopieerd.
    - Gegevens wordt niet versleuteld. Azure Backup een back-up Azure VM's die met behulp van Azure Disk Encryption zijn versleuteld.
    - Momentopname van de gegevens mogelijk niet direct worden gekopieerd naar de kluis. Tijdens piekuren, kan de back-up enkele uren duren. Totaal aantal back-uptijd voor een virtuele machine is minder dan 24 uur voor dagelijkse back-upbeleid.
1. Nadat de gegevens worden verzonden naar de kluis, de momentopname is verwijderd en een herstelpunt wordt gemaakt.

Azure-VM's nodig toegang tot internet voor opdrachten voor het beheer. Als u back-ups van werkbelastingen binnen de virtuele machine (bijvoorbeeld SQL Server-database back-ups), moet de gegevens van de back-end ook toegang tot internet. 

![Back-up van virtuele machines in Azure](./media/backup-architecture/architecture-azure-vm.png)

## <a name="architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders"></a>Architectuur: Directe back-ups van on-premises Windows Server-computers of virtuele machine van Azure-bestanden of mappen

1. Als u het scenario instelt, downloaden en installeren van de MARS-agent op de machine. U selecteert vervolgens wat u moet een back-up, wanneer de back-ups wordt uitgevoerd en hoe lang ze gaat worden bewaard in Azure.
1. De eerste back-up wordt uitgevoerd op basis van de instellingen van uw back-up.
1. De MARS-agent gebruikt VSS op een punt-in-time-momentopname van de volumes die zijn geselecteerd voor back-up.
    - De MARS-agent maakt gebruik van alleen de schrijfbewerking voor Windows-systeem om vast te leggen van de momentopname.
    - Omdat de agent geen gebruik maakt van alle VSS-schrijvers van toepassing, vastleggen niet het app-consistente momentopnamen.
1. Na het maken van de momentopname met VSS, maakt de MARS-agent een virtuele harde schijf (VHD) in de cachemap die u hebt opgegeven tijdens het configureren van de back-up. De agent worden ook opgeslagen controlesommen voor elk gegevensblok.
1. Incrementele back-ups uitgevoerd volgens het schema dat u opgeeft, tenzij u een ad-hoc back-up uitvoert.
1. Gewijzigde bestanden worden geïdentificeerd in incrementele back-ups, en een nieuwe VHD is gemaakt. De VHD is gecomprimeerd en versleuteld, en vervolgens deze wordt verzonden naar de kluis.
1. Nadat de incrementele back-up is voltooid, wordt de nieuwe VHD wordt samengevoegd met de VHD gemaakt na de initiële replicatie. Deze samengevoegde VHD biedt de meest recente status om te worden gebruikt voor vergelijking van continue back-up.

![Back-up van on-premises Windows Server-computers met de MARS-agent](./media/backup-architecture/architecture-on-premises-mars.png)

## <a name="architecture-back-up-to-dpmmabs"></a>Architectuur: Back-up naar DPM/MABS

1. U installeren de DPM- of MABS-beveiligingsagent op computers die u wilt beveiligen. U kunt vervolgens de machines toevoegen aan een DPM-beveiligingsgroep.
    - Ter bescherming van on-premises computers, moet de DPM- of MABS-server zich op locatie.
    - Ter bescherming van virtuele Azure-machines, wordt de MABS-server moet zich in Azure, die wordt uitgevoerd als een Azure-VM.
    - U kunt back-volumes, shares, bestanden en mappen beveiligen met DPM/MABS. U kunt ook beveiligen met systeemstatus van een computer (bare-metal) en u kunt specifieke apps beveiligen met app-bewuste back-upinstellingen.
1. Als u beveiliging voor een machine of een app in DPM/MABS instellen, selecteert u de back-up naar de lokale MABS/DPM-schijf voor kortdurende opslag en naar Azure voor online beveiliging. U geeft ook wanneer de back-up naar de lokale DPM/MABS-opslag moet worden uitgevoerd en wanneer de online back-up naar Azure moet worden uitgevoerd.
1. De schijf van de beveiligde werkbelasting is back-ups op de lokale MABS/DPM-schijven, volgens de planning die u hebt opgegeven.
4. De DPM/MABS-schijven worden ondersteund bij de kluis door de MARS-agent die wordt uitgevoerd op de DPM/MABS-server.

![Back-up van machines en -werkbelastingen beveiligd met DPM- of MABS](./media/backup-architecture/architecture-dpm-mabs.png)

## <a name="azure-vm-storage"></a>Azure VM-opslag

Virtuele machines in Azure gebruiken schijven voor het opslaan van hun besturingssysteem, apps en gegevens. Elke Azure-VM heeft ten minste twee schijven: een schijf voor het besturingssysteem en een tijdelijke schijf. Azure-VM's kunnen ook gegevensschijven voor app-gegevens hebben. Schijven zijn opgeslagen als een VHD.

- VHD's worden opgeslagen als pagina-blobs in de standard- of premium storage-accounts in Azure:
    - **Standard-opslag:** Betrouwbare, voordelige schijfondersteuning voor virtuele machines uitvoeren van workloads die niet gevoelig voor latentie zijn. Standard-opslag kunt gebruiken standaard SSD (solid-state drive)-schijven of standaard vasteschijfstations (HDD)-schijven.
    - **Premium-opslag:** Schijfondersteuning met hoge prestaties. Maakt gebruik van premium SSD-schijven.
- Er zijn verschillende prestatielagen voor schijven:
    - **Standaard harde schijf:** Ondersteund door HDD's en gebruikt voor goedkope opslag.
    - **Standard-SSD-schijf:** Elementen van de premium-SSD-schijven en standaard harde schijven worden gecombineerd. Biedt consistente prestaties en betrouwbaarheid dan harde schijven, maar nog steeds rendabele.
    - **Premium-SSD-schijf:** SSD's en biedt hoge prestaties en lage latentie voor virtuele machines die I/O-intensieve workloads worden uitgevoerd.
- Schijven kunnen worden beheerd of onbeheerd:
    - **Niet-beheerde schijven:** Traditionele type schijven dat wordt gebruikt door virtuele machines. Voor deze schijven die u kunt uw eigen opslagaccount maakt en dit opgeven wanneer u de schijf maakt. Vervolgens moet u bepalen hoe u kunt storage-resources optimaliseren voor uw virtuele machines.
    - **Beheerde schijven:** Azure maakt en beheert de storage-accounts voor u. Geeft u de grootte en prestaties laag voor schijf en Azure beheerde schijven voor u gemaakt. Als u schijven toevoegt en schalen van virtuele machines, regelt Azure storage-accounts.

Zie de volgende artikelen voor meer informatie over disk-opslag en de typen beschikbare schijfruimte voor virtuele machines:

- [Azure-beheerde schijven voor Windows-VM 's](../virtual-machines/windows/managed-disks-overview.md)
- [Azure-beheerde schijven voor virtuele Linux-machines](../virtual-machines/linux/managed-disks-overview.md)
- [Typen beschikbare schijfruimte voor virtuele machines](../virtual-machines/windows/disks-types.md)

### <a name="back-up-and-restore-azure-vms-with-premium-storage"></a>Back-up en herstellen van Azure-VM's met premium storage 

U kunt een back-up van virtuele Azure-machines maken met behulp van premium-opslag met Azure Backup:

- Tijdens het proces van het back-ups van VM's met premium storage, maakt de Backup-service een tijdelijke faseringslocatie met de naam *AzureBackup -*, in de storage-account. De grootte van de faseringslocatie is gelijk aan de grootte van de momentopname herstellen.
- Zorg ervoor dat de premium storage-account voldoende vrije ruimte heeft voor de tijdelijke faseringslocatie. [Meer informatie](../storage/common/storage-scalability-targets.md#premium-performance-storage-account-scale-limits). Wijzig de faseringslocatie niet.
- Nadat de back-uptaak is voltooid, wordt de faseringslocatie verwijderd.
- De prijs van opslag die wordt gebruikt voor de faseringslocatie is consistent met [prijzen voor premium storage](../virtual-machines/windows/disks-types.md#billing).

Wanneer u virtuele Azure-machines herstellen met behulp van de premium-opslag, kunt u ze kunt herstellen naar premium of standard-opslag. Normaal gesproken zou u ze herstellen naar premium storage. Maar als u alleen een subset van bestanden van de virtuele machine nodig hebt, is het mogelijk goedkoper zijn met herstellen naar de standaardopslag.

### <a name="back-up-and-restore-managed-disks"></a>Back-up en herstellen van beheerde schijven

Back-up van virtuele Azure-machines met beheerde schijven:

- U maakt u een back-up van virtuele machines met beheerde schijven op dezelfde manier als andere Azure-VM te doen. U kunt back-up van de virtuele machine rechtstreeks vanuit de instellingen van de virtuele machine, of u kunt back-up inschakelen voor virtuele machines in de Recovery Services-kluis.
- U kunt back-ups maken van virtuele machines op beheerde schijven via RestorePoint-verzamelingen die zijn gebouwd boven op beheerde schijven.
- Azure Backup biedt ook ondersteuning voor back-ups van virtuele machines met beheerde schijven die met behulp van Azure Disk Encryption zijn versleuteld.

Wanneer u virtuele machines met beheerde schijven herstellen, kunt u herstellen naar een volledige VM met beheerde schijven of naar een opslagaccount:

- Tijdens het herstelproces regelt Azure de beheerde schijven. Als u de optie voor het opslagaccount, moet u het opslagaccount dat gemaakt tijdens het herstelproces beheren.
- Als u een beheerde virtuele machine die versleuteld herstelt, zorg ervoor dat de VM sleutels en geheimen in de key vault bestaan voordat u begint met het herstelproces.

## <a name="next-steps"></a>Volgende stappen

- Bekijk de ondersteuningsmatrix voor [meer informatie over ondersteunde functies en beperkingen voor back-upscenario's](backup-support-matrix.md).
- Instellen van back-up voor een van deze scenario's:
    - [Maak een back-up van virtuele Azure-machines](backup-azure-arm-vms-prepare.md).
    - [Rechtstreeks een back-up maken van Windows-machines](tutorial-backup-windows-server-to-azure.md), zonder een back-upserver.
    - [MABS instellen](backup-azure-microsoft-azure-backup.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar MABS maken.
    - [DPM instellen](backup-azure-dpm-introduction.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar DPM maken.


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png


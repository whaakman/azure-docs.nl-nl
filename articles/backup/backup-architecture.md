---
title: Azure Backup-architectuur
description: Biedt een overzicht van de architectuur, onderdelen en processen die worden gebruikt door de Azure Backup-service.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: raynew
ms.openlocfilehash: 4f26c805c42f027409127232fcfef9840939e8d9
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329180"
---
# <a name="azure-backup-architecture"></a>Azure Backup-architectuur

U kunt de [Azure Backup-service](backup-overview.md) gebruiken om een back-up van gegevens naar de Microsoft Azure-cloud te maken. In dit artikel bevat een overzicht van Azure Backup-architectuur, onderdelen en processen. 


## <a name="what-does-azure-backup-do"></a>Wat is Azure Backup?

Azure Backup een back-up van gegevens, status van de machine en workloads die worden uitgevoerd op on-premises machines en virtuele Azure-machines. Er zijn een aantal scenario's voor Azure Backup.

- **Maak een back-up van on-premises computers**:
    - U kunt back-up van on-premises computers rechtstreeks naar Azure met Azure Backup.
    - U kunt on-premises virtuele machines met System Center Data Protection Manager (DPM) of Microsoft Azure Backup-Server (MABS) beveiligen, en klik vervolgens op zijn beurt back-up van de beveiligde gegevens op DPM/MABS naar Azure met Azure Backup.
- **Maak een back-up van virtuele Azure-machines**:
    - U kunt back-up van Azure-VM's rechtstreeks met Azure Backup.
    - U kunt virtuele machines van Azure met DPM- of MABS worden uitgevoerd in Azure beveiligen en klik vervolgens op zijn beurt back-up van de beveiligde gegevens op DPM/MABS-gegevens met Azure Backup.

Meer informatie over [wat u kunt back-up](backup-overview.md), en [back-upscenario's ondersteund](backup-support-matrix.md).


## <a name="where-is-data-backed-up"></a>Waar gegevens een back-up?

Azure Backup-winkels back-upgegevens in Recovery Services-kluis. Een kluis is een online-opslagentiteit in Azure, die wordt gebruikt voor het opslaan van gegevens zoals back-ups, herstelpunten en back-upbeleid.

- Recovery Services-kluizen maken het eenvoudig om uw back-upgegevens te ordenen, terwijl de beheertaken minimaal zijn.
- In elk Azure-abonnement, kunt u maximaal 500 Recovery Services-kluizen maken. 
- U kunt back-ups van items in een kluis, met inbegrip van virtuele machines van Azure en on-premises machines bewaken.
- U kunt de toegang van de kluis met Azure beheren [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
- U opgeven hoe de gegevens in de kluis worden gerepliceerd voor redundantie:
    - **LRS**: Lokaal redundante opslag (LRS) kunt u beschermt tegen fouten in een datacenter. LRS worden gegevens gerepliceerd naar een opslagschaaleenheid. [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
    - **GRS**: Geografisch redundante opslag (GRS), kunt u gebruiken: Beschermt tegen regiobrede storingen. Deze repliceert uw gegevens naar een secundaire regio. [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). 
    - Recovery Services-kluizen voor back-up gebruiken standaard GRS. 



## <a name="how-does-azure-backup-work"></a>Hoe werkt Azure Backup?

Azure back-up wordt uitgevoerd back-uptaken op basis van een standaard of aangepast back-upbeleid. De manier waarop Azure Backup een back-up wordt, is afhankelijk van het scenario.

**Scenario** | **Details** 
--- | ---
**Rechtstreeks een back-up on-premises computers** | Azure Backup gebruikt om rechtstreeks back-up on-premises virtuele machines, de Microsoft Azure Recovery Services agent (MARS). De agent is geïnstalleerd op elke machine die u back wilt-up. <br/><br/> Dit type back-up is niet beschikbaar voor on-premises Linux-computers. 
**Direct back-up van virtuele Azure-machines** | Direct back-ups van virtuele Azure-machines, is een Azure VM-extensie geïnstalleerd op de virtuele machine de eerste keer dat een back-up wordt uitgevoerd voor de virtuele machine. 
**Back-up van machines en apps die zijn beveiligd door DPM of MABS** | In dit scenario wordt wordt de machine/app eerst back-ups op DPM- of MABS lokale opslag. Vervolgens de gegevens in DPM/MABS een back-up naar de kluis back-up van Azure. On-premises virtuele machines kunnen worden beveiligd door DPM/MABS on-premises uitgevoerd. Azure-VM's kunnen worden beveiligd door DPM/MABS worden uitgevoerd in Azure.

[Bekijk een overzicht](backup-overview.md), en Zie [wat wordt ondersteund](backup-support-matrix.md) voor elk scenario.


### <a name="backup-agents"></a>Backup-agents

Azure Backup biedt verschillende agents, afhankelijk van het type back-up.

**Agent** | **Details** 
--- | --- 
**Microsoft Azure Recovery Services agent (MARS)** | Deze agent wordt uitgevoerd op afzonderlijke on-premises Windows-Servers voor back-up van bestanden, mappen en de systeemstatus<br/><br/> Deze agent wordt uitgevoerd op DPM/MABS-servers voor back-up van de schijf van de lokale opslag DPM/MABS. Machines en apps zijn back-ups lokaal op deze schijf DPM/MABS.
**Azure VM-extensie** | Als u wilt back-up van virtuele Azure-machines, wordt een back-upextensie toegevoegd aan de Azure-VM-agent die wordt uitgevoerd op de virtuele machines. 


## <a name="backup-types"></a>Back-uptypen

**Type back-up** | **Details** | **Gebruik**
--- | --- | ---
**Volledige** | Een back-up bevat de hele gegevensbron.<br/><br/> Volledige back-up duurt meer netwerkbandbreedte. | Gebruikt voor de eerste back-up.
**Differentiële** |  Slaat de blokken die zijn gewijzigd sinds de eerste volledige back-up. Een kleinere hoeveelheid netwerk en opslag gebruikt en redundante exemplaren met ongewijzigde gegevens niet behouden.<br/><br/> Inefficiënt omdat de gegevensblokken die ongewijzigd tussen opeenvolgende back-ups zijn overgebracht en opgeslagen. | Niet gebruikt door Azure Backup.
**Incrementele** | Hoge efficiëntie van opslag en netwerk. Alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up worden opgeslagen. <br/><br/> U hoeft te met incrementele back-up, is het niet nodig om aan te vullen met volledige back-ups. | Gebruikt door DPM/MABS voor back-ups van schijf, en gebruikt in alle back-ups naar Azure.

### <a name="comparison"></a>Vergelijking

Opslagverbruik, beoogde hersteltijd (RTO) en netwerkverbruik varieert voor elk type back-up. De volgende afbeelding ziet u een vergelijking van de back-uptypen.
- Gegevensbron die a uit 10 bestaat opslagblokken A1-A10, waarvan back-ups per maand.
- Blokken A2, A3, A4 en A9 zijn in de eerste maand gewijzigd, en blok A5 is de maand erna gewijzigd.
- Voor differentiële back-ups in de tweede maand gewijzigde blokken A2, A3, A4 en A9 back-ups. In de derde maand wordt er opnieuw een back-up gemaakt van dezelfde blokken en van het gewijzigde blok A5. Van de gewijzigde blokken worden back-ups gemaakt totdat de volgende volledige back-up wordt uitgevoerd.
- Voor incrementele back-ups, na het maken van de volledige back-up in de eerste maand blokken A2, A3, A4 en A9 gemarkeerd als gewijzigd en overgedragen naar de tweede maand. In het derde maand wordt alleen gewijzigd blok A5 gemarkeerd als gemarkeerd en overgedragen. 

![afbeelding met vergelijkingen van back-upmethoden](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Back-functies

De volgende tabel geeft een overzicht van functies voor verschillende typen back-up.

**Functie** | **On-premises Windows-computers (direct)** | **Azure VM's** | **Met DPM/MABS-machines/apps**
--- | --- | --- | ---
Back-up naar de kluis | ![Ja][green] | ![Ja][green] | ![Ja][green] 
Back-up naar schijf voor DPM/MABS en Azure | | | ![Ja][green] 
Gegevens die worden verzonden voor back-up comprimeren | ![Ja][green] | Er wordt geen compressie wordt gebruikt bij de overdracht van gegevens. Opslag is enigszins schaalfactor, maar de herstelbewerking is sneller.  | ![Ja][green] 
Incrementele back-up uitvoeren |![Ja][green] |![Ja][green] |![Ja][green] 
Back-up van ontdubbelde schijven | | | ![Gedeeltelijk][yellow]<br/><br/> Voor DPM/MABS-servers geïmplementeerd alleen on-premises. 

![tabelsleutel](./media/backup-architecture/table-key.png)


## <a name="architecture-direct-backup-of-on-premises-windows-machines"></a>Architectuur: Directe back-ups van on-premises Windows-machines

1. Als u het scenario instelt, u downloaden en installeren van de Microsoft Azure Recovery Services agent (MARS) op de computer, selecteert u wat u moet een back-up, wanneer de back-ups wordt uitgevoerd en hoe lang ze gaat worden bewaard in Azure.
2. De eerste back-up wordt uitgevoerd in overeenstemming met de instellingen van uw back-up.
3. De MARS-agent maakt gebruik van de service Windows Volume Shadow Copy (VSS) om een point-in-time-momentopname van de volumes die zijn geselecteerd voor back-up.
    - De MARS-agent gebruikt alleen de Windows-systeem schrijven om vast te leggen van de momentopname.
    - De agent geen gebruik maakt van een toepassing VSS-schrijvers en dus app-consistente momentopnamen niet vastleggen.
3. Na het maken van de momentopname met VSS, de MARS-agent maakt u een VHD in de cachemap die u hebt opgegeven tijdens het configureren van de back-up, en slaat controlesommen voor elke gegevensblokken. 
4. Incrementele back-ups uitgevoerd volgens de planning die u opgeeft, tenzij u een ad-hoc back-up uitvoert.
5. Gewijzigde bestanden worden geïdentificeerd in incrementele back-ups, en een nieuwe VHD is gemaakt. Het is gecomprimeerd en versleuteld en verzonden naar de kluis.
6. Nadat de incrementele back-up is voltooid, wordt de nieuwe VHD wordt samengevoegd met de VHD gemaakt na de initiële replicatie, biedt de meest recente status om te worden gebruikt voor vergelijking van continue back-up. 

![Back-up van on-premises Windows server met de MARS-agent](./media/backup-architecture/architecture-on-premises-mars.png)


## <a name="architecture-direct-backup-of-azure-vms"></a>Architectuur: Directe back-ups van virtuele Azure-machines

1. Wanneer u back-up voor een Azure-VM inschakelt, wordt een back-up wordt uitgevoerd in overeenstemming met de planning die u opgeeft.
2. Een Backup-extensie is geïnstalleerd op virtuele machine tijdens de eerste back-up, als deze wordt uitgevoerd.
    - Voor Windows-VM's de VMSnapshot is extensie geïnstalleerd.
    - Voor Linux-VM's is de VMSnapshot-Linux-extensie geïnstalleerd.
3. De extensie maakt een momentopname van opslag-niveau. 
    - Voor Windows-VM's die worden uitgevoerd, coördineert de back-ups met VSS op een app-consistente momentopname te maken van de virtuele machine. Back-up wordt standaard volledige VSS-back-ups. Als back-up niet kan een app-consistente momentopname te maken, maakt deze een bestand-consistente momentopname.
    - Voor Linux VM's back-up wordt een bestandsconsistente back-up. U moet handmatig aanpassen voor/na-scripts voor app-consistente momentopnamen.
    - Back-up is geoptimaliseerd door de back-ups van elke VM-schijf parallel. Voor elke schijf een back-wordt gemaakt, wordt met Azure Backup leest de blokken op schijf en alleen de gewijzigde gegevens worden opgeslagen. 
4. Nadat de momentopname wordt gemaakt, worden de gegevens overgedragen naar de kluis. 
    - Alleen blokken met gegevens die zijn gewijzigd sinds de laatste back-up worden gekopieerd.
    - Gegevens wordt niet versleuteld. Azure Backup een back-up Azure VM's die zijn versleuteld met behulp van Azure Disk Encryption (ADE).
    - Momentopname van de gegevens mogelijk niet direct worden gekopieerd naar de kluis. Het duurt enkele uren tijdens pieken. Totaal aantal back-uptijd voor een virtuele machine worden kleiner dat 24 uur voor dagelijkse back-upbeleid.
5. Nadat gegevens zijn verzonden naar de kluis, de momentopname is verwijderd en een herstelpunt wordt gemaakt.


![Back-up van virtuele machines in Azure](./media/backup-architecture/architecture-azure-vm.png)


## <a name="architecture-back-up-to-dpmmabs"></a>Architectuur: Back-up naar DPM/MABS

1. U de DPM- of MABS-beveiligingsagent installeren op computers die u wilt beveiligen en de machines toevoegen aan een DPM-beveiligingsgroep.
    - Ter bescherming van on-premises computers, moet de DPM- of MABS-server zich op locatie.
    - Ter bescherming van virtuele Azure-machines, moet de DPM- of MABS-server in Azure, die wordt uitgevoerd als een Azure-VM zich bevinden.
    - Met behulp van DPM/MABS kunt u back-up van volumes, shares, bestanden en map beveiligen. U kunt machines system state/bare metal-beveiligen, en u kunt specifieke apps met app-bewuste back-upinstellingen beveiligen.
2. Als u beveiliging voor een machine of een app in DPM instellen, selecteert u de back-up naar de lokale DPM-schijf voor kortdurende opslag en naar Azure (online beveiliging). U ook opgeven wanneer de back-up naar de lokale DPM/MABS-opslag moet worden uitgevoerd en wanneer de online back-up naar Azure moet worden uitgevoerd.
3. De schijf van de beveiligde werkbelasting is back-ups naar de lokale DPM-schijven, en naar Azure, volgens de planning die u hebt opgegeven.
4. De online back-up naar de kluis wordt verwerkt door de MARS-agent die wordt uitgevoerd op de DPM/MABS-server.

![Back-up van machines /-werkbelastingen beveiligd met DPM- of MABS](./media/backup-architecture/architecture-dpm-mabs.png)







## <a name="azure-vm-storage"></a>Azure VM-opslag

- Virtuele machines in Azure gebruiken schijven voor het opslaan van hun besturingssysteem, apps en gegevens.
- Azure virtuele machines hebben ten minste twee schijven. Een voor het besturingssysteem en een tijdelijke schijf. Ze kunnen ook gegevensschijven voor app-gegevens hebben. Schijven zijn opgeslagen als een VHD.
- VHD's worden opgeslagen als pagina-blobs in de standard- of premium storage-accounts in Azure.
    - Standard-opslag: Betrouwbare, voordelige schijfondersteuning voor virtuele machines uitvoeren van workloads die niet gevoelig voor latentie zijn. Standard-opslag kunt standaardschijven SSD of HDD standaardschijven gebruiken.
    - Premium-opslag: Schijfondersteuning met hoge prestaties. Maakt gebruik van premium SSD-schijven.
- Er zijn verschillende prestatielagen voor schijven:
    - Standaard harde schijf: Ondersteund door HDD's en gebruikt voor goedkope opslag.
    - Standard-SSD-schijf: Element van de premium-SSD-schijven en standaard harde schijven, biedt consistente prestaties en betrouwbaarheid dan harde schijven, maar nog steeds rendabele combineert.
    - Premium-SSD-schijf: Ondersteund door SSD, bieden hoge prestaties en lage latentie voor virtuele machines met i/o-intensieve workloads.
- Schijven kunnen worden beheerd of onbeheerd:
    - Niet-beheerde schijven: Traditionele typen schijven die worden gebruikt door virtuele machines. Voor deze schijven die u kunt uw eigen opslagaccount maakt en dit opgeven wanneer u de schijf maakt. U moet bepalen hoe storage-resources optimaliseren voor uw virtuele machines.
    - Beheerde schijven: Azure regelt het maken en storage-accounts beheren voor u. Geeft u de grootte en prestaties laag voor schijf en Azure maakt, de schijven voor u beheren. Azure regelt de opslag bij het toevoegen van schijven en schalen van virtuele machines.

Meer informatie:

- Meer informatie over disk-opslag voor [Windows](../virtual-machines/windows/managed-disks-overview.md) en [Linux](../virtual-machines/linux/managed-disks-overview.md) VM's.
- Meer informatie over de beschikbare [schijftypen](../virtual-machines/windows/disks-types.md) zoals standard en premium.


### <a name="backing-up-and-restoring-azure-vms-with-premium-storage"></a>Back-up en herstellen van Azure-VM's met premium storage 

U kunt back-up van virtuele Azure-machines met behulp van premium-opslag met Azure Backup:

- Tijdens het back-ups van VM's met premium storage, maakt de Backup-service een tijdelijke faseringslocatie met de naam 'AzureBackup-', in de storage-account. De faseringslocatie is even groot als de momentopname van het herstelpunt.
- Zorg ervoor dat de premium storage-account voldoende vrije ruimte heeft voor de tijdelijke faseringslocatie. [Meer informatie](../storage/common/storage-scalability-targets.md#premium-storage-account-scale-limits). Wijzig de faseringslocatie niet.
- Nadat de back-uptaak is voltooid, wordt de faseringslocatie verwijderd.
- De prijs van opslag die wordt gebruikt voor de faseringslocatie is consistent met [prijzen voor premium storage](../virtual-machines/windows/disks-types.md#billing).

Wanneer u virtuele Azure-machines met behulp van premium-opslag herstelt, kunt u ze kunt herstellen naar premium of standard-opslag. Doorgaans u wilt herstellen naar premium, maar het is mogelijk goedkoper zijn met herstellen naar standard als u alleen een subset van de bestanden van de virtuele machine.

### <a name="backing-up-and-restoring-managed-disks"></a>Back-up en herstellen van beheerde schijven

U kunt back-up van virtuele Azure-machines met beheerde schijven.
- U maakt u een back-up van virtuele machines met beheerde schijven op dezelfde manier als andere Azure-VM te doen. U kunt back-up van de virtuele machine rechtstreeks vanuit de instellingen van de virtuele machine, of u kunt back-up inschakelen voor virtuele machines in de Recovery Services-kluis.
- U kunt back-ups maken van virtuele machines op beheerde schijven via RestorePoint-verzamelingen die zijn gebouwd boven op beheerde schijven.
- Azure Backup biedt ook ondersteuning voor back-ups van virtuele machines die zijn versleuteld met behulp van Azure Disk encryption (ADE) op beheerde schijven.

Wanneer u virtuele machines met beheerde schijven herstelt, kunt u herstellen naar een volledige VM met beheerde schijven, of naar een opslagaccount.
- Azure regelt de beheerde schijven tijdens het herstelproces en met de optie voor het opslagaccount, die u beheert het opslagaccount dat gemaakt tijdens het terugzetten.
- Als u een beheerde virtuele machine die versleuteld herstelt, moeten de VM-sleutels en geheimen afsluiten in de key vault voordat u begint met het herstelproces.




## <a name="next-steps"></a>Volgende stappen

- [Beoordeling](backup-support-matrix.md) de ondersteuningsmatrix voor meer informatie over ondersteunde functies en beperkingen voor back-upscenario's.
- Instellen van back-up voor een van de scenario's:
    - [Back-ups maken van Azure-VM's](backup-azure-arm-vms-prepare.md)
    - [Rechtstreeks een back-up maken van Windows-machines](tutorial-backup-windows-server-to-azure.md), zonder een back-upserver.
    - [MABS instellen](backup-azure-microsoft-azure-backup.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar MABS maken.
    - [DPM instellen](backup-azure-dpm-introduction.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar DPM maken.


[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png


---
title: Ondersteuningsmatrix voor Azure back-up voor Azure VM backup
description: Bevat een samenvatting van instellingen voor de ondersteuning en beperkingen bij het back-ups van virtuele Azure-machines met de Azure Backup-service.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: 2bd499c3afc423047dda9ff3ad657d110dab282a
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673644"
---
# <a name="support-matrix-for-azure-vm-backup"></a>Ondersteuningsmatrix voor Azure VM backup
U kunt de [Azure Backup-service](backup-overview.md) back-up on-premises computers en werkbelastingen en virtuele Azure-machines. In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen bij het back-ups van Azure virtual machines (VM's) met Azure Backup.

Andere matrices ondersteuning:

- [Ondersteuningsmatrix voor algemene](backup-support-matrix.md) voor Azure Backkup
- [Ondersteuningsmatrix voor](backup-support-matrix-mabs-dpm.md) voor Microsoft Azure Backup server/DOM back-up
- [Ondersteuningsmatrix voor](backup-support-matrix-mars-agent.md) voor back-up met de MARS-agent



## <a name="supported-scenarios"></a>Ondersteunde scenario's

Hier volgt hoe u een back-up en herstellen van virtuele Azure-machines met de Azure Backup-service.


**Scenario** | **Een back-up maken** | **Agent** |**Herstellen** 
--- | --- | --- | ---
**Directe back-ups van virtuele Azure-machines** | Back-up van de hele virtuele machine  | Geen agent is vereist op de virtuele machine van Azure. Azure Backup wordt geïnstalleerd en maakt gebruik van een uitbreiding van de [Azure VM-agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) die worden uitgevoerd op de virtuele machine. | Als volgt herstellen:<br/><br/> - **Een basis-VM maken**. Dit is handig als de virtuele machine geen speciale configuratie zoals meerdere IP-adressen heeft.<br/><br/> - **De VM-schijf herstellen**. Herstel de schijf. Vervolgens koppelt u deze aan een bestaande virtuele machine of een nieuwe virtuele machine maken van de schijf met behulp van PowerShell.<br/><br/> - **VM-schijf vervangen**. Als een virtuele machine bestaat en maakt gebruik van beheerde schijven (niet-versleutelde), kunt u een schijf herstellen en deze gebruiken om te vervangen door een bestaande schijf op de virtuele machine.<br/><br/> - **Specifieke bestanden/mappen herstellen**. U kunt bestanden/mappen herstellen van een virtuele machine, in plaats van de hele virtuele machine.  
**Directe back-ups van virtuele Azure-machines (alleen Windows)** | Back-up van bepaalde bestanden/mappen/volume | Installeer de [Microsoft Azure Recovery Services agent (MARS)](backup-azure-file-folder-backup-faq.md).<br/><br/> U kunt de MARS-agent samen met de Backup-extensie voor de Azure VM-agent back-up van de virtuele machine op het niveau van bestanden/mappen kunt uitvoeren. | Specifieke mappen/bestanden herstellen.
**Back-up van virtuele Azure-machines naar back-upserver** |  Back-up van bestanden/mappen/volumes. systeemstatus/bare metal systeembestanden. App-gegevens naar System Center DPM of Microsoft Azure Backup Server (Microsoft Azure Backup server).<br/><br/> DPM/MABS vervolgens back-ups van naar de back-upkluis | Installeer de MABS/DPM-beveiligingsagent op de virtuele machine. De MARS-agent is geïnstalleerd op de DPM/MABS.| Herstellen van bestanden/mappen/volumes. systeemstatus/bare metal systeembestanden. App-gegevens. 

Meer informatie over back-up [met behulp van een back-upserver](backup-architecture.md#architecture-back-up-to-dpmmabs), en [ondersteuningsvereisten](backup-support-matrix-mabs-dpm.md).


## <a name="supported-backup-actions"></a>Ondersteunde back-acties

**Actie** | **Ondersteuning**
--- | ---
Back-up inschakelen wanneer u een virtuele Windows Azure-machine maken | Ondersteund voor:  Windows Server 2016 (Datacenter/Datacenter Core); Windows Server 2012 R2 DataCenter; Windows Server 2008 R2 SP1
Back-up inschakelen wanneer u een Linux-VM maken | Ondersteund voor:<br/><br/> - Ubuntu Server: 1710, 1704, 1604 (LTS), 1404 (LTS)<br/><br/> -Red Hat: RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4<br/><br/> - SUSE Linux Enterprise Server: 11 SP4, 12 SP2, 12 SP3<br/><br/> -Debian: 8, 9<br/><br/> - CentOS: 6.9, 7.3<br/><br/> Oracle Linux: 6.7, 6.8, 6.9, 7.2, 7.3
Back-up van een virtuele machine die VM afsluiten/offline/om te zoeken | Ondersteund.<br/><br/> Is een crash-consistente momentopname alleen, niet toepassingsconsistent is.
Back-up schijven na de migratie naar schijven beheerde. | Ondersteund.<br/><br/> Back-ups blijven werken. Geen actie vereist.
Back-up van beheerde schijven na het inschakelen van de groep resourcevergrendeling | Wordt niet ondersteund.<br/><br/> Back-up van de oudere herstelpunten voor de resource niet verwijderen en back-ups mislukken na het verstrijken van de maximumlimiet van herstelpunten wordt gestart.
Back-upbeleid voor een virtuele machine wijzigen | Ondersteund.<br/><br/> De virtuele machine back-ups met behulp van het schema en de retentie-instellingen in het nieuwe beleid. Als de retentie-instellingen worden uitgebreid, worden de bestaande herstelpunten gemarkeerd en bewaard. Als ze worden beperkt, worden bestaande herstelpunten verwijderd in de eerstvolgende opschoontaak, en uiteindelijk verwijderd.
Een back-uptaak annuleren | Ondersteund tijdens het verwerken van de momentopname.<br/><br/> Niet ondersteund wanneer de momentopname worden overgebracht naar de kluis. 
Back-up van de virtuele machine naar een andere regio of abonnement |  Wordt niet ondersteund.
Back-ups per dag (via de Azure VM-extensie) | Een geplande back-up per dag.<br/><br/> U kunt maximaal vier on-demand back-ups per dag op te nemen.
Back-ups per dag (via de MARS-agent) | Drie geplande back-ups per dag. 
Back-ups per dag (via DPM/MABS) | Twee geplande back-ups per dag.
Maandelijkse/jaarlijkse back-up   | Niet ondersteund wanneer de back-ups maken met Azure VM-extensie. Alleen wordt dagelijkse en wekelijkse ondersteund.<br/><br/> U kunt het beleid zo instellen dat er dagelijks/wekelijks back-ups behouden voor de bewaarperiode van de maandelijkse/jaarlijkse.
Automatische clock correctie | Wordt niet ondersteund.<br/><br/> Azure Backup biedt geen automatische aanpassing van de klok voor de zomer-en wintertijd wijzigingen in aanmerking nemen wanneer back-ups van virtuele machine.<br/><br/>  Het beleid zo nodig handmatig wijzigen.
[Beveiligingsfuncties voor hybride back-up](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) |  Uitschakelen van de functies wordt niet ondersteund.



## <a name="operating-system-support-windows"></a>Ondersteuning voor besturingssysteem (Windows)

De tabel bevat een overzicht van de ondersteunde besturingssystemen wanneer back-ups van Windows Azure-VM's.

**Scenario** | **Ondersteuning voor het besturingssysteem** 
--- | ---
Back-up maken met Azure VM agent-extensie | Windows-Client: Niet ondersteund<br/><br/> Windows Server: Windows Server 2008 R2 of hoger 
Back-up maken met de MARS-agent | [Ondersteund](backup-support-matrix-mars-agent.md#support-for-direct-backups) besturingssystemen.
Back-up via DPM/MABS | Ondersteunde besturingssystemen voor back-up met [MABS](backup-mabs-protection-matrix.md) en [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807).

## <a name="support-for-linux-backup"></a>Ondersteuning voor Linux back-up

Dit wordt ondersteund als u een back-up wilt maken van Linux-machines.

**Actie** | **Ondersteuning**
--- | --- 
Back-up van Linux Azure-VM's met de Linux Azure VM-agent | Consistente back-up.<br/><br/> App-consistente back-up met behulp van [aangepaste scripts](backup-azure-linux-app-consistent.md).<br/><br/> Tijdens het terugzetten kunt u een nieuwe virtuele machine maken, een schijf herstellen en gebruiken om te maken van een virtuele machine, of een schijf herstellen en deze gebruiken om te vervangen door een schijf op een bestaande virtuele machine. U kunt ook afzonderlijke bestanden en mappen herstellen.
Back-up van Linux Azure-VM's met de MARS-agent | Wordt niet ondersteund.<br/><br/> De MARS-agent kan alleen worden geïnstalleerd op Windows-machines.
Back-up van Linux Azure virtuele machines met DPM/MABS | Wordt niet ondersteund.

## <a name="operating-system-support-linux"></a>Ondersteuning voor besturingssysteem (Linux)

Voor back-ups van Azure virtuele machine Linux, Azure Backup biedt ondersteuning voor de lijst met Linux [distributies die zijn goedgekeurd door Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). Houd rekening met het volgende:

- Azure Backup biedt geen ondersteuning voor Core OS Linux.
- Azure Backup biedt geen ondersteuning voor 32-bits besturingssystemen.
- Andere bring-your-own Linux-distributies werken mogelijk, zolang de [Azure VM-agent voor Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) is beschikbaar op de virtuele machine, en ondersteuning biedt voor Python.



## <a name="backup-frequency-and-retention"></a>Back-upfrequentie en bewaartermijn

**Instelling** | **Limieten** 
--- | --- 
Maximumaantal herstelpunten per beveiligd exemplaar (machine/werkbelasting | 9999
De maximale vervaltijd voor een herstelpunt | Geen limiet
Maximale frequentie van back-up naar de kluis (Azure VM-extensie) | Eenmaal per dag
Maximale frequentie van back-up naar de kluis (MARS-agent) | Drie back-ups per dag.
Maximale back-upfrequentie naar DPM/MABS | Om de 15 minuten voor SQL Server<br/><br/> Eenmaal per uur voor andere workloads.
Bewaarperiode van herstelpunt | Dagelijks, wekelijks, maandelijks, jaarlijks.
Maximale bewaarperiode | Afhankelijk van back-upfrequentie.
Herstelpunten op DPM-/MABS-schijf | 64 voor bestandsservers, 448 voor appservers.<br/><br/> Tapeherstelpunten zijn onbeperkt voor on-premises DPM.


## <a name="supported-restore-methods"></a>Ondersteunde methoden

**Methode Restore** | **Details**
--- | ---
**Een nieuwe virtuele machine maken** | U kunt een virtuele machine maken tijdens het herstelproces. <br/><br/> Deze optie wordt een eenvoudige virtuele machine actief en werkend. U kunt de naam van virtuele machine, de resourcegroep, virtueel netwerk, subnet en het opslag opgeven.  
**Een schijf herstellen** | U kunt een schijf herstellen en een virtuele machine maken<br/><br/> Wanneer u deze optie selecteert, kopieert Azure back-up gegevens uit de kluis naar een opslagaccount dat u selecteert. De hersteltaak wordt een sjabloon gegenereerd. U kunt deze sjabloon downloaden en deze gebruiken om aangepaste instellingen van de virtuele machine en een virtuele machine maken.<br/><br/> Deze optie kunt u meer instellingen opgeven die de vorige optie voor het maken van een virtuele machine.<br/><br/>
**Een bestaande schijf vervangen** | U kunt een schijf herstellen en vervolgens met de herstelde schijf een schijf die zich momenteel op een virtuele machine vervangen.
**Bestanden herstellen** | U kunt bestanden herstellen van een geselecteerd herstelpunt. U downloaden een script voor het koppelen van de VM-schijf van het herstelpunt dat. U bladert u door middel van de schijfvolumes te vinden van de bestanden/mappen die u wilt herstellen en de schijf ontkoppelen wanneer u klaar bent.

## <a name="support-for-file-level-restore"></a>Ondersteuning voor herstel op bestandsniveau

**Herstellen** | **Ondersteund**
--- | --- 
Herstellen van bestanden tussen besturingssystemen | U kunt bestanden op elke computer die is van het besturingssysteem hetzelfde (of compatibel) als de VM een back-up herstellen. Zie de [compatibel besturingssysteem tabel](backup-azure-restore-files-from-vm.md#system-requirements).
Herstellen van bestanden op klassieke virtuele machines | Wordt niet ondersteund. 
Bestanden herstellen van versleutelde virtuele machines | Wordt niet ondersteund.
Herstellen van bestanden in opslagaccounts met netwerkbeperkingen | Wordt niet ondersteund.
Herstellen van bestanden op virtuele machines met behulp van Windows-opslagruimten | Terugzetten wordt niet ondersteund op dezelfde virtuele machine.<br/><br/> In plaats daarvan de bestanden op een compatibele virtuele machine herstellen.
Bestanden herstellen voor Linux-VM met LVM/raid-matrices | Terugzetten wordt niet ondersteund op dezelfde virtuele machine.<br/><br/> Herstellen op een compatibele virtuele machine.
Bestanden terugzetten met speciale netwerkinstellingen | Terugzetten wordt niet ondersteund op dezelfde virtuele machine. <br/><br/> Herstellen op een compatibele virtuele machine.

## <a name="support-for-vm-management"></a>Ondersteuning voor VM-beheer

De tabel bevat een overzicht van ondersteuning voor back-up tijdens de VM-beheertaken, zoals het toevoegen of vervangen van VM-schijven.

**Herstellen** | **Ondersteund** 
--- | --- 
Herstellen in de regio-abonnement-zone. | Niet ondersteund 
Herstellen naar een bestaande virtuele machine | Gebruik de optie schijf vervangen.
Schijf herstellen met de storage-account voor Azure Storage Service Encryption (SSE) ingeschakeld | Wordt niet ondersteund.<br/><br/> Herstellen naar een account dat beschikt niet over SSE ingeschakeld.
Herstellen naar gemengde storage-accounts | Wordt niet ondersteund.<br/><br/> Op basis van het opslagaccounttype, wordt alle herstelde schijven worden premium of standard en geen gemengde. 
Herstellen naar een opslagaccount met behulp van ZRS | Niet ondersteund
VM herstellen rechtstreeks aan een beschikbaarheidsset | Voor beheerde schijven, kunt u de schijf herstellen en de beschikbaarheid van set-optie gebruiken in de sjabloon.<br/><br/> Niet ondersteund voor niet-beheerde schijven. Voor niet-beheerde schijven, de schijf herstellen en vervolgens een virtuele machine maken in de beschikbaarheidsset.
Back-up van niet-beheerde virtuele machines herstellen na een upgrade naar beheerde | Ondersteund.<br/><br/> U kunt schijven herstellen en vervolgens een beheerde virtuele machine te maken.
Virtuele machine voor het herstellen van punt voordat de virtuele machine is gemigreerd naar beheerde schijven terugzetten | Ondersteund.<br/><br/> U herstellen naar niet-beheerde schijven (standaard), de herstelde schijven converteren naar beheerde schijf en een virtuele machine maken met de beheerde schijven.
Herstellen van een virtuele machine die wordt verwijderd. | Ondersteund.<br/><br/> U kunt de virtuele machine herstellen vanaf een herstelpunt.
Herstellen van een domeincontroller (DC) virtuele machine die deel uitmaakt van de configuratie van een multi-DC via de portal | Als u de schijf herstellen en maakt een virtuele machine met behulp van PowerShell ondersteund.
Herstellen van de virtuele machine in een ander VNet |  Ondersteund.<br/><br/> Het VNet moet zich in hetzelfde abonnement en regio.




## <a name="vm-compute-support"></a>VM-compute-ondersteuning

**Compute** | **Ondersteuning** 
--- | --- 
VM-grootte |   Elke Azure-VM-grootte met ten minste 2 CPU-kernen en 1 GB RAM-geheugen.<br/><br/> [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) 
Maak een back-up van virtuele machines in [beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) | Ondersteund.<br/><br/> U kunt een virtuele machine in een beschikbare groep met de optie voor het snel maken van een virtuele machine niet terugzetten. In plaats daarvan wanneer u de virtuele machine herstellen moet u ofwel de schijf herstellen en voor het implementeren van een virtuele machine, of een schijf herstellen en deze gebruiken om te vervangen door een bestaande schijf. 
Maak een back-up van virtuele machines in [beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview) |  Wordt niet ondersteund.  
Maak een back-up van virtuele machines die worden geïmplementeerd met [Hybrid Use Benefit (HUB)](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Ondersteund.
Maak een back-up van virtuele machines die worden geïmplementeerd een [schaalset](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) |   Niet ondersteund   
Maak een back-up van virtuele machines die worden geïmplementeerd vanaf de [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=virtual-machine-images)<br/><br/> (Gepubliceerd door Microsoft, van derden) |   Ondersteund.<br/><br/> De virtuele machine moet worden uitgevoerd als een ondersteund besturingssysteem.<br/><br/> Tijdens het herstellen van bestanden op de virtuele machine, kunt u alleen naar een compatibel besturingssysteem (niet een besturingssysteem ouder of hoger) herstellen.
Back-up van virtuele machines die door een aangepaste installatiekopie (derde) |    Ondersteund.<br/><br/> De virtuele machine moet worden uitgevoerd als een ondersteund besturingssysteem.<br/><br/> Tijdens het herstellen van bestanden op de virtuele machine, kunt u alleen naar een compatibel besturingssysteem (niet een besturingssysteem ouder of hoger) herstellen.
Maak een back-up van virtuele machines gemigreerd naar Azure   | Ondersteund.<br/><br/> Als u wilt back-up van de virtuele machine, moet de VM-agent worden geïnstalleerd op de gemigreerde machine. 



## <a name="vm-storage-support"></a>Ondersteuning voor VM-opslag

**Onderdeel** | **Ondersteuning**
--- | ---
Azure VM-gegevensschijven | Back-up van een virtuele machine met 16 of minder gegevensschijven.
Grootte van de gegevensschijf | Afzonderlijke schijf mag maximaal 4095 GB zijn.<br/><br/> Als u de nieuwste versie van Azure VM backup (bekend als Instant Restore) uitvoert, tot 4TB het formaat van schijf worden ondersteund. [Meer informatie](backup-instant-restore-capability.md).
Opslagtype | Standaard harde schijven, standard-SSD, premium SSD <br/><br/> Standard-SSD wordt ondersteund als u de nieuwste versie van Azure VM backup (bekend als Instant Restore) uitvoert, standard-SSD wordt ondersteund. [Meer informatie](backup-instant-restore-capability.md).
Managed Disks | Ondersteund
Versleutelde schijven | Ondersteund.<br/><br/> Azure VM's die zijn ingeschakeld met Azure Disk Encryption (ADE) kunnen worden back-ups (met of zonder de Azure AD-app).<br/><br/> Versleutelde virtuele machines kunnen niet worden hersteld op het niveau van het bestand/map. U moet de volledige virtuele machine herstellen.<br/><br/> U kunt versleuteling op virtuele machines die al worden beveiligd door Azure Backup inschakelen.
Schijven met Write Accelerator is ingeschakeld | Wordt niet ondersteund.<br/><br/> Als u de nieuwste versie van Azure-VM back-up uitvoert (ook wel [direct herstellen](backup-instant-restore-capability.md)), kunt u schijven uitsluiten met Write Accelerator is ingeschakeld vanaf back-up.
Back-up van ontdubbelde schijven | Wordt niet ondersteund.
Schijf toevoegen aan beveiligde virtuele machine | Ondersteund.
Grootte van de schijf op beveiligde virtuele machine wijzigen | Ondersteund.

## <a name="vm-network-support"></a>Ondersteuning voor VM-netwerken


**Onderdeel** | **Ondersteuning**
--- | ---
Aantal NIC 's | Het maximumaantal NIC's ondersteund voor een specifieke Azure-VM-grootte.<br/><br/> NIC's worden gemaakt wanneer de virtuele machine is gemaakt tijdens het herstelproces.<br/><br/> Het aantal NIC's op de herstelde virtuele machine komt overeen met het aantal NIC's op de virtuele machine wanneer u beveiliging ingeschakeld. NIC's verwijderen na het inschakelen van heeft geen invloed op het aantal.
Externe/interne load balancer |   Ondersteund. <br/></br> [Meer informatie](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) over het herstellen van virtuele machines met speciale netwerkinstellingen.
Meerdere gereserveerde IP-adres |  Ondersteund. <br/></br> [Meer informatie](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) over het herstellen van virtuele machines met speciale netwerkinstellingen.
Virtuele machines met meerdere netwerkadapters  | Ondersteund. <br/></br> [Meer informatie](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) over het herstellen van virtuele machines met speciale netwerkinstellingen.
Virtuele machines met openbare IP-adressen    | Ondersteund.<br/><br/> U moet een bestaande openbare IP-adres koppelen aan de NIC, of een adres maakt en deze koppelen aan de NIC nadat het herstel is voltooid.
Netwerkbeveiligingsgroep (NSG) op de NIC/subnet. |   Ondersteund.
Gereserveerde IP-adressen (statisch) | Niet ondersteund.<br/><br/> U kunt geen back-up van een virtuele machine met een gereserveerd IP-adres en geen gedefinieerde eindpunt.
Dynamisch IP-adres |    Ondersteund.<br/><br/> Als de NIC op de bron-VM gebruikmaakt van dynamische IP-adressering, standaard wordt de NIC op de herstelde virtuele machine ook.
Traffic Manager | Ondersteund<br/><br/> Als de back-ups van virtuele machine in Traffic Manager wordt, moet u handmatig de herstelde virtuele machine toevoegen aan de dezelfde Traffic Manager. 
Azure DNS | Ondersteund.
Aangepaste DNS |    Ondersteund.
Uitgaande connectiviteit via een HTTP-proxy | Ondersteund.<br/><br/> Een geverifieerde proxyserver wordt niet ondersteund. 
VNet-service-eindpunten  | Ondersteund.<br/><br/> Firewall- en VNet opslagaccountinstellingen moeten toegang toestaan via alle netwerken. 



## <a name="vm-securityencryption-support"></a>Ondersteuning voor VM-beveiliging/versleuteling

Azure Backup ondersteunt versleuteling voor in-transit- en inactieve gegevens:

Netwerkverkeer naar Azure:
- Back-upverkeer van servers naar de Recovery Services-kluis wordt versleuteld met Advanced Encryption Standard 256.
- Back-upgegevens worden verzonden via een beveiligde HTTPS-koppeling.
- De back-upgegevens worden versleuteld opgeslagen in de Recovery Services-kluis.
- Alleen u beschikt over de wachtwoordzin waarmee deze gegevens kunnen worden ontgrendeld. De back-upgegevens kunnen niet door Microsoft ontsleuteld.
    > [!WARNING]
    > Nadat u de kluis hebt ingesteld, hebt alleen u toegang tot de versleutelingssleutel. Microsoft bewaart nooit een kopie en heeft geen toegang tot de sleutel. Als de sleutel verkeerd wordt geplaatst, kan Microsoft de back-upgegevens niet herstellen.
Gegevensbeveiliging:
- Als u back-ups van virtuele machines van Azure wilt maken, moet u de versleuteling configureren *binnen* de virtuele machine.
- Azure Backup biedt ondersteuning voor Azure Disk Encryption, dat gebruikmaakt van BitLocker op virtuele Windows-machines en **dm-crypt** op virtuele Linux-machines.
- Op de backend maakt Azure Backup gebruik van [Azure Storage Service-versleuteling](../storage/common/storage-service-encryption.md), waarmee data-at-rest worden beschermd.


**Machine** | **In-transit** | **Inactief**
--- | --- | ---
On-premises Windows-machines zonder DPM/MABS | ![Ja][green] | ![Ja][green]
Azure-VM's | ![Ja][green] | ![Ja][green] 
On-premises/Azure VM's met DPM | ![Ja][green] | ![Ja][green]
On-premises/Azure VM's met MABS | ![Ja][green] | ![Ja][green]



## <a name="vm-compression-support"></a>Ondersteuning voor VM-compressie

Backup biedt ondersteuning voor de compressie van back-upverkeer, zoals samengevat in de onderstaande tabel. Houd rekening met het volgende:

- Voor Azure VM's leest de VM-extensie de gegevens rechtstreeks vanuit het Azure Storage-account via het opslagnetwerk. Dit verkeer hoeft daarom niet te worden gecomprimeerd.
- Als u DPM of MABS gebruikt, kunt u de gegevens comprimeren voordat er een back-up van wordt gemaakt in DPM/MABS, om bandbreedte te besparen. 

**Machine** | **Comprimeren naar MABS/DPM (TCP)** | **Comprimeren (HTTPS) naar kluis**
--- | --- | ---
On-premises Windows-machines zonder DPM/MABS | N.v.t. | Ja
Azure-VM's | N.v.t. | N.v.t.
On-premises/Azure VM's met DPM | ![Ja][green] | ![Ja][green]
On-premises/Azure VM's met MABS | ![Ja][green] | ![Ja][green]




## <a name="next-steps"></a>Volgende stappen

- [Back-ups maken van Azure-VM's](backup-azure-arm-vms-prepare.md)
- [Rechtstreeks een back-up maken van Windows-machines](tutorial-backup-windows-server-to-azure.md), zonder een back-upserver.
- [MABS instellen](backup-azure-microsoft-azure-backup.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar MABS maken.
- [DPM instellen](backup-azure-dpm-introduction.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar DPM maken.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png


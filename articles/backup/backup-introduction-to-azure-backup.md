---
title: Wat is Azure Backup? | Microsoft Docs
description: Met Azure Backup kunt u back-ups van gegevens en workloads maken en deze herstellen vanaf Windows-servers en -werkstations, System Center DPM-servers en virtuele Azure-machines.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up en herstel; Recovery Services; back-upoplossingen
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 1/5/2018
ms.author: markgal;trinadhk;anuragm
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 59beaac1d8619c3f4afa1c75074546a849dfce6b
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="overview-of-the-features-in-azure-backup"></a>Overzicht van de functies in Azure Backup
Azure Backup is de Azure-service die u kunt gebruiken voor het maken van back-ups en het herstellen van uw gegevens in de Microsoft-cloud (of deze te beschermen). Met Azure Backup vervangt u uw bestaande on-premises of off-site back-upoplossing door een betrouwbare, veilige en kostenbesparende cloudoplossing. Azure Backup biedt meerdere onderdelen die u kunt downloaden en implementeren op de desbetreffende computer, server, of in de cloud. Welk onderdeel, of welke agent, u implementeert, is afhankelijk van wat u wilt beveiligen. Alle onderdelen van Azure Backup (ongeacht of u gegevens on-premises of in de cloud wilt beveiligen) kunnen worden gebruikt om back-ups te maken naar een Recovery Services-kluis in Azure. Zie de [Azure Backup onderdelentabel](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (verderop in dit artikel) voor informatie over welk onderdeel moet worden gebruikt om specifieke gegevens, toepassingen of workloads te beschermen.

[Een video-overzicht van Azure Backup bekijken](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Waarom Azure Backup gebruiken?
Traditionele back-upoplossingen gebruiken de cloud als een eindpunt, of statische opslaglocatie, die vergelijkbaar is met schijven of bandgeheugen. Hoewel dit een eenvoudig benadering is, is deze beperkt en wordt hiermee niet volledig geprofiteerd van een onderliggend cloudplatform. Hierdoor is het een dure, inefficiënt oplossing. Andere oplossingen zijn duur omdat u betaalt voor het verkeerde type opslag of voor opslag die u niet nodig hebt. Andere oplossingen zijn vaak inefficiënt omdat ze niet het type of de hoeveelheid opslagruimte bieden die u nodig hebt, of omdat administratieve taken te veel tijd in beslag nemen. Azure Backup biedt daarentegen deze belangrijke voordelen:

**Automatisch opslagbeheer**: voor hybride omgevingen is vaak heterogene opslag vereist, soms on-premises en soms in de cloud. Met Azure Backup zijn er geen kosten voor het gebruik van on-premises opslagapparaten. De back-upopslag wordt automatisch door Azure Backup toegewezen en beheerd en u betaalt naar gebruik. U betaalt dus alleen voor de opslag die u daadwerkelijk gebruikt. Zie het artikel [Azure pricing](https://azure.microsoft.com/pricing/details/backup) (Azure-prijzen) voor meer informatie.

**Onbeperkt schalen**: Azure Backup gebruikt de onderliggende kracht en onbeperkte schaal van de Azure-cloud voor hoge beschikbaarheid, zonder overhead voor onderhoud of bewaking. U kunt waarschuwingen instellen voor het weergeven van informatie over gebeurtenissen, maar u hoeft zich geen zorgen te maken over hoge beschikbaarheid van uw gegevens in de cloud.

**Meerdere opslagopties**: een aspect van hoge beschikbaarheid is opslagreplicatie. Azure Backup biedt twee typen replicatie: [lokaal redundante opslag](../storage/common/storage-redundancy.md#locally-redundant-storage) en [geografisch redundante opslag](../storage/common/storage-redundancy.md#geo-redundant-storage). Kies de optie back-upopslag op basis van uw behoeften:

* Met lokaal redundante opslag (Locally Redundant Storage, LRS) worden uw gegevens drie keer gerepliceerd (er worden drie kopieën gemaakt van uw gegevens) in een gekoppeld datacenter in dezelfde regio. LRS is een goedkope optie voor het beschermen van uw gegevens tegen lokale hardwarefouten.

* Met geografisch redundante opslag (GRS) worden uw gegevens gerepliceerd naar een secundaire regio (honderden kilometers verwijderd van de primaire locatie van de brongegevens). GRS is duurder dan LRS, maar biedt een hoger duurzaamheidsniveau voor uw gegevens, zelfs in geval van een regionale onderbreking.

**Onbeperkt gegevensoverdracht**: Azure Backup stelt geen beperking aan de hoeveelheid binnenkomende of uitgaande gegeven die u overbrengt. In Azure Backup wordt gegevensoverdracht niet in rekening gebracht. Als u de Azure Import/Exportservice gebruikt voor het importeren van grote hoeveelheden gegevens, zijn er kosten verbonden aan inkomende gegevens. Zie [Offline back-upworkflow in Azure Backup](backup-azure-backup-import-export.md) (Offline back-upwerkstroom in Azure Backup) voor meer informatie over deze kosten. Uitgaande gegevens zijn gegevens die tijdens een herstelbewerking worden overgebracht uit een Recovery Services-kluis.

**Gegevensversleuteling**: gegevensversleuteling zorgt voor een beveiligde overdracht en opslag van uw gegevens in de openbare cloud. U slaat de wachtwoordzin voor versleuteling lokaal op. Deze wordt nooit verzonden naar of opgeslagen in Azure. Als het herstellen van gegevens noodzakelijk is, bent u de enige met de wachtwoordzin voor versleuteling of de sleutel.

**Toepassingsconsistente back-up**: of u nu back-ups maakt van een bestandsserver, een virtuele machine of een SQL-database, u moet weten dat een herstelpunt over alle vereiste gegevens beschikt om de back-up te herstellen. Azure Backup biedt toepassingsconsistente back-ups, om ervoor te zorgen dat er geen aanvullende correcties nodig zijn om de gegevens te herstellen. Herstellen van toepassingsconsistente gegevens verkort de hersteltijd, zodat u snel weer normaal aan het werk kunt.

**Langdurige opslag**: in plaats van het overzetten van back-ups van schijf naar tape en deze te verplaatsen naar een externe locatie, kunt u Azure gebruiken voor zowel kortdurende als langdurige opslag. Azure heeft geen beperkingen voor hoelang gegevens bewaard blijven in een Backup- of Recovery Services-kluis. U kunt gegevens zo lang als u wilt in een kluis bewaren. Azure Backup heeft een limiet van 9999 herstelpunten per beveiligd exemplaar. Zie het gedeelte [Back-ups en retentie](backup-introduction-to-azure-backup.md#backup-and-retention) in dit artikel voor meer informatie over hoe deze limiet invloed kan hebben op uw back-upbehoeften.  

## <a name="which-azure-backup-components-should-i-use"></a>Welke Azure Backup-onderdelen moet ik gebruiken?
Als u niet zeker weet welke Azure Backup-onderdelen geschikt zijn voor uw behoeften, raadpleegt u de volgende tabel voor informatie over wat u met elk onderdeel kunt beveiligen. Azure Portal bevat een ingebouwde wizard, om u te helpen bij het kiezen van de onderdelen die u kunt downloaden en implementeren. De wizard, die deel uitmaakt van de Recovery Services-kluis, leidt u door de stappen voor het selecteren van een back-uplocatie en het kiezen van de gegevens of de toepassing die u wilt beveiligen.

| Onderdeel | Voordelen | Limieten | Wat wordt er beveiligd? | Waar worden de back-ups opgeslagen? |
| --- | --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<li>Back-ups maken van bestanden en mappen op een fysiek of virtueel Windows-besturingssysteem (VM's kunnen zich on-premises of in Azure bevinden)<li>Geen afzonderlijk back-upserver vereist. |<li>Drie keer per dag een back-up maken <li>Niet toepassingsbewust; alleen herstelbewerkingen op bestands-, map- of volumeniveau, <li>  Geen ondersteuning voor Linux. |<li>Bestanden, <li>Mappen, <li>Systeemstatus |Recovery Services-kluis |
| System Center DPM |<li>App-gerichte momentopnamen (VSS)<li>Volledige flexibiliteit met betrekking tot het moment waarop u back-ups wilt maken<li>Herstelgranulariteit (alles)<li>Kan Recovery Services-kluis gebruiken<li>Linux Support op virtuele Hyper-V- en VMware-machines <li>Back-ups maken van virtuele VMware-machines en deze herstellen met DPM 2012 R2 |Kan geen back-up maken van Oracle-workload.|<li>Bestanden, <li>Mappen,<li> Volumes, <li>VM's,<li> Toepassingen,<li> Workloads |<li>Recovery Services-kluis,<li> Lokaal gekoppelde schijf,<li>  Tape (alleen on-premises) |
| Azure Backup-server |<li>App-gerichte momentopnamen (VSS)<li>Volledige flexibiliteit met betrekking tot het moment waarop u back-ups wilt maken<li>Herstelgranulariteit (alles)<li>Kan Recovery Services-kluis gebruiken<li>Linux Support op virtuele Hyper-V- en VMware-machines<li>Back-ups maken van virtuele VMware-machines en deze herstellen <li>Vereist geen System Center-licentie |<li>Kan geen back-up maken van Oracle-workload.<li>Altijd een live Azure-abonnement nodig<li>Geen ondersteuning voor tape met back-up |<li>Bestanden, <li>Mappen,<li> Volumes, <li>VM's,<li> Toepassingen,<li> Workloads |<li>Recovery Services-kluis,<li> Lokaal gekoppelde schijf |
| Back-up van virtuele machines van Azure IaaS |<li>Systeemeigen back-ups voor Windows/Linux<li>Er zijn geen specifieke agentinstallatie vereist<li>Back-ups op infrastructuurniveau zonder dat er een back-upinfrastructuur nodig is |<li>Eens per dag back-ups maken van VM's <li>VM's alleen op schijfniveau herstellen<li>Geen back-up on-premises |<li>VM's, <li>Alle schijven (met PowerShell) |<p>Recovery Services-kluis</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Wat zijn de implementatiescenario's voor elk onderdeel?
| Onderdeel | Kan worden geïmplementeerd in Azure? | Kan on-premises worden geïmplementeerd? | Doelopslag ondersteund |
| --- | --- | --- | --- |
| Azure Backup-agent (MARS) |<p>**Ja**</p> <p>De Azure Backup-agent kan worden geïmplementeerd op elke virtuele machine van Windows Server die wordt uitgevoerd in Azure.</p> |<p>**Ja**</p> <p>De Azure Backup-agent kan worden geïmplementeerd op elke virtuele machine van Windows Server of fysieke computer.</p> |<p>Recovery Services-kluis</p> |
| System Center DPM |<p>**Ja**</p><p>Meer informatie over [het beveiligen van workloads in Azure met System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ja**</p> <p>Meer informatie over [het beveiligen van workloads en VM's in uw datacenter](https://technet.microsoft.com/system-center-docs/dpm/data-protection-manager).</p> |<p>Lokaal gekoppelde schijf,</p> <p>Recovery Services-kluis,</p> <p>tape (alleen on-premises)</p> |
| Azure Backup-server |<p>**Ja**</p><p>Meer informatie over [het beveiligen van workloads in Azure met een Azure Backup-server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ja**</p> <p>Meer informatie over [het beveiligen van workloads in Azure met een Azure Backup-server](backup-azure-microsoft-azure-backup.md).</p> |<p>Lokaal gekoppelde schijf,</p> <p>Recovery Services-kluis</p> |
| Back-up van virtuele machines van Azure IaaS |<p>**Ja**</p><p>Onderdeel van de Azure-infrastructuur</p><p>Speciaal voor [back-ups van de virtuele machines van Azure IaaS (Infrastructure as a Service)](backup-azure-vms-introduction.md).</p> |<p>**Nee**</p> <p>Gebruik System Center DPM om back-ups van virtuele machines in uw datacenter te maken.</p> |<p>Recovery Services-kluis</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Van welke toepassingen en workloads kan een back-up worden gemaakt?
De volgende tabel bevat een matrix van de gegevens en workloads die kunnen worden beveiligd met Azure Backup. De kolom met Azure Backup oplossingen bevat koppelingen naar de implementatiedocumentatie voor die oplossing. 

| Gegevens of workload | Bronomgeving | Azure Backup-oplossing |
| --- | --- | --- |
| Bestanden en mappen |Windows Server |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Bestanden en mappen |Windows-computer |<p>[Azure Backup-agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Virtuele Hyper-V-machine (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Virtuele Hyper-V-machine (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Virtuele VMware-machine |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ de Azure Backup-agent),</p> <p>[Azure Backup-server](backup-azure-microsoft-azure-backup.md) (bevat de Azure Backup-agent)</p> |
| Virtuele machines van Azure IaaS (Windows) |uitgevoerd in Azure |[Azure Backup (VM-extensie)](backup-azure-vms-introduction.md) |
| Virtuele machines van Azure IaaS (Linux) |uitgevoerd in Azure |[Azure Backup (VM-extensie)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux Support
De volgende tabel bevat de Azure Backup-onderdelen die ondersteuning bieden voor Linux.  

| Onderdeel | Ondersteuning voor Linux (goedgekeurd door Azure) |
| --- | --- |
| Azure Backup-agent (MARS) |Nee (alleen Windows-agent) |
| System Center DPM |<li> Bestandsconsistente back-up van Linux-gast-VM's op Hyper-V en VMware<br/> <li> Linux-gast-VM's herstellen op Hyper-V- en VMware </br> </br>  *Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines* <br/> |
| Azure Backup-server |<li>Bestandsconsistente back-up van Linux-gast-VM's op Hyper-V en VMware<br/> <li> Linux-gast-VM's herstellen op Hyper-V- en VMware </br></br> *Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines*  |
| Back-up van virtuele machines van Azure IaaS |Toepassingsconsistente back-up met [prescript en postscript framework](backup-azure-linux-app-consistent.md)<br/> [Gedetailleerd bestandsherstel](backup-azure-restore-files-from-vm.md)<br/> [Alle VM-schijven herstellen](backup-azure-arm-restore-vms.md#restore-backed-up-disks)<br/> [VM herstellen](backup-azure-arm-restore-vms.md#create-a-new-vm-from-a-restore-point) |

## <a name="using-premium-storage-vms-with-azure-backup"></a>VM's voor Premium Storage met Azure Backup gebruiken
Azure Backup beschermt VM's voor Premium Storage. Azure Premium Storage is opslag op basis van SSD (Solid-State Drive), ontworpen om I/O-intensieve workloads te ondersteunen. Premium Storage is uitermate geschikt voor VM-workloads (virtuele machine). Zie het artikel [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../virtual-machines/windows/premium-storage.md) (Premium Storage: hoogwaardige opslag voor workloads van Azure Virtual Machines) voor meer informatie over Premium Storage.

### <a name="back-up-premium-storage-vms"></a>Een back-up van VM's voor Premium-opslag maken
Tijdens een back-up van virtuele machines voor Premium-opslag, maakt de Backup-service in het Premium-opslagaccount een tijdelijke faseringslocatie met de naam 'AzureBackup-'. De faseringslocatie is even groot als de momentopname van het herstelpunt. Zorg ervoor dat er voldoende vrije ruimte in het Premium Storage-account is voor de tijdelijke faseringslocatie. Zie het artikel [Beperkingen van Premium Storage](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets) voor meer informatie. Zodra de back-uptaak is voltooid, wordt de faseringslocatie verwijderd. De opslagkosten die in rekening worden gebracht voor de faseringslocatie zijn consistent met de [Prijzen voor een Premium-opslag](../virtual-machines/windows/premium-storage.md#pricing-and-billing).

> [!NOTE]
> Wijzig of bewerk de faseringslocatie niet.
>
>

### <a name="restore-premium-storage-vms"></a>VM's voor Premium-opslag herstellen
VM's voor Premium Storage kunnen worden hersteld naar de Premium Storage of de normale opslag. De gebruikelijke herstelprocedure voor een VM voor Premium-opslag is om het herstelpunt te herstellen naar Premium-opslag. Het kan echter rendabel zijn om een herstelpunt voor een VM voor Premium-opslag te herstellen naar de standaardopslag. Dit type herstel kan worden gebruikt als u een subset bestanden van de VM nodig hebt.

## <a name="using-managed-disk-vms-with-azure-backup"></a>Virtuele machines op beheerde schijven gebruiken met Azure Backup
Met Azure Backup beveiligt u virtuele machines op beheerde schijven. Dankzij beheerde schijven hoeft u opslagaccounts van virtuele machines niet te beheren en wordt de VM-inrichting sterk vereenvoudigd.

### <a name="back-up-managed-disk-vms"></a>Back-up maken van virtuele machines op beheerde schijven
Het maken van een back-up van virtuele machines op beheerde schijven en in Resource Manager gebeurt op dezelfde manier. In de Azure Portal kunt u de back-uptaak rechtstreeks configureren in de weergave Virtuele machine of in de weergave Recovery Services-kluis. U kunt back-ups maken van virtuele machines op beheerde schijven via RestorePoint-verzamelingen die zijn gebouwd boven op beheerde schijven. Azure Backup biedt ook ondersteuning voor het maken van back-ups van virtuele machines op beheerde schijven die zijn versleuteld met Azure Disk Encryption (ADE).

### <a name="restore-managed-disk-vms"></a>Virtuele machines op beheerde schijven terugzetten
Met Azure Backup kunt u een volledige virtuele machine terugzetten met beheerde schijven, of beheerde schijven terugzetten naar een opslagaccount. Met Azure worden de beheerde schijven beheert tijdens het herstelproces. U (de klant) beheert het opslagaccount dat is gemaakt als onderdeel van het herstelproces. Voor het herstellen van beheerde versleutelde virtuele machines moeten de sleutels en geheimen van de virtuele machine al bestaan in de Key Vault vóór het starten van de herstelbewerking.

## <a name="what-are-the-features-of-each-backup-component"></a>Wat zijn de functies van elk Backup-onderdeel?
De volgende secties bevatten tabellen met een overzicht van de beschikbaarheid van of de ondersteuning voor verschillende functies in elk Azure Backup-onderdeel. Zie de informatie na elke tabel voor aanvullende ondersteuning of details.

### <a name="storage"></a>Storage
| Functie | Azure Backup-agent | System Center DPM | Azure Backup-server | Back-up van virtuele machines van Azure IaaS |
| --- | --- | --- | --- | --- |
| Recovery Services-kluis |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| File Storage | |![Ja][green] |![Ja][green] | |
| Bandgeheugen | |![Ja][green] | | |
| Compressie <br/>(in Recovery Services-kluis) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Incrementele back-up |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Schijfontdubbeling | |![Gedeeltelijk][yellow] |![Gedeeltelijk][yellow] | | |

![tabelsleutel](./media/backup-introduction-to-azure-backup/table-key.png)

De Recovery Services-kluis is de gewenste doelopslag voor alle onderdelen. System Center DPM en Azure Backup Server bieden ook de mogelijkheid voor een kopie op de lokale schijf. Alleen System Center DPM biedt de mogelijkheid om gegevens naar een opslagapparaat met een tape te schrijven.

#### <a name="compression"></a>Compressie
Back-ups worden gecomprimeerd zodat er minder opslagruimte nodig is. Het enige onderdeel dat u geen compressie gebruikt, is de VM-extensie. De VM-extensie kopieert alle back-upgegevens van uw opslagaccount naar de Recovery Services-kluis in dezelfde regio. Er wordt geen compressie toegepast bij het overdragen van de gegevens. Omdat de gegevens zonder compressie worden overgedragen, wordt er iets meer opslag gebruikt. Doordat de gegevens echter niet worden gecomprimeerd, kunnen ze sneller worden hersteld.


#### <a name="disk-deduplication"></a>Schijfontdubbeling
U kunt profiteren van ontdubbeling wanneer u System Center DPM of Azure Backup Server implementeert [op een virtuele Hyper-V-machine](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). Windows Server voert gegevensontdubbeling uit (op hostniveau) op virtuele harde schijven (VHD's) die als back-upopslag zijn gekoppeld aan de virtuele machine.

> [!NOTE]
> Ontdubbeling is niet beschikbaar in Azure voor Backup-onderdelen. Wanneer de System Center DPM en de Bakckup-server zijn geïmplementeerd in Azure, kunnen de opslagschijven die aan de virtuele machine zijn gekoppeld, niet worden ontdubbeld.
>
>

### <a name="incremental-backup-explained"></a>Incrementele back-up uitgelegd
Elk Azure Backup-onderdeel biedt ondersteuning voor incrementele back-ups, ongeacht de doelopslag (schijf, tape, Recovery Services-kluis). Een incrementele back-up zorgt ervoor dat back-ups opslag- en tijdsefficiënt zijn door alleen de wijzigingen sinds de laatste back-up over te dragen.

#### <a name="comparing-full-differential-and-incremental-backup"></a>Vergelijking tussen Volledige, Differentiële en Incrementele back-ups

Opslagverbruik, Beoogde hersteltijd (RTO) en netwerkverbruik varieert voor elke back-upmethode. Als u de TCO (totale eigendomskosten) laag wilt houden, moet u de voor u geschikte back-upoplossing kiezen. In de volgende afbeelding worden de Volgende back-up, Differentiële back-up en Incrementele back-up vergeleken. In de afbeelding bestaat gegevensbron A uit 10 opslagblokken A1-A10, waarvan maandelijks een back-up wordt gemaakt. Blokken A2, A3, A4 en A9 zijn in de eerste maand gewijzigd, en blok A5 is de maand erna gewijzigd.

![afbeelding met vergelijkingen van back-upmethoden](./media/backup-introduction-to-azure-backup/backup-method-comparison.png)

Bij **Volledige back-up** bevat elke back-up de hele gegevensbron. Bij Volledige back-up wordt een grote hoeveelheid netwerkbandbreedte en -opslag verbruikt telkens wanneer een back-upexemplaar wordt overgedragen.

Bij **Differentiële back-up** worden alleen de blokken opgeslagen die zijn gewijzigd sinds de initiële volledige back-up. Dit resulteert in een lager netwerk- en opslagverbruik. Bij differentiële back-ups worden geen redundante exemplaren met ongewijzigde gegevens bewaard. Omdat de gegevensblokken die ongewijzigd blijven tussen opeenvolgende back-ups, worden overgedragen en opgeslagen, zijn differentiële back-ups inefficiënt. In de tweede maand worden er back-ups gemaakt van A2, A3, A4 en A9. In de derde maand wordt er opnieuw een back-up gemaakt van dezelfde blokken en van het gewijzigde blok A5. Van de gewijzigde blokken worden back-ups gemaakt totdat de volgende volledige back-up wordt uitgevoerd.

Bij **Incrementele back-up** is sprake van zeer efficiëntie opslag en netwerk door alleen gegevensblokken op te slaan die sinds de vorige back-up zijn gewijzigd. Met incrementele back-up hoeft u geen periodieke volledige back-ups te maken. In het voorbeeld worden, nadat de volledige back-up voor de eerste maand is gemaakt, de blokken A2, A3, A4 en A9 gemarkeerd als gewijzigd en overgedragen naar de tweede maand. In het derde maand wordt alleen gewijzigd blok A5 gemarkeerd als gemarkeerd en overgedragen. Door minder gegevens te verplaatsen worden opslag- en netwerkresources bespaard, waardoor de totale eigendomskosten worden verlaagd.   

### <a name="security"></a>Beveiliging
| Functie | Azure Backup-agent | System Center DPM | Azure Backup-server | Back-up van virtuele machines van Azure IaaS |
| --- | --- | --- | --- | --- |
| Netwerkbeveiliging<br/> (naar Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Gedeeltelijk][yellow] |
| Gegevensbeveiliging<br/> (in Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Gedeeltelijk][yellow] |

![tabelsleutel](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Netwerkbeveiliging
Al het back-upverkeer van uw servers naar de Recovery Services-kluis wordt versleuteld met Advanced Encryption Standard 256. De back-upgegevens worden verzonden via een beveiligde HTTPS-koppeling. De back-upgegevens worden ook versleuteld opgeslagen in de Recovery Services-kluis. Alleen u, de Azure-klant, beschikt over de wachtwoordzin waarmee deze gegevens kunnen worden ontgrendeld. De back-upgegevens kunnen niet door Microsoft ontsleuteld.

> [!WARNING]
> Nadat u de Recovery Services-kluis hebt ingesteld, hebt u als enige toegang tot de versleutelingssleutel. Microsoft bewaart nooit een kopie van de versleutelingssleutel en heeft geen toegang tot de sleutel. Als de sleutel is verkeerd wordt geplaatst, kan Microsoft de back-upgegevens niet herstellen.
>
>

#### <a name="data-security"></a>Gegevensbeveiliging
Als u back-ups van virtuele machines van Azure wilt maken, moet u de versleuteling configureren *in* de virtuele machine. Gebruik BitLocker voor virtuele machines van Windows en **dm-crypt** voor virtuele machines van Linux. De gegevens die via dit pad worden verzonden, worden niet automatisch door Azure Backup versleuteld.

### <a name="network"></a>Netwerk
| Functie | Azure Backup-agent | System Center DPM | Azure Backup-server | Back-up van virtuele machines van Azure IaaS |
| --- | --- | --- | --- | --- |
| Netwerkcompressie <br/>(naar de **back-upserver**) | |![Ja][green] |![Ja][green] | |
| Netwerkcompressie <br/>(naar de **Recovery Services-kluis**) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Netwerkprotocol <br/>(naar de **back-upserver**) | |TCP |TCP | |
| Netwerkprotocol <br/>(naar de **Recovery Services-kluis**) |HTTPS |HTTPS |HTTPS |HTTPS |

![tabelsleutel](./media/backup-introduction-to-azure-backup/table-key-2.png)

De VM-extensie (op de IaaS-VM) leest de gegevens rechtstreeks vanuit het Azure Storage-account via het opslagnetwerk. Dit verkeer hoeft daarom niet te worden gecomprimeerd.

Als u een System Center DPM of Azure Backup Server gebruikt als secundaire back-upserver, comprimeert u de gegevens die van de primaire server naar de back-upserver worden overgebracht. Wanneer gegevens worden gecomprimeerd voordat er een back-up van wordt gemaakt op DPM of Azure Backup Server, bespaart u bandbreedte.

#### <a name="network-throttling"></a>Netwerkbeperking
De Azure Backup-agent biedt netwerkbeperking, zodat u kunt bepalen hoe de netwerkbandbreedte tijdens de gegevensoverdracht wordt gebruikt. Beperking kan handig zijn wanneer u onder werktijd een back-up van gegeven moet maken, maar niet wilt dat het back-upproces het andere internetverkeer verstoort. De beperking voor de gegevensoverdracht is van toepassing op back-up- en herstelbewerkingen.

## <a name="backup-and-retention"></a>Back-up en retentie

Azure Backup heeft een limiet van 9999 herstelpunten, ook wel back-ups of momentopnamen genoemd, *per beveiligd exemplaar*. Een beveiligd exemplaar is een computer, een server (fysiek of virtueel) of een werkbelasting die is geconfigureerd voor het opslaan van back-ups van gegevens in Azure. Raadpleeg [Wat is een beveiligd exemplaar](backup-introduction-to-azure-backup.md#what-is-a-protected-instance) voor meer informatie. Een exemplaar is beveiligd zodra er een back-up van de gegevens is opgeslagen. De back-up van de gegevens biedt beveiliging. Als de brongegevens verloren gaan of beschadigd raken, kunt u met de back-up de brongegevens herstellen. In de volgende tabel wordt de maximale back-upfrequentie voor elk onderdeel weergegeven. De configuratie van uw back-upbeleid bepaalt hoe snel u de herstelpunten verbruikt. Als u bijvoorbeeld één herstelpunt per dag maakt, kunt u 27 jaar gebruikmaken van herstelpunten voordat ze opraken. Als u één herstelpunt per maand maakt, duurt het maar liefst 833 jaar voordat uw herstelpunten op zijn. In de Backup-service worden geen verlooptijden ingesteld voor herstelpunten.

|  | Azure Backup-agent | System Center DPM | Azure Backup-server | Back-up van virtuele machines van Azure IaaS |
| --- | --- | --- | --- | --- |
| Back-upfrequentie<br/> (naar de Recovery Services-kluis) |Drie back-ups per dag |Twee back-ups per dag |Twee back-ups per dag |Eén back-up per dag |
| Back-upfrequentie<br/> (naar schijf) |Niet van toepassing |<li>Om de 15 minuten voor SQL Server <li>Elk uur voor andere workloads |<li>Om de 15 minuten voor SQL Server <li>Elk uur voor andere workloads</p> |Niet van toepassing |
| Bewaaropties |Dagelijks, wekelijks, maandelijks, jaarlijks |Dagelijks, wekelijks, maandelijks, jaarlijks |Dagelijks, wekelijks, maandelijks, jaarlijks |Dagelijks, wekelijks, maandelijks, jaarlijks |
| Maximumaantal herstelpunten per beveiligd exemplaar |9999|9999|9999|9999|
| Maximale bewaarperiode |Afhankelijk van back-upfrequentie |Afhankelijk van back-upfrequentie |Afhankelijk van back-upfrequentie |Afhankelijk van back-upfrequentie |
| Herstelpunten op lokale schijf |Niet van toepassing |<li>64 voor bestandsservers,<li>448 voor toepassingsservers |<li>64 voor bestandsservers,<li>448 voor toepassingsservers |Niet van toepassing |
| Herstelpunten op tape |Niet van toepassing |Onbeperkt |Niet van toepassing |Niet van toepassing |

## <a name="what-is-a-protected-instance"></a>Wat is een beveiligd exemplaar?
Een beveiligd exemplaar is een algemene verwijzing naar een Windows-computer, een server (fysiek of virtueel) of een SQL-database die is geconfigureerd voor het maken van back-ups in Azure. Een exemplaar is beveiligd zodra er back-upbeleid is geconfigureerd voor een computer, server of database en er een back-up van de gegevens is gemaakt. Met alle volgende kopieën van de back-upgegevens van het beveiligde exemplaar (die 'herstelpunten' worden genoemd) wordt er meer opslagruimte verbruikt. U kunt maximaal 9999 herstelpunten maken voor een beveiligd exemplaar. Als u een herstelpunt uit de opslag verwijdert, telt dit niet meer mee voor het totaal van 9999 herstelpunten.
Enkele voorbeelden van veelgebruikte beveiligde exemplaren zijn virtuele machines, toepassingsservers, databases en pc's met het Windows-besturingssysteem. Bijvoorbeeld:

* Een virtuele machine met de hypervisorinfrastructuur Hyper-V of Azure IaaS. Het gastbesturingssysteem voor de virtuele machine kan Windows Server of Linux zijn.
* Een toepassingsserver: de toepassingsserver kan een fysieke of virtuele machine zijn met Windows Server en werkbelastingen met gegevens waarvan een back-up moet worden gemaakt. Veelvoorkomende werkbelastingen zijn: Microsoft SQL Server, Microsoft Exchange Server, Microsoft SharePoint Server en de bestandsserverfunctie van Windows Server. Als u een back-up wilt maken van deze werkbelastingen, hebt u System Center Data Protection Manager (DPM) of Azure Backup Server nodig.
* Een pc, werkstation of laptop met het Windows-besturingssysteem.


## <a name="what-is-a-recovery-services-vault"></a>Wat is een Recovery Services-kluis?
Een Recovery Services-kluis is een online-opslagentiteit in Azure die wordt gebruikt om gegevens zoals back-ups, herstelpunten en back-upbeleid op te slaan. U kunt Recovery Services-kluizen gebruiken voor het opslaan van back-upgegevens voor Azure-services en on-premises servers en werkstations. Recovery Services-kluizen maken het eenvoudig om uw back-upgegevens te ordenen, terwijl de beheertaken minimaal zijn. U kunt binnen een abonnement zoveel Recovery Services-kluizen maken als u wilt.

Back-upkluizen, die gebaseerd zijn op Azure Service Manager, waren de eerste versie van de kluis. Recovery Services-kluizen, waaraan de functies van het Azure Resource Manager-model zijn toegevoegd, zijn de tweede versie van de kluis. Zie het artikel [Recovery Services vault overview](backup-azure-recovery-services-vault-overview.md) (Overzicht van Recovery Services-kluizen) voor een volledige beschrijving van de functieverschillen. Het is niet meer mogelijk om de portal te gebruiken om back-upkluizen te maken, maar back-upkluizen worden nog steeds ondersteund. U moet Azure Portal gebruiken voor het beheren van uw Backup-kluizen.

> [!IMPORTANT]
> U kunt uw back-upkluizen upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 30 november 2017 kunt u PowerShell niet meer gebruiken om back-upkluizen te maken. Alle resterende back-upkluizen worden hierna automatisch bijgewerkt naar Recovery Services-kluizen.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Wat is het verschil tussen Azure Backup en Azure Site Recovery?
Azure Backup en Azure Site Recovery lijken in die zin op elkaar dat u met beide services back-ups kunt maken en gegevens kunt herstellen. Deze services worden echter voor verschillende doeleinden gebruikt bij het bieden van zakelijke continuïteit en herstel na noodgevallen voor uw bedrijf. Azure Backup is bedoeld om gegevens te beveiligen en te herstellen op een gedetailleerder niveau. Als er bijvoorbeeld een presentatie op een laptop beschadigd raakt, kunt u Azure Backup gebruiken om de presentatie te herstellen. U kunt Azure Site Recovery gebruiken als u de configuratie en de gegevens op een virtuele machine naar een ander datacenter wilt repliceren.

Azure Backup beschermt gegevens zowel on-premises als in de cloud. Azure Site Recovery coördineert de replicatie, failover en failback van virtuele machines en fysieke servers. Beide services zijn belangrijk omdat uw noodhersteloplossing uw gegevens veilig en herstelbaar (Backup) *en* uw workloads in het geval van een storing beschikbaar moet houden (Site Recovery).

De volgende concepten helpen u belangrijke beslissingen te nemen met betrekking tot het herstellen van back-ups en herstel na een noodgeval.

| Concept | Details | Back-up maken | Herstel na noodgeval (DR; disaster recovery) |
| --- | --- | --- | --- |
| Recovery Point Objective (RPO) |Acceptabele hoeveelheid gegevens die verloren gaat wanneer een herstelbewerking moet worden uitgevoerd. |Back-oplossingen hebben grote variabiliteit met betrekking tot hun acceptabele RPO. Back-ups van virtuele machines hebben doorgaans een RPO van één dag, terwijl de RPO bij databaseback-up soms maar vijftien minuten is. |Oplossingen voor herstel na noodgevallen hebben lage RPO's. De DR-kopie ligt maar een paar seconden of paar minuten achter. |
| Beoogde hersteltijd (RTO) |De hoeveelheid tijd die nodig is om een terugzet- of herstelbewerking te voltooien. |Vanwege de grotere RPO is de hoeveelheid gegevens die een back-upoplossing moet verwerken doorgaans veel grote, wat resulteert in langere RTO's. Het kan bijvoorbeeld dagen duren om gegevens te herstellen vanaf tapes, afhankelijk van de tijd die nodig is voor het transport van de tape vanaf een externe locatie. |Noodhersteloplossingen hebben kleinere RTO's omdat ze beter zijn gesynchroniseerd met de bron. Er hoeven minder wijzigingen te worden verwerkt. |
| Bewaartermijn |Hoe lang gegevens moeten worden bewaard |Voor scenario's waarvoor een operationele herstelbewerking is vereist (beschadigde gegevens, onbedoeld verwijderen van bestanden, OS-fouten), moeten de back-upgegevens doorgaans 30 dagen of korter worden bewaard.<br>Vanuit het oogpunt van de naleving moeten gegevens maanden of zelfs jaren worden bewaard. In dergelijke gevallen kunnen de back-upgegevens het beste worden gearchiveerd. |Voor het herstel na een noodgeval zijn alleen operationele herstelgegevens nodig, die doorgaans een paar uur tot maximaal een dag vergen. Vanwege de verfijnde manier waarop de gegevens door noodhersteloplossingen worden vastgelegd, kunt u de gegevens voor noodherstel beter niet langdurig bewaren. |

## <a name="next-steps"></a>Volgende stappen
Gebruik een van de volgende zelfstudies voor gedetailleerde, stapsgewijze instructies voor het beveiligen van gegevens op Windows Server, of voor het beveiligen van een VM (virtuele machine) in Azure:

* [Een back-up maken van bestanden en mappen](backup-try-azure-backup-in-10-mins.md)
* [Een back-up maken van Azure Virtual Machines](backup-azure-vms-first-look-arm.md)

Lees een van de volgende artikelen voor meer informatie over het beschermen van andere workloads:

* [Een back-up maken van uw Windows Server](backup-configure-vault.md)
* [Een back up maken van uw toepassingsworkloads](backup-azure-microsoft-azure-backup.md)
* [Een back-up maken van Azure IaaS VM's](backup-azure-arm-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png

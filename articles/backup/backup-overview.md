---
title: Wat is Azure Backup?
description: Geeft een overzicht van de Azure Backup-service en hoe u deze kunt implementeren als onderdeel van uw strategie voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR).
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 02/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 79a53c1b11a1bac0ea83afdd1333423aff4d6aa6
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428463"
---
# <a name="what-is-azure-backup"></a>Wat is Azure Backup?

De Azure Backup-service maakt back-ups van gegevens naar de Microsoft Azure-cloud. U kunt een back-up maken van on-premises machines en workloads en van virtuele Azure-machines (VM's).


## <a name="why-use-azure-backup"></a>Waarom Azure Backup gebruiken?

Azure Backup biedt deze belangrijke voordelen:

- **Geen on-premises back-up meer nodig**: Azure Backup biedt een eenvoudige oplossing voor het maken van back-ups van uw on-premises resources naar de cloud. Back-ups op korte en lange termijn zonder de noodzaak om complexe on-premises back-upoplossingen te implementeren. 
- **Back-ups van Azure IaaS-VM's**: Azure Backup biedt onafhankelijke en geïsoleerde back-ups om te voorkomen dat oorspronkelijke gegevens per ongeluk worden vernietigd. Back-ups worden opgeslagen in een Recovery Services-kluis met ingebouwde beheerde herstelpunten. Configuratie en schaalbaarheid is eenvoudig, back-ups zijn geoptimaliseerd en kunt u eenvoudig naar behoefte herstellen.
- **Gemakkelijk schalen**: Azure Backup gebruikt de onderliggende kracht en onbeperkte schaal van de Azure-cloud voor hoge beschikbaarheid, zonder overhead voor onderhoud of bewaking. 
- **Onbeperkte gegevensoverdracht**: Azure Backup stelt geen beperking aan de hoeveelheid binnenkomende of uitgaande gegeven die u overbrengt en brengt geen kosten in rekening voor de gegevens die worden overgebracht.
    - Uitgaande gegevens zijn gegevens die tijdens een herstelbewerking worden overgebracht uit een Recovery Services-kluis.
    - Als u met de Azure Import/Exportservice een offline eerste back-up uitvoert voor het importeren van grote hoeveelheden gegevens, zijn er kosten verbonden aan inkomende gegevens.  [Meer informatie](backup-azure-backup-import-export.md). 
- **Gegevens veilig houden**: Gegevensversleuteling zorgt voor een beveiligde overdracht en opslag van uw gegevens in de openbare cloud. U slaat de wachtwoordzin voor versleuteling lokaal op. Deze wordt nooit verzonden naar of opgeslagen in Azure. Als het herstellen van gegevens noodzakelijk is, bent u de enige met de wachtwoordzin voor versleuteling of de sleutel.
- **App-consistente back-ups**: Een app-consistente back-up betekent dat een herstelpunt alle vereiste gegevens heeft om de back-up te kunnen herstellen. Azure Backup biedt toepassingsconsistente back-ups, om ervoor te zorgen dat er geen aanvullende correcties nodig zijn om de gegevens te herstellen. Herstellen van toepassingsconsistente gegevens verkort de hersteltijd, zodat u snel weer normaal aan het werk kunt.
- **Korte- en langetermijngegevens bewaren**: u kunt de Recovery Services-kluizen gebruiken voor gegevensretentie op korte en lange termijn. Azure heeft geen beperkingen voor hoelang gegevens bewaard blijven in een Recovery Services-kluis. U kunt gegevens zo lang bewaren als u wilt. Azure Backup heeft een limiet van 9999 herstelpunten per beveiligd exemplaar. [Lees meer](backup-introduction-to-azure-backup.md#backup-and-retention) over de invloed van deze limiet op uw back-upbehoeften.
- **Automatisch opslagbeheer**: voor hybride omgevingen is vaak heterogene opslag vereist, soms on-premises en soms in de cloud. Met Azure Backup zijn er geen kosten voor het gebruik van on-premises opslagapparaten. De back-upopslag wordt automatisch door Azure Backup toegewezen en beheerd en u betaalt naar gebruik. Dat betekent dat u alleen betaalt voor de opslag die u verbruikt. [Lees meer](https://azure.microsoft.com/pricing/details/backup) over prijzen.
- **Meerdere opslagopties**: Azure Backup biedt twee typen replicatie om uw opslag/gegevens maximaal beschikbaar te houden.
    - Met [lokaal redundante opslag LRS](../storage/common/storage-redundancy-lrs.md) worden uw gegevens drie keer gerepliceerd (er worden drie kopieën gemaakt van uw gegevens) in een opslagschaaleenheid in een datacenter. Alle kopieën van de gegevens komen binnen dezelfde regio voor. LRS is een goedkope optie voor het beschermen van uw gegevens tegen lokale hardwarefouten.
    - [Geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md) is de standaardinstelling en is de replicatieoptie die wordt aanbevolen. Met GRS worden uw gegevens gerepliceerd naar een secundaire regio (honderden kilometers verwijderd van de primaire locatie van de brongegevens). GRS is duurder dan LRS, maar biedt een hoger duurzaamheidsniveau voor uw gegevens, zelfs in geval van een regionale onderbreking.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>Wat is het verschil tussen Azure Backup en Azure Site Recovery?

Azure Backup en Azure Site Recovery dragen beide bij aan een strategie voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR) in uw bedrijf. BCDR heeft twee algemene doelen:

- Uw bedrijfsgegevens veilig en herstelbaar houden wanneer er zich storingen voordoen.
- Uw apps en workloads in bedrijf te houden tijdens geplande en ongeplande downtime.

Beide services bieden complementaire maar verschillende functionaliteit.

- **Azure Site Recovery**: Site Recovery biedt een oplossing voor herstel na noodgevallen voor on-premises machines en virtuele Azure-machines. U repliceert machines vanaf een primaire naar een secundaire locatie. Wanneer er zich een noodgeval voordoet, voert u een failover naar de secundaire locatie uit en zijn de machines van daaruit toegankelijk. Wanneer alles weer normaal functioneert, voert u een failback uit om ze te herstellen naar de primaire locatie.
- **Azure Backup**: De Azure Backup-service maakt back-ups van gegevens van on-premises machines en Azure-VM's. Gegevens kunnen op een gedetailleerd niveau worden geback-upt en hersteld, inclusief back-up van bestanden, mappen, de status van het computersysteem en appbewuste gegevensback-up. Azure Backup verwerkt gegevens op een gedetailleerder niveau dan Site Recovery. Als er bijvoorbeeld een presentatie op uw laptop beschadigd raakt, kunt u Azure Backup gebruiken om de presentatie te herstellen. Als u een VM-configuratie en gegevens veilig en toegankelijk wilt houden, kunt u Site Recovery gebruiken.  

Gebruik de tabel om uw behoeften op BCDR-gebied te bepalen. 

**Doel** | **Details** | **Vergelijking**
--- | --- | --- | --- |
**Gegevensback-up/-retentie** | Back-upgegevens kunnen dagen, maanden of zelfs jaren worden bewaard en opgeslagen als dat vanuit complianceperspectief nodig is. | Met back-upoplossingen zoals Azure Backup kunt u nauwkeurig gegevens selecteren waarvan u een back-up wilt maken en het back-up- en retentiebeleid nauwkeurig afstemmen.<br/><br/> Site Recovery staat niet dezelfde fijnafstemming toe.
**Recovery point objective (RPO)** | Acceptabele hoeveelheid gegevens die verloren gaat wanneer een herstelbewerking moet worden uitgevoerd. | Back-ups hebben meer variabele RPO.<br/><br/> Back-ups van VM's hebben doorgaans een RPO van één dag, terwijl de RPO bij databaseback-up soms maar 15 minuten is.<br/><br/> Site Recovery biedt een lage RPO omdat replicatie continu of frequent is, zodat de verschillen tussen het bron- en replica-exemplaar klein zijn.
**Beoogde hersteltijd (RTO)** |De hoeveelheid tijd die nodig is om een terugzet- of herstelbewerking te voltooien. | Vanwege de grotere RPO is de hoeveelheid gegevens die een back-upoplossing moet verwerken doorgaans veel grote, wat resulteert in langere RTO's. Het kan bijvoorbeeld dagen duren om gegevens te herstellen vanaf tapes, afhankelijk van de tijd die nodig is voor het transport van de tape vanaf een externe locatie. | Noodhersteloplossingen zoals Site Recovery hebben een lage RPO omdat continue/frequente replicatie meestal betekent dat het doel meer is gesynchroniseerd met de bron. |

## <a name="what-backup-scenarios-are-supported"></a>Welke back-upscenario's worden ondersteund?

Azure Backup kan een back-up maken van zowel on-premises machines als Azure-VM's.

**Machine** | **Back-upscenario**
--- | ---
**On-premises back-up** |  1) Voer de MARS-agent (Azure Backup Microsoft Azure Recovery Services) uit op on-premises Windows-machines om een back-up te maken van afzonderlijke bestanden en systeemstatus. <br/><br/>2) Maak een back-up van on-premises machines naar een back-upserver (System Center Data Protection Manager (DPM) of Microsoft Azure Backup Server (MABS)) en configureer de back-upserver om een back-up te maken naar een Azure Backup Recovery Services-kluis in Azure.
**Azure VM's** | (1) Schakel het maken van back-ups voor afzonderlijke Azure-VM's in. Wanneer u het maken van back-ups inschakelt, installeert Azure Backup een extensie op de Azure VM-agent die op de virtuele machine wordt uitgevoerd. De agent maakt een back-up van de hele virtuele machine.<br/><br/> 2) Voer de MARS-agent uit op een Azure-VM. Dit is handig als u een back-up wilt maken van afzonderlijke bestanden en mappen op de virtuele machine.<br/><br/> 3) Maak een back-up van een Azure-VM naar een DPM-server of MABS die wordt uitgevoerd in Azure. Gebruik vervolgens Azure Backup om een back-up van de DPM-server/MABS te maken naar een kluis. 


## <a name="why-use-a-backup-server"></a>Waarom een back-upserver gebruiken?




Het maken van een back-up van machines en apps naar MABS/DPM-opslag en de DPM/MABS-opslag vervolgens te kopiëren naar een kluis heeft de volgende voordelen:

- Een back-up naar MABS/DPM biedt app-aware back-ups die zijn geoptimaliseerd voor veelgebruikte apps zoals SQL Server, Exchange en SharePoint, naast back-ups van bestanden/mappen/volumes en back-ups van de machinestatus (bare-metal, systeemstatus).
- Voor on-premises machines hoeft u de MARS-agent niet te installeren op elke machine waarvan u een back-up wilt maken. Op elke machine wordt de DPM/MABS-beveiligingsagent uitgevoerd en de MARS-agent wordt alleen op de MABS/DPM uitgevoerd.
- U hebt meer flexibiliteit en gedetailleerde planningsopties voor het uitvoeren van back-ups.
- U kunt één console gebruiken voor het beheren van back-ups van meerdere machines die u verzamelt in beveiligingsgroepen. Dit is bijzonder nuttig wanneer apps gelaagd zijn over meerdere virtuele machines en u er een gezamenlijke back-up van wilt maken.

Lees meer [hoe back-up werkt](backup-architecture.md#architecture-back-up-to-dpmmabs) wanneer u een back-server gebruikt, en over de [ondersteuningsvereisten](backup-support-matrix-mabs-dpm.md) voor back-upservers.

## <a name="what-can-i-back-up"></a>Waarvan kan ik een back-up maken?

**Machine** | **Back-upmethode** | **Een back-up maken**
--- | --- | ---
**On-premises Windows-VM's** | MARS-agent uitvoeren | Back-up van bestanden, mappen, systeemstatus.<br/><br/> Linux-machines worden niet ondersteund.
**On-premises machines** | Back-up naar DPM/MABS | Back-up van alles dat wordt beveiligd door [DPM](backup-support-matrix-mabs-dpm.md#supported-backups-to-dpm) of [MABS](backup-support-matrix-mabs-dpm.md#supported-backups-to-mabs), met inbegrip van bestanden/mappen/shares/volumes en app-specifieke gegevens. 
**Azure VM's** | Back-upextensie voor Azure VM-agent uitvoeren | Back-up van de hele virtuele machine
**Azure VM's** | MARS-agent uitvoeren | Back-up van bestanden, mappen, systeemstatus.<br/><br/> Linux-machines worden niet ondersteund.
**Azure VM's** | Back-up naar MABS/DPM die wordt uitgevoerd in Azure | Back-up van alles dat wordt beveiligd door [MABS](backup-support-matrix-mabs-dpm.md#supported-backups-to-mabs) of [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807), met inbegrip van bestanden/mappen/shares/volumes en app-specifieke gegevens.

## <a name="what-backup-agents-do-i-need"></a>Welke back-upagents heb ik nodig?

**Scenario** | **Agent** 
--- | --- 
**Back-ups maken van Azure-VM's** | Geen agent nodig. Azure VM-extensie voor back-up is geïnstalleerd op de virtuele machine van Azure wanneer u de eerste Azure-VM-back-up uitvoert.<br/><br/> Ondersteunt Windows en Linux.
**Back-up van on-premises Windows-machines** | Download, installeer en voer de MARS-agent rechtstreeks uit op de machine. 
**Back-up van Azure-VM's met de MARS-agent** | Download, installeer en voer de MARS-agent rechtstreeks uit op de machine. De MARS-agent kan naast de back-upextensie worden uitgevoerd.
**Back-up van on-premises machines en Azure-VM's naar DPM/MABS** | De DPM- of MABS-beveiligingsagent wordt uitgevoerd op de machines die u wilt beveiligen. De MARS-agent wordt uitgevoerd op de DPM-server/MABS waarvan u een back-up wilt maken naar Azure.

## <a name="which-backup-agent-should-i-use"></a>Welke back-upagent moet ik gebruiken?

**Een back-up maken** | **Oplossing** | **Beperking**
--- | --- | ---
**Ik wil een back-up maken van een hele Azure VM** | Schakel back-up in voor de virtuele machine. De back-upextensie wordt automatisch geconfigureerd op de Windows- of Linux-Azure-VM. | Er wordt een back-up gemaakt van de hele virtuele machine <br/><br/> Voor Windows-VM's is de back-up app-consistent. Voor Linux is de back-up bestandsconsistent. Als u app-bewustheid voor Linux-VM's nodig hebt, moet u dit configureren met aangepaste scripts.
**Ik wil een back-up van bepaalde bestanden/mappen maken op Azure VM** | Implementeer de MARS-agent op de virtuele machine.
**Ik wil een back-up maken van on-premises Windows-machines** | Installeer de MARS-agent op de machine. | U kunt een back-up van bestanden, mappen en de systeemstatus maken naar Azure. Back-ups zijn niet app-bewust.
**Ik wil een directe back-up maken van on-premises Linux-machines** | U moet DPM of MABS implementeren om een back-up te maken naar Azure.
**Ik wil een back-up maken van apps die on-premises worden uitgevoerd** | Voor app-bewuste back-ups moeten machines worden beveiligd door DPM of MABS.
**Ik wil gedetailleerde en flexibele back-up- en herstelinstellingen voor virtuele Azure-machines** | Bescherm Azure-VM's met MABS/DPM in Azure voor extra flexibiliteit voor back-upplanning en volledige flexibiliteit voor het beschermen en herstellen van bestanden, mappen, volumes, apps en systeemstatus.


## <a name="next-steps"></a>Volgende stappen

- [Bekijk](backup-architecture.md) de architectuur en componenten voor verschillende back-upscenario's.
- [Controleer](backup-support-matrix.md) ondersteunde functies en instellingen voor back-up.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png


---
title: Wat is Azure Backup?
description: Geeft een overzicht van de Azure Backup-service en hoe u deze kunt implementeren als onderdeel van uw strategie voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR).
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 7156042243b1ba28cea712dc3722600b9fc46c42
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360625"
---
# <a name="what-is-azure-backup"></a>Wat is Azure Backup?

De Azure Backup-service maakt back-ups van gegevens naar de Microsoft Azure-cloud. U kunt een back-up maken van on-premises machines en workloads en van virtuele Azure-machines (VM's).


## <a name="why-use-azure-backup"></a>Waarom Azure Backup gebruiken?

Azure Backup biedt deze belangrijke voordelen:

- **Geen on-premises back-up meer nodig**: Azure Backup biedt een eenvoudige oplossing voor het maken van back-ups van uw on-premises resources naar de cloud. Back-ups op korte en lange termijn zonder de noodzaak om complexe on-premises back-upoplossingen te implementeren. U hebt geen tape of offsiteback-up meer nodig.
- **Back-ups van Azure IaaS-VM's**: Azure Backup biedt onafhankelijke en geïsoleerde back-ups om te voorkomen dat oorspronkelijke gegevens per ongeluk worden vernietigd. Back-ups worden opgeslagen in een Recovery Services-kluis met ingebouwde beheerde herstelpunten. Configuratie en schaalbaarheid is eenvoudig, back-ups zijn geoptimaliseerd en kunt u eenvoudig naar behoefte herstellen.
- **Gemakkelijk schalen**: Azure Backup gebruikt de onderliggende kracht en onbeperkte schaal van de Azure-cloud voor hoge beschikbaarheid, zonder overhead voor onderhoud of bewaking. U kunt waarschuwingen instellen voor het weergeven van informatie over gebeurtenissen, maar u hoeft zich geen zorgen te maken over hoge beschikbaarheid van uw gegevens in de cloud.
- **Onbeperkte gegevensoverdracht**: Azure Backup stelt geen beperking aan de hoeveelheid binnenkomende of uitgaande gegeven die u overbrengt. In Azure Backup wordt gegevensoverdracht niet in rekening gebracht. Als u de Azure Import/Exportservice gebruikt voor het importeren van grote hoeveelheden gegevens, zijn er kosten verbonden aan inkomende gegevens. Zie [Offline back-upworkflow in Azure Backup](backup-azure-backup-import-export.md) (Offline back-upwerkstroom in Azure Backup) voor meer informatie over deze kosten. Uitgaande gegevens zijn gegevens die tijdens een herstelbewerking worden overgebracht uit een Recovery Services-kluis.
- **Gegevens veilig houden**: Gegevensversleuteling zorgt voor een beveiligde overdracht en opslag van uw gegevens in de openbare cloud. U slaat de wachtwoordzin voor versleuteling lokaal op. Deze wordt nooit verzonden naar of opgeslagen in Azure. Als het herstellen van gegevens noodzakelijk is, bent u de enige met de wachtwoordzin voor versleuteling of de sleutel.
- **App-consistente back-ups**: Een app-consistente back-up betekent dat een herstelpunt alle vereiste gegevens heeft om de back-up te kunnen herstellen. Azure Backup biedt toepassingsconsistente back-ups, om ervoor te zorgen dat er geen aanvullende correcties nodig zijn om de gegevens te herstellen. Herstellen van toepassingsconsistente gegevens verkort de hersteltijd, zodat u snel weer normaal aan het werk kunt.
- **Korte- en langetermijnretentie**: **Langetermijnretentie**: u kunt de Recovery Services-kluizen gebruiken voor het bewaren van gegevens voor de korte en de lange termijn. Azure heeft geen beperkingen voor hoelang gegevens bewaard blijven in een Recovery Services-kluis. U kunt gegevens zo lang als u wilt in een kluis bewaren. Azure Backup heeft een limiet van 9999 herstelpunten per beveiligd exemplaar. Zie het gedeelte [Back-ups en retentie](backup-introduction-to-azure-backup.md#backup-and-retention) in dit artikel voor meer informatie over hoe deze limiet invloed kan hebben op uw back-upbehoeften.
- **Automatisch opslagbeheer**: voor hybride omgevingen is vaak heterogene opslag vereist, soms on-premises en soms in de cloud. Met Azure Backup zijn er geen kosten voor het gebruik van on-premises opslagapparaten. De back-upopslag wordt automatisch door Azure Backup toegewezen en beheerd en u betaalt naar gebruik. U betaalt dus alleen voor de opslag die u daadwerkelijk gebruikt. Zie het artikel [Azure pricing](https://azure.microsoft.com/pricing/details/backup) (Azure-prijzen) voor meer informatie.
- **Meerdere opslagopties**: een aspect van hoge beschikbaarheid is opslagreplicatie. Azure Backup biedt twee typen replicatie: [lokaal redundante opslag](../storage/common/storage-redundancy-lrs.md) en [geografisch redundante opslag](../storage/common/storage-redundancy-grs.md). Kies de optie back-upopslag op basis van uw behoeften:
    - Met lokaal redundante opslag (Locally Redundant Storage, LRS) worden uw gegevens drie keer gerepliceerd (er worden drie kopieën gemaakt van uw gegevens) in een opslagschaaleenheid in een datacenter. Alle kopieën van de gegevens komen binnen dezelfde regio voor. LRS is een goedkope optie voor het beschermen van uw gegevens tegen lokale hardwarefouten.
    - Geografisch redundante opslag (GRS) is de standaardinstelling en is de replicatieoptie die wordt aanbevolen. Met GRS worden uw gegevens gerepliceerd naar een secundaire regio (honderden kilometers verwijderd van de primaire locatie van de brongegevens). GRS is duurder dan LRS, maar biedt een hoger duurzaamheidsniveau voor uw gegevens, zelfs in geval van een regionale onderbreking.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>Wat is het verschil tussen Azure Backup en Azure Site Recovery?

Azure Backup en Azure Site Recovery dragen beide bij aan een strategie voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR) in uw bedrijf. BCDR heeft twee algemene doelen:

- Uw bedrijfsgegevens veilig en herstelbaar houden wanneer er zich storingen voordoen.
- Uw apps en workloads in bedrijf te houden tijdens geplande en ongeplande downtime.

Beide services bieden complementaire maar verschillende functionaliteit.

- **Azure Site Recovery**: Site Recovery biedt een oplossing voor herstel na noodgevallen voor on-premises machines en virtuele Azure-machines. U repliceert machines vanaf een primaire naar een secundaire locatie. Wanneer zich een noodgeval voordoet, voert u een failover naar de secundaire locatie en zijn de machines van daaruit toegankelijk. Wanneer alles weer normaal functioneert, voert u een failback uit om ze te herstellen naar de primaire locatie.
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
**On-premises machines (fysieke/virtuele)** |  U kunt een back-up maken van afzonderlijke on-premises machines.<br/><br/>U kunt een back-up maken van on-premises machines die worden beveiligd door System Center Data Protection Manager (DPM)<br/><br/> U kunt een back-up maken van on-premises machines die worden beveiligd door Microsoft Azure Backup-Server (MABS).
**Azure VM's** | U kunt een back-up maken van afzonderlijke Azure-VM's.<br/><br/> U kunt een back-up maken van door DPM of MABS beveiligde virtuele Azure-machines.

### <a name="back-up-servers"></a>Back-up van servers

Mogelijk wilt u een back-up maken van on-premises servers en workloads, of van Azure-VM's en hun workloads, eerst naar een back-upserver, en vervolgens naar een Recovery Services-kluis. 

**Back-upserver** | **Details**
--- | ---
**System Center Data Protection Manager (DPM)** | U kunt Azure Backup gebruiken om een back-up te maken van gegevens die door DPM worden beveiligd:<br/><br/> – DPM kan on-premises worden uitgevoerd (fysiek of virtueel) of in Azure.<br/><br/> – U kunt verschillende typen gegevens beschermen die worden uitgevoerd op on-premises machines en virtuele Azure-machines door een back-up van de gegevens te maken op de DPM-server.<br/><br/> Op zijn beurt kan er een back-up van de DPM-server worden gemaakt naar een Recovery Services-kluis met behulp van de Azure Backup-service.<br/><br/> De DPM-server en de machines die ermee worden beveiligd, moeten zich in hetzelfde netwerk bevinden. On-premises computers kunnen alleen worden beveiligd door een on-premises DPM-server. Azure-VM's kunnen ook alleen worden beveiligd door op Azure uitgevoerde DPM.
**Microsoft Azure Backup server (MABS)** | U kunt Azure Backup gebruiken om een back-up te maken van gegevens die door MABS worden beveiligd:<br/><br/> – MABS kan on-premises worden uitgevoerd (fysiek of virtueel) of in Azure.<br/><br/> – U kunt verschillende typen gegevens beschermen die worden uitgevoerd op on-premises machines en virtuele Azure-machines door een back-up van de gegevens te maken naar MABS.<br/><br/> – Op zijn beurt kan er een back-up van MABS worden gemaakt naar een Recovery Services-kluis met behulp van de Azure Backup-service.<br/><br/> – MABS biedt een vergelijkbare functionaliteit als DPM, behalve dat u met MABS geen back-up naar tape kunt maken. Voor MABS is geen System Center-licentie vereist.<br/><br/> Net zoals bij DPM, kunnen on-premises machines alleen worden beveiligd door on-premises MABS. Azure-VM's kunnen alleen worden beveiligd door een MABS in Azure.

Het maken van back-ups eerst naar DPM/MABS en vervolgens naar een kluis heeft de volgende voordelen:

- Een back-up maken naar DPM/MAB biedt app-aware back-ups die zijn geoptimaliseerd voor veelgebruikte apps zoals SQL Server, Exchange en SharePoint, naast back-ups van bestanden/mappen/volumes en back-ups van de machinestatus (bare-metal, systeemstatus).
- U hoeft de Azure Backup-agent niet te installeren op elke machine waarvan u een back-up wilt maken. Elke machine voert de DPM/MABS-beveiligingsagent uit, en de Azure Backup-Microsoft Azure Recovery Services-agent wordt alleen op de DPM-server/MABS uitgevoerd.
- U hebt meer flexibiliteit en gedetailleerde planningsopties voor het uitvoeren van back-ups.
- U kunt één console gebruiken voor het beheren van back-ups van meerdere machines die u verzamelt in beveiligingsgroepen. Dit is bijzonder nuttig wanneer apps gelaagd zijn over meerdere virtuele machines en u er een gezamenlijke back-up van wilt maken.

## <a name="what-can-be-backed-up"></a>Waar kan een back-up van worden gemaakt?

**Machine** | **Back-upserver** | **Back-up**
--- | --- | ---
On-premises Windows-VM's | Geen back-up naar DPM of MABS | Back-up van bestanden, mappen, systeemstatus.
Azure VM's (Windows en Linux) | Geen back-up naar DPM of MABS | Back-up van bestanden, mappen, systeemstatus.<br/><br/> Back-ups zijn app-bewust voor Windows-machines en bestandsbewust voor Linux-machines.
On-premises VM's/Azure-VM's | Beveiligd door DPM | Back-up van alles dat wordt beveiligd door DPM, met inbegrip van bestanden/mappen/shares/volumes en app-specifieke gegevens. [Ontdek](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807) waar DPM een back-up van kan maken.
On-premises VM's/Azure-VM's | Beveiligd door MABS | Back-up van alles dat wordt beveiligd door MABS, met inbegrip van bestanden/mappen/shares/volumes en app-specifieke gegevens. [Ontdek](backup-mabs-protection-matrix.md) waar MABS een back-up van kan maken.

## <a name="what-backup-agents-do-i-need"></a>Welke back-upagents heb ik nodig?
**Scenario** | **Agent** | **Details**
--- | --- | ---
On-premises machines (geen back-upserver) | MARS-agent (Microsoft Azure Recovery Services) wordt uitgevoerd op de Windows-machine. | U downloadt en installeert de MARS-agent tijdens de implementatie van Azure Backup.<br/><br/> Ondersteunt alleen Windows-machines.
Azure VM's (geen back-upserver) | Er is geen expliciete agent vereist. Azure VM-extensie voor back-up wordt uitgevoerd op de Azure-VM. | De extensie wordt geïnstalleerd wanneer u de eerste Azure-VM back-up uitvoert.<br/><br/> Ondersteunt Windows en Linux.
On-premises machines/Azure-VM's beveiligd door DPM. | De MARS-agent wordt uitgevoerd op de DPM-server. | De MARS-agent is niet nodig op afzonderlijke machines.<br/><br/> Wanneer u DPM implementeert, wordt de DPM-beveiligingagent geïnstalleerd op elke machine die u beschermt. 
Back-up van on-premises machines en Azure-VM's beveiligd door MABS | De MARS-agent wordt uitgevoerd op de MABS. | De MARS-agent is niet nodig op afzonderlijke machines.<br/><br/> Wanneer u MABS implementeert, wordt de MABS-beveiligingagent geïnstalleerd op elke machine die u beschermt. 


## <a name="which-backup-agent-should-i-use"></a>Welke back-upagent moet ik gebruiken?

**Back-up** | **Oplossing** | **Beperking**
--- | --- | ---
Ik wil een back-up maken van on-premises Windows-machines. Machines worden niet beveiligd door DPM of MABS | Installeer de MARS-agent op de machine. | U kunt een back-up van bestanden, mappen en de systeemstatus maken naar Azure. Back-ups zijn niet app-bewust.
Ik wil een back-up maken van on-premises Linux-machines. Machines worden niet beveiligd door DPM of MABS. | U moet DPM of MABS implementeren om een back-up te maken naar Azure.
Ik wil een back-up maken van apps die worden uitgevoerd op on-premises Windows-machines | Voor app-bewuste back-ups moeten Windows-machines worden beveiligd door DPM of MABS.
Ik wil een back-up maken van Azure VM's | Voer een back-up uit met behulp van Azure Backup. De back-upextensie wordt automatisch geconfigureerd op de Windows- of Linux-Azure-VM. | Er wordt een back-up gemaakt van de VM-schijven.<br/><br/> Voor Windows-VM's is de back-up app-consistent. Voor Linux is de back-up bestandsconsistent. Als u app-bewustheid nodig hebt, moet u dit configureren met aangepaste scripts.
Ik wil een back-up van Azure-VM's maken met een gedetailleerde flexibiliteit van back-up- en herstelinstellingen | Bescherm Azure VM's met DPM of MABS in Azure voor extra flexibiliteit voor back-upplanning en volledige flexibiliteit voor het beschermen en herstellen van bestanden, mappen, volumes, apps en systeemstatus.


## <a name="next-steps"></a>Volgende stappen

- [Bekijk](backup-architecture.md) de architectuur en componenten voor verschillende back-upscenario's.
- [Controleer](backup-support-matrix.md) ondersteunde functies en instellingen voor back-up.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png


---
title: Over Azure VM Backup
description: Meer informatie over Azure VM backup en houd er rekening mee enkele aanbevolen procedures.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: raynew
ms.openlocfilehash: 1e80b2083a2fce90259ac0634d9e7f796f459fcd
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880946"
---
# <a name="about-azure-vm-backup"></a>Over Azure VM Backup

Dit artikel wordt beschreven hoe u de [Azure Backup-service](backup-introduction-to-azure-backup.md) back-ups van virtuele Azure-machines (VM's).

## <a name="backup-process"></a>Back-upproces

Dit is hoe Azure Backup aangeeft dat een back-up is voltooid voor Azure VM's:

1. Voor Azure-VM's die zijn geselecteerd voor back-up, vanaf Azure Backup een back-uptaak op basis van de back-upschema die u opgeeft.
1. Een Backup-extensie is geïnstalleerd op de virtuele machine tijdens de eerste back-up, als de virtuele machine wordt uitgevoerd.
    - Voor Windows-VM's, de _VMSnapshot_ -extensie is geïnstalleerd.
    - Voor virtuele Linux-machines, de _VMSnapshotLinux_ -extensie is geïnstalleerd.
1. Back-up-coördinaten voor Windows-VM's die worden uitgevoerd, met Windows Volume Shadow Copy Service (VSS) naar een app-consistente momentopname te maken van de virtuele machine.
    - Back-up wordt standaard volledige VSS-back-ups.
    - Als back-up kan niet een app-consistente momentopname te maken, klikt u vervolgens duurt het een bestand-consistente momentopname van de onderliggende opslag (omdat er geen schrijfbewerkingen van toepassingen worden uitgevoerd terwijl de virtuele machine is gestopt).
1. Voor Linux-VM's wordt back-up een bestandsconsistente back-up. Voor app-consistente momentopnamen moet u handmatig aanpassen voor/na-scripts.
1. Nadat de momentopname back-up is, wordt de gegevens overgedragen naar de kluis. 
    - De back-up is geoptimaliseerd door de back-ups van elke VM-schijf parallel.
    - Voor elke schijf die back-up, Azure Backup de blokken op de schijf leest en identificeert en brengt alleen de gegevensblokken die zijn gewijzigd (delta) sinds de vorige back-up.
    - Momentopname van de gegevens mogelijk niet direct worden gekopieerd naar de kluis. Het duurt enkele uren tijdens pieken. Totaal aantal back-uptijd voor een virtuele machine is minder dan 24 uur voor dagelijkse back-upbeleid.

1. Wanneer de overdracht van gegevens voltooid is, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt.

![Virtuele machine van Azure back-architectuur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encryption-of-azure-vm-backups"></a>Versleuteling van back-ups van virtuele Azure-machine

Wanneer u back-up van virtuele Azure-machines met Azure Backup, worden virtuele machines in rust versleuteld met Storage Service Encryption (SSE). Azure Backup kan ook back-up van Azure-VM's die met behulp van Azure Disk Encryption zijn versleuteld.

**Versleuteling** | **Details** | **Ondersteuning**
--- | --- | ---
**Azure Disk Encryption** | Azure Disk Encryption versleutelt zowel besturingssysteem en gegevensschijven voor virtuele Azure-machines.<br/><br/> Azure Disk Encryption kan worden geïntegreerd met BitLocker-versleutelingssleutels (BEKs), die als geheimen worden beveiligd in een key vault. Azure Disk Encryption kan ook worden geïntegreerd met Azure Key Vault-sleutel-versleutelingssleutels (kek's). | Azure Backup biedt ondersteuning voor back-up van beheerde en niet-beheerde Azure-VM's met BEKs alleen of met BEKs samen met de kek's versleuteld.<br/><br/> Zowel BEKs en kek's zijn een back-up en versleuteld.<br/><br/> Omdat kek's en BEKs back-ups, gebruikers met de vereiste machtigingen kunnen herstellen sleutels en geheimen terug naar de key vault indien nodig. Deze gebruikers kunnen ook de versleutelde VM herstellen.<br/><br/> Versleutelde sleutels en geheimen kunnen niet worden gelezen door onbevoegde gebruikers of door Azure.
**SSE** | Met SSE biedt Azure Storage versleuteling-at-rest automatisch door gegevens te coderen voordat u opslaat. Azure Storage ontsleutelt ook gegevens voordat u ze ophaalt. | Azure Backup gebruikt SSE voor versleuteling van inactieve virtuele Azure-machines.

Voor beheerde en onbeheerde Azure VM's ondersteunt back-up zowel virtuele machines die zijn versleuteld met BEKs alleen of virtuele machines die zijn versleuteld met BEKs samen met de kek's.

De back-up BEKs (geheimen) en kek's (sleutels), worden versleuteld. Ze kunnen lezen en wordt alleen gebruikt wanneer ze terug naar de key vault hebt hersteld door gemachtigde gebruikers. Noch Azure als niet-geautoriseerde gebruikers kan lezen of gebruikmaken van een back-up sleutels of geheimen.

BEKs ook back-ups. Dus als de BEKs verloren gaan, geautoriseerde gebruikers kunnen de BEKs terugzetten naar de key vault en de versleutelde virtuele machines herstellen. Alleen gebruikers met het vereiste niveau van de machtigingen kunnen een back-up en herstellen van versleutelde virtuele machines of sleutels en geheimen.

## <a name="snapshot-creation"></a>Momentopname maken

Azure Backup wordt momentopnamen op basis van de back-upschema. 

- **Windows-VM's:** Voor Windows-VM's coördineert de Backup-service met VSS op een app-consistente momentopname te maken van de VM-schijven.

  - Azure Backup wordt standaard volledige VSS-back-ups. [Meer informatie](https://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
  - Als u wilt de instelling wijzigen zodat VSS kopieback-ups worden Azure back-ups, stelt u de volgende registersleutel vanaf een opdrachtprompt:

    **REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d waar /f**

- **Linux-VM's:** Als u app-consistente momentopnamen van virtuele Linux-machines, gebruikt u de Linux-Pre-script en na script framework voor het schrijven van uw eigen aangepaste scripts om te zorgen voor consistentie.

  - Azure Backup aanroept alleen de vooraf/post-scripts die zijn geschreven door u.
  - Als de scripts die voorafgaan aan en scripts die volgen is uitvoert, markeert Azure Backup het herstelpunt als toepassingsconsistente. Echter, wanneer u aangepaste scripts gebruikt, bent u verantwoordelijk is voor de consistentie van toepassingen.
  - [Meer informatie](backup-azure-linux-app-consistent.md) over het configureren van scripts.

### <a name="snapshot-consistency"></a>Consistentie van de momentopname

De volgende tabel beschrijft de verschillende typen momentopname consistentie:

**momentopname** | **Details** | **Recovery** | **Overweging**
--- | --- | --- | ---
**Toepassingsconsistent** | App-consistente back-ups vastleggen geheugen inhoud en i/o-bewerkingen in behandeling. App-consistente momentopnamen gebruiken een VSS-schrijver (of vooraf/post-scripts voor Linux) om te controleren of de consistentie van de app-gegevens voordat u een back-up plaatsvindt. | Wanneer u een virtuele machine met een app-consistente momentopname herstelt, wordt de virtuele machine opgestart. Er is geen beschadigde gegevens of gegevensverlies. De apps starten in een consistente status. | Windows: Alle VSS-schrijvers is voltooid<br/><br/> Linux: Vooraf/post zijn scripts geconfigureerd en is voltooid
**Bestandssysteemconsistent** | Consistentie van bieden bestandssysteem toepassingsconsistente back-ups door het maken van een momentopname van alle bestanden op hetzelfde moment.<br/><br/> | Wanneer u een virtuele machine met een bestandssysteem consistente momentopname herstelt, wordt de virtuele machine opgestart. Er is geen beschadigde gegevens of gegevensverlies. Apps moeten voor het implementeren van hun eigen mechanisme 'bijwerken' om ervoor te zorgen dat de herstelde gegevens consistent is. | Windows: Bepaalde VSS-schrijvers is mislukt <br/><br/> Linux: Standaard (als pre/post-scripts zijn niet geconfigureerd of mislukt)
**Crash-consistente** | Crash-consistente momentopnamen wordt doorgaans uitgevoerd als een Azure-VM uitgeschakeld op het moment van back-up wordt. Alleen de gegevens die al op de schijf op het moment van back-up bestaat is vastgelegd en een back-up.<br/><br/> Een crash-consistente herstelpunt biedt geen garantie voor de consistentie van gegevens voor het besturingssysteem of de app. | Hoewel er geen garanties zijn, meestal door de virtuele machine wordt opgestart en start het een Schijfcontrole om Beschadigingsfouten te herstellen. Een in-memory-gegevens- of schrijfbewerkingen die niet zijn overgebracht naar de schijf voordat de crash gaan verloren. Apps implementeren voor de controle van hun eigen gegevens. Bijvoorbeeld, kunt een database-app met het transactielogboek gebruiken voor verificatie. Als het transactielogboek vermeldingen die zich niet in de database heeft, wordt de databasesoftware transacties terug tot de gegevens consistent is. | Virtuele machine zich in afsluitingsstaat

## <a name="backup-and-restore-considerations"></a>Overwegingen voor back-up en herstel 

**Overweging** | **Details**
--- | ---
**Schijf** | Back-up van VM-schijven is parallelle. Bijvoorbeeld, als een virtuele machine vier schijven heeft, probeert de service back-up naar back-up van alle vier schijven tegelijk. Back-up is incrementeel (alleen de gewijzigde gegevens).
**Planning** |  Als u wilt verkleinen back-upverkeer, back-up van verschillende virtuele machines op verschillende tijdstippen van de dag en zorg ervoor dat de tijd niet overlappen. Back-ups van virtuele machines op hetzelfde moment zorgt ervoor dat verkeer vastlopen.
**Back-ups voorbereiden** | Houd rekening met de benodigde tijd voor het voorbereiden van de back-up. De voorbereidingstijd omvat het installeren of bijwerken van de Backup-extensie en activeren van een momentopname op basis van de back-upschema.
**Gegevensoverdracht** | Houd rekening met de tijd die nodig is voor Azure Backup om de incrementele wijzigingen uit de vorige back-up te bepalen.<br/><br/> In een incrementele back-up bepaalt Azure Backup de wijzigingen door te berekenen van de controlesom van het blok. Als een blok is gewijzigd, wordt deze gemarkeerd voor overdracht naar de kluis. De service analyseert de geïdentificeerde blokken om te proberen te verder beperken van de hoeveelheid gegevens over te dragen. Nadat u de gewijzigde blokken, verzendt Azure Backup de wijzigingen aan de kluis.<br/><br/> Er is mogelijk een vertraging tussen het maken van de momentopname en kopieert deze naar de kluis.<br/><br/> Tijdens piekuren duurt het tot acht uur voor back-ups moeten worden verwerkt. De back-uptijd voor een virtuele machine is minder dan 24 uur voor de dagelijkse back-up.
**Eerste back-up** | Hoewel de totale back-uptijd voor incrementele back-ups minder dan 24 uur, die mogelijk niet het geval is bij de eerste back-up is. De tijd die nodig is voor de eerste back-up is afhankelijk van de grootte van de gegevens en tijdens het verwerken van de back-up.
**Herstellen van wachtrij** | Verwerkt door Azure back-up herstellen taken van meerdere opslagaccounts op hetzelfde moment en aanvragen voor herstellen in een wachtrij wordt geplaatst.
**Kopie** | Tijdens het herstelproces worden gegevens gekopieerd uit de kluis naar het opslagaccount.<br/><br/> De totale hersteltijd, is afhankelijk van de i/o-bewerkingen per seconde (IOPS) en de doorvoer van het opslagaccount.<br/><br/> Als u wilt kopiëren op hoelang, selecteer een opslagaccount die niet is geladen met andere schrijfbewerkingen van toepassingen en leesbewerkingen.

### <a name="backup-performance"></a>Back-upprestaties

Deze algemene scenario's kunnen invloed hebben op de totale uitvoeringstijd van de back-up:

- **Een nieuwe schijf toevoegen aan een beveiligde Azure-VM:** Als u een VM ondergaat incrementele back-up en een nieuwe schijf wordt toegevoegd, wordt de back-uptijd vergroten. De totale back-uptijd mogelijk meer dan 24 uur laatste vanwege de initiële replicatie van de nieuwe schijf, samen met de replicatie van verschillen van bestaande schijven.
- **Gefragmenteerde schijven:** Back-upbewerkingen zijn sneller wanneer schijfwijzigingen aaneengesloten zijn. Als wijzigingen worden verdeeld en gefragmenteerd over een schijf, worden back-up van binnenkomst. 
- **Verloop van de schijf:** Als beveiligde schijven die momenteel worden uitgevoerd van incrementele back-up hebben van een dagelijkse verloop van meer dan 200 GB, back-up kan lang duren (meer dan acht uur) om te voltooien. 
- **Back-versies:** De nieuwste versie van de back-up (bekend als de versie direct herstellen) maakt gebruik van een meer geoptimaliseerde proces dan vergelijking van de controlesom voor het identificeren van wijzigingen. Maar als u direct herstellen en een momentopname van een back-up hebt verwijderd, de back-up schakelt over naar de vergelijking van de controlesom. In dit geval wordt wordt de back-upbewerking meer dan 24 uur (of mislukken).

## <a name="best-practices"></a>Aanbevolen procedures

Wanneer u back-ups van virtuele machine configureert, het is raadzaam de volgende deze procedures:

- Wijzigen van de tijd van de standaard-plannen die zijn ingesteld in een beleid. Bijvoorbeeld, als de standaardwaarde in het beleid 12:00 uur is, verhogen de timing van enkele minuten zodat resources optimaal worden gebruikt.
- Voor de back-up van virtuele machines die van premiumopslag gebruikmaken, het beste de nieuwste versie van Azure Backup ([direct herstellen](backup-instant-restore-capability.md)). Als u niet de nieuwste versie, back-up kan worden toegewezen ongeveer 50 procent van de totale opslagruimte. De Backup-service is vereist voor deze ruimte het kopiëren van de momentopname naar hetzelfde opslagaccount en voor het overdragen van het naar de kluis.
- Als u virtuele machines vanuit één kluis terugzetten bent, is het raadzaam dat u verschillende [opslagaccounts voor algemeen gebruik v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) om ervoor te zorgen dat het doelopslagaccount niet te maken met beperkingen. Elke virtuele machine moet bijvoorbeeld een ander opslagaccount hebben. Als 10 virtuele machines zijn hersteld, gebruikt u bijvoorbeeld 10 verschillende opslagaccounts.
- Het terugzetten van een opslaglaag voor algemeen gebruik v1 (snapshot) wordt in enkele minuten worden voltooid omdat de momentopname in hetzelfde opslagaccount is. Herstellen van de opslaglaag voor algemeen gebruik v2 (kluis) kunnen een uur duren. In gevallen waar de gegevens beschikbaar in de opslag voor algemeen gebruik v1 zijn, raden wij aan dat u de [direct herstellen](backup-instant-restore-capability.md) functie voor sneller herstellen. (Als de gegevens moet worden hersteld van een kluis, klikt u vervolgens het duurt langer.)
- De limiet voor het aantal schijven per opslagaccount gebruikt, is ten opzichte van de mate waarin de schijven worden gebruikt door toepassingen die worden uitgevoerd op een infrastructuur als een dienst (IaaS) virtuele machine. Als het algemeen als 5-10-schijven of meer aanwezig op een enkel opslagaccount zijn, verdeelt u de taken door te verplaatsen van een aantal schijven voor het scheiden van storage-accounts.

## <a name="backup-costs"></a>Kosten voor back-up

Virtuele machines in Azure een back-up met Azure Backup zijn onderhevig aan [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

Facturering start niet totdat de eerste geslaagde back-up is voltooid. Op dit moment de facturering voor zowel opslag als beveiligde VM's wordt gestart. Facturering geldt zolang een back-upgegevens voor de virtuele machine is opgeslagen in een kluis. Als u de beveiliging voor een virtuele machine stopt, maar back-upgegevens voor de virtuele machine in een kluis bestaat, blijft de facturering.

Facturering voor een opgegeven virtuele machine stopt alleen als de beveiliging is gestopt en alle back-upgegevens wordt verwijderd. Wanneer de beveiliging stopt en er zijn geen actieve back-taken, de grootte van de laatste geslaagde VM back-up wordt de grootte van beveiligd exemplaar gebruikt voor de maandelijkse factuur.

De berekening van de grootte van beveiligd exemplaar is gebaseerd op de *werkelijke* grootte van de virtuele machine. De grootte van de virtuele machine is de som van alle gegevens in de virtuele machine, met uitzondering van de tijdelijke opslag. Prijzen zijn gebaseerd op de werkelijke hoeveelheid gegevens die is opgeslagen op de gegevensschijven, niet op de maximale grootte voor elke gegevensschijf die gekoppeld aan de virtuele machine wordt ondersteund.

Op dezelfde manier wordt het back-upopslag-factuur gebaseerd op de hoeveelheid gegevens die zijn opgeslagen in Azure Backup, is de som van de werkelijke gegevens in elk herstelpunt.

Neem bijvoorbeeld een VM voor A2-standaard-grootte waarvoor twee extra gegevensschijven met een maximale grootte van 4 TB. De volgende tabel bevat de werkelijke hoeveelheid gegevens die zijn opgeslagen op elk van deze schijven:

**Schijf** | **Maximale grootte** | **Werkelijke gegevens aanwezig**
--- | --- | ---
Besturingssysteemschijf | 4095 GB | 17 GB
Lokale/tijdelijke schijf | 135 GB | 5 GB (niet inbegrepen voor back-up)
Gegevensschijf 1 | 4095 GB | 30 GB
Gegevensschijf 2 | 4095 GB | 0 GB

De werkelijke grootte van de virtuele machine is in dit geval 17 GB + 30 GB + 0 GB = 47 GB. De grootte van deze beveiligde instantie (47 GB), wordt de basis voor de maandelijkse factuur. Naarmate de hoeveelheid gegevens in de virtuele machine groeit, wordt de status van de grootte van de beveiligde instantie gebruikt voor facturering wijzigingen zodat deze overeenkomt met.

## <a name="next-steps"></a>Volgende stappen

Nu [voorbereiden voor Azure VM backup](backup-azure-arm-vms-prepare.md).

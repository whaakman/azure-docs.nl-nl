---
title: Over Azure VM backup
description: Meer informatie over Azure VM backup en houd er rekening mee enkele aanbevolen procedures.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: cac219414418277ace09ba3a0b442f3bf74e6025
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107426"
---
# <a name="about-azure-vm-backup"></a>Over Azure VM backup

Dit artikel wordt beschreven hoe u de [Azure Backup-service](backup-introduction-to-azure-backup.md) back-ups van virtuele Azure-machines.

## <a name="backup-process"></a>Back-upproces

Hier ziet u hoe Azure Backup aangeeft dat een back-up is voltooid voor virtuele Azure-machines.

1. Voor Azure VM's die zijn geselecteerd voor back-up, initieert de Azure Backup-service een back-uptaak in overeenstemming met de back-upschema die u opgeeft.
2. De service wordt geactiveerd voor de Backup-extensie.
    - Windows-VM's gebruiken de _VMSnapshot_ extensie.
    - Linux-VM's gebruiken de _VMSnapshotLinux_ extensie.
    - De extensie wordt geïnstalleerd tijdens de eerste virtuele machine back-up.
    - Als u wilt de extensie installeren, moet de virtuele machine worden uitgevoerd.
    - Als de VM niet actief is, wordt met de Backup-service een momentopname gemaakt van de onderliggende opslag (aangezien er geen schrijfbewerkingen van toepassingen plaatsvinden als de VM is gestopt).
4. De back-upextensie duurt een opslag-niveau, crash-consistente/bestand-consistente momentopname.
5. Nadat de momentopname is gemaakt worden de gegevens overgedragen naar de kluis. Voor maximale efficiëntie, de service wordt geïdentificeerd en alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up (delta) overgedragen.
5. Wanneer de gegevensoverdracht is voltooid, wordt de momentopname verwijderd en wordt er een herstelpunt gemaakt.

![Virtuele machine van Azure back-architectuur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>Gegevensversleuteling

Azure Backup coderen niet van gegevens als onderdeel van het back-upproces. Azure Backup biedt ondersteuning voor back-up van virtuele Azure-machines die met Azure Disk Encryption zijn versleuteld.

- Back-up van virtuele machines versleuteld met BitLocker-versleuteling Key(BEK) alleen en BEK samen met Key(KEK) voor Key-versleuteling wordt ondersteund voor beheerde en niet-beheerde Azure-VM's.
- De BEK(secrets) en KEK(keys) back-ups worden gecodeerd, zodat ze kunnen worden gelezen en gebruikt alleen wanneer u terug naar de sleutelkluis is hersteld door de gemachtigde gebruikers.
- Omdat de BEK wordt ook back-up gemaakt in scenario's waarin BEK verloren gaan, kunnen geautoriseerde gebruikers de BEK herstellen naar de Key Vault en de versleutelde VM herstellen. Sleutels en geheimen van versleutelde virtuele machines zijn een back-up in een versleutelde vorm, noch Azure als niet-geautoriseerde gebruikers lezen kan of gebruik back-ups van sleutels en geheimen. Alleen gebruikers met het juiste machtigingsniveau kunnen een back-up en herstellen van versleutelde virtuele machines, evenals sleutels en geheimen.

## <a name="snapshot-consistency"></a>Consistentie van de momentopname

Als u momentopnamen wanneer apps worden uitgevoerd, wordt back-up van Azure app-consistente momentopnamen.

- **Windows-VM's**: Voor Windows-VM's coördineert de Backup-service met de Volume Shadow Copy Service (VSS) om op te halen van een consistente momentopname van de VM-schijven.
    - Azure Backup wordt standaard volledige VSS-back-ups. [Meer informatie](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Als u de instelling wijzigen wilt zodat Azure back-ups VSS kopieback-ups neemt, stelt u de volgende registersleutel:
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
- **Virtuele Linux-machines**: Om ervoor te zorgen dat uw virtuele Linux-machines app-consistente zijn wanneer Azure back-up een momentopname maakt, kunt u het Pre-script en post-script framework voor Linux. U kunt uw eigen aangepaste scripts om te zorgen voor consistentie bij het maken van een VM-momentopname schrijven.
    -  Azure Backup alleen aanroept vóór en na scripts die door u zijn geschreven.
    - Als de Pre-script en scripts die volgen is uitvoert, markeert Azure Backup het herstelpunt als toepassingsconsistente. U kunt echter verantwoordelijk is voor de toepassingsconsistentie bij het gebruik van aangepaste scripts.
    - [Meer informatie](backup-azure-linux-app-consistent.md) over het configureren van scripts.


#### <a name="consistency-types"></a>Consistentie typen

De volgende tabel beschrijft de verschillende typen consistentie.

**momentopname** | **Op basis van VSS** | **Details** | **Herstel**
--- | --- | --- | ---
**Toepassingsconsistent** | Ja (alleen Windows) | App-consistente back-ups vastleggen geheugen inhoud en i/o-bewerkingen in behandeling. App-consistente momentopnamen gebruiken VSS-schrijver (of pre-/ post-script voor Linux) die ervoor zorgen dat de consistentie van de app-gegevens voordat u een back-up plaatsvindt. | Tijdens het herstellen van met een app-consistente momentopname, de VM wordt opgestart. Er is geen beschadigde gegevens of gegevensverlies. De apps starten in een consistente status.
**Bestandssysteemconsistent** | Ja (alleen Windows) |  Bestand toepassingsconsistente back-ups bieden consistente back-ups van schijf-bestanden door het maken van een momentopname van alle bestanden op hetzelfde moment.<br/><br/> Azure back-up herstelpunten zijn consistent zijn voor bestand:<br/><br/> -Linux VM's, back-ups die geen pre/post-scripts, of die script hebt dat is mislukt.<br/><br/> -Windows-VM back-ups waar de VSS is mislukt. | Tijdens het herstellen van met een bestand-consistente momentopname, de VM wordt opgestart. Er is geen beschadigde gegevens of gegevensverlies. Apps moeten voor het implementeren van hun eigen mechanisme 'bijwerken' om ervoor te zorgen dat de herstelde gegevens consistent is.
**Crash-consistente** | Nee | Crashconsistentie gebeurt vaak als een Azure-VM uitgeschakeld op het moment van back-up wordt.  Alleen de gegevens die al op de schijf op het moment van back-up bestaat is vastgelegd en een back-up.<br/><br/> Een crash-consistente herstelpunt biedt geen garantie voor de consistentie van gegevens voor het besturingssysteem of de app. | Er zijn geen garanties, maar meestal de VM wordt opgestart, en volgt met een schijf controleren op Beschadigingsfouten te herstellen. Een in-memory-gegevens of schrijven die niet zijn overgebracht naar schijf gaan verloren. Apps implementeren voor de controle van hun eigen gegevens. Bijvoorbeeld, voor een database-app als een transactielogboek heeft vermeldingen die zich niet in de database, de databasesoftware wordt getotaliseerd tot gegevens consistent is.


## <a name="service-and-subscription-limits"></a>Limieten voor service en -abonnement

Azure Backup heeft een aantal beperkingen om abonnementen en -kluizen.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Back-upprestaties

### <a name="disk-considerations"></a>Overwegingen voor schijf

Back-upbewerking wordt geoptimaliseerd door back-ups van elk van de VM schijf parallel. Bijvoorbeeld, als een virtuele machine vier schijven heeft, probeert de service naar de back-up van alle vier schijven tegelijk. Voor elke schijf een back-wordt gemaakt, Azure Backup leest de blokken op de schijf en slaat alleen de gewijzigde gegevens (incrementele back-up).


### <a name="scheduling-considerations"></a>Overwegingen voor planning

Back-upplanning heeft invloed op prestaties.

- Als u beleidsregels configureren, zodat alle virtuele machines op hetzelfde moment worden opgeslagen, kunt u een vastgelopen verkeer hebt gepland als de back-upproces wil back-up van alle schijven tegelijk.
- De back-up om verkeer te beperken, maakt u een back-up van verschillende virtuele machines op andere moment van de dag, met elkaar niet overlappen.


### <a name="time-considerations"></a>Overwegingen voor tijd

Terwijl de meeste back-tijd wordt besteed aan het lezen van en het kopiëren van gegevens, worden andere bewerkingen toevoegen aan de totale tijd die nodig zijn voor de back-up van een virtuele machine:

- **Backup-extensie installeren**: De tijd die nodig is om te installeren of bijwerken van de Backup-extensie.
- **Momentopname van de trigger**: Gebruikte tijd voor het activeren van een momentopname. Momentopnamen zijn geactiveerd dicht bij de geplande back-uptijd.
- **Wachtrij wachttijd**: Sinds de back-up service processen taken van meerdere opslagaccounts van de klant op hetzelfde moment, momentopname van de gegevens mogelijk niet direct worden gekopieerd naar de Recovery Services-kluis. Tijdens piekuren laden duurt het tot acht uur voordat de back-ups worden verwerkt. De totale tijd van de VM-back-up is echter minder dan 24 uur voor dagelijkse back-upbeleid.
- **Eerste back-up**: Hoewel de totale back-uptijd van minder dan 24 uur geldig voor incrementele back-ups is, kan deze niet worden voor de eerste back-up. Tijd die nodig zijn afhankelijk van de grootte van de gegevens en wanneer de back-up wordt gemaakt.
- **Tijd van de gegevensoverdracht**: Tijd die nodig is voor back-upservice compute de incrementele wijzigingen van de vorige back-up en het overdragen van deze wijzigingen naar de kluis van opslag.

### <a name="factors-affecting-backup-time"></a>Factoren die invloed hebben op de back-uptijd

Back-up bestaat uit twee fasen maken van momentopnamen en het overdragen van de momentopnamen naar de kluis. De Backup-service wordt geoptimaliseerd voor opslag.

- Bij de overdracht van de momentopname van de gegevens naar een kluis, draagt de service alleen incrementele wijzigingen van de vorige momentopname.
- Om te bepalen de incrementele wijzigingen, berekent de service de controlesom van de blokken.
    - Als een blok is gewijzigd, wordt het blok wordt aangeduid als een blok moet worden verzonden naar de kluis.
    - De service-oefeningen verder in elk van de geïdentificeerde blokken, zoeken naar mogelijkheden om zo min mogelijk de gegevens over te dragen.
    - Nadat u alle gewijzigde blokken, de service de wijzigingen coalesces en stuurt deze naar de kluis.

Situaties die invloed kunnen hebben op de back-uptijd omvatten het volgende:

- **Eerste back-up voor een nieuw toegevoegde schijf aan een al beveiligde virtuele machine**: Als een virtuele machine incrementele back-up ondergaat en een nieuwe schijf wordt toegevoegd aan deze virtuele machine, en vervolgens de duur van de back-up kunt verder gaan dan 24 uur omdat de nieuw toegevoegde schijf heeft ondergaan initiële replicatie en replicatie van verschillen van bestaande schijven.
- **Fragmentatie**: Back-product scant naar incrementele wijzigingen tussen de twee back-ups bewerkingen. Back-upbewerkingen zijn sneller wanneer de wijzigingen op de schijf worden geplaatst in vergelijking tot wijzigingen worden verspreid via vervolgens de schijf. 
- **Verloop**: Dagelijkse verloop (voor incrementele replicatie) per schijf die groter is dan 200 GB duurt langer dan ~ 8 uur om de bewerking te voltooien. Als de virtuele machine heeft meer dan één schijf en als een van deze schijven langer back-up maken duurt, klikt u vervolgens het kan invloed hebben op de algemene back-upbewerking (of kan resulteren in mislukte). 
- **Controlesom vergelijking (CC) modus**: CC-modus is relatief langzamer dan geoptimaliseerde modus die worden gebruikt door directe RP. Als u al van directe RP gebruikmaakt en de Tier-1-momentopnamen hebt verwijderd, klikt u vervolgens back-up schakelt over naar de CC-modus waardoor de back-up bewerking langer dan 24 uur (of mislukken).

## <a name="restore-considerations"></a>Overwegingen met betrekking tot herstellen

Een herstelbewerking bestaat uit twee belangrijke taken: kopiëren van gegevens van de kluis naar het gekozen opslagaccount en het maken van de virtuele machine. De tijd die nodig is om gegevens te kopiëren uit de kluis is afhankelijk van waar de back-ups worden opgeslagen in Azure en de locatie van de storage-account. Afhankelijk van de tijd om gegevens te kopiëren:

- **Wachtrij wachttijd**: Omdat de serviceprocessen voor het herstellen van taken van meerdere opslagaccounts op hetzelfde moment, worden aanvragen voor herstellen in een wachtrij plaatsen.
- **Het kopiëren van gegevens tijd**: Gegevens worden gekopieerd uit de kluis naar het opslagaccount. Herstellen tijd is afhankelijk van IOPS en doorvoer van het geselecteerde opslagaccount, de Azure Backup-service wordt gebruikt. Als u wilt kopiëren hoelang tijdens het herstelproces, selecteer het opslagaccount dat niet geladen met andere schrijfbewerkingen van toepassingen en leesbewerkingen.

## <a name="best-practices"></a>Aanbevolen procedures

Het is raadzaam na deze procedures tijdens het configureren van back-ups van virtuele machine:

- Upkluizen upgraden naar Instant RP. Bekijk deze [voordelen](backup-upgrade-to-vm-backup-stack-v2.md), [overwegingen met betrekking tot](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade), en vervolgens gaat u verder met de upgrade Volg hiervoor de volgende [instructies](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).  
- Houd rekening met het standaard beleid tijd wijzigen (voor bijvoorbeeld. Als uw beleid standaardtijd 00:00 uur Houd rekening met het oplopend in minuten) wanneer de momentopnamen worden gemaakt om ervoor te zorgen resources optimaal worden gebruikt.
- Voor Premium-VM kan worden back-up op niet - directe RP-functie ~ 50% van de totale opslagruimte voor de account toegewezen. Back-upservice is vereist voor deze ruimte het kopiëren van de momentopname naar hetzelfde opslagaccount en voor het overdragen van het naar de kluis.
- Voor het herstellen van virtuele machines vanuit één kluis, is het raadzaam om te gebruiken verschillende [v2-accounts voor opslag](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) om te controleren of het doelopslagaccount niet te maken met beperkingen. Elke virtuele machine moet bijvoorbeeld verschillende storage-account (als 10 virtuele machines worden hersteld en vervolgens 10 verschillende opslagaccounts kunt u overwegen) hebben.
- Het terugzetten van de opslaglaag Tier-1 (snapshot) wordt voltooid in minuten (omdat het is hetzelfde opslagaccount) op basis van de opslaglaag van laag 2 (kluis) die kan uren duren. We raden u aan het gebruik van [Instant RP](backup-upgrade-to-vm-backup-stack-v2.md) functie voor snellere herstelacties voor gevallen waarin gegevens beschikbaar in Tier-1 zijn (als de gegevens worden hersteld vanuit kluis en vervolgens het duurt tijd).
- De limiet voor het aantal schijven per opslagaccount gebruikt, is ten opzichte van hoe zware de schijven worden gebruikt door toepassingen die worden uitgevoerd op IaaS-VM. Controleer of als meerdere schijven worden gehost op een enkel opslagaccount. Als het algemeen als 5-10-schijven of meer aan één opslagaccount aanwezig zijn, verdeelt u de taken door te verplaatsen van een aantal schijven voor het scheiden van storage-accounts.

## <a name="backup-costs"></a>Kosten voor back-up

Virtuele machines in Azure een back-up met Azure Backup zijn onderhevig aan [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

- Facturering start niet totdat de eerste geslaagde back-up is voltooid. Op dit moment de facturering voor zowel opslag als beveiligde instanties wordt gestart.
- Facturering geldt zolang er back-upgegevens die zijn opgeslagen in een kluis voor de virtuele machine is. Als u de beveiliging voor de virtuele machine stoppen, maar back-upgegevens van virtuele machine in een kluis bestaat, blijft de facturering.
- Facturering voor een opgegeven virtuele machine stopt alleen als de beveiliging is gestopt en alle back-upgegevens wordt verwijderd.
- Wanneer de beveiliging stopt en er zijn geen actieve back-taken, de grootte van de laatste geslaagde VM back-up wordt de grootte van de beveiligde instantie die wordt gebruikt voor de maandelijkse factuur.
- De berekening van de beveiligde instanties wordt gebaseerd op de *werkelijke* grootte van de virtuele machine, de som van alle gegevens in de virtuele machine is, met uitzondering van de tijdelijke opslag.
- Prijzen zijn gebaseerd op de werkelijke hoeveelheid gegevens die zijn opgeslagen in de gegevensschijf.
- Prijzen voor back-ups van virtuele machines zijn niet gebaseerd op de maximale ondersteunde grootte voor elke gegevensschijf die is gekoppeld aan de virtuele machine.
- Op dezelfde manier wordt het back-upopslag-factuur gebaseerd op de hoeveelheid gegevens die zijn opgeslagen in Azure Backup, is de som van de werkelijke gegevens in elk herstelpunt.

Neem bijvoorbeeld een Standard A2-grootte virtuele machine waarvoor twee extra gegevensschijven met een maximale grootte van 4 TB. De volgende tabel bevat de werkelijke hoeveelheid gegevens die zijn opgeslagen op elk van deze schijven:

| Schijftype | Max. grootte | Werkelijke gegevens aanwezig |
| --------- | -------- | ----------- |
| Besturingssysteemschijf |4095 GB |17 GB |
| Lokale schijf / tijdelijke schijf |135 GB |5 GB (niet inbegrepen voor back-up) |
| Gegevensschijf 1 |4095 GB |30 GB |
| Gegevensschijf 2 |4095 GB |0 GB |

- De werkelijke grootte van de virtuele machine virtuele machine is in dit geval 17 GB + 30 GB + 0 GB = 47 GB.
- De grootte van deze beveiligde instantie (47 GB), wordt de basis voor de maandelijkse factuur.
- Naarmate de hoeveelheid gegevens in de virtuele machine groeit, de grootte van de beveiligde instantie die wordt gebruikt voor facturering wijzigingen dienovereenkomstig.


## <a name="next-steps"></a>Volgende stappen

Na het controleren van de back-upproces en prestatie-overwegingen, het volgende doen:

- [Meer informatie over](../virtual-machines/windows/premium-storage-performance.md) afstemmen van apps voor optimale prestaties met Azure storage. Het artikel gericht op premium-opslag, maar het is ook van toepassing op standard-opslagschijven.
- [Aan de slag](backup-azure-arm-vms-prepare.md) met back-up aan de hand van de VM-ondersteuning en beperkingen, het maken van een kluis en virtuele machines voorbereiden voor back-up.

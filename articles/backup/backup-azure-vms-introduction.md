---
title: Over Azure VM backup
description: Meer informatie over Azure VM backup en houd er rekening mee enkele aanbevolen procedures.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: raynew
ms.openlocfilehash: 9a80671a72f059e24a8cebc5de803af9261ad829
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53743949"
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
4. De back-upextensie maakt een opslag-niveau, app-consistente momentopname.
5. Nadat de momentopname is gemaakt worden de gegevens overgedragen naar de kluis. Voor maximale efficiëntie, de service wordt geïdentificeerd en alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up (delta) overgedragen.
5. Wanneer de gegevensoverdracht is voltooid, wordt de momentopname verwijderd en wordt er een herstelpunt gemaakt.

![Virtuele machine van Azure back-architectuur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>Gegevensversleuteling

Azure Backup coderen niet van gegevens als onderdeel van het back-upproces. Azure Backup biedt ondersteuning voor back-up van virtuele Azure-machines die met Azure Disk Encryption zijn versleuteld.

- Back-up van virtuele machines versleuteld met BitLocker-versleuteling Key(BEK) alleen en BEK samen met Key(KEK) voor Key-versleuteling wordt ondersteund voor beheerde en niet-beheerde Azure-VM's.
- De BEK(secrets) en KEK(keys) back-ups worden gecodeerd, zodat ze kunnen worden gelezen en gebruikt alleen wanneer u terug naar de sleutelkluis is hersteld door de gemachtigde gebruikers.
- Omdat de BEK wordt ook back-up gemaakt in scenario's waarin BEK verloren gaan, kunnen geautoriseerde gebruikers de BEK herstellen naar de Key Vault en de versleutelde VM herstellen. Sleutels en geheimen van versleutelde virtuele machines zijn een back-up in een versleutelde vorm, noch Azure als niet-geautoriseerde gebruikers lezen kan of gebruik back-ups van sleutels en geheimen. Alleen gebruikers met het juiste machtigingsniveau kunnen back-up en herstellen van versleutelde virtuele machines, evenals sleutels en geheimen.

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
**Bestandssysteemconsistent** | Ja (alleen Windows) |  Bestand toepassingsconsistente back-ups bieden consistente back-ups van schijf-bestanden door het maken van een momentopname van alle bestanden op hetzelfde moment.<br/><br/> Azure back-up herstelpunten zijn consistent zijn voor bestand:<br/><br/> -Linux VM's, back-ups die geen pre/post-scripts, of die script hebt dat is mislukt.<br/><br/> -Windows-VM back-ups waar de VSS is mislukt. | Tijdens het herstellen van met een bestand-consistente momentopname, de VM wordt opgestart. Er is geen beschadigde gegevens of gegevensverlies. Apps moet voor het implementeren van hun eigen mechanisme 'bijwerken' om ervoor te zorgen dat de herstelde gegevens consistent is.
**Crash-consistente** | Nee | Crashconsistentie gebeurt vaak als een Azure-VM uitgeschakeld op het moment van back-up wordt.  Alleen de gegevens die al op de schijf op het moment van back-up bestaat is vastgelegd en een back-up.<br/><br/> Een crash-consistente herstelpunt biedt geen garantie voor de consistentie van gegevens voor het besturingssysteem of de app. | Er zijn geen garanties, maar meestal de VM wordt opgestart, en volgt met een schijf controleren op Beschadigingsfouten te herstellen. Een in-memory-gegevens of schrijven die niet zijn overgebracht naar schijf gaan verloren. Apps implementeren voor de controle van hun eigen gegevens. Bijvoorbeeld, voor een database-app als een transactielogboek heeft vermeldingen die zich niet in de database, de databasesoftware wordt getotaliseerd tot gegevens consistent is.


## <a name="service-and-subscription-limits"></a>Limieten voor service en -abonnement

Azure Backup heeft een aantal beperkingen om abonnementen en -kluizen.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Back-upprestaties

### <a name="disk-considerations"></a>Overwegingen voor schijf

Back-up probeert om uit te voeren zo snel mogelijk, net zo veel resources als deze kan gebruiken.

- In een poging om de snelheid te maximaliseren, wil het back-upproces back-up van elk van de schijven van de virtuele machine tegelijk.
- Bijvoorbeeld, als een virtuele machine vier schijven heeft, probeert de service naar de back-up van alle vier schijven tegelijk.
- Het aantal schijven back-up is de belangrijkste factor bij het bepalen van de back-upverkeer van storage-account.
- Alle i/o-bewerkingen worden beperkt door de *Doeldoorvoer van één Blob*, heeft een limiet van 60 MB per seconde.
- Voor elke schijf een back-wordt gemaakt, Azure Backup leest de blokken op de schijf en slaat alleen de gewijzigde gegevens (incrementele back-up). U kunt de gemiddelde doorvoersnelheid van onderstaande waarden gebruiken om te schatten van de hoeveelheid tijd die nodig zijn voor de back-up van een schijf van een opgegeven grootte.

    **Bewerking** | **Het doorvoer**
    --- | ---
    Eerste back-up | 160 Mbps |
    Incrementele back-up | 640 Mbps <br><br> Doorvoer komt aanzienlijk als de deltagegevens is verspreid over de schijf.|



### <a name="scheduling-considerations"></a>Overwegingen voor planning

Back-upplanning heeft invloed op prestaties.

- Als u beleidsregels configureren, zodat alle virtuele machines op hetzelfde moment worden ondersteund, hebt u een verkeer is vastgelopen, gepland als de back-upproces wil back-up van alle schijven tegelijk.
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


- **Eerste back-up voor een nieuw toegevoegde schijf aan een al beveiligde virtuele machine**: Als een virtuele machine is mogelijk een incrementele back-up, wanneer een nieuwe schijf wordt vervolgens de back-up toegevoegd de SLA voor één dag, afhankelijk van de grootte van de nieuwe schijf ontbreken.
- **Gefragmenteerde app**: Als een app is niet goed geconfigureerd. deze mogelijk niet optimaal voor opslag:
    - Als de momentopname veel kleine, gefragmenteerde schrijfbewerkingen bevat, in de service de taakwachtrij doorbrengt extra tijd die is geschreven door de toepassingen die gegevens verwerkt.
    - Voor toepassingen die binnen de virtuele machine wordt uitgevoerd, is het aanbevolen schrijfbewerkingen van toepassingen blok minimaal 8 KB. Als uw toepassing gebruikmaakt van een blok van minder dan 8 KB, wordt back-upprestaties heeft.
- **Opslagaccount overbelast**: Een back-up kan worden gepland wanneer de app in productie wordt uitgevoerd, of als er meer dan vijf tot tien schijven worden gehost in hetzelfde opslagaccount.
- **Consistentie controleren (CC) modus**: Voor schijven die groter zijn dan 1TB schijven kan de back-up in de CC-modus voor een aantal redenen zijn:
    - De beheerde schijf wordt verplaatst als onderdeel van de VM opnieuw wordt opgestart.
    - De energiestatus van de momentopname naar base blob.


## <a name="restore-considerations"></a>Overwegingen met betrekking tot herstellen

Een herstelbewerking bestaat uit twee belangrijke taken: kopiëren van gegevens van de kluis naar het gekozen opslagaccount en het maken van de virtuele machine. De tijd die nodig is om gegevens te kopiëren uit de kluis is afhankelijk van waar de back-ups worden opgeslagen in Azure en de locatie van de storage-account. Afhankelijk van de tijd om gegevens te kopiëren:

- **Wachtrij wachttijd**: Omdat de serviceprocessen voor het herstellen van taken van meerdere opslagaccounts op hetzelfde moment, worden aanvragen voor herstellen in een wachtrij plaatsen.
- **Het kopiëren van gegevens tijd**: Gegevens worden gekopieerd uit de kluis naar het opslagaccount. Herstellen tijd is afhankelijk van IOPS en doorvoer van het geselecteerde opslagaccount dat gebruikmaakt van de Azure Backup-service. Als u wilt kopiëren hoelang tijdens het herstelproces, selecteer het opslagaccount dat niet geladen met andere schrijfbewerkingen van toepassingen en leesbewerkingen.

## <a name="best-practices"></a>Aanbevolen procedures

Het is raadzaam na deze procedures tijdens het configureren van back-ups van virtuele machine:

- Geen back-ups plannen voor meer dan 100 virtuele machines van één kluis op hetzelfde moment.
- Virtuele machine back-ups plannen tijdens daluren. Op deze manier de Backup-service maakt gebruik van IOP's voor het overdragen van gegevens van het opslagaccount naar de kluis.
- Als u een back-up van beheerde schijven, verwerkt de Azure Backup-service opslagbeheer. Als u een back-up van niet-beheerde schijven:
    - Zorg ervoor dat u een back-upbeleid toepassen op virtuele machines die zijn verdeeld over meerdere opslagaccounts.
    - Niet meer dan 20 schijven van een enkel opslagaccount moeten worden beveiligd door de dezelfde back-upschema.
    - Als u meer dan 20 schijven in een storage-account hebt, moet u deze virtuele machines verdeeld over meerdere beleid voor het ophalen van de vereiste IOPS-waarde tijdens de fase van de overdracht van het back-upproces.
    - Een virtuele machine uitgevoerd op de premium-opslag hetzelfde opslagaccount niet worden hersteld. Als het herstelproces bewerking met de back-upbewerking overeenkomt, vermindert het beschikbare IOP's voor back-up.
    - U moet slechts 50% van de totale opslagruimte voor de account voor de VM voor Premium-back-up van VM-back-upstack V1 toewijzen, zodat de Backup-service kunt kopiëren van de momentopname naar storage-account en gegevens van het opslagaccount naar de kluis overbrengen.
- Het verdient aanbeveling om te gebruiken van verschillende opslagaccounts in plaats van de dezelfde opslagaccounts om te herstellen van virtuele machines uit een enkele kluis. Dit wordt voorkomen dat beperking en leiden tot 100% terugzetten geslaagd en goede prestaties.
- Het terugzetten van laag 1-opslaglaag wordt in minuten op basis van de laag 2 opslag herstelbewerkingen waarbij paar uur voltooid. We raden u aan het gebruik van [Instant RP-functie](backup-upgrade-to-vm-backup-stack-v2.md) voor sneller herstellen. Dit is alleen van toepassing beheerde Azure-VM's.


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

- Download de [Excel-werkblad voor capaciteitsplanning](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) voor het uitproberen van schijf- en back-up plannen getallen.
- [Meer informatie over](../virtual-machines/windows/premium-storage-performance.md) afstemmen van apps voor optimale prestaties met Azure storage. Het artikel gericht op premium-opslag, maar het is ook van toepassing op standard-opslagschijven.
- [Aan de slag](backup-azure-arm-vms-prepare.md) met back-up aan de hand van de VM-ondersteuning en beperkingen, het maken van een kluis en virtuele machines voorbereiden voor back-up.

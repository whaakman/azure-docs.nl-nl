---
title: Over Azure VM backup
description: Meer informatie over Azure VM backup en houd er rekening mee enkele aanbevolen procedures.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: c331c3617f421c913abbc3554aa5ba17e86cb978
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429174"
---
# <a name="about-azure-vm-backup"></a>Over Azure VM backup

Dit artikel wordt beschreven hoe u de [Azure Backup-service](backup-introduction-to-azure-backup.md) back-ups van virtuele Azure-machines.

## <a name="backup-process"></a>Back-upproces

Hier ziet u hoe Azure Backup aangeeft dat een back-up is voltooid voor virtuele Azure-machines.

1. Voor Azure VM's die zijn geselecteerd voor back-up, initieert de Azure Backup-service een back-uptaak in overeenstemming met de back-upschema die u opgeeft.
2. Een Backup-extensie is geïnstalleerd op virtuele machine tijdens de eerste back-up, als deze wordt uitgevoerd.
    - Voor Windows-VM's de _VMSnapshot_ -extensie is geïnstalleerd.
    - Voor Linux-VM's de _VMSnapshotLinux_ -extensie is geïnstalleerd.
3. Voor Windows-VM's die worden uitgevoerd, coördineert de back-ups met VSS op een app-consistente momentopname te maken van de virtuele machine.
    - Back-up wordt standaard volledige VSS-back-ups.
    - Als de back-up is niet een app-consistente momentopname te maken, en vervolgens het duurt een bestand-consistente momentopname van de onderliggende opslag voordat (aangezien er geen schrijfbewerkingen van toepassingen plaatsvinden terwijl de virtuele machine is gestopt).
4. Voor Linux VM's back-up wordt een bestandsconsistente back-up. U moet handmatig aanpassen voor/na-scripts voor app-consistente momentopnamen.
5. Nadat de momentopname is gemaakt worden de gegevens overgedragen naar de kluis. 
    - Back-up is geoptimaliseerd door de back-ups van elke VM-schijf parallel.
    - Voor elke schijf een back-wordt gemaakt, Azure Backup leest de blokken op schijf, en identificeert en brengt alleen gegevensblokken die zijn gewijzigd sinds de vorige back-up (delta).
    - Nadat de momentopname wordt gemaakt, worden de gegevens overgedragen naar de kluis.
    - Momentopname van de gegevens mogelijk niet direct worden gekopieerd naar de kluis. Het duurt enkele uren tijdens pieken. Totaal aantal back-uptijd voor een virtuele machine worden kleiner dat 24 uur voor dagelijkse back-upbeleid.

6. Wanneer de gegevensoverdracht is voltooid, wordt de momentopname verwijderd en wordt er een herstelpunt gemaakt.

![Virtuele machine van Azure back-architectuur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encrypting-azure-vm-backups"></a>Back-ups van virtuele Azure-machine versleutelen

Wanneer u back-up van virtuele Azure-machines met Azure Backup, worden virtuele machines in rust versleuteld met Storage Service Encryption (SSE). Bovendien kan Azure Backup back-up van Azure-VM's die zijn versleuteld met behulp van Azure Disk Encryption (ADE).


**Versleuteling** | **Details** | **Ondersteuning**
--- | --- | ---
**ADE** | ADE versleutelt codeert zowel besturingssysteem en gegevensschijven voor virtuele Azure-machines.<br/><br/> ADE kan worden geïntegreerd met BitLocker-versleutelingssleutels (BEK) beveiligd in een key vault als geheimen, of met Azure Key Vault-sleutel-versleutelingssleutels (KEK-sleutel). | Azure Backup biedt ondersteuning voor back-up van beheerde en niet-beheerde Azure-VM's met BEK alleen of met de BEK samen met de KEK versleuteld.<br/><br/> Beide BEK en een back-up en versleuteld.<br/><br/> Sinds de KEK-sleutel en BEK back-ups, indien nodig gebruikers met machtigingen kunt herstellen van sleutels en geheimen terug naar de key vault en de versleutelde VM herstellen.<br/><br/> Versleutelde sleutels en geheimen kunnen niet worden gelezen door onbevoegde gebruikers, of door Azure.
**SSE** | Met SSE, Azure storage biedt versleuteling ' at rest ' door het versleutelen van gegevens automatisch voordat u opslaat en ontsleutelt deze voordat ophalen. | Azure Backup gebruikt SSE voor versleuteling bij rust virtuele Azure-machines.

- Back-up van virtuele machines die zijn versleuteld met BitLocker-versleuteling sleutel (BEK) alleen en BEK samen met Key(KEK) voor Key-versleuteling wordt ondersteund voor beheerde en niet-beheerde Azure-VM's.
- De back-ups van BEK (geheimen) en de KEK-sleutel (sleutels) worden versleuteld. Ze kunnen lezen en wordt alleen gebruikt wanneer hersteld terug naar de sleutelkluis door gemachtigde gebruikers. Noch Azure als niet-geautoriseerde gebruikers kan lezen of gebruik back-ups van sleutels of geheimen.
- Omdat de BEK ook back-up gemaakt wordt als de BEK verbroken wordt, kunnen geautoriseerde gebruikers de BEK herstellen naar de Key Vault en de versleutelde VM herstellen. 
- Alleen gebruikers met het juiste machtigingsniveau kunnen een back-up en herstellen van versleutelde virtuele machines, evenals sleutels en geheimen.



## <a name="taking-snapshots"></a>Maken van momentopnamen

Azure Backup-momentopnamen in overeenstemming met de back-upschema. 

- **Virtuele Windows-machines**: Voor Windows-VM's coördineert de Backup-service met de Volume Shadow Copy Service (VSS) naar een app-consistente momentopname te maken van de VM-schijven.
    - Azure Backup wordt standaard volledige VSS-back-ups. [Meer informatie](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Als u de instelling wijzigen wilt zodat Azure back-ups VSS kopieback-ups neemt, stelt u de volgende registersleutel vanaf een opdrachtprompt: **REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d waar /f**.
- **Virtuele Linux-machines**: Als u wilt de app-consistente momentopnamen maken van Linux-VM, gebruikt u de Linux-Pre-script en na script framework aan uw eigen aangepaste scripts om te zorgen voor consistentie kan schrijven.
    -  Azure Backup roept alleen de door u geschreven vooraf/post-scripts.
    - Als de Pre-script en scripts die volgen is uitvoert, markeert Azure Backup het herstelpunt als toepassingsconsistente. U kunt echter verantwoordelijk is voor de toepassingsconsistentie bij het gebruik van aangepaste scripts.
    - [Meer informatie](backup-azure-linux-app-consistent.md) over het configureren van scripts.


### <a name="snapshot-consistency"></a>Consistentie van de momentopname

De volgende tabel beschrijft de verschillende typen consistentie.

**momentopname** | **Details** | **Recovery** | **Overweging**
--- | --- | --- | ---
**Toepassingsconsistent** | App-consistente back-ups vastleggen geheugen inhoud en i/o-bewerkingen in behandeling. App-consistente momentopnamen gebruiken VSS-schrijver (of pre-/ post-script voor Linux) die ervoor zorgen dat de consistentie van de app-gegevens voordat u een back-up plaatsvindt. | Tijdens het herstellen van met een app-consistente momentopname, de VM wordt opgestart. Er is geen beschadigde gegevens of gegevensverlies. De apps starten in een consistente status. | Windows: Alle VSS-schrijvers is voltooid<br/><br/> Linux: Vooraf/post zijn scripts geconfigureerd en is voltooid
**Bestandssysteemconsistent** | Bestand toepassingsconsistente back-ups bieden consistente back-ups van schijf-bestanden door het maken van een momentopname van alle bestanden op hetzelfde moment.<br/><br/> | Tijdens het herstellen van met een bestand-consistente momentopname, de VM wordt opgestart. Er is geen beschadigde gegevens of gegevensverlies. Apps moeten voor het implementeren van hun eigen mechanisme 'bijwerken' om ervoor te zorgen dat de herstelde gegevens consistent is. | Windows: Bepaalde VSS-schrijvers is mislukt <br/><br/> Linux: Standaard (als pre/post-scripts zijn niet geconfigureerd of mislukt)
**Crash-consistente** | Crashconsistentie gebeurt vaak als een Azure-VM uitgeschakeld op het moment van back-up wordt.  Alleen de gegevens die al op de schijf op het moment van back-up bestaat is vastgelegd en een back-up.<br/><br/> Een crash-consistente herstelpunt biedt geen garantie voor de consistentie van gegevens voor het besturingssysteem of de app. | Er zijn geen garanties, maar meestal de VM wordt opgestart, en volgt met een schijf controleren op Beschadigingsfouten te herstellen. Een in-memory-gegevens of schrijven die niet zijn overgebracht naar schijf gaan verloren. Apps implementeren voor de controle van hun eigen gegevens. Bijvoorbeeld, voor een database-app als een transactielogboek heeft vermeldingen die zich niet in de database, de databasesoftware wordt getotaliseerd tot gegevens consistent is. | Virtuele machine zich in afsluitingsstaat


## <a name="restore-considerations"></a>Overwegingen met betrekking tot herstellen 



**Overweging** | **Details**
--- | ---
**Schijf** | Back-up van VM-schijf is parallelle. Bijvoorbeeld, als een virtuele machine vier schijven heeft, probeert de service naar de back-up van alle vier schijven tegelijk. Back-up is incrementeel (alleen de gewijzigde gegevens).
**Planning** |  Back-up om verkeer te beperken, maakt u een back-up van verschillende virtuele machines op verschillende tijdstippen van de dag, met geen overlapt. Back-ups van virtuele machines op hetzelfde moment zorgt ervoor dat verkeer vastlopen.
**Back-ups voorbereiden** | Houd rekening met de back-up voorbereidingstijd, waaronder installeren of bijwerken van de Backup-extensie en activeren van een momentopname in overeenstemming met de back-upschema.
**Gegevensoverdracht** | De tijd die nodig is voor de Backup-service voor het berekenen van de incrementele wijzigingen van de vorige back-up.<br/><br/> In een incrementele back-up, bepaalt u de wijzigingen van de service-computers de controlesom van het blok. Als een blok de geïdentificeerde voor het verzenden naar de kluis wordt gewijzigd. De service-oefeningen in geïdentificeerde blokken proberen te verder zo min mogelijk de gegevens over te dragen. Nadat de evaluatie van alle geblokkeerde gewijzigd worden de wijzigingen worden overgebracht naar de kluis.<br/><br/> Er is mogelijk een vertraging tussen het maken van de momentopname en kopieert deze naar de kluis.<br/><br/> Tijdens piekuren duurt het tot acht uur voor back-ups moet verwerken. De back-uptijd voor een virtuele machine is minder dan 24 uur voor de dagelijkse back-up.
**Eerste back-up** | Hoewel de totale back-uptijd van minder dan 24 uur geldig voor incrementele back-ups is, kan deze niet worden voor de eerste back-up. Tijd die nodig zijn afhankelijk van de grootte van de gegevens en wanneer de back-up wordt gemaakt.
**Herstellen van wachtrij** | Verwerkt door Azure back-up herstellen taken van meerdere opslagaccounts op hetzelfde moment en aanvragen voor herstellen in een wachtrij worden geplaatst.
**Kopie** | Tijdens het herstelproces worden gegevens gekopieerd uit de kluis naar het opslagaccount.<br/><br/> Herstellen tijd is afhankelijk van IOPS en doorvoer van het opslagaccount.<br/><br/> Als u wilt kopiëren op hoelang, selecteer een opslagaccount die niet is geladen met andere schrijfbewerkingen van toepassingen en leesbewerkingen.


### <a name="backup-performance"></a>Back-upprestaties

Deze algemene scenario's kunnen invloed hebben op de back-up:

- Een nieuwe schijf toevoegen aan een beveiligde virtuele machine van Azure: Als een virtuele machine incrementele back-up ondergaat en een nieuwe schijf wordt toegevoegd, kan back-up vanwege de initiële replicatie van de nieuwe schijf, samen met de replicatie van verschillen van bestaande schijven meer dan 24 uur duren.
- Gefragmenteerde schijven: Back-upbewerkingen zijn sneller wanneer schijfwijzigingen zijn geplaatst. Als wijzigingen worden verdeeld en gefragmenteerd over een schijf, worden back-up van binnenkomst. 
- Verloop van de schijf: Als beveiligde schijven die een incrementele back-up hebt een dagelijkse verloop van meer dan 200 GB, kan back-up lang duren (meer dan acht uur) om te voltooien. 
- Back-versies: Als u de nieuwste versie van de back-up (bekend als de versie direct herstellen), wordt een meer geoptimaliseerde proces dan vergelijking van de controlesom voor het vergelijken van wijzigingen. Als u de meest recente versie en een momentopname van een back-up hebt verwijderd, wordt de back-switches te gebruiken van de vergelijking van de controlesom en de back-upbewerking meer dan 24 uur (of mislukken).

## <a name="best-practices"></a>Aanbevolen procedures 
Het is raadzaam na deze procedures tijdens het configureren van back-ups van virtuele machine:

- Houd rekening met het wijzigen van de tijd voor het plannen van standaard ingesteld in een beleid. Bijvoorbeeld, als de standaardwaarde dat het beleid is 12:00 uur is, kunt u verhogen door minuten zodat resources optimaal gebruikt.
- Voor Premium-VM kan worden back-up op niet - directe RP-functie ~ 50% van de totale opslagruimte voor de account toegewezen. Back-upservice is vereist voor deze ruimte het kopiëren van de momentopname naar hetzelfde opslagaccount en voor het overdragen van het naar de kluis.
- Voor het herstellen van virtuele machines vanuit één kluis, is het raadzaam om te gebruiken verschillende [v2-accounts voor opslag](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) om te controleren of het doelopslagaccount niet te maken met beperkingen. Elke virtuele machine moet bijvoorbeeld verschillende storage-account (als 10 virtuele machines worden hersteld en vervolgens 10 verschillende opslagaccounts kunt u overwegen) hebben.
- Het terugzetten van de opslaglaag Tier-1 (snapshot) wordt voltooid in minuten (omdat het is hetzelfde opslagaccount) op basis van de opslaglaag van laag 2 (kluis) die kan uren duren. We raden u aan het gebruik van [direct herstellen](backup-instant-restore-capability.md) functie voor snellere herstelacties voor gevallen waarin gegevens beschikbaar in Tier-1 zijn (als de gegevens worden hersteld vanuit kluis en vervolgens het duurt tijd).
- De limiet voor het aantal schijven per opslagaccount gebruikt, is ten opzichte van hoe zware de schijven worden gebruikt door toepassingen die worden uitgevoerd op IaaS-VM. Controleer of als meerdere schijven worden gehost op een enkel opslagaccount. Als het algemeen als 5-10-schijven of meer aan één opslagaccount aanwezig zijn, verdeelt u de taken door te verplaatsen van een aantal schijven voor het scheiden van storage-accounts.

## <a name="backup-costs"></a>Kosten voor back-up

Virtuele machines in Azure een back-up met Azure Backup zijn onderhevig aan [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

- Facturering start niet totdat de eerste geslaagde back-up is voltooid. Op dit moment de facturering voor zowel opslag als beveiligde VM's wordt gestart.
- Facturering geldt zolang er back-upgegevens die zijn opgeslagen in een kluis voor de virtuele machine is. Als u de beveiliging voor een virtuele machine stopt, maar back-upgegevens voor de virtuele machine in een kluis bestaat, blijft de facturering.
- Facturering voor een opgegeven virtuele machine stopt alleen als de beveiliging is gestopt, en alle back-upgegevens wordt verwijderd.
- Wanneer de beveiliging stopt en er zijn geen actieve back-taken, de grootte van de laatste geslaagde VM back-up wordt de grootte van beveiligd exemplaar gebruikt voor de maandelijkse factuur.
- De berekening van de beveiligde exemplaren is gebaseerd op de *werkelijke* grootte van de virtuele machine, dit de som van alle gegevens in de virtuele machine is, met uitzondering van de tijdelijke opslag.
- Prijzen zijn gebaseerd op de werkelijke hoeveelheid gegevens die zijn opgeslagen in de gegevensschijf.
- Prijzen voor back-ups van virtuele machines zijn niet gebaseerd op de maximale ondersteunde grootte voor elke gegevensschijf die is gekoppeld aan de virtuele machine.
- Op dezelfde manier wordt het back-upopslag-factuur gebaseerd op de hoeveelheid gegevens die zijn opgeslagen in Azure Backup, is de som van de werkelijke gegevens in elk herstelpunt.

Neem bijvoorbeeld een Standard A2-grootte VM waarvoor twee extra gegevensschijven met een maximale grootte van 4 TB. De volgende tabel bevat de werkelijke hoeveelheid gegevens die zijn opgeslagen op elk van deze schijven:

**Schijf** | **Maximale grootte** | **Werkelijke gegevens aanwezig**
--- | --- | ---
Besturingssysteemschijf | 4095 GB | 17 GB 
Lokale/tijdelijke schijf | 135 GB | 5 GB (niet inbegrepen voor back-up) 
Gegevensschijf 1 | 4095 GB | 30 GB 
Gegevensschijf 2 | 4095 GB | 0 GB 

- De werkelijke grootte van de virtuele machine is in dit geval 17 GB + 30 GB + 0 GB = 47 GB.
- Deze grootte van beveiligd exemplaar (47 GB), wordt de basis voor de maandelijkse factuur.
- Naarmate de hoeveelheid gegevens in de virtuele machine toeneemt, worden de grootte van beveiligd exemplaar dienovereenkomstig gebruikt voor facturering wijzigingen.


## <a name="next-steps"></a>Volgende stappen

Nu [voorbereiden](backup-azure-arm-vms-prepare.md) voor Azure VM backup.

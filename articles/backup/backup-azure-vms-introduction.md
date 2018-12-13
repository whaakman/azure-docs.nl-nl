---
title: Planning van uw VM-back-upinfrastructuur in Azure
description: Belangrijke overwegingen bij het plannen van back-up van virtuele machines in Azure
services: backup
author: rayne-wiselman
manager: carmonm
keywords: back-up van VM's, back-up van virtuele machines
ms.service: backup
ms.topic: conceptual
ms.date: 8/29/2018
ms.author: raynew
ms.openlocfilehash: 7ebb71c6c5968f8f3548f1accd8d659039e6b545
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871639"
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>De infrastructuur voor back-ups van virtuele Azure-machines plannen
In dit artikel biedt prestaties en suggesties voor het plannen van uw VM-back-upinfrastructuur resource. Het definieert ook belangrijke aspecten van de Backup-service; deze aspecten kunnen essentieel bij het bepalen van uw architectuur zijn plannen van capaciteit en planning. Als u hebt [bereid uw omgeving](backup-azure-arm-vms-prepare.md), planning van de volgende stap is voordat u begint met [back-up van virtuele machines](backup-azure-arm-vms.md). Als u meer informatie over Azure virtual machines, Zie de [documentatie voor virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Dit artikel is bedoeld voor gebruik met beheerde en onbeheerde schijven. Als u niet-beheerde schijven gebruikt, zijn er aanbevelingen voor opslagaccount. Als u [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md), u hoeft te breken over problemen met prestaties of resource-gebruik. Azure wordt geoptimaliseerd gebruik van de opslag voor u.
>

## <a name="how-does-azure-back-up-virtual-machines"></a>Hoe werkt Azure back-up van virtuele machines?
Wanneer de Azure Backup-service wordt gestart een back-uptaak op het geplande tijdstip, de triggers service de back-upextensie om een point-in-time-momentopname. De Azure Backup-service gebruikt de _VMSnapshot_ -extensie in Windows, en de _VMSnapshotLinux_ -extensie in Linux. De extensie wordt geïnstalleerd tijdens de eerste virtuele machine back-up. Als u wilt de extensie installeren, moet de virtuele machine worden uitgevoerd. Als de VM niet actief is, wordt met de Backup-service een momentopname gemaakt van de onderliggende opslag (aangezien er geen schrijfbewerkingen van toepassingen plaatsvinden als de VM is gestopt).

Wanneer er een momentopname van virtuele Windows-machines wordt gemaakt, werkt de Backup-service samen met de Volume Shadow Copy Service (VSS) om een consistente momentopname van de schijven van de virtuele machine op te halen. Als u een back-up van Linux-VM's, kunt u uw eigen aangepaste scripts om te zorgen voor consistentie bij het maken van een VM-momentopname schrijven. Meer informatie over het aanroepen van deze scripts vindt u verderop in dit artikel.

Nadat de momentopname is gemaakt met de Azure Backup-service, worden de gegevens overgedragen naar de kluis. Voor maximale efficiëntie wordt met de service geïdentificeerd welke gegevensblokken sinds de vorige back-up zijn gewijzigd. Alleen deze worden vervolgens overgedragen.

![Virtuele machine van Azure back-architectuur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Wanneer de gegevensoverdracht is voltooid, wordt de momentopname verwijderd en wordt er een herstelpunt gemaakt.

> [!NOTE]
> 1. Tijdens de back-upproces Azure Backup geen de tijdelijke schijf die is gekoppeld aan de virtuele machine. Zie voor meer informatie de blog over [tijdelijke opslag](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Azure Backup maakt een momentopname van de opslag op serverniveau en brengt het die momentopname naar de kluis. De opslagaccountsleutels niet te wijzigen totdat de back-uptaak is voltooid.
>

### <a name="data-consistency"></a>Gegevensconsistentie
Back-ups maken en herstellen van bedrijven die essentiële gegevens wordt bemoeilijkt door het feit dat kritieke bedrijfsgegevens moet een back-up terwijl de toepassingen die de gegevens produceren die worden uitgevoerd. Om dit op te lossen, Azure Backup biedt ondersteuning voor toepassingsconsistente back-ups voor zowel Windows als Linux-VM 's
#### <a name="windows-vm"></a>Windows-VM
Azure Backup VSS volledige back-ups neemt op Windows-VM's (meer informatie over [VSS volledige back-up](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). Om in te schakelen VSS kopieback-ups, moet de volgende registersleutel worden ingesteld op de virtuele machine.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Virtuele Linux-machines

Azure Backup biedt een framework voor uitvoeren van scripts voor het beheren van de back-werkstroom en de omgeving. Om te controleren of toepassingsconsistente back-ups van Linux-VM, het uitvoeren van scripts framework te gebruiken om aangepaste scripts die voorafgaan aan en scripts die volgen op te maken. Het script voorafgaand aanroepen voordat u de VM-momentopname en vervolgens het script dat volgt aanroepen nadat de virtuele machine de dynamische-momentopnametaak is voltooid. Zie voor meer informatie het artikel [application consistent Linux VM-back-ups](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

> [!NOTE]
> Azure Backup alleen roept de klant geschreven vóór en na-scripts. Als de Pre-script en scripts die volgen met succes uitvoert, markeert Azure Backup het herstelpunt dat als application consistent. De klant is echter verantwoordelijk is voor de toepassingsconsistentie bij het gebruik van aangepaste scripts.
>

De volgende tabel beschrijft de soorten consistentie en de voorwaarden wanneer ze zich voordoen.

| Consistentie | Op basis van VSS | Uitleg en details |
| --- | --- | --- |
| Consistentie van toepassingen |Ja voor Windows|Consistentie van toepassingen is ideaal voor workloads zoals zorgt ervoor dat:<ol><li> De virtuele machine *opgestart*. <li>Er is *geen beschadiging*. <li>Er is *zonder verlies van gegevens*.<li> De gegevens is consistent zijn om de toepassing die gebruikmaakt van de gegevens door met betrekking tot de toepassing op het moment van de back-up--met behulp van VSS of vooraf/post-script.</ol> <li>*Windows-VM's*-meest Microsoft-werkbelastingen hebben VSS-schrijvers die workload-specifieke acties die betrekking hebben op de consistentie van gegevens worden uitgevoerd. Bijvoorbeeld, de SQL Server VSS writer zorgt ervoor dat de schrijfbewerkingen naar het logboekbestand van de transactie en de database, correct worden uitgevoerd. Voor IaaS Windows-VM back-ups, voor het maken van een App-consistente herstelpunt, moet de Backup-extensie aanroepen van de VSS-werkstroom, en deze voltooien voordat u de VM-momentopname. Voor de Azure VM-momentopname nauwkeurige, moeten de VSS-schrijvers van alle Azure-VM-toepassingen ook uitvoeren. (Informatie over de [basisprincipes van VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) en dieper in op de details van [hoe het werkt](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Virtuele Linux-machines*-klanten kunnen uitvoeren [aangepaste Pre-script en post-script om ervoor te zorgen toepassingsconsistentie](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Consistentie van bestandssysteem |Ja, voor Windows-computers |Er zijn twee scenario's waarbij het herstelpunt kan zijn *bestandssysteemconsistent*:<ul><li>Back-ups van virtuele Linux-machines in Azure, zonder de pre-script/na-script of als pre-script/na-script is mislukt. <li>VSS-fout tijdens back-up voor Windows-VM's in Azure.</li></ul> In beide gevallen is het beste die kan worden uitgevoerd om ervoor te zorgen dat: <ol><li> De virtuele machine *opgestart*. <li>Er is *geen beschadiging*.<li>Er is *zonder verlies van gegevens*.</ol> Toepassingen moeten hun eigen mechanisme 'bijwerken' implementeren op de herstelde gegevens. |
| Crashconsistentie |Nee |Deze situatie is gelijk aan een virtuele machine met een "vastloopt" (via een van beide een zacht of hard reset). Crashconsistentie gebeurt meestal wanneer de virtuele machine van Azure op het moment van back-up wordt afgesloten. Een crash-consistente herstelpunt biedt geen garanties de consistentie van de gegevens op het opslagmedium--vanuit het perspectief van het besturingssysteem of de toepassing. Alleen de gegevens die al op de schijf op het moment van back-up bestaat is vastgelegd en een back-up. <br/> <br/> Terwijl er normaal gesproken geen garanties, het besturingssysteem is opgestart, gevolgd door het controleren van de schijf maakt, zoals chkdsk, eventuele Beschadigingsfouten te herstellen. Een in-memory-gegevens- of schrijfbewerkingen die niet zijn overgebracht naar de schijf gaan verloren. De toepassing, meestal met een eigen mechanisme verificatie volgt als terugdraaien van de gegevens moet worden uitgevoerd. <br><br>Een voorbeeld: als het transactielogboek bevat de invoer niet aanwezig in de database, de databasesoftware wordt getotaliseerd terug tot de gegevens consistent is. Wanneer gegevens verdeeld over meerdere virtuele schijven (zoals spanned volumes), biedt een crash-consistente herstelpunt geen garanties voor de juistheid van de gegevens. |

## <a name="performance-and-resource-utilization"></a>Gebruik van de prestaties en resource
Als de back-upsoftware die on-premises geïmplementeerd is, moet u plannen voor capaciteit en het Resourcegebruik behoeften wanneer back-ups van virtuele machines in Azure. De [Azure Storage beperkt](../azure-subscription-service-limits.md#storage-limits) definiëren hoe u VM-implementaties voor maximale prestaties met minimale gevolgen voor het uitvoeren van workloads.

### <a name="number-of-disks"></a>Aantal schijven
Het back-upproces wil zo snel mogelijk een back-uptaak voltooid. In dat geval, verbruikt dit net zo veel resources zoals dat kan. Echter alle i/o-bewerkingen worden beperkt door de *Doeldoorvoer van één Blob*, heeft een limiet van 60 MB per seconde. In een poging om de snelheid te maximaliseren, de back-upproces wil back-up van elk van de VM schijven *parallel*. Als een virtuele machine vier schijven is, probeert de service naar de back-up van alle vier schijven tegelijk. De **aantal schijven** back-up, is de belangrijkste factor bij het bepalen van de back-upverkeer van storage-account.

### <a name="backup-schedule"></a>Back-upschema
Een extra factor die van invloed is op prestaties, is de **back-upschema**. Als u het beleid configureert, zodat alle virtuele machines op hetzelfde moment worden opgeslagen, kunt u een verkeer is vastgelopen hebt gepland. Het back-upproces wil back-up van alle schijven tegelijk. De back-up om verkeer te beperken, maakt u een back-up van verschillende virtuele machines op andere moment van de dag, met elkaar niet overlappen.

## <a name="capacity-planning"></a>Capaciteitsplanning
Download de [VM back-capaciteitsplanning Excel-werkblad](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) het effect van de schijf en de opties voor back-upschema.

### <a name="backup-throughput"></a>Back-doorvoer
Voor elke schijf een back-wordt gemaakt, Azure Backup leest de blokken op de schijf en slaat alleen de gewijzigde gegevens (incrementele back-up). De volgende tabel toont de gemiddelde doorvoer waarden van de back-up-service. Met behulp van de volgende gegevens, kunt u de hoeveelheid tijd die nodig zijn voor de back-up van een schijf van een bepaalde grootte schatten.

| Back-upbewerking | Het doorvoer |
| --- | --- |
| Eerste back-up |160 Mbps |
| Incrementele back-up (DR) |640 Mbps <br><br> Doorvoer komt aanzienlijk als de gewijzigde gegevens (die moet een back-up) is verspreid over de schijf.|

## <a name="total-vm-backup-time"></a>Totale tijd van de VM-back-up
Hoewel de meeste van de back-uptijd is besteed aan het lezen van en kopiëren van gegevens, bijdragen andere bewerkingen en de totale tijd die nodig zijn voor de back-up van een virtuele machine:

* Tijd die nodig zijn voor [installeren of bijwerken van de back-upextensie](backup-azure-arm-vms.md).
* Momentopname van tijd, dit is de tijd voor het activeren van een momentopname. Momentopnamen zijn geactiveerd dicht bij de geplande back-uptijd.
* Wachttijd in wachtrij. Sinds de back-up service processen taken van meerdere klanten op hetzelfde moment, momentopname van de gegevens mogelijk niet direct worden gekopieerd naar de Recovery Services-kluis. Tijdens piekuren laden duurt het tot acht uur voordat de back-ups worden verwerkt. De totale tijd van de VM-back-up is echter minder dan 24 uur voor dagelijkse back-upbeleid.
Totaal aantal back-uptijd van minder dan 24 uur geldig is voor incrementele back-ups, maar mogelijk niet voor de eerste back-up. Eerste back-uptijd is evenredig en kan niet groter zijn dan 24 uur, afhankelijk van de grootte van de gegevens en wanneer de back-up wordt gemaakt.
* Tijd voor overdracht van gegevens, de tijd die nodig is voor back-upservice compute de incrementele wijzigingen van de vorige back-up en het overdragen van deze wijzigingen naar de kluis van opslag.

### <a name="why-are-backup-times-longer-than-12-hours"></a>Waarom zijn back-uptijden langer is dan 12 uur?

Back-up bestaat uit twee fasen: maken van momentopnamen en het overdragen van de momentopnamen naar de kluis. De Backup-service wordt geoptimaliseerd voor opslag. Bij de overdracht van de momentopname van de gegevens naar een kluis, draagt de service alleen incrementele wijzigingen van de vorige momentopname.  Om te bepalen de incrementele wijzigingen, berekent de service de controlesom van de blokken. Als een blok is gewijzigd, wordt het blok wordt aangeduid als een blok moet worden verzonden naar de kluis. Klik verder de oefeningen van de service in elk van de geïdentificeerde blokken, zoeken naar mogelijkheden om zo min mogelijk de gegevens over te dragen. Nadat u alle gewijzigde blokken, de service de wijzigingen coalesces en stuurt deze naar de kluis. Kleine, gefragmenteerde schrijfbewerkingen zijn niet optimaal voor opslag in enkele oudere toepassingen. Als de momentopname veel kleine, gefragmenteerde schrijfbewerkingen bevat, in de service de taakwachtrij doorbrengt extra tijd die is geschreven door de toepassingen die gegevens verwerkt. Voor toepassingen die binnen de virtuele machine wordt uitgevoerd, is het aanbevolen schrijfbewerkingen van toepassingen blok minimaal 8 KB. Als uw toepassing gebruikmaakt van een blok van minder dan 8 KB, wordt back-upprestaties heeft. Zie voor hulp bij het afstemmen van uw toepassing om back-prestaties te verbeteren, [afstemmen van toepassingen voor optimale prestaties met Azure storage](../virtual-machines/windows/premium-storage-performance.md). Hoewel het artikel op back-upprestaties maakt gebruik van Premium storage-voorbeelden, is de richtlijnen van toepassing op Standard-opslagschijven.<br>
Er zijn verschillende redenen voor de lange back-uptijd:
  1. **Eerste back-up voor een nieuw toegevoegde schijf aan een al beveiligde virtuele machine** <br>
    Als een virtuele machine al is mogelijk een incrementele back-up, wanneer een nieuwe schijf of schijven vervolgens de back-up wordt toegevoegd 1 dag SLA, afhankelijk van de grootte van de nieuwe schijf ontbreken.
  2. **Fragmentatie** <br>
    Als de Klanttoepassing is niet goed geconfigureerd die kleine gefragmenteerde schrijfbewerkingen.<br>
  3. **Klantopslagaccount is overbelast** <br>
      a. Als de back-up is gepland tijdens productie toepassing de tijd van de klant.  
      b. Als meer dan 5 tot 10-schijven worden gehost in hetzelfde opslagaccount.<br>
  4. **Consistentie Check(CC) modus** <br>
      > 1 TB schijven, als de back-up in de CC-modus vanwege de redenen gebeurt vermeld hieronder:<br>
        a. De beheerde schijf wordt verplaatst als onderdeel van de klant virtuele machine opnieuw opstarten.<br>
        b. Klant bevordert momentopname naar base blob.<br>

## <a name="total-restore-time"></a>Totaal aantal hersteltijd

Een herstelbewerking bestaat uit twee belangrijke taken: kopiëren van gegevens van de kluis naar het opslagaccount gekozen klant en het maken van de virtuele machine. De tijd die nodig is om gegevens te kopiëren uit de kluis is afhankelijk van waar de back-ups worden opgeslagen in Azure en de locatie van het opslagaccount van de klant. Afhankelijk van de tijd om gegevens te kopiëren:
* Wachttijd in wachtrij - omdat de service wordt verwerkt het herstellen van taken van meerdere klanten op hetzelfde moment, restore-aanvragen zijn in een wachtrij plaatsen.
* Het kopiëren van gegevens tijd - gegevens worden gekopieerd uit de kluis naar het opslagaccount van de klant. Herstellen tijd is afhankelijk van IOPS en doorvoer van Azure Backup-service worden opgehaald op de geselecteerde klant storage-account. Als u wilt kopiëren hoelang tijdens het herstelproces, selecteer het opslagaccount dat niet geladen met andere schrijfbewerkingen van toepassingen en leesbewerkingen.

## <a name="best-practices"></a>Aanbevolen procedures

Het is raadzaam de volgende deze procedures tijdens het configureren van back-ups voor alle virtuele machines:

* Geen back-ups plannen voor meer dan 10 klassieke virtuele machines van de dezelfde cloudservice op hetzelfde moment. Als u wilt spreiden maakt u een back-up van meerdere virtuele machines in dezelfde cloudservice, de back-up keer starten door een uur.
* Geen back-ups plannen voor meer dan 100 virtuele machines van één kluis op hetzelfde moment.
* Virtuele machine back-ups plannen tijdens daluren. Op deze manier de Backup-service maakt gebruik van IOP's voor het overdragen van gegevens van het opslagaccount van de klant naar de kluis.
* Zorg ervoor dat virtuele Linux-machines voor back-up is ingeschakeld, hebben Python versie 2.7 of hoger.

### <a name="best-practices-for-vms-with-unmanaged-disks"></a>Aanbevolen procedures voor virtuele machines met niet-beheerde schijven

De volgende aanbevelingen gelden alleen voor virtuele machines met behulp van niet-beheerde schijven. Als uw virtuele machines met beheerde schijven, verwerkt de Backup-service alle beheertaken voor opslag.

* Zorg ervoor dat u een back-upbeleid toepassen op virtuele machines die zijn verdeeld over meerdere opslagaccounts. Niet meer dan 20 totaal aantal schijven van een enkel opslagaccount moeten worden beveiligd door de dezelfde back-upschema. Als u meer dan 20 schijven in een storage-account hebt, moet u deze virtuele machines verdeeld over meerdere beleid voor het ophalen van de vereiste IOPS-waarde tijdens de fase van de overdracht van het back-upproces.
* Een virtuele machine die worden uitgevoerd op Premium storage hetzelfde opslagaccount niet worden hersteld. Als het herstelproces bewerking met de back-upbewerking overeenkomt, vermindert het beschikbare IOP's voor back-up.
* U moet slechts 50% van de totale opslagruimte voor de account voor de VM voor Premium-back-up van VM-back-upstack V1 toewijzen, zodat de Backup-service kunt kopiëren van de momentopname naar storage-account en gegevens van het opslagaccount naar de kluis overbrengen.


## <a name="data-encryption"></a>Gegevensversleuteling
Azure Backup niet-versleuteld gegevens als onderdeel van het back-upproces. U kunt echter versleutelen van gegevens binnen de virtuele machine en back-up van de beveiligde gegevens naadloos (meer informatie over [back-ups van versleutelde gegevens](backup-azure-vms-encryption.md)).

## <a name="calculating-the-cost-of-protected-instances"></a>Berekenen van de kosten van beveiligde exemplaren
Azure virtuele machines die worden ondersteund door Azure Backup zijn onderhevig aan [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/). De berekening van de beveiligde instanties wordt gebaseerd op de *werkelijke* grootte van de virtuele machine, de som van alle gegevens in de virtuele machine is, met uitzondering van de tijdelijke opslag.

Prijzen voor back-ups van virtuele machines zijn niet gebaseerd op de maximale ondersteunde grootte voor elke gegevensschijf die is gekoppeld aan de virtuele machine. Prijzen zijn gebaseerd op de werkelijke hoeveelheid gegevens die zijn opgeslagen in de gegevensschijf. Op dezelfde manier wordt het back-upopslag-factuur gebaseerd op de hoeveelheid gegevens die zijn opgeslagen in Azure Backup, is de som van de werkelijke gegevens in elk herstelpunt.

Neem bijvoorbeeld een Standard A2-grootte virtuele machine waarvoor twee extra gegevensschijven met een maximale grootte van 4 TB. De volgende tabel bevat de werkelijke hoeveelheid gegevens die zijn opgeslagen op elk van deze schijven:

| Schijftype | Max. grootte | Werkelijke gegevens aanwezig |
| --------- | -------- | ----------- |
| Besturingssysteemschijf |4095 GB |17 GB |
| Lokale schijf / tijdelijke schijf |135 GB |5 GB (niet inbegrepen voor back-up) |
| Gegevensschijf 1 |4095 GB |30 GB |
| Gegevensschijf 2 |4095 GB |0 GB |

De werkelijke grootte van de virtuele machine is in dit geval 17 GB + 30 GB + 0 GB = 47 GB. De grootte van deze beveiligde instantie (47 GB), wordt de basis voor de maandelijkse factuur. Naarmate de hoeveelheid gegevens in de virtuele machine groeit, de grootte van de beveiligde instantie die wordt gebruikt voor facturering wijzigingen dienovereenkomstig.

Facturering start niet totdat de eerste geslaagde back-up is voltooid. Op dit moment de facturering voor zowel opslag als beveiligde instanties wordt gestart. Facturering geldt zolang er back-upgegevens die zijn opgeslagen in een kluis voor de virtuele machine is. Als u de beveiliging voor de virtuele machine stoppen, maar back-upgegevens van virtuele machine in een kluis bestaat, blijft de facturering.

Facturering voor een opgegeven virtuele machine stopt alleen als de beveiliging is gestopt en alle back-upgegevens wordt verwijderd. Wanneer de beveiliging stopt en er zijn geen actieve back-taken, de grootte van de laatste geslaagde VM back-up wordt de grootte van de beveiligde instantie die wordt gebruikt voor de maandelijkse factuur.

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen ziet worden, [stuur ons dan uw feedback](https://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Volgende stappen
* [Back-up maken van virtuele machines](backup-azure-arm-vms.md)
* [Back-up van virtuele machine beheren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-arm-restore-vms.md)
* [VM-back-problemen](backup-azure-vms-troubleshoot.md)

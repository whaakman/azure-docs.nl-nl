---
title: Planning van uw back-upinfrastructuur VM in Azure | Microsoft Docs
description: Belangrijke overwegingen bij het plannen van de back-up van virtuele machines in Azure
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up van virtuele machines, back-up van virtuele machines
ms.assetid: 19d2cf82-1f60-43e1-b089-9238042887a9
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: 0c930c7413b24a811707c3a1ff3d7d70585bc528
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>De infrastructuur voor back-ups van virtuele Azure-machines plannen
In dit artikel biedt de prestaties en suggesties voor het plannen van uw back-upinfrastructuur VM resource. Definieert ook belangrijke aspecten van de Backup-service; deze aspecten kunnen essentieel bij het bepalen van uw architectuur zijn capaciteitsplanning en planning. Als u hebt [uw omgeving voorbereid](backup-azure-vms-prepare.md), is de volgende stap plannen voordat u begint met [back-up van virtuele machines](backup-azure-vms.md). Als u meer informatie over virtuele machines in Azure nodig hebt, raadpleegt u de [documentatie Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Hoe biedt Azure back-up van virtuele machines?
Wanneer een back-uptaak de Azure Backup-service op het geplande tijdstip begint, wordt de Backup-extensie om een momentopname punt in tijd geactiveerd. De Azure Backup-service wordt gebruikt de _VMSnapshot_ de extensie in Windows, en de _VMSnapshotLinux_ extensie in Linux. De uitbreiding wordt geïnstalleerd tijdens de eerste VM back-up. Voor het installeren van de uitbreiding voor moet de virtuele machine worden uitgevoerd. Als de virtuele machine niet wordt uitgevoerd, maakt de Backup-service een momentopname van de onderliggende opslag (omdat er geen schrijfbewerkingen toepassing optreden terwijl de virtuele machine is gestopt).

Wanneer een momentopname maken van VM's van Windows, coördineert de Backup-service met de Volume Shadow Copy Service (VSS) om op te halen van een consistente momentopname van de schijven van de virtuele machine. Als u een back-up van Linux VM's, kunt u uw eigen aangepaste scripts consistentie bij het maken van een VM-momentopname schrijven. Verderop in dit artikel vindt u meer informatie over het aanroepen van deze scripts.

Zodra de Azure Backup-service de momentopname maakt, worden de gegevens worden overgedragen naar de kluis. Als u wilt optimaliseren, wordt de service identificeert en draagt alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up.

![Virtuele machine van Azure Backup-architectuur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Wanneer de overdracht van gegevens voltooid is, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt.

> [!NOTE]
> 1. Tijdens de back-upproces bevat Azure Backup geen de tijdelijke schijf die is gekoppeld aan de virtuele machine. Zie voor meer informatie de blog op [tijdelijke opslag](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Aangezien Azure Backup een momentopname van het niveau van de opslag en brengt het die momentopname naar de kluis, wijzig de opslagaccountsleutels totdat de back-uptaak is voltooid.
> 3. Premium VM's kopiëren we de momentopname naar storage-account. Dit is om ervoor te zorgen dat Azure Backup-service voldoende IOPS opgehaald voor de overdracht van gegevens naar de kluis. Deze extra exemplaar van de opslag is in rekening gebracht volgens de virtuele machine toegewezen grootte. 
>

### <a name="data-consistency"></a>Gegevensconsistentie
Een back-up en herstellen van zakelijke essentiële gegevens wordt bemoeilijkt door het feit dat bedrijfskritieke gegevens moet een back-up tijdens de toepassingen die de gegevens produceren die worden uitgevoerd. Om dit op te lossen, Azure Backup biedt ondersteuning voor toepassingsconsistente back-ups voor zowel Windows als een virtuele Linux-machines
#### <a name="windows-vm"></a>Windows VM
Azure Backup VSS volledige back-ups neemt op VM's van Windows (meer informatie over [VSS volledige back-up](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). De volgende registersleutel moet worden ingesteld op de virtuele machine zodat VSS kopieback-ups.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Virtuele Linux-machines
Azure Backup biedt een scripting-framework. Toepassing een consistent back-ups van virtuele Linux-machines, aangepaste scripts voor vóór en na scripts waarmee de back-werkstroom en de omgeving te maken. Azure Backup roept het Pre-script voordat u de VM-momentopname en roept het script voor na zodra de taak VM-momentopname is voltooid. Zie voor meer informatie [toepassing consistente VM-back-ups met behulp van script voor vóór en na](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Azure Backup alleen roept de klant geschreven vóór en na-scripts. Als de script voor vóór en na scripts uitgevoerd is, markeert Azure Backup het herstelpunt dat als toepassing consistente. De klant is die verantwoordelijk is voor de consistentie van toepassing wanneer u aangepaste scripts.
>


De volgende tabel beschrijft de soorten consistentie en de voorwaarden die ze onder tijdens de virtuele machine van Azure optreden back-up- en herstelprocedures.

| Consistentie | Op basis van VSS | Uitleg en details |
| --- | --- | --- |
| Consistentie van toepassingen |Ja voor Windows|Consistentie van de toepassing is ideaal voor workloads zoals zorgt ervoor dat:<ol><li> De virtuele machine *opgestart*. <li>Er is *geen beschadiging*. <li>Er is *zonder verlies van gegevens*.<li> De gegevens zijn consistent zijn om de toepassing die gebruikmaakt van de gegevens met betrekking tot de toepassing op het moment van back-up--met VSS of vooraf/post script.</ol> <li>*VM's van Windows*-meest Microsoft-werkbelastingen VSS-schrijvers die specifiek acties uitvoeren die betrekking hebben op consistentie van de gegevens hebben. Microsoft SQL Server heeft bijvoorbeeld een VSS-schrijver dat ervoor zorgt dat de geschreven naar het transactielogbestand en de database correct worden uitgevoerd. Voor back-ups van virtuele machine van Windows Azure, voor het maken van een toepassingsconsistente herstelpunt, moet de Backup-extensie de VSS-workflow aanroepen en deze zijn voltooid voordat de VM-momentopname. Voor de Azure VM-momentopname worden nauwkeurige, moeten ook de VSS-schrijvers van alle virtuele machine van Azure-toepassingen uitvoeren. (Meer informatie over de [basisprincipes van VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) en duik diep in de details van [hoe het werkt](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Virtuele Linux-machines*-klanten kunnen uitvoeren [aangepaste script voor vóór en na toepassing consistentie te garanderen](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Consistentie van bestandssysteem |Ja - voor Windows-computers |Er zijn twee scenario's waar het herstelpunt kan zijn *bestandssysteem consistente*:<ul><li>Back-ups van virtuele Linux-machines in Azure, zonder pre-script/na-script of als pre-script/na-script is mislukt. <li>VSS-fout tijdens back-up voor Windows-machines in Azure.</li></ul> In beide gevallen is het beste die kan worden uitgevoerd om ervoor te zorgen dat: <ol><li> De virtuele machine *opgestart*. <li>Er is *geen beschadiging*.<li>Er is *zonder verlies van gegevens*.</ol> Toepassingen moeten hun eigen mechanisme 'bijwerken' implementeren op de herstelde gegevens. |
| Consistentie van de crash |Nee |Deze situatie komt overeen met een virtuele machine met een 'crash' (via ofwel een zacht of hard reset). Crash consistentie gebeurt meestal wanneer de virtuele machine van Azure op het moment van back-up wordt afgesloten. Een crashconsistent herstelpunt biedt geen garanties rond de consistentie van de gegevens op het opslagmedium--vanuit het perspectief van het besturingssysteem of de toepassing. Alleen de gegevens die al op de schijf op het moment van back-up bestaat worden vastgelegd en back-up gemaakt. <br/> <br/> Hoewel er geen garanties, meestal, het besturingssysteem opnieuw is opgestart, gevolgd door het controleren van de schijf procedure, zoals chkdsk eventuele Beschadigingsfouten te herstellen. Alle gegevens in het geheugen of schrijfbewerkingen die niet zijn overgebracht naar de schijf gaan verloren. De toepassing wordt doorgaans met een eigen mechanisme voor verificatie volgt geval terugdraaien van de gegevens moet worden uitgevoerd. <br><br>Een voorbeeld: als het transactielogboek is vermeldingen die niet aanwezig in de database vervolgens de databasesoftware geen terugdraaien tot de gegevens consistent is. Wanneer gegevens worden verspreid over meerdere virtuele schijven (zoals spanned volumes), biedt een crashconsistent herstelpunt geen garanties voor de juistheid van de gegevens. |

## <a name="performance-and-resource-utilization"></a>Prestatie- en -gebruik
Zoals back-upsoftware die geïmplementeerde on-premises, moet u plannen voor de capaciteit en brongebruik behoeften back-ups van virtuele machines in Azure. De [Azure Storage beperkt](../azure-subscription-service-limits.md#storage-limits) structuur van de VM-implementaties voor optimale prestaties met minimale gevolgen voor het uitvoeren van werkbelastingen definiëren.

Let op de volgende Azure Storage-limieten bij het plannen van back-upprestaties:

* Maximum aantal uitgaande per storage-account
* Percentage van totaal aantal aanvragen per storage-account

### <a name="storage-account-limits"></a>Limieten voor opslagaccounts
Back-upgegevens gekopieerd van een opslagaccount, zijn toegevoegd aan de i/o-bewerkingen per seconde (IOPS) en uitgaande (of doorvoer) metrische gegevens van het opslagaccount. Virtuele machines worden op hetzelfde moment ook verbruikt IOPS en doorvoerlimieten. Het doel is om te controleren of de back-up en het verkeer van virtuele machines niet groter is dan de limieten voor opslagaccounts.

### <a name="number-of-disks"></a>Aantal schijven
Het back-upproces probeert een back-uptaak zo snel mogelijk te voltooien. In dat geval, verbruikt deze zo veel resources geplaatst. Echter alle i/o-bewerkingen worden beperkt door de *doel doorvoer voor één Blob*, die heeft een limiet van 60 MB per seconde. In een poging de snelheid maximaliseren, wordt het back-upproces probeert back-up van de VM-schijven *parallel*. Als een virtuele machine vier schijven heeft, probeert de service naar de back-up van alle vier schijven parallel. De **aantal schijven** back-up wordt gemaakt, is de belangrijkste factor bij het bepalen van de back-opslagverkeer-account.

### <a name="backup-schedule"></a>Back-upschema
Een extra factor die invloed op prestaties wordt de **back-upschema**. Als u het beleid configureert zodat alle VM's op hetzelfde moment worden ondersteund, kunt u een verkeer is vastgelopen hebt gepland. Het back-upproces probeert te back-up van alle schijven parallel. Als u het back-verkeer van een opslagaccount, back-up van andere VM's op andere tijd van de dag, zonder overlap.

## <a name="capacity-planning"></a>Capaciteitsplanning
Het samenstellen van de vorige factoren, moet u voor het plannen van de moeten storage-account gebruiken. Download de [VM back-capaciteitsplanning Excel-spreadsheet](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) om te zien van de impact van de schijf en de back-upschema keuzes.

### <a name="backup-throughput"></a>Back-doorvoer
Voor elke schijf back-up wordt gemaakt, Azure Backup leest de blokken op de schijf en slaat alleen de gewijzigde gegevens (incrementele back-up). De volgende tabel toont de gemiddelde doorvoer waarden van de back-up-service. De volgende gegevens kunt u de hoeveelheid tijd die nodig zijn voor de back-up van de schijf van een bepaalde grootte schatten.

| Back-upbewerking | Gunstigste doorvoer |
| --- | --- |
| Eerste back-up |160 Mbps |
| Incrementele back-up (DR) |640 Mbps <br><br> Doorvoer aanzienlijk wordt neergezet als de gewijzigde gegevens (die een back-up moet) verspreid staat in de schijf.|

## <a name="total-vm-backup-time"></a>Totale tijd voor VM-back-up
Hoewel de meeste van de back-tijd is besteed aan het lezen van en kopiëren van gegevens, worden andere bewerkingen bijdragen aan de totale tijd die nodig zijn voor de back-up van een virtuele machine:

* Tijd die nodig zijn voor [installeren of bijwerken van de Backup-extensie](backup-azure-vms.md).
* Momentopname-tijd is de tijd die nodig is voor het activeren van een momentopname. Momentopnamen worden dicht bij de geplande back-uptijd geactiveerd.
* Wachttijd in. Omdat de Backup-service met het verwerken van de back-ups van meerdere klanten, back-upgegevens kopiëren vanuit een momentopname naar de back-up of een Recovery Services-kluis mogelijk niet onmiddellijk starten. In tijden van piek geladen, de wachttijd kunt uitrekken tot acht uur vanwege het aantal back-ups wordt verwerkt. De totale tijd van de VM-back-up is echter minder dan 24 uur voor dagelijkse back-upbeleid.
* Overdrachtstijd gegevens, de tijd die nodig is voor Backup-service voor het berekenen van de incrementele wijzigingen van eerdere back-up en dragen deze wijzigingen naar de opslag-kluis.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Waarom heb ik observeren longer(>12 hours) back-up van tijd?
Back-up bestaat uit twee fasen: maken van momentopnamen en het overdragen van de momentopnamen die naar de kluis. De Backup-service wordt geoptimaliseerd voor opslag. Als u de momentopname gegevensoverdracht naar een kluis, draagt de service alleen incrementele wijzigingen van de vorige momentopname.  Om te bepalen de incrementele wijzigingen, wordt de controlesom van de blokken berekend door de service. Als een blok is gewijzigd, wordt het blok geïdentificeerd als een blok worden verzonden naar de kluis. De service zoomt meer vervolgens naar elk van de geïdentificeerde blokken, zoeken naar mogelijkheden om de gegevens om over te dragen minimaliseren. Na het evalueren van alle gewijzigde blokken worden de service voegt u de wijzigingen samen en zendt deze naar de kluis. Kleine, gefragmenteerde schrijfbewerkingen zijn in sommige oudere toepassingen niet optimaal is voor de opslag. Als de momentopname veel kleine, gefragmenteerde schrijfbewerkingen bevat, de service nodig heeft voor extra tijd verwerken van de gegevens die door de toepassingen worden geschreven. De aanbevolen toepassing schrijven blok van Azure, is voor toepassingen in de virtuele machine worden uitgevoerd, een minimum van 8 KB. Als uw toepassing gebruikmaakt van een blok van minder dan 8 KB, worden back-upprestaties gedaan. Zie voor hulp bij het afstemmen van uw toepassing om back-prestaties te verbeteren, [afstemming van toepassingen voor optimale prestaties met Azure storage](../storage/common/storage-premium-storage-performance.md). Hoewel het artikel over back-upprestaties voorbeelden voor Premium-opslag gebruikt, geldt de richtlijnen voor Standard-opslag-schijven.

## <a name="total-restore-time"></a>Totaal aantal terugzetten
Een herstelbewerking bestaat uit twee belangrijkste sub taken: gegevens terug uit de kluis te kopiëren naar het gekozen klant storage-account en het maken van de virtuele machine. Kopiëren van gegevens van de kluis, is afhankelijk van waar de back-ups op een intern in Azure worden opgeslagen en waar de klant storage-account is opgeslagen. Afhankelijk van de tijd om gegevens te kopiëren:
* Wachttijd - omdat de service wordt verwerkt van meerdere klanten op hetzelfde moment hersteltaken restore-aanvragen zijn in een wachtrij plaatsen.
* Het kopiëren van gegevens keer - gegevens uit de kluis wordt gekopieerd naar het opslagaccount van de klant. Terugzetten tijd is afhankelijk van IOPS en doorvoer Azure Backup-service opgehaald op de geselecteerde klant storage-account. Om het kopiëren tijdens het herstelproces versnellen, selecteer een opslagaccount is niet geladen met een andere toepassing geschreven en gelezen.

## <a name="best-practices"></a>Aanbevolen procedures
We raden na deze procedures bij het configureren van de back-ups voor virtuele machines:

* Niet meer dan 10 klassieke virtuele machines van de cloudservice dezelfde back-up op hetzelfde moment niet plannen. Als u spreiden back-up van meerdere virtuele machines in dezelfde cloudservice, de begintijden van back-up van een uur wilt.
* Kan niet meer dan 40 VM's op hetzelfde moment back-up plannen.
* VM-back-ups plannen tijdens daluren. IOPS op deze manier de Backup-service gebruikt voor het overdragen van gegevens van het opslagaccount van de klant naar de kluis.
* Zorg ervoor dat er een beleid wordt toegepast op virtuele machines die zijn verdeeld over verschillende opslagaccounts. We raden niet meer dan 20 totaal aantal schijven van een enkele storage-account worden beveiligd door dezelfde back-upschema. Als u meer dan 20 schijven in een opslagaccount hebt, moet u deze VMs verdeeld over meerdere beleidsregels voor het ophalen van het vereiste aantal IOPS dat tijdens de fase van de overdracht van het back-upproces.
* Een virtuele machine uitgevoerd op de Premium-opslag hetzelfde opslagaccount niet terugzetten. Als u het herstelproces bewerking samenvalt met de back-upbewerking, vermindert het aantal IOPS dat beschikbaar is voor back-up.
* Premium VM back-up, zorg ervoor dat dit opslagaccount of premium-schijven voor hosts is ten minste 50% vrije ruimte voor het Faseren van momentopname voor een goede back-up. 
* Zorg ervoor dat python-versie op de virtuele Linux-machines ingeschakeld voor back-up 2.7 is

## <a name="data-encryption"></a>Gegevensversleuteling
Azure Backup biedt gegevens als onderdeel van het back-upproces niet coderen. Echter versleutelde gegevens vanuit de virtuele machine en back-up van de beveiligde gegevens naadloos (meer informatie over [back-up van de versleutelde gegevens](backup-azure-vms-encryption.md)).

## <a name="calculating-the-cost-of-protected-instances"></a>Berekenen van de kosten van beveiligde instanties
Azure virtuele machines die worden ondersteund door Azure Backup zijn onderworpen aan [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/). De exemplaren beveiligd berekening is gebaseerd op de *werkelijke* grootte van de virtuele machine, is de som van alle gegevens in de virtuele machine met uitzondering van de resource-schijf.

Prijzen voor back-ups van virtuele machines is *niet* op basis van de maximale ondersteunde grootte voor elke gegevensschijf gekoppeld aan de virtuele machine. Prijzen is gebaseerd op de feitelijke gegevens die zijn opgeslagen in de gegevensschijf. Op deze manier worden de back-upopslag factuur is gebaseerd op de hoeveelheid gegevens die zijn opgeslagen in Azure back-up, dit is de som van de werkelijke gegevens in elk herstelpunt.

Een standaard A2 grote virtuele machine die twee extra gegevensschijven met een maximale grootte van 1 TB heeft bijvoorbeeld op te halen. De volgende tabel bevat de werkelijke hoeveelheid gegevens die zijn opgeslagen op elk van deze schijven:

| Schijftype | Maximale grootte | Werkelijke gegevens aanwezig |
| --- | --- | --- |
| Besturingssysteemschijf |1023 GB |17 GB |
| Lokale schijf / Resource-schijf |135 GB |5 GB (niet voor back-up opgenomen) |
| Gegevensschijf 1 |1023 GB |30 GB |
| Gegevensschijf 2 |1023 GB |0 GB |

De *werkelijke* grootte van de virtuele machine is in dit geval 17 GB + 30 GB + 0 GB = 47 GB. De grootte van deze beveiligde exemplaar (47 GB), wordt de basis voor de maandelijkse factuur. Wanneer de hoeveelheid gegevens in de virtuele machine groeit, gebruikt de beveiligd exemplaargrootte dienovereenkomstig voor facturering wijzigingen.

Facturering start niet totdat de eerste geslaagde back-up is voltooid. Op dit moment begint de facturering voor zowel opslag als beveiligd exemplaren. Facturering geldt zolang er *een back-up van gegevens die zijn opgeslagen in een kluis* voor de virtuele machine. Als u de beveiliging op de virtuele machine stoppen, maar de back-upgegevens virtuele machine in een kluis bestaat, blijft de facturering.

Financieel voor een opgegeven virtuele machine stopt alleen als de beveiliging is gestopt *en* alle back-gegevens worden verwijderd. Wanneer de beveiliging stopt en er zijn geen actieve back-taken, de grootte van de laatste geslaagde VM back-up wordt de grootte van de beveiligde exemplaar gebruikt voor de maandelijkse factuur.

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen ziet worden, [stuur ons dan uw feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Volgende stappen
* [Back-up van virtuele machines](backup-azure-vms.md)
* [Back-up van virtuele machine beheren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-restore-vms.md)
* [VM-back-problemen](backup-azure-vms-troubleshoot.md)

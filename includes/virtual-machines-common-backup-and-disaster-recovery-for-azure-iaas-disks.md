---
title: bestand opnemen
description: bestand opnemen
services: storage
author: luywang
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: luywang
ms.custom: include file
ms.openlocfilehash: 5c7c9938b6a0b3d2e6050940154a8dc3f114341e
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53638796"
---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Back-up en herstel na noodgevallen voor Azure IaaS-schijven

In dit artikel wordt uitgelegd hoe u van plan bent voor back-up en herstel na noodgevallen (DR) van IaaS virtuele machines (VM's) en schijven in Azure. In dit document bevat informatie over zowel beheerde als niet-beheerde schijven.

Eerst behandelen we het ingebouwde mogelijkheden voor fouttolerantie in de Azure-platform die zorgt voor de bescherming tegen lokale fouten. Vervolgens bespreken we het niet volledig wordt gedekt door de ingebouwde mogelijkheden na noodgevallen. We laten ook zien enkele voorbeelden van workload-scenario's waarbij andere back-up en herstel na Noodgevallen overwegingen kunnen toepassen. Vervolgens bespreken we mogelijke oplossingen voor het herstel na Noodgevallen van IaaS-schijven. 

## <a name="introduction"></a>Inleiding

Het Azure-platform maakt gebruik van verschillende methoden voor redundantie en fouttolerantie om klanten te beschermen tegen gelokaliseerde hardwarefouten. Lokale fouten kunnen problemen met een Azure Storage server-machine die deel uitmaken van de gegevens voor een virtuele schijf of fouten van een SSD of HDD worden opgeslagen op die server bevatten. Dergelijke geïsoleerd onderdeel hardwarefouten kunnen gebeuren tijdens normale bewerkingen. 

Het Azure-platform is ontworpen voor tolerantie deze fouten. Grote rampen kunnen resulteren in fouten of de ontoegankelijkheid van veel opslagservers of zelfs een hele datacenter. Hoewel de virtuele machines en de schijven zijn doorgaans beveiligd tegen gelokaliseerde fouten, zijn extra stappen die nodig zijn voor uw workload beschermen tegen regiobrede onherstelbare fouten, zoals een grote ramp, die invloed hebben op de virtuele machine en schijven.

Problemen met een klanttoepassing of de gegevens kunnen naast de mogelijkheid van platform fouten optreden. Een nieuwe versie van uw toepassing kan bijvoorbeeld per ongeluk een wijziging aanbrengt in de gegevens die ervoor zorgt dat deze opsplitsen. In dat geval als u wilt herstellen van de toepassing en de gegevens in een eerdere versie die de laatst bekende goede status bevat. Dit is vereist voor het onderhouden van regelmatige back-ups.

Voor regionaal herstel na noodgevallen, moet u back-up van de IaaS-VM-schijven naar een andere regio. 

Voordat we kijken naar back-up en herstel na Noodgevallen opties, samenvatting gaan we een aantal methoden die beschikbaar zijn voor het verwerken van gelokaliseerde fouten.

### <a name="azure-iaas-resiliency"></a>Flexibiliteit van Azure IaaS

*Tolerantie* verwijst naar de tolerantie voor normale fouten die in de hardware-onderdelen optreden. Flexibiliteit is de mogelijkheid om te herstellen van fouten en blijven werken. Het is niet over de fouten te voorkomen, maar reageren op fouten in een manier waarmee downtime of gegevensverlies. Het doel van flexibiliteit is om de toepassing na een storing weer volledig te laten functioneren. Virtuele machines van Azure en schijven zijn ontworpen om zijn tegen veelvoorkomende hardwarefouten. We bekijken hoe de Azure IaaS-platform deze tolerantie biedt.

Een virtuele machine bestaat voornamelijk uit twee delen: een compute-server en de permanente schijven. Beide invloed op de fouttolerantie van een virtuele machine.

Als er een hardware-uitval zeldzaam is, optreedt in de Azure compute-hostserver waarop uw virtuele machine worden Azure is ontworpen voor de virtuele machine op een andere server wordt automatisch hersteld. Als dit scenario, uw computer opnieuw wordt opgestart en de virtuele machine is hersteld na enige tijd. Azure wordt automatisch detecteert van dergelijke problemen met de hardware en herstelbewerkingen om ervoor te zorgen dat de klant virtuele machine zo snel mogelijk beschikbaar is uitgevoerd.

Met betrekking tot IaaS-schijven is de duurzaamheid van gegevens essentieel voor een platform voor permanente opslag. Azure-klanten hebben belangrijke zakelijke toepassingen wordt uitgevoerd op IaaS en ze afhankelijk zijn van de persistentie van de gegevens. Ontwerpen voor Azure-beveiliging voor deze IaaS-schijven, met drie kopieën van de gegevens die lokaal wordt opgeslagen. Deze exemplaren bieden voor hoge duurzaamheid tegen lokale fouten. Als een van de hardwareonderdelen met de schijf is mislukt, wordt uw virtuele machine niet beïnvloed omdat er twee extra kopieën die op schijf ondersteuningsaanvragen. Het werkt prima, zelfs als twee verschillende hardware-onderdelen die ondersteuning bieden voor een schijf op hetzelfde moment (dit is het zeldzaam) mislukt. 

Om ervoor te zorgen dat u drie replica's altijd onderhouden, een nieuwe kopie van de gegevens op de achtergrond in Azure Storage automatisch worden gestart als een van de drie kopieert niet meer beschikbaar. Dus mag het geen die nodig zijn voor het gebruik van RAID met Azure-schijven voor fouttolerantie. Een eenvoudige RAID-0-configuratie moet voldoende zijn voor striping van de schijven, indien nodig, om grotere volumes te maken.

Vanwege deze architectuur wordt Azure voortdurend zakelijke heeft geleverd duurzaamheid voor IaaS-schijven, met een toonaangevende nul procent [op jaarbasis foutpercentage](https://en.wikipedia.org/wiki/Annualized_failure_rate).

Gelokaliseerde hardwarefouten in de berekening die hosten of in de opslag-platform kunnen soms leiden van de tijdelijk niet beschikbaar zijn van de virtuele machine die wordt gedekt door de [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) voor VM-beschikbaarheid. Azure biedt ook een toonaangevende SLA voor afzonderlijke VM-exemplaren die gebruikmaken van Azure Premium-SSD-schijven.

Als u wilt beveiligen werkbelastingen van toepassingen van downtime vanwege een tijdelijk ontbreken van een schijf of virtuele machine, klanten kunnen gebruikmaken van [beschikbaarheidssets](../articles/virtual-machines/windows/manage-availability.md). Twee of meer virtuele machines in een beschikbaarheidsset redundantie voor de toepassing. Azure maakt vervolgens deze VM's en de schijven in afzonderlijke foutdomeinen met verschillende onderdelen van power-, netwerk- en -server. 

Vanwege deze afzonderlijke foutdomeinen lokaal optredende hardwarestoringen doorgaans niet van invloed op meerdere virtuele machines in de set op hetzelfde moment. Met afzonderlijke foutdomeinen biedt hoge beschikbaarheid voor uw toepassing. Het is een goede gewoonte om gebruik beschikbaarheidssets wanneer maximale beschikbaarheid vereist is als beschouwd. De volgende sectie bevat informatie over het herstel na noodgevallen aspect.

### <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen

Herstel na noodgevallen is de mogelijkheid om te herstellen na zeldzame maar grote, incidenten. Deze incidenten zijn niet-tijdelijke, schaal fouten, zoals serviceonderbrekingen die een hele regio treft. Herstel na noodgevallen omvat back-up en archivering en kan bestaan uit handmatig ingrijpen, zoals het herstellen van een database vanuit een back-up.

Van het Azure-platform ingebouwde bescherming tegen gelokaliseerde fouten mogelijk niet volledig te beveiligen de VM's /-schijven als een grote ramp grootschalige storing veroorzaakt. Deze grootschalige storingen catastrofale gebeurtenissen, zoals opnemen als een datacenter is bereikt, wordt door een orkaan, aardbeving, worden gestart of als er een grote hardwarestoring eenheid. U kunt bovendien tegenkomen storingen als gevolg van toepassing of problemen met gegevens.

Ter bescherming van uw IaaS-workloads van storingen, moet u plannen voor redundantie en back-ups om herstel te schakelen. Voor herstel na noodgevallen, moet u back-up op een andere geografische locatie van de primaire site. Deze aanpak zorgt ervoor dat uw back-up wordt niet beïnvloed door de dezelfde gebeurtenis die oorspronkelijk last van de virtuele machine of de schijven. Zie voor meer informatie, [herstel na noodgevallen voor Azure-toepassingen](/azure/architecture/resiliency/disaster-recovery-azure-applications).

Uw overwegingen voor herstel na Noodgevallen zijn onder andere de volgende aspecten:

- Hoge beschikbaarheid: De mogelijkheid van de toepassing om door te gaan die wordt uitgevoerd in een goede status, zonder significante downtime. Door *status in orde*, deze status betekent dat de toepassing responsief is en dat gebruikers kunnen verbinding maken met de toepassing en er interactie mee hebt. Het is mogelijk dat bepaalde essentiële toepassingen en databases vereist moet altijd beschikbaar, zelfs als er fouten in het platform zijn. Voor deze werkbelastingen moet u mogelijk redundantie voor de toepassing, evenals de gegevens van plan bent.

- Duurzaamheid van gegevens: In sommige gevallen is de belangrijkste overweging ervoor te zorgen dat de gegevens behouden blijft als het geval is na een noodgeval. Daarom moet u mogelijk een back-up van uw gegevens in een andere site. Voor deze werkbelastingen moet u mogelijk niet volledige redundantie voor de toepassing, maar alleen een reguliere-up van de schijven.

## <a name="backup-and-dr-scenarios"></a>Scenario's voor back-up en herstel na Noodgevallen

Bekijk enkele typische voorbeelden van scenario's voor application werkbelasting en de aandachtspunten voor het plannen voor herstel na noodgevallen.

### <a name="scenario-1-major-database-solutions"></a>Scenario 1: Primaire database-oplossingen

Houd rekening met een productie-databaseserver, zoals SQL Server of Oracle, die ondersteuning voor hoge beschikbaarheid bieden. Essentiële productietoepassingen en gebruikers, is afhankelijk van deze database. Het plannen van het herstel na noodgevallen voor dit systeem moet mogelijk de volgende vereisten ondersteunen:

- De gegevens moeten worden beveiligd en hersteld.
- De server moet beschikbaar zijn voor gebruik.

Het noodherstelplan mogelijk onderhouden van een replica van de database in een andere regio als een back-up. De oplossing kan afhankelijk van de vereisten voor beschikbaarheid van de server en herstel van gegevens op periodieke offline back-ups van de gegevens variëren van een actief-actief of actief-passief-replica-site. Relationele databases, zoals SQL Server en Oracle, bieden verschillende opties voor replicatie. Voor SQL Server, gebruikt u [SQL Server AlwaysOn Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx) voor hoge beschikbaarheid.

Ook ondersteuning voor NoSQL-databases, zoals MongoDB, [replica's](https://docs.mongodb.com/manual/replication/) voor redundantie. De replica's voor hoge beschikbaarheid worden gebruikt.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Scenario 2: Een cluster met redundante virtuele machines

Houd rekening met een werkbelasting die wordt verwerkt door een cluster van virtuele machines die redundantie en taakverdeling. Een voorbeeld hiervan is een Cassandra-cluster geïmplementeerd in een regio. Dit type architectuur biedt al een hoge mate van redundantie in die regio. Echter, ter bescherming van de werkbelasting van een fout regionaal niveau, u moet rekening houden met het cluster spreiden over twee regio's of het maken van periodieke back-ups naar een andere regio.

### <a name="scenario-3-iaas-application-workload"></a>Scenario 3: IaaS-workload van toepassing

We bekijken de workload van de IaaS-toepassing. Deze toepassing kan bijvoorbeeld zijn dat een standaard productie-werkbelasting wordt uitgevoerd op een Azure-VM. Het is mogelijk een webserver of bestandsserver die de inhoud en andere resources van een site. Het is mogelijk ook een op maat gemaakte business-toepassing die wordt uitgevoerd op een virtuele machine die de gegevens, resources en de status van toepassing op de VM-schijven die zijn opgeslagen. In dit geval is het belangrijk om ervoor te zorgen dat u regelmatig back-ups uitvoeren. Back-upfrequentie moet worden gebaseerd op de aard van de virtuele machine-werkbelasting. Bijvoorbeeld, als de toepassing wordt elke dag uitgevoerd en gegevens worden gewijzigd, moet klikt u vervolgens de back-up worden genomen om het uur.

Een ander voorbeeld is een rapportserver die gegevens ophaalt uit andere bronnen en genereert samengevoegde rapporten. Het verlies van gegevens van deze virtuele machine of de schijven kan leiden tot het verlies van gegevens van de rapporten. Echter mogelijk opnieuw uitvoeren van het proces voor rapportage en opnieuw genereren van de uitvoer. In dat geval hebt niet echt een verlies van gegevens, zelfs als de rapportserver is bereikt, wordt met een noodgeval. Als gevolg hiervan mogelijk hebt u een hoger niveau van tolerantie voor het onderdeel van de gegevens op de rapportserver verliezen. In dat geval zijn minder frequente back-ups een optie om kosten te verlagen.

### <a name="scenario-4-iaas-application-data-issues"></a>Scenario 4: Problemen met de gegevens van de IaaS-toepassing

Problemen met de gegevens van de IaaS-toepassing zijn een andere mogelijkheid. Houd rekening met een toepassing die worden berekend, onderhouden en kritieke commerciële gegevens, zoals informatie over de prijzen fungeert. Een nieuwe versie van uw toepassing heeft een software-oplossingen die ten onrechte de prijzen berekend en de bestaande commercegegevens geleverd door het platform beschadigd. Hier is de beste loop van de actie om terug te keren naar de eerdere versie van de toepassing en de gegevens. U kunt deze inschakelen nemen periodieke back-ups van uw systeem.

## <a name="disaster-recovery-solution-azure-backup"></a>Oplossing voor noodherstel: Azure Backup 

[Azure Backup](https://azure.microsoft.com/services/backup/) wordt gebruikt voor back-ups en herstel na Noodgevallen, en het werkt met [beheerde schijven](../articles/virtual-machines/windows/managed-disks-overview.md) , evenals [niet-beheerde schijven](../articles/virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). U kunt een back-up maken met back-ups op basis van tijd eenvoudig herstel van de virtuele machine en voor het bewaren van back-up. 

Als u [Premium-SSD-schijven](../articles/virtual-machines/windows/premium-storage.md), [beheerde schijven](../articles/virtual-machines/windows/managed-disks-overview.md), of andere schijftypen met de [lokaal redundante opslag](../articles/storage/common/storage-redundancy-lrs.md) ziet, is het belangrijk dat u periodieke DR-back-ups maken. De gegevens van de Azure Backup worden opgeslagen in uw recovery services-kluis voor langdurige bewaarperioden. Kies de [geografisch redundante opslag](../articles/storage/common/storage-redundancy-grs.md) optie voor de back-recovery services-kluis. Deze optie zorgt ervoor dat de back-ups worden gerepliceerd naar een andere Azure-regio voor het beveiligen van regionale rampen.

Voor [niet-beheerde schijven](../articles/virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), u kunt het type lokaal redundante opslag gebruiken voor IaaS-schijven, maar zorg ervoor dat Azure Backup is ingeschakeld met de geografisch redundante opslag met de optie voor de recovery services-kluis.

> [!NOTE]
> Als u de [geografisch redundante opslag](../articles/storage/common/storage-redundancy-grs.md) of [geografisch redundante opslag met leestoegang](../articles/storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) optie voor uw niet-beheerde schijven, u nog steeds nodig toepassingsconsistente momentopnamen voor back-up en herstel na Noodgevallen. Gebruik een [Azure Backup](https://azure.microsoft.com/services/backup/) of [toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots).

 De volgende tabel bevat een overzicht van de oplossingen die beschikbaar zijn voor herstel na Noodgevallen.

| Scenario | Automatische replicatie | DR-oplossing |
| --- | --- | --- |
| Premium-SSD-schijven | Lokale ([lokaal redundante opslag](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Managed Disks | Lokale ([lokaal redundante opslag](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Niet-beheerde schijven voor lokaal redundante opslag | Lokale ([lokaal redundante opslag](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Geografisch redundante opslag met niet-beheerde schijven | Interregionaal ([geografisch redundante opslag](../articles/storage/common/storage-redundancy-grs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots) |
| Niet-beheerde geografisch redundante opslag met leestoegang schijven | Interregionaal ([geografisch redundante opslag met leestoegang](../articles/storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots) |

Hoge beschikbaarheid is best voldaan met behulp van beheerde schijven in een beschikbaarheidsset, samen met Azure Backup. Als u niet-beheerde schijven gebruikt, kunt u Azure back-up nog steeds gebruiken voor herstel na Noodgevallen. Als u geen gebruik van Azure Backup, waarbij [toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots), zoals beschreven in een volgende sectie, is een alternatieve oplossing voor back-up en herstel na Noodgevallen.

Uw keuzes voor hoge beschikbaarheid, back-up en herstel na Noodgeval op het niveau van toepassings- of -infrastructuur kunnen worden gebruikt, als volgt:

| Niveau |   Hoge beschikbaarheid   | Back-up of herstel na Noodgevallen |
| --- | --- | --- |
| Toepassing | SQL Server AlwaysOn | Azure Backup |
| Infrastructuur    | Beschikbaarheidsset  | Geografisch redundante opslag met toepassingsconsistente momentopnamen |

### <a name="using-azure-backup"></a>Met behulp van Azure Backup 

[Azure Backup](../articles/backup/backup-azure-vms-introduction.md) back-up van uw virtuele machines met Windows of Linux met de Azure recovery services-kluis. Back-up en herstellen van essentiële gegevens wordt bemoeilijkt door het feit dat kritieke zakelijke gegevens moet een back-up terwijl de toepassingen die de gegevens produceren die worden uitgevoerd. 

Om dit probleem op te lossen, biedt Azure Backup toepassingsconsistente back-ups voor Microsoft-werkbelastingen. Het maakt gebruik van de volume shadow service om ervoor te zorgen dat gegevens correct worden geschreven naar opslag. Voor virtuele Linux-machines zijn alleen bestandsconsistente back-ups mogelijk, omdat Linux heeft geen functionaliteit die gelijk is aan de volume shadow service.

![Azure Backup-stroom][1]

Bij Azure back-up maken van een back-uptaak op het geplande tijdstip, wordt deze geactiveerd de Backup-extensie geïnstalleerd in de VM voor het maken van een momentopname van een point-in-time. Een momentopname wordt gemaakt in combinatie met de volume shadow service om op te halen van een consistente momentopname van de schijven in de virtuele machine zonder af te sluiten. De back-upextensie in de virtuele machine leegmaken alle schrijfbewerkingen voor het maken van een consistente momentopname van alle schijven. Na het maken van de momentopname, de gegevens overgedragen door Azure Backup naar de back-upkluis. Als u de back-upproces efficiënter, de service identificeert en brengt alleen de blokken met gegevens die zijn gewijzigd na de laatste back-up.

Als u wilt herstellen, kunt u weergeven van de beschikbare back-ups via Azure Backup en initieer een terugzetbewerking. U kunt maken en herstellen van Azure back-ups met de [Azure-portal](https://portal.azure.com/), door [met behulp van PowerShell](../articles/backup/backup-azure-vms-automation.md), of met behulp van de [Azure CLI](/cli/azure/). 

### <a name="steps-to-enable-a-backup"></a>Stappen voor het inschakelen van een back-up

Gebruik de volgende stappen om in te schakelen van back-ups van uw virtuele machines met behulp van de [Azure-portal](https://portal.azure.com/). Er is een variatie afhankelijk van uw exacte scenario. Raadpleeg de [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) documentatie voor meer informatie. Azure back-up ook [biedt ondersteuning voor virtuele machines met beheerde schijven](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Maak een recovery services-kluis voor een virtuele machine:

    a. In de [Azure-portal](https://portal.azure.com/), Bladeren **alle resources** en zoek **Recovery Services-kluizen**.

    b. Op de **Recovery Services-kluizen** menu, klikt u op **toevoegen** en volg de stappen voor het maken van een nieuwe kluis in dezelfde regio als de virtuele machine. Bijvoorbeeld, als uw virtuele machine zich in de regio VS-West, VS-West voor verzamelen de kluis.

1.  Controleer of de storage-replicatie voor de zojuist gemaakte kluis. Toegang tot de kluis onder **Recovery Services-kluizen** en Ga naar **instellingen** > **back-upconfiguratie**. Zorg ervoor dat de **geografisch redundante opslag** optie is standaard geselecteerd. Deze optie zorgt ervoor dat uw kluis automatisch worden gerepliceerd naar een secundair datacenter. Uw kluis in VS-West bijvoorbeeld automatisch worden gerepliceerd naar VS-Oost.

1.  Het back-upbeleid configureren en selecteer de virtuele machine in dezelfde gebruikersinterface.

1.  Zorg ervoor dat de Backup-Agent is geïnstalleerd op de virtuele machine. Als uw virtuele machine is gemaakt met behulp van de installatiekopie van een Azure-galerie, is klikt u vervolgens de Backup-Agent al geïnstalleerd. Anders (dat wil zeggen, als u een aangepaste installatiekopie gebruiken), gebruikt u de instructies voor het [de VM-agent installeren op een virtuele machine](../articles/backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent).

1.  Zorg ervoor dat de virtuele machine netwerkverbinding voor de back-upservice functie geeft. Volg de instructies voor [netwerkverbinding](../articles/backup/backup-azure-arm-vms-prepare.md#establish-network-connectivity).

1.  Nadat de vorige stappen zijn voltooid, wordt de back-up wordt uitgevoerd met regelmatige intervallen die zijn opgegeven in het back-upbeleid. Indien nodig, kunt u de eerste back-up handmatig vanuit het kluisdashboard op de Azure-portal kunt activeren.

Raadpleeg voor het automatiseren van Azure Backup met behulp van scripts, [PowerShell-cmdlets voor back-up VM](../articles/backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Stappen voor herstel

Als u wilt herstellen of opnieuw opbouwen van een virtuele machine, kunt u de virtuele machine herstellen uit een van de back-up herstelpunten in de kluis. Er zijn een aantal verschillende opties voor het uitvoeren van het herstel:

-   U kunt een nieuwe virtuele machine maken als een point-in-time-weergave van uw VM waarvan een back-up is gemaakt.

-   U kunt de schijven herstellen en vervolgens met de sjabloon voor de virtuele machine kunt aanpassen en opnieuw opbouwen van de herstelde VM. 

Zie voor meer informatie de instructies voor het [de Azure portal gebruiken voor virtuele machines herstellen](../articles/backup/backup-azure-arm-restore-vms.md). Dit document worden ook de specifieke stappen beschreven voor het herstellen van back-up VM's naar een gekoppeld datacenter met behulp van uw geografisch redundante back-upkluis als er een ramp in het primaire datacenter. In dat geval Azure Backup maakt gebruik van de Compute-service van de secundaire regio te maken van de herstelde virtuele machine.

U kunt ook PowerShell voor [herstellen van een virtuele machine](../articles/backup/backup-azure-arm-restore-vms.md#restore-a-vm-during-an-azure-datacenter-disaster) of voor [het maken van een nieuwe VM op basis van schijven hersteld](../articles/backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternatieve oplossing: Toepassingsconsistente momentopnamen

Als u geen gebruik van Azure Backup, kunt u uw eigen back-mechanisme implementeren met behulp van momentopnamen. Toepassingsconsistente momentopnamen voor alle schijven die worden gebruikt door een virtuele machine maken en vervolgens deze momentopnamen te repliceren naar een andere regio is ingewikkeld. Om deze reden beschouwd Azure als een betere optie dan het bouwen van een aangepaste oplossing met behulp van de Backup-service. 

Als u geografisch redundante opslag/geografisch redundante opslag met leestoegang voor schijven, momentopnamen automatisch gerepliceerd naar een secundair datacenter. Als u lokaal redundante opslag voor schijven gebruikt, moet u voor het repliceren van de gegevens. Zie voor meer informatie, [maakt u een Back-up van Azure-niet-beheerde VM-schijven met incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md).

Een momentopname is een weergave van een object op een bepaald tijdstip. Een momentopname van een leidt tot facturering voor de incrementele hoeveelheid gegevens die deze bevat. Zie voor meer informatie, [maken van een blob-momentopname](../articles/storage/blobs/storage-blob-snapshots.md).

### <a name="create-snapshots-while-the-vm-is-running"></a>Maken van momentopnamen terwijl de virtuele machine wordt uitgevoerd

Hoewel u kunt een momentopname op elk gewenst moment als de virtuele machine wordt uitgevoerd, is er nog steeds gegevens worden gestreamd naar de schijven. De momentopnamen bevatten mogelijk gedeeltelijk bewerkingen die tijdens de overdracht zijn. Als er meerdere schijven die betrokken zijn, kunnen er ook de momentopnamen van verschillende schijven zijn opgetreden op verschillende tijdstippen. Deze scenario's mogelijk aan de momentopnamen worden ongecoördineerde. Dit gebrek aan coördinatie is met name problemen opleveren voor striped volumes waarvan de bestanden is mogelijk beschadigd als er wijzigingen zijn aangebracht tijdens back-up.

Om te voorkomen dat deze situatie, moet het back-upproces implementeren de volgende stappen uit:

1.  Blokkeert alle schijven.

1.  Alle in behandeling schrijfbewerkingen leegmaken.

1.  [Een blob-momentopname maken](../articles/storage/blobs/storage-blob-snapshots.md) voor alle schijven.

In sommige Windows-toepassingen, zoals SQL Server, bieden een gecoördineerde back-mechanisme via een volume shadow service toepassingsconsistente back-ups maken. Op Linux, kunt u een hulpprogramma zoals *fsfreeze* voor het coördineren van de schijven. Dit hulpprogramma biedt bestandsconsistente back-ups, maar geen toepassingsconsistente momentopnamen. Dit proces is complex, dus moet u overwegen [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) of een back-upoplossing van derden die al deze procedure implementeert.

Het vorige proces resulteert in een verzameling van gecoördineerde momentopnamen voor alle van de VM-schijven, voor een specifieke point-in-time-weergave van de virtuele machine. Dit is een back-up herstelpunt voor de virtuele machine. U kunt het proces herhalen met regelmatige tussenpozen periodieke back-ups maken. Zie [kopiëren van de back-ups naar een andere regio](#copy-the-snapshots-to-another-region) voor stappen voor het kopiëren van de momentopnamen naar een andere regio voor herstel na Noodgevallen.

### <a name="create-snapshots-while-the-vm-is-offline"></a>Maken van momentopnamen terwijl de virtuele machine offline is

Er is een andere optie voor het maken van toepassingsconsistente back-ups voor de virtuele machine af en blob-momentopnamen van elke schijf. Blobmomentopnamen te maken is eenvoudiger dan het coördineren van momentopnamen van een actieve virtuele machine, maar er is een paar minuten uitvaltijd vereist.

1. De virtuele machine af.

1. Maak een momentopname van elke virtuele harde schijf-blob, die duurt maar een paar seconden.

    Voor het maken van een momentopname, kunt u [PowerShell](../articles/storage/common/storage-powershell-guide-full.md), wordt de [REST-API van Azure Storage](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](/cli/azure/), of een van de Azure Storage-clientbibliotheken, zoals [de Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

1. Start de virtuele machine, die de downtime eindigt. Normaal gesproken het hele proces is voltooid binnen een paar minuten.

Dit proces resulteert in een verzameling van toepassingsconsistente momentopnamen voor alle schijven bieden een back-up herstelpunt voor de virtuele machine.

### <a name="copy-the-snapshots-to-another-region"></a>De momentopnamen kopiëren naar een andere regio

Het maken van de momentopnamen die alleen zijn mogelijk niet voldoende is voor herstel na Noodgevallen. U moet ook de momentopname van back-ups repliceren naar een andere regio.

Als u geografisch redundante opslag of geografisch redundante opslag met leestoegang voor uw schijven, en vervolgens de momentopnamen worden automatisch gerepliceerd naar de secundaire regio. Er zijn een paar minuten vertraging voordat de replicatie. Als het primaire datacenter uitvalt voordat de momentopnamen voltooien repliceren, geen u toegang tot de momentopnamen van het secundaire datacenter. De kans is klein.

> [!NOTE] 
> Alleen met de schijven in een geografisch redundante opslag of geografisch redundante leestoegang worden storage-account niet de virtuele machine beveiligd tegen rampen. U moet ook gecoördineerde momentopnamen maken of gebruiken van Azure Backup. Dit is vereist voor het herstellen van een virtuele machine naar een consistente status.

Als u lokaal redundante opslag gebruikt, moet u de momentopnamen kopiëren naar een ander opslagaccount onmiddellijk na het maken van een momentopname. Het doel voor het kopiëren is mogelijk een account met lokaal redundante opslag in een andere regio, wat resulteert in het exemplaar wordt in een verafgelegen regio. U kunt ook de momentopname van het kopiëren naar een account voor geografisch redundante opslag met leestoegang in dezelfde regio. In dit geval wordt de momentopname lazily gerepliceerd naar de externe secundaire regio. Uw back-up is beveiligd tegen rampen op de primaire site na het kopiëren en replicatie is voltooid.

Als u wilt kopiëren efficiënt uw incrementele momentopnamen voor herstel na Noodgevallen, lees de instructies in [maakt u een Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md).

![Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen][2]

### <a name="recovery-from-snapshots"></a>Herstel van momentopnamen

Als u wilt ophalen van een momentopname, dit een nieuwe BLOB te kopiëren. Als u de momentopname van de primaire account kopieert, kunt u de momentopname via kopiëren naar de basis-blob van de momentopname. Dit proces wordt de schijf naar de momentopname. Dit proces staat bekend als het promoveren van de momentopname. Als u de momentopname van back-up van een secundaire-account in het geval van een account voor geografisch redundante opslag met leestoegang kopieert, moet u het kopiëren naar een primaire-account. U kunt een momentopname van een door kopiëren [met behulp van PowerShell](../articles/storage/common/storage-powershell-guide-full.md) of met behulp van de AzCopy-hulpprogramma. Zie voor meer informatie, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).

Voor virtuele machines met meerdere schijven, moet u de momentopnamen die deel van hetzelfde gecoördineerde herstelpunt uitmaken kopiëren. Nadat u de momentopnamen naar beschrijfbare VHD-blobs kopiëren, kunt u de blobs kunt gebruiken om uw virtuele machine met behulp van de sjabloon voor de virtuele machine opnieuw te maken.

## <a name="other-options"></a>Andere opties

### <a name="sql-server"></a>SQL Server

SQL-Server die wordt uitgevoerd in een virtuele machine heeft een eigen ingebouwde mogelijkheden voor delen maakt u een back-up van SQL Server-database naar Azure Blob-opslag of een bestand. Als het opslagaccount geografisch redundante opslag of geografisch redundante opslag met leestoegang is, kunt u deze back-ups in een secundair datacenter van de storage-account openen in het geval van een noodgeval met dezelfde beperkingen zoals eerder besproken. Zie voor meer informatie, [Back-up en herstel voor SQL Server in virtuele Azure-machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). In aanvulling op back-up en herstellen, [SQL Server AlwaysOn-beschikbaarheidsgroepen](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) secundaire replica's van databases kunt onderhouden. Deze mogelijkheid vermindert aanzienlijk de hersteltijd na noodgevallen.

## <a name="other-considerations"></a>Andere overwegingen

In dit artikel is besproken hoe u een back-up of momentopnamen van uw virtuele machines en de schijven voor de ondersteuning van herstel na noodgevallen en het gebruik van deze back-ups of momentopnamen om uw gegevens te herstellen. Met het Azure Resource Manager-model kunt gebruiken veel mensen sjablonen hun VM's en andere infrastructuren maken in Azure. U kunt een sjabloon gebruiken om te maken van een virtuele machine met dezelfde configuratie elke keer. Als u aangepaste installatiekopieën gebruikt voor het maken van uw virtuele machines, moet u zorgen dat uw afbeeldingen met behulp van een account voor geografisch redundante opslag met leestoegang voor het opslaan van deze zijn beschermd.

Uw back-upproces kan als gevolg daarvan kan bestaan uit een combinatie van twee dingen:

- Back-up van de gegevens (schijven).
- Back-up van de configuratie (sjablonen en aangepaste installatiekopieën).

Afhankelijk van de back-optie die u kiest, mogelijk hebt u voor het afhandelen van de back-up van de gegevens en de configuratie of de back-upservice die voor u kan afhandelen.

## <a name="appendix-understanding-the-impact-of-data-redundancy"></a>Bijlage: Informatie over de impact van de gegevensredundantie

Voor storage-accounts in Azure, er zijn drie typen van de gegevensredundantie die u met betrekking tot herstel na noodgevallen overwegen moet: lokaal redundant, geografisch redundante of geografisch redundant met leestoegang. 

Lokaal redundante opslag behoudt drie kopieën van de gegevens in hetzelfde datacenter. Wanneer de virtuele machine de gegevens schrijft, worden alle drie kopieën bijgewerkt voordat succes wordt geretourneerd naar de aanroeper vastgesteld, zodat u weet dat ze identiek zijn. De schijf is beveiligd tegen lokale fouten omdat het onwaarschijnlijk dat alle drie kopieën op hetzelfde moment worden beïnvloed. In het geval van een lokaal redundante opslag is er geen geo-redundantie, zodat de schijf is niet beveiligd tegen onherstelbare fouten die invloed kunnen zijn op een hele datacenter of de opslag-eenheid.

Met geografisch redundante opslag en geografisch redundante opslag met leestoegang, drie kopieën van uw gegevens worden bewaard in de primaire regio die door u is geselecteerd. Drie extra kopieën van uw gegevens worden bewaard in een bijbehorende secundaire regio die is ingesteld door Azure. Bijvoorbeeld, als u gegevens in VS-West opslaat, wordt de gegevens gerepliceerd naar VS-Oost. Kopie bewaren asynchroon wordt uitgevoerd, en er is een kleine vertraging tussen de updates naar de primaire en secundaire sites. Replica's van de schijven op de secundaire site zijn consistent op basis van de per-schijf (met de vertraging), maar de replica's van meerdere actieve schijven mogelijk niet synchroon met elkaar. Als u consistente replica's over meerdere schijven, zijn toepassingsconsistente momentopnamen nodig.

Het belangrijkste verschil tussen geografisch redundante opslag en geografisch redundante opslag met leestoegang is met geografisch redundante opslag met leestoegang, kunt u de secundaire kopie lezen op elk gewenst moment. Als er een probleem dat de gegevens in de primaire regio niet toegankelijk wordt weergegeven, is het Azure-team spant zich toegang herstellen. Terwijl de primaire ligt, hebt u geografisch redundante opslag met leestoegang ingeschakeld, kunt u toegang tot de gegevens in het secundaire datacenter. Dus als u van plan bent om te lezen van de replica als de primaire niet toegankelijk is, moet klikt u vervolgens geografisch redundante opslag met leestoegang worden overwogen.

Als blijkt te zijn van een aanzienlijke uitval, kan het Azure-team een geo-failover wordt geactiveerd en wijzigen van de primaire DNS-vermeldingen om te verwijzen naar een secundaire opslag. Als u geografisch redundante opslag of geografisch redundante opslag met leestoegang ingeschakeld hebt, kunt u op dit moment de gegevens in de regio die wordt gebruikt om te worden van de secundaire server openen. Met andere woorden, als uw storage-account geografisch redundante opslag is en er een probleem is, u kunt toegang tot de secundaire opslag alleen als er een geo-failover is.

Zie voor meer informatie, [wat te doen als een Azure Storage-storing](../articles/storage/common/storage-disaster-recovery-guidance.md). 

>[!NOTE] 
>Microsoft bepaalt of een failover plaatsvindt. Failover wordt niet beheerd per opslagaccount gebruikt, zodat deze niet wordt bepaald door individuele klanten. Voor het implementeren van herstel na noodgevallen voor specifieke storage-accounts of de schijven van virtuele machines, moet u de technieken die eerder in dit artikel wordt beschreven.



[1]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png

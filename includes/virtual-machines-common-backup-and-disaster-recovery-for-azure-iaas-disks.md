
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Back-up en herstel na noodgevallen voor Azure IaaS-schijven

In dit artikel wordt uitgelegd hoe om te plannen voor back-up en herstel na noodgevallen (DR) van IaaS virtuele machines (VM's) en schijven in Azure. Dit document bevat informatie over beheerde en onbeheerde schijven.

We eerst betrekking op de mogelijkheden voor ingebouwde fouttolerantie in de Azure-platform die beschermt tegen fouten in de lokale. Vervolgens bespreken we de scenario's niet volledig wordt gedekt door de ingebouwde mogelijkheden voor noodherstel. Dit is het belangrijkste onderwerp behandeld in dit document. We zien ook enkele voorbeelden van werkbelasting scenario's waarbij andere back-up en Noodherstel overwegingen kunnen toepassen. Vervolgens bespreken we mogelijke oplossingen voor de DR van IaaS-schijven. 

## <a name="introduction"></a>Inleiding

De Azure-platform gebruikt verschillende methoden voor redundantie en fouttolerantie voor klanten beschermen tegen gelokaliseerde hardwarefouten. Lokale fouten kunnen opnemen problemen met een Azure Storage server-machine die deel uitmaken van de gegevens voor een virtuele schijf of fouten van een SSD of harde schijf op die server worden opgeslagen. Dergelijke geïsoleerde hardwarefouten onderdeel kunnen zich voordoen tijdens normale bewerkingen. 

De Azure-platform is ontworpen voor deze fouten netwerkfouten. Grote rampen kunnen resulteren in fouten of de inaccessibility van veel opslagservers of zelfs een hele datacenter. Hoewel de virtuele machines en de schijven zijn doorgaans beveiligd tegen gelokaliseerde fouten, zijn extra stappen nodig zijn om te voorkomen dat uw werkbelasting regio hele onherstelbare fouten, zoals een noodgeval die invloed hebben op de virtuele machine en de schijven.

Problemen met een klanttoepassing of de gegevens kunnen naast de mogelijkheid van platform fouten optreden. Een nieuwe versie van uw toepassing kan bijvoorbeeld per ongeluk een wijziging aanbrengt in de gegevens die ervoor zorgt dat verbreken. In dat geval als u wilt herstellen van de toepassing en de gegevens naar een eerdere versie die de laatst bekende goede status bevat. Hiervoor moet het onderhouden van regelmatige back-ups.

Voor de regionale noodherstel, moet u back-up uw IaaS VM-schijven in een andere regio. 

Voordat we back-up en Noodherstel opties kijken, samenvatting gaan we een aantal methoden beschikbaar voor het verwerken van gelokaliseerde fouten.

### <a name="azure-iaas-resiliency"></a>Tolerantie van Azure IaaS

*Tolerantie* verwijst naar de tolerantie voor normale fouten die in de hardware-onderdelen optreden. Tolerantie is de mogelijkheid om te herstellen van fouten en blijven werken. Het is niet over de fouten te voorkomen, maar reageert op storingen op een manier die uitvaltijd of verlies van gegevens voorkomt. Het doel van flexibiliteit is om de toepassing na een storing weer volledig te laten functioneren. Virtuele machines in Azure en de schijven zijn ontworpen om netwerkfouten algemene hardwarefouten. Bekijk hoe het Azure IaaS-platform deze tolerantie biedt.

Een virtuele machine bestaat voornamelijk uit twee delen: een compute-server en de permanente schijven. Beide invloed op de fouttolerantie van een virtuele machine.

Als de Azure compute-hostserver met uw virtuele machine te maken een hardwarefout opgetreden die niet vaak voorkomt krijgt, wordt Azure is ontworpen voor het automatisch herstelt de virtuele machine op een andere server. Als dit gebeurt, wordt uw computer opnieuw wordt opgestart, en de virtuele machine weer later opnieuw. Azure dergelijke hardwarefouten detecteert automatisch en wordt uitgevoerd herstelbewerkingen om ervoor te zorgen dat de klant VM zo spoedig mogelijk beschikbaar is.

Met betrekking tot IaaS-schijven, zijn de duurzaamheid van gegevens is essentieel voor een platform voor permanente opslag. Azure-klanten belangrijke zakelijke toepassingen die worden uitgevoerd op IaaS hebben en ze afhankelijk van de persistentie van de gegevens. De beveiliging van de Azure-ontwerpen voor deze IaaS-schijven, met drie kopieën van de gegevens die lokaal wordt opgeslagen. Deze kopieën bieden voor maximale duurzaamheid tegen fouten in de lokale. Als een van de hardwareonderdelen van de schijf uitvalt, wordt uw virtuele machine niet beïnvloed omdat er twee extra kopieën ter ondersteuning van schijfaanvragen. Dit werkt goed, zelfs als twee verschillende hardware-onderdelen die ondersteuning bieden voor een schijf niet op hetzelfde moment (dit is zeldzame). 

Om ervoor te zorgen dat u altijd drie replica's moet onderhouden, een nieuw exemplaar van de gegevens op de achtergrond in Azure Storage automatisch worden gestart als een van de drie kopieert niet meer beschikbaar. Daarom mag niet nodig zijn voor het gebruik van RAID met Azure-schijven voor fouttolerantie. Een eenvoudige RAID-0-configuratie moet voldoende zijn voor het verwijderen van de schijven, indien nodig, grotere volumes maken.

Vanwege deze architectuur Azure consistent bedrijfsniveau heeft geleverd duurzaamheid voor IaaS-schijven, met een toonaangevende nul procent [per jaar Faalpercentage](https://en.wikipedia.org/wiki/Annualized_failure_rate).

Gelokaliseerde hardwarefouten in de berekening hosten of in de opslag-platform kunnen soms resultaat in van de tijdelijke onbeschikbaarheid van de virtuele machine die wordt gedekt door de [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) voor beschikbaarheid van de virtuele machine. Azure biedt ook een toonaangevende SLA voor één VM-exemplaren die gebruikmaken van Azure Premium-opslag-schijven.

Ter bescherming van toepassingsworkloads van downtime als gevolg van het tijdelijke ontbreken van een schijf of de VM, kunnen klanten gebruiken [beschikbaarheidssets](../articles/virtual-machines/windows/manage-availability.md). Twee of meer virtuele machines in een beschikbaarheidsset redundantie voor de toepassing. Azure maakt vervolgens deze virtuele machines en de schijven in afzonderlijke foutdomeinen met verschillende voeding, netwerk- en server-onderdelen. 

Vanwege deze afzonderlijke foutdomeinen gelokaliseerde hardwarefouten doorgaans hebben geen invloed op meerdere virtuele machines in de set op hetzelfde moment. Met afzonderlijke foutdomeinen biedt hoge beschikbaarheid voor uw toepassing. Er is een goede gewoonte beschikbaarheidssets gebruiken als hoge beschikbaarheid vereist is. De volgende sectie bevat informatie over het herstel na noodgevallen aspect.

### <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen

Herstel na noodgevallen is de mogelijkheid van zeldzame, maar belangrijke, incidenten wilt herstellen. Dit omvat tijdelijke, schaal fouten, zoals onderbreking van de service die gevolgen heeft voor een hele regio. Herstel na noodgevallen omvat gegevensback-up en te archiveren, en bevat mogelijk handmatige interventie, zoals het terugzetten van een database van een back-up.

Geïntegreerde beveiliging van de Azure-platform tegen gelokaliseerde fouten mogelijk niet volledig te beveiligen de virtuele machines/schijven als een noodgeval grootschalige storingen veroorzaakt. Dit omvat onherstelbare gebeurtenissen, zoals als een datacenter is bereikt, wordt door een orkaan, aardbeving, fire, of als er is een grootschalige eenheid hardwarefouten. U kunt bovendien storingen als gevolg van de toepassing of problemen tegenkomen.

Om te beschermen de werkbelasting van uw IaaS tegen storingen, moet u plannen voor redundantie en back-ups herstellen hebben. Voor herstel na noodgevallen, een back-up in een andere geografische locatie van de primaire site. Dit zorgt ervoor dat uw back-up wordt niet beïnvloed door de dezelfde gebeurtenis die oorspronkelijk van invloed op de virtuele machine of de schijven. Zie voor meer informatie [herstel na noodgevallen voor Azure-toepassingen](/azure/architecture/resiliency/disaster-recovery-azure-applications).

Uw DR-overwegingen zijn onder andere de volgende aspecten:

- Hoge beschikbaarheid: de mogelijkheid van de toepassing om te blijven uitvoeren in een foutloze toestand bevindt, zonder aanzienlijke uitvaltijd. Door *status in orde*, bedoelen we de toepassing reageert en kunnen gebruikers verbinding maken met de toepassing en ermee. Het is mogelijk dat bepaalde bedrijfskritieke toepassingen en databases vereist moet altijd beschikbaar, zelfs als er fouten van het platform zijn. Voor deze werkbelastingen moet u mogelijk de redundantie voor de toepassing, evenals de gegevens van plan bent.

- Gegevens duurzaamheid: In sommige gevallen kan de belangrijkste overweging is ervoor te zorgen dat de gegevens behouden blijven als een noodgeval gebeurt. Daarom moet u mogelijk een back-up van uw gegevens in een andere site. Voor deze werkbelastingen moet u mogelijk niet volledig redundantie voor de toepassing, maar alleen een reguliere-up van de schijven.

## <a name="backup-and-dr-scenarios"></a>Scenario's voor back-up en Noodherstel

Bekijk enkele typische voorbeelden van toepassing werklast scenario's en de aandachtspunten voor het plannen voor herstel na noodgevallen.

### <a name="scenario-1-major-database-solutions"></a>Scenario 1: Primaire databaseoplossingen

U kunt een productie-databaseserver, zoals SQL Server- of Oracle, die ondersteuning voor hoge beschikbaarheid bieden. Kritieke productietoepassingen en gebruikers, is afhankelijk van deze database. Het noodherstelplan voor dit systeem moet mogelijk de volgende vereisten ondersteunen:

- De gegevens moet zijn beveiligd en hersteld.
- De server moet beschikbaar zijn voor gebruik.

Het noodherstelplan mogelijk onderhouden van een replica van de database in een andere regio als een back-up. De oplossing kan afhankelijk van de vereisten voor beschikbaarheid van de server en herstel van gegevens op periodieke offline back-ups van de gegevens variëren van een actief-actief of actief / passief replica-site. Relationele databases, zoals SQL Server en Oracle, bieden verschillende opties voor replicatie. Gebruik voor SQL Server [SQL Server AlwaysOn-beschikbaarheidsgroepen](https://msdn.microsoft.com/library/hh510230.aspx) voor hoge beschikbaarheid.

NoSQL-databases, zoals MongoDB, bieden ook ondersteuning voor [replica's](https://docs.mongodb.com/manual/replication/) voor redundantie. De replica's voor hoge beschikbaarheid worden gebruikt.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Scenario 2: Een cluster van redundante virtuele machines

U kunt een werkbelasting die wordt verwerkt door een cluster van virtuele machines die redundantie en taakverdeling. Een voorbeeld hiervan is een Cassandra-cluster dat is geïmplementeerd in een regio. Dit type architectuur biedt al een hoog niveau van redundantie in deze regio. Echter om te voorkomen dat de werkbelasting een regionaal niveau fout, moet u het cluster verspreid over twee regio's of periodiek back-ups maken naar een andere regio.

### <a name="scenario-3-iaas-application-workload"></a>Scenario 3: IaaS toepassing werklast

Bekijk de IaaS toepassing werklast. Bijvoorbeeld, dit wordt mogelijk een standaard productie-werkbelasting die wordt uitgevoerd op een Azure VM. Het is mogelijk een webserver of bestandsserver die de inhoud en andere bronnen van een site. Het kan ook een op maat gemaakte business-toepassing uitgevoerd op een virtuele machine die de gegevens, bronnen en de status van toepassing zijn opgeslagen op de VM-schijven zijn. In dit geval is het belangrijk om ervoor te zorgen dat u rekening houden met back-ups regelmatig. Back-upfrequentie moet worden gebaseerd op de aard van de virtuele machine-werkbelasting. Bijvoorbeeld, als de toepassing wordt dagelijks uitgevoerd en gegevens worden gewijzigd, moet klikt u vervolgens de back-up worden gehouden om het uur.

Een ander voorbeeld is een rapportserver die gegevens ophaalt uit andere bronnen en genereert cumulatieve rapporten. Het verlies van deze virtuele machine of de schijven kan leiden tot verlies van de rapporten. Echter, het is mogelijk opnieuw uitvoeren van het proces voor rapportage en opnieuw genereren van de uitvoer. In dat geval niet echt is geïnfecteerd met een verlies van gegevens, zelfs als de rapportserver is bereikt, wordt met een noodgeval. Als gevolg hiervan moet u wellicht een hoger niveau van tolerantie voor verliezen deel van de gegevens op de rapportserver. In dat geval zijn minder frequente back-ups een optie om kosten te verlagen.

### <a name="scenario-4-iaas-application-data-issues"></a>Scenario 4: IaaS toepassingsproblemen gegevens

Problemen met de toepassing IaaS zijn mogelijk. U kunt een toepassing die wordt berekend, onderhouden en kritieke commerciële gegevens, zoals de prijsgegevens fungeert. Een nieuwe versie van uw toepassing heeft een software-fout die onjuist berekende prijzen en de bestaande commerce gegevens geleverd door het platform is beschadigd. Hier is de beste loop van de actie om de eerdere versie van de toepassing en de gegevens te herstellen. U kunt deze inschakelen worden periodiek back-ups van uw systeem.

## <a name="disaster-recovery-solution-azure-backup"></a>Noodherstel: Azure Backup 

[Azure Backup](https://azure.microsoft.com/services/backup/) wordt gebruikt voor back-ups en Noodherstel, en dit proces werkt met [schijven die worden beheerd](../articles/virtual-machines/windows/managed-disks-overview.md) , evenals [zonder begeleiding schijven](../articles/virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks). U kunt een back-uptaak maken met back-ups op basis van tijd eenvoudig herstel van de virtuele machine en back-up bewaarbeleid. 

Als u [Premium-opslag schijven](../articles/virtual-machines/windows/premium-storage.md), [schijven die worden beheerd](../articles/virtual-machines/windows/managed-disks-overview.md), of andere schijftypen met de [lokaal redundante opslag](../articles/storage/common/storage-redundancy.md#locally-redundant-storage) optie, is het belangrijk om ervoor te periodieke DR back-ups. Azure Backup slaat de gegevens in de recovery services-kluis op lange termijn. Kies de [geografisch redundante opslag](../articles/storage/common/storage-redundancy.md#geo-redundant-storage) optie voor de back-up herstellen services-kluis. Deze optie zorgt ervoor dat de back-ups worden gerepliceerd naar een andere Azure-regio voor de beveiliging van regionale noodsituaties.

Voor [zonder begeleiding schijven](../articles/virtual-machines/windows/about-disks-and-vhds.md#unmanaged-disks), kunt u het type lokaal redundante opslag gebruiken voor IaaS-schijven, maar zorg ervoor dat de Azure Backup is ingeschakeld met de optie geografisch redundante opslag voor de recovery services-kluis.

> [!NOTE]
> Als u de [geografisch redundante opslag](../articles/storage/common/storage-redundancy.md#geo-redundant-storage) of [geografisch redundante opslag met leestoegang](../articles/storage/common/storage-redundancy.md#read-access-geo-redundant-storage) optie voor uw niet-beheerde schijven u nog steeds moet toepassingsconsistente momentopnamen voor back-up en Noodherstel. Gebruik een [Azure Backup](https://azure.microsoft.com/services/backup/) of [toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots).

 De volgende tabel bevat een overzicht van de oplossingen die beschikbaar zijn voor herstel na Noodgevallen.

| Scenario | Automatische replicatie | Oplossing voor Noodherstel |
| --- | --- | --- |
| Premium-opslag-schijven | Lokale ([lokaal redundante opslag](../articles/storage/common/storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Managed Disks | Lokale ([lokaal redundante opslag](../articles/storage/common/storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Niet-beheerde schijven met lokaal redundante opslag | Lokale ([lokaal redundante opslag](../articles/storage/common/storage-redundancy.md#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Geografisch redundante opslag met niet-beheerde schijven | Cross-regio ([geografisch redundante opslag](../articles/storage/common/storage-redundancy.md#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots) |
| Niet-beheerde geografisch redundante opslag met leestoegang schijven | Cross-regio ([geografisch redundante opslag met leestoegang](../articles/storage/common/storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots) |

Hoge beschikbaarheid kan worden beste voldaan met behulp van beheerde schijven in een beschikbaarheidsset samen met Azure Backup. Als u niet-beheerde schijven gebruikt, kunt u Azure back-up voor herstel na Noodgevallen. Als u nog geen Azure back-up, duurt [toepassingsconsistente momentopnamen](#alternative-solution-consistent-snapshots), zoals beschreven in een volgende sectie wordt een alternatieve oplossing voor back-up en Noodherstel.

Uw keuzes voor hoge beschikbaarheid, back-up en Noodherstel op niveau van de toepassing of infrastructuur kunnen als volgt worden weergegeven:

| Niveau |   Hoge beschikbaarheid   | Back-up of DR |
| --- | --- | --- |
| Toepassing | SQL Server AlwaysOn | Azure Backup |
| Infrastructuur    | Beschikbaarheidsset  | Geografisch redundante opslag met toepassingsconsistente momentopnamen |

### <a name="using-azure-backup"></a>Met Azure back-up 

[Azure Backup](../articles/backup/backup-azure-vms-introduction.md) kunnen back-up van uw virtuele machines met Windows of Linux naar Azure recovery services-kluis. Back-up en herstellen van essentiële gegevens wordt bemoeilijkt door het feit dat essentiële bedrijfsgegevens moet een back-up terwijl de toepassingen die de gegevens produceren die worden uitgevoerd. 

Om dit probleem op te lossen, biedt Azure Backup toepassingsconsistente back-ups voor Microsoft-werkbelastingen. Dit maakt gebruik van de volume shadow service om ervoor te zorgen dat gegevens correct worden geschreven naar de opslag. Voor Linux VM's zijn alleen bestandsconsistente back-ups mogelijk, omdat Linux niet over functionaliteit in de volume shadow service beschikt.

![Azure Backup-stroom][1]

Wanneer Azure Backup een back-uptaak op het geplande tijdstip initieert, wordt de Backup-extensie die is geïnstalleerd in de virtuele machine om een punt in tijd momentopname geactiveerd. Een momentopname wordt gemaakt in coördinatie met de volume shadow service ophalen van een consistente momentopname van de schijven in de virtuele machine zonder af te sluiten. De Backup-extensie in de virtuele machine leegmaakacties alle schrijfbewerkingen alvorens een consistente momentopname te maken van alle schijven. Nadat u de momentopname, wordt de gegevens overgedragen door Azure Backup naar de back-upkluis. Als u het back-upproces efficiënter, wordt de service identificeert en alleen de blokken met gegevens die zijn gewijzigd na de laatste back-up overdraagt.

Als u wilt herstellen, kunt u bekijken van de beschikbare back-ups via Azure Backup en start vervolgens een herstelpunt. U kunt maken en herstellen van Azure back-ups via de [Azure-portal](https://portal.azure.com/), door [met behulp van PowerShell](../articles/backup/backup-azure-vms-automation.md), of met behulp van de [Azure CLI](/cli/azure/). 

### <a name="steps-to-enable-a-backup"></a>Stappen voor het inschakelen van een back-up

Gebruik de volgende stappen om in te schakelen van back-ups van uw virtuele machines met behulp van de [Azure-portal](https://portal.azure.com/). Er is een variatie afhankelijk van uw exacte scenario. Raadpleeg de [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) documentatie voor volledige informatie. Azure back-up ook [biedt ondersteuning voor virtuele machines met beheerde schijven](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Maak een recovery services-kluis voor een virtuele machine:

    a. In de [Azure-portal](https://portal.azure.com/), Bladeren **alle resources** en zoek **Recovery Services-kluizen**.

    b. Op de **Recovery Services-kluizen** menu, klikt u op **toevoegen** en volg de stappen voor het maken van een nieuwe kluis in dezelfde regio bevinden als de virtuele machine. Bijvoorbeeld, als uw virtuele machine zich in de regio VS-West, VS-West voor verzamelen de kluis.

2.  Controleer of de storage-replicatie voor de nieuwe kluis. Toegang tot de kluis onder **Recovery Services-kluizen** en Ga naar **instellingen** > **back-upconfiguratie**. Zorg ervoor dat de **geografisch redundante opslag** optie is standaard geselecteerd. Dit zorgt ervoor dat uw kluis automatisch wordt gerepliceerd naar een secundair datacenter. Uw kluis in VS-West is bijvoorbeeld automatisch gerepliceerd naar VS-Oost.

3.  Het back-beleid configureren en selecteer de virtuele machine in dezelfde gebruikersinterface.

4.  Zorg ervoor dat de Backup-Agent is geïnstalleerd op de virtuele machine. Als uw virtuele machine wordt gemaakt met behulp van een installatiekopie van een Azure-galerie, is klikt u vervolgens de Backup-Agent al geïnstalleerd. Anders (dat wil zeggen, als u een aangepaste installatiekopie), gebruikt u de instructies voor het [de VM-agent installeren op een virtuele machine](../articles/backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent-on-the-virtual-machine).

5.  Zorg ervoor dat de virtuele machine netwerkverbinding voor de Backup-service naar de functie toestaat. Volg de instructies voor [netwerkverbinding](../articles/backup/backup-azure-arm-vms-prepare.md#establish-network-connectivity).

6.  Nadat de vorige stappen zijn voltooid, wordt de back-up wordt uitgevoerd met regelmatige tussenpozen zoals opgegeven in de back-upbeleid. Indien nodig, kunt u de eerste back-up handmatig van het kluisdashboard op de Azure-portal activeren.

Raadpleeg voor Azure Backup automatiseren met behulp van scripts, [PowerShell-cmdlets voor VM-back-](../articles/backup/backup-azure-vms-automation.md).

### <a name="steps-for-recovery"></a>Stappen voor herstel

Als u wilt herstellen of opnieuw opbouwen van een virtuele machine, kunt u de virtuele machine herstellen uit een van de back-up herstelpunten in de kluis. Er zijn een aantal verschillende opties voor het uitvoeren van het herstel:

-   U kunt een nieuwe virtuele machine maken als een punt in tijd representatie van een back-up van de VM.

-   U kunt de schijven herstellen en vervolgens de sjabloon voor de virtuele machine aan te passen en de herstelde virtuele machine opnieuw gebruiken. 

Voor meer informatie, Zie de instructies voor het [de Azure portal gebruiken voor virtuele machines herstellen](../articles/backup/backup-azure-arm-restore-vms.md). Dit document wordt ook uitgelegd voor de procedure voor het herstellen van de back-up virtuele machines naar een gekoppeld datacenter met behulp van uw geografisch redundante back-upkluis als er een noodgeval op het primaire datacenter. Azure Backup gebruikt in dat geval de Compute-service van de secundaire regio voor de herstelde virtuele machine maken.

U kunt ook PowerShell voor [herstellen van een virtuele machine](../articles/backup/backup-azure-arm-restore-vms.md#restore-a-vm-during-an-azure-datacenter-disaster) of voor [schijven maken van een nieuwe virtuele machine uit hersteld](../articles/backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="alternative-solution-consistent-snapshots"></a>Alternatieve oplossing: toepassingsconsistente momentopnamen

Als u nog geen Azure Backup gebruiken, kunt u uw eigen back-mechanisme implementeren met behulp van momentopnamen. Toepassingsconsistente momentopnamen voor alle schijven die worden gebruikt door een virtuele machine maken en vervolgens deze momentopnamen worden gerepliceerd naar een andere regio is ingewikkeld. Om deze reden beschouwd Azure als een betere optie dan een aangepaste oplossing bouwen met behulp van de Backup-service. 

Als u geografisch redundante opslag/geografisch redundante opslag met leestoegang voor schijven, momentopnamen automatisch gerepliceerd naar een secundair datacenter. Als u lokaal redundante opslag voor schijven gebruikt, moet u de replicatie van de gegevens. Zie voor meer informatie [Back-up zonder begeleiding van Azure VM-schijven met incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md).

Een momentopname is een weergave van een object op een bepaald punt in tijd. Facturering voor de grootte van de gegevens van de incrementele deze blokkeringen leidt ertoe dat een momentopname. Zie voor meer informatie [momentopname maken van een blob](../articles/storage/blobs/storage-blob-snapshots.md).

### <a name="create-snapshots-while-the-vm-is-running"></a>Maken van momentopnamen terwijl de virtuele machine wordt uitgevoerd

Hoewel u een momentopname op elk gewenst moment uitvoeren kunt als de virtuele machine wordt uitgevoerd, er zijn nog steeds gegevens worden gestreamd naar de schijven en de momentopnamen bevatten mogelijk gedeeltelijk bewerkingen die tijdens de vlucht zijn. Als er meerdere schijven die zijn betrokken zijn, kunnen er ook de momentopnamen van verschillende schijven zijn opgetreden op verschillende tijdstippen. Dit betekent dat deze momentopnamen kunnen niet worden gecoördineerd. Dit is vooral problematisch voor striped volumes waarvan de bestanden is mogelijk beschadigd als wijzigingen tijdens de back-up wordt gemaakt.

Om deze situatie te voorkomen, moet het back-upproces implementeren de volgende stappen uit:

1.  Blokkeert alle schijven.

2.  Leegmaken van de in behandeling geschreven.

3.  [Momentopname maken van een blob](../articles/storage/blobs/storage-blob-snapshots.md) voor alle schijven.

Sommige Windows-toepassingen zoals SQL Server bieden een gecoördineerde back-mechanisme via een volume shadow service toepassingsconsistente back-ups maken. Op Linux, kunt u een hulpprogramma zoals fsfreeze voor de coördinatie van de schijven. Dit hulpprogramma biedt bestandsconsistente back-ups, maar niet toepassingsconsistente momentopnamen. Dit proces is complex, dus moet u overwegen [Azure Backup](../articles/backup/backup-azure-vms-introduction.md) of een back-upoplossing van derden die al deze procedure wordt geïmplementeerd.

Het vorige proces resulteert in een verzameling van gecoördineerde momentopnamen voor alle van de VM-schijven voor een bepaald punt in tijd weergave van de virtuele machine. Dit is een herstelpunt voor back-up voor de virtuele machine. U kunt het proces herhalen met regelmatige tussenpozen periodiek back-ups maken. Zie [kopiëren van de back-ups naar een andere regio](#copy-the-snapshots-to-another-region) voor stappen voor het kopiëren van de momentopnamen voor een andere regio voor herstel na Noodgevallen.

### <a name="create-snapshots-while-the-vm-is-offline"></a>Maken van momentopnamen terwijl de virtuele machine offline is.

Een andere optie voor het maken van consistente back-ups is het afsluiten van de virtuele machine en momentopnamen van blob van elke schijf. Maken van momentopnamen van de blob is eenvoudiger dan het coördineren van momentopnamen van een actieve virtuele machine, maar een paar minuten uitvaltijd is vereist.

1. De virtuele machine afgesloten.

2. Maak een momentopname van elke blob virtuele harde schijf, die slechts enkele seconden duurt.

    U kunt gebruiken voor het maken van een momentopname [PowerShell](../articles/storage/common/storage-powershell-guide-full.md), wordt de [REST-API van Azure Storage](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](/cli/azure/), of een van de clientbibliotheken van Azure Storage, zoals [de Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx).

3. Start de virtuele machine, waardoor de uitvaltijd eindigt. Normaal gesproken het hele proces is voltooid binnen een paar minuten.

Dit proces resulteert in een verzameling van toepassingsconsistente momentopnamen voor alle schijven bieden een back-up herstelpunt voor de virtuele machine.

### <a name="copy-the-snapshots-to-another-region"></a>De momentopnamen kopiëren naar een andere regio

Het maken van de momentopnamen die alleen zijn mogelijk niet voldoende is voor herstel na Noodgevallen. U moet ook de momentopnameback-ups worden gerepliceerd naar een andere regio.

Als u geografisch redundante opslag of geografisch redundante opslag met leestoegang voor de schijven, en vervolgens de momentopnamen automatisch naar de secundaire regio worden gerepliceerd. Er zijn een paar minuten vertraging vóór de replicatie. Als het primaire datacenter wordt uitgeschakeld voordat de momentopnamen voltooien repliceren, kunt u de momentopnamen niet openen van het secundaire datacenter. De kans is klein.

> [!NOTE] 
> Alleen dat de schijven in een geografisch redundante opslag of leestoegang geografisch redundante worden storage-account de virtuele machine niet beveiligd tegen rampen. U moet ook gecoördineerde momentopnamen maken of gebruiken van Azure Backup. Dit is vereist voor het herstellen van een virtuele machine naar een consistente status.

Als u lokaal redundante opslag gebruikt, moet u de momentopnamen kopiëren naar een ander opslagaccount onmiddellijk na het maken van een momentopname. Het doel voor het kopiëren is mogelijk een account met lokaal redundante opslag in een andere regio, wat resulteert in het exemplaar wordt in een externe regio. U kunt ook de momentopname kopiëren naar een geografisch redundante opslag met leestoegang account in dezelfde regio. In dit geval wordt de momentopname traag gerepliceerd naar de externe secundaire regio. Uw back-up is beveiligd tegen rampen op de primaire site na het kopiëren en replicatie is voltooid.

Als u wilt kopiëren efficiënt uw incrementele momentopnamen voor herstel na Noodgevallen, lees de instructies in [Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md).

![Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen][2]

### <a name="recovery-from-snapshots"></a>Herstel van momentopnamen

Voor het ophalen van een momentopname, zodat een nieuwe blob te kopiëren. Als u de momentopname van de primaire account kopieert, kunt u de momentopname via kopiëren naar de base blob van de momentopname. Als u de schijf voor de momentopname wordt teruggezet dit proces. Dit proces staat bekend als het promoveren van de momentopname. Als u bij het kopiëren van de momentopname back-up van een secundaire-account in het geval van een account geografisch redundante opslag met leestoegang kopieert u deze aan een primaire-account. U kunt een momentopname door kopiëren [met behulp van PowerShell](../articles/storage/common/storage-powershell-guide-full.md) of met het AzCopy-hulpprogramma. Zie voor meer informatie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).

Voor virtuele machines met meerdere schijven, moet u de momentopnamen die deel van hetzelfde gecoördineerde herstelpunt uitmaken kopiëren. Nadat u de momentopnamen naar beschrijfbare VHD-blobs kopiëren, kunt u de blobs kunt gebruiken om uw virtuele machine met behulp van de sjabloon voor de virtuele machine opnieuw te maken.

## <a name="other-options"></a>Andere opties

### <a name="sql-server"></a>SQL Server

SQL Server wordt uitgevoerd op een virtuele machine heeft een eigen ingebouwde mogelijkheden voor delen maakt u een back-up van uw SQL Server-database naar Azure Blob-opslag of een bestand. Als het opslagaccount geografisch redundante opslag of geografisch redundante opslag met leestoegang is, kunt u deze back-ups in het opslagaccount secundair datacenter er in een noodsituatie openen met de dezelfde beperkingen, zoals eerder besproken. Zie voor meer informatie [Back-up en herstel voor SQL Server op Azure virtual machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md). In aanvulling op back-up en herstellen, [SQL Server AlwaysOn-beschikbaarheidsgroepen](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) secundaire replica's van databases kunt onderhouden. Dit vermindert aanzienlijk de hersteltijd na noodgevallen.

## <a name="other-considerations"></a>Andere overwegingen

In dit artikel is besproken hoe u back-up of momentopnamen van uw virtuele machines en hun schijven voor de ondersteuning van herstel na noodgevallen en hoe u die uw gegevens te herstellen. Met het Azure Resource Manager-model veel mensen sjablonen gebruiken hun virtuele machines en andere infrastructuren maken in Azure. Een sjabloon kunt u een virtuele machine met dezelfde configuratie elke keer maken. Als u aangepaste installatiekopieën gebruikt voor het maken van uw virtuele machines, moet u zorgen dat uw afbeeldingen met behulp van een account geografisch redundante opslag met leestoegang voor het opslaan van deze zijn beschermd.

Uw back-upproces kan als gevolg daarvan kan een combinatie van twee dingen zijn:

- Back-up van de gegevens (schijven).
- Back-up van de configuratie (sjablonen en aangepaste installatiekopieën).

Mogelijk moet u de back-up van zowel de gegevens en de configuratie te verwerken, afhankelijk van de back-optie die u kiest, of de Backup-service al die voor u kan verwerken.

## <a name="appendix-understanding-the-impact-of-data-redundancy"></a>Bijlage: De invloed van gegevensredundantie

Voor de storage-accounts in Azure, er zijn drie typen van de redundantie van de gegevens die u met betrekking tot herstel na noodgevallen overwegen moet: lokaal redundante geografisch redundante of geografisch redundante met leestoegang. 

Lokaal redundante opslag behoudt drie kopieën van de gegevens in hetzelfde datacenter. Wanneer de gegevens voor de virtuele machine wordt geschreven, worden alle drie kopieën worden bijgewerkt voordat succes aan de aanroeper wordt geretourneerd, zodat u weet dat ze identiek zijn. De schijf is beveiligd tegen lokale stroomstoringen, omdat het is zeer onwaarschijnlijk dat alle drie kopieën op hetzelfde moment worden beïnvloed. In het geval van een lokaal redundante opslag is er geen geografische redundantie, zodat de schijf niet is beveiligd tegen kritieke fouten optreden die een datacenter of opslag eenheid kunnen beïnvloeden.

Met geografisch redundante opslag en geografisch redundante opslag met leestoegang worden drie kopieën van uw gegevens in de primaire regio die wordt geselecteerd door u worden bewaard. Drie extra kopieën van uw gegevens worden bewaard in een bijbehorende secundaire regio die door Azure is ingesteld. Bijvoorbeeld, als u gegevens in VS-West opslaat, wordt de gegevens gerepliceerd naar VS-Oost. Kopie bewaren asynchroon wordt uitgevoerd en er is een kleine vertraging optreden tussen de updates naar de primaire en secundaire sites. Replica's van de schijven op de secundaire site zijn consistent op basis van per schijf (met de vertraging), maar de replica's van meerdere actieve schijven mogelijk niet synchroon met elkaar. Toepassingsconsistente momentopnamen zijn om een consistente replica's maken over meerdere schijven, nodig.

Het belangrijkste verschil tussen geografisch redundante opslag en geografisch redundante opslag met leestoegang is met geografisch redundante opslag met leestoegang, kunt u de secundaire kopie lezen op elk gewenst moment. Als er een probleem dat de gegevens in de primaire regio niet toegankelijk weergeeft, kunt u het team van Azure mijden toegang herstellen. Terwijl de primaire niet beschikbaar is, hebt u geografisch redundante opslag met leestoegang is ingeschakeld, kunt u toegang tot de gegevens in het secundaire datacenter. Dus als u van plan bent om te lezen van de replica als de primaire niet toegankelijk is, klikt u vervolgens geografisch redundante opslag met leestoegang rekening.

Als blijkt te zijn van een aanzienlijke onderbreking, kan het team van Azure activeert u een geo-failover en het wijzigen van de primaire DNS-vermeldingen om te verwijzen naar de secundaire opslag. Op dit moment hebt u geografisch redundante opslag of geografisch redundante opslag met leestoegang is ingeschakeld, kunt u de gegevens in de regio die wordt gebruikt om te worden van de secundaire openen. Met andere woorden, als uw storage-account geografisch redundante opslag is en er een probleem is, u kunt toegang tot de secundaire opslag alleen als er een geo-failover.

Zie voor meer informatie [wat te doen als een Azure Storage-storing optreedt,](../articles/storage/common/storage-disaster-recovery-guidance.md). 

>[!NOTE] 
>Microsoft controleert of er een failover plaatsvindt. Failover wordt niet beheerd per storage-account, zodat deze niet wordt bepaald door de individuele klanten. Voor het herstel na noodgevallen voor specifieke storage-accounts of schijven op virtuele machine implementeren, moet u de technieken die eerder in dit artikel wordt beschreven.



[1]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png

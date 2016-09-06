<properties
    pageTitle="Overzicht van Azure Batch-functies voor ontwikkelaars | Microsoft Azure"
    description="Informatie over de functies van de Batch-service en de API's ervan, vanuit het standpunt van ontwikkelaars."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# Overzicht van Batch-functies voor ontwikkelaars

In dit overzicht van de kernonderdelen van de Azure Batch-service worden de primaire servicefuncties en resources besproken die Batch-ontwikkelaars kunnen gebruiken voor het bouwen van grootschalige parallelle rekenoplossingen.

Of u nu een gedistribueerde rekenkundige toepassing of service ontwikkelt die directe [Batch REST][batch_rest_api]-API-aanroepen uitgeeft of een van de [Batch SDK's](batch-technical-overview.md#batch-development-apis) gebruikt, u maakt gebruik van veel van de resources en functies die in dit artikel worden besproken.

> [AZURE.TIP] Zie [Basisbeginselen van Azure Batch](batch-technical-overview.md) voor een introductie op hoger niveau van de Batch-service.

## Werkstroom van de Batch-service

De volgende werkstroom op hoog niveau is gangbaar voor bijna alle toepassingen en services die gebruikmaken van de Batch-service voor de verwerking van parallelle workloads:

1. Upload de **gegevensbestanden** die u wilt verwerken voor een [Azure-opslagaccount][azure_storage]. Batch bevat ingebouwde ondersteuning voor toegang tot Azure-blobopslag en uw taken kunnen deze bestanden downloaden naar [rekenknooppunten](#compute-node) wanneer de taken worden uitgevoerd.

2. Upload de **toepassingsbestanden** die uw taken gaan uitvoeren. Deze bestanden kunnen binaire bestanden of scripts en ervan afhankelijke elementen zijn, en worden uitgevoerd door de taken in uw jobs. Uw taken kunnen deze bestanden downloaden uit uw opslagaccount, maar u kunt ook de functie voor [toepassingspakketten](#application-packages) van Batch voor het beheren en implementeren van toepassingen gebruiken.

3. Maak een [pool](#pool) van rekenknooppunten. Wanneer u een pool maakt, geeft u het aantal rekenknooppunten voor de pool, de grootte en het besturingssysteem op. Wanneer elke taak in de job wordt uitgevoerd, wordt deze voor uitvoering toegewezen aan een van de knooppunten in uw pool.

4. Maak een [job](#job). Een job beheert een verzameling van taken. U koppelt elke job aan een specifieke pool waar de taken van die job worden uitgevoerd.

5. Voeg [taken](#task) toe aan de job. Elke taak voert de toepassing of het script uit die door u is geüpload voor het verwerken van de uit uw opslagaccount gedownloade gegevensbestanden. Wanner een taak is voltooid, kunt u de uitvoer ervan uploaden naar Azure Storage.

6. Bewaak de voortgang van de taak en haal de taakuitvoer op uit Azure Storage.

In de volgende secties worden deze en andere resources van Batch besproken waarmee u uw gedistribueerde rekenkundige scenario kunt uitvoeren.

> [AZURE.NOTE] U hebt een [Batch-account](batch-account-create-portal.md) nodig om de Batch-service te gebruiken. Bovendien gebruiken vrijwel alle oplossingen een [Azure-opslagaccount][azure_storage] om bestanden op te slaan en op te halen. Batch ondersteunt momenteel alleen het opslagaccounttype **Algemeen**, zoals beschreven in stap 5 van [Een opslagaccount maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/storage-create-storage-account.md).

## Batch-serviceresources

Sommige van de volgende resources (accounts, rekenknooppunten, pools, jobs en taken) zijn vereist zijn voor alle oplossingen die gebruikmaken van de Batch-service. Andere resources, zoals taakplanningen en toepassingspakketten, zijn nuttige, maar optionele functies.

- [Account](#account)
- [Rekenknooppunt](#compute-node)
- [Pool](#pool)
- [Job](#job)

  - [Jobplanningen](#scheduled-jobs)

- [Taak](#task)

  - [Begintaak](#start-task)
  - [Jobbeheertaak](#job-manager-task)
  - [Jobvoorbereidingstaken en jobvrijgevingstaken](#job-preparation-and-release-tasks)
  - [Taken met meerdere instanties (MPI)](#multi-instance-tasks)
  - [Taakafhankelijkheden](#task-dependencies)

- [Toepassingspakketten](#application-packages)

## Account

Een Batch-account is een uniek geïdentificeerde entiteit in de Batch-service. Alle verwerkingen zijn gekoppeld aan een Batch-account. Wanneer u met de Batch-service bewerkingen wilt uitvoeren, hebt u de accountnaam en een van de bijbehorende accountsleutels nodig. U kunt [een Azure Batch-account in de Azure Portal maken](batch-account-create-portal.md).

## Rekenknooppunt

Een rekenknooppunt is een virtuele machine (VM) van Azure die aan een specifiek deel van de workload van uw toepassing is toegewezen. De grootte van een knooppunt bepaalt het aantal CPU-kernen, de geheugencapaciteit en de grootte van het lokale bestandssysteem die aan het knooppunt worden toegewezen. U kunt pools van Windows- of Linux-knooppunten maken met behulp van Azure Cloud Services of Virtual Machines Marketplace-installatiekopieën. Zie de volgende sectie ([Pool](#pool)) voor meer informatie over deze opties.

Knooppunten kunnen elk uitvoerbaar bestand of script uitvoeren dat wordt ondersteund door de besturingssysteemomgeving van het knooppunt. Dit omvat \*.exe \*.cmd, \*.bat en PowerShell-scripts voor Windows - en binaire bestanden, shell- en Python-scripts voor Linux.

Alle rekenknooppunten in Batch omvatten ook:

- Een standaard[mapstructuur](#files-and-directories) en daaraan gekoppelde [omgevingsvariabelen](#environment-settings-for-tasks) die beschikbaar zijn voor verwijzing door taken.
- **Firewall**instellingen die zijn geconfigureerd om de toegang te beheren.
- [Externe toegang](#connecting-to-compute-nodes) voor Windows-knooppunten (Remote Desktop Protocol (RDP)) en Linux-knooppunten (Secure Shell (SSH)).

> [AZURE.NOTE] Linux-ondersteuning in Batch wordt momenteel nog verder ontwikkeld. Zie [Linux-rekenknooppunten in Azure Batch-pools inrichten](batch-linux-nodes.md) voor meer informatie.

## Pool

Een pool is een verzameling knooppunten waarop uw toepassing wordt uitgevoerd. De pool kan door u handmatig worden gemaakt of automatisch door de Batch-service worden gemaakt wanneer u het werk opgeeft dat moet worden uitgevoerd. U kunt een pool maken en beheren die voldoet aan de resourcevereisten van uw toepassing. Een pool kan alleen worden gebruikt door de Batch-account waarin deze is gemaakt. Een Batch-account kan meer dan één pool hebben.

Azure Batch-groepen worden gebouwd boven op het kernrekenplatform van Azure. Deze bieden grootschalige toewijzing, installatie van toepassingen, gegevensdistributie, statuscontrole en flexibele aanpassing van het aantal rekenknooppunten binnen een pool ([vergroten/verkleinen](#scaling-compute-resources)).

Aan elk knooppunt dat aan een pool wordt toegevoegd, wordt een unieke naam en een uniek IP-adres toegewezen. Wanneer een knooppunt uit een pool wordt verwijderd, gaan alle wijzigingen in het besturingssysteem of de bestanden verloren. De naam en het IP-adres van het verwijderde knooppunt worden vrijgegeven voor toekomstig gebruik. Wanneer een knooppunt een pool verlaat, is de levensduur ervan beëindigd.

Wanneer u een pool maakt, kunt u de volgende kenmerken opgeven:

- **Besturingssysteem** en **versie** van rekenknooppunt

    Wanneer u een besturingssysteem voor de knooppunten in de pool selecteert, beschikt u over twee opties: **Virtuele-machineconfiguratie** en **Cloud Services-configuratie**.

    **Virtuele-machineconfiguratie** biedt Linux- en Windows-installatiekopieën voor rekenknooppunten via de [Azure Virtual Machines Marketplace][vm_marketplace].
    Wanneer u een pool met virtuele-machineconfiguratieknooppunten maakt, moet u niet alleen de grootte van de knooppunten opgeven, maar ook de **verwijzing naar de installatiekopie van de virtuele machine** en de Batch-**knooppuntagent-SKU** die op de knooppunten moet worden geïnstalleerd. Zie [Linux-rekenknooppunten in Azure Batch-pools inrichten](batch-linux-nodes.md) voor meer informatie over het opgeven van deze pooleigenschappen.

    **Cloud Services-configuratie** biedt *alleen* Windows rekenknooppunten. Beschikbare besturingssystemen voor Cloud Services-configuratiepools worden weergegeven in de [Azure-compatibiliteitsmatrix voor releases van gastbesturingssystemen en SDK’s](../cloud-services/cloud-services-guestos-update-matrix.md). Wanneer u een pool met Cloud Services-knooppunten maakt, moet u de grootte van het knooppunt en het bijbehorende *type besturingssysteem* opgeven. Wanneer u pools met Windows rekenknooppunten maakt, maakt u meestal gebruik van Cloud Services.

    - Het *type besturingssysteem* is ook bepalend voor de versies van .NET die samen met het besturingssysteem zijn geïnstalleerd.
    - Net als bij werkrollen in Cloud Services kan het *type besturingssysteem* worden opgegeven. (Zie de sectie [Informatie over Cloud Services](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) in [Overzicht van Cloud Services](../cloud-services/cloud-services-choose-me.md) voor meer informatie over werkrollen.)
    - Net als bij werkrollen verdient het aanbeveling om `*` op te geven voor de *versie van het besturingssysteem*, zodat de knooppunten automatisch worden bijgewerkt en er geen werk vereist is om tegemoet te komen aan nieuwe versies. Er wordt voornamelijk voor een specifieke versie van een besturingssysteem gekozen om ervoor te zorgen dat toepassingen compatibel blijven, en om compatibiliteitstests met eerdere versies te kunnen uitvoeren alvorens toe te staan dat de versie mag worden bijgewerkt. Na de validatie kan de *versie van het besturingssysteem* voor de pool worden bijgewerkt en kan de nieuwe installatiekopie van het besturingssysteem worden geïnstalleerd. Elke actieve taak wordt onderbroken en opnieuw in de wachtrij geplaatst.

- **Grootte van de knooppunten**

    De grootte van rekenknooppunten met **Cloud Services-configuratie** wordt vermeld in [Groottes voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Batch ondersteunt alle Cloud Services van alle groottes met uitzondering van `ExtraSmall`.

    De grootte van rekenknooppunten met **virtuele-machineconfiguratie** wordt vermeld in [Groottes voor virtuele machines in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) en [Groottes voor virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Batch ondersteunt alle Azure VM-groottes met uitzondering van `STANDARD_A0` en die met Premium Storage (de serie `STANDARD_GS`, `STANDARD_DS` en `STANDARD_DSV2`).

    Houd bij het kiezen van een knooppuntgrootte rekening met de kenmerken en vereisten van de toepassingen die op de knooppunten zullen worden uitgevoerd. Meestal selecteert u een knooppuntgrootte in de veronderstelling dat er op het knooppunt één taak tegelijk wordt uitgevoerd. Houd ook rekening met het feit of bijvoorbeeld de toepassing meerdere threads heeft en hoeveel geheugen deze gebruikt. Zo kunt u gemakkelijker de meest geschikte en voordeligste knooppuntgrootte bepalen. Het is mogelijk om meerdere taken te hebben waarbij meerdere toepassingsinstanties [parallel worden uitgevoerd](batch-parallel-node-tasks.md). In dat geval kiest u meestal voor een groter knooppunt. Zie de volgende sectie (Taakplanningsbeleid) voor meer informatie.

    Alle knooppunten in een pool zijn even groot. Als u toepassingen met verschillende systeemvereisten en/of workloadniveaus uitvoert, moet u afzonderlijke pools maken.

- **Beoogd aantal knooppunten**

    Dit is het aantal rekenknooppunten dat u in de pool wilt implementeren. Dit wordt aangeduid als *beoogd* omdat de pool in sommige gevallen mogelijk niet het gewenste aantal knooppunten bereikt. Een pool bereikt mogelijk niet het gewenste aantal knooppunten als deze het [kernquotum](batch-quota-limit.md#batch-account-quotas) voor uw Batch-account bereikt of als er een formule voor automatisch vergroten/verkleinen is die u hebt toegepast op de pool die het maximumaantal knooppunten bereikt. Zie de volgende sectie (Beleid voor vergroten/verkleinen).

- **Beleid voor vergroten/verkleinen**

    Als u geen vast aantal knooppunten wilt opgeven, kunt u in plaats daarvan een [formule voor automatisch vergroten/verkleinen](#scaling-compute-resources) voor een groep schrijven en toepassen. De Batch-service evalueert de formule periodiek en past het aantal knooppunten in de pool aan op basis van verschillende pool-, job- en taakparameters die u kunt opgeven.

- **Taakplanningsbeleid**

    De configuratieoptie [Maximumaantal taken per knooppunt](batch-parallel-node-tasks.md) bepaalt het maximumaantal taken dat parallel kan worden uitgevoerd op elk rekenknooppunt in de pool.

    Bij de standaardconfiguratie wordt er op een knooppunt één taak tegelijk uitgevoerd, maar er zijn scenario's waarin het nuttig is om meer dan één taak tegelijk op een knooppunt uit te voeren. Zie het [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het artikel over [gelijktijdige knooppunttaken](batch-parallel-node-tasks.md) om te bekijken hoe u kunt profiteren van meerdere taken per knooppunt.

    U kunt ook een *opvultype* opgeven dat bepaalt of Batch de taken gelijkmatig verspreidt over alle knooppunten in een pool of in elk knooppunt het maximumaantal taken gebruikt alvorens taken toe te wijzen aan een ander knooppunt.

- **Communicatiestatus** van rekenknooppunten

    In de meeste gevallen functioneren taken onafhankelijk en hoeven ze niet met elkaar te communiceren. Maar mogelijk hebt u ook toepassingen waarin taken moeten communiceren (bijvoorbeeld [MPI-scenario's](batch-mpi.md)).

    U kunt een pool zodanig configureren dat **communicatie tussen knooppunten** in de pool is toegestaan. Wanneer communicatie tussen knooppunten is ingeschakeld, kunnen knooppunten in de configuratie voor Cloud Services-pools met elkaar communiceren op poorten die groter zijn dan 1100. Voor virtuele-machineconfiguratiepools is verkeer op geen enkele poort beperkt.

    Houd er rekening mee dat het inschakelen van communicatie tussen knooppunten ook van invloed is op de plaatsing van de knooppunten binnen clusters en het maximumaantal knooppunten in een groep mogelijk wordt beperkt door implementatiebeperkingen. Als voor uw toepassing geen communicatie tussen knooppunten is vereist, kan de Batch-service een potentieel groter aantal knooppunten aan de pool toewijzen vanuit vele verschillende clusters en datacenters, om meer parallelle verwerkingskracht mogelijk te maken.

- **Begintaak** voor rekenknooppunten

    De optionele *begintaak* wordt in elk knooppunt uitgevoerd wanneer dat knooppunt aan de pool wordt toegevoegd en telkens wanneer dat knooppunt opnieuw wordt opgestart of er een nieuwe installatiekopie van wordt gemaakt. De begintaak is vooral handig voor de voorbereiding van rekenknooppunten voor het uitvoeren van taken, zoals het installeren van de toepassingen die door de taken worden uitgevoerd.

- **Toepassingspakketten**

    U kunt [toepassingspakketten](#application-packages) opgeven die moeten worden geïmplementeerd in de rekenknooppunten in de groep. Toepassingspakketten bieden vereenvoudigde implementatie en versies van de toepassingen die de taken uitvoeren. Toepassingspakketten die u voor een groep van toepassingen opgeeft, worden geïnstalleerd op elk knooppunt dat lid wordt van de groep, en elke keer dat er een knooppunt opnieuw wordt opgestart of er een installatiekopie wordt hersteld.

- **Netwerkconfiguratie**

    U kunt de id van een [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md) van Azure opgeven waarin de rekenknooppunten van de groep moeten worden gemaakt. U kunt de vereisten voor het opgeven van een VNet voor uw groep vinden in [Een groep toevoegen aan een account][vnet] in de Batch REST API-referentie.

> [AZURE.IMPORTANT] Alle Batch-accounts hebben een standaard**quotum** dat het aantal **kernen** (en dus rekenknooppunten) in een Batch-account beperkt. U vindt de standaardquota en instructies over het [verhogen van een quotum](batch-quota-limit.md#increase-a-quota) (zoals het maximum aantal kernen in uw Batch-account) in [Quotas and limits for the Azure Batch service](batch-quota-limit.md) (Quota en limieten voor de Azure Batch-service). Rijzen er vragen zoals "Waarom kan mijn pool niet meer dan X knooppunten bevatten?", dan is dit quotum voor kernen mogelijk de oorzaak.

## Job

Een job is een verzameling taken. Deze beheert hoe de berekening door de taken op rekenknooppunten in een pool wordt uitgevoerd.

- De job bepaalt de **pool** waarin het werk wordt uitgevoerd. U kunt voor elke job een nieuwe pool maken of één groep gebruiken voor een groot aantal jobs. U kunt een pool maken voor elke job die aan een jobplanning is gekoppeld, maar ook voor alle jobs die aan een jobplanning zijn gekoppeld.

- U kunt desgewenst een **jobprioriteit** opgeven. Wanneer er een job wordt verzonden met een hogere prioriteit dan de jobs die momenteel worden uitgevoerd, worden de taken van de job met hogere prioriteit in de wachtrij ingevoegd vóór de taken van de jobs met lagere prioriteit. Taken met lagere prioriteit die al worden uitgevoerd, kunnen niet worden verschoven.

- Met **beperkingen** voor jobs kunt u bepaalde limieten aan uw taken stellen:

    U kunt een **maximale kloktijd** instellen, zodat als de uitvoering van een job langer duurt dan de maximale kloktijd die is opgegeven, de job en alle bijbehorende taken worden beëindigd.

    Batch kan dit detecteren en vervolgens de mislukte taken opnieuw uitvoeren. U kunt het **maximumaantal nieuwe pogingen voor een taak** opgegeven als beperking. U kunt er ook voor kiezen om *altijd* of *nooit* een poging te doen om een taak opnieuw uit te voeren. Wanneer u een taak opnieuw probeert uit te voeren, wordt deze weer in de wachtrij geplaatst om opnieuw te worden uitgevoerd.

- De clienttoepassing kan taken toevoegen aan een taak, maar u kunt ook een [jobbeheertaak](#job-manager-task) opgeven. Een jobbeheertaak bevat de benodigde informatie om de vereiste taken voor een job te maken, waarbij de jobbeheertaak wordt uitgevoerd op een van de rekenknooppunten binnen de pool. De jobbeheertaak wordt specifiek door Batch verwerkt en wordt in de wachtrij geplaatst zodra de job is gemaakt en opnieuw opgestart als deze mislukt. Een jobbeheertaak is *vereist* voor jobs die door een [jobplanning](#scheduled-jobs) zijn gemaakt, omdat dit de enige manier is om de taken te definiëren voordat de job wordt geïnstantieerd.

- Standaard blijven jobs in de actieve status wanneer alle taken binnen de job zijn voltooid. U kunt dit gedrag wijzigen zodat de job automatisch wordt beëindigd wanneer alle taken binnen de job zijn voltooid. Stel de eigenschap **onAllTasksComplete** ([OnAllTasksComplete][net_onalltaskscomplete] in Batch .NET) van de job in op *terminatejob* om de job automatisch te beëindigen wanneer alle taken de status voltooid hebben.

    Houd er rekening mee dat de Batch-service een job *zonder* taken ziet als een job waarvan alle taken zijn voltooid. Daarom wordt deze optie meestal gebruikt met een [Jobbeheertaak](#job-manager-task). Als u de automatische beëindiging van een job wilt gebruiken zonder jobbeheer, moet u eerst de eigenschap **onAllTasksComplete** van een nieuwe job instellen op *noaction*. Vervolgens stelt u de eigenschap in op *terminatejob* als u klaar bent met taken toevoegen aan de job.

### Jobprioriteit

Aan jobs die u in Batch maakt, kunt u een prioriteit toewijzen. De Batch-service gebruikt de prioriteit van de job om de volgorde van de jobplanning binnen een account te bepalen (verwar dit niet met een [geplande job](#scheduled-jobs)). De prioriteitswaarden gaan van -1000 tot 1000, waarbij -1000 de laagste prioriteit is en 1000 de hoogste. U kunt de prioriteit van een job bijwerken door gebruik te maken van de bewerking [Update the properties of a job (De eigenschappen van een job bijwerken)][rest_update_job] (Batch REST) of door de eigenschap [CloudJob.Priority][net_cloudjob_priority] (Batch .NET) te wijzigen.

Binnen hetzelfde account hebben jobs met hogere prioriteit in de planning voorrang op jobs met lagere prioriteit. Een job met hogere prioriteit in het ene account heeft geen planningsvoorrang op een andere job met een lagere prioriteitswaarde in een ander account.

De jobplanning tussen pools verloopt onafhankelijk. Tussen verschillende pools is er geen garantie dat een job met hogere prioriteit eerst wordt gepland als de gekoppelde pool over te weinig niet-actieve knooppunten beschikt. Binnen dezelfde pool hebben jobs met dezelfde prioriteit evenveel kans om gepland te worden.

### Geplande jobs

Met behulp van [jobplanningen][rest_job_planningen] kunt u in de Batch-service terugkerende jobs maken. Een jobplanning geeft aan wanneer u jobs moet uitvoeren en bevat de specificaties voor de uit te voeren jobs. U kunt de duur van de planning opgeven (hoelang en wanneer de planning geldt) en hoe vaak in dat tijdsbestek jobs zouden moeten worden gemaakt.

## Taak

Een taak is een rekeneenheid die aan een job is gekoppeld. Deze wordt uitgevoerd op een knooppunt. Taken worden toegewezen aan een knooppunt om te worden uitgevoerd of in een wachtrij geplaatst tot een knooppunt beschikbaar is. Eenvoudig gesteld: een taak voert een of meer programma's of scripts uit op een rekenknooppunt om het benodigde werk uit te voeren.

Wanneer u een taak maakt, kunt u het volgende opgeven:

- De **opdrachtregel** van de taak. Dit is de opdrachtregel die uw toepassing of script op het rekenknooppunt uitvoert.

    Houd er rekening mee dat de opdrachtregel niet daadwerkelijk wordt uitgevoerd onder een shell. Daarom kan deze niet op systeemeigen wijze profiteren van shell-functies zoals [omgevingsvariabele](#environment-settings-for-tasks)-uitbreiding (dit omvat het `PATH`). Als u wilt profiteren van dergelijke functies, start u de shell op de opdrachtregel (bijvoorbeeld door het starten van `cmd.exe` op Windows-knooppunten of `/bin/sh` op Linux):

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Als uw taken toepassingen of scripts moeten uitvoeren die zich niet in het `PATH` van het knooppunt bevinden of verwijzen naar omgevingsvariabelen, start u de shell expliciet in de opdrachtregel van de taak.

- **Bronbestanden** die de te verwerken gegevens bevatten. In een Azure-opslagaccount van het type **Algemeen** worden deze bestanden voordat de opdrachtregel van de taak wordt uitgevoerd, automatisch vanuit blobopslag naar het knooppunt gekopieerd. Zie de secties [Begintaak](#start-task) en [Bestanden en mappen](#files-and-directories) voor meer informatie.

- De **omgevingsvariabelen** die zijn vereist voor de toepassing. Zie de sectie [Omgevingsinstellingen voor taken](#environment-settings-for-tasks) voor meer informatie.

- De **beperkingen** waaronder de taak moet worden uitgevoerd. Bijvoorbeeld de maximale duur dat de taak mag worden uitgevoerd, het maximumaantal keren dat een taak opnieuw moet worden uitgevoerd indien deze mislukt en de maximale duur dat bestanden in de werkmap van de taak behouden blijven.

- **Toepassingspakketten** voor het implementeren in het rekenknooppunt waarop de taak staat gepland voor uitvoering. [Toepassingspakketten](#application-packages) bieden vereenvoudigde implementatie en versies van de toepassingen die de taken uitvoeren. Toepassingspakketten op taakniveau zijn met name nuttig in omgevingen met gedeelde groepen. Verschillende jobs worden uitgevoerd op één groep en de groep wordt niet verwijderd wanneer een job is voltooid. Als de job minder taken dan knooppunten in de groep heeft, kunnen toepassingspakketten van taken gegevensoverdracht minimaliseren omdat uw toepassing alleen wordt geïmplementeerd op de knooppunten die taken uitvoeren.

Naast de taken die u definieert om een berekening op een knooppunt uit te voeren, stelt de Batch-service ook de volgende speciale taken beschikbaar:

- [Begintaak](#start-task)
- [Jobbeheertaak](#job-manager-task)
- [Jobvoorbereidingstaken en jobvrijgevingstaken](#job-preparation-and-release-tasks)
- [Taken met meerdere instanties (MPI)](#multi-instance-tasks)
- [Taakafhankelijkheden](#task-dependencies)

### Begintaak

Door een **begintaak** aan een pool te koppelen, kunt u de besturingsomgeving van de knooppunten ervan voorbereiden. U kunt bijvoorbeeld acties uitvoeren zoals het installeren van de toepassingen die de taken uitvoeren en het starten van achtergrondprocessen. Zolang deze in de pool blijft, wordt de begintaak uitgevoerd telkens wanneer een knooppunt wordt gestart, ook wanneer het knooppunt voor het eerst aan de pool wordt toegevoegd en wanneer deze opnieuw wordt opgestart of er een installatiekopie wordt hersteld.

Een groot voordeel van de begintaak is dat deze alle informatie kan bevatten die nodig is voor het configureren van een rekenknooppunt en het installeren van de toepassingen die nodig zijn voor taakuitvoering. Hierdoor kan het aantal knooppunten in een pool net zo gemakkelijk worden verhoogd als dat een nieuw doelaantal knooppunten wordt opgegeven. Batch heeft de informatie die nodig is om de nieuwe knooppunten te configureren en ze gereed te maken voor het accepteren van taken.

Net als bij elke Azure Batch-taak kunt u een lijst met **resourcebestanden** in [Azure Storage][azure_storage] opgeven, naast een uit te voeren **opdrachtregel**. Batch kopieert eerst de resourcebestanden naar het knooppunt vanuit Azure Storage en voert vervolgens de opdrachtregel uit. Voor een pool-begintaak bevat de lijst met bestanden meestal de taaktoepassing en de bijbehorende afhankelijkheden.

Deze kan echter ook referentiegegevens bevatten die worden gebruikt door alle taken die op het rekenknooppunt worden uitgevoerd. De opdrachtregel van een begintaak kan bijvoorbeeld een `robocopy`bewerking uitvoeren voor het kopiëren van toepassingsbestanden (die zijn opgegeven als resourcebestanden en zijn gedownload naar het knooppunt) vanuit de [werkmap](#files-and-directories) van de begintaak naar de [gedeelde map](#files-and-directories), en vervolgens een MSI of `setup.exe` uitvoeren.

> [AZURE.IMPORTANT] Batch ondersteunt momenteel *alleen* het opslagaccounttype **Algemeen**, zoals beschreven in stap 5 van [Een opslagaccount maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Over Azure-opslagaccounts](../storage/storage-create-storage-account.md). Uw Batch-taken (inclusief standaardtaken, begintaken, jobvoorbereidingstaken en jobvrijgevingstaken) moeten bronbestanden opgeven die zich *alleen* in opslagaccounts van het type **Algemeen** bevinden.

Het is doorgaans wenselijk dat de Batch-service wacht tot de begintaak is voltooid voordat het knooppunt als gereed wordt beschouwd om er taken aan toe te wijzen, maar dit kunt u configureren.

Als een begintaak op een rekenknooppunt mislukt, wordt de status van het knooppunt bijgewerkt om de fout aan te geven en zal het knooppunt niet beschikbaar zijn om taken toe te wijzen. Een begintaak kan mislukken als er een probleem optreedt bij het kopiëren van de bronbestanden van de begintaak uit de opslag, of als het proces dat door de opdrachtregel ervan wordt uitgevoerd een andere afsluitcode dan nul retourneert.

Als u de starttaak voor een *bestaande* groep toevoegt of bijwerkt, moet u de rekenknooppunten voor de starttaak opnieuw toepassen op de knooppunten.

### Jobbeheertaak

Doorgaans gebruikt u een **jobbeheertaak** voor het beheren en/of volgen van de jobuitvoering. Bijvoorbeeld om de taken voor een job te maken en te verzenden, om te bepalen welke extra taken er moeten worden uitgevoerd en om te bepalen wanneer het werk is voltooid. Een jobbeheertaak is echter niet beperkt tot deze activiteiten. Het is een volledig zelfstandige taak die alle acties kan uitvoeren die voor de job zijn vereist. Een jobbeheertaak kan bijvoorbeeld een bestand downloaden dat als een parameter is opgegeven, de inhoud van dat bestand analyseren en op basis van die inhoud aanvullende taken verzenden.

Een jobbeheertaak wordt vóór alle andere taken gestart. Deze biedt de volgende functies:

- Een jobbeheertaak wordt automatisch door de Batch-service verzonden als een taak wanneer de job wordt gemaakt.

- De uitvoering van een jobbeheertaak wordt gepland voor de andere taken in een job.

- Het hieraan gekoppelde knooppunt is het laatste dat moeten worden verwijderd uit een pool wanneer de pool wordt verkleind.

- De beëindiging ervan kan worden gekoppeld aan de beëindiging van alle taken in de job.

- Een jobbeheertaak krijgt de hoogste prioriteit wanneer deze opnieuw moet worden gestart. Als een niet-actief knooppunt niet beschikbaar is, kan de Batch-service een van de andere actieve taken in de pool beëindigen om ruimte te maken voor het uitvoeren van de jobbeheertaak.

- Een jobbeheertaak in de ene job heeft geen hogere prioriteit dan de taken van andere jobs. Tussen jobs worden alleen prioriteiten op jobniveau in acht genomen.

### Jobvoorbereidingstaken en jobvrijgevingstaken

Batch biedt jobvoorbereidingstaken om de job in te stellen voordat deze wordt uitgevoerd. Jobvrijgevingstaken zijn er voor onderhoud of opschoning nadat de job is uitgevoerd.

- **Jobvoorbereidingstaak**: een jobvoorbereidingstaak wordt uitgevoerd op alle rekenknooppunten die zijn gepland om taken uit te voeren, voordat een van de andere taken van de job wordt uitgevoerd. U kunt een jobvoorbereidingstaak gebruiken om bijvoorbeeld gegevens te kopiëren die door alle taken worden gedeeld, maar uniek zijn voor de job.
- **Jobvrijgevingstaak**: wanneer een job is voltooid, wordt er een jobvrijgevingstaak uitgevoerd op elk knooppunt in de pool dat ten minste één taak heeft uitgevoerd. U kunt een jobvrijgevingstaak gebruiken om bijvoorbeeld gegevens te verwijderen die door de jobvoorbereidingstaak zijn gekopieerd of om diagnostische logboekgegevens te comprimeren en te uploaden.

Met zowel jobvoorbereidingstaken als jobvrijgevingstaken kunt u een opdrachtregel opgeven die moet worden uitgevoerd wanneer de taak wordt gestart. Ze bieden functies zoals het downloaden van bestanden, uitvoering met verhoogde bevoegdheden, aangepaste omgevingsvariabelen, maximale uitvoeringsduur, aantal pogingen en bewaartijd voor bestanden.

Zie [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md)  (Jobvoorbereidings- en jobvrijgevingstaken uitvoeren op Azure Batch-rekenknooppunten) voor meer informatie over jobvoorbereidings- en jobvrijgevingstaken.

### Taak met meerdere instanties

Een [taak met meerdere instanties](batch-mpi.md) is een taak die is geconfigureerd om op meer dan één rekenknooppunt tegelijk te worden uitgevoerd. Bij taken met meerdere instanties kunt u High Performance Computing-scenario's inschakelen, waarvoor een groep rekenknooppunten samen moet worden toegewezen om één workload te verwerken (zoals Message Passing Interface (MPI)).

Zie [Taken met meerdere instanties gebruiken om Message Passing Interface (MPI)-toepassingen uit te voeren in Azure Batch](batch-mpi.md) voor gedetailleerde informatie over het uitvoeren van MPI-jobs in Batch met behulp van de Batch .NET-bibliotheek.

### Taakafhankelijkheden

Zoals de naam al aangeeft, kunt u met [taakafhankelijkheden](batch-task-dependencies.md) opgeven dat een taak afhangt van de voltooiing van andere taken voordat deze wordt uitgevoerd. Deze functie biedt ondersteuning voor situaties waarin een 'downstream'-taak gebruikmaakt van de uitvoer van een 'upstream'-taak, of wanneer een 'upstream'-taak initialisaties uitvoert die zijn vereist voor een 'downstream'-taak. Als u deze functie wilt gebruiken, moet u eerst taakafhankelijkheden inschakelen in uw Batch-job. Daarna geeft u voor elke taak die afhankelijk is van een andere (of vele andere), de taken op waarvan die taak afhankelijk is.

Bij taakafhankelijkheden kunt u scenario's zoals de volgende configureren:

* *taakB* hangt af van *taakA* (*taakB* kan pas worden uitgevoerd nadat *taakA* is voltooid).
* *taakC* is afhankelijk van *taakA* én *taakB*.
* *taakD* is afhankelijk van een bereik van taken, zoals taken *1* t/m *10*, voordat deze wordt uitgevoerd.

Bekijk [Taakafhankelijkheden in Azure Batch](batch-task-dependencies.md) en het codevoorbeeld van [TaskDependencies][github_voorbeeld_taakafhankelijkheden] in de [azure-batch-samples][github_voorbeelden] GitHub-opslagplaats voor meer informatie over deze functie.

## Omgevingsinstellingen voor taken

Elke taak die in een Batch-opdracht wordt uitgevoerd, heeft toegang tot omgevingsvariabelen die zowel worden ingesteld door de Batch-service (servicegedefinieerd, zoals beschreven in de volgende tabel) als aangepaste variabelen die u voor uw taken kunt instellen. De toepassingen en scripts die door taken op rekenknooppunten worden uitgevoerd, hebben tijdens de uitvoering toegang tot deze omgevingsvariabelen.

U kunt aangepaste omgevingsvariabelen instellen op het niveau van de taak of de job door voor deze entiteiten de eigenschap voor *omgevingsinstellingen* in te vullen. Zie bijvoorbeeld de bewerking [Add a task to a job][rest_toevoegen_taak] (Batch REST-API) of de eigenschappen [CloudTask.EnvironmentSettings][net_cloudtask_env] en [CloudJob.CommonEnvironmentSettings][net_job_env] in Batch .NET.

Uw clienttoepassing of -service kan de omgevingsvariabelen, zowel door de service gedefinieerde als aangepaste, verkrijgen met behulp van de bewerking [Get information about a task (Informatie over een taak ophalen)][rest_get_task_info] (Batch REST) of via de eigenschap [CloudTask.EnvironmentSettings][net_cloudtask_env] (Batch .NET). Processen die op een rekenknooppunt worden uitgevoerd, kunnen ook toegang krijgen tot deze en andere omgevingsvariabelen in het knooppunt, bijvoorbeeld met behulp van de vertrouwde syntaxis van `%VARIABLE_NAME%` (Windows) of `$VARIABLE_NAME` (Linux).

De volgende omgevingsvariabelen worden ingesteld door de Batch-service en zijn beschikbaar voor toegang door uw taken:

| Naam van omgevingsvariabele       | Beschrijving                                                              |
|---------------------------------|--------------------------------------------------------------------------|
| `AZ_BATCH_ACCOUNT_NAME`         | De naam van het account waartoe de taak behoort.                       |
| `AZ_BATCH_JOB_ID`               | De ID van de job waartoe de taak behoort.                             |
| `AZ_BATCH_JOB_PREP_DIR`         | Het volledige pad naar de map van de jobvoorbereidingstaak in het knooppunt.         |
| `AZ_BATCH_JOB_PREP_WORKING_DIR` | Het volledige pad naar de werkmap van de jobvoorbereidingstaak in het knooppunt. |
| `AZ_BATCH_NODE_ID`              | De ID van het knooppunt waarin de taak wordt uitgevoerd.                         |
| `AZ_BATCH_NODE_ROOT_DIR`        | Het volledige pad naar de hoofdmap in het knooppunt.                         |
| `AZ_BATCH_NODE_SHARED_DIR`      | Het volledige pad van de gedeelde map in het knooppunt.                       |
| `AZ_BATCH_NODE_STARTUP_DIR`     | Het volledige pad naar de map van de begintaak van het rekenknooppunt in het knooppunt.    |
| `AZ_BATCH_POOL_ID`              | De ID van de pool waarin de taak wordt uitgevoerd.                         |
| `AZ_BATCH_TASK_DIR`             | Het volledige pad naar de map van de taak in het knooppunt.                         |
| `AZ_BATCH_TASK_ID`              | De id van de huidige taak.                                              |
| `AZ_BATCH_TASK_WORKING_DIR`     | Het volledige pad naar de werkmap van de taak in het knooppunt.                 |

>[AZURE.IMPORTANT] Deze omgevingsvariabelen zijn alleen beschikbaar in de context van de **taakgebruiker**, dat wil zeggen: het gebruikersaccount in het knooppunt waaronder een taak wordt uitgevoerd. U ziet deze *niet* als u [extern verbinding maakt](#connecting-to-compute-nodes) met een rekenknooppunt via Remote Desktop Protocol (RDP) of Secure Shell (SSH) en de omgevingsvariabelen weergeeft. Dit komt doordat het gebruikersaccount dat voor de externe verbinding wordt gebruikt, niet hetzelfde is als het account dat door de taak wordt gebruikt.

## Bestanden en mappen

Elke taak heeft een *werkmap* waaronder nul of meer bestanden en mappen worden gemaakt. Deze werkmap kan worden gebruikt voor het opslaan van het programma dat wordt uitgevoerd door de taak, de gegevens die erdoor worden verwerkt en de uitvoer van de verwerking die wordt uitgevoerd. Alle bestanden en mappen van een taak zijn het eigendom van de taakgebruiker.

De Batch-service geeft een deel van het bestandssysteem in een knooppunt weer als de *hoofdmap*. Taken hebben toegang tot de hoofdmap door te verwijzen naar de omgevingsvariabele `AZ_BATCH_NODE_ROOT_DIR`. Zie [Omgevingsinstellingen voor taken](#environment-settings-for-tasks) voor meer informatie over het gebruik van omgevingsvariabelen.

De hoofdmap bevat de volgende mapstructuur:

![Mapstructuur rekenknooppunt][1]

- **gedeeld**: deze map biedt lees-/schrijftoegang tot *alle* taken die in een knooppunt worden uitgevoerd. Taken die in het knooppunt worden uitgevoerd, kunnen in deze map bestanden maken, lezen, bijwerken en verwijderen. Taken kunnen toegang krijgen tot deze map door te verwijzen naar de omgevingsvariabele `AZ_BATCH_NODE_SHARED_DIR`.

- **startup**: deze map wordt door een begintaak gebruikt als werkmap. Alle bestanden die door de begintaak naar het knooppunt zijn gedownload, worden hier opgeslagen. De begintaak kan in deze map bestanden maken, lezen, bijwerken en verwijderen. Taken kunnen toegang krijgen tot deze map door te verwijzen naar de omgevingsvariabele `AZ_BATCH_NODE_STARTUP_DIR`.

- **Taken**: voor elke taak die in het knooppunt wordt uitgevoerd, wordt een map gemaakt. Deze is toegankelijk door te verwijzen naar de omgevingsvariabele `AZ_BATCH_TASK_DIR`.

    Binnen elk taakmap maakt de Batch-service een werkmap (`wd`) waarvan het unieke pad wordt opgegeven door de omgevingsvariabele `AZ_BATCH_TASK_WORKING_DIR`. Deze map biedt lees-/schrijftoegang tot de taak. De taak kan in deze map bestanden maken, lezen, bijwerken en verwijderen. Deze map wordt behouden op basis van de *RetentionTime*-beperking die is opgegeven voor de taak.

    `stdout.txt` en `stderr.txt`: deze bestanden worden naar de taakmap geschreven bij het uitvoeren van de taak.

>[AZURE.IMPORTANT] Wanneer een knooppunt uit de pool wordt verwijderd, worden *alle* bestanden verwijderd die in het knooppunt zijn opgeslagen.

## Toepassingspakketten

Met de functie voor [toepassingspakketten](batch-application-packages.md) kunt u toepassingen in de rekenknooppunten in uw pools eenvoudig beheren en implementeren. U kunt eenvoudig meerdere versies van de toepassingen die door uw taken worden uitgevoerd uploaden en beheren, zoals de bijbehorende binaire bestanden en ondersteuningsbestanden. Vervolgens kunt u automatisch een of meer van deze toepassingen implementeren in de rekenknooppunten in uw pool.

U kunt toepassingspakketten opgeven op het niveau van de groep en de taak. Wanneer u toepassingspakketten voor de groep opgeeft, wordt de toepassing geïmplementeerd in elk knooppunt van de groep. Wanneer u toepassingspakketten voor de taak opgeeft, wordt de toepassing alleen geïmplementeerd in knooppunten die ten minste één van de taken van de job moeten uitvoeren, net voordat de opdrachtregel van de taak wordt uitgevoerd.

Batch regelt de samenwerking met Azure Storage om uw toepassingspakketten op te slaan en in rekenknooppunten te implementeren, zodat uw code en beheeroverhead kunnen worden vereenvoudigd.

Zie [Application deployment with Azure Batch application packages](batch-application-packages.md) (Toepassingsimplementatie met Azure Batch-toepassingspakketten) voor meer informatie over de functie voor toepassingspakketten.

>[AZURE.NOTE] Als u toepassingspakketten van een groep toevoegt aan een *bestaande* groep, moeten de rekenknooppunten opnieuw worden opgestart zodat de toepassingspakketten in de knooppunten kunnen worden geïmplementeerd.

## Levensduur van pool en rekenknooppunt

Bij het ontwerp van uw Azure Batch-oplossing moet u een ontwerpbeslissing maken over hoe en wanneer pools worden gemaakt en hoelang rekenknooppunten binnen die pools beschikbaar blijven.

Aan het ene uiteinde van het spectrum kunt u een pool maken voor elke job wanneer deze wordt verzonden en kunnen de knooppunten ervan worden verwijderd zodra taken zijn uitgevoerd. Hiermee maximaliseert u het gebruik omdat de knooppunten alleen worden toegewezen wanneer deze echt nodig zijn en worden afgesloten wanneer ze inactief zijn. Hoewel dit betekent dat de job moet wachten tot de knooppunten zijn toegewezen, is het belangrijk om te weten dat taken voor knooppunten worden gepland zodra ze afzonderlijk beschikbaar zijn, toegewezen zijn en de begintaak is voltooid. Batch wacht *niet* tot alle knooppunten in een pool beschikbaar zijn alvorens taken toe te wijzen. Hiermee zorgt u voor maximaal gebruik van alle beschikbare knooppunten.

Als, aan het andere uiteinde van het spectrum, het onmiddellijk starten van jobs de hoogste prioriteit heeft, kunt u een pool voortijdig maken en de knooppunten ervan beschikbaar stellen voordat er jobs worden verzonden. In dit scenario kunnen taken van jobs onmiddellijk worden gestart, maar blijven knooppunten mogelijk inactief terwijl wordt gewacht tot er taken worden toegewezen.

Een gecombineerde benadering wordt meestal gebruikt voor het verwerken van een variabele, maar continue workload. U kunt een pool hebben waarnaar meerdere taken worden verzonden en het aantal knooppunten omhoog of omlaag schalen volgens de jobbelasting (zie [Rekenresources vergroten/verkleinen](#scaling-compute-resources) in de volgende sectie). U kunt dit reactief doen, op basis van de huidige workload, of proactief als de workload kan worden voorspeld.

## Rekenresources vergroten/verkleinen

Door [automatisch te vergroten/verkleinen](batch-automatic-scaling.md) kunt u het aantal rekenknooppunten in een pool dynamisch laten aanpassen door de Batch-service op basis van de huidige workload en het resourcegebruik van uw rekenscenario. Zo kunt u de totale kosten van het uitvoeren van uw toepassing verlagen door alleen de benodigde resources te gebruiken en de resources die u niet nodig hebt, vrij te geven.

U schakelt automatische vergroting/verkleining in door een [formule voor automatisch vergroten/verkleinen](batch-automatic-scaling.md#automatic-scaling-formulas) te schrijven en die formule te koppelen aan een pool. De Batch-service gebruikt deze formule om het doelaantal knooppunten in de pool te bepalen voor het volgende interval voor vergroten/verkleinen (een interval dat u kunt configureren). U kunt de instellingen voor automatisch vergroten/verkleinen voor een pool opgeven wanneer u deze maakt of op een later moment voor een pool inschakelen. U kunt de instellingen voor automatisch vergroten/verkleinen ook bijwerken in een pool waarvoor vergroten/verkleinen is ingeschakeld.

Voor een bepaalde job moet u bijvoorbeeld een groot aantal taken verzenden die moeten worden uitgevoerd. U kunt aan de pool een formule voor vergroten/verkleinen toewijzen die het aantal knooppunten in de pool aanpast op basis van het huidige aantal taken in de wachtrij en de snelheid waarmee de taken in de job worden voltooid. De Batch-service evalueert periodiek de formule en past de grootte van de pool aan op basis van de workload (knooppunten toevoegen voor veel taken in de wachtrij en knooppunten verwijderen voor geen taken in de wachtrij of actieve taken) en de andere formule-instellingen.

Een formule voor vergroten/verkleinen kan op de volgende metrische gegevens worden gebaseerd:

- **Metrische gegevens voor tijd** zijn gebaseerd op statistieken die om de vijf minuten worden verzameld binnen het opgegeven aantal uren.

- **Metrische gegevens voor resources** zijn gebaseerd op CPU-gebruik, bandbreedtegebruik, geheugengebruik en het aantal knooppunten.

- **Metrische gegevens voor taken** zijn gebaseerd op de taakstatus, zoals *Actief* (in de wachtrij) *Wordt uitgevoerd* of *Voltooid*.

Wanneer met automatisch vergroten/verkleinen het aantal rekenknooppunten in een groep vermindert, moet u bedenken hoe taken moeten worden afgehandeld die worden uitgevoerd op het moment van de verkleining. Hiervoor voorziet Batch in een *optie voor knooppunttoewijzing* die u kunt opnemen in uw formules. U kunt bijvoorbeeld opgeven dat actieve taken onmiddellijk worden gestopt, onmiddellijk worden gestopt en vervolgens opnieuw in de wachtrij worden geplaatst om te worden uitgevoerd op een ander knooppunt, of moeten worden voltooid voordat het knooppunt uit de pool wordt verwijderd.

Zie [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) (Rekenknooppunten in een Azure Batch-pool automatisch vergroten/verkleinen) voor meer informatie over het automatisch vergroten/verkleinen van een toepassing.

> [AZURE.TIP] Als u het gebruik van rekenresources wilt maximaliseren, stelt u het doelaantal knooppunten aan het einde van een job in op nul, maar staat u toe dat actieve taken mogen worden voltooid.

## Beveiliging met certificaten

Certificaten gebruikt u doorgaans bij het versleutelen of ontsleutelen van gevoelige gegevens voor taken, zoals de sleutel voor een [Azure-opslagaccount][azure_storage]. Ter ondersteuning hiervan installeert u certificaten op knooppunten. Versleutelde geheimen worden via opdrachtregelparameters doorgegeven aan taken of worden ingesloten in een van de taakresources. De geïnstalleerde certificaten kunnen dan worden gebruikt om ze te ontsleutelen.

U gebruikt de bewerking[Certificaat toevoegen][rest_add_cert] (Batch REST) of de methode [CertificateOperations.CreateCertificate][net_create_cert] (Batch .NET) om een certificaat aan een Batch-account toe te voegen. Daarna kunt u het certificaat aan een nieuwe of bestaande pool koppelen. Wanneer een certificaat aan een pool is gekoppeld, wordt het door de Batch-service in elk knooppunt in de pool geïnstalleerd. De Batch-service installeert de juiste certificaten wanneer het knooppunt wordt gestart, voordat er een taak wordt gestart (met inbegrip van begintaken en jobbeheertaken).

Als u certificaten toevoegt aan een *bestaande* groep, moeten de rekenknooppunten opnieuw worden opgestart zodat de certificaten op de knooppunten kunnen worden toegepast.

## Foutafhandeling

Soms is het nodig om in uw Batch-oplossing taak- en toepassingsfouten af te handelen.

### Afhandeling van taakfouten
Taakfouten kunnen worden onderverdeeld in deze categorieën:

- **Planningsfouten**

    Als de overdracht van bestanden die voor een taak zijn opgegeven, om een bepaalde reden mislukt, wordt voor de taak een planningsfout ingesteld.

    Planningsfouten kunnen optreden doordat bestanden zijn verplaatst, doordat het opslagaccount niet langer beschikbaar is of doordat een ander probleem ertoe heeft geleid dat bestanden niet naar het knooppunt kunnen worden gekopieerd.

- **Toepassingsfouten**

    Het proces dat door de opdrachtregel van de taak wordt opgegeven, kan ook mislukken. Het proces wordt als mislukt beschouwd wanneer het proces dat door de taak wordt uitgevoerd, een andere afsluitcode dan nul retourneert (zie *Taakafsluitcodes* in de volgende sectie).

    Voor toepassingsfouten kunt u Batch configureren om automatisch een opgegeven aantal pogingen te doen om de taak opnieuw uit te voeren.

- **Beperkingsfouten**

    U kunt de beperking *maxWallClockTime* instellen om te bepalen hoelang een job of taak maximaal kan worden uitgevoerd. Deze maximale kloktijd kan handig zijn om vastgelopen taken te beëindigen.

    Wanneer de maximale hoeveelheid tijd wordt overschreden, wordt de taak als *voltooid* gemarkeerd, maar wordt de afsluitcode ingesteld op `0xC000013A` en wordt het veld *schedulingError* gemarkeerd als `{ category:"ServerError", code="TaskEnded"}`.

### Foutopsporing van toepassingsfouten

- `stderr` en `stdout`

    Bij het uitvoeren van een toepassing kan deze een diagnostische uitvoer produceren die handig is voor het oplossen van problemen. Zoals eerder is vermeld in [Bestanden en mappen](#files-and-directories), verzendt de Batch-service standaarduitvoer en standaardfoutuitvoer naar de bestanden `stdout.txt` en `stderr.txt` in de taakmap in het rekenknooppunt. U kunt de Azure Portal of een van de Batch-SDK's gebruiken om deze bestanden te downloaden. U kunt deze en andere bestanden bijvoorbeeld ophalen om problemen op te lossen met behulp van [ComputeNode.GetNodeFile][net_getfile_node] en [CloudTask.GetNodeFile][net_getfile_task] in de .NET-bibliotheek van Batch.

- **Taakafsluitcodes**

    Zoals eerder vermeld, wordt een taak door de Batch-service gemarkeerd als mislukt als het proces dat door de taak wordt uitgevoerd, een afsluitcode retourneert die niet nul is. Wanneer een taak een proces uitvoert, vult Batch de *retourcode van het proces* als afsluitcode-eigenschap van de taak in. Houd voor ogen dat de afsluitcode van een taak **niet** wordt bepaald door de Batch-service, maar wordt vastgesteld door het proces zelf of het besturingssysteem waarop het proces wordt uitgevoerd.

### Verklaring voor mislukte taken of onderbrekingen van taken

Van tijd tot tijd kunnen taken mislukken of worden onderbroken. In de taaktoepassing zelf kan een fout optreden, het knooppunt waarin de taak wordt uitgevoerd, kan opnieuw zijn opgestart of het knooppunt is mogelijk uit de pool verwijderd bij een vergroot-/verkleinbewerking (indien het beleid voor het ongedaan maken van de toewijzing van een knooppunt is ingesteld om knooppunten onmiddellijk te verwijderen zonder te wachten tot taken zijn voltooid). In alle gevallen moet de taak door Batch automatisch opnieuw in de wachtrij worden geplaatst voor uitvoering in een ander knooppunt.

Het is ook mogelijk dat een onregelmatig probleem ertoe leidt dat een taak vastloopt of dat de uitvoering ervan te lang duurt. U kunt de maximale uitvoeringstijd voor een taak instellen. Als deze wordt overschreden, wordt de taaktoepassing door Batch onderbroken.

### Verbinding maken met rekenknooppunten

U kunt extra foutopsporing en probleemoplossing uitvoeren door u op afstand aan te melden bij een rekenknooppunt. U kunt via de Azure Portal een RDP-bestand (Remote Desktop Protocol) downloaden voor Windows-knooppunten en SSH-verbindingsinformatie (Secure Shell) verkrijgen voor Linux-knooppunten. U kunt dit ook doen met behulp van de Batch-API's. Bijvoorbeeld met [Batch .NET][net_rdpfile] of [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Als u via RDP of SSH verbinding wilt maken met een knooppunt, moet u in het knooppunt eerst een gebruiker maken. Om dit te doen, kunt u de Azure Portal gebruiken, [een gebruikersaccount toevoegen aan een knooppunt][rest_gebruiker_maken] met behulp van de Batch REST-API, de methode [ComputeNode.CreateComputeNodeUser][net_gebruiker_maken] aanroepen in Batch .NET of de methode [gebruiker_toevoegen][py_gebruiker_toevoegen] aanroepen in de Batch Python-module.

### Problemen met onjuiste rekenknooppunten oplossen

In situaties waarin een aantal taken mislukken, kan uw Batch-clienttoepassing of -service de metagegevens van de mislukte taken onderzoeken om een knooppunt te identificeren dat zich niet normaal gedraagt. Elk knooppunt in een pool krijgt een unieke id en het knooppunt waarin een taak wordt uitgevoerd, is opgenomen in de metagegevens van de taak. Als u eenmaal een probleemknooppunt hebt geïdentificeerd, kunt u verschillende acties uitvoeren:

- **Het knooppunt opnieuw opstarten** ([REST][rest_reboot] | [.NET][net_reboot])

    Soms kunnen latente problemen zoals vastgelopen of gecrashte processen worden opgelost door het knooppunt opnieuw op te starten. Houd er rekening mee dat als uw pool een begintaak gebruikt of als uw job een jobvoorbereidingstaak gebruikt, deze worden uitgevoerd wanneer het knooppunt opnieuw wordt opgestart.

- **Installatiekopie van knooppunt terugzetten** ([REST][rest_reimage] | [.NET][net_reimage])

    Hiermee wordt het besturingssysteem opnieuw geïnstalleerd in het knooppunt. Net als bij het opnieuw opstarten van een knooppunt worden begintaken en jobvoorbereidingstaken opnieuw uitgevoerd nadat de installatiekopie van het knooppunt is teruggezet.

- **Het knooppunt uit de pool verwijderen** ([REST][rest_remove] | [.NET][net_remove])

    Soms is het nodig om het knooppunt volledig uit de pool te verwijderen.

- **Taakplanning in het knooppunt uitschakelen** ([REST][rest_offline] | [.NET][net_offline])

    Hierdoor wordt het knooppunt effectief offline geplaatst, zodat er geen taken meer aan worden toegewezen, maar kan het knooppunt actief en in de pool blijven. Zo kunt u verder onderzoek verrichten naar de oorzaak van de fouten zonder verlies van gegevens van de mislukte taak en zonder dat het knooppunt extra taakfouten veroorzaakt. U kunt bijvoorbeeld de taakplanning in het knooppunt uitschakelen en u vervolgens [extern aanmelden](#connecting-to-compute-nodes) om de gebeurtenislogboeken van het knooppunt te onderzoeken of een andere probleemoplossing uitvoeren. Wanneer u klaar bent met het onderzoek, kunt u het knooppunt weer online plaatsen door de taakplanning in te schakelen ([REST][rest_online] | [.NET][net_online]) of een van de andere acties uitvoeren die eerder zijn besproken.

> [AZURE.IMPORTANT] Met elk van de acties die in deze sectie zijn besproken (opnieuw opstarten, installatiekopie terugzetten, verwijderen en taakplanning uitschakelen), kunt u opgeven hoe taken die momenteel in het knooppunt worden uitgevoerd, worden afgehandeld wanneer u de actie uitvoert. Wanneer u bijvoorbeeld de taakplanning uitschakelt op een knooppunt met de clientbibliotheek Batch .NET, kunt u een enum-waarde [DisableComputeNodeSchedulingOption][net_offline_option] opgeven om aan te geven of u actieve taken wilt **beëindigen**, **opnieuw in de wachtrij wilt plaatsen** voor planning op andere knooppunten of wilt toestaan dat taken worden voltooid voordat de actie wordt uitgevoerd (**TaskCompletion**).

## Volgende stappen

- Stapsgewijs een Batch-voorbeeld-app doorlopen in [Aan de slag met de Azure Batch-bibliotheek voor .NET](batch-dotnet-get-started.md). Er is ook een [Python-versie](batch-python-tutorial.md) van de zelfstudie waarin een workload in Linux-rekenknooppunten wordt uitgevoerd.

- Het [Batch Explorer][github_batchexplorer]-voorbeeldproject voor gebruik bij het ontwikkelen van uw Batch-oplossingen downloaden en bouwen. Met behulp van Batch Explorer kunt u het volgende en nog veel meer uitvoeren:
  - Pools, jobs en taken in uw Batch-account controleren en bewerken
  - `stdout.txt`, `stderr.txt` en andere bestanden van knooppunten downloaden
  - Gebruikers maken op knooppunten en RDP-bestanden downloaden voor externe aanmelding

- Leren hoe u [pools kunt maken van Linux-rekenknooppunten](batch-linux-nodes.md).

- Een bezoek brengen aan het [Azure Batch-forum][batch_forum] op MSDN. Het forum is een goede plaats om vragen te stellen, of u nu net begint met Batch of een expert bent in het gebruik hiervan.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_voorbeelden]: https://github.com/Azure/azure-batch-samples
[github_voorbeeld_taakafhankelijkheden]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_gebruiker_maken]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_gebruiker_toevoegen]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_toevoegen_taak]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_gebruiker_maken]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_planningen]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/



<!--HONumber=ago16_HO5-->



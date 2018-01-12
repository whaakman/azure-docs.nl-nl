# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Premium-opslag voor hoge prestaties en beheerde schijven voor virtuele machines
Azure Premium-opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines (VM's) met input/output (I/O)-intensieve werkbelastingen. VM-schijven die gebruikmaken van Premium-opslag opslaan gegevens op de SSD-schijven (SSD's). Als u wilt profiteren van de snelheid en prestaties van schijven met opslagruimte premium, kunt u bestaande VM-schijven kunt migreren naar Premium-opslag.

In Azure, kunt u verschillende premium opslagschijven koppelen aan een VM. Uw toepassingen met meerdere schijven biedt maximaal 256 TB aan opslag per VM. Met de Premium-opslag, kunnen uw toepassingen 80.000 i/o-bewerkingen per seconde (IOPS) per virtuele machine en de schijfdoorvoer van een van maximaal 2000 megabytes per seconde (MB/s) per VM bereiken. Leesbewerkingen bieden u een zeer lage latenties.

Azure biedt voor Premium-opslag, de mogelijkheid om te echt lift-en-shift veeleisende bedrijfstoepassingen zoals Dynamics AX, Dynamics CRM, Exchange Server, SAP Business Suite en SharePoint-farms naar de cloud. U kunt de database van de prestatie-intensieve werkbelastingen uitvoeren in toepassingen zoals SQL Server, Oracle, MongoDB, MySQL en Redis, waarvoor consistent hoge prestaties en lage latentie.

> [!NOTE]
> Voor de beste prestaties voor uw toepassing, wordt u aangeraden een VM-schijf waarvoor hoge IOPS naar Premium-opslag te migreren. Als de schijf niet hoge IOPS vereist, kunt u kosten beperken door deze te houden in standard-Azure-opslag. Standard-opslag worden VM schijfgegevens opgeslagen op de vaste harde schijven (HDD's) in plaats van op SSD's.
> 

Azure biedt twee manieren maken premium-opslag-schijven voor virtuele machines:

* **Niet-beheerde schijven**

    De oorspronkelijke methode is niet-beheerde schijven te gebruiken. In een niet-beheerde schijf beheert u de storage-accounts die u gebruikt voor het opslaan van de virtuele harde schijf (VHD)-bestanden die met uw VM-schijven overeenkomen. VHD-bestanden worden opgeslagen als pagina-blobs in Azure storage-accounts. 

* **Beheerde schijven**

    Wanneer u de optie [Azure beheerd schijven](../articles/virtual-machines/windows/managed-disks-overview.md), Azure beheert de storage-accounts die u voor uw VM-schijven gebruikt. U geeft het schijftype (Premium of standaard) en de grootte van de schijf die u nodig hebt. Azure maakt en beheert de schijf voor u. U hoeft niet te hoeven maken over het plaatsen van de schijven in meerdere opslagaccounts om ervoor te zorgen dat u altijd binnen de schaalbaarheidslimieten voor voor uw storage-accounts. Azure verwerkt die voor u.

Het is raadzaam dat u beheerde schijven kiest, om te profiteren van veel functies.

Aan de slag met Premium-opslag [uw gratis Azure-account maken](https://azure.microsoft.com/pricing/free-trial/). 

Zie voor meer informatie over het migreren van uw bestaande virtuele machines naar Premium-opslag [een Windows VM van niet-beheerde schijven converteren naar beheerde schijven](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) of [een Linux-VM van niet-beheerde schijven converteren naar beheerde schijven](../articles/virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Premium-opslag is beschikbaar in de meeste regio's. Voor een lijst met beschikbare regio's, raadpleegt u de rij voor **schijfopslag** in [Azure producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/#services).
> 

## <a name="features"></a>Functies

Hier volgen enkele van de functies van Premium-opslag:

* **Premium-opslag-schijven**

    Premium-opslag biedt ondersteuning voor VM-schijven die kunnen worden bijgevoegd aan een specifieke virtuele machines voor grootte-serie. Premium-opslag ondersteunt DS-serie, DSv2-serie GS-serie, Ls-serie en virtuele machines Fs-serie. U hebt de mogelijkheid van zeven schijfgrootten: P4 (32GB), P6 (64GB), P10 (128GB), P20 (512GB), P30 (1024GB), p 40 (2048GB), P50 (4095GB). P4 en P6 schijfgrootten worden alleen nog ondersteund voor schijven die worden beheerd. De grootte van elke schijf heeft zijn eigen prestatiespecificaties. Afhankelijk van uw toepassing, kunt u een of meer schijven aan uw virtuele machine te koppelen. De specificaties in nader worden beschreven [Premium-opslag schaalbaarheids- en prestatiedoelen](#scalability-and-performance-targets).

* **Premium-pagina-blobs**

    Premium-opslag biedt ondersteuning voor pagina-blobs. Pagina-blobs gebruikt voor het opslaan van permanente, niet-beheerde schijven voor virtuele machines in Premium-opslag. Premium-opslag komt niet in tegenstelling tot standaard Azure Storage ondersteunt blok-blobs, toevoeg-blobs, bestanden, tabellen of wachtrijen. Premium-pagina-blobs ondersteunt zes groottes van P10 P50 en P60 (8191GiB). P60 Premium-pagina-blob wordt niet ondersteund als VM-schijven worden gekoppeld. 

    Een object dat wordt geplaatst in een premium storage-account is een pagina-blob. De paginablob uitgelijnd op een van de ondersteunde ingerichte formaten. Daarom een premium storage-account is niet bedoeld voor gebruikt voor het opslaan van kleine blobs.

* **Premium-opslagaccount**

    Maak een premium storage-account voor niet-beheerde schijven eerst u Premium-opslag. In de [Azure-portal](https://portal.azure.com), zodat een premium-opslagaccount maakt, kiest u de **Premium** prestatielaag. Selecteer de **lokaal redundante opslag (LRS)** replicatie-optie. U kunt een premium storage-account ook maken door het type in te stellen op **Premium_LRS** in een van de volgende locaties:
    * [REST API voor Storage](https://docs.microsoft.com/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (versie 2014-02-14 of een latere versie)
    * [REST API van Opslagservicebeheer](http://msdn.microsoft.com/library/azure/ee460799.aspx) (versie 2014-10-01 of een latere versie voor Azure klassieke implementaties)
    * [Azure Storage Resource Provider REST-API](https://docs.microsoft.com/rest/api/storagerp) (voor implementaties van Azure Resource Manager)
    * [Azure PowerShell](/powershell/azureps-cmdlets-docs.md) (versie 0.8.10 of een latere versie)

    Zie voor meer informatie over limieten voor opslagaccounts premium, [Premium-opslag schaalbaarheids- en prestatiedoelen](#premium-storage-scalability-and-performance-targets).

* **Lokaal redundant Premium-opslag**

    Premium-opslagaccount ondersteunt alleen lokaal redundante opslag als de replicatie-optie. Lokaal redundante opslag houdt drie kopieën van de gegevens in één regio. Voor regionale noodherstel, u moet back-up in een andere regio uw VM-schijven met behulp van [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md). U moet ook een geografisch redundante opslag (GRS)-account gebruiken als de back-upkluis. 

    Azure maakt gebruik van uw storage-account als een container voor uw niet-beheerde schijven. Wanneer u een Azure Active Directory-serie, DSv2-serie GS-serie, maakt of Fs-serie-VM met niet-beheerde schijven, en u selecteert een premium storage-account, het besturingssysteem en gegevensschijven worden opgeslagen in dit opslagaccount.

## <a name="supported-vms"></a>Ondersteunde virtuele machines
Premium-opslag ondersteunt DS-serie, DSv2-serie GS-serie, Ls-serie Fs-serie en B-serie virtuele machines. Met deze VM-typen kunt u schijven standard en premium storage. U niet premium-opslag-schijven gebruiken met VM-reeks die geen Premium-opslag-compatibel.

Zie voor meer informatie over de VM-typen en groottes in Azure voor Windows [Windows VM-grootten](../articles/virtual-machines/windows/sizes.md). Zie voor meer informatie over de VM-typen en groottes in Azure voor Linux [Linux VM-grootten](../articles/virtual-machines/linux/sizes.md).

Dit zijn enkele van de functies van de DS-serie, DSv2-serie GS-serie, Ls-serie en Fs-serie virtuele machines:

* **Cloudservice**

    U kunt DS-serie virtuele machines toevoegen aan een cloudservice met alleen virtuele machines de DS-serie. DS-serie virtuele machines niet aan een bestaande cloudservice met elk type dan DS-serie virtuele machines toevoegen. U kunt uw bestaande VHD's migreren naar een nieuwe cloudservice die alleen DS-serie virtuele machines wordt uitgevoerd. Als u wilt gebruiken hetzelfde virtuele IP-adres voor de nieuwe cloudservice die als host fungeert voor uw virtuele machines van DS-serie gebruik [gereserveerde IP-adressen](../articles/virtual-network/virtual-networks-instance-level-public-ip.md). Virtuele machines GS-serie kunnen worden toegevoegd aan een bestaande cloudservice met alleen GS-serie virtuele machines.

* **Schijf van besturingssysteem**

    U kunt uw Premium-opslag-VM instellen om een premium of een standaardbesturingssysteem worden uitgevoerd-schijf te gebruiken. Voor de beste ervaring wordt u aangeraden de schijf van een besturingssysteem op basis van een Premium-opslag.

* **Gegevensschijven**

    U kunt premium en standard schijven in de dezelfde VM voor Premium-opslag gebruiken. U kunt een virtuele machine inrichten en meerdere permanente gegevensschijven koppelen aan de virtuele machine met Premium-opslag. Indien nodig, om te verhogen van de capaciteit en prestaties van het volume, kunt u stripe over uw schijven.

    > [!NOTE]
    > Als u premium-opslag-gegevensschijven stripe met behulp van [opslagruimten](http://technet.microsoft.com/library/hh831739.aspx), opslagruimten instellen met 1 kolom voor elke schijf die u gebruikt. Prestaties van de striped volumes mogelijk anders lager is dan verwacht vanwege ongelijke verdeling van het verkeer op de schijven. Standaard wordt in Serverbeheer kunt u kolommen voor maximaal 8 schijven instellen. Als u meer dan 8 schijven hebt, kunt u PowerShell gebruiken om het volume te maken. Het aantal kolommen handmatig opgeven. De Serverbeheer-UI blijft anders 8 kolommen te gebruiken, zelfs als u meer schijven voor hebt. Bijvoorbeeld, als er 32 schijven in een stripeset één, 32 kolommen opgeven. Het aantal kolommen de virtuele schijf gebruikt opgeven de [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell-cmdlet gebruikt de *NumberOfColumns* parameter. Zie voor meer informatie [overzicht van opslagruimten](http://technet.microsoft.com/library/hh831739.aspx) en [Storage Spaces Veelgestelde vragen over](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Cache**

    Virtuele machines in de reeks grootte die ondersteuning bieden voor Premium-opslag hebben een unieke cachebewerkingen mogelijkheid voor een hoge mate van doorvoer en latentie. De cachebewerkingen capaciteit overschrijdt onderliggende schijfprestaties voor premium-opslag. U kunt instellen dat de schijf cachebeleid op schijven met premium-opslag naar **ReadOnly**, **ReadWrite**, of **geen**. De standaard schijf cachebeleid **ReadOnly** voor alle schijven voor premium-gegevens, en **ReadWrite** voor besturingssysteem schijven. Gebruik de juiste cache-instelling voor optimale prestaties voor uw toepassing. Bijvoorbeeld voor lezen zware of alleen-lezen gegevensschijven zoals SQL Server-gegevensbestanden, stelt u het beleid van de schijfcache **ReadOnly**. Voor schrijven zware of alleen-schrijven gegevensschijven, zoals SQL Server-logboekbestanden, stelt het beleid van de schijfcache **geen**. Zie voor meer informatie over het optimaliseren van uw ontwerp met Premium-opslag, [ontwerpen voor prestaties voor Premium-opslag](../articles/virtual-machines/windows/premium-storage-performance.md).

* **Analytische gegevens**

    Voor het VM-prestaties analyseren met behulp van de schijven in Premium-opslag, schakelt u VM diagnostische gegevens in de [Azure-portal](https://portal.azure.com). Zie voor meer informatie [Azure VM bewaking met de extensie voor diagnostische gegevens van Azure](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    Overzicht van de prestaties van de schijf op basis van een besturingssysteem gebruiken om opties, zoals [Windows Prestatiemeter](https://technet.microsoft.com/library/cc749249.aspx) voor VM's van Windows en de [iostat](http://linux.die.net/man/1/iostat) opdracht voor virtuele Linux-machines.

* **VM-schaallimieten en prestaties**

    Elke Premium-opslag ondersteund VM-grootte heeft schaallimieten en prestatiespecificaties voor IOPS, bandbreedte en het aantal schijven dat per virtuele machine kan worden gekoppeld. Wanneer u premium-opslag-schijven met virtuele machines gebruikt, zorg ervoor dat er voldoende IOPS en bandbreedte op de virtuele machine op het station schijf verkeer.

    Een VM STANDARD_DS1 heeft bijvoorbeeld een toegewezen bandbreedte van 32 MB/s voor premium schijf opslagverkeer. Een schijf P10 premium-opslag kan bieden een bandbreedte van 100 MB/s. Als een schijf P10 premium-opslag is gekoppeld aan deze virtuele machine, kan deze alleen maximaal 32 MB/s gaat. Niet kiezen voor de maximale 100 MB/s die de schijf P10 kan bieden.

    De grootste virtuele machine in de DS-serie is momenteel de Standard_DS15_v2. De Standard_DS15_v2 kan maximaal 960 MB/s over alle schijven bieden. De grootste virtuele machine in de GS-serie is de Standard_GS5. De Standard_GS5 kan maximaal 2.000 MB/s over alle schijven bieden.

    Houd er rekening mee dat deze limieten alleen beschikbaar voor schijf verkeer zijn. Deze limieten bevatten geen treffers in cache en netwerkverkeer. Een afzonderlijke bandbreedte is beschikbaar voor VM-netwerkverkeer. Bandbreedte voor netwerkverkeer wijkt af van de toegewezen bandbreedte die wordt gebruikt door de premium-opslag-schijven.

    Zie voor de meest actuele informatie over maximale IOPS en doorvoerlimieten (bandbreedte) voor virtuele machines Premium-opslag ondersteund [Windows VM-grootten](../articles/virtual-machines/windows/sizes.md) of [Linux VM-grootten](../articles/virtual-machines/linux/sizes.md).

    Zie de tabel in de volgende sectie voor meer informatie over de premium-opslag-schijven en de IOPS en doorvoerlimieten ervan.

## <a name="scalability-and-performance-targets"></a>Schaalbaarheids- en prestatiedoelen
In deze sectie beschrijven we de schaalbaarheids- en prestatiedoelen in overweging moet nemen wanneer u Premium-opslag gebruikt.

Premium-opslagaccounts hebben de volgende schaalbaarheidsdoelen:

| Capaciteit van de totale account | Totale bandbreedte voor een account met lokaal redundante opslag |
| --- | --- | 
| Schijf capaciteit: 35 TB <br>Momentopname maken van capaciteit: 10 TB | Up 50 gigabits per seconde voor binnenkomend verkeer<sup>1</sup> + uitgaande<sup>2</sup> |

<sup>1</sup> alle gegevens (aanvragen) die worden verzonden naar een opslagaccount

<sup>2</sup> alle gegevens (antwoorden) die afkomstig zijn van een opslagaccount

Zie voor meer informatie [Azure Storage schaalbaarheids- en prestatiedoelen](../articles/storage/common/storage-scalability-targets.md).

Als u premium storage-accounts voor niet-beheerde schijven gebruikt en uw toepassing de schaalbaarheidsdoelen van een enkele storage-account overschrijdt, is het raadzaam om te migreren naar beheerde schijven. Als u niet wilt migreren naar beheerde schijven, moet u uw toepassing te gebruiken van meerdere storage-accounts maken. Partitioneren vervolgens uw gegevens over de storage-accounts. Bijvoorbeeld, als u wilt koppelen 51 TB schijven over meerdere virtuele machines, verdeeld over deze twee storage-accounts. 35 TB is de limiet voor een enkele premium storage-account. Zorg ervoor dat een enkele premium storage-account nooit meer dan 35 TB aan ingerichte schijven heeft.

### <a name="premium-storage-disk-limits"></a>Premium-opslaglimieten schijf
Wanneer u de schijf voor een premium-opslag inricht, bepaalt de grootte van de schijf de maximale IOPS en doorvoerlimieten (bandbreedte). Azure biedt zeven schijftypen premium storage: P4 (beheerd schijven alleen), P6 (beheerd schijven alleen), P10, P20 P30, p 40 en P50. Elk type opslagschijf premium heeft bepaalde limieten voor IOPS en doorvoerlimieten. Limieten voor de schijftypen worden in de volgende tabel beschreven:

| Premium-schijven Type  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 32 GB| 64 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde | 

> [!NOTE]
> Zorg ervoor dat voldoende bandbreedte beschikbaar is op de virtuele machine op station schijf verkeer, zoals beschreven in [VMs Premium-opslag ondersteund](#premium-storage-supported-vms). Anders wordt is uw schijfdoorvoer en IOP's beperkt tot het verlagen van waarden. Maximale doorvoer en IOP's zijn gebaseerd op de VM-limieten, niet op de schijf limieten beschreven in de voorgaande tabel.  
> 
> 

Hier volgen een aantal belangrijke zaken weten over de schaalbaarheids- en prestatiedoelen Premium-opslag:

* **Ingerichte capaciteit en prestaties**

    Wanneer u een premium-opslagschijf, in tegenstelling tot standaard opslag inrichten kunt u de capaciteit, IOPS en doorvoer van die schijf zijn gegarandeerd. Als u een schijf P50 maakt, richt Azure bijvoorbeeld 4095 GB opslagcapaciteit 7.500 IOPS en 250 MB/s doorvoer voor die schijf. Uw toepassing kunt gebruiken of een deel van de capaciteit en prestaties.

* **Grootte van de schijf**

    De schijfgrootte (naar boven afronden) Azure toegewezen aan de dichtstbijzijnde premium storage schijf wordt gebruikt, zoals opgegeven in de tabel in de vorige sectie. Bijvoorbeeld, is een schijfgrootte van 100 GB geclassificeerd als een optie P10. Maximaal 500 IOP's, met maximaal 100 MB/s doorvoer kan uitvoeren. Op deze manier een schijf met een grootte van die 400 GB is geclassificeerd als een P20. Maximaal 2300 IOP's, met 150 MB/s doorvoer kan uitvoeren.
    
    > [!NOTE]
    > U kunt gemakkelijk de grootte van bestaande schijven verhogen. U wilt bijvoorbeeld Verhoog de grootte van een schijf 30 GB op 128 GB of zelfs op 1 TB. Of u kunt uw schijf P20 niet converteren naar een schijf P30 omdat u meer capaciteit of meer IOPS en doorvoerlimieten moet. 
    > 
 
* **I/o-grootte**

    De grootte van een i/o is 256 KB. De gegevens worden overgebracht is minder dan 256 KB, worden beschouwd als 1 i/o-eenheid. I/o-grotere worden geteld als meerdere i/o's van de grootte van 256 KB. Bijvoorbeeld, worden 1100 KB i/o geteld als 5 i/o-eenheden.

* **Doorvoer**

    De maximale doorvoer omvat schrijfbewerkingen naar de schijf en het omvat leesbewerkingen schijf die uit de cache worden niet behandeld. Een schijf P10 heeft bijvoorbeeld 100 MB/s doorvoer per schijf. Enkele voorbeelden van geldige doorvoer voor een schijf P10 worden weergegeven in de volgende tabel:

    | Maximale doorvoer per P10 schijf | Niet-cache leest van de schijf | Niet-cache schrijfbewerkingen naar de schijf |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/s | 40 MB/s |

* **Treffers in cache**

    Treffers in cache zijn niet beperkt door de toegewezen IOPS of doorvoer van de schijf. Bijvoorbeeld, wanneer u een gegevensschijf met gebruikt een **ReadOnly** cache-instelling op een virtuele machine die wordt ondersteund door de Premium-opslag, leest die uit de cache worden behandeld vallen niet onder de IOPS en doorvoerlimieten caps van de schijf. Als de werkbelasting van een schijf hoofdzakelijk is leest, kunt u zeer hoge doorvoersnelheid krijgen. De cache is onderworpen aan afzonderlijke IOPS en doorvoerlimieten op de virtuele machine niveau, op basis van de VM-grootte. DS-serie VMs hebben ongeveer 4000 IOPS en 33 MB/s doorvoer per core voor cache en lokale SSD-i/o's. GS-serie VM's hebben een limiet van 5000 IOP's en 50 MB/s doorvoer per core voor cache en lokale SSD-i/o's. 

## <a name="throttling"></a>Beperking
Beperking optreden, als uw toepassing IOPS of doorvoer groter is dan de toegewezen limieten voor de schijf voor een premium-opslag. Beperking ook optreden als het verkeer van de totale schijfruimte op alle schijven op de virtuele machine groter is dan de schijf Bandbreedtelimiet beschikbaar voor de virtuele machine. Om te voorkomen beperking, is het raadzaam dat u het aantal openstaande i/o-aanvragen voor de schijf beperken. Gebruik een beperken op basis van schaalbaarheids- en prestatiedoelen voor de schijf die u hebt ingericht, en de schijf bandbreedte die beschikbaar zijn voor de virtuele machine.  

Uw toepassing kunt de laagste latentie bereiken wanneer deze is ontworpen om te voorkomen dat beperking. Echter, als het aantal openstaande i/o-aanvragen voor de schijf te klein is, uw toepassing kan niet profiteren van de maximale IOPS en doorvoerniveaus die beschikbaar voor de schijf zijn.

De volgende voorbeelden laten zien hoe bandbreedteregeling niveaus berekenen. Alle berekeningen zijn gebaseerd op een i/o-grootte van 256 KB.

### <a name="example-1"></a>Voorbeeld 1
Uw toepassing is 495 i/o-eenheden met een grootte van 16 KB in één seconde op een schijf P10 verwerkt. De i/o-eenheden worden beschouwd als 495 IOPS. Als u een 2 MB i/o's in dezelfde tweede probeert, is het totaal van i/o-eenheden gelijk is aan 495 + 8 IOPS. Dit komt doordat 2 MB i/o = 2048 KB / 256 KB = 8 i/o-eenheden, wanneer de grootte van de i/o-eenheid 256 KB is. Omdat de som van 495 + 8 bevat meer dan 500 IOP's voor de schijf, plaats beperking.

### <a name="example-2"></a>Voorbeeld 2
Uw toepassing is 400 i/o-eenheden met een grootte van 256 KB op een schijf P10 verwerkt. De totale bandbreedte is (400 &#215; 256) * 1024 KB = 100 MB/s. Een schijf P10 heeft een doorvoer limiet van 100 MB/s. Als uw toepassing probeert te meer i/o-bewerkingen uitvoeren in die tweede, is deze beperkt omdat deze groter is dan de limiet is toegewezen.

### <a name="example-3"></a>Voorbeeld 3
U hebt een VM DS4 met twee P30 schijven die zijn gekoppeld. Elke schijf P30 is geschikt 200 MB/s-doorvoer. Een VM DS4 heeft echter een capaciteit van de totale schijfruimte netwerkbandbreedte van 256 MB/s. U kan niet beide gekoppelde schijven aan de maximale doorvoer station op deze VM DS4 op hetzelfde moment. U lost dit op, kunt u verkeer van 200 MB/s op één schijf en 56 MB/s op de andere schijf tolereren. Als de som van uw verkeer schijf via 256 MB/s gaat, wordt de schijf verkeer beperkt.

> [!NOTE]
> Als uw schijf verkeer voornamelijk uit kleine i/o-grootten bestaat, zal uw toepassing waarschijnlijk de limiet IOPS bereikt voordat de limiet voor de doorvoer. Echter, als het verkeer van de schijf voornamelijk uit grote i/o-grootten bestaat, uw toepassing waarschijnlijk wordt bereikt die is de doorvoer eerst in plaats van de maximale IOPS. U kunt uw toepassing IOPS en doorvoercapaciteit met behulp van de optimale grootte voor i/o-maximaliseren. U kunt ook het aantal openstaande i/o-aanvragen voor een schijf beperken.
> 

Zie voor meer informatie over ontwerpen voor hoge prestaties met behulp van Premium-opslag, [ontwerpen voor prestaties voor Premium-opslag](../articles/virtual-machines/windows/premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Momentopnamen en Blob kopiëren

Het VHD-bestand is met de Storage-service een pagina-blob. U kunt momentopnamen van pagina-blobs en kopieer deze naar een andere locatie, zoals naar een ander opslagaccount.

### <a name="unmanaged-disks"></a>Niet-beheerde schijven

Maak [incrementele momentopnamen](../articles/virtual-machines/linux/incremental-snapshots.md) voor niet-beheerde premium schijven dezelfde manier als u momentopnamen met standard-opslag gebruiken. Premium-opslag ondersteunt alleen lokaal redundante opslag als de replicatie-optie. U wordt aangeraden momentopnamen maken en vervolgens de momentopnamen te kopiëren naar een geografisch redundante standaard opslagaccount. Zie voor meer informatie [Azure redundantie opslagopties](../articles/storage/common/storage-redundancy.md).

Als een schijf aan een virtuele machine is gekoppeld, worden bepaalde API-bewerkingen op de schijf niet toegestaan. Bijvoorbeeld, u kunt niet uitvoeren een [Blob kopiëren](/rest/api/storageservices/Copy-Blob) bewerking op de blob als de schijf is gekoppeld aan een virtuele machine. In plaats daarvan eerst een momentopname maken van blob met behulp van de [momentopname Blob](/rest/api/storageservices/Snapshot-Blob) REST-API. Voer vervolgens de [Blob kopiëren](/rest/api/storageservices/Copy-Blob) van de momentopname naar de gekoppelde schijf kopiëren. U kunt ook Ontkoppel de schijf en vervolgens alle noodzakelijke bewerkingen uitvoeren.

De volgende beperkingen gelden voor premium-opslag-blob momentopnamen:

| Limiet voor Premium-opslag | Waarde |
| --- | --- |
| Maximum aantal momentopnamen per blob | 100 |
| Account opslagcapaciteit voor momentopnamen<br>(Gegevens alleen momentopnamen bevat. Bevat geen gegevens in basis blob.) | 10 TB |
| Minimale tijd tussen opeenvolgende momentopnamen | 10 minuten |

Om te blijven geografisch redundante exemplaren van de momentopnamen, kunt u momentopnamen van een premium storage-account aan een account standaard geografisch redundante opslag met kopiëren met behulp van AzCopy of Blob kopiëren. Zie voor meer informatie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](../articles/storage/common/storage-use-azcopy.md) en [Blob kopiëren](/rest/api/storageservices/Copy-Blob).

Zie voor gedetailleerde informatie over het uitvoeren van de REST-bewerkingen op de pagina-blobs in een premium-opslagaccount [servicebewerkingen met Azure Premium Storage-Blob](http://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Managed Disks

Een momentopname voor een beheerde schijf is een alleen-lezen kopie van de beheerde schijf. De momentopname wordt opgeslagen als een standard-beheerde schijven. Op dit moment [incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md) worden niet ondersteund voor beheerde schijven. Zie voor meer informatie over het maken van een momentopname voor een beheerde schijf, [een kopie maken van een VHD die is opgeslagen als een Azure beheerd schijf met beheerde momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md) of [een kopie maken van een VHD die is opgeslagen als een Azure beheerd schijf met behulp van beheerd momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md).

Als een beheerde schijf aan een virtuele machine is gekoppeld, worden bepaalde API-bewerkingen op de schijf niet toegestaan. U kunt bijvoorbeeld een shared access signature (SAS) naar een kopieerbewerking niet uitvoeren terwijl de schijf is gekoppeld aan een virtuele machine niet genereren. In plaats daarvan maakt eerst een momentopname van de schijf en voer de kopie van de momentopname. U kunt ook Ontkoppel de schijf en vervolgens een SAS om uit te voeren de kopieerbewerking te genereren.


## <a name="premium-storage-for-linux-vms"></a>Premium-opslag voor virtuele Linux-machines
Gebruik de volgende informatie kan u helpen bij het instellen van uw virtuele Linux-machines in Premium-opslag:

Als u schaalbaarheidsdoelen in Premium-opslag voor alle schijven van premium-opslag met cache ingesteld op **ReadOnly** of **geen**, moet u "barrières" uitschakelen wanneer u het bestandssysteem koppelt. U hoeft niet barrières in dit scenario omdat schrijfbewerkingen naar de premium-opslag-schijven duurzame voor deze cache-instellingen zijn. Wanneer de schrijfaanvraag met succes is voltooid, is in het permanente archief gegevens geschreven. Als u wilt uitschakelen 'barrières', een van de volgende methoden te gebruiken. Kies voor het bestandssysteem:
  
* Voor **reiserFS**, zodat barrières uitschakelen, gebruikt de `barrier=none` optie koppelen. (Gebruik zodat barrières `barrier=flush`.)
* Voor **ext3/ext4**, zodat barrières uitschakelen, gebruikt de `barrier=0` optie koppelen. (Gebruik zodat barrières `barrier=1`.)
* Voor **XFS**, zodat barrières uitschakelen, gebruikt de `nobarrier` optie koppelen. (Gebruik zodat barrières `barrier`.)
* Voor premium-opslag schijven met de cache ingesteld op **ReadWrite**, barrières voor schrijven duurzaamheid inschakelen.
* Voor volumelabels om vast te leggen nadat de virtuele machine opnieuw is opgestart, moet u /etc/fstab bijwerken met de referenties (UUID) universally unique identifier van de schijven. Zie voor meer informatie [een beheerde schijf toevoegen aan een Linux-VM](../articles/virtual-machines/linux/add-disk.md).

De volgende Linux-distributies zijn gevalideerd voor Azure Premium-opslag. Voor betere prestaties en stabiliteit met Premium-opslag, is het raadzaam dat u uw virtuele machines upgraden naar een van deze versies, ten minste (of naar een nieuwere versie). Sommige van de versies van vereist de meest recente Linux Integration Services (LIS), v4.0, voor Azure. Als u wilt downloaden en installeren van een distributiepunt, volgt u de koppeling wordt weergegeven in de volgende tabel. We toevoegen installatiekopieën aan de lijst als we validatie voltooid. Houd er rekening mee dat onze validaties weergegeven dat de prestaties wisselend voor elke installatiekopie. Prestaties zijn afhankelijk van werkbelasting kenmerken en de instellingen van uw installatiekopie. Andere installatiekopieën zijn voor verschillende soorten werkbelastingen afgestemd.

| Distributie | Versie | Ondersteunde kernel | Details |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-AMD64-server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-AMD64-server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SUSE-sles-12-prioriteit-v20150213 <br> SUSE-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | Virtuele CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 vereist](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Zie de opmerking in het volgende gedeelte* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [Aanbevolen LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Zie de opmerking in het volgende gedeelte* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 of RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 of RHCK met[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 of RHCK met[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>LIS stuurprogramma's voor OpenLogic CentOS

Als u OpenLogic CentOS virtuele machines uitvoert, voert u de volgende opdracht om de meest recente stuurprogramma's installeren:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

Voor het activeren van de nieuwe stuurprogramma's moet de computer opnieuw opstarten.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u Premium-opslag gebruikt, zijn de volgende factureringsvoorwaarden van toepassing:

* **Premium-opslag schijf en de blob-grootte**

    Facturering voor een schijf voor premium-opslag of blob is afhankelijk van de ingerichte grootte van de schijf of een blob. De grootte van de ingerichte (naar boven afronden) Azure toegewezen aan de dichtstbijzijnde premium storage schijf wordt gebruikt. Voor meer informatie, Zie de tabel in [Premium-opslag schaalbaarheids- en prestatiedoelen](#premium-storage-scalability-and-performance-targets). Elke schijf toegewezen aan een ondersteunde ingerichte schijfgrootte en dienovereenkomstig wordt gefactureerd. Facturering voor ingerichte schijven wordt elk uur naar rato met behulp van de maandelijks tarief voor de Premium-opslag-aanbieding. Bijvoorbeeld, als u een schijf P10 ingericht en deze na 20 uur verwijderd, wordt u gefactureerd voor de aanbieding P10 naar rato 20 uur. Dit is ongeacht de hoeveelheid gegevens geschreven naar de schijf of de IOPS en doorvoerlimieten die worden gebruikt.

* **Premium niet-beheerde schijven momentopnamen**

    Momentopnamen op zonder begeleiding premium-schijven wordt gefactureerd voor de extra capaciteit die wordt gebruikt door de momentopnamen. Zie voor meer informatie over momentopnamen [een momentopname maken van een blob](/rest/api/storageservices/Snapshot-Blob).

* **Premium-beheerde schijven momentopnamen**

    Een momentopname van een beheerde schijf is een alleen-lezen kopie van de schijf. De schijf wordt opgeslagen als een standard-beheerde schijven. Een momentopname kosten hetzelfde als een standaard schijf beheerde. De kosten van de momentopname is bijvoorbeeld gelijk aan een standaard beheerde schijf van 128 GB als u een momentopname van een beheerde schijf van 128 GB premium.  

* **Uitgaande gegevensoverdracht**

    [Uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) (gegevens uit de Azure-datacenters gaan) worden gefactureerd voor bandbreedtegebruik.

Zie voor gedetailleerde informatie over prijzen voor Premium-opslag, Premium-opslag ondersteund VM's en beheerde schijven, deze artikelen:

* [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Beheerde schijven prijzen](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Ondersteuning van Azure back-up 

Voor regionale noodherstel, u moet back-up in een andere regio uw VM-schijven met behulp van [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) en een GRS-opslagaccount als een back-upkluis.

Maakt een back-uptaak met back-ups op basis van tijd, eenvoudig herstel van de virtuele machine en bewaarbeleid voor back-up, gebruikt u Azure Backup. U kunt back-up beide gebruiken met niet-beheerde en beheerde schijven. Zie voor meer informatie [Azure Backup voor VM's met niet-beheerde schijven](../articles/backup/backup-azure-vms-first-look-arm.md) en [Azure Backup voor virtuele machines met beheerde schijven](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Premium-opslag.

### <a name="design-and-implement-with-premium-storage"></a>Ontwerpen en implementeren met Premium-opslag
* [Ontwerpen voor prestaties voor Premium-opslag](../articles/virtual-machines/windows/premium-storage-performance.md)
* [BLOB storage-bewerkingen met Premium-opslag](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>Gebruiksaanwijzing
* [Migreren naar Azure Premium-opslag](../articles/storage/common/storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Blogberichten
* [Algemeen beschikbaar Azure Premium-opslag](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [Aankondiging van de GS-serie: ondersteuning voor Premium-opslag toe te voegen aan de grootste virtuele machines in de openbare cloud](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)

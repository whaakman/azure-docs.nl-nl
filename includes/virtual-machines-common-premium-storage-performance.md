# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Storage: Ontwerp voor hoge prestaties

In dit artikel bevat richtlijnen voor het bouwen van toepassingen met hoge prestaties met behulp van Azure Premium Storage. U kunt de instructies in dit document in combinatie met aanbevolen procedures voor prestaties van toepassing op de technologieën die worden gebruikt door uw toepassing gebruiken. Ter illustratie van de richtlijnen, hebben we met SQL Server op Premium-opslag als voorbeeld in dit document gebruikt.

Terwijl de prestaties van scenario's voor de opslaglaag in dit artikel behandelen we, moet u het niveau van de toepassing optimaliseren. Bijvoorbeeld, als u een SharePoint-Farm op Azure Premium Storage host, kunt u de SQL Server-voorbeelden uit dit artikel om te optimaliseren van de database-server. Bovendien het Optimaliseer de webserver en de toepassingsserver op de meeste prestaties van de SharePoint-Farm.

Dit artikel voor het antwoord volgen Veelgestelde vragen over het optimaliseren van prestaties van toepassingen op Azure Premium Storage

* Hoe kunt u voor het meten van prestaties van uw toepassing?  
* Waarom ziet u niet verwacht hoge prestaties?  
* Welke factoren van invloed zijn op de prestaties van uw toepassing op Premium Storage?  
* Hoe deze factoren van invloed op prestaties van uw toepassing op Premium Storage?  
* Hoe kunt u optimaliseren voor IOPS, bandbreedte en wachttijden?  

We hebben deze richtlijnen die specifiek voor Premium-opslag omdat workloads die worden uitgevoerd op de Premium-opslag zeer gevoelige prestaties zijn. Indien van toepassing zijn zijn er voorbeelden beschikbaar. U kunt sommige van deze richtlijnen ook toepassen op toepassingen die worden uitgevoerd op virtuele IaaS-machines met Standard Storage-schijven.

Voordat u begint, als u niet bekend bent met Premium Storage, lees eerst de [Premium Storage: hoogwaardige opslag voor Azure Virtual Machine-werkbelasting](../articles/virtual-machines/windows/premium-storage.md) en [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md)artikelen.

## <a name="application-performance-indicators"></a>Toepassing prestatie-indicatoren
We beoordelen of een toepassing wordt uitgevoerd met behulp van prestatie-indicatoren, zoals goed of niet, hoe snel een toepassing een gebruikersaanvraag, hoeveel gegevens een aanvraag wordt verwerkt per aanvraag wordt verwerkt, het aantal aanvragen is een toepassing met de verwerking van een specifieke periode, hoe lang die een gebruiker moet worden gewacht voordat een antwoord krijgt nadat u hebt de aanvraag heeft. De voorwaarden van de technische voor deze prestatie-indicatoren zijn, IOPS, doorvoer of bandbreedte en latentie.

In deze sectie bespreken we de algemene prestatie-indicatoren in de context van Premium Storage. In de sectie vereisten van webtoepassingen verzameld leert u hoe u voor het meten van deze prestatie-indicatoren voor uw toepassing. Verderop in het optimaliseren van prestaties van toepassingen leert u over de factoren die invloed hebben op deze prestatie-indicatoren en aanbevelingen om ze te optimaliseren.

## <a name="iops"></a>IOPS
IOPS getal is van de aanvragen die uw toepassing worden verzonden naar de opslagschijven in één seconde. Een i/o-bewerking kan worden gelezen of opeenvolgende of willekeurig schrijven. OLTP-toepassingen, zoals een detailhandel online-website moeten veel gelijktijdige gebruikersaanvragen onmiddellijk verwerkt. De aanvragen van gebruikers zijn invoegen en bijwerken van intensieve databasetransacties, die de toepassing moet snel worden verwerkt. OLTP-toepassingen moeten daarom zeer hoge IOPS. Dergelijke toepassingen verwerken miljoenen kleine en willekeurige i/o-aanvragen. Als u een dergelijke toepassing hebt, moet u de infrastructuur van de toepassing om te optimaliseren voor IOPS ontwerpen. In de volgende sectie *optimaliseert de prestaties van toepassingen*, we in detail bespreken met de factoren waarmee u rekening houden moet om op te halen van hoge IOPS.

Wanneer u een premium opslagschijf koppelen aan uw grote schaal VM, Azure-bepalingen voor u een gegarandeerd aantal IOP's aan de hand van de schijf-specificatie. Bijvoorbeeld: richt een P50 schijf 7500 IOPS. Elke grootschalige VM-grootte heeft ook een specifieke IOPS-limiet voor dat het bestand is tegen. Een standaard GS5-VM heeft bijvoorbeeld 80.000 IOP's beperken.

## <a name="throughput"></a>Doorvoer
Doorvoer of bandbreedte is de hoeveelheid gegevens die uw toepassing worden verzonden naar het storage-schijven in een opgegeven interval. Als uw toepassing invoer/uitvoer-bewerkingen met grote i/o-eenheid grootten presteert, is er een hoge doorvoer vereist. Datawarehouse-toepassingen vaak om uit te geven van de scan bewerkingsintensieve bewerkingen die toegang tot grote delen van gegevens op een tijdstip en vaak bulksgewijs bewerkingen uit te voeren. Met andere woorden, hogere doorvoer nodig hebben voor deze toepassingen. Als u een dergelijke toepassing hebt, moet u de infrastructuur te optimaliseren voor doorvoer ontwerpen. In de volgende sectie wordt besproken hoe in detail de factoren die u moet afstemmen om dit te bereiken.

Wanneer u een premium opslagschijf koppelen aan een grote schaal VM, Azure-bepalingen doorvoer volgens de specificatie van die schijf. Bijvoorbeeld: richt een P50 schijf 250 MB per seconde schijf doorvoer. Elke grootschalige VM-grootte heeft ook als specifieke doorvoerlimiet die het bestand is tegen. Standard GS5-VM heeft bijvoorbeeld een maximale doorvoer van 2000 MB per seconde. 

Er is een relatie tussen de doorvoer en IOPS, zoals wordt weergegeven in de onderstaande formule.

![](media/premium-storage-performance/image1.png)

Daarom is het belangrijk om te bepalen van de optimale doorvoer en IOPS-waarden die uw toepassing vereist. Als u een optimaliseren probeert, wordt de andere ook beïnvloed. In een volgende sectie *optimaliseert de prestaties van toepassingen*, bespreken we met meer details over het optimaliseren van IOPS en doorvoer.

## <a name="latency"></a>Latentie
Latentie is de tijd die nodig is een aanvraag ontvangen van een enkele aanvraag verzenden naar de opslagschijven en het antwoord naar de client te verzenden. Dit is een kritieke meting van de prestaties van een toepassing naast IOPS en doorvoer. De latentie van de schijf voor een premium-opslag is de tijd die nodig zijn voor het ophalen van de gegevens voor een aanvraag en communiceren terug naar uw toepassing. Premium Storage biedt een consistente lage latenties. Als u alleen-lezentoegang hostcache in op premium-opslagschijven inschakelt, kunt u veel lagere latentie voor het lezen. We in schijfcache wordt besproken in de volgende sectie in meer detail op *optimaliseert de prestaties van toepassingen*.

Wanneer u uw toepassing om op te halen van hogere IOPS en doorvoer optimaliseert, heeft dit invloed op de latentie van uw toepassing. Na het afstemmen van de prestaties van toepassingen, moet u altijd de latentie van de toepassing om te voorkomen van onverwachte hoge latentie gedrag evalueren.

## <a name="gather-application-performance-requirements"></a>Verzamelen van prestaties toepassingsvereisten
De eerste stap bij het ontwerpen van toepassingen met hoge prestaties die worden uitgevoerd op Azure Premium Storage is om te begrijpen van de prestatievereisten van uw toepassing. Nadat u prestatie-eisen verzamelen, kunt u uw toepassing in de meest optimale prestaties kunt optimaliseren.

In de vorige sectie, we de algemene prestatie-indicatoren, IOPS, Throughput and Latency uitgelegd. U moet bepalen welke van deze prestatie-indicatoren zijn essentieel voor uw toepassing om de gewenste gebruiker-ervaring te leveren. Bijvoorbeeld, belangrijkst hoge IOPS is voor de verwerking van miljoenen transacties in een tweede OLTP-toepassingen. Terwijl hoge doorvoer essentieel voor het verwerken van grote hoeveelheden gegevens in een tweede Data Warehouse-toepassingen is. Extreem lage latentie is van cruciaal belang voor realtime-toepassingen, zoals live videostreaming van websites.

Meet vervolgens de maximale prestatie-eisen van uw toepassing gedurende hun levensduur. Gebruik de voorbeeld-controlelijst hieronder als een begin. Noteer de vereisten voor maximale prestaties tijdens normaal, de piek- en buiten kantooruren werkbelasting punten. Door het identificeren van de vereisten voor alle workloads niveaus, kunt u zich om te bepalen van de algehele prestaties behoefte van uw toepassing. De normale werkbelasting van een e-commerce-website worden bijvoorbeeld de transacties die het resultaat wordt gebruikt in de meeste dagen in een jaar. De piekworkload van de website worden de transacties die het resultaat wordt gebruikt tijdens de feestdagen of speciale verkoop gebeurtenissen. De piekworkload is doorgaans ervaren gedurende een beperkte periode, maar kan vereisen om de schaal van twee of meer keer de normale werking van uw toepassingen. Ontdek de 50 percentiel, 90 percentiel en 99 percentiel vereisten. Dit kunt uitfilteren uitbijters in de prestatie-eisen en u uw inspanningen zich kunt richten op het optimaliseren van voor de juiste waarden.

**Controlelijst voor de vereisten van de toepassing prestaties**

| **Prestatie-eisen** | **50 percentiel** | **90 percentiel** | **99 percentiel** |
| --- | --- | --- | --- |
| Met maximaal Transacties per seconde | | | |
| % Leesbewerkingen | | | |
| % Schrijfbewerkingen | | | |
| Willekeurige %-bewerkingen | | | |
| % Sequentiële activiteiten | | | |
| Grootte van i/o-aanvraag | | | |
| Gemiddelde doorvoer | | | |
| Met maximaal Doorvoer | | | |
| Min. Latentie | | | |
| Gemiddelde latentie | | | |
| Met maximaal CPU | | | |
| Gemiddeld CPU-gebruik | | | |
| Met maximaal Geheugen | | | |
| Gemiddeld geheugen | | | |
| Wachtrijdiepte | | | |

> [!NOTE]
> U moet rekening houden met deze getallen op basis van de verwachte toekomstige groei van uw toepassing schalen. Het is een goed idee om te plannen voor groei van tevoren, omdat deze mogelijk moeilijker te wijzigen van de infrastructuur voor het verbeteren van de prestaties later opnieuw.
>
>

Als u een bestaande toepassing hebt en wilt overstappen op Premium Storage, moet u eerst de controlelijst hierboven voor de bestaande toepassing bouwen. Vervolgens een prototype van uw toepassing op Premium Storage maken en ontwerpen van de toepassing op basis van de richtlijnen die worden beschreven in *optimaliseert de prestaties van toepassingen* in een volgende sectie van dit document. De volgende sectie beschrijft de hulpmiddelen die u gebruiken kunt voor het verzamelen van de metingen van de prestaties.

Maak een controlelijst die vergelijkbaar is met uw bestaande toepassing voor het model. Met behulp van hulpprogramma's voor Benchmarking kunt u de werkbelastingen te simuleren en meten van prestaties van de toepassing prototype. Zie de sectie over [Benchmarking](#benchmarking) voor meer informatie. Hierdoor kunt u bepalen of Premium-opslag kan overeenkomen met of groter zijn dan de prestatievereisten van uw toepassing. Vervolgens kunt u dezelfde richtlijnen als voor uw productietoepassing implementeren.

### <a name="counters-to-measure-application-performance-requirements"></a>Tellers voor het meten van prestaties toepassingsvereisten
De beste manier om te meten van de prestatievereisten van uw toepassing, is met bewaking van toepassingsprestaties hulpprogramma's voor door het besturingssysteem van de server. U kunt PerfMon voor Windows en iostat voor Linux. Deze hulpprogramma's vastleggen tellers overeenkomt met elke meting wordt uitgelegd in de bovenstaande sectie. U moet de waarden van deze prestatiemeteritems vastleggen wanneer uw toepassing wordt uitgevoerd, zijn normaal, de piek- en buiten kantooruren werkbelastingen.

De prestatiemeteritems zijn beschikbaar voor processor, geheugen, en elke logische schijf en de fysieke schijf van uw server. Als u premium storage-schijven met een virtuele machine, de fysieke schijf-prestatiemeteritems maken voor elke premium-opslagschijf en prestatiemeteritems voor logische schijf zijn voor elk volume dat is gemaakt op de premium-opslagschijven. U kunt de waarden voor de schijven die als host de workload van uw toepassing fungeren moet vastleggen. Als er een één-op-een-toewijzing tussen logische en fysieke schijven, kunt u verwijzen naar prestatiemeteritems voor fysieke schijf; Raadpleeg anders de prestatiemeteritems voor logische schijf. Op Linux genereert de opdracht iostat een CPU en het disk utilization-rapport. Het disk utilization-rapport voorziet in statistieken per fysieke apparaat of de partitie. Als u een database-server met de gegevens en logboekbestanden op afzonderlijke schijven hebt, verzamelen van deze gegevens voor beide schijven. Onderstaande tabel worden beschreven tellers voor schijven, processor en geheugen:

| Teller | Beschrijving | PerfMon | Iostat |
| --- | --- | --- | --- |
| **IOP's of transacties per seconde** |Het aantal i/o-aanvragen die zijn verleend aan de schijf voor opslag per seconde. |Schijf lezen per seconde <br> Schijf schrijven per seconde |tps <br> r/s <br> w/s |
| **Schijf lees- en schrijfbewerkingen** |% van leesbewerkingen en bewerkingen die worden uitgevoerd op de schijf te schrijven. |Percentage schijftijd voor lezen <br> Percentage schijftijd voor schrijven |r/s <br> w/s |
| **Doorvoer** |Hoeveelheid gegevens lezen uit of schrijven naar de schijf per seconde. |Bytes gelezen op schijf/sec <br> Bytes geschreven naar schijf/sec |kB_read/s <br> kB_wrtn/s |
| **Latentie** |Totale tijd om een schijf-i/o-aanvraag te voltooien. |Gemiddelde leestijd voor schijf <br> Gemiddelde aantal schrijfbewerkingen per seconde |await <br> svctm |
| **I/o-grootte** |De grootte van i/o-aanvragen problemen aan de storage-schijven. |Gemiddeld aantal gelezen Bytes <br> Gemiddelde aantal Bytes/schrijven |avgrq sz |
| **Wachtrijdiepte** |Aantal openstaande i/o-aanvragen wachten om te lezen uit of geschreven naar de schijf voor opslag. |Huidige wachtrijlengte voor schijf |avgqu sz |
| **Max. Geheugen** |Hoeveelheid geheugen die nodig is voor het uitvoeren van toepassing probleemloos |% Toegewezen Bytes in gebruik |Vmstat gebruiken |
| **Max. CPU** |Hoeveelheid CPU vereist voor het uitvoeren van toepassing probleemloos |% Processortijd |% gebr. |

Meer informatie over [iostat](https://linux.die.net/man/1/iostat) en [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>De prestaties van toepassingen optimaliseren
De belangrijkste factoren die invloed hebben op prestaties van een toepassing die wordt uitgevoerd op de Premium-opslag zijn aard van i/o-aanvragen, VM-grootte, schijfgrootte, het aantal schijven, opslaan in schijfcache, Multithreading en wachtrijdiepte. Sommige van deze factoren kunt u bepalen met knoppen geleverd door het systeem. De meeste toepassingen kunnen u niet een optie voor het wijzigen van de i/o-grootte en de wachtrijdiepte rechtstreeks geven. Bijvoorbeeld, als u SQL Server gebruikt, kiezen u niet de diepte van de i/o-grootte en wachtrij. SQL Server kiest de optimale i/o-grootte en wachtrij diepte waarden om de meeste prestaties te verkrijgen. Het is belangrijk om te begrijpen van de gevolgen van beide typen factoren voor de prestaties van uw toepassing, zodat u de juiste resources om te voldoen aan prestatiebehoeften kunt inrichten.

Raadpleeg de controlelijst voor de vereisten van toepassing die u hebt gemaakt, om te bepalen hoeveel moet u het optimaliseren van prestaties van uw toepassing in deze sectie. Op basis van die, kunt u zich om te bepalen welke factoren van deze sectie moet u om af te stemmen. Te wonen de effecten van elke factor op de prestaties van uw toepassing, hulpprogramma's voor benchmarking worden uitgevoerd op de toepassingsinstellingen van uw. Raadpleeg de [Benchmarking](#Benchmarking) sectie aan het einde van dit artikel voor stappen voor het uitvoeren van algemene hulpprogramma's voor benchmarking op Windows en Linux-machines.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>IOPS, Throughput and Latency optimaliseren in een oogopslag

De onderstaande tabel bevat een overzicht van de prestatiefactoren en de benodigde stappen voor het optimaliseren van IOPS, doorvoer en latentie. In de secties na dit overzicht wordt beschrijven elk van meerdere factoren is veel meer diepgang.

Zie voor meer informatie over VM-grootten en op de IOPS, doorvoer en latentie beschikbaar voor elk type virtuele machine, [Linux VM-grootten](../articles/virtual-machines/linux/sizes.md) of [Windows VM-grootten](../articles/virtual-machines/windows/sizes.md).

| &nbsp; | **IOPS** | **Doorvoer** | **Latentie** |
| --- | --- | --- | --- |
| **Voorbeeldscenario 's** |Enterprise OLTP-toepassing vereisen zeer hoog transacties per seconde tarief. |Enterprise Data warehousing toepassing verwerking van grote hoeveelheden gegevens. |In de buurt van real-time toepassingen die een directe antwoorden op aanvragen van gebruikers, zoals onlinegames vereisen. |
| Prestatiefactoren | &nbsp; | &nbsp; | &nbsp; |
| **I/o-grootte** |Kleinere i/o-grootte leidt tot hogere IOPS. |Grotere i/o-grootte leidt tot hogere doorvoer. | &nbsp;|
| **VM-grootte** |Gebruik een VM-grootte die groter is dan de vereisten van uw toepassing IOP's biedt. |Een VM-grootte met doorvoerlimiet groter is dan de vereisten van uw toepassing gebruiken. |Gebruik een VM-grootte dat aanbiedingen limieten groter is dan de vereisten van uw toepassing schalen. |
| **Schijfgrootte** |Gebruik een schijfgrootte, IOPS is groter dan de vereisten van uw toepassing biedt. |Gebruik een grootte van de schijf met doorvoerlimiet groter is dan de vereisten van uw toepassing. |Gebruik een grootte van de schijf dat aanbiedingen limieten groter is dan de vereisten van uw toepassing schalen. |
| **Virtuele machine en Schaallimieten voor schijf** |IOPS-limiet van de VM-grootte gekozen moet groter zijn dan totaal aantal IOPS aangestuurd door premium storage-schijven die zijn gekoppeld aan deze. |Doorvoerlimiet van de VM-grootte gekozen moet groter zijn dan de totale doorvoer aangestuurd door premium storage-schijven die zijn gekoppeld aan deze zijn. |Schaallimieten van de VM-grootte gekozen moet groter zijn dan totaal aantal schaallimieten van gekoppelde premium storage-schijven. |
| **Schijfcache** |Alleen-lezen Cache inschakelen op premium storage-schijven met zware leesbewerkingen om op te halen van hogere IOPS voor lezen. | &nbsp; |Alleen-lezen Cache inschakelen op premium storage-schijven met gereed zware bewerkingen om op te halen lezen zeer lage latentie. |
| **Striping van de schijf** |Meerdere schijven gebruiken en deze samen als u een gecombineerde hogere limiet voor IOPS en doorvoer stripe. Houd er rekening mee dat de gecombineerde limiet per virtuele machine hoger zijn dan de gecombineerde limieten van gekoppelde premium-schijven moet. | &nbsp; | &nbsp; |
| **Streepgrootte** |Kleinere streep voor willekeurige kleine i/o-patroon gezien in OLTP-toepassingen. Gebruik bijvoorbeeld stripe-grootte van 64KB voor SQL Server OLTP-toepassing. |Grotere stripe voor opeenvolgende grote i/o-patroon bekijken in Data Warehouse-toepassingen. Gebruik bijvoorbeeld Streepgrootte van 256KB voor SQL Server Data warehouse-toepassing. | &nbsp; |
| **Multithreading** |Gebruik multithreading pushen hoe hoger de waarde van aanvragen naar de Premium-opslag die tot hogere IOPS en doorvoer leiden. Stel bijvoorbeeld een hoge waarde van MAXDOP meer CPU's worden toegewezen aan SQL Server op SQL Server. | &nbsp; | &nbsp; |
| **Wachtrijdiepte** |Grotere wachtrijdiepte leidt tot hogere IOPS. |Grotere wachtrijdiepte leidt tot hogere doorvoer. |Kleinere wachtrijdiepte leidt tot kortere wachttijden. |

## <a name="nature-of-io-requests"></a>De aard van de i/o-aanvragen
Een i/o-aanvraag is een eenheid van i/o-bewerking dat door uw toepassing wordt uitgevoerd. Identificeren van de aard van i/o-aanvragen, willekeurige of opeenvolgende, kunt lezen of schrijven, klein of groot is, u bepalen van de prestatievereisten van uw toepassing. Het is heel belangrijk om te begrijpen van de aard van i/o-aanvragen, de juiste beslissingen bij het ontwerpen van de infrastructuur van uw toepassing.

I/o-grootte is een van de belangrijkste factoren. De i/o-grootte is de grootte van de bewerkingsaanvraag van de i/o-door uw toepassing wordt gegenereerd. De i/o-grootte heeft een aanzienlijke invloed op de prestaties met name op de IOPS en bandbreedte die de toepassing kan bereiken. De volgende formule wordt de relatie tussen IOP's, i/o-grootte en de bandbreedte/doorvoer.  
    ![](media/premium-storage-performance/image1.png)

Sommige toepassingen kunnen u de i/o-grootte, alter, maar sommige toepassingen niet. Bijvoorbeeld, SQL Server bepaalt de optimale grootte voor i/o-zelf en biedt gebruikers met alle knoppen om deze te wijzigen. Aan de andere kant Oracle biedt een parameter met de naam [DB\_blokkeren\_grootte](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) die u kunt configureren met de grootte van de i/o-aanvraag van de database.

Gebruik de richtlijnen in dit artikel als u een toepassing die u wilt wijzigen van de i/o-grootte niet toestaat, het optimaliseren van de KPI's die het meest relevant is voor uw toepassing. Bijvoorbeeld:

* Een OLTP-toepassing genereert miljoenen kleine en willekeurige i/o-aanvragen. Voor het afhandelen van dit type i/o-aanvragen, moet u de infrastructuur van uw toepassingen om op te halen van hogere IOPS ontwerpen.  
* Een datawarehouse-aanvraag genereert grote en opeenvolgende i/o-aanvragen. Voor het afhandelen van dit type i/o-aanvragen, moet u de infrastructuur van uw toepassingen om op te halen van de hogere bandbreedte of doorvoer ontwerpen.

Als u een toepassing, waarmee u de i/o-grootte te wijzigen, gebruikt u deze vuistregel voor de i/o-grootte naast andere prestatierichtlijnen

* Kleinere i/o-grootte aan hogere IOPS. Bijvoorbeeld: 8 KB voor een OLTP-toepassing.  
* Grotere i/o-grootte om hogere bandbreedte/doorvoer te krijgen. Bijvoorbeeld 1024 KB voor een datawarehouse-toepassing.

Hier volgt een voorbeeld van hoe u de IOPS en doorvoer/bandbreedte voor uw toepassing kunt berekenen. Houd rekening met een toepassing met behulp van een P30-schijf. De maximale IOPS en doorvoer/bandbreedte een P30-schijf kunnen maar liefst is 5000 IOP's en 200 MB per seconde respectievelijk. Nu, als uw toepassing vereist dat de maximale IOPS van de P30-schijf en u een kleinere i/o-grootte, zoals van 8 KB gebruikt, is de resulterende bandbreedte kunt u zich aan 40 MB per seconde. Als uw toepassing vereist dat de maximale doorvoer/bandbreedte van P30-schijf en u een grotere i/o-grootte, zoals 1024 KB, de IOPS-waarde wordt wel minder, 200 IOPS. Stem de i/o-grootte daarom zo dat deze voldoet aan de vereiste IOPS en doorvoer/bandbreedte van zowel uw toepassing. De volgende tabel geeft een overzicht van de i/o-grootte en hun bijbehorende IOPS en doorvoer voor een P30-schijf.

| Vereisten voor toepassingsimplementatie | I/o-grootte | IOPS | Doorvoer/bandbreedte |
| --- | --- | --- | --- |
| Max. IOP's |8 kB |5.000 |40 MB per seconde |
| Max. doorvoer |1024 kB |200 |200 MB per seconde |
| Maximale Throughput + hoge IOPS |64 kB |3,200 |200 MB per seconde |
| Maximale IOPS en hoge doorvoer |32 KB |5.000 |160 MB per seconde |

Voor IOPS en bandbreedte die hoger is dan de maximale waarde van een enkele premium-opslagschijf, gebruikt u meerdere premium-schijven striped samen. Bijvoorbeeld, stripe twee P30 schijven voor het verkrijgen van een gecombineerde IOPS van 10.000 IOP's of een gecombineerde doorvoer van 400 MB per seconde. Zoals uitgelegd in de volgende sectie, moet u een VM-grootte die ondersteuning biedt voor de gecombineerde schijf-IOPS en doorvoer.

> [!NOTE]
> Als u IOPS of de andere verhoogt ook de doorvoer verhoogt, zorg er dan voor dat u doorvoer of IOPS-limieten van de schijf of virtuele machine niet tegenkomen wanneer u een verhogen.
>
>

U kunt te wonen de gevolgen van i/o-grootte voor de prestaties van toepassingen, hulpprogramma's voor benchmarking uitvoeren op de virtuele machine en schijven. Maak meerdere testuitvoeringen en andere i/o-grootte voor elke uitvoering gebruiken om te zien van de impact. Raadpleeg de [Benchmarking](#Benchmarking) sectie aan het einde van dit artikel voor meer informatie.

## <a name="high-scale-vm-sizes"></a>Grote schaal VM-grootten
Wanneer u het ontwerpen van een toepassing start, is een van de eerste dingen te doen, kiest u een virtuele machine voor het hosten van uw toepassing. Premium Storage wordt geleverd met hoge schaal VM-grootten die toepassingen die meer rekenkracht en een hoge lokale schijf i/o-prestaties nodig hebben kunnen worden uitgevoerd. Deze virtuele machines hebben snellere processors, een hogere geheugen-kernverhouding en een Solid-State Drive (SSD) voor de lokale schijf. Voorbeelden van hoge schaal VM's die Premium-opslag zijn de DS, DSv2 en GS-serie VM's.

Hoge schaal VM's zijn beschikbaar in verschillende grootten met een ander aantal CPU-kernen, geheugen, besturingssysteem en grootte van de tijdelijke schijf. Elke VM-grootte heeft ook het maximum aantal gegevensschijven die u aan de virtuele machine koppelen kunt. Daarom de gekozen VM-grootte is van invloed op hoeveelheid verwerking, geheugen en opslagcapaciteit is beschikbaar voor uw toepassing. Dit geldt ook voor de Compute en Storage-kosten. Hieronder vindt u bijvoorbeeld de specificaties van de grootste VM-grootte in een DS-serie, DSv2-serie en GS-serie:

| VM-grootte | CPU-kernen | Geheugen | Schijf-VM-grootten | Met maximaal Gegevensschijven | Cachegrootte | IOPS | Bandbreedte Cache i/o-limieten |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |OS 1023 GB = <br> Lokale SSD 224 GB = |32 |576 GB |MAAR LIEFST 50.000 IOPS <br> 512 MB per seconde |4000 IOPS en 33 MB per seconde |
| Standard_GS5 |32 |448 GB |OS 1023 GB = <br> Lokale SSD 896 GB = |64 |4224 GB |80.000 IOP 'S <br> 2.000 MB per seconde |5000 IOP's en 50 MB per seconde |

Als u wilt weergeven van een volledige lijst van alle beschikbare Azure-VM-grootten, verwijzen naar [Windows VM-grootten](../articles/virtual-machines/windows/sizes.md) of [Linux VM-grootten](../articles/virtual-machines/linux/sizes.md). Kies een VM-grootte die kan voldoen en aanpassen aan uw prestatievereisten van de gewenste toepassing. Daarnaast rekening mee dat volgende belangrijke overwegingen bij het kiezen van VM-grootten.

*Schaallimieten*  
De maximale IOPS-limieten per VM en per schijf zijn verschillende en onafhankelijk van elkaar. Zorg ervoor dat de toepassing IOP's binnen de grenzen van de virtuele machine, evenals de premium-schijven zijn gekoppeld aan deze wordt bestuurd. De prestaties van toepassingen maken anders beperking.

Stel bijvoorbeeld dat een vereiste toepassing is maximaal 4000 IOPS. Om dit te doen, moet u een P30-schijf op een VM DS1 inrichten. De P30-schijf kan maximaal 5000 IOP's leveren. De VM DS1 is echter beperkt tot 3200 IOPS. Als gevolg daarvan kunnen de toepassingsprestaties zal worden beperkt door de limiet van de virtuele machine op een 3200 IOPS en zullen er verminderde prestaties. Om te voorkomen dat deze situatie, kiest u een schijf en VM-grootte die beide voldoen aan de vereisten van webtoepassingen.

*Kosten van bewerking*  
In veel gevallen is het mogelijk dat uw totale kosten van het opnieuw met Premium Storage lager is dan het gebruik van Standard-opslag.

Neem bijvoorbeeld een toepassing 16.000 IOPS vereisen. Deze om prestaties te behalen, moet u een standaard\_D14 Azure IaaS VM, zodat een maximale IOPS van 16.000 met behulp van 32 standard storage-schijven voor 1 TB. Elke schijf van 1TB standard-opslag kunt maximaal 500 IOPS bereiken. De geschatte kosten van deze virtuele machine per maand is $1,570. De maandelijkse kosten van 32 standard storage-schijven is $1,638. De geschatte totale maandelijkse kosten worden $3,208.

Echter, als u dezelfde toepassing op Premium-opslag die wordt gehost, moet u een kleinere virtuele machine en minder premium storage-schijven, waardoor de totale kosten. Een standaard\_DS13-VM kan voldoen aan de 16.000 IOPS vereiste met behulp van vier P30-schijven. De DS13-virtuele machine heeft een maximale IOPS van 25,600 en elke P30-schijf heeft een maximale IOPS van 5000. Algemene, deze configuratie 5.000 x 4 = 20.000 kan maar liefst IOPS. De geschatte kosten van deze virtuele machine per maand is $1,003. De maandelijkse kosten van vier P30 premium storage-schijven is $544.34. De geschatte totale maandelijkse kosten worden $1,544.

De volgende tabel geeft een overzicht van de verdeling van de kosten van dit scenario voor Standard en Premium-opslag.

| &nbsp; | **Standard** | **Premium** |
| --- | --- | --- |
| **Kosten van virtuele machine per maand** |$1,570.58 (standard\_D14) |$1,003.66 (standard\_DS13) |
| **Kosten van de schijven per maand** |$1,638.40 (32 x 1 TB schijven) |$544.34 (4 x P30 schijven) |
| **Totale kosten per maand** |$3,208.98 |$1,544.34 |

*Linux-distributies*  

Met Azure Premium Storage krijgt u hetzelfde niveau van de prestaties voor virtuele machines met Windows en Linux. We bieden ondersteuning voor vele versies van Linux-distributies en ziet u de volledige lijst [hier](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het is belangrijk te weten dat verschillende distributies beter voor verschillende soorten workloads geschikt zijn. Hier ziet u verschillende prestatieniveaus, afhankelijk van de distro die uw workload wordt uitgevoerd op. De Linux-distributies met uw toepassing testen en kiest de optie die het beste werkt.

Wanneer waarop Linux wordt uitgevoerd met Premium Storage, controleert u de meest recente updates over de vereiste stuurprogramma's om te controleren of hoge prestaties.

## <a name="premium-storage-disk-sizes"></a>Premium Storage-schijfgrootten
Azure Premium Storage biedt momenteel acht schijfgrootten. De grootte van elke schijf heeft de limiet van een andere schaal voor IOPS, bandbreedte en opslag. Kies het recht voor Premium Storage-schijfgrootte, afhankelijk van de toepassingsvereisten voor de en de hoge schaal VM-grootte. De onderstaande tabel ziet u de grootte van de acht schijven en de bijbehorende mogelijkheden. P4 P6 en P15-grootten zijn momenteel alleen ondersteund voor Managed Disks.

| Schijftype voor Premium-schijven  | P4    | P6    | P10   | P15 | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 32 GB | 64 GB | 128 GB| 256 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 1100 | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde |125 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde | 


Het aantal schijven die u kiest is afhankelijk van de schijf het formaat van de gekozen. U kunt één P50 schijf of meerdere P10-schijven gebruiken om te voldoen aan de vereisten van uw toepassing. Aandachtspunten voor gebruikersaccounts die hieronder worden vermeld bij het maken van de keuze rekening.

*De Schaallimieten (IOPS en doorvoer)*  
De limieten voor IOPS en doorvoer van elke Premium-schijfgrootte is anders en onafhankelijk van de schaallimieten voor virtuele machine. Zorg ervoor dat het totale aantal IOPS en doorvoer van de schijven zich binnen de grenzen van de schaal van de gekozen VM-grootte.

Bijvoorbeeld, als een vereiste van toepassing is een maximum van 250 MB per seconde doorvoer en u gebruikt een DS4-VM met een enkele P30-schijf. De DS4-VM kunt geven tot 256 MB per seconde doorvoer. Een enkele P30-schijf heeft echter doorvoerlimiet van 200 MB per seconde. Als gevolg hiervan wordt de toepassing worden beperkt op 200 MB per seconde door de limiet van de schijf. Om te strijden tegen deze limiet, meer dan één gegevensschijven aan de virtuele machine inrichten of het formaat wijzigen van de schijven P40 of P50.

> [!NOTE]
> Leesbewerkingen geleverd door de cache zijn niet opgenomen in de schijf-IOPS en doorvoer en kan daarom niet onderworpen is aan de schijflimieten. Cache heeft de afzonderlijke limiet voor IOPS en doorvoer per virtuele machine.
>
> Bijvoorbeeld, zijn in eerste instantie de lees- en schrijfbewerkingen 60MB per seconde en 40MB per seconde. Na verloop van tijd, de cache van warms en meer en meer van de gelezen uit de cache fungeert. Vervolgens krijgt u hogere schrijven doorvoer van de schijf.
>
>

*Aantal schijven*  
Bepaalt het aantal schijven, u gaat door de beoordeling van de vereisten van webtoepassingen. Elke VM-grootte heeft ook een limiet voor het aantal schijven die u aan de virtuele machine koppelen kunt. Dit is meestal twee keer het aantal kernen. Zorg ervoor dat de VM-grootte die u kunt ondersteuning voor het aantal schijven die nodig zijn.

Vergeet niet dat de Premium Storage-schijven hebben hogere prestaties levert vergeleken met de Standard-opslagschijven. Dus als u uw toepassing vanuit Azure IaaS-VM met behulp van standaardopslag naar Premium Storage migreert, moet u waarschijnlijk minder premium-schijven de dezelfde of een hogere prestaties voor uw toepassing bereiken.

## <a name="disk-caching"></a>Schijfcache
Hoge schaal-virtuele machines die gebruikmaken van Azure Premium Storage hebben een meerlagige cachetechnologie BlobCache genoemd. Een combinatie van de virtuele Machine RAM-geheugen en lokale SSD BlobCache gebruikt voor de cache. Deze cache is beschikbaar voor de permanente schijven voor Premium-opslag en de lokale VM-schijven. Standaard is deze cache-instelling ingesteld op lezen/schrijven van besturingssysteemschijven en alleen-lezen voor gegevensschijven die worden gehost op Premium Storage. Met opslaan in schijfcache ingeschakeld op de Premium-opslagschijven, kunt de grote schaal VM's zeer hoge prestatieniveau die groter zijn dan de onderliggende schijfprestaties bereiken.

> [!WARNING]
> Als u de cache-instelling van een Azure-schijf losgekoppeld en opnieuw wordt de doelschijf. Als de besturingssysteemschijf is, kan de virtuele machine opnieuw wordt opgestart. Stop alle toepassingen en services die kunnen worden beïnvloed door deze wordt onderbroken voordat u de schijf-cache-instelling wijzigt.
>
>

Raadpleeg voor meer informatie over de werking van BlobCache binnen [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogbericht.

Het is belangrijk om in te schakelen van de cache op de juiste set met schijven. Of u moet inschakelen op een premium-schijf opslaan in schijfcache of niet zal afhankelijk zijn van het workloadpatroon die schijf verwerkt. De onderstaande tabel ziet u de cache-instellingen voor het besturingssysteem en gegevensschijven.

| **Schijftype** | **Standaardinstelling voor Cache** |
| --- | --- |
| Besturingssysteemschijf |ReadWrite |
| Gegevensschijf |ReadOnly |

Hieronder vindt u de aanbevolen schijf cache-instellingen voor gegevensschijven,

| **Schijf-cache-instelling** | **Aanbeveling van het moment waarop u deze instelling wilt gebruiken** |
| --- | --- |
| Geen |Host-cache configureren als geen voor de alleen-schrijven en schrijfintensief schijven. |
| ReadOnly |Host-cache configureren als alleen-lezen voor alleen-lezen en lezen / schrijven-schijven. |
| ReadWrite |Host-cache configureren als ReadWrite alleen als uw toepassing correct worden verwerkt in de cache opgeslagen gegevens schrijven naar een permanente schijven wanneer dat nodig is. |

*Alleen-lezen*  
Door het configureren van alleen-lezentoegang opslaan in cache op Premium Storage-gegevens op schijven, kunt u bereiken met lage latentie voor lezen en ophalen van zeer hoge IOPS voor lezen en doorvoer voor uw toepassing. Dit is vanwege twee redenen

1. Leesbewerkingen uitgevoerd vanuit de cache, dit op de virtuele machine geheugen en lokale SSD is, zijn veel sneller dan leesbewerkingen van de gegevensschijf die zich in de Azure blob-opslag.  
1. Premium-opslag telt niet mee voor de leesbewerkingen worden aangeleverd vanuit de cache op de schijf-IOPS en doorvoer. Daarom is uw toepassing kunnen hogere totale IOPS en doorvoer te realiseren.

*ReadWrite*  
De OS-schijven hebben standaard de caching van ingeschakeld. We hebben onlangs ondersteuning voor ReadWrite in cache opslaan van gegevens ook schijven toegevoegd. Als u van ReadWrite in cache opslaan gebruikmaakt, moet u een goede manier om de gegevens uit de cache schrijven naar de permanente schijven hebben. Bijvoorbeeld, SQL Server verwerkt het schrijven van gegevens in de cache naar de permanente opslag-schijven op een eigen. ReadWrite cache gebruiken met een toepassing die niet behouden van de vereiste gegevens verwerkt, kan leiden tot verlies van gegevens, als de virtuele machine vastloopt.

U kunt bijvoorbeeld deze richtlijnen toepassen op SQL Server op Premium Storage wordt uitgevoerd door het volgende te doen

1. 'Alleen-lezen' cache configureren op premium storage-schijven die als host fungeert gegevensbestanden.  
   a.  Het snelle wordt gelezen uit cache lager de Querytijd SQL Server omdat gegevenspagina's veel sneller worden opgehaald uit de cache in vergelijking met rechtstreeks vanuit de gegevens schijven.  
   b.  Lezen uit cache levert, betekent dat er extra doorvoer van premium-gegevensschijven beschikbaar. SQL Server kan deze extra doorvoer voor het ophalen van meer gegevenspagina's en andere bewerkingen zoals back-up/herstel gebruiken, batch-belastingen en index opnieuw opgebouwd.  
1. Configureren 'None' in de cache op premium storage-schijven die als host fungeert voor de logboekbestanden.  
   a.  Logboekbestanden hebben voornamelijk schrijfintensief bewerkingen. Ze kunnen daarom niet gebruikmaken van de alleen-lezen-cache.

## <a name="disk-striping"></a>Striping van de schijf
Wanneer een grootschalige die virtuele machine is gekoppeld met verschillende premium storage-permanente schijven, kunnen u de schijven samen striped voor het samenvoegen van hun IOPs, bandbreedte en opslagcapaciteit.

Op Windows, kunt u Storage Spaces stripe schijven samen. U moet een kolom voor elke schijf in een pool configureren. Anders kan de algehele prestaties van striped volumes lager dan verwacht vanwege een ongelijke distributie van verkeer op de schijven zijn.

Belangrijk: Met behulp van Serverbeheer-UI, kunt u het totale aantal kolommen maximaal 8 voor een striped volume instellen. Bij het toevoegen van meer dan 8 schijven, kunt u PowerShell gebruiken om het volume te maken. Met behulp van PowerShell, kunt u instellen het aantal kolommen gelijk zijn aan het aantal schijven. Bijvoorbeeld, als er 16 schijven in een stripeset één; Geef 16 kolommen in de *NumberOfColumns* parameter van de *New-VirtualDisk* PowerShell-cmdlet.

Gebruik het hulpprogramma MDADM stripe-schijven samen op Linux. Voor gedetailleerde stappen voor het striping schijven op Linux naar verwijzen [Software-RAID configureren onder Linux](../articles/virtual-machines/linux/configure-raid.md).

*Streepgrootte*  
Een belangrijke configuratie in schijfsegmentering wordt de stripe-grootte. De Streepgrootte van of de blokgrootte is de kleinste hoeveelheid gegevens die de toepassing op een striped volumes oplossen kunt. De stripe-grootte die u configureert, is afhankelijk van het type van de toepassing en het gebruikspatroon van de aanvraag. Als u de verkeerde stripe-grootte kiest, kan dit leiden tot i/o-uitlijning, die tot verminderde prestaties van uw toepassing leiden.

Bijvoorbeeld, als een i/o-aanvraag die is gegenereerd door uw toepassing groter dan de Streepgrootte van de schijf is, schrijft het opslagsysteem deze grenzen stripe-eenheid op meer dan één schijf. Wanneer is het tijd om de toegang tot de gegevens, is er om te zoeken voor meer dan één stripe-eenheden om de aanvraag te voltooien. Het cumulatieve effect van dergelijk gedrag kan leiden tot aanzienlijke prestatievermindering. Anderzijds, kunnen als de grootte van de i/o-aanvraag is kleiner dan Streepgrootte, en als het willekeurig is, de i/o-aanvragen oplopen op dezelfde schijf als een knelpunt veroorzaakt en uiteindelijk vernederen de i/o-prestaties.

Afhankelijk van het type werkbelasting van uw toepassing wordt uitgevoerd, kiest u een juiste stripe-grootte. Gebruik een kleinere streep voor willekeurige kleine i/o-aanvragen. Dat aanvragen voor grote opeenvolgende i/o een groter stripe gebruiken. Ontdek de stripe aanbevelingen voor de grootte voor de toepassing dat u zal worden uitgevoerd op Premium Storage. Configureren voor SQL Server, stripe-grootte van 64KB voor OLTP-workloads en 256KB voor magazijnbeheer data-workloads. Zie [aanbevolen procedures voor prestaties voor SQL Server op Azure Virtual machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) voor meer informatie.

> [!NOTE]
> U kunt stripe samen een maximum van 32 premium storage-schijven op een virtuele machine uit de DS-serie en 64 premium storage-schijven op een virtuele machine uit de GS-serie.
>
>

## <a name="multi-threading"></a>Multithreading
Azure is zodanig ontworpen Premium Storage-platform, worden uitgebreide parallelle verwerkingsarchitectuur. Een toepassing met meerdere threads realiseert daarom veel betere prestaties dan een toepassing met één thread. Een toepassing met meerdere threads zijn taken splitst in meerdere threads en verhoogt de efficiëntie van de uitvoering ervan door het gebruik van de schijf en VM-resources naar het maximum.

Als uw toepassing wordt uitgevoerd op een enkele kern VM met twee threads, kunt u voor de CPU, kunt schakelen tussen de twee threads efficiëntie. Terwijl een thread op een schijf-i/o om te voltooien wacht, wordt de CPU kunt overschakelen naar de andere thread. Op deze manier kunnen uitvoeren twee threads meer dan één thread wilt. Als de virtuele machine meer dan één kern heeft, vermindert het verder uitvoeringstijd omdat elke core taken parallel kunt uitvoeren.

Kunt u mogelijk niet de manier waarop een gebruiksklare toepassing wordt geïmplementeerd op een enkele thread of multithreading. Bijvoorbeeld, is SQL Server kan verwerken van meerdere CPU en meerdere kernen. SQL Server besluit echter onder welke voorwaarden het gebruik van een of meer threads voor het verwerken van een query. Het kan query's uitvoeren en opbouwen van indexen met multithreading. SQL Server gebruikt voor een query die omvat het samenvoegen van grote tabellen en het sorteren van gegevens voordat u terugkeert naar de gebruiker, waarschijnlijk meerdere threads. Een gebruiker kan echter niet instellen of SQL Server wordt uitgevoerd een query met een enkele thread of meerdere threads.

Er zijn configuratie-instellingen die u wijzigen kunt als u wilt dit van invloed zijn op multithreading of parallelle verwerking van een toepassing. In het geval van SQL Server is het bijvoorbeeld de configuratie van de maximale graad van parallelle uitvoering. Deze instelling met de naam MAXDOP, kunt u configureren van het maximum aantal processors dat SQL Server kunt gebruiken bij het parallel verwerken. U kunt MAXDOP configureren voor afzonderlijke query's of indexen. Dit is nuttig wanneer u resources van uw systeem voor een essentiële toepassing prestaties in balans brengen.

Stel bijvoorbeeld dat uw toepassing met behulp van SQL Server wordt uitgevoerd een grote query's en een indexbewerking op hetzelfde moment. Laat het ons wordt ervan uitgegaan dat u wilt dat de indexbewerking naar meer prestaties in vergelijking met de grote query. In dat geval kunt u de MAXDOP-waarde van de bewerking van de index moet hoger zijn dan de waarde van MAXDOP voor de query instellen. Op deze manier heeft SQL Server meer aantal processors dat u deze kunt gebruiken voor de indexbewerking in vergelijking met het aantal processors dat deze aan de grote query toewijzen kunt. Denk eraan dat u niet bepalen dat het aantal threads dat SQL Server wordt gebruikt voor elke bewerking. U kunt bepalen het maximum aantal processors dat wordt toegewezen voor multithreading.

Meer informatie over [graden van parallelle uitvoering](https://technet.microsoft.com/library/ms188611.aspx) in SQL Server. Ontdek dergelijke instellingen die van invloed zijn op multithreading in uw toepassing en de bijbehorende configuraties om prestaties te optimaliseren.

## <a name="queue-depth"></a>Wachtrijdiepte
De wachtrijdiepte of de lengte van wachtrij of de grootte van de wachtrij is het aantal openstaande i/o-aanvragen in het systeem. De waarde van wachtrijdiepte bepaalt hoeveel i/o-bewerkingen die uw toepassing uitlijnen kunt, die de opslagschijven wordt verwerkt. Het is van invloed op alle de drie toepassing prestatie-indicatoren dat we in dit artikel zoals, IOPS, Throughput and Latency besproken.

In de wachtrij diepte en multithreading zijn nauw verwante. De waarde van de wachtrijdiepte geeft aan hoeveel multithreading kan worden bereikt door de toepassing. Als de wachtrijdiepte groot is, toepassingen kunt uitvoeren meer bewerkingen gelijktijdig, met andere woorden, meer multithreading. Als de diepte van de wachtrij klein, is zelfs als de toepassing meerdere threads is, wordt er niet voldoende uitgelijnd voor de uitvoering van gelijktijdige aanvragen.

Normaal gesproken overal verkrijgbare toepassingen kunt u niet te wijzigen van de wachtrijdiepte omdat als onjuist doet meer beschadigd dan goed is ingesteld. Toepassingen worden de juiste waarde van wachtrijdiepte om de optimale prestaties te verkrijgen. Het is echter belangrijk te begrijpen dit concept, zodat u prestatieproblemen met uw toepassing kunt oplossen. U kunt ook de gevolgen van de wachtrijdiepte zien door het uitvoeren van hulpprogramma's voor benchmarking op uw systeem.

Sommige toepassingen Geef de instellingen voor de wachtrijdiepte van invloed zijn op. Bijvoorbeeld, wordt de instelling MAXDOP (maximale graad van parallelle uitvoering) in SQL Server in de vorige sectie beschreven. MAXDOP is een manier om te beïnvloeden wachtrijdiepte en multithreading, hoewel deze de waarde wachtrijdiepte van SQL Server niet rechtstreeks wijzigen.

*Hoge wachtrijdiepte*  
Een hoge wachtrijdiepte regels u meer bewerkingen op de schijf. De schijf kent de volgende aanvraag in de wachtrij tevoren. Als gevolg daarvan kan de schijf bewerkingen tevoren plannen en ze in een reeks voor een optimale verwerken. Omdat de toepassing meer aanvragen naar de schijf verzonden wordt, kan de schijf meer parallelle IOs verwerken. De toepassing worden uiteindelijk kunnen hogere IOP's realiseren. Sinds de toepassing wordt meer aanvragen verwerkt, verhoogt ook de totale doorvoer van de toepassing.

Normaal gesproken een toepassing kunt bereiken met maximale doorvoer met 8-16 + openstaande IO's per gekoppelde schijf. Als een wachtrijdiepte een is toepassing is niet voldoende IOs pushen naar het systeem en minder hoeveelheid worden verwerkt in een bepaalde periode. Met andere woorden, minder doorvoer.

Bijvoorbeeld, in SQL Server informeert de MAXDOP-waarde voor een query naar "4" SQL Server maximaal vier kernen voor het uitvoeren van de query te gebruiken. SQL Server, bepaalt wat de beste wachtrijdiepte waarde en het aantal cores voor de uitvoering van de query is.

*Optimale wachtrijdiepte*  
Zeer hoog wachtrijdiepte waarde heeft ook de nadelen. Als de wachtrijdiepte waarde is te hoog, probeert de toepassing om zeer hoge IOPS te stimuleren. Tenzij de toepassing heeft permanente schijven met voldoende ingerichte IOPS, kan deze toepassing latenties negatief beïnvloeden. Formule te volgen, wordt de relatie tussen IOP's, latentie en wachtrijdiepte.  
    ![](media/premium-storage-performance/image6.png)

U moet wachtrijdiepte niet configureren voor een hoge waarde, maar voor een optimale waarde, die voldoende IOP's voor de toepassing zonder latenties kan bieden. Bijvoorbeeld, als de wachttijd van de toepassing moet zijn van 1 milliseconde, de wachtrijdiepte 5000 IOP's realiseren vereist is, Wachtrijdiepte = 5000 x 0,001 = 5.

*Wachtrijdiepte voor Striped volumes*  
Voor een striped volume wachtrijdiepte een hoog genoeg zijn zodat elke schijf een piek wachtrijdiepte afzonderlijk heeft. Neem bijvoorbeeld een toepassing die een wachtrijdiepte van 2 pushes en er zijn 4 schijven in de stripe. De twee i/o-aanvragen wordt omgeleid naar twee schijven en resterende twee schijven niet actief is. Daarom de wachtrijdiepte configureren zodat alle schijven bezet is. Formule hieronder laat zien hoe om te bepalen van de wachtrijdiepte van striped volumes.  
    ![](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Beperking
Azure Premium Storage-bepalingen opgegeven aantal IOPS en doorvoer, afhankelijk van de VM-grootten en schijfgrootten die u kiest. Telkens wanneer uw toepassing probeert om het stimuleren van IOP's of doorvoer boven deze limieten van wat de virtuele machine of de schijf kan worden verwerkt, wordt het Premium Storage beperken. Dit zich voordoet in de vorm van slechtere prestaties in uw toepassing. Dit kan betekenen hogere latentie, lagere doorvoer of IOPS lager. Als Premium-opslag niet beperken biedt, wordt uw toepassing kan volledig mislukken door meer dan wat de daarbij behorende bronnen zijn geschikt voor het bereiken. Dus om prestatieproblemen te voorkomen vanwege een beperking, altijd voldoende bronnen zijn voor uw toepassing te creëren. Rekening mee te houden wat we in de VM-grootten en schijf-grootten hierboven besproken. Benchmarking is de beste manier om te achterhalen welke resources u moet uw toepassing wordt gehost.

## <a name="benchmarking"></a>Benchmarking
Benchmarking is het proces van het simuleren van verschillende workloads op uw toepassing en de prestaties van toepassingen voor elke werkbelasting te meten. Met behulp van de stappen in een eerdere sectie, hebt u de prestatie-eisen voor toepassingen verzameld. Door het uitvoeren van hulpprogramma's voor benchmarking op de virtuele machines die als host fungeert voor de toepassing, kunt u de prestatieniveaus die binnen uw toepassing met Premium Storage bereiken kunt bepalen. In deze sectie bieden wij u voorbeelden van benchmarks van een Standard DS14-virtuele machine ingericht met Azure Premium Storage-schijven.

We hebben respectievelijk algemene hulpprogramma's voor benchmarking Iometer en FIO, voor Windows en Linux gebruikt. Deze hulpprogramma's starten meerdere threads simuleren van een productie-achtige werkbelasting en meten van prestaties van het systeem. U kunt ook een parameters, zoals het block-grootte en wachtrij diepte, die u normaal gesproken niet voor een toepassing wijzigen configureren met behulp van de hulpprogramma's. Dit biedt u meer flexibiliteit om de maximale prestaties op een grote schaal VM ingericht met premium-schijven voor verschillende soorten werkbelastingen van toepassingen te stimuleren. Voor meer informatie over elke benchmarking hulpprogramma gaat u naar [Iometer](http://www.iometer.org/) en [FIO](http://freecode.com/projects/fio).

Volg de onderstaande voorbeelden, een standaard DS14-virtuele machine maken en 11 Premium-opslagschijven koppelen aan de virtuele machine. 11 schijven, 10 schijven configureren met opslaan in cache als 'Geen' en deze in een volume naam NoCacheWrites stripe. Opslaan in cache als 'Alleen-lezen' op de resterende schijf configureren en maken van een volume naam CacheReads met deze schijf. Met deze instellingen, kunt u zich om te zien van de maximale prestaties voor lezen en schrijven van een Standard DS14-virtuele machine. Ga voor gedetailleerde stappen over het maken van een DS14-VM met premium-schijven naar [maken en gebruiken een Premium Storage-account voor een VM-gegevensschijf](../articles/virtual-machines/windows/premium-storage.md).

*Opwarmen van de Cache*  
De schijf met alleen-lezentoegang opslaan in cache is mogelijk dat hoger IOP's dan de limiet van de schijf. Als u dit maximum leesprestaties uit de cache van de host, moet eerst u opgewarmd de cache van deze schijf. Dit zorgt ervoor dat de lezen IOs welke benchmarking hulpprogramma wordt station op CacheReads volume daadwerkelijk treffers in de cache en niet op de schijf rechtstreeks. Het resultaat van de cache treffers in extra IOP's uit de cache van één schijf ingeschakeld.

> **Belangrijk:**  
> U moet de cache opgewarmd voordat benchmarking, wordt uitgevoerd telkens wanneer de virtuele machine opnieuw is opgestart.
>
>

#### <a name="iometer"></a>Iometer
[Download het hulpprogramma Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) op de virtuele machine.

*Testbestand*  
Iometer maakt gebruik van een testbestand dat is opgeslagen op het volume op waarop u de benchmarking test wordt uitgevoerd. Deze stations leest en schrijft voor dit testbestand voor het meten van de schijf IOPS en doorvoer. Iometer wordt deze testbestand gemaakt als u deze niet hebt opgegeven. Maak een bestand voor het testen van 200GB iobw.tst aangeroepen op de volumes CacheReads en NoCacheWrites.

*Access-specificaties*  
De specificaties aanvragen i/o-grootte % lezen/schrijven, willekeurige/opeenvolgende % zijn geconfigureerd op het tabblad 'Specificaties Access' in Iometer. Maak een specificatie van de toegang voor elk van de scenario's die hieronder worden beschreven. De toegangsspecificaties maken en 'Opslaan' met een geschikte naam zoals – RandomWrites\_8 kB, RandomReads\_8 kB. Selecteer de bijbehorende specificatie bij het uitvoeren van de Testscenario.

Hieronder, ziet u een voorbeeld van toegang tot de specificaties voor maximale schrijven IOP's-scenario  
    ![](media/premium-storage-performance/image8.png)

*Maximale IOPS Test specificaties*  
Gebruiken om te demonstreren maximale IOPs, kleinere aanvraaggrootte. Aanvraaggrootte van 8 kB gebruiken en maken van de specificaties voor willekeurige schrijfbewerkingen en leesbewerkingen.

| Access-specificatie | Aanvraaggrootte | Willekeurige % | De % lezen |
| --- | --- | --- | --- |
| RandomWrites\_8 kB |8 KB |100 |0 |
| RandomReads\_8 kB |8 KB |100 |100 |

*Maximale doorvoer Test specificaties*  
Gebruiken om te demonstreren maximale doorvoer, grotere grootte van de aanvraag. Aanvraag voor de grootte van 64 kB gebruiken en maken van de specificaties voor willekeurige schrijfbewerkingen en leesbewerkingen.

| Access-specificatie | Aanvraaggrootte | Willekeurige % | De % lezen |
| --- | --- | --- | --- |
| RandomWrites\_64 kB |64 KB |100 |0 |
| RandomReads\_64 kB |64 KB |100 |100 |

*Uitvoering van de Test Iometer*  
Voer de stappen hieronder om te oefenen cache uit

1. Twee toegangsspecificaties maken met waarden die hieronder worden weergegeven

   | Naam | Aanvraaggrootte | Willekeurige % | De % lezen |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
1. Voer de test Iometer voor het initialiseren van cacheschijf met de volgende parameters. Drie werkthreads gebruiken voor het doelvolume en een wachtrijdiepte van 128. Stel de duur 'Uitvoeringstijd' van de test te 2hrs op het tabblad 'Testen instellen'.

   | Scenario | Doelvolume | Naam | Duur |
   | --- | --- | --- | --- |
   | Cacheschijf initialiseren |CacheReads |RandomWrites\_1 MB |2hrs |
1. Voer de test Iometer voor het opwarmen van cacheschijf met de volgende parameters. Drie werkthreads gebruiken voor het doelvolume en een wachtrijdiepte van 128. Stel de duur 'Uitvoeringstijd' van de test te 2hrs op het tabblad 'Testen instellen'.

   | Scenario | Doelvolume | Naam | Duur |
   | --- | --- | --- | --- |
   | Warme van Cache-schijf |CacheReads |RandomReads\_1 MB |2hrs |

Nadat de cacheschijf is opgewarmd, gaat u verder met de test-scenario's die hieronder worden vermeld. Als u wilt de Iometer test uitvoert, gebruikt u ten minste drie werkthreads voor **elke** volume als doel. Voor elke werkthread, selecteert u het doelvolume, wachtrijdiepte ingesteld en selecteert u een van de specificaties opgeslagen test, zoals wordt weergegeven in de onderstaande tabel, om uit te voeren van het bijbehorende Testscenario. De tabel toont ook verwachte resultaten voor IOPS en doorvoer bij het uitvoeren van deze tests. Voor alle scenario's, wordt een kleine i/o-grootte van 8KB en een hoge wachtrijdiepte van 128 gebruikt.

| Testscenario | Doelvolume | Naam | Resultaat |
| --- | --- | --- | --- |
| Met maximaal Gelezen IOP 's |CacheReads |RandomWrites\_8 kB |MAAR LIEFST 50.000 IOPS |
| Met maximaal IOPS-schrijfbewerkingen |NoCacheWrites |RandomReads\_8 kB |MAAR LIEFST 64.000 IOPS |
| Met maximaal Gecombineerde IOPS |CacheReads |RandomWrites\_8 kB |100.000 IOPS |
| NoCacheWrites |RandomReads\_8 kB | &nbsp; | &nbsp; |
| Met maximaal MB per seconde gelezen |CacheReads |RandomWrites\_64 kB |524 MB per seconde |
| Met maximaal MB per seconde schrijven |NoCacheWrites |RandomReads\_64 kB |524 MB per seconde |
| Gecombineerde MB per seconde |CacheReads |RandomWrites\_64 kB |1000 MB per seconde |
| NoCacheWrites |RandomReads\_64 kB | &nbsp; | &nbsp; |

Hieronder ziet u schermafbeeldingen van de Iometer testresultaten voor gecombineerde scenario's voor IOPS en doorvoer.

*Gecombineerde lees- en schrijfbewerkingen maximale IOPS*  
![](media/premium-storage-performance/image9.png)

*Gecombineerde lees- en schrijfbewerkingen maximale doorvoer*  
![](media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO is een populair hulpprogramma naar een benchmark-opslag op de virtuele Linux-machines. Deze heeft de flexibiliteit om te selecteren van verschillende i/o-grootte, sequentiële of willekeurige leest en schrijft. Deze gestart werkthreads of processen de opgegeven i/o-bewerkingen uit te voeren. U kunt het type van de i/o-bewerkingen die elke werkthread moet uitvoeren met behulp van de taakbestanden opgeven. We één taakbestand per scenario is geïllustreerd in de onderstaande voorbeelden hebt gemaakt. U kunt de specificaties van deze taakbestanden benchmark-test uitvoeren verschillende werkbelastingen die worden uitgevoerd op Premium Storage kunt wijzigen. In de voorbeelden gebruiken we een Standard DS-14 VM die wordt uitgevoerd **Ubuntu**. Gebruik dezelfde instellingen die worden beschreven in het begin van de [Benchmarking sectie](#Benchmarking) en warme van de cache voordat u de benchmarking tests uitvoert.

Voordat u begint, [FIO downloaden](https://github.com/axboe/fio) en installeer deze op uw virtuele machine.

Voer de volgende opdracht uit voor Ubuntu,

```
apt-get install fio
```

We vier werkthreads voor het aansturen van schrijfbewerkingen en vier werkthreads gebruiken voor sparen leesbewerkingen op de schijven. De werknemers schrijven wordt verkeer over verkeer op het volume "nocache", die 10-schijven met cache ingesteld op 'None'. De werknemers lezen wordt verkeer verkeer op het volume "readcache", die 1 schijf met de cache is ingesteld op 'Alleen-lezen is'.

*Maximale IOPS voor schrijven*  
De taakbestand maken met de volgende specificaties om op te halen van de maximale IOPS voor schrijven. Geef het de naam 'fiowrite.ini'.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Houd er rekening mee de volgen belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties besproken. Deze specificaties zijn essentieel om het maximum aantal IOP's, te stimuleren  

* Een hoge wachtrijdiepte van 256.  
* Een kleine blokgrootte van 8KB.  
* Meerdere threads uitvoeren van willekeurige schrijfbewerkingen.

De volgende opdracht om te worden gehouden, trappen af de FIO test gedurende 30 seconden  

```
sudo fio --runtime 30 fiowrite.ini
```

Tijdens de test wordt uitgevoerd, zich kunt u kan het aantal schrijven IOP's van de virtuele machine en het leveren van Premium-schijven. Zoals u in het onderstaande voorbeeld, levert de DS14-virtuele machine de maximale IOPS-limiet van maar liefst 50.000 IOPS-schrijven.  
    ![](media/premium-storage-performance/image11.png)

*Maximale gelezen IOP 's*  
De taakbestand maken met de volgende specificaties om op te halen van de maximale IOPS voor lezen. Geef het de naam 'fioread.ini'.

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Houd er rekening mee de volgen belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties besproken. Deze specificaties zijn essentieel om het maximum aantal IOP's, te stimuleren

* Een hoge wachtrijdiepte van 256.  
* Een kleine blokgrootte van 8KB.  
* Meerdere threads uitvoeren van willekeurige schrijfbewerkingen.

De volgende opdracht om te worden gehouden, trappen af de FIO test gedurende 30 seconden

```
sudo fio --runtime 30 fioread.ini
```

Tijdens de test wordt uitgevoerd, zich kunt u kan het aantal gelezen IOP's van de virtuele machine en het leveren van Premium-schijven. Zoals weergegeven in het voorbeeld hieronder ziet, is de DS14-virtuele machine het leveren van meer dan 64.000 IOPS voor lezen. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](media/premium-storage-performance/image12.png)

*Maximale lezen en schrijven IOP 's*  
De taakbestand maken met de volgende specificaties om op te halen maximale gecombineerde lezen en schrijven IOP's. Geef het de naam 'fioreadwrite.ini'.

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Houd er rekening mee de volgen belangrijke dingen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties besproken. Deze specificaties zijn essentieel om het maximum aantal IOP's, te stimuleren

* Een hoge wachtrijdiepte van 128.  
* Een kleine blokgrootte van 4KB.  
* Meerdere threads uitvoeren van willekeurige leest en schrijft.

De volgende opdracht om te worden gehouden, trappen af de FIO test gedurende 30 seconden

```
sudo fio --runtime 30 fioreadwrite.ini
```

Tijdens de test wordt uitgevoerd, kunt u zich kunt zien hoeveel gecombineerde lezen en schrijven IOP's van de virtuele machine en leveren van Premium-schijven. Zoals wordt weergegeven in het onderstaande voorbeeld, de DS14-virtuele machine is het leveren van meer dan 100.000 gecombineerde lezen en schrijven IOP's. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](media/premium-storage-performance/image13.png)

*Maximale gecombineerde doorvoer*  
Als u de maximale gecombineerde lezen en schrijven doorvoer, gebruikt u een groter blok en grote wachtrijdiepte met meerdere threads uitvoeren van lees- en schrijfbewerkingen. U kunt een blokgrootte van 64KB en wachtrijdiepte van 128.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure Premium Storage:

* [Premium Storage: opslag met hoge prestaties voor de werkbelasting van virtuele Azure-machines](../articles/virtual-machines/windows/premium-storage.md)  

Lees de artikelen over aanbevolen procedures voor prestaties voor SQL Server voor SQL Server-gebruikers:

* [Aanbevolen procedures voor prestaties voor SQL Server in Azure Virtual Machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium Storage biedt de beste prestaties voor SQL Server in virtuele Azure-machine](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)

# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium-opslag: Ontwerp voor hoge prestaties
## <a name="overview"></a>Overzicht
In dit artikel bevat richtlijnen voor het bouwen van krachtige toepassingen met behulp van Azure Premium-opslag. U kunt de instructies in dit document gecombineerd met aanbevolen procedures voor prestaties van toepassing op de technologieën die worden gebruikt door uw toepassing gebruiken. Ter illustratie van de richtlijnen hebben we gebruikt SQL Server wordt uitgevoerd op de Premium-opslag als voorbeeld in dit hele document.

Hoewel we prestaties scenario's voor de Storage-laag in dit artikel, moet u de toepassingslaag te optimaliseren. Bijvoorbeeld als u een SharePoint-Farm op Azure Premium-opslag host, kunt u de SQL Server-voorbeelden uit dit artikel om te optimaliseren van de database-server. Bovendien optimaliseren van de webserver en de toepassingsserver ophalen van de meeste prestaties van de SharePoint-Farm.

Dit artikel helpt antwoord volgen Veelgestelde vragen over het optimaliseren van de toepassingsprestaties op Azure Premium-opslag

* Hoe kan ik voor het meten van de toepassingsprestaties van uw?  
* Waarom zijn verwachte hoge prestaties niet zien?  
* Welke factoren van invloed op de toepassingsprestaties van uw op Premium-opslag?  
* Hoe deze factoren van invloed op prestaties van uw toepassing op Premium-opslag?  
* Hoe kunt u optimaliseren voor IOPS, bandbreedte en latentie?  

We hebben deze richtlijnen die specifiek voor Premium-opslag omdat werkbelasting op Premium-opslag zeer gevoelige prestaties zijn. We hebben voorbeelden verstrekt, indien van toepassing. U kunt sommige van deze richtlijnen ook toepassen op toepassingen die worden uitgevoerd op IaaS VM's met Standard-opslag-schijven.

Voordat u begint, als u niet bekend met Premium-opslag bent, lees eerst de [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../articles/virtual-machines/windows/premium-storage.md) en [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md)artikelen.

## <a name="application-performance-indicators"></a>Prestatie-indicatoren van toepassing
We controleren of een toepassing wordt uitgevoerd met behulp van prestatie-indicatoren, zoals goed of niet, hoe snel een gebruikersaanvraag, hoeveel gegevens van een toepassing per aanvraag verwerken wordt verwerkt door een toepassing, hoeveel aanvragen verwerken van een toepassing is in een specifieke periode, hoe lang die een gebruiker moet worden gewacht met het ophalen van een antwoord nadat hun aanvraag heeft. De technische voorwaarden voor deze prestatie-indicatoren zijn IOPS, doorvoer of bandbreedte en latentie.

In deze sectie bespreken we de algemene prestatie-indicatoren in de context van Premium-opslag. In de volgende sectie toepassingsvereisten verzamelen, leert u hoe u deze prestatie-indicatoren voor uw toepassing te meten. Verderop in de prestaties te optimaliseren leert u over de factoren die invloed hebben op deze prestatie-indicatoren en aanbevelingen voor het optimaliseren van deze.

## <a name="iops"></a>IOPS
IOPS getal is van aanvragen die uw toepassing worden verzonden naar de opslagschijven in één seconde. Een i/o-bewerking kan worden gelezen of opeenvolgende of willekeurig schrijven. OLTP-toepassingen zoals een online retail-website moeten onmiddellijk veel gelijktijdige aanvragen verwerkt. Verzoek van de gebruiker zich invoegen en bijwerken van intensieve databasetransacties die de toepassing snel moet verwerken. Daarom vereisen OLTP-toepassingen zeer hoge IOPS. Dergelijke toepassingen miljoenen kleine en willekeurige i/o-aanvragen worden verwerkt. Als u een dergelijke toepassing hebt, moet u de infrastructuur van de toepassing om te optimaliseren voor IOPS ontwerpen. In de volgende sectie *toepassingsprestaties optimaliseren*, bespreken we in detail de factoren die u overwegen moet om op te halen van hoge IOPS.

Wanneer u een premium opslagschijf koppelen met uw grootschalige virtuele machine, Azure voorziet u een gegarandeerd aantal IOPS volgens de specificatie van de schijf. Een schijf P50 levert bijvoorbeeld 7500 IOPS. Elke grote schaal VM-grootte heeft ook een specifieke IOPS limiet die deze kan tolereren. Zo heeft een standaard GS5 VM 80.000 IOP's beperken.

## <a name="throughput"></a>Doorvoer
Doorvoer- of bandbreedte is de hoeveelheid gegevens die uw toepassing worden verzonden naar de opslagschijven in een opgegeven interval. Als uw toepassing met het uitvoeren van de i/o-bewerkingen met grote i/o-eenheid grootten, is er een hoge doorvoer vereist. Datawarehouse-toepassingen vaak uitgeven scan bewerkingsintensieve bewerkingen die toegang tot grote delen van gegevens op een tijdstip en meest bulkbewerkingen worden uitgevoerd. Met andere woorden, vereisen dergelijke toepassingen hogere doorvoer. Als u een dergelijke toepassing hebt, moet u de infrastructuur te optimaliseren voor doorvoer ontwerpen. In de volgende sectie besproken in detail de factoren die u moet afstemmen om dit te bereiken.

Wanneer u een premium-opslagschijf koppelen met een grootschalige virtuele machine, Azure bepalingen doorvoer volgens de specificatie van die schijf. Een schijf P50 levert bijvoorbeeld 250 MB per tweede schijf doorvoer. Elke grote schaal VM-grootte heeft ook als specifieke doorvoer limiet die deze kan tolereren. Standaard GS5 VM heeft bijvoorbeeld een maximale doorvoer van 2.000 MB per seconde. 

Er is een relatie tussen de doorvoer en IOP's, zoals wordt weergegeven in de onderstaande formule.

![](media/premium-storage-performance/image1.png)

Het is daarom belangrijk om te bepalen van de optimale doorvoer en IOPS-waarden die vereist zijn voor uw toepassing. Als u een optimaliseren probeert, wordt de andere ook beïnvloed. In een volgende sectie *toepassingsprestaties optimaliseren*, behandeld in meer informatie over het optimaliseren van IOPS en Doorvoerlimieten.

## <a name="latency"></a>Latentie
Latentie is de tijd die een toepassing op een enkele aanvraag ontvangen, verzenden naar de opslagschijven en verzenden van het antwoord op de client nodig. Dit is een kritieke meting van de prestaties van een toepassing naast IOPS en Doorvoerlimieten. De latentie van de schijf voor een premium-opslag is de tijd die nodig is voor het ophalen van de gegevens voor een aanvraag en weer aan uw toepassing communiceren. Premium-opslag biedt een consistente lage latenties. Als u alleen-lezen-hostcaching op schijven met opslagruimte premium inschakelt, kunt u veel lagere latentie mogelijk lezen krijgen. Bespreken we schijfcache in de volgende sectie uitvoeriger op *toepassingsprestaties optimaliseren*.

Wanneer u uw toepassing hoger IOPS en Doorvoerlimieten optimaliseert, zal dit invloed heeft op de latentie van uw toepassing. Na het afstemmen van de toepassingsprestaties, moet u altijd de latentie van de toepassing om te voorkomen dat hoge latentie onverwacht gedrag geëvalueerd.

## <a name="gather-application-performance-requirements"></a>Verzamelen van prestaties toepassingsvereisten
De eerste stap bij het ontwerpen van krachtige toepassingen die worden uitgevoerd op Azure Premium-opslag is de prestatievereisten van uw toepassing te begrijpen. Wanneer u prestatie-eisen verzameld, kunt u uw toepassing naar de meest optimale prestaties te optimaliseren.

In de vorige sectie we de algemene prestatie-indicatoren, IOPS, Throughput and Latency uitgelegd. U moet vaststellen welke van deze prestatie-indicatoren essentieel zijn voor uw toepassing om de gewenste gebruikerservaring te bieden. Bijvoorbeeld: hoge IOPS belangrijkste miljoenen transacties worden verwerkt in een tweede OLTP-toepassingen. Dat hoge doorvoersnelheid essentieel voor de verwerking van grote hoeveelheden gegevens in een tweede Data Warehouse-toepassingen is. Zeer lage latentie is van cruciaal belang voor realtime toepassingen zoals live videostreaming websites.

Vervolgens wordt de maximale prestatie-eisen van uw toepassing tijdens de levensduur meten. Gebruik de controlelijst voorbeeld hieronder als een begin. Noteer de maximale prestatie-eisen tijdens normaal, de piek- en rustige uren werkbelasting perioden. Door het identificeren van de vereisten voor alle werkbelastingen niveaus zijn u kunnen bepalen van de algehele prestaties behoefte van uw toepassing. De normale werkbelasting van een webwinkel wordt bijvoorbeeld de transacties die het meeste dagen in een jaar fungeert. De piek-werkbelasting van de website worden de transacties die het tijdens de feestdagen of speciale verkoop gebeurtenissen fungeert. De piek-werkbelasting is doorgaans ervaren voor een beperkte periode, maar kan uw toepassing schalen van twee of meer keer de normale werking vereisen. Ontdek de 50 percentiel, 90 percentiel en 99 percentiel vereisten. Dit helpt uitfilteren eventuele uitschieters in de prestatie-eisen en kunt u uw inspanningen zich richten op optimaliseren voor de juiste waarden.

**Controlelijst voor de vereisten van de toepassingsprestaties**

| **Prestatie-eisen** | **50 percentiel** | **90 percentiel** | **99 percentiel** |
| --- | --- | --- | --- |
| Met maximaal Transacties per seconde | | | |
| % Leesbewerkingen | | | |
| % Schrijfbewerkingen | | | |
| % Willekeurige bewerkingen | | | |
| % Opeenvolgende bewerkingen | | | |
| De grootte van de i/o-aanvraag | | | |
| Gemiddelde doorvoersnelheid | | | |
| Met maximaal Doorvoer | | | |
| Min. Latentie | | | |
| Gemiddelde latentie | | | |
| Met maximaal CPU | | | |
| Gemiddelde CPU | | | |
| Met maximaal Geheugen | | | |
| Gemiddelde geheugen | | | |
| Wachtrijdiepte | | | |

> [!NOTE]
> U moet overwegen deze getallen op basis van de verwachte toekomstige groei van uw toepassing schalen. Er is een goed idee om te plannen voor groei tevoren, omdat deze mogelijk moeilijker te wijzigen van de infrastructuur voor het verbeteren van de prestaties later.
>
>

Als u een bestaande toepassing hebt en wilt overstappen op Premium-opslag, moet u eerst de controlelijst hierboven voor de bestaande toepassing bouwen. Vervolgens bouwen van een prototype van uw toepassing op Premium-opslag en ontwerpen van de toepassing op basis van de richtlijnen in *toepassingsprestaties optimaliseren* in verderop in dit document. De volgende sectie beschrijft de hulpmiddelen die u gebruiken kunt voor het verzamelen van de metingen van de prestaties.

Maak een controlelijst vergelijkbaar is met uw bestaande toepassing voor het model. Met Benchmarking hulpprogramma's kunt u de werkbelastingen te simuleren en prestaties van de toepassing prototype meten. Zie de sectie over [Benchmarking](#benchmarking) voor meer informatie. Hierdoor kunt u bepalen of Premium-opslag kan overeenkomen met of groter zijn dan de prestatie-eisen van uw toepassing. Vervolgens kunt u dezelfde richtlijnen voor de productietoepassing implementeren.

### <a name="counters-to-measure-application-performance-requirements"></a>Tellers voor het meten van de prestaties van toepassingsvereisten
De beste manier om te meten prestatie-eisen van uw toepassing is met bewaking van toepassingsprestaties hulpmiddelen die worden geleverd door het besturingssysteem van de server. U kunt PerfMon voor Windows- en iostat voor Linux. Deze hulpprogramma's vastleggen items die overeenkomen met elke meting uitgelegd in de bovenstaande sectie. Wanneer uw toepassing wordt uitgevoerd, zijn normaal, de piek- en rustige uren werkbelastingen, moet u de waarden van deze prestatiemeteritems vastleggen.

De prestatiemeteritems zijn beschikbaar voor de processor, geheugen, en elke logische schijf en de fysieke schijf van uw server. Als u premium-opslag-schijven met een virtuele machine, het item fysieke schijf zijn voor elke schijf premium-opslag en de prestatiemeteritems voor logische schijf voor elk volume dat is gemaakt op de premium-opslag-schijven zijn. U moet de waarden voor de schijven die als host fungeren van uw toepassing werklast vastleggen. Als er een één-op-een-toewijzing tussen logische en fysieke schijven, kunt u verwijzen naar de fysieke schijf tellers; Raadpleeg anders de prestatiemeteritems voor logische schijf. Op Linux genereert de opdracht iostat een CPU- en schijfresources utilization-rapport. Het disk utilization-rapport voorziet in statistieken per fysieke apparaat of partitie. Als u een databaseserver met de gegevens en logboekbestanden op afzonderlijke schijven hebt, kunt u deze gegevens voor beide schijven verzamelen. Onderstaande tabel worden beschreven tellers voor schijven, processor en geheugen:

| Prestatiemeteritems | Beschrijving | PerfMon | Iostat |
| --- | --- | --- | --- |
| **IOPS of transacties per seconde** |Het aantal i/o-aanvragen die zijn uitgegeven aan de opslagschijf per seconde. |Schijf lezen per seconde <br> Schijf schrijven per seconde |tps <br> r/s <br> w/s |
| **Schijf lees- en schrijfbewerkingen** |% van de lees- en schrijfbewerkingen uitgevoerd op de schijf. |Percentage schijftijd voor lezen <br> Percentage schijftijd voor schrijven |r/s <br> w/s |
| **Doorvoer** |De hoeveelheid gegevens lezen uit of schrijven naar de schijf per seconde. |Gelezen Bytes per seconde <br> Geschreven Bytes per seconde |kB_read/s <br> kB_wrtn/s |
| **Latentie** |Totale tijd om een schijf i/o-aanvraag te voltooien. |Gemiddelde leestijd voor schijf <br> Gemiddelde fysieke schijf |await <br> svctm |
| **I/o-grootte** |De grootte van i/o-problemen naar de opslagschijven aanvragen. |Gemiddeld aantal gelezen Bytes <br> Gemiddelde aantal geschreven Bytes |avgrq sz |
| **Wachtrijdiepte** |Het aantal openstaande i/o-aanvragen in afwachting van formulier lezen of schrijven naar de opslagschijf. |Huidige wachtrijlengte voor schijf |avgqu sz |
| **Max. Geheugen** |Vereiste toepassing probleemloos hoeveelheid geheugen |Percentage toegewezen Bytes in gebruik |Vmstat gebruiken |
| **Max. CPU** |De hoeveelheid CPU probleemloos toepassing vereist |Percentage processortijd |% util |

Meer informatie over [iostat](http://linuxcommand.org/man_pages/iostat1.html) en [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Optimaliseert de prestaties van toepassingen
De belangrijkste factoren die van invloed op prestaties van een toepassing die wordt uitgevoerd op de Premium-opslag zijn aard van i/o-aanvragen, VM-grootte, de grootte van de schijf, het aantal schijven, schijfcache, Multithreading en wachtrijdiepte. U kunt sommige van deze factoren beheren met de knoppen geleverd door het systeem. De meeste toepassingen mogelijk niet bieden u een optie voor het wijzigen van de i/o-grootte en de wachtrijdiepte rechtstreeks. Bijvoorbeeld, als u SQL Server gebruikt, kiezen u niet de i/o-grootte en wachtrij diepte. SQL Server kiest de optimale IO wachtrij diepte waarden voor grootte en om de meeste prestaties te behalen. Het is belangrijk om te begrijpen van de gevolgen van beide typen factoren voor de toepassingsprestaties van uw zodat u de juiste resources om te voldoen aan de prestatiebehoeften kunt inrichten.

In deze sectie, raadpleegt u de toepassing controlelijst voor de vereisten die u hebt gemaakt, om te identificeren hoeveel u nodig om de toepassingsprestaties van uw te optimaliseren. Op basis van die, kunt u zich kunt bepalen welke factoren in deze sectie u nodig om af te stemmen. Te wonen de gevolgen van elke factor voor de toepassingsprestaties van uw, moet u benchmarking's uitvoeren voor de installatie van uw toepassing. Raadpleeg de [Benchmarking](#Benchmarking) sectie aan het einde van dit artikel voor stappen voor het uitvoeren van algemene benchmarking's voor Windows en Linux-machines.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>IOPS, Throughput and Latency optimaliseren in één oogopslag
De onderstaande tabel bevat een overzicht van alle prestatiefactoren en de stappen voor het optimaliseren van IOPS, doorvoer en latentie. Elk bevat een beschrijving van de secties na dit overzicht factor is veel meer diepte.

| &nbsp; | **IOPS** | **Doorvoer** | **Latentie** |
| --- | --- | --- | --- |
| **Voorbeeldscenario 's** |Enterprise OLTP-toepassing waarvoor zeer hoge transacties per tweede snelheid. |Enterprise gegevensopslag toepassing verwerking van grote hoeveelheden gegevens. |Bijna realtime toepassingen vereisen directe antwoorden op aanvragen van gebruikers, zoals online gaming. |
| Prestatiefactoren | &nbsp; | &nbsp; | &nbsp; |
| **I/o-grootte** |I/o-kleinere levert hogere IOPS. |I/o-groter naar levert hogere doorvoer. | &nbsp;|
| **VM-grootte** |Gebruik een VM-grootte die groter zijn dan de vereisten voor toepassingsimplementatie IOPS biedt. Zie VM-grootten en hun IOPS-limieten. |Gebruik een VM-grootte met doorvoer limiet groter is dan de vereisten van uw toepassing. VM-grootten en hun doorvoerlimieten hier zien. |Gebruik een VM-grootte of aanbiedingen limieten groter is dan de vereiste toepassing schalen. VM-grootten en hun limieten hier zien. |
| **Grootte van de schijf** |Gebruik een grootte van de schijf die groter zijn dan de vereisten voor toepassingsimplementatie IOPS biedt. Zie schijfgrootten en hun IOPS-limieten. |De grootte van een schijf met doorvoer limiet groter is dan de vereisten van uw toepassing gebruiken. Zie schijfgrootten en hun doorvoerlimieten hier. |Gebruik een grootte van de schijf dat aanbiedingen limieten groter is dan de vereiste toepassing schalen. Zie schijfgrootten en hier hun limieten. |
| **Virtuele machine en Schaallimieten voor schijf** |IOPS-limiet van de VM-grootte gekozen moet groter zijn dan totale IOP's aangedreven door premium-opslag-schijven is gekoppeld. |Limiet van de doorvoer van de VM-grootte gekozen moet groter zijn dan de totale doorvoer van premium storage schijven zijn gekoppeld aan deze basis. |Schaallimieten van de VM-grootte gekozen moet groter zijn dan de totale schaallimieten van gekoppelde premium-opslag-schijven. |
| **Schijfcache** |ReadOnly-Cache inschakelen op schijven met premium-opslag met zware leesbewerkingen hoger lezen IOPS ophalen. | &nbsp; |ReadOnly-Cache inschakelen op schijven met premium-opslag met gereed zware bewerkingen ophalen lezen zeer lage latenties. |
| **Striping van de schijf** |Gebruik van meerdere schijven en ze samen een gecombineerde hogere limiet voor IOPS en Doorvoerlimieten stripe. Houd er rekening mee dat de gecombineerde limiet is per virtuele machine hoger zijn dan de gecombineerde limieten van de gekoppelde premium-schijven moet. | &nbsp; | &nbsp; |
| **Stripe-grootte** |Kleinere stripe voor willekeurige kleine i/o-patroon gezien in OLTP-toepassingen. Gebruik bijvoorbeeld stripe-grootte van 64KB voor SQL Server OLTP-toepassing. |Grotere stripe voor opeenvolgende grote i/o-patroon gezien in de datawarehouse-toepassingen. Gebruik bijvoorbeeld 256KB stripe-grootte voor SQL Server Data warehouse-toepassing. | &nbsp; |
| **Multithreading** |Gebruik voor de push-hoger aantal aanvragen naar Premium-opslag die tot hogere IOPS en Doorvoerlimieten leiden multithreading. Stel bijvoorbeeld een hoge waarde MAXDOP meer CPU's toewijzen aan SQL Server op SQL Server. | &nbsp; | &nbsp; |
| **Wachtrijdiepte** |Grotere wachtrijdiepte levert hogere IOPS. |Grotere wachtrijdiepte levert hogere doorvoer. |Kleinere wachtrijdiepte levert lagere latenties. |

## <a name="nature-of-io-requests"></a>Aard van i/o-aanvragen
Een i/o-aanvraag is een eenheid van i/o-bewerking die uw toepassing gaan uitvoeren. Identificeren van de aard van i/o-aanvragen, willekeurige of opeenvolgende, kunt lezen of schrijven, klein of groot is, u bepalen van de prestatie-eisen van uw toepassing. Het is belangrijk om te begrijpen wat de aard van i/o-aanvragen, om de juiste beslissingen te nemen bij het ontwerpen van de infrastructuur van uw toepassing.

I/o-grootte is een van de belangrijkste factoren. De i/o-grootte is de grootte van de invoer-/ uitvoerbewerking aanvraag gegenereerd door uw toepassing. De i/o-grootte heeft een aanzienlijke invloed op de prestaties met name op de IOPS en bandbreedte die de toepassing bereiken. De volgende formule toont de relatie tussen IOP's, i/o-grootte en bandbreedte/doorvoer.  
    ![](media/premium-storage-performance/image1.png)

Sommige toepassingen kunnen u voor het wijzigen van hun i/o-grootte, maar sommige toepassingen niet. Bijvoorbeeld, SQL Server bepaalt de optimale grootte voor i/o zelf en biedt geen gebruikers met alle knoppen om deze te wijzigen. Aan de andere kant Oracle biedt een parameter met de naam [DB\_blokkeren\_grootte](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) die u kunt configureren met de grootte van de i/o-aanvraag van de database.

Als u van een toepassing die u gebruikmaakt wilt wijzigen van de i/o-grootte niet toestaat, gebruik de richtlijnen in dit artikel om de KPI's die het meest relevant voor uw toepassing van de prestaties te optimaliseren. Bijvoorbeeld:

* Een OLTP-toepassing genereert miljoenen kleine en willekeurige i/o-aanvragen. Voor het afhandelen van dit type i/o-aanvragen, moet u uw infrastructuur van de toepassing ophalen hogere IOPS ontwerpen.  
* Een datawarehouse-toepassing genereert grote en opeenvolgende i/o-aanvragen. Voor het afhandelen van dit type i/o-aanvragen, moet u de infrastructuur van uw toepassingen ophalen hogere bandbreedte of doorvoer ontwerpen.

Als u van een toepassing, waarmee u de i/o-grootte wijzigen gebruikmaakt, gebruikt u deze vuistregel voor de i/o-grootte naast de andere richtlijnen prestaties

* Kleinere IO hogere IOPS ophalen. Bijvoorbeeld: 8 KB voor een OLTP-toepassing.  
* Grotere IO hogere bandbreedte/doorvoer ophalen. Bijvoorbeeld 1024 KB voor een datawarehouse-toepassing.

Hier volgt een voorbeeld van hoe u de IOPS en doorvoer/bandbreedte voor uw toepassing berekenen kunt. U kunt een toepassing met een schijf P30. De maximale IOPS en een schijf P30 doorvoer/bandbreedte kunt bereiken is 5000 IOP's en 200 MB per seconde respectievelijk. Als uw toepassing vereist dat de maximale IOPS-waarde van de schijf P30 en gebruiken van een kleinere IO zoals 8 KB, heeft de resulterende bandbreedte is het mogelijk om op te halen nu 40 MB per seconde. Als uw toepassing vereist dat de maximale doorvoer/bandbreedte van P30 schijf en gebruiken van een groter IO zoals 1024 KB, de resulterende IOP's zijn echter minder 200 IOPS. Daarom de i/o-grootte instellen zodat deze voldoet aan de vereiste IOPS en doorvoer/bandbreedte van zowel uw toepassing. De volgende tabel geeft een overzicht van de verschillende i/o-grootte en hun bijbehorende IOPS en Doorvoerlimieten voor een schijf P30.

| Vereisten voor toepassingsimplementatie | I/o-grootte | IOPS | Doorvoer/bandbreedte |
| --- | --- | --- | --- |
| Max. IOP 's |8 kB |5,000 |40 MB per seconde |
| Maximale doorvoer |1024 KB |200 |200 MB per seconde |
| Maximale Throughput + hoge IOPS |64 kB |3,200 |200 MB per seconde |
| Maximale IOPS + hoge doorvoersnelheid |32 KB |5,000 |160 MB per seconde |

Als u IOPS en bandbreedte die hoger is dan de maximumwaarde van een schijf één premium-opslag, gebruikt u meerdere premium-schijven striped samen. Bijvoorbeeld, stripe twee P30 schijven als u een gecombineerde IOPS van 10.000 IOPS of een gecombineerde doorvoer van 400 MB per seconde. Zoals wordt beschreven in de volgende sectie, moet u een VM-grootte die ondersteuning biedt voor de gecombineerde IOPS en Doorvoerlimieten schijf.

> [!NOTE]
> Als u verhoogt IOP's of andere verhoogt ook de doorvoer, zorg er dan voor dat u doorvoer of IOPS-limieten van de schijf of virtuele machine niet bereikt wanneer ofwel een verhogen.
>
>

Om de effecten van i/o-grootte op de prestaties van toepassingen schijfwitness, kunt u benchmarking's uitvoeren op de virtuele machine en de schijven. Maak meerdere test wordt uitgevoerd en andere i/o-grootte voor elke run gebruiken om te zien van de impact. Raadpleeg de [Benchmarking](#Benchmarking) sectie aan het einde van dit artikel voor meer informatie.

## <a name="high-scale-vm-sizes"></a>VM-grootten van grote schaal
Wanneer u begint met het ontwerpen van een toepassing, is een van de eerste dingen te doen, kiest u een virtuele machine voor het hosten van uw toepassing. Premium-opslag wordt geleverd met hoge Scale VM-grootten die toepassingen waarbij hogere rekencapaciteit en een hoge lokale schijf i/o-prestaties kunnen worden uitgevoerd. Deze virtuele machines bieden snellere processors, een hogere ratio van geheugen-core- en een Solid-State stations (SSD) voor de lokale schijf. Voorbeelden van hoge Scale VMs ondersteunende Premium-opslag zijn de DS, DSv2 en GS-serie virtuele machines.

Maximale schaal VM's zijn beschikbaar in verschillende grootten met een verschillend aantal CPU-kernen, geheugen, OS en grootte van de tijdelijke schijf. Elk VM-grootte heeft ook het maximum aantal gegevensschijven die u aan de virtuele machine koppelen kunt. Daarom de gekozen VM-grootte van invloed is op hoeveel bewerkingen, geheugen, en de opslagcapaciteit is beschikbaar voor uw toepassing. Dit ook van invloed op de berekening en opslag kosten. Hieronder ziet u bijvoorbeeld de specificaties van de grootste VM-grootte in een DS-serie, DSv2 reeks en een GS-serie:

| VM-grootte | CPU-kernen | Geheugen | Schijfgrootten VM | Met maximaal Gegevensschijven | Cachegrootte | IOPS | Bandbreedtelimieten i/o-Cache |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |OS = 1023 GB <br> Lokale SSD = 224 GB |32 |576 GB |50.000 IOP 'S <br> 512 MB per seconde |4000 IOPS en 33 MB per seconde |
| Standard_GS5 |32 |448 GB |OS = 1023 GB <br> Lokale SSD = 896 GB |64 |4224 GB |80.000 IOP 'S <br> 2.000 MB per seconde |5000 IOP's en 50 MB per seconde |

Als u wilt weergeven van een volledige lijst met alle beschikbare grootten voor virtuele machine in Azure, verwijzen naar [Windows VM-grootten](../articles/virtual-machines/windows/sizes.md) of [Linux VM-grootten](../articles/virtual-machines/linux/sizes.md). Kies een VM-grootte die kan voldoen aan en kan worden uitgebreid naar de gewenste toepassing prestatie-eisen. Hiernaast rekening volgen enkele belangrijke overwegingen bij het kiezen van de VM-grootten.

*Schaallimieten*  
De maximale IOPS-limieten per virtuele machine en per schijf zijn verschillende en onafhankelijk van elkaar. Zorg ervoor dat de toepassing IOP's binnen de grenzen van de virtuele machine, evenals de premium-schijven zijn gekoppeld aan deze wordt bestuurd. Anders merken toepassingsprestaties beperking.

Stel bijvoorbeeld dat een vereiste toepassing is maximaal 4000 IOPS. Om dit te bereiken, moet u een schijf P30 op een VM DS1 inrichten. De schijf P30 kan maximaal 5000 IOP's bieden. De VM DS1 is echter beperkt tot 3200 IOPS. Als gevolg daarvan kan de toepassingsprestaties zal worden beperkt door de VM-grens bij 3200 IOPS en zullen er verminderde prestaties. Om dit te voorkomen, kiest u een schijf en VM-grootte die wordt zowel voldoen aan de vereisten van de toepassing.

*Kosten van bewerking*  
In veel gevallen is het mogelijk dat de totale kosten van bewerking u Premium-opslag is lager dan het gebruik van de Standard-opslag.

Neem bijvoorbeeld een toepassing 16.000 IOPS vereisen. Als u wilt deze prestaties bereiken, moet u een standaard\_D14 Azure IaaS VM, waardoor een maximumaantal IOPS op 16.000 met 32 standaard opslagschijven voor 1 TB. Elke schijf van 1TB standard-opslag kunt maximaal 500 IOPS bereiken. De geschatte kosten van deze virtuele machine per maand worden $1,570. De maandelijkse kosten van 32 standaardopslag schijven worden $1,638. De geschatte kosten van de totale maandelijkse worden $3,208.

Echter, als u dezelfde toepassing op Premium-opslag gehost, moet u een kleinere VM en minder schijven voor premium-opslag, waardoor de totale kosten. Een standaard\_DS13 VM kunnen voldoen aan de 16.000 IOPS vereiste met vier P30 schijven. De VM DS13 heeft een maximumaantal IOPS op 25,600 en elke schijf P30 heeft een maximumaantal IOPS op 5.000. Over het algemeen deze configuratie 5.000 x 4 = 20.000 kunt bereiken IOPS. De geschatte kosten van deze virtuele machine per maand worden $1,003. De maandelijkse kosten van vier P30 premium-opslag-schijven worden $544.34. De geschatte kosten van de totale maandelijkse worden $1,544.

De volgende tabel geeft een overzicht van de verdeling van de kosten van dit scenario voor Standard en Premium-opslag.

| &nbsp; | **Standard** | **Premium** |
| --- | --- | --- |
| **Kosten van de virtuele machine per maand** |$1,570.58 (standaard\_D14) |$1,003.66 (standaard\_DS13) |
| **Kost van schijven per maand** |1,638.40 (32 x 1 TB schijven) |544.34 (4 x P30 schijven) |
| **Totale kosten per maand** |$3,208.98 |$1,544.34 |

*Linux-distributies*  

Met Azure Premium-opslag krijgt u hetzelfde niveau van de prestaties voor virtuele machines met Windows en Linux. We bieden ondersteuning voor veel versies van Linux-distributies en ziet u de volledige lijst [hier](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het is belangrijk te weten dat verschillende distributies beter zijn voor verschillende typen werkbelastingen geschikt. U ziet, afhankelijk van de distro die uw werkbelasting wordt uitgevoerd op verschillende prestatieniveaus. De Linux-distributies met uw toepassing testen en kiezen die het beste werkt.

Wanneer waarop Linux wordt uitgevoerd met Premium-opslag, controleert u de meest recente updates over vereiste stuurprogramma's voor hoge prestaties te garanderen.

## <a name="premium-storage-disk-sizes"></a>Schijfgrootte voor Premium-opslag
Azure Premium-opslag biedt momenteel zeven schijfgrootten. De grootte van elke schijf heeft de limiet van een andere schaal voor IOPS, bandbreedte en opslag. Kies de juiste grootte van de schijf voor Premium-opslag, afhankelijk van de toepassingsvereisten en de grote schaal VM-grootte. De volgende tabel toont de grootten zeven schijven en hun mogelijkheden. P4 en P6 grootten zijn momenteel alleen ondersteund voor schijven die worden beheerd.

| Premium-schijven Type  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 32 GB | 64 GB | 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde | 


Hoeveel schijven die u kiest is afhankelijk van de schijf het formaat van door u gekozen. U kunt één P50 schijf of meerdere P10 schijven om te voldoen aan uw vereisten voor toepassingsimplementatie. Aandachtspunten voor gebruikersaccounts hieronder bij het maken van de keuze rekening.

*Schaallimieten (IOPS en Doorvoerlimieten)*  
De limieten voor IOPS en Doorvoerlimieten van elke Premium-schijfgrootte is verschillende en onafhankelijk van de schaallimieten voor virtuele machine. Zorg ervoor dat het totale aantal IOPS en Doorvoerlimieten van de schijven zich binnen de grenzen van de schaal van de gekozen VM-grootte.

Bijvoorbeeld als een vereiste toepassing maximaal 250 MB per seconde doorvoer is en u gebruikmaakt van een VM DS4 met één P30 schijf. De VM DS4 kunt geven tot 256 MB per seconde doorvoer. Een enkele P30 schijf heeft echter doorvoer limiet van 200 MB per seconde. Als gevolg daarvan wordt de toepassing worden beperkt op 200 MB per seconde als gevolg van de limiet voor de schijf. Om deze limiet is, omzeilen inrichten meer dan één gegevensschijven naar de virtuele machine of het formaat van uw schijven tot p 40 of P50.

> [!NOTE]
> Leesbewerkingen geleverd door de cache worden niet opgenomen in de schijf-IOPS en doorvoer, daarom niet onderworpen aan beperkingen van de schijf. Cache heeft de afzonderlijke IOPS en Doorvoerlimieten bereikt per VM.
>
> In eerste instantie zijn lees- en schrijfbewerkingen bijvoorbeeld 60MB per seconde en 40MB per seconde respectievelijk. Na verloop van tijd uit de cache van warms en meer en meer van de gelezen uit de cache fungeert. Vervolgens kunt u hogere schrijven doorvoer ophalen van de schijf.
>
>

*Aantal schijven*  
Bepaal het aantal schijven die u moet door toepassingsvereisten vast te stellen. Elk VM-grootte heeft ook een limiet van het aantal schijven die u aan de virtuele machine koppelen kunt. Dit is meestal twee keer het aantal kernen. Zorg ervoor dat de VM-grootte die u kiest u het aantal schijven nodig kan ondersteunen.

Vergeet niet de Premium-opslag-schijven hebben hogere prestaties levert vergeleken met de Standard-opslag-schijven. Dus als u uw toepassing van Azure IaaS virtuele machine met behulp van de Standard-opslag naar Premium-opslag migreert, moet waarschijnlijk u de dezelfde of een hogere prestaties voor uw toepassing bereiken minder premium-schijven.

## <a name="disk-caching"></a>Schijfcache
Maximale schaal virtuele machines die gebruikmaken van Azure Premium-opslag hebben een meerlaagse caching-technologie BlobCache aangeroepen. BlobCache gebruikt een combinatie van de virtuele Machine RAM-geheugen en de lokale SSD voor opslaan in cache. Deze cache is beschikbaar voor de Premium-opslag permanente schijven en de VM lokale schijven. Deze cache-instelling is standaard tot lezen/schrijven voor OS-schijven en alleen-lezen voor gegevensschijven gehost op Premium-opslag. Met de schijfcache is ingeschakeld op de schijven Premium-opslag, kunt de grootschalige virtuele machines zeer hoge prestatieniveaus die groter is dan de onderliggende schijfprestaties bereiken.

> [!WARNING]
> Het wijzigen van de cache-instelling van een Azure-schijf wordt losgekoppeld en opnieuw toegevoegd de doelschijf is. Als de besturingssysteemschijf is, wordt de virtuele machine opnieuw gestart. Stop alle toepassingen en services die kunnen worden beïnvloed door deze onderbreking voordat u de schijfcache-instelling wijzigt.
>
>

Raadpleeg voor meer informatie over de werking van BlobCache binnen [Azure Premium-opslag](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogbericht.

Het is belangrijk om in te schakelen van cache op de juiste set met schijven. Of u moet inschakelen schijfcache op een schijf premium of niet hangen af van het patroon van de werkbelasting die schijf verwerkt. De volgende tabel ziet u de cache-instellingen voor het besturingssysteem en gegevensschijven.

| **Schijftype** | **Standaardinstelling voor Cache** |
| --- | --- |
| Besturingssysteemschijf |ReadWrite |
| Gegevensschijf |Geen |

Hieronder vindt u de aangeraden schijfruimte cache-instellingen voor gegevensschijven,

| **Instelling van de schijfcache** | **Aanbeveling op wanneer deze instelling te gebruiken** |
| --- | --- |
| Geen |Host-cache configureren als geen voor de alleen-schrijven en schrijven zware schijven. |
| Alleen-lezen |Host-cache configureren als alleen-lezen voor alleen-lezen en alleen-lezen-schijven. |
| ReadWrite |Host-cache configureren als ReadWrite alleen als uw toepassing op correcte wijze schrijven van gegevens in de cache met permanente schijven wanneer deze nodig is. |

*Alleen-lezen*  
U kunt door ReadOnly cachegebruik op Premium-opslag gegevens schijven te configureren, lage latentie voor lezen bereiken en zeer hoge lezen IOPS en Doorvoerlimieten ophalen voor uw toepassing. Dit is vanwege twee redenen

1. Leesbewerkingen vanuit cache, die zich op de lokale SSD en het geheugen van de virtuele machine wordt uitgevoerd, zijn veel sneller dan leesbewerkingen van de gegevensschijf op Azure blob storage.  
2. Premium-opslag gelezen gegevens geleverd vanuit cache, naar de schijf-IOPS en de doorvoer niet worden geteld. Uw toepassing is daarom hogere totale IOP's en de doorvoer te bereiken.

*ReadWrite*  
De OS-schijven hebben standaard ReadWrite cachebewerkingen ingeschakeld. Ondersteuning voor cachegebruik op gegevens ook schijven ReadWrite onlangs toegevoegd. Als u van ReadWrite in cache opslaan gebruikmaakt, moet u een goede manier om de gegevens uit cache schrijven naar permanente schijven hebben. SQL Server verwerkt bijvoorbeeld gegevens in de cache schrijven naar de permanente opslagschijven op zichzelf. ReadWrite cache gebruiken met een toepassing die behouden blijven van de vereiste gegevens niet verwerkt, kan leiden tot verlies van gegevens als de virtuele machine is vastgelopen.

Als u bijvoorbeeld kunt u deze richtlijnen toepassen op de SQL Server wordt uitgevoerd op de Premium-opslag door het volgende te doen

1. 'ReadOnly' cache configureren op de premium-opslag-schijven die als host fungeert voor gegevensbestanden.  
   a.  De fast leest uit cache lager de SQL Server-Querytijd omdat data-pagina's veel sneller worden opgehaald uit de cache in vergelijking met rechtstreeks vanuit de gegevens schijven.  
   b.  Leesbewerkingen uit cache verstrekken, betekent dat er is meer doorvoer van gegevensschijven premium beschikbaar. SQL Server kan gebruikmaken van deze extra doorvoer naar meer data-pagina's en andere bewerkingen zoals back-up/herstel ophalen, batch-belastingen en index wordt opnieuw gemaakt.  
2. Configureer 'None' in de cache op premium-opslag-schijven die als host fungeert voor de logboekbestanden.  
   a.  Logboekbestanden hebben voornamelijk schrijven zware bewerkingen. Ze kunnen daarom niet gebruikmaken van de cache van het kenmerk alleen-lezen.

## <a name="disk-striping"></a>Striping van de schijf
Wanneer een grootschalige die virtuele machine is gekoppeld met verschillende premium permanente opslagschijven, kunnen u de schijven samen striped voor het samenvoegen van hun IOP's, bandbreedte en opslagcapaciteit.

In Windows, kunt u opslagruimten naar stripe schijven samen. U moet één kolom voor elke schijf in een pool configureren. Anders kan de algehele prestaties van striped volumes niet lager zijn dan verwacht vanwege ongelijke verdeling van het verkeer op de schijven.

Belangrijk: Met Server Manager UI, kunt u het totale aantal kolommen maximaal 8 voor striped volumes instellen. Bij het toevoegen van meer dan 8 schijven kunt u PowerShell gebruiken om het volume te maken. Met PowerShell, kunt u instellen het aantal kolommen gelijk zijn aan het aantal schijven. Bijvoorbeeld, als er 16 schijven in een stripeset één; Geef 16 kolommen in de *NumberOfColumns* parameter van de *New-VirtualDisk* PowerShell-cmdlet.

Op Linux, het hulpprogramma MDADM stripe schijven niet gezamenlijk gebruiken. Voor gedetailleerde stappen voor het striping schijven op Linux naar verwijzen [Software-RAID configureren op Linux](../articles/virtual-machines/linux/configure-raid.md).

*Stripe-grootte*  
Een belangrijke configuratie in striping van de schijf wordt de stripe-grootte. De stripe-grootte of blokgrootte is de kleinste chunk van gegevens die van toepassing op striped volumes kunt oplossen. De stripe-grootte die u configureert, is afhankelijk van het type toepassing en de aanvraag-patroon. Als u de verkeerde stripe-grootte kiest, kan dit ertoe leiden dat de onjuiste uitlijning i/o, wat tot verminderde prestaties van uw toepassing leidt.

Bijvoorbeeld, als een i/o-aanvraag die is gegenereerd door de toepassing groter dan de schijf stripe-grootte is, schrijft het opslagsysteem deze buiten de grenzen van de stripe-eenheid op meer dan één schijf. Wanneer is het tijd om toegang tot die gegevens, heeft dit zoeken over meer dan één stripe eenheden om de aanvraag te voltooien. Het cumulatieve effect van dit gedrag kan leiden tot verminderde prestaties aanzienlijk. Anderzijds, kunnen als de grootte van de i/o-aanvraag is kleiner dan stripe-grootte en als het willekeurig, de i/o-aanvragen optellen op dezelfde schijf als een knelpunt veroorzaakt en uiteindelijk de i/o-prestaties beïnvloeden.

Afhankelijk van het type werkbelasting van uw toepassing wordt uitgevoerd, kies een passende stripe-grootte. Gebruik een kleinere stripe voor willekeurige kleine i/o-aanvragen. Terwijl voor grote opeenvolgende i/o-aanvragen stripe groter gebruiken. Ontdek de stripe grootte aanbevelingen voor de toepassing dat u uitvoert op Premium-opslag. Configureer stripe-grootte van 64KB voor OLTP-werkbelastingen en 256KB voor magazijnbeheer werkbelastingen van gegevens voor SQL Server. Zie [best practices prestaties for SQL Server op Azure Virtual machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) voor meer informatie.

> [!NOTE]
> U kunt stripe samen een maximum van 32 premium-opslag-schijven op een DS-serie VM en 64 premium-opslag-schijven op een GS-serie VM.
>
>

## <a name="multi-threading"></a>Multithreading
Azure heeft Premium-opslag-platform massively parallelle worden ontworpen. Een toepassing met meerdere threads realiseert daarom veel hogere prestaties dan een toepassing met één thread. Een toepassing met meerdere threads zijn taken opgesplitst in meerdere threads en verhoogt de efficiëntie van de uitvoering door het gebruik van de virtuele machine en de schijf bronnen voor het maximum.

Als uw toepassing wordt uitgevoerd op één kern VM met twee threads, kunt u voor de CPU, kan schakelen tussen de twee threads efficiëntie bereiken. Terwijl een thread op een schijf-i/o om te voltooien wacht, wordt de CPU kunt overschakelen naar de andere thread. Op deze manier doen twee threads meer dan één thread zou. Als de virtuele machine meer dan één kern heeft, sneller het verdere uitgevoerd omdat elke core parallel taken kunt uitvoeren.

Mogelijk niet wijzigen van de manier waarop een gebruiksklare toepassing wordt geïmplementeerd op een enkele thread of multithreading. SQL Server is bijvoorbeeld geschikt is voor het verwerken van meerdere CPU en meerdere kernen. SQL Server besluit echter onder welke omstandigheden wordt deze gebruikmaken van een of meer threads voor het verwerken van een query. Het kan query's uitvoeren en bouwen indexen met multithreading. SQL Server gebruikt voor een query waarbij grote tabellen koppelen en gegevens worden gesorteerd voordat u terugkeert naar de gebruiker, waarschijnlijk meerdere threads. Een gebruiker kan echter niet instellen of SQL Server wordt uitgevoerd een query met een enkele thread of meerdere threads.

Er zijn configuratie-instellingen die u wijzigen kunt als u wilt dit van invloed zijn op multithreading of parallelle verwerking van een toepassing. Bijvoorbeeld, in geval van SQL Server is het de maximale mate van parallelle uitvoering configuratie. Deze instelling aangeroepen MAXDOP, kunt u configureren van het maximum aantal processors dat SQL Server kunt gebruiken wanneer parallelle verwerking. U kunt MAXDOP voor afzonderlijke query's of indexbewerkingen configureren. Dit is nuttig wanneer u wilt een balans vinden tussen resources van uw systeem voor een kritieke prestaties-toepassing.

Stel dat uw toepassing met behulp van SQL Server een te grote query en een indexbewerking op hetzelfde moment wordt uitgevoerd. Laat het ons wordt ervan uitgegaan dat u de indexbewerking wilt moet meer zodat vergeleken met de grote query. In dat geval kunt u de waarde van de indexbewerking moet hoger dan de waarde MAXDOP voor de query MAXDOP instellen. Op deze manier heeft SQL Server meer aantal processors dat deze kan gebruikmaken van voor de indexbewerking vergeleken met het aantal processors die deze aan de grote query toewijzen kunt. Denk eraan dat u niet bepalen dat het aantal threads dat SQL Server wordt gebruikt voor elke bewerking. U kunt bepalen het maximum aantal processors worden toegewezen voor multithreading.

Meer informatie over [graden van parallelle uitvoering](https://technet.microsoft.com/library/ms188611.aspx) in SQL Server. Ontdek dergelijke instellingen die van invloed zijn op multithreading in uw toepassing en de bijbehorende configuraties om prestaties te optimaliseren.

## <a name="queue-depth"></a>Wachtrijdiepte
De wachtrijdiepte of wachtrijlengte of grootte van de wachtrij is het aantal openstaande i/o-aanvragen in het systeem. De waarde van wachtrijdiepte bepaalt hoeveel i/o-bewerkingen die uw toepassing kunt uitgelijnd, die de opslagschijven verwerken. Het is van invloed op alle de drie toepassing prestatie-indicatoren die is besproken in dit artikel te weten, IOPS, doorvoer en latentie.

Wachtrij diepte en multithreading zijn nauw verwant zijn. De waarde wachtrijdiepte geeft aan hoeveel multithreading kan worden bereikt door de toepassing. Als de wachtrijdiepte groot is, toepassing gelijktijdig kan worden uitgevoerd meer bewerkingen, met andere woorden, meer multithreading. Als de wachtrijdiepte klein is, zelfs als de toepassing met meerdere threads is, heeft dit niet voldoende aanvragen voor gelijktijdige uitvoering uitgelijnd.

Normaal gesproken overal verkrijgbare toepassingen staan geen u de wachtrijdiepte wijzigen omdat als onjuist voert deze bewerkingen meer schade dan goed is ingesteld. Toepassingen stelt u de juiste waarde van wachtrijdiepte ophalen van de optimale prestaties. Het is echter belangrijk dat u dit concept begrijpt dat voor het oplossen van prestatieproblemen met uw toepassing. U kunt ook de effecten van wachtrijdiepte zien door met het uitvoeren van benchmarking's op uw systeem.

Sommige toepassingen Geef de instellingen voor de wachtrijdiepte beïnvloeden. Bijvoorbeeld, de instelling MAXDOP (maximale graad van parallelle uitvoering) in SQL Server worden beschreven in de vorige sectie. MAXDOP is een manier om te beïnvloeden wachtrijdiepte en multithreading, hoewel deze de waarde wachtrijdiepte van SQL Server niet rechtstreeks wijzigen.

*Hoge wachtrijdiepte*  
Een hoge wachtrijdiepte meer bewerkingen op de schijf wordt uitgelijnd. De schijf kent de volgende aanvraag in de wachtrij tevoren. Als gevolg daarvan kan de schijf bewerkingen tevoren plannen en op een optimale wijze worden verwerkt. Sinds de toepassing meer aanvragen naar de schijf verzonden worden, kan de schijf meer parallelle IOs verwerken. De toepassing worden uiteindelijk hoger IOPS bereiken. Sinds de toepassing meer aanvragen verwerkt, verhoogt ook de totale doorvoer van de toepassing.

Normaal gesproken een toepassing kunt bereiken van maximale doorvoer met 8-16 + openstaande IO's per gekoppelde schijf. Als een wachtrijdiepte een toepassing is niet voldoende IOs pushen naar het systeem en minder hoeveelheid in een bepaalde periode wordt verwerkt. Met andere woorden, minder doorvoer.

Bijvoorbeeld, in SQL Server informeert MAXDOP waarde ingesteld voor een query naar een "4" SQL Server dat deze maximaal vier kernen gebruiken kunt om de query wordt uitgevoerd. SQL Server bepaalt wat de beste wachtrijdiepte waarde en het aantal kernen voor het uitvoeren van de query is.

*Optimale wachtrijdiepte*  
Zeer hoge wachtrijdiepte waarde heeft ook de nadelen. Als de wachtrijdiepte waarde is te hoog, probeert de toepassing om zeer hoge IOPS. Tenzij de toepassing heeft permanente schijven met voldoende ingerichte IOP's, kan dit toepassingslatenties negatief beïnvloeden. De formule te volgen, wordt de relatie tussen IOPS en latentie wachtrijdiepte.  
    ![](media/premium-storage-performance/image6.png)

U moet de wachtrijdiepte niet configureren met een hoge waarde, maar met een optimale waarde, die voldoende IOPS aan voor de toepassing zonder latenties kan bieden. Bijvoorbeeld, als de latentie van de toepassing moet zijn van 1 milliseconde, de wachtrijdiepte voor 5000 IOP's vereist is, Wachtrijdiepte = 5000 x 0,001 = 5.

*Wachtrijdiepte voor Striped volumes*  
Voor een striped volume een hoog genoeg wachtrijdiepte te onderhouden zodat elke schijf een piek wachtrijdiepte afzonderlijk heeft. Neem bijvoorbeeld een toepassing die een wachtrijdiepte van 2 pushes en er zijn 4 schijven in de streep. De twee i/o-aanvragen gaat naar twee schijven en resterende van twee schijven worden niet actief. Daarom de wachtrijdiepte te configureren dat alle schijven bezet is. Formule hieronder ziet u hoe de wachtrijdiepte voor striped volumes bepalen.  
    ![](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Beperking
Azure Premium-opslag voorziet in een opgegeven aantal IOPS en Doorvoerlimieten afhankelijk van de VM-grootten en de schijfgrootte die u kiest. Telkens wanneer de toepassing probeert te IOPS of doorvoer station boven deze limieten van wat de virtuele machine of de schijf kan verwerken, wordt het Premium-opslag vertraging. Dit manifesten in de vorm van verminderde prestaties in uw toepassing. Dit kan betekenen hogere latentie, doorvoer verlagen of IOPS verlagen. Als Premium-opslag niet beperken biedt, wordt uw toepassing kan volledig mislukt door meer dan wat de daarbij behorende bronnen zijn geschikt om dat te bereiken. Dus om prestatieproblemen te voorkomen vanwege een beperking, altijd voldoende bronnen zijn voor uw toepassing te creëren. In overweging nemen we in de VM-grootten en schijf grootten hierboven besproken. Benchmarking is de beste manier om te achterhalen welke resources u nodig hebt voor het hosten van uw toepassing.

## <a name="benchmarking"></a>Benchmarking
Benchmarking is het proces van andere werkbelastingen op uw toepassing simuleren en de prestaties van toepassingen voor elke werkbelasting te meten. U hebt de prestatie-eisen van toepassing met de stappen in een eerdere sectie, verzameld. Door te voeren benchmarking hulpprogramma's op de virtuele machines die als host fungeert voor de toepassing, kunt u de prestaties die uw toepassing met Premium-opslag bereiken kunt te bepalen. In deze sectie bieden we u voorbeelden van benchmarks van een standaard DS14 VM ingericht met Azure Premium-opslag-schijven.

We hebben respectievelijk Iometer en FIO, algemene benchmarking's gebruikt voor Windows en Linux. Deze hulpprogramma's voor meerdere threads simuleren een productie-achtige werkbelasting kan worden gemaakt en meet de systeemprestaties. U kunt ook een parameters zoals blok grootte en wachtrij diepte die u normaal gesproken niet voor een toepassing wijzigen configureren met de hulpprogramma's. Dit biedt u meer flexibiliteit om de maximale prestaties op een grootschalige virtuele machine die zijn ingericht met premium-schijven voor verschillende soorten toepassingsworkloads. Voor meer informatie over elke benchmarking hulpprogramma vindt u [Iometer](http://www.iometer.org/) en [FIO](http://freecode.com/projects/fio).

Wilt u de volgende voorbeelden, een standaard DS14 virtuele machine maken en 11 Premium opslagschijven koppelen aan de virtuele machine. 11 schijven 10 schijven met hostcaching als 'Geen' configureren en ze naar een volume genaamd NoCacheWrites stripe. Hostcaching als 'ReadOnly' op de resterende schijf configureren en een volume CacheReads aangeroepen met deze schijf maken. Met deze instellingen, kunt u zich kunt zien van de maximale prestaties voor lezen en schrijven van een standaard DS14 virtuele machine. Ga voor gedetailleerde stappen voor het maken van een VM DS14 met premium-schijven naar [maken en gebruiken een Premium-opslag-account voor een virtuele machine gegevensschijf](../articles/virtual-machines/windows/premium-storage.md).

*Opwarmen van de Cache*  
De schijf met het kenmerk alleen-lezen-hostcaching zich geven hogere IOPS dan de limiet voor de schijf. Als u de maximale prestaties lezen uit de cache van de host, moet eerst u opgewarmd de cache van deze schijf. Dit zorgt ervoor dat het bestand lezen IOs welke benchmarking hulpprogramma op CacheReads volume veranderingen daadwerkelijk treffers in de cache en niet op de schijf rechtstreeks. Het resultaat van de treffers in cache in extra IOP's uit de cache voor één schijf ingeschakeld.

> **Belangrijk:**  
> U moet de cache opgewarmd voordat benchmarking, wordt uitgevoerd telkens wanneer de VM opnieuw wordt opgestart.
>
>

#### <a name="iometer"></a>Iometer
[Download het hulpprogramma Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) op de virtuele machine.

*Testbestand*  
Iometer maakt gebruik van een testbestand dat is opgeslagen op het volume waarop u de benchmarking test wordt uitgevoerd. Deze stations leest en schrijft voor dit testbestand voor het meten van de schijf IOPS en Doorvoerlimieten. Iometer maakt deze testbestand als u deze niet hebt opgegeven. Maak een bestand voor het testen van 200GB iobw.tst aangeroepen voor de volumes CacheReads en NoCacheWrites.

*Toegangsspecificaties*  
De specificaties aanvragen i/o-grootte, % lezen/schrijven, willekeurige/sequentiële % zijn geconfigureerd op het tabblad 'Specificaties Access' in Iometer. Maakt een access-specificatie voor elk van de scenario's die hieronder worden beschreven. Maak de toegangsspecificaties en 'Opgeslagen' met een geschikte naam zoals – RandomWrites\_8 kB, RandomReads\_8 kB. Selecteer de bijbehorende specificatie bij het uitvoeren van de Testscenario.

Een voorbeeld van de toegangsspecificaties voor maximale IOPS schrijven scenario wordt hieronder weergegeven  
    ![](media/premium-storage-performance/image8.png)

*Maximale IOPS Test specificaties*  
Gebruiken om te demonstreren maximumaantal IOPs, kleinere aanvraaggrootte. Gebruik van 8 kB aangevraagde grootte en maak specificaties voor willekeurige geschreven en gelezen.

| Specificatie toegang | De aanvraaggrootte van de | Willekeurige % | % Lezen |
| --- | --- | --- | --- |
| RandomWrites\_8 kB |8 KB |100 |0 |
| RandomReads\_8 kB |8 KB |100 |100 |

*Maximale doorvoer Test specificaties*  
Gebruiken om te demonstreren maximale doorvoer, groter formaat van de aanvraag. De grootte van 64K aanvraag gebruiken en specificaties voor willekeurige schrijfbewerkingen en leesbewerkingen maken.

| Specificatie toegang | De aanvraaggrootte van de | Willekeurige % | % Lezen |
| --- | --- | --- | --- |
| RandomWrites\_64 kB |64 KB |100 |0 |
| RandomReads\_64 kB |64 KB |100 |100 |

*Uitvoering van de Test Iometer*  
Voer onderstaande stappen voor het opgewarmd cache

1. Twee toegangsspecificaties maken met de onderstaande waarden,

   | Naam | De aanvraaggrootte van de | Willekeurige % | % Lezen |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
2. De test Iometer voor het initialiseren van de cache-schijf met de volgende parameters worden uitgevoerd. Drie werkthreads gebruiken voor het doelvolume en een wachtrijdiepte van 128. Stel de 'Uitvoeren tijd' duur van de test naar 2hrs op het tabblad 'Instellingen testen'.

   | Scenario | Doelvolume | Naam | Duur |
   | --- | --- | --- | --- |
   | Initialiseer de schijf Cache |CacheReads |RandomWrites\_1 MB |2hrs |
3. De test Iometer voor het opwarmen van cache-schijf met de volgende parameters worden uitgevoerd. Drie werkthreads gebruiken voor het doelvolume en een wachtrijdiepte van 128. Stel de 'Uitvoeren tijd' duur van de test naar 2hrs op het tabblad 'Instellingen testen'.

   | Scenario | Doelvolume | Naam | Duur |
   | --- | --- | --- | --- |
   | Warme van Cache-schijf |CacheReads |RandomReads\_1 MB |2hrs |

Nadat de cache-schijf is opgewarmd, doorgaan met het onderstaande Testscenario's. Voor het uitvoeren van de test Iometer, gebruikt u ten minste drie worker threads voor **elke** volume als doel. Selecteer het doelvolume voor elke werkthread, wachtrijdiepte ingesteld en selecteert u een van de specificaties opgeslagen test, zoals wordt weergegeven in de onderstaande tabel, om uit te voeren van het bijbehorende Testscenario. De tabel bevat ook verwachte resultaten voor IOPS en Doorvoerlimieten wanneer deze tests wordt uitgevoerd. Voor alle scenario's is een kleine i/o-grootte van 8KB en een hoge wachtrijdiepte van 128 gebruikt.

| Testscenario | Doelvolume | Naam | Resultaat |
| --- | --- | --- | --- |
| Met maximaal Lees IOP 's |CacheReads |RandomWrites\_8 kB |50.000 IOP 'S |
| Met maximaal IOPS schrijven |NoCacheWrites |RandomReads\_8 kB |64.000 IOP 'S |
| Met maximaal Gecombineerde IOP 's |CacheReads |RandomWrites\_8 kB |100.000 IOP 'S |
| NoCacheWrites |RandomReads\_8 kB | &nbsp; | &nbsp; |
| Met maximaal Lees MB per seconde |CacheReads |RandomWrites\_64 kB |524 MB per seconde |
| Met maximaal Schrijven van MB per seconde |NoCacheWrites |RandomReads\_64 kB |524 MB per seconde |
| Gecombineerde MB per seconde |CacheReads |RandomWrites\_64 kB |1000 MB per seconde |
| NoCacheWrites |RandomReads\_64 kB | &nbsp; | &nbsp; |

Hieronder vindt u schermopnamen van de Iometer testresultaten voor gecombineerde IOPS en Doorvoerlimieten scenario's.

*Gecombineerde lees- en schrijfbewerkingen maximumaantal IOPS*  
![](media/premium-storage-performance/image9.png)

*Gecombineerde lees- en schrijfbewerkingen maximale doorvoer*  
![](media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO is een populair hulpprogramma benchmark opslag op de virtuele Linux-machines. Heeft de flexibiliteit om te selecteren van verschillende i/o-grootte, sequentiële of willekeurige leest en schrijft. Deze gestart werkthreads of processen de opgegeven i/o-bewerkingen uit te voeren. U kunt het type van i/o-bewerkingen die elke werkthread moet uitvoeren met behulp van taakbestanden opgeven. Een taakbestand per scenario geïllustreerd in de volgende voorbeelden is gemaakt. U kunt de specificaties die in deze taakbestanden voor het benchmarken van andere werkbelastingen die worden uitgevoerd op de Premium-opslag wijzigen. In de voorbeelden gebruiken we een actieve standaard DS 14 VM **Ubuntu**. Gebruik dezelfde instellingen die worden beschreven in het begin van de [sectie Benchmarking](#Benchmarking) en warme van de cache voordat u de benchmarking tests uitvoert.

Voordat u begint, [FIO downloaden](https://github.com/axboe/fio) en installeer deze op uw virtuele machine.

Voer de volgende opdracht voor Ubuntu,

```
apt-get install fio
```

We gebruiken voor aangedreven leesbewerkingen op de schijven in vier werkthreads voor het besturen van schrijfbewerkingen en vier werkthreads. De werknemers schrijven wordt verkeer op het volume 'nocache', die 10 schijven met de cache ingesteld op 'Geen heeft' besturen. De werknemers lezen wordt verkeer op het volume 'readcache', die 1 schijf bevat met de cache ingesteld op 'ReadOnly' besturen.

*Maximale schrijven IOP 's*  
De taakbestand maken met de volgende specificaties maximumaantal IOPS schrijven ophalen. Geef deze de naam 'fiowrite.ini'.

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

Noteer de belangrijke dingen waarop volgen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties beschreven zijn. Deze specificaties zijn essentieel voor de maximale IOPS station  

* Een hoge wachtrijdiepte van 256.  
* Een kleine blokgrootte van 8KB.  
* Meerdere threads uitvoeren van willekeurige schrijfbewerkingen.

Voer de volgende opdracht te ere van de test FIO gedurende 30 seconden  

```
sudo fio --runtime 30 fiowrite.ini
```

Tijdens de test wordt uitgevoerd, kunt u zich kan het aantal schrijven IOP's van de virtuele machine en het leveren van Premium-schijven. De VM DS14 levert de maximale IOPS-limiet van 50.000 IOPS-schrijven, zoals in het voorbeeld hieronder weergegeven.  
    ![](media/premium-storage-performance/image11.png)

*Maximale lezen IOP 's*  
De taakbestand maken met de volgende specificaties maximumaantal IOPS op lezen ophalen. Geef deze de naam 'fioread.ini'.

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

Noteer de belangrijke dingen waarop volgen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties beschreven zijn. Deze specificaties zijn essentieel voor de maximale IOPS station

* Een hoge wachtrijdiepte van 256.  
* Een kleine blokgrootte van 8KB.  
* Meerdere threads uitvoeren van willekeurige schrijfbewerkingen.

Voer de volgende opdracht te ere van de test FIO gedurende 30 seconden

```
sudo fio --runtime 30 fioread.ini
```

Tijdens de test wordt uitgevoerd, kunt u zich kan het aantal gelezen IOP's van de virtuele machine en het leveren van Premium-schijven. Zoals u in het voorbeeld hieronder, levert de DS14 VM maximaal 64.000 lezen IOPS. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](media/premium-storage-performance/image12.png)

*Maximale lezen en schrijven IOP 's*  
Maken van de taak met de volgende specificaties ophalen maximale gecombineerde lezen en schrijven IOPS. Geef deze de naam 'fioreadwrite.ini'.

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

Noteer de belangrijke dingen waarop volgen die in overeenstemming met de richtlijnen voor het ontwerpen in de vorige secties beschreven zijn. Deze specificaties zijn essentieel voor de maximale IOPS station

* Een hoge wachtrijdiepte van 128.  
* Een kleine blokgrootte van 4KB.  
* Meerdere threads uitvoeren van willekeurige leest en schrijft.

Voer de volgende opdracht te ere van de test FIO gedurende 30 seconden

```
sudo fio --runtime 30 fioreadwrite.ini
```

Tijdens de test wordt uitgevoerd, kunt u zich kunnen zien van het aantal gecombineerde lezen en schrijven IOP's van de virtuele machine en leveren van Premium-schijven. Zoals u in het voorbeeld hieronder, wordt de VM DS14 leveren meer dan 100.000 gecombineerde lezen en schrijven IOPS. Dit is een combinatie van de schijf en de prestaties van de cache.  
    ![](media/premium-storage-performance/image13.png)

*Maximale gecombineerde doorvoer*  
Als u de maximale gecombineerde lezen en schrijven doorvoer, gebruikt een groter blok en grote wachtrijdiepte met meerdere threads uitvoeren van lees- en schrijfbewerkingen. U kunt een blokgrootte van 64KB en wachtrijdiepte van 128.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure Premium-opslag:

* [Premium Storage: opslag met hoge prestaties voor de werkbelasting van virtuele Azure-machines](../articles/virtual-machines/windows/premium-storage.md)  

Lees de artikelen over aanbevolen procedures voor prestaties voor SQL Server voor SQL Server-gebruikers:

* [Best Practices prestaties for SQL Server in Azure Virtual Machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium-opslag biedt de beste prestaties voor SQL Server in Azure VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
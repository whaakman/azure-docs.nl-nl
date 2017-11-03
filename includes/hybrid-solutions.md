# <a name="azure-service-bus"></a>Azure Service Bus
Een toepassing of service moet vaak communiceren met andere toepassingen of services, ongeacht of de toepassing of service in de cloud of on-premises wordt uitgevoerd. Als u wilt een veelzijdige, handige manier om dit te doen, biedt Azure Service Bus. In dit artikel wordt een overzicht gegeven van deze technologie en wordt beschreven wat de service inhoudt en waarom u deze zou kunnen overwegen.

## <a name="service-bus-fundamentals"></a>Grondbeginselen van Service Bus
Verschillende situaties vragen om verschillende communicatiemethoden. Soms is het de beste oplossing als toepassingen berichten verzenden en ontvangen via een eenvoudige wachtrij. In andere gevallen is een gewone wachtrij niet voldoende en voldoet een wachtrij met een mechanisme voor publiceren en abonneren beter. En in sommige gevallen is het enige dat echt nodig is een verbinding tussen de toepassingen &#151; wachtrijen zijn niet vereist. Service Bus biedt alle drie de opties, zodat uw toepassingen op verschillende manieren communiceren.

Service Bus is een multitenant-cloudservice, hetgeen betekent dat de service wordt gedeeld door meerdere gebruikers. Elke gebruiker, zoals een toepassingsontwikkelaar, maakt een *naamruimte* en definieert vervolgens de binnen deze naamruimte benodigde communicatiemechanismen. [Afbeelding 1](#Fig1) ziet u hoe dit eruitziet.

<a name="Fig1"></a>![Diagram van Azure Servicebus][svc-bus]

**Afbeelding 1: Service Bus voorziet in een multitenant-service voor verbinding van toepassingen via de cloud.**

Binnen een naamruimte kunt u een of meer exemplaren van vier verschillende communicatiemechanismen gebruiken, die toepassingen alle vier op een andere manier verbinden. De opties zijn:

* *Wachtrijen*, waarmee communicatie in één richting mogelijk is. Elke wachtrij fungeert als intermediaire service (ook wel *broker* genoemd) die de verzonden berichten opslaat totdat ze worden ontvangen. Elk bericht wordt door één ontvanger ontvangen.
* *Onderwerpen*, die communicatie in één richting bieden via *abonnementen*, waarbij een enkel onderwerp meerdere abonnementen kan hebben. Net zoals een wachtrij fungeert een onderwerp als broker, maar elk abonnement kan optioneel een filter gebruiken om alleen berichten te ontvangen die aan bepaalde criteria voldoen.
* *Relays*, die bidirectionele communicatie bieden. In tegenstelling tot wachtrijen en onderwerpen, worden via een Relay berichten die onderweg zijn niet opgeslagen. Een Relay is geen broker. De berichten worden slechts doorgegeven aan de doeltoepassing.
* *Event Hubs*, verwerkingsservices die zeer grote hoeveelheden gebeurtenissen en telemetriegegevens verzamelen in de cloud, met een lage latentie en een hoge betrouwbaarheid.

Wanneer u een wachtrij, onderwerp, Relay of Event Hub maakt, geeft u deze een naam. In combinatie met de naam van uw naamruimte wordt aan de hand van deze naam een unieke id voor het object gemaakt. Toepassingen kunnen deze naam verstrekken aan Service Bus en vervolgens de wachtrij, het onderwerp, de Relay of de Event Hub gebruiken om met elkaar te communiceren. 

Windows-toepassingen kunnen WCF (Windows Communication Foundation) gebruiken om een van deze objecten te gebruiken. Voor wachtrijen, onderwerpen en Event Hubs kunnen Windows-toepassingen ook door Service Bus gedefinieerde berichtenservice-API's gebruiken. Microsoft biedt SDK's voor Java, Node.js en andere talen om deze objecten gemakkelijker te kunnen gebruiken vanuit niet-Windows-toepassingen. U hebt ook toegang tot wachtrijen, onderwerpen en Event Hubs met behulp van REST-API's via HTTP. 

Het is belangrijk te begrijpen dat toepassingen die Service Bus gebruiken overal kunnen worden uitgevoerd, hoewel Service Bus zelf wordt uitgevoerd in de cloud (dat wil zeggen, in Azure-datacenters van Microsoft). U kunt Service Bus bijvoorbeeld gebruiken om verbinding te maken met toepassingen die worden uitgevoerd in Azure, of toepassingen die worden uitgevoerd binnen uw eigen datacenter. U kunt Service Bus ook gebruiken om verbinding te maken met een toepassing die wordt uitgevoerd in Azure of een ander cloudplatform met een on-premises toepassing, of met een tablet of telefoon. Het is zelfs mogelijk huishoudelijke apparaten, sensoren en andere apparaten te verbinden met een centrale toepassing of met elkaar. Service Bus is een algemeen communicatiemechanisme in de cloud, dat vanaf vrijwel elke locatie toegankelijk is. De manier waarop u Service Bus gebruikt, hangt af van de taak van uw toepassingen.

## <a name="queues"></a>Wachtrijen
Stel dat u met twee toepassingen verbinding wilt maken via een Service Bus-wachtrij. [Afbeelding 2](#Fig2) ziet u deze situatie.

<a name="Fig2"></a>![Diagram van Service Bus-wachtrijen][queues]

**Afbeelding 2: Service Bus-wachtrijen bieden asynchrone wachtrijservices in één richting.**

Het proces is eenvoudig: een afzender verzendt een bericht naar een Service Bus-wachtrij en een ontvanger neemt het bericht op een later tijdstip in ontvangst. Een wachtrij kan slechts één ontvanger, hebben als [afbeelding 2](#Fig2) toont of meerdere toepassingen uit dezelfde wachtrij kunnen lezen. In het laatste geval wordt elk bericht gelezen door slechts één ontvanger. Voor een multicast-service is het beter om een onderwerp te gebruiken.

Elk bericht bestaat uit twee delen: een reeks eigenschappen, voor elk bericht een sleutel-waardepaar, en een binaire berichttekst. Hoe deze worden gebruikt, hangt af van wat een toepassing probeert te doen. Een toepassing die een bericht verzendt over een recente verkoop, kan bijvoorbeeld de eigenschappen *Verkoper = Ava* en *Bedrag = 10000* bevatten. De berichttekst kan een gescande afbeelding van de ondertekende verkoopovereenkomst bevatten of, indien deze er niet is, leeg blijven.

Een ontvanger kan op twee verschillende manieren een bericht in een Service Bus-wachtrij lezen. Met de eerste optie, *ReceiveAndDelete*, wordt een bericht uit de wachtrij verwijderd en onmiddellijk gewist. Dit is eenvoudig, maar als de ontvanger vastloopt voordat het bericht is verwerkt, gaat het bericht verloren. Andere ontvangers hebben geen toegang tot het bericht, omdat het uit de wachtrij is verwijderd. 

De tweede optie, *PeekLock*, is bedoeld om dit probleem te ondervangen. Zoals ReceiveAndDelete wordt een leesbewerking PeekLock een bericht uit de wachtrij. Het bericht wordt echter niet gewist. In plaats daarvan wordt het vergrendeld, waardoor het onzichtbaar wordt voor andere ontvangers. Vervolgens wordt gewacht op een van drie gebeurtenissen:

* Als de ontvanger het bericht met succes heeft verwerkt, ontvangt de wachtrij de melding *Voltooid* en wordt het bericht uit de wachtrij verwijderd. 
* Als de ontvanger het bericht niet naar behoren kan verwerken, ontvangt de wachtrij de melding *Afbreken*. De vergrendeling van het bericht in de wachtrij wordt ongedaan gemaakt en het bericht is beschikbaar voor andere ontvangers.
* Als de ontvanger binnen de geconfigureerde tijd (standaard 60 seconden) geen van beide meldingen afgeeft, wordt ervan uitgegaan dat het ontvangen is mislukt. In dit geval gedraagt deze bewerking zich alsof de ontvanger heeft aangeroepen afbreken, het bericht beschikbaar maken voor andere ontvangers.

Hetzelfde bericht kan dus tweemaal worden geleverd, mogelijk aan twee verschillende ontvangers. Toepassingen die gebruikmaken van Service Bus-wachtrijen, moeten hiermee rekening houden. Detectie van duplicaten om gemakkelijker te maken, dat elk bericht heeft een unieke MessageID-eigenschap die standaard hetzelfde, ongeacht hoe vaak blijft is het bericht gelezen uit een wachtrij. 

Wachtrijen zijn handig in tal van situaties. Met behulp van wachtrijen kunnen toepassingen zelfs communiceren wanneer ze niet tegelijkertijd worden uitgevoerd. Dit is met name handig in het geval van batchtoepassingen en mobiele toepassingen. Een wachtrij met meerdere ontvangers biedt ook automatische taakverdeling, aangezien verzonden berichten worden verdeeld over deze ontvangers.

## <a name="topics"></a>Onderwerpen
Hoewel ze handig zijn, zijn wachtrijen niet altijd de juiste oplossing. Service Bus-onderwerpen komen soms beter van pas. [Afbeelding 3](#Fig3) laat dit zien.

<a name="Fig3"></a>![Diagram van Service Bus-onderwerpen en abonnementen][topics-subs]

**Afbeelding 3: Op basis van het filter dat door een geabonneerde toepassing wordt opgegeven, kan de toepassing enkele of alle berichten ontvangen die naar een Service Bus-onderwerp zijn verzonden.**

Een onderwerp lijkt op veel manieren op een wachtrij. Afzenders verzenden berichten op dezelfde manier naar een onderwerp als naar een wachtrij en de berichten zien er hetzelfde uit als bij een wachtrij. Het grote verschil is dat onderwerpen elke ontvangende toepassing een eigen abonnement maken door te definiëren, kunnen een *filter*. Een abonnee ziet vervolgens alleen de berichten die overeenkomen met het filter. Bijvoorbeeld: [afbeelding 3](#Fig3) ziet u een afzender en een onderwerp met drie abonnees, elk met een eigen filter:

* Abonnee 1 ontvangt alleen berichten met de eigenschap *Verkoper =Ava*.
* Abonnee 2 ontvangt berichten met de eigenschap *Verkoper = Ruby* en/of een eigenschap *Bedrag* met een waarde van meer dan 100.000. Misschien Ruby de Verkoopmanager is en dus wil ze zowel haar eigen verkopen als alle omvangrijke verkopen ongeacht wie ze zien.
* Abonnee 3 heeft het filter ingesteld op *Waar*, hetgeen betekent dat alle berichten worden ontvangen. Wellicht is deze toepassing bijvoorbeeld verantwoordelijk voor het onderhouden van een audittrail en moet deze alle berichten zien.

Als bij een wachtrij, lezen abonnees naar een onderwerp berichten met ReceiveAndDelete of PeekLock. In tegenstelling tot wachtrijen kan echter een enkel bericht dat naar een onderwerp is verzonden, worden ontvangen door meerdere abonnees. Deze benadering, doorgaans aangeduid *publiceren en abonneren*, is nuttig wanneer meerdere toepassingen mogelijk geïnteresseerd zijn in dezelfde berichten. Door het juiste filter te definiëren, kunnen abonnees alleen het gedeelte van de berichtenstroom ontvangen dat ze nodig hebben.

## <a name="relays"></a>Relays
Wachtrijen en onderwerpen bieden allebei asynchrone communicatie in één richting via een broker. Het verkeer stroomt in één richting en er is geen directe verbinding tussen afzenders en ontvangers. Maar wat als u dit niet wilt? Stel dat uw toepassingen berichten moeten verzenden en berichten moeten ontvangen, of stel dat u een rechtstreekse koppeling tussen uw toepassingen wilt en geen broker nodig hebt voor het opslaan van berichten. Soort scenario's zoals deze, biedt Service Bus relays, als [afbeelding 4](#Fig4) bevat.

<a name="Fig4"></a>![Diagram van Service Bus Relay][relay]

**Afbeelding 4: Service Bus Relay biedt synchrone communicatie in twee richtingen tussen toepassingen.**

De voor de hand liggende vraag is: waarom zou u een Relay gebruiken? Zelfs als u geen wachtrijen nodig hebt, waarom zouden toepassingen dan moeten communiceren via een cloudservice in plaats van dat ze rechtstreeks communiceren? Het antwoord is dat rechtstreeks communiceren lastiger kan zijn dan u denkt.

Stel dat u twee on-premises toepassingen wilt verbinden, die allebei worden uitgevoerd binnen bedrijfsdatacenters. Elk van deze toepassingen bevindt zich achter een firewall en elk datacenter maakt waarschijnlijk gebruik van NAT (netwerkadresomzetting). De firewall blokkeert binnenkomende gegevens op bijna alle poorten en NAT impliceert dat de computer waarop de toepassingen worden uitgevoerd, geen vast IP-adres heeft dat u rechtstreeks kunt bereiken buiten het datacenter. Zonder extra hulp is het lastig om deze toepassingen met elkaar te verbinden via openbaar internet.

Een Service Bus Relay biedt de benodigde hulp. Voor communicatie in twee richtingen via een Relay, brengt elke toepassing een uitgaande TCP-verbinding tot stand met Service Bus. Deze verbinding blijft geopend. Alle communicatie tussen de twee toepassingen verloopt via deze verbindingen. Omdat elke verbinding vanuit het datacenter tot stand is gebracht, staat de firewall inkomend verkeer naar elke toepassing toe zonder nieuwe poorten te openen. Via deze benadering wordt ook het NAT-probleem opgelost, omdat elke toepassing tijdens de communicatie een consistent eindpunt in de cloud heeft. Door gegevens uit te wisselen via de Relay kunnen de toepassingen problemen voorkomen die de communicatie anders zouden bemoeilijken. 

Voor het gebruik van Service Bus relays zijn toepassingen afhankelijk van Windows Communication Foundation (WCF). Service Bus biedt WCF-bindingen waarmee Windows-toepassingen eenvoudig kunnen communiceren via Relays. Toepassingen die al gebruikmaken van WCF hoeven doorgaans slechts een van deze bindingen op te geven en kunnen vervolgens met elkaar communiceren via een Relay. Relays kunnen worden gebruikt vanuit niet-Windows-toepassingen, maar in tegenstelling tot wachtrijen en onderwerpen vereist dit enige programmering. Er worden geen standaardbibliotheken geleverd.

In tegenstelling tot wachtrijen en onderwerpen, maken toepassingen Relays niet expliciet. Wanneer een toepassing die berichten wil ontvangen, een TCP-verbinding tot stand brengt met Service Bus, wordt automatisch een Relay gemaakt. Wanneer de verbinding wordt verbroken, wordt de Relay verwijderd. Als u wilt dat een toepassing de relay gemaakt door een specifieke listener vinden, voorziet Service Bus in een register waarmee toepassingen een specifieke relay vinden op naam.

Relays zijn de juiste oplossing wanneer rechtstreekse communicatie tussen toepassingen nodig is. Denk bijvoorbeeld aan een reserveringssysteem voor een luchtvaartmaatschappij dat wordt uitgevoerd in een on-premises datacenter en dat toegankelijk moet zijn via incheckbalies, mobiele apparaten en andere computers. Toepassingen die op al deze systemen worden uitgevoerd, zouden op Service Bus Relays in de cloud kunnen vertrouwen voor communicatie, waar ze ook worden uitgevoerd.

## <a name="event-hubs"></a>Event Hubs
Event Hubs is een zeer schaalbaar systeem voor inkomende gegevens dat miljoenen gebeurtenissen per seconde kan verwerken. Uw toepassing kan hiermee de enorme hoeveelheden gegevens verwerken en analyseren die worden geproduceerd door uw verbonden apparaten en toepassingen. U kunt bijvoorbeeld een Event Hub gebruiken voor het verzamelen van live gegevens over motorprestaties van een wagenpark. Als ze eenmaal in Event Hubs zijn verzameld, kunt u de gegevens omzetten en opslaan met een realtime analytics-provider of opslagcluster. Zie voor meer informatie over Event Hubs, de [overzicht van Event Hubs][Event Hubs overview].

## <a name="summary"></a>Samenvatting
Het verbinden van toepassingen is altijd onderdeel geweest van het ontwikkelen van complete oplossingen. De verscheidenheid van scenario's waarin toepassingen en services met elkaar moeten communiceren, zal toenemen naarmate meer toepassingen en apparaten zijn verbonden met internet. Service Bus is erop gericht om deze essentiële functie gemakkelijker te implementeren en op grotere schaal beschikbaar te maken door cloudtechnologieën te leveren die deze communicatie mogelijk maken via wachtrijen, onderwerpen, Relays en Event Hubs.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Event Hubs overview]: https://msdn.microsoft.com/library/azure/dn836025.aspx

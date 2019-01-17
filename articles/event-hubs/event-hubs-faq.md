---
title: Veelgestelde vragen - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een lijst met veelgestelde vragen (FAQ) voor Azure Event Hubs en de antwoorden.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: bcbb249c6cb3c886b94f8ad68dd668cb8858ac77
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354572"
---
# <a name="event-hubs-frequently-asked-questions"></a>Veelgestelde vragen over Eventhubs

## <a name="general"></a>Algemeen

### <a name="what-is-an-event-hubs-namespace"></a>Wat is een Event Hubs-naamruimte?
Een naamruimte is een scoping container voor Event Hub/Kafka-onderwerpen. Dit biedt u een unieke [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Een naamruimte fungeert als een toepassingscontainer dat met meerdere Event Hub/Kafka-onderwerpen werken kan. 

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Wat is het verschil tussen Event Hubs basis en standaard-laag?

De prijscategorie Standard van Azure Event Hubs biedt functies dan wat er beschikbaar is in de laag basis. De volgende functies zijn inbegrepen in Standard:

* Langere bewaartermijn van de gebeurtenis
* Extra brokered verbindingen met een vaste kosten voor meer dan het aantal opgenomen
* Meer dan één [consumentengroep](event-hubs-features.md#consumer-groups)
* [vastleggen](event-hubs-capture-overview.md)
* [Kafka-integratie](event-hubs-for-kafka-ecosystem-overview.md)

Zie voor meer informatie over de prijzen van lagen, met inbegrip van Event Hubs Dedicated, de [Event Hubs prijsinformatie](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="where-is-azure-event-hubs-available"></a>Waar is Azure Event Hubs beschikbaar?

Azure Event Hubs is beschikbaar in alle ondersteunde Azure-regio's. Voor een lijst, gaat u naar de [Azure-regio's](https://azure.microsoft.com/regions/) pagina.  

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Kan ik één AMQP-verbinding gebruiken om te verzenden en ontvangen van meerdere eventhubs?

Ja, zolang alle eventhubs in dezelfde naamruimte zijn.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Wat is de maximale bewaarperiode voor gebeurtenissen?

Event Hubs Standard-laag biedt momenteel ondersteuning voor een maximale bewaarperiode van zeven dagen. Eventhubs zijn niet bedoeld als permanente gegevensopslag. Bewaarperioden langer dan 24 uur zijn bedoeld voor scenario's waarbij het handig is dat een gebeurtenisstroom dezelfde systemen; Als u bijvoorbeeld wilt trainen of te controleren of een nieuw model voor machine learning op bestaande gegevens. Als u de bewaarperiode na zeven dagen bericht moet, waardoor [Event Hubs Capture](event-hubs-capture-overview.md) op uw event hub haalt de gegevens uit uw event hub in de Storage-account of Azure Data Lake-Service-account van uw keuze. Capture inschakelen leidt tot een kosten in rekening gebracht op basis van de aangekochte doorvoereenheden.

### <a name="how-do-i-monitor-my-event-hubs"></a>Hoe bewaak ik mijn Event Hubs?
Eventhubs verzendt uitgebreide metrische gegevens die de status van uw resources te bieden [Azure Monitor](../azure-monitor/overview.md). Ook kunt u bij het bepalen van de algemene status van de Event Hubs-service niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de. Meer informatie over welke bewaking wordt aangeboden voor [Azure Event Hubs](event-hubs-metrics-azure-monitor.md).

## <a name="apache-kafka-integration"></a>Integratie van Apache Kafka

### <a name="how-do-i-integrate-my-existing-kafka-application-with-event-hubs"></a>Hoe Integreer ik mijn bestaande Kafka-toepassing met Event Hubs?
Eventhubs biedt een Kafka-eindpunt dat kan worden gebruikt door uw bestaande Apache Kafka op basis van toepassingen. Een wijziging in de configuratie is het enige dat is vereist om de ervaring PaaS Kafka. Het biedt een alternatief voor het uitvoeren van uw eigen Kafka-cluster. Eventhubs biedt ondersteuning voor Apache Kafka 1.0 en nieuwere clientversies en werkt met uw bestaande toepassingen, hulpprogramma's en frameworks in Kafka. Zie voor meer informatie, [Event Hubs voor Kafka-opslagplaats](https://github.com/Azure/azure-event-hubs-for-kafka).

### <a name="what-configuration-changes-need-to-be-done-for-my-existing-application-to-talk-to-event-hubs"></a>Welke wijzigingen in de configuratie moeten worden uitgevoerd voor mijn bestaande toepassingen kan communiceren met Event Hubs?
Voor verbinding met een Gebeurtenishub waarvoor Kafka is ingeschakeld, moet u de configuraties van Kafka-client bijwerken. Dit wordt gedaan door het maken van een Event Hubs-naamruimte en het verkrijgen van de [verbindingsreeks](event-hubs-get-connection-string.md). De bootstrap.servers dat deze de FQDN-naam Event Hubs en de poort naar 9093 wijzigen. Bijwerken van de sasl.jaas.config als u wilt de Kafka-client naar het eindpunt van uw Event Hubs waarvoor Kafka is ingeschakeld (dit is de verbindingsreeks die u hebt verkregen), met de juiste verificatie, zoals hieronder weergegeven:

bootstrap.servers={Your. EVENT HUBS. FQDN-naam}: 9093 request.timeout.ms=60000 security.protocol=SASL_SSL sasl.mechanism=PLAIN sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule vereist gebruikersnaam = '$ConnectionString' wachtwoord = "{uw. EVENT HUBS. DE VERBINDING. TEKENREEKS} ";

Voorbeeld:

bootstrap.servers=dummynamespace.servicebus.Windows.NET:9093 request.timeout.ms=60000 security.protocol=SASL_SSL sasl.mechanism=PLAIN sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule vereist gebruikersnaam = "$ Password="Endpoint=sb://dummynamespace.servicebus.windows.net/ ConnectionString'; SharedAccessKeyName = DummyAccessKeyName; SharedAccessKey = 5dOntTRytoC24opYThisAsit3is2B + OGY1US/fuL3ly = ";

Opmerking: Als sasl.jaas.config geen ondersteunde configuratie in uw framework is, vindt u de configuraties die worden gebruikt voor het instellen van de SASL-gebruikersnaam en het wachtwoord en deze in plaats daarvan gebruiken. Stel de gebruikersnaam op $ConnectionString en het wachtwoord voor uw Event Hubs-verbindingsreeks.

### <a name="what-is-the-messageevent-size-for-kafka-enabled-event-hubs"></a>Wat is de bericht-/ gebeurtenisgrootte voor Event Hubs waarvoor Kafka is ingeschakeld?
De maximale berichtgrootte die is toegestaan voor de Event-Hubs voor Kafka-functionaliteit is 1MB.

## <a name="throughput-units"></a>Doorvoereenheden

### <a name="what-are-event-hubs-throughput-units"></a>Wat zijn Event hub-doorvoereenheden?
Doorvoer in Event Hubs definieert de hoeveelheid gegevens in bytes mega of het aantal (in duizendtallen) gebeurtenissen van 1 KB die inkomend en uitgaand verkeer via Event Hubs. Deze doorvoer wordt gemeten in doorvoereenheden (tabel). Doorvoereenheden aanschaffen voordat u kunt beginnen met behulp van de Event Hubs-service. U kunt expliciet Event Hubs-Doorvoereenheden selecteren met behulp van de portal of Event Hubs-Resource Manager-sjablonen. 


### <a name="do-throughput-units-apply-to-all-event-hubs-in-a-namespace"></a>Doorvoereenheden van toepassing op alle eventhubs in een naamruimte?
Ja, doorvoereenheden (Doorvoereenheden) van toepassing op alle eventhubs in een Event Hubs-naamruimte. Betekent dit dat u Doorvoereenheden op het niveau van de naamruimte aanschaft en worden gedeeld tussen de eventhubs in die naamruimte. Elke TU voorziet de naamruimte van de volgende mogelijkheden:

- Maximaal 1 MB per seconde aan ingangsgebeurtenissen (gebeurtenissen die worden verzonden naar een event hub), maar niet meer dan 1000 ingangsgebeurtenissen, beheerbewerkingen of besturingselement API-aanroepen per seconde.
- Maximaal 2 MB per seconde voor uitgaande gebeurtenissen (gebeurtenissen gebruikt vanaf een event hub), maar niet meer dan 4096 uitgaande gebeurtenissen.
- Maximaal 84 GB voor opslag van gebeurtenissen (voldoende voor de bewaartermijn voor 24 uur).

### <a name="how-are-throughput-units-billed"></a>Hoe worden doorvoereenheden gefactureerd?
Throughput units (Doorvoereenheden) worden gefactureerd op uurbasis. De facturering is gebaseerd op het maximum aantal eenheden dat tijdens het opgegeven uur is geselecteerd. 

### <a name="how-can-i-optimize-the-usage-on-my-throughput-units"></a>Hoe kan ik het gebruik van mijn doorvoereenheden optimaliseren?
U kunt slechts één doorvoereenheid mogelijk (TU) starten en inschakelen [automatisch vergroten](event-hubs-auto-inflate.md). Het automatisch vergroten functie kunt u uw Doorvoereenheden groeien als uw verkeer/nettolading toeneemt. U kunt ook een bovengrens instellen op het aantal Doorvoereenheden.

### <a name="how-does-auto-inflate-feature-of-event-hubs-work"></a>Hoe werkt de functie voor automatisch vergroten van Event Hubs?
De functie kunt u uw doorvoereenheden (Doorvoereenheden opschalen) automatisch vergroten. Betekent dit dat u kunt beginnen door lage Doorvoereenheden aanschaffen en automatisch schalen van uw Doorvoereenheden vergroten als uw invoer toeneemt. Het biedt een voordelige optie zijn en de volledige controle over het aantal Doorvoereenheden te beheren. Deze functie is een **scale-up alleen** functie, en u kunt zelf de volledige controle het omlaag schalen van het aantal Doorvoereenheden door deze bij te werken. 

U wilt beginnen met lage doorvoereenheden (tabel), bijvoorbeeld 2 Doorvoereenheden. Als u dat uw verkeer tot 15 Doorvoereenheden uitbreiden kan voorspellen inschakelen de functie op uw naamruimte automatisch vergroten en de maximale limiet ingesteld op 15 Doorvoereenheden. U kunt uw Doorvoereenheden nu automatisch groeien als uw verkeer toeneemt.

### <a name="is-there-a-cost-associated-when-i-turn-on-the-auto-inflate-feature"></a>Zijn er kosten wanneer ik inschakelen nadat de functie voor automatisch vergroten?
Er is **gratis** die zijn gekoppeld aan deze functie. 

### <a name="how-are-throughput-limits-enforced"></a>Hoe wordt de doorvoerlimieten toegepast?
Als de totale ingangsdoorvoer of de totale snelheid van ingangsgebeurtenissen voor alle eventhubs in een naamruimte groter is dan de cumulatieve throughput unit-limiet, worden de afzenders zijn beperkt en ontvangen ze foutberichten die aangeven dat het ingangsquotum is overschreden.

Als de totale uitgangsdoorvoer of de totale snelheid van uitgangsgebeurtenissen voor alle eventhubs in een naamruimte groter is dan de cumulatieve throughput unit-limiet, worden de ontvangers worden vertraagd en ontvangen ze foutberichten die aangeven dat het uitgangsquotum is overschreden. Inkomend en uitgaand verkeer worden afzonderlijk toegepast, waardoor afzenders geen vertraging voor gebeurtenisverbruik veroorzaken kan te vertragen, en niet kan een ontvanger te voorkomen dat gebeurtenissen worden verzonden naar een event hub.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-tus-that-can-be-reservedselected"></a>Is er een limiet voor het aantal doorvoereenheden (tabel) die gereserveerd/geselecteerd worden kan?
Op een aanbieding met meerdere tenants, kunnen doorvoereenheden meegroeien, maximaal 40 Doorvoereenheden (u kunt maximaal 20 Doorvoereenheden selecteert in de portal en een ondersteuningsticket om te verhogen naar 40 Doorvoereenheden op dezelfde naamruimte verhogen). Meer dan 40 Doorvoereenheden Event Hubs biedt het bron/capaciteit-model met de naam de **Event Hubs Dedicated clusters**. Toegewezen clusters worden verkocht in capaciteitseenheden (Cu's).

## <a name="dedicated-clusters"></a>Toegewezen clusters

### <a name="what-are-event-hubs-dedicated-clusters"></a>Wat zijn Event Hubs Dedicated clusters?
Event Hubs Dedicated-clusters bieden één tenant-implementaties voor klanten met de meest veeleisende behoeften. Deze aanbieding bouwt een cluster op basis van capaciteit die niet door doorvoereenheden is gekoppeld. Dit betekent dat u het cluster als u wilt opnemen en streamen van uw gegevens, zoals bepaald door de CPU- en geheugengebruik van het cluster kan gebruiken. Zie voor meer informatie, [Event Hubs Dedicated clusters](event-hubs-dedicated-overview.md).

### <a name="how-much-does-a-single-capacity-unit-let-me-achieve"></a>Hoeveel kiest een één capaciteitseenheid me bereiken?
Voor een specifieke cluster hoeveel u kunt opnemen en stream is afhankelijk van diverse factoren, zoals uw producenten, consumenten, de snelheid waarmee u kan opnemen en verwerken en nog veel meer. 

Onderstaande tabel ziet u de benchmarkresultaten dat we tijdens onze testen bereikt:

| Nettolading vorm | Ontvangers | Binnenkomende bandbreedte| Binnenkomende berichten | Uitgaande bandbreedte | Uitgaande berichten | Totaal aantal Doorvoereenheden | Doorvoereenheden per Capaciteitseenheid |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Batches van 100x1KB | 2 | 400 MB per seconde | 400 k berichten per seconde | 800 MB per seconde | 800 k berichten/sec | 400 Doorvoereenheden | 100 Doorvoereenheden | 
| Batches van 10x10KB | 2 | 666 MB per seconde | 66.6 k berichten/sec | 1.33 GB/sec | 133 k berichten/sec | 666 Doorvoereenheden | 166 Doorvoereenheden |
| Batches van 6x32KB | 1 | 1,05 GB/sec | 34 k berichten / sec | 1,05 GB/sec | 34 k berichten/sec | 1000 Doorvoereenheden | 250 Doorvoereenheden |

In het testen, is de volgende criteria gebruikt:

- Er is een specifieke Event Hubs-cluster met vier capaciteitseenheden (Cu's) gebruikt. 
- De event hub die wordt gebruikt voor gegevensopname heeft 200 partities. 
- De gegevens die is opgenomen is ontvangen door twee ontvanger toepassingen ontvangen van alle partities.

De resultaten geven u een idee van wat kan worden bereikt met een specifieke Event Hubs-cluster. Bovendien een cluster toewijzen wordt geleverd met de Event Hubs Capture ingeschakeld voor uw scenario's van microbatches als langdurige opslag.

### <a name="how-do-i-create-an-event-hubs-dedicated-cluster"></a>Hoe maak ik een Event Hubs Dedicated-cluster?
U een specifieke Event Hubs-cluster maken door het indienen van een [ondersteuningsaanvraag voor quotum toename](https://portal.azure.com/#create/Microsoft.Support) of neem contact op met de [team van Event Hubs](mailto:askeventhubs@microsoft.com). Het duurt meestal ongeveer twee weken voor het cluster geïmplementeerd en afgegeven aan door u worden gebruikt. Dit proces is tijdelijk totdat een volledige Self-service voor stroomactiviteitvoortgang beschikbaar wordt gesteld via de Azure portal of Azure Resource Manager-sjablonen, wat ongeveer twee uur duurt om het cluster te implementeren.

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="how-many-partitions-do-i-need"></a>Het aantal partities heb ik nodig?

Het aantal partities op een event hub kan niet worden gewijzigd na de installatie. Met die in rekening met is het belangrijk om na te denken over het aantal partities die u nodig hebt voordat u aan de slag. 

Eventhubs is ontworpen om toe te staan een lezer één partitie per consumergroep. Gebruik in de meeste gevallen is de standaardinstelling van vier partities voldoende. Als u schalen van de verwerking van gebeurtenissen wilt, kunt u overwegen extra partities toe te voegen. Er is geen doorvoerlimiet voor specifieke op een partitie, maar de geaggregeerde doorvoer in uw naamruimte wordt beperkt door het aantal doorvoereenheden. Als u het aantal throughput units in uw naamruimte verhoogt, kunt u extra partities om toe te staan van gelijktijdige lezers hun eigen maximale doorvoer te realiseren.

Echter, hebt u een model waarin uw toepassing een affiniteit met een bepaalde partitie heeft, waardoor het aantal partities mogelijk niet van een voordeel voor u. Zie voor meer informatie, [beschikbaarheid en consistentie](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Prijzen

### <a name="where-can-i-find-more-pricing-information"></a>Waar vind ik meer informatie over de prijzen?

Zie voor meer informatie over prijzen van Event Hubs, de [Event Hubs prijsinformatie](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Is er een post voor die gebeurtenissen van Event Hubs bewaren gedurende meer dan 24 uur?

De Event Hubs Standard-laag is toegestaan voor bewaarperiode van bericht perioden van meer dan 24 uur, maximaal zeven dagen. Als de grootte van het totale aantal opgeslagen gebeurtenissen de opslaglimiet voor het aantal geselecteerde doorvoereenheden (84 GB per doorvoereenheid) overschrijdt, de maximaal toegestane limiet overschrijdt, wordt in rekening gebracht tegen de gepubliceerde tarief voor de Azure Blob-opslag. De opslaglimiet voor elke throughput unit omvat alle opslagkosten voor bewaarperioden van 24 uur (de standaardinstelling), zelfs als de throughput unit in de limiet voor de maximale ingangslimiet is verbruikt.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Hoe wordt de Event Hubs-opslaggrootte berekend en in rekening gebracht?

De totale grootte van alle opgeslagen gebeurtenissen, inclusief interne overhead voor gebeurteniskoppen of opslagstructuren op schijf in alle eventhubs, wordt gedurende de hele dag gemeten. Aan het einde van de dag wordt de grootte van de piekopslag berekend. De dagelijkse opslaglimiet wordt berekend op basis van het minimum aantal doorvoereenhden dat gedurende de dag is geselecteerd (elke doorvoereenheid biedt een limiet van 84 GB). Als de totale grootte van de berekende dagelijkse opslaglimiet overschrijdt, de overtollige opslag wordt gefactureerd met behulp van Azure Blob storage-tarieven (op de **lokaal redundante opslag** tarief).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Hoe worden ingangsgebeurtenissen voor Event Hubs berekend?

Elke gebeurtenis verzonden naar een event hub telt als een factureerbaar bericht. Een *ingangsgebeurtenis* is gedefinieerd als een eenheid van de gegevens die kleiner is dan of gelijk is aan 64 KB. Elke gebeurtenis kleiner dan of gelijk is aan 64 KB groot is, wordt beschouwd als één factureerbare gebeurtenis. Als de gebeurtenis groter dan 64 KB is, wordt het aantal factureerbare gebeurtenissen berekend op basis van de gebeurtenisgrootte in veelvouden van 64 KB. Bijvoorbeeld, een 8 KB gebeurtenis verzonden naar de event hub wordt gefactureerd als één gebeurtenis, maar een bericht van 96 KB-bericht verzonden naar de event hub wordt gefactureerd als twee gebeurtenissen.

Gebeurtenissen gebruikt vanaf een event hub, evenals beheerbewerkingen en controle-aanroepen, zoals controlepunten, worden niet gerekend als factureerbare ingangsgebeurtenissen, maar doorlopen tot de limiet voor doorvoereenheden.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Gelden er kosten voor brokered Connections naar Event Hubs?

Verbindingskosten van toepassing alleen wanneer het AMQP-protocol wordt gebruikt. Er worden geen verbindingskosten in rekening gebracht voor het verzenden van gebeurtenissen via HTTP, ongeacht het aantal verzendsystemen/-apparaten. Als u van plan bent AMQP (bijvoorbeeld, voor een efficiëntere gebeurtenisstroom of om bi-directionele communicatie in IoT-opdracht inschakelen en beheren van scenario's) gebruiken, Zie de [informatie prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) pagina voor meer informatie over hoeveel verbindingen zijn opgenomen in elke servicelaag.

### <a name="how-is-event-hubs-capture-billed"></a>Hoe wordt Event Hubs Capture gefactureerd?

Vastleggen is ingeschakeld wanneer een event hub in de naamruimte de optie vastleggen is ingeschakeld is. Event Hubs Capture wordt per uur gefactureerd per aangeschafte doorvoereenheid. Als het aantal doorvoereenheden toe- of afneemt, Event Hubs Capture facturering deze wijzigingen in stappen van hele uren weerspiegeld worden weergegeven. Zie voor meer informatie over Event Hubs Capture facturering [informatie prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="do-i-get-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>In rekening gebracht voor het opslagaccount dat ik voor Event Hubs Capture Selecteer?

Vastleggen maakt gebruik van een opslagaccount dat die u verschaft wanneer dit is ingeschakeld op een event hub. Uw storage-account is, worden eventuele wijzigingen voor deze configuratie met uw Azure-abonnement gefactureerd.

## <a name="quotas"></a>Quota

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Zijn er geen quota's die zijn gekoppeld aan de Event Hubs?

Zie voor een lijst van alle Event Hubs-quota's, [quota](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Waarom kan ik geen een naamruimte maken na het verwijderen van een ander abonnement? 
Wanneer u een naamruimte uit een abonnement verwijdert, te wachten gedurende 4 uur voordat opnieuw worden gemaakt met dezelfde naam in een ander abonnement. 

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Wat zijn enkele van de uitzonderingen die worden gegenereerd door de Event Hubs en hun voorgestelde acties?

Zie voor een lijst van mogelijke uitzonderingen voor Event Hubs, [uitzonderingen overzicht](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Diagnostische logboeken

Eventhubs ondersteunt twee typen [diagnoselogboeken](event-hubs-diagnostic-logs.md) -foutenlogboeken en operationele logboeken - beide worden weergegeven in json en kunnen worden ingeschakeld via de Azure-portal vastleggen.

### <a name="support-and-sla"></a>Ondersteuning en SLA

Technische ondersteuning voor Event Hubs is beschikbaar via de [communityforums](https://social.msdn.microsoft.com/forums/azure/home?forum=servbus). Ondersteuning bij facturering en abonnementsbeheer is gratis.

Zie voor meer informatie over onze SLA de [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) pagina.

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Event Hubs automatisch vergroten](event-hubs-auto-inflate.md)

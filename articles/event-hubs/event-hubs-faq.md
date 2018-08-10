---
title: Veelgestelde vragen over Azure Eventhubs | Microsoft Docs
description: Azure Event Hubs Veelgestelde vragen (FAQ)
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 06/07/2018
ms.author: shvija
ms.openlocfilehash: 3e98d27c297e223231e0990adc51f8765678b884
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006382"
---
# <a name="event-hubs-frequently-asked-questions"></a>Veelgestelde vragen over Eventhubs

## <a name="general"></a>Algemeen

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Wat is het verschil tussen Event Hubs basis en standaard-laag?

De prijscategorie Standard van Azure Event Hubs biedt functies dan wat er beschikbaar is in de laag basis. De volgende functies zijn inbegrepen in Standard:

* Langere bewaartermijn van de gebeurtenis
* Extra brokered verbindingen met een vaste kosten voor meer dan het aantal opgenomen
* Meer dan één [consumentengroep](event-hubs-features.md#consumer-groups)
* [vastleggen](event-hubs-capture-overview.md)

Zie voor meer informatie over de prijzen van lagen, met inbegrip van Event Hubs Dedicated, de [Event Hubs prijsinformatie](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Wat zijn Event hub-doorvoereenheden?

U selecteren expliciet Event Hubs-doorvoereenheden, via de Azure-portal of de Event Hubs-Resource Manager-sjablonen. Doorvoereenheden zijn van toepassing op alle eventhubs in een Event Hubs-naamruimte, en elke throughput unit voorziet de naamruimte van de volgende mogelijkheden:

* Maximaal 1 MB per seconde aan ingangsgebeurtenissen (gebeurtenissen die worden verzonden naar een event hub), maar niet meer dan 1000 ingangsgebeurtenissen, beheerbewerkingen of besturingselement API-aanroepen per seconde.
* Maximaal 2 MB per seconde voor uitgaande gebeurtenissen (gebeurtenissen gebruikt vanaf een event hub), maar niet meer dan 4096 uitgaande gebeurtenissen.
* Maximaal 84 GB voor opslag van gebeurtenissen (voldoende voor de gebruikelijke bewaarperiode van 24 uur).

Event Hubs-doorvoereenheden worden per uur gefactureerd op basis van het maximum aantal eenheden dat tijdens het opgegeven uur is geselecteerd. U kunt automatisch [verhogen van het aantal doorvoereenheden met behulp van automatisch vergroten](event-hubs-auto-inflate.md) als uw gebruik toeneemt.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hoe worden Event Hubs-eenheid doorvoerlimieten toegepast?

Als de totale ingangsdoorvoer of de totale snelheid van ingangsgebeurtenissen voor alle eventhubs in een naamruimte groter is dan de cumulatieve throughput unit-limiet, worden de afzenders zijn beperkt en ontvangen ze foutberichten die aangeven dat het ingangsquotum is overschreden.

Als de totale uitgangsdoorvoer of de totale snelheid van uitgangsgebeurtenissen voor alle eventhubs in een naamruimte groter is dan de cumulatieve throughput unit-limiet, worden de ontvangers worden vertraagd en ontvangen ze foutberichten die aangeven dat het uitgangsquotum is overschreden. Inkomend en uitgaand verkeer worden afzonderlijk toegepast, waardoor afzenders geen vertraging voor gebeurtenisverbruik veroorzaken kan te vertragen, en niet kan een ontvanger te voorkomen dat gebeurtenissen worden verzonden naar een event hub.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Geldt er een limiet voor het aantal Throughput Units dat kan worden geselecteerd?

Er geldt een standaardquotum van 20 throughput units per naamruimte. U kunt een grotere quotum throughput Units aanvragen via een ondersteuningsticket. Voorbij de unit-limiet van 20 doorvoereenheden zijn bundels beschikbaar in 20-100 doorvoereenheden. Houd er rekening mee dat met behulp van meer dan 20 doorvoereenheden verwijdert u de mogelijkheid om te wijzigen van het aantal doorvoereenheden zonder een ondersteuningsticket in te dienen.

Met behulp van de [automatisch vergroten](event-hubs-auto-inflate.md) functie, kunt u automatisch het aantal doorvoereenheden als uw gebruik toeneemt verhogen.

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Kan ik één AMQP-verbinding gebruiken om te verzenden en ontvangen van meerdere eventhubs?

Ja, zolang alle eventhubs in dezelfde naamruimte zijn.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Wat is de maximale bewaarperiode voor gebeurtenissen?

Event Hubs Standard-laag biedt momenteel ondersteuning voor een maximale bewaarperiode van 7 dagen. Event Hubs zijn echter niet bedoeld als permanente gegevensopslag. Bewaarperioden langer dan 24 uur zijn bedoeld voor scenario's waarbij het handig is dat een gebeurtenisstroom dezelfde systemen; Als u bijvoorbeeld wilt trainen of te controleren of een nieuw model voor machine learning op bestaande gegevens. Als u de bewaarperiode na zeven dagen bericht moet, waardoor [Event Hubs Capture](event-hubs-capture-overview.md) op uw event hub haalt de gegevens uit uw event hub in de Storage-account of Azure Data Lake-Service-account van uw keuze. Capture inschakelen leidt tot een kosten in rekening gebracht op basis van de aangekochte doorvoereenheden.

### <a name="where-is-azure-event-hubs-available"></a>Waar is Azure Event Hubs beschikbaar?

Azure Event Hubs is beschikbaar in alle ondersteunde Azure-regio's. Voor een lijst, gaat u naar de [Azure-regio's](https://azure.microsoft.com/regions/) pagina.  

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="how-many-partitions-do-i-need"></a>Het aantal partities heb ik nodig?

Houd er rekening mee dat het aantal partities op een event hub kan niet worden gewijzigd na de installatie. Met die in rekening met is het belangrijk om na te denken over het aantal partities die u nodig hebt voordat u aan de slag. 

Eventhubs is ontworpen om toe te staan een lezer één partitie per consumergroep. Gebruik in de meeste gevallen is de standaardinstelling van vier partities voldoende. Als u schalen van de verwerking van gebeurtenissen wilt, kunt u overwegen extra partities toe te voegen. Er is geen doorvoerlimiet voor specifieke op een partitie, maar de geaggregeerde doorvoer in uw naamruimte wordt beperkt door het aantal doorvoereenheden. Als u het aantal throughput units in uw naamruimte verhoogt, kunt u extra partities om toe te staan van gelijktijdige lezers hun eigen maximale doorvoer te realiseren.

Echter, hebt u een model waarin uw toepassing een affiniteit met een bepaalde partitie heeft, waardoor het aantal partities mogelijk niet van een voordeel voor u. Zie voor meer informatie over deze [beschikbaarheid en consistentie](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Prijzen

### <a name="where-can-i-find-more-pricing-information"></a>Waar vind ik meer informatie over de prijzen?

Zie voor meer informatie over prijzen van Event Hubs, de [Event Hubs prijsinformatie](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Is er een post voor die gebeurtenissen van Event Hubs bewaren gedurende meer dan 24 uur?

De Event Hubs Standard-laag is toegestaan voor bewaarperiode van bericht perioden van meer dan 24 uur, maximaal 7 dagen. Als de grootte van het totale aantal opgeslagen gebeurtenissen de opslaglimiet voor het aantal geselecteerde doorvoereenheden (84 GB per doorvoereenheid) overschrijdt, de maximaal toegestane limiet overschrijdt, wordt in rekening gebracht tegen de gepubliceerde tarief voor de Azure Blob-opslag. De opslaglimiet voor elke throughput unit omvat alle opslagkosten voor bewaarperioden van 24 uur (de standaardinstelling), zelfs als de throughput unit in de limiet voor de maximale ingangslimiet is verbruikt.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Hoe wordt de Event Hubs-opslaggrootte berekend en in rekening gebracht?

De totale grootte van alle opgeslagen gebeurtenissen, inclusief interne overhead voor gebeurteniskoppen of opslagstructuren op schijf in alle eventhubs, wordt gedurende de hele dag gemeten. Aan het einde van de dag wordt de grootte van de piekopslag berekend. De dagelijkse opslaglimiet wordt berekend op basis van het minimum aantal doorvoereenhden dat gedurende de dag is geselecteerd (elke doorvoereenheid biedt een limiet van 84 GB). Als de totale grootte van de berekende dagelijkse opslaglimiet overschrijdt, de overtollige opslag wordt gefactureerd met behulp van Azure Blob storage-tarieven (op de **lokaal redundante opslag** tarief).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Hoe worden ingangsgebeurtenissen voor Event Hubs berekend?

Elke gebeurtenis verzonden naar een event hub telt als een factureerbaar bericht. Een *ingangsgebeurtenis* is gedefinieerd als een eenheid van de gegevens die kleiner is dan of gelijk is aan 64 KB. Elke gebeurtenis kleiner dan of gelijk is aan 64 KB groot is, wordt beschouwd als één factureerbare gebeurtenis. Als de gebeurtenis groter dan 64 KB is, wordt het aantal factureerbare gebeurtenissen berekend op basis van de gebeurtenisgrootte in veelvouden van 64 KB. Bijvoorbeeld, een gebeurtenis van 8 KB verzonden naar de event hub wordt gefactureerd als één gebeurtenis, maar een bericht van 96 KB verzonden naar de event hub wordt gefactureerd als twee gebeurtenissen.

Gebeurtenissen gebruikt vanaf een event hub, evenals beheerbewerkingen en controle-aanroepen, zoals controlepunten, worden niet gerekend als factureerbare ingangsgebeurtenissen, maar doorlopen tot de limiet voor doorvoereenheden.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Gelden er kosten voor brokered Connections naar Event Hubs?

Verbindingskosten van toepassing alleen wanneer het AMQP-protocol wordt gebruikt. Er worden geen verbindingskosten in rekening gebracht voor het verzenden van gebeurtenissen via HTTP, ongeacht het aantal verzendsystemen/-apparaten. Als u van plan bent AMQP (bijvoorbeeld, voor een efficiëntere gebeurtenisstroom of om bi-directionele communicatie in IoT-opdracht inschakelen en beheren van scenario's) gebruiken, Zie de [informatie prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) pagina voor meer informatie over hoeveel verbindingen zijn opgenomen in elke servicelaag.

### <a name="how-is-event-hubs-capture-billed"></a>Hoe wordt Event Hubs Capture gefactureerd?

Vastleggen is ingeschakeld wanneer een event hub in de naamruimte de optie vastleggen is ingeschakeld is. Event Hubs Capture wordt per uur gefactureerd per aangeschafte doorvoereenheid. Als het aantal doorvoereenheden toe- of afneemt, Event Hubs Capture facturering deze wijzigingen in stappen van hele uren weerspiegeld worden weergegeven. Zie voor meer informatie over Event Hubs Capture facturering [informatie prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Wordt ik gefactureerd voor het opslagaccount dat ik voor Event Hubs Capture Selecteer?

Vastleggen maakt gebruik van een opslagaccount dat die u verschaft wanneer dit is ingeschakeld op een event hub. Omdat dit uw storage-account, worden eventuele wijzigingen voor deze configuratie met uw Azure-abonnement gefactureerd.

## <a name="quotas"></a>Quota

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Zijn er geen quota's die zijn gekoppeld aan de Event Hubs?

Zie voor een lijst van alle Event Hubs-quota's, [quota](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Problemen oplossen

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

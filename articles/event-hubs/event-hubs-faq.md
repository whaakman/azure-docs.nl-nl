---
title: Azure Event Hubs Veelgestelde vragen | Microsoft Docs
description: Azure Event Hubs Veelgestelde vragen (FAQ)
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: bfa10984-eb22-4671-861a-f377a90d9372
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: c4faa071c4f2401fe3e852e787e3b7d4da0c7d44
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-frequently-asked-questions"></a>Veelgestelde vragen over Event Hubs

## <a name="general"></a>Algemeen

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Wat is het verschil tussen Event Hubs Basic en Standard lagen?

De prijscategorie Standard van Azure Event Hubs biedt functies afgezien van wat er beschikbaar is in de basisstaffel. De volgende functies zijn opgenomen in de standaard:
* Langere bewaartermijn van gebeurtenis
* Extra brokered verbindingen met een overschrijding kosten voor meer dan het aantal opgenomen
* Meer dan één groep Consumer
* [Vastleggen](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)

Zie voor meer informatie over prijzen voor lagen, waaronder Event Hubs is toegewezen, de [Event Hubs prijsinformatie](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Wat zijn de doorvoereenheden Event Hubs?

U selecteren expliciet Event Hubs-doorvoereenheden, via de Azure-portal of Event Hubs Resource Manager-sjablonen. Doorvoereenheden zijn van toepassing op alle event hubs in een Event Hubs-naamruimte en elke doorvoereenheid kunt de naamruimte met de volgende mogelijkheden:

* Up 1 MB per seconde van ingangsgebeurtenissen (gebeurtenissen verzonden naar een event hub), maar niet meer dan 1000 ingangsgebeurtenissen, beheerbewerkingen of besturingselement API-aanroepen per seconde.
* Tot 2 MB per seconde van uitgaande gebeurtenissen (gebeurtenissen van een event hub gebruikt).
* 84 GB gebeurtenis opslag (volstaat voor de bewaartermijn voor 24 uur).

Event Hubs doorvoereenheden per uur worden gefactureerd, op basis van het maximum aantal eenheden dat is geselecteerd in het opgegeven uur. U kunt automatisch [Verhoog het aantal doorvoereenheden](event-hubs-auto-inflate.md) als uw gebruik toeneemt.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hoe kan ik Event Hubs eenheid doorvoerlimieten afgedwongen?
De doorvoer totaal inkomend of de snelheid van de totale inkomende gebeurtenissen over alle event hubs in een naamruimte groter is dan de geaggregeerde doorvoer eenheid rechten, afzenders zijn beperkt als er fouten optreden die aangeeft dat het inkomend quotum is overschreden.

Het totaal aantal uitgaande doorvoer of de snelheid van het totaal aantal gebeurtenissen uitgaande over alle event hubs in een naamruimte groter is dan de geaggregeerde doorvoer eenheid rechten, ontvangers zijn beperkt als er fouten optreden die aangeeft dat het uitgaande quotum is overschreden. Inkomende en uitgaande quota worden afzonderlijk afgedwongen, zodat geen afzender leiden gebeurtenisgebruik tot kan te verlagen en kan een ontvanger te voorkomen dat gebeurtenissen in een event hub worden verzonden.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Geldt er een limiet voor het aantal Throughput Units dat kan worden geselecteerd?
Er is een standaardquotum van 20 doorvoereenheden per naamruimte. U kunt een grotere quotum van doorvoereenheden aanvragen door het indienen van een ondersteuningsticket. De limiet overschrijdt van 20 doorvoereenheden eenheid zijn bundels beschikbaar in 20-100 doorvoereenheden. Houd er rekening mee dat de mogelijkheid het aantal doorvoereenheden wijzigen zonder het indienen van een ondersteuningsticket met behulp van meer dan 20 doorvoereenheden verwijderd.

Met behulp van de [automatisch vergroten](event-hubs-auto-inflate.md) functie, kunt u automatisch het aantal doorvoereenheden als uw gebruik toeneemt verhogen.

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Kan ik een enkele AMQP-verbinding verzenden en ontvangen van meerdere event hubs gebruiken?
Ja, zolang alle event hubs in dezelfde naamruimte zijn.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Wat is de maximale bewaarperiode voor gebeurtenissen?
Event Hubs standaardcategorie ondersteunt momenteel een maximale bewaarperiode van 7 dagen. Houd er rekening mee event hubs zijn niet bedoeld als een permanente gegevensarchief. De bewaarperiode is langer dan 24 uur zijn bedoeld voor scenario's waarin het is handig voor de replay van een stroom gebeurtenissen in de dezelfde systemen; Als u bijvoorbeeld wilt trainen of een nieuwe machine learning-model op bestaande gegevens te verifiëren. Als u bewaren dan 7 dagen-bericht moet, waardoor [Event Hubs vastleggen](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) op uw event hub de gegevens ophaalt uit uw event hub aan de Storage-account of een Azure Data Lake-Service-account van uw keuze. Een kosten op basis van uw aangeschafte doorvoereenheid leidt ertoe dat vastleggen inschakelen.

### <a name="where-is-azure-event-hubs-available"></a>Waar Azure Event Hubs beschikbaar is?
Azure Event Hubs is beschikbaar in alle ondersteunde Azure-regio's. Voor een lijst, gaat u naar de [Azure-regio's](https://azure.microsoft.com/regions/) pagina.  

## <a name="best-practices"></a>Aanbevolen procedures

### <a name="how-many-partitions-do-i-need"></a>Het aantal partities moet ik gebruiken?
Houd er rekening mee dat de partitie rekenen op een event hub worden niet moet gewijzigd na de installatie. Met daarom is het belangrijk om na te denken over hoeveel partities die u nodig hebt voordat u aan de slag. 

Event Hubs is zo ontworpen dat een lezer één partitie per consumergroep. Gebruik in de meeste gevallen is de standaardinstelling van vier partities is voldoende. Als u wilt schalen van de verwerking van gebeurtenissen, kunt u overwegen extra partities toe te voegen. Er is geen limiet specifieke doorvoer op een andere partitie, maar de geaggregeerde doorvoer in uw naamruimte wordt beperkt door het aantal doorvoereenheden. Als u het aantal doorvoereenheden in uw naamruimte verhoogt, kunt u extra partities om toe te staan van gelijktijdige lezers hun eigen maximale doorvoer te bereiken.

Echter, als er een model waarin uw toepassing een affiniteit met een bepaalde partitie heeft, waardoor het aantal partities mogelijk niet van voordelen voor u. Zie voor meer informatie over deze [beschikbaarheid en consistentie](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Prijzen

### <a name="where-can-i-find-more-pricing-information"></a>Waar vind ik meer informatie over de prijzen
Zie voor meer informatie over prijzen van Event Hubs de [Event Hubs prijsinformatie](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Is er kosten met zich mee voor het bewaren van gebeurtenissen van Event Hubs voor meer dan 24 uur?
De Event Hubs standaardcategorie staat toe dat bericht bewaren perioden van meer dan 24 uur, maximaal 7 dagen. Als de grootte van het totale aantal opgeslagen gebeurtenissen overschrijdt de toegestane opslag voor het aantal geselecteerde doorvoereenheden (84 GB per doorvoereenheid), de grootte die langer is dan de toegestane wordt in rekening gebracht gepubliceerde Azure Blob storage. De toegestane opslag in elke doorvoereenheid bevat informatie over alle kosten voor opslag voor de bewaarperiode van 24 uur (standaardinstelling) zelfs als de doorvoereenheid van wordt gebruikt voor de toegestane maximale inkomende gegevens.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Hoe wordt de maximale grootte van het Event Hubs berekend en in rekening gebracht?
De totale grootte van alle opgeslagen gebeurtenissen, met inbegrip van een interne overhead voor gebeurtenissen, kopteksten of op schijf opslag structuren in alle event hubs wordt gemeten gedurende de dag. Aan het einde van de dag wordt de grootte van de piekopslag berekend. De dagelijkse opslag vergoeding wordt berekend op basis van het minimum aantal doorvoereenheden die zijn geselecteerd tijdens de dag (elke doorvoereenheid biedt een correctie van 84 GB). Als de totale grootte de berekende dagelijkse vergoeding voor opslag overschrijdt, de overtollige opslag wordt gefactureerd met behulp van Azure Blob storage tarieven (op het **lokaal redundante opslag** snelheid).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Hoe kan ik Event Hubs ingangsgebeurtenissen berekend?
Elke gebeurtenis verzonden naar een event hub, telt als een factureerbare bericht. Een *inkomend gebeurtenis* is gedefinieerd als een gegevenseenheid die kleiner is dan of gelijk aan 64 KB. De gebeurtenis die kleiner is dan of gelijk aan 64 KB groot wordt beschouwd als een factureerbare gebeurtenis. Als de gebeurtenis groter dan 64 KB is, wordt het aantal gebeurtenissen factureerbare berekend afhankelijk van de gebeurtenisgrootte, in veelvouden van 64 KB. Bijvoorbeeld, een 8 KB gebeurtenis verzonden naar de event hub wordt gefactureerd als een gebeurtenis, maar een 96 KB-bericht verzonden naar de event hub wordt gefactureerd als twee gebeurtenissen.

Gebeurtenissen van een event hub, alsmede beheerbewerkingen en besturingselement aanroepen zoals controlepunten worden niet meegeteld als factureerbare ingangsgebeurtenissen maar doorlopen tot de doorvoer eenheid toegestane verbruikt.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Pas kosten brokered verbinding naar Event Hubs?
Verbinding gelden alleen wanneer het AMQP-protocol wordt gebruikt. Er zijn geen kosten verbinding voor het verzenden van gebeurtenissen met behulp van HTTP, ongeacht het aantal systemen of apparaten verzenden. Als u van plan bent te AMQP (bijvoorbeeld als u de streaming-efficiënter gebeurtenis of te bidirectionele communicatie in IoT-opdracht inschakelen en beheren van scenario's) gebruiken, raadpleegt u de [Event Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/) pagina voor meer informatie over het aantal verbindingen zijn opgenomen in elke servicecategorie.

### <a name="how-is-event-hubs-capture-billed"></a>Hoe wordt Event Hubs Capture gefactureerd?
Vastleggen is ingeschakeld wanneer een event hub in de naamruimte de optie vastleggen is ingeschakeld is. Vastleggen van Event Hubs wordt per uur gefactureerd per doorvoereenheid hebt aangeschaft. Als het aantal van de eenheid doorvoer wordt vergroot of verkleind, weerspiegelt Event Hubs vastleggen facturering deze wijzigingen in de hele intervallen van uren. Zie voor meer informatie over Event Hubs vastleggen facturering [Event Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Ik krijgt voor het opslagaccount dat ik Selecteer voor het vastleggen van Event Hubs?
Vastleggen maakt gebruik van een opslagaccount dat u opgeeft wanneer ingeschakeld voor een event hub. Als dit uw storage-account is, worden eventuele wijzigingen voor deze configuratie gefactureerd op uw Azure-abonnement.

## <a name="quotas"></a>Quota

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Zijn er geen quota's die zijn gekoppeld aan de Event Hubs?
Zie voor een lijst van alle Event Hubs quota [quota](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Wat zijn enkele van de uitzonderingen die worden gegenereerd door de Event Hubs en hun voorgestelde acties?
Zie voor een lijst van mogelijke Event Hubs uitzonderingen [uitzonderingen overzicht](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Diagnostische logboeken
Event Hubs ondersteunt twee typen [diagnostische logboeken](event-hubs-diagnostic-logs.md) -foutenlogboeken en operationele logboeken - die beide worden weergegeven in json en kunnen worden ingeschakeld via de Azure portal vastleggen.

### <a name="support-and-sla"></a>Ondersteuning en SLA
Technische ondersteuning voor Event Hubs is beschikbaar via de [communityforums](https://social.msdn.microsoft.com/forums/azure/home). Ondersteuning bij facturering en abonnementsbeheer is gratis.

Zie voor meer informatie over onze SLA, de [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) pagina.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Automatische-vergroten van Event Hubs](event-hubs-auto-inflate.md)

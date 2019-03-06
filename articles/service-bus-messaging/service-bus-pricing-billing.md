---
title: Service Bus prijzen en facturering | Microsoft Docs
description: Overzicht van Service Bus prijsstructuur.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2019
ms.author: aschhab
ms.openlocfilehash: 859add6c9310cf5f18ed7090c8e93d4896b59a0b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433379"
---
# <a name="service-bus-pricing-and-billing"></a>Service Bus prijzen en facturering

Azure Service Bus is beschikbaar in Standard en [Premium](service-bus-premium-messaging.md) lagen. U kunt een servicelaag voor elke Service Bus-Servicenaamruimte die u maakt en deze selectie laag geldt voor alle entiteiten die in die naamruimte gemaakt.

> [!NOTE]
> Zie voor gedetailleerde informatie over de huidige prijzen van Service Bus, de [pagina met prijzen van Azure Service Bus](https://azure.microsoft.com/pricing/details/service-bus/), en de [Veelgestelde vragen over Service Bus](service-bus-faq.md#pricing).
>
>

Service Bus maakt gebruik van de volgende 2 meters voor wachtrijen en onderwerpen/abonnementen:

1. **Berichtbewerkingen**: Gedefinieerd als API-aanroepen voor service-eindpunten van wachtrijen of onderwerpen/abonnementen. Deze meter vervangt berichten verzonden of ontvangen als de primaire werkeenheid factureerbare gebruik voor wachtrijen en onderwerpen/abonnementen.
2. **Brokered Connections**: Gedefinieerd als het hoogste aantal permanente verbindingen geopend voor wachtrijen, onderwerpen of abonnementen tijdens een opgegeven testperiode van één uur. Deze meter is alleen van toepassing in de laag standaard, waarin u extra verbindingen kunt openen (verbindingen werden voorheen beperkt tot 100 per wachtrij/onderwerp/abonnement) tegen een nominaal bedrag per verbinding.

De **Standard** laag introduceert gestaffelde prijzen voor bewerkingen die worden uitgevoerd met wachtrijen en onderwerpen/abonnementen, wat resulteert in op basis van een volume kortingen tot wel 80% op het hoogste gebruiksniveau van het. Er is ook een Standard-laag basiskosten van $10 per maand; deze kunt u maximaal 12,5 miljoen bewerkingen per maand gratis uit te voeren.

De **Premium** laag biedt isolatie van resources op het niveau van de CPU en geheugen, zodat elke workload van een klant geïsoleerd wordt uitgevoerd. Deze resourcecontainer wordt een *Messaging-eenheid* genoemd. Aan elke Premium-naamruimte wordt ten minste één Messaging-eenheid toegewezen. U kunt voor elke Service Bus Premium-naamruimte 1, 2 of 4 Messaging-eenheden aanschaffen. Een enkele workload of entiteit kan meerdere Messaging-eenheden omspannen en het aantal Messaging-eenheden kan naar wens worden gewijzigd, hoewel facturering plaatsvindt tegen een 24-uurs of dagelijks tarief. Dit resulteert in voorspelbare en herhaalbare prestaties voor uw Service Bus-oplossing. Niet alleen zijn de prestaties beter voorspelbaar en beschikbaar, ze zijn ook sneller.

> [!NOTE]
> Onderwerpen en -abonnementen zijn alleen beschikbaar in de Standard of Premium-Prijscategorieën; de Basic-laag ondersteunt alleen wachtrijen.

De basiskosten voor Standard-laag wordt in rekening gebracht slechts één keer per maand per Azure-abonnement. Dit betekent dat wanneer u een één Standard-laag Service Bus-naamruimte hebt gemaakt, kunt u zoveel extra Standard-naamruimten die u onder hetzelfde Azure-abonnement, wilt zonder dat er extra basiskosten in rekening gebracht.

De [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) tabel bevat een overzicht van de functionele verschillen tussen de lagen standaard en Premium.

## <a name="messaging-operations"></a>Berichtbewerkingen

Wachtrijen en onderwerpen/abonnementen worden gefactureerd per 'bewerking', niet per bericht. Een bewerking verwijst naar een API-aanroep ten opzichte van een wachtrij of onderwerp/abonnement service-eindpunt. Dit zijn onder andere de bewerkingen voor beheer, verzenden/ontvangen en sessiestatus.

| Type bewerking | Description |
| --- | --- |
| Beheer |Maken, lezen, bijwerken, verwijderen (CRUD) aan de hand wachtrijen of onderwerpen/abonnementen. |
| Berichten |Verzenden en ontvangen van berichten met wachtrijen of onderwerpen/abonnementen. |
| Sessiestatus |Ophalen of instellen van status van de sessie op een wachtrij of onderwerp/abonnement. |

Voor kosten voor meer informatie, Zie de prijzen die worden vermeld op de [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) pagina.

## <a name="brokered-connections"></a>Brokered Connections

*Brokered verbindingen* geschikt voor gebruikspatronen die betrekking hebben op een groot aantal 'permanent verbonden' afzenders/ontvangers voor wachtrijen, onderwerpen of abonnementen. Permanent verbonden afzenders/ontvangers zijn die verbinding maken met behulp van AMQP of HTTP met een niet-nul ontvangen time-out (bijvoorbeeld HTTP-long polling). HTTP-afzenders en ontvangers met een direct time-out genereren geen brokered verbindingen.

Zie voor verbinding quota's en andere Servicelimieten de [Service Bus-quota](service-bus-quotas.md) artikel. Zie voor meer informatie over brokered verbindingen de [Veelgestelde vragen over](#faq) verderop in dit artikel.

De Standard-laag Hiermee verwijdert u de limiet van de verwerkte verbindingen per naamruimte en brokered connection met statistische gebruik telt via het Azure-abonnement. Zie voor meer informatie de [Brokered connections](https://azure.microsoft.com/pricing/details/service-bus/) tabel.

> [!NOTE]
> 1.000 brokered connections zijn opgenomen in de standaardlaag voor berichten (via de basiskosten) en kunnen worden gedeeld met alle wachtrijen, onderwerpen en abonnementen binnen het bijbehorende Azure-abonnement.
>
>

<br />

> [!NOTE]
> De facturering is gebaseerd op het hoogste aantal gelijktijdige verbindingen en wordt pro rata per uur op basis van 730 uur per maand.
>
>

### <a name="premium-tier"></a>Premium-laag

Voor Brokered Connections wordt niets in rekening gebracht op de Premium-laag.

## <a name="faq"></a>Veelgestelde vragen

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Wat zijn brokered verbindingen en hoe ik gebracht?

Een verwerkte verbinding wordt gedefinieerd als:

1. Een AMQP-verbinding van een client naar een Service Bus-wachtrij of onderwerp/abonnement.
2. Een HTTP-oproep om een bericht te ontvangen van een Service Bus-onderwerp of -wachtrij waarvoor een time-out voor ontvangen geldt met waarde die hoger is dan nul.

Service Bus-kosten voor het hoogste aantal gelijktijdige brokered verbindingen die groter zijn dan het inbegrepen aantal (1000 in de Standard-laag). Pieken worden berekend op uurbasis, pro rata door te delen door 730 uur per maand en opgeteld gedurende de maandelijkse factureringsperiode. Het inbegrepen aantal (1.000 Brokered Connections per maand) wordt toegepast aan het einde van de factureringsperiode tegen het totaal van de pieken per uur pro rata.

Bijvoorbeeld:

1. Elk van 10.000 apparaten verbindt via één AMQP-verbinding en daarvan opdrachten ontvangt van een Service Bus-onderwerp. De apparaten verstuurt telemetriegebeurtenissen naar een Event Hub. Als alle apparaten elke dag 12 uur verbinding maken, zijn van toepassing de volgende verbindingskosten berekend (naast eventuele andere Service Bus-onderwerp kosten): 10.000 verbindingen * 12 uur * 30 dagen / 730 = 4,931 brokered verbindingen. Na aftrek van de maandelijkse 1.000 brokered connections, u kosten in rekening worden gebracht 4.000 brokered Connections, tegen het tarief van $0,03 per brokered connection, met een totaal van $120.
2. 10.000 apparaten ontvangen berichten van een Service Bus-wachtrij via HTTP, een time-out van nul seconden op te geven. Als alle apparaten elke dag 12 uur verbinding maken, ziet u de volgende verbindingskosten berekend (naast eventuele andere Service Bus-kosten): 10.000 verbindingen HTTP voor ontvangen * 12 uur per dag * 30 dagen / 730 uur = 4,931 brokered verbindingen.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Zijn kosten voor Brokered Connection van toepassing op wachtrijen en onderwerpen/abonnementen?

Ja. Er worden geen verbindingskosten in rekening gebracht voor het verzenden van gebeurtenissen via HTTP, ongeacht het aantal verzendsystemen/-apparaten. Ontvangen van gebeurtenissen via HTTP met een time-out groter dan nul, ook wel 'long polling', genereert kosten voor brokered Connections. AMQP-verbindingen genereren kosten voor Brokered Connections ongeacht of de verbindingen worden gebruikt om te verzenden of te ontvangen. De eerste 1.000 brokered connections in alle standaardnaamruimtes in een Azure-abonnement zijn inbegrepen geen extra kosten in rekening gebracht (naast de basiskosten). Aangezien deze aantallen voldoende zijn voor veel service-naar-service-berichtscenario's, pas kosten voor brokered Connections meestal relevant als u van plan bent AMQP of HTTP long-polling gebruiken met een groot aantal clients; Als u bijvoorbeeld een efficiëntere gebeurtenisstroom of om bi-directionele communicatie mogelijk met veel apparaten of exemplaren van de toepassing.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de prijzen van Service Bus, de [Service Bus-pagina met prijzen](https://azure.microsoft.com/pricing/details/service-bus/).
* Zie de [Veelgestelde vragen over Service Bus](service-bus-faq.md#pricing) voor sommige algemene veelgestelde vragen over servicebus prijzen en facturering.

[Azure portal]: https://portal.azure.com

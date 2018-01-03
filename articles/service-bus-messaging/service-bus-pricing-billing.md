---
title: Service Bus-prijzen en facturering | Microsoft Docs
description: Overzicht van Service Bus structuur prijzen.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 8ccb44b5009588c28bc79bb45e1a7640ead6c817
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="service-bus-pricing-and-billing"></a>Service Bus-prijzen en facturering

Azure Service Bus in de standaardmodus wordt aangeboden en [Premium](service-bus-premium-messaging.md) lagen. U kunt een servicelaag voor elke Service Bus-Servicenaamruimte die u maakt en deze selectie laag kan toepassen op alle entiteiten in die naamruimte gemaakt.

> [!NOTE]
> Zie voor gedetailleerde informatie over prijzen voor huidige Service Bus de [pagina met prijzen van Azure Service Bus](https://azure.microsoft.com/pricing/details/service-bus/), en de [Veelgestelde vragen over de Service Bus](service-bus-faq.md#pricing).
>
>

Service Bus maakt gebruik van de volgende 2 meter voor wachtrijen en onderwerpen/abonnementen:

1. **Messaging-bewerkingen**: gedefinieerd als de API-aanroepen op basis van de wachtrij of onderwerp/abonnement service-eindpunten. Deze meter vervangt berichten verzonden of ontvangen als de primaire werkeenheid factureerbare schijfgebruik voor wachtrijen en onderwerpen/abonnementen.
2. **Brokered verbindingen**: gedefinieerd als het maximum aantal permanente verbindingen op basis van wachtrijen, onderwerpen en abonnementen tijdens een opgegeven testperiode van één uur openen. Deze meter is alleen van toepassing in de prijscategorie Standard, waarin u, is een extra verbindingen openen kunt (verbindingen werden voorheen beperkt tot 100 per onderwerp-wachtrij-abonnement) voor de kosten van een nominaal verbinding.

De **standaard** laag introduceert geleidelijk prijzen voor bewerkingen die worden uitgevoerd met wachtrijen en onderwerpen/abonnementen, wat resulteert in op basis van een volume kortingen tot 80% op het hoogste niveau van de informatie over het gebruik. Er is ook een Standard-laag base kosten van $10 per maand, die u kunt uitvoeren 12,5 miljoen operations per maand zonder extra kosten.

De **Premium** laag biedt isolatie van resources op de laag van CPU en geheugen, zodat elke workload van een klant geïsoleerd wordt uitgevoerd. Deze resourcecontainer wordt een *Messaging-eenheid* genoemd. Aan elke Premium-naamruimte wordt ten minste één Messaging-eenheid toegewezen. U kunt voor elke Service Bus Premium-naamruimte 1, 2 of 4 Messaging-eenheden aanschaffen. Een enkele workload of entiteit kan meerdere Messaging-eenheden omspannen en het aantal Messaging-eenheden kan naar wens worden gewijzigd, hoewel facturering plaatsvindt tegen een 24-uurs of dagelijks tarief. Dit resulteert in voorspelbare en herhaalbare prestaties voor uw Service Bus-oplossing. Niet alleen zijn de prestaties beter voorspelbaar en beschikbaar, ze zijn ook sneller. 

Houd er rekening mee dat de prijscategorie Standard base kosten in rekening wordt gebracht slechts één keer per maand per Azure-abonnement. Dit betekent dat wanneer u een één standaard-laag Service Bus-naamruimte gemaakt, kunt u zo veel extra Standard naamruimten gewenste onder die dezelfde Azure-abonnement, zonder extra kosten voor basis kunt maken.

De [Service Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/) tabel ziet u de functionele verschillen tussen de lagen Standard en Premium.

## <a name="messaging-operations"></a>Berichtbewerkingen

Wachtrijen en onderwerpen/abonnementen wordt gefactureerd per 'bewerking', niet per bericht. Een bewerking verwijst naar een API-aanroep ten opzichte van een wachtrij of onderwerp/abonnement service-eindpunt. Dit zijn onder andere de bewerkingen voor beheer, verzenden/ontvangen en sessiestatus.

| Type bewerking | Beschrijving |
| --- | --- |
| Beheer |Maken, lezen, bijwerken, verwijderen (CRUD) op basis van wachtrijen en onderwerpen/abonnementen. |
| Berichten |Verzenden en ontvangen berichten met wachtrijen en onderwerpen/abonnementen. |
| Sessiestatus |Ophalen of instellen van status van de sessie op een wachtrij of onderwerp/abonnement. |

Zie voor meer informatie kosten de prijzen weergegeven op de [Service Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/) pagina.

## <a name="brokered-connections"></a>Brokered Connections

*Brokered verbindingen* ruimte gebruikspatronen die betrekking hebben op een groot aantal 'permanent verbonden' afzenders/ontvangers op basis van wachtrijen, onderwerpen en abonnementen. Permanent verbonden afzenders/ontvangers zijn die verbinding maken via de AMQP of HTTP met een niet-nul time-out (bijvoorbeeld HTTP-lange polling) ontvangen. HTTP-afzenders en ontvangers met een onmiddellijke time-out genereren geen brokered verbindingen.

Zie voor verbinding quota's en andere Servicelimieten de [Service Bus-quota](service-bus-quotas.md) artikel. Zie voor meer informatie over brokered verbindingen de [Veelgestelde vragen over](#faq) verderop in dit artikel.

De prijscategorie Standard verwijdert de brokered verbindingslimiet per naamruimte en telt gebruik cumulatieve brokered verbinding via het Azure-abonnement. Zie voor meer informatie de [Brokered verbindingen](https://azure.microsoft.com/pricing/details/service-bus/) tabel.

> [!NOTE]
> 1000 brokered verbindingen zijn opgenomen in de prijscategorie Standard messaging (via de base kosten) en kunnen worden gedeeld door alle wachtrijen, onderwerpen en abonnementen in het gekoppelde Azure-abonnement.
>
>

<br />

> [!NOTE]
> De facturering is gebaseerd op het hoogste aantal gelijktijdige verbindingen en wordt per uur naar rato bepaald op basis van 744 uur per maand.
>
>

### <a name="premium-tier"></a>Premium-laag

Voor Brokered Connections wordt niets in rekening gebracht op de Premium-laag.

## <a name="faq"></a>Veelgestelde vragen

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Wat zijn brokered verbindingen en hoe ik ophalen in rekening gebracht voor hen?

Een verwerkte verbinding wordt gedefinieerd als:

1. Een AMQP-verbinding van een client aan een Service Bus-wachtrij of onderwerp/abonnement.
2. Een HTTP-oproep om een bericht te ontvangen van een Service Bus-onderwerp of -wachtrij waarvoor een time-out voor ontvangen geldt met waarde die hoger is dan nul.

Service Bus-kosten voor het maximum aantal gelijktijdige brokered verbindingen die groter is dan de inbegrepen aantal (1000 in de prijscategorie Standard). Pieken worden berekend op uurbasis, pro rata door te delen door 744 uur in een maand en opgeteld gedurende de maandelijkse factureringsperiode. Het inbegrepen aantal (1.000 Brokered Connections per maand) wordt toegepast aan het einde van de factureringsperiode tegen het totaal van de pieken per uur pro rata.

Bijvoorbeeld:

1. Elk van 10.000 apparaten verbindt via een enkele AMQP-verbinding en opdrachten ontvangt van een Service Bus-onderwerp. De apparaten verzenden telemetrische gebeurtenissen naar een Event Hub. Als alle apparaten verbinding voor 12 uur per dag maken, de volgende verbinding gelden (naast eventuele andere Service Bus-onderwerp kosten): 10.000 verbindingen * 12 uur * 31 dagen / 744 = 5.000 brokered verbindingen. Na het maandelijkse tegoed van 1000 brokered verbindingen u zou worden in rekening gebracht voor 4000 brokered verbindingen met een snelheid van $0,03 per brokered-verbinding voor een totaal van $120.
2. 10.000 apparaten ontvangen berichten van een Service Bus-wachtrij via HTTP, geven een time-out dan nul. Als alle apparaten verbinding voor elke dag van 12 uur maken, ziet u de volgende verbinding kosten (naast eventuele andere Service Bus-kosten): 10.000 ontvangen van HTTP-verbindingen * 12 uur per dag * 744 uur-31 dagen = 5.000 brokered verbindingen.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Zijn kosten voor Brokered Connection van toepassing op wachtrijen en onderwerpen/abonnementen?

Ja. Er worden geen verbindingskosten in rekening gebracht voor het verzenden van gebeurtenissen via HTTP, ongeacht het aantal verzendsystemen/-apparaten. Gebeurtenissen ontvangen met HTTP met behulp van een time-out groter zijn dan nul, 'lange polling,', worden genoemd, genereert brokered verbinding kosten. AMQP-verbindingen genereren kosten voor Brokered Connections ongeacht of de verbindingen worden gebruikt om te verzenden of te ontvangen. De eerste 1000 brokered verbindingen in alle standaard naamruimten in een Azure-abonnement zijn opgenomen zonder extra kosten (buiten de base kosten). Omdat deze rechten voldoende zijn zijn voor veel service to service berichtverzending, brokered verbinding kosten meestal alleen moeten worden relevant als u van plan bent AMQP of HTTP lange-polling gebruiken met een groot aantal clients; Als u bijvoorbeeld wilt bereiken efficiënter gebeurtenis streamen of bidirectionele communicatie met veel apparaten of exemplaren van een toepassing in te schakelen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over prijzen voor Service Bus de [Service Bus pagina met prijzen](https://azure.microsoft.com/pricing/details/service-bus/).
* Zie de [Veelgestelde vragen over de Service Bus](service-bus-faq.md#pricing) voor een aantal algemene veelgestelde vragen over servicebus-prijzen en facturering.

[Azure portal]: https://portal.azure.com

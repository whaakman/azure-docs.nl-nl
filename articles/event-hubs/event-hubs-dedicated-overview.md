---
title: Overzicht van Azure Event Hubs toegewezen capaciteit | Microsoft Docs
description: Overzicht van Microsoft Azure Event Hubs toegewezen capaciteit.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: sethm;babanisa
ms.openlocfilehash: 613ea691e38b6f0bcd8873fc2ec6bcafb3cc6c78
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Overzicht van Event Hubs Dedicated

*Event Hubs toegewezen* capaciteit biedt één tenant implementaties voor klanten met de meest veeleisende vereisten. Op volledige schaal Azure Event Hubs inkomend kan meer dan 2 miljoen gebeurtenissen per seconde of maximaal 2 GB per seconde telemetrie met volledig duurzame opslag- en onderliggende tweede latentie. Hierdoor kunnen ook geïntegreerde oplossingen door realtime verwerking en batch op hetzelfde systeem. Met [Event Hubs vastleggen](event-hubs-capture-overview.md) opgenomen in de aanbieding, kunt u de complexiteit van uw oplossing kunt verkleinen doordat één stream realtime en op basis van batch pijplijnen ondersteunen.

De volgende tabel worden de beschikbare service-lagen van Event Hubs vergeleken. De aanbieding Dedicated van Event Hubs is een vaste maandbedrag vergeleken met het gebruik van prijzen voor de meeste functies van standaard. De speciale laag biedt de functies van de standaard-plan, maar met enterprise scale capaciteit voor klanten met veeleisende werkbelastingen. 

| Functie | Standard | Toegewezen |
| --- |:---:|:---:|:---:|
| Ingangsgebeurtenissen | Betalen per miljoen gebeurtenissen | Inbegrepen |
| Doorvoereenheid (1 MB per seconde inkomend, uitgaande van 2 MB per seconde) | Betalen per uur | Inbegrepen |
| Berichtgrootte | 256 kB | 1 MB |
| Beleid voor uitgevers | Ja | Ja |   
| Consumergroepen | 20 | 20 |
| Berichtherhaling | Ja | Ja |
| Maximum aantal Throughput Units | 20 (flexibele en 100)   | 1 CU≈50 |
| Brokered Connections | 1000 opgenomen | 100 K opgenomen |
| Extra Brokered Connections | Ja | Ja |
| Bewaartermijn voor berichten | 1 dag inbegrepen | Tot 7 dagen inbegrepen |
| Capture | Betalen per uur | Inbegrepen |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Voordelen van Event Hubs toegewezen capaciteit

De volgende voordelen zijn beschikbaar bij gebruik van Event Hubs toegewezen:

* Één tenant hosten met geen ruis van andere tenants.
* Grootte van het bericht verhoogt 1 MB in vergelijking met 256 KB voor standaard.
* Herhaalbare prestaties elke keer.
* Capaciteit om te voldoen aan de behoeften van uw burst gegarandeerd.
* Bevat de [vastleggen](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) functie van Azure Event Hubs te bieden integratie met micro-batch- en langdurige bewaarperiode
* Onderhoud nul: we beheren taakverdeling, OS-updates, beveiligingspatches en partitioneren.
* Vaste prijs per uur.
* Bewaren van bericht aan 7 dagen met zonder extra kosten

Event Hubs toegewezen verwijdert u ook een aantal beperkingen doorvoer van de standaard aanbieding. Doorvoereenheden in de prijscategorie Standard aanspraak u op 1000 gebeurtenissen per seconde of 1 MB per seconde van toegangsroutes per TU en dubbele die hoeveelheid uitgaande. De aanbieding speciale scale heeft geen beperkingen op toegangsroutes en uitgaande gebeurtenis telt. Deze limieten gelden alleen door de verwerkingscapaciteit van de aangeschafte event hubs.

Deze gereserveerd, toegewezen omgeving biedt andere mogelijkheden die uniek is voor deze laag, zoals:

* Het aantal naamruimten in uw cluster beheren
* Doorvoer limitson, elk van de naamruimten opgeven
* Het aantal Event Hubs voor elke naamruimte configureren
* Het maximale aantal partities bepalen

Deze service is gericht op de grootste telemetrie-gebruikers en is beschikbaar voor klanten met een enterprise agreement.

## <a name="how-to-onboard"></a>Hoe voorbereiden

U kunt de capaciteit van de omhoog of omlaag schalen gedurende de maand om te voldoen aan uw behoeften door toe te voegen of te verwijderen van CUs. De toegewezen planning is uniek in dat er een meer praktijkervaring voorbereiding van het productteam Event Hubs om op te halen van de flexibele implementatie die geschikt is voor u. Voorbereiden op deze SKU, neem contact op met (ondersteuning voor facturering) [https://ms.portal.azure.com/#create/Microsoft.Support] of uw Microsoft-vertegenwoordiger.

## <a name="next-steps"></a>Volgende stappen
Neem contact op met uw Microsoft-vertegenwoordiger of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit. U kunt ook meer informatie over Event Hubs via de volgende koppelingen:

De volgende koppelingen vindt u gedetailleerde informatie over prijzen:

- [Prijzen van Event Hubs toegewezen](https://azure.microsoft.com/pricing/details/event-hubs/). U kunt ook contact opnemen met uw Microsoft-vertegenwoordiger of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit.
- De [Event Hubs Veelgestelde vragen over](event-hubs-faq.md) bevat informatie over de prijzen en antwoorden op enkele veelgestelde vragen over Event Hubs. 

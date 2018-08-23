---
title: Overzicht van Azure Event Hubs Dedicated capaciteit | Microsoft Docs
description: Overzicht van Microsoft Azure Event Hubs toegewezen capaciteit.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 68a55d8a16e55e2f5e4b48153f045daa40c9a68c
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42058418"
---
# <a name="overview-of-event-hubs-dedicated"></a>Overzicht van Eventhubs Dedicated

*Event Hubs Dedicated* capaciteit biedt één tenant-implementaties voor klanten met de meest veeleisende behoeften. Volledige geschaalde, Azure Event Hubs kunnen er meer dan 2 miljoen gebeurtenissen per seconde of maximaal 2 GB per seconde van telemetrie met volledig duurzame opslag en dan een seconde latentie. Hierdoor kunnen ook geïntegreerde oplossingen door de verwerking van realtime en batch op hetzelfde systeem. Met [Event Hubs Capture](event-hubs-capture-overview.md) opgenomen in het product, kunt u de complexiteit van uw oplossing verkorten door met één stroom zowel real-time en batch op basis van pijplijnen ondersteunen.

De volgende tabel vergelijkt de beschikbare service-lagen van Event Hubs. De aanbieding Event Hubs Dedicated wordt maandelijks een vast bedrag, vergeleken met het gebruik van de prijzen voor de meeste functies van Standard. De speciale laag biedt alle functies van de Standard-abonnement, maar met een enterprise schaal capaciteit voor klanten met veeleisende workloads. 

| Functie | Standard | Toegewezen |
| --- |:---:|:---:|:---:|
| Ingangsgebeurtenissen | Betaal per miljoen gebeurtenissen | Inbegrepen |
| Throughput unit (1 MB/s ingang, 2 MB per seconde uitgaand verkeer) | Betalen per uur | Inbegrepen |
| Berichtgrootte | 256 kB | 1 MB |
| Beleid voor uitgevers | Ja | Ja |   
| Consumergroepen | 20 | 20 |
| Berichtherhaling | Ja | Ja |
| Maximum aantal Throughput Units | 20 (flexibele en 100)   | 1 capaciteitseenheid (CU) ≈ 50 |
| Brokered Connections | 1000 opgenomen | 100 K inbegrepen |
| Extra Brokered Connections | Ja | Ja |
| Bewaartermijn voor berichten | 1 dag inbegrepen | Tot 7 dagen inbegrepen |
| Capture | Betalen per uur | Inbegrepen |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Voordelen van Event Hubs toegewezen capaciteit

De volgende voordelen zijn beschikbaar bij het gebruik van Event Hubs Dedicated:

* Één tenant hosten met geen ruis van andere tenants.
* Grootte van het bericht wordt verhoogd naar 1 MB in vergelijking met 256 KB voor Standard.
* Herhaalbare prestaties telkens.
* Capaciteit om te voldoen aan de behoeften van uw burst gegarandeerd.
* Bevat de [vastleggen](event-hubs-capture-overview.md) functie van Event Hubs, om integratie met microbatches als langdurige opslag te bieden.
* Onderhoud nul: de service beheert taakverdeling, OS-updates, beveiligingspatches en partitioneren.
* Vaste prijzen per uur.
* Bewaarperiode van bericht van 7 dagen zonder extra kosten.

Event Hubs Dedicated verwijdert u ook enkele van de beperkingen van de doorvoer van de Standard-aanbieding. Doorvoereenheden in de Standard-laag recht op 1000 gebeurtenissen per seconde of 1 MB per seconde voor invoer per TU en dubbele die hoeveelheid uitgaande gegevens. De toegewezen scale-aanbieding heeft geen beperkingen voor inkomend en uitgaand verkeer gebeurtenis telt. Deze limieten gelden alleen de verwerkingscapaciteit van de aangeschafte eventhubs.

Deze gereserveerde, toegewezen omgeving biedt andere mogelijkheden die uniek is voor deze laag, zoals:

* Hiermee bepaalt u het aantal naamruimten in uw cluster.
* Hiermee geeft u doorvoerlimieten op elk van de naamruimten.
* Hiermee configureert u het nummer van eventhubs elke naamruimte.
* Bepaalt de limiet voor het aantal partities.

Deze service is gericht op de grootste telemetrie-gebruikers en is beschikbaar voor klanten met een enterprise agreement.

## <a name="how-to-onboard"></a>Hoe moet worden vrijgegeven

U kunt uw capaciteit omhoog of omlaag schalen gedurende de maand om te voldoen aan uw behoeften door toe te voegen of te verwijderen van Cu's. De toegewezen planning is uniek in dat u een hoe u een onboarding uit de Event Hubs-productteam ondervindt om de implementatie van de flexibele die geschikt is voor u. Voor Onboarding van deze SKU [Neem contact op met ondersteuning voor facturering](https://ms.portal.azure.com/#create/Microsoft.Support) of uw Microsoft-vertegenwoordiger.

## <a name="next-steps"></a>Volgende stappen

Neem contact op met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit. U kunt ook meer informatie over Event Hubs Prijscategorieën door naar de volgende koppelingen te gaan:

- [Prijzen van Event Hubs Dedicated](https://azure.microsoft.com/pricing/details/event-hubs/). U kunt ook contact opnemen met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit.
- De [Veelgestelde vragen over Event-Hubs](event-hubs-faq.md) bevat informatie over de prijzen en vindt u antwoorden op enkele veelgestelde vragen over Event Hubs. 

---
title: Overzicht van speciale eventhubs - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht van speciale Azure Event Hubs, dat één tenant implementaties van eventhubs biedt.
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
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: e8a2d8321a42e8b3d090c1ce1fdb3fd9a7ee3714
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138666"
---
# <a name="overview-of-event-hubs-dedicated"></a>Overzicht van Eventhubs Dedicated

Azure Event Hubs-clusters bieden één tenant-implementaties voor klanten met de meest veeleisende behoeften voor streaming. Deze aanbieding voor één tenant heeft een gegarandeerde SLA van 99,99%. Het is alleen beschikbaar op de Dedicated prijscategorie.

Een Event Hubs-cluster kunt inkomend verkeer miljoenen gebeurtenissen per seconde met gegarandeerde capaciteit en subsecond latentie. Naamruimten en event hubs gemaakt binnen de toegewezen cluster bevatten alle functies van het Standard-aanbod en nog veel meer, maar zonder eventuele beperkingen voor inkomend verkeer. Dit omvat ook de [Event Hubs Capture](event-hubs-capture-overview.md) functie zonder extra kosten. U kunt het automatisch batch- en logboekbestanden-gegevensstromen naar Azure Storage of Azure Data Lake.

Toegewezen clusters zijn ingericht en gefactureerd op basis van capaciteitseenheden (Cu's). Cu's zijn een vooraf toegewezen hoeveelheid CPU en geheugenbronnen. U kunt aanschaffen 1, 2, 4, 8, 12, 16 of 20 Cu's voor elk cluster. Hoeveel u kunt opnemen en stream per Capaciteitseenheid is afhankelijk van factoren zoals het aantal producenten en consumenten, de vorm van nettolading en de snelheid van uitgangsgebeurtenissen.

Zie de tabel met de benchmarkresultaten voor meer informatie.

## <a name="why-use-event-hubs-dedicated"></a>Waarom een Event Hubs Dedicated gebruiken?

Event Hubs Dedicated biedt drie voordelen voor klanten die op ondernemingsniveau capaciteit nodig hebt.

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Single-tenants garandeert capaciteit voor betere prestaties

Een specifieke cluster gegarandeerd capaciteit op volledige schaal. Het kan tot gigabytes van streaming gegevens met een volledig duurzame opslag en subsecond latentie voor het afhandelen van pieken in verkeer.

#### <a name="inclusive-and-exclusive-access-to-features"></a>Liggen en exclusieve toegang tot functies 
De speciaal aanbod bevat functies zoals vastleggen zonder extra kosten. Het biedt ook exclusieve toegang tot de nieuwe functies, zoals BYOK. De service beheert ook de taakverdeling, OS-updates, beveiligingspatches en partitioneren. Met deze mogelijkheden kunt u minder tijd kunt besteden aan het onderhoud van infrastructuur en meer tijd voor het bouwen van client-side-functies.

#### <a name="cost-savings"></a>Besparingen
Bij hoge inkomend verkeer volumes, een cluster kosten aanzienlijk minder per uur dan wanneer u aankoop van een vergelijkbaar aantal throughput Units (Doorvoereenheden) in de Standard-product. Een hoog volume is > 100 Doorvoereenheden.


## <a name="event-hubs-standard-vs-dedicated"></a>Event Hubs standaard vs. Toegewezen

De volgende tabel vergelijkt de beschikbare service-lagen van Event Hubs. De aanbieding Event Hubs Dedicated wordt gefactureerd op basis van een vaste maandelijkse prijs voor gebruik van de prijs voor de meeste functies van Standard vergeleken. De speciale laag biedt alle functies van de Standard-abonnement, maar met grootschalige capaciteit voor klanten met veeleisende workloads.

| Functie | Standard | Toegewezen |
| --- |:---:|:---:|
| Ingangsgebeurtenissen | Betaal per miljoen gebeurtenissen | Inbegrepen |
| Throughput unit (1 MB/s ingang, 2 MB per seconde uitgaand verkeer) | Betalen per uur | Inbegrepen |
| Berichtgrootte | 1 MB | 1 MB |
| Partities | 40 per naamruimte | 2000 per Capaciteitseenheid |
| Consumergroepen | 20 per event hub | 1000 per event hub |
| Maximale bandbreedte | 20 Doorvoereenheden (maximaal 40 Doorvoereenheden) | 20 Cu 's |
| Brokered Connections | 1000 opgenomen | 100.000 opgenomen |
| Bewaartermijn voor berichten | Een dag inbegrepen | Tot zeven dagen inbegrepen |
| Capture | Betalen per uur | Inbegrepen |

## <a name="what-can-i-achieve-with-a-cluster"></a>Wat doe ik dit met een cluster?

Voor een Event Hubs-cluster, hoeveel u kunt opnemen en stream is afhankelijk van uw producenten, uw consumenten, de snelheid waarmee u opnemen en verwerken en nog veel meer.

De volgende tabel ziet u de benchmarkresultaten die zijn verkregen tijdens het testen.

| Nettolading vorm | Ontvangers | Binnenkomende bandbreedte| Binnenkomende berichten | Uitgaande bandbreedte | Uitgaande berichten | Totaal aantal Doorvoereenheden | Doorvoereenheden per Capaciteitseenheid |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Batches van 100x1KB | 2 | 400 MB per seconde | 400.000 berichten per seconde | 800 MB per seconde | 800.000 berichten/sec. | 400 Doorvoereenheden | 100 Doorvoereenheden | 
| Batches van 10x10KB | 2 | 666 MB per seconde | 66,600 berichten/sec. | 1.33 GB/sec | 133,000 berichten/sec. | 666 Doorvoereenheden | 166 Doorvoereenheden |
| Batches van 6x32KB | 1 | 1,05 GB/sec | 34.000 berichten/sec. | 1,05 GB/sec | 34.000 berichten/sec. | 1000 Doorvoereenheden | 250 Doorvoereenheden |

In de test, zijn de volgende criteria gebruikt:

- Er is een speciaal laag Event Hubs-cluster met vier capaciteitseenheden gebruikt.
- De event hub die wordt gebruikt voor gegevensopname heeft 200 partities.
- De gegevens die is opgenomen is ontvangen door de twee toepassingen van de ontvanger. Deze ontvangen gegevens van alle partities.

## <a name="use-event-hubs-dedicated"></a>Eventhubs Dedicated gebruiken

Gebruik van Event Hubs Dedicated, [Neem contact op met ondersteuning voor facturering](https://ms.portal.azure.com/#create/Microsoft.Support) of uw Microsoft-vertegenwoordiger. U kunt uw capaciteit omhoog of omlaag schalen gedurende de maand om te voldoen aan uw behoeften door toe te voegen of te verwijderen van Cu's. Het productteam van Event Hubs helpt u bij de implementatie van de flexibele die geschikt is voor u.

## <a name="next-steps"></a>Volgende stappen

Neem contact op met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit. Voor meer informatie over prijzen van lagen van Event Hubs, gebruik de volgende koppelingen:

- [Prijzen van Event Hubs Dedicated](https://azure.microsoft.com/pricing/details/event-hubs/). U kunt ook contact opnemen met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit.
- De [Veelgestelde vragen over Event-Hubs](event-hubs-faq.md) bevat informatie over de prijzen en vindt u antwoorden op enkele veelgestelde vragen over Event Hubs.

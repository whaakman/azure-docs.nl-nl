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
ms.openlocfilehash: 52d46009464c7417d5b525154fdac09c030aabe7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708007"
---
# <a name="overview-of-event-hubs-dedicated"></a>Overzicht van Eventhubs Dedicated

*Event Hubs-clusters* bieden één tenant-implementaties voor klanten met de meest veeleisende behoeften voor streaming. Deze aanbieding voor één tenant een gegarandeerde SLA van 99,99% is en is alleen beschikbaar op onze Dedicated prijscategorie. Een Event Hubs-cluster kunt inkomend verkeer miljoenen gebeurtenissen per seconde met gegarandeerde capaciteit en dan een seconde latentie. Naamruimten en event hubs gemaakt binnen de toegewezen cluster bevatten alle functies van het Standard-aanbod en nog veel meer, maar zonder eventuele beperkingen voor inkomend verkeer. Dit omvat ook de populaire [Event Hubs Capture](event-hubs-capture-overview.md) functie zonder extra kosten, zodat u kunt automatisch batch- en logboekbestanden-gegevensstromen naar Azure Storage of Azure Data Lake. 

Toegewezen clusters zijn ingericht en gefactureerd op basis van **capaciteitseenheden (Cu's)**, een vooraf toegewezen hoeveelheid CPU en geheugenbronnen. U kunt 1, 2, 4, 8, 12, 16 of 20 Cu's voor elk cluster kopen. Hoeveel u kunt opnemen en stream per Capaciteitseenheid is afhankelijk van diverse factoren, zoals het aantal producenten en consumenten, de nettolading van de vorm van uitgaand verkeer tarief (Zie de benchmarkresultaten hieronder voor meer informatie). 

## <a name="why-dedicated"></a>Waarom toegewezen?

Toegewezen Event Hubs biedt drie aantrekkelijke voordelen voor klanten die op ondernemingsniveau capaciteit nodig hebt:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Single-tenants garandeert capaciteit voor betere prestaties

Een specifieke cluster garandeert capaciteit op volledige schaal en kunt u tot gigabytes van streaming gegevens met een volledig duurzame opslag en dan een seconde latentie voor een pieken in verkeer. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Liggen en exclusieve toegang tot functies 
De speciaal aanbod bevat functies zoals vastleggen op geen extra kosten, evenals de exclusieve toegang tot de nieuwe functies, zoals BYOK. De service beheert ook taakverdeling, OS-updates, beveiligingspatches en voor de klant, het partitioneren van zodat u minder tijd aan onderhoud aan de infrastructuur en meer tijd besteden kunt voor het bouwen van client-side-functies.  

#### <a name="cost-savings"></a>Profiteer van kostenbesparing
Op hoog inkomend verkeer volumes (> 100 Doorvoereenheden), een cluster kosten die aanzienlijk minder per uur dan voor de aanschaf van een vergelijkbaar aantal throughput Units in de Standard-product.


## <a name="event-hubs-standard-vs-dedicated"></a>Event Hubs standaard vs. Toegewezen

De volgende tabel vergelijkt de beschikbare service-lagen van Event Hubs. De aanbieding Event Hubs Dedicated wordt gefactureerd op basis van een vaste maandelijkse prijs, vergeleken met het gebruik van de prijzen voor de meeste functies van Standard. De speciale laag biedt alle functies van de Standard-abonnement, maar met een enterprise schaal capaciteit voor klanten met veeleisende workloads. 

| Functie | Standard | Toegewezen |
| --- |:---:|:---:|
| Ingangsgebeurtenissen | Betaal per miljoen gebeurtenissen | Inbegrepen |
| Throughput unit (1 MB/s ingang, 2 MB per seconde uitgaand verkeer) | Betalen per uur | Inbegrepen |
| Berichtgrootte | 1 MB | 1 MB |
| Partities | 40 per naamruimte | 2000 per Capaciteitseenheid |
| Consumergroepen | 20 per Event Hub | 1000 per Event Hub |
| Met maximaal Bandbreedte | 20 Doorvoereenheden (maximaal 40 Doorvoereenheden)    | 20 Cu 's |
| Brokered Connections | 1000 opgenomen | 100 K inbegrepen |
| Bewaartermijn voor berichten | 1 dag inbegrepen | Tot 7 dagen inbegrepen |
| Capture | Betalen per uur | Inbegrepen |

## <a name="what-can-i-achieve-with-a-cluster"></a>Wat doe ik dit met een cluster?

Voor een Event Hubs-cluster, hoeveel u kunt opnemen en stream is afhankelijk van diverse factoren, zoals uw producenten, consumenten, de snelheid waarmee u kan opnemen en verwerken en nog veel meer. 

Onderstaande tabel ziet u de benchmarkresultaten dat we tijdens onze testen bereikt:

| Nettolading vorm | Ontvangers | Binnenkomende bandbreedte| Binnenkomende berichten | Uitgaande bandbreedte | Uitgaande berichten | Totaal aantal Doorvoereenheden | Doorvoereenheden per Capaciteitseenheid |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Batches van 100x1KB | 2 | 400 MB per seconde | 400 k berichten per seconde | 800 MB per seconde | 800 k berichten/sec | 400 Doorvoereenheden | 100 Doorvoereenheden | 
| Batches van 10x10KB | 2 | 666 MB per seconde | 66.6 k berichten/sec | 1.33 GB/sec | 133 k berichten/sec | 666 Doorvoereenheden | 166 Doorvoereenheden |
| Batches van 6x32KB | 1 | 1,05 GB/sec | 34 k berichten / sec | 1,05 GB/sec | 34 k berichten/sec | 1000 Doorvoereenheden | 250 Doorvoereenheden |

In het testen, is de volgende criteria gebruikt:

- Er is een speciale laag Event Hubs-cluster met vier capaciteitseenheden (Cu's) gebruikt. 
- De event hub die wordt gebruikt voor gegevensopname heeft 200 partities. 
- De gegevens die is opgenomen is ontvangen door twee ontvanger toepassingen ontvangen van alle partities.

## <a name="how-to-onboard"></a>Hoe moet worden vrijgegeven

Voor Onboarding van deze SKU [Neem contact op met ondersteuning voor facturering](https://ms.portal.azure.com/#create/Microsoft.Support) of uw Microsoft-vertegenwoordiger. U kunt uw capaciteit omhoog of omlaag schalen gedurende de maand om te voldoen aan uw behoeften door toe te voegen of te verwijderen van Cu's. De toegewezen planning is uniek in dat u een hoe u een onboarding uit de Event Hubs-productteam ondervindt om de implementatie van de flexibele die geschikt is voor u. 

## <a name="next-steps"></a>Volgende stappen

Neem contact op met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit. U kunt ook meer informatie over Event Hubs Prijscategorieën door naar de volgende koppelingen te gaan:

- [Prijzen van Event Hubs Dedicated](https://azure.microsoft.com/pricing/details/event-hubs/). U kunt ook contact opnemen met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit.
- De [Veelgestelde vragen over Event-Hubs](event-hubs-faq.md) bevat informatie over de prijzen en vindt u antwoorden op enkele veelgestelde vragen over Event Hubs.

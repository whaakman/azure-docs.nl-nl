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
ms.openlocfilehash: 4f721dc4fda5bef002c794d79dfd2f054f9eaf38
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65511182"
---
# <a name="overview-of-event-hubs-dedicated"></a>Overzicht van Eventhubs Dedicated

*Event Hubs-clusters* bieden één tenant-implementaties voor klanten met de meest veeleisende behoeften voor streaming. Deze aanbieding voor één tenant een gegarandeerde SLA van 99,99% is en is alleen beschikbaar op onze Dedicated prijscategorie. Een Event Hubs-cluster kunt inkomend verkeer miljoenen gebeurtenissen per seconde met gegarandeerde capaciteit en dan een seconde latentie. Naamruimten en event hubs gemaakt binnen de toegewezen cluster bevatten alle functies van het Standard-aanbod en nog veel meer, maar zonder eventuele beperkingen voor inkomend verkeer. Dit omvat ook de populaire [Event Hubs Capture](event-hubs-capture-overview.md) functie zonder extra kosten, zodat u kunt automatisch batch- en logboekbestanden-gegevensstromen naar Azure Storage of Azure Data Lake. 

Clusters worden ingericht en gefactureerd op basis van **capaciteitseenheden (Cu's)**, een vooraf toegewezen hoeveelheid CPU en geheugenbronnen. U kunt 1, 2, 4, 8, 12, 16 of 20 Cu's voor elk cluster kopen. Hoeveel u kunt opnemen en stream per Capaciteitseenheid is afhankelijk van diverse factoren, zoals het aantal producenten en consumenten, de nettolading van de vorm van uitgaand verkeer tarief (Zie de benchmarkresultaten hieronder voor meer informatie). 

> [!NOTE]
> Alle Event Hubs-clusters zijn standaard ingeschakeld Kafka en bieden ondersteuning voor Kafka-eindpunten die kunnen worden gebruikt door uw bestaande Kafka op basis van toepassingen. Met Kafka is ingeschakeld op het cluster heeft geen invloed op uw niet-Kafka gebruiksvoorbeelden; Er is geen optie of de noodzaak voor het uitschakelen van Kafka op een cluster.

## <a name="why-dedicated"></a>Waarom toegewezen?

Toegewezen Event Hubs biedt drie aantrekkelijke voordelen voor klanten die op ondernemingsniveau capaciteit nodig hebt:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Single-tenants garandeert capaciteit voor betere prestaties

Een specifieke cluster garandeert capaciteit op volledige schaal en kunt u tot gigabytes van streaming gegevens met een volledig duurzame opslag en dan een seconde latentie voor een pieken in verkeer. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Liggen en exclusieve toegang tot functies 
De speciaal aanbod bevat functies zoals vastleggen op geen extra kosten, evenals de exclusieve toegang tot de nieuwe functies, zoals BYOK. De service beheert ook taakverdeling, OS-updates, beveiligingspatches en voor de klant, het partitioneren van zodat u minder tijd aan onderhoud aan de infrastructuur en meer tijd besteden kunt voor het bouwen van client-side-functies.  

#### <a name="cost-savings"></a>Profiteer van kostenbesparing
Op hoog inkomend verkeer volumes (> 100 Doorvoereenheden), een cluster kosten die aanzienlijk minder per uur dan voor de aanschaf van een vergelijkbaar aantal throughput Units in de Standard-product.


## <a name="event-hubs-dedicated-quotas-and-limits"></a>Eventhubs Dedicated quota en limieten

De aanbieding Event Hubs Dedicated wordt in rekening gebracht tegen een vaste maandelijkse prijs met een minimum van 4 uur van het gebruik van. De speciale laag biedt alle functies van de Standard-abonnement, maar met enterprise schaal capaciteiten en limieten voor klanten met veeleisende workloads. 

| Functie | Standard | Toegewezen |
| --- |:---:|:---:|
| Bandbreedte | 20 Doorvoereenheden (maximaal 40 Doorvoereenheden) | 20 Cu 's |
| Naamruimten |  1 | 50 per Capaciteitseenheid |
| Event Hubs |  10 | Geen limiet voor event hubs/onderwerpen |
| Gebeurtenissen voor inkomend verkeer | Betaal per miljoen gebeurtenissen | Opgenomen |
| Berichtgrootte | 1 Million Bytes | 1 Million Bytes |
| Partities | 40 per naamruimte | 2000 per Capaciteitseenheid |
| Consumentengroepen | 20 per Event Hub | Geen limiet per Capaciteitseenheid, 1000 per event hub |
| Brokered verbindingen | 1000 opgenomen | 100 K inbegrepen |
| Bewaarperiode van bericht | 7 dagen, 84 GB inbegrepen per TU | 90 dagen, 10 TB, inbegrepen per Capaciteitseenheid |
| Vastleggen | Betalen per uur | Opgenomen |

## <a name="how-to-onboard"></a>Hoe moet worden vrijgegeven

Voor Onboarding van Event Hubs Dedicated, neem contact op met de [team van Event Hubs](mailto:askeventhubs@microsoft.com). De toegewezen planning is uniek in dat u een hoe u een onboarding uit de Event Hubs-productteam ondervindt om de implementatie van de flexibele die geschikt is voor u. 

## <a name="faqs"></a>Veelgestelde vragen

#### <a name="what-can-i-achieve-with-a-cluster"></a>Wat doe ik dit met een cluster?

Voor een Event Hubs-cluster, hoeveel u kunt opnemen en stream is afhankelijk van diverse factoren, zoals uw producenten, consumenten, de snelheid waarmee u kan opnemen en verwerken en nog veel meer. 

Onderstaande tabel ziet u de benchmarkresultaten dat we tijdens onze testen bereikt:

| Nettolading vorm | Ontvangers | Binnenkomende bandbreedte| Binnenkomende berichten | Uitgaande bandbreedte | Uitgaande berichten | Totaal aantal Doorvoereenheden | Doorvoereenheden per Capaciteitseenheid |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Batches van 100x1KB | 2 | 400 MB per seconde | 400 k berichten/sec. | 800 MB per seconde | 800 k berichten/sec. | 400 Doorvoereenheden | 100 Doorvoereenheden | 
| Batches van 10x10KB | 2 | 666 MB per seconde | 66.6 k berichten/sec. | 1.33 GB/sec | 133 k berichten/sec. | 666 Doorvoereenheden | 166 Doorvoereenheden |
| Batches van 6x32KB | 1 | 1,05 GB/sec | 34 k berichten / sec | 1,05 GB/sec | 34 k berichten/sec. | 1000 Doorvoereenheden | 250 Doorvoereenheden |

In het testen, is de volgende criteria gebruikt:

- Er is een speciale laag Event Hubs-cluster met vier capaciteitseenheden (Cu's) gebruikt. 
- De event hub die wordt gebruikt voor gegevensopname heeft 200 partities. 
- De gegevens die is opgenomen is ontvangen door twee ontvanger toepassingen ontvangen van alle partities.

#### <a name="can-i-scale-down-my-cluster"></a>Kan ik mijn cluster verkleinen?

Na het maken van clusters worden in rekening gebracht voor minimaal 4 gebruiksuren. In de Preview-versie van de self-service voor stroomactiviteitvoortgang-ervaring, kunt u indienen een [ondersteuningsaanvraag](https://ms.portal.azure.com/#create/Microsoft.Support) aan het team Event Hubs onder *technische > quotum > aanvraag voor schaal omhoog of omlaag toegewezen Cluster schalen*. Het duurt maximaal 7 dagen om de aanvraag voor het schalen van uw cluster te voltooien. 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>Hoe werkt Geo-DR met mijn cluster?

U kunt geo-combinatie van een naamruimte in een cluster Dedicated-laag met een andere naamruimte in een cluster Dedicated-laag. We bevorderen niet koppelen met een naamruimte Dedicated-laag met een naamruimte in onze Standard-aanbieding, omdat de maximale doorvoer zullen niet compatibel zal dit leiden tot fouten. 

#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>Kan ik mijn Standard-naamruimten voor deel uitmaken van een cluster toegewezen laag migreren?
We ondersteunen momenteel geen een geautomatiseerd proces voor het migreren van uw event hubs-gegevens van een Standard-naamruimte naar een speciaal een. Als u wilt migreren naar een cluster Dedicated-laag, raden wij Verwerkingsstop voor eventuele berichten links in de Standard-laag eventhubs en de verbindingseindpunten vervangen door die van de toegewezen naamruimte.

## <a name="next-steps"></a>Volgende stappen

Neem contact op met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit. U kunt ook meer informatie over Event Hubs Prijscategorieën door naar de volgende koppelingen te gaan:

- [Prijzen van Event Hubs Dedicated](https://azure.microsoft.com/pricing/details/event-hubs/). U kunt ook contact opnemen met uw Microsoft-verkoopmedewerker of Microsoft Support om aanvullende informatie over Event Hubs toegewezen capaciteit.
- De [Veelgestelde vragen over Event-Hubs](event-hubs-faq.md) bevat informatie over de prijzen en vindt u antwoorden op enkele veelgestelde vragen over Event Hubs.

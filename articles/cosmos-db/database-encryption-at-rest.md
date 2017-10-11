---
title: 'Versleuteling van de database in rust: Azure Cosmos DB | Microsoft Docs'
description: Meer informatie over hoe Azure Cosmos DB standaard codering van alle gegevens biedt.
services: cosmos-db
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: voellm
ms.openlocfilehash: d8967d4504a8ccabb444c7f3d5635e2d00f287c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Versleuteling van Azure DB Cosmos-database in rust

Codering in rust is een wachtwoordzin die meestal naar de versleuteling van gegevens op opslagapparaten met niet-vluchtige, verwijst zoals Solid-State stations (SSD's) en harde schijven (HDD's). De primaire databases cosmos DB opgeslagen op SSD's. De mediabijlagen en de back-ups worden opgeslagen in Azure Blob-opslag is doorgaans een back-up door HDD's. Met de release van versleuteling in rust voor Cosmos DB, worden alle databases, mediabijlagen en back-ups versleuteld. Uw gegevens nu in transit versleuteld (via het netwerk) en at-rest (niet-vluchtige opslag), zodat u end-to-end-versleuteling.

Als een PaaS-service DB Cosmos zeer eenvoudig te gebruiken is. Omdat alle gebruikersgegevens zijn opgeslagen in de DB Cosmos is versleuteld in rust en bij transport, er geen geen actie te ondernemen. Plaats deze op een andere manier is dat versleuteling in rust 'op' standaard is. Er zijn geen besturingselementen om te schakelen in- of uitschakelen. We leveren deze functie terwijl we blijven voldoen aan onze [beschikbaarheid en prestaties van SLA's](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implement-encryption-at-rest"></a>Codering in rust implementeren

Codering in rust wordt geïmplementeerd met behulp van een aantal beveiligingstechnologieën, met inbegrip van beveiligde sleutel opslagsystemen, gecodeerde netwerken en cryptografische API's. Systemen die gegevens verwerken en ontsleuteld hebben om te communiceren met systemen die sleutels beheren. Het diagram toont hoe de opslag van versleutelde gegevens en het beheer van sleutels wordt gescheiden. 

![Diagram ontwerpen](./media/database-encryption-at-rest/design-diagram.png)

De basisprincipes van de aanvraag van een gebruiker is als volgt:
- Het gebruikersaccount van de database gereed wordt gemaakt en opslagsleutels worden opgehaald via een aanvraag met de Management-Resource Provider.
- Een gebruiker maakt een verbinding met Cosmos DB via HTTPS/secure transport. (De SDK's abstracte de details.)
- De gebruiker verzendt een JSON-document via de eerder gemaakte beveiligde verbinding worden opgeslagen.
- Het JSON-document wordt geïndexeerd, tenzij de gebruiker heeft uitgeschakeld indexeren.
- Zowel de JSON-document en index de gegevens worden geschreven naar beveiligde opslag.
- Periodiek gegevens lezen uit de veilige opslag en een back-up naar Azure versleutelde Blob Store.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>V: hoe veel meer kost Azure Storage als versleuteling van opslag-Service is ingeschakeld?
A: Er is geen extra kosten.

### <a name="q-who-manages-the-encryption-keys"></a>V: die de versleutelingssleutels beheert?
A: de sleutels worden beheerd door Microsoft.

### <a name="q-how-often-are-encryption-keys-rotated"></a>V: hoe vaak worden versleutelingssleutels gedraaid?
A: Microsoft heeft een set van interne richtlijnen voor codering sleutel worden gedraaid dat volgt op Cosmos-DB. De specifieke richtlijnen worden niet gepubliceerd. Microsoft publiceren de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx), die is gezien als een subset van interne richtlijnen en aanbevolen procedures voor nuttig is voor ontwikkelaars.

### <a name="q-can-i-use-my-own-encryption-keys"></a>V: kan ik mijn eigen versleutelingssleutels gebruiken?
A: cosmos DB is een PaaS-service en wordt hard gewerkt om de service eenvoudig te gebruiken. Ons opgevallen dat deze vraag is vaak gevraagd als een vraag proxy voor een nalevingsvereiste zoals PCI-DSS voldoen. Als onderdeel van het bouwen van deze functie gewerkt we met naleving auditors om ervoor te zorgen dat klanten die gebruikmaken van de Cosmos-DB voldoen aan de vereisten zonder de noodzaak voor het beheren van sleutels zelf.
Als gevolg hiervan bieden wij gebruikers de optie te belasten zelf met Sleutelbeheer.

### <a name="q-what-regions-have-encryption-turned-on"></a>V: wat regio's hebt u versleuteling is ingeschakeld?
A: alle regio's van Azure Cosmos DB hebben versleuteling ingeschakeld voor alle gebruikersgegevens.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>V: versleuteling beïnvloedt de prestaties, latentie en doorvoer Sla's?
A: Er is geen gevolgen of wijzigingen aan de SLA's nu dat versleuteling in rust is ingeschakeld voor alle bestaande en nieuwe accounts van de prestaties. U vindt meer op de [SLA voor Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) pagina om te bekijken van de meest recente garanties.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>V: de lokale emulator ondersteunt de versleuteling in rust?
A: de emulator is een zelfstandig hulpprogramma dat ontwikkelen en testen en de key management-services die de beheerde Cosmos-DB-service gebruikt geen gebruikt. Onze aanbeveling is BitLocker in te schakelen op stations waar u gevoelige emulator testgegevens opslaat. De [emulator biedt ondersteuning voor het wijzigen van de standaard-gegevensmap](local-emulator.md) of via een bekende locatie.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van de Cosmos-DB beveiligings- en de laatste verbeteringen [Azure Cosmos DB databasebeveiliging](database-security.md).
Zie voor meer informatie over Microsoft certificeringen de [Azure Vertrouwenscentrum](https://azure.microsoft.com/en-us/support/trust-center/).

---
title: Gegevens globaal distribueren met Azure Cosmos DB | Microsoft Docs
description: Meer informatie over wereldwijde geo-replicatie, meerdere masters, failover en gegevens herstellen met behulp van de globale databases van Azure Cosmos DB, een wereldwijd gedistribueerde, multi-model databaseservice.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: c90450fa4cc35b460198f5a351a965aee4ea4f4b
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636408"
---
# <a name="build-globally-distributed-applications-with-azure-cosmos-db"></a>Bouw wereldwijd gedistribueerde toepassingen met Azure Cosmos DB

Veel van de huidige toepassingen moeten worden uitgevoerd in meerdere datacenters. Deze toepassingen worden aangeduid als wereldwijd gedistribueerde toepassingen. Deze toepassingen zijn altijd 'op' en ze zijn toegankelijk voor gebruikers overal ter wereld. Beheren van de wereldwijde distributie van gegevens die worden gebruikt door deze toepassingen en lage latentie, elastische schaalbaarheid en hoge beschikbaarheid over de hele wereld, die tegelijkertijd is een vaste probleem. Azure Cosmos DB is een wereldwijd gedistribueerde databaseservice die is ontworpen voor lage latentie, elastische schaalbaarheid van doorvoer, goed gedefinieerde semantiek voor de consistentie van gegevens en hoge beschikbaarheid. Kort gezegd, als de behoeften van uw toepassing gegarandeerd snelle reactietijd overal ter wereld, als deze vereist is om te worden altijd online en onbeperkte en flexibele schaalbaarheid van de doorvoer en opslag nodig heeft, u moet rekening houden met het bouwen van toepassingen met Azure Cosmos DB.

Azure Cosmos DB is een fundamentele Azure-service en is beschikbaar in alle [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/) standaard. Microsoft Azure-datacenters in 54 + regio's over de hele wereld werkt en blijft om uit te breiden, regionaal aanwezigheid om te voldoen aan de groeiende behoeften van klanten. Wanneer u een Azure Cosmos-account maakt, kunt u bepalen welke regio('s) moet worden geïmplementeerd in. Microsoft heeft de Azure Cosmos DB service 24/7, zodat u zich op uw toepassingen richten kunt.

U kunt uw databases zijn wereldwijd gedistribueerde en beschikbaar zijn in een van de Azure-regio's configureren. Als u wilt de latentie verlagen, plaatst u de gegevens dichter naar waar uw gebruikers zich bevinden. De vereiste regio's te kiezen, is afhankelijk van het wereldwijde bereik van uw toepassing en waar uw gebruikers zich bevinden. Azure Cosmos DB repliceert de gegevens in uw account transparant naar alle regio's die zijn gekoppeld aan uw account. Het biedt één integraal beeld van de wereldwijd gedistribueerde Azure Cosmos-database en de containers die uw toepassing kunt lezen en schrijven naar lokaal. U kunt met Azure Cosmos DB, toevoegen of verwijderen van de regio's die zijn gekoppeld aan uw account op elk gewenst moment. Uw toepassing hoeft niet te worden onderbroken of opnieuw als u wilt toevoegen of verwijderen van een regio zijn geïmplementeerd. Deze maximaal beschikbaar blijft continu vanwege de multi-homingmogelijkheden die de service biedt.

## <a name="key-benefits-of-global-distribution"></a>Belangrijkste voordelen van wereldwijde distributie

**Globale actief / actief-apps bouwen**: met de mogelijkheid meerdere masters, wordt elke regio een regio voor schrijfbewerkingen (behalve dat kan worden gelezen). Meerdere masters functie ook garanties - onbeperkt elastisch schrijven schaalbaarheid 99,999% lezen en schrijven van beschikbaarheid van alle rond de hele wereld en gegarandeerde lees-/ schrijfbewerkingen geleverd in minder dan 10 milliseconden in het 99e percentiel.  

Door het gebruik van Azure Cosmos DB multihoming-API's, uw toepassing is op de hoogte van de dichtstbijzijnde regio en er kunnen aanvragen worden verzonden voor deze regio. Dichtstbijzijnde regio wordt zonder eventuele wijzigingen in de configuratie geïdentificeerd. Het toevoegen en verwijderen van regio's van uw Azure Cosmos DB-account, uw toepassing hoeft niet te worden geïmplementeerd en blijft deze maximaal beschikbaar.

**Bouw uiterst snel reagerende apps**: uw toepassing kan eenvoudig worden ontworpen om uit te voeren in de buurt van real-time lees- en schrijfbewerkingen, met een latentie van slechts enkele milliseconden op basis van alle regio's die u hebt gekozen voor uw database.  De replicatie van gegevens tussen regio's verwerkt Azure Cosmos DB intern zodat het consistentieniveau gekozen voor de Azure Cosmos-account kan worden gegarandeerd.

Veel toepassingen profiteren van de prestatieverbeteringen die worden geleverd met de mogelijkheid om uit te voeren schrijfbewerkingen in meerdere regio's (lokaal). Sommige toepassingen waarvoor grote consistentie wil alle schrijfbewerkingen trechter in één regio. Ter ondersteuning van deze toepassingen, ondersteunt Azure Cosmos DB zowel enkele regio en configuraties voor meerdere regio's.

**Maximaal beschikbare apps bouwen**: uitvoeren van een database in meerdere regio's verhoogt de beschikbaarheid van de database. Als één regio niet beschikbaar is, worden in andere regio's aanvragen voor toepassingen automatisch verwerken. Azure Cosmos DB biedt voor 99,999% lezen en schrijven van beschikbaarheid voor databases van meerdere regio's.

**Bedrijfscontinuïteit tijdens een regionale onderbreking**: Azure Cosmos DB biedt ondersteuning voor [automatische failover](how-to-manage-database-account.md#automatic-failover) tijdens een regionale onderbreking. Bovendien tijdens een regionale storing blijft Azure Cosmos DB behouden de latentie, beschikbaarheid, consistentie en doorvoer Sla's. Om te zorgen dat uw gehele toepassing maximaal beschikbaar, biedt Azure Cosmos DB handmatige failover-API voor het simuleren van een regionale onderbreking. U kunt met behulp van deze API, normale zakelijke continuïteit oefeningen uitvoeren.

**Algemene lezen en schrijven van schaalbaarheid**: met meerdere masters capaciteit, kunt u elastisch schalen lezen en schrijven van doorvoer over de hele wereld. Meerdere masters mogelijkheid wordt gegarandeerd dat de doorvoer die uw toepassing wordt geconfigureerd op een Azure Cosmos DB-database of een container wordt geleverd in alle regio's en beveiligd door [met financiële Sla's ondersteund](https://aka.ms/acdbsla).

**Meerdere duidelijk gedefinieerde consistentiemodellen**: replicatie-protocol voor Azure Cosmos DB is ontworpen om u te bieden van vijf duidelijk omschreven, praktische en intuïtieve consistentiemodellen. Elke consistentiemodel heeft een balans tussen consistentie en prestaties. Deze consistentiemodellen kunnen u wereldwijd gedistribueerde toepassingen bouwen met een eenvoudig.

## <a id="Next Steps"></a>Volgende stappen

Meer informatie over globale distributie in de volgende artikelen:

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Regio's van uw Azure Cosmos-account toevoegen/verwijderen](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Een aangepaste conflict resolutie beleid voor de SQL API-accounts maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
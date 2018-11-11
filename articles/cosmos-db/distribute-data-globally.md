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
ms.openlocfilehash: 4aa5e4ff46eeaa4e8d8c723f626dd1f1193fd12a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281605"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale gegevensdistributie met Azure Cosmos DB

Veel van de huidige toepassingen op op planetaire schaal worden uitgevoerd. Deze toepassingen zijn altijd op en toegankelijk is voor gebruikers overal ter wereld. Beheren van de wereldwijde distributie van gegevens die door deze toepassingen worden gebruikt bij het leveren van hoge prestaties en hoge beschikbaarheid is een vaste probleem. Azure Cosmos DB is een wereldwijd gedistribueerde databaseservice die is ontworpen om hoge prestaties en hoge beschikbaarheid te bieden. Azure Cosmos DB is vanwege daarom bij uitstek geschikt voor deze realtime-toepassingen.

Cosmos DB is een fundamentele Azure-service en is beschikbaar in alle [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/) standaard. Microsoft Azure-datacenters in 54 + regio's over de hele wereld werkt en blijft om uit te breiden om te voldoen aan de groeiende behoeften van klanten. Wanneer u een Cosmos DB-account maakt, kunt u bepalen welke regio('s) in moeten worden geïmplementeerd. Microsoft heeft de Cosmos DB service 24/7, zodat u zich op uw toepassingen richten kunt.

U kunt uw databases zijn wereldwijd gedistribueerde en beschikbaar zijn in een van de meer dan 50 Azure-regio's configureren. Als u wilt de latentie verlagen, plaatst u de gegevens dichter naar de locatie van de gebruiker. De vereiste regio's te kiezen, is afhankelijk van het wereldwijde bereik van uw toepassing en waar uw gebruikers zich bevinden. Cosmos DB repliceert de gegevens in uw account transparant naar alle geconfigureerde regio's. Het biedt één integraal beeld van uw Cosmos-database en de containers waarmee uw toepassing kan lezen en schrijven lokaal. U kunt met Cosmos DB, toevoegen of verwijderen van de regio's die zijn gekoppeld aan uw account op elk gewenst moment. Uw toepassing hoeft niet te worden onderbroken of opnieuw als u wilt toevoegen of verwijderen van een regio zijn geïmplementeerd. Deze maximaal beschikbaar blijft continu vanwege de multi-homingmogelijkheden die de service biedt.

## <a name="key-benefits-of-global-distribution"></a>Belangrijkste voordelen van wereldwijde distributie

**Globale actief / actief-apps bouwen**: met de mogelijkheid meerdere masters, wordt elke regio een regio voor schrijfbewerkingen (behalve dat kan worden gelezen). Meerdere masters functie ook garanties - onbeperkt elastisch schrijven schaalbaarheid 99,999% lezen en schrijven van beschikbaarheid van alle rond de hele wereld en gegarandeerde lees-/ schrijfbewerkingen geleverd in minder dan 10 milliseconden in het 99e percentiel.  

Door het gebruik van Azure Cosmos DB multihoming-API's, uw toepassing is op de hoogte van de dichtstbijzijnde regio en er kunnen aanvragen worden verzonden voor deze regio. Dichtstbijzijnde regio wordt zonder eventuele wijzigingen in de configuratie geïdentificeerd. Het toevoegen en verwijderen van regio's van uw Cosmos DB-account, uw toepassing hoeft niet te worden geïmplementeerd en blijft deze maximaal beschikbaar.

**Bouw uiterst snel reagerende apps**: uw toepassing kan eenvoudig worden ontworpen om uit te voeren in de buurt van real-time lees- en schrijfbewerkingen, met een latentie van slechts enkele milliseconden op basis van alle regio's die u hebt gekozen voor uw database.  De replicatie van gegevens tussen regio's verwerkt Azure Cosmos DB intern zodat het consistentieniveau gekozen voor de Cosmos-account kan worden gegarandeerd.

Veel toepassingen profiteren van de prestatieverbeteringen die worden geleverd met de mogelijkheid om uit te voeren schrijfbewerkingen in meerdere regio's (lokaal). Sommige toepassingen waarvoor grote consistentie wil alle schrijfbewerkingen trechter in één regio. Ter ondersteuning van deze toepassing, ondersteunt Cosmos DB zowel enkele regio en configuraties voor meerdere regio's.

**Maximaal beschikbare apps bouwen**: uitvoeren van een database in meerdere regio's verhoogt de beschikbaarheid van de database. Als één regio niet beschikbaar is, worden in andere regio's aanvragen voor toepassingen automatisch verwerken. Azure Cosmos DB biedt voor 99,999% lezen en schrijven van beschikbaarheid voor databases van meerdere regio's.

**Bedrijfscontinuïteit tijdens een regionale onderbreking**: Azure Cosmos DB biedt ondersteuning voor [automatische failover](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) tijdens een regionale onderbreking. Bovendien tijdens een regionale storing blijft Cosmos DB behouden de latentie, beschikbaarheid, consistentie en doorvoer Sla's. Om te zorgen dat uw gehele toepassing maximaal beschikbaar, biedt Azure Cosmos DB handmatige failover-API voor het simuleren van een regionale onderbreking. U kunt met behulp van deze API, normale zakelijke continuïteit oefeningen uitvoeren.

**Algemene lezen en schrijven van schaalbaarheid**: met meerdere masters capaciteit, kunt u elastisch schalen lezen en schrijven van doorvoer over de hele wereld. Meerdere masters mogelijkheid wordt gegarandeerd dat de doorvoer die uw toepassing wordt geconfigureerd op een Azure Cosmos DB-database of een container wordt geleverd in alle regio's en beveiligd door [met financiële Sla's ondersteund](https://aka.ms/acdbsla).

**Meerdere duidelijk gedefinieerde consistentiemodellen**: replicatie-protocol voor Azure Cosmos DB is ontworpen om u te bieden van vijf duidelijk omschreven, praktische en intuïtieve consistentiemodellen. Elke consistentiemodel heeft een balans tussen consistentie en prestaties. Deze consistentiemodellen kunnen u wereldwijd gedistribueerde toepassingen bouwen met een eenvoudig.

## <a id="Next Steps"></a>Volgende stappen

Meer informatie over globale distributie in de volgende artikelen:

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Regio's van uw Cosmos-account toevoegen/verwijderen](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Een aangepaste conflict resolutie beleid voor de SQL API-accounts maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
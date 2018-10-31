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
ms.openlocfilehash: 68ac603964593892dfcbc3488b4e6f2c91a0eb92
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238271"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale gegevensdistributie met Azure Cosmos DB

Veel van de huidige toepassingen worden uitgevoerd of hebben ambities op op planetaire schaal uitvoeren. Ze altijd zijn ingeschakeld en zijn toegankelijk voor gebruikers over de hele wereld. Beheren van de wereldwijde distributie van gegevens die door deze toepassingen worden gebruikt bij het leveren van hoge prestaties en hoge beschikbaarheid is een vaste probleem. Cosmos DB is een wereldwijd gedistribueerde databaseservice die vanaf het begin heeft is ontworpen om te voldoen aan deze uitdagingen.

Cosmos DB is een fundamentele Azure-service en is beschikbaar in alle [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/) standaard. Microsoft Azure-datacenters in meer dan 50 regio's over de hele wereld werkt en blijft om uit te breiden om te voldoen aan de groeiende behoeften van klanten. Microsoft heeft de Cosmos DB service 24/7, zodat u zich op uw toepassingen richten kunt. Wanneer u een Cosmos DB-account maakt, kunt u bepalen welke regio('s) in moeten worden geïmplementeerd.

Cosmos DB-klanten kunnen hun databases dat deze wereldwijd gedistribueerde en beschikbaar zijn in een willekeurige plaats van 1 tot 50 + Azure-regio configureren. U moet de latentie met een lager, plaatst u de gegevens dichter naar de locatie van uw gebruikers, dus bepalen hoe groot aantal regio's en in welke regio u moet uitvoeren afhankelijk van het globale is bereik van uw toepassing en waar uw gebruikers zijn. Cosmos DB repliceert alle gegevens in uw Cosmos-account transparant naar alle geconfigureerde regio's. Cosmos DB biedt één integraal beeld van uw Cosmos-database en containers, zodat uw toepassing kunt lezen en lokaal schrijven. U kunt met Cosmos DB, toevoegen of verwijderen van de regio's die zijn gekoppeld aan uw account op elk gewenst moment. Uw toepassing hoeft niet te worden onderbroken of opnieuw geïmplementeerd en blijft deze maximaal beschikbaar die gegevens te allen tijde, dankzij de multihoming-mogelijkheden die de service biedt.

## <a name="key-benefits-of-global-distribution"></a>Belangrijkste voordelen van wereldwijde distributie

**Globale actief / actief-apps eenvoudig bouwen**: met de mogelijkheid meerdere masters, elke regio worden geschreven (behalve dat kan worden gelezen), onbeperkt elastisch inschakelen schrijven van schaalbaarheid, 99,999% lezen en schrijven van beschikbaarheid, overal ter wereld, en gegarandeerde snelle lees- en schrijfbewerkingen die worden geleverd in minder dan 10 milliseconden in het 99e percentiel.  

Met Azure Cosmos DB multihoming-API's, uw toepassing altijd weet de dichtstbijzijnde regio en verzendt aanvragen naar deze regio. Dichtstbijzijnde regio wordt zonder eventuele wijzigingen in de configuratie geïdentificeerd. Het toevoegen en verwijderen van regio's van uw Cosmos DB-account, uw toepassing hoeft niet te worden geïmplementeerd en blijft deze maximaal beschikbaar.

**Bouw uiterst snel reagerende apps**: uw toepassing kan eenvoudig worden ontworpen om uit te voeren in de buurt van real-time lees- en schrijfbewerkingen, met slechts enkele milliseconden latentie, op basis van alle regio's die u hebt gekozen voor uw database.  Azure Cosmos DB wordt intern verwerkt de gegevensreplicatie tussen regio's op een manier die het consistentieniveau gekozen voor de Cosmos-account wordt gegarandeerd.

Veel toepassingen profiteren van de prestatieverbeteringen die worden geleverd met de mogelijkheid om uit te voeren schrijfbewerkingen in meerdere regio's (lokaal). In sommige toepassingen, zoals systemen waarvoor grote consistentie wil alle schrijfbewerkingen trechter in één regio. Cosmos DB biedt ondersteuning voor configuraties, één regio en meerdere regio's.

**Maximaal beschikbare apps bouwen**: uitvoeren van een database in meerdere regio's verbetert de beschikbaarheid van de database. Als één regio niet beschikbaar is, worden in andere regio's aanvragen voor toepassingen automatisch verwerken. Azure Cosmos DB biedt voor 99,999% lezen en schrijven van beschikbaarheid voor databases van meerdere regio's.

**Bedrijfscontinuïteit tijdens een regionale onderbreking**: Azure Cosmos DB biedt ondersteuning voor [automatische failover](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) tijdens een regionale onderbreking. Bovendien tijdens een regionale storing blijft Cosmos DB behouden de latentie, beschikbaarheid, consistentie en doorvoer Sla's. Zorg ervoor dat uw app is maximaal beschikbaar biedt, Azure Cosmos DB handmatige failover-API voor het simuleren van een regionale onderbreking. U kunt met behulp van deze API, normale zakelijke continuïteit oefeningen uitvoeren en klaar is.

**Algemene lezen en schrijven van schaalbaarheid**: met meerdere masters capaciteit, kunt u elastisch schalen lezen en schrijven van doorvoer over de hele wereld. Meerdere masters mogelijkheid wordt gegarandeerd dat de doorvoer die uw toepassing wordt geconfigureerd op een Azure Cosmos DB-database of een container wordt geleverd in alle regio's en beveiligd door [met financiële Sla's ondersteund](https://aka.ms/acdbsla).

**Meerdere duidelijk gedefinieerde consistentiemodellen**: replicatie-protocol voor Azure Cosmos DB is ontworpen om u te bieden vijf duidelijk omschreven, praktische en intuïtieve consistentie modellen allemaal zijn voorzien van een duidelijke balans tussen consistentie en prestaties. Deze consistentiemodellen kunnen u juiste wereldwijd gedistribueerde toepassingen bouwen met een eenvoudig.

## <a id="Next Steps"></a>Volgende stappen

Meer informatie over globale distributie in de volgende artikelen:

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Regio's van uw database toevoegen/verwijderen](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Een aangepaste conflict resolutie beleid voor de SQL API-accounts maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
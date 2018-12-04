---
title: Gegevens globaal distribueren met Azure Cosmos DB
description: Meer informatie over wereldwijde geo-replicatie, meerdere masters, failover en gegevens herstellen met behulp van de globale databases van Azure Cosmos DB, een wereldwijd gedistribueerde, multi-model databaseservice.
services: cosmos-db
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.openlocfilehash: 2c217a1a89d3b573bfe2297a263bf55849b5f6e1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843849"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale gegevensdistributie met Azure Cosmos DB

Voor hedendaagse toepassingen hoeven te worden zeer responsieve en altijd online. Als u wilt bereiken met lage latentie en hoge beschikbaarheid, moeten de exemplaren van deze toepassingen worden geïmplementeerd in datacenters die zich dicht bij hun gebruikers. Deze toepassingen worden doorgaans geïmplementeerd in meerdere datacenters en wereldwijd gedistribueerde worden genoemd. Wereldwijd gedistribueerde toepassingen moeten een globaal gedistribueerde database die de gegevens overal ter wereld om in te schakelen van de toepassingen die moeten worden uitgevoerd op een kopie van de gegevens die zich dicht bij de gebruikers daarvan transparant kunt repliceren. 

Azure Cosmos DB is een wereldwijd gedistribueerde databaseservice die is ontwikkeld voor lage latentie, elastische schaalbaarheid van doorvoer, goed gedefinieerde semantiek voor de consistentie van gegevens en hoge beschikbaarheid. Kort gezegd, als uw toepassing nodig gegarandeerde snelle reactietijd overal ter wereld, heeft als dit is vereist om altijd online en het moet onbeperkte en flexibele schaalbaarheid van de doorvoer en opslag, houd rekening met het bouwen van toepassingen met behulp van Azure Cosmos DB.

U kunt uw databases zijn wereldwijd gedistribueerde en beschikbaar zijn in een van de Azure-regio's configureren. Als u wilt de latentie verlagen, plaats de gegevens dichter naar waar uw gebruikers zijn. De vereiste regio's te kiezen, is afhankelijk van het wereldwijde bereik van uw toepassing en waar uw gebruikers zich bevinden. Azure Cosmos DB repliceert de gegevens in uw account transparant naar alle regio's die zijn gekoppeld aan uw account. Het biedt één integraal beeld van de wereldwijd gedistribueerde Azure Cosmos-database en de containers die uw toepassing kunt lezen en schrijven naar lokaal. 

U kunt met Azure Cosmos DB, toevoegen of verwijderen van de regio's die zijn gekoppeld aan uw account op elk gewenst moment. Uw toepassing hoeft niet te worden onderbroken of opnieuw als u wilt toevoegen of verwijderen van een regio zijn geïmplementeerd. Deze maximaal beschikbaar blijft continu vanwege de multihoming-mogelijkheden die de service biedt.

## <a name="key-benefits-of-global-distribution"></a>Belangrijkste voordelen van wereldwijde distributie

**Bouw wereldwijd actief / actief-apps.** Met de functie voor meerdere masters is elke regio een regio voor schrijven. Het is ook kan worden gelezen. De functie voor meerdere masters garandeert ook:

- Onbeperkt aantal elastische schaalbaarheid schrijven. 
- 99,999% lezen en schrijven van de beschikbaarheid van de hele wereld.
- Gegarandeerde schrijf- en leesbewerkingen geleverd in minder dan 10 milliseconden in het 99e percentiel.

Uw toepassing is op de hoogte van de dichtstbijzijnde regio met behulp van de Azure Cosmos DB multihoming-API's. Vervolgens kan verzenden aanvragen voor deze regio. De dichtstbijzijnde regio wordt zonder eventuele wijzigingen in de configuratie geïdentificeerd. Het toevoegen en verwijderen van regio's van uw Azure Cosmos DB-account, moet uw toepassing niet opnieuw implementeren. De toepassing nog steeds maximaal beschikbaar.

**Bouw uiterst snel reagerende apps.** Uw toepassing kan eenvoudig worden ontworpen om uit te voeren in de buurt van real-time lees- en schrijfbewerkingen. Er kan slechts enkele milliseconden latentie gebruik voor alle regio's die u hebt gekozen voor uw database. Azure Cosmos DB wordt intern verwerkt de gegevensreplicatie tussen regio's. Als gevolg hiervan, het consistentieniveau geselecteerd voor de Azure Cosmos DB-account kan worden gegarandeerd.

Veel toepassingen profiteren van de prestatieverbeteringen die worden geleverd met de mogelijkheid om uit te voeren schrijfbewerkingen in meerdere regio's (lokaal). Sommige toepassingen waarvoor grote consistentie wil alle schrijfbewerkingen trechter in één regio. Voor deze toepassingen ondersteunt Azure Cosmos DB één regio en configuraties voor meerdere regio's.

**Maak maximaal beschikbare apps.** Een database die wordt uitgevoerd in meerdere regio's verhoogt de beschikbaarheid van de database. Als één regio niet beschikbaar is, wordt in andere regio's automatisch aanvragen verwerken. Azure Cosmos DB biedt voor 99,999% lezen en schrijven van beschikbaarheid voor databases van meerdere regio's.

**Zakelijke continuïteit onderhouden tijdens een regionale onderbreking.** Azure Cosmos DB ondersteunt [automatische failover](how-to-manage-database-account.md#automatic-failover) tijdens een regionale onderbreking. Tijdens een regionale storing blijft Azure Cosmos DB onderhouden van de SLA's voor latentie, beschikbaarheid, consistentie en doorvoer. Om ervoor te zorgen dat de gehele toepassing maximaal beschikbaar is, biedt Azure Cosmos DB een handmatige failover API voor het simuleren van een regionale onderbreking. Met behulp van deze API kunt u normale zakelijke continuïteit oefeningen uitvoeren.

**Schaal lezen en schrijven doorvoer wereldwijd.** Met de functie voor meerdere masters, kunt u elastisch schalen lezen en schrijven van doorvoer over de hele wereld. De functie voor meerdere masters zorgt ervoor dat de doorvoer die uw toepassing configureert op een Azure Cosmos DB-database of een container wordt geleverd in alle regio's. Ook de doorvoer wordt beveiligd door [met financiële Sla's ondersteund](https://aka.ms/acdbsla).

**Kies uit meerdere duidelijk gedefinieerde consistentiemodellen.** Het protocol van de replicatie Azure Cosmos DB biedt vijf duidelijk omschreven, praktische en intuïtieve consistentiemodellen. Elk model heeft een balans tussen consistentie en prestaties. Gebruik deze consistentiemodellen om wereldwijd gedistribueerde toepassingen met gemak.

## <a id="Next Steps"></a>Volgende stappen

Meer informatie over globale distributie in de volgende artikelen:

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Toevoegen of verwijderen van regio's van uw Azure Cosmos DB-account](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Een aangepaste conflict resolutie beleid voor de SQL API-accounts maken](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
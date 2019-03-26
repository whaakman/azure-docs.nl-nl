---
title: Hoge beschikbaarheid in Azure Cosmos DB
description: Dit artikel wordt beschreven hoe Azure Cosmos DB biedt hoge beschikbaarheid
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/24/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 1e866560ceab342f08a98ba3db05980a2b0947d2
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407552"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Hoge beschikbaarheid met Azure Cosmos DB

Azure Cosmos DB repliceert uw gegevens transparant voor alle Azure-regio's die zijn gekoppeld aan uw Cosmos-account. Cosmos DB heeft meerdere lagen van redundantie voor uw gegevens, zoals wordt weergegeven in de volgende afbeelding:

![Fysieke partitioneren](./media/high-availability/cosmosdb-data-redundancy.png)

- De gegevens binnen de Cosmos-containers zijn [horizontaal gepartitioneerde](partitioning-overview.md).

- Elke partitie wordt binnen elke regio worden beveiligd door een replica-set met alle schrijfbewerkingen gerepliceerd en blijvend zijn doorgevoerd door een meerderheid van de replica's. Replica's worden verdeeld over foutdomeinen voor wel 10-20.

- Elke partitie in alle regio's worden gerepliceerd. Elke regio kan bevat alle gegevenspartities van een Cosmos-container en schrijfbewerkingen accepteren en leesbewerkingen fungeren.  

Als uw Cosmos-account wordt verdeeld over *N* Azure-regio's, zullen er ten minste *N* x 4 kopieën van al uw gegevens. Naast biedt gegevenstoegang met lage latentie en doorvoer voor schrijven/lezen in de regio's die zijn gekoppeld aan uw Cosmos-account schalen, hebt u meer regio's (hoger *N*) verder verbetert de beschikbaarheid.  

## <a name="slas-for-availability"></a>Sla's voor beschikbaarheid

Als een wereldwijd gedistribueerde database biedt Cosmos DB uitgebreide Sla's lengte doorvoer en latentie in het 99e percentiel, consistentie en beschikbaarheid. De volgende tabel toont de garanties voor hoge beschikbaarheid van Cosmos DB voor de accounts voor één of meerdere regio's. Voor hoge beschikbaarheid, configureert u altijd uw Cosmos-accounts voor meerdere regio's voor schrijven.

|Bewerkingstype  | Één regio |Meerdere regio's (schrijfbewerkingen in één regio)|Meerdere regio's (schrijfbewerkingen in meerdere regio's) |
|---------|---------|---------|-------|
|Schrijfbewerkingen    | 99.99    |99.99   |99.999|
|Leesbewerkingen     | 99.99    |99.999  |99.999|

> [!NOTE]
> De beschikbaarheid van de werkelijke schrijven voor gebonden veroudering, sessie, consistent voorvoegsel en modellen van uiteindelijke consistentie is in de praktijk aanzienlijk hoger is dan de gepubliceerde Sla's. De werkelijke leesbeschikbaarheid voor alle consistentieniveaus is aanzienlijk hoger dan de gepubliceerde Sla's.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Hoge beschikbaarheid met Cosmos DB in het geval van regionale storingen

Regionale storingen niet ongebruikelijk en Azure Cosmos DB zorgt ervoor dat de database is altijd maximaal beschikbaar. De volgende details vastleggen Cosmos DB-gedrag tijdens een storing, afhankelijk van de configuratie van uw Cosmos-account:

- Met Cosmos DB, voordat een schrijfbewerking is bevestigd naar de client, de gegevens is blijvend zijn doorgevoerd door een quorum van replicaties in de regio waarin de schrijfbewerkingen zijn toegestaan.

- Accounts voor meerdere regio's geconfigureerd met meerdere schrijfregio's is maximaal beschikbaar zijn voor zowel schrijfbewerkingen en leesbewerkingen. Regionale failovers worden onmiddellijk en eventuele wijzigingen van de toepassing vereisen.

- **Accounts voor meerdere regio's met een één-schrijfregio (uitval schrijven regio):** Tijdens een storing in de regio schrijven, wordt deze accounts hoge mate beschikbaar blijven voor leesbewerkingen. Het schrijven van moet u echter **'automatische failover inschakelen'** op uw Cosmos-account voor de failover de betrokken regio naar een andere regio. De failover wordt uitgevoerd in volgorde van de regio prioriteit die u hebt opgegeven. Als de betrokken regio weer online is de niet-gerepliceerde gegevens aanwezig zijn in de betrokken schrijfregio gedurende de storing beschikbaar wordt gesteld via de [conflicten feed](how-to-manage-conflicts.md#read-from-conflict-feed). Toepassingen kunnen lezen de conflicten feed, de conflicten op basis van de toepassing-specifieke logica en de bijgewerkte gegevens teruggeschreven naar de container van Cosmos waar nodig. Zodra de eerder betrokken schrijfregio wordt hersteld, wordt deze automatisch beschikbaar gesteld als een leesregio. U kunt aanroepen van een handmatige failover en het configureren van de betrokken regio als de schrijfregio. U kunt een handmatige failover opnieuw doen met behulp van [Azure CLI of Azure-portal](how-to-manage-database-account.md#manual-failover). Er is **zonder verlies van gegevens of beschikbaarheid** voor, tijdens of na de handmatige failover. Uw toepassing nog steeds maximaal beschikbaar. 

- **Accounts voor meerdere regio's met een één-schrijfregio (leesregio uitval):** Deze accounts wordt tijdens een storing leesregio hoge mate beschikbaar blijven voor lees- en schrijfbewerkingen. Het betrokken gebied automatisch vanuit de schrijfregio wordt afgebroken en offline worden gemarkeerd. De [Cosmos DB SDK's](sql-api-sdk-dotnet.md) omleiden aanroepen naar de volgende beschikbare regio in de lijst met voorkeursregio leest. Als geen van de regio's in de lijst met voorkeursregio beschikbaar is, terugvallen gesprekken automatisch op de huidige schrijfregio. Er zijn geen wijzigingen zijn vereist in uw toepassingscode om af te handelen leesregio onderbreking. Uiteindelijk, wanneer het betrokken gebied weer online is, de eerder betrokken leesregio wordt automatisch een synchronisatie met de huidige schrijfregio en beschikbaar opnieuw om te lezen aanvragen verwerken. Opeenvolgende leesbewerkingen worden omgeleid naar de herstelde regio zonder wijzigingen in uw toepassingscode. Tijdens de failover zowel lukt van een eerder mislukte regio, consistentie garanties blijven worden herkend door Cosmos DB worden gelezen.

- Accounts voor één regio kunnen verloren gaan beschikbaarheid na een regionale onderbreking. Het is altijd raadzaam om in te stellen **ten minste twee regio's** (bij voorkeur ten minste twee schrijven regio's) met uw Cosmos-account om te hoge beschikbaarheid te allen tijde.

- Zelfs in een zeer vaak voorkomen en zijn erg vervelend gebeurtenis wanneer de Azure-regio permanent niet meer worden hersteld is, er is zonder verlies van gegevens als uw Cosmos-account voor meerdere regio's is geconfigureerd met het standaardconsistentieniveau van *sterke*. In het geval van een permanent niet meer worden hersteld schrijfregio, voor de Cosmos-accounts voor meerdere regio's die geconfigureerd met consistentie voor gebonden veroudering, het venster van de mogelijke gegevens verliezen is beperkt tot het venster veroudering (*K* of *T*); het venster van de mogelijke gegevensverlies is voor sessie, consistent voorvoegsel en uiteindelijke consistentieniveaus, beperkt tot een maximum van vijf seconden.

## <a name="building-highly-available-applications"></a>Het bouwen van maximaal beschikbare toepassingen

- Zorg ervoor dat hoge schrijven en beschikbaarheid voor lezen, configureren van uw Cosmos-account ten minste twee regio's met meerdere schrijfregio's omvatten. Deze configuratie biedt de hoogste beschikbaarheid, de laagste latentie en beste schaalbaarheid voor zowel lees- en schrijfbewerkingen ondersteund door Sla's. Zie voor meer informatie over het [uw Cosmos-account configureren met meerdere schrijven-regio's](tutorial-global-distribution-sql-api.md).

- Voor meerdere regio's Cosmos-accounts die zijn geconfigureerd met een één-schrijfregio, [automatische failover inschakelen met behulp van Azure CLI of Azure-portal](how-to-manage-database-account.md#automatic-failover). Nadat u automatische failover ingeschakeld wanneer er sprake is van een regionaal noodgeval, vindt Cosmos DB automatisch failover plaats uw account.  

- Zelfs als uw Cosmos-account maximaal beschikbaar is, uw toepassing mogelijk niet goed ontworpen voor hoge mate beschikbaar blijven. Als u wilt de hoge beschikbaarheid van de end-to-end van uw toepassing testen, periodiek aanroepen de [handmatige failover met behulp van Azure CLI of Azure-portal](how-to-manage-database-account.md#manual-failover), als onderdeel van uw toepassing testen of herstel na noodgevallen (DR) oefeningen.

- Binnen de databaseomgeving van een wereldwijd gedistribueerde moet u er een directe relatie is tussen de consistentie van niveau en gegevens duurzaamheid met een regiobrede uitval. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit, moet u inzicht in de maximaal acceptabele tijd voordat de toepassing volledig is hersteld na een storing. De tijd die nodig is voor een toepassing om volledig te herstellen, staat bekend als de beoogde hersteltijd (RTO). U moet ook weten wat de maximale periode van recente Gegevensupdates de toepassing kan tolereren verliezen tijdens het herstellen na een storing. De periode van updates die u in het ergste geval kan kwijtraken staat bekend als het beoogde herstelpunt (RPO). Zie voor de RTO en RPO voor Azure Cosmos DB [consistentie niveaus en gegevens duurzaamheid](consistency-levels-tradeoffs.md#rto)

## <a name="next-steps"></a>Volgende stappen

U kunt vervolgens de volgende artikelen lezen:

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Wereldwijd schalen ingerichte doorvoer](scaling-throughput.md)
* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)
* [Hoe u uw Cosmos-account configureert met meerdere regio's voor schrijven](how-to-multi-master.md)

---
title: Hoge beschikbaarheid in Azure Cosmos DB
description: Dit artikel wordt beschreven hoe Azure Cosmos DB biedt hoge beschikbaarheid
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fb7de4a913fa1edb3329e25276f83be2fb6cecd8
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961803"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Hoge beschikbaarheid in Azure Cosmos DB

Azure Cosmos DB repliceert uw gegevens transparant voor alle Azure-regio's die zijn gekoppeld aan uw Cosmos-account. Cosmos DB heeft meerdere lagen van redundantie voor uw gegevens, zoals wordt weergegeven in de volgende afbeelding:

![Partitioneren van resource](./media/high-availability/figure1.png) 

- De gegevens in Cosmos containers is horizontaal gepartitioneerd.

- Elke partitie wordt binnen elke regio worden beveiligd door een replica-set met alle schrijfbewerkingen gerepliceerd en blijvend zijn doorgevoerd door een meerderheid van de replica's. Replica's worden verdeeld over foutdomeinen voor wel 10-20.

- Elke partitie in alle regio's worden gerepliceerd. Elke regio kan bevat alle gegevenspartities van een Cosmos-container en schrijfbewerkingen accepteren en leesbewerkingen fungeren.  

Als uw Cosmos-account wordt verdeeld over N Azure-regio's, worden er ten minste N x 4 kopieën van al uw gegevens. Naast biedt gegevenstoegang met lage latentie en doorvoer voor schrijven/lezen in de regio's die zijn gekoppeld aan uw Cosmos-account schalen, verbetert de beschikbaarheid met meer regio's (hoger N) ook.  

## <a name="slas-for-availability"></a>Sla's voor beschikbaarheid   

Als een wereldwijd gedistribueerde database biedt Cosmos DB uitgebreide Sla's lengte doorvoer en latentie in het 99e percentiel, consistentie en beschikbaarheid. In de volgende tabel wordt beschreven de garanties met betrekking tot hoge beschikbaarheid door Cosmos DB voor accounts voor één of meerdere regio's. Voor hoge beschikbaarheid, configureert u uw Cosmos-accounts voor meerdere regio's voor schrijven.

|Bewerkingstype  | Één regio |Meerdere regio's (schrijfbewerkingen in één regio)|Meerdere regio's (schrijfbewerkingen in meerdere regio's) |
|---------|---------|---------|-------|
|Schrijfbewerkingen    | 99,99    |99,99   |99,999|
|Leesbewerkingen     | 99,99    |99,999  |99,999|

> [!NOTE]
> De beschikbaarheid van de werkelijke schrijven voor gebonden veroudering, sessie, consistent voorvoegsel en uiteindelijke consistentiemodellen is in de praktijk aanzienlijk hoger is dan de gepubliceerde Sla's. De werkelijke leesbeschikbaarheid voor alle consistentieniveaus is aanzienlijk hoger dan de gepubliceerde Sla's.

## <a name="regional-outages"></a>Regionale storingen

Regionale storingen niet ongebruikelijk en Azure Cosmos DB zorgt ervoor dat de database is altijd beschikbaar. De volgende details vastleggen Cosmos DB-gedrag tijdens een storing, afhankelijk van de configuratie van uw Cosmos-account: 

- Accounts voor meerdere regio's geconfigureerd met meerdere schrijfregio's blijven maximaal beschikbaar voor zowel schrijfbewerkingen en leesbewerkingen. Regionale failovers worden onmiddellijk en hoeven niet alle wijzigingen van de toepassing.

- Accounts voor meerdere regio's met een één-schrijfregio: tijdens een storing in de regio schrijven, deze accounts wordt hoge mate beschikbaar blijven voor leesbewerkingen. Echter, wordt het schrijven van moet u 'inschakelen automatische failover' op uw Cosmos-account voor de failover de betrokken regio naar een andere regio die is gekoppeld. De failover wordt uitgevoerd in volgorde van de regio prioriteit die u hebt opgegeven. Uiteindelijk, wanneer het betrokken gebied weer online is, wordt de niet-gerepliceerde gegevens aanwezig zijn in de betrokken schrijfregio gedurende de storing beschikbaar gesteld via de feed conflicten. Toepassingen kunnen lezen de conflicten feed, de conflicten op basis van toepassingsspecifieke logica en de bijgewerkte gegevens teruggeschreven naar de container van Cosmos waar nodig. Zodra de eerder betrokken schrijfregio wordt hersteld, wordt deze automatisch beschikbaar gesteld als een leesregio. U kunt een handmatige failover aanroepen en brengen de betrokken regio bevinden als de schrijfregio. U kunt een handmatige failover doen met behulp van [Azure CLI of Azure-portal](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account).  

- Accounts voor meerdere regio's met een één-schrijfregio: tijdens een storing leesregio deze accounts wordt hoge mate beschikbaar blijven voor lees- en schrijfbewerkingen. Het betrokken gebied automatisch vanuit de schrijfregio wordt afgebroken en offline worden gemarkeerd. De Cosmos DB SDK's worden gelezen aanroepen naar de volgende beschikbare regio in de lijst met voorkeursregio omgeleid. Als geen van de regio's in de lijst met voorkeursregio beschikbaar is, terugvallen gesprekken automatisch op de huidige schrijfregio. Er zijn geen wijzigingen zijn vereist in uw toepassingscode om af te handelen leesregio onderbreking. Uiteindelijk, wanneer het betrokken gebied weer online is, de eerder betrokken leesregio wordt automatisch een synchronisatie met de huidige schrijfregio en beschikbaar opnieuw om te lezen aanvragen verwerken. Opeenvolgende leesbewerkingen worden omgeleid naar de herstelde regio zonder wijzigingen in uw toepassingscode. Tijdens de failover zowel lukt van een eerder mislukte regio, blijven lezen consistentiegarantie worden herkend door Cosmos DB.

- Accounts voor één regio kunnen verloren gaan beschikbaarheid in het geval van regionale uitval. Het is raadzaam om in te stellen van ten minste twee regio's (bij voorkeur ten minste twee schrijfregio's) met uw Cosmos-account om te hoge beschikbaarheid te allen tijde.

## <a name="building-highly-available-applications"></a>Het bouwen van maximaal beschikbare toepassingen

- Zorg ervoor dat hoge schrijven en beschikbaarheid voor lezen, configureren van uw Cosmos-account ten minste twee regio's met meerdere schrijfregio's omvatten. Deze configuratie zorgt ervoor dat de laagste latentie, beschikbaarheid en schaalbaarheid voor zowel lees- en schrijfbewerkingen ondersteund door Sla's. Zie voor meer informatie over het [uw Cosmos-account configureren met meerdere schrijven-regio's](tutorial-global-distribution-sql-api.md).

- Voor meerdere regio's Cosmos-accounts die zijn geconfigureerd met een één-schrijfregio, [automatische failover inschakelen met behulp van Azure CLI of Azure-portal](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account). Nadat u automatische failover ingeschakeld wanneer er sprake is van een regionaal noodgeval, vindt Cosmos DB automatisch failover plaats uw account.  

- Zelfs als uw Cosmos-account maximaal beschikbaar is, uw toepassing mogelijk niet goed ontworpen voor hoge mate beschikbaar blijven. Om ervoor te zorgen de hoge beschikbaarheid van de end-to-end voor uw toepassing, periodiek aanroepen de [handmatige failover met behulp van Azure CLI of Azure-portal](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account), als onderdeel van uw toepassing testen of herstel na noodgevallen (DR) oefeningen. 

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u meer informatie over het schalen van doorvoer in het volgende artikel:

* [Doorvoer vergroten/verkleinen](scaling-throughput.md)

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)

* [Wereldwijd schalen van doorvoer](scaling-throughput.md)

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)

* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)



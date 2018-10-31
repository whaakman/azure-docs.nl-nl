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
ms.openlocfilehash: fdf1bf227ed21111c1aa0754a8423c1ee6aa151b
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244141"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Hoge beschikbaarheid in Azure Cosmos DB

Azure Cosmos DB repliceert uw gegevens transparant voor de Azure-regio's die gekoppeld aan uw Cosmos-account zijn.  Cosmos DB maakt gebruik van meerdere lagen van redundantie voor uw gegevens. Zoals te zien van de afbeelding hieronder:

- De gegevens in Cosmos containers is horizontaal gepartitioneerd.
- Elke partitie wordt binnen elke regio worden beveiligd door een replica-set met alle schrijfbewerkingen gerepliceerd en blijvend zijn doorgevoerd door een meerderheid van de replica's. Replica's worden verdeeld over foutdomeinen voor wel 10-20.
- In alle regio's, worden alle partities gerepliceerd. Elke regio kan bevat alle van de gegevenspartities van een Cosmos-container en schrijfbewerkingen accepteren en leesbewerkingen fungeren.  

![Partitioneren van resource](./media/high-availability/figure1.png) 

Als uw Cosmos-account wordt verdeeld over N Azure-regio's, worden er ten minste N x 4 kopieën van al uw gegevens. Naast het verlenen van toegang met lage latentie tot uw gegevens en vergroten/verkleinen schrijven en leesdoorvoer die in de regio's die zijn gekoppeld aan uw Cosmos-account, verbetert de beschikbaarheid met meer regio's (hoger N) ook.  

## <a name="availability-slas"></a>Beschikbaarheid van SLA 's  

Als een wereldwijd gedistribueerde database biedt Cosmos DB uitgebreide Sla's dat doorvoer en latentie in het 99e percentiel, consistentie en beschikbaarheid. In de volgende tabel wordt beschreven de garanties met betrekking tot hoge beschikbaarheid door Cosmos DB voor accounts voor één of meerdere regio's. Voor de hoogst mogelijke beschikbaarheid, wordt aangeraden uw Cosmos-accounts hebben meerdere schrijfregio's configureren.

|Type bewerking  | Één regio |Meerdere regio's (schrijfbewerkingen in één regio)|Meerdere regio's (meerdere regio's schrijft|
|---------|---------|---------|-------|
|Schrijfbewerkingen    | 99,99    |99,99   |99,999|
|Leesbewerkingen     | 99,99    |99,999  |99,999|

> [!NOTE]
> De beschikbaarheid van de werkelijke schrijven voor gebonden veroudering, sessie, consistent voorvoegsel en uiteindelijke consistentiemodellen is in de praktijk aanzienlijk hoger is dan de gepubliceerde Sla's. En in de praktijk, de beschikbaarheid van de werkelijke lezen voor alle consistentieniveaus is aanzienlijk hoger is dan de gepubliceerde Sla's.

## <a name="regional-outages"></a>Regionale storingen

Regionale storingen niet ongebruikelijk en Azure Cosmos DB zorgt ervoor dat de database is altijd beschikbaar. De volgende vastgelegd Cosmos DB-gedrag tijdens een storing, afhankelijk van de configuratie van uw Cosmos-account: 

- Accounts voor meerdere regio's geconfigureerd met meerdere schrijfregio's blijven maximaal beschikbaar voor zowel schrijf- en leesbewerkingen tijdens een regionale onderbreking. Regionale failovers worden onmiddellijk en hoeven niet alle wijzigingen van de toepassing.

- Accounts voor meerdere regio's met een één-schrijfregio tijdens een storing in de regio schrijven wordt hoge mate beschikbaar blijven voor leesbewerkingen. Echter voor schrijfbewerkingen moet u 'inschakelen automatische failover' op uw Cosmos-account voor Cosmos DB voor de failover de betrokken regio naar een andere regio die is gekoppeld aan uw Cosmos-account. De failover wordt uitgevoerd in volgorde van de regio prioriteit die u hebt opgegeven. Uiteindelijk, wanneer het betrokken gebied weer online is, wordt de niet-gerepliceerde gegevens aanwezig zijn in de betrokken schrijfregio gedurende de storing beschikbaar gesteld via de feed conflicten. Toepassingen kunnen lezen de conflicten feed, de conflicten op basis van toepassingsspecifieke logica en de bijgewerkte gegevens teruggeschreven naar de container van Cosmos waar nodig. Zodra de eerder betrokken schrijfregio wordt hersteld, wordt deze automatisch beschikbaar gesteld als een leesregio. U kunt een handmatige failover aanroepen en brengen de betrokken regio bevinden als de schrijfregio. U kunt een handmatige failover doen met behulp van Azure CLI of Azure-portal.  

- Accounts voor meerdere regio's met een één-schrijfregio tijdens een storing in de leesregio blijven maximaal beschikbaar voor lees- en schrijfbewerkingen. Het betrokken gebied automatisch vanuit de schrijfregio wordt afgebroken en offline worden gemarkeerd. De Cosmos DB SDK's worden gelezen aanroepen naar de volgende beschikbare regio in de lijst met voorkeursregio omgeleid. Als geen van de regio's in de lijst met voorkeursregio beschikbaar is, terugvallen gesprekken automatisch op de huidige schrijfregio. Er zijn geen wijzigingen zijn vereist in uw toepassingscode om af te handelen leesregio onderbreking. Uiteindelijk, wanneer het betrokken gebied weer online is, de eerder betrokken leesregio wordt automatisch een synchronisatie met de huidige schrijfregio en opnieuw voor leesbewerkingen die beschikbaar zijn. Opeenvolgende leesbewerkingen worden omgeleid naar de herstelde regio zonder wijzigingen in uw toepassingscode. Tijdens de failover zowel lukt van een eerder mislukte regio, blijven lezen consistentiegarantie worden herkend door Cosmos DB.

- Accounts voor één regio kunnen verloren gaan beschikbaarheid in het geval van regionale uitval. We raden ten zeerste ten minste twee regio's (bij voorkeur ten minste twee schrijfregio's) met uw Cosmos-account om te hoge beschikbaarheid te allen tijde instellen.

## <a name="building-highly-available-applications"></a>Het bouwen van maximaal beschikbare toepassingen

- Zorg ervoor dat hoge schrijven en beschikbaarheid voor lezen, configureren van uw Cosmos-account ten minste twee regio's met meerdere schrijfregio's omvatten. Deze configuratie zorgt ervoor dat de aanbevolen beschikbaarheid, laagste latentie en schaalbaarheid voor lees- en schrijfbewerkingen basis van serviceovereenkomsten. Zie hoe u [uw Cosmos-account configureren met meerdere schrijven-regio's](tutorial-global-distribution-sql-api.md).

- Inschakelen voor meerdere regio's Cosmos-accounts die zijn geconfigureerd met een één-schrijfregio, 'automatische failover' met behulp van Azure CLI of Azure-portal.  Nadat u automatische failover ingeschakeld wanneer er sprake is van een regionaal noodgeval, vindt Cosmos DB automatisch failover plaats uw account.  

- Zelfs als uw Cosmos-account maximaal beschikbaar is, uw toepassing mogelijk niet goed ontworpen voor hoge mate beschikbaar blijven. Zorg ervoor dat de hoge beschikbaarheid van de end-to-end voor uw toepassing, de 'handmatige failover"periodiek aanroepen met behulp van Azure CLI of Azure-portal, als onderdeel van uw toepassing testen of herstel na noodgevallen (DR) ingezoomd. Zie voor meer informatie, de regionale prioriteiten voor uw Cosmos-account wijzigen).  

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u meer informatie over het schalen van doorvoer in het volgende artikel:

* [Doorvoer vergroten/verkleinen](scaling-throughput.md)

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)

* [Wereldwijd schalen van doorvoer](scaling-throughput.md)

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)

* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)



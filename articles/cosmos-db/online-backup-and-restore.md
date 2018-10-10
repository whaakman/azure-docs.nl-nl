---
title: Online back-up en herstel met Azure Cosmos DB | Microsoft Docs
description: Informatie over het uitvoeren van automatische back-up en herstellen op een Azure Cosmos DB-database.
keywords: back-up en herstel, online back-up
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: 657b75e5e3bb5c35bb23221235e62298fc797046
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902668"
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatische online back-up en herstel met Azure Cosmos DB
Azure Cosmos DB maakt back-ups van al uw gegevens automatisch met regelmatige intervallen. De automatische back-ups worden zonder gevolgen voor de prestaties of beschikbaarheid van uw databasebewerkingen genomen. Uw back-ups apart zijn opgeslagen in een andere storage-service, en deze back-ups wereldwijd voor bescherming tegen regionale rampen zijn gerepliceerd. De automatische back-ups zijn bedoeld voor scenario's als u per ongeluk uw Cosmos DB-container verwijdert en herstel van gegevens later nodig.  

In dit artikel begint met een korte samenvatting van de gegevensredundantie en beschikbaarheid in Cosmos DB, en vervolgens besproken back-ups. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Hoge beschikbaarheid met Cosmos DB - een samenvatting
Cosmos DB is ontworpen om te worden [wereldwijd gedistribueerde](distribute-data-globally.md) – Hiermee kunt u de doorvoer te schalen over meerdere Azure-regio's samen met beleid voor failover en transparante multihoming-API's. Azure Cosmos DB biedt [99,99% beschikbaarheids-Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db) voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en leesbeschikbaarheid van 99,999% beschikbaarheid voor alle databaseaccounts voor meerdere regio's. Alle schrijfbewerkingen in Azure Cosmos DB zijn voordat de client zijn bevestigd definitief doorgevoerd naar lokale schijven op een quorum van replicaties in een lokaal Datacenter. De hoge beschikbaarheid van Cosmos DB is gebaseerd op lokale opslag en is niet afhankelijk van een externe opslagtechnologieën. Als uw databaseaccount gekoppeld aan meer dan één Azure-regio is, worden bovendien de schrijfbewerkingen in andere regio's ook gerepliceerd. U kunt om te schalen uw doorvoer en gegevens met lage latentie, zoals bij veel lees regio's die zijn gekoppeld aan uw databaseaccount als u wilt hebben. In elke regio lezen, is de (gerepliceerde) gegevens blijvend blijven behouden tussen een replicaset.  

Zoals wordt geïllustreerd in het volgende diagram, een enkele container van Cosmos DB is [horizontaal gepartitioneerde](partition-data.md). Een "partitie" wordt aangeduid met een cirkel in het volgende diagram en elke partitie is maximaal beschikbaar is via de replica's gemaakt. Dit is de lokale distributie binnen een enkel Azure-regio (aangeduid met de X-as). Elke partitie (met de bijbehorende replica is ingesteld) wordt verder dan wereldwijd gedistribueerd over meerdere regio's die zijn gekoppeld aan uw databaseaccount (bijvoorbeeld: in deze afbeelding de drie regio's: VS-Oost, VS-West en centraal-India). De 'partitieset' is een wereldwijd gedistribueerde entiteit zijn opgebouwd met meerdere kopieën van uw gegevens in elke regio (aangeduid met de Y-as). U kunt de prioriteit toewijzen aan de regio's die zijn gekoppeld aan uw databaseaccount en Cosmos DB wordt transparante failover naar de volgende regio in geval van nood. U kunt ook handmatig failover als u wilt de end-to-end beschikbaarheid van uw toepassing testen simuleren.  

De volgende afbeelding ziet u de hoge mate van redundantie met Cosmos DB.

![Hoge mate van redundantie met Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Hoge mate van redundantie met Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Volledige, automatische, online back-ups
Oeps, ik heb verwijderd mijn container of de database. Met Cosmos DB, niet alleen uw gegevens, maar de back-ups van uw gegevens zijn ook aangebracht maximaal redundante en flexibele regionale rampen. Deze geautomatiseerde back-ups op dat moment gaat ongeveer elke vier uur en meest recente twee back-ups te allen tijde worden opgeslagen. Als de gegevens per ongeluk is verwijderd of is beschadigd, neem dan contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/options/) binnen acht uur. 

De back-ups worden zonder gevolgen voor de prestaties of beschikbaarheid van uw databasebewerkingen genomen. Cosmos DB gebruikt de back-up op de achtergrond, zonder uw ingerichte ru's verbruiken of die betrekking hebben op de prestaties en zonder gevolgen voor de beschikbaarheid van uw database. 

In tegenstelling tot de gegevens die zijn opgeslagen in Azure Cosmos DB, worden de automatische back-ups worden opgeslagen in Azure Blob Storage-service. De momentopname van de back-up is om te garanderen dat het uploaden van de lage latentie/efficiënt, geüpload naar een exemplaar van Azure Blob-opslag in dezelfde regio als de huidige schrijfregio van uw Cosmos DB-databaseaccount. Voor bescherming tegen regionale na noodgevallen, wordt elke momentopname van uw back-upgegevens in Azure Blob-opslag opnieuw gerepliceerd via geo-redundante opslag (GRS) naar een andere regio. Het volgende diagram ziet u dat de volledige Cosmos DB-container (met alle drie primaire partities in VS-West, in dit voorbeeld) de back-up in een externe Azure Blob Storage-account in VS-West en vervolgens GRS is gerepliceerd naar VS-Oost. 

De volgende afbeelding ziet periodieke volledige back-ups van alle Cosmos DB-entiteiten in Azure Storage voor GRS.

![Periodieke volledige back-ups van alle Cosmos DB-entiteiten in GRS Azure Storage](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Bewaarperiode voor back-up
Zoals hierboven beschreven, duurt Azure Cosmos DB momentopnamen van uw gegevens om de vier uur op het niveau van de partitie. Op elk moment worden alleen de laatste twee momentopnamen worden bewaard. Echter, als de container/database wordt verwijderd, behoudt Azure Cosmos DB de bestaande momentopnamen voor alle verwijderde partities binnen de opgegeven container/database gedurende 30 dagen.

Voor SQL-API, als u wilt behouden, uw eigen momentopnamen, kunt u doen met behulp van de volgende opties:

* Het exporteren naar een JSON-optie gebruiken in de Azure Cosmos DB [hulpprogramma voor gegevensmigratie](import-data.md#export-to-json-file) aanvullende back-ups plannen.

* Gebruik [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) periodiek verplaatsen van gegevens.

* Azure Cosmos DB gebruiken [wijzigingenfeed](change-feed.md) lezen van gegevens periodiek voor volledige back-up en afzonderlijk voor incrementele wijzigen en verplaatsen naar de bestemming van uw blob. 

* Voor het beheren van warme back-ups, is het mogelijk om gegevens periodiek uit wijzigingenfeeds lezen en vertraging van het schrijven naar een andere verzameling. Dit zorgt ervoor hoeft u niet om de gegevens te herstellen en u onmiddellijk de gegevens voor het probleem kunt kijken. 

> [!NOTE]
> Als u "Inrichten doorvoer voor een set van containers op databaseniveau" – Houd er rekening mee wordt de terugzetbewerking gebeurt op niveau van de account volledige Database. Ook moet u ervoor zorgen dat contact binnen acht uur het ondersteuningsteam als u de container voor het per ongeluk verwijdert. Gegevens kan niet worden hersteld als u geen contact opnemen met het ondersteuningsteam binnen acht uur.

## <a name="restoring-a-database-from-an-online-backup"></a>Herstellen van een database vanuit een online back-up

Als u uw database of de container per ongeluk verwijdert, kunt u [een ondersteuningsticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) of [contact opnemen met ondersteuning voor Azure](https://azure.microsoft.com/support/options/) de gegevens van de laatste automatische back-up wilt terugzetten. Azure-ondersteuning is beschikbaar voor de geselecteerde abonnementen alleen zoals Standard, ontwikkelaar, wordt niet ondersteund bij Basic-abonnement. Zie voor meer informatie over de verschillende ondersteuningsplannen, [ondersteuningsabonnementen voor Azure](https://azure.microsoft.com/support/plans/) pagina. 

Als u wilt herstellen van uw database vanwege een probleem met gegevens (inclusief gevallen waar de documenten in een container worden verwijderd), raadpleegt u [beschadigde gegevens verwerken](#handling-data-corruption) behoefte aanvullende stappen uitvoeren om te voorkomen dat de beschadigde gegevens overschrijft de bestaande back-ups. Voor een specifieke momentopname van de back-up kunnen worden hersteld, vereist Cosmos DB dat de gegevens beschikbaar voor de duur van de back-cyclus die momentopname te maken is.

> [!NOTE]
> Databases of verzamelingen kunnen alleen op expliciete klantaanvragen worden hersteld. Het is de verantwoordelijkheid van de klant te verwijderen van de container of de database direct na het afstemmen van de gegevens. Als u niet de herstelde databases of verzamelingen verwijdert, ze worden kosten in rekening gebracht voor aanvraageenheden, opslag en uitgaand verkeer.

## <a name="handling-data-corruption"></a>Beschadigde gegevens verwerken

Azure Cosmos DB behoudt de laatste twee back-ups van elke partitie in de databaseaccount. Dit model is geschikt als een container (verzameling van documenten, grafiek en tabel) of een database per ongeluk worden verwijderd omdat een van de laatste versies kan worden hersteld. Echter, in het geval wanneer gebruikers leiden een probleem met beschadigde gegevens tot kunnen, Azure Cosmos DB kunnen zich niet bewust zijn van de beschadigde gegevens en het is mogelijk de beschadiging van de bestaande back-ups mogelijk zijn overschreven. 

Als beschadigd wordt gedetecteerd, moet de gebruiker de beschadigde container (verzameling/grafiek/table) verwijderen zodat back-ups worden beveiligd met beschadigde gegevens wordt overschreven. En bovenal contact op met Microsoft Support en een ticket bij de specifieke aanvraag van ernst 2 verhogen. 

De volgende afbeelding ziet u het maken van de aanvraag voor ondersteuning voor container(collection/graph/table) herstellen via Azure-portal voor het per ongeluk verwijderen of bijwerken van gegevens binnen een container

![Herstellen van een container voor onjuiste bijwerken of verwijderen van gegevens in Cosmos DB](./media/online-backup-and-restore/backup-restore-support.png)

Als het herstellen is voltooid voor dit soort scenario's - gegevens wordt hersteld naar een ander account (met het achtervoegsel van '-hersteld ') en de container. Deze terugzetten wordt niet uitgevoerd in plaats voor een kans dat aan de klant valideren van de gegevens en de gegevens zo nodig verplaatsen. De herstelde container is in dezelfde regio met dezelfde ru's en beleidsregels voor indexering. Gebruiker beheerder van abonnement of coadmin is ziet dit herstelde account.


> [!NOTE]
> Als u de gegevens voor het oplossen van het probleem te herstellen of alleen voor testdoeleinden, neem van plan bent om ze te verwijderen binnenkort kost zoals de taak wordt uitgevoerd als hersteld containers of een database extra - op basis van de ingerichte doorvoer. 
## <a name="next-steps"></a>Volgende stappen

Als u wilt repliceren van uw database in meerdere datacenters, Zie [uw gegevens globaal distribueren met Cosmos DB](distribute-data-globally.md). 

Op bestand Neem contact op met ondersteuning voor Azure, [bestand een ticket van de Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


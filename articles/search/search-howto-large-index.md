---
title: Uitschalen met ingebouwde indexeerfuncties - Azure Search indexeren
description: Nieuwe elementen toe te voegen, bestaande elementen of documenten bijwerken of verwijderen van verouderde documenten in een volledig opnieuw samenstellen of een gedeeltelijke incrementele indexeren om te vernieuwen van een Azure Search-index.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 5f268de43f4f860458c062cb80e5bea0134b4407
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316666"
---
# <a name="how-to-scale-out-indexing-in-azure-search"></a>Hoe u scale-out in Azure Search indexeren

Als de gegevensvolumes toenemen of verwerking moet wijzigen, ziet u wellicht dat eenvoudige [opnieuw op te bouwen en opnieuw indexeren taken](search-howto-reindex.md) zijn niet voldoende. 

Als eerste stap bij het nakomen van toegenomen vraag, wordt aangeraden dat u verhoogt de [schaalbaarheid en capaciteit](search-capacity-planning.md) binnen de grenzen van uw bestaande service. 

Een tweede stap als kunt u [indexeerfuncties](search-indexer-overview.md), mechanismen voor schaalbare indexeren wordt toegevoegd. Indexeerfuncties worden geleverd met een ingebouwde scheduler waarmee u kunt het pakket uit het indexeren met regelmatige intervallen of uitbreiden van de verwerking van buiten de 24-uurs tijdvenster. Daarnaast in combinatie met gegevensbrondefinities, helpen indexeerfuncties u bij het behalen van een vorm van parallelle uitvoering door te partitioneren van gegevens en schema's parallel uitvoeren.

### <a name="scheduled-indexing-for-large-data-sets"></a>Geplande indexering voor grote gegevenssets

Planning is een belangrijk mechanisme voor het verwerken van grote gegevenssets en trage analyses, zoals het analyseren van afbeeldingen in een pijplijn cognitief zoeken. Verwerking van de indexeerfunctie werkt binnen een 24-uurs tijdvenster. Als de verwerking niet kan worden voltooid binnen 24 uur, wordt het gedrag van het plannen van de indexeerfunctie kunnen werken in uw voordeel. 

Standaard gepland indexering begint met een bepaald interval, met een taak die doorgaans voltooien voordat u met de volgende geplande interval wordt hervat. Als de verwerking niet binnen het interval wordt voltooid, stopt de indexeerfunctie echter (vanwege tijd onvoldoende). Op het volgende interval bijhouden verwerking wordt hervat waar deze laatste afgebroken, met het system-bijhouden van waar dat wordt uitgevoerd. 

U kunt de indexeerfunctie volgens een schema 24-uurs plaatsen in de praktijk bij index belastingen spanning enkele dagen. Tijdens het indexeren wordt hervat voor de volgende 24 uur per dag in het verleden, opnieuw wordt gestart op de laatste bekende goede document. Op deze manier kan een indexeerfunctie eraan via een achterstand van het document werken gedurende een reeks dagen totdat alle niet-verwerkte documenten worden verwerkt. Zie voor meer informatie over deze benadering, [indexeren van grote gegevenssets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Parallelle indexeren

Een tweede keuze is voor het instellen van een parallelle strategie te indexeren. Voor niet-routine, vereisten, zoals OCR op gescande documenten in een pijplijn voor cognitief zoeken, indexeren voor rekenintensieve een parallelle indexeren strategie mogelijk de juiste aanpak voor die specifieke doel. Analyse van de afbeelding en verwerking van natuurlijke taal zijn langere tijd worden uitgevoerd in een pijplijn van de verrijking cognitief zoeken. Parallelle indexeren van een service die niet tegelijkertijd van queryaanvragen verwerken is wordt mogelijk een beschikbare optie voor het uitvoeren van een grote instantie van de inhoud van de langzame-verwerking. 

Een strategie voor parallelle verwerking heeft de volgende elementen:

+ Brongegevens tussen meerdere containers of meerdere virtuele mappen in dezelfde container delen. 
+ Elke mini gegevensset die moet worden toegewezen een [Datumbron](https://docs.microsoft.com/rest/api/searchservice/create-data-source), in een eigen gekoppelde [indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Verwijzen naar hetzelfde voor cognitief zoeken [vaardigheden](https://docs.microsoft.com/rest/api/searchservice/create-skillset) in elke definitie van de indexeerfunctie.
+ Schrijven naar het hetzelfde doel search-index. 
+ Alle indexeerfuncties om uit te voeren op hetzelfde moment plannen.

> [!Note]
> Azure Search biedt geen ondersteuning voor uitsluitend replica's of partities op specifieke workloads. Het risico van zware gelijktijdige indexeren is uw systeem ten koste van prestaties van query's overbelasten. Als u een testomgeving hebt, voert u parallelle indexeren er eerst voor meer informatie over het gebruik van systeembronnen.

## <a name="configure-parallel-indexing"></a>Parallelle indexering configureren

Voor indexeerfuncties, capaciteit voor het verwerken is los op basis van een indexeerfunctie-subsysteem voor elke service-eenheid (SU) die wordt gebruikt door uw search-service. Er zijn meerdere gelijktijdige indexeerfuncties op Azure Search-services die zijn ingericht op basis of standaard lagen hebben van ten minste twee replica's mogelijk. 

1. In de [Azure-portal](https://portal.azure.com), op het servicedashboard van uw zoekopdracht **overzicht** pagina, Controleer de **prijscategorie** om te bevestigen is geschikt voor parallelle indexeren. De lagen basis en standaard bieden meerdere replica's.

2. In **instellingen** > **schaal**, [replica's vergroten](search-capacity-planning.md) voor parallelle verwerking: één extra replica voor elke workload van een indexeerfunctie. Laat voldoende voor bestaande queryvolume. Dit ten koste gaat querywerkbelastingen voor indexeren is niet een goede afweging.

3. Gegevens verdelen over meerdere containers op een niveau dat Azure Search-Indexeerfuncties kunnen bereiken. Dit wordt mogelijk meerdere tabellen in Azure SQL Database, meerdere containers in Azure Blob-opslag of meerdere verzamelingen. Definieer een gegevensbronobject voor elke tabel of de container.

4. Maken en plannen van meerdere indexeerfuncties om parallel uit voeren:

   + Wordt ervan uitgegaan dat een service met zes replica's. Configureer zes indexeerfuncties, elke service die is toegewezen aan een gegevensbron met een zesde van de gegevensset voor een 6-manier splitsing van de volledige gegevensset. 

   + Elke indexeerfunctie verwijzen naar dezelfde index. Voor workloads met cognitief zoeken, in de dezelfde vaardigheden Wijs elke indexeerfunctie.

   + Binnen elke definitie van de indexeerfunctie hetzelfde patroon runtime-uitvoering plannen Bijvoorbeeld, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` een planning maakt op 2018-05-15 op alle indexeerfuncties, met intervallen van acht uur.

Alle indexeerfuncties begint op het geplande tijdstip wordt uitgevoerd, het laden van gegevens, het toepassen van enrichments (als u een pijplijn cognitief zoeken geconfigureerd) en het schrijven naar de index. Azure Search wordt de index voor de updates niet worden vergrendeld. Gelijktijdige schrijfbewerkingen worden beheerd met nieuwe pogingen als een bepaalde schrijven niet op de eerste poging is gelukt.

> [!Note]
> Overweeg bij het verhogen van replica's, het aantal partities verhogen als de grootte van prognoses aanzienlijk toenemen. Partities kunnen worden opgeslagen segmenten van de geïndexeerde inhoud: de meer partities die u hebt, de kleinere segmenten elkaar heeft om op te slaan.

## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [Indexeren in de portal](search-import-data-portal.md)
+ [Indexeerfunctie voor Azure SQL-Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md)
+ [Indexeerfunctie voor Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexeerfunctie voor Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Beveiliging in Azure Search](search-security-overview.md)
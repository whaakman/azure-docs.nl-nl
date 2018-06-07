---
title: Opnieuw samenstellen van een Azure Search-index of vernieuwen doorzoekbare inhoud | Microsoft Docs
description: Toevoegen van nieuwe elementen, bestaande elementen of documenten bijwerken of verwijderen van verouderde documenten in een volledig opnieuw samenstellen of een gedeeltelijke incrementele indexeren om te vernieuwen van een Azure Search-index.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: cb99096c1217fca1527b17946dc12390ddf3f62c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655222"
---
# <a name="how-to-scale-out-indexing-in-azure-seearch"></a>Het indexeren van scale-out in Azure Seearch

Als gegevensvolumes groeien of verwerking wijzigen moet, ziet u wellicht dat eenvoudige [opnieuw worden opgebouwd en het opnieuw indexeren taken](search-howto-reindex.md) zijn niet voldoende. 

Als eerste stap bij het nakomen van de toegenomen vraag het is raadzaam dat u verhoogt de [schaal en capaciteit](search-capacity-planning.md) binnen de grenzen van uw bestaande service. 

Een tweede stap als kunt u [indexeerfuncties](search-indexer-overview.md), voegt mechanismen voor schaalbare indexeren. Indexeerfuncties worden geleverd met een ingebouwde scheduler waarmee u het pakket uit met regelmatige tussenpozen te indexeren of breid verwerking buiten het venster 24 uur. Bovendien wanneer gekoppeld aan de definities van gegevensbronnen, kunnen indexeerfuncties u een vorm van parallelle uitvoering door te partitioneren van gegevens en schema's worden parallel uitgevoerd.

### <a name="scheduled-indexing-for-large-data-sets"></a>Geplande indexering voor grote gegevenssets

Planning is een belangrijk mechanisme voor het verwerken van grote gegevenssets en analyses van trage uitgevoerd zoals analyse van de installatiekopie in een pijplijn cognitieve zoeken. Verwerking van de indexeerfunctie wordt uitgevoerd in een 24-uurs-venster. Als de verwerking niet kan worden voltooid binnen 24 uur, kunnen het gedrag van het plannen van indexeerfunctie in uw voordeel werken. 

Standaard gepland indexering begint met een bepaald interval aan een taak die doorgaans voltooien voordat wordt hervat op het volgende geplande interval. Als verwerking niet binnen het interval wordt voltooid, stopt de indexeerfunctie echter (vanwege onvoldoende tijd). Met het volgende interval bijhouden verwerking hervat waar het laatst werd afgebroken, met het bijhouden van het systeem van waar dit het geval is. 

U kunt de indexeerfunctie volgens een schema 24-uurs plaatsen in de praktijk voor index belastingen spanning enkele dagen. Tijdens het indexeren hervat voor de volgende 24-uurs stint, opnieuw wordt gestart op de laatste bekende goede document. Op deze manier kunt een indexeerfunctie gedurende een reeks dagen totdat alle niet-verwerkte documenten worden verwerkt door een achterstand document werken. Zie voor meer informatie over deze benadering [indexeren grote gegevenssets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Parallelle indexeren

Een tweede keuze is voor het instellen van een parallelle strategie te indexeren. Voor niet-routine, rekenkracht vereisten op, zoals OCR op gescande documenten in een pijplijn cognitieve search indexeren een parallelle indexeren strategie mogelijk de juiste aanpak voor die specifieke doel. In een pijplijn cognitieve zoeken verrijking zijn de installatiekopie van analyse en verwerking van natuurlijke taal lang uitgevoerd. Parallelle indexeren op een service die niet tegelijkertijd queryaanvragen verwerkt mogelijk een praktische optie voor het uitvoeren van een grote hoeveelheid inhoud trage verwerking. 

Een strategie voor parallelle verwerking heeft deze elementen:

+ Delen van gegevens tussen meerdere containers of meerdere virtuele mappen in de container. 
+ Toewijzen van elke mini set gegevens naar een [Datumbron](https://docs.microsoft.com/rest/api/searchservice/create-data-source), gekoppeld aan een eigen [indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Verwijzen naar dezelfde voor cognitieve zoekopdracht [vaardigheden](https://docs.microsoft.com/rest/api/searchservice/create-skillset) in elke definitie van de indexeerfunctie.
+ Schrijven naar het hetzelfde doel search-index. 
+ Plan alle indexeerfuncties om uit te voeren op hetzelfde moment.

> [!Note]
> Azure Search biedt geen ondersteuning voor dat replica's of partities voor specifieke werkbelastingen. Het risico van zware gelijktijdige indexeren is uw systeem ten koste van de prestaties van query's te overbelasten. Als u een testomgeving hebt, implementeert u parallelle indexeren er eerst voor informatie over de voor-en nadelen.

## <a name="configure-parallel-indexing"></a>Parallelle indexering configureren

Voor indexeerfuncties, verwerkingscapaciteit los gebaseerd op een indexeerfunctie subsysteem voor elke service-eenheid (SU) die wordt gebruikt door uw search-service. Er zijn meerdere gelijktijdige indexeerfuncties op Azure Search-services die zijn ingericht op basis of standaard lagen met ten minste twee replica's mogelijk. 

1. In de [Azure-portal](https://portal.azure.com), op uw dashboard search-service **overzicht** pagina controle van de **prijscategorie** om te bevestigen is geschikt voor parallelle indexeren. Basic- en Standard lagen bieden meerdere replica's.

2. In **instellingen** > **Scale**, [replica's vergroten](search-capacity-planning.md) voor parallelle verwerking: één extra replica voor elke werkbelasting indexeerfunctie. Laat voldoende voor bestaande query volume. Verlies van werkbelastingen voor indexeren query is niet een goede afweging.

3. Gegevens verdelen over meerdere containers op een niveau die Azure Search Indexeerfuncties kunnen bereiken. Dit kan meerdere tabellen in Azure SQL Database, meerdere containers in Azure Blob-opslag of meerdere verzamelingen zijn. Definieer een gegevensbronobject voor elke tabel of de container.

4. Maken en plannen van meerdere indexeerfuncties parallel worden uitgevoerd:

   + Stel dat een service met zes replica's. Configureer zes indexeerfuncties, elk criterium toegewezen aan een gegevensbron met een-zesde van de gegevensverzameling voor een 6-manier splitsing van de volledige gegevensset. 

   + Elke indexeerfunctie verwijzen naar dezelfde index. De dezelfde vaardigheden Wijs elke indexeerfunctie cognitieve zoeken werkbelastingen.

   + Binnen elke definitie indexeerfunctie plannen hetzelfde patroon runtime-uitvoering. Bijvoorbeeld: `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` maakt u een planning op 2018-05-15 op alle indexeerfuncties, met een interval van acht uur uitgevoerd.

Op het geplande tijdstip beginnen alle indexeerfuncties kan worden uitgevoerd bij het laden van gegevens, het toepassen van enrichments (als u een pijplijn cognitieve zoeken geconfigureerd) en het schrijven naar de index. De index voor updates wordt niet door Azure Search worden vergrendeld. Gelijktijdige schrijfbewerkingen worden beheerd met opnieuw als een bepaalde schrijven niet op de eerste poging is gelukt.

> [!Note]
> Overweeg bij replica's vergroten, het aantal partities verhogen als de indexgrootte van de prognoses aanzienlijk toenemen. Partities kunnen worden opgeslagen segmenten van de geïndexeerde inhoud; meer partities die u hebt, des te kleiner het segment elkaar heeft om op te slaan.

## <a name="see-also"></a>Zie ook

+ [Overzicht van de indexeerfunctie](search-indexer-overview.md)
+ [In de portal te indexeren](search-import-data-portal.md)
+ [Azure SQL Database-indexeerfunctie](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-indexeerfunctie](search-howto-index-cosmosdb.md)
+ [Indexeerfunctie voor Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Indexeerfunctie voor Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Beveiliging in Azure Search](search-security-overview.md)
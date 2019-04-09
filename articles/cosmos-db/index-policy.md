---
title: Azure Cosmos DB indexeringsbeleid
description: Begrijp hoe indexering werkt in Azure Cosmos DB. Informatie over het configureren en wijzigen van het indexeringsbeleid voor automatische indexering en betere prestaties.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: 6998db1679e67f8ac4bf7c81ea9373c66a9618ee
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278561"
---
# <a name="index-policy-in-azure-cosmos-db"></a>Index-beleid in Azure Cosmos DB

U kunt overschrijven de standaardbeleidsregels voor indexering van beleid voor een Azure Cosmos-container door het configureren van de volgende parameters:

* **Of -items en paden uitsluiten bij de index**: U kunt uitsluiten of specifieke items in de index bevatten bij het invoegen of vervangen door de items in een container. U kunt ook opnemen of uitsluiten van specifieke paden/eigenschappen containers worden geïndexeerd. Paden kunnen bevatten jokertekenpatronen, bijvoorbeeld: *.

* **Index typen configureren**: Daarnaast om een bereik geïndexeerde paden, u kunt andere typen toevoegen van indexen, zoals ruimtelijke.

* **Configureren van index modi**: Met behulp van het indexeringsbeleid voor een container, kunt u verschillende modi voor indexering zoals *consistente* of *geen*.

## <a name="indexing-modes"></a>Modi indexeren

Azure Cosmos DB ondersteunt twee modi voor indexering die u op een Azure Cosmos-container via indexeringsbeleid kunt configureren:

* **Consistente**: Als een Azure Cosmos-container-beleid is ingesteld op *consistente*, volgt u de query's op een specifieke container hetzelfde consistentieniveau als het account dat is opgegeven voor punt-leesbewerkingen (bijvoorbeeld: sterk, gebonden veroudering, sessie, of uiteindelijke). 

  De index wordt synchroon bijgewerkt tijdens het bijwerken van de items. Bijvoorbeeld, resulteert invoegen, vervangen, update en delete-bewerkingen op een item in de index-update. Consistente indexeren biedt ondersteuning voor consistente-query's leiden die invloed hebben op de doorvoer van schrijfbewerkingen. De vermindering van de doorvoer van schrijfbewerkingen, is afhankelijk van de 'paden opgenomen in de index"en de 'consistentieniveau'. Consistente indexering modus is ontworpen om de index up-to-date houden met alle updates en voor het bieden van direct query's.

* **Geen**: Een container met een geen Indexmodus geen index die is gekoppeld heeft. Dit wordt vaak gebruikt als Azure Cosmos-database wordt gebruikt als een sleutel / waarde-opslag en items zijn alleen toegankelijk voor de ID-eigenschap.

  > [!NOTE]
  > Configureren van de indexing-modus als een *geen* heeft een neveneffect van het verwijderen van alle bestaande indexen. U moet deze optie gebruiken als uw patronen voor databasetoegang ID vereisen of self link alleen.

Query-consistentieniveaus worden die vergelijkbaar is met de reguliere leesbewerkingen bijgehouden. Azure Cosmos-database wordt een fout geretourneerd als u de container met een query een *geen* indexering modus. U kunt de query's uitvoeren als scans via de expliciete **x-ms-documentdb-enable-scan** -header in de REST-API of de **EnableScanInQuery** optie vragen via de .NET SDK. Sommige query functies, zoals ORDER BY worden momenteel niet ondersteund met **EnableScanInQuery**, omdat ze een bijbehorende index verplichten.

## <a name="modifying-the-indexing-policy"></a>Wijzigen van het indexeringsbeleid

In Azure Cosmos DB, kunt u het indexeringsbeleid van een container op elk gewenst moment bijwerken. Een wijziging in het indexeringsbeleid op een Azure Cosmos-container kan leiden tot een wijziging in de vorm van de index. Deze wijziging is van invloed op de paden die kunnen worden geïndexeerd, hun precisie en het consistentiemodel van de index zelf. Een wijziging in het indexeringsbeleid effectief vereist een transformatie van de oude index in een nieuwe index.

### <a name="index-transformations"></a>Index transformaties

Alle transformaties van de index worden online uitgevoerd. De items die zijn geïndexeerd per het oude beleid zijn efficiënt per het nieuwe beleid getransformeerd zonder gevolgen voor de beschikbaarheid voor schrijven of de doorvoer die is ingericht voor de container. De consistentie van lezen en schrijven van bewerkingen die worden uitgevoerd met behulp van de REST-API, SDK's, of met behulp van opgeslagen procedures en triggers wordt niet beïnvloed tijdens de transformatie van de index.

Indexeringsbeleid wijzigen is een asynchrone bewerking en de tijd om de bewerking te voltooien, is afhankelijk van het aantal items, ingerichte doorvoer en de grootte van de items. Uw query kan niet alle overeenkomende resultaten retourneren als de query's met behulp van de index die wordt gewijzigd terwijl opnieuw indexeren uitgevoerd wordt, en eventuele fouten/fouten wordt niet geretourneerd door de query's. Wordt uitgevoerd opnieuw indexeren, zijn query's uiteindelijk consistent, ongeacht de indexering modus-configuratie. Nadat de index blijft transformatie is voltooid, om consistente resultaten te bekijken. Dit geldt voor query's dat is uitgegeven door de interfaces, zoals REST-API, SDK's, of opgeslagen procedures en triggers. Index transformatie wordt asynchroon uitgevoerd op de achtergrond, op de replica's met behulp van de ongebruikte bronnen die beschikbaar voor specifieke replica's zijn.

Alle transformaties van index worden gemaakt op locatie. Azure Cosmos DB biedt twee kopieën van de index niet behouden. Daarom is er geen extra schijfruimte vereist of in uw containers wordt verbruikt, terwijl index transformatie wordt uitgevoerd.

Wanneer u het indexeringsbeleid wijzigt, worden de wijzigingen worden toegepast om te verplaatsen van de oude index naar de nieuwe index en zijn voornamelijk gebaseerd op de indexering modus-configuraties. Indexering configuraties spelen een belangrijke rol in vergelijking met andere eigenschappen op, zoals opgenomen/uitgesloten paden, index-typen en precisie.

Als de oude en nieuwe beleidsregels indexeren **consistente** indexeren, Azure Cosmos-database een online-index transformatie uitvoert. U kunt een andere indexering beleidswijziging met consistente indexering modus terwijl de transformatie uitgevoerd wordt niet toepassen. Wanneer u op geen modus indexeren verplaatst, wordt de index onmiddellijk verwijderd. Verplaatsen naar geen is handig als u wilt een transformatie in uitvoering annuleren en opnieuw beginnen met een ander beleid voor indexering.

## <a name="modifying-the-indexing-policy---examples"></a>Wijzigen van het indexeringsbeleid - voorbeelden

Hier volgen de meest voorkomende gebruiksvoorbeelden wanneer u wilt een indexeringsbeleid bijwerken:

* Als u wilt om consistente resultaten tijdens normale werking, maar terugvallen op de **geen** indexering modus tijdens bulkimport voor gegevens.

* Als u beginnen met de indexing-onderdelen op uw huidige Azure-Cosmos-containers wilt. U kunt bijvoorbeeld gebruik georuimtelijke uitvoeren van query's, waarvoor de ruimtelijke index type of ORDER BY / bereik-query's, waarvoor het type tekenreeks bereik index tekenreeks.

* Als u de eigenschappen die wilt moeten worden geïndexeerd en wijzig ze na verloop van tijd om aan te passen aan uw workloads handmatig selecteren.

* Als u de indexering precisie om de queryprestaties te verbeteren of om te beperken van de verbruikte opslag af te stemmen wilt.

## <a name="next-steps"></a>Volgende stappen

Lees meer over indexeren in de volgende artikelen:

* [Overzicht indexeren](index-overview.md)
* [Indextypen](index-types.md)
* [Indexpaden](index-paths.md)
* [Indexeringsbeleid beheren](how-to-manage-indexing-policy.md)

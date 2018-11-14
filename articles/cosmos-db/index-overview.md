---
title: Indexeren in Azure Cosmos DB
description: Begrijp hoe indexering werkt in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: c0525cfba16fb61f8388ae4d6a693be3bb71674c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628991"
---
# <a name="indexing-in-azure-cosmos-db"></a>Indexeren in Azure Cosmos DB

Azure Cosmos DB is een schema-agnostische database en kunt u het herhalen voor uw toepassing snel zonder dat u hoeft te bekommeren om schema's of indexbeheer. Standaard indexeert Azure Cosmos DB automatisch alle items in de container zonder schema of secundaire indexen van ontwikkelaars.

## <a name="items-as-trees"></a>Items als structuren

Projectie van items in een container als JSON-documenten en vertegenwoordigen als structuren, Azure Cosmos DB normaliseert zowel de structuur en de waarden voor items in de uniforme concept van een **dynamisch gecodeerd padstructuur** . In deze weergave wordt elk label in een JSON-document, waaronder de namen van eigenschappen en de bijbehorende waarden een knooppunt van de structuur. De knooppunten van de structuur van de werkelijke waarden en de tussenliggende knooppunten bevatten de schema-informatie. De volgende afbeelding ziet u de structuren die zijn gemaakt voor twee items (1 en 2) in een container:

![Boomstructuur voor twee verschillende items in een Azure Cosmos-container](./media/index-overview/indexing-as-tree.png)

Een pseudo-hoofdknooppunt wordt gemaakt als de werkelijke knooppunten die overeenkomen met de labels in het document onder een bovenliggend element. De geneste gegevensstructuren station de hiërarchie in de structuur. Tussenliggende kunstmatige knooppunten met het label met numerieke waarden (bijvoorbeeld 0, 1,...) worden gebruikt voor de vertegenwoordiging van opsommingen en matrix van indexen.

## <a name="index-paths"></a>Index paden

Azure Cosmos DB-projecten items als JSON-documenten en index als structuren. Vervolgens kunt u aan het beleid voor paden in de structuur afstemmen. U kunt opnemen in of paden uitsluiten van het indexeren. Dit kan bieden verbeterde schrijfprestaties en verlaag de indexopslag voor scenario's waarbij de querypatronen vooruit bekend. Zie voor meer informatie, [Index paden](index-paths.md).

## <a name="indexing-under-the-hood"></a>Indexering: achter de schermen

Azure Cosmos-database is van toepassing automatische indexering tot de gegevens, waarbij elk pad in een boomstructuur worden geïndexeerd, tenzij u configureert om bepaalde paden uitsluiten.

Azure Cosmos-database gebruik omgekeerd index gegevensstructuur voor het opslaan van de gegevens van elk item en om efficiënt weergave voor het uitvoeren van query's mogelijk te maken. De structuur van de index is een document dat is opgesteld met de vereniging van alle van de structuren die afzonderlijke items in de container vertegenwoordigt. De structuur van de index neemt na verloop van tijd toe naarmate er nieuwe items worden toegevoegd of bestaande artikelen zijn bijgewerkt in de container. In tegenstelling tot het indexeren van relationele database, worden Azure Cosmos DB niet opnieuw starten voor het indexeren helemaal nieuwe velden worden ingevoerd, nieuwe items toegevoegd aan de bestaande structuur. 

Elk knooppunt van de structuur van de index is een vermelding met de waarden label en de positie, met de term, de naam en de id's van de items, de boekingen genoemd. De berichten in de accolades (bijvoorbeeld {1,2}) in de omgekeerde index afbeelding overeenkomen met de items zoals Document1 en Document2 met de labelwaarde van het opgegeven. Een belangrijk implicatie van verwerking van de schema-labels en de waarden op uniforme wijze is dat alles is verpakt in een grote index. Een exemplaarwaarde die is nog steeds in de knooppunten wordt niet herhaald, kan het zijn in verschillende rollen voor items met verschillende schema labels, maar het is dezelfde waarde. De volgende afbeelding ziet u omgekeerde indexering voor verschillende items:

![Index indexeren achter de schermen, omgekeerd](./media/index-overview/inverted-index.png)

> [!NOTE]
> De omgekeerde index kan worden weergegeven die vergelijkbaar is met de indexering structuren gebruikt in een zoekmachine in het domein van het ophalen van gegevens. Met deze methode kunt Azure Cosmos DB u zoeken naar de database voor elk item, ongeacht de schemastructuur.

Voor het pad genormaliseerde codeert de index het forward pad helemaal vanaf de hoofdmap op de waarde, samen met de informatie van de waarde. Het pad en de waarde zijn voor verschillende soorten geïndexeerd, zoals het bereik, ruimtelijke typen gecodeerd. De waarde codering is ontworpen om unieke waarde of een samenstelling van een set met paden te geven.

## <a name="querying-with-indexes"></a>Query's uitvoeren met indexen

De omgekeerde index Hiermee kan een query de documenten die aan het querypredicaat snel identificeren. De omgekeerde index is door de waarden voor de schema- en -exemplaar op uniforme wijze in termen van paden te behandelen, ook een structuur. De index en de resultaten kunnen worden geserialiseerd naar een geldig JSON-document en dus geretourneerd als de documenten zelf als ze worden geretourneerd in de boomstructuur. Deze methode kunt recursing boven de resultaten voor extra query's. De volgende afbeelding toont een voorbeeld van het indexeren in een point-query:  

![Voorbeeld van punt](./media/index-overview/index-point-query.png)

Voor een bereikquery is GermanTax een door de gebruiker gedefinieerde functie uitgevoerd als onderdeel van de verwerking van query's. De gebruiker gedefinieerde functie wordt elke geregistreerde, Javascript-functie die de biedt uitgebreide programming logica geïntegreerd in de query. De volgende afbeelding toont een voorbeeld van het indexeren in een bereikquery:

![Voorbeeld van bereik](./media/index-overview/index-range-query.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het indexeren in de volgende artikelen:

- [Indexeringsbeleid](index-policy.md)
- [Index-typen](index-types.md)
- [Index paden](index-paths.md)
- [Indexeringsbeleid beheren](how-to-manage-indexing-policy.md)
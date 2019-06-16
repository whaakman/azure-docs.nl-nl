---
title: Gedistribueerde gegevens in Azure Database voor PostgreSQL – grootschalige (Citus) (preview)
description: Tabellen en shards verdeeld in de servergroep.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 9020ee690d93a1b477471fac4a482a909fca5935
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077333"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Gedistribueerde gegevens in Azure Database voor PostgreSQL – grootschalige (Citus) (preview)

In dit artikel bevat een overzicht van de drie tabeltypen in grootschalige (Citus).
U ziet hoe gedistribueerde tabellen worden opgeslagen als shards en de manier waarop shards worden geplaatst op knooppunten.

## <a name="table-types"></a>Tabeltypen

Er zijn drie typen van tabellen in een grootschalige-servergroep, elk voor verschillende doeleinden gebruikt.

### <a name="type-1-distributed-tables"></a>Type 1: gedistribueerde tabellen

Is het eerste type en de meest voorkomende *gedistribueerde* tabellen. Ze normale tabellen met SQL-instructies worden weergegeven, maar worden horizontaal *gepartitioneerde* voor worker-knooppunten. Dit betekent dat de rijen van de tabel zijn opgeslagen op verschillende knooppunten, in fragment tabellen met de naam *shards*.

Zeer grootschalige niet alleen SQL wordt uitgevoerd, maar DDL-componenten in een cluster, waardoor het wijzigen van het schema van een distributietabel trapsgewijs om bij te werken van de tabel shards voor werknemers.

#### <a name="distribution-column"></a>Distributiekolom

Zeer grootschalige maakt gebruik van algoritmische sharding rijen toewijzen aan shards. De toewijzing wordt gemaakt deterministische wijze op basis van de waarde van de kolom in een tabel met de naam de *distributiekolom.* De Clusterbeheerder moet deze kolom opgeven tijdens het distribueren van een tabel.
Maken van de juiste keuze is belangrijk voor prestaties en functionaliteit.

### <a name="type-2-reference-tables"></a>Type 2: tabellen

Een tabel is een type tabel waarvan de volledige inhoud in een enkele shard zijn geconcentreerd. De shard wordt gerepliceerd op elke worker, waardoor query's op elke worker toegang heeft tot de referentie-informatie lokaal, zonder de overhead van het netwerk voor het aanvragen van rijen uit een ander knooppunt. Tabellen hebben geen distributiekolom omdat niet hoeft te onderscheiden van afzonderlijke shards per rij.

Tabellen zijn meestal klein en worden gebruikt voor het opslaan van gegevens die relevant is voor query's die worden uitgevoerd op elke worker-knooppunt. Bijvoorbeeld, waarden, zoals statussen of productcategorieën geïnventariseerd.

### <a name="type-3-local-tables"></a>Type 3: lokale tabellen van het

Wanneer u grootschalige, is het coördinatorknooppunt dat u verbinding met maakt een reguliere PostgreSQL-database. U kunt gewone tabellen maken op de coördinator en ervoor kiest geen te shard ze.

Een goede kandidaat voor lokale tabellen worden kleine administratieve tabellen die geen deel uitmaken van join-query's. Bijvoorbeeld, een tabel met gebruikers voor toepassing aanmelding en verificatie.

## <a name="shards"></a>Shards

De vorige sectie wordt beschreven hoe gedistribueerde tabellen worden opgeslagen als shards op worker-knooppunten. In dit gedeelte wordt meer in de technische details.

De `pg_dist_shard` metagegevenstabel op de coördinator bevat een rij voor elke shard van elke tabel in het systeem. De rij overeenkomt met een shard-ID met een bereik van gehele getallen in een hash-ruimte (shardminvalue, shardmaxvalue):

```sql
SELECT * from pg_dist_shard;
 logicalrelid  | shardid | shardstorage | shardminvalue | shardmaxvalue 
---------------+---------+--------------+---------------+---------------
 github_events |  102026 | t            | 268435456     | 402653183
 github_events |  102027 | t            | 402653184     | 536870911
 github_events |  102028 | t            | 536870912     | 671088639
 github_events |  102029 | t            | 671088640     | 805306367
 (4 rows)
```

Als het coördinatorknooppunt wil bepalen welke shard bevat een rij met `github_events`, de waarde van de distributiekolom in de rij-hashes en controleert welke shard\'s bereik bevat de hash-waarde. (De bereiken ligt die zijn gedefinieerd, zodat de installatiekopie van de hash-functie de niet-aaneengesloten vereniging is.)

### <a name="shard-placements"></a>Shard-plaatsingen

Stel dat shard 102027 is gekoppeld aan de betreffende rij. De rij worden gelezen of geschreven in een tabel met de naam `github_events_102027` in een van de werknemers. Welke werknemer? Die wordt bepaald volledig door de van metagegevenstabellen en de toewijzing van shard werknemer staat bekend als de shard *plaatsing*.

Het coördinatorknooppunt herschrijft query's in fragmenten die naar de specifieke tabellen, zoals verwijzen `github_events_102027`, en deze fragmenten worden uitgevoerd op de juiste werknemers. Hier volgt een voorbeeld van een query uitvoeren op de achtergrond op het knooppunt waarop de shard-ID 102027 vinden.

```sql
SELECT
    shardid,
    node.nodename,
    node.nodeport
FROM pg_dist_placement placement
JOIN pg_dist_node node
  ON placement.groupid = node.groupid
 AND node.noderole = 'primary'::noderole
WHERE shardid = 102027;
```

    ┌─────────┬───────────┬──────────┐
    │ shardid │ nodename  │ nodeport │
    ├─────────┼───────────┼──────────┤
    │  102027 │ localhost │     5433 │
    └─────────┴───────────┴──────────┘

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [kiezen van een distributiekolom](concepts-hyperscale-choose-distribution-column.md) voor gedistribueerde tabellen

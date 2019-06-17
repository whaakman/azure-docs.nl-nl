---
title: Concepten van de server in Azure Database for PostgreSQL
description: In dit artikel vindt u overwegingen en richtlijnen voor het configureren en beheren van Azure Database voor PostgreSQL-servers.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: d03cfd49887adf1f6a4650e374d3e13eeca735a4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077468"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Plaatsing van de tabel in Azure Database voor PostgreSQL – grootschalige (Citus) (preview)

CO-locatie betekent het opslaan van gerelateerde informatie samen op dezelfde knooppunten. Query's kunnen gaan snel wanneer de nodige gegevens beschikbaar zonder al het netwerkverkeer zijn. Gerelateerde gegevens plaatsen op verschillende knooppunten kunt u query's efficiënt parallel uitgevoerd op elk knooppunt.

## <a name="data-colocation-for-hash-distributed-tables"></a>Plaatsing van gegevens voor hash-gedistribueerde tabellen

Een rij wordt opgeslagen in een shard in zeer grootschalige, als de hash van de waarde in de distributiekolom binnen het bereik van van de shard-hash valt. Shards met het hash-bereik worden altijd op hetzelfde knooppunt geplaatst. Rijen met kolomwaarden evenredige verdeling zijn altijd op hetzelfde knooppunt in tabellen.

![Shards](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Een voorbeeld van CO-locatie

Houd rekening met de volgende tabellen die deel van een multitenant-webanalyse SaaS uitmaken kunnen:

```sql
CREATE TABLE event (
  tenant_id int,
  event_id bigint,
  page_id int,
  payload jsonb,
  primary key (tenant_id, event_id)
);

CREATE TABLE page (
  tenant_id int,
  page_id int,
  path text,
  primary key (tenant_id, page_id)
);
```

Nu willen we beantwoorden van query's die kunnen worden uitgegeven door een klantgerichte-dashboard, zoals: "Het aantal bezoeken retourneren in de afgelopen week voor alle pagina's die beginnen met ' / blog' in de tenant zes."

Als onze gegevens zich in een optie voor implementatie met één server, kan er eenvoudig onze query met behulp van de uitgebreide set relationele bewerkingen die worden aangeboden door SQL express:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Zolang de [werkset](https://en.wikipedia.org/wiki/Working_set) voor deze query in het geheugen past, een tabel met één server is een geschikte oplossing. Maar laten we eens de mogelijkheden van het gegevensmodel met de Implementatieoptie grootschalige schalen.

### <a name="distributing-tables-by-id"></a>Distribueren van tabellen op ID

Single-server-query's starten wanneer het aantal tenants en de gegevens die zijn opgeslagen voor elke tenant groeit vertragen. De werkset stopt aanpassen in het geheugen en CPU een knelpunt wordt.

In dit geval kunnen we shard de gegevens over veel knooppunten met behulp van grote schaal. De keuze voor de eerste en belangrijkste die we hoeven te maken wanneer sharding de distributiekolom is. Laten we beginnen met een keuze naïve van het gebruik van `event_id` voor de tabel event en `page_id` voor de `page` tabel:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Wanneer gegevens is verspreid over verschillende werknemers, kan er een join niet uitvoeren als we in op een enkele PostgreSQL-knooppunt. In plaats daarvan moeten we twee query's uitgeven:

```sql
-- (Q1) get the relevant page_ids
SELECT page_id FROM page WHERE path LIKE '/blog%' AND tenant_id = 6;

-- (Q2) get the counts
SELECT page_id, count(*) AS count
FROM event
WHERE page_id IN (/*…page IDs from first query…*/)
  AND tenant_id = 6
  AND (payload->>'time')::date >= now() - interval '1 week'
GROUP BY page_id ORDER BY count DESC LIMIT 10;
```

Daarna moeten de resultaten van de twee stappen kunnen worden gecombineerd met de toepassing.

Uitvoeren van de query's, moet de gegevens in shards verspreid over knooppunten raadplegen.

![inefficiënt query 's](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

In dit geval maakt u de verdeling van de gegevens substantieel nadelen:

-   Overhead bij het opvragen van elke shard, uitvoeren van meerdere query 's
-   Extra overhead van Q1 veel rijen retourneren naar de client
-   Q2 groot
-   De noodzaak om query's schrijven in meerdere stappen vereist wijzigingen in de toepassing

Omdat de gegevens worden verdeeld over, kan de query's kunnen worden geparallelliseerd. Het is echter alleen nuttig als de hoeveelheid werk die de query heeft aanzienlijk groter is dan de overhead van het uitvoeren van query's veel shards tegelijk.

### <a name="distributing-tables-by-tenant"></a>Distribueren van tabellen door de tenant

In zeer grootschalige, rijen met de dezelfde waarde in de kolom distributie gegarandeerd kan beschikken op hetzelfde knooppunt. Beginnen, kunt u maken de tabellen met `tenant_id` als de distributiekolom.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Zeer grootschalige kan nu antwoord van de oorspronkelijke query één server zonder aanpassingen (Q1):

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Vanwege het filter en join op tenant_id weet grootschalige dat de query met behulp van de set geplaatst shards die de gegevens voor de specifieke tenant bevatten kan worden beantwoord. Een enkele PostgreSQL-knooppunt kunt antwoord van de query in één stap.

![betere query](media/concepts-hyperscale-colocation/colocation-better-query.png)

In sommige gevallen moeten de query's en tabelschema voor het opnemen van de tenant-ID in unieke beperkingen en deelnemen aan de voorwaarden worden gewijzigd. Dit is echter meestal een eenvoudige wijziging.

## <a name="next-steps"></a>Volgende stappen

- Zie hoe de gegevens van de tenant wordt geplaatst de [multitenant-zelfstudie](tutorial-design-database-hyperscale-multi-tenant.md)

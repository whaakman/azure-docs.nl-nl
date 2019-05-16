---
title: Azure Database voor PostgreSQL – grootschalige (Citus) (preview) Quick Start
description: Snelstartgids voor het maken en query gedistribueerd tabellen op de Azure Database voor PostgreSQL Hyperscale (Citus) (preview).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/14/2019
ms.openlocfilehash: efc3801ab03f739761a41bec754f975fe43dcd8e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65757515"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-preview-in-the-azure-portal"></a>Quickstart: Maak een Azure Database voor PostgreSQL - grootschalige (Citus) (preview) in Azure portal

Azure Database for PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. Deze snelstartgids leest u over het maken van een Azure Database voor PostgreSQL - grootschalige (Citus) (preview) server-groep met behulp van de Azure portal. U gedistribueerde gegevens verkennen: sharding van tabellen voor de knooppunten, het opnemen van voorbeeldgegevens en het uitvoeren van query's die worden uitgevoerd op meerdere knooppunten.

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="create-and-distribute-tables"></a>Maken en distribueren van tabellen

Eenmaal verbinding hebben met het coördinatorknooppunt voor grootschalige met behulp van psql, kunt u enkele eenvoudige taken kunt voltooien.

Servers er zijn drie typen tabellen in grootschalige:

- Gedistribueerde of shard tabellen (verspreid om te schalen voor prestaties en parallellisering)
- Verwijzing naar tabellen (meerdere exemplaren worden onderhouden)
- Lokale tabellen (vaak gebruikt voor interne admin tabellen)

In deze snelstartgids wordt we voornamelijk gericht op gedistribueerde tabellen en bekend mee aan.

Het gegevensmodel gaan we werken met is eenvoudig: gebruiker en gebeurtenis gegevens vanuit GitHub. Gebeurtenissen zijn het maken van de fork, git doorgevoerd met betrekking tot een organisatie, en meer.

Nadat u verbinding hebt gemaakt via psql, gaan we de tabellen te maken. In de psql-console uitvoeren:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

De `payload` veld `github_events` JSONB gegevenstype heeft. JSONB is het JSON-gegevenstype in binaire vorm in Postgres. Het gegevenstype kunt u eenvoudig is om op te slaan een flexibel schema in één kolom.

Postgres kunt maken een `GIN` index voor dit type, dat elke sleutel en waarde binnen het geïndexeerd. Met een index, wordt deze snelle en eenvoudige query uitvoeren op de nettolading met verschillende voorwaarden. Laten we doorgaan en maakt u een aantal indexen voordat we onze gegevens worden geladen. In de psql:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Vervolgens gaan we deze tabellen Postgres ondernemen voor het coördinatorknooppunt en grootschalige vertellen aan de shard ze via de werknemers. Om dit te doen we voeren uit een query voor elke tabel op te geven de shard-sleutel op. In het huidige voorbeeld wordt de gebeurtenissen en de gebruikers-tabel op shard `user_id`:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

We zijn klaar om gegevens te laden. In de psql nog steeds, shell uit om de bestanden te downloaden:

```sql
\! curl -O https://examples.citusdata.com/users.csv
\! curl -O https://examples.citusdata.com/events.csv
```

Vervolgens laadt u de gegevens uit de bestanden in de gedistribueerde tabellen:

```sql
\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```

## <a name="run-queries"></a>Query's uitvoeren

Nu is het tijd voor het plezier onderdeel, aantal query's worden uitgevoerd. Laten we beginnen met een eenvoudige `count (*)` om te zien hoeveel gegevens geladen:

```sql
SELECT count(*) from github_events;
```

Die mooi gewerkt. We u keert u terug naar deze sortering van aggregatie in een bits, maar u kunt nu gaan we kijken naar enkele andere query's. Binnen de JSONB `payload` kolom er is een goede deel van de gegevens, maar dit varieert afhankelijk van het gebeurtenistype. `PushEvent` gebeurtenissen bevatten een grootte die het aantal afzonderlijke doorgevoerd voor de push-bewerking bevat. We kunnen deze gebruiken voor het vinden van het totale aantal doorvoeringen per uur:

```sql
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

Tot nu toe de query's hebben uitgevoerd voor de github\_gebeurtenissen uitsluitend, maar we kunnen deze informatie combineren met github\_gebruikers. Sinds we shard zowel gebruikers- en gebeurtenissen op dezelfde id (`user_id`), worden de rijen van beide tabellen met overeenkomende gebruikers-id's [geplaatst](https://docs.citusdata.com/en/stable/sharding/data_modeling.html#colocation) op dezelfde database-knooppunten en kan eenvoudig worden samengevoegd.

Als we deelnemen aan op `user_id`, grootschalige kunt beneden wordt geduwd de join-uitvoering in shards voor de uitvoering van parallel op worker-knooppunten. We willen weten, bijvoorbeeld de gebruikers die het grootste aantal opslagplaatsen gemaakt:

```sql
SELECT login, count(*)
FROM github_events ge
JOIN github_users gu
ON ge.user_id = gu.user_id
WHERE event_type = 'CreateEvent' AND
      payload @> '{"ref_type": "repository"}'
GROUP BY login
ORDER BY count(*) DESC;
```

## <a name="clean-up-resources"></a>Resources opschonen

In de voorgaande stappen hebt u Azure-resources in een servergroep gemaakt. Als u niet verwacht deze resources in de toekomst nodig hebt dat, verwijdert u de server-groep. Druk op de **verwijderen** knop in de **overzicht** pagina voor de servergroep. Wanneer u hierom wordt gevraagd op een pop-pagina, Controleer de naam van de server-groep en klikt u op de laatste **verwijderen** knop.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd over het inrichten van een grootschalige (Citus) server-groep. U verbonden met psql, een schema gemaakt en gedistribueerde gegevens.

Vervolgens gaat u als volgt een zelfstudie voor het bouwen van schaalbare toepassingen met meerdere tenants.
> [!div class="nextstepaction"]
> [Een Multitenant-Database ontwerpen](https://aka.ms/hyperscale-tutorial-multi-tenant)

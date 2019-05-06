---
title: Azure Database voor PostgreSQL – grootschalige (Citus) (preview) Quick Start
description: Snelstartgids voor het maken en query gedistribueerd tabellen op de Azure Database voor PostgreSQL Hyperscale (Citus) (preview).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: 30de4da43569abf4d7bd668fd0fa481ecac23f4d
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65081099"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-preview-in-the-azure-portal"></a>Quickstart: Maak een Azure Database voor PostgreSQL - grootschalige (Citus) (preview) in Azure portal

Azure Database for PostgreSQL is een beheerde service waarmee u PostgreSQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. Deze snelstartgids leest u over het maken van een Azure Database voor PostgreSQL - grootschalige (Citus) (preview) server-groep met behulp van de Azure portal. U gedistribueerde gegevens verkennen: sharding van tabellen voor de knooppunten, het opnemen van voorbeeldgegevens en het uitvoeren van query's die worden uitgevoerd op meerdere knooppunten.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Een Azure Database voor PostgreSQL-server maken

Volg deze stappen voor het maken van een Azure Database voor PostgreSQL-server:
1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor PostgreSQL** op de pagina **Databases**.
3. Voor de Implementatieoptie, klikt u op de **maken** knop onder **servergroep grootschalige (Citus) - voorbeeld.**
4. Vul het formulier voor de gegevens van de nieuwe server als volgt in:
   - Resourcegroep: klik op de **nieuw** koppeling onder het tekstvak voor dit veld. Voer een naam zoals **myresourcegroup**.
   - Naam van servergroep: Voer een unieke naam voor de nieuwe server-groep, die ook worden gebruikt voor een subdomein van de server.
   - Gebruikersnaam van beheerder: Voer een unieke gebruikersnaam, wordt later opnieuw verbinding maken met de database worden gebruikt.
   - Wachtwoord: moet ten minste acht tekens lang zijn en moet tekens bevatten uit drie van de volgende categorieën: Nederlandse hoofdletters, Nederlandse kleine letters, cijfers (0-9) en niet-alfanumerieke tekens (!, $, #, %, etc.)
   - Locatie: Gebruik de locatie die zich het dichtst bij uw gebruikers zodat ze de snelst mogelijke toegang tot de gegevens.

   > [!IMPORTANT]
   > De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.

5. Klik op **configureren servergroep**. Laat de instellingen in die sectie ongewijzigd en klik op **opslaan**.
6. Klik op **revisie + maken** en vervolgens **maken** voor het inrichten van de server. De inrichting duurt een paar minuten.
7. De pagina wordt omgeleid voor het controleren van de implementatie. Wanneer de live status verandert van **uw implementatie wordt uitgevoerd** naar **uw implementatie is voltooid**, klikt u op de **uitvoer** menu-item aan de linkerkant van de pagina.
8. De uitvoer-pagina bevat een hostnaam coördinator met een knop naast het om de waarde naar het Klembord kopiëren. Noteer deze informatie voor later gebruik.

## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

De Azure Database voor PostgreSQL – grootschalige (Citus) (preview)-service wordt gebruikt een firewall op serverniveau. Standaard de firewall voorkomt dat externe toepassingen en hulpprogramma's verbinding maken met het coördinatorknooppunt en alle databases op. Er moet een regel voor het openen van de firewall voor een specifiek IP-adresbereik toevoegen.

1. Uit de **uitvoer** sectie waarnaar u de hostnaam van het knooppunt coordinator eerder hebt gekopieerd, klik op terug naar de **overzicht** menu-item.

2. De naam van de implementatiegroep vergroten/verkleinen wordt voorafgegaan door 'AG-'. Deze vinden in de lijst met resources en klik erop.

3. Klik op **Firewall** onder **Security** in het linkermenu.

4. Klik op de koppeling **+ firewallregel toevoegen voor de huidige IP-adres van client**. Ten slotte klikt u op de **opslaan** knop.

5. Klik op **Opslaan**.

   > [!NOTE]
   > De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 5432 openstelt.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Verbinding maken met de database met behulp van psql in Cloud Shell

U gaat nu het opdrachtregelprogramma [psql](https://www.postgresql.org/docs/current/app-psql.html) gebruiken om verbinding te maken met de Azure Database for PostgreSQL-server.
1. Open Azure Cloud Shell via het terminalpictogram in het navigatiedeelvenster bovenaan.

   ![Azure Database voor PostgreSQL - Azure Cloud Shell-terminalpictogram](./media/quickstart-create-hyperscale-portal/psql-cloud-shell.png)

2. Azure Cloud Shell wordt geopend in uw browser zodat u bash-opdrachten kunt invoeren.

   ![Azure-Database voor PostgreSQL - Azure Shell-bash-prompt](./media/quickstart-create-hyperscale-portal/psql-bash.png)

3. In het Cloud Shell-prompt maakt u verbinding met uw Azure Database voor PostgreSQL-server met de psql-opdrachten. De volgende indeling wordt gebruikt om verbinding te maken met een Azure Database voor PostgreSQL-server via het [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html)-hulpprogramma:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Bijvoorbeeld de volgende opdracht maakt verbinding met de standaarddatabase **citus** op uw PostgreSQL-server **mydemoserver.postgres.database.azure.com** met behulp van toegangsreferenties. Voer het wachtwoord van de serverbeheerder in wanneer dat wordt gevraagd.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="create-and-distribute-tables"></a>Maken en distribueren van tabellen

Eenmaal verbinding hebben met het coördinatorknooppunt voor grootschalige met behulp van psql, kunt u enkele eenvoudige taken kunt voltooien.

Servers er zijn drie typen tabellen in grootschalige:

- Gedistribueerde of shard tabellen (verspreid om te schalen voor prestaties en parallellisering)
- Verwijzing naar tabellen (meerdere exemplaren worden onderhouden)
- Lokale tabellen (vaak gebruikt voor interne admin tabellen)

In deze Quick Start gaat we voornamelijk gericht op gedistribueerde tabellen en bekend mee aan.

Het gegevensmodel gaan we werken met is eenvoudig: gebruiker en gebeurtenis gegevens vanuit GitHub. Gebeurtenissen zijn het maken van de fork, git doorgevoerd met betrekking tot een organisatie, en meer.

Nadat u verbinding hebt via psql laten we onze tabellen maken. In de psql-console uitvoeren:

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

De `payload` veld `github_events` JSONB gegevenstype heeft. JSONB is het JSON-gegevenstype in binaire vorm in Postgres. Hiermee kunt u eenvoudig voor het opslaan van een flexibel schema in één kolom.

Postgres kunt maken een `GIN` index op dit type die elke sleutel en waarde binnen het geïndexeerd. Met een index, wordt deze snelle en eenvoudige query uitvoeren op de nettolading met verschillende voorwaarden. Laten we doorgaan en maakt u een aantal indexen voordat we onze gegevens worden geladen. In de psql:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Vervolgens gaan we deze tabellen Postgres ondernemen voor het coördinatorknooppunt en grootschalige vertellen aan de shard ze via de werknemers. Om dit te doen we voeren uit een query voor elke tabel op te geven de shard-sleutel op. In het huidige voorbeeld wordt de gebeurtenissen en de gebruikers-tabel op shard `user_id`:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

We zijn klaar om gegevens te laden. Het van de twee voorbeeldbestanden downloaden [users.csv](https://examples.citusdata.com/users.csv) en [gebeurtenissen.csv](https://examples.citusdata.com/events.csv). Nadat de bestanden downloadt, verbinding maken met de database met behulp van psql, zorg ervoor dat u de psql uitvoeren vanuit de map met de bestanden die u hebt gedownload. De gegevens laden met de `\copy` opdracht:

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

Tot nu toe de query's hebben uitgevoerd voor de github\_gebeurtenissen uitsluitend, maar we kunnen deze informatie combineren met github\_gebruikers. Sinds we shard zowel gebruikers- en gebeurtenissen op dezelfde id (`user_id`), worden de rijen van beide tabellen met overeenkomende gebruikers-id's [geplaatst](http://docs.citusdata.com/en/stable/sharding/data_modeling.html#colocation) op dezelfde database-knooppunten en kan eenvoudig worden samengevoegd.

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

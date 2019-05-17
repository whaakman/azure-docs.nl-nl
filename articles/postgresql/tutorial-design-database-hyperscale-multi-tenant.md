---
title: Een multitenant-database ontwerpen met Azure Database voor PostgreSQL – grootschalige (Citus) (preview) zelfstudie
description: Deze zelfstudie laat zien hoe u kunt maken, vullen en het opvragen van gedistribueerde tabellen op de Azure Database voor PostgreSQL Hyperscale (Citus) (preview).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: 73d7aebf3dbff59320e0ef92cbd54811503c71b4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792273"
---
# <a name="tutorial-design-a-multi-tenant-database-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Zelfstudie: een multitenant-database ontwerpen met behulp van Azure Database voor PostgreSQL – grootschalige (Citus) (preview)

In deze zelfstudie gebruikt u Azure Database voor PostgreSQL - grootschalige (Citus) (preview) voor meer informatie over het:

> [!div class="checklist"]
> * Maak een servergroep grootschalige (Citus)
> * Psql gebruiken om een schema te maken
> * Shard-tabellen op knooppunten
> * Voorbeeldgegevens opnemen
> * Gegevens van de query-tenant
> * Delen van gegevens tussen tenants
> * Het schema per tenant aanpassen

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="use-psql-utility-to-create-a-schema"></a>Psql gebruiken om een schema te maken

Eenmaal verbinding hebben met de Azure Database voor PostgreSQL - grootschalige (Citus) (preview) met behulp van psql, u kunt enkele eenvoudige taken uit te. In deze zelfstudie wordt beschreven hoe u een web-app waarmee adverteerders om bij te houden van hun campagnes.

Meerdere bedrijven kan de app gebruiken, laten we een tabel voor het opslaan van bedrijven en andere voor hun campagnes maken. Voer deze opdrachten uit in de psql-console:

```sql
CREATE TABLE companies (
  id bigserial PRIMARY KEY,
  name text NOT NULL,
  image_url text,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL
);

CREATE TABLE campaigns (
  id bigserial,
  company_id bigint REFERENCES companies (id),
  name text NOT NULL,
  cost_model text NOT NULL,
  state text NOT NULL,
  monthly_budget bigint,
  blacklisted_site_urls text[],
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id)
);
```

Elke campagne wordt betaalt voor het uitvoeren van advertenties. Een tabel voor advertenties, door het uitvoeren van de volgende code in psql na de bovenstaande code toevoegen:

```sql
CREATE TABLE ads (
  id bigserial,
  company_id bigint,
  campaign_id bigint,
  name text NOT NULL,
  image_url text,
  target_url text,
  impressions_count bigint DEFAULT 0,
  clicks_count bigint DEFAULT 0,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, campaign_id)
    REFERENCES campaigns (company_id, id)
);
```

Ten slotte, houden we statistieken die aangeven klikken en hits voor elke ad:

```sql
CREATE TABLE clicks (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  clicked_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_click_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);

CREATE TABLE impressions (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  seen_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_impression_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);
```

U kunt de zojuist gemaakte tabellen in de lijst met tabellen nu beschikbaar in de psql zien door te voeren:

```postgres
\dt
```

Toepassingen met meerdere tenants kunnen afdwingen uniekheid per tenant, die daarom alle primaire en refererende sleutels bevatten de id van het bedrijf.

## <a name="shard-tables-across-nodes"></a>Shard-tabellen op knooppunten

Een grootschalige implementatie slaat tabelrijen op verschillende knooppunten op basis van de waarde van een door de gebruiker opgegeven kolom. Deze "distributiekolom"-markeringen die tenant is eigenaar van welke rijen.

Stel de distributiekolom moet bedrijf\_-id, de tenant-id. Voer in de psql, deze functies:

```sql
SELECT create_distributed_table('companies',   'id');
SELECT create_distributed_table('campaigns',   'company_id');
SELECT create_distributed_table('ads',         'company_id');
SELECT create_distributed_table('clicks',      'company_id');
SELECT create_distributed_table('impressions', 'company_id');
```

## <a name="ingest-sample-data"></a>Voorbeeldgegevens opnemen

Buitenkant van psql nu downloaden voorbeelden van gegevenssets in de normale opdrachtregel:

```bash
for dataset in companies campaigns ads clicks impressions geo_ips; do
  curl -O https://examples.citusdata.com/mt_ref_arch/${dataset}.csv
done
```

Terug in psql, bulksgewijs laden van de gegevens. Zorg dat voor het uitvoeren van psql in dezelfde map waar u de bestanden gedownload.

```sql
\copy companies from 'companies.csv' with csv
\copy campaigns from 'campaigns.csv' with csv
\copy ads from 'ads.csv' with csv
\copy clicks from 'clicks.csv' with csv
\copy impressions from 'impressions.csv' with csv
```

Deze gegevens worden nu worden verdeeld over de worker-knooppunten.

## <a name="query-tenant-data"></a>Gegevens van de query-tenant

Wanneer de toepassing gegevens voor één tenant aanvraagt, kan de database de query uitvoeren op een enkele worker-knooppunt. Query's voor één tenant filteren op een enkele tenant-ID. Bijvoorbeeld, de volgende filters query `company_id = 5` voor advertenties en indruk. Voer het uit in de psql om de resultaten te bekijken.

```sql
SELECT a.campaign_id,
       RANK() OVER (
         PARTITION BY a.campaign_id
         ORDER BY a.campaign_id, count(*) desc
       ), count(*) as n_impressions, a.id
  FROM ads as a
  JOIN impressions as i
    ON i.company_id = a.company_id
   AND i.ad_id      = a.id
 WHERE a.company_id = 5
GROUP BY a.campaign_id, a.id
ORDER BY a.campaign_id, n_impressions desc;
```

## <a name="share-data-between-tenants"></a>Delen van gegevens tussen tenants

Tot nu alle tabellen zijn gedistribueerd door `company_id`, maar sommige gegevens op natuurlijke wijze "behoort niet' tot een tenant in het bijzonder, en kan worden gedeeld. Alle bedrijven in het voorbeeld van de ad-platform wilt bijvoorbeeld geografische informatie voor de doelgroep op basis van IP-adressen.

Maak een tabel voor het opslaan van gedeelde geografische informatie. Voer de volgende opdrachten in psql:

```sql
CREATE TABLE geo_ips (
  addrs cidr NOT NULL PRIMARY KEY,
  latlon point NOT NULL
    CHECK (-90  <= latlon[0] AND latlon[0] <= 90 AND
           -180 <= latlon[1] AND latlon[1] <= 180)
);
CREATE INDEX ON geo_ips USING gist (addrs inet_ops);
```

Vervolgens maken `geo_ips` een verwijzingstabel' ' voor het opslaan van een kopie van de tabel op elke worker-knooppunt.

```sql
SELECT create_reference_table('geo_ips');
```

Laden met voorbeeldgegevens. Vergeet niet om uit te voeren met deze opdracht in psql uit in de map waar u de gegevensset hebt gedownload.

```sql
\copy geo_ips from 'geo_ips.csv' with csv
```

Lid worden van de tabel klikken met geografisch\_IP-adressen is efficiënt op alle knooppunten.
Hier volgt een join te vinden van de locaties waar iedereen die op ad geklikt
290. Probeer de query wordt uitgevoerd in psql.

```sql
SELECT c.id, clicked_at, latlon
  FROM geo_ips, clicks c
 WHERE addrs >> c.user_ip
   AND c.company_id = 5
   AND c.ad_id = 290;
```

## <a name="customize-the-schema-per-tenant"></a>Het schema per tenant aanpassen

Elke tenant wellicht speciale gegevens niet nodig voor andere gebruikers op te slaan. Echter, delen alle tenants een gemeenschappelijke infrastructuur met een identieke databaseschema. Waar kan de extra gegevens gaan?

Een truc is het gebruik van een type met een kolom, zoals PostgreSQL JSONB.  Onze schema heeft een veld JSONB in `clicks` met de naam `user_data`.
Een onderneming (bijvoorbeeld 5), kunt u de kolom gebruiken om te controleren of de gebruiker zich op een mobiel apparaat.

Hier volgt een query te vinden die meer klikt: mobiele of traditionele bezoekers.

```sql
SELECT
  user_data->>'is_mobile' AS is_mobile,
  count(*) AS count
FROM clicks
WHERE company_id = 5
GROUP BY user_data->>'is_mobile'
ORDER BY count DESC;
```

We kunnen deze query voor een enkel bedrijf optimaliseren door het maken van een [gedeeltelijke index](https://www.postgresql.org/docs/current/static/indexes-partial.html).

```sql
CREATE INDEX click_user_data_is_mobile
ON clicks ((user_data->>'is_mobile'))
WHERE company_id = 5;
```

Meer over het algemeen kunnen we maken een [GIN indexen](https://www.postgresql.org/docs/current/static/gin-intro.html) op elke sleutel en waarde in de kolom.

```sql
CREATE INDEX click_user_data
ON clicks USING gin (user_data);

-- this speeds up queries like, "which clicks have
-- the is_mobile key present in user_data?"

SELECT id
  FROM clicks
 WHERE user_data ? 'is_mobile'
   AND company_id = 5;
```

## <a name="clean-up-resources"></a>Resources opschonen

In de voorgaande stappen hebt u Azure-resources in een servergroep gemaakt. Als u niet verwacht deze resources in de toekomst nodig hebt dat, verwijdert u de server-groep. Druk op de *verwijderen* knop in de *overzicht* pagina voor de servergroep. Wanneer u hierom wordt gevraagd op een pop-pagina, Controleer de naam van de server-groep en klikt u op de laatste *verwijderen* knop.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het inrichten van een grootschalige (Citus) server-groep. U verbonden met psql, een schema gemaakt en gedistribueerde gegevens. Hebt u geleerd om gegevens te doorzoeken binnen en tussen tenants en het aanpassen van het schema per tenant.

Hierna leert over de concepten van grootschalige.
> [!div class="nextstepaction"]
> [Zeer grootschalige knooppunttypen](https://aka.ms/hyperscale-concepts)

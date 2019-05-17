---
title: Een realtime dashboard met Azure Database for PostgreSQL – grootschalige (Citus) (preview) ontwerpen zelfstudie
description: Deze zelfstudie laat zien hoe u kunt maken, vullen en het opvragen van gedistribueerde tabellen op de Azure Database voor PostgreSQL Hyperscale (Citus) (preview).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: a5e4b2073a29785ee851b2733c12d6331afe59d8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791303"
---
# <a name="tutorial-design-a-real-time-analytics-dashboard-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Zelfstudie: Een realtime analytics-dashboard ontwerpen met behulp van Azure Database voor PostgreSQL – grootschalige (Citus) (preview)

In deze zelfstudie gebruikt u Azure Database voor PostgreSQL - grootschalige (Citus) (preview) voor meer informatie over het:

> [!div class="checklist"]
> * Maak een servergroep grootschalige (Citus)
> * Psql gebruiken om een schema te maken
> * Shard-tabellen op knooppunten
> * Voorbeeldgegevens genereren
> * Updatepakketten uitvoeren
> * Onbewerkte en samengevoegde gegevens op te vragen
> * Gegevens laten verlopen

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="use-psql-utility-to-create-a-schema"></a>Psql gebruiken om een schema te maken

Eenmaal verbinding hebben met de Azure Database voor PostgreSQL - grootschalige (Citus) (preview) met behulp van psql, u kunt enkele eenvoudige taken uit te. In deze zelfstudie helpt u bij het ophalen van verkeersgegevens van web analytics, de gegevens voor realtime-dashboards op basis van die gegevens vervolgens getotaliseerd.

Laten we een tabel die wordt verbruikt al onze verkeer onbewerkte gegevens maken. Voer de volgende opdrachten in de terminal psql:

```sql
CREATE TABLE http_request (
  site_id INT,
  ingest_time TIMESTAMPTZ DEFAULT now(),

  url TEXT,
  request_country TEXT,
  ip_address TEXT,

  status_code INT,
  response_time_msec INT
);
```

We gaan maken van een tabel die onze per minuut statistische functies kan bevatten en een tabel die de positie van onze laatste updatepakket onderhoudt. Voer de volgende opdrachten in psql ook:

```sql
CREATE TABLE http_request_1min (
  site_id INT,
  ingest_time TIMESTAMPTZ, -- which minute this row represents

  error_count INT,
  success_count INT,
  request_count INT,
  average_response_time_msec INT,
  CHECK (request_count = error_count + success_count),
  CHECK (ingest_time = date_trunc('minute', ingest_time))
);

CREATE INDEX http_request_1min_idx ON http_request_1min (site_id, ingest_time);

CREATE TABLE latest_rollup (
  minute timestamptz PRIMARY KEY,

  CHECK (minute = date_trunc('minute', minute))
);
```

Hier ziet u de zojuist gemaakte tabellen in de lijst met tabellen nu met de volgende psql-opdracht:

```postgres
\dt
```

## <a name="shard-tables-across-nodes"></a>Shard-tabellen op knooppunten

Een grootschalige implementatie slaat tabelrijen op verschillende knooppunten op basis van de waarde van een door de gebruiker opgegeven kolom. Deze distributiekolom"" markeert hoe gegevens shard wordt over knooppunten.

Stel de distributiekolom moet site\_-id, de shard-sleutel. Voer in de psql, deze functies:

  ```sql
SELECT create_distributed_table('http_request',      'site_id');
SELECT create_distributed_table('http_request_1min', 'site_id');
```

## <a name="generate-sample-data"></a>Voorbeeldgegevens genereren

Nu moet onze servergroep nu enkele gegevens opnemen. Er kan worden uitgevoerd de volgende lokaal uit onze `psql` verbinding met gegevens continu invoegen.

```sql
DO $$
  BEGIN LOOP
    INSERT INTO http_request (
      site_id, ingest_time, url, request_country,
      ip_address, status_code, response_time_msec
    ) VALUES (
      trunc(random()*32), clock_timestamp(),
      concat('http://example.com/', md5(random()::text)),
      ('{China,India,USA,Indonesia}'::text[])[ceil(random()*4)],
      concat(
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2)
      )::inet,
      ('{200,404}'::int[])[ceil(random()*2)],
      5+trunc(random()*150)
    );
    COMMIT;
    PERFORM pg_sleep(random() * 0.25);
  END LOOP;
END $$;
```

De query voegt u ongeveer acht rijen per seconde. De rijen worden opgeslagen op verschillende worker-knooppunten wijze die door de distributiekolom `site_id`.

   > [!NOTE]
   > Laat u de generatie query uitgevoerd en opent u een tweede psql-verbinding voor de resterende opdrachten in deze zelfstudie.
   >

## <a name="query"></a>Query

De grootschalige webhosting kan meerdere knooppunten voor het verwerken van query's parallel voor snelheid. Bijvoorbeeld, de database aggregaties zoals som en aantal op worker-knooppunten berekend en combineert de resultaten in een definitieve antwoord.

Hier volgt een query voor het tellen van webaanvragen per minuut, samen met enkele statistieken.
Voer het uit in de psql en bekijk de resultaten.

```sql
SELECT
  site_id,
  date_trunc('minute', ingest_time) as minute,
  COUNT(1) AS request_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
  SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
FROM http_request
WHERE date_trunc('minute', ingest_time) > now() - '5 minutes'::interval
GROUP BY site_id, minute
ORDER BY minute ASC;
```

## <a name="rolling-up-data"></a>Rolling van gegevens

De vorige query werkt goed in de eerste fasen, maar de prestaties verslechteren als uw gegevens verandert. Zelfs met gedistribueerde verwerking is het sneller om te berekenen vooraf de gegevens dan als u wilt het herhaaldelijk te berekenen.

We kunnen dat het dashboard blijft snel door regelmatig de onbewerkte gegevens in een samengevoegde tabel. U kunt experimenteren met een duur van de aggregatie. Wordt een aggregatietabel per minuut gebruikt, maar u kunt gegevens opsplitsen in 5, 15 of 60 minuten in plaats daarvan.

Als u wilt deze updatetotalisatie eenvoudig kunt uitvoeren, gaan we deze in een functie plpgsql plaatsen. Voer deze opdrachten uit in de psql maken de `rollup_http_request` functie.

```sql
-- initialize to a time long ago
INSERT INTO latest_rollup VALUES ('10-10-1901');

-- function to do the rollup
CREATE OR REPLACE FUNCTION rollup_http_request() RETURNS void AS $$
DECLARE
  curr_rollup_time timestamptz := date_trunc('minute', now());
  last_rollup_time timestamptz := minute from latest_rollup;
BEGIN
  INSERT INTO http_request_1min (
    site_id, ingest_time, request_count,
    success_count, error_count, average_response_time_msec
  ) SELECT
    site_id,
    date_trunc('minute', ingest_time),
    COUNT(1) as request_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
    SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
  FROM http_request
  -- roll up only data new since last_rollup_time
  WHERE date_trunc('minute', ingest_time) <@
          tstzrange(last_rollup_time, curr_rollup_time, '(]')
  GROUP BY 1, 2;

  -- update the value in latest_rollup so that next time we run the
  -- rollup it will operate on data newer than curr_rollup_time
  UPDATE latest_rollup SET minute = curr_rollup_time;
END;
$$ LANGUAGE plpgsql;
```

Met de functie in plaats het ongedaan maken van de gegevens te worden uitgevoerd:

```sql
SELECT rollup_http_request();
```

En met onze gegevens in een vooraf geaggregeerde vorm kunnen we de Samenvouwtabel om op te halen van hetzelfde rapport net als eerst op te vragen. Voer de volgende query uit:

```sql
SELECT site_id, ingest_time as minute, request_count,
       success_count, error_count, average_response_time_msec
  FROM http_request_1min
 WHERE ingest_time > date_trunc('minute', now()) - '5 minutes'::interval;
 ```

## <a name="expiring-old-data"></a>Oude gegevens verlopen

De updatepakketten sneller query's maken, maar we moeten nog steeds oude gegevens om te voorkomen dat niet-gebonden opslagkosten laten verlopen. Bepaal hoe lang u wilt dat blijven gegevens voor elke granulariteit en standard-query's gebruiken om verlopen gegevens te verwijderen. We besloten te houden van onbewerkte gegevens voor één dag en per minuut aggregaties gedurende één maand in het volgende voorbeeld:

```sql
DELETE FROM http_request WHERE ingest_time < now() - interval '1 day';
DELETE FROM http_request_1min WHERE ingest_time < now() - interval '1 month';
```

U kunt in productie, inpakken van deze query's in een functie en deze aanroepen per minuut in een cron-taak.

## <a name="clean-up-resources"></a>Resources opschonen

In de voorgaande stappen hebt u Azure-resources in een servergroep gemaakt. Als u niet verwacht deze resources in de toekomst nodig hebt dat, verwijdert u de server-groep. Druk op de *verwijderen* knop in de *overzicht* pagina voor de servergroep. Wanneer u hierom wordt gevraagd op een pop-pagina, Controleer de naam van de server-groep en klikt u op de laatste *verwijderen* knop.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het inrichten van een grootschalige (Citus) server-groep. U verbonden met psql, een schema gemaakt en gedistribueerde gegevens. Hebt u geleerd om gegevens te doorzoeken beide in de onbewerkte vorm regelmatig dat gegevens worden geaggregeerd, vragen de samengevoegde tabellen en oude gegevens laten verlopen.

Hierna leert over de concepten van grootschalige.
> [!div class="nextstepaction"]
> [Zeer grootschalige knooppunttypen](https://aka.ms/hyperscale-concepts)
---
title: Query Store in Azure Database for PostgreSQL
description: Dit artikel beschrijft de functie voor Query Store in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 149840157c5e9bb47be70f669b2078585fe4b56c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953020"
---
# <a name="monitor-performance-with-the-query-store"></a>Prestaties bijhouden met de Query Store

**Is van toepassing op:** Azure Database for PostgreSQL 9.6 en 10

> [!IMPORTANT]
> De functie voor Query Store is in openbare Preview.


De functie voor Query Store in Azure Database for PostgreSQL biedt een manier voor het bijhouden van prestaties van query's na verloop van tijd. Query Store vereenvoudigt het door u te helpen snel problemen met prestaties vinden het langst lopende en meest resource-intensieve query's. Query Store automatisch een geschiedenis van query's en runtime-statistieken worden vastgelegd en behoudt hij ze controleren. Deze scheidt gegevens door tijdvensters zodat u databasegebruikspatronen kunt zien. Gegevens voor alle gebruikers, databases en query's worden opgeslagen in een database met de naam **azure_sys** in de Azure Database for PostgreSQL-exemplaar.

> [!IMPORTANT]
> Wijzig niet de **azure_sys** database of de schema's. In dat geval wordt voorkomen dat prestaties gerelateerd aan functies en Query Store goed werkt.

## <a name="enabling-query-store"></a>Query Store inschakelen
Query Store is een functie opt-in, zodat deze niet standaard op een server actief is. Het archief is ingeschakeld of uitgeschakeld wereldwijd voor alle databases op een bepaalde server en kan niet worden ingeschakeld in- of uitschakelen per database.

### <a name="enable-query-store-using-the-azure-portal"></a>Query Store met behulp van de Azure-portal inschakelen
1. Meld u aan bij Azure portal en selecteer uw Azure Database for PostgreSQL-server.
2. Selecteer **serverparameters** in de **instellingen** gedeelte van het menu.
3. Zoek de **pg_qs.query_capture_mode** parameter.
4. Werk de waarde van geen naar boven en opslaan.

U kunt ook instellen dat deze parameter met de Azure CLI.
```azurecli-interactive
az postgres server configuration set --name pg_qs.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value TOP
```

Maximaal 20 minuten voor de eerste batch van gegevens om vast te leggen in de database azure_sys toestaan.

## <a name="information-in-query-store"></a>Gegevens in Query Store
Query Store heeft twee stores:
- Een runtime-statistieken-archief voor het opslaan van de statistische gegevens van de query kan worden uitgevoerd.
- Een ogenblik geduld statistieken store voor het opslaan van statistische gegevens wacht.

Algemene scenario's voor het gebruik van de Query Store zijn onder andere:
- Bepalen van het aantal keren dat is een query uitgevoerd in een opgegeven periode
- De gemiddelde uitvoeringstijd van een query vergelijken voor tijdvensters om te zien van grote delta 's
- Identificeren van de langste uitvoeren van query's in het verleden X uur
- Top N-query's die op resources wachten identificeren
- Understanding wacht aard voor een bepaalde query

Om te beperken gebruik van schijfruimte, worden de runtime-statistieken voor uitvoering in het archief van de runtime-statistieken voor een vaste en configureerbare periode geaggregeerd. De informatie in deze archieven is zichtbaar door het opvragen van de query store-weergaven.

De volgende query retourneert informatie over query's in Query Store:
```sql
SELECT * FROM query_store.qs_view; 
``` 

Of deze query voor Wachtstatistieken voor:
```sql
SELECT * FROM query_store.pgms_wait_sampling_view;
```

## <a name="finding-wait-queries"></a>Zoeken naar wait-query 's
Wacht gebeurtenistypen combineren verschillende wacht gebeurtenissen in buckets die vergelijkbaar zijn. Query Store biedt het gebeurtenistype wachten, gebeurtenisnaam specifieke wachten en de query in kwestie. Dat kunnen correleren van deze informatie wachten met de query-runtime statistieken betekent dat u een beter inzicht in wat draagt bij aan de prestatiekenmerken van de query kan krijgen.

Hier volgen enkele voorbeelden van hoe u krijgt meer inzicht in uw workload met behulp van de statistieken wacht in Query Store:

| **Observatie van** | **Actie** |
|---|---|
|Hoge vergrendeling moet wachten | Controleer de tekst van de query voor de betrokken query's en identificeren van de doel-entiteiten. Zoek in de Query Store naar andere query's bij het wijzigen van dezelfde entiteit, die vaak wordt uitgevoerd en/of maximale duur hebben. Nadat u deze query's, kunt u overwegen de toepassingslogica ter verbetering van gelijktijdigheid wijzigen of een minder beperkend isolatieniveau gebruiken.|
| Hoge i/o-Buffer wacht | De query's met een groot aantal fysieke leesbewerkingen in Query Store vinden. Als ze overeenkomen met de query's met hoge i/o-wachttijd, houd rekening met de introductie van een index voor de onderliggende entiteit hiervoor zoekt in plaats van scans. Dit zou de i/o-overhead van de query's beperken. Controleer de **aanbevelingen voor prestaties** voor uw server in de portal om te zien of er indexaanbevelingen voor deze server die u zou de query's optimaliseren.|
| Hoge geheugen moet wachten | Het bovenste geheugen verbruikt query's in Query Store vinden. Deze query's zijn waarschijnlijk vertragen verder voortgang van de betrokken query's. Controleer de **aanbevelingen voor prestaties** voor uw server in de portal om te zien of er indexaanbevelingen die zou deze query's optimaliseren.|

## <a name="configuration-options"></a>Configuratie-opties
Als de Query Store is ingeschakeld worden gegevens opgeslagen in windows voor aggregatie van 15 minuten, maximaal 500 afzonderlijke query's per tijdvenster van. 

De volgende opties zijn beschikbaar voor Query Store-parameters configureren.
| **Parameter** | **Beschrijving** | **Standaard** | **Bereik**|
|---|---|---|---|
| pg_qs.query_capture_mode | Hiermee stelt u welke instructies worden bijgehouden. | Boven | None, top, alle |
| pg_qs.max_query_text_length | Hiermee stelt u de maximale lengte die kan worden opgeslagen. Meer query's worden afgekapt. | 6000 | 100 - 10K |
| pg_qs.retention_period_in_days | Hiermee stelt u de bewaarperiode. | 7 | 1 - 30 |
| pg_qs.track_utility | Wordt ingesteld of hulpprogramma opdrachten worden bijgehouden | op | op uitgeschakeld |

De volgende opties gelden specifiek voor het wachten van statistieken.
| **Parameter** | **Beschrijving** | **Standaard** | **Bereik**|
|---|---|---|---|
| pgms_wait_sampling.query_capture_mode | Sets die instructies worden bijgehouden voor wachten statistieken. | geen | geen, alle|
| Pgms_wait_sampling.history_period | Stel de frequentie, in milliseconden, op welke wacht gebeurtenissen worden vastgelegd. | 100 | 1-600000 |

> [!NOTE] 
> **pg_qs.query_capture_mode** vervangt **pgms_wait_sampling.query_capture_mode**. Als pg_qs.query_capture_mode ingesteld op geen is, heeft de instelling pgms_wait_sampling.query_capture_mode geen effect.


Gebruik de [Azure-portal](howto-configure-server-parameters-using-portal.md) of [Azure CLI](howto-configure-server-parameters-using-cli.md) ophalen of instellen van een andere waarde voor een parameter.

## <a name="views-and-functions"></a>Weergaven en functies
Weergeven en beheren van de Query Store met behulp van de volgende functies en weergaven. Iedereen in de openbare rol PostgreSQL kunt deze weergaven gebruiken om de gegevens in Query Store te zien. Deze weergaven zijn alleen beschikbaar in de **azure_sys** database.

Query's zijn genormaliseerd door te kijken de structuur ervan na het verwijderen van letterlijke waarden en constanten. Als twee query's, met uitzondering van letterlijke waarden identiek zijn, hebben ze dezelfde hash.

### <a name="querystoreqsview"></a>query_store.qs_view
In deze weergave retourneert alle gegevens in Query Store. Er is één rij voor elke afzonderlijke database-ID, gebruikers-ID en query-ID. 

|**Naam**   |**Type** | **Verwijzingen**  | **Beschrijving**|
|---|---|---|---|
|runtime_stats_entry_id |bigint | | ID van de tabel runtime_stats_entries|
|USER_ID    |OID    |pg_authid.OID  |OID van de gebruiker heeft de instructie uitgevoerd|
|%{db_id/  |OID    |pg_database.OID    |OID van database waarin de instructie is uitgevoerd|
|ID   |bigint  || Interne hash-code, berekend vanaf parseringsstructuur van de instructie|
|query_sql_text |Varchar(10000)  || Tekst van een representatieve instructie. Verschillende query's met dezelfde structuur zijn samen; geclusterd Deze tekst is de tekst voor de eerste dag van de query's in het cluster.|
|het queryplan    |bigint |   |ID van het abonnement nog overeenkomt met deze query is niet beschikbaar|
|start_time |tijdstempel  ||  Query's worden samengevoegd per keer buckets - de tijdsduur van een bucket is 15 minuten standaard maar kunnen worden geconfigureerd. Dit is de begintijd die overeenkomt met het tijdsinterval voor deze vermelding.|
|end_time   |tijdstempel  ||  De eindtijd die overeenkomt met het tijdsinterval voor deze vermelding.|
|oproepen  |bigint  || Aantal keren dat de query wordt uitgevoerd|
|TOTAL_TIME |dubbele precisie   ||  Totaal aantal query uitvoeringstijd, in milliseconden|
|min_time   |dubbele precisie   ||  Minimale query uitvoeringstijd, in milliseconden|
|max_time   |dubbele precisie   ||  Maximale uitvoeringstijd, in milliseconden|
|mean_time  |dubbele precisie   ||  Gemiddelde uitvoeringstijd van de query, in milliseconden|
|stddev_time|   dubbele precisie    ||  Standaarddeviatie van de tijd van de query kan worden uitgevoerd in milliseconden |
|rijen   |bigint ||  Totale aantal rijen opgehaald of beïnvloed door de instructie|
|shared_blks_hit|   bigint  ||  Totaal aantal treffers in cache en gedeelde blokkeren door de instructie|
|shared_blks_read|  bigint  ||  Totale aantal gedeelde blokken gelezen door de instructie|
|shared_blks_dirtied|   bigint   || Totale aantal gedeelde blokken dirtied door de instructie |
|shared_blks_written|   bigint  ||  Totale aantal gedeelde blokken die zijn geschreven door de instructie|
|local_blks_hit|    bigint ||   Totaal aantal treffers in cache en lokale blokkeren door de instructie|
|local_blks_read|   bigint   || Totaal aantal lokale blokken gelezen door de instructie|
|local_blks_dirtied|    bigint  ||  Totaal aantal lokale blokken dirtied door de instructie|
|local_blks_written|    bigint  ||  Totaal aantal lokale blokken die zijn geschreven door de instructie|
|temp_blks_read |bigint  || Totaal aantal tijdelijke blokken gelezen door de instructie|
|temp_blks_written| bigint   || Totaal aantal tijdelijke blokken die zijn geschreven door de instructie|
|blk_read_time  |dubbele precisie    || Totale tijd dat de instructie dat is besteed aan blokken bij het lezen, in milliseconden (als track_io_timing is ingeschakeld, anders nul)|
|blk_write_time |dubbele precisie    || Totale tijd dat de instructie dat is besteed aan blokken bij het schrijven, in milliseconden (als track_io_timing is ingeschakeld, anders nul)|
    
### <a name="querystorequerytextsview"></a>query_store.query_texts_view
In deze weergave retourneert tekst querygegevens in Query Store. Er is één rij voor elke afzonderlijke query_text.

|**Naam**|  **Type**|   **Beschrijving**|
|---|---|---|
|query_text_id  |bigint     |ID van de tabel query_texts|
|query_sql_text |Varchar(10000)     |Tekst van een representatieve instructie. Verschillende query's met dezelfde structuur zijn samen; geclusterd Deze tekst is de tekst voor de eerste dag van de query's in het cluster.|

### <a name="querystorepgmswaitsamplingview"></a>query_store.pgms_wait_sampling_view
In deze weergave retourneert gegevens van statuswijzigingsgebeurtenissen in Query Store wachten. Er is één rij voor elke afzonderlijke database-ID, de gebruikers-ID, de query-ID en de gebeurtenis.

|**Naam**|  **Type**|   **Verwijzingen**| **Beschrijving**|
|---|---|---|---|
|USER_ID    |OID    |pg_authid.OID  |OID van de gebruiker heeft de instructie uitgevoerd|
|%{db_id/  |OID    |pg_database.OID    |OID van database waarin de instructie is uitgevoerd|
|ID   |bigint     ||Interne hash-code, berekend vanaf parseringsstructuur van de instructie|
|event_type |tekst       ||Het type gebeurtenis waarvoor de back-end is in afwachting|
|gebeurtenis  |tekst       ||De naam van de wachttijd als back-end is momenteel in afwachting|
|oproepen  |Geheel getal        ||Aantal dezelfde gebeurtenis vastgelegd|


### <a name="functions"></a>Functions
Query_store.qs_reset() geeft als resultaat void

`qs_reset` Hiermee verwijdert alle statistische gegevens die tot nu toe door de Query Store worden verzameld. Deze functie kan alleen worden uitgevoerd door de rol van de server-beheerder.

Query_store.staging_data_reset() geeft als resultaat void

`staging_data_reset` Hiermee verwijdert alle statistische gegevens die in het geheugen worden verzameld door de Query Store (dat wil zeggen, de gegevens in het geheugen dat niet is verwijderd, maar met de database). Deze functie kan alleen worden uitgevoerd door de rol van de server-beheerder.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [scenario's waarbij Query Store met name handig zijn kan](concepts-query-store-scenarios.md).
- Meer informatie over [aanbevolen procedures voor het gebruik van de Query Store](concepts-query-store-best-practices.md).
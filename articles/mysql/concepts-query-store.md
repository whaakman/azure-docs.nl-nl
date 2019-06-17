---
title: Query Store in Azure Database for MySQL
description: In dit artikel beschrijft de functie voor Query Store in Azure Database voor MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 5cd88eeb2016a90a95f0e54a334eb3c88cb75744
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078740"
---
# <a name="monitor-azure-database-for-mysql-performance-with-query-store"></a>Azure Database voor MySQL-prestaties met Query Store bewaken

**Is van toepassing op:**  Azure Database voor MySQL 5.7

> [!NOTE]
> Query Store is beschikbaar als preview. Ondersteuning voor Query Store in Azure portal is geïmplementeerd en kan nog niet beschikbaar in uw regio.

De functie voor Query Store in Azure Database for MySQL biedt een manier voor het bijhouden van prestaties van query's na verloop van tijd. Query Store vereenvoudigt het door u te helpen snel problemen met prestaties vinden het langst lopende en meest resource-intensieve query's. Query Store automatisch een geschiedenis van query's en runtime-statistieken worden vastgelegd en behoudt hij ze controleren. Deze scheidt gegevens door tijdvensters zodat u databasegebruikspatronen kunt zien. Gegevens voor alle gebruikers, databases en query's worden opgeslagen in de **mysql** schemadatabase in de Azure Database for MySQL-exemplaar.

## <a name="common-scenarios-for-using-query-store"></a>Algemene scenario's voor het gebruik van de Query Store

Query store kan worden gebruikt in een aantal scenario's, waaronder het volgende:

- Verminderde query's detecteren
- Bepalen van het aantal keren dat is een query uitgevoerd in een opgegeven periode
- De gemiddelde uitvoeringstijd van een query vergelijken voor tijdvensters om te zien van grote delta 's
- Identificeren van de langste uitvoeren van query's in het verleden X uur
- Top N-query's die op resources wachten identificeren
- Understanding wacht aard van een query
- Informatie over trends voor resource wacht en waar bronconflicten bestaat

## <a name="enabling-query-store"></a>Query Store inschakelen

Query Store is een functie opt-in, zodat deze niet standaard op een server actief is. De query store is ingeschakeld of uitgeschakeld wereldwijd voor alle databases op een bepaalde server en kan niet worden ingeschakeld in- of uitschakelen per database.

### <a name="enable-query-store-using-the-azure-portal"></a>Query Store met behulp van de Azure-portal inschakelen

1. Meld u aan bij Azure portal en selecteer uw Azure Database voor MySQL-server.
1. Selecteer **serverparameters** in de **instellingen** gedeelte van het menu.
1. Zoeken naar de parameter query_store_capture_mode.
1. Stel de waarde in op alle en **opslaan**.

Statistieken voor de wachttijd in uw Query Store inschakelen:

1. Zoeken naar de parameter query_store_wait_sampling_capture_mode.
1. Stel de waarde in op alle en **opslaan**.

Kunnen maximaal 20 minuten voor de eerste batch van gegevens om vast te leggen in de mysql-database.

## <a name="information-in-query-store"></a>Gegevens in Query Store

Query Store heeft twee stores:

- Een runtime-statistieken voor het opslaan van de query kan worden uitgevoerd statistische gegevens worden opgeslagen.
- Een ogenblik geduld statistieken store voor het opslaan van statistische gegevens wacht.

Voor het gebruik van schijfruimte beperken, zijn de runtime-statistieken voor uitvoering in het archief van de runtime-statistieken zijn een vaste en configureerbare periode geaggregeerd. De informatie in deze archieven is zichtbaar door het opvragen van de query store-weergaven.

De volgende query retourneert informatie over query's in Query Store:

```sql
SELECT * FROM mysql.query_store;
```

Of deze query voor wait-statistieken:

```sql
SELECT * FROM mysql.query_store_wait_stats;
```

## <a name="finding-wait-queries"></a>Zoeken naar wait-query 's

Wacht gebeurtenistypen combineren verschillende wacht gebeurtenissen in buckets die vergelijkbaar zijn. Query Store biedt het gebeurtenistype wachten, gebeurtenisnaam specifieke wachten en de query in kwestie. Dat kunnen correleren van deze informatie wachten met de query-runtime statistieken betekent dat u een beter inzicht in wat draagt bij aan de prestatiekenmerken van de query kan krijgen.

Hier volgen enkele voorbeelden van hoe u krijgt meer inzicht in uw workload met behulp van de statistieken wacht in Query Store:

| **Observatie van** | **Actie** |
|---|---|
|Hoge vergrendeling moet wachten | Controleer de tekst van de query voor de betrokken query's en identificeren van de doel-entiteiten. Zoek in de Query Store naar andere query's bij het wijzigen van dezelfde entiteit, die vaak wordt uitgevoerd en/of maximale duur hebben. Nadat u deze query's, kunt u overwegen de toepassingslogica ter verbetering van gelijktijdigheid wijzigen of een minder beperkend isolatieniveau gebruiken. |
|Hoge i/o-Buffer wacht | De query's met een groot aantal fysieke leesbewerkingen in Query Store vinden. Als ze overeenkomen met de query's met hoge i/o-wachttijd, houd rekening met de introductie van een index op de onderliggende entiteit, doen zoekt in plaats van scans. Dit zou de i/o-overhead van de query's beperken. Controleer de **aanbevelingen voor prestaties** voor uw server in de portal om te zien of er indexaanbevelingen voor deze server die u zou de query's optimaliseren. |
|Hoge geheugen moet wachten | Het bovenste geheugen verbruikt query's in Query Store vinden. Deze query's zijn waarschijnlijk vertragen verder voortgang van de betrokken query's. Controleer de **aanbevelingen voor prestaties** voor uw server in de portal om te zien of er indexaanbevelingen die zou deze query's optimaliseren.|

## <a name="configuration-options"></a>Configuratie-opties

Als de Query Store is ingeschakeld worden gegevens opgeslagen in windows voor aggregatie van 15 minuten, maximaal 500 afzonderlijke query's per tijdvenster van.

De volgende opties zijn beschikbaar voor Query Store-parameters configureren.

| **Parameter** | **Beschrijving** | **Standaard** | **Range** |
|---|---|---|---|
| query_store_capture_mode | Schakel de functie voor query store aan/uit op basis van de waarde. Opmerking: Als performance_schema uitgeschakeld is, wordt query_store_capture_mode inschakelen inschakelen performance_schema en een subset van de prestaties schema instrumenten vereist voor deze functie. | ALLE | GEEN, ALLE |
| query_store_capture_interval | De query store vastleggen interval in minuten. Hiermee kunt het interval waarin de query metrische gegevens worden samengevoegd op te geven | 15 | 5 - 60 |
| query_store_capture_utility_queries | IN te schakelen of uit voor het vastleggen van alle hulpprogramma-query's dat wordt uitgevoerd in het systeem. | NO | JA, NEE |
| query_store_retention_period_in_days | Periode in dagen voor het behouden van de gegevens in de query store. | 7 | 1 - 30 |

De volgende opties gelden specifiek voor het wachten van statistieken.

| **Parameter** | **Beschrijving** | **Standaard** | **Range** |
|---|---|---|---|
| query_store_wait_sampling_capture_mode | Kunnen in te schakelen / uitschakelen van de statistieken wachten. | GEEN | GEEN, ALLE |
| query_store_wait_sampling_frequency | De frequentie van de Alters van wait-densitysampling in seconden. 5 tot en met 300 seconden. | 30 | 5-300 |

> [!NOTE]
> Op dit moment **query_store_capture_mode** vervangt deze configuratie, wat betekent dat zowel **query_store_capture_mode** en **query_store_wait_sampling_capture_mode** moet worden ingeschakeld op alle voor statistieken wacht om te werken. Als **query_store_capture_mode** is uitgeschakeld, en vervolgens wachten statistieken is ook uitgeschakeld omdat het wacht statistieken maakt gebruik van de performance_schema ingeschakeld en de query_text vastgelegd door query store.

Gebruik de [Azure-portal](howto-server-parameters.md) of [Azure CLI](howto-configure-server-parameters-using-cli.md) ophalen of instellen van een andere waarde voor een parameter.

## <a name="views-and-functions"></a>Weergaven en functies

Weergeven en beheren van de Query Store met behulp van de volgende functies en weergaven. Iedereen in de [de openbare rol selecteren bevoegdheden](howto-create-users.md#how-to-create-additional-admin-users-in-azure-database-for-mysql) deze weergaven kunt gebruiken om de gegevens in Query Store te zien. Deze weergaven zijn alleen beschikbaar in de **mysql** database.

Query's zijn genormaliseerd door te kijken de structuur ervan na het verwijderen van letterlijke waarden en constanten. Als twee query's, met uitzondering van letterlijke waarden identiek zijn, hebben ze dezelfde hash.

### <a name="mysqlquerystore"></a>mysql.query_store

In deze weergave retourneert alle gegevens in Query Store. Er is één rij voor elke afzonderlijke database-ID, gebruikers-ID en query-ID.

| **Naam** | **Gegevenstype** | **IS_NULLABLE** | **Beschrijving** |
|---|---|---|---|
| `schema_name`| varchar(64) | NO | Naam van het schema |
| `query_id`| bigint(20) | NO| Unieke ID voor de specifieke query wordt gegenereerd als de dezelfde query wordt uitgevoerd in een ander schema, een nieuwe ID worden gegenereerd |
| `timestamp_id` | timestamp| NO| Tijdstempel waarin de query wordt uitgevoerd. Dit is gebaseerd op de configuratie van de query_store_interval|
| `query_digest_text`| longtext| NO| De genormaliseerde querytekst na het verwijderen van de letterlijke tekenreeksen zijn|
| `query_sample_text` | longtext| NO| Eerste weergave van de werkelijke query met letterlijke waarden|
| `query_digest_truncated` | bit| JA| Of de querytekst is afgekapt. Waarde is Ja als de query meer dan 1 KB is|
| `execution_count` | bigint(20)| NO| Het aantal keren dat die de query is uitgevoerd voor deze timestamp-ID / gedurende de Intervalperiode van het geconfigureerde|
| `warning_count` | bigint(20)| NO| Aantal waarschuwingen dat deze query gegenereerd tijdens de interne|
| `error_count` | bigint(20)| NO| Het aantal fouten dat deze query gegenereerd tijdens het interval van|
| `sum_timer_wait` | double| JA| Totale uitvoeringstijd van deze query tijdens het interval|
| `avg_timer_wait` | double| JA| Gemiddelde uitvoeringstijd voor deze query tijdens het interval|
| `min_timer_wait` | double| JA| Minimale uitvoeringstijd voor deze query|
| `max_timer_wait` | double| JA| Maximale uitvoeringstijd|
| `sum_lock_time` | bigint(20)| NO| Totale hoeveelheid tijd die voor de vergrendelingen voor deze query kan worden uitgevoerd tijdens deze periode is besteed|
| `sum_rows_affected` | bigint(20)| NO| Aantal rijen beïnvloed|
| `sum_rows_sent` | bigint(20)| NO| Aantal rijen dat is verzonden naar client|
| `sum_rows_examined` | bigint(20)| NO| Aantal rijen onderzocht|
| `sum_select_full_join` | bigint(20)| NO| Aantal volledige joins|
| `sum_select_scan` | bigint(20)| NO| Het aantal select scans |
| `sum_sort_rows` | bigint(20)| NO| Aantal rijen gesorteerd|
| `sum_no_index_used` | bigint(20)| NO| Aantal keren dat wanneer de query geen indexen niet hebt gebruikt|
| `sum_no_good_index_used` | bigint(20)| NO| Aantal keren dat wanneer de engine voor het uitvoeren van query geen goede indexen niet hebt gebruikt|
| `sum_created_tmp_tables` | bigint(20)| NO| Totaal aantal tijdelijke tabellen die zijn gemaakt|
| `sum_created_tmp_disk_tables` | bigint(20)| NO| Totaal aantal tijdelijke tabellen die zijn gemaakt op de schijf (i/o genereert)|
| `first_seen` | timestamp| NO| De eerste instantie (UTC) van de query in het venster aggregatie|
| `last_seen` | timestamp| NO| Het laatste exemplaar (UTC) van de query tijdens dit venster aggregatie|

### <a name="mysqlquerystorewaitstats"></a>mysql.query_store_wait_stats

In deze weergave retourneert gegevens van statuswijzigingsgebeurtenissen in Query Store wachten. Er is één rij voor elke afzonderlijke database-ID, de gebruikers-ID, de query-ID en de gebeurtenis.

| **Naam**| **Gegevenstype** | **IS_NULLABLE** | **Beschrijving** |
|---|---|---|---|
| `interval_start` | timestamp | NO| Begin van het interval (15 minuten verhogen)|
| `interval_end` | timestamp | NO| Einde van het interval (15 minuten verhogen)|
| `query_id` | bigint(20) | NO| Gegenereerde unieke ID van de genormaliseerde query (uit het queryarchief)|
| `query_digest_id` | varchar(32) | NO| De genormaliseerde querytekst na het verwijderen van alle letterlijke waarden (in query store) |
| `query_digest_text` | longtext | NO| Eerste weergave van de werkelijke query met letterlijke waarden (in query store) |
| `event_type` | varchar(32) | NO| Categorie van de gebeurtenis wait |
| `event_name` | varchar(128) | NO| Naam van de gebeurtenis wait |
| `count_star` | bigint(20) | NO| Aantal gebeurtenissen dat wacht voorbeelden worden gemaakt tijdens het interval voor de query |
| `sum_timer_wait_ms` | double | NO| De totale wachttijd (in milliseconden) van deze query tijdens het interval |

### <a name="functions"></a>Functions

| **Naam**| **Beschrijving** |
|---|---|
| `mysql.az_purge_querystore_data(TIMESTAMP)` | Hiermee verwijdert u alle gegevens in de query opslaan voordat u de opgegeven tijdstempel |
| `mysql.az_procedure_purge_querystore_event(TIMESTAMP)` | Worden alle gebeurtenisgegevens voor de opgegeven tijdstempel wachten |
| `mysql.az_procedure_purge_recommendation(TIMESTAMP)` | Hiermee verwijdert u aanbevelingen waarvan verlopen voor de opgegeven tijdstempel is |

## <a name="limitations-and-known-issues"></a>Beperkingen en bekende problemen

- Als een MySQL-server de parameter heeft `default_transaction_read_only` , Query Store niet kan vastleggen van gegevens.
- Query Store functionaliteit kan worden onderbroken als er lange Unicode-query's (\>= 6000 bytes).
- De bewaarperiode voor wait statistieken is 24 uur.
- Wacht statistieken gebruikt voorbeeld ti vastleggen een fractie van de gebeurtenissen. De frequentie kan worden gewijzigd met de parameter `query_store_wait_sampling_frequency`.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Query prestatie-inzichten](concepts-query-performance-insight.md)
---
title: PostgreSQL-extensies gebruiken in Azure Database for PostgreSQL
description: Beschrijft de mogelijkheid om uit te breiden de functionaliteit van uw database met behulp van uitbreidingen in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: 7d052ee2d3d3bdf6cca99dd6a91b88176983113f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57888073"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-extensies in Azure Database for PostgreSQL
PostgreSQL biedt de mogelijkheid om uit te breiden de functionaliteit van uw database met behulp van extensies. Extensies bieden samen bundeling van meerdere verwante SQL-objecten in één pakket die kan worden geladen of verwijderd uit de database met slechts één opdracht. Extensies kunnen na worden geladen in de database, functioneren als ingebouwde functies. Zie voor meer informatie over de PostgreSQL-extensies, [verpakking verwante objecten in een extensie](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Het gebruik van de PostgreSQL-extensies
PostgreSQL-extensies moeten worden geïnstalleerd in uw database voordat u ze kunt gebruiken. Voer voor het installeren van een bepaalde extensie de [EXTENSIE maken](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) opdracht uit vanaf de psql-hulpprogramma voor het laden van de verpakte objecten in uw database.

Azure Database voor PostgreSQL ondersteunt momenteel een subset van de belangrijkste extensies zoals hieronder vermeld. Extensies dan de waarden worden niet ondersteund. u kunt uw eigen uitbreiding maken met Azure Database voor PostgreSQL-service.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>-Extensies ondersteund door Azure Database for PostgreSQL
De volgende tabellen worden de standaard PostgreSQL-extensies die momenteel worden ondersteund door Azure Database voor PostgreSQL. Deze informatie is ook beschikbaar door uit te voeren `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Gegevenstypen extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Biedt een gegevenstype voor wachtwoorden automatisch versleuteld. |
> | [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Biedt een niet-hoofdlettergevoelige tekenreeks tekentype. |
> | [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Biedt een gegevenstype voor multidimensionale kubussen. |
> | [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Biedt een gegevenstype voor het opslaan van sets met sleutel/waarde-paren. |
> | [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | Biedt gegevenstypen voor internationale standaarden nummering-product. |
> | [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Biedt een gegevenstype voor hiërarchische boomstructuren. |

### <a name="functions-extensions"></a>Functions-extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Biedt een manier om te berekenen great cirkel afstanden op het oppervlak van de aarde. |
> | [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Biedt verschillende functies om te bepalen van overeenkomsten en de afstand tussen de tekenreeksen. |
> | [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Biedt de functies en operatoren voor het manipuleren van null-free-matrices met gehele getallen. |
> | [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Biedt cryptografische functies. |
> | [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Gepartitioneerde tabellen worden beheerd door de tijd of -ID. |
> | [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Biedt de functies en operatoren voor het bepalen van de overeenkomsten van alfanumerieke tekst op basis van trigram overeenkomst. |
> | [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Biedt functies die hele tabellen, inclusief een kruistabel bewerken. |
> | [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Genereert universele, unieke id (UUID's). |

### <a name="full-text-search-extensions"></a>Extensies zoeken in volledige tekst

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Biedt een sjabloon zoeken woordenlijst voor gehele getallen. |
> | [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Een woordenlijst van de tekst zoeken waarmee accenten (diakritische tekens) uit lexemes worden verwijderd. |

### <a name="index-types-extensions"></a>Index typen extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [bTree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Voorbeeld GIN operator klassen die B-tree, zoals gedrag voor bepaalde gegevenstypen implementeren bevat. |
> | [bTree\_basisvertalingen](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Biedt basisvertalingen index operator klassen die B-tree implementeren. |

### <a name="language-extensions"></a>Taalextensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL procedurele taal kan worden geladen. |
> | [plv8](https://plv8.github.io/) | Een Javascript-taal-extensie voor PostgreSQL die kan worden gebruikt voor opgeslagen procedures, triggers, enzovoort. |

### <a name="miscellaneous-extensions"></a>Overige extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Biedt een manier voor het onderzoeken wat er gebeurt in de buffercache gedeelde in realtime. |
> | [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Biedt een manier om de relatie gegevens laden in de buffercache. |
> | [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Biedt een manier voor het bijhouden van Uitvoeringsstatistieken van alle SQL-instructies uitvoeren op een server. (Zie hieronder voor een notitie van deze extensie). |
> | [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Biedt een manier voor het weergeven van beveiliging op rijniveau vergrendelen informatie. |
> | [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Biedt een manier voor het weergeven van statistieken van de tuple op serverniveau. |
> | [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Extern-data-wrapper gebruikt voor toegang tot gegevens die zijn opgeslagen in externe PostgreSQL-servers. (Zie hieronder voor een notitie van deze extensie).|
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | Biedt een manier voor het maken van hypothetische indexen die geen CPU of schijf kosten. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Een module die ondersteuning biedt voor verbindingen met andere PostgreSQL-databases uit in een databasesessie. (Zie hieronder voor een notitie van deze extensie). |


### <a name="postgis-extensions"></a>PostGIS-extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal | Ruimtelijke en geografische objecten voor PostgreSQL. |
> | adres\_standardizer, adres\_standardizer\_gegevens\_ons | Gebruikt voor het parseren van een adres naar de bijbehorende elementen. Gebruikt ter ondersteuning van geocodering adres normalisering stap. |
> | [pgrouting](https://pgrouting.org/) | Breidt de PostGIS / PostgreSQL georuimtelijke database voor georuimtelijke functies routering. |


### <a name="time-series-extensions"></a>Time series-extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [TimescaleDB](https://docs.timescale.com/latest) | Een time series SQL-database die ondersteunt automatische voor het partitioneren van sneller opnemen en query's. Voorziet in analytische functies tijdgebaseerde, optimalisatie, en kan worden geschaald PostgreSQL voor time series-werkbelastingen. TimescaleDB is ontwikkeld door en een geregistreerd handelsmerk van [tijdschaal, Inc.](https://www.timescale.com/) (Zie hieronder voor een notitie van deze extensie). |


## <a name="pgstatstatements"></a>pg_stat_statements
De [pg\_stat\_instructies extensie](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) is vooraf geladen op elke Azure Database for PostgreSQL-server zodat u kunt een manier voor het bijhouden van Uitvoeringsstatistieken van SQL-instructies.
De instelling `pg_stat_statements.track`, die bepaalt welke instructies zijn geteld op basis van de extensie, wordt standaard ingesteld op `top`, wat betekent dat alle instructies uitgegeven rechtstreeks door clients worden bijgehouden. De twee andere traceringsniveaus zijn `none` en `all`. Deze instelling kan worden geconfigureerd als een serverparameter via de [Azure-portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) of de [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Er is een verschil tussen de querygegevens kan worden uitgevoerd als pg_stat_statements biedt en de impact op de prestaties van de server als deze zich aanmeldt met elke SQL-instructie. Als u de extensie pg_stat_statements niet actief gebruikt, is het raadzaam dat u instelt `pg_stat_statements.track` naar `none`. Houd er rekening mee dat sommige van derden bewaken van services mogelijk afhankelijk van pg_stat_statements query prestatie-inzichten leveren, dus controleer of dit het geval is voor u is of niet.

## <a name="dblink-and-postgresfdw"></a>dblink en postgres_fdw
dblink en postgres_fdw kunt u verbinding maken vanaf een PostgreSQL-server naar een andere of naar een andere database op dezelfde server. U moet de ontvangende server verbindingen toestaan vanaf de server verzenden via de firewall. Wanneer u deze uitbreidingen te verbinden met Azure Database for PostgreSQL-servers, kan dit worden gedaan door in te stellen 'Toegang tot Azure-services toestaan' op ON. Dit is ook nodig als u de extensies wilt aan lus terug naar dezelfde server gebruiken. De instelling 'Toegang tot Azure-services toestaan' vindt u in de Azure portal-pagina voor de Postgres-server, onder de beveiliging van de verbinding. 'Toestaan toegang tot Azure-services' op accounttoewijzing alle Azure-IP-adressen te schakelen.

Uitgaande verbindingen van Azure Database for PostgreSQL worden op dit moment niet ondersteund, met uitzondering van verbindingen met andere Azure-Database voor PostgreSQL-servers.

## <a name="timescaledb"></a>TimescaleDB
TimescaleDB is een time series-database die wordt geleverd als een uitbreiding voor PostgreSQL. TimescaleDB voorziet in analytische functies tijdgebaseerde, optimalisatie, en kan worden geschaald Postgres voor time series-werkbelastingen.

[Meer informatie over TimescaleDB](https://docs.timescale.com/latest), een geregistreerd handelsmerk van [tijdschaal, Inc.](https://www.timescale.com/)

### <a name="installing-timescaledb"></a>TimescaleDB installeren
Als u wilt installeren TimescaleDB, die u wilt opnemen in de gedeelde vooraf bibliotheken van de server. Een wijziging in de Postgres-bibliotheken voor gedeelde vooraf vereist een **opnieuw is opgestart** pas van kracht.

> [!NOTE]
> TimescaleDB kan worden ingeschakeld voor Azure Database voor PostgreSQL-versie 9.6 en 10

Met behulp van de [Azure-portal](https://portal.azure.com/):

1. Selecteer uw Azure Database for PostgreSQL-server.

2. Selecteer op de zijbalk **serverparameters**.

3. Zoek de `shared_preload_libraries` parameter.

4. Kopieer en plak het volgende als de waarde voor `shared_preload_libraries`
   ```
   timescaledb
   ```

5. Selecteer **opslaan** uw wijzigingen te behouden. U ontvangt een melding wanneer de wijziging is opgeslagen. 

6. Na de kennisgeving **opnieuw** de server om deze wijzigingen te passen. Als u wilt weten hoe u een server opnieuw opstarten, Zie [opnieuw starten van een Azure Database for PostgreSQL-server](howto-restart-server-portal.md).


U kunt nu TimescaleDB inschakelen in uw Postgres-database. Verbinding maken met de database en voert u de volgende opdracht uit:
```sql
CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;
```
> [!TIP]
> Als u een fout ziet, controleert u of u [de server opnieuw opgestart](howto-restart-server-portal.md) na het opslaan van shared_preload_libraries. 

U kunt nu een hypertable TimescaleDB maken [helemaal](https://docs.timescale.com/getting-started/creating-hypertables) of migreren [bestaande time series-gegevens in PostgreSQL](https://docs.timescale.com/getting-started/migrating-data).


## <a name="next-steps"></a>Volgende stappen
Als u een extensie die u wilt gebruiken, laat het ons weten niet ziet. Stem voor bestaande aanvragen of maken van nieuwe feedback en verzoeken in onze [forum met feedback van klanten](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

---
title: PostgreSQL-extensies in Azure Database for PostgreSQL - grootschalige (Citus) (preview)
description: Beschrijft de mogelijkheid om uit te breiden de functionaliteit van uw database met behulp van uitbreidingen in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 32870f37781b4161de692af91c79fe47efb3737e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077318"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql---hyperscale-citus-preview"></a>PostgreSQL-extensies in Azure Database for PostgreSQL - grootschalige (Citus) (preview)

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
> | [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Biedt een niet-hoofdlettergevoelige tekenreeks tekentype. |
> | [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Biedt een gegevenstype voor multidimensionale kubussen. |
> | [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Biedt een gegevenstype voor het opslaan van sets met sleutel/waarde-paren. |
> | [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | Biedt gegevenstypen voor internationale standaarden nummering-product. |
> | [lo](https://www.postgresql.org/docs/current/lo.html) | Groot objectonderhoud. |
> | [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Biedt een gegevenstype voor hiërarchische boomstructuren. |
> | [seg](https://www.postgresql.org/docs/current/seg.html) | Het gegevenstype voor lijnsegmenten of getal met drijvende komma intervallen. |
> | [topn](https://github.com/citusdata/postgresql-topn/) | Typ voor top-n JSONB. |

### <a name="functions-extensions"></a>Functions-extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [autoinc](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | Functies voor autoincrementing velden. |
> | [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Biedt een manier om te berekenen great cirkel afstanden op het oppervlak van de aarde. |
> | [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Biedt verschillende functies om te bepalen van overeenkomsten en de afstand tussen de tekenreeksen. |
> | [invoegen\_gebruikersnaam](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | Functies voor het bijhouden van wie een tabel heeft gewijzigd. |
> | [intagg](https://www.postgresql.org/docs/current/intagg.html) | Geheel getal aggregator en enumerator (verouderd). |
> | [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Biedt de functies en operatoren voor het manipuleren van null-free-matrices met gehele getallen. |
> | [moddatetime](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | Functies voor het bijhouden van de tijd van laatste wijziging. |
> | [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Biedt cryptografische functies. |
> | [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Gepartitioneerde tabellen worden beheerd door de tijd of -ID. |
> | [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Biedt de functies en operatoren voor het bepalen van de overeenkomsten van alfanumerieke tekst op basis van trigram overeenkomst. |
> | [refint](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | Functies voor het implementeren van referentiële integriteit aannemen (verouderd). |
> | sessie\_analytics | Functies voor het uitvoeren van query's hstore matrices. |
> | [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Biedt functies die hele tabellen, inclusief een kruistabel bewerken. |
> | [tcn](https://www.postgresql.org/docs/current/tcn.html) | Meldingen voor wachtwoordwijzigingen wordt geactiveerd. |
> | [timetravel](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | Functies voor het implementeren van tijd reizen. |
> | [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Genereert universele, unieke id (UUID's). |

### <a name="full-text-search-extensions"></a>Extensies zoeken in volledige tekst

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Biedt een sjabloon zoeken woordenlijst voor gehele getallen. |
> | [dict\_xsyn](https://www.postgresql.org/docs/current/dict-xsyn.html) | Tekst zoeken woordenlijst sjabloon voor de verwerking van uitgebreide synoniem. |
> | [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Een woordenlijst van de tekst zoeken waarmee accenten (diakritische tekens) uit lexemes worden verwijderd. |

### <a name="index-types-extensions"></a>Index typen extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [waas](https://www.postgresql.org/docs/current/bloom.html) | Toegangsmethode waas - de handtekening op basis van de index. |
> | [bTree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Voorbeeld GIN operator klassen die B-tree, zoals gedrag voor bepaalde gegevenstypen implementeren bevat. |
> | [bTree\_basisvertalingen](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Biedt basisvertalingen index operator klassen die B-tree implementeren. |

### <a name="language-extensions"></a>Taalextensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL procedurele taal kan worden geladen. |

### <a name="hyperscale-extensions"></a>Zeer grootschalige extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [citus](https://github.com/citusdata/citus) | Citus gedistribueerde database. |
> | shard\_rebalancer | Veilig opnieuw verdelen gegevens in de servergroep van een in het geval van knooppunt toevoegen of verwijderen. |

### <a name="miscellaneous-extensions"></a>Overige extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [adminpack](https://www.postgresql.org/docs/current/adminpack.html) | Beheerfuncties voor PostgreSQL. |
> | [amcheck](https://www.postgresql.org/docs/current/amcheck.html) | Functies voor het controleren van de integriteit van de relatie. |
> | [bestand\_fdw](https://www.postgresql.org/docs/current/file-fdw.html) | Afwijkend-data-wrapper voor toegang tot het platte bestand. |
> | [pageinspect](https://www.postgresql.org/docs/current/pageinspect.html) | De inhoud van de pagina's van een database op een laag niveau controleren. |
> | [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Biedt een manier voor het onderzoeken wat er gebeurt in de buffercache gedeelde in realtime. |
> | [pg\_cron](https://github.com/citusdata/pg_cron) | Taakplanner voor PostgreSQL. |
> | [pg\_freespacemap](https://www.postgresql.org/docs/current/pgfreespacemap.html) | Bekijk de beschikbare ruimte-kaart (FSM). |
> | [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Biedt een manier om de relatie gegevens laden in de buffercache. |
> | [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Biedt een manier voor het bijhouden van Uitvoeringsstatistieken van alle SQL-instructies uitvoeren op een server. (Zie hieronder voor een notitie van deze extensie). |
> | [PG\_zichtbaarheid](https://www.postgresql.org/docs/current/pgvisibility.html) | Controleer de zichtbaarheid van kaart (VM) en de zichtbaarheid op paginaniveau info. |
> | [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Biedt een manier voor het weergeven van beveiliging op rijniveau vergrendelen informatie. |
> | [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Biedt een manier voor het weergeven van statistieken van de tuple op serverniveau. |
> | [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Extern-data-wrapper gebruikt voor toegang tot gegevens die zijn opgeslagen in externe PostgreSQL-servers. (Zie hieronder voor een notitie van deze extensie).|
> | [sslinfo](https://www.postgresql.org/docs/current/sslinfo.html) | Informatie over SSL-certificaten. |
> | [TSM\_system\_rijen](https://www.postgresql.org/docs/current/tsm-system-rows.html) | Methode TABLESAMPLE dat aantal rijen als een limiet accepteert. |
> | [TSM\_system\_tijd](https://www.postgresql.org/docs/current/tsm-system-time.html) | TABLESAMPLE methode waarmee de tijd in milliseconden als een limiet accepteert. |
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | Biedt een manier voor het maken van hypothetische indexen die geen CPU of schijf kosten. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Een module die ondersteuning biedt voor verbindingen met andere PostgreSQL-databases uit in een databasesessie. (Zie hieronder voor een notitie van deze extensie). |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | XPath-query's en XSLT. |


### <a name="postgis-extensions"></a>PostGIS-extensies

> [!div class="mx-tableFixed"]
> | **Extensie** | **Beschrijving** |
> |---|---|
> | [PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal | Ruimtelijke en geografische objecten voor PostgreSQL. |
> | adres\_standardizer, adres\_standardizer\_gegevens\_ons | Gebruikt voor het parseren van een adres naar de bijbehorende elementen. Gebruikt ter ondersteuning van geocodering adres normalisering stap. |
> | postgis\_sfcgal | PostGIS SFCGAL functies. |
> | postgis\_tiger\_geocoder | PostGIS tiger geocoder en omgekeerde geocoder. |
> | postgis\_topologie | PostGIS topologie ruimtelijke gegevenstypen en -functies. |


## <a name="pgstatstatements"></a>pg_stat_statements
De [pg\_stat\_instructies extensie](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) is vooraf geladen op elke Azure Database for PostgreSQL-server zodat u kunt een manier voor het bijhouden van Uitvoeringsstatistieken van SQL-instructies.
De instelling `pg_stat_statements.track`, die bepaalt welke instructies zijn geteld op basis van de extensie, wordt standaard ingesteld op `top`, wat betekent dat alle instructies uitgegeven rechtstreeks door clients worden bijgehouden. De twee andere traceringsniveaus zijn `none` en `all`. Deze instelling kan worden geconfigureerd als een serverparameter via de [Azure-portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) of de [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Er is een verschil tussen de querygegevens kan worden uitgevoerd als pg_stat_statements biedt en de impact op de prestaties van de server als deze zich aanmeldt met elke SQL-instructie. Als u de extensie pg_stat_statements niet actief gebruikt, is het raadzaam dat u instelt `pg_stat_statements.track` naar `none`. Houd er rekening mee dat sommige van derden bewaken van services mogelijk afhankelijk van pg_stat_statements query prestatie-inzichten leveren, dus controleer of dit het geval is voor u is of niet.

## <a name="dblink-and-postgresfdw"></a>dblink en postgres_fdw
dblink en postgres_fdw kunt u verbinding maken vanaf een PostgreSQL-server naar een andere of naar een andere database op dezelfde server. U moet de ontvangende server verbindingen toestaan vanaf de server verzenden via de firewall. Wanneer u deze uitbreidingen te verbinden met Azure Database for PostgreSQL-servers, kan dit worden gedaan door in te stellen 'Toegang tot Azure-services toestaan' op ON. Dit is ook nodig als u de extensies wilt aan lus terug naar dezelfde server gebruiken. De instelling 'Toegang tot Azure-services toestaan' vindt u in de Azure portal-pagina voor de Postgres-server, onder de beveiliging van de verbinding. 'Toestaan toegang tot Azure-services' op accounttoewijzing alle Azure-IP-adressen te schakelen.

Uitgaande verbindingen van Azure Database for PostgreSQL worden op dit moment niet ondersteund, met uitzondering van verbindingen met andere Azure-Database voor PostgreSQL-servers.

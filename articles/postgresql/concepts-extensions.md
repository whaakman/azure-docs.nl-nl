---
title: PostgreSQL-extensies gebruiken in Azure Database for PostgreSQL
description: Beschrijft de mogelijkheid om uit te breiden de functionaliteit van uw database met behulp van uitbreidingen in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/30/2018
ms.openlocfilehash: f9216800f0e91d71f1da3f2bac16ecfcbf8b4850
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376547"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-extensies in Azure Database for PostgreSQL
PostgreSQL biedt de mogelijkheid om uit te breiden de functionaliteit van uw database met behulp van extensies. Extensies bieden samen bundeling van meerdere verwante SQL-objecten in één pakket die kan worden geladen of verwijderd uit de database met slechts één opdracht. Extensies kunnen na worden geladen in de database, functioneren als ingebouwde functies. Zie voor meer informatie over de PostgreSQL-extensies, [verpakking verwante objecten in een extensie](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Het gebruik van de PostgreSQL-extensies
PostgreSQL-extensies moeten worden geïnstalleerd in uw database voordat u ze kunt gebruiken. Voer voor het installeren van een bepaalde extensie de [EXTENSIE maken](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) opdracht uit vanaf de psql-hulpprogramma voor het laden van de verpakte objecten in uw database.

Azure Database voor PostgreSQL ondersteunt momenteel een subset van de belangrijkste extensies zoals hieronder vermeld. Extensies dan de waarden worden niet ondersteund. u kunt uw eigen uitbreiding maken met Azure Database voor PostgreSQL-service.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>-Extensies ondersteund door Azure Database for PostgreSQL
De volgende tabellen worden de standaard PostgreSQL-extensies die momenteel worden ondersteund door Azure Database voor PostgreSQL. Deze informatie is ook beschikbaar door uit te voeren `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Gegevenstypen extensies

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Biedt een gegevenstype voor wachtwoorden automatisch versleuteld. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Biedt een niet-hoofdlettergevoelige tekenreeks tekentype. |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Biedt een gegevenstype voor multidimensionale kubussen. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Biedt een gegevenstype voor het opslaan van sets met sleutel/waarde-paren. |
| [is](https://www.postgresql.org/docs/9.6/static/isn.html) | Biedt gegevenstypen voor internationale standaarden nummering-product. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Biedt een gegevenstype voor hiërarchische boomstructuren. |

### <a name="functions-extensions"></a>Functions-extensies

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Biedt een manier om te berekenen great cirkel afstanden op het oppervlak van de aarde. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Biedt verschillende functies om te bepalen van overeenkomsten en de afstand tussen de tekenreeksen. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Biedt de functies en operatoren voor het manipuleren van null-free-matrices met gehele getallen. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Biedt cryptografische functies. |
| [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Gepartitioneerde tabellen worden beheerd door de tijd of -ID. |
| [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Biedt de functies en operatoren voor het bepalen van de overeenkomsten van alfanumerieke tekst op basis van trigram overeenkomst. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Biedt functies die hele tabellen, inclusief een kruistabel bewerken. |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Genereert universele, unieke id (UUID's). |

### <a name="full-text-search-extensions"></a>Extensies zoeken in volledige tekst

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [Dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Biedt een sjabloon zoeken woordenlijst voor gehele getallen. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Een woordenlijst van de tekst zoeken waarmee accenten (diakritische tekens) uit lexemes worden verwijderd. |

### <a name="index-types-extensions"></a>Index typen extensies

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [bTree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Voorbeeld GIN operator klassen die B-tree, zoals gedrag voor bepaalde gegevenstypen implementeren bevat. |
| [bTree\_basisvertalingen](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Biedt basisvertalingen index operator klassen die B-tree implementeren. |

### <a name="language-extensions"></a>Taalextensies

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL procedurele taal kan worden geladen. |

### <a name="miscellaneous-extensions"></a>Overige extensies

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Biedt een manier voor het onderzoeken wat er gebeurt in de buffercache gedeelde in realtime. |
| [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Biedt een manier om de relatie gegevens laden in de buffercache. |
| [PG\_stat\_instructies](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Biedt een manier voor het bijhouden van Uitvoeringsstatistieken van alle SQL-instructies uitvoeren op een server. (Zie hieronder voor een notitie van deze extensie). |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Biedt een manier voor het weergeven van beveiliging op rijniveau vergrendelen informatie. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Biedt een manier voor het weergeven van statistieken van de tuple op serverniveau. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Extern-data-wrapper gebruikt voor toegang tot gegevens die zijn opgeslagen in externe PostgreSQL-servers. |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | Biedt een manier voor het maken van hypothetische indexen die geen CPU of schijf kosten. |
| [plv8](https://plv8.github.io/) | Een Javascript-taal-extensie voor PostgreSQL die kan worden gebruikt voor opgeslagen procedures, triggers, enzovoort. |

### <a name="postgis-extensions"></a>PostGIS-extensies

> [!div class="mx-tableFixed"]
| **Extensie** | **Beschrijving** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal | Ruimtelijke en geografische objecten voor PostgreSQL. |
| adres\_standardizer, adres\_standardizer\_gegevens\_ons | Gebruikt voor het parseren van een adres naar de bijbehorende elementen. Gebruikt ter ondersteuning van geocodering adres normalisering stap. |
| [pgrouting](http://pgrouting.org/) | Breidt de PostGIS / PostgreSQL georuimtelijke database voor georuimtelijke functies routering. |


### <a name="using-pgstatstatements"></a>Met behulp van pg_stat_statements
De [pg\_stat\_instructies extensie](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) is vooraf geladen op elke Azure Database for PostgreSQL-server zodat u kunt een manier voor het bijhouden van Uitvoeringsstatistieken van SQL-instructies.
De instelling `pg_stat_statements.track`, die bepaalt welke instructies zijn geteld op basis van de extensie, wordt standaard ingesteld op `top`, wat betekent dat alle instructies uitgegeven rechtstreeks door clients worden bijgehouden. De twee andere traceringsniveaus zijn `none` en `all`. Deze instelling kan worden geconfigureerd als een serverparameter via de [Azure-portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) of de [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Er is een verschil tussen de querygegevens kan worden uitgevoerd als pg_stat_statements biedt en de impact op de prestaties van de server als deze zich aanmeldt met elke SQL-instructie. Als u de extensie pg_stat_statements niet actief gebruikt, is het raadzaam dat u instelt `pg_stat_statements.track` naar `none`. Houd er rekening mee dat sommige van derden bewaken van services mogelijk afhankelijk van pg_stat_statements query prestatie-inzichten leveren, dus controleer of dit het geval is voor u is of niet.


## <a name="next-steps"></a>Volgende stappen
Als u een extensie die u wilt gebruiken, laat het ons weten niet ziet. Stem voor bestaande aanvragen of maken van nieuwe feedback en verzoeken in onze [forum met feedback van klanten](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

---
title: PostgreSQL-extensies in de Azure-Database voor PostgreSQL gebruiken
description: Beschrijft de mogelijkheid om uit te breiden de functionaliteit van de extensies in de Azure-Database gebruiken voor PostgreSQL-database.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/30/2018
ms.openlocfilehash: c1e541450103c0c30854c027c5dc39f3b94cdafd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640571"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-uitbreidingen in een Azure-Database voor PostgreSQL
PostgreSQL biedt de mogelijkheid om uit te breiden de functionaliteit van de database met de extensies. Extensies kunnen voor bundeling van meerdere verwante objecten in de SQL samen in één pakket die kan worden geladen of verwijderd uit de database met één opdracht. Extensies kunnen na wordt geladen in de database, functioneren als ingebouwde functies. Zie voor meer informatie over PostgreSQL extensies [verpakking verwante objecten in een uitbreiding](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Het gebruik van PostgreSQL-extensies
PostgreSQL-extensies moeten worden geïnstalleerd in uw database voordat u ze kunt gebruiken. U installeert een bepaalde extensie door de [EXTENSIE maken](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) opdracht vanuit psql hulpprogramma laden van de ingepakte objecten in uw database.

Azure PostgreSQL-Database ondersteunt momenteel een subset van de belangrijkste extensies zoals hieronder weergegeven. Extensies afgezien van de programma's worden niet ondersteund. u kunt uw eigen uitbreiding met Azure-Database maken voor PostgreSQL-service.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Uitbreidingen voor PostgreSQL wordt ondersteund door Azure Database
De volgende tabellen worden de standaard PostgreSQL-uitbreidingen die momenteel worden ondersteund door Azure-Database voor PostgreSQL. Deze informatie is ook beschikbaar door het uitvoeren van `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Gegevens typen uitbreidingen

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Biedt een gegevenstype voor wachtwoorden automatisch versleuteld. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Bevat een niet-hoofdlettergevoelige teken tekenreekstype. |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Biedt een gegevenstype voor multidimensionale kubussen. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Biedt een gegevenstype voor het opslaan van sets van sleutel-waardeparen. |
| [niet](https://www.postgresql.org/docs/9.6/static/isn.html) | Biedt gegevenstypen voor internationale product nummering standaarden. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Biedt een gegevenstype voor hiërarchische boomstructuren. |

### <a name="functions-extensions"></a>Uitbreidingen van functies

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Biedt een manier voor het berekenen van groot cirkel afstanden op het vlak van de aarde. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Biedt verschillende functies om te bepalen van overeenkomsten en de afstand tussen tekenreeksen. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Biedt de functies en operators voor het manipuleren van null gratis matrices van gehele getallen. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Biedt cryptografische functies. |
| [PG\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Gepartitioneerde tabellen wordt beheerd door de time- of -ID. |
| [PG\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Functies en operatoren biedt voor het bepalen van de overeenkomsten van alfanumerieke tekst op basis van overeenkomst trigram. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Biedt de functies die hele tabellen, inclusief kruistabel bewerken. |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Genereert Universeel unieke id's (UUID's). |

### <a name="full-text-search-extensions"></a>Zoekopdracht in volledige tekst-extensies

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [Dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Biedt een sjabloon van tekst zoeken woordenlijst voor gehele getallen. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Een woordenlijst van de tekst zoeken waarmee accenten (diakritische tekens) uit lexemes worden verwijderd. |

### <a name="index-types-extensions"></a>Index typen uitbreidingen

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [bTree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Voorbeeld GIN operator klassen die B-tree zoals gedrag voor bepaalde gegevenstypen implementeren biedt. |
| [bTree\_basisvertalingen](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Biedt basisvertalingen index operator klassen die B-tree implementeren. |

### <a name="language-extensions"></a>Taal-extensies

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL geladen procedurele taal. |

### <a name="miscellaneous-extensions"></a>Diverse uitbreidingen

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Biedt een manier voor het onderzoeken van wat er gebeurt in de cache gedeelde buffer in realtime. |
| [PG\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Biedt een manier relatie om gegevens te laden in het cachegeheugen van de buffer. |
| [PG\_stat\_instructies](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Biedt een manier voor het bijhouden van Uitvoeringsstatistieken van alle SQL-instructies uitgevoerd door een server. (Zie hieronder voor een opmerking op voor deze extensie). |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Biedt een manier voor het weergeven van rijniveau vergrendelingsfout informatie. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Biedt een manier voor het weergeven van statistieken tuple niveau. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Afwijkend gegevens wrapper gebruikt voor toegang tot gegevens die zijn opgeslagen in externe PostgreSQL-servers. |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | Biedt een manier voor het maken van hypothetische indexen die geen CPU of schijf kosten. |

### <a name="postgis-extensions"></a>PostGIS-extensies

> [!div class="mx-tableFixed"]
| **De extensie** | **Beschrijving** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal | Ruimtelijke en geografische objecten voor PostgreSQL. |
| adres\_standardizer, adres\_standardizer\_gegevens\_ons | Gebruikt bij het parseren van een adres in de elementen. Gebruikt ter ondersteuning van geocodering adres normalisatie stap. |
| [pgrouting](http://pgrouting.org/) | Breidt de PostGIS / PostgreSQL georuimtelijke database een georuimtelijke routering functionaliteit. |


### <a name="using-pgstatstatements"></a>Met behulp van pg_stat_statements
De [pg\_stat\_instructies extensie](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) is geladen in elke Azure-Database voor PostgreSQL server u een manier voor het bijhouden van Uitvoeringsstatistieken van SQL-instructies te bieden.
De instelling `pg_stat_statements.track`, die bepaalt welke instructies worden geteld door de extensie, wordt standaard ingesteld op `top`, wat betekent dat alle instructies die zijn uitgegeven door clients rechtstreeks worden bijgehouden. De twee andere traceringsniveaus zijn `none` en `all`. Deze instelling kan worden geconfigureerd als een serverparameter via de [Azure-portal](https://docs.microsoft.com/en-us/azure/postgresql/howto-configure-server-parameters-using-portal) of de [Azure CLI](https://docs.microsoft.com/en-us/azure/postgresql/howto-configure-server-parameters-using-cli).

Er is een afweging tussen de uitvoering querygegevens pg_stat_statements biedt en de impact op de prestaties van de server omdat deze zich aanmeldt met elke SQL-instructie. Als u de extensie pg_stat_statements niet actief gebruikt, raden wij aan dat u `pg_stat_statements.track` naar `none`. Houd er rekening mee dat sommige derde partij die services controleren zijn mogelijk afhankelijk van pg_stat_statements te leveren inzichten voor queryprestaties, dus controleer of dit voor u het geval is, of niet.


## <a name="next-steps"></a>Volgende stappen
Als u een extensie die u wilt gebruiken, laat ons weten niet ziet. Stemmen voor bestaande aanvragen of maak nieuwe feedback en aanvragen in onze [forum met feedback van klanten](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

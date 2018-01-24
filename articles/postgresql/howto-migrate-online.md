---
title: Minimale downtime migratie naar Azure-Database voor PostgreSQL | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een minimale downtime migratie uitvoert door een PostgreSQL-database in te pakken op een bestand, de PostgreSQL-database terugzetten vanuit een archiefbestand door pg_dump in Azure-Database gemaakt voor PostgreSQL en instellen van de eerste keer wordt geladen en continue synchroniseren van gegevens uit de brondatabase met de doeldatabase met behulp van Attunity repliceren voor Microsoft Migrations.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: efbd4f227880875c11e2c43c84716dfc49c5717d
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Minimale downtime migratie naar Azure-Database voor PostgreSQL
U kunt uw bestaande PostgreSQL-database migreren naar Azure-Database voor PostgreSQL door Attunity repliceren voor Microsoft Migrations. Attunity repliceren is een gezamenlijke aanbieding van Attunity en Microsoft. Samen met de Azure-databaseservice migratie is het opgenomen zonder extra kosten voor klanten van Microsoft. 

Attunity repliceren helpt uitvaltijd tijdens migraties van de database en het zorgt ervoor dat de brondatabase operationeel tijdens het proces.

Attunity repliceren is een hulpprogramma voor de data replication waarmee het synchroniseren van gegevens tussen verschillende bronnen en -doelen. Dit geeft het script voor het schema maken en gegevens die zijn gekoppeld aan elke databasetabel. Attunity repliceren wordt niet doorgegeven eventuele andere artefacten (zoals SP, triggers, functies, enzovoort) of converteren, bijvoorbeeld, de PL-SQL-code die wordt gehost in een dergelijke artefacten met T-SQL.

> [!NOTE]
> Hoewel Attunity repliceren een uitgebreide reeks migratiescenario's ondersteunt, ligt de nadruk op ondersteuning voor een specifieke subset van de bron/doel-paren.

Een overzicht van het proces voor het uitvoeren van een migratie met minimale downtime omvat:

* **Migreren van het schema van de bron PostgreSQL** voor een Azure-Database voor PostgreSQL-database met behulp van de [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) opdracht met de parameter - n en vervolgens via de [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) opdracht.

* **Instellen van de initiële belasting en synchroniseren van doorlopende gegevens uit de brondatabase voor de doeldatabase** door Attunity repliceren voor Microsoft Migrations. Hierdoor minimaliseert de tijd die de brondatabase moet worden ingesteld als alleen-lezen tijdens het voorbereiden van uw toepassingen en de doeldatabase PostgreSQL overschakelen op Azure.

Zie voor meer informatie over de repliceren Attunity voor Microsoft Migrations biedt de volgende bronnen:
 - Ga naar de [Attunity repliceren voor Microsoft Migrations](https://aka.ms/attunity-replicate) webpagina.
 - Download [Attunity repliceren voor Microsoft-migraties](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Ga naar de [Attunity repliceren Community](https://aka.ms/attunity-community/) voor een introductiehandleiding, zelfstudies en ondersteuning.
 - Zie voor stapsgewijze instructies over het gebruik van Attunity repliceren PostgreSQL migreren naar Azure-Database voor PostgreSQL de [Database Migratiehandleiding](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).
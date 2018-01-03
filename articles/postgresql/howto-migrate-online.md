---
title: Online migratie naar Azure-Database voor PostgreSQL | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een online migratie uitvoeren door te pakken een PostgreSQL-database op een dumpbestand PostgreSQL-database terugzetten vanuit een archiefbestand gemaakt door pg_dump in Azure-Database voor PostgreSQL en het instellen van de eerste keer wordt geladen en continue synchroniseren van gegevens uit de brondatabase met de doeldatabase met behulp van Attunity repliceren voor Microsoft Migrations.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: cf17ad87b33ed6196d86cebaf6efe699a542bf39
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="online-migration-to-azure-database-for-postgresql"></a>Online migratie naar Azure-Database voor PostgreSQL
U kunt uw bestaande PostgreSQL-database met Azure-Database voor PostgreSQL migreren door middel van Attunity repliceren voor Microsoft Migrations, een aanbieding CO gesponsorde, gezamenlijke van Attunity en van Microsoft die is opgegeven samen met de Azure-Service voor het migreren van Database Er zijn geen extra kosten voor klanten van Microsoft. Attunity repliceren voor Microsoft Migrations kunnen ook minimale downtime bij databasemigratie en de brondatabase blijft operationeel zijn tijdens de migratie.

Attunity repliceren is een hulpprogramma voor de data replication waarmee het synchroniseren van gegevens tussen verschillende bronnen en doelen, doorgeven van het script voor het schema maken en gegevens die zijn gekoppeld aan elke databasetabel. Alle andere artefacten (zoals SP, triggers, functies, enzovoort) wordt niet doorgegeven door Attunity repliceren of convert, bijvoorbeeld PL/SQL code gehost in een dergelijke artefacten, met T-SQL.

> [!NOTE]
> Terwijl Attunity repliceren een uitgebreide reeks migratiescenario's ondersteunt, worden Attunity repliceren voor Microsoft Migrations is gericht op ondersteuning voor een specifieke subset van de bron/doel-paren.

Een overzicht van het proces voor het uitvoeren van een online migratie omvat het volgende:

1. **Migreren van het schema van de bron PostgreSQL** voor Azure-Database voor PostgreSQL met behulp van [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) en [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) met de parameter - n.

2. **Instellen van initiële laden en synchroniseren van doorlopende gegevens uit de brondatabase naar de doeldatabase** door Attunity repliceren voor Microsoft Migrations. Hierdoor minimaliseert zodat de tijd die de brondatabase worden ingesteld als alleen-lezen moet terwijl u voorbereiden overschakelen van uw toepassingen aan de beoogde PostgreSQL-database in Azure.

Voor meer informatie over de repliceren Attunity voor Microsoft Migrations aanbieding weergeven van de volgende bronnen:
 - De repliceren Attunity voor Microsoft Migrations [webpagina](https://aka.ms/attunity-replicate).
 - [Download](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Attunity repliceren voor Microsoft-migraties.
 - Raadpleeg voor stapsgewijze instructies over het gebruik van Attunity PostgreSQL migreren naar Azure-Database voor PostgreSQL de [Database Migratiehandleiding](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).
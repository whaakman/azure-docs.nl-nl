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
ms.openlocfilehash: 8c98c58042e7f1d1726eaad6ee03d1531b6c910e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Minimale downtime migratie naar Azure-Database voor PostgreSQL
U kunt uw bestaande PostgreSQL-database met Azure-Database voor PostgreSQL migreren door middel van Attunity repliceren voor Microsoft Migrations, een aanbieding CO gesponsorde, gezamenlijke van Attunity en van Microsoft die is opgegeven samen met de Azure-Service voor het migreren van Database Er zijn geen extra kosten voor klanten van Microsoft. Attunity repliceren voor Microsoft Migrations kunnen ook minimale downtime bij databasemigratie en de brondatabase blijft operationeel zijn tijdens de migratie.

Attunity repliceren is een hulpprogramma voor de data replication waarmee het synchroniseren van gegevens tussen verschillende bronnen en doelen, doorgeven van het script voor het schema maken en gegevens die zijn gekoppeld aan elke databasetabel. Alle andere artefacten (zoals SP, triggers, functies, enzovoort) wordt niet doorgegeven door Attunity repliceren of convert, bijvoorbeeld PL/SQL code gehost in een dergelijke artefacten, met T-SQL.

> [!NOTE]
> Terwijl Attunity repliceren een uitgebreide reeks migratiescenario's ondersteunt, worden Attunity repliceren voor Microsoft Migrations is gericht op ondersteuning voor een specifieke subset van de bron/doel-paren.

Een overzicht van het proces voor het uitvoeren van een migratie met minimale downtime omvat:

1. **Migreren van het schema van de bron PostgreSQL** voor een Azure-Database voor PostgreSQL-database met behulp van de [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) opdracht met de parameter - n en vervolgens via de [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) opdracht.

2. **Instellen van de initiële belasting en synchroniseren van doorlopende gegevens uit de brondatabase voor de doeldatabase** door Attunity repliceren voor Microsoft Migrations. Hierdoor minimaliseert de tijd die de brondatabase moet worden ingesteld als alleen-lezen tijdens het voorbereiden van uw toepassingen en de doeldatabase PostgreSQL overschakelen op Azure.

Voor meer informatie over de repliceren Attunity voor Microsoft Migrations aanbieding weergeven van de volgende bronnen:
 - De repliceren Attunity voor Microsoft Migrations [webpagina](https://aka.ms/attunity-replicate).
 - [Download](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Attunity repliceren voor Microsoft-migraties.
 - De repliceren Attunity [Community](https://microsoft.attunity.com/), met een introductiehandleiding, zelfstudies en ondersteuning.
 - Raadpleeg voor stapsgewijze instructies over het gebruik van Attunity PostgreSQL migreren naar Azure-Database voor PostgreSQL de [Database Migratiehandleiding](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).
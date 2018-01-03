---
title: Online migratie naar Azure-Database voor MySQL | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een online migratie van een MySQL-database naar Azure-Database voor MySQL uitvoert en het instellen van de eerste belasting en synchroniseren van doorlopende gegevens uit de brondatabase naar de doeldatabase Attunity repliceren met voor Microsoft Migrations.
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: e3a30510e1b3070bc320faa6071cf546a2024e2f
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="online-migration-to-azure-database-for-mysql"></a>Online migratie naar Azure-Database voor MySQL
U kunt uw bestaande MySQL-database met Azure-Database voor MySQL migreren door middel van Attunity repliceren voor Microsoft Migrations, een aanbieding CO gesponsorde, gezamenlijke van Attunity en van Microsoft die is opgegeven samen met de Service Azure Database migratie zonder extra kosten voor Microsoft-klanten. Attunity repliceren voor Microsoft Migrations kunnen ook minimale downtime bij databasemigratie en de brondatabase blijft operationeel zijn tijdens de migratie.

Attunity repliceren is een hulpprogramma voor de data replication waarmee het synchroniseren van gegevens tussen verschillende bronnen en doelen, doorgeven van het script voor het schema maken en gegevens die zijn gekoppeld aan elke databasetabel. Alle andere artefacten (zoals SP, triggers, functies, enzovoort) wordt niet doorgegeven door Attunity repliceren of convert, bijvoorbeeld PL/SQL code gehost in een dergelijke artefacten, met T-SQL.

> [!NOTE]
> Terwijl Attunity repliceren een uitgebreide reeks migratiescenario's ondersteunt, worden Attunity repliceren voor Microsoft Migrations is gericht op ondersteuning voor een specifieke subset van de bron/doel-paren.

Een overzicht van het proces voor het uitvoeren van een online migratie omvat het volgende:

1. **Migreren van het schema van de bron MySQL** voor Azure-Database voor MySQL met behulp van [MySQL Workbench](https://www.mysql.com/products/workbench/).

2. **Instellen van initiële laden en synchroniseren van doorlopende gegevens uit de brondatabase naar de doeldatabase** door Attunity repliceren voor Microsoft Migrations. Hierdoor minimaliseert zodat de tijd die de brondatabase worden ingesteld als alleen-lezen moet terwijl u voorbereiden overschakelen van uw toepassingen aan de doel-MySQL-database in Azure.

Voor meer informatie over de repliceren Attunity voor Microsoft Migrations aanbieding weergeven van de volgende bronnen:
 - De repliceren Attunity voor Microsoft Migrations [webpagina](https://aka.ms/attunity-replicate).
 - [Download](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Attunity repliceren voor Microsoft-migraties.
 - Raadpleeg voor stapsgewijze instructies over het gebruik van Attunity MySQL migreren naar Azure-Database voor MySQL de [Database Migratiehandleiding](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).
---
title: Minimale downtime migratie naar Azure-Database voor MySQL | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een minimale downtime migratie van een MySQL-database naar Azure-Database voor MySQL uitvoert en het instellen van de eerste belasting en synchroniseren van doorlopende gegevens uit de brondatabase naar de doeldatabase Attunity repliceren met voor Microsoft Migraties.
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 429f5c9f939a802184a6513a63fb9115abf4b235
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Minimale downtime migratie naar Azure-Database voor MySQL
U kunt uw bestaande MySQL-database met Azure-Database voor MySQL migreren door middel van Attunity repliceren voor Microsoft Migrations, een aanbieding CO gesponsorde, gezamenlijke van Attunity en van Microsoft die is opgegeven samen met de Service Azure Database migratie zonder extra kosten voor Microsoft-klanten. Attunity repliceren voor Microsoft Migrations kunnen ook minimale downtime bij databasemigratie en de brondatabase blijft operationeel zijn tijdens de migratie.

Attunity repliceren is een hulpprogramma voor de data replication waarmee het synchroniseren van gegevens tussen verschillende bronnen en doelen, doorgeven van het script voor het schema maken en gegevens die zijn gekoppeld aan elke databasetabel. Alle andere artefacten (zoals SP, triggers, functies, enzovoort) wordt niet doorgegeven door Attunity repliceren of convert, bijvoorbeeld PL/SQL code gehost in een dergelijke artefacten, met T-SQL.

> [!NOTE]
> Terwijl Attunity repliceren een uitgebreide reeks migratiescenario's ondersteunt, worden Attunity repliceren voor Microsoft Migrations is gericht op ondersteuning voor een specifieke subset van de bron/doel-paren.

Een overzicht van het proces voor het uitvoeren van een migratie met minimale downtime omvat:

1. **Migreren van het schema van de bron MySQL** voor een Azure-Database voor de MySQL-database met behulp van de [MySQL Workbench](https://www.mysql.com/products/workbench/).

2. **Instellen van de initiële belasting en synchroniseren van doorlopende gegevens uit de brondatabase voor de doeldatabase** door Attunity repliceren voor Microsoft Migrations. Hierdoor minimaliseert de tijd die de brondatabase moet worden ingesteld als alleen-lezen tijdens het voorbereiden van het overschakelen van uw toepassingen aan de doel-MySQL-database in Azure.

Voor meer informatie over de repliceren Attunity voor Microsoft Migrations aanbieding weergeven van de volgende bronnen:
 - De repliceren Attunity voor Microsoft Migrations [webpagina](https://aka.ms/attunity-replicate).
 - [Download](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Attunity repliceren voor Microsoft-migraties.
 - De repliceren Attunity [Community](https://microsoft.attunity.com/), met een introductiehandleiding, zelfstudies en ondersteuning.
 - Raadpleeg voor stapsgewijze instructies over het gebruik van Attunity MySQL migreren naar Azure-Database voor MySQL de [Database Migratiehandleiding](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).
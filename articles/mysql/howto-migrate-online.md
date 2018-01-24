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
ms.openlocfilehash: d23628fd8446f6e7e0e5ed14b98da13c09b2d592
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Minimale downtime migratie naar Azure-Database voor MySQL
U kunt uw bestaande MySQL-database migreren naar Azure-Database voor MySQL Attunity repliceren met voor Microsoft Migrations. Attunity repliceren is een gezamenlijke aanbieding van Attunity en Microsoft. Samen met de Azure-databaseservice migratie is het opgenomen zonder extra kosten voor klanten van Microsoft. 

Attunity repliceren helpt uitvaltijd tijdens migraties van de database en het zorgt ervoor dat de brondatabase operationeel tijdens het proces.

Attunity repliceren is een hulpprogramma voor de data replication waarmee het synchroniseren van gegevens tussen verschillende bronnen en -doelen. Dit geeft het script voor het schema maken en gegevens die zijn gekoppeld aan elke databasetabel. Attunity repliceren wordt niet doorgegeven eventuele andere artefacten (zoals SP, triggers, functies, enzovoort) of converteren, bijvoorbeeld, de PL-SQL-code die wordt gehost in een dergelijke artefacten met T-SQL.

> [!NOTE]
> Hoewel Attunity repliceren een uitgebreide reeks migratiescenario's ondersteunt, ligt de nadruk op ondersteuning voor een specifieke subset van de bron/doel-paren.

Een overzicht van het proces voor het uitvoeren van een migratie met minimale downtime omvat:

* **Migreren van het schema van de bron MySQL** beheerd met een Azure-Database voor MySQL database-service met behulp van [MySQL Workbench](https://www.mysql.com/products/workbench/).

* **Instellen van de initiële belasting en synchroniseren van doorlopende gegevens uit de brondatabase voor de doeldatabase** door Attunity repliceren voor Microsoft Migrations. Hierdoor minimaliseert de tijd die de brondatabase moet worden ingesteld als alleen-lezen tijdens het voorbereiden van het overschakelen van uw toepassingen aan de doel-MySQL-database in Azure.

Zie voor meer informatie over de repliceren Attunity voor Microsoft Migrations biedt de volgende bronnen:
 - Ga naar de [Attunity repliceren voor Microsoft Migrations](https://aka.ms/attunity-replicate) webpagina.
 - Download [Attunity repliceren voor Microsoft-migraties](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Ga naar de [Attunity repliceren Community](https://aka.ms/attunity-community) voor een introductiehandleiding, zelfstudies en ondersteuning.
 - Zie voor stapsgewijze instructies over het gebruik van Attunity repliceren naar uw MySQL-database migreren naar Azure-Database voor MySQL de [Database Migratiehandleiding](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).
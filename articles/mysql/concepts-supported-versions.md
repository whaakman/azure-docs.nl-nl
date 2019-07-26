---
title: Ondersteunde versies in Azure Database for MySQL
description: Beschrijft de ondersteunde versies in Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 7/25/2019
ms.openlocfilehash: 3d4bab4558ebfd0f6031ef00a0b67bb0d5b61120
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501448"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Ondersteunde Azure Database for MySQL server versies

Azure Database for MySQL is ontwikkeld vanuit de [MySQL Community Edition](https://www.mysql.com/products/community/), met behulp van de InnoDB-engine.

MySQL maakt gebruik van het schema X. Y. Z. X is de primaire versie, Y de secundaire versie en Z de release van de fout correctie. Zie de [MySQL-documentatie](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)voor meer informatie over het schema.

> [!NOTE]
> In de service wordt een gateway gebruikt om de verbindingen met Server exemplaren om te leiden. Nadat de verbinding tot stand is gebracht, wordt in de MySQL-client de versie van MySQL-set in de gateway weer gegeven, niet de daad werkelijke versie die wordt uitgevoerd op het MySQL-Server exemplaar. Als u de versie van uw MySQL-Server exemplaar wilt bepalen `SELECT VERSION();` , gebruikt u de opdracht op de MySQL-prompt.

Azure Database for MySQL ondersteunt momenteel de volgende versies:

## <a name="mysql-version-56"></a>MySQL-versie 5,6

Release van de oplossing voor fouten: 5.6.42

Raadpleeg de opmerkingen bij de [versie](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-42.html) van MySQL voor meer informatie over verbeteringen en oplossingen in MySQL 5.6.42.

## <a name="mysql-version-57"></a>MySQL-versie 5,7

Release van de oplossing voor fouten: 5.7.24

Raadpleeg de opmerkingen bij de [versie](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-24.html) van MySQL voor meer informatie over verbeteringen en oplossingen in MySQL 5.7.24.

## <a name="mysql-version-80"></a>MySQL-versie 8,0

> [!NOTE]
> MySQL 8,0 is momenteel beschikbaar als preview-versie. Als MySQL 8,0 niet wordt weer geven in de Azure Portal, is de implementatie mogelijk niet voltooid in uw regio. 

Release van de oplossing voor fouten: 8.0.15

Raadpleeg de opmerkingen bij de [versie](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) van MySQL voor meer informatie over verbeteringen en oplossingen in MySQL 8.0.15.

## <a name="managing-updates-and-upgrades"></a>Updates en upgrades beheren
De service beheert automatisch patches voor het oplossen van problemen met versie-updates. Bijvoorbeeld 5.7.20 installeren naar 5.7.21.  

Upgrades van de secundaire en primaire versie worden momenteel niet ondersteund. Een upgrade van MySQL 5,6 naar MySQL 5,7 wordt bijvoorbeeld niet ondersteund. Als u een upgrade wilt uitvoeren van 5,6 naar 5,7, neemt u een [dump op en herstelt](./concepts-migrate-dump-restore.md) u deze op een server die is gemaakt met de nieuwe engine versie.

## <a name="next-steps"></a>Volgende stappen

Zie [service lagen](./concepts-pricing-tiers.md) voor informatie over specifieke resource quota en beperkingen op basis **van uw servicelaag**.

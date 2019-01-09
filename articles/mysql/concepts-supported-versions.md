---
title: Ondersteunde versies in Azure Database for MySQL
description: Beschrijft de ondersteunde versies in Azure Database voor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/07/2019
ms.openlocfilehash: ecd6466d8d7a7e4497d076ced0c9f2375d5dfb7f
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106032"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Ondersteunde Azure-Database voor MySQL-server-versies
Azure Database voor MySQL is ontwikkeld in [MySQL in Communityversie](https://www.mysql.com/products/community/), met behulp van de InnoDB-engine.

MySQL maakt gebruik van het naamgevingsschema X.Y.Z. X is de primaire versie, Y is de secundaire versie en Z is de versie opgelost probleem. Zie voor meer informatie over het schema, de [MySQL documentatie](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

Azure Database for MySQL ondersteunt momenteel de volgende versies:

## <a name="mysql-version-56"></a>MySQL-versie 5.6

Opgelost probleem release: 5.6.39

Raadpleeg de MySQL [opmerkingen bij de release](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) voor meer informatie over verbeteringen en oplossingen in MySQL 5.6.39.

## <a name="mysql-version-57"></a>MySQL-versie 5.7

Opgelost probleem release: 5.7.21

Raadpleeg de MySQL [opmerkingen bij de release](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) voor meer informatie over verbeteringen en oplossingen in MySQL 5.7.21.

> [!NOTE]
> Een gateway wordt gebruikt in de service, zodat de verbindingen worden omgeleid naar de server-exemplaren. Nadat de verbinding tot stand is gebracht, wordt de versie van MySQL instellen in de gateway, niet de daadwerkelijke versie die op uw exemplaar van de MySQL-server weergegeven in de MySQL-client. Als u wilt bepalen welke versie van uw MySQL-server-exemplaar, gebruikt u de `SELECT VERSION();` opdracht aan de MySQL-prompt.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
De service beheert automatisch patches voor foutoplossing versie-updates. Bijvoorbeeld: 5.7.20-5.7.21.  

Op dit moment worden niet kleine en grote versie-upgrades ondersteund. Bijvoorbeeld, wordt niet een upgrade van MySQL 5.6 naar MySQL 5.7 ondersteund. Als u upgraden van 5.6 naar 5.7 wilt, krijgen een [dump maken en terugzetten](./concepts-migrate-dump-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

## <a name="next-steps"></a>Volgende stappen

Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-pricing-tiers.md)

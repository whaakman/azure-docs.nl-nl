---
title: Ondersteunde versies in Azure Database for MySQL
description: Beschrijft de ondersteunde versies in Azure Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: f2a9348e267ad6a5929fda64cc08dcd837243e71
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093337"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Ondersteunde Azure-Database voor MySQL-server-versies
Azure Database voor MySQL is ontwikkeld in [MySQL in Communityversie](https://www.mysql.com/products/community/), met behulp van de InnoDB-engine. Azure Database for MySQL ondersteunt momenteel de volgende versies:

## <a name="mysql-version-5639"></a>MySQL-versie 5.6.39
Raadpleeg de MySQL [documentatie](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) voor meer informatie over verbeteringen en oplossingen in MySQL 5.6.39.

## <a name="mysql-version-5721"></a>MySQL-versie 5.7.21
Raadpleeg de MySQL [documentatie](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) voor meer informatie over verbeteringen en oplossingen in MySQL 5.7.21.

> [!NOTE]
> Een gateway wordt gebruikt in de service, zodat de verbindingen worden omgeleid naar de server-exemplaren. Nadat de verbinding tot stand is gebracht, wordt de versie van MySQL instellen in de gateway, niet de daadwerkelijke versie die op uw exemplaar van de MySQL-server weergegeven in de MySQL-client. Als u wilt bepalen welke versie van uw MySQL-server-exemplaar, gebruikt u de `SELECT VERSION();` opdracht aan de MySQL-prompt.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
De service beheert automatisch patches voor foutoplossing versie-updates. Secundaire versie-upgrade is op dit moment niet ondersteund. Bijvoorbeeld, wordt een upgrade van MySQL 5.6 naar MySQL 5.7 niet ondersteund. Als u wilt upgraden naar de volgende secundaire versie, een [dump maken en terugzetten](./concepts-migrate-dump-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

## <a name="next-steps"></a>Volgende stappen

Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-pricing-tiers.md)

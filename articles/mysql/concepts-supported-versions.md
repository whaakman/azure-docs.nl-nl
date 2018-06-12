---
title: Ondersteunde versies in Azure voor MySQL-Database
description: Beschrijft de ondersteunde versies in Azure-Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/23/2018
ms.openlocfilehash: c9a533ed9b9eb9ac53a02439b98a78954c7aaa11
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265245"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Ondersteunde Azure-Database voor MySQL server-versies
Azure MySQL-Database is ontwikkeld van [MySQL Community Edition](https://www.mysql.com/products/community/), de InnoDB-engine.  Azure MySQL-Database ondersteunt momenteel de volgende versies:

## <a name="mysql-version-5639"></a>MySQL-versie 5.6.39
Raadpleeg de MySQL [documentatie](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) voor meer informatie over verbeteringen en verbeteringen in MySQL 5.6.39.

## <a name="mysql-version-5721"></a>MySQL-versie 5.7.21
Raadpleeg de MySQL [documentatie](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) voor meer informatie over verbeteringen en oplossingen in MySQL 5.7.21.

> [!NOTE]
> Een gateway wordt in de service gebruikt voor het omleiden van de verbindingen naar de server-exemplaren. Nadat de verbinding tot stand is gebracht, toont de MySQL-client de versie van MySQL ingesteld in de gateway, niet de werkelijke versie uitgevoerd op uw MySQL-server-exemplaar. Gebruiken om te bepalen welke versie van uw MySQL-server-exemplaar, de `SELECT VERSION();` opdracht bij de MySQL-prompt. 

## <a name="managing-updates-and-upgrades"></a>Het beheer van updates en upgrades
De service beheert automatisch patchen voor secundaire versie-updates. Primaire versie-upgrades worden niet ondersteund (bijvoorbeeld) upgraden van MySQL 5.6 naar MySQL 5.7).

## <a name="next-steps"></a>Volgende stappen

Voor informatie over specifieke resource quota's en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-pricing-tiers.md)

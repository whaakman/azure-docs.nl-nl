---
title: Ondersteunde versies in Azure Database for PostgreSQL
description: Beschrijft de ondersteunde versies in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/12/2018
ms.openlocfilehash: a03ead5e577b261b99e635addf6f9a98d8f3cadd
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619689"
---
# <a name="supported-postgresql-database-versions"></a>Ondersteunde versies van de PostgreSQL-database
Microsoft beoogt ter ondersteuning van n-2 versies van de PostgreSQL-engine in de Azure Database for PostgreSQL-service. De versies worden de huidige primaire versie van Azure (n) en de twee voorgaande primaire versies (-2).

Azure Database voor PostgreSQL ondersteunt momenteel de volgende versies:

## <a name="postgresql-version-105"></a>PostgreSQL-versie 10,5
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/10/static/release-10-5.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9610"></a>PostgreSQL-versie 9.6.10
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.6/static/release-9-6-10.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9514"></a>PostgreSQL-versie 9.5.14
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.5/static/release-9-5-14.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
Azure Database voor PostgreSQL beheert automatisch patches voor secundaire versie. Primaire versie-upgrade is op dit moment niet ondersteund. Bijvoorbeeld, wordt een upgrade van PostgreSQL 9.5 naar PostgreSQL 9.6 niet ondersteund. Als u upgraden naar de volgende primaire versie wilt, maakt u een database [dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de ondersteuning van verschillende PostgreSQL-extensies [PostgreSQL-extensies](concepts-extensions.md).

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
ms.date: 09/07/2018
ms.openlocfilehash: f337679279f4a3eb6d01808f15a59fee4c8f3cd1
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630573"
---
# <a name="supported-postgresql-database-versions"></a>Ondersteunde PostgreSQL-Database-versies
Microsoft beoogt ter ondersteuning van n-2 versies van de PostgreSQL-engine in de Azure Database for PostgreSQL-service, wat betekent dat de huidige uitgebrachte primaire versie (n) en de twee voorgaande primaire versies (-2).

Azure Database voor PostgreSQL ondersteunt momenteel de volgende versies:

## <a name="postgresql-version-104"></a>PostgreSQL-versie 10.4
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/10/static/release-10-4.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-969"></a>PostgreSQL-versie 9.6.9
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.6/static/release-9-6-9.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9513"></a>PostgreSQL-versie 9.5.13
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.5/static/release-9-5-13.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
Azure Database voor PostgreSQL beheert automatisch patches voor secundaire versie-updates. Primaire versie-upgrade is op dit moment niet ondersteund. Bijvoorbeeld, wordt een upgrade van PostgreSQL 9.5 naar PostgreSQL 9.6 niet ondersteund. Als u wilt upgraden naar de volgende primaire versie, een [dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de ondersteuning van verschillende PostgreSQL-extensies [PostgreSQL-extensies](concepts-extensions.md).

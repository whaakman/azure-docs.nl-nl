---
title: Ondersteunde versies in Azure Database for PostgreSQL - één Server
description: Beschrijft de ondersteunde versies in Azure Database voor PostgreSQL - één Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4dcbaf159fce6b5f5495a6a25a2a3420cad9e5e8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65067265"
---
# <a name="supported-postgresql-database-versions"></a>Ondersteunde versies van de PostgreSQL-database
Microsoft beoogt ter ondersteuning van n-2 versies van de PostgreSQL-engine in Azure Database voor PostgreSQL - één Server. De versies worden de huidige primaire versie van Azure (n) en de twee voorgaande primaire versies (-2).

Azure Database voor PostgreSQL ondersteunt momenteel de volgende versies:

## <a name="postgresql-version-107"></a>PostgreSQL-versie 10.7
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/10/static/release-10-7.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9612"></a>PostgreSQL-versie 9.6.12
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9516"></a>PostgreSQL-versie 9.5.16
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
Azure Database voor PostgreSQL beheert automatisch patches voor secundaire versie. Primaire versie-upgrade is op dit moment niet ondersteund. Bijvoorbeeld, wordt een upgrade van PostgreSQL 9.5 naar PostgreSQL 9.6 niet ondersteund. Als u upgraden naar de volgende primaire versie wilt, maakt u een database [dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de ondersteuning van verschillende PostgreSQL-extensies [PostgreSQL-extensies](concepts-extensions.md).

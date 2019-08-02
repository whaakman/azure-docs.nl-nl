---
title: Ondersteunde versies in Azure Database for PostgreSQL-één server
description: Beschrijft de ondersteunde versies in Azure Database for PostgreSQL-één server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/26/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: fae8dfb2e2cc532f4aed17f83f13bbee31d29113
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68551367"
---
# <a name="supported-postgresql-database-versions"></a>Ondersteunde versies van PostgreSQL-data base
Micro soft streeft ernaar ondersteuning te bieden voor n-2 versies van de PostgreSQL-engine in Azure Database for PostgreSQL-één server. De versies zijn de huidige primaire versie op Azure (n) en de twee voor gaande primaire versies (-2).

Azure Database for PostgreSQL ondersteunt momenteel de volgende versies:

## <a name="postgresql-version-112"></a>PostgreSQL-versie 11,2
Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/11/static/release-11-2.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-107"></a>PostgreSQL-versie 10,7
Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/10/static/release-10-7.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9612"></a>PostgreSQL-versie 9.6.12
Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9516"></a>PostgreSQL-versie 9.5.16
Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="managing-updates-and-upgrades"></a>Updates en upgrades beheren
Azure Database for PostgreSQL beheert automatisch secundaire versie patches. Momenteel wordt de upgrade van de primaire versie niet ondersteund. Een upgrade van PostgreSQL 9,5 naar PostgreSQL 9,6 wordt bijvoorbeeld niet ondersteund. Als u wilt upgraden naar de volgende primaire versie, maakt u een database [dump en herstelt](./howto-migrate-using-dump-and-restore.md) u deze op een server die is gemaakt met de nieuwe engine versie.

> Houd er rekening mee dat het [postgresql-versie beleid](https://www.postgresql.org/support/versioning/) als een _primaire versie_ -upgrade voor postgresql versie 10 als een verhoging van het eerste _of_ tweede nummer moet worden uitgevoerd (bijvoorbeeld 9,5 tot 9,6 werd beschouwd als een _primaire_ versie-upgrade).
> Vanaf versie 10 wordt alleen een wijziging in het eerste nummer beschouwd als een primaire versie-upgrade (bijvoorbeeld 10,0 tot 10,1 is een _secundaire_ versie-upgrade en 10 tot 11 is een _primaire_ versie-upgrade).

## <a name="next-steps"></a>Volgende stappen
Zie [postgresql-uitbrei dingen](concepts-extensions.md)voor informatie over de ondersteuning van verschillende postgresql-uitbrei dingen.

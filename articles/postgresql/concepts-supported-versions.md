---
title: Ondersteunde versies in Azure Database for PostgreSQL-één server
description: Beschrijft de ondersteunde versies in Azure Database for PostgreSQL-één server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: b77fd082be43b8cbdedf7cbe5875a8931eb0474a
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68837890"
---
# <a name="supported-postgresql-database-versions"></a>Ondersteunde versies van PostgreSQL-data base
Micro soft streeft ernaar ondersteuning te bieden voor n-2 versies van de PostgreSQL-engine in Azure Database for PostgreSQL-één server. De versies zijn de huidige primaire versie op Azure (n) en de twee voor gaande primaire versies (-2).

Azure Database for PostgreSQL ondersteunt momenteel de volgende primaire versies:

## <a name="postgresql-version-11"></a>PostgreSQL-versie 11
De huidige secundaire versie is 11,4. Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/11/static/release-11-4.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-10"></a>PostgreSQL-versie 10
De huidige secundaire versie is 10,9. Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/10/static/release-10-9.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-96"></a>PostgreSQL-versie 9,6
De huidige secundaire versie is 9.6.14. Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/9.6/static/release-9-6-14.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-95"></a>PostgreSQL-versie 9,5
De huidige secundaire versie is 9.5.18. Raadpleeg de [postgresql-documentatie](https://www.postgresql.org/docs/9.5/static/release-9-5-18.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="managing-upgrades"></a>Upgrades beheren
Azure Database for PostgreSQL beheert automatisch secundaire versie-upgrades. 

Automatische primaire versie-upgrade wordt niet ondersteund. Er is bijvoorbeeld geen automatische upgrade van PostgreSQL 9,5 naar PostgreSQL 9,6. Als u een upgrade naar de volgende primaire versie wilt uitvoeren, maakt u een [database dump en herstelt](./howto-migrate-using-dump-and-restore.md) u een server die is gemaakt met de nieuwe engine versie.

### <a name="version-syntax"></a>Versie syntaxis
Vóór PostgreSQL versie 10 wordt het [postgresql-versie beleid](https://www.postgresql.org/support/versioning/) beschouwd als een _primaire versie_ -upgrade, zodat het eerste _of_ tweede nummer toeneemt. Bijvoorbeeld: 9,5 tot 9,6 werd beschouwd als een _primaire_ versie-upgrade. Vanaf versie 10 wordt alleen een wijziging in het eerste getal beschouwd als een primaire versie-upgrade. Bijvoorbeeld: 10,0 tot 10,1 is een _secundaire_ versie-upgrade. Versie 10 tot 11 is een _primaire_ versie-upgrade.

## <a name="next-steps"></a>Volgende stappen
Zie [het document extensies](concepts-extensions.md)voor meer informatie over ondersteunde postgresql-uitbrei dingen.

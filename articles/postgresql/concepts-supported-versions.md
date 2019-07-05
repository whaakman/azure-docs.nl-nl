---
title: Ondersteunde versies in Azure Database for PostgreSQL - één Server
description: Beschrijft de ondersteunde versies in Azure Database voor PostgreSQL - één Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/11/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: c328f0dc510c1b7d36b547e6560a292f98d72f6f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448047"
---
# <a name="supported-postgresql-database-versions"></a>Ondersteunde versies van de PostgreSQL-database
Microsoft beoogt ter ondersteuning van n-2 versies van de PostgreSQL-engine in Azure Database voor PostgreSQL - één Server. De versies worden de huidige primaire versie van Azure (n) en de twee voorgaande primaire versies (-2).

Azure Database voor PostgreSQL ondersteunt momenteel de volgende versies:

## <a name="postgresql-version-112"></a>PostgreSQL-versie 11.2
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/11/static/release-11-2.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

>[!NOTE]
> PostgreSQL versie 11 is beschikbaar in preview. Ondersteuning voor maakt met behulp van de Azure-portal is geïmplementeerd en kan nog niet beschikbaar in uw regio. U kunt de [Azure CLI](quickstart-create-server-database-azure-cli.md) een Postgres-11-server maken in elke regio. Bijvoorbeeld `az postgres server create -g group -n server -u username -p password -l westeurope --sku-name GP_Gen5_2 --version 11`.

## <a name="postgresql-version-107"></a>PostgreSQL-versie 10.7
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/10/static/release-10-7.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9612"></a>PostgreSQL-versie 9.6.12
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="postgresql-version-9516"></a>PostgreSQL-versie 9.5.16
Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html) voor meer informatie over verbeteringen en oplossingen in deze secundaire versie.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
Azure Database voor PostgreSQL beheert automatisch patches voor secundaire versie. Primaire versie-upgrade is op dit moment niet ondersteund. Bijvoorbeeld, wordt een upgrade van PostgreSQL 9.5 naar PostgreSQL 9.6 niet ondersteund. Als u upgraden naar de volgende primaire versie wilt, maakt u een database [dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

> Houd er rekening mee dat voorafgaand aan de PostgreSQL-versie 10, de [PostgreSQL versiebeheer beleid](https://www.postgresql.org/support/versioning/) beschouwd als een _hoofdversie_ upgrade naar een toename van de eerste worden _of_ tweede getal (voor voorbeeld, 9.5-9.6 werd beschouwd als een _belangrijke_ versie-upgrade).
> Vanaf versie 10, alleen een wijziging in het eerste getal wordt beschouwd als een grote versie-upgrade (10.0-10.1 is bijvoorbeeld een _kleine_ versie-upgrade en 10 tot en met 11 is een _belangrijke_ versie-upgrade).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de ondersteuning van verschillende PostgreSQL-extensies [PostgreSQL-extensies](concepts-extensions.md).

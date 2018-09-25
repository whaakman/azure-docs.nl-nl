---
title: Ondersteunde versies in Azure Database voor MariaDB
description: Beschrijft de ondersteunde versies in Azure Database voor MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: aa83b9955839aaa03aa2ebf46c9e464dc75c8d3a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977499"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Ondersteunde Azure-Database voor MariaDB server-versies
Azure Database voor MariaDB heeft is ontwikkeld op basis van de open-source [MariaDB Server](https://downloads.mariadb.org/), met behulp van de InnoDB-engine. Azure Database voor MariaDB ondersteunt momenteel de volgende versie:

## <a name="mariadb-version-10217"></a>MariaDB versie 10.2.17
Raadpleeg de [MariaDB documentatie](https://downloads.mariadb.org/mariadb/10.2.17/) voor meer informatie over verbeteringen en oplossingen in MariaDB 10.2.17.

> [!NOTE]
> Een gateway wordt gebruikt in de service, zodat de verbindingen worden omgeleid naar de server-exemplaren. Nadat de verbinding tot stand is gebracht, wordt de versie van MariaDB instellen in de gateway, niet de daadwerkelijke versie die op uw server-exemplaar van MariaDB weergegeven in de MySQL-client. Als u wilt bepalen welke versie van uw server-exemplaar van MariaDB, gebruikt u de `SELECT VERSION();` opdracht aan de MySQL-prompt.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades
De service beheert automatisch patches voor secundaire versie-updates.

## <a name="next-steps"></a>Volgende stappen
Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-pricing-tiers.md)
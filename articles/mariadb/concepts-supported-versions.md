---
title: Ondersteunde versies in Azure Database voor MariaDB
description: Beschrijft de ondersteunde versies in Azure Database voor MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 760cb39ea1f3980faba348c7aa1de68a66b20a8d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065728"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Ondersteunde Azure-Database voor MariaDB server-versies

Azure Database voor MariaDB heeft is ontwikkeld op basis van de open-source [MariaDB Server](https://downloads.mariadb.org/), met behulp van de InnoDB-engine. Azure Database voor MariaDB ondersteunt momenteel de volgende versie:

## <a name="mariadb-version-102"></a>MariaDB versie 10.2

Raadpleeg de [MariaDB documentatie](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/) voor meer informatie over verbeteringen en oplossingen in MariaDB 10.2.23.

## <a name="mariadb-version-103"></a>MariaDB versie 10.3

Raadpleeg de [MariaDB documentatie](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/) voor meer informatie over verbeteringen en oplossingen in MariaDB 10.3.14.

> [!NOTE]
> Een gateway wordt gebruikt in de service, zodat de verbindingen worden omgeleid naar de server-exemplaren. Nadat de verbinding tot stand is gebracht, wordt de versie van MariaDB instellen in de gateway, niet de daadwerkelijke versie die op uw server-exemplaar van MariaDB weergegeven in de MySQL-client. Als u wilt bepalen welke versie van uw server-exemplaar van MariaDB, gebruikt u de `SELECT VERSION();` opdracht aan de MySQL-prompt.

## <a name="managing-updates-and-upgrades"></a>Beheren van updates en upgrades

De service beheert automatisch patches voor secundaire versie-updates.

## <a name="next-steps"></a>Volgende stappen

- Voor informatie over specifieke resource quota en beperkingen op basis van uw **servicelaag**, Zie [Servicelagen](./concepts-pricing-tiers.md).

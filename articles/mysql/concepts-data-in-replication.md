---
title: Gegevens repliceren in Azure Database voor MySQL.
description: Dit artikel beschrijft de gegevens in de replicatie voor Azure Database voor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: f91a6da9a305c6620e4e01ab7aa3c554374cb5d7
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55691519"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Gegevens repliceren in Azure Database for MySQL

Gegevens in replicatie kunt u om gegevens te synchroniseren vanaf een externe MySQL-server in de Azure Database for MySQL-service. De externe server kan worden on-premises of in virtuele machines of een databaseservice die wordt gehost door andere cloudproviders. Gegevens in replicatie is gebaseerd op de binaire logboek (binlog) op basis van positie bestandsreplicatie systeemeigen met MySQL. Zie voor meer informatie over binlog replicatie, de [MySQL binlog replicatie-overzicht](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Wanneer u gegevens in replicatie
De belangrijkste scenario's met behulp van gegevens in replicatie zijn:

- **Synchronisatie van hybride gegevens:** U kunt gegevens worden gesynchroniseerd tussen uw on-premises servers en Azure Database for MySQL kunt houden met gegevens in replicatie. Deze synchronisatie is handig voor het maken van hybride toepassingen. Deze methode is aantrekkelijke wanneer u een bestaande lokale database-server hebt, maar wilt de gegevens te verplaatsen naar een regio dichter bij te stellen aan eindgebruikers.
- **Meerdere Cloud-synchronisatie:** Voor complexe cloudoplossingen, gebruikt u replicatie van gegevens in om te synchroniseren van gegevens tussen Azure Database for MySQL en andere cloud-providers, zoals virtuele machines en databaseservices die worden gehost in die clouds.

## <a name="limitations-and-considerations"></a>Beperkingen en overwegingen

### <a name="data-not-replicated"></a>Gegevens die niet worden gerepliceerd
De [ *mysql systeemdatabase* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) op de hoofd-server wordt niet gerepliceerd. Wijzigingen in accounts en machtigingen op de hoofd-server worden niet gerepliceerd. Als u een account op de hoofd-server maken en dit account moet toegang hebben tot de replica-server, moet u handmatig hetzelfde account dat aan de serverzijde replica maken. Zie voor meer informatie over welke tabellen zijn opgenomen in de database, de [MySQL handmatig](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Vereisten
- De versie van de hoofd-server moet ten minste versie 5.6 voor MySQL. 
- De hoofd- en replica-server-versies moeten hetzelfde zijn. Bijvoorbeeld, beide moet MySQL versie 5.6 of beide moet MySQL versie 5.7.
- Elke tabel moet een primaire sleutel hebben.
- Hoofd-server moet de MySQL-InnoDB-engine gebruiken.
- Gebruiker moet machtigingen hebben voor binaire logboekregistratie configureren en maken van nieuwe gebruikers op de hoofd-server.

### <a name="other"></a>Overige
- Gegevens in de replicatie is alleen ondersteund in het algemeen gebruik en geoptimaliseerd voor geheugen Prijscategorieën.
- Algemene transactie-id's (GTID) worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [gegevens in replicatie instellen](howto-data-in-replication.md)
- Meer informatie over [repliceren in Azure met replica's lezen](concepts-read-replicas.md)

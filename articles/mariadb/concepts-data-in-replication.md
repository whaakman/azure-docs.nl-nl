---
title: Gegevens repliceren in Azure Database voor MariaDB.
description: Dit artikel beschrijft de gegevens in de replicatie voor Azure Database voor MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0a1ead1580f6764fec7d1d18daa38bf093f242f2
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547602"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>Het repliceren van gegevens in Azure Database voor MariaDB

Gegevens in replicatie kunt u gegevens synchroniseren met een MariaDB-server die on-premises worden uitgevoerd in virtuele machines of database-services die worden gehost door andere cloudproviders in de Azure Database voor MariaDB-service. Gegevens in replicatie is gebaseerd op de binaire logboek (binlog) bestand positie-replicatie op basis van systeemeigen naar MariaDB. Zie voor meer informatie over binlog replicatie, de [binlog replicatie-overzicht](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Wanneer u gegevens in replicatie
De belangrijkste scenario's met behulp van gegevens in replicatie zijn:

- **Synchronisatie van hybride gegevens:** U kunt de gegevens worden gesynchroniseerd tussen uw on-premises servers en Azure Database voor MariaDB houden met gegevens in replicatie. Deze synchronisatie is handig voor het maken van hybride toepassingen. Deze methode is aantrekkelijke wanneer u een bestaande lokale database-server hebt, maar u wilt de gegevens te verplaatsen naar een regio dichter bij te stellen aan eindgebruikers.
- **Meerdere Cloud-synchronisatie:** Voor complexe cloudoplossingen, door gegevens in replicatie te gebruiken om gegevens tussen Azure-Database voor MariaDB en andere cloud-providers, zoals virtuele machines en databaseservices die worden gehost in deze clouds te synchroniseren.

## <a name="limitations-and-considerations"></a>Beperkingen en overwegingen

### <a name="data-not-replicated"></a>Gegevens die niet worden gerepliceerd
De [ *mysql systeemdatabase* ](https://mariadb.com/kb/en/library/the-mysql-database-tables/) op de hoofd-server niet is gerepliceerd. Wijzigingen in accounts en machtigingen op de hoofd-server worden niet gerepliceerd. Als u een account op de hoofd-server maken en dit account moet toegang hebben tot de replica-server, klikt u vervolgens handmatig maken hetzelfde account dat aan de serverzijde van de replica. Zie voor meer informatie over welke tabellen zijn opgenomen in de database, de [MariaDB documentatie](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Vereisten
- De versie van de hoofd-server moet ten minste versie 10.2 MariaDB.
- De hoofd- en replica-server-versies moeten hetzelfde zijn. Bijvoorbeeld, moeten beide MariaDB versie 10.2.
- Elke tabel moet een primaire sleutel hebben.
- Hoofd-server moet de InnoDB-engine gebruiken.
- Gebruiker moet machtigingen hebben voor binaire logboekregistratie configureren en maken van nieuwe gebruikers op de hoofd-server.

### <a name="other"></a>Overige
- Gegevens in de replicatie is alleen ondersteund in het algemeen gebruik en geoptimaliseerd voor geheugen Prijscategorieën.
- Algemene transactie-id's (GTID) worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [instellen van replicatie van gegevens in](howto-data-in-replication.md).

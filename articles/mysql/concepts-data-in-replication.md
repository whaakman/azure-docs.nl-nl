---
title: Gegevens repliceren in Azure Database voor MySQL.
description: Dit artikel beschrijft de gegevens in de replicatie voor Azure Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: 6135e4a0182f3af7db54eab974e4c307402185ab
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666073"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Gegevens repliceren in Azure Database for MySQL

Gegevens in replicatie kunt u gegevens synchroniseren met een MySQL-server die on-premises worden uitgevoerd in virtuele machines of database-services die worden gehost door andere cloudproviders in de Azure Database for MySQL-service. Gegevens in replicatie is gebaseerd op de binaire logboek (binlog) op basis van positie bestandsreplicatie systeemeigen met MySQL. Zie voor meer informatie over binlog replicatie, de [MySQL binlog replicatie-overzicht](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Wanneer u gegevens in replicatie
De belangrijkste scenario's met behulp van gegevens in replicatie zijn:

- **Hybride gegevenssynchronisatie:** met gegevens in replicatie, kunt u gegevens worden gesynchroniseerd tussen uw on-premises servers en Azure Database voor MySQL. Deze synchronisatie is handig voor het maken van hybride toepassingen. Deze methode is aantrekkelijke wanneer u een bestaande lokale database-server hebt, maar u wilt de gegevens te verplaatsen naar een regio dichter bij te stellen aan eindgebruikers.
- **Meerdere Cloud-synchronisatie:** voor complexe cloudoplossingen, gebruiken om te synchroniseren van gegevens tussen Azure Database for MySQL en andere cloudproviders, is de replicatie van gegevens in met inbegrip van virtuele machines en databaseservices die worden gehost in die clouds.

## <a name="limitations-and-considerations"></a>Beperkingen en overwegingen

### <a name="data-not-replicated"></a>Gegevens die niet worden gerepliceerd
De [ *mysql systeemdatabase* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) op de hoofd-server niet is gerepliceerd. Wijzigingen in accounts en machtigingen op de hoofd-server worden niet gerepliceerd. Als u een account op de hoofd-server maken en dit account moet toegang hebben tot de replica-server, klikt u vervolgens handmatig maken hetzelfde account dat aan de serverzijde van de replica. Zie voor meer informatie over welke tabellen zijn opgenomen in de database, de [MySQL handmatig](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Vereisten
- De versie van de hoofd-server moet ten minste versie 5.6 voor MySQL. 
- De hoofd- en replica-server-versies moeten hetzelfde zijn. Bijvoorbeeld, beide moet MySQL versie 5.6 of beide moet MySQL versie 5.7.
- Elke tabel moet een primaire sleutel hebben.
- Hoofd-server moet de MySQL-InnoDB-engine gebruiken.
- Gebruiker moet machtigingen hebben voor binaire logboekregistratie configureren en maken van nieuwe gebruikers op de hoofd-server.

### <a name="other"></a>Overige
- Gegevens in replicatie wordt alleen ondersteund in het algemeen gebruik en geoptimaliseerd voor geheugen Prijscategorieën
- Algemene transactie-id's (GTID) worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [gegevens in replicatie instellen](howto-data-in-replication.md)

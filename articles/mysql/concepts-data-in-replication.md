---
title: Gegevens repliceren aan Azure MySQL-Database.
description: Dit artikel worden de gegevens in replicatie voor de Azure-Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/20/2018
ms.openlocfilehash: 72f8211ecc0534b15402911de8fc0ec3d541a835
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294901"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Gegevens repliceren aan Azure MySQL-Database

Gegevens in replicatie kunt u gegevens van een MySQL-server on-premises uitgevoerd in de virtuele machines of databaseservices die worden gehost door andere cloudproviders aan de Azure-Database MySQL-service synchroniseren. Gegevens in de replicatie is gebaseerd op de binaire logboek (binlog) bestand positie replicatie op basis van systeemeigen naar MySQL. Zie voor meer informatie over binlog replicatie, de [MySQL binlog replicatie-overzicht](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Wanneer gebruikt u gegevens in replicatie
De belangrijkste scenario's overwegen om gegevens in replicatie zijn:

- **Hybride gegevenssynchronisatie:** met gegevens in replicatie, kunt u gegevens worden gesynchroniseerd tussen uw lokale servers en Azure-Database voor MySQL. Deze synchronisatie is nuttig voor het maken van hybride toepassingen. Deze methode is interessante wanneer u een bestaande lokale database-server hebt, maar u wilt dat de gegevens worden verplaatst naar een regio dichter aan eindgebruikers.
- **Meerdere Cloud-synchronisatie:** voor complexe cloudoplossingen, gebruik replicatie-gegevens in om te synchroniseren van gegevens tussen Azure-Database voor MySQL en providers van andere cloud, met inbegrip van virtuele machines en databaseservices gehost in die clouds.

## <a name="limitations-and-considerations"></a>Beperkingen en overwegingen

### <a name="data-not-replicated"></a>Niet-gerepliceerde gegevens
De [ *mysql systeemdatabase* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) op de primaire server niet is gerepliceerd. Wijzigingen in accounts en machtigingen op de primaire server worden niet gerepliceerd. Als u een account op de primaire server maken en dit account moet toegang tot de replicaserver, klikt u vervolgens handmatig maken hetzelfde account aan de serverzijde van de replica. Om te begrijpen welke tabellen zijn opgenomen in de systeemdatabase, Zie de [MySQL handmatige](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Vereisten
- De versie van de primaire server moet ten minste versie 5.6 MySQL. 
- De versies van de primaire en replica-server moeten hetzelfde zijn. Bijvoorbeeld: beide MySQL versie 5.6 moeten of beide moet MySQL versie 5.7.
- Elke tabel moet een primaire sleutel hebben.
- Primaire server, moet de MySQL InnoDB-engine gebruiken.
- Gebruiker moet machtigingen hebben voor binaire logboekregistratie configureren en maak een nieuwe gebruikers op de primaire server.

### <a name="other"></a>Overige
- Algemene transactie-id's (GTID) worden niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [gegevens in de replicatie instellen](howto-data-in-replication.md)

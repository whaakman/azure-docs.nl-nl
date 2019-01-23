---
title: Lezen-replica's in Azure Database voor MySQL.
description: Dit artikel beschrijft lezen replica's voor Azure Database voor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 1/22/2019
ms.openlocfilehash: 6ebbaece66d9055fd2bff68eee873b012b4a6d50
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462417"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Lezen-replica's in Azure Database for MySQL

De functie lezen replica (openbare preview) kunt u gegevens uit een Azure Database for MySQL-server (master) op maximaal vijf alleen-lezen-servers (replica's) binnen dezelfde Azure-regio repliceren. Alleen-lezen replica's worden asynchroon bijgewerkt met de MySQL-engine systeemeigen binair logboek (binlog)-bestand op basis van positie-Replicatietechnologie. Zie voor meer informatie over binlog replicatie, de [MySQL binlog replicatie-overzicht](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

Replica's die zijn gemaakt in de Azure Database for MySQL-service zijn nieuwe servers die op dezelfde manier als normale/zelfstandige MySQL-servers kunnen worden beheerd. U wordt gefactureerd voor elke lezen replica voor de ingerichte Computing in vCores en ingerichte opslag in GB/maand. 


Raadpleeg voor meer informatie over functies voor MySQL-replicatie en problemen met de [MySQL replicatie documentatie](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-read-replicas"></a>Wanneer u meer replica 's

Toepassingen en werkbelastingen die worden gelezen intensieve kunnen worden geleverd door de alleen-lezen replica's. Meer replica's helpen verhogen van het bedrag van meer capaciteit die beschikbaar is in vergelijking met zijn als u slechts één server gebruiken voor zowel lezen en schrijven. De lees-workloads kunnen worden geïsoleerd voor de replica's, terwijl schrijven workloads kunnen worden omgeleid naar het hoofdniveau.

Een veelvoorkomend scenario is dat BI en analytische werkbelastingen gebruiken de lezen replica als de gegevensbron voor rapportage.

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

### <a name="pricing-tiers"></a>Prijscategorieën

Lezen-replica's zijn momenteel alleen beschikbaar in de Prijscategorieën voor algemeen gebruik en geoptimaliseerd voor geheugen.

### <a name="master-server-restart"></a>Hoofd-server opnieuw opstarten

Deze Preview-versie, als u een replica maakt voor een model waarvoor geen bestaande replica's, wordt het model eerst opnieuw zelf voorbereiden voor replicatie. Dit rekening mee te houden en het uitvoeren van deze bewerkingen gedurende een periode buiten piektijden.

### <a name="stopping-replication"></a>Replicatie stoppen

U kunt kiezen om replicatie tussen een hoofd- en een replica-server te stoppen. Replicatie beëindigen, verwijdert u de replicatierelatie tussen de hoofd- en replica-server.

Wanneer replicatie is gestopt, wordt de replica-server een zelfstandige server. De gegevens in de zelfstandige server zijn de gegevens die beschikbaar zijn op de replica op het moment dat de opdracht is 'replicatie gestopt' is gestart. De zelfstandige server met de hoofd-server geen af van. Deze server kan niet opnieuw worden gemaakt in een replica.

### <a name="replicas-are-new-servers"></a>Replica's worden nieuwe servers

Replica's worden gemaakt als nieuwe Azure Database for MySQL-servers. Bestaande servers kunnen niet worden gemaakt in de replica's.

### <a name="replica-server-configuration"></a>Configuratie van de replica-server

Replica-servers worden gemaakt met behulp van de configuraties van dezelfde server als het model, waaronder de volgende configuraties:

- Prijscategorie
- Bewerking voor COMPUTE
- vCores
- Storage
- Bewaarperiode voor back-up
- Optie voor back-redundantie
- MySQL-engine-versie
- Firewall-regels

Nadat een replica is gemaakt, kunt u de prijscategorie wijzigen (met uitzondering van en naar Basic), compute genereren, vCores, opslag en back-upretentie onafhankelijk van de hoofd-server.

### <a name="master-server-configuration"></a>Configuratie van de hoofd-server

Als een master van de configuratie van de server (ex.) vCores of opslag) wordt bijgewerkt, configuratie van de replica's moet ook worden bijgewerkt naar waarden gelijk zijn aan of groter zijn. Zonder deze optie wordt de replica-server wellicht geen wijzigingen aangebracht in de master bijhouden en als gevolg hiervan kan vastlopen.

Nieuwe firewallregels toegevoegd aan de hoofd-server nadat een replica-server is gemaakt, worden niet gerepliceerd naar de replica. De replica moet worden bijgewerkt met deze nieuwe firewallregel ook.

### <a name="deleting-the-master-server"></a>De hoofd-server verwijderen

Wanneer een hoofd-server wordt verwijderd, wordt replicatie gestopt op alle lezen-replica's. Zelfstandige servers, worden deze replica's. De hoofd-server zelf wordt verwijderd.

### <a name="user-accounts"></a>Gebruikersaccounts

Gebruikers op de hoofd-server worden gerepliceerd naar de lezen-replica's. U kunt alleen verbinding maken met een lezen-replica met behulp van de gebruikersaccounts die beschikbaar op de hoofd-server.

### <a name="other"></a>Overige

- Algemene transactie-id's (GTID) worden niet ondersteund.
- Het maken van een replica van een replica wordt niet ondersteund.
- Tabellen in het geheugen kunnen ertoe leiden dat de replica's worden niet gesynchroniseerd. Dit is een beperking van de MySQL-Replicatietechnologie. Meer informatie in de [MySQL-referentiedocumentatie](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html) voor meer informatie.
- Afstemmen van de [ `innodb_file_per_table` ](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) parameter op een hoofdserver na het maken van een replica-server ertoe leiden de replica dat kan wordt gesynchroniseerd. De replica-server is niet op de hoogte van de verschillende tabelruimten.
- Bekijk de volledige lijst met MySQL replicatiebeperkingen in de [MySQL-documentatie](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)


## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [maken en beheren van meer replica's met behulp van de Azure portal](howto-read-replicas-portal.md)
- Leer hoe u [maken en beheren van meer replica's met de Azure CLI](howto-read-replicas-cli.md)

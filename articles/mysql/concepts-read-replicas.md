---
title: Lezen-replica's in Azure Database voor MySQL.
description: Dit artikel beschrijft lezen replica's voor Azure Database voor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/30/2019
ms.openlocfilehash: 2d70e1b5434b2fb263d1f4587888d4758fac2828
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66225364"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>Lezen-replica's in Azure Database for MySQL

De functie lezen replica kunt u voor het repliceren van gegevens uit een Azure Database for MySQL-server met een alleen-lezen-server. U kunt vanaf de hoofd-server repliceren naar maximaal vijf replica's. Replica's zijn bijgewerkt asynchroon met de MySQL-engine systeemeigen binair logboek (binlog)-bestand op basis van positie-Replicatietechnologie. Zie voor meer informatie over binlog replicatie, de [MySQL binlog replicatie-overzicht](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

> [!IMPORTANT]
> U kunt een lezen replica maken in dezelfde regio als de hoofd-server of in een andere Azure-regio van uw keuze. Regio-overschrijdende replicatie is momenteel in openbare preview.

Replica's zijn nieuwe servers die u die vergelijkbaar is met normale Azure Database for MySQL-servers beheert. Voor elke replica hebt gelezen en u wordt gefactureerd voor de ingerichte Computing in vCores en opslag in GB / maand.

Raadpleeg voor meer informatie over functies voor MySQL-replicatie en problemen met de [MySQL replicatie documentatie](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-a-read-replica"></a>Wanneer u een replica lezen

De functie lezen replica helpt bij het verbeteren van de prestaties en schaal van lees-intensieve werkbelastingen. Lezen workloads kunnen worden geïsoleerd voor de replica's, terwijl schrijven workloads kunnen worden omgeleid naar het hoofdniveau.

Een veelvoorkomend scenario is dat BI en analytische werkbelastingen gebruiken de lezen replica als de gegevensbron voor rapportage.

Omdat de replica's zijn alleen-lezen, verlagen niet rechtstreeks schrijven-capaciteit Bezig te houden op de hoofddoelserver. Deze functie is niet gericht op schrijven-intensieve workloads.

De functie lezen replica maakt gebruik van asynchrone replicatie van de MySQL. De functie is niet bedoeld voor scenario's met synchrone replicatie. Er is een meetbare vertraging tussen het model en de replica. De gegevens op de replica wordt uiteindelijk consistent zijn met de gegevens op de hoofddoelserver. Gebruik deze functie voor workloads die deze vertraging aankan.

Lezen-replica's kunnen u uw plan voor herstel na noodgevallen verbeteren. Als er een regionaal noodgeval en de hoofd-server niet beschikbaar is, kunt u uw werkbelasting naar een replica in een andere regio sturen. Om dit te doen, kunt u eerst de replica schrijfbewerkingen accepteren met behulp van de functie van de replicatie stoppen. U kunt uw toepassing vervolgens omleiden door de verbindingsreeks bij te werken. Meer informatie in de [replicatie stoppen](#stop-replication) sectie.

## <a name="create-a-replica"></a>Replica's maken

Als een hoofd-server geen bestaande replicaservers heeft, wordt eerst het model opnieuw zelf voorbereiden voor replicatie.

Wanneer u de werkstroom van de replica maken start, wordt een lege Azure Database voor MySQL-server gemaakt. De nieuwe server is gevuld met de gegevens op de hoofd-server. De tijd voor het maken, is afhankelijk van de hoeveelheid gegevens op de hoofd- en de tijd sinds de laatste wekelijkse volledige back-up. De tijd kan variëren van enkele minuten tot enkele uren.

Elke replica is ingeschakeld voor de opslag [automatisch vergroten](concepts-pricing-tiers.md#storage-auto-grow). De functie auto-grow kunt de replica om te blijven van de gegevens gerepliceerd naar deze, en te voorkomen dat een onderbreking in replicatie veroorzaakt door buiten Opslagfouten.

Meer informatie over het [een lezen replica maken in Azure portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Verbinding maken met een replica

Wanneer u een replica maakt, overnemen niet het van de firewall-regels of VNet-service-eindpunt van de hoofd-server. Deze regels moeten afzonderlijk worden ingesteld voor de replica.

De replica neemt het beheerdersaccount van de hoofd-server. Alle gebruikersaccounts op de hoofd-server worden gerepliceerd naar de lezen-replica's. U kunt alleen verbinding maken met een replica lezen met behulp van de gebruikersaccounts die beschikbaar op de hoofd-server zijn.

U kunt verbinding maken met de replica met behulp van de hostnaam en een geldig gebruikersaccount, zoals u zou voor een gewone Azure Database voor MySQL-server doen. Voor een server met de naam **myreplica** met de gebruikersnaam van beheerder **myadmin**, u kunt verbinding maken met de replica met behulp van de mysql-CLI:

```bash
mysql -h myreplica.mysql.database.azure.com -u myadmin@myreplica -p
```

Voer het wachtwoord voor het gebruikersaccount dat bij de prompt.

## <a name="monitor-replication"></a>Monitor voor replicatie

Azure Database for MySQL biedt de **vertraging van replicatie in een paar seconden** metrische gegevens in Azure Monitor. Met deze metriek is beschikbaar voor alleen replica's.

Met deze metriek wordt berekend met behulp van de `seconds_behind_master` metrische gegevens beschikbaar zijn in de MySQL `SHOW SLAVE STATUS` opdracht.

Stel een waarschuwing om u te informeren wanneer de vertraging van replicatie een waarde die niet acceptabel voor uw workload is bereikt.

## <a name="stop-replication"></a>Replicatie stoppen

U kunt replicatie tussen een hoofd- en een replica stoppen. Nadat de replicatie tussen een hoofd-server en een lezen replica is gestopt, wordt de replica een zelfstandige server. De gegevens in de zelfstandige server zijn de gegevens die beschikbaar zijn op de replica is op het moment dat de opdracht stop-replicatie is gestart. De zelfstandige server actueel met de hoofd-server niet.

Als u ervoor kiest om replicatie naar een replica te stoppen, verliest deze alle koppelingen naar de vorige hoofd- en andere replica's. Er is geen automatische failover tussen een model en de bijbehorende replica.

> [!IMPORTANT]
> De zelfstandige server kan niet opnieuw worden gemaakt in een replica.
> Voordat u replicatie op een replica lezen stoppen, controleert u of de replica de gegevens die u nodig hebt.

Meer informatie over het [stop de replicatie naar een replica](howto-read-replicas-portal.md).

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

### <a name="pricing-tiers"></a>Prijscategorieën

Lezen-replica's zijn momenteel alleen beschikbaar in de Prijscategorieën voor algemeen gebruik en geoptimaliseerd voor geheugen.

### <a name="master-server-restart"></a>Hoofd-server opnieuw opstarten

Wanneer u een replica voor een model waarvoor geen bestaande replica's maakt, wordt eerst het model opnieuw zelf voorbereiden voor replicatie. Dit rekening mee te houden en het uitvoeren van deze bewerkingen gedurende een periode buiten piektijden.

### <a name="new-replicas"></a>Nieuwe replica 's

Een lezen replica wordt gemaakt als een nieuwe Azure-Database voor MySQL-server. Een bestaande server kan niet worden gemaakt in een replica. U kunt een replica van een andere lezen replica kan niet maken.

### <a name="replica-configuration"></a>Configuratie van de replica

Een replica wordt gemaakt met behulp van de configuratie van de dezelfde server als het model. Nadat een replica is gemaakt, verschillende instellingen van de hoofdserver onafhankelijk van elkaar kunnen worden gewijzigd: compute genereren, vCores, opslag, bewaarperiode voor back- en MySQL-engine-versie. De prijscategorie kan ook afzonderlijk worden gewijzigd met uitzondering van of naar de Basic-laag.

> [!IMPORTANT]
> Werk de configuratie van de replica op gelijke of grotere waarden voordat een hoofd-server-configuratie is bijgewerkt met nieuwe waarden. Deze actie zorgt ervoor dat de replica kunt houden met eventuele wijzigingen in het model.

### <a name="stopped-replicas"></a>Gestopte replica 's

Als u replicatie tussen een hoofd-server en een lezen replica stopt, wordt de gestopte replica een zelfstandige server die zowel lees- en schrijfbewerkingen accepteert. De zelfstandige server kan niet opnieuw worden gemaakt in een replica.

### <a name="deleted-master-and-standalone-servers"></a>Verwijderde hoofd- en zelfstandige servers

Wanneer een hoofd-server wordt verwijderd, wordt replicatie gestopt op alle lezen-replica's. Zelfstandige servers, worden deze replica's. De hoofd-server zelf wordt verwijderd.

### <a name="user-accounts"></a>Gebruikersaccounts

Gebruikers op de hoofd-server worden gerepliceerd naar de lezen-replica's. U kunt alleen verbinding maken met een lezen-replica met behulp van de gebruikersaccounts die beschikbaar op de hoofd-server.

### <a name="server-parameters"></a>Serverparameters

Om te voorkomen dat gegevens worden gesynchroniseerd en om te voorkomen van gegevensverlies of beschadiging van mogelijke, zijn sommige parameters van de server worden bijgewerkt wanneer met behulp van replica's worden gelezen vergrendeld.

De volgende parameters van de server zijn vergrendeld op de hoofd- en de replica-servers:
- [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) 
- [`log_bin_trust_function_creators`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators)

De [ `event_scheduler` ](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_event_scheduler) parameter is vergrendeld op de replicaservers. 

### <a name="other"></a>Overige

- Algemene transactie-id's (GTID) worden niet ondersteund.
- Het maken van een replica van een replica wordt niet ondersteund.
- Tabellen in het geheugen kunnen ertoe leiden dat de replica's worden niet gesynchroniseerd. Dit is een beperking van de MySQL-Replicatietechnologie. Meer informatie in de [MySQL-referentiedocumentatie](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html) voor meer informatie.
- Zorg ervoor dat de hoofd-server-tabellen primaire sleutels hebben. Gebrek aan primaire sleutels kan leiden tot replicatielatentie tussen de hoofd- en replica's.
- Bekijk de volledige lijst met MySQL replicatiebeperkingen in de [MySQL-documentatie](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [maken en beheren van meer replica's met behulp van de Azure portal](howto-read-replicas-portal.md)
- Leer hoe u [maken en beheren van meer replica's met de Azure CLI](howto-read-replicas-cli.md)

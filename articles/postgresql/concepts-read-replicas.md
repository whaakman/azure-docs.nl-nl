---
title: Leesreplica's in Azure Database for PostgreSQL
description: Dit artikel beschrijft de functie lezen replica in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: f340f1e42b6993a1f834ab05570c669d4241222b
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58847185"
---
# <a name="read-replicas-in-azure-database-for-postgresql"></a>Leesreplica's in Azure Database for PostgreSQL

De functie lezen replica kunt u voor het repliceren van gegevens uit een Azure Database for PostgreSQL-server met een alleen-lezen-server. U kunt naar maximaal vijf replica's binnen dezelfde Azure-regio repliceren van de hoofd-server. Replica's worden asynchroon bijgewerkt met de systeemeigen Replicatietechnologie van de PostgreSQL-engine.

Replica's zijn nieuwe servers die u die vergelijkbaar is met normale Azure Database for PostgreSQL-servers beheert. Voor elke replica hebt gelezen en u wordt gefactureerd voor de ingerichte Computing in vCores en opslag in GB / maand.

Meer informatie over het [maken en beheren van replica's](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Wanneer u een replica lezen
De functie lezen replica helpt bij het verbeteren van de prestaties en schaal van lees-intensieve werkbelastingen. Lezen workloads kunnen worden geïsoleerd voor de replica's, terwijl schrijven workloads kunnen worden omgeleid naar het hoofdniveau.

Een veelvoorkomend scenario is dat BI en analytische werkbelastingen gebruiken de lezen replica als de gegevensbron voor rapportage.

Omdat de replica's zijn alleen-lezen, verlagen niet rechtstreeks schrijven-capaciteit Bezig te houden op de hoofddoelserver. Deze functie is niet gericht op schrijven-intensieve workloads.

De functie lezen replica maakt gebruik van PostgreSQL asynchrone replicatie. De functie is niet bedoeld voor scenario's met synchrone replicatie. Er is een meetbare vertraging tussen het model en de replica. De gegevens op de replica wordt uiteindelijk consistent zijn met de gegevens op de hoofddoelserver. Gebruik deze functie voor workloads die deze vertraging aankan.

## <a name="create-a-replica"></a>Een replica maken
De hoofd-server moet de `azure.replication_support` parameter ingesteld op **REPLICA**. Als deze parameter wordt gewijzigd, is een server opnieuw opstarten vereist voor de wijziging door te voeren. (De `azure.replication_support` parameter wordt toegepast op alleen de lagen algemeen gebruik en geoptimaliseerd voor geheugen).

Wanneer u de werkstroom van de replica maken start, wordt een lege Azure Database for PostgreSQL-server gemaakt. De nieuwe server is gevuld met de gegevens op de hoofd-server. De tijd voor het maken, is afhankelijk van de hoeveelheid gegevens op de hoofd- en de tijd sinds de laatste wekelijkse volledige back-up. De tijd kan variëren van enkele minuten tot enkele uren.

De functie lezen replica maakt gebruik van PostgreSQL fysieke replicatie niet logische replicatie. Streaming-replicatie met behulp van replicatie sleuven is de standaardmodus voor de bewerking. Wanneer dat nodig is, wordt back-upfunctie gebruikt om te achterhalen.

> [!NOTE]
> Als u geen een waarschuwing instellen voor opslag van op uw servers, wordt u aangeraden dit te doen. De waarschuwing wordt geïnformeerd wanneer een server nadert de limiet voor opslag, die van invloed is op de replicatie.

Meer informatie over het [een lezen replica maken in Azure portal](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Verbinding maken met een replica
Wanneer u een replica maakt, overnemen niet het van de firewall-regels of VNet-service-eindpunt van de hoofd-server. Deze regels moeten afzonderlijk worden ingesteld voor de replica.

De replica neemt het beheerdersaccount van de hoofd-server. Alle gebruikersaccounts op de hoofd-server worden gerepliceerd naar de lezen-replica's. U kunt alleen verbinding maken met een replica lezen met behulp van de gebruikersaccounts die beschikbaar op de hoofd-server zijn.

U kunt verbinding maken met de replica met behulp van de hostnaam en een geldig gebruikersaccount, zoals u zou voor een gewone Azure Database voor PostgreSQL-server doen. Voor een server met de naam **myreplica** met de gebruikersnaam van beheerder **myadmin**, u kunt verbinding maken met de replica met behulp van psql:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

Voer het wachtwoord voor het gebruikersaccount dat bij de prompt.

## <a name="monitor-replication"></a>Monitor voor replicatie
Azure Database voor PostgreSQL biedt de **maximale vertraging voor replica's** metrische gegevens in Azure Monitor. Met deze metriek is beschikbaar op de hoofdserver. De metrische gegevens geeft de vertraging in bytes tussen de hoofd- en de meeste achtergebleven-replica. 

Azure Database voor PostgreSQL biedt ook de **Replica Lag** metrische gegevens in Azure Monitor. Met deze metriek is beschikbaar voor alleen replica's. 

De metrische gegevens wordt berekend op basis van de `pg_stat_wal_receiver` weergeven:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```

De Replica Lag ziet u de tijd sinds de laatste herhaalde transactie. Als er geen transacties plaatsvinden op de hoofd-server, geeft de metriek deze vertraging.

Stel een waarschuwing om u te informeren wanneer de vertraging replica een waarde die niet acceptabel voor uw workload is bereikt. 

Query uitvoeren op de hoofd-server rechtstreeks voor de vertraging van replicatie in bytes op alle replica's voor meer inzicht.

In de PostgreSQL-versie 10:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

In PostgreSQL versie 9,6 en eerdere versies:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Als een hoofd-server of meer replica's opnieuw is opgestart, wordt de tijd die nodig zijn om te starten en alle weerspiegeld in de Replica Lag metrische gegevens.

## <a name="stop-replication"></a>Replicatie stoppen
U kunt replicatie tussen een hoofd- en een replica stoppen. De actie bij stoppen zorgt ervoor dat de replica opnieuw te starten en te verwijderen van de replicatie-instellingen. Nadat de replicatie tussen een hoofd-server en een lezen replica is gestopt, wordt de replica een zelfstandige server. De gegevens in de zelfstandige server zijn de gegevens die beschikbaar zijn op de replica is op het moment dat de opdracht stop-replicatie is gestart. De zelfstandige server actueel met de hoofd-server niet.

> [!IMPORTANT]
> De zelfstandige server kan niet opnieuw worden gemaakt in een replica.
> Voordat u replicatie op een replica lezen stoppen, controleert u of de replica de gegevens die u nodig hebt.

Meer informatie over het [stop de replicatie naar een replica](howto-read-replicas-portal.md).


## <a name="considerations"></a>Overwegingen

In deze sectie bevat een overzicht van de overwegingen over de functie lezen replica.

### <a name="prerequisites"></a>Vereisten
Voordat u een replica lezen, maken de `azure.replication_support` parameter moet worden ingesteld op **REPLICA** op de hoofd-server. Als deze parameter wordt gewijzigd, is een server opnieuw opstarten vereist voor de wijziging door te voeren. De `azure.replication_support` parameter wordt toegepast op alleen de lagen algemeen gebruik en geoptimaliseerd voor geheugen.

### <a name="new-replicas"></a>Nieuwe replica 's
Een lezen replica wordt gemaakt als een nieuwe Azure Database for PostgreSQL-server. Een bestaande server kan niet worden gemaakt in een replica. Een replica lezen kan alleen worden gemaakt in dezelfde Azure-regio als het model. U kunt een replica van een andere lezen replica kan niet maken.

### <a name="replica-configuration"></a>Configuratie van de replica
Een replica wordt gemaakt met behulp van de configuratie van de dezelfde server als het model. Nadat een replica is gemaakt, verschillende instellingen van de hoofdserver onafhankelijk van elkaar kunnen worden gewijzigd: compute genereren, vCores, opslag en back-up bewaarperiode. De prijscategorie kan ook afzonderlijk worden gewijzigd met uitzondering van of naar de Basic-laag.

> [!IMPORTANT]
> Werk de configuratie van de replica op gelijke of grotere waarden voordat een hoofd-server-configuratie is bijgewerkt met nieuwe waarden. Deze actie zorgt ervoor dat de replica kunt houden met eventuele wijzigingen in het model.

PostgreSQL vereist dat de waarde van de `max_connections` parameter voor de lezen-replica moet groter zijn dan of gelijk zijn aan de hoofd-waarde; anders wordt de replica niet start. In Azure Database for PostgreSQL, de `max_connections` parameterwaarde is gebaseerd op de SKU. Zie voor meer informatie, [limieten in Azure Database for PostgreSQL](concepts-limits.md). 

Als u probeert om de waarden van de server te werken, maar niet aan de beperkingen voldoen, ontvangt u een foutbericht.

### <a name="stopped-replicas"></a>Gestopte replica 's
Als u replicatie tussen een hoofd-server en een lezen replica stopt, wordt de replica opnieuw wordt opgestart als de wijziging wilt toepassen. De gestopte replica wordt een zelfstandige server die zowel lees- en schrijfbewerkingen accepteert. De zelfstandige server kan niet opnieuw worden gemaakt in een replica.

### <a name="deleted-master-and-standalone-servers"></a>Verwijderde hoofd- en zelfstandige servers
Wanneer een hoofd-server wordt verwijderd, worden alle bijbehorende lezen replica's zelfstandige servers. De replica's opnieuw worden gestart om deze wijziging weer te geven.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [maken en beheren van meer replica's in Azure portal](howto-read-replicas-portal.md).

---
title: Beperkingen in Azure Database for MySQL
description: Dit artikel wordt beschreven beperkingen in Azure Database voor MySQL, zoals het aantal verbindingen en opties voor opslag-engine.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/6/2018
ms.openlocfilehash: 9088e9ad98633b46dc3a7f0ee9002a0dd9fc5a55
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65551878"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Beperkingen in Azure Database for MySQL
De volgende secties beschrijven capaciteit, ondersteuning voor de opslag-engine, bevoegdheden ondersteuning, gegevens manipuleren instructie ondersteuning en functionele limieten in de database-service. Zie ook [algemene beperkingen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) van toepassing op de MySQL-database-engine.

## <a name="maximum-connections"></a>Maximum aantal verbindingen
Het maximum aantal verbindingen per prijscategorie en vCores zijn als volgt: 

|**Prijscategorie**|**vCore(s)**| **Maximum aantal verbindingen**|
|---|---|---|
|Basic| 1| 50|
|Basic| 2| 100|
|Algemeen doel| 2| 300|
|Algemeen doel| 4| 625|
|Algemeen doel| 8| 1250|
|Algemeen doel| 16| 2500|
|Algemeen doel| 32| 5000|
|Algemeen doel| 64| 10\.000|
|Geoptimaliseerd geheugen| 2| 600|
|Geoptimaliseerd geheugen| 4| 1250|
|Geoptimaliseerd geheugen| 8| 2500|
|Geoptimaliseerd geheugen| 16| 5000|
|Geoptimaliseerd geheugen| 32| 10\.000|

Wanneer verbindingen de limiet overschrijdt, wordt de volgende fout:
> ERROR 1040 (08004): Te veel verbindingen

## <a name="storage-engine-support"></a>Ondersteuning voor de opslag-engine

### <a name="supported"></a>Ondersteund
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [GEHEUGEN](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Niet-ondersteund
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [ZWARTE GAT](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVEREN](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATIEVE](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ondersteuning van bevoegdheden

### <a name="unsupported"></a>Niet-ondersteund
- DBA rol: Veel parameters van de server en instellingen kunnen per ongeluk serverprestaties slechter of ACID-eigenschappen van de DBMS negatief moet worden gemaakt. Als zodanig wilt behouden de integriteit van de service en SLA op het productniveau van een, wordt deze service niet weergegeven de DBA-rol. De standaard-gebruikersaccount, die is gemaakt wanneer een nieuwe database-exemplaar wordt gemaakt, kan die gebruiker voor het uitvoeren van de meeste DDL en DML-instructies in de beheerde database-instantie. 
- SUPER bevoegdheden: Op dezelfde manier [SUPER bevoegdheden](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) is ook beperkt.
- OPSTELLER: Vereist super bevoegdheden voor het maken en is beperkt. Als u gegevens met behulp van een back-up importeert, verwijdert u de `CREATE DEFINER` opdrachten handmatig of met behulp van de `--skip-definer` bij het uitvoeren van een mysqldump opdracht.

## <a name="data-manipulation-statement-support"></a>Beheerondersteuning-instructie bewerken

### <a name="supported"></a>Ondersteund
- `LOAD DATA INFILE` wordt ondersteund, maar de `[LOCAL]` parameter moet worden opgegeven en doorgestuurd naar een UNC-pad (Azure storage wordt gekoppeld via SMB).

### <a name="unsupported"></a>Niet-ondersteund
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Functionele beperkingen

### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaling naar en van de Basic Prijscategorieën wordt momenteel niet ondersteund.
- Waardoor de opslaggrootte van de server wordt niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine wordt momenteel niet ondersteund. Als u wilt upgraden naar de volgende primaire versie, een [dump maken en terugzetten](./concepts-migrate-dump-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
- Wanneer u de functie PITR, wordt de nieuwe server gemaakt met dezelfde configuratie als de server die is gebaseerd op.
- Een verwijderde server herstelt, wordt niet ondersteund.

### <a name="vnet-service-endpoints"></a>VNet-service-eindpunten
- Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

### <a name="storage-size"></a>Opslaggrootte
- Raadpleeg [Prijscategorieën](concepts-pricing-tiers.md) voor de maximale grootte van opslag per prijscategorie.

## <a name="current-known-issues"></a>Huidige bekende problemen
- MySQL-server-exemplaar wordt de juiste server-versie nadat de verbinding tot stand is gebracht. Als u de juiste server-exemplaar-engine-versie, gebruikt de `select version();` opdracht.

## <a name="next-steps"></a>Volgende stappen
- [Wat is beschikbaar in elke servicelaag](concepts-pricing-tiers.md)
- [Ondersteunde versies van de MySQL-database](concepts-supported-versions.md)

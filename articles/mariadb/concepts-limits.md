---
title: Beperkingen in Azure Database voor MariaDB
description: Dit artikel wordt beschreven beperkingen in Azure Database voor MariaDB, zoals het aantal verbindingen en opties voor opslag-engine.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: 79d6e185b64fdaf332f877718487809ba6273441
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895785"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Beperkingen in Azure Database voor MariaDB
De volgende secties beschrijven capaciteit, ondersteuning voor de opslag-engine, bevoegdheden ondersteuning, gegevens manipuleren instructie ondersteuning en functionele limieten in de database-service.

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
|Geoptimaliseerd geheugen| 2| 600|
|Geoptimaliseerd geheugen| 4| 1250|
|Geoptimaliseerd geheugen| 8| 2500|
|Geoptimaliseerd geheugen| 16| 5000|

Wanneer verbindingen de limiet overschrijdt, wordt de volgende fout:
> ERROR 1040 (08004): Te veel verbindingen

## <a name="storage-engine-support"></a>Ondersteuning voor de opslag-engine

### <a name="supported"></a>Ondersteund
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [GEHEUGEN](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Niet-ondersteund
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [ZWARTE GAT](https://mariadb.com/kb/en/library/blackhole/)
- [ARCHIVEREN](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Ondersteuning van bevoegdheden

### <a name="unsupported"></a>Niet-ondersteund
- DBA rol: Veel parameters van de server en instellingen kunnen per ongeluk serverprestaties slechter of ACID-eigenschappen van de DBMS negatief moet worden gemaakt. Als zodanig wilt behouden de integriteit van de service en SLA op het productniveau van een, wordt deze service niet weergegeven de DBA-rol. De standaard-gebruikersaccount, die is gemaakt wanneer een nieuwe database-exemplaar wordt gemaakt, kan die gebruiker voor het uitvoeren van de meeste DDL en DML-instructies in de beheerde database-instantie.
- SUPER bevoegdheden: Op dezelfde manier [SUPER bevoegdheden](https://mariadb.com/kb/en/library/grant/#global-privileges) is ook beperkt.
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
- Automatische migratie tussen versies van de primaire database-engine wordt momenteel niet ondersteund.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
- Wanneer u de functie PITR, wordt de nieuwe server gemaakt met dezelfde configuratie als de server die is gebaseerd op.
- Een verwijderde server herstelt, wordt niet ondersteund.

### <a name="subscription-management"></a>Abonnementsbeheer
- Dynamisch verplaatsen van vooraf gemaakte servers in het abonnement en resourcegroep wordt momenteel niet ondersteund.

### <a name="vnet-service-endpoints"></a>VNet-service-eindpunten
- Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

## <a name="current-known-issues"></a>Huidige bekende problemen
- MariaDB server-exemplaar wordt de onjuiste serverversie weergegeven nadat de verbinding tot stand is gebracht. Als u de juiste server-exemplaar-engine-versie, gebruikt de `select version();` opdracht.

## <a name="next-steps"></a>Volgende stappen
- [Wat is beschikbaar in elke servicelaag](concepts-pricing-tiers.md)
- [Ondersteunde versies van MariaDB-database](concepts-supported-versions.md)

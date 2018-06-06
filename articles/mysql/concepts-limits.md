---
title: Beperkingen in Azure-Database voor MySQL
description: Dit artikel wordt beschreven beperkingen in Azure-Database voor MySQL, zoals het aantal verbinding en opties voor opslag-engine.
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/04/2018
ms.openlocfilehash: 7d05d3aa94fffb13a96fc9074d55d8fa67cadb04
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763116"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Beperkingen in Azure-Database voor MySQL
De volgende secties beschrijven capaciteit, ondersteuning voor opslag-engine, ondersteuning van bevoegdheden, gegevensondersteuning manipulatie instructie en functionele limieten in de database-service. Zie ook [algemene beperkingen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) van toepassing op de MySQL-database-engine.

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

Wanneer er verbindingen de limiet overschrijdt, wordt de volgende fout:
> FOUT (08004) 1040: Te veel verbindingen

## <a name="storage-engine-support"></a>Ondersteuning voor opslag-engine

### <a name="supported"></a>Ondersteund
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [GEHEUGEN](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Niet-ondersteund
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [ZWARTE GAT](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIEF](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATIEVE](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ondersteuning van bevoegdheden

### <a name="unsupported"></a>Niet-ondersteund
- DBA rol: veel parameters van de server en -instellingen kunnen per ongeluk serverprestaties slechter of ACID-eigenschappen van het DBMS uitsluiten. Als zodanig om te blijven de integriteit van de service en de SLA op het productniveau van een, wordt deze service niet weergegeven de DBA-rol. De standaard-gebruikersaccount is samengesteld wanneer een nieuwe database-exemplaar wordt gemaakt, kan die gebruiker voor het grootste deel van de DDL- en DML-instructies in het beheerde database-exemplaar. 
- SUPER bevoegdheden: op dezelfde manier [SUPER bevoegdheid](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) wordt ook beperkt.

## <a name="data-manipulation-statement-support"></a>Ondersteuning voor gegevens manipulatie-instructie

### <a name="supported"></a>Ondersteund
- LOAD gegevens INVOERBESTAND - worden ondersteund, maar het moet de [lokale] parameter die wordt omgeleid naar een UNC-pad (Azure-opslag gekoppeld via XSMB) opgeven.

### <a name="unsupported"></a>Niet-ondersteund
- SELECTEREN... IN HET UITVOERBESTAND

## <a name="functional-limitations"></a>Functionele beperkingen

### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaalbaarheid van servers in de prijscategorie is momenteel niet ondersteund. Dat wil zeggen, schakelen tussen Basic, algemeen gebruik en geoptimaliseerd voor geheugen Prijscategorieën.
- Verkleinen storage server wordt niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine is momenteel niet ondersteund.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
- Herstellen naar andere servicelaag en/of Compute-eenheden en de opslaggrootte is niet toegestaan.
- Herstellen van een verwijderde server wordt niet ondersteund.

### <a name="subscription-management"></a>Abonnementsbeheer
- Dynamisch vooraf gemaakte servers verplaatsen tussen abonnement en resourcegroep is momenteel niet ondersteund.

## <a name="current-known-issues"></a>Huidige bekende problemen
- MySQL-serverexemplaar wordt de verkeerde serverversie nadat de verbinding tot stand is gebracht. Als u de juiste server exemplaar versiebeheer, gebruikt u Selecteer version(); de opdracht bij de MySQL-prompt.

## <a name="next-steps"></a>Volgende stappen
- [Wat is beschikbaar in elke servicelaag](concepts-pricing-tiers.md)
- [Ondersteunde versies van de MySQL-database](concepts-supported-versions.md)

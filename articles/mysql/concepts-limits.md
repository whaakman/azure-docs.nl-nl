---
title: Beperkingen in Azure-Database voor MySQL | Microsoft Docs
description: Preview-beperkingen in Azure-Database voor MySQL beschrijft.
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/11/2018
ms.openlocfilehash: f0f9a10f987f19d8ae77a07038cffe23446856fd
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Beperkingen in Azure-Database voor MySQL
De Azure-Database voor de MySQL-service is in de openbare preview. De volgende secties beschrijven capaciteit, ondersteuning voor opslag-engine, ondersteuning van bevoegdheden, gegevensondersteuning manipulatie instructie en functionele limieten in de database-service. Zie ook [algemene beperkingen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) van toepassing op de MySQL-database-engine.

## <a name="service-tier-maximums"></a>Service tier maximumwaarden
Azure MySQL-Database heeft meerdere Servicelagen kiezen bij het maken van een server. Zie voor meer informatie [begrijpen wat er beschikbaar is in elke servicelaag](concepts-service-tiers.md).  

Er is een maximum aantal verbindingen, Compute-eenheden en opslag in elke servicelaag tijdens de preview, als volgt: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maximum aantal verbindingen**        |                   |
| Basic 50 Compute-eenheden     | 50-verbindingen    |
| Basic 100 Compute-eenheden    | 100 verbindingen   |
| Standaard 100 Compute-eenheden | 200-verbindingen   |
| Standaard 200 Compute-eenheden | 400-verbindingen   |
| Standaard 400 Compute-eenheden | 800 verbindingen   |
| Standaard 800 Compute-eenheden | 1600-verbindingen  |
| **Maximale Compute-eenheden**      |                   |
| Basisservicelaag         | 100 compute-eenheden |
| Standaardservicelaag      | 800 compute-eenheden |
| **Maximale opslag**            |                   |
| Basisservicelaag         | 1 TB              |
| Standaardservicelaag      | 1 TB              |

Wanneer er te veel verbindingen zijn bereikt, wordt de volgende fout:
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
- DBA rol veel server parameters en instellingen kunnen per ongeluk serverprestaties slechter of ACID-eigenschappen van het DBMS dat. Als zodanig voor het onderhouden van onze service integriteit en SLA op het productniveau van een blootstellen we niet de rol DBA aan klanten. De standaard-gebruikersaccount is samengesteld wanneer een nieuwe database-exemplaar wordt gemaakt, kan klanten de meeste DDL en DML-instructies in het beheerde database-exemplaar uitvoeren. 
- SUPER bevoegdheid op dezelfde manier [SUPER bevoegdheid](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) wordt ook beperkt.

## <a name="data-manipulation-statement-support"></a>Ondersteuning voor gegevens manipulatie-instructie

### <a name="supported"></a>Ondersteund
- LOAD gegevens INVOERBESTAND - worden ondersteund, maar het moet de [lokale] parameter die wordt omgeleid naar een UNC-pad (Azure-opslag gekoppeld via XSMB) opgeven.

### <a name="unsupported"></a>Niet-ondersteund
- SELECTEREN... IN HET UITVOERBESTAND

## <a name="preview-functional-limitations"></a>Functionele beperkingen Preview

### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaalbaarheid van servers in Servicelagen is momenteel niet ondersteund. Dat wil zeggen, schakelen tussen servicecategorieën Basic en Standard.
- Dynamische toename mogelijk op aanvraag van opslag op vooraf gemaakte server is momenteel niet ondersteund.
- Verkleinen storage server wordt niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine is momenteel niet ondersteund.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
- Herstellen naar andere servicelaag en/of Compute-eenheden en de opslaggrootte is niet toegestaan.
- Herstellen van een verwijderde server wordt niet ondersteund.

## <a name="functional-limitations"></a>Functionele beperkingen

### <a name="subscription-management"></a>Abonnementsbeheer
- Dynamisch vooraf gemaakte servers verplaatsen tussen abonnement en resourcegroep is momenteel niet ondersteund.

## <a name="current-known-issues"></a>Huidige bekende problemen
- MySQL-serverexemplaar wordt de verkeerde serverversie nadat de verbinding tot stand is gebracht. Als u de juiste server exemplaar versiebeheer, gebruikt u Selecteer version(); de opdracht bij de MySQL-prompt.

## <a name="next-steps"></a>Volgende stappen
- [Wat is beschikbaar in elke servicelaag](concepts-service-tiers.md)
- [Ondersteunde versies van de MySQL-database](concepts-supported-versions.md)

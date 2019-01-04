---
title: Beperkingen in Azure Database for PostgreSQL
description: Dit artikel wordt beschreven beperkingen in Azure Database voor PostgreSQL, zoals het aantal verbindingen en opties voor opslag-engine.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/12/2018
ms.openlocfilehash: 4616ab535e7edca6d5f919824e9cadaf90886d5f
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548558"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Beperkingen in Azure Database for PostgreSQL
De volgende secties beschrijven de capaciteits- en functionele limieten in de database-service.

## <a name="maximum-connections"></a>Maximum aantal verbindingen
Het maximum aantal verbindingen per prijscategorie en vCores zijn als volgt: 

|**Prijscategorie**| **vCore(s)**| **Maximum aantal verbindingen** |
|---|---|---|
|Basic| 1| 50 |
|Basic| 2| 100 |
|Algemeen doel| 2| 150|
|Algemeen doel| 4| 250|
|Algemeen doel| 8| 480|
|Algemeen doel| 16| 950|
|Algemeen doel| 32| 1500|
|Algemeen doel| 64| 1900|
|Geoptimaliseerd geheugen| 2| 300|
|Geoptimaliseerd geheugen| 4| 500|
|Geoptimaliseerd geheugen| 8| 960|
|Geoptimaliseerd geheugen| 16| 1900|
|Geoptimaliseerd geheugen| 32| 3000|

Wanneer verbindingen de limiet overschrijdt, wordt de volgende fout:
> Onherstelbare fout: er al te veel clients

De Azure-systeem is vijf verbindingen voor het bewaken van de Azure Database for PostgreSQL-server vereist. 

## <a name="functional-limitations"></a>Functionele beperkingen
### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaling naar en van de Basic Prijscategorieën wordt momenteel niet ondersteund.
- Verkleinen server storage is momenteel niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine wordt momenteel niet ondersteund. Als u wilt upgraden naar de volgende primaire versie, een [dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

### <a name="vnet-service-endpoints"></a>VNet-service-eindpunten
- Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

### <a name="restoring-a-server"></a>Herstellen van een server
- Wanneer u de functie PITR, wordt de nieuwe server gemaakt met dezelfde prijzen laag schijfconfiguraties als de server die is gebaseerd op.
- De nieuwe server die is gemaakt tijdens een terugzetbewerking hoeft niet de firewallregels die aanwezig op de oorspronkelijke server was. Firewallregels moeten afzonderlijk worden ingesteld voor deze nieuwe server.
- Een verwijderde server herstelt, wordt niet ondersteund.

### <a name="utf-8-characters-on-windows"></a>UTF-8 tekens op Windows
- In sommige scenario's zijn niet volledig UTF-8 tekens ondersteund in open-source PostgreSQL op Windows, die invloed hebben op Azure Database voor PostgreSQL. Raadpleeg de thread op [fout #15476 in de postgresql-archief](https://www.postgresql-archive.org/BUG-15476-Problem-on-show-trgm-with-4-byte-UTF-8-characters-td6056677.html) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
- Inzicht in [wat beschikbaar is in elke prijscategorie](concepts-pricing-tiers.md)
- Meer informatie over [ondersteunde versies van de PostgreSQL-Database](concepts-supported-versions.md)
- Beoordeling [hoe u back-up en herstellen van een server in Azure Database for PostgreSQL via Azure portal](howto-restore-server-portal.md)

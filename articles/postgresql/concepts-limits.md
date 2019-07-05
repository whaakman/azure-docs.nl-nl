---
title: Limieten in Azure Database for PostgreSQL - één Server
description: Dit artikel beschrijft de limieten in Azure Database voor PostgreSQL - één Server, zoals het aantal verbindingen en opties voor opslag-engine.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/25/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: e4752112acf136d9ffb19a0b7383bc3aff5de5e0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448089"
---
# <a name="limits-in-azure-database-for-postgresql---single-server"></a>Limieten in Azure Database for PostgreSQL - één Server
De volgende secties beschrijven de capaciteits- en functionele limieten in de database-service. Als u wilt meer informatie over de lagen van de resource (berekenen, geheugen, opslag), raadpleegt u de [Prijscategorieën](concepts-pricing-tiers.md) artikel.


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
|Geoptimaliseerd geheugen| 32| 1987|

Wanneer verbindingen de limiet overschrijdt, wordt de volgende fout:
> Onherstelbare fout: er al te veel clients

De Azure-systeem is vijf verbindingen voor het bewaken van de Azure Database for PostgreSQL-server vereist. 

## <a name="functional-limitations"></a>Functionele beperkingen
### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaling naar en van de Basic Prijscategorieën wordt momenteel niet ondersteund.
- Verkleinen server storage is momenteel niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine wordt momenteel niet ondersteund. Als u wilt upgraden naar de volgende primaire versie, een [dump maken en terugzetten](./howto-migrate-using-dump-and-restore.md) deze naar een server die is gemaakt met de versie van de nieuwe engine.

> Houd er rekening mee dat voorafgaand aan de PostgreSQL-versie 10, de [PostgreSQL versiebeheer beleid](https://www.postgresql.org/support/versioning/) beschouwd als een _hoofdversie_ upgrade naar een toename van de eerste worden _of_ tweede getal (voor voorbeeld, 9.5-9.6 werd beschouwd als een _belangrijke_ versie-upgrade).
> Vanaf versie 10, alleen een wijziging in het eerste getal wordt beschouwd als een grote versie-upgrade (10.0-10.1 is bijvoorbeeld een _kleine_ versie-upgrade en 10 tot en met 11 is een _belangrijke_ versie-upgrade).

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

---
title: Beperkingen in Azure Database for PostgreSQL
description: Dit artikel wordt beschreven beperkingen in Azure Database voor PostgreSQL, zoals het aantal verbindingen en opties voor opslag-engine.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/30/2018
ms.openlocfilehash: 52dfac826de86f67b3143cce49c35088547c4b39
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171769"
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
|Geoptimaliseerd geheugen| 2| 150|
|Geoptimaliseerd geheugen| 4| 250|
|Geoptimaliseerd geheugen| 8| 480|
|Geoptimaliseerd geheugen| 16| 950|

Wanneer verbindingen de limiet overschrijdt, wordt de volgende fout:
> Onherstelbare fout: er al te veel clients

De Azure-systeem is vijf verbindingen voor het bewaken van de Azure Database for PostgreSQL-server vereist. 

## <a name="functional-limitations"></a>Functionele beperkingen
### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaling naar en van de Basic Prijscategorieën wordt momenteel niet ondersteund.
- Verkleinen server storage is momenteel niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine wordt momenteel niet ondersteund.

### <a name="subscription-management"></a>Abonnementsbeheer
- Dynamisch servers verplaatsen tussen abonnementen en resourcegroepen wordt momenteel niet ondersteund.

### <a name="vnet-service-endpoints"></a>VNet-service-eindpunten
- Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

### <a name="restoring-a-server"></a>Herstellen van een server
- Wanneer u de functie PITR, wordt de nieuwe server gemaakt met dezelfde prijzen laag schijfconfiguraties als de server die is gebaseerd op.
- De nieuwe server die is gemaakt tijdens een terugzetbewerking hoeft niet de firewallregels die aanwezig op de oorspronkelijke server was. Firewallregels moeten afzonderlijk worden ingesteld voor deze nieuwe server.
- Een verwijderde server herstelt, wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Inzicht in [wat beschikbaar is in elke prijscategorie](concepts-pricing-tiers.md)
- Meer informatie over [ondersteunde versies van de PostgreSQL-Database](concepts-supported-versions.md)
- Beoordeling [hoe u back-up en herstellen van een server in Azure Database for PostgreSQL via Azure portal](howto-restore-server-portal.md)

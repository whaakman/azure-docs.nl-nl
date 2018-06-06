---
title: Beperkingen in Azure-Database voor PostgreSQL
description: Dit artikel wordt beschreven beperkingen in Azure-Database voor PostgreSQL, zoals het aantal verbinding en opties voor opslag-engine.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/04/2018
ms.openlocfilehash: 5cd829236d8d8a58e68f7bf766790aa3f0cb656e
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757413"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Beperkingen in Azure-Database voor PostgreSQL
De volgende secties beschrijven de capaciteit en functionele limieten in de database-service.

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

Wanneer er verbindingen de limiet overschrijdt, wordt de volgende fout:
> Onherstelbare fout: er al te veel clients

Het Azure systeem vereist vijf verbindingen voor het bewaken van de Azure-Database voor PostgreSQL-server. 

## <a name="functional-limitations"></a>Functionele beperkingen
### <a name="scale-operations"></a>Schaalbewerkingen
1.  Dynamische schaalbaarheid van servers in de prijscategorie is momenteel niet ondersteund. Dat wil zeggen, overschakelen tussen lagen Basic, algemeen of geoptimaliseerd voor geheugen.
2.  Verkleinen storage server is momenteel niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine is momenteel niet ondersteund.

### <a name="subscription-management"></a>Abonnementsbeheer
- Dynamisch servers verplaatsen tussen abonnementen en resourcegroepen is momenteel niet ondersteund.

### <a name="point-in-time-restore-pitr"></a>Punt-in-time-herstel (PITR)
1.  Wanneer u de functie PITR gebruikt, wordt de nieuwe server gemaakt met dezelfde configuratie als de server die is gebaseerd op.
2.  Herstellen van een verwijderde server wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Begrijpen [wat is beschikbaar in elke prijscategorie](concepts-pricing-tiers.md)
- Meer informatie over [ondersteunde versies van PostgreSQL-Database](concepts-supported-versions.md)
- Bekijk [back-up en herstellen van een server in Azure-Database voor PostgreSQL met de Azure portal](howto-restore-server-portal.md)

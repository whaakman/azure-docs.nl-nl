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
ms.date: 03/20/2018
ms.openlocfilehash: 7e06cdba7c9c9f7e5c1d621e7421a18c342c0fdb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Beperkingen in Azure-Database voor PostgreSQL
De volgende secties beschrijven de capaciteit en functionele limieten in de database-service.

## <a name="pricing-tier-maximums"></a>Laag maximumwaarden prijzen
Azure PostgreSQL-Database heeft meerdere Prijscategorieën die u bij het maken van een server kiezen kunt uit. Zie voor meer informatie [prijscategorieën in Azure-Database voor PostgreSQL](concepts-pricing-tiers.md).  

Er is een maximum aantal verbindingen, compute-eenheden en opslag in elke prijscategorie als volgt: 

|Prijscategorie| COMPUTE generatie| vCore(s)| Maximum aantal verbindingen |
|---|---|---|---|
|Basic| Gen 4| 1| 50 |
|Basic| Gen 4| 2| 100 |
|Basic| Gen 5| 1| 50 |
|Basic| Gen 5| 2| 100 |
|Algemeen doel| Gen 4| 2| 150|
|Algemeen doel| Gen 4| 4| 250|
|Algemeen doel| Gen 4| 8| 480|
|Algemeen doel| Gen 4| 16| 950|
|Algemeen doel| Gen 4| 32| 1500|
|Algemeen doel| Gen 5| 2| 150|
|Algemeen doel| Gen 5| 4| 250|
|Algemeen doel| Gen 5| 8| 480|
|Algemeen doel| Gen 5| 16| 950|
|Algemeen doel| Gen 5| 32| 1500|
|Geoptimaliseerd geheugen| Gen 5| 2| 150|
|Geoptimaliseerd geheugen| Gen 5| 4| 250|
|Geoptimaliseerd geheugen| Gen 5| 8| 480|
|Geoptimaliseerd geheugen| Gen 5| 16| 950|

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

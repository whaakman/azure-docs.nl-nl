---
title: Beperkingen in Azure-Database voor PostgreSQL | Microsoft Docs
description: Beschrijft de beperkingen in Azure-Database voor PostgreSQL.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 6dbed1a834d74047178a9f996683d65520047e66
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Beperkingen in Azure-Database voor PostgreSQL
De Azure-Database voor PostgreSQL-service is in de openbare preview. De volgende secties beschrijven de capaciteit en functionele limieten in de database-service.

## <a name="service-tier-maximums"></a>Service Tier maximumwaarden
Azure PostgreSQL-Database heeft meerdere Servicelagen die u kiezen kunt uit bij het maken van een server. Zie voor meer informatie [begrijpen wat er beschikbaar is in elke servicelaag](concepts-service-tiers.md).  

Er is een maximum aantal verbindingen, compute-eenheden en opslag in elke servicelaag tijdens de preview service als volgt: 

| | |
| :------------------------- | :---------------- |
| **Maximum aantal verbindingen**        |                   |
| Basic 50 Compute-eenheden     | 55-verbindingen    |
| Basic 100 Compute-eenheden    | 105 verbindingen   |
| Standaard 100 Compute-eenheden | 150 verbindingen   |
| Standaard 200 Compute-eenheden | 250 verbindingen   |
| Standaard 400 Compute-eenheden | 480 verbindingen   |
| Standaard 800 Compute-eenheden | 950 verbindingen   |
| **Maximale Compute-eenheden**      |                   |
| Basisservicelaag         | 100 compute-eenheden |
| Standaardservicelaag      | 800 compute-eenheden |
| **Maximale opslag**            |                   |
| Basisservicelaag         | 1 TB              |
| Standaardservicelaag      | 1 TB              |

Het Azure systeem vereist vijf verbindingen voor het bewaken van de Azure-Database voor PostgreSQL-server. Wanneer er te veel verbindingen zijn bereikt, wordt de volgende fout:
> Onherstelbare fout: er al te veel clients


## <a name="preview-functional-limitations"></a>Functionele beperkingen Preview
### <a name="scale-operations"></a>Schaalbewerkingen
1.  Dynamische schaalbaarheid van servers in Servicelagen is momenteel niet ondersteund. Dat wil zeggen, schakelen tussen servicecategorieën Basic en Standard.
2.  Dynamische toename mogelijk op aanvraag van opslag op vooraf gemaakte server is momenteel niet ondersteund.
3.  Verkleinen storage server wordt niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine is momenteel niet ondersteund.

### <a name="subscription-management"></a>Abonnementsbeheer
- Dynamisch vooraf gemaakte servers verplaatsen tussen abonnement en resourcegroep is momenteel niet ondersteund.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
1.  Herstellen naar andere servicelaag en/of Compute-eenheden en de opslaggrootte is niet toegestaan.
2.  Herstellen van een uitgevallen server wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Begrijpen [wat is beschikbaar in elke prijscategorie](concepts-service-tiers.md)
- Begrijpen [ondersteunde versies van PostgreSQL-Database](concepts-supported-versions.md)
- Bekijk [Back-up en herstellen van een server in Azure-Database voor PostgreSQL met de Azure portal](howto-restore-server-portal.md)

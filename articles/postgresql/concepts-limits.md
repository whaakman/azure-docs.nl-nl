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
ms.date: 11/03/2017
ms.openlocfilehash: dbb88e033d5be73b7b069d69c095d8df2c1faf1b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Beperkingen in Azure-Database voor PostgreSQL
De Azure-Database voor PostgreSQL-service is in de openbare preview. De volgende secties beschrijven de capaciteit en functionele limieten in de database-service.

## <a name="service-tier-maximums"></a>Service Tier maximumwaarden
Azure PostgreSQL-Database heeft meerdere Servicelagen die u kiezen kunt uit bij het maken van een server. Zie voor meer informatie [begrijpen wat er beschikbaar is in elke servicelaag](concepts-service-tiers.md).  

Er is een maximum aantal verbindingen, compute-eenheden en opslag in elke servicelaag tijdens de preview service als volgt: 

| | |
| :------------------------- | :---------------- |
| **Maximum aantal verbindingen**        |                   |
| Basic 50 Compute-eenheden     | 50-verbindingen    |
| Basic 100 Compute-eenheden    | 100-verbindingen   |
| Standaard 100 Compute-eenheden | 200-verbindingen   |
| Standaard 200 Compute-eenheden | 300-verbindingen   |
| Standaard 400 Compute-eenheden | 400-verbindingen   |
| Standaard 800 Compute-eenheden | 500-verbindingen   |
| **Maximale Compute-eenheden**      |                   |
| Basisservicelaag         | 100 compute-eenheden |
| Standaardservicelaag      | 800 compute-eenheden |
| **Maximale opslag**            |                   |
| Basisservicelaag         | 1 TB              |
| Standaardservicelaag      | 1 TB              |

Wanneer er te veel verbindingen zijn bereikt, wordt de volgende fout:
> Onherstelbare fout: er al te veel clients

## <a name="preview-functional-limitations"></a>Functionele beperkingen Preview
### <a name="scale-operations"></a>Schaalbewerkingen
1.  Dynamische schaalbaarheid van servers in Servicelagen is momenteel niet ondersteund. Dat wil zeggen, schakelen tussen servicecategorieën Basic en Standard.
2.  Dynamische toename mogelijk op aanvraag van opslag op vooraf gemaakte server is momenteel niet ondersteund.
3.  Verkleinen storage server wordt niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine is momenteel niet ondersteund.

### <a name="subscription-management"></a>Beheer van abonnementen
- Dynamisch vooraf gemaakte servers verplaatsen tussen abonnement en resourcegroep is momenteel niet ondersteund.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
1.  Herstellen naar andere servicelaag en/of Compute-eenheden en de opslaggrootte is niet toegestaan.
2.  Herstellen van een uitgevallen server wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- Begrijpen [wat is beschikbaar in elke prijscategorie](concepts-service-tiers.md)
- Begrijpen [ondersteunde versies van PostgreSQL-Database](concepts-supported-versions.md)
- Bekijk [Back-up en herstellen van een server in Azure-Database voor PostgreSQL met de Azure portal](howto-restore-server-portal.md)

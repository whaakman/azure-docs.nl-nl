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
ms.date: 10/26/2017
ms.openlocfilehash: b3fba38cacf5b5abcdea7f0def8c1d39e653f0a8
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>Beperkingen in Azure-Database voor MySQL (Preview)
De Azure-Database voor de MySQL-service is in de openbare preview. De volgende secties beschrijven de capaciteit en functionele limieten in de database-service. Zie ook [algemene beperkingen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) van toepassing op de MySQL-database-engine.

## <a name="service-tier-maximums"></a>Service tier maximumwaarden
Azure MySQL-Database heeft meerdere Servicelagen kiezen bij het maken van een server. Zie voor meer informatie [begrijpen wat er beschikbaar is in elke servicelaag](concepts-service-tiers.md).  

Er is een maximum aantal verbindingen, Compute-eenheden en opslag in elke servicelaag tijdens de preview, als volgt: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maximum aantal verbindingen**        |                   |
| Basic 50 Compute-eenheden     | 50-verbindingen    |
| Basic 100 Compute-eenheden    | 100-verbindingen   |
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

## <a name="preview-functional-limitations"></a>Functionele beperkingen Preview

### <a name="scale-operations"></a>Schaalbewerkingen
- Dynamische schaalbaarheid van servers in Servicelagen is momenteel niet ondersteund. Dat wil zeggen, schakelen tussen servicecategorieën Basic en Standard.
- Dynamische toename mogelijk op aanvraag van opslag op vooraf gemaakte server is momenteel niet ondersteund.
- Verkleinen storage server wordt niet ondersteund.

### <a name="server-version-upgrades"></a>Server-versie-upgrades
- Automatische migratie tussen versies van de primaire database-engine is momenteel niet ondersteund.

### <a name="subscription-management"></a>Beheer van abonnementen
- Dynamisch vooraf gemaakte servers verplaatsen tussen abonnement en resourcegroep is momenteel niet ondersteund.

### <a name="point-in-time-restore"></a>Een punt in de tijd herstellen
- Herstellen naar andere servicelaag en/of Compute-eenheden en de opslaggrootte is niet toegestaan.
- Herstellen van een uitgevallen server wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
- [Wat is beschikbaar in elke servicelaag](concepts-service-tiers.md)
- [Ondersteunde versies van de MySQL-database](concepts-supported-versions.md)

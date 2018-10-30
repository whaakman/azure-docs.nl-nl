---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/29/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 314f29a1135e355597e890b72ef3c0b7372194e6
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226654"
---
### <a name="sql-databases"></a>SQL Databases

|  |  |
|---------|---------|
| [Toegestane SQL DB-SKU's](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Zorgt ervoor dat SQL-databases een goedgekeurde SKU gebruiken. U geeft een matrix op van toegestane SKU-id's of een matrix van toegestane SKU-namen. |
| [Instelling dreigingsdetectieniveau DB controleren](../articles/governance/policy/samples/audit-db-threat-det-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Versleuteling van SQL-database controleren](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld. |
| [Auditinstellingsniveau SQL DB controleren](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL-database overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld.  |
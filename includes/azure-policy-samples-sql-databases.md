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
ms.openlocfilehash: 7bf885f2ab81e5d86878d2b0b53f4e98b8aedd9a
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318240"
---
### <a name="sql-databases"></a>SQL Databases

|  |  |
|---------|---------|
| [Toegestane SQL DB-SKU's](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Zorgt ervoor dat SQL-databases een goedgekeurde SKU gebruiken. U geeft een matrix op van toegestane SKU-id's of een matrix van toegestane SKU-namen. |
| [Instelling dreigingsdetectieniveau DB controleren](../articles/governance/policy/samples/audit-db-threat-detection-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Versleuteling van SQL-database controleren](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld. |
| [Auditinstellingsniveau SQL DB controleren](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL-database overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld.  |
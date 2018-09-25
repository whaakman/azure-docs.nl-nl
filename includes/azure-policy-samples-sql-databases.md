---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 70128735aef64d273c63236a13b0ae28edb6077d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003645"
---
### <a name="sql-databases"></a>SQL Databases

|  |  |
|---------|---------|
| [Toegestane SQL DB-SKU's](../articles/governance/policy/samples/allowed-sql-db-skus.md) | Zorgt ervoor dat SQL-databases een goedgekeurde SKU gebruiken. U geeft een matrix op van toegestane SKU-id's of een matrix van toegestane SKU-namen. |
| [Instelling dreigingsdetectieniveau DB controleren](../articles/governance/policy/samples/audit-db-threat-det-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Versleuteling van SQL-database controleren](../articles/governance/policy/samples/sql-database-encryption-audit.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld. |
| [Auditinstellingsniveau SQL DB controleren](../articles/governance/policy/samples/audit-sql-db-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL-database overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld.  |
| [Controlestatus van transparante gegevensversleuteling](../articles/governance/policy/samples/audit-trans-data-enc-status.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld.  |
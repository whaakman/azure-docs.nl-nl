---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 50a6388a95bce14bc9af7e0c9a5387e148f6fa73
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664493"
---
### <a name="sql-databases"></a>SQL Databases

|  |  |
|---------|---------|
| [Toegestane SQL DB-SKU's](../articles/azure-policy/scripts/allowed-sql-db-skus.md) | Zorgt ervoor dat SQL-databases een goedgekeurde SKU gebruiken. U geeft een matrix op van toegestane SKU-id's of een matrix van toegestane SKU-namen. |
| [Instelling dreigingsdetectieniveau DB controleren](../articles/azure-policy/scripts/audit-db-threat-det-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Versleuteling van SQL-database controleren](../articles/azure-policy/scripts/sql-database-encryption-audit.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld. |
| [Auditinstellingsniveau SQL DB controleren](../articles/azure-policy/scripts/audit-sql-db-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL-database overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld.  |
| [Controlestatus van transparante gegevensversleuteling](../articles/azure-policy/scripts/audit-trans-data-enc-status.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld.  |
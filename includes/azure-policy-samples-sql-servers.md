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
ms.openlocfilehash: 298b92205e73b3623db02fb1dd803edac8379700
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664487"
---
### <a name="sql-servers"></a>SQL-servers

|  |  |
|---------|---------|
| [Op een Azure Active Directory-beheerder controleren](../articles/azure-policy/scripts/audit-no-aad-admin.md) | Controleert of er een Azure Active Directory-beheerder is toegewezen aan de SQL Server. |
| [Instelling dreigingsdetectieniveau server controleren](../articles/azure-policy/scripts/audit-sql-ser-threat-det-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Auditinstellingen SQL Server controleren](../articles/azure-policy/scripts/sql-server-audit.md) | Controleert SQL Server op basis van of de auditinstellingen zijn ingeschakeld. |
| [Auditinstellingsniveau SQL Server controleren](../articles/azure-policy/scripts/audit-sql-ser-leve-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL Server overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld. |
| [SQL Server-versie 12.0 vereisen](../articles/azure-policy/scripts/req-sql-12.md) | Vereist dat SQL Server-versie 12.0 wordt gebruikt.  |
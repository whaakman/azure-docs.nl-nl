---
title: Azure SQL-Database app voor meerdere tenants voorbeeld - Wingtip SaaS | Microsoft Docs
description: Informatie over met behulp van een voorbeeld van multitenant-toepassing die gebruikmaakt van Azure SQL Database, het Wingtip SaaS-voorbeeld
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: 1c16ea44418d99ee1f80a7d0ef7a3e5b3f118f46
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432331"
---
# <a name="introduction-to-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Inleiding tot een multitenant SaaS-app die gebruikmaakt van het patroon van de database-per-tenant met SQL Database

De Wingtip SaaS-toepassing is een voorbeeld-app voor meerdere tenants. De app maakt gebruik van de SaaS-toepassingspatroon database-per-tenant voor meerdere tenants. De app worden gepresenteerd functies van Azure SQL Database die SaaS-scenario's met behulp van verschillende ontwerp- en -beheerpatronen voor SaaS. Als u wilt snel aan de slag, wordt de Wingtip SaaS-app geïmplementeerd in minder dan vijf minuten.

Toepassing source code en scripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Voordat u begint, Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets-management-scripts.

## <a name="application-architecture"></a>Toepassingsarchitectuur

De Wingtip SaaS-app maakt gebruik van de database-per-tenant-model. Elastische SQL-groepen wordt gebruikt voor een maximale efficiency. Voor het inrichten en de toewijzing van tenants tot hun gegevens, wordt een catalogusdatabase gebruikt. De belangrijkste Wingtip SaaS-toepassing maakt gebruik van een pool met drie voorbeeldtenants, plus de catalogusdatabase. De catalogus en tenant-servers zijn ingericht met DNS-aliassen. Deze aliassen worden gebruikt voor het onderhouden van een verwijzing naar de actieve resources die worden gebruikt door de Wingtip-toepassing. Deze aliassen zijn bijgewerkt om te verwijzen naar de recovery-resources in de disaster recovery-zelfstudies. Voltooien van veel van de resultaten van de Wingtip SaaS-zelfstudies in uitbreidingen op de eerste implementatie. Invoegtoepassingen, zoals analytische databases en tussen databases Schemabeheer zijn geïntroduceerd.


![Wingtip SaaS-architectuur](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Als u de zelfstudies doorlopen en met de app werken, zich richten op de SaaS-patronen zoals ze betrekking op de gegevenslaag hebben. Met andere woorden, richt u op de gegevenslaag en de app zelf geen overanalyze. Inzicht krijgen in de implementatie van deze SaaS-patronen is essentieel voor het implementeren van deze patronen in uw toepassingen. U kunt ook welke wijzigingen nodig zijn voor uw specifieke zakelijke vereisten.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL Database Wingtip SaaS-zelfstudies

Nadat u de app hebt geïmplementeerd, bekijk de volgende zelfstudies die op de eerste implementatie voortbouwen. Deze zelfstudies verkennen veelvoorkomende SaaS-patronen die van ingebouwde functies van SQL Database, Azure SQL Data Warehouse en andere Azure-services gebruikmaken. Zelfstudies zijn PowerShell-scripts met gedetailleerde uitleg. De uitleg vereenvoudigen begrijpen en de implementatie van de dezelfde SaaS-beheerpatronen in uw toepassingen.


| Zelfstudie | Description |
|:--|:--|
| [Richtlijnen en tips voor het SQL-Database multitenant SaaS-app-voorbeeld](saas-tenancy-wingtip-app-guidance-tips.md) | Downloaden en uitvoeren van PowerShell-scripts om voor te bereiden van onderdelen van de toepassing. |
|[De Wingtip SaaS-toepassing implementeren en verkennen](saas-dbpertenant-get-started-deploy.md)|  Implementeren en verkennen van de Wingtip SaaS-toepassing met uw Azure-abonnement. |
|[Tenants inrichten en catalogiseren](saas-dbpertenant-provision-and-catalog.md)| Meer informatie over hoe de toepassing verbinding maakt met tenants met behulp van een catalogusdatabase, en hoe tenants in de catalogus worden toegewezen aan hun gegevens. |
|[Prestaties controleren en beheren](saas-dbpertenant-performance-monitoring.md)| Informatie over het gebruik controlefuncties van SQL-Database en waarschuwingen kunt instellen wanneer drempelwaarden worden overschreden. |
|[Bewaken met Azure Monitor-Logboeken](saas-dbpertenant-log-analytics.md) | Meer informatie over het gebruik van [logboeken van Azure Monitor](../log-analytics/log-analytics-overview.md) voor het bewaken van grote hoeveelheden van resources in meerdere pools. |
|[Een enkele tenant herstellen](saas-dbpertenant-restore-single-tenant.md)| Leer hoe u een tenantdatabase herstellen naar een eerdere in-time. U leert ook hoe u herstellen met een parallelle database, die de database van de bestaande tenant online verlaat. |
|[Database-tenantschema beheren](saas-tenancy-schema-management.md)| Informatie over het schema en het bijwerken van referentiegegevens in alle tenantdatabases. |
|[Cross-tenant gedistribueerde query's uitvoeren](saas-tenancy-cross-tenant-reporting.md) | Maken van een ad-hoc analytics-database, en realtime gedistribueerde query's uitvoeren voor alle tenants.  |
|[Analyses uitvoeren op gegevens van de uitgepakte tenant](saas-tenancy-tenant-analytics.md) | Extraheer de gegevens van de tenant naar een analytics-database of het datawarehouse voor offline analysequery's. |


## <a name="next-steps"></a>Volgende stappen

- [Algemene richtlijnen en tips voor wanneer u implementeert en gebruik het voorbeeld van Wingtip Tickets SaaS-app](saas-tenancy-wingtip-app-guidance-tips.md)
- [De Wingtip SaaS-toepassing implementeren](saas-dbpertenant-get-started-deploy.md)

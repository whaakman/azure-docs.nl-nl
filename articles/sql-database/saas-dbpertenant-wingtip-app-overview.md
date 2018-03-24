---
title: Azure SQL Database multitenant app voorbeeld - Wingtip SaaS | Microsoft Docs
description: Meer informatie over met behulp van een multitenant voorbeeldtoepassing die gebruikmaakt van Azure SQL Database, het Wingtip SaaS-voorbeeld
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: 563d82076721a669069ba3e36df84a050188813c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Inleiding tot een multitenant SaaS-app die gebruikmaakt van het patroon database per tenant met SQL Database

De Wingtip SaaS-toepassing is een voorbeeld van multitenant-app. De app gebruikmaakt van het patroon database per tenant SaaS-toepassing voor het onderhouden van meerdere tenants. De app gepresenteerd functies van Azure SQL Database die SaaS-scenario's inschakelen met behulp van verschillende SaaS-ontwerp- en patronen. Als u snel gebruiksklaar, wordt de Wingtip SaaS-app geïmplementeerd in minder dan vijf minuten.

Bron en het beheer toepassingsscripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Voordat u begint, Zie de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de Wingtip Tickets management scripts deblokkeren.

## <a name="application-architecture"></a>Toepassingsarchitectuur

De app Wingtip SaaS maakt gebruik van de database per tenant. Elastische pools SQL wordt gebruikt om het te optimaliseren. Voor het inrichten en de toewijzing van tenants tot hun gegevens, wordt de catalogusdatabase van een gebruikt. De kern Wingtip SaaS-toepassing maakt gebruik van een groep met drie voorbeeld tenants, plus de catalogusdatabase. Veel van de resultaten van de zelfstudies Wingtip SaaS in invoegtoepassingen voor de initiële implementatie is voltooid. Invoegtoepassingen zoals analytische databases en management cross-databaseschema worden geïntroduceerd.


![Wingtip SaaS-architectuur](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Als u de zelfstudies doorlopen en met de app werkt, zich richten op de SaaS-patronen die betrekking heeft op de gegevenslaag. Met andere woorden, ligt de nadruk op de gegevenslaag en de app zelf niet overanalyze. Informatie over de implementatie van deze SaaS patronen essentieel is voor het implementeren van deze patronen in uw toepassingen. U kunt ook de benodigde wijzigingen voor uw specifieke bedrijfsvereisten.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL Database Wingtip SaaS-zelfstudies

Nadat u de app hebt geïmplementeerd, gebruik de volgende zelfstudies die zijn gebaseerd op de eerste implementatie. Deze zelfstudies verkennen algemene SaaS-patronen die van de ingebouwde functies van SQL-Database, Azure SQL Data Warehouse en andere Azure-services gebruikmaken. Zelfstudies bevatten PowerShell-scripts met gedetailleerde uitleg. De uitleg vereenvoudigen begrip en de implementatie van dezelfde SaaS-beheer patronen in uw toepassingen.


| Zelfstudie | Beschrijving |
|:--|:--|
| [Richtlijnen en tips voor het SQL-Database multitenant SaaS-app-voorbeeld](saas-tenancy-wingtip-app-guidance-tips.md) | Downloaden en uitvoeren van PowerShell-scripts voor het voorbereiden van de onderdelen van de toepassing. |
|[Implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md)|  Implementeer en Verken de Wingtip SaaS-toepassing met uw Azure-abonnement. |
|[Inrichten en catalogus tenants](saas-dbpertenant-provision-and-catalog.md)| Informatie over hoe de toepassing verbinding maakt met tenants met behulp van een catalogusdatabase, en hoe tenants in de catalogus worden toegewezen aan hun gegevens. |
|[Bewaken en beheren van prestaties](saas-dbpertenant-performance-monitoring.md)| Informatie over het gebruik van de controlemogelijkheden van SQL-Database en stel meldingen wanneer drempelwaarden worden overschreden. |
|[Monitor met Azure-logboekanalyse (Operations Management Suite)](saas-dbpertenant-log-analytics.md) | Informatie over het gebruik [logboekanalyse](../log-analytics/log-analytics-overview.md) voor het bewaken van grote hoeveelheden resources tussen meerdere pools. |
|[Herstellen van een enkele tenant](saas-dbpertenant-restore-single-tenant.md)| Ontdek hoe u een tenant-database naar een eerder tijdstip herstellen. Ook informatie over het om een parallelle database en de bestaande tenant-database online blijft te herstellen. |
|[Tenant-databaseschema beheren](saas-tenancy-schema-management.md)| Informatie over het schema en het bijwerken van referentiegegevens voor alle databases van de tenant. |
|[Cross-tenant gedistribueerde query's uitvoeren](saas-tenancy-cross-tenant-reporting.md) | Maken van een ad-hoc analytics-database en realtime gedistribueerde query's uitvoeren op alle tenants.  |
|[Analytics worden uitgevoerd op gegevens van de uitgepakte tenant](saas-tenancy-tenant-analytics.md) | Tenant-gegevens ophalen naar een analytics database of de data warehouse voor offline analytics query's. |


## <a name="next-steps"></a>Volgende stappen

- [Algemene richtlijnen en tips wanneer u implementeert en gebruik het voorbeeld Wingtip Tickets SaaS-app](saas-tenancy-wingtip-app-guidance-tips.md)
- [De Wingtip SaaS-toepassing implementeren](saas-dbpertenant-get-started-deploy.md)

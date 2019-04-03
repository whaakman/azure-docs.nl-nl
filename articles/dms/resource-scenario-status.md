---
title: Migratie-scenario databasestatus | Microsoft Docs
description: Meer informatie over de status van de migratie-scenario's ondersteund door Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 7017566092530dce2359d45314ac00dca63b8ad0
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58886230"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Status van het migratiescenario's ondersteund door Azure Database Migration Service
Azure Database Migration Service is ontworpen ter ondersteuning van verschillende migratiescenario's (bron-/ doelparen) voor zowel offline (eenmalig) en online (doorlopende synchronisatie). De dekking scenario is geleverd door de Azure Database Migration Service wordt uitgebreid na verloop van tijd. Nieuwe scenario's er worden regelmatig toegevoegd. In dit artikel geeft migratiescenario's die momenteel worden ondersteund door Azure Database Migration Service en de status (Private Preview, Public Preview of algemeen beschikbaar) voor elk scenario.

## <a name="offline-versus-online-migrations"></a>Offline en online migraties
Met Azure Database Migration Service, kunt u een offline of een online migratie uitvoeren. Met *offline* migraties, downtime van toepassingen wordt gestart op het moment dat de migratie begint. Als u wilt beperken uitvaltijd voor de benodigde tijd voor het overzetten van naar de nieuwe omgeving wanneer de migratie is voltooid, gebruikt u een *online* migratie. Het is raadzaam om te testen een offlinemigratie om te bepalen of de uitvaltijd toegestaan is. Als dat niet het geval is, moet u een online-migratie.

## <a name="migration-scenario-status"></a>Status van de migratie-scenario
De status van het migratiescenario's ondersteund door Azure Database Migration Service is afhankelijk van tijd. Over het algemeen scenario's zijn voor het eerst uitgebracht **Private Preview**. Die deel uitmaken van beperkte Preview-versie moet klanten om in te dienen een nominatie via de [DMS voorbeeldsite](https://aka.ms/dms-preview). Na de Private Preview, verandert de status scenario in **Public Preview**. Azure Database Migration Service-gebruikers kunnen uitproberen in scenario's voor migratie in openbare Preview rechtstreeks vanuit de gebruikersinterface. Geen aanmelding is vereist.  Migratiescenario's in openbare preview-versie is echter mogelijk niet beschikbaar in alle regio's en mogelijk aanvullende wijzigingen voordat de definitieve versie ondergaan. Na de openbare Preview, verandert de status scenario in **algemeen beschikbaar**. In het algemeen beschikbaar is (GA) is de status van de definitieve versie en de functionaliteit is voltooid en toegankelijk voor alle gebruikers. 

## <a name="migration-scenario-support"></a>Ondersteuning voor scenario
De volgende tabellen ziet welke migratiescenario's worden ondersteund bij het gebruik van Azure Database Migration Service.

> [!NOTE]
> Als u een scenario ondersteund hieronder niet wordt weergegeven in de gebruikersinterface, neemt u contact op met de [gegevens Migratieteam](mailto:datamigrationteam@microsoft.com) voor meer informatie.

> [!IMPORTANT]
> Voor scenario's die momenteel worden ondersteund door Azure Database Migration Service in Private Preview, raadpleegt u de [DMS voorbeeldsite](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Ondersteuning voor offline (eenmalig) worden gemigreerd
De volgende tabel ziet u Azure Database Migration Service-ondersteuning voor migraties van offline.

| Doel  | Bron | Ondersteuning | Status |
| ------------- | ------------- | :-------------: | :-------------: |
| **Azure SQL Database** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL |  |  |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL |  |  |
|   | Oracle |  |   |
| **Azure SQL-VM** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | Openbare preview |
| **Azure DB voor MySQL** | MySQL |   |   |
|   | RDS MySQL |   |   |
| **Azure DB voor PostgreSQL** | PostgreSQL |  |
|  | RDS PostgreSQL |   |   |

### <a name="online-continuous-sync-migration-support"></a>Ondersteuning voor online (doorlopende synchronisatie)
De volgende tabel ziet u Azure Database Migration Service ondersteuning, Public Preview of algemeen beschikbaar voor online migraties.

| Doel  | Bron | Ondersteuning | Status |
| ------------- | ------------- | :-------------: | :-------------: |
| **Azure SQL Database** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL | ✔ | Algemene beschikbaarheid |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL | ✔ | Algemene beschikbaarheid |
|   | Oracle | ✔ | Beperkte Preview-versie |
| **Azure SQL-VM** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | Openbare preview |
| **Azure DB voor MySQL** | MySQL | ✔ | Algemene beschikbaarheid |
|   | RDS MySQL | ✔ | Algemene beschikbaarheid |
| **Azure DB voor PostgreSQL** | PostgreSQL | ✔ | Algemene beschikbaarheid |
|   | RDS PostgreSQL | ✔ | Algemene beschikbaarheid |
|   | Oracle | ✔ | Beperkte Preview-versie |

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van Azure Database Migration Service en de regionale beschikbaarheid, [wat is de Azure Database Migration Service](dms-overview.md).

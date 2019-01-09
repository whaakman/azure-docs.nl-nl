---
title: Migratie-scenario databasestatus | Microsoft Docs
description: Meer informatie over de status van de migratie-scenario's ondersteund door de Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/08/2019
ms.openlocfilehash: 9e153cca321e94233cfda2a03cf52ba85a0f6b02
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54102802"
---
# <a name="status-of-migration-scenarios-supported-by-the-azure-database-migration-service"></a>Status van het migratiescenario's ondersteund door de Azure Database Migration Service
Azure Database Migration Service is ontworpen ter ondersteuning van verschillende migratiescenario's (bron-/ doelparen) voor zowel offline (eenmalig) en online (doorlopende synchronisatie). De dekking scenario is geleverd door de Azure Database Migration Service wordt uitgebreid na verloop van tijd. Nieuwe scenario's er worden regelmatig toegevoegd. In dit artikel geeft de migratiescenario's die momenteel worden ondersteund door de Azure Database Migration Service en de status (privé [of beperkte] Preview, Public Preview of algemeen beschikbaar) of elk scenario.

## <a name="offline-versus-online-migrations"></a>Offline en online migraties
Wanneer u databases naar Azure migreren met behulp van de Azure Database Migration Service, kunt u een offline of een online migratie uitvoeren. Met *offline* migraties, downtime van toepassingen wordt gestart op het moment dat de migratie begint. Voor *online* migraties uitvaltijd is beperkt tot de benodigde tijd voor het overzetten van naar de nieuwe omgeving wanneer de migratie is voltooid. Het is raadzaam om te testen een offlinemigratie om te bepalen of de uitvaltijd toegestaan is. Als dat niet het geval is, moet u een online migratie uit te voeren.

## <a name="migration-scenario-status"></a>Status van de migratie-scenario
De status van elke migratiescenario ondersteund door de Azure Database Migration Service is afhankelijk van tijd. Over het algemeen scenario's zijn voor het eerst uitgebracht **Private Preview**, en u profiteert van de functionaliteit is vereist dat een klant een nominatie via indienen de [DMS voorbeeldsite](https://aka.ms/dms-preview). Als beperkte Preview-versie is voltooid, verandert de status van het scenario in **Public Preview**. Alle Azure Database Migration Service-gebruikers kunnen profiteren van migratiescenario's is beschikbaar in openbare Preview. Echter het migratiescenario mogelijk niet beschikbaar in alle regio's en de functionaliteit mogelijk aanvullende wijzigingen voordat de definitieve versie ondergaan. Als verandert in een migratiescenario **algemeen beschikbaar**, de status van de uiteindelijke, uitgebracht, de functionaliteit is voltooid en toegankelijk voor alle gebruikers van Azure Database Migration Service. 

## <a name="migration-scenario-support"></a>Ondersteuning voor scenario

De volgende tabellen ziet welke migratiescenario's worden ondersteund bij het gebruik van de Azure Database Migration Service.

> [!NOTE]
> Als u een scenario ondersteund hieronder niet wordt weergegeven in de gebruikersinterface, neemt u contact op met de [gegevens Migratieteam](mailto:datamigrationteam@microsoft.com) voor meer informatie.

### <a name="offline-one-time-migration-support"></a>Ondersteuning voor offline (eenmalig) worden gemigreerd
De volgende tabel ziet u Azure Database Migration Service ondersteuning voor offline migraties.

| Doel  | Bron | Ondersteuning |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | EXTERN BUREAUBLAD-SERVICES SQL  |  ✔ |
|   | Oracle  |   |
| **Azure SQL DB MI**  | SQL Server  | ✔ |
|   | EXTERN BUREAUBLAD-SERVICES SQL  | ✔ |
|   | Oracle  | ✔  |
| **Azure SQL-VM**  | SQL Server  | ✔ |
|   | Oracle  |   |
| **Cosmos DB**  | MongoDB  | ✔ |
| **Azure DB voor MySQL**  | MySLQ  |  |
|   | RDS MySQL  |  |
| **Azure database voor PostgresSQL**  | PostgreSQL |  |
|  | Extern bureaublad-services PostgreSQL  |  |

### <a name="online-continuous-sync-migration-support"></a>Ondersteuning voor online (doorlopende synchronisatie)
De volgende tabel ziet u Azure Database Migration Service ondersteuning voor online migraties.

| Doel  | Bron | Ondersteuning |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | EXTERN BUREAUBLAD-SERVICES SQL  |   |
|   | Oracle  |  ✔ |
| **Azure SQL DB MI**  | SQL Server  | ✔ |
|   | EXTERN BUREAUBLAD-SERVICES SQL  |  |
|   | Oracle  | ✔  |
| **Azure SQL-VM**  | SQL Server  |   |
|   | Oracle  | ✔  |
| **Cosmos DB**  | MongoDB  | ✔ |
| **Azure DB voor MySQL**  | MySLQ  | ✔ |
|   | RDS MySQL  | ✔ |
| **Azure database voor PostgresSQL**  | PostgreSQL | ✔ |
|  | Extern bureaublad-services PostgreSQL  | ✔ |

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de Azure Database Migration Service en de regionale beschikbaarheid [wat is de Azure Database Migration Service](dms-overview.md). 

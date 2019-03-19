---
title: SQL Data Warehouse classificatie | Microsoft Docs
description: Richtlijnen voor het gebruik van bestandsclassificatie voor het beheren van de urgentie van een gelijktijdigheid van taken en rekenresources voor query's in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: bcc09095955a28bde3ed999f23180e08485543fc
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "57994006"
---
# <a name="sql-data-warehouse-workload-classification-preview"></a>SQL Data Warehouse workload classificatie (Preview)

In dit artikel wordt uitgelegd dat het proces van SQL Data Warehouse workload classificatie van het toewijzen van een resourceklasse en urgentie op binnenkomende aanvragen.

> [!Note]
> Classificatie van de werkbelasting is beschikbaar in SQL Data Warehouse Gen2.

## <a name="classification"></a>Classificatie

> [!Video https://www.youtube.com/embed/QcCRBAhoXpM]

Workload management classificatie kunt werkbelasting beleid moet worden toegepast op aanvragen via het toewijzen van [resourceklassen](resource-classes-for-workload-management.md#what-are-resource-classes) en [belang](sql-data-warehouse-workload-importance.md).

Er zijn veel manieren om te classificeren datawarehousing-workloads, is de eenvoudigste en meest voorkomende classificatie laden en query's uitvoeren. Laden van gegevens met insert, update en delete-instructies.  U de gegevens opvragen met selecteert. Een datawarehouse-oplossing hebben vaak een beleid voor werkbelasting voor de load-activiteit, zoals het toewijzen van een hogere resourceklasse met meer resources. Een beleid voor verschillende werkbelastingen kan toepassen op query's, zoals een lagere prioriteit ten opzichte van voor het laden van activiteiten.

U kunt ook uw workloads laden en query subclassify. Subclassificatie biedt u meer controle over uw workloads. Querywerkbelastingen kunnen bijvoorbeeld bestaan uit de kubus wordt vernieuwd, dashboard query's of ad-hocquery's. U kunt elk van deze query-workloads met een andere resource-klassen of instellingen van belang te classificeren. Belasting kan ook profiteren van subclassificatie. Grote transformaties kunnen worden toegewezen aan grotere resourceklassen. Hogere prioriteit kan worden gebruikt om ervoor te zorgen sleutel verkoopgegevens loader voordat weergegevens of een sociale gegevensfeed.

## <a name="classification-process"></a>Classificatieproces

Classificatie in SQL Data Warehouse vandaag wordt bereikt door het toewijzen van gebruikers aan een rol heeft een bijbehorende resourceklasse toegewezen met behulp van die [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). De mogelijkheid om aan te geven van een aanmelding met een resourceklasse aanvragen is beperkt met deze functie. Een uitgebreidere methode voor de classificatie is nu beschikbaar met de [WERKBELASTING classificatie maken](/sql/t-sql/statements/create-workload-classifier-transact-sql) syntaxis.  Met de volgende syntaxis kunnen gebruikers van SQL Data Warehouse belang en een resourceklasse toewijzen aan aanvragen.  

> [!NOTE]
> Classificatie wordt geëvalueerd op basis van per aanvraag. Anders kunnen meerdere aanvragen in één sessie worden geclassificeerd.

## <a name="classification-precedence"></a>Prioriteit van classificatie

Als onderdeel van het classificatieproces voor is prioriteit om te bepalen welke resourceklasse is toegewezen. Classificatie op basis van een databasegebruiker heeft voorrang op lidmaatschap van de rol. Als u een classificatie waarbij een gebruiker van de gebruiker a database wordt toegewezen aan de bronklasse mediumrc maken. Vervolgens worden de RoleA databaserol (van die gebruiker a is een lid) toegewezen aan de bronklasse largerc. De classificatie die een gebruiker van de database wordt toegewezen aan de bronklasse mediumrc voorrang krijgt boven de classificatie die de databaserol RoleA wordt toegewezen aan de bronklasse largerc.

Als een gebruiker lid is van meerdere functies met verschillende resourceklassen toegewezen of in meerdere classificaties overeenkomen, krijgt de gebruiker de toewijzing van de hoogste resource-klasse.  Dit gedrag is consistent zijn met bestaande toewijzingsgedrag van de resource-klasse.

## <a name="system-classifiers"></a>Systeem classificaties

Classificatie van de werkbelasting heeft system belasting classificaties. De classificaties system bestaande rollidmaatschappen van de resource-klasse worden toegewezen aan resource klasse resourcetoewijzingen met normale prioriteit. Systeem classificaties kunnen niet worden verwijderd. Als u system classificaties, kunt u uitvoeren de onder de query:

```sql
SELECT * FROM sys.workload_management_workload_classifiers where classifier_id <= 12
```

## <a name="mixing-resource-class-assignments-with-classifiers"></a>Resource met een combinatie van toewijzingen van de klasse met classificaties

Systeem classificaties gemaakt namens bieden een eenvoudige manier om te migreren naar classificatie van de werkbelasting. Met behulp van de rol van klassetoewijzingen resource met classificatie prioriteit, kan leiden tot misclassification voordat u begint te maken van nieuwe classificaties met urgentie.

Houd rekening met het volgende scenario:

•An bestaande datawarehouse heeft een databasegebruiker die dbauser aan de resourceklasserol largerc toegewezen. De toewijzing van de resource-klasse is met sp_addrolemember uitgevoerd.
•Het datawarehouse is nu bijgewerkt met beheer van de werkbelasting.
Slechts test u de nieuwe classificatie syntaxis van de databaserol DBARole (dit DBAUser is een lid van), heeft een classificatie voor hen deze toe te wijzen aan mediumrc en hoge urgentie hebt gemaakt.
•When DBAUser zich aanmeldt en een query wordt uitgevoerd, wordt de query worden toegewezen aan largerc. Omdat een gebruiker, hebben voorrang boven het lidmaatschap van een rol.

Ter vereenvoudiging van het oplossen van problemen misclassification, raden wij dat u resource-klassetoewijzingen rol verwijderen bij het maken van classificaties van de werkbelasting.  De onderstaande code retourneert bestaande resource rollidmaatschappen klasse.  Voer [sp_droprolemember](/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql) geretourneerd voor de lidnaam van elk van de bijbehorende resourceklasse.

```sql
SELECT  r.name AS [Resource Class]
,       m.name AS membername
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r ON rm.role_principal_id = r.principal_id
JOIN    sys.database_principals AS m ON rm.member_principal_id = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc','staticrc10','staticrc20','staticrc30','staticrc40','staticrc50','staticrc60','staticrc70','staticrc80');

--for each row returned run
sp_droprolemember ‘[Resource Class]’, membername
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over SQL Data Warehouse workload classificatie en urgentie [maken van een classificatie van de werkbelasting](quickstart-create-a-workload-classifier-tsql.md) en [SQL Data Warehouse belang](sql-data-warehouse-workload-importance.md). Zie [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) om query's en het belang toegewezen weer te geven.

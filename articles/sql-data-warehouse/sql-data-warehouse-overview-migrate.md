---
title: Uw oplossing migreren naar SQL Data Warehouse | Microsoft Docs
description: Hulp bij de migratie naar Azure SQL Data Warehouse-platform om uw oplossing.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: 464f3768ec6bd679513e7817d1b5185787094044
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461302"
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a>Uw oplossing migreren naar Azure SQL Data Warehouse
Zie wat betrokken bij een bestaande databaseoplossing migreren naar Azure SQL Data Warehouse. 

## <a name="profile-your-workload"></a>Profileren van uw workload
Voordat u migreert, die u wilt worden dat bepaalde SQL Data Warehouse is de juiste oplossing voor uw workload. SQL Data Warehouse is een gedistribueerd systeem dat is ontworpen om analyses uitvoeren op grote hoeveelheden gegevens.  Migreren naar SQL Data Warehouse, moet enkele ontwerpwijzigingen die niet zijn te hard om te begrijpen, maar kunnen even duren om te implementeren. Als uw bedrijf een enterprise-klasse datawarehouse vereist, zijn de voordelen zeker de moeite waard. Als u de kracht van SQL Data Warehouse hoeft, is het echter voordeliger SQL Server of Azure SQL Database te gebruiken.

SQL Data Warehouse kunt u overwegen wanneer u:
- Hebt u een of meer Terabytes aan gegevens
- Wilt u analyses uitvoeren op grote hoeveelheden gegevens
- Moet u de mogelijkheid om reken- en schalen 
- Wilt u kosten besparen door rekenresources onderbreken wanneer u ze niet nodig.

Gebruik geen SQL Data Warehouse voor operationele (OLTP)-werkbelastingen met:
- Hoge frequentie lees- en schrijfbewerkingen
- Groot aantal singleton wordt geselecteerd
- Grote volumes aan één rij invoegen
- Rij moet verwerken
- Niet-compatibele indeling (JSON, XML)


## <a name="plan-the-migration"></a>De migratie plannen

Nadat u hebt besloten om een bestaande oplossing migreren naar SQL Data Warehouse, is het belangrijk dat u van plan bent de migratie voordat u aan de slag. 

Een doel van de planning is om te controleren of dat uw gegevens, het tabelschema en de code zijn compatibel met SQL Data Warehouse. Er zijn enkele verschillen compatibiliteit tussen uw huidige systeem- en SQL Data Warehouse omzeilen. Bovendien kunt migreren van grote hoeveelheden gegevens naar Azure neemt tijd. Zorgvuldige planning versnelt het ophalen van uw gegevens naar Azure. 

Een ander doel van de planning is het ontwerp aanpassingen om te controleren of dat uw oplossing wordt gebruikgemaakt van de hoge queryprestaties die SQL Data Warehouse is ontworpen om te bieden. Het ontwerpen van datawarehouses voor schaal, introduceert verschillende ontwerp patronen en dus traditionele benaderingen niet altijd de beste. Hoewel u ontwerpen aanpassingen na de migratie doorvoeren kunt, wordt sneller wijzigingen rechtstreeks in het proces op een later tijdstip opgeslagen.

Als u wilt uitvoeren van een geslaagde migratie, moet u het tabelschema, uw code en uw gegevens te migreren. Zie voor meer informatie over deze migratieonderwerpen:

-  [Uw schema migreren](sql-data-warehouse-migrate-schema.md)
-  [Uw code migreren](sql-data-warehouse-migrate-code.md)
-  [Migreer uw gegevens](sql-data-warehouse-migrate-data.md). 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a>Volgende stappen
De CAT (Customer Advisory Team) heeft ook enkele geweldige SQL Data Warehouse-instructie, die ze via blogs publiceren.  Kijk eens hun artikel [migreren van gegevens naar Azure SQL Data Warehouse in de praktijk] [ Migrating data to Azure SQL Data Warehouse in practice] voor meer informatie over migratie.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/

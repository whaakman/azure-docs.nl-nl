---
title: Migreren van uw oplossing naar SQL Data Warehouse | Microsoft Docs
description: Migratie-instructies voor uw oplossing te brengen naar Azure SQL Data Warehouse-platform.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 771b9456e66b8a1e41f72340b695b19e2adaf793
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a>Uw oplossing voor het migreren naar Azure SQL Data Warehouse
Zie wat betrokken bij het migreren van een bestaande databaseoplossing naar Azure SQL Data Warehouse. 

## <a name="profile-your-workload"></a>De werkbelasting van uw profiel
Voordat u migreert, die u wilt worden dat bepaalde SQL Data Warehouse is de juiste oplossing voor uw workload. SQL Data Warehouse is een gedistribueerd systeem ontworpen voor het uitvoeren van analyses op grote hoeveelheden gegevens.  Migreren naar SQL Data Warehouse vereist sommige ontwerpwijzigingen die zijn niet te hard om te begrijpen maar het kunnen even duren om te implementeren. Als uw bedrijf een datawarehouse bedrijfsniveau vereist, zijn de voordelen winnen. Als u de kracht van SQL Data Warehouse hoeft, is het echter rendabeler SQL Server of Azure SQL Database wilt gebruiken.

Overweeg het gebruik van SQL Data Warehouse wanneer u:
- Een of meer Terabytes aan gegevens hebben
- Plan voor het uitvoeren van analyses op grote hoeveelheden gegevens
- De mogelijkheid hebben om te schalen berekeningen en opslag 
- Wilt u kosten besparen door onderbreken rekenresources wanneer u ze niet nodig.

Gebruik geen SQL Data Warehouse voor operationele (OLTP-systemen)-werkbelastingen met:
- Hoge frequentie leest en schrijft
- Hiermee selecteert u een groot aantal singleton
- Grote hoeveelheden één rij invoegen
- Rij moet verwerken
- Niet-compatibele indeling (JSON, XML)


## <a name="plan-the-migration"></a>De migratie plannen

Zodra u hebt besloten om een bestaande oplossing voor het migreren naar SQL Data Warehouse, is het belangrijk om te plannen van de migratie voordat het aan de slag. 

Een doel van de planning is om te controleren of dat uw gegevens, het tabelschema en uw code compatibel zijn met SQL Data Warehouse. Er zijn enkele verschillen compatibiliteit tussen uw huidige systeem- en SQL Data Warehouse omzeilen. Bovendien door grote hoeveelheden gegevens te migreren naar Azure vergt tijd. Een zorgvuldige planning versnelt opnemen van uw gegevens naar Azure. 

Een ander doel van de planning is het ontwerp aanpassen om te controleren of dat uw oplossing maakt gebruik van de hoge queryprestaties die SQL Data Warehouse is ontworpen om te bieden. Het ontwerpen van datawarehouses voor schaal introduceert verschillende ontwerp patronen en dus traditionele methoden niet altijd het beste. Hoewel na de migratie kunt u bepaalde aanpassingen van het ontwerp, sneller wijzigingen aanbrengen in het proces op een later tijdstip, worden opgeslagen.

Voor een succesvolle migratie, moet u uw tabelschema, uw code en uw gegevens te migreren. Zie voor richtlijnen over deze migratieonderwerpen:

-  [Uw schema's migreren](sql-data-warehouse-migrate-schema.md)
-  [Uw code migreren](sql-data-warehouse-migrate-code.md)
-  [Uw gegevens migreren](sql-data-warehouse-migrate-data.md). 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a>Volgende stappen
De CAT (Customer Advisory Team) heeft ook enkele geweldige SQL Data Warehouse richtlijnen, ze via blogs publiceren.  Kijk eens naar hun artikel [gegevens voor het migreren naar Azure SQL Data Warehouse in de praktijk] [ Migrating data to Azure SQL Data Warehouse in practice] voor aanvullende hulp bij de migratie.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/

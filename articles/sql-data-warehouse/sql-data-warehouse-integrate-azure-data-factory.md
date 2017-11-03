---
title: Azure Data Factory gebruiken met SQL datawarehouse | Microsoft Docs
description: Tips voor het gebruik van Azure Data Factory (ADF) met Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 492de762-c7a2-4cdb-943f-3135230e94f1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6adfa1264c9d196d6c6e57f1d108710b9ee73265
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Azure Data Factory gebruiken met SQL datawarehouse
Azure Data Factory biedt een volledig beheerde methode voor het organiseren van de overdracht van gegevens en de uitvoering van opgeslagen procedures op de SQL Data Warehouse.  Hierdoor kunt u eenvoudig kunt instellen en planning complexe extraheren Transform and Load (ETL) procedures met SQL Data Warehouse. Zie voor een volledig overzicht van Azure Data Factory de [documentatie Azure Data Factory][Azure Data Factory documentation].

## <a name="data-movement"></a>Gegevensverplaatsing
Azure Data Factory kunt verplaatsing van gegevens tussen zowel lokale bronnen en andere Azure-services.  Algemene, de huidige integratie met Azure Data Factory ondersteunt de verplaatsing van gegevens naar en van de volgende locaties:

* Azure blob-opslag
* Azure SQL Database
* On-premises SQL Server
* SQL Server op IaaS

Zie voor informatie over het instellen van een kopieeractiviteit [kopiëren van gegevens met Azure Data Factory][Copy data with Azure Data Factory]

## <a name="stored-procedures"></a>Opgeslagen procedures
 Op dezelfde manier die kan worden gebruikt om te plannen, gegevensoverdracht, worden Azure Data Factory ook gebruikt voor het indelen van de uitvoering van opgeslagen procedures.  Hiermee kan complexere pijplijnen om te worden gemaakt en wordt uitgebreid Azure Data Factory kunnen gebruikmaken van de verwerkingskracht van SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de integratie van [overzicht van de integratie van SQL Data Warehouse][SQL Data Warehouse integration overview].
Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview] voor meer tips voor ontwikkelaars.

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/copy-activity-overview.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/


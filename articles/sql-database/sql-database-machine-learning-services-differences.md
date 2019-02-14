---
title: Belangrijke verschillen voor Machine Learning-Services (met R) in het overzicht van Azure SQL Database (Preview)
description: Dit onderwerp beschrijft de belangrijkste verschillen tussen Azure SQL Database Machine Learning-Services (met R) en SQL Server Machine Learning-Services.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: 4350fb0e75f140e120ba6cd2f074ffa1816a8fce
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237481"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-and-sql-server"></a>Belangrijke verschillen tussen Machine Learning-Services in Azure SQL Database en SQL Server

De functionaliteit van Machine Learning Services (met R) in Azure SQL Database is vergelijkbaar met [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Hieronder vindt u enkele belangrijke verschillen tussen deze.

## <a name="language-support"></a>Taalondersteuning

SQL Server biedt ondersteuning voor R en Python via de [uitbreidingsframework](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework). SQL Database biedt geen ondersteuning voor beide talen. De belangrijkste verschillen zijn:

- R is de enige ondersteunde taal in SQL-Database. Er is op dit moment geen ondersteuning voor Python.
- De R-versie is 3.4.4.
- Er is niet nodig om te configureren `external scripts enabled` via `sp_configure`. Wanneer u bent [aangemeld](sql-database-machine-learning-services-overview.md#signup), machine learning-is ingeschakeld voor uw SQL-database.

## <a name="package-management"></a>Pakketbeheer

Beheer van R-pakket en de installatie werkt verschil is tussen de SQL-Database en SQL Server. Deze verschillen zijn:

- R-pakketten zijn geïnstalleerd [sqlmlutils](https://github.com/Microsoft/sqlmlutils) of [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Pakketten uitvoeren niet netwerkaanroepen van uitgaande. Deze beperking is vergelijkbaar met de [standaard firewall-regels voor Machine Learning-Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) in SQL Server, maar kan niet worden gewijzigd in SQL-Database.
- Er is geen ondersteuning voor pakketten die afhankelijk zijn van externe runtimes (zoals Java) of toegang to APIs OS nodig is voor installatie of het gebruik.

## <a name="resource-governance"></a>Resourcebeheer

Het is niet mogelijk om te beperken van R-resources via [Resourceregeling](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) en externe resourcegroepen. R-resources zijn een percentage van de SQL Database-resources en zijn afhankelijk van welke servicelaag die u kiest. Zie voor meer informatie, [modellen aanschaffen van Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

## <a name="security-isolation"></a>Isolatie

In Azure SQL Database biedt SQL Platform Abstraction Layer (SQLPAL) isolatie voor externe processen. Deze isolatie biedt een extra beveiligingslaag voor het uitvoeren van R-scripts.

## <a name="next-steps"></a>Volgende stappen

- Zie de [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics) documentatie voor algemene informatie
- Zie voor meer informatie over het gebruik van Machine Learning-Services (met R) in Azure SQL Database, [snelstartgids](sql-database-connect-query-r.md).
- Leer meer met [zelfstudies voor SQL Server R-taal](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials)
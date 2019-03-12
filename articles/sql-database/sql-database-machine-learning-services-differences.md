---
title: Belangrijke verschillen voor Azure SQL Database Machine Learning-Services (preview)
description: Dit onderwerp beschrijft de belangrijkste verschillen tussen Azure SQL Database Machine Learning-Services (met R) en SQL Server Machine Learning-Services.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: 57ea52c179376e8378680f436d396ffaf9357f68
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771847"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Belangrijke verschillen tussen Machine Learning-Services in Azure SQL Database (preview) en SQL Server

De functionaliteit van Azure SQL Database Machine Learning-Services (met R) in (preview) is vergelijkbaar met [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Hieronder vindt u enkele belangrijke verschillen.

> [!IMPORTANT]
> Azure SQL Database Machine Learning-Services is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

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

Het is niet mogelijk om te beperken van R-resources via [Resourceregeling](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) en externe resourcegroepen.

Tijdens de openbare preview, R-resources zijn ingesteld op een maximum van 20% van de SQL Database-resources en zijn afhankelijk van welke servicelaag die u kiest. Zie voor meer informatie, [modellen aanschaffen van Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

### <a name="insufficient-memory-error"></a>Fout door onvoldoende geheugen

Als er onvoldoende geheugen beschikbaar voor R is, krijgt u een foutbericht weergegeven. Veelvoorkomende foutberichten zijn:

- Kan niet communiceren met de runtime voor 'R'-script voor aanvraag-id: ***. Controleer of de vereisten van 'R'-runtime
- 'R' script error occurred during execution of 'sp_execute_external_script' with HRESULT 0x80004004. ...an external script error occurred: "..could not allocate memory (0 Mb) in C function 'R_AllocStringBuffer'"
- Er is een externe scriptfout opgetreden: Fout: kan geen vector van grootte toewijzen.

Gebruik is afhankelijk van hoeveel geheugen wordt gebruikt in uw R-scripts en het aantal parallelle query's wordt uitgevoerd. Als u de bovenstaande fouten ontvangt, kunt u uw database naar een hogere servicelaag dit oplossen kunt schalen.

## <a name="next-steps"></a>Volgende stappen

- Zie de [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics) documentatie voor algemene informatie
- Zie voor meer informatie over het gebruik van Machine Learning-Services (met R) in Azure SQL Database, [snelstartgids](sql-database-connect-query-r.md).
- Leer meer met [zelfstudies voor SQL Server R-taal](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials)
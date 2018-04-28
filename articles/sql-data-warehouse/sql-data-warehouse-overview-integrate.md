---
title: Geïntegreerde oplossingen bouwen met SQL Data Warehouse | Microsoft Docs
description: "Hulpprogramma's en partners met oplossingen die zijn geïntegreerd met SQL Data Warehouse. "
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: f198a99fc03a079be77c7f8167580bb7b758579e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>Andere services integreren met SQL Data Warehouse
Naast de kernfunctionaliteit kunnen SQL Data Warehouse gebruikers om te integreren met veel van de andere services in Azure. Enkele van deze services:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

SQL Data Warehouse blijft geïntegreerd met meer services in Azure, en meer [integratiepartners](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
Integratie van Power BI kunt u de rekencapaciteit van SQL Data Warehouse worden gecombineerd met de dynamische reporting en visualisatie van Power BI. Power BI-integratie omvat momenteel:

* **Verbinding maken met directe**: een meer geavanceerde verbinding met logische naar beneden duwen tegen SQL Data Warehouse. Naar beneden duwen bevat sneller analyse op grotere schaal.
* **Openen in Power BI**: de knop 'Openen in Power BI' geeft exemplaargegevens over aan Power BI voor een simplifed manier om te verbinden.

Zie voor meer informatie [integreren met Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md), of de [Power BI-documentatie](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory biedt gebruikers een beheerd platform kunt maken van complexe uitpakken en pijplijnen laden. SQL Data Warehouse-integratie met Azure Data Factory omvat:

* **Opgeslagen Procedures**: indelen van de uitvoering van opgeslagen procedures op de SQL Data Warehouse.
* **Kopiëren**: ADF gebruiken om gegevens te verplaatsen naar SQL Data Warehouse. Deze bewerking kunt ADF standaardgegevens gegevensverplaatsing mechanisme of PolyBase gebruiken onder de behandelt. 

Zie voor meer informatie [integreren met Azure Data Factory](sql-data-warehouse-get-started-visualize-with-power-bi.md).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde analytics-service, waarmee u complexe modellen met behulp van een groot aantal voorspellende hulpprogramma's kunt maken. SQL Data Warehouse wordt ondersteund als een bron- en doel voor deze modellen met de volgende functies:

* **Gegevens lezen:** modellen station op grote schaal met T-SQL met SQL Data Warehouse.
* **Schrijven van gegevens:** doorvoeren wordt gewijzigd van een model terug naar SQL Data Warehouse.

Zie voor meer informatie [integreren met Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics is een volledig beheerde complexe infrastructuur voor het verwerken en gebruiken van gebeurtenisgegevens die zijn gegenereerd uit Azure Event Hub.  Integratie met SQL Data Warehouse kan voor streaming gegevens effectief worden verwerkt en opgeslagen samen met relationele gegevens diepere, meer geavanceerde analyse inschakelen.  

* **Taakuitvoer:** uitvoer van de Stream Analytics-taken rechtstreeks naar SQL Data Warehouse verzenden.

Zie voor meer informatie [integreren met Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).

## <a name="next-steps"></a>Volgende stappen
Als u wilt integreren met Azure SQL Database, Zie [elastische query SQL-Database configureren](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)


---
title: Geïntegreerde oplossingen bouwen met SQL Data Warehouse | Microsoft Docs
description: "Hulpprogramma's en partners met oplossingen die kunnen worden geïntegreerd met SQL Data Warehouse. "
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 3546dd26ac7d376eed142629ceeeb30e0709e162
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57309873"
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>Andere services integreren met SQL Data Warehouse
Naast de kernfunctionaliteit kunnen SQL Data Warehouse gebruikers om te integreren met veel van de andere services in Azure. Enkele van deze services zijn onder andere:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

SQL Data Warehouse blijft om te integreren met meer services in Azure, en meer [integratiepartners](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
Power BI-integratie kunt u de compute-kracht van SQL Data Warehouse combineren met de dynamische rapportage en visualisatie van Power BI. Power BI-integratie is momenteel inclusief:

* **Directe verbinding**: Een meer geavanceerde verbinding met logisch pushdown op basis van SQL Data Warehouse. Pushdown biedt snellere analyses op grotere schaal.
* **Openen in Power BI**: De knop 'Openen in Power BI' geeft informatie over het exemplaar aan Power BI voor een vereenvoudigde manier om verbinding te maken.

Zie voor meer informatie, [integreren met Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md), of de [Power BI-documentatie](https://blogs.msdn.com/b/powerbi/archive/20../../exploring-azure-sql-data-warehouse-with-power-bi.aspx).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory biedt gebruikers een beheerd platform voor het maken van complexe uitpakken en laden van pijplijnen. SQL Data Warehouse-integratie met Azure Data Factory bevat:

* **Opgeslagen Procedures**: Coördineer de uitvoering van opgeslagen procedures voor SQL Data Warehouse.
* **Kopie**: Met ADF kunt verplaatsen van gegevens in SQL Data Warehouse. Met deze bewerking kunt ADF standaardgegevens verkeer mechanisme of PolyBase gebruiken op de achtergrond. 

Zie voor meer informatie, [integreren met Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?toc=/azure/sql-data-warehouse/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde Analyseservice waarmee u complexe modellen met behulp van een groot aantal hulpprogramma's voor voorspellende maken. SQL Data Warehouse wordt ondersteund als een bron- en doel voor deze modellen met de volgende functionaliteit:

* **Gegevens lezen:** Station modellen op schaal met behulp van T-SQL met SQL Data Warehouse.
* **Schrijven van gegevens:** Wijzigingen doorvoeren van een model terug naar SQL Data Warehouse.

Zie voor meer informatie, [integreren met Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics is een complexe, volledig beheerde infrastructuur voor het verwerken en gebruiken van de gegevens van de gebeurtenis is gegenereerd op basis van Azure Event Hub.  Integratie met SQL Data Warehouse kunt u streaminggegevens effectief worden verwerkt en opgeslagen samen met relationele gegevens diepere, geavanceerde analyse inschakelen.  

* **Taakuitvoer:** Uitvoer van Stream Analytics-taken rechtstreeks verzenden naar SQL Data Warehouse.

Zie voor meer informatie, [integreren met Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).

## <a name="next-steps"></a>Volgende stappen
Als u wilt integreren met Azure SQL Database, Zie [elastische query's voor SQL Database configureren](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)


---
title: "Geïntegreerde oplossingen bouwen met SQL Data Warehouse | Microsoft Docs"
description: "Hulpprogramma's en partners met oplossingen die zijn geïntegreerd met SQL Data Warehouse. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: d407c29f99fd7537590ec787febd84a9e3f4f353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a>Gebruikmaken van andere services met SQL Data Warehouse
Naast de kernfunctionaliteit kunnen SQL Data Warehouse gebruikers gebruikmaken van veel van de andere services in Azure ernaast.  In het bijzonder hebben we stappen diep integreren met de volgende momenteel genomen:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

We werken aan verbinding maken met meer services die via het Azure-ecosysteem.

## <a name="power-bi"></a>Power BI
Integratie van Power BI kunt u gebruikmaken van de rekencapaciteit van SQL Data Warehouse met dynamische rapportage en visualisatie van Power BI. Power BI-integratie omvat momenteel:

* **Verbinding maken met directe**: een meer geavanceerde verbinding met logische naar beneden duwen tegen SQL Data Warehouse.  Dit biedt sneller analyse op grotere schaal.
* **Openen in Power BI**: de knop 'Openen in Power BI' geeft informatie over het exemplaar aan Power BI zodat voor een meer naadloze verbinding.

Zie [integreren met Power BI](sql-data-warehouse-integrate-power-bi.md) of de [Power BI-documentatie](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) voor meer informatie.

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory biedt gebruikers een beheerd platform voor het maken van complexe Extract Load pijplijnen.  SQL Data Warehouse-integratie met Azure Data Factory omvat het volgende:

* **Opgeslagen Procedures**: indelen van de uitvoering van opgeslagen procedures op de SQL Data Warehouse.
* **Kopiëren**: ADF gebruiken om gegevens te verplaatsen naar SQL Data Warehouse.  Deze bewerking kunt ADF standaardgegevens gegevensverplaatsing mechanisme of PolyBase gebruiken onder de behandelt. 

Zie [integreren met Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) of de [documentatie Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) voor meer informatie.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde analytics-service waarmee gebruikers om complexe modellen gebruik te maken van een groot aantal voorspellende hulpprogramma's te maken.  SQL Data Warehouse wordt ondersteund als een bron- en doel voor deze modellen met de volgende functies:

* **Gegevens lezen:** modellen station op grote schaal met T-SQL met SQL Data Warehouse.
* **Schrijven van gegevens:** doorvoeren wordt gewijzigd van een model terug naar SQL Data Warehouse.

Zie [integreren met Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) of de [Azure Machine Learning-documentatie](https://azure.microsoft.com/services/machine-learning/) voor meer informatie.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics is een volledig beheerde complexe infrastructuur voor het verwerken en gebruiken van gebeurtenisgegevens die zijn gegenereerd uit Azure Event Hub.  Integratie met SQL Data Warehouse kan voor streaming gegevens effectief worden verwerkt en opgeslagen samen met relationele gegevens diepere, meer geavanceerde analyse inschakelen.  

* **Taakuitvoer:** uitvoer van de Stream Analytics-taken rechtstreeks naar SQL Data Warehouse verzenden.

Zie [integreren met Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) of de [Azure Stream Analytics-documentatie](https://azure.microsoft.com/documentation/services/stream-analytics/) voor meer informatie.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->

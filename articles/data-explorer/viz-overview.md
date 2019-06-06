---
title: Gegevensvisualisatie met Azure Data Explorer
description: Meer informatie over de verschillende manieren die kunt u uw Azure Data Explorer-gegevens visualiseren
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 85c37b6d626fc9942f5df956e738431d2727d282
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481832"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Gegevensvisualisatie met Azure Data Explorer 

Azure Data Explorer is een snelle en schaalbare data exploration-service voor logboek- en telemetrie-gegevens die wordt gebruikt voor het bouwen van complexe analytics-oplossingen voor grote hoeveelheden gegevens. Azure Data Explorer kan worden geïntegreerd met verschillende hulpprogramma's voor visualisatie, zodat u kunt uw gegevens visualiseren en delen van de resultaten in uw organisatie. Deze gegevens kan worden omgezet in bruikbare inzichten om indruk te maken van uw bedrijf.

Gegevensvisualisatie en rapportage is een belangrijke stap in de data analytics-proces. Azure Data Explorer ondersteunt veel BI-services, zodat u kunt gebruiken die het beste past bij uw scenario en budget.

* Azure Data Explorer visualisaties: Met behulp van Kusto-query-taal de [ `render operator` ](/azure/kusto/query/renderoperator) biedt verschillende visualisatietypen om de queryresultaten weer. Queryvisualisaties zijn handig bij het detecteren van afwijkingen en prognoses, machine learning en meer.

* [Power BI](https://powerbi.microsoft.com): Azure Data Explorer biedt de mogelijkheid om te verbinden met Power BI met behulp van verschillende methoden: 

  * [Ingebouwde systeemeigen Power BI-connector](/azure/data-explorer/power-bi-connector)

  * [Query importeren uit Azure Data Explorer in Power BI](/azure/data-explorer/power-bi-imported-query)
 
  * [SQL-query](/azure/data-explorer/power-bi-sql-query).

* [Microsoft Excel](https://products.office.com/excel): Azure Data Explorer biedt de mogelijkheid om te verbinden met Excel-bestand met de ingebouwde systeemeigen Excel-connector of een query importeren uit Azure Data Explorer in Excel.

* [Grafana](https://grafana.com): Grafana biedt een Azure Data Explorer-invoegtoepassing waarmee u voor het visualiseren van gegevens uit Azure Data Explorer. U [Azure Data Explorer instellen als een gegevensbron voor Grafana en vervolgens de gegevens visualiseren](/azure/data-explorer/grafana)

* [Sisense](https://www.sisense.com): Azure Data Explorer biedt de mogelijkheid om te verbinden met Sisense met behulp van de connector JDBC. U [Azure Data Explorer instellen als een gegevensbron voor Sisense en visualiseren van de gegevens](/azure/data-explorer/sisense).

* [Tableau](https://www.tableau.com): Azure Data Explorer biedt de mogelijkheid om te verbinden met Tableau via de [ODBC-connector en de gegevens visualiseren in Tableau](/azure/data-explorer/connect-odbc).

* [Qlik](https://www.qlik.com): Azure Data Explorer biedt de mogelijkheid om te verbinden met behulp van Qlik de [ODBC connector](/azure/data-explorer/connect-odbc).
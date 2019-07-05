---
title: Gegevensvisualisatie met Azure Data Explorer
description: Meer informatie over de verschillende manieren die kunt u uw Azure Data Explorer-gegevens visualiseren
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: d1c73d8eb65ed5d67d5250b4a3bca3b80450001e
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536724"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Gegevensvisualisatie met Azure Data Explorer 

Azure Data Explorer is een snelle en schaalbare data exploration-service voor logboek- en telemetrie-gegevens die wordt gebruikt voor het bouwen van complexe analytics-oplossingen voor grote hoeveelheden gegevens. Azure Data Explorer kan worden geïntegreerd met verschillende hulpprogramma's voor visualisatie, zodat u kunt uw gegevens visualiseren en delen van de resultaten in uw organisatie. Deze gegevens kan worden omgezet in bruikbare inzichten om indruk te maken van uw bedrijf.

Gegevensvisualisatie en rapportage is een belangrijke stap in de data analytics-proces. Azure Data Explorer ondersteunt veel BI-services, zodat u kunt gebruiken die het beste past bij uw scenario en budget.

## <a name="kusto-query-language-visualizations"></a>Kusto-query language visualisaties

De querytaal Kusto [ `render operator` ](/azure/kusto/query/renderoperator) biedt verschillende visualisaties, zoals tabellen, cirkeldiagrammen en staafdiagrammen tot queryresultaten weer. Queryvisualisaties zijn handig bij het detecteren van afwijkingen en prognoses, machine learning en meer.

## <a name="power-bi"></a>Power BI

Azure Data Explorer biedt de mogelijkheid om te verbinden met [Power BI](https://powerbi.microsoft.com) verschillende methoden gebruiken: 

  * [Ingebouwde systeemeigen Power BI-connector](/azure/data-explorer/power-bi-connector)

  * [Query importeren uit Azure Data Explorer in Power BI](/azure/data-explorer/power-bi-imported-query)
 
  * [SQL-query](/azure/data-explorer/power-bi-sql-query)

## <a name="microsoft-excel"></a>Microsoft Excel

Azure Data Explorer biedt de mogelijkheid om te verbinden met [Microsoft Excel](https://products.office.com/excel) met behulp van de ingebouwde native connector voor Excel of een query importeren uit Azure Data Explorer in Excel.

## <a name="grafana"></a>Grafana

[Grafana](https://grafana.com) biedt een invoegtoepassing van Azure Data Explorer waarmee u voor het visualiseren van gegevens uit Azure Data Explorer. U [Azure Data Explorer instellen als een gegevensbron voor Grafana en visualiseren van de gegevens](/azure/data-explorer/grafana). 

## <a name="odbc-connector"></a>ODBC-connector

Azure Data Explorer biedt een [Open Database Connectivity (ODBC)-connector](connect-odbc.md) zodat alle toepassingen die ondersteuning biedt voor ODBC verbinding met Azure Data Explorer maken kan.

## <a name="tableau"></a>Tableau

Azure Data Explorer biedt de mogelijkheid om te verbinden met [Tableau](https://www.tableau.com) met behulp van de [ODBC connector](/azure/data-explorer/connect-odbc) en vervolgens [de gegevens visualiseren in Tableau](tableau.md).

## <a name="qlik"></a>Qlik

Azure Data Explorer biedt de mogelijkheid om te verbinden met [Qlik](https://www.qlik.com) met behulp van de [ODBC connector](/azure/data-explorer/connect-odbc) en Qlik Sense-dashboards maken en de gegevens visualiseren. Met behulp van de volgende video, kunt u leren om gegevens van Azure Data Explorer met Qlik te visualiseren. 

> [!VIDEO https://www.youtube.com/embed/nhWIiBwxjjU]  

## <a name="sisense"></a>Sisense

Azure Data Explorer biedt de mogelijkheid om te verbinden met [Sisense](https://www.sisense.com) met behulp van de connector JDBC. U [Azure Data Explorer instellen als een gegevensbron voor Sisense en visualiseren van de gegevens](/azure/data-explorer/sisense).
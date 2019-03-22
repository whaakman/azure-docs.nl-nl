---
title: Azure SQL Data Warehouse opmerkingen bij de Release van augustus 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/13/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 9547380d4636c1a72add9dab7a9eed850abd0307
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57889706"
---
# <a name="whats-new-in-azure-sql-data-warehouse-august-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Augustus 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in augustus 2018.

## <a name="automatic-intelligent-insights"></a>Automatic Intelligent Insights
Microsoft heeft geïntroduceerd [automatische intelligent insights](https://azure.microsoft.com/blog/automatic-intelligent-insights-to-optimize-performance-with-sql-data-warehouse/) op de cloud belofte van automatisering voor uw datawarehouse te leveren. U wordt niet meer nodig hebt voor het bewaken van uw datawarehouse voor gegevens scheeftrekken en suboptimale tabelstatistieken. SQL Data Warehouse oppervlakken zonder extra kosten, intelligent insights voor alle exemplaren van Gen2. Door te integreren met [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), kunt u aanbevolen werkwijzen besproken voor het verbeteren van de prestaties van uw actieve werklasten automatisch ontvangen. SQL Data Warehouse analyseert uw werkbelasting en oppervlakken aanbevelingen op basis van uw gebruik. Deze analyse gebeurt dagelijkse zodat u kunt de rapporten over het gebruik en de aanbevelingen voor verbeteringen aan uw workload controleren.

U kunt de aanbevelingen weergeven in de portal voor Azure Advisor: ![Azure Advisor Portal aanbevelingen voor Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/4e205b6d-df04-48db-8eec-d591f2592cf4.png)

U kunt inzoomen op elke categorie om te zien van de aanbevelingen voor de specifieke waarschuwing: ![Meer informatie voor Azure Advisor Portal aanbeveling voor Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/3c42426e-6969-46e3-9025-c34c0755a302.png)


## <a name="bug-fixes"></a>Opgeloste fouten

| Titel | Description |
|:---|:---|
| **Mogelijke Query mislukt wanneer het aantal split overschrijdt de maximale limiet** |Wanneer de bovengrens van 1 miljoen bestand-split-limiet wordt overschreden is een onverwerkte uitzondering veroorzaakt de SQL-Engine dump en alle query's is mislukt. Deze oplossing hebben oog voor het probleem met de uitzondering correct verwerken en een fout geretourneerd zonder dat de query's mislukken. |
| **Verbeterde ExternalMoveReadersPerNode standaardwaarde laden prestaties te verbeteren** |Dit probleem is veroorzaakt door de instelling van de eigenschap ExternalMoveReadersPerNode wordt niet gesynchroniseerd met de service fabric instellen. Deze regressie veroorzaakt een verslechterde Gen2 laadprestaties. De oplossing voorziet in Gen2 laadprestaties binnen geoptimaliseerde ontwerpparameters gebruikt.|


## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

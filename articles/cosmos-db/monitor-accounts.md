---
title: Azure Cosmos DB-aanvragen en -opslag controleren | Microsoft Docs
description: Informatie over het bewaken van uw Azure Cosmos DB-account voor metrische gegevens voor prestaties, zoals aanvragen en serverfouten, en metrische gegevens over gebruik, zoals het gebruik van opslag.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: sngun
ms.openlocfilehash: 1f4bf4ba660e83be9879c56d04b27c932d563e17
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49354056"
---
# <a name="monitor-azure-cosmos-db"></a>Azure Cosmos DB bewaken
U kunt bewaken van uw Azure Cosmos DB-accounts in de [Azure-portal](https://portal.azure.com/). Voor elke Azure Cosmos DB-account is een volledige suite met metrische gegevens beschikbaar voor het bewaken van doorvoer, opslag, beschikbaarheid, latentie en consistentie.

Metrische gegevens kunnen worden gecontroleerd op de pagina met het Account voor de nieuwe pagina met metrische gegevens, of in Azure Monitor.

## <a name="view-performance-metrics-on-the-metrics-page"></a>Metrische gegevens over prestaties weergeven op de pagina metrische gegevens
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **alle services**, schuif naar **Databases**, klikt u op **Azure Cosmos DB**, en klik vervolgens op de naam van de Azure Cosmos DB-account waarvoor u wilt weergeven van metrische gegevens voor prestaties.
2. Wanneer de nieuwe pagina wordt geladen, in het resourcemenu onder **bewaking**, klikt u op **metrische gegevens**.
3. Wanneer de metrische gegevens pagina wordt geopend, selecteert u de verzameling om te controleren van de **verzameling(en)** vervolgkeuzelijst.

   De Azure portal wordt weergegeven de suite van verzameling metrische gegevens beschikbaar. Houd er rekening mee dat de doorvoer, opslag, beschikbaarheid, latentie en consistentie van metrische gegevens worden geleverd op een afzonderlijk tabblad. Meteen aanvullende details over de gegevens hebt opgegeven, klikt u op de dubbele pijl in de rechterbovenhoek van elk deelvenster metrische gegevens.

   ![Schermopname van de lens bewaking waarin de suite metrische gegevens](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Metrische gegevens over prestaties weergeven met behulp van Azure Monitoring
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **Monitor** op de linkerbalk.
2. Klik in het resourcemenu op **metrische gegevens**.
3. In de **controleren - metrische gegevens** venster in de **resourcegroep** vervolgkeuzelijst, selecteer de resourcegroep die is gekoppeld aan het Azure Cosmos DB-account dat u wilt bewaken. 
4. In de **Resource** vervolgkeuzelijst, selecteer de database-account om te controleren.
5. In de lijst met **beschikbare metrische gegevens**, selecteert u de metrische gegevens om weer te geven. Gebruik de knop CTRL voor meervoudige selectie. 

## <a name="view-performance-metrics-on-the-account-page"></a>Metrische gegevens over prestaties weergeven op de accountpagina
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **alle services**, schuif naar **Databases**, klikt u op **Azure Cosmos DB**, en klik vervolgens op de naam van de Azure Cosmos DB-account waarvoor u wilt weergeven van metrische gegevens voor prestaties.
2. De **bewaking** lens standaard worden de volgende tegels weergegeven:
   
   * Totaal aantal aanvragen voor de huidige dag.
   * Opslag die wordt gebruikt.
   
   ![Schermopname van de lens bewaking waarin de aanvragen en het opslaggebruik](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Te klikken op de dubbele pijl in de rechterbovenhoek van de **aanvragen** tegel wordt geopend op een een gedetailleerde **Metric** pagina.
4. De **Metric** pagina ziet u details over het totaal aantal aanvragen. 

## <a name="set-up-alerts-in-the-portal"></a>Stel waarschuwingen in de portal
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **alle services**, klikt u op **Azure Cosmos DB**, en klik vervolgens op de naam van de Azure Cosmos DB-account waarvoor u wilt instellen van prestaties metrische waarschuwingen.
2. Klik in het resourcemenu op **waarschuwingsregels** om de pagina regels voor waarschuwingen te openen.  
   ![Schermopname van de waarschuwing regels geselecteerd](./media/monitor-accounts/madocdb10.5.png)
3. In de **waarschuwingsregels** pagina, klikt u op **waarschuwing toevoegen**.  
   ![Schermafbeelding van de pagina regels voor waarschuwingen met de knop van het type waarschuwing toevoegen gemarkeerd](./media/monitor-accounts/madocdb11.png)
4. In de **een waarschuwingsregel toevoegen** pagina:
   
   * De naam van de waarschuwingsregel die u instelt.
   * Een beschrijving van de nieuwe waarschuwingsregel.
   * De metrische gegevens voor de waarschuwingsregel.
   * De voorwaarde, drempelwaarde en periode die bepalen wanneer de waarschuwing wordt geactiveerd. Bijvoorbeeld, een server aantal fouten groter is dan 5 in de afgelopen 15 minuten.
   * Of de servicebeheerder en medebeheerders per e-mail verzonden worden wanneer de waarschuwing wordt geactiveerd.
   * Aanvullende e-mailadressen voor meldingen van waarschuwingen.  
     ![Schermopname van de toevoegen een waarschuwingsregel-pagina](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programmatically"></a>Azure Cosmos DB controleren via een programma
De account metingen op het niveau beschikbaar in de portal, zoals account opslag gebruik en totaal aantal aanvragen, zijn niet beschikbaar via de SQL-API's. U kunt echter gebruiksgegevens op het niveau verzameling ophalen met behulp van de SQL-API's. Om op te halen op gegevens te verzamelen, het volgende doen:

* De REST-API gebruikt [GET uitvoeren op de verzameling](https://msdn.microsoft.com/library/mt489073.aspx). De quota en het gebruik de informatie voor de verzameling wordt in de x-ms-resource-quota en x-ms-resource-usage-headers in het antwoord geretourneerd.
* Voor het gebruik van de .NET SDK, gebruiken de [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) methode retourneert een [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) die bevat een aantal eigenschappen van gebruik, zoals  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**, en nog veel meer.

Voor toegang tot aanvullende metrische gegevens, gebruikt de [SDK van Azure Monitor](https://www.nuget.org/packages/Microsoft.Azure.Insights). Beschikbare metrische definities kunnen worden opgehaald door aan te roepen:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Query's om op te halen van afzonderlijke metrische gegevens over gebruik de volgende indeling:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Zie voor meer informatie, [bij het ophalen van metrische gegevens voor resources via de REST-API van Azure Monitor](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Houd er rekening mee dat 'Azure Insights' is gewijzigd 'Azure Monitor'.  Dit blogbericht verwijst naar de naam van de ouder.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de capaciteitsplanning van Azure Cosmos DB, de [Azure Cosmos DB capaciteit planner calculator](https://www.documentdb.com/capacityplanner).


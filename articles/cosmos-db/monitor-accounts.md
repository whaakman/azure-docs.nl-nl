---
title: Azure DB die Cosmos-aanvragen en opslag bewaken | Microsoft Docs
description: Informatie over het bewaken van uw Azure DB die Cosmos-account voor prestatiegegevens, zoals aanvragen en serverfouten, en meetgegevens voor softwaregebruik, zoals opslagverbruik.
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 4c6a2e6f-6e78-48e3-8dc6-f4498b235a9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: mimig
ms.openlocfilehash: f07489172306b4f6d03b5a9b1399ed92e007c3c1
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="monitor-azure-cosmos-db"></a>Azure Cosmos DB bewaken
U kunt uw Azure DB die Cosmos-accounts in bewaken de [Azure-portal](https://portal.azure.com/). Voor elk Azure DB die Cosmos-account is een volledige reeks metrische gegevens beschikbaar zijn voor het bewaken van doorvoer, opslag, beschikbaarheid, latentie en consistentie.

Metrische gegevens kunnen worden gecontroleerd op de pagina met het Account voor de nieuwe pagina van de metrische gegevens of in de Azure-Monitor.

## <a name="view-performance-metrics-on-the-metrics-page"></a>Weergave maatstaven voor prestaties op de pagina metrische gegevens
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **meer Services**, schuif naar **Databases**, klikt u op **Azure Cosmos DB**, en klik vervolgens op de naam van de Azure DB die Cosmos-account waarvan u wilt weergeven van maatstaven voor prestaties.
2. Wanneer de nieuwe pagina wordt geladen, in het menu resource onder **bewaking**, klikt u op **metrische gegevens**.
3. Als de metrische gegevens pagina wordt geopend, selecteert u de verzameling om te controleren van de **verzameling(en)** vervolgkeuzelijst.

   De Azure portal wordt weergegeven de suite van verzameling metrische gegevens beschikbaar. Houd er rekening mee dat doorvoer, opslag, beschikbaarheid, latentie en consistentie metrische gegevens beschikbaar zijn op afzonderlijke tabbladen. Direct extra details van de metrische gegevens hebt opgegeven, klik op de dubbele pijl in de rechterbovenhoek van elke deelvenster metrische gegevens.

   ![Schermopname van de bewaking lens waarin de suite metrische gegevens](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>De prestaties van metrische gegevens weergeven met behulp van Azure-bewaking
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **Monitor** in de linkerbalk.
2. Klik in het menu resource **metrische gegevens**.
3. In de **Monitor - metrische gegevens** venster in de **resourcegroep** vervolgkeuzelijst, selecteer de resourcegroep die zijn gekoppeld aan het Azure DB die Cosmos-account dat u wilt bewaken. 
4. In de **Resource** vervolgkeuzelijst, selecteer de database-account om te controleren.
5. In de lijst met **beschikbare metrische gegevens**, selecteert u de metrische gegevens om weer te geven. Gebruik de knop CTRL voor meervoudige selectie. 

## <a name="view-performance-metrics-on-the-account-page"></a>Weergave maatstaven voor prestaties op de accountpagina
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **meer Services**, schuif naar **Databases**, klikt u op **Azure Cosmos DB**, en klik vervolgens op de naam van de Azure DB die Cosmos-account waarvan u wilt weergeven van maatstaven voor prestaties.
2. De **bewaking** lens wordt standaard de volgende tegels weergegeven:
   
   * Totaal aantal aanvragen voor de huidige dag.
   * Opslag gebruikt.
   
   ![Schermopname van de bewaking lens waarin de aanvragen en het opslaggebruik](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Klik op de dubbele pijl in de rechterbovenhoek van de **aanvragen** tegel Hiermee opent u een gedetailleerde **metriek** pagina.
4. De **metriek** pagina ziet u details over het totaal aantal aanvragen. 

## <a name="set-up-alerts-in-the-portal"></a>Stel waarschuwingen in de portal
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **meer Services**, klikt u op **Azure Cosmos DB**, en klik vervolgens op de naam van de Azure DB die Cosmos-account waarvan u wilt instellen op prestaties metrische waarschuwingen.
2. Klik in het menu resource **waarschuwingsregels** om de pagina met regels voor waarschuwingen te openen.  
   ![Schermopname van de waarschuwing regels geselecteerd](./media/monitor-accounts/madocdb10.5.png)
3. In de **waarschuwing regels** pagina, klikt u op **waarschuwing toevoegen**.  
   ![Schermafbeelding van de pagina met regels voor waarschuwingen met de knop van het type waarschuwing toevoegen gemarkeerd](./media/monitor-accounts/madocdb11.png)
4. In de **een waarschuwingsregel toevoegen** pagina:
   
   * De naam van de waarschuwingsregel die u instelt.
   * Een beschrijving van de nieuwe waarschuwingsregel.
   * De metrische gegevens voor de waarschuwingsregel.
   * De voorwaarde, drempelwaarde en periode om te bepalen wanneer de waarschuwing wordt geactiveerd. Bijvoorbeeld, een server aantal fouten groter is dan 5 in de afgelopen 15 minuten.
   * Hiermee wordt aangegeven of de servicebeheerder en coadministrators per e-mail verzonden worden wanneer de waarschuwing wordt geactiveerd.
   * Extra e-mailadressen voor meldingen van waarschuwingen.  
     ![Schermopname van de toevoegen een waarschuwingsregel-pagina](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programmatically"></a>Azure Cosmos DB programmatisch bewaken
De account niveau metrische gegevens beschikbaar zijn in de portal, zoals account opslag gebruik en totaal aantal aanvragen, zijn niet beschikbaar via de SQL-API's. U kunt echter gebruiksgegevens op het niveau verzameling ophalen met behulp van de SQL-API's. Voor het ophalen van gegevens te verzamelen niveau het volgende doen:

* De REST-API gebruiken [GET uitvoeren op de verzameling](https://msdn.microsoft.com/library/mt489073.aspx). De quota's en gebruik van informatie voor de verzameling wordt de x-ms-resource-quota- en x-ms--Resourcegebruik aangegeven in het antwoord geretourneerd.
* De .NET SDK, gebruikt de [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) methode retourneert een [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) die bevat een aantal eigenschappen van gebruik, zoals **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**, en meer.

Voor toegang tot aanvullende gegevens, gebruikt de [Azure Monitor SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Beschikbare metrische definities kunnen worden opgehaald door aan te roepen:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Query's voor het ophalen van afzonderlijke metrische gegevens gebruik de volgende notatie:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

Zie voor meer informatie [bij het ophalen van metrische gegevens voor resources via de REST-API van Azure Monitor](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Houd er rekening mee dat de "Azure Insights" is gewijzigd 'Azure Monitor'.  Dit blogbericht verwijst naar de naam van de oudere.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de capaciteitsplanning van Azure Cosmos DB, de [Azure Cosmos DB capaciteit planner Rekenmachine](https://www.documentdb.com/capacityplanner).


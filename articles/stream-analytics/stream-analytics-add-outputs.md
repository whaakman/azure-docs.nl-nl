---
title: Het configureren van de gegevens levert voor Azure Stream Analytics-taken
description: Uitvoer voor Stream Analytics-taken configureren | leren padsegment.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/26/2017
ms.openlocfilehash: 2e54b7970e4748ff56844fb06fa5177ddb207fa6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Het configureren van de gegevens levert voor Stream Analytics-taken

Azure Stream Analytics-taken kunnen worden verbonden met een of meer gegevens uitvoer, die een verbinding met een bestaande gegevens sink definieert. Als uw Stream Analytics-taak verwerkt en binnenkomende gegevens worden omgezet, wordt een stream met gegevens uitvoergebeurtenissen geschreven naar uitvoer van de taak.

Stream Analytics-gegevens uitvoer kunnen worden gebruikt als bron voor realtime-dashboards of waarschuwingen, data movement werkstromen geactiveerd, of gewoon archiveren van gegevens voor de batch-verwerking later op. Stream Analytics heeft eerste-klas integratie met verschillende Azure-services, die in details hier worden beschreven.

Uitvoer toevoegen aan Stream Analytics-taak:

1. In de [Azure-portal](https://portal.azure.com), opent u de taak en klikt u op **uitvoer** en klik vervolgens op **toevoegen** in de blade uitvoer die wordt weergegeven.
   
    ![Uitvoer toevoegen](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Geef een beschrijvende naam voor deze uitvoer in de **Uitvoeraliassen** vak. Deze naam kan worden gebruikt in uw job query later op om te verwijzen naar de uitvoer.  
   
    Vul in de rest van de vereiste verbindingseigenschappen verbinding maken met de uitvoer.  Deze velden is afhankelijk van het uitvoertype en hier worden gedefinieerd.  
   
    ![Gegevenstype verplaatsing kiezen](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. Afhankelijk van het uitvoertype moet u mogelijk opgeven hoe de gegevens is geserialiseerd of geformatteerd. De serialisatie van specifieke instellingen voor elk uitvoertype worden hier beschreven.
   
    Vul in de rest van de vereiste verbindingseigenschappen verbinding maken met de gegevensbron. Deze velden verschillen per type van het type invoer- en bron en worden gedefinieerd in detail toe aan de [taak maken artikel](stream-analytics-create-a-job.md).  

> [!Note]
>
> Elk element output is toegevoegd aan de job moet bestaan voordat de taak is gestart en gebeurtenissen start stromende. Bijvoorbeeld, als u een Blob-opslag als uitvoer gebruikt, wordt de taak niet storage-account automatisch gemaakt. Deze moeten worden gemaakt door de gebruiker voordat de ASA-taak is gestart.
> 
 

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)


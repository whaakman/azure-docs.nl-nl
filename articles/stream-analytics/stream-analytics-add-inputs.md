---
title: Een data invoer toevoegen aan Stream Analytics-taken | Microsoft Docs
description: Informatie over het aansluiten van een gegevensbron om uw Stream Analytics-taak als streaming gegevensinvoer uit Event Hubs of verwijzing gegevens uit Blog van storage.
keywords: gegevens invoeren, streamen van gegevens
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 7a4eb8642a0496e126b79724b4048bae7cc15a68
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Een streaming gegevens invoer- of gegevens toevoegen aan Stream Analytics-taak
Informatie over het aansluiten van een gegevensbron om uw Stream Analytics-taak als streaming gegevensinvoer uit Event Hubs of verwijzing gegevens uit Blob storage.

Azure Stream Analytics-taken kunnen worden verbonden met een invoer of meer, die elk een verbinding met een bestaande gegevensbron definieert. Wanneer gegevens worden verzonden aan die gegevensbron, wordt gebruikt door de Stream Analytics-taak en verwerkt in realtime als het streamen van gegevens. Stream Analytics heeft eerste-klas integratie met [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) en [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) zowel binnen als buiten het abonnement van de taak.

In dit artikel is een stap in de [leertraject voor Stream Analytics](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Invoer: Streaming en verwijzingsgegevens
Er zijn twee verschillende soorten invoer in Stream Analytics: gegevensstromen en referentiegegevens.

* **Gegevensstromen**: Stream Analytics-taken moeten ten minste één gegevensstroominvoer worden verbruikt en wordt omgezet door de taak bevatten. Azure Blob storage en Azure Event Hubs worden ondersteund als invoer gegevensbronnen stroom. Azure Event Hubs worden gebruikt voor het verzamelen van verbonden apparaten, services en toepassingen. Azure Blob-opslag kan worden gebruikt als een invoerbron voor het opnemen van grote hoeveelheden gegevens als een stroom.  
* **Referentiegegevens**: Stream Analytics ondersteunt een tweede gegevenstype reserve invoerverwijzing genoemd.  In plaats van de gegevens in beweging, worden deze gegevens zijn statisch of vertraging wijzigen.  Dit wordt meestal gebruikt voor het uitvoeren van de op te zoeken en correlatie met gegevensstromen voor het maken van een uitgebreidere set van gegevens.  Azure Blob-opslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens.  

Een invoer toevoegen aan Stream Analytics-taak:

1. Klik in de Azure portal op **invoer** en klik vervolgens op **invoer toevoegen** in uw Stream Analytics-taak.
   
    ![Azure-portal - invoer toevoegen.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    Klik in de Azure portal op de **invoer** -tegel in de Stream Analytics-taak.  
   
    ![Azure-portal - invoer toevoegen.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. Geef het type van de invoer: beide **gegevensstroom** of **referentiegegevens**.
   
    ![Voer, gestreamd of verwijst naar de juiste gegevens toevoegen](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Voer, gestreamd of verwijst naar de juiste gegevens toevoegen](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. Als een gegevensstroom invoer maakt, geeft u het brontype voor de invoer.  Deze stap worden overgeslagen tijdens het maken van referentiegegevens als alleen-Blob-opslag op dit moment wordt ondersteund.
   
    ![Gegevens gegevensstroominvoer toevoegen](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Gegevens gegevensstroominvoer toevoegen](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. Geef een beschrijvende naam voor deze invoer in het vak Invoeralias.  Deze naam wordt gebruikt in uw job query later op om te verwijzen naar de invoer.
   
    Vul in de rest van de vereiste verbindingseigenschappen verbinding maken met de gegevensbron. Deze velden verschillen per type van het type invoer- en bron en worden gedefinieerd in detail [hier](stream-analytics-create-a-job.md).  
   
    ![Event hub gegevensinvoer toevoegen](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. Geef de serialisatie-instellingen voor de ingevoerde gegevens:
   
   * Om er zeker van te zijn uw query's werken zoals verwacht, geef de **gebeurtenis serialisatie-indeling** van binnenkomende gegevens.  Ondersteunde serialisatie-indelingen zijn JSON, CSV en Avro.
   * Controleer of de **codering** voor de gegevens.  De enige ondersteunde coderingsindeling is UTF-8 op dit moment.
     
     ![Instellingen voor serialisatie van gegevens voor de gegevens invoeren](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Instellingen voor serialisatie van gegevens voor de gegevens invoeren](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. Na het voltooien van het maken van de invoer, Stream Analytics controleert of dat deze verbinding met de invoerbron maken kan.  U kunt de status van de bewerking verbinding testen bekijken in de Notification hub.
   
    ![Verbinding van de invoer streaminggegevens testen](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Verbinding van de invoer streaminggegevens testen](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Hulp bij het streaming-gegevens invoeren
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)


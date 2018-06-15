---
title: Invoer voor Azure Stream Analytics kennen
description: In dit artikel wordt het concept van invoerwaarden in een Azure Stream Analytics-taak vergelijken streaming invoer voor de gegevensinvoer verwijzing beschreven.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 926821e2ba9912ae0140f11c9fe9a2d504609a1e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32186059"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Invoer voor Azure Stream Analytics kennen

Azure Stream Analytics-taken verbinding met een of meer invoerwaarden van gegevens. Elke invoer definieert een verbinding met een bestaande gegevensbron. Stream Analytics accepteert binnenkomende gegevens van verschillende soorten bronnen van gebeurtenissen met inbegrip van Event Hubs, IoT-Hub en Blob-opslag. De invoer wordt verwezen door de naam in de streaming SQL-query die u voor elke taak schrijft. U kunt in de query meerdere invoeren voor het combineren van gegevens of het vergelijken van streaming gegevens met een lookup referentiegegevens koppelen en de resultaten doorgeven aan uitvoer. 

Stream Analytics is een uitstekende integratie van drie soorten resources als invoer:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Deze invoer resources kunnen bevinden zich in dezelfde Azure-abonnement als uw Stream Analytics-taak of vanuit een ander abonnement.

U kunt de [Azure-portal](stream-analytics-quick-create-portal.md#configure-input-to-the-job), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/New-AzureRmStreamAnalyticsInput), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST-API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), en [Visual Studio](stream-analytics-tools-for-visual-studio.md)maken, bewerken en testen van de Stream Analytics-taak invoeren.

## <a name="stream-and-reference-inputs"></a>Stroom en de referentie-invoer
Als u gegevens naar een gegevensbron is gepusht, heeft gebruikt door de Stream Analytics-taak en verwerkt in realtime. Invoer worden onderverdeeld in twee typen: gegevens stream invoer en verwijzen naar gegevens invoer.

### <a name="data-stream-input"></a>Gegevensstroom invoer
Een gegevensstroom is een niet-gebonden reeks gebeurtenissen gedurende een bepaalde periode. Stream Analytics-taken moeten ten minste één gegevensstroominvoer bevatten. Event Hubs, IoT-Hub en Blob-opslag worden ondersteund als invoer gegevensbronnen stroom. Event Hubs worden gebruikt voor het verzamelen van meerdere apparaten en services. Deze stromen kunnen sociale media activiteitsfeeds, gegevens over voorraad handel of gegevens van sensoren bevatten. IoT Hubs zijn geoptimaliseerd voor het verzamelen van gegevens van verbonden apparaten in scenario's voor Internet der dingen (IoT).  BLOB-opslag kan worden gebruikt als een invoerbron voor het opnemen van grote hoeveelheden gegevens als een stream, zoals logboekbestanden.  

Zie voor meer informatie over gegevens invoer streaming [gegevens als invoer Stream Analytics streamen](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>De gegevensinvoer verwijzing
Stream Analytics ondersteunt ook bekend als invoer *referentiegegevens*. Referentiegegevens is volledig statisch of traag wordt gewijzigd. Dit wordt meestal gebruikt voor correlatie en zoekacties. Bijvoorbeeld, u mogelijk koppelt gegevens in de invoer van de gegevensstroom tot gegevens in de referentiegegevens eruit als u een SQL-join statische waarden wilt uitvoeren. Azure Blob-opslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens. Verwijzing data source blobs zijn beperkt tot 100 MB groot.

Zie voor meer informatie over verwijzing gegevens invoer [Using referentiegegevens voor zoekacties in de Stream Analytics](stream-analytics-use-reference-data.md)

In dit artikel is een stap in de [leertraject voor Stream Analytics](/documentation/learning-paths/stream-analytics/).

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Snelstartgids: Een Stream Analytics-taak maken met behulp van de Azure-portal](stream-analytics-quick-create-portal.md)
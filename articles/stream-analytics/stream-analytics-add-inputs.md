---
title: Meer informatie over invoer voor Azure Stream Analytics
description: Het concept van de invoer in een Azure Stream Analytics-taak vergelijken streaming invoer voor de referentie-invoer voor gegevens in dit artikel beschreven.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: a6b6b548f1e5a9fa1e92b5fdbd714117758d75b4
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737073"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Meer informatie over invoer voor Azure Stream Analytics

Azure Stream Analytics-taken verbinden met een of meer gegevensinvoer. Elke invoer definieert een verbinding met een bestaande gegevensbron. Stream Analytics accepteert binnenkomende gegevens vanuit verschillende soorten bronnen van gebeurtenissen, inclusief Event Hubs, IoT-Hub en Blob-opslag. De invoer wordt verwezen door de naam in de streaming SQL-query die u voor elke taak schrijven. U kunt in de query, deelnemen aan meerdere invoeren voor het combineren van gegevens of het vergelijken van streaming gegevens met een zoekopdracht verwijzen naar gegevens en de resultaten geven aan uitvoer. 

Stream Analytics is een uitstekende integratie uit drie soorten resources als invoer:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Deze invoer resources kunnen bevinden zich in hetzelfde Azure-abonnement als uw Stream Analytics-taak, of vanuit een ander abonnement.

U kunt de [Azure-portal](stream-analytics-quick-create-portal.md#configure-job-input), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST-API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), en [Visual Studio](stream-analytics-tools-for-visual-studio-install.md)maken, bewerken en testen van de invoer van Stream Analytics.

## <a name="stream-and-reference-inputs"></a>Stream en referentie-invoer
Wanneer er gegevens binnenkomen met een gegevensbron, heeft die worden gebruikt door de Stream Analytics-taak en verwerkt in realtime. Invoer worden onderverdeeld in twee typen: gegevens streamen van invoer en verwijzen naar gegevensinvoer.

### <a name="data-stream-input"></a>Gegevens streamen invoer
Een gegevensstroom is een niet-gebonden reeks gebeurtenissen na verloop van tijd. Stream Analytics-taken moeten ten minste één gegevensstroominvoer bevatten. Eventhubs, IoT-Hub en Blob-opslag worden ondersteund als invoer gegevensbronnen stream. Eventhubs worden gebruikt om gebeurtenisstromen te verzamelen van meerdere apparaten en services. Deze stromen advies inwinnen activiteit socialemediafeeds, gegevens over voorraad handel of gegevens van sensoren. IoT-Hubs zijn geoptimaliseerd voor het verzamelen van gegevens van verbonden apparaten in scenario's met Internet of Things (IoT).  BLOB-opslag kan worden gebruikt als een invoerbron voor het opnemen van grote hoeveelheden gegevens als een stroom, zoals logboekbestanden.  

Zie voor meer informatie over de gegevensinvoer streamen [Stream gegevens als invoer in Stream Analytics](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Referentie-invoer voor gegevens
Stream Analytics ondersteunt ook bekend als invoer *verwijzen naar gegevens*. Referentiegegevens is een volledig statische of langzaam wordt gewijzigd. Dit wordt meestal gebruikt om uit te voeren van de correlatie en zoekacties. U kan bijvoorbeeld gegevens in de invoer van de gegevens stream met gegevens in de referentiegegevens, samenvoegen, net als u een SQL-join om te controleren of statische waarden wilt uitvoeren. Azure Blob-opslag is momenteel de enige ondersteunde invoerbron voor referentiegegevens. Referentiegegevens-blobs bron hebben een limiet van 300 MB in grootte, afhankelijk van de complexiteit van de query en Streaming-eenheden toegewezen.

Zie voor meer informatie over het verwijzen naar gegevensinvoer [met referentiegegevens voor zoekacties in Stream Analytics](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Snelstart: Een Stream Analytics-taak maken met behulp van Azure portal](stream-analytics-quick-create-portal.md)

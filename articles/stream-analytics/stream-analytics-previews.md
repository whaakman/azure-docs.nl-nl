---
title: Azure Stream Analytics preview-functies
description: In dit artikel geeft een lijst van de Azure Stream Analytics-functies die momenteel in preview.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2019
ms.openlocfilehash: f3838bf6b9f7daa24c0cdb5b0c5a08d41d164530
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65561142"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics preview-functies

In dit artikel bevat een overzicht van alle functies die momenteel in Preview-versie van Azure Stream Analytics. Preview-functies gebruiken in een productieomgeving wordt niet aanbevolen.

## <a name="public-previews"></a>Openbare preview-versies

De volgende functies zijn in openbare preview. U kunt profiteren van deze functies vandaag, maar niet gebruiken in uw productieomgeving.

### <a name="visual-studio-code-for-azure-stream-analytics-released-may-2019"></a>Visual Studio Code for Azure Stream Analytics (uitgebracht mei 2019)

De functies van Azure Stream Analytics-taken kunnen worden geschreven in Visual Studio Code. Zie onze [zelfstudie VS Code aan de slag](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).

### <a name="anomaly-detection"></a>Anomaliedetectie

Azure Stream Analytics introduceert nieuwe machine learning-modellen met ondersteuning voor *piek* en *dalen* detectie naast bidirectionele, trage positief en traag negatieve trends detecteren. Ga voor meer informatie naar [anomaliedetectie in Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="sql-database-reference-data"></a>Referentiegegevens voor SQL-Database

Azure Stream Analytics biedt ondersteuning voor Azure SQL Database als een bron van de invoer voor referentiegegevens. U kunt SQL-Database gebruiken als referentiegegevens voor uw Stream Analytics-taak in de Azure-portal en Visual Studio met Stream Analytics-hulpprogramma's. Voor meer informatie gaat u naar, [gebruik referentiegegevens uit een SQL-Database voor een Azure Stream Analytics-taak](sql-reference-data.md).

### <a name="integration-with-azure-machine-learning"></a>Integratie met Azure Machine Learning

Stream Analytics-taken met Machine Learning (ML)-functies, kunt u schalen. Voor meer informatie over hoe u ML-functies in uw Stream Analytics-taak gebruiken kunt, gaat u naar [schalen van uw Stream Analytics-taak met Azure Machine Learning-functies](stream-analytics-scale-with-machine-learning-functions.md). Bekijk een Praktijkscenario met [uitvoeren van sentimentanalyses met behulp van Azure Stream Analytics en Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="javascript-user-defined-aggregate"></a>De gebruiker gedefinieerde aggregatie JavaScript

Azure Stream Analytics ondersteunt de gebruiker gedefinieerde aggregaties (UDA) die zijn geschreven in JavaScript, waarmee u complexe stateful bedrijfslogica implementeren. Informatie over het gebruik van UDA's van de [Azure Stream Analytics gebruiker gedefinieerde JavaScript-verzamelingen](stream-analytics-javascript-user-defined-aggregates.md) documentatie. 

### <a name="live-data-testing-in-visual-studio"></a>Live gegevens testen in Visual Studio

Visual Studio-hulpprogramma's voor Azure Stream Analytics, verbeter de lokale testen functie waarmee u voor het testen van query's voor live-gebeurtenis-stromen van cloudbronnen, zoals Event Hub of IoT-hub. Meer informatie over het [live gegevens lokaal met behulp van Azure Stream Analytics-hulpprogramma's voor Visual Studio Test](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>.NET-gebruiker gedefinieerde functies op IoT Edge

Met .NET standard-door gebruiker gedefinieerde functies, kunt u de standaard .NET-code uitvoeren als onderdeel van uw streamingpijplijn automatiseert. U kunt eenvoudig C#-klassen maken of importeren van volledige-project en -bibliotheken. Volledige Maak- en debugfase van ervaring wordt ondersteund in Visual Studio. Ga voor meer informatie naar [.NET Standard ontwikkelen door de gebruiker gedefinieerde functies voor Azure Stream Analytics Edge-taken](stream-analytics-edge-csharp-udf-methods.md).

## <a name="other-previews"></a>Andere voorbeelden

De volgende functies zijn ook beschikbaar in preview.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge-and-cloud-announced-may-2019"></a>C#aangepaste deserializer voor Azure Stream Analytics op IoT Edge en de Cloud (Announced mei 2019)

Ontwikkelaars kunnen aangepaste deserializers in implementeren C# deserialiseren van gebeurtenissen die worden ontvangen door Azure Stream Analytics. Voorbeelden van indelingen die kunnen worden gedeserialiseerd zijn Parquet, Protobuf, XML of een binaire indeling. Aanmelden voor deze preview [hier](https://aka.ms/asapreview1).

### <a name="parquet-output-announced-may-2019"></a>Parquet-uitvoer (aangekondigd mei 2019)
Parquet is een kolomindeling inschakelen van efficiënte verwerking van big data. Door het uitvoeren van gegevens in Parquet-indeling in een data lake, kunt u profiteren van Azure Stream Analytics streaming ETL voor grootschalige power en verwerking van de batch, machine learning-algoritmen te trainen of interactieve query's uitvoeren op uw historische gegevens. Aanmelden voor deze preview [hier](https://aka.ms/asapreview1).

### <a name="one-click-integration-with-event-hubs-announced-may-2019"></a>Eénkliks-integratie met Event Hubs (Announced mei 2019) 
Met deze integratie kunt zich u nu binnenkomende gegevens visualiseren en beginnen met het schrijven van een Stream Analytics-query met één Klik vanuit de Event Hub-portal. Zodra uw query gereed is, kunt u zich het productize met enkele muisklikken en starten om realtime inzichten te verkrijgen. Dit zal aanzienlijk minder tijd en kosten voor het ontwikkelen van realtime analyseoplossingen. Aanmelden voor deze preview [hier](https://aka.ms/asapreview1).

### <a name="support-for-azure-stack-announced-may-2019"></a>Ondersteuning voor Azure Stack (aangekondigd mei 2019)
Deze functie is ingeschakeld op de Azure IoT Edge-runtime, maakt gebruik van aangepaste Azure Stack-functies, zoals de systeemeigen ondersteuning voor lokale invoer en uitvoer die wordt uitgevoerd op Azure Stack (bijvoorbeeld Event Hubs, IoT-Hub, Blob-opslag). Deze nieuwe integratie kunt u bouwen van hybride architecturen die uw gegevens dicht bij waar deze is gegenereerd, latentie te verlagen en inzichten te maximaliseren kunnen analyseren.
Aanmelden voor deze preview [hier](https://aka.ms/asapreview1).


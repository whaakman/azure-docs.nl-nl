---
title: Azure Stream Analytics preview-functies
description: In dit artikel geeft een lijst van de Azure Stream Analytics-functies die momenteel in preview.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/05/2018
ms.openlocfilehash: 124e936b619e3078c71094156bf91a437a28492b
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48862765"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics preview-functies

In dit artikel bevat een overzicht van alle functies die momenteel in Preview-versie van Azure Stream Analytics. Preview-functies gebruiken in een productieomgeving wordt niet aanbevolen.

## <a name="public-previews"></a>Openbare preview-versies

De volgende functies zijn in openbare preview. U kunt profiteren van deze functies vandaag, maar niet gebruiken in uw productieomgeving.

### <a name="azure-stream-analytics-on-iot-edge"></a>Azure Stream Analytics op IoT Edge

Azure Stream Analytics op IoT Edge kunnen ontwikkelaars near-real-time-analytics op IoT Edge-apparaten implementeren. Zie voor meer informatie de [Azure Stream Analytics op IoT Edge](stream-analytics-edge.md) documentatie.

### <a name="integration-with-azure-machine-learning"></a>Integratie met Azure Machine Learning

Stream Analytics-taken met Machine Learning (ML)-functies, kunt u schalen. Voor meer informatie over hoe u ML-functies in uw Stream Analytics-taak gebruiken kunt, gaat u naar [schalen van uw Stream Analytics-taak met Azure Machine Learning-functies](stream-analytics-scale-with-machine-learning-functions.md). Bekijk een Praktijkscenario met [uitvoeren van sentimentanalyses met behulp van Azure Stream Analytics en Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="session-windows"></a>Sessie-windows

Stream Analytics biedt ingebouwde ondersteuning voor windowing functies, zodat ontwikkelaars kunnen de auteur van complexe verwerking taken met een minimale inspanning. [Sessie windows](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) groep gebeurtenissen die op hetzelfde tijdstip binnenkomen, gefilterd op perioden wanneer er geen gegevens zijn. Voor meer informatie over windowing functies, gaat u naar [Inleiding tot Stream Analytics windowing functies](stream-analytics-window-functions.md).

### <a name="blob-output-partitioning-by-custom-time"></a>BLOB-uitvoer partitionering met aangepaste tijd

Azure Stream Analytics kunnen worden uitgevoerd naar Blob-opslag op basis van aangepaste kenmerken. Ga voor meer informatie naar [aangepaste datum/tijd-padpatronen voor Azure Stream Analytics blob storage-uitvoer](stream-analytics-custom-path-patterns-blob-storage-output.md).

### <a name="javascript-user-defined-aggregate"></a>De gebruiker gedefinieerde aggregatie JavaScript

Azure Stream Analytics ondersteunt de gebruiker gedefinieerde aggregaties (UDA) die zijn geschreven in JavaScript, waarmee u complexe stateful bedrijfslogica implementeren. Informatie over het gebruik van UDA's van de [Azure Stream Analytics gebruiker gedefinieerde JavaScript-verzamelingen](stream-analytics-javascript-user-defined-aggregates.md) documentatie. 

### <a name="live-data-testing-in-visual-studio"></a>Live gegevens testen in Visual Studio

Visual Studio-hulpprogramma's voor Azure Stream Analytics, verbeter de lokale testen functie waarmee u voor het testen van query's voor live-gebeurtenis-stromen van cloudbronnen, zoals Event Hub of IoT-hub. Meer informatie over het [live gegevens lokaal met behulp van Azure Stream Analytics-hulpprogramma's voor Visual Studio Test](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>.NET-gebruiker gedefinieerde functies op IoT Edge

Met .NET standard-door gebruiker gedefinieerde functies, kunt u de standaard .NET-code uitvoeren als onderdeel van uw streamingpijplijn automatiseert. U kunt eenvoudig C#-klassen maken of importeren van volledige-project en -bibliotheken. Volledige Maak- en debugfase van ervaring wordt ondersteund in Visual Studio. Ga voor meer informatie naar [.NET Standard ontwikkelen door de gebruiker gedefinieerde functies voor Azure Stream Analytics Edge-taken](stream-analytics-edge-csharp-udf-methods.md).

## <a name="private-previews"></a>Private Preview-versies

De volgende functies zijn in de beperkte Preview-versie. Voor toegang tot deze previews, gaat u naar de private preview van Azure Stream Analytics [aanmelden](https://aka.ms/ASApreview1) pagina.

### <a name="anomaly-detection"></a>Anomaliedetectie

Azure Stream Analytics introduceert nieuwe machine learning-modellen met ondersteuning voor *piek* en *dalen* detectie naast bidirectionele, trage positief en traag negatieve trends detecteren.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>C# aangepaste deserializer voor Azure Stream Analytics op IoT Edge

Ontwikkelaars kunnen nu aangepaste deserializers in C# voor het deserialiseren van gebeurtenissen die worden ontvangen door Azure Stream Analytics implementeren. Voorbeelden van indelingen die kunnen worden gedeserialiseerd zijn Parquet, Protobuf, XML of een binaire indeling.

### <a name="blob-output-partitioning-by-custom-attribute"></a>BLOB-uitvoer partitioneren door aangepast kenmerk

Het is nu mogelijk voor het partitioneren van de uitvoer van uw Azure Stream Analytics naar Blob-opslag op basis van een andere kolom in de query.

### <a name="managed-service-identity-msi-authentication-to-azure-data-lake-storage"></a>Beheerde Service Identity (MSI) verificatie met Azure Data Lake Storage

U kunt nu uw realtime pijplijnen met verificatie op basis van MSI tijdens het schrijven naar Azure Data Lake Storage Gen1 uitvoeren zodat u kunt taken via een programma te maken. Ga voor meer informatie naar [gebruik beheerde identiteiten die verificatie van Azure Stream Analytics-taken naar Azure Data Lake Storage-Gen1 uitvoer](stream-analytics-managed-identities-adls.md).

## <a name="next-steps"></a>Volgende stappen

* [Acht nieuwe functies in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [4 nieuwe functies nu beschikbaar in Azure Stream Analytics](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)

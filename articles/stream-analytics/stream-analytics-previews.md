---
title: Azure Stream Analytics preview-functies
description: In dit artikel geeft een lijst van de Azure Stream Analytics-functies die momenteel in preview.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/05/2019
ms.openlocfilehash: e4037e79812f0cf548ddfcc277c1b66332e0f1f7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57902872"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics preview-functies

In dit artikel bevat een overzicht van alle functies die momenteel in Preview-versie van Azure Stream Analytics. Preview-functies gebruiken in een productieomgeving wordt niet aanbevolen.

## <a name="public-previews"></a>Openbare preview-versies

De volgende functies zijn in openbare preview. U kunt profiteren van deze functies vandaag, maar niet gebruiken in uw productieomgeving.

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

## <a name="private-previews"></a>Private Preview-versies

De volgende functies zijn in de beperkte Preview-versie.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>C# aangepaste deserializer voor Azure Stream Analytics op IoT Edge

Ontwikkelaars kunnen nu aangepaste deserializers in C# voor het deserialiseren van gebeurtenissen die worden ontvangen door Azure Stream Analytics implementeren. Voorbeelden van indelingen die kunnen worden gedeserialiseerd zijn Parquet, Protobuf, XML of een binaire indeling.

### <a name="managed-identities-for-azure-resource-authentication-to-azure-data-lake-storage"></a>Beheerde identiteiten voor Azure-resource-verificatie met Azure Data Lake Storage

U kunt nu uw realtime pijplijnen met beheerde identiteiten voor Azure-resources op basis van verificatie tijdens het schrijven naar Azure Data Lake Storage Gen1 uitvoeren zodat u kunt taken via een programma te maken. Ga voor meer informatie naar [identiteiten beheerde gebruiken voor Azure-resources te verifiëren Azure Stream Analytics-taken naar Azure Data Lake Storage-Gen1 uitvoer](stream-analytics-managed-identities-adls.md).

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code for Azure Stream Analytics

De functies van Azure Stream Analytics-taken kunnen worden geschreven in Visual Studio Code. Voor toegang tot de hulpprogramma's private preview-functies, contact op met aan *ASAToolsfeedback\@microsoft.com*.

## <a name="next-steps"></a>Volgende stappen

* [Acht nieuwe functies in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [Vier nieuwe functies nu beschikbaar in Azure Stream Analytics](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)

---
title: Migreren van uw project naar het 3.0 API
titlesuffix: Azure Cognitive Services
description: Informatie over het migreren van Custom Vision projecten uit de vorige versie van de API in de 3.0 API.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: areddish
ms.openlocfilehash: 9dd473aadd7123cafc27209f5c34322fdbcffb71
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044003"
---
# <a name="migrate-to-the-30-api"></a>Migreren naar de 3.0 API

Custom Vision nu service algemeen beschikbaar is geworden en is onderworpen aan een API-update.
Deze update bevat een aantal nieuwe functies en, belangrijker is, een paar belangrijke wijzigingen:

* De Voorspellings-API is nu splitsen in twee op basis van het project.
* De optie voor het exporteren van Vision AI Developer Kit (VAIDK) vereist een project maken in een specifieke manier.
* Standaard-iteraties zijn verwijderd en vervangen door een publiceren / publicatie ongedaan maken van een benoemde iteratie.

Deze handleiding wordt beschreven hoe uw projecten werkt met de nieuwe API-versie bijwerken. Zie de [opmerkingen bij de Release](release-notes.md) voor een volledige lijst van de wijzigingen.

## <a name="use-the-updated-prediction-api"></a>De bijgewerkte voorspelling API gebruiken

De 2.x API's gebruikt dezelfde voorspelling aanroep voor zowel beeldclassificeerders en object detector projecten. Beide projecttypen zijn aanvaardbaar is voor de **PredictImage** en **PredictImageUrl** aanroepen. Beginnen met 3.0, hebben we deze API splitsen zodat u moet overeenkomen met het type voor de aanroep:

* Gebruik **[ClassifyImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15)** en **[ClassifyImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c14)** voorspellingen voor installatiekopie classificatie projecten ophalen.
* Gebruik **[DetectImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c19)** en **[DetectImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c18)** voorspellingen voor projecten met detectie van een object ophalen.

## <a name="use-the-new-iteration-publishing-workflow"></a>De nieuwe werkstroom voor het publiceren van iteratie gebruikt

De 2.x API's gebruikt de standaard-iteratie of een opgegeven herhaling-ID voor het kiezen van de iteratie moet worden gebruikt voor de voorspelling. 3.0 vanaf biedt hebben we vastgesteld dat een publishing stroom Gegevensbeheerders, waarin u eerst een iteratie onder de naam van een opgegeven van de training-API hebt gepubliceerd. Vervolgens geeft u de naam voor de voorspelling methoden om op te geven welke iteratie te gebruiken.

> [!IMPORTANT]
> De 3.0 API's gebruik niet de standaard iteratie-functie. Totdat we de oudere API's afschaffen, kunt u echter ook doorgaan met de 2.x API's in een iteratie als de standaard-of uitschakelen. Deze API's voor een bepaalde periode worden onderhouden en roept u de **[UpdateIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b818)** methode voor het markeren van een iteratie als standaard.

### <a name="publish-an-iteration"></a>Een iteratie publiceren

Zodra een iteratie wordt getraind, kunt u deze beschikbaar zijn voor het gebruik van de voorspelling van de **[PublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c82db28bf6a2b11a8247bbc)** methode. Als u wilt een iteratie publiceren, moet u de voorspelling van resource-ID, die beschikbaar is op de pagina instellingen van de website van de CustomVision.

![De Custom Vision website-instellingen-pagina met de voorspelling van resource-ID die worden beschreven.](./media/update-application-to-3.0-sdk/prediction-id.png)

> [!TIP]
> U krijgt ook deze informatie uit de [Azure Portal](https://portal.azure.com) door te gaan naar de resource Custom Vision voorspelling en selecteren **eigenschappen**.

Zodra uw iteratie is gepubliceerd, kunnen apps deze gebruiken voor voorspelling door de naam op te geven in de voorspellings-API-oproep. Als u een iteratie niet beschikbaar voor het aanroepen van de productie, gebruikt u de **[UnpublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b81a)** API.

## <a name="additional-export-options"></a>Opties voor extra exporteren

Met de 3.0 wordt weergegeven twee extra API's exporteren doelen: ARM-architectuur en Vision AI Developer Kit.

* Voor het gebruik van ARM, moet u alleen een compacte domein kiezen en kies vervolgens de docker-bestand en vervolgens ARM als de opties voor exporteren.
* Vision AI Dev Kit, het project moet worden gemaakt met de __Algemeen (cd)__ domein, evenals VAIDK op te geven in de doel-platforms argument exporteren.

## <a name="next-steps"></a>Volgende stappen

* [Training API-referentiedocumentatie (REST)](https://go.microsoft.com/fwlink/?linkid=865446)
* [Documentatie voor productie-API reference (REST)](https://go.microsoft.com/fwlink/?linkid=865445)
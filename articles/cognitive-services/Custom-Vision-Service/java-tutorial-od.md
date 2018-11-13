---
title: 'Snelstart: een objectdetectieproject maken met de Custom Vision-SDK voor Java'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg labels toe, upload afbeeldingen, train uw project en detecteer objecten met de Java-SDK.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 10/31/2018
ms.author: areddish
ms.openlocfilehash: d5dd481d32ede9a400def02d421a7cb8b816338a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51279582"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-sdk-for-java"></a>Snelstart: een objectdetectieproject maken met de Custom Vision-SDK voor Java

Dit artikel biedt informatie en voorbeeldcode om u op weg te helpen met de Custom Vision-SDK voor Java om een objectdetectiemodel te maken. Wanneer u het project hebt gemaakt, kunt u gelabelde regio's toevoegen, afbeeldingen uploaden, het project trainen, de standaardeindpunt-URL voor voorspellingen ophalen en het eindpunt gebruiken om afbeeldingen programmatisch te testen. Gebruik dit voorbeeld als een sjabloon om uw eigen Java-toepassing te maken. 

## <a name="prerequisites"></a>Vereisten

- Een Java IDE naar keuze
- [JDK 7 of 8](https://aka.ms/azure-jdks) is geïnstalleerd.
- Maven is geïnstalleerd

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>De Custom Vision-SDK en voorbeeldcode ophalen
Als u een Java-app wilt schrijven die gebruikmaakt van Custom Vision, hebt u de Maven-pakketten van Custom Vision nodig. Deze zijn opgenomen in het voorbeeldproject dat u gaat downloaden, maar u kunt ze hier afzonderlijk openen.

U kunt de Custom Vision-SDK uit de centrale maven-opslagplaats installeren:
* [Training-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Voorspelling-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Kloon of download het project [Voorbeelden voor Cognitive Services Java SDK](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Ga naar de map **Vision/CustomVision/**.

Met dit Java-project wordt een nieuwe objectdetectieproject van Custom Vision gemaakt met de naam __Sample Java OD Project__. Dit kan worden geopend via de [Custom Vision-website](https://customvision.ai/). Vervolgens worden afbeeldingen geüpload om een classificatie te trainen en te testen. In dit project moet met de classificatie worden vastgesteld of een boom een __Canadese den__ of een __Japanse sierkers__ is.

[!INCLUDE [get-keys](includes/get-keys.md)]

Het programma is zodanig geconfigureerd dat uw belangrijkste gegevens als omgevingsvariabelen worden opgeslagen. Stel deze variabelen in via de map **Vision/CustomVision** in PowerShell. Voer daarna de volgende opdrachten in:

```PowerShell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>De code begrijpen

Laad het `Vision/CustomVision`-project in uw Java IDE en open het bestand _CustomVisionSamples.java_. Zoek de methode **runSample** en maak de methodeaanroep **ImageClassification_Sample** inactief door er commentaar van te maken. Hierdoor wordt het afbeeldingsclassificatiescenario uitgevoerd, dat niet in deze handleiding wordt besproken. De methode **ObjectDetection_Sample** implementeert de primaire functie van deze snelstart, namelijk het navigeren naar de definitie ervan en het inspecteren van de code. 

### <a name="create-a-new-custom-vision-service-project"></a>Een nieuw Custom Vision Service-project maken

Ga naar het codeblok waarmee een trainingsclient en een objectdetectieproject worden gemaakt. Het project wordt weergegeven op de [Custom Vision-website](https://customvision.ai/), die u eerder hebt bezocht. 

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=181-206)]

### <a name="add-tags-to-your-project"></a>Tags toevoegen aan uw project

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=208-218)]

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

Als u afbeeldingen labelt in objectdetectieprojecten, dient u de regio van elk gelabeld object op te geven met behulp van genormaliseerde coördinaten. Ga naar de definitie van de kaart `regionMap`. Met deze code wordt elk voorbeeld van een afbeelding aan de bijbehorende gelabelde regio gekoppeld.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=130-179)]

Ga vervolgens naar het codeblok waarmee de afbeeldingen aan het project worden toegevoegd. De afbeeldingen worden gelezen vanuit de map **src/main/resources** van het project en worden met de bijbehorende labels en regiocoördinaten naar de service geüpload.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=220-231)]

Het vorige codefragment maakt gebruik van twee hulpfuncties die de afbeeldingen als resourcestreams ophalen en ze naar de service uploaden.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=277-314)]

### <a name="train-the-project"></a>Het project trainen

Met deze code wordt de eerste iteratie in het project gemaakt. De iteratie wordt gemarkeerd als de standaarditeratie. De standaarditeratie weerspiegelt de versie van het model die reageert op voorspellingsaanvragen. Update deze elke keer als u het model opnieuw gaat trainen.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=233-242)]

### <a name="use-the-prediction-endpoint"></a>Voorspellingseindpunt gebruiken

Het voorspellingseindpunt, dat door het `predictor`-object hier wordt weergegeven, is de verwijzing waarmee u een afbeelding naar het huidige model kunt verzenden en een classificatievoorspelling kunt verkrijgen. In dit voorbeeld wordt `predictor` elders gedefinieerd met de omgevingsvariabele voor de voorspellingssleutel.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=244-270)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Als u de oplossing met Maven wilt compileren en uitvoeren, voert u de volgende opdracht uit in de projectmap in PowerShell:

```PowerShell
mvn compile exec:java
```

Bekijk de uitvoer in de console voor logboekregistratie en voorspellingsresultaten. Vervolgens kunt u controleren of de testafbeelding op de juiste wijze wordt gelabeld en of de detectieregio juist is.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe elke stap van het objectdetectieproces in code kan worden uitgevoerd. Met dit voorbeeld wordt één trainingsinteratie uitgevoerd, maar vaak zult u uw model meerdere keren willen trainen en testen om het nauwkeuriger te maken. In de volgende handleiding wordt classificatie van afbeeldingen behandeld. De principes zijn soortgelijk aan die van objectdetectie.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](test-your-model.md)
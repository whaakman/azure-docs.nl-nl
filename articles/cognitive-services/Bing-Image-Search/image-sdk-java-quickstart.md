---
title: 'Snelstartgids: Zoeken voor afbeeldingen met behulp van de Bing afbeeldingen zoeken-SDK en Java'
description: Gebruik deze Quick Start om te zoeken en -installatiekopieën zoeken op Internet met behulp van de Bing afbeeldingen zoeken-SDK en Java.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 12bd6f9a9a0b43b4571a7e0311ffbea54c7b9054
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574058"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-java"></a>Snelstartgids: Zoeken voor afbeeldingen met de Bing afbeeldingen zoeken-SDK en Java

Met deze Quick Start kunt u uw eerste afbeeldingen zoeken met behulp van de Bing afbeeldingen zoeken SDK, die een wrapper voor de API en bevat de dezelfde functies. Deze eenvoudige Java-toepassing verzendt een zoekquery afbeelding, parseert de JSON-antwoord en Hiermee geeft u de URL van de eerste afbeelding geretourneerd.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingImageSearch/Quickstart) met extra foutafhandeling en aantekeningen. 

## <a name="prerequisites"></a>Vereisten 

De nieuwste versie van de [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK)

Installeer de Bing afbeeldingen zoeken-SDK-afhankelijkheden met behulp van Maven of Gradle, een ander systeem voor Afhankelijkheidsbeheer. Het Maven POM-bestand moet de declaratie van het volgende:

```xml
 <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-imagesearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
 </dependencies> 
```

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Maken en initialiseren van de toepassing

1. Maak een nieuwe Java-project in uw favoriete IDE of editor, en voeg de volgende import toe aan uw klasse implimentation:

    ```java
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchAPI;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchManager;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImageObject;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImagesModel;
    ```

2. Variabelen voor de abonnementssleutel van uw maken in uw belangrijkste methode en zoekterm. Vervolgens exemplaar maken van de Bing afbeeldingen zoeken-client.

    ```java
    final String subscriptionKey = "COPY_YOUR_KEY_HERE";
    String searchTerm = "canadian rockies";
    //Image search client
    BingImageSearchAPI client = BingImageSearchManager.authenticate(subscriptionKey);
    ```

## <a name="send-a-search-request-to-the-bing-image-search-api"></a>Een search-aanvraag verzenden naar de Bing afbeeldingen zoeken-API

1. Met behulp van `bingImages().search()`, verzenden van de HTTP-aanvraag met de zoekquery. Sla het antwoord als een `ImagesModel`.
    ```java
    ImagesModel imageResults = client.bingImages().search()
                .withQuery(searchTerm)
                .withMarket("en-us")
                .execute();
    ```

## <a name="parse-and-view-the-result"></a>Parseren en het resultaat weer te geven

Parseren van de afbeeldingsresultaten in het antwoord geretourneerd.
Als het antwoord bevat een lijst met zoekresultaten, het eerste resultaat opslaan en afdrukken van de details, zoals een miniatuur-URL, wordt de oorspronkelijke URL, samen met het totale aantal installatiekopieën geretourneerd.  

```java
if (imageResults != null && imageResults.value().size() > 0) {
    // Image results
    ImageObject firstImageResult = imageResults.value().get(0);

    System.out.println(String.format("Total number of images found: %d", imageResults.value().size()));
    System.out.println(String.format("First image thumbnail url: %s", firstImageResult.thumbnailUrl()));
    System.out.println(String.format("First image content url: %s", firstImageResult.contentUrl()));
}
else {
        System.out.println("Couldn't find image results!");
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) 
* [Java-voorbeelden voor de SDK van Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

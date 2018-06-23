---
title: Afbeelding inzichten verkrijgen | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe de Bing installatiekopie zoeken-API gebruiken voor meer informatie over een afbeelding.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0BCD936E-D4C0-472D-AE40-F4B2AB6912D5
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: f651d9f773f475e633aed698e134aa6a7c07393b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345477"
---
# <a name="get-insights-about-an-image"></a>Inzicht in een afbeelding ophalen

> [!IMPORTANT]
> In plaats van de installatiekopieën/details van het eindpunt naar afbeelding inzichten verkrijgen, moet u [Visual zoeken](../bing-visual-search/overview.md) omdat het uitgebreider inzicht biedt.


Elke installatiekopie bevat een token insights die u gebruiken kunt voor informatie over de installatiekopie. Bijvoorbeeld, krijgt u een verzameling verwante afbeeldingen, webpagina's met de afbeelding of een lijst met verkopers waar u het product weergegeven in de installatiekopie kunt kopen.  
  
Voor inzicht in een installatiekopie kunt vastleggen van de afbeelding [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken) token in het antwoord. 

```json
"value" : [{
        . . .
        "name":"sailing dinghy.jpg",
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "insightsSourcesSummary" : {
            "shoppingSourcesCount" : 9,
            "recipeSourcesCount" : 0
        },
        . . .
}],
```

Vervolgens aanroepen van het eindpunt van de Details van de installatiekopie en stel de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) queryparameter naar het token in `imageInsightsToken`.  

Als de inzichten die u wilt opgeven, stelt u de `modules` queryparameter. Als u alle inzichten, ingesteld `modules` naar `All`. Als u alleen de bijschrift en verzameling inzichten, ingesteld `modules` naar `Caption%2CCollection`. Zie voor een volledige lijst van mogelijke inzicht [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested). Niet alle insights zijn beschikbaar voor alle installatiekopieën. Het antwoord bevat alle insights die u hebt aangevraagd, indien beschikbaar.

Alle beschikbare inzichten voor de voorgaande afbeelding-aanvragen in het volgende voorbeeld.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_D6426898706EC7...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
``` 

## <a name="getting-insights-of-a-known-image"></a>Inzichten van een installatiekopie van een bekende ophalen

Als u de URL naar een afbeelding die u wilt inzichten van verkrijgen, gebruikt u de [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl) queryparameter in plaats van de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parameter om de installatiekopie. Of als u het bestand hebt, kunt u het binaire bestand van de installatiekopie kunt verzenden in de hoofdtekst van een POST-aanvraag. Als u een POST-aanvraag met de `Content-Type` header moet worden ingesteld op `multipart/data-form`. In beide gevallen kan de grootte van de installatiekopie niet meer dan 1 MB.  
  
Als u een URL naar de installatiekopie hebt, geeft het volgende voorbeeld laat zien hoe aanvragen insights van een afbeelding.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&imgUrl=https%3A%2F%2Fwww.mydomain.com%2Fimages%2Fsunflower.png&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
``` 
  
## <a name="getting-all-image-insights"></a>Alle installatiekopie insights ophalen  

Instellen voor het aanvragen van alle insights met een installatiekopie van het [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) queryparameter naar `All`. Als u verwante zoekopdrachten, moet de aanvraag van de gebruiker queryreeks bevatten. Dit voorbeeld ziet u met behulp van de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) om op te geven van de installatiekopie.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_68364D764J...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Het object dat op het hoogste niveau is een [ImageInsightsResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) object in plaats van een [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) object.  
  
```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "bcid_3297E6A54E4787A5F51C49D9BA342B9A*ccid_Fe2Hx...",
    "bestRepresentativeQuery" : {
        "text" : "Sailing Dinghy",
        "displayText" : "Sailing Dinghy",
        "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Sailing+Dinghy...",
    },
    "pagesIncluding" : {
        "value" : [
            {
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
                "name" : "Powerboating Dublin, Dinghy Sailing Courses...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
                "datePublished" : "2017-01-20T00:41:00.0000000Z",
                "contentUrl" : "http:\/\/www.contoso.ie\/content...",
                "hostPageUrl" : "http:\/\/www.contoso.ie\/powerboating...",
                "contentSize" : "59063 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "www.contoso.ie\/powerboating...",
                "width" : 800,
                "height" : 600,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 225
                },
                "imageInsightsToken" : "ccid_pHjQIA0x*mid_17F61B1316A39C92214...",
                "imageId" : "17F61B1316A39C922143FFDE9DFB5B0FB41171",
                "accentColor" : "0997C2"
            },
            . . .
        ]
    },
    "relatedSearches" : {
        "value" : [
            {
                "text" : "Sailing Fun",
                "displayText" : "Sailing Fun",
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Sailing...",
                "thumbnail" : {
                    "url" : "https:\/\/tse1.mm.bing.net\/th?q=Sailing+Fun..."
                }
            },
            . . .
        ]
    },
    "visuallySimilarImages" : {
        "value" : [
            {
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
                "name" : "Weekend On the Water",
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2010-09-05T12:00:00.0000000Z",
                "contentUrl" : "http:\/\/1.bp.contoso.com\/_dc_6...",
                "hostPageUrl" : "http:\/\/contoso.com\/2010...",
                "contentSize" : "203806 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "contoso.com\/2010...",
                "width" : 1600,
                "height" : 1249,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 234
                },
                "imageInsightsToken" : "ccid_Jg1Kwuc4*mid_5B7DA43976D3A422...",
                "imageId" : "5B7DA43976D3A422BA679A3AB019BB52C08DBC",
                "accentColor" : "0B2543"
            },
            . . .
        ]
    },
    "imageTags" : {
        "value" : [
            {
                "name" : "sail boat"
            },
            . . .
        ]
    }
}
```

## <a name="recognizing-entities-in-an-image"></a>Entiteiten in een installatiekopie te herkennen  

De functie voor entiteit identificeert entiteiten in een afbeelding, momenteel alleen de personen. Instellen voor entiteiten in een installatiekopie van het [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) queryparameter naar `RecognizedEntities`.  

> [!NOTE]
> U kunt deze module niet opgeven met elke andere module. Als u deze module met andere modules opgeeft, is het antwoord bevat geen herkende entiteiten.  
  
Hieronder vindt u de installatiekopie opgeven met behulp van de [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl) parameter. Houd er rekening mee naar URL de queryparameters coderen.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=faith+hill&insightsToken=mid_68364D764J...&modules=RecognizedEntities&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  
  
Hieronder ziet u het antwoord op de vorige aanvraag. Aangezien de afbeelding twee personen met bevat, het antwoord geeft aan dat een regio voor elke persoon. In dit geval de personen die u in de groepen CelebrityAnnotations en CelebRecognitionAnnotations herkend. Bing worden de personen in elke groep op basis van de kans dat ze overeenkomen met de persoon in de originele installatiekopie. De lijst is in aflopende volgorde van vertrouwen. De groep CelebRecognitionAnnotations biedt het hoogste niveau van het vertrouwen dat overeenkomst met de juist is.  
  
```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "recognizedEntityGroups" : {
        "value" : [
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebRecognitionAnnotations"
            },
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebrityAnnotations"
            }
        ]
    }
}
```
  
De `region` veld identificeert het gebied van de afbeelding waarop de entiteit in Bing worden herkend. Voor personen vertegenwoordigt de regio van de persoon face.  
  
De waarden van de rechthoek zijn ten opzichte van de breedte en hoogte van de originele installatiekopie en zijn in het bereik 0,0 en 1,0. Bijvoorbeeld, als de installatiekopie 300 x 200 en boven de regio is, linkerhoek op punt (10, 20) en onder rechterhoek is op punt (290, 150), wordt de genormaliseerde rechthoek is:  
  
-   Links: 10 / 300 = 0.03333...  
-   Top: 20 / 200 = 0,1  
-   Rechts: 290 / 300 = 0.9667...  
-   Onder: 150 / 200 = 0,75  
  
U kunt de regio die Bing in latere insights aanroepen retourneert. Als u bijvoorbeeld visueel soortgelijke afbeeldingen van de herkende entiteit ophalen. Zie voor meer informatie [te gebruiken met een visueel vergelijkbare en entiteit erkenning Modules installatiekopieën bijsnijden](#croppingimages). Hieronder ziet u de toewijzing tussen de velden regio en de queryparameters dat u gebruiken wilt voor het bijsnijden van afbeeldingen.  
  
-   Links wordt toegewezen aan [cal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
-   Top is toegewezen aan [cat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
-   Rechts wordt toegewezen aan [auto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
-   Toewijzingen naar beneden [CAB-bestand](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  

## <a name="finding-visually-similar-images"></a>Het zoeken van elkaar lijkende afbeeldingen  

Ga voor installatiekopieën die visueel vergelijkbaar met de originele installatiekopie zijn ingesteld de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) queryparameter naar SimilarImages.  
  
De volgende aanvraag laat zien hoe visueel soortgelijke afbeeldingen ophalen. De aanvraag gebruikt de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) queryparameter om te identificeren van de originele installatiekopie. Ter verbetering van de relevantie, moet u de queryreeks van de gebruiker opnemen.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?insightsToken=mid_68364D764J...&modules=SimilarImages&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

  
Hieronder ziet u het antwoord op de vorige aanvraag.  
  
```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "visuallySimilarImages" : {
        "value" : [
            {
                "name" : "typical Hawaiian Sunset! :) | Scenes of Hawaii",
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detailv2...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP.Mda2a86...",
                 . . .
            }
        ]
    }
```
  
## <a name="cropping-images-to-use-with-visually-similar-and-entity-recognition-modules"></a>Te gebruiken met installatiekopieën bijsnijden visueel vergelijkbare en entiteit voor modules  

De regio van de installatiekopie die gebruikmaakt van Bing om te bepalen of installatiekopieën er hetzelfde uitzien of entiteit erkenning uitvoeren, gebruikt u de [cal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal), [cat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat), [cab](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab), en [auto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car) queryparameters. Bing gebruikt standaard de volledige afbeelding.  
  
De parameters geven boven, linkerhoek en onder, rechterhoek van de regio die gebruikmaakt van Bing voor vergelijking. Geef de waarden als breuken van de breedte en hoogte van de originele installatiekopie. De decimale waarden beginnen met (0,0, 0,0) boven, linkerhoek en eindigen met (1.0, 1.0) in de rechterbenedenhoek. Bijvoorbeeld wilt opgeven dat de linkerbovenhoek hoek een kwartaal van de manier waarop omlaag vanaf de bovenkant en een kwartaal van de manier in vanaf de linkerkant begint, stelt `cal` op 0,25 en `cat` 0,25.  
  
De volgende reeks aanroepen ziet u het effect van het opgeven van de bijsnijden regio. De eerste aanroep bevatten geen bijsnijden en Bing herkent twee personen met elkaar permanent in het midden van de installatiekopie.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Het antwoord ziet u twee herkende entiteiten.  
  
```json 
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value": [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                },  
                {  
                    "region" : {  
                        "left" : 0.25,  
                        "top" : 0.2488889,  
                        "right" : 0.4466667,  
                        "bottom" : 0.5111111  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Marcus Appel",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
            "name" : "CelebRecognitionAnnotations"
        }]
    }  
}  
```  
  
De afbeelding verticaal omlaag het midden van de tweede aanroep bijsnijden en Bing herkend één persoon aan de rechterkant van de afbeelding.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?cal=0.5&cat=0.0&car=1.0&cab=1.0&modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Het antwoord bevat één herkende entiteit.  
  
```json  
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value" : [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
                "name" : "CelebRecognitionAnnotations"  
            }
        ]
    }
}  
```  
  
## <a name="finding-visually-similar-products"></a>Visueel vergelijkbare producten zoeken  

Om te vinden van afbeeldingen met producten die visueel vergelijkbaar met de producten gevonden in de originele installatiekopie zijn, stelt de [modueles](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) queryparameter naar SimilarProducts.  
  
De volgende aanvraag laat zien hoe afbeeldingen van visueel vergelijkbare producten ophalen. De aanvraag gebruikt de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) query-parameter voor het identificeren van de oorspronkelijke installatiekopie die is geretourneerd in een eerdere aanvraag. Ter verbetering van de relevantie, moet u de queryreeks van de gebruiker opnemen.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=anne+klein+dresses&modules=SimilarProducts&insightsToken=ccid_WOeyfoSp*mid_4B0A357&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```
  
Hieronder ziet u het antwoord op de vorige aanvraag. Het antwoord bevat een afbeelding van een vergelijkbaar product en geeft aan hoeveel verkopers bieden het product online of er product classificaties zijn en de laagste prijs gevonden (Zie de `aggregateOffer` veld).  
  
```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "visuallySimilarProducts" : {
        "value" : [
            {
                "name" : "Sequin One-Shoulder Twist-Drape Dress",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=de...",  
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP.M85bdee...",  
                . . .
            },
            . . .
        ]
    }
}
```
  
Voor een lijst van de verkopers die worden geboden door het product on line (Zie de [offerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer-offercount) veld), roept u de API en stelt u `modules` naar ShoppingSources. Vervolgens stelt u de `insightsToken` queryparameter naar het token gevonden in de installatiekopie van de samenvatting van het product.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=ShoppingSources&insightsToken=ccid_hb3uRvUk*mid_BF5C252A47F2C765...&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Hier volgt het antwoord op de vorige aanvraag.  
  
```json  
{  
    "_type" : "ImageInsights",  
    "shoppingSources" : {  
        "offers" : [{  
            "url" : "http:\/\/www.contoso.com\/dp\/B00O...",  
            "seller" : {  
                "name" : "Contoso",  
                "image" : {  
                    "url" : "https:\/\/tse3.mm.bing.net\/th?id=A10d50fe..."  
                }  
            },  
            "price" : 126.87,  
            "priceCurrency" : "USD",  
            "availability" : "InStock"  
        },  
        {  
            "url" : "http:\/\/www.adatum.com\/product\/heritage...\/",  
            "seller" : {  
                "name" : "fabrikam.com"  
            },  
            "price" : 495,  
            "priceCurrency" : "USD",  
            "availability" : "InStock"  
        }]  
    }  
}  
```

---
title: Krijg inzichten van de installatiekopie - Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Bing afbeeldingen zoeken-API voor meer informatie over een afbeelding.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 0BCD936E-D4C0-472D-AE40-F4B2AB6912D5
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 03/04/2019
ms.author: scottwhi
ms.openlocfilehash: 33dfbe70d75335eca79d32807407b97e32c2dbbf
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440859"
---
# <a name="get-image-insights-with-the-bing-image-search-api"></a>Verkrijg inzichten van de installatiekopie met de Bing afbeeldingen zoeken-API

> [!IMPORTANT]
> In plaats van het eindpunt/afbeeldingen/details om installatiekopie-inzichten te verkrijgen, moet u [visuele zoekopdrachten](../bing-visual-search/overview.md) omdat het uitgebreidere inzichten biedt.


Elke installatiekopie bevat een token inzichten die u gebruiken kunt voor informatie over de installatiekopie. Bijvoorbeeld, krijgt u een verzameling van gerelateerde afbeeldingen, webpagina's die de installatiekopie van het opnemen of een lijst kunnen maatschappijen waar u het product weergegeven in de afbeelding kunt kopen.  

Vastleggen van de afbeelding om inzichten te verkrijgen over een afbeelding, [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken) token in het antwoord.

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

Vervolgens de Details van de installatiekopie-eindpunt aanroepen en stel de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) queryparameter naar het token in `imageInsightsToken`.  

Als de inzichten die u wilt opgeven, stelt de `modules` queryparameter. Alle om inzichten te verkrijgen, stellen `modules` naar `All`. Alleen het bijschrift en verzameling om inzichten te verkrijgen, stellen `modules` naar `Caption%2CCollection`. Zie voor een volledige lijst van mogelijke insights [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested). Niet alle inzichten zijn beschikbaar voor alle installatiekopieën. Het antwoord bevat alle inzichten die u hebt aangevraagd, indien beschikbaar.

Het volgende voorbeeld vraagt alle beschikbare inzichten voor de voorgaande afbeelding.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_D6426898706EC7...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

## <a name="getting-insights-of-a-known-image"></a>Inzicht van de installatiekopie van een bekende ophalen

Als u de URL naar een afbeelding die u wilt krijg inzichten van hebt, gebruikt u de [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl) queryparameter in plaats van de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parameter opgeven voor de installatiekopie. Of, als u het bestand hebt, kunt u het binaire bestand van de installatiekopie kunt verzenden in de hoofdtekst van een POST-aanvraag. Als u een POST-aanvraag, de `Content-Type` header moet worden ingesteld op `multipart/data-form`. In beide gevallen kan de grootte van de installatiekopie van het niet meer dan 1 MB.  

Hebt u een URL naar de installatiekopie, ziet het volgende voorbeeld u hoe om aan te vragen van inzichten van een afbeelding.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&imgUrl=https%3A%2F%2Fwww.mydomain.com%2Fimages%2Fsunflower.png&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

## <a name="getting-all-image-insights"></a>Ophalen van alle inzichten in afbeeldingen  

Instellen om aan te vragen alle inzichten van een afbeelding, de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parameter query `All`. Als u verwante zoekopdrachten, moet de aanvraag van de gebruiker queryreeks bevatten. In dit voorbeeld laat zien met de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) om op te geven van de installatiekopie.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_68364D764J...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Het object op het hoogste niveau is een [ImageInsightsResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) object in plaats van een [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) object.  

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

## <a name="recognizing-entities-in-an-image"></a>Entiteiten in een afbeelding herkennen  

De functie voor de entiteit identificeert entiteiten in een afbeelding, momenteel alleen de personen. Voor het identificeren van entiteiten in een afbeelding, stel de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parameter query `RecognizedEntities`.  

> [!NOTE]
> U kunt deze module niet opgeven met elke andere module. Als u deze module met andere modules opgeeft, is het antwoord bevat geen herkende entiteiten.  

Hieronder ziet u hoe u de installatiekopie opgeven met behulp van de [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl) parameter. Houd er rekening mee naar URL de queryparameters coderen.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=faith+hill&insightsToken=mid_68364D764J...&modules=RecognizedEntities&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Hieronder ziet u het antwoord op de vorige aanvraag. Aangezien de afbeelding twee personen bevat, het antwoord geeft aan dat een regio voor elke persoon. In dit geval zijn de mensen herkend in de groepen CelebrityAnnotations en CelebRecognitionAnnotations. Bing geeft een lijst van de mensen in elke groep op basis van de kans dat ze overeenkomen met de persoon die in de oorspronkelijke afbeelding. De lijst is in aflopende volgorde van vertrouwen. De groep CelebRecognitionAnnotations biedt het hoogste niveau van het vertrouwen dat de overeenkomst juist is.  

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

De `region` veld identificeert het gebied van de afbeelding waar de entiteit in Bing worden herkend. Voor personen vertegenwoordigt de regio van de persoon face.  

De waarden van de rechthoek zijn ten opzichte van de breedte en hoogte van de oorspronkelijke afbeelding en in het bereik 0,0 en 1,0 zijn. Bijvoorbeeld als de afbeelding 300 x 200 en van de regio-bovenaan is, links bevindt zich op een moment (10, 20) en onder rechterhoek op een moment (290, 150), wordt de genormaliseerde rechthoek is:  

-   Links: 10 / 300 = 0.03333...  
-   Boven:  20 / 200 = 0.1  
-   Rechts: 290 / 300 = 0.9667...  
-   Onder: 150 / 200 = 0.75  

U kunt de regio die Bing in latere insights aanroepen retourneert. Als u bijvoorbeeld visueel vergelijkbare afbeeldingen van de herkende entiteit ophalen. Zie voor meer informatie te gebruiken met visueel vergelijkbare en entiteit erkenning Modules bijsnijden installatiekopieën. Hieronder ziet u de toewijzing tussen de velden van de regio en de queryparameters die u gebruiken wilt om te bijsnijden afbeeldingen.  

-   Links wordt toegewezen aan [cal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
-   Top is toegewezen aan [cat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
-   Rechts wordt toegewezen aan [auto's](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
-   Toewijzingen naar beneden [CAB-bestand](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  

## <a name="finding-visually-similar-images"></a>Het zoeken van visueel vergelijkbare afbeeldingen  

Om te vinden van installatiekopieën die visueel vergelijkbaar met de oorspronkelijke afbeelding zijn, stelt de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parameter SimilarImages query.  

De volgende aanvraag laat zien hoe visueel vergelijkbare afbeeldingen. Maakt gebruik van de aanvraag de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) queryparameter voor het identificeren van de oorspronkelijke afbeelding. Ter verbetering van relevantie, moet u de queryreeks van de gebruiker bevatten.  

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

## <a name="cropping-images-to-use-with-visually-similar-and-entity-recognition-modules"></a>Installatiekopieën voor gebruik met bijsnijden visueel vergelijkbare en entiteit erkenning modules  

Om op te geven de regio van de installatiekopie die gebruikmaakt van Bing om te bepalen of afbeeldingen visueel vergelijkbare zijn of voor het uitvoeren van de entiteit erkenning, gebruikt u de [cal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal), [cat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat), [CAB-bestand](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab), en [auto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car) queryparameters. Bing gebruikt standaard de volledige afbeelding.  

De parameters geven de boven, linkerhoek en onder, de rechterhoek van de regio die Bing voor de vergelijking gebruikt. Geef de waarden als delen van de breedte en hoogte van de oorspronkelijke afbeelding. De decimale waarde begint met (0.0, 0.0) aan de bovenkant, linkerhoek en eindigen met (1.0, 1.0) in de rechterbenedenhoek. Bijvoorbeeld, als u wilt opgeven dat de hoek van de linkerbovenhoek van de manier waarop een kwart omlaag vanaf de bovenkant en een kwart van de manier aan de linkerkant begint, ingesteld `cal` op 0,25 en `cat` 0,25.  

De volgende reeks aanroepen ziet u het effect van de bijsnijden regio op te geven. De eerste aanroep bevatten niet bijsnijden en Bing herkent twee personen permanent naast elkaar in het midden van de installatiekopie.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Het antwoord bevat twee herkende entiteiten.  

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

De tweede aanroep snijdt de afbeelding verticaal in het midden en Bing één persoon herkend aan de rechterkant van de installatiekopie.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?cal=0.5&cat=0.0&car=1.0&cab=1.0&modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Het antwoord geeft één herkende entiteit.  

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

Instellen als u wilt zoeken in afbeeldingen met producten die visueel vergelijkbaar met de producten gevonden in de oorspronkelijke afbeelding zijn, de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parameter SimilarProducts query.  

De volgende aanvraag laat zien hoe u afbeeldingen van visueel vergelijkbare producten. Maakt gebruik van de aanvraag de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) query parameter voor het identificeren van de oorspronkelijke afbeelding die is geretourneerd in een eerdere aanvraag. Ter verbetering van relevantie, moet u de queryreeks van de gebruiker bevatten.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=anne+klein+dresses&modules=SimilarProducts&insightsToken=ccid_WOeyfoSp*mid_4B0A357&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Hieronder ziet u het antwoord op de vorige aanvraag. Het antwoord bevat een afbeelding van een soortgelijke product en geeft aan hoeveel kunnen maatschappijen bieden online, het product of er Productbeoordelingen, en de laagste koers gevonden (Zie de `aggregateOffer` veld).  

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

Voor een lijst van de verkopers die het product online aan (Zie de [offerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) veld), de API opnieuw aanroepen en stel `modules` naar ShoppingSources. Vervolgens stelt u de `insightsToken` queryparameter naar het token gevonden in de installatiekopie van de samenvatting van het product.  

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

---
title: De Computer Vision-API aanroepen | Microsoft Docs
description: Meer informatie over het aanroepen van de Computer Vision-API met behulp van REST in Cognitive Services.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/20/2017
ms.author: kefre
ms.openlocfilehash: 34705681e665b57a89c43f31ca695e0acb45ae3f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760230"
---
# <a name="how-to-call-the-computer-vision-api"></a>Over het aanroepen van de Computer Vision-API

Deze handleiding laat zien over het aanroepen van de Computer Vision-API met behulp van REST. De voorbeelden zijn geschreven in C# met behulp van de clientbibliotheek van de Computer Vision-API, zowel als HTTP POST/GET-aanroepen. We richten ons op:

-   Klik hier voor meer informatie over het ophalen van 'Tags', 'Description' en 'Categorieën'.
-   Klik hier voor meer informatie over het ophalen van "Domeinspecifieke" gegevens (beroemdheden).

### <a name="Prerequisites">Vereisten</a> 
Afbeeldings-URL of pad naar lokaal opgeslagen afbeelding.
  * Invoermethoden ondersteund: Raw image binaire in de vorm van een toepassing/octet-stream of de afbeelding-URL
  * Ondersteunde afbeeldingsindelingen voor: JPEG, PNG-, GIF-, BMP
  * Bestand Afbeeldingsgrootte: minder dan 4MB
  * De installatiekopie van dimensie: groter is dan 50 x 50 pixels
  
In de onderstaande voorbeelden worden getoond van de volgende functies:

1. Analyse van een installatiekopie en het ophalen van een matrix van tags en een beschrijving die wordt geretourneerd.
2. Analyse van een afbeelding met een model domeinspecifieke (met name 'beroemdheden'-model) en het ophalen van de bijbehorende leiden tot JSON stemmen.

Functies zijn onderverdeeld in:

  * **Optie 1:** binnen bereik van analyse - alleen voor een bepaald model analyseren
  * **Optie 2:** geavanceerde analyse - analyseren voor meer informatie met [86 categorieën taxonomie](../Category-Taxonomy.md)
  
### <a name="Step1">Stap 1: Toestaan dat de API-aanroep</a> 
Voor elke aanroep naar de Computer Vision-API is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een tekenreeksparameter of zijn opgegeven in de aanvraagheader. 

Als u wilt een abonnementssleutel, Zie [hoe Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md
).

**1.** De abonnementssleutel via een query-tekenreeks doorgegeven, hieronder vindt u een Computer Vision-API-voorbeeld:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** De abonnementssleutel doorgeven kan ook worden opgegeven in de header van de HTTP-aanvraag:

```ocp-apim-subscription-key: <Your subscription key>```

**3.** Wanneer u de clientbibliotheek, wordt de abonnementssleutel doorgegeven de constructor van VisionServiceClient:

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">Stap 2: Upload een afbeelding naar de Computer Vision-API-service en teruggaan tags, beschrijvingen en beroemdheden</a>
De eenvoudige manier voor het uitvoeren van de Computer Vision-API-aanroep is door rechtstreeks uploaden van een afbeelding. Dit wordt gedaan door een aanvraag "POST" met de inhoudstype application/octet-stream samen met de gegevens lezen van de installatiekopie te verzenden. Voor 'Tags' en 'Description', wordt deze Uploadmethode zijn hetzelfde voor alle aanroepen van de Computer Vision-API. Het enige verschil is de queryparameters die de gebruiker opgeeft. 

Hier is 'Tags' en 'Description' ophalen voor een bepaalde installatiekopie:

**Optie 1:** aan de lijst 'Labels' een 'Description'
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```
```
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```
**Optie twee** -lijst ophalen van 'Labels' alleen, of lijst met 'Description' alleen:

###### <a name="tags-only"></a>Alleen-codes:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Alleen-beschrijving:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>Hier is over het verkrijgen van domeinspecifieke analysis (in ons geval voor beroemdheden).

**Optie 1:** binnen bereik van analyse - alleen voor een bepaald model analyseren
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
Alle andere queryparameters {visualFeatures, details} zijn voor deze optie is niet geldig. Als u zien van alle ondersteunde modellen wilt, gebruikt: 
```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```
**Optie 2:** geavanceerde analyse - analyseren voor meer informatie met [86 categorieën taxonomie](../Category-Taxonomy.md)

Voor toepassingen waar u wilt de analyse van de installatiekopie van het algemene gedetailleerde gegevens ophalen uit een of meer domeinspecifieke modellen, uitbreiden we de v1-API met de queryparameter modellen.
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```
Wij zullen de classificatie 86-categorie wanneer deze methode wordt aangeroepen, eerst bellen. Als een van de categorieën die van een model overeenkomen met bekende/overeenkomen, wordt een tweede fase van de classificatie-aanroepen uitgevoerd. Bijvoorbeeld, als ' details = all ', of 'details' 'beroemdheden' bevatten, noemen we het model beroemdheden nadat de classificatie 86 categorie wordt genoemd en het resultaat de persoon die categorie bevat. Hierdoor neemt de latentie voor gebruikers die geïnteresseerd zijn in beroemdheden, vergeleken met een optie.

Alle v1-queryparameters zal werken op dezelfde manier in dit geval.  Als visualFeatures = categorieën niet is opgegeven, wordt impliciet worden ingeschakeld.

### <a name="Step3">Stap 3: Het ophalen van en inzicht krijgen in de JSON-uitvoer voor analyseren en visualFeatures Tags, beschrijving =</a>

Hier volgt een voorbeeld:
```
  {
    “tags”: [
    {
    "name": "outdoor",
        "score": 0.976
    },
    {
    "name": "bird",
        "score": 0.95
    }
            ],
    “description”: 
    {
    "tags": [
    "outdoor",
    "bird"
            ],
    "captions": [
    {
    "text”: “partridge in a pear tree”,
            “confidence”: 0.96
    }
                ]
    }
  }
```
Veld   | Type  | Inhoud
------|------|------|
Tags    | object    | Op het hoogste niveau object voor de matrix met tags
[] met tags. De naam | tekenreeks    | Trefwoorden uit tags classificatie
[] met tags. Score    | getal    | Betrouwbaarheidsscore tussen 0 en 1.
description  | object   | Op het hoogste niveau object voor een beschrijving.
Description.tags] |    tekenreeks  | Lijst met tags.  Als er onvoldoende vertrouwen in de mogelijkheid voor het produceren van een bijschrift bij van de tags mogelijk de enige informatie beschikbaar voor de oproepende functie.
Description.captions[].Text | tekenreeks    | Een wachtwoordzin met een beschrijving van de installatiekopie.
Description.captions[].Confidence   | getal    | Vertrouwen voor de zin.

### <a name="Step4">Stap 4: Het ophalen van en inzicht krijgen in de JSON-uitvoer van domeinspecifieke modellen</a>

**Optie 1:** binnen bereik van analyse - alleen voor een bepaald model analyseren

De uitvoer is een matrix van labels, zoals in dit voorbeeld worden een voorbeeld:
```
  { 
    "result": [ 
      { 
    "name": "golden retriever", 
    "score": 0.98
    },
    { 
    "name": "Labrador retriever", 
    "score": 0.78
    }
              ]
  }
```

**Optie 2:** geavanceerde analyse - analyseren om aanvullende informatie verschaffen tot 86 categorieën taxonomie

Voor domeinspecifieke modellen met de optie twee (analyse van uitgebreide), retourneert de categorieën type wordt uitgebreid. Hier volgt een voorbeeld:
```
  {
    "requestId": "87e44580-925a-49c8-b661-d1c54d1b83b5",
    "metadata":     {
      "width": 640,
      "height": 430,
      "format": "Jpeg"
                    },
    "result": {
      "celebrities": 
      [
        {
        "name": "Richard Nixon",
        "faceRectangle": {
          "left": 107,
          "top": 98,
          "width": 165,
          "height": 165
                         },
        "confidence": 0.9999827
        }
      ]
  }
```

Het veld met categorieën wordt een lijst met een of meer van de [86 categorieën](../Category-Taxonomy.md) in de oorspronkelijke taxonomie. Houd er ook rekening mee dat u dat eindigt op een onderstrepingsteken categorieën overeenkomen met die categorie en de onderliggende objecten (bijvoorbeeld people_ evenals people_group voor beroemdheden model).

Veld   | Type  | Inhoud
------|------|------|
categorieën | object | Op het hoogste niveau object
categorieën [] .name    | tekenreeks   | Geef de naam van 86 categorietaxonomie
categorieën [] .score  | getal    | Betrouwbaarheidsscore tussen 0 en 1
categorieën [] .detail  | object?      | Optionele detail-object

Houd er rekening mee dat als meerdere categorieën overeenkomen (86 categorie classificatie retourneert bijvoorbeeld een score voor zowel people_ en people_young als model = beroemdheden), de details zijn gekoppeld aan de meest algemene niveau overeenkomst (people_ in dit voorbeeld).

### <a name="Errors">Antwoorden op fouten</a>
Dit zijn identiek aan vision.analyze, met de extra fout van NotSupportedModel fout (HTTP 400), die kan worden geretourneerd in een optie en optie twee scenario's. Voor optie twee (analyse van uitgebreide), als een van de modellen die zijn opgegeven in de gegevens niet worden herkend, de API retourneert een NotSupportedModel, zelfs als een of meer van deze geldig zijn.  Gebruikers kunnen aanroepen listModels om erachter te komen welke modellen worden ondersteund.

### <a name="Summary">Samenvatting</a>

Dit zijn de basic-functies van de Computer Vision-API: het uploaden van afbeeldingen en in ruil kostbare metagegevens ophalen.

Voor het gebruik van de REST-API, gaat u naar [Computer Vision-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
 

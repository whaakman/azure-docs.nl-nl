---
title: 'Voorbeeld: de Analyze image-API aanroepen - Computer Vision'
titlesuffix: Azure Cognitive Services
description: Lees hoe u de Computer Vision-API aanroept met behulp van REST in Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: sample
ms.date: 01/20/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 9520d4bcec0e170700aacc5ef4bc69100e333af1
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581705"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>Voorbeeld: de Computer Vision-API aanroepen

In deze handleiding ziet u hoe u de Computer Vision-API aanroept met behulp van REST. De voorbeelden zijn zowel in C# geschreven met behulp van de clientbibliotheek van de Computer Vision-API als in HTTP POST/GET-aanroepen. We richten ons op:

-   Instructies voor het ophalen van Tags, Beschrijving en Categorieën.
-   Domeinspecifieke informatie ophalen (beroemdheden).

### <a name="Prerequisites">Vereisten</a> 
Afbeeldings-URL of pad naar een lokaal opgeslagen afbeelding.
  * Ondersteunde invoermethoden: Onbewerkte binaire gegevens van een afbeelding in de vorm van een toepassings-/octetstream- of afbeeldings-URL.
  * Ondersteunde afbeeldingsindelingen: JPEG, PNG, GIF, BMP
  * Grootte van het afbeeldingsbestand: Minder dan 4 MB
  * Afbeeldingsgrootte: Groter dan 50 x 50 pixels
  
In de onderstaande voorbeelden worden de volgende functies getoond:

1. Het analyseren van een afbeelding en het ophalen van een matrix van tags en een geretourneerde beschrijving.
2. Het analyseren van een afbeelding met een domeinspecifiek model (met name het 'beroemdhedenmodel') en het verkrijgen van het bijbehorende resultaat in JSON retune.

Functies worden onderverdeeld op:

  * **Optie 1:** Gerichte analyse: alleen een gegeven model analyseren
  * **Optie 2:** Uitgebreide analyse: een analyse uitvoeren om meer informatie te geven met [86-categorietaxonomie](../Category-Taxonomy.md)
  
### <a name="Step1">Stap 1: De API-aanroep autoriseren</a> 
Voor elke aanroep naar de Computer Vision-API is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een tekenreeksparameter of zijn opgegeven in de aanvraagheader. 

Raadpleeg [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md
) als u een abonnementssleutel wilt verkrijgen.

**1.** De abonnementssleutel kan worden doorgegeven via een querytekenreeks. Hieronder ziet u een voorbeeld met de Computer Vision-API:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** Het doorgeven van de abonnementssleutel kan ook worden opgegeven in de HTTP-aanvraagheader:

```ocp-apim-subscription-key: <Your subscription key>```

**3.** Als u gebruikmaakt van de clientbibliotheek, wordt de abonnementssleutel doorgegeven via de constructor van VideoServiceClient:

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">Stap 2: Een afbeelding uploaden naar de Computer Vision-API-service en tags, beschrijvingen en beroemdheden ontvangen</a>
De eenvoudigste manier om de Computer Vision-API-aanroep uit te voeren is door rechtstreeks een afbeelding te uploaden. Dit wordt gedaan door de aanvraag POST te verzenden met het inhoudstype toepassing/octet-stream in combinatie met de gegevens die uit de afbeelding zijn gelezen. Voor Tags en Beschrijving is dit dezelfde uploadmethode als voor alle Computer Vision-API-aanroepen. Het enige verschil wordt bepaald door de queryparameters die de gebruiker opgeeft. 

Hier ziet u hoe u Tags en Beschrijving voor een gegeven afbeelding kunt ophalen:

**Optie 1:** Een lijst ophalen met Tags en een lijst met Beschrijving
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
**Optie 2:** Een lijst ophalen met alleen Tags, of een lijst met alleen Beschrijving:

###### <a name="tags-only"></a>Alleen Tags:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Alleen Beschrijving:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>Hier ziet u hoe u een domeinspecifieke analyse kunt ophalen (in ons geval voor beroemdheden).

**Optie 1:** Gerichte analyse: alleen een gegeven model analyseren
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
Voor deze optie zijn alle andere queryparameters {visualFeatures, details} niet geldig. Als u alle ondersteunde modellen wilt zien, gebruikt u: 
```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```
**Optie 2:** Uitgebreide analyse: een analyse uitvoeren om meer informatie te geven met [86-categorietaxonomie](../Category-Taxonomy.md)

Voor toepassingen waarbij u een generieke afbeeldingsanalyse wilt ophalen naast details uit één of meer domeinspecifieke modellen, breiden we de v1-API uit met de queryparameter voor modellen.
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```
Wanneer deze methode is ingetrokken, roepen we eerst de 86-categorieënclassificatie aan. Als een van de categorieën overeenkomt met de categorie van een bekend/overeenkomstig model, treedt een tweede ronde van classificatie-aanroepen op. Als bijvoorbeeld 'details=alle' of 'details' 'beroemdheden' bevat, roepen we het beroemdhedenmodel aan nadat de 86-categorieënclassificatie is aangeroepen en bevat het resultaat de persoon uit deze categorie. Dit verhoogt de latentie voor gebruikers die geïnteresseerd zijn in beroemdheden, ten opzichte van Optie 1.

Alle v1-queryparameters gedragen zich in dit geval op dezelfde manier.  Als visualFeatures=categorieën niet is opgegeven, wordt dit impliciet ingeschakeld.

### <a name="Step3">Stap 3: De JSON-uitvoer voor analyze&visualFeatures=Tags, Beschrijving ophalen en begrijpen</a>

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
Tags    | object    | Object op het hoogste niveau voor matrix van tags
tags[].Name | tekenreeks    | Trefwoord uit classificatie van tags
tags[].Score    | getal    | Betrouwbaarheidsscore, tussen 0 en 1.
description  | object   | Object op het hoogste niveau voor een beschrijving.
description.tags[] |    tekenreeks  | Een lijst met tags.  Als de mogelijkheid om een bijschrift te produceren niet betrouwbaar genoeg is, zijn de tags mogelijk de enige informatiebron die voor de aanroepende functie beschikbaar is.
description.captions[].text | tekenreeks    | Een zin die de afbeelding beschrijft.
description.captions[].confidence   | getal    | Vertrouwen voor de zin.

### <a name="Step4">Stap 4: De JSON-uitvoer van domeinspecifieke modellen ophalen en begrijpen</a>

**Optie 1:** Gerichte analyse: alleen een gegeven model analyseren

De uitvoer is een matrix van tags, zoals in dit voorbeeld:
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

**Optie 2:** Uitgebreide analyse: een analyse uitvoeren om meer informatie te geven met 86-categorietaxonomie

Voor domeinspecifieke modellen die Optie 2 (Uitgebreide analyse) gebruiken, wordt het retourtype voor categorieën uitgebreid. Neem het volgende voorbeeld:
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

Het categorieënveld is een lijst met één of meer van de [86-categorieën](../Category-Taxonomy.md) in de oorspronkelijke taxonomie. U ziet ook dat categorieën die op een onderstrepingsteken eindigen, overeenkomen met die categorie en de bijbehorende onderliggende elementen (bijvoorbeeld zowel personen_ als personen_groep, voor het beroemdhedenmodel).

Veld   | Type  | Inhoud
------|------|------|
categorieën | object | Object op het hoogste niveau
categories[].name    | tekenreeks   | Naam van 86-categorietaxonomie
categories[].score  | getal    | Betrouwbaarheidsscore, tussen 0 en 1
categories[].detail  | object?      | Optioneel detailobject

Let op: als meerdere categorieën overeenkomen (de 86-categorieënclassificatie retourneert bijvoorbeeld een score voor zowel personen_ als personen_jong wanneer model=beroemdheden), worden de details gekoppeld aan de overeenkomst op het meest algemene niveau (in dit voorbeeld personen_).

### <a name="Errors">Antwoorden op fouten</a>
Deze zijn hetzelfde als vision.analyze, met de aanvullende NotSupportedModel-fout (HTTP 400), die mogelijk wordt geretourneerd in scenario's voor zowel Optie 1 als Optie 2. Als voor Optie 2 (Uitgebreide analyse) eender welke van de modellen die bij de details zijn opgegeven niet worden herkend, retourneert de API een NotSupportedModel, zelfs als één of meer geldig zijn.  Gebruikers kunnen listModels oproepen om te achterhalen welke modellen worden ondersteund.

### <a name="Summary">Samenvatting</a>

Dit zijn de basisfunctionaliteiten van de Computer Vision-API: hoe u afbeeldingen kunt uploaden en daardoor waardevolle metagegevens kunt ophalen.

Als u de REST-API wilt gebruiken, gaat u naar [Computer Vision-API-referentie](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
 

---
title: Cognitive Services koppelen aan uw vaardigheden - Azure Search
description: Instructies voor een Cognitive Services All-in-One-abonnement koppelen aan een cognitieve vaardigheden
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 7f604d1b94e51db11e6d6992b7f070d64ae869dd
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317233"
---
# <a name="associate-cognitive-services-resource-with-a-skillset"></a>Cognitive Services-resource koppelen aan een set vaardigheden 

> [!NOTE]
> Vanaf December 21 mei 2018, kunt u zich een Cognitive Services-resource koppelen aan een Azure Search-vaardigheden. Hierdoor kunnen we beginnen kosten te bereken voor uitvoering van vaardigheden. Op deze datum ook in rekening voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken. Tekst extractie van documenten blijven worden aangeboden zonder extra kosten.
>
> De uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/) . Afbeelding extractie prijsstelling wordt in rekening gebracht op de preview-prijzen en wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


Cognitieve zoekopdrachten worden uitgepakt en verrijkt gegevens zodat deze kan worden doorzocht in Azure Search. We noemen extractie en verrijking *cognitieve vaardigheden*. De set vaardigheden aangeroepen tijdens het indexeren van inhoud zijn gedefinieerd in een *vaardigheden*. Een set vaardigheden kunt [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) of aangepaste vaardigheden (Zie [voorbeeld: Maak een aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md) voor meer informatie).

In dit artikel leert u hoe u koppelt de [Cognitive Services ](https://azure.microsoft.com/services/cognitive-services/) resource die u wilt uw cognitieve vaardigheden.

De Cognitive Services-resource die u selecteert, wordt de ingebouwde cognitieve vaardigheden aansturen. Deze bron wordt ook worden gebruikt voor factureringsdoeleinden. Alle enrichments die u uitvoert met behulp van de ingebouwde cognitieve vaardigheden wordt gefactureerd op basis van de Cognitive Services-resource die u selecteert. Deze wordt gefactureerd tegen hetzelfde tarief als u een Cognitive Services-resource met dezelfde taak heeft uitgevoerd. Zie [prijzen voor Cognitive Service](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Beperkingen bij het geen Cognitive Services-resource is geselecteerd
Vanaf 1 februari 2019, als u doet een Cognitive Services-abonnement niet koppelen aan uw vaardigheden uit, kunt u alleen zich op een klein aantal documenten voor gratis (20 documenten per dag) te verrijken. 

## <a name="associating-a-cognitive-services-resource-with-a-new-skillset"></a>Een Cognitive Services-resource koppelen aan een nieuwe set vaardigheden

1. Als onderdeel van de *gegevens importeren* ondervindt, nadat u verbinding hebt gemaakt met de gegevensbron die u u naar gaat, de *toevoegen cognitief zoeken* optionele stap. 

1. Vouw de *Cognitive Services koppelen* sectie. Hier ziet u alle Cognitive Service resources die u in dezelfde regio's als uw Search-Service hebt. 
![Uitgevouwen Cognitive Service koppelen](./media/cognitive-search-attach-cognitive-services/attach1.png "uitgevouwen koppelen van Cognitive Services")

1. Selecteer een bestaande resource voor Cognitive Services, of *Maak een nieuwe Cognitive Services-resource*. Als u selecteert de *gratis (beperkte Enrichments) resource*, kunt u zich alleen verrijken van een klein aantal documenten voor gratis (20 documenten per dag). Als u hebt geklikt op *Maak een nieuwe Cognitive Services-resource*, een nieuw tabblad dat wordt geopend kunt u om de Cognitive Services-resource te maken. 

1. Als u een nieuwe resource gemaakt, klikt u op *vernieuwen* vernieuwen van de lijst met Cognitive Services-resources en selecteer de resource. 
![Geselecteerde Resource voor Cognitive Service](./media/cognitive-search-attach-cognitive-services/attach2.png "Cognitive Serviceresource geselecteerd")

1. Als u dit hebt gedaan, kunt u uitbreiden de *Enrichments toevoegen* sectie om te selecteren van de specifieke cognitieve vaardigheden die u wilt uitvoeren op uw gegevens en gaat u verder met de rest van de stroom.

## <a name="associating-a-cognitive-services-resource-with-an-existing-skillset"></a>Een Cognitive Services-resource koppelen aan een bestaande vaardigheden

1. Selecteer op de pagina overzicht van de Service de *kennis en vaardigheden* tabblad. ![Kennis en vaardigheden tabblad](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "kennis en vaardigheden tabblad")

1. *Klik op* op de vaardigheden die u wilt wijzigen. Hiermee opent u een blade waarmee u kunt een set vaardigheden bewerken.

1. Selecteer een bestaande resource voor Cognitive Services of *Maak een nieuwe Cognitive Services-resource*. Als u selecteert de *gratis (beperkte Enrichments) resource*, kunt u zich alleen verrijken van een klein aantal documenten voor gratis (20 documenten per dag). Als u hebt geklikt op *Maak een nieuwe Cognitive Services-resource*, een nieuw tabblad dat wordt geopend kunt u om de Cognitive Services-resource te maken. <n/> 
<img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Als u een nieuwe resource gemaakt, klikt u op *vernieuwen* vernieuwen van de lijst met Cognitive Services-resources en selecteer de resource.
1. Klik op *OK* om uw wijzigingen te bevestigen

## <a name="associating-a-cognitive-services-resource-programmatically"></a>Een Cognitive Services-resource koppelen via een programma

Bij het definiëren van de vaardigheden via een programma, Voeg een `cognitiveServices` sectie. De sectie moet de sleutel van de Cognitive Services-resource die u wilt koppelen aan de vaardigheden bevatten, evenals de @odata.type, die moet worden ingesteld op '#Microsoft.Azure.Search.CognitiveServicesByKey'. Dit patroon wordt weergegeven in het onderstaande voorbeeld.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey"
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimating-the-cost-of-document-cracking-and-enrichment"></a>Voorbeeld: De kostenschatting van documenten kraken en verrijking
Het is raadzaam om in te schatten hoeveel kost te verrijken van een bepaald type document. De volgende oefening is slechts een voorbeeld, maar het kan handig zijn voor u zijn.

Stel dat we 1000 PDF-bestanden hebben en we verwachten dat op elk van deze documenten hebt gemiddelde 6-pagina's elk. Stel dat ze allemaal één afbeelding per pagina voor deze oefening. We gaan ook imagine die gemiddeld, zijn er ongeveer 3000 tekens per pagina. 

Nu gaan we ervan uit dat we willen om uit te voeren van de volgende stappen uit als onderdeel van de pijplijn verrijking:
1. Als onderdeel van documenten kraken, pak de inhoud en installatiekopieën uit het document.
1. Als onderdeel van verrijking OCR van de pagina's hebt uitgepakt, Combineer de tekst voor alle pagina's en pak vervolgens elk van de bedrijven in de gecombineerde tekst van alle installatiekopieën.

Laten we schat hoeveel zouden de kosten voor opname van deze documenten, stap voor stap.

Voor het 1000 documenten:

1. Documenten kraken, zouden we een gecombineerde aantal 6000 installatiekopieën extraheren. Ervan uitgaande dat $1 voor elke 1000 afbeeldingen uitgepakt, die ons $6,00 zou kosten.

2. We zouden de tekst extraheren uit elk van deze 6000 installatiekopieën. In het Engels gebruikt de OCR cognitieve vaardigheden het beste algoritme (DescribeText). Ervan uitgaande dat een prijs van $2,50 per 1000 afbeeldingen moeten worden geanalyseerd, zouden we $15,00 betalen voor deze stap.

3. Voor het ophalen van de entiteit hebben we een totaal van 3 tekstrecords per pagina (elke record is 1000 tekens). 3 tekst-records/pagina * 6000 pagina's = 18.000, tekstrecords. Ervan uitgaande dat $2,00 / 1000 tekstrecords, deze stap zou kosten us $36,00.

Dit alles, zouden we betaalt 57.00 $ 1000 pdf-documenten met dit aard opnemen met de vaardigheden die worden beschreven.  In deze oefening wordt aangenomen dat de duurste prijs per transactie, het lagere vanwege opwaarderen prijzen kan zijn. Zie [prijzen voor Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Volgende stappen
+ [Pagina in Azure Search-prijzen](https://azure.microsoft.com/pricing/details/search/)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)

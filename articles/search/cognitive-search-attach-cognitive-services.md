---
title: Een Cognitive Services-resource koppelen aan een set vaardigheden in Azure Search
description: Instructies voor een Cognitive Services All-in-One-abonnement koppelen aan een cognitieve vaardigheden in Azure Search
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 52efa685bba330879365f56e547881d62a52a185
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000152"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Een Cognitive Services-resource koppelen aan een set vaardigheden in Azure Search 

Cognitieve zoekopdrachten worden uitgepakt en verrijkt gegevens zodat deze kan worden doorzocht in Azure Search. We noemen extractie en verrijking *cognitieve vaardigheden*. De set vaardigheden aangeroepen tijdens het indexeren van inhoud zijn gedefinieerd in een *vaardigheden*. Een set vaardigheden kunt [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) of aangepaste vaardigheden. Zie voor meer informatie, [voorbeeld: Maak een aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md).

In dit artikel leert u hoe u koppelt een [Azure Cognitive Services ](https://azure.microsoft.com/services/cognitive-services/) resource met de cognitieve vaardigheden.

De Cognitive Services-resource die u selecteert, wordt de ingebouwde cognitieve vaardigheden aansturen. Deze bron wordt ook worden gebruikt voor factureringsdoeleinden. Alle enrichments die u uitvoert met behulp van de ingebouwde cognitieve vaardigheden wordt gefactureerd op basis van de Cognitive Services-resource die u selecteert. Deze wordt gefactureerd tegen hetzelfde tarief als u dezelfde taak heeft uitgevoerd met behulp van een resource voor Cognitive Services. Zie [prijzen voor Cognitive Service](https://azure.microsoft.com/pricing/details/cognitive-services/).

> [!NOTE]
> U kunt vanaf 21 December 2018, een Cognitive Services-resource koppelen aan een Azure Search-vaardigheden. Dit kan we kosten in rekening gebracht voor de uitvoering van vaardigheden. Op deze datum begon wordt ook kosten in rekening gebracht voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken. Tekst extractie van documenten nog steeds worden aangeboden zonder extra kosten.
>
> De uitvoering van de ingebouwde vaardigheden in rekening gebracht tegen de [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijsstelling wordt in rekening gebracht op de preview-prijzen en wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Beperkingen bij het geen Cognitive Services-resource is geselecteerd
Vanaf 1 februari 2019, als u niet een Cognitive Services-abonnement koppelen aan uw vaardigheden uit, kunt u zich aan het verrijken van slechts een klein aantal documenten voor gratis (20 documenten per dag). 

## <a name="associate-a-cognitive-services-resource-with-a-new-skillset"></a>Een Cognitive Services-resource koppelen aan een nieuwe set vaardigheden

1. Als onderdeel van de [gegevens importeren](search-import-data-portal.md) wizard, nadat u verbinding maken met uw gegevensbron, gaat u naar de **toevoegen cognitief zoeken** optionele stap. Dit is de tweede stap in de wizard.

1. Vouw de **Cognitive Services koppelen** sectie. Deze stap ziet u alle Cognitive Services-resources die in dezelfde regio's als de Azure Search-service hebt.

   ![Cognitive Services koppelen sectie uitgevouwen](./media/cognitive-search-attach-cognitive-services/attach1.png "uitgevouwen koppelen Cognitive Services")

1. Selecteer een bestaande resource voor Cognitive Services, of selecteer **nieuwe Cognitive Services-resource maken**. Als u selecteert **gratis (beperkte Enrichments)**, kunt u zich aan het verrijken van slechts een klein aantal documenten voor gratis (20 documenten per dag). Als u selecteert **nieuwe Cognitive Services-resource maken**, een nieuw tabblad geopend waarin u de resource kunt maken. 

1. Als u een nieuwe resource gemaakt, selecteert u **vernieuwen** Vernieuw de lijst met Cognitive Services-resources, en selecteer vervolgens de resource. 

   ![Cognitive Services-resource hebt geselecteerd](./media/cognitive-search-attach-cognitive-services/attach2.png "Cognitive Service Resource geselecteerd")

1. Vouw de **Enrichments toevoegen** sectie om te selecteren van de specifieke cognitieve vaardigheden die u wilt uitvoeren op uw gegevens en gaat u verder met de rest van de stroom.

## <a name="associate-a-cognitive-services-resource-with-an-existing-skillset"></a>Een Cognitive Services-resource koppelen aan een bestaande vaardigheden

1. Op de **Serviceoverzicht** weergeeft, schakelt de **kennis en vaardigheden** tabblad.

   ![Kennis en vaardigheden tabblad](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "kennis en vaardigheden tabblad")

1. Selecteer de vaardigheden die u wilt wijzigen. Deze stap wordt een blade waarin u een set vaardigheden kunt bewerken geopend.

1. Selecteer een bestaande resource voor Cognitive Services, of selecteer **nieuwe Cognitive Services-resource maken**. Als u selecteert **gratis (beperkte Enrichments)**, kunt u zich aan het verrijken van slechts een klein aantal documenten voor gratis (20 documenten per dag). Als u selecteert **nieuwe Cognitive Services-resource maken**, een nieuw tabblad geopend waarin u de resource kunt maken.

   <n/> 
   <img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Als u een nieuwe resource gemaakt, selecteert u **vernieuwen** Vernieuw de lijst met Cognitive Services-resources, en selecteer vervolgens de resource.

1. Selecteer **OK** om uw wijzigingen te bevestigen.

## <a name="associate-a-cognitive-services-resource-programmatically"></a>Een Cognitive Services-resource via een programma koppelen

Tijdens het definiëren van de vaardigheden via een programma, Voeg een `cognitiveServices` sectie. In de sectie de sleutel van de Cognitive Services-resource die u wilt koppelen aan de vaardigheden te bevatten. Ook `@odata.type`, en stel deze in op `#Microsoft.Azure.Search.CognitiveServicesByKey`. Het volgende voorbeeld ziet dit patroon.

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
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimate-the-cost-of-document-cracking-and-enrichment"></a>Voorbeeld: Maak een schatting van de kosten van documenten kraken en verrijking
Het is raadzaam om in te schatten hoeveel kost te verrijken van een type document. De volgende oefening is slechts een voorbeeld, maar het kan handig zijn voor u zijn.

Stel dat u 1000 PDF-bestanden hebt. U schat dat gemiddeld, elk van deze documenten 6-pagina's een. Elke pagina heeft 1 afbeelding. Er zijn gemiddeld ongeveer 3000 tekens per pagina. 

Nu wordt ervan uitgegaan dat u wilt de volgende stappen uitvoeren als onderdeel van de pijplijn verrijking:
1. Als onderdeel van documenten kraken, pak de inhoud en installatiekopieën uit het document.
1. Als onderdeel van verrijking, optische tekenherkenning (OCR) gebruiken voor elk van de opgehaalde pagina's, Combineer de tekst voor alle pagina's en pak vervolgens elk van de bedrijven in de gecombineerde tekst van alle installatiekopieën.

Laten we schat hoeveel zouden de kosten voor opname van die 1000 documenten, stap voor stap:

1. Voor documenten kraken, zou u een gecombineerde aantal 6000 installatiekopieën uitpakken. Ervan uitgaande dat $1 voor elke 1000 afbeeldingen uitgepakt, die u $6,00 zou kosten.

2. U zou de tekst extraheren uit elk van deze 6000 installatiekopieën. In het Engels gebruikt de OCR cognitieve vaardigheden het beste algoritme (DescribeText). Ervan uitgaande dat een prijs van $2,50 per 1000 afbeeldingen moeten worden geanalyseerd, zou u $15,00 betalen voor deze stap.

3. Voor entiteiten extraheren moet u een totaal van 3 tekstrecords per pagina. (Elke record is 1000 tekens). Drie tekstrecords per pagina * 6000 pagina's = 18.000, tekstrecords. $2,00 per 1000 tekstrecords, ervan uitgaande dat deze stap $36,00 zou kosten.

Dit alles, betaalt u $57.00 voor opname van 1000 PDF-documenten van deze aard met de vaardigheden beschreven. In deze oefening wordt ervan uitgegaan dat de duurste prijs per transactie. Het kan zijn lager vanwege opwaarderen prijzen. Zie [prijzen voor Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Volgende stappen
+ [Azure Search pagina met prijzen](https://azure.microsoft.com/pricing/details/search/)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)

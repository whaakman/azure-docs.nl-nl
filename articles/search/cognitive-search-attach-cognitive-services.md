---
title: Een Cognitive Services-resource koppelen aan een set vaardigheden - Azure Search
description: Instructies voor het koppelen van een Cognitive Services All-in-One-abonnement voor een pijplijn cognitieve verrijking in Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 01/07/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 315e050f9ff8768cfeb8ff5417834e8e163ce334
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231424"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Een Cognitive Services-resource koppelen aan een set vaardigheden in Azure Search 

De AI-algoritmen te stimuleren [cognitief zoeken pijplijnen](cognitive-search-concept-intro.md) voor de verwerking van niet-gestructureerde gegevens die zijn gebaseerd op [ **Cognitive Services-resources**](https://azure.microsoft.com/services/cognitive-services/). Resources, zoals [ **Computer Vision** ](https://azure.microsoft.com/services/cognitive-services/computer-vision/) analyse van de afbeelding en optische tekenherkenning (OCR) opgeven voor het extraheren van tekst en de structuur uit afbeeldingsbestanden, terwijl [ **tekst Analytics** ](https://azure.microsoft.com/services/cognitive-services/text-analytics/) biedt, zoals entiteit erkenning en de sleutel vindt er sleuteltermextractie plaats, een paar te noemen verwerking van natuurlijke taal.

U kunt een beperkt aantal documenten gratis verrijken of een factureerbare Cognitive Services-resource voor workloads met grotere en meer frequente koppelen. In dit artikel leert u hoe u een Cognitive Services-resource koppelen aan uw cognitieve vaardigheden aan gegevens duidelijker maken tijdens het indexeren.

Als uw pijplijn bestaat uitsluitend uit [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md), u hoeft niet te koppelen van een resource voor Cognitive Services.

> [!NOTE]
> U kunt vanaf 21 December 2018, een Cognitive Services-resource koppelen aan een Azure Search-vaardigheden. Dit kan we kosten in rekening gebracht voor de uitvoering van vaardigheden. Op deze datum begon wordt ook kosten in rekening gebracht voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken. Tekst extractie van documenten blijft zonder extra kosten worden aangeboden.
>
> De uitvoering van [ingebouwde cognitieve vaardigheden](cognitive-search-predefined-skills.md) wordt in rekening gebracht tegen de [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/), op dezelfde prijs als u de taak onafhankelijk van Azure Search heeft uitgevoerd. Afbeelding extractie prijsstelling wordt in rekening gebracht op de preview-prijzen en wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="use-free-resources"></a>Gratis resources gebruiken

U kunt een beperkt, gratis verwerkingsoptie waarmee u hebt op 20 document enrichments dagelijks, voldoende is recht voor het voltooien van de zelfstudie en de Snelstartgids oefeningen voor cognitief zoeken. 

> [!Important]
> Vanaf 1 februari 2019, de **gratis (beperkte Enrichments)** is beperkt tot 20 documenten per dag. 

1. Open de **gegevens importeren** wizard.

   ![Gegevensopdracht importeren](media/search-get-started-portal/import-data-cmd2.png "gegevensopdracht importeren")

1. Kies een gegevensbron en doorgaan met het **cognitief zoeken toevoegen (optioneel)**. Zie voor een stapsgewijze uitleg van deze wizard [importeren, index en query's met behulp van portal-hulpprogramma's](search-get-started-portal.md).

1. Vouw **Cognitive Services koppelen** en selecteer **gratis (beperkte Enrichments)**.

   ![Cognitive Services koppelen sectie uitgevouwen](./media/cognitive-search-attach-cognitive-services/attach1.png "uitgevouwen koppelen Cognitive Services")

Doorgaan naar de volgende stap **enrichments toevoegen**. Zie voor een beschrijving van de vaardigheden die beschikbaar zijn in de portal, [' stap 2: Cognitieve vaardigheden toevoegen'](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in de Snelstartgids cognitief zoeken.

## <a name="use-billable-resources"></a>Factureerbare resources gebruiken

Voor workloads die meer dan 20 documenten dagelijks nummering, moet u een factureerbare Cognitive Services-resource.

1. In de **gegevens importeren** wizard in **Cognitive Services koppelen**, selecteert u een bestaande resource of klik op **nieuwe Cognitive Services-resource maken**.

1. Voor **nieuwe Cognitive Services-resource maken**, er wordt een nieuw tabblad geopend zodat u van de resource maken kunt. De resource een unieke naam geven.

1. Kies dezelfde locatie als de Azure Search. Cognitieve vaardigheden indexeren wordt momenteel ondersteund in deze regio's:

  * US - west-centraal
  * US - zuid-centraal
  * US - oost
  * US - oost 2
  * US - west 2
  * Canada - midden
  * Europa -west
  * Verenigd Koninkrijk Zuid
  * Europa - noord
  * Brazilië - zuid
  * Azië - zuidoost
  * India - centraal
  * Australië - oost

1. Kies de prijscategorie All-in-one, **S0**. Deze laag biedt de visie en taal-functies die de vooraf gedefinieerde vaardigheden in cognitief zoeken back-ups maken.

    ![Maak een nieuwe Cognitive Services-resource](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Maak een nieuwe Cognitive Services-resource")

1. Klik op **maken** voor het inrichten van de nieuwe Cognitive Services-resource. 

1. Ga terug naar de vorige tabblad met de **gegevens importeren** wizard. Klik op **vernieuwen** voor het weergeven van de Cognitive Services-resource, en selecteer vervolgens de resource.

   ![Cognitive Services-resource hebt geselecteerd](./media/cognitive-search-attach-cognitive-services/attach2.png "Cognitive Service Resource geselecteerd")

1. Vouw de **Enrichments toevoegen** sectie om te selecteren van de specifieke cognitieve vaardigheden die u wilt uitvoeren op uw gegevens en gaat u verder met de rest van de stroom. Zie voor een beschrijving van de vaardigheden die beschikbaar zijn in de portal, [' stap 2: Cognitieve vaardigheden toevoegen'](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in de Snelstartgids cognitief zoeken.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Een bestaande vaardigheden koppelen aan een resource voor Cognitive Services

Als u een bestaande vaardigheden hebt, kunt u deze koppelen aan een nieuwe of andere Cognitive Services-resource.

1. Op de **Serviceoverzicht** pagina, klikt u op **kennis en vaardigheden**.

   ![Kennis en vaardigheden tabblad](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "kennis en vaardigheden tabblad")

1. Klik op de naam van de vaardigheden, en selecteer een bestaande resource of een nieuwe maken. Klik op **OK** om uw wijzigingen te bevestigen. 

   ![Lijst met resources vaardigheden](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "resourcelijst vaardigheden")

Intrekken die **gratis (beperkte Enrichments)** is beperkt tot 20 documenten dagelijks, en dat **nieuwe Cognitive Services-resource maken** wordt gebruikt voor het inrichten van een nieuwe factureerbare bron. Als u een nieuwe resource maakt, selecteert u **vernieuwen** Vernieuw de lijst met Cognitive Services-resources, en selecteer vervolgens de resource.

## <a name="attach-cognitive-services-programmatically"></a>Cognitive Services via een programma koppelen

Tijdens het definiëren van de vaardigheden via een programma, Voeg een `cognitiveServices` sectie om de vaardigheden. In de sectie de sleutel van de Cognitive Services-resource die u wilt koppelen aan de vaardigheden te bevatten. Let erop dat de resource moet zich in dezelfde regio als de Azure Search. Ook `@odata.type`, en stel deze in op `#Microsoft.Azure.Search.CognitiveServicesByKey`. 

Het volgende voorbeeld ziet dit patroon. U ziet de sectie cognitiveServices aan de onderkant van de definitie van de

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
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Voorbeeld: Kosten schatten

Om een schatting kosten in verband met cognitief zoeken, indexeren, te beginnen met een idee van wat een gemiddelde document ziet eruit als zodat u sommige getallen kunt uitvoeren. Bijvoorbeeld, voor schattingsdoeleinden gebruiken, u mogelijk bij benadering:

+ 1000 PDF-bestanden
+ Elke zes pagina 's
+ Één afbeelding per pagina (6000 installatiekopieën)
+ 3000 tekens per pagina

Wordt ervan uitgegaan dat u een pijplijn die bestaat uit een document kraken van elke PDF-document met afbeeldingen en tekst extractie, optische tekenherkenning (OCR) van installatiekopieën, en de naam van entiteit erkenning van organisaties. 

In deze oefening gebruiken we de duurste prijs per transactie. Werkelijke kosten kunnen worden lagere vanwege gestaffelde prijzen. Zie [prijzen voor Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Tekst extractie is voor een document met tekst en afbeeldingen inhoud kraken, die momenteel beschikbaar is. 6000 afbeeldingen, wordt ervan uitgegaan $1 voor elke 1000 afbeeldingen uitgepakt, kosten $6,00 voor deze stap.

2. De OCR cognitieve vaardigheden gebruikt voor OCR van 6000 afbeeldingen in het Engels, het beste algoritme (DescribeText). Ervan uitgaande dat een prijs van $2,50 per 1000 afbeeldingen moeten worden geanalyseerd, zou u $15,00 betalen voor deze stap.

3. Voor entiteiten extraheren moet u een totaal van 3 tekstrecords per pagina. Elke record is 1000 tekens. Drie tekstrecords per pagina * 6000 pagina's = 18.000, tekstrecords. $2,00 per 1000 tekstrecords, ervan uitgaande dat deze stap $36,00 zou kosten.

Dit alles, betaalt u ongeveer $57.00 voor opname van 1000 PDF-documenten van deze aard met de vaardigheden beschreven. 

## <a name="next-steps"></a>Volgende stappen
+ [Azure Search pagina met prijzen](https://azure.microsoft.com/pricing/details/search/)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)

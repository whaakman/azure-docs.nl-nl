---
title: Een Cognitive Services-resource met een set vaardigheden - Azure Search koppelen
description: Instructies voor het koppelen van een Cognitive Services All-in-One-abonnement voor een pijplijn cognitieve verrijking in Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 09695f764ff71b274e125e90835f5314eb25c980
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683967"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Een Cognitive Services-resource met een set vaardigheden in Azure Search koppelen 

AI-algoritmen station de [cognitieve indexering pijplijnen](cognitive-search-concept-intro.md) gebruikt voor het verwerken van niet-gestructureerde gegevens in Azure Search. Deze algoritmen zijn gebaseerd op [Cognitive Services-resources](https://azure.microsoft.com/services/cognitive-services/), waaronder [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/) voor analyse van de afbeelding en optische tekenherkenning (OCR) en [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/)voor herkenning entiteit, sleuteltermextractie en andere enrichments.

U kunt een beperkt aantal documenten gratis verrijken, of een factureerbare Cognitive Services-resource bijvoegen voor grotere en frequentere workloads. In dit artikel leert u hoe u een Cognitive Services-resource koppelen aan uw cognitieve vaardigheden uit om gegevens tijdens waardevoller [Azure Search indexeren](search-what-is-an-index.md).

Als uw pijplijn uit de vaardigheden die geen verband houdt met Cognitive Services-API's bestaat, moet u nog steeds een Cognitive Services-resource koppelen. Doet dit het geval is onderdrukkingen de **gratis** resource waarmee u een kleine hoeveelheid enrichments per dag wordt beperkt. Er zijn geen kosten voor de vaardigheden die niet zijn gebonden aan Cognitive Services API's. Deze vaardigheden opnemen: [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md), [tekst samenvoegen](cognitive-search-skill-textmerger.md), [tekst splitsen](cognitive-search-skill-textsplit.md), en [shaper](cognitive-search-skill-shaper.md).

> [!NOTE]
> Als u een bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of meer AI-algoritmen toe te voegen, moet u een factureerbare Cognitive Services-resource koppelen. Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van [ingebouwde cognitieve vaardigheden](cognitive-search-predefined-skills.md) uitvoering in rekening gebracht tegen de [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services), op hetzelfde tarief als u de taak rechtstreeks heeft uitgevoerd. Uitpakken van de installatiekopie is een gratis Azure Search, weergegeven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="use-free-resources"></a>Gratis resources gebruiken

Een beperkte, gratis verwerkingsoptie kunt u de cognitief zoeken zelfstudie en de Snelstartgids oefeningen. 

**Gratis (beperkte Enrichments)** zijn beperkt tot 20 documenten per dag, per abonnement.

1. Open de **gegevens importeren** wizard.

   ![Gegevensopdracht importeren](media/search-get-started-portal/import-data-cmd2.png "gegevensopdracht importeren")

1. Kies een gegevensbron en doorgaan met het **cognitief zoeken toevoegen (optioneel)**. Zie voor een stapsgewijze uitleg van deze wizard [importeren, index en query's met behulp van portal-hulpprogramma's](search-get-started-portal.md).

1. Vouw **Cognitive Services koppelen** en selecteer **gratis (beperkte Enrichments)**.

   ![Cognitive Services koppelen sectie uitgevouwen](./media/cognitive-search-attach-cognitive-services/attach1.png "uitgevouwen koppelen Cognitive Services")

Doorgaan naar de volgende stap **enrichments toevoegen**. Zie voor een beschrijving van de vaardigheden die beschikbaar zijn in de portal, [' stap 2: Cognitieve vaardigheden toevoegen'](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in de Snelstartgids cognitief zoeken.

## <a name="use-billable-resources"></a>Factureerbare resources gebruiken

Voor workloads die meer dan 20 enrichments dagelijks nummering, moet u een factureerbare Cognitive Services-resource koppelen. 

U betaalt alleen voor de vaardigheden die de Cognitive Services-API's aanroepen. Niet-API's gebaseerd vaardigheden, zoals [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md), [tekst samenvoegen](cognitive-search-skill-textmerger.md), [tekst splitsen](cognitive-search-skill-textsplit.md), en [shaper](cognitive-search-skill-shaper.md) vaardigheden zijn niet in rekening gebracht.

1. Open de **gegevens importeren** wizard, kiest u een gegevensbron en blijven **cognitief zoeken toevoegen (optioneel)**. 

1. Vouw **Cognitive Services koppelen** en selecteer vervolgens **nieuwe Cognitive Services-resource maken**. Een nieuw tabblad geopend zodat u van de resource maken kunt. 

   ![Een Cognitive Services-resource maken](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "een Cognitive Services-resource maken")

1. Kies in de locatie, dezelfde regio als de Azure Search uitgaande bandbreedte om kosten te vermijden tussen regio's.

1. Kies in de prijscategorie, **S0** om op te halen, de alles-in-één-verzameling van Cognitive Services-functies, met inbegrip van de visie en taal-functies die de vooraf gedefinieerde vaardigheden die worden gebruikt door Azure Search back-ups maken. 

   Voor de S0-laag, kunt u tarieven vinden voor specifieke werkbelastingen op de [pagina prijzen van Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + In **Selecteer bieden**, zorg ervoor dat *Cognitive Services* is geselecteerd.
   + Onder functies van de taal, de tarieven voor *Text Analytics Standard* zijn van toepassing op het indexeren van AI. 
   + Onder Vision-functies, de tarieven voor *Computer Vision S1* worden toegepast.

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

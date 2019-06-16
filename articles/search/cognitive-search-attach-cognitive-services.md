---
title: Een Cognitive Services-resource met een set vaardigheden - Azure Search koppelen
description: Instructies voor het koppelen van een Cognitive Services All-in-one-abonnement voor een pijplijn cognitieve verrijking in Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 44f16b3334b991e071fa85ca4cffbc0837f0a6ec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244435"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Een Cognitive Services-resource met een set vaardigheden in Azure Search koppelen 

AI-algoritmen station de [cognitieve indexering pijplijnen](cognitive-search-concept-intro.md) voor document-verrijking in Azure Search worden gebruikt. Deze algoritmen zijn gebaseerd op Azure Cognitive Services-bronnen, met inbegrip van [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/) voor analyse van de afbeelding en optische tekenherkenning (OCR) en [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) voor herkenning van entiteit Extractie van cruciale frasen, en andere enrichments. Gebruikt door Azure Search voor document verrijking doeleinden, de algoritmen zijn verpakt in een *vaardigheid*, geplaatst in een *vaardigheden*, en waarnaar wordt verwezen door een *indexeerfunctie* tijdens indexeren.

U kunt een beperkt aantal documenten gratis verrijken. Of u kunt een factureerbare Cognitive Services-resource te koppelen een *vaardigheden* voor workloads met grotere en meer frequente. In dit artikel leert u hoe u een factureerbare Cognitive Services-resource te verrijken van documenten in Azure Search koppelen [indexeren](search-what-is-an-index.md).

> [!NOTE]
> Aanroepen naar Cognitive Services API's en afbeeldingsbestanden extractie als onderdeel van de fase documenten kraken zijn factureerbare gebeurtenissen in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten of voor de vaardigheden die Cognitive Services niet aanroepen.
>
> Uitvoering van factureerbare vaardigheden plaats op de [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Zie voor de installatiekopie extractie prijzen, de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="same-region-requirement"></a>Vereiste in dezelfde regio

We vereist dat Azure Search en Azure Cognitive Services binnen dezelfde regio. Anders, krijgt u dit bericht tijdens runtime: `"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

Er is geen manier om te verplaatsen van een service in regio's. Als u deze fout optreedt, moet u een nieuwe Cognitive Services-resource maken in dezelfde regio als de Azure Search.

## <a name="use-free-resources"></a>Gratis resources gebruiken

Een beperkte, gratis verwerkingsoptie kunt u de cognitief zoeken zelfstudie en de Snelstartgids oefeningen.

Gratis (beperkte enrichments) resources zijn beperkt tot 20 documenten per dag, per abonnement.

1. Open de wizard gegevens importeren:

   ![Open de wizard gegevens importeren](media/search-get-started-portal/import-data-cmd2.png "opent u de wizard gegevens importeren")

1. Kies een gegevensbron en doorgaan met het **toevoegen cognitief zoeken (optioneel)** . Zie voor een stapsgewijze uitleg van deze wizard [importeren, index en query's met behulp van portal-hulpprogramma's](search-get-started-portal.md).

1. Vouw **Cognitive Services koppelen** en selecteer vervolgens **gratis (beperkte enrichments)** :

   ![Cognitive Services koppelen sectie uitgevouwen](./media/cognitive-search-attach-cognitive-services/attach1.png "sectie uitgevouwen koppelen Cognitive Services")

1. Doorgaan naar de volgende stap **toevoegen Enrichments**. Zie voor een beschrijving van de vaardigheden die beschikbaar zijn in de portal, [stap 2: Cognitieve vaardigheden toevoegen](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in de Snelstartgids cognitief zoeken.

## <a name="use-billable-resources"></a>Factureerbare resources gebruiken

Workloads die meer dan 20 enrichments per dag maakt, zorg er voor een factureerbare Cognitive Services-resource koppelen. We raden aan dat u altijd een factureerbare Cognitive Services-resource koppelen, zelfs als u deze niet nodig om aan te roepen van Cognitive Services API's. Bezig met koppelen van een resource, overschrijft de dagelijkse limiet.

U betaalt alleen voor de vaardigheden die de Cognitive Services-API's aanroepen. U bent niet in rekening gebracht [aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md), of vaardigheden, zoals [tekst samenvoegen](cognitive-search-skill-textmerger.md), [tekst splitsen](cognitive-search-skill-textsplit.md), en [shaper](cognitive-search-skill-shaper.md), die niet zijn API's gebaseerd.

1. Open de wizard gegevens importeren, kiest u een gegevensbron en blijven **toevoegen cognitief zoeken (optioneel)** .

1. Vouw **Cognitive Services koppelen** en selecteer vervolgens **nieuwe Cognitive Services-resource maken**. Een nieuw tabblad geopend zodat u van de resource maken kunt:

   ![Een Cognitive Services-resource maken](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "een Cognitive Services-resource maken")

1. In de **locatie** , selecteert u de regio waar uw Azure Search-service zich bevindt. Zorg ervoor dat u deze regio voor betere prestaties. Met behulp van deze regio, ongeldig het uitgaande bandbreedte kosten maken tussen regio's.

1. In de **prijscategorie** in de lijst met **S0** om op te halen, de alles-in-één-verzameling van Cognitive Services-functies, met inbegrip van de visie en taal-functies die de vooraf gedefinieerde vaardigheden die worden gebruikt door Azure Search back-ups maken.

   Voor de S0-laag, kunt u tarieven vinden voor specifieke werkbelastingen op de [pagina prijzen van Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + In de **Selecteer bieden** lijst, controleert u of **Cognitive Services** is geselecteerd.
   + Onder **taal** functies, de tarieven voor **Text Analytics Standard** zijn van toepassing op het indexeren van AI.
   + Onder **Vision** functies, de tarieven voor **Computer Vision S1** toepassen.

1. Selecteer **maken** voor het inrichten van de nieuwe Cognitive Services-resource.

1. Ga terug naar het vorige tabblad, waarin de wizard gegevens importeren. Selecteer **vernieuwen** voor het weergeven van de Cognitive Services-resource, en selecteer vervolgens de resource:

   ![Selecteer de Cognitive Services-resource](./media/cognitive-search-attach-cognitive-services/attach2.png "de Cognitive Services-resource selecteren")

1. Vouw de **Enrichments toevoegen** sectie om te selecteren van de specifieke cognitieve vaardigheden die u wilt uitvoeren op uw gegevens. Voltooi de rest van de wizard. Zie voor een beschrijving van de vaardigheden die beschikbaar zijn in de portal, [stap 2: Cognitieve vaardigheden toevoegen](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) in de Snelstartgids cognitief zoeken.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Een bestaande vaardigheden koppelen aan een resource voor Cognitive Services

Als u een bestaande vaardigheden hebt, kunt u deze koppelen aan een nieuwe of andere Cognitive Services-resource.

1. Op de **Serviceoverzicht** weergeeft, schakelt **kennis en vaardigheden**:

   ![Kennis en vaardigheden tabblad](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "kennis en vaardigheden tabblad")

1. Selecteer de naam van de vaardigheden en selecteer een bestaande resource of een nieuwe maken. Selecteer **OK** om uw wijzigingen te bevestigen.

   ![Lijst met resources vaardigheden](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "resourcelijst vaardigheden")

   Houd er rekening mee dat de **gratis (beperkte enrichments)** optie beperkt tot 20 documenten dagelijks en kunt u **nieuwe Cognitive Services-resource maken** voor het inrichten van een nieuwe factureerbare bron. Als u een nieuwe resource maakt, selecteert u **vernieuwen** Vernieuw de lijst met Cognitive Services-resources, en selecteer vervolgens de resource.

## <a name="attach-cognitive-services-programmatically"></a>Cognitive Services via een programma koppelen

Tijdens het definiëren van de vaardigheden via een programma, Voeg een `cognitiveServices` sectie om de vaardigheden. In deze sectie bevatten de sleutel van de Cognitive Services-resource die u wilt koppelen aan de vaardigheden. Houd er rekening mee dat de resource moet zich in dezelfde regio als uw Azure Search-resource. Ook `@odata.type`, en stel deze in op `#Microsoft.Azure.Search.CognitiveServicesByKey`.

Het volgende voorbeeld ziet dit patroon. U ziet dat de `cognitiveServices` aan het eind van de definitie van de sectie.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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

Om een schatting van de kosten voor cognitieve zoekopdrachten indexeren, te beginnen met een beter beeld van hoe een gemiddelde document eruitziet, zodat u sommige getallen kunt uitvoeren. Bijvoorbeeld, u mogelijk geschatte:

+ 1000 PDF-bestanden.
+ Zes elk pagina's.
+ Één afbeelding per pagina (6000 afbeeldingen).
+ 3\.000 tekens per pagina.

Wordt ervan uitgegaan dat u een pijplijn die uit documenten kraken van elke PDF, afbeeldingen en tekst extractie, optische tekenherkenning (OCR) van afbeeldingen, en de entiteit erkenning van organisaties bestaat.

De prijzen in dit artikel vindt u hypothetische. Ze worden gebruikt ter illustratie van het proces voor schatting. Uw kosten kunnen lager zijn. Zie voor de werkelijke prijzen van transacties, [prijzen voor Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Tekst extractie is voor een document met tekst en afbeeldingen inhoud kraken, die momenteel beschikbaar is. 6000 afbeeldingen, wordt ervan uitgegaan $1 voor elke 1000 afbeeldingen hebt uitgepakt. Dat is een prijs van $6,00 voor deze stap.

2. De OCR cognitieve vaardigheden gebruikt voor OCR van 6000 afbeeldingen in het Engels, het beste algoritme (DescribeText). Ervan uitgaande dat een prijs van $2,50 per 1000 afbeeldingen moeten worden geanalyseerd, zou u $15,00 betalen voor deze stap.

3. Voor entiteiten extraheren moet u een totaal van drie tekstrecords per pagina. Elke record is 1000 tekens. Drie tekstrecords per pagina vermenigvuldigd met 6000 pagina's is gelijk aan 18.000, tekstrecords. $2,00 per 1000 tekstrecords, ervan uitgaande dat deze stap $36,00 zou kosten.

Dit alles, betaalt u ongeveer $57.00 voor opname van 1000 PDF-documenten van dit type met de vaardigheden beschreven.

## <a name="next-steps"></a>Volgende stappen
+ [Azure Search pagina met prijzen](https://azure.microsoft.com/pricing/details/search/)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)

---
title: Een vaardigheden maken in een pijplijn cognitieve zoeken (Azure Search) | Microsoft Docs
description: Gegevens extraheren natuurlijke taal verwerking, definiëren of afbeelding analysis stappen voor het aanvullen en gestructureerde gegevens ophalen uit uw gegevens voor gebruik in Azure Search.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: luisca
ms.openlocfilehash: 997b106f748a2f18e8141f77f3b9ff8bb6b9d971
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268232"
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Het maken van een vaardigheden in een pijplijn verrijking

Cognitieve zoeken extraheert en verrijkt de gegevens zodat deze kan worden doorzocht in Azure Search. We noemen extractie en verrijking *cognitieve vaardigheden*gecombineerde in een *vaardigheden* waarnaar wordt verwezen tijdens het indexeren. Een vaardigheden kunt [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) of aangepaste vaardigheden (Zie [voorbeeld: Maak een aangepaste kwalificatie](cognitive-search-create-custom-skill-example.md) voor meer informatie).

In dit artikel leert u hoe u een pijplijn verrijking voor de vaardigheden die u wilt gebruiken. Een vaardigheden is gekoppeld aan een Azure Search [indexeerfunctie](search-indexer-overview.md). Een deel van de pipeline-ontwerp, die in dit artikel wordt de vaardigheden zelf maken. 

> [!NOTE]
> Een ander deel van het ontwerp van de pijplijn is een indexeerfunctie behandeld in geven de [volgende stap](#next-step). De definitie van een indexeerfunctie bevat een verwijzing naar de vaardigheden, plus veldtoewijzingen gebruikt voor het verbinden van invoer voor de uitvoer in de doelindex.

Belangrijke punten met:

+ U kunt slechts één vaardigheden per indexeerfunctie hebben.
+ Een vaardigheden moet ten minste één kwalificatie hebben.
+ U kunt meerdere kwalificaties van hetzelfde type (bijvoorbeeld varianten van een installatiekopie analysis-kwalificatie) maken, maar elke kwalificatie kan slechts eenmaal in de dezelfde vaardigheden gebruikt.

## <a name="begin-with-the-end-in-mind"></a>Beginnen met het einde rekening

Een aanbevolen eerste stap is bepalen welke gegevens ophalen uit de onbewerkte gegevens en hoe u wilt dat de gegevens in een zoekoplossing gebruiken. Maken van een afbeelding van de hele verrijking pijplijn, kunt u de benodigde stappen identificeren.

Stel dat u geïnteresseerd bent in een set van financiële analistopmerkingen verwerken. Voor elk bestand dat u wilt ophalen bedrijfsnamen en de algemene gevoel opmerkingen. U kunt ook een aangepaste enricher die gebruikmaakt van de Bing-entiteit-zoekservice voor meer informatie over het bedrijf, zoals wat voor soort bedrijven die het bedrijf is bezig te schrijven. In wezen, u wilt ophalen van gegevens, zoals de volgende geïndexeerd voor elk document:

| record-tekst | Bedrijven | Gevoel | Bedrijfsbeschrijvingen |
|--------|-----|-----|-----|
|voorbeeld-record| ['Microsoft', 'LinkedIn'] | 0,99 | ['Microsoft Corporation is een voor American meertalige technologiebedrijf...', 'LinkedIn is een en arbeid-bedrijfsgegevens sociale netwerken...']

Het volgende diagram illustreert een hypothetische verrijking-pijplijn:

![Een pijplijn hypothetische verrijking](media/cognitive-search-defining-skillset/sample-skillset.png "een hypothetische verrijking pijplijn")


U hebt één keer evenredige idee van wat u in de pijplijn, kunt u de vaardigheden die deze stappen biedt uitdrukken. De vaardigheden is functioneel, uitgedrukt als u de definitie van de indexeerfunctie naar Azure Search uploaden. Zie voor meer informatie over het uploaden van de indexeerfunctie, de [indexeerfunctie documentatie](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


In het diagram de *document kraken* stap gebeurt automatisch. In wezen Azure Search kunnen worden bekende bestanden openen en maakt een *inhoud* veld met de tekst opgehaald uit elk document. De witte vakken zijn ingebouwde enrichers en het decimale ' Bing entiteit ' zoekvak vertegenwoordigt een aangepaste enricher die u maakt. Zoals wordt geïllustreerd, bevat de vaardigheden drie vaardigheden.

## <a name="skillset-definition-in-rest"></a>Vaardigheden definitie in REST

Een vaardigheden is gedefinieerd als een matrix met vaardigheden. Elke kwalificatie definieert de bron van de ingevoerde gegevens en de naam van de uitvoer geproduceerd. Met behulp van de [vaardigheden REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset), kunt u een vaardigheden die correspondeert met het vorige diagram: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

```json
{
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "Calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      },
      "context": "/document/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
  ]
}
```

## <a name="create-a-skillset"></a>Een set vaardigheden maken

Tijdens het maken van een vaardigheden, kunt u een beschrijving op om de vaardigheden spreekt opgeven. Een beschrijving is optioneel, maar nuttig voor het bijhouden van wat een vaardigheden doet. Omdat vaardigheden een JSON-document, dat geen opmerkingen toestaat is, moet u een `description` element voor deze.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Het volgende gedeelte in de vaardigheden is een matrix van vaardigheden. U kunt elke kwalificatie beschouwen als een primitief van verrijking. Elke kwalificatie uitvoert een kleine taak in deze pipeline verrijking. Elke een duurt invoer (of een set van invoerwaarden) en retourneert een aantal uitvoer. De volgende secties richt u op het vooraf gedefinieerde en aangepaste vaardigheden, vaardigheden samen-koppeling door middel van invoer en uitvoer verwijzingen opgeven. Invoer kunnen afkomstig zijn van de brongegevens of van een andere kwalificatie. Uitvoer kunnen worden toegewezen aan een veld in een search-index of gebruikt als invoer voor een downstream-kwalificatie.

## <a name="add-predefined-skills"></a>Vooraf gedefinieerde vaardigheden toevoegen

Bekijk de eerste kwalificatie, de vooraf gedefinieerde is [met de naam entiteit erkenning kwalificatie](cognitive-search-skill-named-entity-recognition.md):

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    }
```

* Elke vooraf gedefinieerde kwalificatie heeft `odata.type`, `input`, en `output` eigenschappen. Kwalificatie-specifieke eigenschappen bevatten aanvullende informatie die van toepassing op deze kwalificatie. Voor de entiteit herkenning `categories` is één entiteit tussen een vaste set van Entiteitstypen die het pretrained model kunt herkennen.

* Elke kwalificatie moet een ```"context"```. De context vertegenwoordigt het niveau waarop bewerkingen plaatsvinden. De context is in de kwalificatie, het hele document, wat betekent dat de benoemde entiteit erkenning kwalificatie eenmaal per document wordt aangeroepen. Uitvoer worden ook op dat niveau geproduceerd. Meer specifiek, ```"organizations"``` worden gegenereerd als lid van ```"/document"```. In downstream vaardigheden, kunt u verwijzen naar de nieuwe informatie als ```"/document/organizations"```.  Als de ```"context"``` veld niet expliciet is ingesteld, is het document van de standaardcontext.

* De kwalificatie heeft één invoer genaamd 'tekst', met een bron invoer set ```"/document/content"```. De kwalificatie (met de naam voor entiteit) is van invloed op de *inhoud* veld van elk document een standaard veld is gemaakt door de Azure blob-indexering. 

* De kwalificatie heeft een uitvoer aangeroepen ```"organizations"```. Uitvoer bestaan alleen tijdens de verwerking. Om een keten van deze uitvoer naar een downstream kwalificatie invoer verwijzen naar de uitvoer als ```"/document/organizations"```.

* Voor een bepaald document, de waarde van ```"/document/organizations"``` is een matrix met organisaties opgehaald uit de tekst. Bijvoorbeeld:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Sommige situaties aanroepen voor elk element van een matrix afzonderlijk verwijzende. Stel bijvoorbeeld dat u wilt doorgeven van elk element van ```"/document/organizations"``` afzonderlijk naar een andere kwalificatie (zoals de aangepaste Bing entiteit zoeken enricher). U kunt verwijzen naar elk element van de matrix door een sterretje toe te voegen aan het pad. ```"/document/organizations/*"``` 

De tweede kwalificatie voor gevoel extractie volgt hetzelfde patroon als de eerste enricher. Het duurt ```"/document/content"``` als invoer- en retourneert een gevoel beoordelen voor elk exemplaar van de inhoud. Omdat u niet ingesteld de ```"context"``` expliciet veld, de uitvoer (mySentiment) is nu een onderliggend element van ```"/document"```.

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
```

## <a name="add-a-custom-skill"></a>Een aangepaste kwalificatie toevoegen

De structuur van de aangepaste Bing entiteit zoeken enricher intrekken:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      }
      "context": "/document/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
```

Deze definitie is een aangepaste kwalificatie die een web-API als onderdeel van het proces verrijking aanroept. Voor elke organisatie geïdentificeerd door opname van de benoemde entiteit, roept deze kwalificatie een web-API de beschrijving van die organisatie vinden. De orchestration van wanneer de web-API-aanroep en hoe de ontvangen gegevens stromen wordt intern verwerkt door de engine verrijking. De initialisatie van de nodig voor het aanroepen van deze aangepaste API moet echter worden opgegeven in de JSON (zoals uri, httpHeaders en de invoer verwacht). Zie voor richtlijnen bij het maken van een aangepaste web-API voor de pijplijn verrijking [het definiëren van een aangepaste interface](cognitive-search-custom-skill-interface.md).

U ziet dat het contextveld '' is ingesteld op ```"/document/organizations/*"``` met een sterretje, wat betekent dat de stap verrijking heet *voor elk* organisatie onder ```"/document/organizations"```. 

Uitvoer wordt in dit geval een bedrijfsbeschrijving gegenereerd voor elke organisatie geïdentificeerd. Wanneer u verwijst naar de beschrijving in een downstream stap (bijvoorbeeld bij het uitpakken van sleutel woordgroep), zou u het pad ```"/document/organizations/*/description"``` om dit te doen. 

## <a name="enrichments-create-structure-out-of-unstructured-information"></a>Enrichments maken structuur buiten niet-gestructureerde gegevens

De vaardigheden genereert gestructureerde gegevens buiten niet-gestructureerde gegevens. Houd rekening met het volgende voorbeeld:

*'In de vierde kwartaal Microsoft vastgelegd $1.1 miljard in de omzet van LinkedIn, het sociale netwerken bedrijf die het afgelopen jaar hebt gekocht. De overname kan Microsoft combineren LinkedIn mogelijkheden met de CRM- en Office-mogelijkheden. Aandeelhouders zijn enthousiast met de voortgang van de tot nu toe."*

Een waarschijnlijke resultaat zou zijn de gegenereerde structuur vergelijkbaar met de volgende afbeelding:

![Voorbeeld uitvoer structuur](media/cognitive-search-defining-skillset/enriched-doc.png "voorbeeld uitvoer structuur")

Intrekken dat deze structuur is voor intern gebruik. U kunt deze grafiek in de code niet daadwerkelijk ophalen.

<a name="next-step"></a>
## <a name="next-steps"></a>Volgende stappen

Nu dat u bekend met de verrijking pijplijn en vaardigheden bent, doorgaan met [aantekeningen in een vaardigheden verwijzen naar](cognitive-search-concept-annotations-syntax.md) of [uitvoer toewijzen aan velden in een index](cognitive-search-output-field-mapping.md). 

---
title: Maken van een set vaardigheden in een pijplijn cognitief zoeken - Azure Search
description: Ophalen van gegevens, natuurlijke taal verwerken, definiëren of afbeelding analysis stappen voor meer informatie en gestructureerde informatie halen uit uw gegevens voor gebruik in Azure Search.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 091a165dacbf0e98532f343745e56c4acf765b84
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53320792"
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Over het maken van een set vaardigheden in een pijplijn verrijking

Cognitieve zoekopdrachten worden uitgepakt en verrijkt gegevens zodat deze kan worden doorzocht in Azure Search. We noemen extractie en verrijking *cognitieve vaardigheden*, gecombineerd in een *vaardigheden* waarnaar wordt verwezen tijdens het indexeren. Een set vaardigheden kunt [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) of aangepaste vaardigheden (Zie [voorbeeld: Maak een aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md) voor meer informatie).

In dit artikel leert u hoe u een pijplijn verrijking voor de vaardigheden die u wilt gebruiken. Een set vaardigheden is gekoppeld aan een Azure Search [indexeerfunctie](search-indexer-overview.md). Een deel van de pijplijn-ontwerp, in dit artikel besproken, is de vaardigheden zelf maken. 

> [!NOTE]
> Een ander deel van het ontwerp van de pijplijn is op te geven een indexeerfunctie, behandeld in de [volgende stap](#next-step). Definitie van een indexeerfunctie bevat een verwijzing naar de vaardigheden, plus veldtoewijzingen gebruikt om verbinding te maken van invoer voor uitvoer in de doelindex.

Belangrijke punten om te onthouden:

+ U kunt slechts één vaardigheden per indexeerfunctie hebben.
+ Een set vaardigheden beschikken over ten minste één kwalificatie.
+ U kunt meerdere vaardigheden van hetzelfde type (bijvoorbeeld varianten van een installatiekopie analysis-kwalificatie) maken.

## <a name="begin-with-the-end-in-mind"></a>Beginnen met het einde in gedachten

Een eerste aanbevolen stap met het bepalen van de gegevens die u wilt extraheren uit de onbewerkte gegevens en hoe u gegevens wilt gebruiken die in een zoekoplossing. Het maken van een afbeelding van de gehele verrijking pijplijn, kunt u de benodigde stappen identificeren.

Stel dat u geïnteresseerd bent in het verwerken van een set financieel analistenopmerkingen. Voor elk bestand dat u wilt ophalen van de bedrijfsnamen van het en het algemene sentiment van de opmerkingen. U kunt ook het schrijven van een aangepaste enricher die gebruikmaakt van de Bing Entity Search-service voor meer informatie over het bedrijf, zoals wat voor soort bedrijven die het bedrijf is bezig. In wezen, u wilt ophalen van gegevens, zoals de volgende geïndexeerd voor elk document:

| record-tekst | Bedrijven | sentiment | Bedrijfsbeschrijvingen |
|--------|-----|-----|-----|
|voorbeeld-record| ["Microsoft", "LinkedIn"] | 0,99 | ["Microsoft Corporation is een Amerikaanse multinationale technologisch bedrijf...", "LinkedIn is een bedrijf en werk-gebaseerde sociale netwerken..."]

Het volgende diagram illustreert een hypothetische verrijking pijplijn:

![Een pijplijn hypothetische verrijking](media/cognitive-search-defining-skillset/sample-skillset.png "een hypothetische verrijking-pijplijn")


U hebt één keer geoorloofd idee van wat u wilt in de pijplijn, kunt u de vaardigheden die deze stappen biedt express. De vaardigheden is functioneel, uitgedrukt als u de definitie van de indexeerfunctie naar Azure Search uploaden. Zie voor meer informatie over het uploaden van de indexeerfunctie, de [indexer-documentatie](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


In het diagram wordt de *documenten kraken* stap gebeurt automatisch. In principe Azure Search kan worden geopend bekende bestanden en maakt een *inhoud* veld met de tekst die is geëxtraheerd uit elk document. De witte vakken zijn ingebouwde enrichers en de stippellijn met "Bing entiteiten zoeken" vak vertegenwoordigt een aangepaste enricher die u maakt. Zoals wordt geïllustreerd, bevat de vaardigheden drie vaardigheden.

## <a name="skillset-definition-in-rest"></a>De definitie van de vaardigheden in REST

Een set vaardigheden wordt gedefinieerd als een matrix van vaardigheden. Elke vaardigheid definieert de bron van de ingevoerde gegevens en de naam van de uitvoer geproduceerd. Met behulp van de [vaardigheden REST-API maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset), kunt u een set vaardigheden die overeenkomt met het vorige diagram: 

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

Tijdens het maken van een set vaardigheden, kunt u een beschrijving, zodat de vaardigheden zelf documenteren opgeven. Een beschrijving is optioneel, maar handig voor het bijhouden van de betekenis van een set vaardigheden. Omdat vaardigheden een JSON-document, die geen opmerkingen toestaat is, moet u een `description` -element voor dit.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Het volgende gedeelte in de vaardigheden is een matrix van vaardigheden. U kunt elke vaardigheid beschouwen als een primitieve nemen van verrijking. Elke vaardigheid voert een kleine taak in deze pijplijn verrijking. Elke een invoer (of een set van invoer) en bepaalde uitvoer geretourneerd. De volgende gedeelten zijn gericht op vooraf gedefinieerde en aangepaste ervaring, vaardigheden samen-koppeling door middel van invoer en uitvoer verwijzingen opgeven. Invoer kunnen afkomstig zijn van de brongegevens of van een andere vraagstukken. Uitvoer kunnen worden toegewezen aan een veld in een search-index of gebruikt als invoer voor een downstream-kwalificatie.

## <a name="add-predefined-skills"></a>Vooraf gedefinieerde vaardigheden toevoegen

Bekijk de eerste kwalificatie, dit de vooraf gedefinieerde is [met de naam van entiteit erkenning vaardigheid](cognitive-search-skill-named-entity-recognition.md):

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

* Elke vooraf gedefinieerde vaardigheid heeft `odata.type`, `input`, en `output` eigenschappen. Kwalificatie-specifieke eigenschappen bevatten aanvullende informatie die van toepassing zijn op deze kwalificatie. Voor de herkenning van entiteit `categories` is een entiteit uit een vaste set Entiteitstypen die het pretrained model kan herkennen.

* Elke vaardigheid moet een ```"context"```. De context geeft het niveau waarop-bewerkingen plaatsvinden. De context is in de bovenstaande vaardigheid het hele document, wat betekent dat de kwalificatie van de herkenning van benoemde entiteiten één keer per document wordt genoemd. Uitvoer worden ook op dat niveau geproduceerd. Meer specifiek, ```"organizations"``` worden gegenereerd als een lid van ```"/document"```. In downstream vaardigheden, kunt u verwijzen naar de zojuist gemaakte informatie als ```"/document/organizations"```.  Als de ```"context"``` veld niet expliciet is ingesteld, is de standaard-context het document.

* De kwalificatie heeft één invoer met de naam 'tekst', waarbij een bron invoer is ingesteld op ```"/document/content"```. De kwalificatie (met de naam van entiteit erkenning) is van invloed op de *inhoud* veld van elk document een standaardveld is gemaakt door de Azure blob-indexeerfunctie. 

* De kwalificatie heeft een uitvoer met de naam ```"organizations"```. Uitvoer bestaan alleen tijdens de verwerking. Verwijzen naar de uitvoer als om een keten van deze uitvoer naar een downstream vaardigheid invoer ```"/document/organizations"```.

* Voor een bepaald document, de waarde van ```"/document/organizations"``` is een matrix van organisaties die zijn geëxtraheerd uit de tekst. Bijvoorbeeld:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Sommige situaties vragen om afzonderlijk die verwijzen naar elk element van een matrix. Stel bijvoorbeeld dat u wilt doorgeven van elk element van ```"/document/organizations"``` afzonderlijk naar een andere vaardigheid (zoals de aangepaste Bing entity search enricher). U kunt verwijzen naar elk element van de matrix door een sterretje aan het pad toe te voegen: ```"/document/organizations/*"``` 

De tweede kwalificatie voor sentimentanalyse, extractie volgt hetzelfde patroon als het eerste enricher. Het duurt ```"/document/content"``` als invoer en retourneert een sentimentscore voor elk exemplaar van de inhoud. Omdat u niet hebt ingesteld de ```"context"``` expliciet veld, de uitvoer (mySentiment) is nu een onderliggend element van ```"/document"```.

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

## <a name="add-a-custom-skill"></a>Een aangepaste vaardigheid toevoegen

De structuur van de aangepaste Bing entity search enricher intrekken:

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

Deze definitie wordt een aangepaste vaardigheden die een web-API-als onderdeel van het proces verrijking aanroepen. Voor elke organisatie aangeduid met herkenning van benoemde entiteiten, roept deze kwalificatie een web-API om te zoeken, de beschrijving van die organisatie. De indeling van wanneer de web-API-aanroep en hoe de ontvangen gegevens stromen wordt intern verwerkt door de engine verrijking. De initialisatie van die nodig zijn voor deze aangepaste API oproept moet echter worden opgegeven in de JSON (zoals uri, httpHeaders en de invoer verwacht). Zie voor richtlijnen bij het maken van een aangepaste web-API voor de pijplijn verrijking [over het definiëren van een aangepaste interface](cognitive-search-custom-skill-interface.md).

U ziet dat het veld 'context' is ingesteld op ```"/document/organizations/*"``` met een sterretje, wat betekent dat de stap verrijking heet *voor elk* organisatie onder ```"/document/organizations"```. 

De uitvoer, wordt in dit geval een beschrijving van bedrijf gegenereerd voor elke organisatie geïdentificeerd. Met betrekking tot de beschrijving in een downstream stap (bijvoorbeeld in sleuteltermextractie), gebruikt u het pad ```"/document/organizations/*/description"``` om dit te doen. 

## <a name="enrichments-create-structure-out-of-unstructured-information"></a>Enrichments maken structuur buiten niet-gestructureerde gegevens

De vaardigheden genereert gestructureerde informatie uit de niet-gestructureerde gegevens. Houd rekening met het volgende voorbeeld:

*"In de vierde kwartaal Microsoft geregistreerd 1.1 miljard dollar in opbrengst van LinkedIn, het sociale netwerken bedrijf het afgelopen jaar hebt gekocht. De aanschaf kan Microsoft combineren LinkedIn-mogelijkheden met de CRM en Office. Aandeelhouders zijn blij met de voortgang van de tot nu toe."*

Een waarschijnlijke resultaat is een gegenereerde structuur die vergelijkbaar is met de volgende afbeelding:

![Voorbeeld van uitvoer structuur](media/cognitive-search-defining-skillset/enriched-doc.png "voorbeeld van uitvoer structuur")

Intrekken dat deze structuur intern is. U daadwerkelijk ophalen deze grafiek in de code niet.

<a name="next-step"></a>
## <a name="next-steps"></a>Volgende stappen

Nu u bekend bent met de pijplijn voor verrijking en de kennis en vaardigheden, doorgaan met [aantekeningen in een set vaardigheden verwijzen naar](cognitive-search-concept-annotations-syntax.md) of [uitvoer toewijzen aan velden in een index](cognitive-search-output-field-mapping.md). 

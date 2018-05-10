---
title: Vaardigheden maken (REST api-version = 2017-11-11-Preview)-Azure Search | Microsoft Docs
description: Een vaardigheden is een verzameling van cognitieve vaardigheden die bestaan uit een verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a5277d4137ede5fe6dacf993413eefd7c9e46ad4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-skillset-api-version2017-11-11-preview"></a>Vaardigheden maken (api-version = 2017-11-11-Preview)

**Van toepassing op:** api-version-2017-11-11-Preview

Een vaardigheden is een verzameling van cognitieve vaardigheden gebruikt voor de verwerking van natuurlijke taal en andere transformaties. Vaardigheden bevatten belangrijke woordgroep extraheren tekst logische groepen te verdelen in logische pagina's, onder andere benoemde entiteit extraheren.

Voor het gebruik van de vaardigheden ernaar wordt verwezen in een Azure Search-indexeerfunctie en voer vervolgens de indexeerfunctie gegevens importeren en de uitvoervelden toewijzen aan een index transformaties en verrijking aanroepen. Een vaardigheden op hoog niveau resource is, maar alleen binnen de verwerking van de indexeerfunctie actief is. Als een bron op hoog niveau, kunt u eenmaal een vaardigheden ontwerpen en vervolgens ernaar wordt verwezen in meerdere indexeerfuncties. 

Een vaardigheden wordt uitgedrukt in Azure Search via een HTTP PUT- of POST-aanvraag. De hoofdtekst van de aanvraag is voor de opslag, een JSON-schema waarmee wordt aangegeven welke vaardigheden worden aangeroepen. Vaardigheden aan elkaar worden gekoppeld via de koppelingen input-output, waar de uitvoer van een transformatie naar een andere wordt als invoer.

Een vaardigheden moet ten minste één kwalificatie hebben. Er is geen theoretische limiet voor het maximum aantal vaardigheden, maar drie tot vijf is een algemene configuratie-instellingen.  


> [!NOTE]
> Cognitieve zoekopdrachten bevindt zich in de openbare preview-fase en momenteel wordt uitvoering van vaardigheden gratis aangeboden. Op een later moment worden de prijzen voor deze mogelijkheid bekend gemaakt.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```  

## <a name="request"></a>Aanvraag  
 HTTPS is vereist voor alle aanvragen van de service. De **vaardigheden maken** aanvraag kan worden samengesteld met behulp van een PUT-methode met de naam van de vaardigheden als onderdeel van de URL. Als de vaardigheden niet bestaat, wordt deze gemaakt. Als deze al bestaat, wordt deze bijgewerkt naar de nieuwe definitie. U ziet dat u alleen een vaardigheden tegelijk kunt plaatsen.  

 De naam van de vaardigheden moet voldoen aan de volgende vereisten:

- Worden in kleine letters
- Beginnen en eindigen met een letter of cijfer
- Geen slashes of punten
- Minder dan 128 tekens bevatten 

De rest van de naam kan na het starten van de naam van de vaardigheden met een letter of cijfer bevatten een letter, cijfer en streepjes, zolang de streepjes niet opeenvolgende zijn.  

De **api-versie** parameter is vereist. De enige beschikbare versie is `2017-11-11-Preview`. Zie [API-versies in Azure Search](https://go.microsoft.com/fwlink/?linkid=834796) voor een lijst met alle versies. 


### <a name="request-headers"></a>Aanvraagheaders  

 De volgende tabel beschrijft de vereiste en optionele aanvraagheaders.  

|Aanvraag-Header|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Stel dit in op `application/json`|  
|*API-sleutel:*|Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor uw zoekservice. Het is een tekenreekswaarde die uniek is voor uw service. De **vaardigheden maken** aanvraag moet bevatten een `api-key` header ingesteld op de administratorsleutel (in plaats van een querysleutel).|  

U moet ook de naam van de service om de aanvraag-URL samen te stellen. U krijgt de servicenaam en `api-key` van uw servicedashboard in de Azure portal. Zie [maken van een Azure Search-service in de portal](search-create-service-portal.md) voor pagina navigatie hulp.  

### <a name="request-body-syntax"></a>Syntaxis van de aanvraag hoofdtekst  

De hoofdtekst van de aanvraag bevat de definitie van vaardigheden, die bestaan uit een of meer opgegeven volledig vaardigheden, evenals de optionele parameters: naam en beschrijving.  

De syntaxis voor het structureren nettolading van de aanvraag is als volgt. Een voorbeeld van een aanvraag vindt u verderop in dit artikel en ook in [het definiëren van een vaardigheden](cognitive-search-defining-skillset.md).  

```
{   
    "name" : "Required for POST, optional for PUT. Friendly name of the skillset",  
    "description" : "Optional. Anything you want, or null",  
    "Skills" : "Required. An array of skills. Each skill has an odata.type, name, input and output parameters",  
}  
```

### <a name="request-example"></a>Voorbeeld van de aanvraag
 Het volgende voorbeeld wordt een vaardigheden gebruikt voor een verzameling van financiële documenten uit te breiden.

```http
PUT https://[servicename].search.windows.net/skillsets/financedocenricher?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

De hoofdtekst van de aanvraag is een JSON-document. Deze bepaalde vaardigheden maakt gebruik van twee vaardigheden asynchroon, onafhankelijk van elkaar verwerken van de inhoud van de `contents` als twee verschillende transformaties. U kunt ook instellen dat de uitvoer van een transformatie worden de invoer van een andere. Zie voor meer informatie [het definiëren van een vaardigheden](cognitive-search-defining-skillset.md).

```json
{
  "name": "financedocenricher",
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
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
  ]
}
```

## <a name="response"></a>Antwoord  

 Voor een aanvraag is gelukt ziet u de statuscode '201 gemaakt'.  

 Standaard bevat de antwoordtekst de JSON voor de definitie van de vaardigheden die is gemaakt. Echter, als de aanvraagheader Prefer is ingesteld om terug te keren = minimaal de antwoordtekst wordt niet leeg zijn en de code van de status geslaagd zijn ' 204 geen inhoud ' in plaats van '201 gemaakt'. Dit geldt ongeacht of de PUT- of POST wordt gebruikt voor het maken van de vaardigheden.   

## <a name="see-also"></a>Zie ook

+ [Zoekoverzicht van cognitieve](cognitive-search-concept-intro.md)
+ [Snelstartgids: Probeer cognitieve zoeken](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Verrijkt indexeren van Azure blobs](cognitive-search-tutorial-blob.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Het toewijzen van velden](cognitive-search-output-field-mapping.md)
+ [Het definiëren van een aangepaste interface](cognitive-search-custom-skill-interface.md)
+ [Voorbeeld: een aangepaste kwalificatie maken](cognitive-search-create-custom-skill-example.md)
+ [Vooraf gedefinieerde sklls](cognitive-search-predefined-skills.md)
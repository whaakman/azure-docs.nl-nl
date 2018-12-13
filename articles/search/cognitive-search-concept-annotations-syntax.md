---
title: Referentie-invoer en uitvoer in cognitive zoeken pijplijnen - Azure Search
description: Verklaart de syntaxis van de aantekening en hoe u om te verwijzen naar een aantekening in de invoer en uitvoer van een set vaardigheden in een pijplijn cognitief zoeken in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 57fed710d7d58199fb3cb70640d1f2d3f316f180
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315802"
---
# <a name="how-to-reference-annotations-in-a-cognitive-search-skillset"></a>Hoe om te verwijzen naar aantekeningen in een vaardigheden cognitief zoeken

In dit artikel leert u hoe u om te verwijzen naar aantekeningen in definities van vaardigheden, met behulp van voorbeelden ter illustratie van verschillende scenario's. Wanneer de inhoud van een document via een set vaardigheden, haalt deze verrijkt met aantekeningen. Aantekeningen kunnen worden gebruikt als invoer voor verdere downstream verrijking of toegewezen aan een uitvoerveld in een index. 
 
Voorbeelden in dit artikel zijn gebaseerd op de *inhoud* veld automatisch gegenereerd [Azure Blob-indexeerfuncties](search-howto-indexing-azure-blob-storage.md) als onderdeel van het document fase kraken. Met betrekking tot documenten uit een Blob-container, gebruikt u een indeling zoals `"/document/content"`, waarbij de *inhoud* veld deel uitmaakt van de *document*. 

## <a name="background-concepts"></a>Achtergrondconcepten

Voordat u de syntaxis van de bekeken, gaan we terug naar een paar belangrijke concepten voor meer informatie over de voorbeelden verderop in dit artikel.

| Termijn | Description |
|------|-------------|
| Verrijkt Document | Een geavanceerde document is een interne structuur gemaakt en gebruikt door de pijplijn voor het opslaan van alle aantekeningen met betrekking tot een document. Een document verrijkt beschouwen als een boomstructuur van aantekeningen. Een aantekening gemaakt op basis van een vorige aantekening wordt over het algemeen het onderliggende.<p/>Verrijkt documenten bestaan alleen voor de duur van de uitvoering van vaardigheden. Wanneer inhoud wordt toegewezen aan de search-index, wordt het verrijkt document niet meer nodig hebt. Hoewel u niet rechtstreeks met verrijkt documenten communiceren, is het handig om een mentale model van de documenten hebt bij het maken van een set vaardigheden. |
| Verrijking Context | De context waarin de verrijking plaatsvindt, in termen van welke element is uitgebreid. De context verrijking is standaard op de `"/document"` niveau binnen het bereik van afzonderlijke documenten. Wanneer een kwalificatie wordt uitgevoerd, wordt de uitvoer van deze kwalificatie worden [eigenschappen van de gedefinieerde context](#example-2).|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Voorbeeld 1: Met eenvoudige aantekeningsverwijzing

Stel dat u beschikt over tal van bestanden met verwijzingen naar de namen van personen die u wilt ophalen met herkenning van named entity's in Azure Blob-opslag. In de definitie van de kwalificatie hieronder `"/document/content"` is de tekstuele weergave genereren van het hele document en 'iedereen' is een extractie van de volledige naam voor entiteiten die zijn geïdentificeerd als personen.

Omdat de standaardcontext `"/document"`, de lijst met personen kan nu worden verwezen als `"/document/people"`. In dit specifieke geval `"/document/people"` is van een aantekening, dat kan nu worden toegewezen aan een veld in een index, of gebruikt in een ander niveau in de dezelfde vaardigheden.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```

<a name="example-2"></a>

## <a name="example-2-reference-an-array-within-a-document"></a>Voorbeeld 2: Verwijzen naar een matrix in een document

In dit voorbeeld is gebaseerd op het vorige voorbeeld, waarin u wordt getoond hoe een stap verrijking meerdere keren aangeroepen via hetzelfde document. Wordt ervan uitgegaan dat het vorige voorbeeld gegenereerd een matrix met tekenreeksen met 10 mensen namen vanuit één document. Een redelijke volgende stap is mogelijk een tweede verrijking die de achternaam uit een volledige naam ophaalt. Omdat er 10 namen, wilt u deze stap moet worden aangeroepen 10 keer in dit document, één keer voor elke persoon. 

Voor het aanroepen van het juiste aantal iteraties, stelt u de context als `"/document/people/*"`, waarbij het sterretje (`"*"`) staat voor alle knooppunten in het document verrijkt als afstammelingen van `"/document/people"`. Hoewel deze kwalificatie is slechts één keer gedefinieerd in de matrix vaardigheden, wordt deze aangeroepen voor elk lid in het document totdat alle leden worden verwerkt.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the last name from a full name",
    "uri": "http://names.azurewebsites.net/api/GetLastName",
    "context" : "/document/people/*",
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "fullname",
        "source": "/document/people/*"
      }
    ],
    "outputs": [
      {
        "name": "lastname",
        "targetName": "last"
      }
    ]
  }
```

Wanneer aantekeningen matrices of verzamelingen van tekenreeksen, is het raadzaam om u te richten op specifieke leden in plaats van de matrix als geheel. Het bovenstaande voorbeeld worden gegenereerd met de naam van een aantekening `"last"` onder elk knooppunt dat wordt vertegenwoordigd door de context. Als u wilt om te verwijzen naar deze reeks aantekeningen, kunt u de syntaxis van de `"/document/people/*/last"`. Als u wilt om te verwijzen naar een bepaalde aantekening, kunt u een expliciete index: `"/document/people/1/last`' om te verwijzen naar de achternaam van de eerste persoon die wordt geïdentificeerd in het document. U ziet dat in deze syntaxis matrices '0 geïndexeerd'.

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Voorbeeld 3: Leden van de referentie in een matrix

Soms moet u aan de groep alle aantekeningen van een bepaald type ze doorgeven aan een bepaalde kwalificatie. Houd rekening met een hypothetische aangepaste vaardigheden waarmee de meest voorkomende achternaam van de laatste namen in voorbeeld 2 hebt uitgepakt. Om alleen de laatste namen aan de aangepaste vaardigheid, geef de context als `"/document"` en de invoer als `"/document/people/*/lastname"`.

Houd er rekening mee dat de kardinaliteit van `"/document/people/*/lastname"` groter is dan die van het document. Mogelijk zijn er 10 lastname knooppunten terwijl er slechts één documentknooppunt voor dit document. In dat geval wordt het systeem automatisch maakt een matrix met `"/document/people/*/lastname"` met alle van de elementen in het document.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the most common string from an array of strings",
    "uri": "http://names.azurewebsites.net/api/MostCommonString",
    "context" : "/document",
    "inputs": [
      {
        "name": "strings",
        "source": "/document/people/*/lastname"
      }
    ],
    "outputs": [
      {
        "name": "mostcommon",
        "targetName": "common-lastname"
      }
    ]
  }
```



## <a name="see-also"></a>Zie ook
+ [Hoe u een aangepaste vaardigheden integreren in een pijplijn verrijking](cognitive-search-custom-skill-interface.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen aan een index](cognitive-search-output-field-mapping.md)

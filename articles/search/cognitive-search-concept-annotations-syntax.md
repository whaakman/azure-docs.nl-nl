---
title: Verwijzen naar een aantekening in invoer en uitvoer in een pijplijn cognitieve zoeken in Azure Search | Microsoft Docs
description: Verklaart de syntaxis van de aantekening en hoe om te verwijzen naar een aantekening in de invoer en uitvoer van een vaardigheden in een pijplijn cognitieve zoeken in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 2e838e9c94d5b19565bea3d02890fe6164bb37d0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-reference-annotations-in-a-cognitive-search-skillset"></a>Aantekeningen in een vaardigheden cognitieve zoeken verwijzen naar

In dit artikel leert u hoe om te verwijzen naar aantekeningen in de kwalificatie definities, met voorbeelden ter illustratie van verschillende scenario's. Als de inhoud van een document via een set vaardigheden loopt, wordt deze verrijkt met aantekeningen. Aantekeningen worden gebruikt als invoer voor verdere downstream verrijking of toegewezen aan een uitvoerveld in een index. 
 
De voorbeelden in dit artikel zijn gebaseerd op de *inhoud* veld gegenereerd automatisch door [indexeerfuncties in Azure Blob](search-howto-indexing-azure-blob-storage.md) als onderdeel van het document kraken fase. Bij het verwijzen naar documenten van een blobcontainer, gebruikt u een indeling, zoals `"/document/content"`, waarbij de *inhoud* veld deel uitmaakt van de *document*. 

## <a name="background-concepts"></a>Achtergrondconcepten

Voordat u de syntaxis van de bekeken, bezoekt u gaan we enkele belangrijke concepten voor een beter begrip van de voorbeelden verderop in dit artikel.

| Termijn | Beschrijving |
|------|-------------|
| Verrijkt Document | Een verrijkt document is een interne structuur gemaakt en gebruikt door de pijplijn voor het opslaan van alle aantekeningen met betrekking tot een document. Een document verrijkt beschouwen als een boomstructuur van aantekeningen. In het algemeen wordt gemaakt op basis van een vorige aantekening van een aantekening het onderliggende.<p/>Verrijkt documenten bestaan alleen voor de duur van vaardigheden worden uitgevoerd. Wanneer inhoud wordt toegewezen aan de search-index, wordt het verrijkt document niet langer nodig. Hoewel u niet rechtstreeks met verrijkt documenten werken, is het nuttig om een geestelijke model van de documenten hebt bij het maken van een vaardigheden. |
| Verrijking Context | De context waarin de verrijking plaatsvindt, in termen van welke element is uitgebreid. De context verrijking is standaard op de `"/document"` niveau binnen het bereik van afzonderlijke documenten. Wanneer een kwalificatie wordt uitgevoerd, wordt de uitvoer van deze kwalificatie worden [eigenschappen van de gedefinieerde context](#example-2).|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Voorbeeld 1: Eenvoudige aantekeningsverwijzing

Stel tal van bestanden met verwijzingen naar namen van mensen die u wilt ophalen met behulp van de benoemde entiteit opname in Azure Blob-opslag. In de onderstaande kwalificatie-definitie `"/document/content"` is de tekstweergave van het hele document en 'gebruikers' is een extractie van volledige namen voor entiteiten die zijn geïdentificeerd als personen.

Omdat de standaardcontext `"/document"`, de lijst met personen kan nu worden verwezen als `"/document/people"`. In dit specifieke geval `"/document/people"` is van een aantekening kan nu worden toegewezen aan een veld in een index of gebruikt in een ander niveau in de dezelfde vaardigheden.

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

In dit voorbeeld is gebaseerd op het vorige voorbeeld, waarin u wordt beschreven hoe u een stap verrijking meerdere keren aanroepen via hetzelfde document. Stel dat het vorige voorbeeld een matrix met tekenreeksen met de namen van 10 mensen gegenereerd vanuit één document. Een redelijke volgende stap is mogelijk een tweede verrijking die de achternaam uit een volledige naam ophaalt. Omdat er 10 namen, wilt u deze stap moet worden aangeroepen 10 keer in dit document, één keer voor elke persoon. 

Als u wilt het juiste aantal iteraties aanroepen, stelt u de context als `"/document/people/*"`, waarbij het sterretje (`"*"`) vertegenwoordigt alle knooppunten in het document verrijkt als descendanten van `"/document/people"`. Hoewel deze kwalificatie is slechts één keer gedefinieerd in de matrix vaardigheden, wordt deze aangeroepen voor elk lid in het document totdat alle leden zijn verwerkt.

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

Wanneer aantekeningen matrices of verzamelingen van tekenreeksen zijn, wilt u mogelijk specifieke leden in plaats van de matrix in zijn geheel als doel. Het bovenstaande voorbeeld van een aantekening aangeroepen genereert `"last"` onder elk knooppunt dat wordt vertegenwoordigd door de context. Als u verwijzen naar deze reeks aantekeningen wilt, kunt u gebruiken de syntaxis van de `"/document/people/*/last"`. Als u om te verwijzen naar een bepaalde aantekening wilt, kunt u een expliciete index: `"/document/people/1/last`' als u verwijst naar de achternaam van de eerste persoon geïdentificeerd in het document. U ziet dat de matrices in deze syntaxis '1 geïndexeerd' zijn.

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Voorbeeld 3: Verwijzingsleden binnen een matrix

Soms moet u alle aantekeningen van een bepaald type ze doorgeven aan een bepaalde kwalificatie groep. U kunt een hypothetische aangepaste kwalificatie waarmee de meest voorkomende achternaam van de laatste namen in voorbeeld 2 hebt uitgepakt. Als u alleen de laatste namen voor de aangepaste kwalificatie, geef de context als `"/document"` en de invoer als `"/document/people/*/lastname"`.

Houd er rekening mee dat de kardinaliteit van `"/document/people/*/lastname"` groter is dan die van het document. Mogelijk zijn er 10 lastname knooppunten terwijl er slechts één documentknooppunt voor dit document. Het systeem wordt in dat geval automatisch maken van een matrix van `"/document/people/*/lastname"` met alle van de elementen in het document.

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
+ [Het integreren van een aangepaste kwalificatie in een pijplijn verrijking](cognitive-search-custom-skill-interface.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Vaardigheden (REST) maken](ref-create-skillset.md)
+ [Het verrijkt velden toewijzen aan een index](cognitive-search-output-field-mapping.md)

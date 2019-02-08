---
title: Schema-indeling - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de schema-indeling in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 23120e45a1070f46ae4e1927a29bdab4c990d96f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860708"
---
# <a name="schema-format"></a>Schema-indeling

Het schema is opgegeven in een JSON-bestand dat de kenmerk-structuur van de objecten in het gegevensbestand dat wordt gebruikt voor het maken van de index wordt beschreven.  Het schema voor elk kenmerk Hiermee geeft u de naam, gegevenstype, optioneel bewerkingen en optionele synoniemen lijst.  Een object kan 0 of meer waarden voor elk kenmerk hebben.  Hieronder volgt een voorbeeld van een vereenvoudigde van een domein academische publicatie:

``` json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Kenmerknamen zijn hoofdlettergevoelig id's die beginnen met een letter en bestaan alleen uit letters (A-Z), cijfers (0-9) en onderstrepingstekens (\_).  Het kenmerk gereserveerde 'logprob' wordt gebruikt om op te geven van de relatieve natuurlijke logboek kansen tussen objecten.

## <a name="attribute-type"></a>Kenmerktype

Hieronder volgt een lijst met ondersteunde kenmerk gegevenstypen:

| Type | Description | Bewerkingen | Voorbeeld |
|------|-------------|------------|---------|
| String | Tekenreeks (1-1024 tekens) | is gelijk aan, starts_with | "Hallo wereld" |
| Int32 | 32-bits geheel getal | gelijk aan, starts_with, is_between | 2016 |
| Int64 | 64-bits geheel getal | gelijk aan, starts_with, is_between | 9876543210 |
| Double | Dubbele precisie drijvende-kommawaarde | gelijk aan, starts_with, is_between | 1.602e-19 |
| Date | Datum (1400-01-01 op 9999-12-31) | equals, is_between | '2016-03-14' |
| Guid | Unieke id | is gelijk aan | "602DD052-CC47-4B23-A16A-26B52D30C05B" |
| Blob | Intern gecomprimeerde gegevens met niet-geïndexeerde | *Geen* | 'Elke persoon en iedere organisatie op de planeet om meer te bereiken maken' |
| Samengestelde | Samenstelling van meerdere onderliggende kenmerken| *N.v.t.* | {"Naam": "harry shum", "Relatie": "microsoft"} |

Tekenreekskenmerken worden gebruikt om aan te duiden tekenreekswaarden die kunnen worden weergegeven als onderdeel van de gebruikersquery.  Deze ondersteuning bieden voor de exacte overeenkomst *gelijk is aan* bewerking, evenals de *starts_with* bewerking voor de query is voltooid scenario's, zoals die overeenkomen met "micros" met "microsoft".  Niet-hoofdlettergevoelig en zoeken bij benadering die overeenkomt met voor de verwerking van spelfouten worden ondersteund in een toekomstige release.

Int32/Int64/dubbele kenmerken worden gebruikt om numerieke waarden te geven.  De *is_between* bewerking ongelijkheid ondersteuning (lt, le, gt, ge) wordt tijdens de uitvoering.  De *starts_with* bewerking ondersteunt query voltooid scenario's, zoals die overeenkomen met "20" met '2016' of '3'. met '3,14'.

Datumkenmerken worden gebruikt voor het efficiënt coderen date-waarden.  De *is_between* bewerking ongelijkheid ondersteuning (lt, le, gt, ge) wordt tijdens de uitvoering.
  
GUID-kenmerken worden gebruikt om aan te duiden efficiënt GUID-waarden met standaardondersteuning voor de *gelijk is aan* bewerking.

BLOB-kenmerken worden gebruikt voor het efficiënt coderen mogelijk grote hoeveelheden gegevensblobs voor het opzoeken van de runtime van het bijbehorende object, zonder ondersteuning voor indexering bewerkingen op basis van de inhoud van de blob-waarden.

### <a name="composite-attributes"></a>Samengestelde kenmerken

Samengestelde kenmerken worden gebruikt om weer te geven van een groepering van kenmerkwaarden.  De naam van elke onderliggende kenmerk begint met de naam van het samengestelde kenmerk gevolgd door '. '.  Waarden voor samengestelde kenmerken worden opgegeven als een JSON-object met de geneste kenmerkwaarden.  Samengestelde kenmerken kunnen meerdere waarden van het object hebben.  Samengestelde kenmerken kunnen echter geen onderliggende kenmerken die zelf samengestelde kenmerken.

Hierdoor kunnen de service om te zoeken naar documenten door "harry shum" terwijl hij is 'microsoft' in het bovenstaande voorbeeld academische publicatie.  Zonder samengestelde kenmerken, de service kan alleen een query voor documenten die daar een van de auteurs "harry shum' en een van de auteurs op 'microsoft'.  Zie voor meer informatie, [samengestelde query's](SemanticInterpretation.md#composite-function).

## <a name="attribute-operations"></a>Kenmerk bewerkingen

Standaard wordt elk kenmerk ter ondersteuning van alle bewerkingen die beschikbaar zijn voor het kenmerktype geïndexeerd.  Als een bepaalde bewerking niet vereist is, kan het aantal geïndexeerde bewerkingen expliciet worden opgegeven om de grootte van de index te beperken.  In het volgende fragment uit het bovenstaande voorbeeldschema het kenmerk Author.Id wordt geïndexeerd als u wilt ondersteunen alleen de *gelijk is aan* bewerking, maar niet de extra *starts_with* en *is_between*  bewerkingen voor Int32 kenmerken.
```json
{"name":"Author.Id", "type":"Int32", "operations":["equals"]}
```

Wanneer een kenmerk wordt verwezen in een grammatica, de *starts_with* bewerking moet worden opgegeven in het schema in volgorde van de service voor het genereren van voltooiingen van gedeeltelijke query's.  

## <a name="attribute-synonyms"></a>Kenmerk synoniemen

Vaak is het wenselijk om te verwijzen naar een tekenreekswaarde voor bepaalde kenmerk door een synoniem.  Gebruikers kunnen bijvoorbeeld 'Microsoft' als 'MSFT' of 'MS' raadplegen.  In dergelijke gevallen kan de kenmerkdefinitie Geef de naam van een schemabestand zich in dezelfde map als het schemabestand.  Elke regel in het bestand synoniem vertegenwoordigt een synoniem vermelding in de volgende JSON-indeling: `["<canonical>", "<synonym>"]`.  In het voorbeeldschema is 'AuthorName.syn' een JSON-bestand met waarden voor het kenmerk Author.Name synoniem.

`{"name":"Author.Name", "type":"String", "synonyms":"AuthorName.syn"}`


Een canonieke waarde mogelijk meerdere synoniemen.  Meerdere canonieke waarden kunnen delen een gemeenschappelijke synoniem.  In dergelijke gevallen wordt de service via meerdere interpretaties dubbelzinnigheid opgelost.  Hieronder wordt een voorbeeld AuthorName.syn synoniemen bestand overeenkomt met het bovenstaande schema:
```json
["harry shum","heung-yeung shum"]
["harry shum","h shum"]
["henry shum","h shum"]
...
```

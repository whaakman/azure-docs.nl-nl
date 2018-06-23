---
title: Schema-indeling in de kennis exploratie Service API | Microsoft Docs
description: Meer informatie over de schema-indeling in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 3009392a5acb12a8f4df3d30a2cbe5e74f2172fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344522"
---
# <a name="schema-format"></a>Schema-indeling
Het schema is opgegeven in een JSON-bestand dat de kenmerk-structuur van de objecten in het gegevensbestand gebruikt voor het maken van de index wordt beschreven.  Het schema voor elk kenmerk Hiermee geeft u de naam, gegevenstype, optionele operations en optionele synoniemen lijst.  Een object kan 0 of meer waarden van elk kenmerk hebben.  Hieronder vindt u een voorbeeld van een domein academic publicatie van een vereenvoudigde:

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

Kenmerknamen zijn hoofdlettergevoelig id's die beginnen met een letter en bestaan alleen uit letters (A-Z), cijfers (0-9) en onderstrepingsteken (\_).  De gereserveerde 'logprob'-kenmerk wordt gebruikt om op te geven van de relatieve natuurlijke logboek waarschijnlijkheid tussen objecten.

## <a name="attribute-type"></a>Kenmerktype
Hieronder vindt u een lijst met ondersteunde kenmerk gegevenstypen:

| Type | Beschrijving | Bewerkingen | Voorbeeld |
|------|-------------|------------|---------|
| Reeks | Tekenreeks (1 en 1024 tekens) | is gelijk aan, starts_with | "Hallo wereld" |
| Int32 | 32-bits geheel getal | is gelijk aan, starts_with, is_between | 2016 |
| Int64 | 64-bits geheel getal | is gelijk aan, starts_with, is_between | 9876543210 |
| Double | Kommagetal met dubbele precisie | is gelijk aan, starts_with, is_between | 1.602e-19 |
| Date | Datum (01-01-1400 naar 9999-12-31) | is gelijk aan, is_between | ' 2016-03-14' |
| GUID | Globaal unieke id | is gelijk aan | '602DD052-CC47-4B23-A16A-26B52D30C05B' |
| Blob | Intern gecomprimeerde gegevens van de niet-geïndexeerde | *Geen* | 'Zorgen dat elke gebruiker en elke organisatie ter wereld te bereiken meer' |
| Samengestelde | Samenstelling van meerdere onderliggende kenmerken| *N.v.t.* | {{'Name': 'harry shum', 'Relatie': 'microsoft'} |

Tekenreekskenmerken worden gebruikt om aan te duiden tekenreekswaarden die kunnen worden weergegeven als onderdeel van de gebruikersquery.  Deze ondersteuning bieden voor de exacte overeenkomst *gelijk is aan* bewerking, evenals de *starts_with* bewerking voor de query voltooiing scenario's, zoals die overeenkomt met 'micros' met 'microsoft'.  Niet-hoofdlettergevoelige en fuzzy overeenkomende voor het afhandelen van fouten spelling worden ondersteund in een toekomstige release.

Int64-Int32/dubbele kenmerken worden gebruikt om numerieke waarden te geven.  De *is_between* bewerking ongelijk ondersteuning (lt, RP, gt, ge) wordt tijdens de uitvoering.  De *starts_with* bewerking ondersteunt query voltooiing scenario's, zoals die overeenkomen met '20' met "2016" of "3". met '3,14'.

Datumkenmerken worden gebruikt voor het coderen van efficiënt date-waarden.  De *is_between* bewerking ongelijk ondersteuning (lt, RP, gt, ge) wordt tijdens de uitvoering.
  
GUID-kenmerken worden gebruikt om aan te duiden efficiënt GUID-waarden met standaardondersteuning voor de *gelijk is aan* bewerking.

BLOB-kenmerken worden gebruikt voor het coderen van efficiënt blobs potentieel grote hoeveelheden gegevens voor het opzoeken van de runtime van het bijbehorende object, zonder ondersteuning voor indexering bewerkingen op basis van de inhoud van de blob-waarden.

### <a name="composite-attributes"></a>Samengestelde kenmerken
Samengestelde kenmerken worden gebruikt om weer te geven van een groepering van kenmerkwaarden.  De naam van elke onderliggende kenmerk begint met de naam van de samengestelde kenmerk gevolgd door '. '.  Waarden voor samengestelde kenmerken worden opgegeven als een JSON-object met de geneste kenmerkwaarden.  Samengestelde kenmerken kunnen meerdere objectwaarden hebben.  Samengestelde kenmerken mag echter geen onderliggende kenmerken die zelf samengestelde kenmerken zijn.

Hierdoor kunnen de service om te zoeken naar documenten door 'harry shum' terwijl hij op 'microsoft is' in het bovenstaande voorbeeld academic publicatie.  Zonder samengestelde kenmerken, de service kan alleen een query voor documenten, waarbij een van de auteurs is 'harry shum' en een van de auteurs op 'microsoft'.  Zie voor meer informatie [samengestelde query's](SemanticInterpretation.md#composite-function).

## <a name="attribute-operations"></a>Kenmerk-bewerkingen
Standaard wordt elk kenmerk ter ondersteuning van alle bewerkingen die beschikbaar zijn met het kenmerktype geïndexeerd.  Als een bepaalde bewerking niet vereist is, kan de set geïndexeerde bewerkingen expliciet worden opgegeven om de grootte van de index te beperken.  In het volgende fragment uit het bovenstaande voorbeeldschema het kenmerk Author.Id wordt geïndexeerd als u wilt ondersteunen alleen de *gelijk is aan* bewerking, maar niet de extra *starts_with* en *is_between*  bewerkingen voor Int32 kenmerken.
```json
{"name":"Author.Id", "type":"Int32", "operations":["equals"]}
```

Wanneer een kenmerk wordt verwezen in een grammatica de *starts_with* bewerking moet worden opgegeven in het schema in om de service voor het genereren van voltooiingen van gedeeltelijke query's.  

## <a name="attribute-synonyms"></a>Kenmerk synoniemen
Vaak is het wenselijk om te verwijzen naar een bepaalde reeks kenmerkwaarde door een synoniem.  Gebruikers kunnen bijvoorbeeld 'Microsoft' als 'MSFT' of 'MS' raadplegen.  In dergelijke gevallen kan de kenmerkdefinitie Geef de naam van een schemabestand zich in dezelfde map als het schemabestand.  Elke regel in het bestand synoniem vertegenwoordigt een synoniem vermelding in de volgende JSON-indeling: `["<canonical>", "<synonym>"]`.  In het voorbeeldschema is 'AuthorName.syn' een JSON-bestand met waarden voor het kenmerk naam.auteur synoniem.

`{"name":"Author.Name", "type":"String", "synonyms":"AuthorName.syn"}`


Een canonieke waarde mogelijk meerdere synoniemen.  Meerdere canonieke waarden kunnen delen een gemeenschappelijke synoniem.  In dergelijke gevallen wordt de service de dubbelzinnigheid via meerdere interpretaties opgelost.  Hieronder wordt een voorbeeld AuthorName.syn synoniemen bestand overeenkomt met het bovenstaande schema:
```json
["harry shum","heung-yeung shum"]
["harry shum","h shum"]
["henry shum","h shum"]
...
```

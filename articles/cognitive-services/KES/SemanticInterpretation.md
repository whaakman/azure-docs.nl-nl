---
title: Semantische interpretatie - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van semantische interpretatie in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 2b0065dbdac8e3bdbc535f2d7d103b24110e1d02
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217268"
---
# <a name="semantic-interpretation"></a>Semantische interpretatie

Semantische interpretatie koppelt semantische uitvoer aan elk geïnterpreteerde pad via de grammatica.  In het bijzonder de service beoordeelt wat de volgorde van de instructies in de `tag` elementen door de interpretatie voor het berekenen van de uiteindelijke uitvoer doorlopen.  

Een instructie is mogelijk een toewijzing van een letterlijke tekenreeks of een variabele naar een andere variabele.  Het kan ook de uitvoer van een functie met een 0 of meer parameters toewijzen aan een variabele.  Elke parameter van de functie kan worden opgegeven met een letterlijke tekenreeks of een variabele.  Als de functie geen uitvoer retourneert, wordt de toewijzing wordt weggelaten.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Een variabele is opgegeven met behulp van een naam-id die begint met een letter en bestaat alleen uit letters (A-Z), cijfers (0-9) en het onderstrepingsteken (\_).  Het type is impliciet afgeleid van de letterlijke waarde of functie waarde is toegewezen aan deze uitvoer. 

Hieronder volgt een lijst met ondersteunde gegevenstypen:

|Type|Beschrijving|Voorbeelden|
|----|----|----|
|Reeks|Volgorde van 0 of meer tekens|"Hallo wereld!"<br/>""|
|BOOL|Booleaanse waarde|true<br/>false|
|Int32|32-bits ondertekende integer.  -2.1e9-2.1e9|123<br/>-321|
|Int64|64-bits ondertekende integer. -9.2e18 en 9.2e18|9876543210|
|Double-waarde|Dubbele precisie met drijvende komma. 1.7E +/-308 (15 cijfers)|123.456789<br/>1.23456789e2|
|GUID|Unieke id|"602DD052-CC47-4B23-A16A-26B52D30C05B"|
|Query’s uitvoeren|Query-expressie waarmee een subset van objecten in de index|All()<br/>And(*q1*, *q2*)|

## <a name="semantic-functions"></a>Semantische functies

Er is een ingebouwde set semantische functies.  Ze staan de constructie van geavanceerde query's en contextafhankelijk controle bieden over een perfecte ervaring bij grammatica.

### <a name="and-function"></a>And -functie

`query = And(query1, query2);`

Retourneert een query die is samengesteld uit het snijpunt van de twee ingevoerde query's.

### <a name="or-function"></a>Anders werkt

`query = Or(query1, query2);`

Retourneert een query die is samengesteld uit de vereniging van de twee ingevoerde query's.

### <a name="all-function"></a>Alle functie

`query = All();`

Retourneert een query uitvoert met alle objecten voor gegevens.

In het volgende voorbeeld gebruiken we de functie All() iteratief bouwen van een query die is gebaseerd op het snijpunt van 1 of meer trefwoorden.

```
<tag>query = All();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = And(query, keyword);</tag>
</item>
```

### <a name="none-function"></a>Geen functie

`query = None();`

Retourneert een query die geen objecten bevat.

In het volgende voorbeeld gebruiken we de functie None() iteratief bouwen van een query op basis van de samenvoeging van 1 of meer trefwoorden.

```
<tag>query = None();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = Or(query, keyword);</tag>
</item>
```

### <a name="query-function"></a>Functie voor query

```
query = Query(attrName, value)
query = Query(attrName, value, op)
```

Retourneert een query die alleen objecten bevat waarvan het kenmerk *attrName* komt overeen met de waarde *waarde* op basis van de opgegeven bewerking *op*, die standaard ingesteld op 'eq'.  Meestal gebruikt een `attrref` element te maken van een query op basis van de overeenkomende invoer voor de query-tekenreeks.  Als een waarde wordt opgegeven of via andere middelen verkregen, kan de functie Query() kan worden gebruikt om een query die overeenkomt met deze waarde te maken.

In het volgende voorbeeld gebruiken we de functie Query() voor het implementeren van ondersteuning voor het opgeven van academische publicaties vanaf een bepaalde decennium.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

### <a name="composite-function"></a>Samengestelde functie

`query = Composite(innerQuery);`

Retourneert een query die wordt ingekapseld een *innerQuery* bestaat van overeenkomsten op basis van onderliggende kenmerken van een gemeenschappelijk samengestelde kenmerk *%{attr/*.  De inkapseling moet het samengestelde kenmerk *%{attr/* van een overeenkomende gegevensobject dat ten minste één waarde die afzonderlijk voldoet aan de *innerQuery*.  Houd er rekening mee dat een query op onderliggende kenmerken van een samengestelde kenmerk heeft te integreren met behulp van de functie Composite() voordat deze kan worden gecombineerd met andere query's.

De volgende query retourneert bijvoorbeeld academische publicaties door "harry shum" terwijl hij op 'microsoft is':
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

De volgende query retourneert aan de andere kant academische publicaties daar een van de auteurs "harry shum' en een van de lidmaatschappen is 'microsoft':
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>De functie GetVariable

`value = GetVariable(name, scope);`

Retourneert de waarde van variabele *naam* gedefinieerd onder het opgegeven *bereik*.  *naam* is een id die begint met een letter en bestaat alleen uit letters (A-Z), cijfers (0-9) en het onderstrepingsteken (_).  *bereik* kan worden ingesteld op 'aanvragen' of 'systeem'.  Houd er rekening mee dat variabelen die zijn gedefinieerd onder verschillende bereiken, verschilt van elkaar worden verbonden, inclusief resources via de uitvoer van semantische functies gedefinieerd.

Aanvraag bereik variabelen zijn verdeeld over alle interpretaties binnen de huidige interpreteren-aanvraag.  Ze kunnen worden gebruikt voor het beheren van de zoekopdracht voor een perfecte ervaring bij via de grammatica.

Systeemvariabelen zijn vooraf gedefinieerd door de service en kunnen worden gebruikt om op te halen van verschillende statistische gegevens over de huidige status van het systeem.  Hieronder volgt de set met ondersteunde systeemvariabelen:

|Name|Type|Beschrijving|
|----|----|----|
|IsAtEndOfQuery|BOOL|True als de huidige interpretatie heeft overeenkomende alle invoer voor de query-tekst|
|IsBeyondEndOfQuery|BOOL|True als de huidige interpretatie voltooiingen buiten de tekst van de invoer voor de query is voorgesteld|

### <a name="setvariable-function"></a>De functie SetVariable

`SetVariable(name, value, scope);`

Toegewezen *waarde* aan variabele *naam* onder de opgegeven *bereik*.  *naam* is een id die begint met een letter en bestaat alleen uit letters (A-Z), cijfers (0-9) en het onderstrepingsteken (_).  Op dit moment de enige geldige waarde voor *bereik* is 'aanvragen'.  Er zijn geen instelbare systeemvariabelen.

Aanvraag bereik variabelen zijn verdeeld over alle interpretaties binnen de huidige interpreteren-aanvraag.  Ze kunnen worden gebruikt voor het beheren van de zoekopdracht voor een perfecte ervaring bij via de grammatica.

### <a name="assertequals-function"></a>De functie AssertEquals

`AssertEquals(value1, value2);`

Als *value1* en *value2* equivalent, de functie is voltooid en heeft geen bijwerkingen zijn.  Anders de functie mislukt en de interpretatie worden geweigerd.

### <a name="assertnotequals-function"></a>De functie AssertNotEquals

`AssertNotEquals(value1, value2);`

Als *value1* en *value2* zijn geen equivalent, de functie is voltooid en heeft geen bijwerkingen.  Anders de functie mislukt en de interpretatie worden geweigerd.



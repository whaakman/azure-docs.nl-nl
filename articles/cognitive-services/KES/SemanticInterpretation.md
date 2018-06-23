---
title: Semantische interpretatie in kennis exploratie Service API | Microsoft Docs
description: Informatie over het semantische interpretatie in het Knowledge exploratie Service (KES) API in cognitieve Services gebruiken.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 022188464eb7269b69f96a058b444167b587387c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344550"
---
# <a name="semantic-interpretation"></a>Semantische interpretatie
Semantische uitvoer koppelt semantische interpretatie aan elk geïnterpreteerde pad via de grammatica.  In het bijzonder de service beoordeelt wat de volgorde van de instructies in de `tag` elementen doorkruist door de interpretatie berekenen van de uiteindelijke uitvoer.  

Een instructie is mogelijk een toewijzing van een letterlijke waarde of een variabele aan een andere variabele.  Het kan ook de uitvoer van een functie met 0 of meer parameters toewijzen aan een variabele.  Elke parameter van de functie kan worden opgegeven met een letterlijke waarde of een variabele.  Als de functie geen uitvoer retourneert, wordt de toewijzing weggelaten.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Een variabele wordt opgegeven met een naam-id die begint met een letter en bestaat alleen uit letters (A-Z), cijfers (0-9) en het onderstrepingsteken (\_).  Het type impliciet is afgeleid van de letterlijke waarde of -functie uitvoer waarde toegewezen. 

Hieronder volgt een lijst van ondersteunde gegevenstypen:

|Type|Beschrijving|Voorbeelden|
|----|----|----|
|Reeks|Reeks 0 of meer tekens bevatten|"Hello World!"<br/>""|
|BOOL|Booleaanse waarde|true<br/>false|
|Int32|32-bits geheel getal met teken.  -2.1e9-2.1e9|123<br/>-321|
|Int64|64-bits geheel getal met teken. -9.2e18 en 9.2e18|9876543210|
|Double|Dubbele precisie met drijvende komma. 1.7E +/-308 (15 cijfers)|123.456789<br/>1.23456789e2|
|GUID|Globaal unieke id|'602DD052-CC47-4B23-A16A-26B52D30C05B'|
|Query’s uitvoeren|Query-expressie waarmee een subset van gegevensobjecten in de index|All()<br/>En (*W1*, *k2*)|

<a name="semantic-functions"></a>
## <a name="semantic-functions"></a>Semantische functies
Er is een ingebouwde verzameling semantische functies.  Ze kunnen de constructie van geavanceerde query's en contextafhankelijke controle bieden over interpretatie voor de grammatica.

### <a name="and-function"></a>En de functie
`query = And(query1, query2);`

Retourneert een query bestaat uit het snijpunt van twee invoer query's.

### <a name="or-function"></a>Of functie
`query = Or(query1, query2);`

Retourneert een query bestaat uit de samenvoeging van twee invoer query's.

### <a name="all-function"></a>Alle functie
`query = All();`

Retourneert een query die alle gegevensobjecten bevat.

In het volgende voorbeeld gebruiken we de functie All() iteratief bouwen van een query op basis van het snijpunt van 1 of meer trefwoorden.

```
<tag>query = All();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = And(query, keyword);</tag>
</item>
```

### <a name="none-function"></a>Geen werken
`query = None();`

Retourneert een query die geen gegevensobjecten bevat.

In het volgende voorbeeld gebruiken we de functie None() iteratief bouwen van een query op basis van de samenvoeging van 1 of meer trefwoorden.

```
<tag>query = None();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = Or(query, keyword);</tag>
</item>
```

### <a name="query-function"></a>Queryfunctie
```
query = Query(attrName, value)
query = Query(attrName, value, op)
```

Retourneert een query die alleen gegevensobjecten bevat waarvan het kenmerk *attrName* overeenkomt met de waarde *waarde* volgens de opgegeven bewerking *op*, die standaard ingesteld op 'eq'.  Normaal gesproken gebruikt een `attrref` element voor het maken van een query op basis van de overeenkomende invoer voor de query-tekenreeks.  Als een waarde is opgegeven of andere wijze worden verkregen, kan de functie Query() kan worden gebruikt voor het maken van een query die overeenkomt met deze waarde.

In het volgende voorbeeld gebruiken we de Query()-functie voor het implementeren van ondersteuning voor het opgeven van academic publicaties vanuit een bepaalde tien jaar.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

<a name="composite-function"/>
### <a name="composite-function"></a>Samengestelde functie
`query = Composite(innerQuery);`

Retourneert een query die ingekapseld een *innerQuery* bestaat uit overeenkomsten met onderliggende kenmerken van een gemeenschappelijk samengestelde kenmerk *kenmerk*.  De inkapseling moet het samengestelde kenmerk *%{attr/* van alle overeenkomende gegevensobject hebben van ten minste één waarde die afzonderlijk voldoet aan de *innerQuery*.  Houd er rekening mee dat er een query op onderliggende kenmerken van een samengesteld kenmerk heeft te integreren met behulp van de functie Composite() voordat deze kan worden gecombineerd met andere query's.

Bijvoorbeeld retourneert de volgende query academic publicaties door 'harry shum' terwijl hij op 'microsoft':
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

De volgende query retourneert aan de andere kant academic publicaties waarin een van de auteurs is 'harry shum' en een van de verwantschappen 'microsoft':
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>De functie GetVariable
`value = GetVariable(name, scope);`

Retourneert de waarde van variabele *naam* gedefinieerd onder de opgegeven *bereik*.  *naam* is een id die begint met een letter en bestaat alleen uit letters (A-Z), cijfers (0-9) en het onderstrepingsteken (_).  *bereik* kan worden ingesteld op 'aanvraag' of 'systeem'.  Houd er rekening mee dat variabelen gedefinieerd onder verschillende bereiken van elkaar verschillen zijn, inclusief gedefinieerd via de uitvoer van de semantische functies.

Aanvraag bereik variabelen worden gedeeld door alle interpretaties binnen de huidige interpret-aanvraag.  Ze kunnen worden gebruikt voor het zoeken naar interpretatie voor de controle over de grammatica.

Systeemvariabelen zijn vooraf gedefinieerd door de service en kunnen worden gebruikt voor het ophalen van verschillende statistische gegevens over de huidige status van het systeem.  Hieronder volgt de reeks momenteel ondersteunde systeemvariabelen:

|Naam|Type|Beschrijving|
|----|----|----|
|IsAtEndOfQuery|BOOL|waar als de huidige interpretatie heeft komt overeen met alle tekst in de invoer voor de query|
|IsBeyondEndOfQuery|BOOL|waar als de huidige interpretatie voltooiingen afgezien van de tekst van de invoer voor de query heeft voorgesteld|

### <a name="setvariable-function"></a>De functie SetVariable
`SetVariable(name, value, scope);`

Wijst *waarde* aan de variabele *naam* onder het opgegeven *bereik*.  *naam* is een id die begint met een letter en bestaat alleen uit letters (A-Z), cijfers (0-9) en het onderstrepingsteken (_).  Op dit moment de enige geldige waarde voor *bereik* is 'vragen'.  Er zijn geen variabelen worden ingesteld.

Aanvraag bereik variabelen worden gedeeld door alle interpretaties binnen de huidige interpret-aanvraag.  Ze kunnen worden gebruikt voor het zoeken naar interpretatie voor de controle over de grammatica.

### <a name="assertequals-function"></a>De functie AssertEquals
`AssertEquals(value1, value2);`

Als *value1* en *value2* zijn gelijkwaardig, is de functie kan worden uitgevoerd en heeft geen bijwerkingen.  Anders wordt de functie mislukt en wordt de interpretatie geweigerd.

### <a name="assertnotequals-function"></a>De functie AssertNotEquals
`AssertNotEquals(value1, value2);`

Als *value1* en *value2* niet equivalent zijn, de functie kan worden uitgevoerd en heeft geen bijwerkingen zijn.  Anders wordt de functie mislukt en wordt de interpretatie geweigerd.



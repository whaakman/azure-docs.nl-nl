---
title: Grammatica-indeling in de Knowledge Exploration Service API | Microsoft Docs
description: Meer informatie over de grammatica-indeling in de Knowledge Exploration Service KES () API in Cognitive Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: b64025be2f5a9708162da475c1f037d7f253d2c6
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865750"
---
# <a name="grammar-format"></a>Grammatica-indeling
De grammatica is een XML-bestand dat Hiermee geeft u de gewogen set query's in natuurlijke taal die de service interpreteren kan, en hoe deze query's in natuurlijke taal worden vertaald in functionaliteit voor semantische query-expressies.  De syntaxis van de grammatica is gebaseerd op [SRGS](http://www.w3.org/TR/speech-grammar/), een W3C-standaard voor spraakherkenning grammatica's, met de extensies voor de ondersteuning van index gegevensintegratie en semantische functies.

De volgende beschrijving van de syntactische elementen die kunnen worden gebruikt in een grammatica.  Zie [in dit voorbeeld](#example) voor een volledige grammatica waarmee ziet u het gebruik van deze elementen in de context.

### <a name="grammar-element"></a>grammatica-Element 
De `grammar` -element is het element op het hoogste niveau in de grammatica-specificatie XML.  De vereiste `root` kenmerk Hiermee geeft u de naam van de regel van de hoofdmap waarin het beginpunt van de grammatica.

```xml
<grammar root="GetPapers">
```

### <a name="import-element"></a>Element importeren
De `import` element importeert de schemadefinitie van een uit een extern bestand om in te schakelen kenmerk verwijzingen. Het element moet een onderliggend element van het hoogste niveau `grammar` element en worden weergegeven voor alle `attrref` elementen. De vereiste `schema` kenmerk geeft de naam van een schemabestand zich in dezelfde map als de grammatica XML-bestand. De vereiste `name` element Hiermee geeft u het schema alias dat volgende `attrref` elementen gebruiken wanneer u verwijst naar kenmerken die zijn gedefinieerd in dit schema.

```xml
  <import schema="academic.schema" name="academic"/>
```

### <a name="rule-element"></a>regel Element
De `rule` element wordt gedefinieerd voor een grammatica-regel, een structurele eenheid waarmee een set van query-expressies die het systeem kan worden geïnterpreteerd.  Het element moet een onderliggend element van het hoogste niveau `grammar` element.  De vereiste `id` kenmerk geeft de naam van de regel, waarnaar wordt verwezen vanuit `grammar` of `ruleref` elementen.

Een `rule` element wordt gedefinieerd voor een set met juridische uitbreidingen.  Tekst tokens vergelijken rechtstreeks met de invoer voor de query.  `item` elementen herhalingen opgeven en alter interpretatie kansen.  `one-of` elementen duiden op andere opties.  `ruleref` elementen inschakelen opbouw van complexere uitbreidingen van eenvoudiger namen.  `attrref` elementen kunnen overeenkomsten op basis van de kenmerkwaarden van de index.  `tag` elementen de semantiek van de interpretatie opgeven en de kans op interpretatie kunnen wijzigen.

```xml
<rule id="GetPapers">...</rule>
```

### <a name="example-element"></a>Voorbeeld van Element
De optionele `example` element Hiermee geeft u de voorbeeld-zinnen die kunnen worden geaccepteerd door de `rule` definitie.  Dit kan worden gebruikt voor documentatie en/of automatisch testen.

```xml
<example>papers about machine learning by michael jordan</example>
```

### <a name="item-element"></a>item Element
De `item` element een reeks grammatica constructies groepen.  Het kan worden gebruikt om aan te geven meerdere kopieën van de uitbreiding van de takenreeks of om alternatieven in combinatie met de `one-of` element.

Wanneer een `item` element is geen onderliggende site van een `one-of` -element, kunt deze herhaling van de ingesloten reeks opgeven door toe te wijzen de `repeat` kenmerk aan een waarde van het aantal.  De waarde van een aantal van '*n*' (waarbij *n* is een geheel getal) geeft aan dat de takenreeks precies moet plaatsvinden *n* tijden.  De waarde van een aantal van '*m*-*n*' kunt u de volgorde worden weergegeven tussen *m* en *n* time-out, liggen.  De waarde van een aantal van '*m*-' geeft aan dat de takenreeks moet worden ten minste weergegeven *m* tijden.  De optionele `repeat-logprob` kenmerk kan worden gebruikt om te wijzigen van de waarschijnlijkheid interpretatie voor elke extra herhaling dan het minimum.

```xml
<item repeat="1-" repeat-logprob="-10">...</item>
```

Wanneer `item` elementen verschijnen als onderliggende items van een `one-of` -element, ze de set met uitbreiding alternatieven definiëren.  In dit gebruik, de optionele `logprob` kenmerk bevat de kans dat relatieve log tussen de verschillende mogelijkheden.  De kans gegeven *p* tussen 0 en 1, de bijbehorende logboekbestanden kans wordt berekend als het logboek (*p*), waarbij log() staat voor de functie natuurlijke logboek.  Indien niet opgegeven, `logprob` standaard ingesteld op 0, waardoor de kans op interpretatie niet gewijzigd.  Houd er rekening mee log kans is altijd een negatieve waarde van de drijvende komma of 0.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="one-of-element"></a>Eén-van-Element
De `one-of` element Hiermee geeft u alternatieve uitbreidingen tussen een van de onderliggende `item` elementen.  Alleen `item` elementen kunnen worden weergegeven in een `one-of` element.  Relatieve kansen tussen de verschillende opties kunnen worden opgegeven de `logprob` kenmerk in elke onderliggende `item`.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="ruleref-element"></a>ruleref Element
De `ruleref` element Hiermee geeft u geldige uitbreidingen via verwijzingen naar een andere `rule` element.  Via het gebruik van `ruleref` elementen, meer complexe expressies kunnen worden gebouwd van eenvoudiger regels.  De vereiste `uri` kenmerk geeft aan dat de naam van de waarnaar wordt verwezen `rule` met behulp van de syntaxis ' #*rulename*'.  Voor het vastleggen van de semantische uitvoer van de regel waarnaar wordt verwezen, gebruik het optionele `name` kenmerk om op te geven van de naam van een variabele die de functionaliteit voor semantische uitvoer is toegewezen.
 
```xml
<ruleref uri="#GetPaperYear" name="year"/>
```

### <a name="attrref-element"></a>attrref Element
De `attrref` element verwijst naar een indexkenmerk, zodat die overeenkomt met tegen kenmerkwaarden waargenomen in de index.  De vereiste `uri` kenmerk geeft de indexnaam schema en de naam van het kenmerk met de syntaxis '*schemaName*#*attrName*'.  Er moet een voorgaande `import` element dat de invoer van het schema met de naam *schemaName*.  Naam van het kenmerk is de naam van een kenmerk in het bijbehorende schema is gedefinieerd.

Naast de invoer van de gebruiker, die overeenkomt met de `attrref` element retourneert ook een gestructureerde query-object als uitvoer dat de subset met objecten in de index die overeenkomt met de invoerwaarde wordt geselecteerd.  Gebruik het optionele `name` kenmerk om op te geven van de naam van de variabele waarin de uitvoer van de query-object moet worden opgeslagen.  Het queryobject samengesteld kan zijn, aan andere queryobjecten om te vormen van meer complexe expressies.  Zie [semantische interpretatie](SemanticInterpretation.md) voor meer informatie.  

```xml
<attrref uri="academic#Keyword" name="keyword"/>
```

#### <a name="query-completion"></a>Query is voltooid 
Ter ondersteuning van query's bij het interpreteren van gedeeltelijke gebruikersquery's, moet elk kenmerk waarnaar wordt verwezen als een bewerking in de schemadefinitie van 'starts_with' bevatten.  Een voorvoegsel van de query gebruiker opgegeven `attrref` komen overeen met alle waarden in de index die door het voorvoegsel worden voltooid, en elke volledige waarde opleveren als een afzonderlijke interpretatie van de grammatica.  

Voorbeelden:
* Overeenkomende `<attrref uri="academic#Keyword" name="keyword"/>` op basis van de query voorvoegsel 'dat' genereert een interpretatie voor artikelen over 'database', een interpretatie voor artikelen over 'data-gegevensanalyse', enzovoort.
* Overeenkomende `<attrref uri="academic#Year" name="year"/>` op basis van de query voorvoegsel '200' genereert een interpretatie voor documenten in '2000', een interpretatie voor documenten in '2001', enzovoort.

#### <a name="matching-operations"></a>Overeenkomende bewerkingen
Naast de exacte overeenkomst Selecteer kenmerken van het type ook ondersteuning voor voorvoegsel en ongelijkheid komt overeen met via de optionele `op` kenmerk.  Als er geen object in de index een waarde die overeenkomt met heeft, het pad van de grammatica is geblokkeerd en de service wordt niet gegenereerd voor een perfecte ervaring bij het doorlopen van via dit pad grammatica.   De `op` standaardinstellingen van het kenmerk op 'eq'.

```xml
in <attrref uri="academic#Year" name="year"/>
before <attrref uri="academic#Year" op="lt" name="year"/
```

De volgende tabel bevat de ondersteunde `op` waarden voor elk kenmerktype.  Het gebruik ervan is vereist voor de bijbehorende indexbewerking moet worden opgenomen in de definitie van de schema-kenmerk.

| Kenmerktype | Op waarde | Beschrijving | Indexbewerking
|----|----|----|----|
| Reeks | eq | Exacte tekenreeksovereenkomst | is gelijk aan |
| Reeks | starts_with | Voorvoegsel tekenreeksovereenkomst | starts_with |
| Double-waarde voor Int32, Int64, | eq |  Numerieke gelijkheid | is gelijk aan |
| Double-waarde voor Int32, Int64, | lt, le, gt, ge | Numerieke ongelijkheid overeenkomst (<, < =, >, > =) | is_between |
| Double-waarde voor Int32, Int64, | starts_with | Voorvoegselovereenkomst van waarde in decimale notatie | starts_with |

Voorbeelden:
* `<attrref uri="academic#Year" op="lt" name="year"/>` komt overeen met de ingevoerde tekenreeks '2000' en alle documenten raadplegen die zijn gepubliceerd voordat het jaar 2000 uitsluitend retourneert.
* `<attrref uri="academic#Year" op="lt" name="year"/>` komt niet overeen met de invoer tekenreeks "20" omdat er geen documenten in de index die is gepubliceerd voordat het jaar 20.
* `<attrref uri="academic#Keyword" op="starts_with" name="keyword"/>` komt overeen met de ingevoerde tekenreeks 'dat' en retourneert in een enkele interpretatie documenten over 'database', 'datamining', enz.  Dit is een zeldzame use case.
* `<attrref uri="academic#Year" op="starts_with" name="year"/>` komt overeen met de ingevoerde tekenreeks "20" en wordt geretourneerd in een enkele interpretatie-documenten die zijn gepubliceerd in 200 299, 2000-2999, enzovoort.  Dit is een zeldzame use case.

### <a name="tag-element"></a>code-Element
De `tag` element geeft aan hoe een pad via de grammatica is om te worden geïnterpreteerd.  Het bevat een reeks instructies puntkomma is beëindigd.  Een instructie is mogelijk een toewijzing van een letterlijke tekenreeks of een variabele naar een andere variabele.  Het kan ook de uitvoer van een functie met een 0 of meer parameters toewijzen aan een variabele.  Elke parameter van de functie kan worden opgegeven met een letterlijke tekenreeks of een variabele.  Als de functie geen uitvoer retourneert, wordt de toewijzing wordt weggelaten.  Bereik van een variabele is lokaal op de betreffende regel.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Elke `rule` in de grammatica heeft met een vooraf gedefinieerde variabele met de naam 'uit', de semantische uitvoer van de regel.  De waarde wordt berekend door het evalueren van elk van de semantische overzichten algemeen wordt gebruikt door het pad via de `rule` invoer van de gebruiker te vergelijken met query.  De waarde die is toegewezen aan de variabele 'uit' aan het einde van de evaluatie is de functionaliteit voor semantische uitvoer van de regel.  De semantische uitvoer van het interpreteren van de gebruikersquery van een op de grammatica is de functionaliteit voor semantische uitvoer van de basis-regel.

Sommige instructies kunnen de kans op een pad interpretatie wijzigen door de introductie van een kans additieve log offset.  Sommige instructies kunnen de interpretatie afwijzen helemaal als opgegeven voorwaarden niet is voldaan.

Zie voor een lijst van ondersteunde semantische functies, [semantische functies](SemanticInterpretation.md#semantic-functions).

## <a name="interpretation-probability"></a>Interpretatie waarschijnlijkheid
De kans op een pad interpretatie via de grammatica is de kans op cumulatieve logboek van alle de `<item>` -elementen en functionaliteit voor semantische functies weg aangetroffen.  Hierin wordt beschreven voor het relatieve risico op die overeenkomt met een bepaalde volgorde van de invoer.

De kans gegeven *p* tussen 0 en 1, de bijbehorende logboekbestanden kans wordt berekend als het logboek (*p*), waarbij log() staat voor de functie natuurlijke logboek.  Met behulp van log kansen, kan het systeem worden verzameld van de gecombineerde waarschijnlijkheid van een pad interpretatie via eenvoudige optelsom.  Ook wordt met drijvende komma negatieve overloop gebruikelijk om dergelijke gecombineerde waarschijnlijkheid-berekeningen te voorkomen.  Houd er rekening mee dat standaard de kans op log altijd is een negatieve waarde van de drijvende komma of 0, waarbij hogere waarden geven voor hogere kans.

<a name="example"></a>
## <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld XML uit het domein academische publicaties waarin de verschillende elementen van een grammatica wordt gedemonstreerd:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="academic.schema" name="academic"/>
  
  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>
    
    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>
  
    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>
        
      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>
        
        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>
        
        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>
  
  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="lt" name="year"/></item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="gt" name="year"/></item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```

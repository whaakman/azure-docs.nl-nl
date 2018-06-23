---
title: De grammatica-indeling in kennis exploratie Service API | Microsoft Docs
description: Meer informatie over de grammatica-indeling in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 27202379b8c36696a380049336229cac040b0108
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344574"
---
# <a name="grammar-format"></a>De grammatica-indeling
De grammatica is een XML-bestand waarin de gewogen set van natuurlijke taal query's die de service kan worden geïnterpreteerd, en hoe deze query's van natuurlijke taal worden vertaald in semantische query-expressies.  De syntaxis van de grammatica is gebaseerd op [SRGS](http://www.w3.org/TR/speech-grammar/), een W3C standard voor spraak erkenning grammatica's, met extensies die ondersteuning bieden voor index gegevensintegratie en semantische functies.

De volgende beschrijving van elk van de syntactische elementen die kunnen worden gebruikt in een grammatica.  Zie [in dit voorbeeld](#example) voor een volledige grammatica over het gebruik van deze elementen in de context.

### <a name="grammar-element"></a>grammatica Element 
De `grammar` -element is het element op het hoogste niveau in de grammatica-specificatie XML.  De vereiste `root` kenmerk geeft de naam van de basis-regel die het beginpunt van de grammatica definieert.

```xml
<grammar root="GetPapers">
```

### <a name="import-element"></a>Element importeren
De `import` element schemadefinitie importeert uit een extern bestand kenmerk verwijzingen inschakelen. Het element moet een onderliggend element van het hoogste niveau `grammar` element en worden weergegeven voordat een `attrref` elementen. De vereiste `schema` kenmerk geeft de naam van een schemabestand zich in dezelfde map als het bestand van de XML-grammatica. De vereiste `name` element Hiermee geeft u het schema dat alias daaropvolgende `attrref` elementen gebruiken wanneer u verwijst naar de kenmerken die zijn gedefinieerd in dit schema.

```xml
  <import schema="academic.schema" name="academic"/>
```

### <a name="rule-element"></a>regel Element
De `rule` element een grammaticaregel, een structurele eenheid waarmee een reeks query-expressies die kan worden geïnterpreteerd door het systeem wordt gedefinieerd.  Het element moet een onderliggend element van het hoogste niveau `grammar` element.  De vereiste `id` kenmerk geeft de naam van de regel, waarnaar wordt verwezen vanaf `grammar` of `ruleref` elementen.

Een `rule` element definieert een aantal juridische uitbreidingen.  Tekst tokens overeenkomst rechtstreeks met de invoer voor de query.  `item` elementen Geef wordt herhaald en interpretatie kansen alter.  `one-of` alternatieve mogelijkheden wijzen.  `ruleref` elementen inschakelen constructie van complexere uitbreidingen van eenvoudiger codes.  `attrref` elementen toestaan overeenkomsten met kenmerkwaarden van de index.  `tag` elementen Geef de semantiek van de interpretatie en de kans interpretatie kunnen wijzigen.

```xml
<rule id="GetPapers">...</rule>
```

### <a name="example-element"></a>Voorbeeld van Element
De optionele `example` element geeft voorbeeld zinnen die kunnen worden geaccepteerd door de `rule` definitie.  Dit kan worden gebruikt voor documentatie en/of automatisch testen.

```xml
<example>papers about machine learning by michael jordan</example>
```

### <a name="item-element"></a>item Element
De `item` element een reeks grammatica constructies groepen.  Kan worden gebruikt om aan te geven herhalingen van de uitbreiding van de takenreeks of om alternatieven in combinatie met de `one-of` element.

Wanneer een `item` element is geen onderliggend element van een `one-of` element, het herhaling van de ingesloten sequence kunt opgeven door toe te wijzen de `repeat` kenmerk voor de waarde van een aantal.  De waarde van een aantal van '*n*' (waarbij *n* is een geheel getal) geeft aan dat de reeks precies moet plaatsvinden *n* tijden.  De waarde van een aantal van '*m*-*n*' kunt u de volgorde worden weergegeven tussen *m* en *n* time-out liggen.  De waarde van een aantal van '*m*-' Hiermee bepaalt u dat de reeks moet ten minste *m* tijden.  De optionele `repeat-logprob` kenmerk kan worden gebruikt voor het wijzigen van de waarschijnlijkheid interpretatie voor elke extra herhaling dan het minimum.

```xml
<item repeat="1-" repeat-logprob="-10">...</item>
```

Wanneer `item` elementen worden weergegeven als onderliggende elementen van een `one-of` element ze instelt voor uitbreiding van alternatieven.  In dit gebruik, de optionele `logprob` kenmerk geeft u de kans relatieve logboek tussen de verschillende mogelijkheden.  Gezien een kans *p* tussen 0 en 1, de bijbehorende logboekbestanden kans worden berekend als logboek (*p*), waarbij log() is de functie natuurlijke log.  Als niet wordt opgegeven, `logprob` standaard ingesteld op 0, wat de kans interpretatie niet gewijzigd.  Houd er rekening mee logboek kans is altijd een negatieve waarde van de drijvende komma of 0.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="one-of-element"></a>een-element
De `one-of` element geeft alternatieve uitbreidingen tussen een van de onderliggende `item` elementen.  Alleen `item` elementen kunnen voorkomen binnen een `one-of` element.  Relatieve kansen tussen de verschillende opties kunnen worden opgegeven de `logprob` kenmerk in elke onderliggende `item`.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="ruleref-element"></a>ruleref Element
De `ruleref` element geeft geldig uitbreidingen via verwijzingen naar een andere `rule` element.  Met behulp van `ruleref` elementen, complexe expressies kunnen worden gebouwd van eenvoudiger regels.  De vereiste `uri` kenmerk geeft de naam van de waarnaar wordt verwezen `rule` met de syntaxis ' #*rulename*'.  Voor het vastleggen van de semantische uitvoer van de regel waarnaar wordt verwezen, gebruik het optionele `name` het kenmerk met de naam opgeven van een variabele waaraan de uitvoer van de semantische is toegewezen.
 
```xml
<ruleref uri="#GetPaperYear" name="year"/>
```

### <a name="attrref-element"></a>attrref Element
De `attrref` element verwijst naar een indexkenmerk toe die overeenkomt met tegen kenmerkwaarden waargenomen in de index.  De vereiste `uri` kenmerk geeft de indexnaam schema en de naam van het kenmerk met de syntaxis '*schemaName*#*attrName*'.  Er moet een voorgaande `import` element dat de invoer van het schema met de naam *schemaName*.  Naam van het kenmerk is de naam van een kenmerk dat is gedefinieerd in het bijbehorende schema.

Naast de invoer van gebruiker, die overeenkomt met de `attrref` element retourneert ook een structured query-object als uitvoer de subset met objecten in de index die overeenkomt met de invoerwaarde selecteren.  Gebruik het optionele `name` het kenmerk met de naam opgeven van de variabele waarin de uitvoer van de query-object moet worden opgeslagen.  Het queryobject samengesteld kan zijn, met andere objecten query om meer complexe expressies.  Zie [semantische interpretatie](SemanticInterpretation.md) voor meer informatie.  

```xml
<attrref uri="academic#Keyword" name="keyword"/>
```

#### <a name="query-completion"></a>Query is voltooid 
Ter ondersteuning van query voltooiingen bij het interpreteren van gedeeltelijke gebruikersquery, moet elk kenmerk waarnaar wordt verwezen als een bewerking in de schemadefinitie 'starts_with' bevatten.  Een voorvoegsel van de query gebruiker opgegeven `attrref` overeenkomen met alle waarden in de index die door het voorvoegsel voltooid en resulteert in elke volledige waarde als een afzonderlijke interpretatie van de grammatica.  

Voorbeelden:
* Overeenkomende `<attrref uri="academic#Keyword" name="keyword"/>` op basis van de query voorvoegsel 'dat' genereert een interpretatie voor artikelen over 'database', één interpretatie voor artikelen over 'datamining', enzovoort.
* Overeenkomende `<attrref uri="academic#Year" name="year"/>` op basis van de query voorvoegsel "200" genereert een interpretatie Papers in '2000' Eén interpretatie voor documenten in '2001', enz.

#### <a name="matching-operations"></a>Overeenkomende bewerkingen
Naast de exacte overeenkomst Selecteer kenmerken van het type ook ondersteuning voor voorvoegsel en ongelijk komt overeen met via de optionele `op` kenmerk.  Als er geen object in de index een waarde die overeenkomt met heeft, het pad van de grammatica is geblokkeerd en de service interpretaties via dit pad grammatica passeert geen genereert.   De `op` standaardwaarden kenmerk 'eq'.

```xml
in <attrref uri="academic#Year" name="year"/>
before <attrref uri="academic#Year" op="lt" name="year"/
```

De volgende tabel bevat de ondersteunde `op` waarden voor elk kenmerktype.  Het gebruik ervan vereist de bijbehorende indexbewerking moet worden opgenomen in de definitie van de schema-kenmerk.

| Kenmerktype | De waarde op | Beschrijving | Index-bewerking
|----|----|----|----|
| Reeks | eq | Tekenreeks exact overeenkomen | is gelijk aan |
| Reeks | starts_with | Voorvoegsel tekenreeksovereenkomst | starts_with |
| Int32, Int64, Double | eq |  Numerieke gelijkheid | is gelijk aan |
| Int32, Int64, Double | lt, RP, gt, ge | Numerieke ongelijk overeen (<, < =, >, > =) | is_between |
| Int32, Int64, Double | starts_with | Het voorvoegsel dat moet overeenkomen met de waarde in decimale notatie | starts_with |

Voorbeelden:
* `<attrref uri="academic#Year" op="lt" name="year"/>` komt overeen met de invoerreeks '2000' en alle documenten die zijn uitgegeven voordat het jaar 2000 uitsluitend retourneert.
* `<attrref uri="academic#Year" op="lt" name="year"/>` komt niet overeen met de invoer tekenreeks '20' omdat er geen documenten in de index gepubliceerd voordat het jaar 20.
* `<attrref uri="academic#Keyword" op="starts_with" name="keyword"/>` komt overeen met de invoerreeks 'dat' en retourneert in een enkel interpretatie artikelen over 'database', 'datamining', enzovoort.  Dit is een zeldzame gebruiksvoorbeeld.
* `<attrref uri="academic#Year" op="starts_with" name="year"/>` komt overeen met de invoerreeks '20' en het rendement in een enkel interpretatie documenten die zijn gepubliceerd in 200 299, 2000-2999, enz.  Dit is een zeldzame gebruiksvoorbeeld.

### <a name="tag-element"></a>tag Element
De `tag` element geeft aan hoe een pad via de grammatica wordt moeten worden geïnterpreteerd.  Het bevat een reeks instructies puntkomma is beëindigd.  Een instructie is mogelijk een toewijzing van een letterlijke waarde of een variabele aan een andere variabele.  Het kan ook de uitvoer van een functie met 0 of meer parameters toewijzen aan een variabele.  Elke parameter van de functie kan worden opgegeven met een letterlijke waarde of een variabele.  Als de functie geen uitvoer retourneert, wordt de toewijzing weggelaten.  Variabelenbereik is lokaal op de regel met.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Elke `rule` in de grammatica heeft met een vooraf gedefinieerde variabele met de naam 'out' de semantische uitvoer van de regel.  De waarde wordt berekend door de evaluatie van elk van de semantische instructies doorkruist door het pad via de `rule` die overeenkomt met de gebruiker invoer een query.  De waarde die is toegewezen aan de variabele 'out' aan het einde van de evaluatie is de semantische uitvoer van de regel.  De semantische uitvoer van een gebruikersquery op de grammatica interpreteren is de semantische uitvoer van de Hoofdregel.

Sommige instructies kunnen de kans op een pad interpretatie wijzigen door de introductie van een offset van de kans additieve logboek.  Sommige instructies kunnen de interpretatie afwijzen helemaal als opgegeven voorwaarden niet is voldaan.

Zie voor een lijst van ondersteunde functies van de semantische [semantische functies](SemanticInterpretation.md#semantic-functions).

## <a name="interpretation-probability"></a>Interpretatie kans
De kans van een pad interpretatie via de grammatica is de kans cumulatieve logboek van alle de `<item>` elementen en semantische functies langs de manier waarop aangetroffen.  Beschrijft de relatieve kans op die overeenkomt met een bepaalde volgorde van de invoer.

Een waarschijnlijkheid gegeven *p* tussen 0 en 1, de bijbehorende logboekbestanden kans worden berekend als logboek (*p*), waarbij log() de functie natuurlijke log.  Met behulp van logboek kansen kan het systeem worden verzameld van de gemeenschappelijke waarschijnlijkheid van een pad interpretatie via eenvoudige toevoegen.  Dit voorkomt ook drijvende-negatieve overloop voor dergelijke gemeenschappelijke op een loterij lijken.  Houd er rekening mee dat door het ontwerp van de kans op logboek altijd is een negatieve drijvende-kommawaarde of 0, waarbij hogere waarden voor de grotere kans geven.

<a name="example"></a>
## <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld XML uit het domein academic publicaties waarin de verschillende elementen van een grammatica wordt gedemonstreerd:

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

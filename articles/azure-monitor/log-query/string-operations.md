---
title: Werken met tekenreeksen in Logboeken-query's van Azure Monitor | Microsoft Docs
description: Beschrijft hoe u bewerken, vergelijken, te zoeken en tal van andere bewerkingen op tekenreeksen uitvoeren in Azure Monitor logboeken-query's.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 4b2763629a3036551cb3d362e609c72737436f4a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58012231"
---
# <a name="work-with-strings-in-azure-monitor-log-queries"></a>Werken met tekenreeksen in Logboeken-query's van Azure Monitor


> [!NOTE]
> U moet voltooien [aan de slag met Azure Monitor Log-Analytics](get-started-portal.md) en [aan de slag met Azure Monitor logboeken-query's](get-started-queries.md) voordat het voltooien van deze zelfstudie.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

In dit artikel wordt beschreven hoe u bewerken, vergelijken, te zoeken en tal van andere bewerkingen op tekenreeksen uitvoeren.

Elk teken in een tekenreeks heeft een indexnummer op basis van de locatie. Het eerste teken is bij index 0, het volgende teken is 1, en dus een. Indexnummers verschillende tekenreeks-functies gebruiken zoals wordt weergegeven in de volgende secties. Veel van de volgende voorbeelden gebruiken de **afdrukken** voor de opdracht om te demonstreren manipuleren van tekenreeksen zonder gebruik van een specifieke gegevensbron.


## <a name="strings-and-escaping-them"></a>Tekenreeksen en aanhalingstekens ze
Tekenreekswaarden zijn verpakt met ofwel met enkele of dubbele aanhalingstekens. Backslash (\) wordt gebruikt voor het escape-tekens op het teken, zoals \t voor tabblad \n voor nieuwe regel, volgen en \" het aanhalingsteken zelf.

```Kusto
print "this is a 'string' literal in double \" quotes"
```

Om te voorkomen dat '\\'die fungeert als een escape-teken, toevoegen'\@' als voorvoegsel van de tekenreeks:

```Kusto
print @"C:\backslash\not\escaped\with @ prefix"
```


## <a name="string-comparisons"></a>Tekenreeksvergelijkingen

Operator       |Description                         |Hoofdlettergevoelig|Voorbeeld (levert `true`)
---------------|------------------------------------|--------------|-----------------------
`==`           |Is gelijk aan                              |Ja           |`"aBc" == "aBc"`
`!=`           |Niet gelijk aan                          |Ja           |`"abc" != "ABC"`
`=~`           |Is gelijk aan                              |Nee            |`"abc" =~ "ABC"`
`!~`           |Niet gelijk aan                          |Nee            |`"aBc" !~ "xyz"`
`has`          |Rechts naast is een hele term in links naast |Nee|`"North America" has "america"`
`!has`         |Rechts naast is niet een volledige term in links naast       |Nee            |`"North America" !has "amer"` 
`has_cs`       |Rechts naast is een hele term in links naast |Ja|`"North America" has_cs "America"`
`!has_cs`      |Rechts naast is niet een volledige term in links naast       |Ja            |`"North America" !has_cs "amer"` 
`hasprefix`    |Rechts naast is een term-voorvoegsel in de links aan         |Nee            |`"North America" hasprefix "ame"`
`!hasprefix`   |Rechts naast is niet een term-voorvoegsel in de links aan     |Nee            |`"North America" !hasprefix "mer"` 
`hasprefix_cs`    |Rechts naast is een term-voorvoegsel in de links aan         |Ja            |`"North America" hasprefix_cs "Ame"`
`!hasprefix_cs`   |Rechts naast is niet een term-voorvoegsel in de links aan     |Ja            |`"North America" !hasprefix_cs "CA"` 
`hassuffix`    |Rechts naast is een term-achtervoegsel in de links aan         |Nee            |`"North America" hassuffix "ica"`
`!hassuffix`   |Rechts naast is niet een term-achtervoegsel in de links aan     |Nee            |`"North America" !hassuffix "americ"`
`hassuffix_cs`    |Rechts naast is een term-achtervoegsel in de links aan         |Ja            |`"North America" hassuffix_cs "ica"`
`!hassuffix_cs`   |Rechts naast is niet een term-achtervoegsel in de links aan     |Ja            |`"North America" !hassuffix_cs "icA"`
`contains`     |Rechts naast treedt op als een subsequence van links zijde  |Nee            |`"FabriKam" contains "BRik"`
`!contains`    |Rechts naast niet wordt uitgevoerd in de links aan           |Nee            |`"Fabrikam" !contains "xyz"`
`contains_cs`   |Rechts naast treedt op als een subsequence van links zijde  |Ja           |`"FabriKam" contains_cs "Kam"`
`!contains_cs`  |Rechts naast niet wordt uitgevoerd in de links aan           |Ja           |`"Fabrikam" !contains_cs "Kam"`
`startswith`   |Rechts naast is een eerste subsequence van links zijde|Nee            |`"Fabrikam" startswith "fab"`
`!startswith`  |Rechts naast is niet een initiële subsequence van links zijde|Nee        |`"Fabrikam" !startswith "kam"`
`startswith_cs`   |Rechts naast is een eerste subsequence van links zijde|Ja            |`"Fabrikam" startswith_cs "Fab"`
`!startswith_cs`  |Rechts naast is niet een initiële subsequence van links zijde|Ja        |`"Fabrikam" !startswith_cs "fab"`
`endswith`     |Rechts naast is een subsequence afsluiting van de links aan|Nee             |`"Fabrikam" endswith "Kam"`
`!endswith`    |Rechts naast is niet een subsequence afsluiting van de links aan|Nee         |`"Fabrikam" !endswith "brik"`
`endswith_cs`     |Rechts naast is een subsequence afsluiting van de links aan|Ja             |`"Fabrikam" endswith "Kam"`
`!endswith_cs`    |Rechts naast is niet een subsequence afsluiting van de links aan|Ja         |`"Fabrikam" !endswith "brik"`
`matches regex`|links naast bevat een overeenkomst voor rechts zijde        |Ja           |`"Fabrikam" matches regex "b.*k"`
`in`           |Is gelijk aan op een van de elementen       |Ja           |`"abc" in ("123", "345", "abc")`
`!in`          |Niet gelijk is aan een van de elementen   |Ja           |`"bca" !in ("123", "345", "abc")`


## <a name="countof"></a>countof

Telt het aantal exemplaren van een subtekenreeks in een tekenreeks. Kan overeenkomen met gewone tekenreeksen of reguliere expressie. Eenvoudige tekenreeks komt overeen met overlappen op en komt overeen met reguliere expressie niet.

### <a name="syntax"></a>Syntaxis
```
countof(text, search [, kind])
```

### <a name="arguments"></a>Argumenten:
- `text` -De ingevoerde tekenreeks 
- `search` -Eenvoudige tekenreeks of reguliere expressie in om in tekst.
- `kind` - _normale_ | _regex_ (standaard: normaal).

### <a name="returns"></a>Retourneert

Het aantal keren dat de zoekreeks kan worden gekoppeld in de container. Eenvoudige tekenreeks komt overeen met overlappen op en komt overeen met reguliere expressie niet.

### <a name="examples"></a>Voorbeelden

#### <a name="plain-string-matches"></a>Eenvoudige tekenreeks komt overeen met

```Kusto
print countof("The cat sat on the mat", "at");  //result: 3
print countof("aaa", "a");  //result: 3
print countof("aaaa", "aa");  //result: 3 (not 2!)
print countof("ababa", "ab", "normal");  //result: 2
print countof("ababa", "aba");  //result: 2
```

#### <a name="regex-matches"></a>Komt overeen met reguliere expressie

```Kusto
print countof("The cat sat on the mat", @"\b.at\b", "regex");  //result: 3
print countof("ababa", "aba", "regex");  //result: 1
print countof("abcabc", "a.c", "regex");  // result: 2
```


## <a name="extract"></a>Uitpakken

Hiermee haalt u een overeenkomst is met een reguliere expressie uit een bepaalde tekenreeks. U kunt desgewenst ook zet de uitgepakte subtekenreeks het opgegeven type.

### <a name="syntax"></a>Syntaxis

```Kusto
extract(regex, captureGroup, text [, typeLiteral])
```

### <a name="arguments"></a>Argumenten

- `regex` -Een reguliere expressie.
- `captureGroup` -Er is een positief geheel getal zijn constante die wijzen op de capture-groep om op te halen. 0 voor de gehele overeenkomst, 1 voor de waarde die overeenkomt met de eerste '(' haakje')' in de reguliere expressie, 2 of meer voor latere tussen haakjes.
- `text` -Een tekenreeks te zoeken.
- `typeLiteral` -Een optionele type letterlijke waarde (bijvoorbeeld typeof(long)). Indien opgegeven, wordt de uitgepakte subtekenreeks wordt geconverteerd naar dit type.

### <a name="returns"></a>Retourneert
De subtekenreeks vergeleken met het vastleggen van de opgegeven groep captureGroup, wordt eventueel geconverteerd naar typeLiteral.
Als er geen overeenkomst, of het typeconversie is mislukt, null te retourneren.

### <a name="examples"></a>Voorbeelden

Het volgende voorbeeld haalt de laatste achttal werd van *computerip uit* uit een heartbeat-record:
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| project ComputerIP, last_octet=extract("([0-9]*$)", 1, ComputerIP) 
```

Het volgende voorbeeld haalt de laatste achttal werd, cast deze naar een *echte* Typ (getal) en de volgende IP-waarde wordt berekend
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| extend last_octet=extract("([0-9]*$)", 1, ComputerIP, typeof(real)) 
| extend next_ip=(last_octet+1)%255
| project ComputerIP, last_octet, next_ip
```

In het volgende voorbeeld wordt de tekenreeks *Trace* wordt gezocht naar een definitie van 'Duur'. De overeenkomst is geconverteerd naar *echte* en vermenigvuldigd met een constante (1 s) *die duur naar het type timespan cast*.
```Kusto
let Trace="A=12, B=34, Duration=567, ...";
print Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real));  //result: 567
print Duration_seconds =  extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s);  //result: 00:09:27
```


## <a name="isempty-isnotempty-notempty"></a>IsEmpty, isnotempty, notempty

- *IsEmpty* retourneert ' True ' als het argument een lege tekenreeks is of null (Zie ook *isnull*).
- *isnotempty* retourneert ' True ' als het argument is niet een lege tekenreeks of een null-waarde (Zie ook *isnotnull*). alias: *notempty*.

### <a name="syntax"></a>Syntaxis

```Kusto
isempty(value)
isnotempty(value)
```

### <a name="examples"></a>Voorbeelden

```Kusto
print isempty("");  // result: true

print isempty("0");  // result: false

print isempty(0);  // result: false

print isempty(5);  // result: false

Heartbeat | where isnotempty(ComputerIP) | take 1  // return 1 Heartbeat record in which ComputerIP isn't empty
```


## <a name="parseurl"></a>ParseURL

Splitst een URL in de delen (protocol, host, poort, enzovoort) en retourneert een object dictionary die de onderdelen als tekenreeksen.

### <a name="syntax"></a>Syntaxis

```
parseurl(urlstring)
```

### <a name="examples"></a>Voorbeelden

```Kusto
print parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")
```

Het resultaat is:
```
{
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
}
```


## <a name="replace"></a>vervangen

Alle overeenkomsten met reguliere expressie vervangen door een andere tekenreeks. 

### <a name="syntax"></a>Syntaxis

```
replace(regex, rewrite, input_text)
```

### <a name="arguments"></a>Argumenten

- `regex` -De reguliere expressie zodat deze overeenkomen met door. Groepen opnemen in '('haakjes')' kan bevatten.
- `rewrite` -De vervangende reguliere expressie voor elke overeenkomst op overeenkomende reguliere expressie. \0 gebruiken om te verwijzen naar de volledige overeenkomst, \1 voor de eerste capture-groep, \2, enzovoort voor groepen van de volgende vastleggen.
- `input_text` -De ingevoerde tekenreeks om in te zoeken.

### <a name="returns"></a>Retourneert
De tekst na het vervangen van alle resultaten van de reguliere expressie met evaluaties van herschrijven. Komt overeen met elkaar niet overlappen.

### <a name="examples"></a>Voorbeelden

```Kusto
SecurityEvent
| take 1
| project Activity 
| extend replaced = replace(@"(\d+) -", @"Activity ID \1: ", Activity) 
```

Kan hebben de volgende resultaten:

Activiteit                                        |vervangen
------------------------------------------------|----------------------------------------------------------
4663 - er is geprobeerd toegang tot een object  |Activiteits-ID 4663: Er is geprobeerd toegang tot een object.


## <a name="split"></a>split

Hiermee wordt een opgegeven tekenreeks op basis van een opgegeven scheidingsteken, en retourneert een matrix met de resulterende subtekenreeksen.

### <a name="syntax"></a>Syntaxis
```
split(source, delimiter [, requestedIndex])
```

### <a name="arguments"></a>Argumenten:

- `source` -De tekenreeks die moet worden verdeeld op basis van het opgegeven scheidingsteken.
- `delimiter` -Het scheidingsteken dat wordt gebruikt om de brontekenreeks te splitsen.
- `requestedIndex` -Een optioneel op nul gebaseerde index. Indien opgegeven, de geretourneerde tekenreeksmatrix alleen dat item bevatten (indien aanwezig).


### <a name="examples"></a>Voorbeelden

```Kusto
print split("aaa_bbb_ccc", "_");    // result: ["aaa","bbb","ccc"]
print split("aa_bb", "_");          // result: ["aa","bb"]
print split("aaa_bbb_ccc", "_", 1); // result: ["bbb"]
print split("", "_");               // result: [""]
print split("a__b", "_");           // result: ["a","","b"]
print split("aabbcc", "bb");        // result: ["aa","cc"]
```

## <a name="strcat"></a>strcat

Tekenreeksargumenten (argumenten ondersteunt 1-16) worden samengevoegd.

### <a name="syntax"></a>Syntaxis
```
strcat("string1", "string2", "string3")
```

### <a name="examples"></a>Voorbeelden
```Kusto
print strcat("hello", " ", "world") // result: "hello world"
```


## <a name="strlen"></a>strlen

Retourneert de lengte van een tekenreeks.

### <a name="syntax"></a>Syntaxis
```
strlen("text_to_evaluate")
```

### <a name="examples"></a>Voorbeelden
```Kusto
print strlen("hello")   // result: 5
```


## <a name="substring"></a>de subtekenreeks

Een subtekenreeks uit een bepaalde bron-tekenreeks, beginnend bij de opgegeven index. (Optioneel) kan de lengte van de aangevraagde subtekenreeks worden opgegeven.

### <a name="syntax"></a>Syntaxis
```
substring(source, startingIndex [, length])
```

### <a name="arguments"></a>Argumenten:

- `source` -De brontekenreeks die de subtekenreeks wordt opgehaald uit.
- `startingIndex` -De op nul gebaseerde beginpositie van de aangevraagde subtekenreeks.
- `length` -Een optionele parameter waarmee kan worden gebruikt om op te geven van de vereiste lengte van de geretourneerde subtekenreeks.

### <a name="examples"></a>Voorbeelden
```Kusto
print substring("abcdefg", 1, 2);   // result: "bc"
print substring("123456", 1);       // result: "23456"
print substring("123456", 2, 2);    // result: "34"
print substring("ABCD", 0, 2);  // result: "AB"
```


## <a name="tolower-toupper"></a>tolower, toupper

Zet een opgegeven tekenreeks om alle kleine en hoofdletters.

### <a name="syntax"></a>Syntaxis
```
tolower("value")
toupper("value")
```

### <a name="examples"></a>Voorbeelden
```Kusto
print tolower("HELLO"); // result: "hello"
print toupper("hello"); // result: "HELLO"
```



## <a name="next-steps"></a>Volgende stappen
Doorgaan met de geavanceerde zelfstudies:
* [Aggregatiefuncties](aggregations.md)
* [Geavanceerde aggregaties](advanced-aggregations.md)
* [Grafieken en diagrammen](charts.md)
* [Werken met JSON en gegevensstructuren](json-data-structures.md)
* [Geavanceerde query schrijven](advanced-query-writing.md)
* [Join - cross-analyse](joins.md)

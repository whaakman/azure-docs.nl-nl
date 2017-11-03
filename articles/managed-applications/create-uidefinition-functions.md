---
title: Azure-beheerde toepassing UI definitie functies maken | Microsoft Docs
description: Beschrijft de functies voor gebruik bij het samenstellen van UI-definities voor beheerde Azure-toepassingen
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: dcf570ca4bdc8eacb7e4d7a8ff0011c8e07b7a40
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition functies
Deze sectie bevat de handtekeningen voor alle ondersteunde functies van een CreateUiDefinition.

Een functie wilt gebruiken, plaatst u rond de declaratie vierkante haken. Bijvoorbeeld:

```json
"[function()]"
```

Tekenreeksen en andere functies kunnen worden verwezen als parameters voor een functie, maar tekenreeksen moeten tussen enkele aanhalingstekens worden geplaatst. Bijvoorbeeld:

```json
"[fn1(fn2(), 'foobar')]"
```

Indien van toepassing, kunt u verwijzen naar eigenschappen van de uitvoer van een functie met behulp van het punt. Bijvoorbeeld:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Functies die verwijzen naar
Deze functies kunnen worden gebruikt om te verwijzen naar uitvoer van de eigenschappen of de context van een CreateUiDefinition.

### <a name="basics"></a>basisbeginselen
Retourneert de uitvoerwaarden van een element dat is gedefinieerd in de stap basisbeginselen.

Het volgende voorbeeld wordt de uitvoer van het element met de naam `foo` in de basisprincipes van stap:

```json
"[basics('foo')]"
```

### <a name="steps"></a>stappen
Retourneert de uitvoerwaarden van een element dat is gedefinieerd in de stap. Als u de uitvoerwaarden van elementen in de stap basisbeginselen, gebruikt `basics()` in plaats daarvan.

Het volgende voorbeeld wordt de uitvoer van het element met de naam `bar` in de stap die met de naam `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Retourneert de locatie die is geselecteerd in de stap basisbeginselen of de huidige context.

Het volgende voorbeeld kan retourneren `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Tekenreeks-functies
Deze functies kunnen alleen worden gebruikt met een JSON-tekenreeks.

### <a name="concat"></a>concat
Een of meer reeksen aaneengeschakeld.

Bijvoorbeeld, als de uitvoerwaarde van `element1` als `"bar"`, en vervolgens het volgende voorbeeld wordt de tekenreeks `"foobar!"`:

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a>de subtekenreeks
Retourneert de subtekenreeks van de opgegeven tekenreeks. De subtekenreeks begint bij de opgegeven index en heeft de opgegeven lengte.

Het volgende voorbeeld wordt `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>vervangen
Retourneert een tekenreeks waarin alle instanties van de opgegeven tekenreeks in de huidige tekenreeks worden vervangen door een andere tekenreeks.

Het volgende voorbeeld wordt `"Everything is awesome!"`:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>GUID
Genereert een globally unique identifier (GUID)-tekenreeks.

Het volgende voorbeeld kan retourneren `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
Retourneert een tekenreeks die wordt omgezet in kleine letters.

Het volgende voorbeeld wordt `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Retourneert een tekenreeks naar hoofdletters is geconverteerd.

Het volgende voorbeeld wordt `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Functies van de verzameling
Deze functies kunnen worden gebruikt met verzamelingen, zoals JSON tekenreeksen, matrices en objecten.

### <a name="contains"></a>Bevat
Retourneert `true` als een tekenreeks de opgegeven subtekenreeks bevat een matrix de opgegeven waarde bevat of een object de opgegeven sleutel bevat.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld wordt `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>lengte
Retourneert het aantal tekens in een tekenreeks, het aantal waarden in een matrix of het aantal sleutels in een object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld wordt `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>leeg
Retourneert `true` als de tekenreekswaarde, een matrix of object null of leeg is.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld wordt `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Voorbeeld 4: null zijn en niet-gedefinieerde
Stel `element1` is `null` of niet-gedefinieerde. Het volgende voorbeeld wordt `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>eerste
Retourneert het eerste teken van de opgegeven tekenreeks; eerste waarde van de opgegeven matrix; of de eerste sleutel en waarde van het opgegeven object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Het volgende voorbeeld wordt `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>laatste
Retourneert het laatste teken van de opgegeven tekenreeks, de laatste waarde van de opgegeven matrix of de laatste sleutel en waarde van het opgegeven object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld wordt `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>duren
Retourneert een opgegeven aantal aaneengesloten tekens vanaf het begin van de tekenreeks, een opgegeven aantal opeenvolgende waarden vanaf het begin van de matrix of een opgegeven aantal aaneengesloten sleutels en waarden van het begin van het object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld wordt `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Overslaan
Een opgegeven aantal elementen in een verzameling omzeilt en retourneert vervolgens de resterende elementen.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld wordt `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Stel `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld wordt `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Stel `element1` geretourneerd:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Het volgende voorbeeld wordt `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Logische functies
Deze functies kunnen worden gebruikt in voorwaardelijke. Sommige functies mogelijk niet alle JSON-gegevenstypen ondersteund.

### <a name="equals"></a>is gelijk aan
Retourneert `true` als beide parameters hetzelfde type en waarde hebben. Deze functie ondersteunt alle JSON-gegevenstypen.

Het volgende voorbeeld wordt `true`:

```json
"[equals(0, 0)]"
```

Het volgende voorbeeld wordt `true`:

```json
"[equals('foo', 'foo')]"
```

Het volgende voorbeeld wordt `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>minder
Retourneert `true` als de eerste parameter uitsluitend bedoeld kleiner is dan de tweede parameter is. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld wordt `true`:

```json
"[less(1, 2)]"
```

Het volgende voorbeeld wordt `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Retourneert `true` als de eerste parameter kleiner dan of gelijk zijn aan de tweede parameter is. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld wordt `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>groter
Retourneert `true` als de eerste parameter strikt groter is dan de tweede parameter. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld wordt `false`:

```json
"[greater(1, 2)]"
```

Het volgende voorbeeld wordt `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Retourneert `true` als de eerste parameter groter dan of gelijk zijn aan de tweede parameter is. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld wordt `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>en
Retourneert `true` als alle parameters resulteren in `true`. Deze functie biedt ondersteuning voor twee of meer parameters van het type Booleaans alleen.

Het volgende voorbeeld wordt `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Het volgende voorbeeld wordt `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>of
Retourneert `true` als ten minste één van de parameters resulteert in `true`. Deze functie biedt ondersteuning voor twee of meer parameters van het type Booleaans alleen.

Het volgende voorbeeld wordt `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Het volgende voorbeeld wordt `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>niet
Retourneert `true` als de parameter resulteert in `false`. Deze functie biedt ondersteuning voor parameters van het type Booleaans alleen.

Het volgende voorbeeld wordt `true`:

```json
"[not(false)]"
```

Het volgende voorbeeld wordt `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>Coalesce
Retourneert de waarde van de eerste niet-null-parameter. Deze functie ondersteunt alle JSON-gegevenstypen.

Stel `element1` en `element2` zijn niet gedefinieerd. Het volgende voorbeeld wordt `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Van conversiefuncties
Deze functies kunnen worden gebruikt om waarden tussen JSON-gegevenstypen en coderingen te converteren.

### <a name="int"></a>int
De parameter wordt omgezet in een geheel getal. Deze functie biedt ondersteuning voor parameters van het nummer van het type en de tekenreeks.

Het volgende voorbeeld wordt `1`:

```json
"[int('1')]"
```

Het volgende voorbeeld wordt `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>Float
De parameter converteert naar een drijvende komma. Deze functie biedt ondersteuning voor parameters van het nummer van het type en de tekenreeks.

Het volgende voorbeeld wordt `1.0`:

```json
"[float('1.0')]"
```

Het volgende voorbeeld wordt `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>Tekenreeks
De parameter converteert naar een tekenreeks. Deze functie biedt ondersteuning voor parameters van alle JSON-gegevenstypen.

Het volgende voorbeeld wordt `"1"`:

```json
"[string(1)]"
```

Het volgende voorbeeld wordt `"2.9"`:

```json
"[string(2.9)]"
```

Het volgende voorbeeld wordt `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

Het volgende voorbeeld wordt `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>BOOL
De parameter converteert naar een Booleaanse waarde. Deze functie biedt ondersteuning voor parameters van het nummer van het type tekenreeks en Booleaanse waarde. Net als bij Booleaanse waarden in JavaScript kunt een waarde, behalve `0` of `'false'` retourneert `true`.

Het volgende voorbeeld wordt `true`:

```json
"[bool(1)]"
```

Het volgende voorbeeld wordt `false`:

```json
"[bool(0)]"
```

Het volgende voorbeeld wordt `true`:

```json
"[bool(true)]"
```

Het volgende voorbeeld wordt `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>parseren
De parameter wordt omgezet in een systeemeigen type. Deze functie is met andere woorden, de inverse van `string()`. Deze functie biedt ondersteuning voor parameters die alleen van het type tekenreeks.

Het volgende voorbeeld wordt `1`:

```json
"[parse('1')]"
```

Het volgende voorbeeld wordt `true`:

```json
"[parse('true')]"
```

Het volgende voorbeeld wordt `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

Het volgende voorbeeld wordt `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Codeert de parameter naar een Base64-gecodeerde tekenreeks. Deze functie biedt ondersteuning voor parameters die alleen van het type tekenreeks.

Het volgende voorbeeld wordt `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Decodeert de parameter van een Base64-gecodeerde tekenreeks. Deze functie biedt ondersteuning voor parameters die alleen van het type tekenreeks.

Het volgende voorbeeld wordt `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeUriComponent
Codeert de parameter naar een URL-gecodeerde tekenreeks. Deze functie biedt ondersteuning voor parameters die alleen van het type tekenreeks.

Het volgende voorbeeld wordt `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeUriComponent
Decodeert de parameter van een URL-gecodeerde tekenreeks. Deze functie biedt ondersteuning voor parameters die alleen van het type tekenreeks.

Het volgende voorbeeld wordt `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Rekenkundige functies
### <a name="add"></a>toevoegen
Telt twee getallen en retourneert het resultaat.

Het volgende voorbeeld wordt `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Sub
Het tweede nummer van het eerste getal wordt afgetrokken en retourneert het resultaat.

Het volgende voorbeeld wordt `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>mul
Vermenigvuldigt twee getallen en retourneert het resultaat.

Het volgende voorbeeld wordt `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
Het eerste getal door het tweede getal deelt, en retourneert het resultaat. Het resultaat is altijd een geheel getal.

Het volgende voorbeeld wordt `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>Mod
Het eerste getal door het tweede getal deelt, en retourneert het restgetal.

Het volgende voorbeeld wordt `0`:

```json
"[mod(6, 3)]"
```

Het volgende voorbeeld wordt `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>min.
Retourneert de kleine van de twee getallen.

Het volgende voorbeeld wordt `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>Maximum aantal
Retourneert de grootste van de twee getallen.

Het volgende voorbeeld wordt `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>bereik
Genereert een reeks integraal getallen binnen het opgegeven bereik.

Het volgende voorbeeld wordt `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Retourneert een willekeurig geheel getal in het opgegeven bereik. Deze functie geen cryptografisch beveiligde willekeurige getallen gegenereerd.

Het volgende voorbeeld kan retourneren `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Floor
Retourneert het grootste gehele getal kleiner dan of gelijk zijn aan het opgegeven getal.

Het volgende voorbeeld wordt `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>ceil
Retourneert het grootste gehele getal groter dan of gelijk zijn aan het opgegeven getal.

Het volgende voorbeeld wordt `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Datumfuncties
### <a name="utcnow"></a>utcNow
Retourneert een tekenreeks in de ISO 8601-notatie van de huidige datum en tijd op de lokale computer.

Het volgende voorbeeld kan retourneren `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>addSeconds
Een integraal aantal seconden aan de opgegeven tijdstempel toegevoegd.

Het volgende voorbeeld wordt `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Een integraal aantal minuten aan de opgegeven tijdstempel toegevoegd.

Het volgende voorbeeld wordt `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Een integraal aantal uren aan de opgegeven tijdstempel toegevoegd.

Het volgende voorbeeld wordt `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding in Azure Resource Manager [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).


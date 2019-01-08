---
title: Azure beheerde toepassingen maken functies voor interfacedefinitie | Microsoft Docs
description: Beschrijft de functies te gebruiken bij het maken van definities van de gebruikersinterface voor Azure Managed Applications
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 80fd593eecf189d516a8c9d7ef2a94ec9f23fc39
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063791"
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition-functies
In deze sectie bevat de handtekeningen voor alle ondersteunde functies van een CreateUiDefinition.

Een functie wilt gebruiken, moet u de declaratie tussen vierkante haken staan. Bijvoorbeeld:

```json
"[function()]"
```

Tekenreeksen en andere functies kunnen worden verwezen als parameters voor een functie, maar tekenreeksen moeten tussen enkele aanhalingstekens worden geplaatst. Bijvoorbeeld:

```json
"[fn1(fn2(), 'foobar')]"
```

Indien van toepassing, kunt u verwijzen naar eigenschappen van de uitvoer van een functie met behulp van de punt. Bijvoorbeeld:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Verwijzingsfuncties
Deze functies kunnen worden gebruikt om te verwijzen naar de uitvoer van de eigenschappen of de context van een CreateUiDefinition.

### <a name="basics"></a>Grondbeginselen
Retourneert de uitvoerwaarden voor de van een element dat is gedefinieerd in de stap van de basisprincipes.

Het volgende voorbeeld wordt de uitvoer van het element met de naam `foo` in de stap basisprincipes:

```json
"[basics('foo')]"
```

### <a name="steps"></a>stappen
Retourneert de uitvoerwaarden van een element dat is gedefinieerd in de volgende stap. Als u de uitvoerwaarden van elementen in de stap basisbeginselen, gebruikt `basics()` in plaats daarvan.

Het volgende voorbeeld wordt de uitvoer van het element met de naam `bar` in de stap met de naam `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Retourneert de locatie die is geselecteerd in de stap Basics of de huidige context.

Het volgende voorbeeld kan retourneren `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Tekenreeksfuncties
Deze functies kunnen alleen worden gebruikt met JSON-tekenreeksen.

### <a name="concat"></a>concat
Hiermee voegt u een of meer tekenreeksen samen.

Bijvoorbeeld, als de uitvoerwaarde van `element1` als `"bar"`, en vervolgens in het volgende voorbeeld de tekenreeks retourneert `"foobar!"`:

```json
"[concat('foo', steps('step1').element1, '!')]"
```

### <a name="substring"></a>de subtekenreeks
Retourneert de subtekenreeks van de opgegeven tekenreeks. De subtekenreeks begint bij de opgegeven index en heeft de opgegeven lengte.

Het volgende voorbeeld retourneert `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>vervangen
Retourneert een tekenreeks waarin alle instanties van de opgegeven tekenreeks in de huidige tekenreeks door een andere tekenreeks vervangen.

Het volgende voorbeeld retourneert `"Everything is awesome!"`:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>GUID
Genereert een wereldwijd unieke tekenreeks (GUID).

Het volgende voorbeeld kan retourneren `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
Retourneert een tekenreeks die wordt geconverteerd naar kleine letters.

Het volgende voorbeeld retourneert `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Retourneert een tekenreeks naar hoofdletters is geconverteerd.

Het volgende voorbeeld retourneert `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Verzameling functies
Deze functies kunnen worden gebruikt met verzamelingen, zoals JSON-tekenreeksen, matrices en objecten.

### <a name="contains"></a>bevat
Retourneert `true` als een tekenreeks de opgegeven subtekenreeks bevat, bevat de opgegeven waarde van een matrix of een object de opgegeven sleutel bevat.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld retourneert `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>Lengte
Retourneert het aantal tekens in een tekenreeks, het aantal waarden in een matrix of het aantal sleutels in een object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld retourneert `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>Leeg
Retourneert `true` als de tekenreeks, een matrix of een object null of leeg zijn is.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld retourneert `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Voorbeeld 4: null zijn en moet een niet-gedefinieerde
Wordt ervan uitgegaan dat `element1` is `null` of niet-gedefinieerde. Het volgende voorbeeld retourneert `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>eerste
Retourneert het eerste teken van de opgegeven tekenreeks; eerste waarde van de opgegeven matrix. of de eerste sleutel en waarde van het opgegeven object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Het volgende voorbeeld retourneert `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>laatste
Retourneert het laatste teken van de opgegeven tekenreeks, de laatste waarde van de opgegeven matrix of de laatste sleutel en waarde van het opgegeven object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld retourneert `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>toets maken
Retourneert een opgegeven aantal opeenvolgende tekens vanaf het begin van de tekenreeks, een opgegeven aantal opeenvolgende waarden vanaf het begin van de matrix of een opgegeven aantal aaneengesloten sleutels en waarden vanaf het begin van het object.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Het volgende voorbeeld retourneert `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>overslaan
Een opgegeven aantal elementen in een verzameling omzeilt en retourneert vervolgens de resterende elementen.

#### <a name="example-1-string"></a>Voorbeeld 1: tekenreeks
Het volgende voorbeeld retourneert `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Voorbeeld 2: matrix
Wordt ervan uitgegaan dat `element1` retourneert `[1, 2, 3]`. Het volgende voorbeeld retourneert `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Voorbeeld 3: object
Wordt ervan uitgegaan dat `element1` geeft als resultaat:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Het volgende voorbeeld retourneert `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Logische functies
Deze functies kunnen worden gebruikt in voorwaarden. Sommige functies mogelijk geen ondersteuning voor alle typen voor JSON-gegevens.

### <a name="equals"></a>is gelijk aan
Retourneert `true` als beide parameters de hetzelfde type en de waarde hebben. Deze functie biedt ondersteuning voor alle typen voor JSON-gegevens.

Het volgende voorbeeld retourneert `true`:

```json
"[equals(0, 0)]"
```

Het volgende voorbeeld retourneert `true`:

```json
"[equals('foo', 'foo')]"
```

Het volgende voorbeeld retourneert `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>minder
Retourneert `true` als de eerste parameter alleen kleiner is dan de tweede parameter is. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld retourneert `true`:

```json
"[less(1, 2)]"
```

Het volgende voorbeeld retourneert `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Retourneert `true` als de eerste parameter kleiner dan of gelijk zijn aan de tweede parameter is. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld retourneert `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>meer
Retourneert `true` als de eerste parameter strikt groter is dan de tweede parameter. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld retourneert `false`:

```json
"[greater(1, 2)]"
```

Het volgende voorbeeld retourneert `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Retourneert `true` als de eerste parameter groter dan of gelijk zijn aan de tweede parameter is. Deze functie biedt ondersteuning voor parameters alleen van het nummer van het type en de tekenreeks.

Het volgende voorbeeld retourneert `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>en
Retourneert `true` als alle parameters worden geëvalueerd tot `true`. Deze functie biedt ondersteuning voor twee of meer parameters alleen van het type Booleaanse waarde.

Het volgende voorbeeld retourneert `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Het volgende voorbeeld retourneert `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>of
Retourneert `true` als ten minste één van de parameters wordt geëvalueerd als `true`. Deze functie biedt ondersteuning voor twee of meer parameters alleen van het type Booleaanse waarde.

Het volgende voorbeeld retourneert `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Het volgende voorbeeld retourneert `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>niet
Retourneert `true` als de parameter wordt geëvalueerd als `false`. Deze functie biedt ondersteuning voor parameters alleen van het type Booleaanse waarde.

Het volgende voorbeeld retourneert `true`:

```json
"[not(false)]"
```

Het volgende voorbeeld retourneert `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>samenvoegen
Retourneert de waarde van de eerste niet-null-parameter. Deze functie biedt ondersteuning voor alle typen voor JSON-gegevens.

Wordt ervan uitgegaan dat `element1` en `element2` zijn gedefinieerd. Het volgende voorbeeld retourneert `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Conversiefuncties
Deze functies kunnen worden gebruikt om waarden tussen JSON-gegevenstypen en coderingen te converteren.

### <a name="int"></a>int
De parameter wordt omgezet in een geheel getal zijn. Deze functie biedt ondersteuning voor parameters van het nummer van het type en de tekenreeks.

Het volgende voorbeeld retourneert `1`:

```json
"[int('1')]"
```

Het volgende voorbeeld retourneert `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>float
De parameter converteert naar een drijvende komma. Deze functie biedt ondersteuning voor parameters van het nummer van het type en de tekenreeks.

Het volgende voorbeeld retourneert `1.0`:

```json
"[float('1.0')]"
```

Het volgende voorbeeld retourneert `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>string
De parameter wordt omgezet in een tekenreeks. Deze functie biedt ondersteuning voor parameters van alle typen voor JSON-gegevens.

Het volgende voorbeeld retourneert `"1"`:

```json
"[string(1)]"
```

Het volgende voorbeeld retourneert `"2.9"`:

```json
"[string(2.9)]"
```

Het volgende voorbeeld retourneert `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

Het volgende voorbeeld retourneert `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
De parameter converteert naar een Booleaanse waarde. Deze functie biedt ondersteuning voor parameters van het nummer van het type tekenreeks en Booleaanse waarde. Net als bij Booleaanse waarden in JavaScript, een waarde, behalve `0` of `'false'` retourneert `true`.

Het volgende voorbeeld retourneert `true`:

```json
"[bool(1)]"
```

Het volgende voorbeeld retourneert `false`:

```json
"[bool(0)]"
```

Het volgende voorbeeld retourneert `true`:

```json
"[bool(true)]"
```

Het volgende voorbeeld retourneert `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>parseren
De parameter wordt omgezet in een eigen type. Deze functie is met andere woorden, de inverse van `string()`. Deze functie biedt ondersteuning voor parameters alleen van het typetekenreeks.

Het volgende voorbeeld retourneert `1`:

```json
"[parse('1')]"
```

Het volgende voorbeeld retourneert `true`:

```json
"[parse('true')]"
```

Het volgende voorbeeld retourneert `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

Het volgende voorbeeld retourneert `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
De parameter een tekenreeks met base 64-codering codeert. Deze functie biedt ondersteuning voor parameters alleen van het typetekenreeks.

Het volgende voorbeeld retourneert `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Decodeert de parameter van een tekenreeks met base 64-codering. Deze functie biedt ondersteuning voor parameters alleen van het typetekenreeks.

Het volgende voorbeeld retourneert `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeUriComponent
Codeert met de parameter een tekenreeks van URL-codering. Deze functie biedt ondersteuning voor parameters alleen van het typetekenreeks.

Het volgende voorbeeld retourneert `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeUriComponent
Decodeert de parameter uit een tekenreeks van URL-codering. Deze functie biedt ondersteuning voor parameters alleen van het typetekenreeks.

Het volgende voorbeeld retourneert `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Wiskundige functies
### <a name="add"></a>add
Telt twee getallen en retourneert het resultaat.

Het volgende voorbeeld retourneert `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Sub
Het tweede nummer van het eerste getal aftrekt, en retourneert het resultaat.

Het volgende voorbeeld retourneert `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>mul
Vermenigvuldigt twee getallen en retourneert het resultaat.

Het volgende voorbeeld retourneert `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
Het eerste getal door het tweede getal deelt, en retourneert het resultaat. Het resultaat is altijd een geheel getal zijn.

Het volgende voorbeeld retourneert `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>Mod
Het eerste getal door het tweede getal deelt, en retourneert het restgetal.

Het volgende voorbeeld retourneert `0`:

```json
"[mod(6, 3)]"
```

Het volgende voorbeeld retourneert `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>min.
Retourneert de kleine van de twee getallen.

Het volgende voorbeeld retourneert `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>max
Retourneert de grootste waarde van de twee getallen.

Het volgende voorbeeld retourneert `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>Bereik
Genereert een reeks integraal nummer binnen het opgegeven bereik.

Het volgende voorbeeld retourneert `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Retourneert een willekeurig integraal getal binnen het opgegeven bereik. Deze functie geen cryptografisch beveiligd willekeurige getallen gegenereerd.

Het volgende voorbeeld kan retourneren `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Floor
Retourneert het grootste gehele getal kleiner dan of gelijk zijn aan het opgegeven getal.

Het volgende voorbeeld retourneert `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>ceil
Retourneert het grootste gehele getal groter dan of gelijk zijn aan het opgegeven getal.

Het volgende voorbeeld retourneert `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Datumfuncties
### <a name="utcnow"></a>utcNow
Retourneert een tekenreeks in ISO 8601-notatie van de huidige datum en tijd op de lokale computer.

Het volgende voorbeeld kan retourneren `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>addSeconds
Een integraal aantal seconden toegevoegd aan de opgegeven timestamp.

Het volgende voorbeeld retourneert `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Een integraal aantal minuten toegevoegd aan de opgegeven timestamp.

Het volgende voorbeeld retourneert `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Een integraal aantal uren toegevoegd aan de opgegeven timestamp.

Het volgende voorbeeld retourneert `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot Azure Resource Manager [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).


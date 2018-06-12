---
title: Functies in de werkstroom Definition Language - Azure Logic Apps | Microsoft Docs
description: Meer informatie over functies voor het maken van logische apps met de werkstroom Definition Language
services: logic-apps
author: ecfan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 6f1871e1e135ecb9e7cb37c0bedff3737d3febb7
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301385"
---
# <a name="workflow-definition-language-functions-reference-for-azure-logic-apps"></a>Werkstroom definitie functies Naslaggids voor Azure Logic Apps

In dit artikel beschrijft de functies die u gebruiken kunt bij het maken van werkstromen met [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Zie voor meer informatie over functies in logic app-definities, [werkstroom Definition Language voor Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md#functions). 

> [!NOTE]
> De syntaxis voor parameterdefinities, zijn een vraagteken (?) die wordt weergegeven nadat een parameter betekent de parameter dat is optioneel. Zie bijvoorbeeld [getFutureTime()](#getFutureTime).

<a name="action"></a>

## <a name="action"></a>actie

Retourneert de *huidige* actie-uitvoer in runtime of waarden van een andere naam en waarde-paren JSON, die u aan een expressie toewijzen kunt. Deze functie verwijst naar de volledige actie-object, maar u kunt optioneel een eigenschap waarvan u waarde wilt opgeven. Zie ook [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

U kunt de `action()` functie op de volgende locaties: 

* De `unsubscribe` eigenschap voor de actie van een webhook zodat u toegang hebt tot het resultaat van de oorspronkelijke `subscribe` aanvraag
* De `trackedProperties` eigenschap voor een actie
* De `do-until` lus voorwaarde voor een actie

```
action()
action().outputs.body.<property> 
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Eigenschap*> | Nee | Reeks | De naam voor de actie van de eigenschap object waarvan de gewenste waarde: **naam**, **startTime**, **endTime**, **invoer**,  **levert**, **status**, **code**, **trackingId**, en **clientTrackingId**. U kunt deze eigenschappen aan de hand van een specifieke uitvoeringsgeschiedenis details vinden in de Azure portal. Zie voor meer informatie [REST-API - uitvoeren werkstroomacties](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-uitvoer*> | Reeks | De uitvoer van de huidige actie of eigenschap | 
|||| 

<a name="actionBody"></a>

## <a name="actionbody"></a>actionBody

Retourneren van een actie `body` uitvoer tijdens runtime. Steno voor `actions('<actionName>').outputs.body`. Zie [body()](#body) en [actions()](#actions).

```
actionBody('<actionName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie `body` uitvoer die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-instantie-uitvoer*> | Reeks | De `body` de uitvoer van de opgegeven actie | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `body` de uitvoer van de actie Twitter `Get user`: 

```
actionBody('Get_user')
```

En dit resultaat wordt weergegeven:

```json
"body": {
  "FullName": "Contoso Corporation",
  "Location": "Generic Town, USA",
  "Id": 283541717,
  "UserName": "ContosoInc",
  "FollowersCount": 172,
  "Description": "Leading the way in transforming the digital workplace.",
  "StatusesCount": 93,
  "FriendsCount": 126,
  "FavouritesCount": 46,
  "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="actionOutputs"></a>

## <a name="actionoutputs"></a>actionOutputs

Retourneert een actie uitvoer tijdens runtime. Steno voor `actions('<actionName>').outputs`. Zie [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie die u wilt-uitvoer | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*Uitvoer*> | Reeks | De uitvoer van de opgegeven actie | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de uitvoer van de actie Twitter `Get user`: 

```
actionOutputs('Get_user')
```

En dit resultaat wordt weergegeven:

```json
{ 
  "statusCode": 200,
  "headers": {
    "Pragma": "no-cache",
    "Vary": "Accept-Encoding",
    "x-ms-request-id": "a916ec8f52211265d98159adde2efe0b",
    "X-Content-Type-Options": "nosniff",
    "Timing-Allow-Origin": "*",
    "Cache-Control": "no-cache",
    "Date": "Mon, 09 Apr 2018 18:47:12 GMT",
    "Set-Cookie": "ARRAffinity=b9400932367ab5e3b6802e3d6158afffb12fcde8666715f5a5fbd4142d0f0b7d;Path=/;HttpOnly;Domain=twitter-wus.azconn-wus.p.azurewebsites.net",
    "X-AspNet-Version": "4.0.30319",
    "X-Powered-By": "ASP.NET",
    "Content-Type": "application/json; charset=utf-8",
    "Expires": "-1",
    "Content-Length": "339"
  },
  "body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
  }
}
```

<a name="actions"></a>

## <a name="actions"></a>acties

De uitvoer van de actie in runtime of waarden van andere JSON naam en waarde-paren, die u aan een expressie toewijzen kunt geretourneerd. Standaard de functie verwijst naar de volledige actie-object, maar u kunt optioneel een eigenschap opgeven waarvan de waarde die u wilt. Zie voor steno versies [actionBody()](#actionBody), [actionOutputs()](#actionOutputs), en [body()](#body). Zie voor de huidige actie [action()](#action).

> [!NOTE] 
> Voorheen kunt u de `actions()` functie of de `conditions` element of een actie uitgevoerd op basis van de uitvoer van een andere actie opgeven. Echter, om te declareren expliciet afhankelijkheden tussen acties, moet u nu gebruiken van de afhankelijke actie `runAfter` eigenschap. Voor meer informatie over de `runAfter` eigenschap, Zie [opvangen en afhandelen van fouten met de eigenschap runAfter](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property> 
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor het actie-object waarvan de gewenste uitvoer  | 
| <*Eigenschap*> | Nee | Reeks | De naam voor de actie van de eigenschap object waarvan de gewenste waarde: **naam**, **startTime**, **endTime**, **invoer**,  **levert**, **status**, **code**, **trackingId**, en **clientTrackingId**. U kunt deze eigenschappen aan de hand van een specifieke uitvoeringsgeschiedenis details vinden in de Azure portal. Zie voor meer informatie [REST-API - uitvoeren werkstroomacties](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-uitvoer*> | Reeks | De uitvoer van de opgegeven actie of eigenschap | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `status` waarde van de eigenschap van de actie Twitter `Get user` tijdens runtime: 

```
actions('Get_user').outputs.body.status 
```

En dit resultaat wordt weergegeven: `"Succeeded"`

<a name="add"></a>

## <a name="add"></a>toevoegen

Retourneert het resultaat van het toevoegen van twee getallen.

```
add(<summand_1>, <summand_2>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*summand_1*>, <*summand_2*> | Ja | Geheel getal, Float, of een gemengd | De getallen om toe te voegen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*resultaat som*> | Geheel getal of drijvende-kommawaarde | Het resultaat van het toevoegen van de opgegeven getallen | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de opgegeven getallen toegevoegd:

```
add(1, 1.5)
```

En dit resultaat wordt weergegeven: `2.5`

<a name="addDays"></a>

## <a name="adddays"></a>addDays

Een aantal dagen aan een tijdstempel toevoegen.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*dagen*> | Ja | Geheel getal | Het positief of negatief aantal dagen om toe te voegen | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De tijdstempel plus het opgegeven aantal dagen  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 dagen aan de opgegeven tijdstempel toegevoegd:

```
addDays('2018-03-15T13:00:00Z', 10)
```

En dit resultaat wordt weergegeven: `"2018-03-25T00:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt afgetrokken van vijf dagen van de opgegeven tijdstempel:

```
addDays('2018-03-15T00:00:00Z', -5)
```

En dit resultaat wordt weergegeven: `"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

## <a name="addhours"></a>addHours

Een aantal uren toevoegen aan een tijdstempel.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Uren*> | Ja | Geheel getal | Het positieve of negatieve aantal uren om toe te voegen | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De tijdstempel plus het opgegeven aantal uren  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 uur aan de opgegeven tijdstempel toegevoegd:

```
addHours('2018-03-15T00:00:00Z', 10)
```

En dit resultaat wordt weergegeven: `"2018-03-15T10:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt afgetrokken van vijf uur van de opgegeven tijdstempel:

```
addHours('2018-03-15T15:00:00Z', -5)
```

En dit resultaat wordt weergegeven: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

## <a name="addminutes"></a>addMinutes

Een aantal minuten toevoegen aan een tijdstempel.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*minuten*> | Ja | Geheel getal | Het positieve of negatieve aantal minuten toevoegen | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De tijdstempel plus het opgegeven aantal minuten | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt de 10 minuten aan de opgegeven tijdstempel toegevoegd:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:20:00.0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt afgetrokken van vijf minuten van het tijdstempel voor de opgegeven:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

## <a name="addproperty"></a>addProperty

Een eigenschap en de waarde of de naam / waarde-paar toevoegen aan een JSON-object en het bijgewerkte object retourneren. Als het object al tijdens runtime bestaat, de functie een fout genereert.

```
addProperty(<object>, '<property>', <value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Ja | Object | De JSON-object waar u een eigenschap toevoegen | 
| <*Eigenschap*> | Ja | Reeks | De naam op voor de eigenschap toevoegen | 
| <*Waarde*> | Ja | Alle | De waarde voor de eigenschap |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt-object*> | Object | Het bijgewerkte JSON-object met de opgegeven eigenschap | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `accountNumber` eigenschap in op de `customerProfile` -object, dat wordt geconverteerd naar JSON met de [JSON()](#json) functie. De functie wijst een waarde die wordt gegenereerd door de [guid()](#guid) functioneren en retourneert het bijgewerkte object:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

## <a name="addseconds"></a>addSeconds

Een aantal seconden aan een tijdstempel toevoegen.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Seconden*> | Ja | Geheel getal | Het positieve of negatieve aantal seconden toevoegen | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De tijdstempel plus het opgegeven aantal seconden  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 seconden tot de opgegeven tijdstempel toegevoegd:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:00:10.0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt afgetrokken van vijf seconden tot de opgegeven tijdstempel:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

## <a name="addtotime"></a>addToTime

Een aantal tijdseenheden toevoegen aan een tijdstempel. Zie ook [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Interval*> | Ja | Geheel getal | Het aantal eenheden van de opgegeven tijd om toe te voegen | 
| <*timeUnit*> | Ja | Reeks | Tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', "Maand", "Jaar" | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De tijdstempel plus het opgegeven aantal tijdseenheden  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt een dag aan de opgegeven tijdstempel toegevoegd:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day') 
```

En dit resultaat wordt weergegeven: `"2018-01-02T00:00:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt een dag aan de opgegeven tijdstempel toegevoegd:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

En retourneert het resultaat met de optionele "D"-indeling: `"Tuesday, January 2, 2018"`

<a name="and"></a>

## <a name="and"></a>en

Controleer of alle expressies voldaan wordt. Retourneert waar wanneer alle expressies waar zijn of onwaar retourneren wanneer ten minste één expressie ingesteld op false is.

```
and(<expression1>, <expression2>, ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Expression1*>, <*expression2*>,... | Ja | Boole-waarde | De expressies om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar als alle expressies voldaan wordt. Geretourneerde waarde false wanneer ten minste één expressie ingesteld op false is. | 
|||| 

*Voorbeeld 1*

Deze voorbeelden kunt u controleren of de opgegeven Booleaanse waarden alle waar zijn:

```
and(true, true)
and(false, true)
and(false, false)
```

En deze resultaten retourneert:

* Eerste voorbeeld: beide expressies ' True ', dus retourneert `true`. 
* Tweede voorbeeld: een expressie is ingesteld op false, dus retourneert `false`.
* Derde voorbeeld: beide expressies zijn ingesteld op false, dus retourneert `false`.

*Voorbeeld 2*

Deze voorbeelden kunt u controleren of de opgegeven expressies alle waar zijn:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

En deze resultaten retourneert:

* Eerste voorbeeld: beide expressies ' True ', dus retourneert `true`. 
* Tweede voorbeeld: een expressie is ingesteld op false, dus retourneert `false`.
* Derde voorbeeld: beide expressies zijn ingesteld op false, dus retourneert `false`.

<a name="array"></a>

## <a name="array"></a>matrix

Retourneert een matrix van één opgegeven invoer. Zie voor meerdere invoer [createArray()](#createArray). 

```
array('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks voor het maken van een matrix | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*waarde*>] | Matrix | Een matrix met de opgegeven invoer van één | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een matrix van de tekenreeks 'Hallo':

```
array('hello')
```

En dit resultaat wordt weergegeven: `["hello"]`

<a name="base64"></a>

## <a name="base64"></a>Base64

De versie base64-gecodeerd om een tekenreeks retourneren.

```
base64('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De invoerreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Base64-tekenreeks*> | Reeks | De versie base64-gecodeerd voor de invoerreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de tekenreeks 'Hallo' geconverteerd naar een base64-gecodeerde tekenreeks:

```
base64('hello')
```

En dit resultaat wordt weergegeven: `"aGVsbG8="`

<a name="base64ToBinary"></a>

## <a name="base64tobinary"></a>base64ToBinary

De binaire versie voor een base64-gecodeerde tekenreeks retourneren.

```
base64ToBinary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De base64-gecodeerde tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-base64-tekenreeks*> | Reeks | De binaire versie voor de met base64-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de ' aGVsbG8 = "base64-gecodeerde tekenreeks naar een binaire tekenreeks:

```
base64ToBinary('aGVsbG8=')
```

En dit resultaat wordt weergegeven: 

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

## <a name="base64tostring"></a>base64ToString

De tekenreeksversie voor een base64-gecodeerde tekenreeks effectief decoderen van base64-tekenreeks geretourneerd. Deze functie wilt gebruiken in plaats [decodeBase64()](#decodeBase64). Hoewel beide functies op dezelfde manier werken `base64ToString()` verdient de voorkeur.

```
base64ToString('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De base64-gecodeerde tekenreeks decoderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-base64-tekenreeks*> | Reeks | De tekenreeksversie voor een base64-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de ' aGVsbG8 = "base64-gecodeerde tekenreeks naar alleen een tekenreeks:

```
base64ToString('aGVsbG8=')
```

En dit resultaat wordt weergegeven: `"hello"`

<a name="binary"></a>

## <a name="binary"></a>Binaire 

De binaire versie naar een tekenreeks retourneren.

```
binary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair bestand voor invoer waarde*> | Reeks | De binaire versie voor de opgegeven tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de tekenreeks 'Hallo' geconverteerd naar een binaire tekenreeks:

```
binary('hello')
```

En dit resultaat wordt weergegeven: 

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

## <a name="body"></a>hoofdtekst

Retourneren van een actie `body` uitvoer tijdens runtime. Steno voor `actions('<actionName>').outputs.body`. Zie [actionBody()](#actionBody) en [actions()](#actions).

```
body('<actionName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie `body` uitvoer die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-instantie-uitvoer*> | Reeks | De `body` de uitvoer van de opgegeven actie | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `body` de uitvoer van de `Get user` Twitter actie: 

```
body('Get_user')
```

En dit resultaat wordt weergegeven: 

```json
"body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="bool"></a>

## <a name="bool"></a>BOOL

De Booleaanse versie voor een waarde retourneren.

```
bool(<value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Alle | De waarde te converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | De Booleaanse versie voor de opgegeven waarde | 
|||| 

*Voorbeeld*

De opgegeven waarden converteren in deze voorbeelden naar Boole-waarden: 

```
bool(1)
bool(0)
```

En deze resultaten retourneert: 

* Eerste voorbeeld: `true` 
* Tweede voorbeeld: `false`

<a name="coalesce"></a>

## <a name="coalesce"></a>Coalesce

Retourneert de eerste niet-null-waarde uit een of meer parameters. Lege tekenreeksen, matrices leeg en lege objecten zijn niet null.

```
coalesce(<object_1>, <object_2>, ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object_1*>, <*object_2*>,... | Ja | Eventuele mix van typen | Een of meer items om te controleren op null | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*eerste-niet-null-item*> | Alle | Het eerste item of -waarde die niet null is. Als alle parameters null zijn, is deze functie retourneert null. | 
|||| 

*Voorbeeld*

Deze voorbeelden worden de eerste niet-null-waarde retourneren van de opgegeven waarden, of null als alle waarden null zijn:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

En deze resultaten retourneert: 

* Eerste voorbeeld: `true` 
* Tweede voorbeeld: `"hello"`
* Derde voorbeeld: `null`

<a name="concat"></a>

## <a name="concat"></a>concat

Twee of meer reeksen combineren en de gecombineerde tekenreeks retourneren. 

```
concat('<text1>', '<text2>', ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst1*>, <*Tekst2*>,... | Ja | Reeks | Ten minste twee tekenreeksen combineren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*text1text2...*> | Reeks | De tekenreeks die is gemaakt op basis van de gecombineerde invoer tekenreeksen | 
|||| 

*Voorbeeld*

In dit voorbeeld worden de tekenreeksen "Hello" en "Wereld" gecombineerd:

```
concat('Hello', 'World')
```

En dit resultaat wordt weergegeven: `"HelloWorld"`

<a name="contains"></a>

## <a name="contains"></a>bevat

Controleer of een verzameling een specifiek item heeft. Retourneert waar wanneer het item is gevonden of onwaar retourneren wanneer niet gevonden. Deze functie is hoofdlettergevoelig.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Deze functie werkt in het bijzonder op de verzamelingtypen van deze: 

* Een *tekenreeks* vinden een *subtekenreeks*
* Een *matrix* vinden een *waarde*
* Een *woordenlijst* vinden een *sleutel*

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix woordenlijst | De verzameling om te controleren | 
| <*Waarde*> | Ja | Tekenreeks of matrix woordenlijst, respectievelijk | Het item dat u wilt zoeken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert true als het item is gevonden. Retourneert onwaar wanneer het niet gevonden. |
|||| 

*Voorbeeld 1*

In dit voorbeeld controleert de tekenreeks "Hallo wereld' voor de subtekenreeks 'wereld' en ' true ' geretourneerd:

```
contains('hello world', 'world')
```

*Voorbeeld 2*

In dit voorbeeld controleert de tekenreeks "Hallo wereld' voor de subtekenreeks 'universe' en retourneert false:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

## <a name="convertfromutc"></a>convertFromUtc

Een tijdstempel van Coordinated van Universal Time (UTC) naar de doel-tijdzone converteren.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*destinationTimeZone*> | Ja | Reeks | De naam voor de doel-tijdzone. Zie voor meer informatie [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geconverteerd tijdstempel*> | Reeks | De tijdstempel is geconverteerd naar de doel-tijdzone | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt een tijdstempel geconverteerd naar de opgegeven tijdzone: 

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

En dit resultaat wordt weergegeven: `"2018-01-01T00:00:00.0000000"`

*Voorbeeld 2*

In dit voorbeeld wordt een tijdstempel geconverteerd naar de opgegeven tijdzone en de indeling:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

En dit resultaat wordt weergegeven: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

## <a name="converttimezone"></a>convertTimeZone

Een tijdstempel van de tijdzone van de bron converteren naar de doel-tijdzone.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*sourceTimeZone*> | Ja | Reeks | De naam voor de tijdzone van de bron. Zie voor meer informatie [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*destinationTimeZone*> | Ja | Reeks | De naam voor de doel-tijdzone. Zie voor meer informatie [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geconverteerd tijdstempel*> | Reeks | De tijdstempel is geconverteerd naar de doel-tijdzone | 
|||| 

*Voorbeeld 1*

In het volgende voorbeeld wordt de tijdzone van de bron naar de doel-tijdzone: 

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

En dit resultaat wordt weergegeven: `"2018-01-01T00:00:00.0000000"`

*Voorbeeld 2*

In dit voorbeeld wordt een tijdzone geconverteerd naar de opgegeven tijdzone en de indeling:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

En dit resultaat wordt weergegeven: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

## <a name="converttoutc"></a>convertToUtc

Een tijdstempel van de tijdzone van de bron voor Coordinated van Universal Time (UTC) converteren.

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*sourceTimeZone*> | Ja | Reeks | De naam voor de tijdzone van de bron. Zie voor meer informatie [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geconverteerd tijdstempel*> | Reeks | De tijdstempel is geconverteerd naar UTC | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt een tijdstempel geconverteerd naar UTC: 

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

En dit resultaat wordt weergegeven: `"2018-01-01T08:00:00.0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt een tijdstempel geconverteerd naar UTC:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

En dit resultaat wordt weergegeven: `"Monday, January 1, 2018"`

<a name="createArray"></a>

## <a name="createarray"></a>createArray

Retourneert een matrix uit meerdere invoer. Zie voor één invoer matrices, [array](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object1*>, <*object2*>,... | Ja | Aanwezig, maar niet in de gemengde modus | Ten minste twee items voor het maken van de matrix | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*object1*>, <*object2*>,...] | Matrix | De matrix gemaakt op basis van de invoer items | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een matrix van deze invoer:

```
createArray('h', 'e', 'l', 'l', 'o')
```

En dit resultaat wordt weergegeven: `["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

## <a name="datauri"></a>dataUri

Een data uniform resource identifier (URI) voor een tekenreeks retourneren. 

```
dataUri('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gegevens-uri*> | Reeks | De gegevens-URI voor de invoerreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt een gegevens-URI voor de tekenreeks 'Hallo':

```
dataUri('hello') 
```

En dit resultaat wordt weergegeven: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

## <a name="datauritobinary"></a>dataUriToBinary

De binaire versie voor een data uniform resource identifier (URI) retourneren. Deze functie wilt gebruiken in plaats [decodeDataUri()](#decodeDataUri). Hoewel beide functies op dezelfde manier werken `decodeDataUri()` verdient de voorkeur.

```
dataUriToBinary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De gegevens-URI te converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-gegevens-uri*> | Reeks | De binaire versie voor de gegevens-URI | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt een binaire versie voor deze gegevens-URI:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

En dit resultaat wordt weergegeven: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

## <a name="datauritostring"></a>dataUriToString

De tekenreeksversie voor een data uniform resource identifier (URI) retourneren.

```
dataUriToString('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De gegevens-URI te converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*tekenreeks voor gegevens uri*> | Reeks | De tekenreeksversie voor de gegevens-URI | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt een tekenreeks op voor deze gegevens-URI:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

En dit resultaat wordt weergegeven: `"hello"`

<a name="dayOfMonth"></a>

## <a name="dayofmonth"></a>DayOfMonth

De dag van de maand retourneren van een tijdstempel. 

```
dayOfMonth('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*dag van de maand*> | Geheel getal | De dag van de maand van het tijdstempel voor de opgegeven | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de waarde voor de dag van de maand van dit tijdstempel:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

En dit resultaat wordt weergegeven: `15`

<a name="dayOfWeek"></a>

## <a name="dayofweek"></a>DayOfWeek

De dag van de week van een tijdstempel retourneren.  

```
dayOfWeek('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*dag van de week*> | Geheel getal | De dag van de week van de opgegeven tijdstempel waar zondag is ingesteld op 0 maandag is 1, enzovoort | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de waarde voor de dag van de week van dit tijdstempel:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

En dit resultaat wordt weergegeven: `3`

<a name="dayOfYear"></a>

## <a name="dayofyear"></a>DayOfYear

De dag van het jaar retourneren van een tijdstempel. 

```
dayOfYear('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*dag van jaar*> | Geheel getal | De dag van het jaar van de opgegeven tijdstempel | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de waarde van de dag van het jaar van dit tijdstempel:

```
dayOfYear('2018-03-15T13:27:36Z')
```

En dit resultaat wordt weergegeven: `74`

<a name="decodeBase64"></a>

## <a name="decodebase64"></a>decodeBase64

De tekenreeksversie voor een base64-gecodeerde tekenreeks effectief decoderen van base64-tekenreeks geretourneerd. Overweeg het gebruik van [base64ToString()](#base64ToString) plaats `decodeBase64()`. Hoewel beide functies op dezelfde manier werken `base64ToString()` verdient de voorkeur.

```
decodeBase64('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De base64-gecodeerde tekenreeks decoderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-base64-tekenreeks*> | Reeks | De tekenreeksversie voor een base64-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt een tekenreeks op voor een base64-gecodeerde tekenreeks:

```
decodeBase64('aGVsbG8=')
```

En dit resultaat wordt weergegeven: `"hello"`

<a name="decodeDataUri"></a>

## <a name="decodedatauri"></a>decodeDataUri

De binaire versie voor een data uniform resource identifier (URI) retourneren. Overweeg het gebruik van [dataUriToBinary()](#dataUriToBinary), plaats `decodeDataUri()`. Hoewel beide functies op dezelfde manier werken `dataUriToBinary()` verdient de voorkeur.

```
decodeDataUri('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De gegevens te decoderen URI-tekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-gegevens-uri*> | Reeks | De binaire versie voor een gegevens-URI-tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de binaire versie voor deze gegevens-URI:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

En dit resultaat wordt weergegeven: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

## <a name="decodeuricomponent"></a>decodeUriComponent

Een tekenreeks of vervangt escape-tekens met versie gedecodeerde retourneren. 

```
decodeUriComponent('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks met de escape-tekens te decoderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-uri*> | Reeks | De bijgewerkte tekenreeks met de gedecodeerde escape-tekens | 
|||| 

*Voorbeeld*

In dit voorbeeld worden de escape-tekens in deze tekenreeks vervangen door gedecodeerde versies:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

En dit resultaat wordt weergegeven: `"https://contoso.com"`

<a name="div"></a>

## <a name="div"></a>div

Retourneert het resultaat geheel getal van de deling van twee getallen. Als u de rest-resultaat, Zie [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*deeltal*> | Ja | Geheel getal of drijvende-kommawaarde | Het nummer te delen door het *deler* | 
| <*deler*> | Ja | Geheel getal of drijvende-kommawaarde | Het getal dat verdeelt de *deeltal*, maar mag niet 0 zijn | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*quotiënt resultaat*> | Geheel getal | De integer als resultaat van de deling van het eerste getal door het tweede getal | 
|||| 

*Voorbeeld*

Beide voorbeelden delen het eerste getal door het tweede getal:

```
div(10, 5)
div(11, 5)
```

En dit resultaat te retourneren: `2`

<a name="encodeUriComponent"></a>

## <a name="encodeuricomponent"></a>encodeUriComponent

Retourneert een uniform resource identifier (URI) gecodeerde versie naar een tekenreeks URL onveilige tekens te vervangen door een escape-tekens. Overweeg het gebruik van [uriComponent()](#uriComponent), plaats `encodeUriComponent()`. Hoewel beide functies op dezelfde manier werken `uriComponent()` verdient de voorkeur.

```
encodeUriComponent('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks converteren naar URI-notatie | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*codering-uri*> | Reeks | De URI-gecodeerde tekenreeks met escape-tekens | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt een URI-codering versie voor deze tekenreeks:

```
encodeUriComponent('https://contoso.com')
```

En dit resultaat wordt weergegeven: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

## <a name="empty"></a>Leeg

Controleren of een verzameling leeg is. Retourneert waar wanneer de verzameling leeg is of ONWAAR wanneer deze niet leeg retourneren.

```
empty('<collection>')
empty([<collection>])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreekswaarde, een matrix of Object | De verzameling om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar als de verzameling leeg is. Retourneert onwaar wanneer deze niet leeg zijn. | 
|||| 

*Voorbeeld* 

Deze voorbeelden kunt u controleren of de opgegeven verzamelingen leeg zijn:

```
empty('')
empty('abc')
```

En deze resultaten retourneert: 

* Eerste voorbeeld: geeft een lege tekenreeks, zodat de functie retourneert `true`. 
* Tweede voorbeeld: geeft u de tekenreeks "abc", zodat de functie retourneert `false`. 

<a name="endswith"></a>

## <a name="endswith"></a>endsWith

Controleren of een tekenreeks op een specifieke subtekenreeks eindigt. Retourneert waar wanneer de subtekenreeks is gevonden of onwaar retourneren wanneer niet gevonden. Deze functie is niet hoofdlettergevoelig.

```
endsWith('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks om te controleren | 
| <*Brontekst*> | Ja | Reeks | De einddatum subtekenreeks vinden | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR  | Boole-waarde | Retourneert waar wanneer de laatste subtekenreeks is gevonden. Retourneert onwaar wanneer het niet gevonden. | 
|||| 

*Voorbeeld 1* 

Dit voorbeeld wordt gecontroleerd of de tekenreeks "Hallo wereld" op de tekenreeks "wereld eindigt":

```
endsWith('hello world', 'world')
```

En dit resultaat wordt weergegeven: `true`

*Voorbeeld 2*

Dit voorbeeld wordt gecontroleerd of de tekenreeks "Hallo wereld" op de tekenreeks 'universe eindigt':

```
endsWith('hello world', 'universe')
```

En dit resultaat wordt weergegeven: `false`

<a name="equals"></a>

## <a name="equals"></a>is gelijk aan

Controleer of zowel waarden, expressies of objecten equivalent zijn. Retourneert waar wanneer beide gelijk zijn, of onwaar retourneren wanneer ze dat niet gelijk zijn.

```
equals('<object1>', '<object2>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object1*>, <*object2*> | Ja | verschillende | De waarden, expressies of objecten die u wilt vergelijken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar wanneer beide gelijk zijn. Retourneert onwaar wanneer geen equivalent. | 
|||| 

*Voorbeeld*

Deze voorbeelden kunt u controleren of de opgegeven invoer equivalent zijn. 

```
equals(true, 1)
equals('abc', 'abcd')
```

En deze resultaten retourneert: 

* Eerste voorbeeld: beide waarden equivalent zijn, zodat de functie retourneert `true`.
* Voorbeeld van de tweede: beide waarden niet het geval is, zodat de functie retourneert `false`.

<a name="first"></a>

## <a name="first"></a>eerste

Retourneert het eerste item uit een tekenreeks of matrix.

```
first('<collection>')
first([<collection>])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix | De verzameling waar vind ik het eerste item |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*eerste verzamelingsitem*> | Alle | Het eerste item in de verzameling | 
|||| 

*Voorbeeld*

Deze voorbeelden vinden het eerste item in deze verzamelingen:

```
first('hello')
first([0, 1, 2])
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `"h"`
* Voorbeeld van de tweede: `0`

<a name="float"></a>

## <a name="float"></a>drijvend

De tekenreeksversie van een voor een getal met drijvende komma converteren naar een werkelijke getal met drijvende komma. U kunt deze functie alleen als aangepaste parameters wordt doorgegeven aan een app, zoals een logische app.

```
float('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks met een geldig getal met drijvende komma converteren |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Float-waarde*> | Float | De drijvende-nummer voor de opgegeven tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt een tekenreeksversie voor dit getal met drijvende komma:

```
float('10.333')
```

En dit resultaat wordt weergegeven: `10.333`

<a name="formatDateTime"></a>

## <a name="formatdatetime"></a>formatDateTime

Retourneert een tijdstempel in de opgegeven indeling.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*opnieuw geformatteerd tijdstempel*> | Reeks | De bijgewerkte tijdstempel in de opgegeven indeling | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt een tijdstempel voor de opgegeven indeling geconverteerd:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

En dit resultaat wordt weergegeven: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

## <a name="formdatamultivalues"></a>formDataMultiValues

Retourneert een matrix met waarden die overeenkomen met de naam van een sleutel in een actie *formuliergegevens* of *formulier gecodeerd* uitvoer. 

```
formDataMultiValues('<actionName>', '<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De actie waarvan de uitvoer is de waarde van de sleutel die u wilt | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan de gewenste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*matrix met sleutel waarden*>] | Matrix | Een matrix met de waarden die overeenkomen met de opgegeven sleutel | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt een matrix van waarde in de opgegeven actie formuliergegevens of formulier gecodeerde uitvoer van de 'Onderwerp' sleutel gemaakt:  

```
formDataMultiValues('Send_an_email', 'Subject')
```

En resulteert in de tekst van het onderwerp in een matrix, bijvoorbeeld: `["Hello world"]`

<a name="formDataValue"></a>

## <a name="formdatavalue"></a>formDataValue

Retourneert een waarde die overeenkomt met de naam van een sleutel in een actie *formuliergegevens* of *formulier gecodeerd* uitvoer. Als de functie meer dan een overeenkomst wordt gevonden, wordt in de functie een fout genereert.

```
formDataValue('<actionName>', '<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De actie waarvan de uitvoer is de waarde van de sleutel die u wilt | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan de gewenste waarde |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*sleutel / waarde-*> | Reeks | De waarde in de opgegeven sleutel  | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt een tekenreeks van de sleutel 'Onderwerp'-waarde in de opgegeven actie formuliergegevens of formulier gecodeerde uitvoer:  

```
formDataValue('Send_an_email', 'Subject')
```

En retourneert het onderwerp wordt een tekenreeks, bijvoorbeeld: `"Hello world"`

<a name="getFutureTime"></a>

## <a name="getfuturetime"></a>getFutureTime

Retourneert de huidige tijdstempel plus de opgegeven periode-eenheden.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Interval*> | Ja | Geheel getal | Het aantal eenheden opgegeven tijdstip moet worden afgetrokken | 
| <*timeUnit*> | Ja | Reeks | Tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', "Maand", "Jaar" | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De huidige tijdstempel plus het opgegeven aantal tijdseenheden | 
|||| 

*Voorbeeld 1*

Stel dat de huidige tijdstempel is ' 2018-03-01T00:00:00.0000000Z '. In dit voorbeeld wordt vijf dagen aan die tijdstempel toegevoegd:

```
getFutureTime(5, 'Day')
```

En dit resultaat wordt weergegeven: `"2018-03-06T00:00:00.0000000Z"`

*Voorbeeld 2*

Stel dat de huidige tijdstempel is ' 2018-03-01T00:00:00.0000000Z '. Dit voorbeeld wordt vijf dagen en het resultaat wordt omgezet in de indeling "D":

```
getFutureTime(5, 'Day', 'D')
```

En dit resultaat wordt weergegeven: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

## <a name="getpasttime"></a>getPastTime

Retourneert de huidige tijdstempel minus de opgegeven periode-eenheden.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Interval*> | Ja | Geheel getal | Het aantal eenheden opgegeven tijdstip moet worden afgetrokken | 
| <*timeUnit*> | Ja | Reeks | Tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', "Maand", "Jaar" | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De huidige tijdstempel minus het opgegeven aantal tijdseenheden | 
|||| 

*Voorbeeld 1*

Stel dat de huidige tijdstempel is ' 2018-02-01T00:00:00.0000000Z '. In dit voorbeeld wordt afgetrokken van vijf dagen uit tijdstempel:

```
getPastTime(5, 'Day')
```

En dit resultaat wordt weergegeven: `"2018-01-27T00:00:00.0000000Z"`

*Voorbeeld 2*

Stel dat de huidige tijdstempel is ' 2018-02-01T00:00:00.0000000Z '. In dit voorbeeld wordt afgetrokken van vijf dagen en het resultaat wordt omgezet in de indeling "D":

```
getPastTime(5, 'Day', 'D')
```

En dit resultaat wordt weergegeven: `"Saturday, January 27, 2018"`

<a name="greater"></a>

## <a name="greater"></a>groter

Controleer of de eerste waarde groter dan de tweede waarde. Retourneert waar wanneer de eerste waarde meer is, of retourneert false als minder.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, Float of String | De eerste waarde om te controleren of groter dan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, Float of String, respectievelijk | De vergelijkingswaarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar als de eerste waarde groter dan de tweede waarde is. Retourneert onwaar als de eerste waarde gelijk aan of kleiner is dan de tweede waarde is. | 
|||| 

*Voorbeeld*

Deze voorbeelden kunt u controleren of de eerste waarde groter dan de tweede waarde is:

```
greater(10, 5)
greater('apple', 'banana')
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `true`
* Tweede voorbeeld: `false`

<a name="greaterOrEquals"></a>

## <a name="greaterorequals"></a>greaterOrEquals

Controleer of de eerste waarde groter dan of gelijk zijn aan de tweede waarde.
Retourneert waar wanneer de eerste waarde groter of gelijk is of onwaar retourneren wanneer de eerste waarde kleiner is.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, Float of String | De eerste waarde om te controleren of groter dan of gelijk zijn aan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, Float of String, respectievelijk | De vergelijkingswaarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar als de eerste waarde groter dan of gelijk zijn aan de tweede waarde is. Retourneert onwaar als de eerste waarde kleiner is dan de tweede waarde. | 
|||| 

*Voorbeeld*

Deze voorbeelden kunt u controleren of de eerste waarde groter of gelijk dan de tweede waarde is:

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `true`
* Tweede voorbeeld: `false`

<a name="guid"></a>

## <a name="guid"></a>GUID

Een globally unique identifier (GUID) genereren als een tekenreeks, bijvoorbeeld 'c2ecc88d-88c8-4096-912c-d6f2e2b138ce': 

```
guid()
```

U kunt ook een andere indeling opgeven voor de GUID dan de standaardindeling, "D", 32 cijfers is, gescheiden door koppeltekens.

```
guid('<format>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Indeling*> | Nee | Reeks | Een enkel [aanduiding opmaken](https://msdn.microsoft.com/library/97af8hh4) voor de geretourneerde GUID. De indeling "D" is, maar u kunt "N", "D", "B", 'P' of 'X'. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*GUID-waarde*> | Reeks | Een willekeurig gegenereerde GUID | 
|||| 

*Voorbeeld* 

In dit voorbeeld genereert dezelfde GUID, maar als 32 cijfers, gescheiden door koppeltekens en tussen haakjes: 

```
guid('P')
```

En dit resultaat wordt weergegeven: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

## <a name="if"></a>if

Controleer of een expressie waar of ONWAAR. Op basis van het resultaat, een opgegeven waarde retourneren.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*expressie*> | Ja | Boole-waarde | De expressie om te controleren | 
| <*waardeindienwaar*> | Ja | Alle | De waarde wilt retourneren wanneer de expressie waar is | 
| <*WaardeAlsOnwaar*> | Ja | Alle | De waarde wilt retourneren wanneer de expressie onwaar is | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*opgegeven return waarde*> | Alle | De opgegeven waarde die wordt geretourneerd op basis van of de expressie is true of false | 
|||| 

*Voorbeeld* 

In dit voorbeeld retourneert `"yes"` omdat de opgegeven expressie ' true ' geretourneerd. Anders wordt het voorbeeld retourneert `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

## <a name="indexof"></a>indexOf

Retourneert de beginpositie of indexwaarde voor een subtekenreeks. Deze functie is niet hoofdlettergevoelig en indexen beginnen bij nummer 0. 

```
indexOf('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks met de subtekenreeks vinden | 
| <*Brontekst*> | Ja | Reeks | De subtekenreeks vinden | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde voor index*>| Geheel getal | De positie of index van de beginwaarde voor de opgegeven subtekenreeks. <p>Het getal -1 retourneren als de tekenreeks niet gevonden is, </br>Als de tekenreeks leeg is, retourneert de waarde 0. | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de beginwaarde van de index voor de subtekenreeks "wereld" in de tekenreeks 'Hallo wereld':

```
indexOf('hello world', 'world')
```

En dit resultaat wordt weergegeven: `6`

<a name="int"></a>

## <a name="int"></a>int

De versie van het geheel getal voor een tekenreeks retourneren.

```
int('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*resultaat van een geheel getal*> | Geheel getal | De versie van het geheel getal voor de opgegeven tekenreeks | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt een geheel getal-versie voor de tekenreeks '10':

```
int('10')
```

En dit resultaat wordt weergegeven: `10`

<a name="item"></a>

## <a name="item"></a>item

Wanneer gebruikt in een terugkerende actie via een matrix, retourneert u het huidige item in de matrix tijdens de huidige herhaling van de actie. U kunt ook de waarden van de eigenschappen van dat item ophalen. 

```
item()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*huidige-matrix-item*> | Alle | Het huidige item in de matrix voor de huidige herhaling van de actie | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de `body` element uit het huidige bericht voor de actie 'Send_an_email' binnen de huidige herhaling een voor elke lus:

```
item().body
```

<a name="items"></a>

## <a name="items"></a>items

Het huidige item van elke cyclus in een voor elke lus geretourneerd. Gebruik deze functie binnen het voor elke lus.

```
items('<loopName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Ja | Reeks | De naam voor de voor elke lus | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*item*> | Alle | Het item uit de huidige cyclus in de opgegeven voor elke lus | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt het huidige item opgehaald uit de opgegeven voor elke lus:

```
items('myForEachLoopName')
```

<a name="json"></a>

## <a name="json"></a>json

De waarde van type Notation JSON (JavaScript Object) of het object voor een tekenreeks of het XML-retourneren.

```
json('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Tekenreeks of XML | De tekenreeks of het XML-converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*JSON-resultaat*> | Systeemeigen JSON-type of object | De systeemeigen JSON-typewaarde of het object voor de opgegeven tekenreeks of XML. Als de tekenreeks null is, retourneert de functie een leeg object. | 
|||| 

*Voorbeeld 1* 

In het volgende voorbeeld wordt deze tekenreeks naar de JSON-waarde:

```
json('[1, 2, 3]')
```

En dit resultaat wordt weergegeven: `[1, 2, 3]`

*Voorbeeld 2*

In dit voorbeeld wordt deze tekenreeks geconverteerd naar JSON: 

```
json('{"fullName": "Sophia Owen"}')
```

En dit resultaat wordt weergegeven:

```
{
  "fullName": "Sophia Owen"
}
```

*Voorbeeld 3*

In dit voorbeeld wordt deze XML geconverteerd naar JSON: 

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

En dit resultaat wordt weergegeven:

```json
{ 
   "?xml": { "@version": "1.0" }, 
   "root": { 
      "person": [ { 
         "@id": "1", 
         "name": "Sophia Owen", 
         "occupation": "Engineer" 
       } ] 
   } 
}
```

<a name="intersection"></a>

## <a name="intersection"></a>snijpunt

Retourneert een verzameling met *alleen* de algemene items in de opgegeven verzamelingen. Een item moet worden weergegeven in het resultaat, worden weergegeven in de verzamelingen die zijn doorgegeven aan deze functie. Als een of meer items dezelfde naam hebben, is het laatste item met deze naam wordt weergegeven in het resultaat.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*collection2*>,... | Ja | Matrix of Object, maar niet beide | De verzamelingen van waar u *alleen* de algemene items | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*algemene items*> | Matrix of Object, respectievelijk | Een verzameling met alleen de algemene items in de gespecificeerde verzamelingen | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt gezocht naar de algemene items in deze matrices:  

```
intersection([1, 2, 3], [101, 2, 1, 10], [6, 8, 1, 2])
```

En retourneert een matrix met *alleen* deze items: `[1, 2]`

<a name="join"></a>

## <a name="join"></a>join

Retour een tekenreeks die alle items uit een matrix heeft en elk teken gescheiden door een *scheidingsteken*.

```
join([<collection>], '<delimiter>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Matrix | De matrix met de items om toe te voegen |  
| <*Scheidingsteken*> | Ja | Reeks | Het scheidingsteken dat wordt weergegeven tussen elke teken in de resulterende tekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Tekens1 voor*><*scheidingsteken*><*char2*><*scheidingsteken*>... | Reeks | De resulterende tekenreeks die is gemaakt op basis van de items in de opgegeven matrix |
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een tekenreeks van de items in deze matrix met het opgegeven teken als scheidingsteken:

```
join([a, b, c], '.')
```

En dit resultaat wordt weergegeven: `"a.b.c"`

<a name="last"></a>

## <a name="last"></a>laatste

Retourneert het laatste item uit een verzameling.

```
last('<collection>')
last([<collection>])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix | De verzameling waar vind ik het laatste item | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*laatste verzamelingsitem*> | Tekenreeks of matrix, respectievelijk | Het laatste item in de verzameling | 
|||| 

*Voorbeeld* 

Deze voorbeelden vinden het laatste item in deze verzamelingen:

```
last('abcd')
last([0, 1, 2, 3])
```

En deze resultaten retourneert: 

* Eerste voorbeeld: `"d"`
* Tweede voorbeeld: `3`

<a name="lastindexof"></a>

## <a name="lastindexof"></a>lastIndexOf

De laatste positie of index van de waarde voor een subtekenreeks retourneren. Deze functie is niet hoofdlettergevoelig en indexen beginnen bij nummer 0.

```
lastIndexOf('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks met de subtekenreeks vinden | 
| <*Brontekst*> | Ja | Reeks | De subtekenreeks vinden | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*beëindigen indexwaarde*> | Geheel getal | De positie of index van de eindwaarde voor de opgegeven subtekenreeks. <p>Het getal -1 retourneren als de tekenreeks niet gevonden is, </br>Als de tekenreeks leeg is, retourneert de waarde 0. | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de eindwaarde van de index voor de subtekenreeks "wereld" in de tekenreeks 'Hallo wereld':

```
lastIndexOf('hello world', 'world')
```

En dit resultaat wordt weergegeven: `10`

<a name="length"></a>

## <a name="length"></a>lengte

Retourneert het aantal items in een verzameling.

```
length('<collection>')
length([<collection>])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix | De verzameling met de items te tellen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*de lengte of count*> | Geheel getal | Het aantal items in de verzameling | 
|||| 

*Voorbeeld*

Deze voorbeelden Tel het aantal items in deze verzamelingen: 

```
length('abcd')
length([0, 1, 2, 3])
```

En dit resultaat te retourneren: `4`

<a name="less"></a>

## <a name="less"></a>minder

Controleer of de eerste waarde kleiner is dan de tweede waarde.
Retourneert waar wanneer de eerste waarde kleiner is of onwaar retourneren wanneer de eerste waarde meer is.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, Float of String | De eerste waarde om te controleren of kleiner zijn dan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, Float of String, respectievelijk | Het item vergelijking | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar als de eerste waarde kleiner is dan de tweede waarde. Retourneert onwaar als de eerste waarde gelijk aan of groter dan de tweede waarde is. | 
|||| 

*Voorbeeld*

Deze voorbeelden Controleer of de eerste waarde kleiner is dan de tweede waarde.

```
less(5, 10)
less('banana', 'apple')
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `true`
* Tweede voorbeeld: `false`

<a name="lessOrEquals"></a>

## <a name="lessorequals"></a>lessOrEquals

Controleer of de eerste waarde kleiner dan of gelijk zijn aan de tweede waarde.
Retourneert waar wanneer de eerste waarde kleiner dan of gelijk is of onwaar retourneren wanneer de eerste waarde meer is.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, Float of String | De eerste waarde om te controleren of kleiner zijn dan of gelijk aan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, Float of String, respectievelijk | Het item vergelijking | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR  | Boole-waarde | Retourneert waar wanneer de eerste waarde kleiner is dan of gelijk aan de tweede waarde. Retourneert onwaar als de eerste waarde groter dan de tweede waarde is. |  
|||| 

*Voorbeeld*

Deze voorbeelden kunt u controleren of de eerste waarde kleiner dan of gelijk dan de tweede waarde is.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `true`
* Tweede voorbeeld: `false`

<a name="listCallbackUrl"></a>

## <a name="listcallbackurl"></a>listCallbackUrl

De 'callback URL geretourneerd ' die een actie of -trigger aanroept. Deze functie werkt alleen met triggers en acties voor de **HttpWebhook** en **ApiConnectionWebhook** connector typt, maar niet de **handmatige**,  **Terugkeerpatroon**, **HTTP**, en **APIConnection** typen. 

```
listCallbackUrl()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*callback-URL*> | Reeks | De callback-URL voor een actie of trigger |  
|||| 

*Voorbeeld*

Dit voorbeeld ziet een URL van de callback voorbeeld dat deze functie mogelijk retourneren:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

## <a name="max"></a>max

Retourneert de hoogste waarde uit een lijst of een matrix met getallen die aan beide uiteinden liggen. 

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Getal1*>, <*getal2*>,... | Ja | Geheel getal, Float of beide | De set van getallen, van waaruit u wilt dat de hoogste waarde | 
| [<*Getal1*>, <*getal2*>,...] | Ja | Matrix - geheel getal, Float of beide | De matrix van getallen, van waaruit u wilt dat de hoogste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Max-waarde*> | Geheel getal of drijvende-kommawaarde | De hoogste waarde in de opgegeven matrix of een reeks getallen | 
|||| 

*Voorbeeld* 

Deze voorbeelden Haal de hoogste waarde van de reeks cijfers en de matrix:

```
max(1, 2, 3)
max([1, 2, 3])
```

En dit resultaat te retourneren: `3`

<a name="min"></a>

## <a name="min"></a>min.

Retourneert de laagste waarde uit een reeks cijfers of een matrix.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Getal1*>, <*getal2*>,... | Ja | Geheel getal, Float of beide | De set van getallen, van waaruit u wilt dat de laagste waarde | 
| [<*Getal1*>, <*getal2*>,...] | Ja | Matrix - geheel getal, Float of beide | De matrix van getallen, van waaruit u wilt dat de laagste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*min-waarde*> | Geheel getal of drijvende-kommawaarde | De laagste waarde in de opgegeven reeks cijfers of de opgegeven matrix | 
|||| 

*Voorbeeld* 

Deze voorbeelden ophalen de laagste waarde in de reeks cijfers en de matrix:

```
min(1, 2, 3)
min([1, 2, 3])
```

En dit resultaat te retourneren: `1`

<a name="mod"></a>

## <a name="mod"></a>Mod

Retourneert de rest van de deling van twee getallen. Als u de integer als resultaat, Zie [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*deeltal*> | Ja | Geheel getal of drijvende-kommawaarde | Het nummer te delen door het *deler* | 
| <*deler*> | Ja | Geheel getal of drijvende-kommawaarde | Het getal dat verdeelt de *deeltal*, maar mag niet 0 zijn. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*modulo resultaat*> | Geheel getal of drijvende-kommawaarde | De rest van de deling van het eerste getal door het tweede getal | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt het eerste getal door het tweede getal verdeeld:

```
mod(3, 2)
```

En dit resultaat te retourneren: `1`

<a name="mul"></a>

## <a name="mul"></a>mul

Het product van twee getallen vermenigvuldigen geretourneerd.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*multiplicand1*> | Ja | Geheel getal of drijvende-kommawaarde | Het aantal te vermenigvuldigen met *multiplicand2* | 
| <*multiplicand2*> | Ja | Geheel getal of drijvende-kommawaarde | Het getal dat veelvouden *multiplicand1* | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*product-resultaat*> | Geheel getal of drijvende-kommawaarde | Het product uit vermenigvuldigd met het eerste getal door het tweede getal | 
|||| 

*Voorbeeld* 

Deze voorbeelden meerdere het eerste getal door het tweede getal:

```
mul(1, 2)
mul(1.5, 2)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: `2`
* Tweede voorbeeld `3`

<a name="multipartBody"></a>

## <a name="multipartbody"></a>multipartBody

De instantie voor een bepaald deel in de uitvoer van een actie die meerdere onderdelen heeft retourneren.

```
multipartBody('<actionName>', <index>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie die is uitgevoerd met meerdere onderdelen | 
| <*Index*> | Ja | Geheel getal | De waarde voor de index voor het onderdeel dat u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Hoofdtekst*> | Reeks | De instantie voor het opgegeven onderdeel | 
|||| 

<a name="not"></a>

## <a name="not"></a>niet

Controleer of een expressie ingesteld op false is. Retourneert waar wanneer de expressie ingesteld op false is, of ONWAAR wanneer de waarde true retourneren.

```
not(<expression>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*expressie*> | Ja | Boole-waarde | De expressie om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | Retourneert waar als de expressie false is. Retourneert onwaar als de expressie waar is. |  
|||| 

*Voorbeeld 1*

Deze voorbeelden kunt u controleren of de opgegeven expressies false zijn: 

```
not(false)
not(true)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: de expressie is ingesteld op false, zodat de functie retourneert `true`.
* Tweede voorbeeld: de expressie is waar, zodat de functie retourneert `false`.

*Voorbeeld 2*

Deze voorbeelden kunt u controleren of de opgegeven expressies false zijn: 

```
not(equals(1, 2))
not(equals(1, 1))
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: de expressie is ingesteld op false, zodat de functie retourneert `true`.
* Tweede voorbeeld: de expressie is waar, zodat de functie retourneert `false`.

<a name="or"></a>

## <a name="or"></a>of

Controleer of er ten minste één expressie waar is. Retourneert waar wanneer ten minste één expressie waar is of onwaar retourneren wanneer alle ingesteld op false zijn.

```
or(<expression1>, <expression2>, ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Expression1*>, <*expression2*>,... | Ja | Boole-waarde | De expressies om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Boole-waarde | True wordt geretourneerd wanneer ten minste één expressie waar is. Geretourneerde waarde false als alle expressies ingesteld op false zijn. |  
|||| 

*Voorbeeld 1*

Deze voorbeelden kunt u controleren of er ten minste één expressie waar is:

```
or(true, false)
or(false, false)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: ten minste één expressie is waar, zodat de functie retourneert `true`.
* Tweede voorbeeld: beide expressies zijn ingesteld op false, zodat de functie retourneert `false`.

*Voorbeeld 2*

Deze voorbeelden kunt u controleren of er ten minste één expressie waar is:

```
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: ten minste één expressie is waar, zodat de functie retourneert `true`.
* Tweede voorbeeld: beide expressies zijn ingesteld op false, zodat de functie retourneert `false`.

<a name="parameters"></a>

## <a name="parameters"></a>parameters

Retourneert de waarde voor een parameter die wordt beschreven in de definitie van de logische app. 

```
parameters('<parameterName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*parameterName*> | Ja | Reeks | De naam voor de parameter waarvan de gewenste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde van parameter*> | Alle | De waarde voor de opgegeven parameter | 
|||| 

*Voorbeeld* 

Stel dat u hebt deze JSON-waarde:

```json
{
  "fullName": "Sophia Owen"
}
```

In dit voorbeeld wordt de waarde voor de opgegeven parameter:

```
parameters('fullName')
```

En dit resultaat wordt weergegeven: `"Sophia Owen"`

<a name="rand"></a>

## <a name="rand"></a>rand

Een willekeurige geheel getal retourneren vanaf een opgegeven adresbereik op dat alleen aan het einde van de begindatum liggen wordt.

```
rand(<minValue>, <maxValue>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*MinValue*> | Ja | Geheel getal | De laagste geheel getal | 
| <*MaxValue*> | Ja | Geheel getal | Het gehele getal dat volgt op het hoogste gehele getal in het bereik dat de functie terugkeren kunt. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*willekeurige resultaat*> | Geheel getal | De willekeurig geheel geretourneerd uit het opgegeven bereik |  
|||| 

*Voorbeeld*

In dit voorbeeld wordt een willekeurig geheel opgehaald uit het opgegeven bereik, met uitzondering van de maximale waarde: 

```
rand(1, 5)
```

En retourneert een van deze getallen als gevolg: `1`, `2`, `3`, of `4` 

<a name="range"></a>

## <a name="range"></a>bereik

Retourneert een matrix die wordt gestart van een geheel getal zijn opgegeven.

```
range(<startIndex>, <count>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*startIndex*> | Ja | Geheel getal | De waarde van geheel getal dat de matrix als het eerste item begint | 
| <*Aantal*> | Ja | Geheel getal | Het aantal getallen in de matrix | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*bereik resultaat*>] | Matrix | De matrix met gehele getallen vanaf de opgegeven index |  
|||| 

*Voorbeeld*

In dit voorbeeld wordt een matrix die wordt gestart vanuit de opgegeven index en heeft het opgegeven aantal gehele getallen zijn gemaakt:

```
range(1, 4)
```

En dit resultaat wordt weergegeven: `[1, 2, 3, 4]`

<a name="replace"></a>

## <a name="replace"></a>vervangen

Een subtekenreeks vervangen door de opgegeven tekenreeks en de resultaattekenreeks geretourneerd. Deze functie is hoofdlettergevoelig.

```
replace('<text>', '<oldText>', '<newText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks met de subtekenreeks vervangen | 
| <*OudeTekst*> | Ja | Reeks | De subtekenreeks vervangen | 
| <*NieuweTekst*> | Ja | Reeks | De vervangende tekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tekst*> | Reeks | De bijgewerkte tekenreeks na de subtekenreeks vervangen <p>Als de subtekenreeks niet wordt gevonden, retourneert u de oorspronkelijke reeks. | 
|||| 

*Voorbeeld* 

In dit voorbeeld zoekt de subtekenreeks 'oude' in 'de oude tekenreeks' en 'oude' met 'new' vervangt: 

```
replace('the old string', 'old', 'new')
```

En dit resultaat wordt weergegeven: `"the new string"`

<a name="removeProperty"></a>

## <a name="removeproperty"></a>removeProperty

Een eigenschap van een object verwijderen en het bijgewerkte object retourneren.

```
removeProperty(<object>, '<property>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Ja | Object | De JSON-object waaruit u wilt een eigenschap verwijderen | 
| <*Eigenschap*> | Ja | Reeks | De naam op voor de eigenschap verwijderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt-object*> | Object | De bijgewerkte JSON-object zonder dat de opgegeven eigenschap | 
|||| 

*Voorbeeld*

In dit voorbeeld verwijdert u de `"accountLocation"` eigenschap van een `"customerProfile"` -object, dat wordt geconverteerd naar JSON met de [JSON()](#json) functioneren en retourneert het bijgewerkte object:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

## <a name="setproperty"></a>setProperty

Stel de waarde voor eigenschap van een object en het bijgewerkte object retourneren. U kunt deze functie gebruiken om een nieuwe eigenschap toevoegen, of de [addProperty()](#addProperty) functie.

```
setProperty(<object>, '<property>', <value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Ja | Object | De JSON-object waarvan de eigenschap wilt instellen | 
| <*Eigenschap*> | Ja | Reeks | De naam op voor de bestaande of nieuwe eigenschap instellen | 
| <*Waarde*> | Ja | Alle | De waarde in te stellen voor de opgegeven eigenschap |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt-object*> | Object | De bijgewerkte JSON-object waarvan de eigenschap instellen | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `"accountNumber"` -eigenschap op een `"customerProfile"` -object, dat wordt geconverteerd naar JSON met de [JSON()](#json) functie. De functie die worden gegenereerd door de waarde wordt toegewezen [guid()](#guid) functioneren en retourneert het bijgewerkte JSON-object:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

## <a name="skip"></a>Overslaan

Items verwijderen uit het begin van een verzameling en terug te keren *alle andere* items.

```
skip([<collection>], <count>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Matrix | De verzameling waarvan items die u wilt verwijderen | 
| <*Aantal*> | Ja | Geheel getal | Een positief geheel getal voor het aantal items vooraan verwijderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*bijgewerkt verzameling*>] | Matrix | De bijgewerkte verzameling na het verwijderen van de opgegeven items | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt één item, het getal 0, vanaf het begin van de opgegeven matrix verwijderd. 

```
skip([0, 1, 2, 3], 1)
```

En retourneert deze matrix met de overige items: `[1,2,3]`

<a name="split"></a>

## <a name="split"></a>split

Retour een matrix die alle tekens uit een tekenreeks bevat en elk teken is gescheiden door een *scheidingsteken*.

```
split('<text>', '<separator>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks met de tekens splitsen |  
| <*Scheidingsteken*> | Ja | Reeks | Het scheidingsteken dat wordt weergegeven tussen elke teken in de resulterende matrix | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*Tekens1 voor*><*scheidingsteken*><*char2*><*scheidingsteken*>...] | Matrix | De resulterende matrix gemaakt op basis van de items in de opgegeven tekenreeks |
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een matrix van de opgegeven tekenreeks, elk teken scheiden met een komma als scheidingsteken:

```
split('abc', ',')
```

En dit resultaat wordt weergegeven: `[a, b, c]`

<a name="startOfDay"></a>

## <a name="startofday"></a>startOfDay

Het begin van de dag van een tijdstempel retourneren. 

```
startOfDay('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De opgegeven tijdstempel maar vanaf de markering nul uur voor de dag | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt gezocht naar het begin van de dag van dit tijdstempel:

```
startOfDay('2018-03-15T13:30:30Z')
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

## <a name="startofhour"></a>startOfHour

Het begin van het uur voor een tijdstempel retourneren. 

```
startOfHour('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De opgegeven tijdstempel maar vanaf de markering nul minuut voor het uur | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt gezocht naar het begin van het uur voor dit tijdstempel:

```
startOfHour('2018-03-15T13:30:30Z')
```

En dit resultaat wordt weergegeven: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

## <a name="startofmonth"></a>startOfMonth

Het begin van de maand voor een tijdstempel retourneren. 

```
startOfMonth('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De opgegeven tijdstempel maar vanaf de eerste dag van de maand nul uur | 
|||| 

*Voorbeeld* 

In dit voorbeeld retourneert het begin van de maand voor dit tijdstempel:

```
startOfMonth('2018-03-15T13:30:30Z')
```

En dit resultaat wordt weergegeven: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

## <a name="startswith"></a>StartsWith

Controleren of een tekenreeks met een specifieke subtekenreeks begint. Retourneert waar wanneer de subtekenreeks is gevonden of onwaar retourneren wanneer niet gevonden. Deze functie is niet hoofdlettergevoelig.

```
startsWith('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks om te controleren | 
| <*Brontekst*> | Ja | Reeks | De eerste tekenreeks vinden | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR  | Boole-waarde | Retourneert waar wanneer de eerste subtekenreeks is gevonden. Retourneert onwaar wanneer het niet gevonden. | 
|||| 

*Voorbeeld 1* 

Dit voorbeeld wordt gecontroleerd of de tekenreeks "Hallo wereld" met de subtekenreeks 'Hallo begint':

```
startsWith('hello world', 'hello')
```

En dit resultaat wordt weergegeven: `true`

*Voorbeeld 2*

Dit voorbeeld wordt gecontroleerd of de tekenreeks "Hallo wereld" met de subtekenreeks 'begroetingen begint':

```
startsWith('hello world', 'greetings')
```

En dit resultaat wordt weergegeven: `false`

<a name="string"></a>

## <a name="string"></a>tekenreeks

De tekenreeksversie voor een waarde retourneren.

```
string(<value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Alle | De waarde te converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*String-waarde*> | Reeks | De tekenreeksversie voor de opgegeven waarde | 
|||| 

*Voorbeeld 1* 

In dit voorbeeld wordt de tekenreeksversie voor dit nummer:

```
string(10)
```

En dit resultaat wordt weergegeven: `"10"`

*Voorbeeld 2*

In dit voorbeeld wordt een tekenreeks op voor het opgegeven JSON-object gemaakt en gebruikt het backslash-teken (\\) als een escapeteken voor het aanhalingsteken (").

```
string( { "name": "Sophie Owen" } )
```

En dit resultaat wordt weergegeven: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

## <a name="sub"></a>Sub

Geeft het resultaat van het tweede nummer van het eerste nummer af te trekken.

```
sub(<minuend>, <subtrahend>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*minuend*> | Ja | Geheel getal of drijvende-kommawaarde | Het getal waarvan u wilt de *subtrahend* | 
| <*subtrahend*> | Ja | Geheel getal of drijvende-kommawaarde | Het getal moet worden afgetrokken van de *minuend* | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Resultaat*> | Geheel getal of drijvende-kommawaarde | Het resultaat van het tweede nummer van het eerste nummer af te trekken | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt afgetrokken van het tweede nummer van het eerste nummer:

```
sub(10.3, .3)
```

En dit resultaat wordt weergegeven: `10`

<a name="substring"></a>

## <a name="substring"></a>de subtekenreeks

Tekens retourneren uit een reeks vanaf de opgegeven positie of index. Index waarden starten met de waarde 0. 

```
substring('<text>', <startIndex>, <length>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks waarvan de gewenste tekens | 
| <*startIndex*> | Ja | Geheel getal | Een positief getal voor de beginpositie of indexwaarde | 
| <*lengte*> | Ja | Geheel getal | Een positief getal tekens bevat die u wilt dat in de subtekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*de subtekenreeks resultaat*> | Reeks | Een subtekenreeks met het opgegeven aantal tekens, vanaf de opgegeven index voor de positie in de brontekenreeks | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een subtekenreeks vijf tekens van de opgegeven tekenreeks is de waarde voor de index 6 vanaf:

```
substring('hello world', 6, 5)
```

En dit resultaat wordt weergegeven: `"world"`

<a name="subtractFromTime"></a>

## <a name="subtractfromtime"></a>subtractFromTime

Een aantal tijdseenheden van een tijdstempel afgetrokken. Zie ook [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks met de timestamp | 
| <*Interval*> | Ja | Geheel getal | Het aantal eenheden opgegeven tijdstip moet worden afgetrokken | 
| <*timeUnit*> | Ja | Reeks | Tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', "Maand", "Jaar" | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tijdstempel*> | Reeks | De tijdstempel minus het opgegeven aantal tijdseenheden | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt afgetrokken van één dag van dit tijdstempel:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day') 
```

En dit resultaat wordt weergegeven: `"2018-01-01T00:00:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt afgetrokken van één dag van dit tijdstempel:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D') 
```

En dit resultaat wordt weergegeven met de optionele "D"-indeling: `"Monday, January, 1, 2018"`

<a name="take"></a>

## <a name="take"></a>duren

Items retourneren vanaf het begin van een verzameling. 

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix | De verzameling waarvan items die u wilt | 
| <*Aantal*> | Ja | Geheel getal | Een positief geheel getal voor het aantal items die u vanaf het begin wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*subset*> of [<*subset*>] | Tekenreeks of matrix, respectievelijk | Een tekenreeks of een matrix met het opgegeven aantal items die afkomstig zijn uit het begin van de oorspronkelijke verzameling | 
|||| 

*Voorbeeld*

Het opgegeven aantal items ophalen in deze voorbeelden vanaf het begin van deze verzamelingen:

```
take('abcde`, 3)
take([0, 1, 2, 3, 4], 3)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: `"abc"`
* Tweede voorbeeld: `[0, 1, 2]`

<a name="ticks"></a>

## <a name="ticks"></a>tikken

Retourneert de `ticks` eigenschapwaarde voor een tijdstempel voor een opgegeven. Een *maatstreepjes* is van een interval van 100 nanoseconden.

```
ticks('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*tijdstempel*> | Ja | Reeks | De tekenreeks voor een tijdstempel | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*ticks getal*> | Geheel getal | Het aantal ticks sinds de opgegeven tijdstempel | 
|||| 

<a name="toLower"></a>

## <a name="tolower"></a>toLower

Retourneert een tekenreeks in kleine letter. Als een teken in de tekenreeks is geen in kleine letters, blijft dit teken ongewijzigd in de tekenreeks geretourneerd.

```
toLower('<text>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks om in kleine letters indeling te retourneren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*kleine tekst*> | Reeks | De oorspronkelijke reeks in kleine letters-indeling | 
|||| 

*Voorbeeld* 

In dit voorbeeld converteert deze tekenreeks naar kleine letters: 

```
toLower('Hello World')
```

En dit resultaat wordt weergegeven: `"hello world"`

<a name="toUpper"></a>

## <a name="toupper"></a>toUpper

Retourneert een tekenreeks in hoofdletters. Als een teken in de tekenreeks is geen hoofdletters, blijft dit teken ongewijzigd in de tekenreeks geretourneerd.

```
toUpper('<text>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks om in hoofdletters te retourneren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*hoofdletters tekst*> | Reeks | De oorspronkelijke reeks hoofdletters | 
|||| 

*Voorbeeld* 

In dit voorbeeld converteert deze tekenreeks naar hoofdletters:

```
toUpper('Hello World')
```

En dit resultaat wordt weergegeven: `"HELLO WORLD"`

<a name="trigger"></a>

## <a name="trigger"></a>Trigger

De uitvoer van een trigger in runtime of waarden van andere JSON naam en waarde-paren, die u aan een expressie toewijzen kunt geretourneerd. 

* Binnen een trigger-invoer retourneert deze functie de uitvoer van de vorige uitvoering. 

* Binnen een triggervoorwaarde retourneert deze functie de uitvoer van de huidige uitvoering. 

Standaard de functie verwijst naar het hele triggerobject, maar u kunt optioneel een eigenschap opgeven waarvan de waarde die u wilt. Ook deze functie heeft steno versies die beschikbaar zijn, Zie [triggerOutputs()](#triggerOutputs) en [triggerBody()](#triggerBody). 

```
trigger()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*trigger-uitvoer*> | Reeks | De uitvoer van een trigger tijdens runtime | 
|||| 

<a name="triggerBody"></a>

## <a name="triggerbody"></a>triggerBody

Retourneren van een trigger `body` uitvoer tijdens runtime. Steno voor `trigger().outputs.body`. Zie [trigger()](#trigger). 

```
triggerBody()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*instantie-trigger-uitvoer*> | Reeks | De `body` de uitvoer van de trigger | 
|||| 

<a name="triggerFormDataMultiValues"></a>

## <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

Retourneert een matrix met waarden die overeenkomen met de naam van een sleutel in een trigger *formuliergegevens* of *formulier gecodeerd* uitvoer. 

```
triggerFormDataMultiValues('<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan de gewenste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*matrix met sleutel waarden*>] | Matrix | Een matrix met de waarden die overeenkomen met de opgegeven sleutel | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een matrix van de sleutelwaarde van 'feedUrl' in een RSS-trigger formuliergegevens of formulier gecodeerde uitvoer: 

```
triggerFormDataMultiValues('feedUrl')
```

En deze matrix retourneert als het resultaat van een voorbeeld: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

## <a name="triggerformdatavalue"></a>triggerFormDataValue

Retourneert een tekenreeks met één waarde die overeenkomt met de naam van een sleutel in een trigger *formuliergegevens* of *formulier gecodeerd* uitvoer. Als de functie meer dan een overeenkomst wordt gevonden, wordt in de functie een fout genereert.

```
triggerFormDataValue('<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan de gewenste waarde |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*sleutel / waarde-*> | Reeks | De waarde in de opgegeven sleutel | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een tekenreeks van de sleutelwaarde van 'feedUrl' in een RSS-trigger formuliergegevens of formulier gecodeerde uitvoer:

```
triggerFormDataValue('feedUrl')
```

En deze tekenreeks geretourneerd als het resultaat van een voorbeeld: `"http://feeds.reuters.com/reuters/topNews"` 

<a name="triggerMultipartBody"></a>

De instantie voor een bepaald deel in de uitvoer van een trigger die meerdere onderdelen heeft retourneren. 

```
triggerMultipartBody(<index>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Index*> | Ja | Geheel getal | De waarde voor de index voor het onderdeel dat u wilt |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Hoofdtekst*> | Reeks | De instantie voor het opgegeven onderdeel in een trigger meerdelige uitvoer | 
|||| 

<a name="triggerOutputs"></a>

## <a name="triggeroutputs"></a>triggerOutputs

De uitvoer van een trigger in runtime of waarden van andere JSON-naam en waarde-paren worden geretourneerd. Steno voor `trigger().outputs`. Zie [trigger()](#trigger). 

```
triggerOutputs()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*trigger-uitvoer*> | Reeks | De uitvoer van een trigger tijdens runtime  | 
|||| 

<a name="trim"></a>

## <a name="trim"></a>Trim

Voorloop-en volgspaties verwijderen uit een tekenreeks en de bijgewerkte tekenreeks retourneren.

```
trim('<text>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks met de witruimte voorloop- en volgspaties te verwijderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*updatedText*> | Reeks | Een bijgewerkte versie voor de oorspronkelijke reeks zonder voorloop- of volgspaties witruimte | 
|||| 

*Voorbeeld* 

In dit voorbeeld worden de witruimte voorloop- en volgspaties uit de tekenreeks "Hallo wereld" verwijderd:  

```
trim(' Hello World  ')
```

En dit resultaat wordt weergegeven: `"Hello World"`

<a name="union"></a>

## <a name="union"></a>Union

Retourneert een verzameling met *alle* de items van de gespecificeerde verzamelingen. In het resultaat wordt weergegeven, kan een item in een verzameling die is doorgegeven aan deze functie worden weergegeven. Als een of meer items dezelfde naam hebben, is het laatste item met deze naam wordt weergegeven in het resultaat. 

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*collection2*>,...  | Ja | Matrix of Object, maar niet beide | De verzamelingen van waar u *alle* de items | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*updatedCollection*> | Matrix of Object, respectievelijk | Een verzameling met alle items uit de gespecificeerde verzamelingen - geen duplicaten | 
|||| 

*Voorbeeld* 

In dit voorbeeld opgehaald *alle* de items van deze verzamelingen: 

```
union([1, 2, 3], [1, 2, 10, 101])
```

En dit resultaat wordt weergegeven: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

## <a name="uricomponent"></a>uriComponent

Retourneert een uniform resource identifier (URI) gecodeerde versie naar een tekenreeks URL onveilige tekens te vervangen door een escape-tekens. Deze functie wilt gebruiken in plaats [encodeUriComponent()](#encodeUriComponent). Hoewel beide functies op dezelfde manier werken `uriComponent()` verdient de voorkeur.

```
uriComponent('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks converteren naar URI-notatie | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*codering-uri*> | Reeks | De URI-gecodeerde tekenreeks met escape-tekens | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt een URI-codering versie voor deze tekenreeks:

```
uriComponent('https://contoso.com')
```

En dit resultaat wordt weergegeven: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

## <a name="uricomponenttobinary"></a>uriComponentToBinary

Retourneren van de binaire versie voor een uniform resource identifier (URI)-onderdeel.

```
uriComponentToBinary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De URI-gecodeerde tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-gecodeerd-uri*> | Reeks | De binaire versie voor de URI-gecodeerde tekenreeks. De binaire inhoud wordt base64-gecodeerd en dat wordt vertegenwoordigd door `$content`. | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt de binaire versie voor deze URI-gecodeerde tekenreeks: 

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

En dit resultaat wordt weergegeven: 

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

## <a name="uricomponenttostring"></a>uriComponentToString

Retourneert dat de tekenreeksversie voor een uniform resource identifier (URI)-gecodeerde tekenreeks, effectief decoderen van de URI-gecodeerde tekenreeks.

```
uriComponentToString('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De URI-gecodeerde tekenreeks decoderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-uri*> | Reeks | De versie gedecodeerde voor de URI-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de tekenreeksversie gedecodeerde voor deze URI-gecodeerde tekenreeks: 

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

En dit resultaat wordt weergegeven: `"https://contoso.com"` 

<a name="uriHost"></a>

## <a name="urihost"></a>uriHost

Retourneert de `host` waarde voor een uniform resource identifier (URI).

```
uriHost('<uri>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Ja | Reeks | De URI waarvan `host` waarde die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde van de host*> | Reeks | De `host` waarde voor de opgegeven URI | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt gezocht naar de `host` waarde voor deze URI: 

```
uriHost('https://www.localhost.com:8080')
```

En dit resultaat wordt weergegeven: `"www.localhost.com"`

<a name="uriPath"></a>

## <a name="uripath"></a>uriPath

Retourneert de `path` waarde voor een uniform resource identifier (URI). 

```
uriPath('<uri>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Ja | Reeks | De URI waarvan `path` waarde die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*pad-waarde*> | Reeks | De `path` waarde voor de opgegeven URI. Als `path` niet een waarde hebben, kunt u het teken '/' retourneren. | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt gezocht naar de `path` waarde voor deze URI: 

```
uriPath('http://www.contoso.com/catalog/shownew.htm?date=today')
```

En dit resultaat wordt weergegeven: `"/catalog/shownew.htm"`

<a name="uriPathAndQuery"></a>

## <a name="uripathandquery"></a>uriPathAndQuery

Retourneert de `path` en `query` waarden voor een uniform resource identifier (URI).

```
uriPathAndQuery('<uri>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Ja | Reeks | De URI waarvan `path` en `query` waarden die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*pad querywaarde*> | Reeks | De `path` en `query` waarden voor de opgegeven URI. Als `path` niet een waarde opgeeft, worden geretourneerd van het teken '/'. | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt gezocht naar de `path` en `query` waarden voor deze URI:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

En dit resultaat wordt weergegeven: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

## <a name="uriport"></a>uriPort

Retourneert de `port` waarde voor een uniform resource identifier (URI).

```
uriPort('<uri>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Ja | Reeks | De URI waarvan `port` waarde die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*poort-waarde*> | Geheel getal | De `port` waarde voor de opgegeven URI. Als `port` niet een waarde opgeeft, worden de standaardpoort voor het protocol retourneren. | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de `port` waarde voor deze URI:

```
uriPort('http://www.localhost:8080')
```

En dit resultaat wordt weergegeven: `8080`

<a name="uriQuery"></a>

## <a name="uriquery"></a>uriQuery

Retourneert de `query` waarde voor een uniform resource identifier (URI).

```
uriQuery('<uri>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Ja | Reeks | De URI waarvan `query` waarde die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde van de query*> | Reeks | De `query` waarde voor de opgegeven URI | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de `query` waarde voor deze URI: 

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

En dit resultaat wordt weergegeven: `"?date=today"`

<a name="uriScheme"></a>

## <a name="urischeme"></a>UriScheme

Retourneert de `scheme` waarde voor een uniform resource identifier (URI).

```
uriScheme('<uri>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Ja | Reeks | De URI waarvan `scheme` waarde die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*schema-waarde*> | Reeks | De `scheme` waarde voor de opgegeven URI | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de `scheme` waarde voor deze URI:

```
uriScheme('http://www.contoso.com/catalog/shownew.htm?date=today')
```

En dit resultaat wordt weergegeven: `"http"`

<a name="utcNow"></a>

## <a name="utcnow"></a>utcNow

De huidige tijdstempel retourneren. 

```
utcNow('<format>')
```

Desgewenst kunt u een andere indeling met de <*indeling*> parameter. 


| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Indeling*> | Nee | Reeks | Ofwel een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepaste indeling patroon](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en informatie over de tijdzone behoudt. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Huidig timestamp*> | Reeks | De huidige datum en tijd | 
|||| 

*Voorbeeld 1*

Stel dat vandaag is 15 April 2018 op 1:00:00 PM. In dit voorbeeld wordt de huidige tijdstempel opgehaald: 

```
utcNow()
```

En dit resultaat wordt weergegeven: `"2018-04-15T13:00:00.0000000Z"`

*Voorbeeld 2*

Stel dat vandaag is 15 April 2018 op 1:00:00 PM. In dit voorbeeld wordt de huidige tijdstempel met de optionele "D"-indeling:

```
utcNow('D')
```

En dit resultaat wordt weergegeven: `"Sunday, April 15, 2018"`

<a name="variables"></a>

## <a name="variables"></a>variabelen

De waarde voor een opgegeven variabele retourneren. 

```
variables('<variableName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Variabelenaam*> | Ja | Reeks | De naam voor de variabele waarvan de gewenste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde variabele*> | Alle | De waarde voor de opgegeven variabele | 
|||| 

*Voorbeeld*

Stel dat de huidige waarde voor een variabele 'numItems' is 20. In dit voorbeeld wordt de integerwaarde voor deze variabele:

```
variables('numItems')
```

En dit resultaat wordt weergegeven: `20`

<a name="workflow"></a>

## <a name="workflow"></a>werkstroom

Retourneert de details over de werkstroom zichzelf tijdens runtime. 

```
workflow().<property>
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Eigenschap*> | Nee | Reeks | De naam op voor de eigenschap van de werkstroom waarvan de gewenste waarde <p>Een werkstroomobject heeft deze eigenschappen: **naam**, **type**, **id**, **locatie**, en **uitvoeren**. De **uitvoeren** eigenschapswaarde is ook een object waarvoor deze eigenschappen: **naam**, **type**, en **id**. | 
||||| 

*Voorbeeld*

In dit voorbeeld retourneert de naam voor de huidige run een werkstroom:

```
workflow().run.name
```

<a name="xml"></a>

## <a name="xml"></a>xml

De XML-versie voor een tekenreeks met een JSON-object retourneren. 

```
xml('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks met de JSON-object converteren <p>De JSON-object moet slechts één hoofd-eigenschap hebben. <br>Gebruik het backslash-teken (\\) als een escape-teken voor de dubbele aanhalingstekens ("). | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*XML-versie*> | Object | De gecodeerde XML voor de opgegeven tekenreeks of het JSON-object | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt de XML-versie voor deze tekenreeks die een JSON-object bevat: 

`xml( '{ \"name\": \"Sophia Owen\" }' )`

En wordt dit resultaat-XML: 

```xml
<name>Sophia Owen</name>
```

*Voorbeeld 2*

Stel dat u hebt dit JSON-object:

```json
{ 
  "person": { 
    "name": "Sophia Owen", 
    "city": "Seattle" 
  } 
}
```

In dit voorbeeld maakt XML voor een tekenreeks zijn met dit JSON-object:

`xml( '{ \"person\": { \"name\": \"Sophia Owen\", \"city\": \"Seattle\" } }' )`

En wordt dit resultaat-XML: 

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

## <a name="xpath"></a>XPath

Controleer de XML-gegevens voor knooppunten of waarden die overeenkomen met een expressie XPath (XML Path Language) en de overeenkomende knooppunten of waarden retourneren. Een XPath-expressie of alleen 'XPath', kunt u een XML-documentstructuur navigeren, zodat u knooppunten of compute-waarden in de XML-inhoud kunt selecteren.

```
xpath('<xml>', '<xpath>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*XML*> | Ja | Alle | De XML-tekenreeks te zoeken naar knooppunten of waarden die overeenkomen met de waarde van een XPath-expressie | 
| <*XPath*> | Ja | Alle | De XPath-expressie gebruikt voor het zoeken van overeenkomende waarden of XML-knooppunt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*XML-knooppunt*> | XML | Een XML-knooppunt wanneer slechts één knooppunt overeenkomt met de opgegeven XPath-expressie | 
| <*Waarde*> | Alle | De waarde van een XML-knooppunt wanneer slechts één waarde overeenkomt met de opgegeven XPath-expressie | 
| [<*xml knooppunt1*>, <*xml Knooppunt2*>,...] </br>-of- </br>[<*value1*>, <*value2*>,...] | Matrix | Een matrix met XML-knooppunt of waarden die overeenkomen met de opgegeven XPath-expressie | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt gezocht naar knooppunten die overeenkomen met de `<name></name>` knooppunt in de opgegeven argumenten en retourneert een matrix met deze knooppuntwaarden: 

`xpath(xml(parameters('items')), '/produce/item/name')`

Hier volgen de argumenten:

* De tekenreeks 'items', die deze XML-code bevat:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  In het voorbeeld wordt de [parameters()](#parameters) werken voor het ophalen van de XML-tekenreeks uit het argument 'items', maar moet ook de tekenreeks converteren naar XML-indeling met behulp van de [xml()](#xml) functie. 

* Deze XPath-expressie die wordt doorgegeven als een tekenreeks:

  `"/produce/item/name"`

Hier volgt de resultaat-matrix met knooppunten die overeenkomen met `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*Voorbeeld 2*

Dit voorbeeld van de te volgen in voorbeeld 1, wordt gezocht naar de knooppunten die overeenkomen met de `<count></count>` knooppunt en voegt deze knooppuntwaarden met de `sum()` functie:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

En dit resultaat wordt weergegeven: `30`

*Voorbeeld 3*

In dit voorbeeld vinden beide expressies knooppunten die overeenkomen met de `<location></location>` knooppunt in de opgegeven argumenten, waaronder de XML met een naamruimte. De expressies gebruiken het backslash-teken (\\) als een escape-teken voor de dubbele aanhalingstekens (").

* *Expressie 1*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *Expressie 2* 

  `xpath(xml(body('Http')), '/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]')`

Hier volgen de argumenten:

* Deze XML, waaronder de XML-document naamruimte `xmlns="http://contoso.com"`: 

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* De XPath-expressie hier:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]`

Hier wordt het resultaat-knooppunt dat overeenkomt met de `<location></location` knooppunt:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Voorbeeld 4*

In dit voorbeeld te volgen in voorbeeld 3, wordt de waarde in de `<location></location>` knooppunt: 

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

En dit resultaat wordt weergegeven: `"Paris"`

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [werkstroom Definition Language](../logic-apps/logic-apps-workflow-definition-language.md)

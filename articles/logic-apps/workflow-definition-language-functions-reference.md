---
title: Naslaginformatie over functies voor Definitietaal van werkstroom - Azure Logic Apps | Microsoft Docs
description: Meer informatie over werkstroom Definition Language-functies voor Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 08/15/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 0f2543ff10f19d6f1ccd656855dbb41cf42e6ae2
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018992"
---
# <a name="functions-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Functiereferentie voor Definitietaal van werkstroom in Azure Logic Apps

Sommige [expressies](../logic-apps/logic-apps-workflow-definition-language.md#expressions) in [Azure Logic Apps](../logic-apps/logic-apps-overview.md) worden de waarden van de runtime-acties die mogelijk nog niet wanneer de definitie van de werkstroom van de logische app begint te lopen. U kunt gebruiken om te verwijzen naar of te werken met deze waarden in expressies, *functies* geleverd door de [Werkstroomdefinitietaal](../logic-apps/logic-apps-workflow-definition-language.md). Bijvoorbeeld, kunt u wiskundige functies voor berekeningen, zoals de [add()](../logic-apps/workflow-definition-language-functions-reference.md#add) functie, de som van gehele getallen of tekst berekent. Hier volgen enkele meer voorbeeld kunt u taken met functions uitvoeren:

| Taak | Syntaxis van de functie | Resultaat | 
| ---- | --------------- | ------ | 
| Retourneert een tekenreeks in kleine letters indeling. | toLower ('<*tekst*>') <p>Bijvoorbeeld: toLower('Hello') | "Hallo" | 
| Retourneert een globally unique identifier (GUID). | GUID() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Dit artikel beschrijft de functies die u gebruiken kunt bij het maken van uw logische app-definities.
Functies moeten worden gezocht [op basis van hun algemene](#ordered-by-purpose), gaat u verder met de volgende tabellen. Of Zie voor gedetailleerde informatie over elke functie, de [alfabetische lijst](#alphabetical-list). 

> [!NOTE]
> De syntaxis voor parameterdefinities, zijn een vraagteken (?) die wordt weergegeven nadat een parameter betekent de parameter dat is optioneel. Zie bijvoorbeeld [getFutureTime()](#getFutureTime).

## <a name="functions-in-expressions"></a>Functies in expressies

Als u wilt weergeven over het gebruik van een functie in een expressie, in dit voorbeeld toont hoe krijgt u de waarde van de `customerName` parameter en wijs die waarde die moet worden de `accountName` eigenschap met behulp van de [parameters()](#parameters) functie in een expressie:

```json
"accountName": "@parameters('customerName')"
```

Hier volgen enkele andere algemene manieren dat u functies kunt gebruiken in expressies voor:

| Taak | Syntaxis van de functie in een expressie | 
| ---- | -------------------------------- | 
| Werken met een item door door te geven dat item aan een functie uitvoeren. | "\@<*functienaam*> (<*item*>)" | 
| 1. Krijgen de *parameterName*van waarde met behulp van de geneste `parameters()` functie. </br>2. Werken met het resultaat uitvoeren door door te geven die waarde aan *functienaam*. | "\@<*functienaam*> (parameters ('<*parameterName*>")) " | 
| 1. Het resultaat ophalen uit de geneste binnenste functie *functienaam*. </br>2. Het resultaat doorgeven aan de buitenste functie *functionName2*. | "\@<*functionName2*> (<*functienaam*> (<*item*>))" | 
| 1. Het resultaat van *functienaam*. </br>2. Gezien het feit dat het resultaat een object met de eigenschap is *propertyName*, de waarde van die eigenschap ophalen. | "\@<*functienaam*>(<*item*>). <*propertyName*>" | 
||| 

Bijvoorbeeld, de `concat()` functie kunnen twee of meer tekenreekswaarden als parameters. Deze functie combineert deze tekenreeksen in één tekenreeks. U kunt doorgeven in de letterlijke tekenreeks, bijvoorbeeld "Sophia" en "Owen" zodat u een gecombineerde tekenreeks, 'SophiaOwen' niet ophalen:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Of u kunt tekenreekswaarden krijgen van parameters. In dit voorbeeld wordt de `parameters()` functie in elk `concat()` parameter en de `firstName` en `lastName` parameters. U geeft u de resulterende tekenreeksen die moeten worden de `concat()` functie zodat u een gecombineerde tekenreeks, bijvoorbeeld 'SophiaOwen' niet ophalen:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

In beide gevallen beide voorbeelden toewijzen het resultaat dat de `customerName` eigenschap. 

Hier zijn de beschikbare functies gesorteerd op hun algemeen gebruik, of kunt u de functies op basis van [alfabetische volgorde](#alphabetical-list).

<a name="ordered-by-purpose"></a>
<a name="string-functions"></a>

## <a name="string-functions"></a>Tekenreeksfuncties

Als u wilt werken met tekenreeksen, kunt u deze tekenreeksfuncties en ook enkele [verzameling functies](#collection-functions). Tekenreeks-functies werken alleen op tekenreeksen. 

| Tekenreeksfunctie | Taak | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Twee of meer tekenreeksen combineren en retourneert de gecombineerde tekenreeks. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Controleer of een tekenreeks met de opgegeven subtekenreeks eindigt. | 
| [GUID](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Genereer een globally unique identifier (GUID) als een tekenreeks. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Retourneert de beginpositie voor een subtekenreeks. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Retourneert de beginpositie voor het laatste exemplaar van een subtekenreeks. | 
| [vervangen](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Vervangen door de opgegeven tekenreeks een subtekenreeks en retourneert de bijgewerkte tekenreeks. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Retourneert een matrix met subtekenreeksen, gescheiden door komma's, uit een grotere tekenreeks op basis van een opgegeven scheidingsteken in de oorspronkelijke reeks. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Controleren of een tekenreeks met een specifieke subtekenreeks begint. | 
| [de subtekenreeks](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Tekens retourneren uit een tekenreeks, beginnend vanaf de opgegeven positie. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Retourneert een tekenreeks in kleine letters indeling. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Retourneert een tekenreeks in hoofdletters. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Voorloop-en volgspaties verwijderen uit een tekenreeks en retourneert de bijgewerkte tekenreeks. | 
||| 

<a name="collection-functions"></a>

## <a name="collection-functions"></a>Verzameling functies

Als u wilt werken met verzamelingen, in het algemeen matrices, tekenreeksen en soms woordenboeken, kunt u de functies van deze verzameling. 

| Verzameling-functie | Taak | 
| ------------------- | ---- | 
| [bevat](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Controleer of een verzameling een specifiek item heeft. |
| [leeg zijn](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Controleer of een verzameling leeg is. | 
| [eerste](../logic-apps/workflow-definition-language-functions-reference.md#first) | Retourneert het eerste item van een verzameling. | 
| [snijpunt](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Retourneert een verzameling met *alleen* de algemene items in de gespecificeerde verzamelingen. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | Retourneert een tekenreeks is die is *alle* de items uit een array, gescheiden door het opgegeven teken. | 
| [laatste](../logic-apps/workflow-definition-language-functions-reference.md#last) | Retourneert het laatste item uit een verzameling. | 
| [Lengte](../logic-apps/workflow-definition-language-functions-reference.md#length) | Retourneert het aantal items in een tekenreeks of een matrix. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Items verwijderen uit het voorste deel van een verzameling en retourneren *alle andere* items. | 
| [toets maken](../logic-apps/workflow-definition-language-functions-reference.md#take) | Items retourneren vanaf het begin van een verzameling. | 
| [Union](../logic-apps/workflow-definition-language-functions-reference.md#union) | Retourneert een verzameling met *alle* de items van de gespecificeerde verzamelingen. | 
||| 

<a name="comparison-functions"></a>

## <a name="logical-comparison-functions"></a>Van de logische vergelijkingsfuncties

Als u wilt werken met voorwaarden, waarden en de expressieresultaten vergelijken of verschillende soorten logische evalueren, kunt u deze vergelijkingsfuncties logische. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Logische vergelijkingsfunctie. | Taak | 
| --------------------------- | ---- | 
| [en](../logic-apps/workflow-definition-language-functions-reference.md#and) | Controleer of alle expressies ' True ' zijn. | 
| [is gelijk aan](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Controleer of beide waarden gelijk zijn. | 
| [meer](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Controleer of de eerste waarde groter dan de tweede waarde is. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Controleer of de eerste waarde groter dan of gelijk zijn aan de tweede waarde is. | 
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) | Controleer of een expressie waar of ONWAAR is. Op basis van het resultaat, een opgegeven waarde retourneren. | 
| [minder](../logic-apps/workflow-definition-language-functions-reference.md#less) | Controleer of de eerste waarde kleiner is dan de tweede waarde. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Controleer of de eerste waarde kleiner zijn dan of gelijk zijn aan de tweede waarde is. | 
| [niet](../logic-apps/workflow-definition-language-functions-reference.md#not) | Controleer of een expressie onwaar is. | 
| [or](../logic-apps/workflow-definition-language-functions-reference.md#or) | Controleer of ten minste één expressie waar is. |
||| 

<a name="conversion-functions"></a>

## <a name="conversion-functions"></a>Conversiefuncties

Het type of de indeling van een waarde wilt wijzigen, kunt u deze conversiefuncties. U kunt bijvoorbeeld een waarde van een Booleaanse waarde wijzigen naar een geheel getal. Zie voor meer informatie hoe logische Apps inhoudstypen verwerken tijdens de conversie, [inhoudstypen verwerken](../logic-apps/logic-apps-content-type.md). Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Conversiefunctie | Taak | 
| ------------------- | ---- | 
| [Matrix](../logic-apps/workflow-definition-language-functions-reference.md#array) | Retourneert een matrix van één opgegeven invoer. Zie voor meerdere invoergegevens [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Retourneert de met base64 gecodeerde versie voor een tekenreeks. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | Retourneert de binaire versie voor een base64-gecodeerde tekenreeks. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Retourneert de tekenreeksversie voor een base64-gecodeerde tekenreeks. | 
| [binaire bestanden](../logic-apps/workflow-definition-language-functions-reference.md#binary) | De binaire versie voor een invoerwaarde retourneren. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Retourneert de Booleaanse versie voor een invoerwaarde. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Retourneert een matrix van meerdere invoergegevens. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Retourneert de gegevens-URI voor een invoerwaarde. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Retourneert de binaire versie voor een gegevens-URI. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | De tekenreeksversie voor een gegevens-URI geretourneerd. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Retourneert de tekenreeksversie voor een base64-gecodeerde tekenreeks. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Retourneert de binaire versie voor een gegevens-URI. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Een tekenreeks die wordt vervangen escape-tekens met gedecodeerde versies retourneren. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Retourneert een tekenreeks is die wordt vervangen door de URL-onveilige tekens met escape-tekens. | 
| [drijvende komma](../logic-apps/workflow-definition-language-functions-reference.md#float) | Retourneert een drijvende-kommagetal zijn voor een invoerwaarde. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Retourneert de integer-versie voor een tekenreeks. | 
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | Retourneert de waarde van het JavaScript Object Notation (JSON) of het object voor een tekenreeks of XML. | 
| [Tekenreeks](../logic-apps/workflow-definition-language-functions-reference.md#string) | De tekenreeksversie voor een invoerwaarde retourneren. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Retourneert de versie URI-codering voor een invoerwaarde URL onveilige tekens te vervangen door een escape-tekens. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | Retourneert de binaire versie van een URI-gecodeerde tekenreeks. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Retourneert de tekenreeksversie voor een URI-gecodeerde tekenreeks. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | Retourneert de XML-versie voor een tekenreeks. | 
||| 

<a name="math-functions"></a>

## <a name="math-functions"></a>Wiskundige functies

Als u wilt werken met gehele getallen en landen, kunt u deze wiskundige functies. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Math functie | Taak | 
| ------------- | ---- | 
| [add](../logic-apps/workflow-definition-language-functions-reference.md#add) | Retourneert het resultaat van het toevoegen van twee getallen. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Retourneert het resultaat van het delen van twee getallen. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Retourneert de hoogste waarde van een reeks cijfers of een matrix. | 
| [min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Retourneert de laagste waarde van een reeks cijfers of een matrix. | 
| [Mod](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Retourneert de rest van het delen van twee getallen. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Retourneert het product van twee getallen vermenigvuldigen. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Retourneert een willekeurig geheel getal van een opgegeven bereik. | 
| [Bereik](../logic-apps/workflow-definition-language-functions-reference.md#range) | Retourneert een matrix met gehele getallen die met een opgegeven geheel getal zijn begint. | 
| [sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Retourneert het resultaat van af te trekken van het tweede nummer van het eerste getal. | 
||| 

<a name="date-time-functions"></a>

## <a name="date-and-time-functions"></a>Datum- en tijdfuncties

Als u wilt werken met datums en tijden, kunt u deze functies date en time.
Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Datum of tijd functie | Taak | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Een aantal dagen toevoegen aan een tijdstempel. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Een aantal uren toevoegen aan een tijdstempel. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Een aantal minuten toevoegen aan een tijdstempel. | 
| [addSeconds](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Voeg een aantal seconden een tijdstempel. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Een aantal tijdseenheden toevoegen aan een tijdstempel. Zie ook [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Een tijdstempel van Coordinated van Universal Time (UTC) naar de doeltijdzone converteren. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Een tijdstempel van de brontijdzone converteren naar de doeltijdzone. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Een tijdstempel van de brontijdzone Coordinated van Universal Time (UTC) converteren. | 
| [dayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Retourneert de dag van het maandonderdeel van een tijdstempel. | 
| [dayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Retourneert de dag van het onderdeel van de week van een tijdstempel. | 
| [dayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Retourneert de dag van het jaargedeelte van een tijdstempel. | 
| [formatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Retourneert de datum van een tijdstempel. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Retourneert de huidige tijdstempel plus de opgegeven periode-eenheden. Zie ook [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Retourneert de huidige tijdstempel minus de opgegeven periode-eenheden. Zie ook [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Retourneert het begin van de dag van een tijdstempel. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Retourneert het begin van het uur voor een tijdstempel. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Retourneert het begin van de maand voor een tijdstempel. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Een aantal eenheden van een tijdstempel aftrekken. Zie ook [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [maatstreepjes](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Retourneert de `ticks` eigenschapwaarde voor een opgegeven timestamp. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Retourneert de huidige tijdstempel als een tekenreeks. | 
||| 

<a name="workflow-functions"></a>

## <a name="workflow-functions"></a>Werkstroomfuncties

Deze werkstroomfuncties kunnen u helpen:

* Informatie ophalen over een werkstroomexemplaar dat tijdens de uitvoering. 
* Werken met de invoer die is gebruikt voor het instantiëren van logische apps.
* Verwijzen naar de uitvoer van triggers en acties.

U kunt bijvoorbeeld verwijzen naar de uitvoer van een actie en die gegevens gebruiken in een latere actie. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Werkstroomfunctie | Taak | 
| ----------------- | ---- | 
| [Actie](../logic-apps/workflow-definition-language-functions-reference.md#action) | Retourneert de huidige actie-uitvoer in runtime, of waarden die van andere naam en waarde-paren van JSON. Zie ook [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Resultaat van een actie `body` uitvoer tijdens runtime. Zie ook [hoofdtekst](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Retourneert de uitvoer van een actie tijdens runtime. Zie [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [Acties](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Retourneert de uitvoer van een actie in runtime, of waarden van andere naam en waarde-paren van JSON. Zie ook [actie](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [De hoofdtekst](#body) | Resultaat van een actie `body` uitvoer tijdens runtime. Zie ook [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Maken van een matrix met de waarden die overeenkomen met de naam van een sleutel in *formuliergegevens* of *formuliercodes* actie-uitvoer. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Retourneert één waarde die overeenkomt met een naam in van een actie *formuliergegevens* of *formuliercodes uitvoer*. | 
| [Item](../logic-apps/workflow-definition-language-functions-reference.md#item) | Wanneer u zich binnen een herhalingsactie via een matrix, het huidige item in de matrix te retourneren tijdens de huidige herhaling van de actie. | 
| [items](../logic-apps/workflow-definition-language-functions-reference.md#items) | Wanneer in een voor elke of doen totdat lus retourneren het huidige item uit de opgegeven lus.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | Retourneert de "callback-URL' die een actie of trigger aanroept. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Retourneert de instantie voor een bepaald deel in de uitvoer van een actie die bestaat uit meerdere delen. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Retourneert de waarde voor een parameter die wordt beschreven in de definitie van uw logische app. | 
| [Trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Retourneert de uitvoer van een trigger tijdens runtime, of vanuit een andere naam en waarde-paren van JSON. Zie ook [triggerOutputs](#triggerOutputs) en [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Resultaat van een trigger `body` uitvoer tijdens runtime. Zie [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Die overeenkomen met de naam van een sleutel in één waarde retourneren *formuliergegevens* of *formuliercodes* uitvoer activeren. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | De instantie voor een bepaald deel in een meerdelige triggeruitvoer retourneren. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Maken van een matrix waarvan de waarden overeenkomen met de naam van een sleutel in *formuliergegevens* of *formuliercodes* uitvoer activeren. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Retourneert de uitvoer van een trigger in runtime, of waarden van andere naam en waarde-paren van JSON. Zie [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [Variabelen](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Retourneert de waarde voor een opgegeven variabele. | 
| [Werkstroom](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Als resultaat de details over de werkstroom zelf tijdens runtime. | 
||| 

<a name="uri-parsing-functions"></a>

## <a name="uri-parsing-functions"></a>Functies voor URI parseren

Als u wilt werken met uniform resource-id's (URI) en waarden van verschillende eigenschappen voor deze URI's te halen, kunt u deze functies voor URI parseren. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| URI parseren van de functie | Taak | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Retourneert de `host` waarde voor een uniform resource identifier (URI). | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Retourneert de `path` waarde voor een uniform resource identifier (URI). | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Retourneert de `path` en `query` waarden voor een uniform resource identifier (URI). | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Retourneert de `port` waarde voor een uniform resource identifier (URI). | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Retourneert de `query` waarde voor een uniform resource identifier (URI). | 
| [uriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Retourneert de `scheme` waarde voor een uniform resource identifier (URI). | 
||| 

<a name="manipulation-functions"></a>

## <a name="manipulation-functions-json--xml"></a>Functies voor het bewerken: JSON en XML

Als u wilt werken met JSON-objecten en XML-knooppunt, kunt u deze functies voor het bewerken. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische lijst](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Functie voor het bewerken | Taak | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Een eigenschap en de waarde of de naam / waarde-paar, toevoegen aan een JSON-object en het bijgewerkte object retourneren. | 
| [samenvoegen](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Retourneert de eerste niet-null-waarde van een of meer parameters. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Verwijderen van een eigenschap van een JSON-object en het bijgewerkte object retourneren. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Stel de waarde voor eigenschap van een JSON-object en het bijgewerkte object geretourneerd. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Controleren of XML knooppunten of waarden die overeenkomen met een (XML Path-taal) XPath-expressie en retourneert de overeenkomende waarden of knooppunten. | 
||| 

<a name="alphabetical-list"></a>
<a name="action"></a>

### <a name="action"></a>actie

Retourneert de *huidige* actie-uitvoer in runtime, of waarden uit een andere naam en waarde-paren van JSON, die u aan een expressie toewijzen kunt. Deze functie verwijst naar de volledige actie-object, maar u kunt optioneel een eigenschap waarvoor u waarde wilt opgeven. Zie ook [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

U kunt de `action()` functie alleen op de volgende locaties: 

* De `unsubscribe` eigenschap voor een webhookactie zodat u toegang hebt tot het resultaat van de oorspronkelijke `subscribe` aanvraag
* De `trackedProperties` eigenschap voor een actie
* De `do-until` voorwaarde voor een actie in een lus

```
action()
action().outputs.body.<property> 
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*De eigenschap*> | Nee | Reeks | De naam op voor het actie-object eigenschap waarvan u wilt dat de waarde: **naam**, **startTime**, **endTime**, **invoer**,  **uitvoer**, **status**, **code**, **trackingId**, en **clientTrackingId**. U kunt deze eigenschappen aan de hand van een specifieke uitvoeringsgeschiedenis details vinden in de Azure-portal. Zie voor meer informatie, [REST-API - uitvoeren werkstroomacties](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-uitvoer*> | Reeks | De uitvoer van de huidige actie of de eigenschap | 
|||| 

<a name="actionBody"></a>

### <a name="actionbody"></a>actionBody

Resultaat van een actie `body` uitvoer tijdens runtime. De korte benaming voor `actions('<actionName>').outputs.body`. Zie [body()](#body) en [actions()](#actions).

```
actionBody('<actionName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam van de actie `body` uitvoer die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-instantie-uitvoer*> | Reeks | De `body` uitvoer van de opgegeven actie | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `body` uitvoer van de Twitter-actie `Get user`: 

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

### <a name="actionoutputs"></a>actionOutputs

Retourneert de uitvoer van een actie tijdens runtime. De korte benaming voor `actions('<actionName>').outputs`. Zie [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie-uitvoer die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*Uitvoer*> | Reeks | De uitvoer van de opgegeven actie | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de uitvoer van de Twitter-actie `Get user`: 

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

### <a name="actions"></a>Acties

Retourneert de uitvoer van een actie in runtime, of waarden van andere naam en waarde-paren van JSON, die u aan een expressie toewijzen kunt. Standaard de functie verwijst naar het hele action-object, maar u kunt optioneel een eigenschap opgeven waarvan de waarde die u wilt. Zie voor steno versies [actionBody()](#actionBody), [actionOutputs()](#actionOutputs), en [body()](#body). Zie voor de huidige actie [action()](#action).

> [!NOTE] 
> Eerder, kunt u de `actions()` functie of het `conditions` element als op te geven of een actie wordt uitgevoerd op basis van de uitvoer van een andere actie. Echter, om te declareren expliciet afhankelijkheden tussen acties, moet u nu gebruiken van de afhankelijke actie `runAfter` eigenschap. Voor meer informatie over de `runAfter` eigenschap, Zie [onderscheppen en afhandelen van fouten met de eigenschap runAfter](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property> 
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie voor het object waarvan de gewenste uitvoer  | 
| <*De eigenschap*> | Nee | Reeks | De naam op voor het actie-object eigenschap waarvan u wilt dat de waarde: **naam**, **startTime**, **endTime**, **invoer**,  **uitvoer**, **status**, **code**, **trackingId**, en **clientTrackingId**. U kunt deze eigenschappen aan de hand van een specifieke uitvoeringsgeschiedenis details vinden in de Azure-portal. Zie voor meer informatie, [REST-API - uitvoeren werkstroomacties](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-uitvoer*> | Reeks | De uitvoer van de opgegeven actie of de eigenschap | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `status` eigenschapswaarde van de Twitter-actie `Get user` tijdens runtime: 

```
actions('Get_user').outputs.body.status 
```

En dit resultaat wordt weergegeven: `"Succeeded"`

<a name="add"></a>

### <a name="add"></a>toevoegen

Retourneert het resultaat van het toevoegen van twee getallen.

```
add(<summand_1>, <summand_2>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*summand_1*>, <*summand_2*> | Ja | Geheel getal, drijvende komma, of | De getallen om toe te voegen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*resultaat som*> | Geheel getal of een gegevenstype met drijvende komma | Het resultaat van het toevoegen van de opgegeven getallen | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de opgegeven getallen toegevoegd:

```
add(1, 1.5)
```

En dit resultaat wordt weergegeven: `2.5`

<a name="addDays"></a>

### <a name="adddays"></a>addDays

Een aantal dagen toevoegen aan een tijdstempel.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*dagen*> | Ja | Geheel getal | Het positieve of negatieve aantal dagen om toe te voegen | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De tijdstempel plus het opgegeven aantal dagen  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 dagen aan de opgegeven timestamp toegevoegd:

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

### <a name="addhours"></a>addHours

Een aantal uren toevoegen aan een tijdstempel.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*Uur*> | Ja | Geheel getal | Het positieve of negatieve aantal uren om toe te voegen | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De tijdstempel plus het opgegeven aantal uren  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 uur aan de opgegeven timestamp toegevoegd:

```
addHours('2018-03-15T00:00:00Z', 10)
```

En dit resultaat wordt weergegeven: `"2018-03-15T10:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld trekt vijf uur van de opgegeven tijdstempel:

```
addHours('2018-03-15T15:00:00Z', -5)
```

En dit resultaat wordt weergegeven: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

### <a name="addminutes"></a>addMinutes

Een aantal minuten toevoegen aan een tijdstempel.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*minuten*> | Ja | Geheel getal | Het positieve of negatieve aantal minuten om toe te voegen | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De tijdstempel plus het opgegeven aantal minuten | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 minuten aan de opgegeven timestamp toegevoegd:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:20:00.0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt afgetrokken van vijf minuten van de opgegeven tijdstempel:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

### <a name="addproperty"></a>addProperty

Een eigenschap en de waarde of de naam / waarde-paar, toevoegen aan een JSON-object en het bijgewerkte object retourneren. Als het object al tijdens runtime bestaat, is de functie genereert een fout.

```
addProperty(<object>, '<property>', <value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object*> | Ja | Object | Het JSON-object waar u een eigenschap toevoegen | 
| <*De eigenschap*> | Ja | Reeks | De naam op voor de eigenschap toe te voegen | 
| <*Waarde*> | Ja | Alle | De waarde voor de eigenschap |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt-object*> | Object | Het bijgewerkte JSON-object met de opgegeven eigenschap | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `accountNumber` eigenschap in op de `customerProfile` object, dat wordt geconverteerd naar JSON met de [JSON()](#json) functie. De functie wijst een waarde die wordt gegenereerd door de [guid()](#guid) functioneren en retourneert het bijgewerkte object:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

### <a name="addseconds"></a>addSeconds

Voeg een aantal seconden een tijdstempel.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*seconden*> | Ja | Geheel getal | Het positieve of negatieve aantal seconden dat wordt toegevoegd | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De tijdstempel plus het opgegeven aantal seconden  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt 10 seconden aan de opgegeven timestamp toegevoegd:

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

### <a name="addtotime"></a>addToTime

Een aantal tijdseenheden toevoegen aan een tijdstempel. Zie ook [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*interval*> | Ja | Geheel getal | Het aantal eenheden van de opgegeven tijd toe te voegen | 
| <*timeUnit*> | Ja | Reeks | De tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month', 'Year' | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De tijdstempel plus het opgegeven aantal tijdseenheden  | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt één dag aan de opgegeven timestamp toegevoegd:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day') 
```

En dit resultaat wordt weergegeven: `"2018-01-02T00:00:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt één dag aan de opgegeven timestamp toegevoegd:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

En retourneert het resultaat met behulp van de optionele "D"-indeling: `"Tuesday, January 2, 2018"`

<a name="and"></a>

### <a name="and"></a>en

Controleer of alle expressies ' True ' zijn. Retourneert waar wanneer alle expressies waar zijn of onwaar retourneren wanneer ten minste één expressie onwaar is.

```
and(<expression1>, <expression2>, ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Expression1*>, <*expression2*>,... | Ja | Booleaans | De expressies om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| waar of ONWAAR | Booleaans | Retourneert ' True ' wanneer alle expressies ' True ' zijn. Retourneert de waarde false wanneer ten minste één expressie onwaar is. | 
|||| 

*Voorbeeld 1*

Deze voorbeelden te controleren of de opgegeven Booleaanse waarden alle waar zijn:

```
and(true, true)
and(false, true)
and(false, false)
```

En deze resultaten retourneert:

* Eerste voorbeeld: beide expressies ' True ' zijn, dus als resultaat `true`. 
* Voorbeeld van de tweede: één expressie is ONWAAR, dus als resultaat `false`.
* Voorbeeld van de derde: beide expressies zijn ingesteld op false, dus als resultaat `false`.

*Voorbeeld 2*

Deze voorbeelden te controleren of de opgegeven expressies alle waar zijn:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

En deze resultaten retourneert:

* Eerste voorbeeld: beide expressies ' True ' zijn, dus als resultaat `true`. 
* Voorbeeld van de tweede: één expressie is ONWAAR, dus als resultaat `false`.
* Voorbeeld van de derde: beide expressies zijn ingesteld op false, dus als resultaat `false`.

<a name="array"></a>

### <a name="array"></a>matrix

Retourneert een matrix van één opgegeven invoer. Zie voor meerdere invoergegevens [createArray()](#createArray). 

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

In dit voorbeeld maakt u een matrix van de tekenreeks "Hallo":

```
array('hello')
```

En dit resultaat wordt weergegeven: `["hello"]`

<a name="base64"></a>

### <a name="base64"></a>met base64

Retourneert de met base64 gecodeerde versie voor een tekenreeks.

```
base64('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De ingevoerde tekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Base64-tekenreeks*> | Reeks | De met base64 gecodeerde versie voor de ingevoerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de tekenreeks "Hallo" geconverteerd naar een met base64 gecodeerde tekenreeks:

```
base64('hello')
```

En dit resultaat wordt weergegeven: `"aGVsbG8="`

<a name="base64ToBinary"></a>

### <a name="base64tobinary"></a>base64ToBinary

Retourneert de binaire versie voor een base64-gecodeerde tekenreeks.

```
base64ToBinary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De met base64 gecodeerde tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-base64-tekenreeks*> | Reeks | De binaire versie van de base64-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de ' aGVsbG8 = "base64-gecodeerde tekenreeks naar een binaire tekenreeks:

```
base64ToBinary('aGVsbG8=')
```

En dit resultaat wordt weergegeven: 

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

### <a name="base64tostring"></a>base64ToString

De tekenreeksversie voor een base64-gecodeerde tekenreeks, effectief decoderen van base64-tekenreeks geretourneerd. Deze functie wilt gebruiken in plaats van [decodeBase64()](#decodeBase64). Hoewel beide functies werken op dezelfde manier, `base64ToString()` verdient de voorkeur.

```
base64ToString('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De met base64 gecodeerde tekenreeks die moet worden gedecodeerd | 
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

### <a name="binary"></a>binaire bestanden 

Retourneert de binaire versie van een tekenreeks.

```
binary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks die moet worden geconverteerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair bestand voor invoer waarde*> | Reeks | De binaire versie voor de opgegeven tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de tekenreeks "Hallo" geconverteerd naar een binaire tekenreeks:

```
binary('hello')
```

En dit resultaat wordt weergegeven: 

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

### <a name="body"></a>hoofdtekst

Resultaat van een actie `body` uitvoer tijdens runtime. De korte benaming voor `actions('<actionName>').outputs.body`. Zie [actionBody()](#actionBody) en [actions()](#actions).

```
body('<actionName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam van de actie `body` uitvoer die u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | -----| ----------- | 
| <*actie-instantie-uitvoer*> | Reeks | De `body` uitvoer van de opgegeven actie | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `body` uitvoer van de `Get user` Twitter-actie: 

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

### <a name="bool"></a>BOOL

Retourneert de Booleaanse versie voor een waarde.

```
bool(<value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Alle | De waarde te converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | De Booleaanse versie voor de opgegeven waarde | 
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

### <a name="coalesce"></a>samenvoegen

Retourneert de eerste niet-null-waarde van een of meer parameters. Lege tekenreeksen, lege matrices en lege objecten zijn niet null zijn.

```
coalesce(<object_1>, <object_2>, ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object_1*>, <*object_2*>,... | Ja | Eventuele kunt typen combineren | Een of meer items om te controleren op null-waarden | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*eerste-niet-null-item*> | Alle | Het eerste item of -waarde die niet null is. Als alle parameters null zijn, is deze functie retourneert null zijn. | 
|||| 

*Voorbeeld*

Deze voorbeelden retourneren de eerste niet-null-waarde van de opgegeven waarden, of null zijn wanneer alle waarden null zijn:

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

### <a name="concat"></a>concat

Twee of meer tekenreeksen combineren en retourneert de gecombineerde tekenreeks. 

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

In dit voorbeeld worden de tekenreeksen "Hello" en "World" gecombineerd:

```
concat('Hello', 'World')
```

En dit resultaat wordt weergegeven: `"HelloWorld"`

<a name="contains"></a>

### <a name="contains"></a>bevat

Controleer of een verzameling een specifiek item heeft. Retourneert waar wanneer het item is gevonden, of onwaar retourneren wanneer niet gevonden. Deze functie is hoofdlettergevoelig.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Deze functie werkt met name op dergelijke verzameling: 

* Een *tekenreeks* vinden een *subtekenreeks*
* Een *matrix* vinden een *waarde*
* Een *woordenlijst* vinden een *sleutel*

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix woordenlijst | De verzameling om te controleren | 
| <*Waarde*> | Ja | Tekenreeks of matrix woordenlijst, respectievelijk | Het item om te zoeken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar wanneer het item is gevonden. Retourneert de waarde false wanneer niet gevonden. |
|||| 

*Voorbeeld 1*

In dit voorbeeld controleert de tekenreeks "Hallo wereld" voor de subtekenreeks "wereld" en retourneert ' True ':

```
contains('hello world', 'world')
```

*Voorbeeld 2*

In dit voorbeeld controleert de tekenreeks "Hallo wereld" voor de subtekenreeks 'universe' en retourneert ' false ':

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

### <a name="convertfromutc"></a>convertFromUtc

Een tijdstempel van Coordinated van Universal Time (UTC) naar de doeltijdzone converteren.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*destinationTimeZone*> | Ja | Reeks | De naam voor de doel-tijdzone. Zie voor meer informatie, [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geconverteerd timestamp*> | Reeks | De tijdstempel geconverteerd naar de doeltijdzone | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt een tijdstempel op de opgegeven tijdzone geconverteerd: 

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

En dit resultaat wordt weergegeven: `"2018-01-01T00:00:00.0000000"`

*Voorbeeld 2*

In dit voorbeeld wordt een tijdstempel aan de opgegeven tijdzone en indeling geconverteerd:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

En dit resultaat wordt weergegeven: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

### <a name="converttimezone"></a>convertTimeZone

Een tijdstempel van de brontijdzone converteren naar de doeltijdzone.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*sourceTimeZone*> | Ja | Reeks | De naam voor de tijdzone van de bron. Zie voor meer informatie, [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*destinationTimeZone*> | Ja | Reeks | De naam voor de doel-tijdzone. Zie voor meer informatie, [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geconverteerd timestamp*> | Reeks | De tijdstempel geconverteerd naar de doeltijdzone | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt de brontijdzone geconverteerd naar de doeltijdzone: 

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

En dit resultaat wordt weergegeven: `"2018-01-01T00:00:00.0000000"`

*Voorbeeld 2*

In dit voorbeeld wordt een tijdzone geconverteerd naar de opgegeven tijdzone en indeling:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

En dit resultaat wordt weergegeven: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

### <a name="converttoutc"></a>convertToUtc

Een tijdstempel van de brontijdzone Coordinated van Universal Time (UTC) converteren.

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*sourceTimeZone*> | Ja | Reeks | De naam voor de tijdzone van de bron. Zie voor meer informatie, [tijdzone-id's](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geconverteerd timestamp*> | Reeks | De tijdstempel die wordt geconverteerd naar UTC | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt een tijdstempel wordt geconverteerd naar UTC: 

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

En dit resultaat wordt weergegeven: `"2018-01-01T08:00:00.0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt een tijdstempel wordt geconverteerd naar UTC:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

En dit resultaat wordt weergegeven: `"Monday, January 1, 2018"`

<a name="createArray"></a>

### <a name="createarray"></a>createArray

Retourneert een matrix van meerdere invoergegevens. Zie voor één invoer matrices [array](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object1*>, <*object2*>,... | Ja | , Maar niet in de gemengde modus | Ten minste twee items voor het maken van de matrix | 
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

### <a name="datauri"></a>dataUri

Retourneert een gegevens uniform resource identifier (URI) voor een tekenreeks. 

```
dataUri('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks die moet worden geconverteerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gegevens-uri*> | Reeks | De gegevens-URI voor de ingevoerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een gegevens-URI voor de tekenreeks "Hallo":

```
dataUri('hello') 
```

En dit resultaat wordt weergegeven: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

### <a name="datauritobinary"></a>dataUriToBinary

Retourneert de binaire versie voor een data uniform resource identifier (URI). Deze functie wilt gebruiken in plaats van [decodeDataUri()](#decodeDataUri). Hoewel beide functies werken op dezelfde manier, `decodeDataUri()` verdient de voorkeur.

```
dataUriToBinary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De gegevens-URI moet worden geconverteerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-gegevens-uri*> | Reeks | De binaire versie van de gegevens-URI | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een binaire versie voor deze gegevens-URI:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

En dit resultaat wordt weergegeven: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

### <a name="datauritostring"></a>dataUriToString

De tekenreeksversie voor een data uniform resource identifier (URI) retourneren.

```
dataUriToString('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De gegevens-URI moet worden geconverteerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*tekenreeks-voor-gegevens-uri*> | Reeks | De tekenreeksversie voor de gegevens-URI | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een tekenreeks op voor deze gegevens-URI:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

En dit resultaat wordt weergegeven: `"hello"`

<a name="dayOfMonth"></a>

### <a name="dayofmonth"></a>dayOfMonth

Retourneert de dag van de maand van een tijdstempel. 

```
dayOfMonth('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*dag van maand*> | Geheel getal | De dag van de maand van de opgegeven timestamp | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert het getal voor de dag van de maand van dit tijdstempel:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

En dit resultaat wordt weergegeven: `15`

<a name="dayOfWeek"></a>

### <a name="dayofweek"></a>dayOfWeek

Retourneert de dag van de week van een tijdstempel.  

```
dayOfWeek('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*dag van week*> | Geheel getal | De dag van de week van de opgegeven timestamp waar zondag is ingesteld op 0 maandag is 1, enzovoort | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert het getal voor de dag van de week van dit tijdstempel:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

En dit resultaat wordt weergegeven: `3`

<a name="dayOfYear"></a>

### <a name="dayofyear"></a>dayOfYear

Retourneert de dag van het jaar van een tijdstempel. 

```
dayOfYear('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*dag van jaar*> | Geheel getal | De dag van het jaar van de opgegeven timestamp | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de waarde van de dag van het jaar van dit tijdstempel:

```
dayOfYear('2018-03-15T13:27:36Z')
```

En dit resultaat wordt weergegeven: `74`

<a name="decodeBase64"></a>

### <a name="decodebase64"></a>decodeBase64

De tekenreeksversie voor een base64-gecodeerde tekenreeks, effectief decoderen van base64-tekenreeks geretourneerd. Overweeg het gebruik van [base64ToString()](#base64ToString) in plaats van `decodeBase64()`. Hoewel beide functies werken op dezelfde manier, `base64ToString()` verdient de voorkeur.

```
decodeBase64('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De met base64 gecodeerde tekenreeks die moet worden gedecodeerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-base64-tekenreeks*> | Reeks | De tekenreeksversie voor een base64-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een tekenreeks op voor een base64-gecodeerde tekenreeks:

```
decodeBase64('aGVsbG8=')
```

En dit resultaat wordt weergegeven: `"hello"`

<a name="decodeDataUri"></a>

### <a name="decodedatauri"></a>decodeDataUri

Retourneert de binaire versie voor een data uniform resource identifier (URI). Overweeg het gebruik van [dataUriToBinary()](#dataUriToBinary), in plaats van `decodeDataUri()`. Hoewel beide functies werken op dezelfde manier, `dataUriToBinary()` verdient de voorkeur.

```
decodeDataUri('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De gegevens-URI-tekenreeks moet worden gedecodeerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-gegevens-uri*> | Reeks | De binaire versie van een gegevens-URI-tekenreeks | 
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

### <a name="decodeuricomponent"></a>decodeUriComponent

Een tekenreeks die wordt vervangen escape-tekens met gedecodeerde versies retourneren. 

```
decodeUriComponent('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks met de escape-tekens moet worden gedecodeerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-uri*> | Reeks | De bijgewerkte tekenreeks met de gedecodeerde escape-tekens | 
|||| 

*Voorbeeld*

In dit voorbeeld worden de escapetekens in deze tekenreeks vervangen door gedecodeerde versies:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

En dit resultaat wordt weergegeven: `"https://contoso.com"`

<a name="div"></a>

### <a name="div"></a>div

Retourneert het resultaat geheel getal van twee getallen delen. Als u de rest-resultaat, Zie [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*deeltal*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal waardoor moet worden de *deler* | 
| <*deler*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal dat wordt gedeeld de *deeltal*, maar mag niet 0 | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*quotiënt-resultaat*> | Geheel getal | De integer als resultaat van het delen van het eerste getal door het tweede getal | 
|||| 

*Voorbeeld*

Beide voorbeelden delen het eerste getal door het tweede getal:

```
div(10, 5)
div(11, 5)
```

En dit resultaat te retourneren: `2`

<a name="encodeUriComponent"></a>

### <a name="encodeuricomponent"></a>encodeUriComponent

Retourneert een uniform resource identifier (URI) gecodeerde versie voor een tekenreeks-URL-onveilige tekens te vervangen door een escape-tekens. Overweeg het gebruik van [uriComponent()](#uriComponent), in plaats van `encodeUriComponent()`. Hoewel beide functies werken op dezelfde manier, `uriComponent()` verdient de voorkeur.

```
encodeUriComponent('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks die moet worden geconverteerd naar de URI-notatie | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gecodeerde-uri*> | Reeks | De URI-gecodeerde tekenreeks met escape-tekens | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een URI-gecodeerde versie voor deze tekenreeks:

```
encodeUriComponent('https://contoso.com')
```

En dit resultaat wordt weergegeven: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

### <a name="empty"></a>Leeg

Controleer of een verzameling leeg is. Retourneert waar wanneer de verzameling leeg is of retourneren ONWAAR wanneer deze niet leeg zijn.

```
empty('<collection>')
empty([<collection>])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks, matrix of Object | De verzameling om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar als de verzameling leeg is. Retourneert de waarde false als deze niet leeg zijn. | 
|||| 

*Voorbeeld* 

Deze voorbeelden te controleren of de gespecificeerde verzamelingen leeg zijn:

```
empty('')
empty('abc')
```

En deze resultaten retourneert: 

* Eerste voorbeeld: een lege tekenreeks doorgegeven, zodat de functie geeft als resultaat `true`. 
* Voorbeeld van de tweede: geeft u de tekenreeks "abc", zodat de functie retourneert `false`. 

<a name="endswith"></a>

### <a name="endswith"></a>endsWith

Controleer of een tekenreeks met een specifieke subtekenreeks eindigt. Retourneert waar wanneer de subtekenreeks wordt gevonden, of onwaar retourneren wanneer niet gevonden. Deze functie is niet hoofdlettergevoelig.

```
endsWith('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks om te controleren | 
| <*Brontekst*> | Ja | Reeks | De einddatum subtekenreeks om te zoeken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR  | Booleaans | Retourneert waar wanneer de einddatum subtekenreeks wordt gevonden. Retourneert de waarde false wanneer niet gevonden. | 
|||| 

*Voorbeeld 1* 

In dit voorbeeld controleert of de tekenreeks "Hallo wereld" met de tekenreeks "wereld eindigt":

```
endsWith('hello world', 'world')
```

En dit resultaat wordt weergegeven: `true`

*Voorbeeld 2*

In dit voorbeeld controleert of de tekenreeks "Hallo wereld" met de tekenreeks "universe eindigt":

```
endsWith('hello world', 'universe')
```

En dit resultaat wordt weergegeven: `false`

<a name="equals"></a>

### <a name="equals"></a>is gelijk aan

Controleer of zowel waarden, expressies of objecten equivalent zijn. Retourneert ' True ' wanneer beide gelijk zijn, of onwaar retourneren wanneer ze niet gelijk zijn.

```
equals('<object1>', '<object2>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object1*>, <*object2*> | Ja | Verschillende | De waarden, de expressies, of de objecten die u wilt vergelijken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert ' True ' wanneer beide gelijk zijn. Retourneert de waarde false wanneer geen equivalent. | 
|||| 

*Voorbeeld*

Deze voorbeelden kunt u controleren of de opgegeven invoer gelijk zijn. 

```
equals(true, 1)
equals('abc', 'abcd')
```

En deze resultaten retourneert: 

* Eerste voorbeeld: beide waarden zijn equivalent, zodat de functie retourneert `true`.
* Tweede voorbeeld: beide waarden niet het geval is, zodat de functie retourneert `false`.

<a name="first"></a>

### <a name="first"></a>eerste

Retourneert het eerste item van een tekenreeks of een matrix.

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
| <*eerste-verzameling-item*> | Alle | Het eerste item in de verzameling | 
|||| 

*Voorbeeld*

Deze voorbeelden wordt het eerste item vindt in deze verzamelingen:

```
first('hello')
first([0, 1, 2])
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `"h"`
* Tweede voorbeeld: `0`

<a name="float"></a>

### <a name="float"></a>drijvend

De tekenreeksversie van een voor een drijvend-kommagetal converteren naar een werkelijke getal met drijvende komma. U kunt deze functie alleen als aangepaste parameters wordt doorgegeven aan een app, zoals een logische app.

```
float('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks met een geldige drijvend-kommagetal converteren |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Float-waarde*> | Float | Het getal met drijvende komma voor de opgegeven tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een tekenreeksversie van dit getal met drijvende komma:

```
float('10.333')
```

En dit resultaat wordt weergegeven: `10.333`

<a name="formatDateTime"></a>

### <a name="formatdatetime"></a>formatDateTime

Retourneert een timestamp worden opgegeven in de indeling die is opgegeven.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*opnieuw geformatteerd timestamp*> | Reeks | De bijgewerkte tijdstempel in de opgegeven indeling | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt een tijdstempel geconverteerd naar de opgegeven indeling:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

En dit resultaat wordt weergegeven: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

### <a name="formdatamultivalues"></a>formDataMultiValues

Retourneert een matrix met waarden die overeenkomen met een naam in van een actie *formuliergegevens* of *formuliercodes* uitvoer. 

```
formDataMultiValues('<actionName>', '<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De actie waarvan de uitvoer heeft de waarde van de sleutel die u wilt | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan u wilt dat de waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*matrix met sleutel waarden*>] | Matrix | Een matrix met de waarden die overeenkomen met de opgegeven sleutel | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt een matrix gemaakt van de waarde van de sleutel van de 'Onderwerp' in de opgegeven actie formuliergegevens of formuliercodes uitvoer:  

```
formDataMultiValues('Send_an_email', 'Subject')
```

En resulteert in de tekst van het onderwerp in een matrix, bijvoorbeeld: `["Hello world"]`

<a name="formDataValue"></a>

### <a name="formdatavalue"></a>formDataValue

Retourneert één waarde die overeenkomt met een naam in van een actie *formuliergegevens* of *formuliercodes* uitvoer. Als de functie vindt meer dan één overeenkomst, is de functie genereert een fout.

```
formDataValue('<actionName>', '<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De actie waarvan de uitvoer heeft de waarde van de sleutel die u wilt | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan u wilt dat de waarde |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*sleutel / waarde-*> | Reeks | De waarde in de opgegeven sleutel  | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een tekenreeks van de 'Onderwerp' sleutel-waarde in de opgegeven actie formuliergegevens of formuliercodes uitvoer:  

```
formDataValue('Send_an_email', 'Subject')
```

En retourneert de tekst van het onderwerp een tekenreeks, bijvoorbeeld: `"Hello world"`

<a name="getFutureTime"></a>

### <a name="getfuturetime"></a>getFutureTime

Retourneert de huidige tijdstempel plus de opgegeven periode-eenheden.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*interval*> | Ja | Geheel getal | Het aantal eenheden van de opgegeven tijd om af te trekken | 
| <*timeUnit*> | Ja | Reeks | De tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month', 'Year' | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De huidige tijdstempel plus het opgegeven aantal tijdseenheden | 
|||| 

*Voorbeeld 1*

Stel dat de huidige tijdstempel is ' 2018-03-01T00:00:00.0000000Z '. In dit voorbeeld worden vijf dagen opgeteld bij tijdstempel:

```
getFutureTime(5, 'Day')
```

En dit resultaat wordt weergegeven: `"2018-03-06T00:00:00.0000000Z"`

*Voorbeeld 2*

Stel dat de huidige tijdstempel is ' 2018-03-01T00:00:00.0000000Z '. In dit voorbeeld worden vijf dagen opgeteld en het resultaat wordt omgezet in de indeling "D":

```
getFutureTime(5, 'Day', 'D')
```

En dit resultaat wordt weergegeven: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

### <a name="getpasttime"></a>getPastTime

Retourneert de huidige tijdstempel minus de opgegeven periode-eenheden.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*interval*> | Ja | Geheel getal | Het aantal eenheden van de opgegeven tijd om af te trekken | 
| <*timeUnit*> | Ja | Reeks | De tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month', 'Year' | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De huidige tijdstempel minus het opgegeven aantal tijdseenheden | 
|||| 

*Voorbeeld 1*

Stel dat de huidige tijdstempel is ' 2018-02-01T00:00:00.0000000Z '. In dit voorbeeld wordt afgetrokken van vijf dagen van tijdstempel:

```
getPastTime(5, 'Day')
```

En dit resultaat wordt weergegeven: `"2018-01-27T00:00:00.0000000Z"`

*Voorbeeld 2*

Stel dat de huidige tijdstempel is ' 2018-02-01T00:00:00.0000000Z '. In dit voorbeeld wordt afgetrokken van vijf dagen en wordt het resultaat geconverteerd naar "D"-indeling:

```
getPastTime(5, 'Day', 'D')
```

En dit resultaat wordt weergegeven: `"Saturday, January 27, 2018"`

<a name="greater"></a>

### <a name="greater"></a>meer

Controleer of de eerste waarde groter dan de tweede waarde is. Retourneert waar wanneer de eerste waarde meer is en retourneert false als minder.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, drijvende komma of tekenreeks | De eerste waarde om te controleren of groter zijn dan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, drijvende komma of tekenreeks, respectievelijk | De vergelijkingswaarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar als de eerste waarde hoger is dan de tweede waarde. Retourneert onwaar als de eerste waarde gelijk aan of kleiner is dan de tweede waarde is. | 
|||| 

*Voorbeeld*

Deze voorbeelden gecontroleerd of de eerste waarde groter is dan de tweede waarde:

```
greater(10, 5)
greater('apple', 'banana')
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `true`
* Tweede voorbeeld: `false`

<a name="greaterOrEquals"></a>

### <a name="greaterorequals"></a>greaterOrEquals

Controleer of de eerste waarde groter dan of gelijk zijn aan de tweede waarde is.
Retourneert waar wanneer de eerste waarde groter of gelijk zijn is, of onwaar retourneren als de eerste waarde kleiner is.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, drijvende komma of tekenreeks | De eerste waarde om te controleren of groter zijn dan of gelijk zijn aan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, drijvende komma of tekenreeks, respectievelijk | De vergelijkingswaarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar als de eerste waarde groter dan of gelijk zijn aan de tweede waarde is. Retourneert onwaar als de eerste waarde kleiner is dan de tweede waarde. | 
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

### <a name="guid"></a>GUID

Een globally unique identifier (GUID) genereren als een tekenreeks, bijvoorbeeld 'c2ecc88d-88c8-4096-912c-d6f2e2b138ce': 

```
guid()
```

U kunt ook een andere indeling opgeven voor de GUID dan de standaardindeling, "D", 32 cijfers gescheiden door afbreekstreepjes bevatten is.

```
guid('<format>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Indeling*> | Nee | Reeks | Een enkel [aanduiding opmaken](https://msdn.microsoft.com/library/97af8hh4) voor de geretourneerde GUID. De indeling "D" is, maar u kunt "N", "D", "B", "P" of "X". | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*GUID-waarde*> | Reeks | Een willekeurig gegenereerde GUID | 
|||| 

*Voorbeeld* 

In dit voorbeeld genereert dezelfde GUID, maar als 32 cijfers, gescheiden door afbreekstreepjes en tussen haakjes: 

```
guid('P')
```

En dit resultaat wordt weergegeven: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

### <a name="if"></a>if

Controleer of een expressie waar of ONWAAR is. Op basis van het resultaat, een opgegeven waarde retourneren.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*expressie*> | Ja | Booleaans | De expressie om te controleren | 
| <*waardeindienwaar*> | Ja | Alle | De waarde moet worden geretourneerd wanneer de expressie waar is | 
| <*WaardeAlsOnwaar*> | Ja | Alle | De waarde moet worden geretourneerd wanneer de expressie onwaar is | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*opgegeven-retour-waarde*> | Alle | De opgegeven waarde die wordt geretourneerd op basis van of u de expressie is waar of ONWAAR | 
|||| 

*Voorbeeld* 

In dit voorbeeld retourneert `"yes"` omdat de opgegeven expressie ' True retourneert '. Anders wordt het voorbeeld retourneert `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

### <a name="indexof"></a>indexOf

Retourneert de beginpositie of de indexwaarde voor een subtekenreeks. Deze functie is niet hoofdlettergevoelig en indexen beginnen met de waarde 0. 

```
indexOf('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks die de subtekenreeks om te zoeken | 
| <*Brontekst*> | Ja | Reeks | De subtekenreeks om te zoeken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*index-waarde*>| Geheel getal | De positie of index beginwaarde voor de opgegeven subtekenreeks. <p>Als de tekenreeks wordt gevonden, retourneert het getal -1. | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de beginwaarde van de index voor de subtekenreeks "wereld" in de tekenreeks "Hallo wereld":

```
indexOf('hello world', 'world')
```

En dit resultaat wordt weergegeven: `6`

<a name="int"></a>

### <a name="int"></a>int

Retourneert de integer-versie voor een tekenreeks.

```
int('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks die moet worden geconverteerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*geheel getal-resultaat*> | Geheel getal | De versie van het geheel getal voor de opgegeven tekenreeks | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een geheel getal-versie voor de tekenreeks "10":

```
int('10')
```

En dit resultaat wordt weergegeven: `10`

<a name="item"></a>

### <a name="item"></a>Item

Wanneer gebruikt binnen een herhalingsactie via een matrix, retourneert u het huidige item in de matrix tijdens de huidige herhaling van de actie. U kunt ook de waarden ophalen uit de eigenschappen van dat item. 

```
item()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*huidige-matrix-item*> | Alle | Het huidige item in de matrix voor de huidige herhaling van de actie | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de `body` -element van het huidige bericht voor de actie 'Send_an_email' in de huidige iteratie van een voor elke lus:

```
item().body
```

<a name="items"></a>

### <a name="items"></a>items

Retourneert het huidige item van elke cyclus in een voor elke lus. Deze functie binnen de voor-each-lus gebruiken.

```
items('<loopName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Ja | Reeks | De naam van de voor-each-lus | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Item*> | Alle | Het item uit de huidige cyclus in de opgegeven voor elke lus | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt het huidige item opgehaald uit de opgegeven voor elke lus:

```
items('myForEachLoopName')
```

<a name="json"></a>

### <a name="json"></a>json

Retourneert de waarde van het JavaScript Object Notation (JSON) of het object voor een tekenreeks of XML.

```
json('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Tekenreeks of XML | De tekenreeks of XML converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*JSON-resultaat*> | Systeemeigen JSON-type of het object | De systeemeigen waardetype JSON of het object voor de opgegeven tekenreeks of XML. Als de tekenreeks leeg is, retourneert de functie een leeg object. | 
|||| 

*Voorbeeld 1* 

In dit voorbeeld converteert deze tekenreeks naar de JSON-waarde:

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

### <a name="intersection"></a>snijpunt

Retourneert een verzameling met *alleen* de algemene items in de gespecificeerde verzamelingen. Een item moet worden weergegeven in het resultaat, worden weergegeven in de verzamelingen die zijn doorgegeven aan deze functie. Als een of meer items dezelfde naam hebben, wordt het laatste item met die naam in het resultaat weergegeven.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*verzameling1*>, <*verzameling2*>,... | Ja | Matrix of Object, maar niet beide | De verzamelingen van waar u *alleen* de algemene items | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gemeenschappelijke-items*> | Matrix of één Object, respectievelijk | Een verzameling met alleen de algemene items in de gespecificeerde verzamelingen | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt gezocht naar de algemene items via deze matrices:  

```
intersection([1, 2, 3], [101, 2, 1, 10], [6, 8, 1, 2])
```

En retourneert een matrix met *alleen* deze items: `[1, 2]`

<a name="join"></a>

### <a name="join"></a>join

Geretourneerd die een tekenreeks is die alle items uit een matrix heeft en elk teken gescheiden door een *scheidingsteken*.

```
join([<collection>], '<delimiter>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Matrix | De matrix met de items om toe te voegen |  
| <*scheidingsteken*> | Ja | Reeks | Het scheidingsteken dat wordt weergegeven tussen elk teken in de resulterende tekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Tekens1 voor*><*scheidingsteken*><*char2*><*scheidingsteken*>... | Reeks | De resulterende tekenreeks die is gemaakt op basis van de items in de opgegeven matrix |
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een tekenreeks van alle items in deze matrix met het opgegeven teken als scheidingsteken:

```
join([a, b, c], '.')
```

En dit resultaat wordt weergegeven: `"a.b.c"`

<a name="last"></a>

### <a name="last"></a>laatste

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

### <a name="lastindexof"></a>lastIndexOf

Retourneert de beginpositie of de indexwaarde voor het laatste exemplaar van een subtekenreeks. Deze functie is niet hoofdlettergevoelig en indexen beginnen met de waarde 0.

```
lastIndexOf('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks die de subtekenreeks om te zoeken | 
| <*Brontekst*> | Ja | Reeks | De subtekenreeks om te zoeken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*laatste indexwaarde*> | Geheel getal | De positie of index beginwaarde van het laatste exemplaar van de opgegeven subtekenreeks. <p>Als de tekenreeks wordt gevonden, retourneert het getal -1. | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de beginwaarde van de index voor het laatste exemplaar van de subtekenreeks "wereld" in de tekenreeks "Hallo wereld":

```
lastIndexOf('hello world', 'world')
```

En dit resultaat wordt weergegeven: `6`

<a name="length"></a>

### <a name="length"></a>Lengte

Retourneert het aantal items in een verzameling.

```
length('<collection>')
length([<collection>])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix | De verzameling met de items die moeten worden geteld | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*de lengte of aantal*> | Geheel getal | Het aantal items in de verzameling | 
|||| 

*Voorbeeld*

Deze voorbeelden tellen het aantal items in deze verzamelingen: 

```
length('abcd')
length([0, 1, 2, 3])
```

En dit resultaat te retourneren: `4`

<a name="less"></a>

### <a name="less"></a>minder

Controleer of de eerste waarde kleiner is dan de tweede waarde.
Retourneert waar wanneer de eerste waarde lager is, of onwaar geretourneerd wanneer de eerste waarde meer is.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, drijvende komma of tekenreeks | De eerste waarde om te controleren of minder dan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, drijvende komma of tekenreeks, respectievelijk | Het item vergelijking | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar als de eerste waarde kleiner is dan de tweede waarde. Retourneert onwaar als de eerste waarde gelijk aan of groter is dan de tweede waarde is. | 
|||| 

*Voorbeeld*

Deze voorbeelden controleren of de eerste waarde kleiner is dan de tweede waarde.

```
less(5, 10)
less('banana', 'apple')
```

En deze resultaten worden geretourneerd: 

* Eerste voorbeeld: `true`
* Tweede voorbeeld: `false`

<a name="lessOrEquals"></a>

### <a name="lessorequals"></a>lessOrEquals

Controleer of de eerste waarde kleiner zijn dan of gelijk zijn aan de tweede waarde is.
Retourneert waar wanneer de eerste waarde kleiner dan of gelijk zijn is, of onwaar geretourneerd wanneer de eerste waarde meer is.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Geheel getal, drijvende komma of tekenreeks | De eerste waarde om te controleren of minder dan of gelijk zijn aan de tweede waarde | 
| <*compareTo*> | Ja | Geheel getal, drijvende komma of tekenreeks, respectievelijk | Het item vergelijking | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR  | Booleaans | Retourneert waar wanneer de eerste waarde kleiner is dan of gelijk zijn aan de tweede waarde. Retourneert onwaar als de eerste waarde hoger is dan de tweede waarde. |  
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

### <a name="listcallbackurl"></a>listCallbackUrl

Retourneert de "callback-URL' die een actie of trigger aanroept. Deze functie werkt alleen met triggers en acties voor de **HttpWebhook** en **ApiConnectionWebhook** connector typen, maar niet de **handmatig**,  **Terugkeerpatroon**, **HTTP**, en **APIConnection** typen. 

```
listCallbackUrl()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*URL voor terugbellen*> | Reeks | De callback-URL voor een trigger of actie |  
|||| 

*Voorbeeld*

In dit voorbeeld toont een voorbeeld-URL voor terugbellen dat deze functie kan retourneren:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

### <a name="max"></a>max

Retourneert de hoogste waarde van een lijst of een matrix met getallen die wordt ook meegerekend aan beide uiteinden. 

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Getal1*>, <*getal2*>,... | Ja | Geheel getal, drijvende komma of beide | Het instellen van de getallen waarvan u wilt dat de hoogste waarde | 
| [<*Getal1*>, <*getal2*>,...] | Ja | Matrix - geheel getal, drijvende komma of beide | De matrix met getallen van waaruit u wilt dat de hoogste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Max-waarde*> | Geheel getal of een gegevenstype met drijvende komma | De hoogste waarde in de opgegeven matrix of een groep getallen | 
|||| 

*Voorbeeld* 

Deze voorbeelden de hoogste waarde niet ophalen uit de reeks cijfers en de matrix:

```
max(1, 2, 3)
max([1, 2, 3])
```

En dit resultaat te retourneren: `3`

<a name="min"></a>

### <a name="min"></a>min.

Retourneert de laagste waarde van een reeks cijfers of een matrix.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Getal1*>, <*getal2*>,... | Ja | Geheel getal, drijvende komma of beide | Het instellen van de getallen waarvan u wilt dat de laagste waarde | 
| [<*Getal1*>, <*getal2*>,...] | Ja | Matrix - geheel getal, drijvende komma of beide | De matrix met getallen van waaruit u wilt dat de laagste waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*min-waarde*> | Geheel getal of een gegevenstype met drijvende komma | De laagste waarde in de opgegeven set met getallen of de opgegeven matrix | 
|||| 

*Voorbeeld* 

Deze voorbeelden krijgen de laagste waarde in de reeks cijfers en de matrix:

```
min(1, 2, 3)
min([1, 2, 3])
```

En dit resultaat te retourneren: `1`

<a name="mod"></a>

### <a name="mod"></a>Mod

Retourneert de rest van het delen van twee getallen. Als u het resultaat geheel getal zijn, raadpleegt u [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*deeltal*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal waardoor moet worden de *deler* | 
| <*deler*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal dat wordt gedeeld de *deeltal*, maar mag niet 0 zijn. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*modulo resultaat*> | Geheel getal of een gegevenstype met drijvende komma | De rest van het delen van het eerste getal door het tweede getal | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt het eerste getal door het tweede getal gedeeld:

```
mod(3, 2)
```

En dit resultaat te retourneren: `1`

<a name="mul"></a>

### <a name="mul"></a>mul

Retourneert het product van twee getallen vermenigvuldigen.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*multiplicand1*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het aantal om te vermenigvuldigen met *multiplicand2* | 
| <*multiplicand2*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal dat veelvouden *multiplicand1* | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*product-resultaat*> | Geheel getal of een gegevenstype met drijvende komma | Het product van het vermenigvuldigen van het eerste getal door het tweede getal | 
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

### <a name="multipartbody"></a>multipartBody

Retourneert de instantie voor een bepaald deel in de uitvoer van een actie die bestaat uit meerdere delen.

```
multipartBody('<actionName>', <index>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Ja | Reeks | De naam voor de actie die is uitgevoerd met meerdere onderdelen | 
| <*Index*> | Ja | Geheel getal | De indexwaarde voor het onderdeel dat u wilt | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*De hoofdtekst*> | Reeks | De instantie voor het opgegeven onderdeel | 
|||| 

<a name="not"></a>

### <a name="not"></a>niet

Controleer of een expressie onwaar is. Retourneert waar wanneer de expressie onwaar is, of ONWAAR wanneer de waarde true geretourneerd.

```
not(<expression>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*expressie*> | Ja | Booleaans | De expressie om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar als de expressie onwaar is. Retourneert onwaar als de expressie waar is. |  
|||| 

*Voorbeeld 1*

Deze voorbeelden kunt u controleren of de opgegeven expressies ingesteld op false zijn: 

```
not(false)
not(true)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: de expressie is ONWAAR, zodat de functie retourneert `true`.
* Tweede voorbeeld: de expressie is waar, zodat de functie retourneert `false`.

*Voorbeeld 2*

Deze voorbeelden kunt u controleren of de opgegeven expressies ingesteld op false zijn: 

```
not(equals(1, 2))
not(equals(1, 1))
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: de expressie is ONWAAR, zodat de functie retourneert `true`.
* Tweede voorbeeld: de expressie is waar, zodat de functie retourneert `false`.

<a name="or"></a>

### <a name="or"></a>of

Controleer of ten minste één expressie waar is. Retourneert waar wanneer ten minste één expressie waar is of onwaar retourneren wanneer alle ingesteld op false zijn.

```
or(<expression1>, <expression2>, ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Expression1*>, <*expression2*>,... | Ja | Booleaans | De expressies om te controleren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR | Booleaans | Retourneert waar wanneer ten minste één expressie waar is. Retourneert onwaar als alle expressies ingesteld op false zijn. |  
|||| 

*Voorbeeld 1*

Deze voorbeelden kunt u controleren of er ten minste één expressie waar is:

```
or(true, false)
or(false, false)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: ten minste één expressie is waar, zodat de functie retourneert `true`.
* Voorbeeld van de tweede: beide expressies zijn ingesteld op false, zodat de functie retourneert `false`.

*Voorbeeld 2*

Deze voorbeelden kunt u controleren of er ten minste één expressie waar is:

```
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: ten minste één expressie is waar, zodat de functie retourneert `true`.
* Voorbeeld van de tweede: beide expressies zijn ingesteld op false, zodat de functie retourneert `false`.

<a name="parameters"></a>

### <a name="parameters"></a>parameters

Retourneert de waarde voor een parameter die wordt beschreven in de definitie van uw logische app. 

```
parameters('<parameterName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*parameterName*> | Ja | Reeks | De naam voor de parameter waarvan u wilt dat de waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde van parameter*> | Alle | De waarde voor de opgegeven parameter | 
|||| 

*Voorbeeld* 

Stel dat u deze JSON-waarde hebt:

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

### <a name="rand"></a>rand

Retourneert een willekeurig geheel getal van een opgegeven bereik, ook meegerekend alleen aan het begin-eind wordt.

```
rand(<minValue>, <maxValue>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Minimumwaarde*> | Ja | Geheel getal | Het laagste gehele getal in het bereik | 
| <*Maximumwaarde*> | Ja | Geheel getal | Het gehele getal dat volgt op het hoogste gehele getal in het bereik dat de functie kan retourneren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*willekeurige-resultaat*> | Geheel getal | De willekeurig geheel getal geretourneerd uit het opgegeven bereik |  
|||| 

*Voorbeeld*

In dit voorbeeld wordt een willekeurig geheel getal zijn opgehaald uit het opgegeven bereik, met uitzondering van de maximale waarde: 

```
rand(1, 5)
```

En een van deze getallen wordt geretourneerd als het resultaat: `1`, `2`, `3`, of `4` 

<a name="range"></a>

### <a name="range"></a>Bereik

Retourneert een matrix met gehele getallen die met een opgegeven geheel getal zijn begint.

```
range(<startIndex>, <count>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*startIndex*> | Ja | Geheel getal | De waarde van geheel getal zijn dat de matrix als het eerste item begint | 
| <*Aantal*> | Ja | Geheel getal | Het aantal gehele getallen in de matrix | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*bereik-resultaat*>] | Matrix | De matrix met gehele getallen vanaf de opgegeven index |  
|||| 

*Voorbeeld*

In dit voorbeeld wordt een matrix met gehele getallen die begint met de opgegeven index en het opgegeven aantal gehele getallen is gemaakt:

```
range(1, 4)
```

En dit resultaat wordt weergegeven: `[1, 2, 3, 4]`

<a name="replace"></a>

### <a name="replace"></a>vervangen

Vervangen door de opgegeven tekenreeks een subtekenreeks en de resultaattekenreeks retourneren. Deze functie is hoofdlettergevoelig.

```
replace('<text>', '<oldText>', '<newText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks die de subtekenreeks weer die moet worden vervangen | 
| <*OudeTekst*> | Ja | Reeks | De subtekenreeks weer die moet worden vervangen | 
| <*newText*> | Ja | Reeks | De vervangende tekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt tekst*> | Reeks | De bijgewerkte tekenreeks nadat de subtekenreeks vervangen <p>Als de subtekenreeks niet wordt gevonden, retourneert u de oorspronkelijke reeks. | 
|||| 

*Voorbeeld* 

In dit voorbeeld zoekt de subtekenreeks 'oude' in 'de oude tekenreeks' en 'oude' met 'Nieuw' vervangt: 

```
replace('the old string', 'old', 'new')
```

En dit resultaat wordt weergegeven: `"the new string"`

<a name="removeProperty"></a>

### <a name="removeproperty"></a>removeProperty

Een eigenschap van een object verwijderen en het bijgewerkte object geretourneerd.

```
removeProperty(<object>, '<property>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object*> | Ja | Object | Het JSON-object waaruit u wilt verwijderen van een eigenschap | 
| <*De eigenschap*> | Ja | Reeks | De naam op voor de eigenschap te verwijderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt-object*> | Object | Het bijgewerkte JSON-object zonder dat de opgegeven eigenschap | 
|||| 

*Voorbeeld*

In het volgende voorbeeld verwijdert u de `"accountLocation"` eigenschap van een `"customerProfile"` object, dat wordt geconverteerd naar JSON met de [JSON()](#json) functioneren en retourneert het bijgewerkte object:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

### <a name="setproperty"></a>setProperty

Stel de waarde voor eigenschap van een object en het bijgewerkte object geretourneerd. U kunt deze functie gebruiken om toe te voegen een nieuwe eigenschap, of de [addProperty()](#addProperty) functie.

```
setProperty(<object>, '<property>', <value>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Object*> | Ja | Object | Het JSON-object waarvan de eigenschap u wilt instellen | 
| <*De eigenschap*> | Ja | Reeks | De naam op voor de bestaande of nieuwe eigenschap instellen | 
| <*Waarde*> | Ja | Alle | De waarde die moet worden ingesteld voor de opgegeven eigenschap |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt-object*> | Object | Het bijgewerkte JSON-object waarvan de eigenschap instellen | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt de `"accountNumber"` eigenschap op een `"customerProfile"` object, dat wordt geconverteerd naar JSON met de [JSON()](#json) functie. De functie wordt een waarde die is gegenereerd door toegewezen [guid()](#guid) functioneren en retourneert het bijgewerkte JSON-object:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

### <a name="skip"></a>overslaan

Items verwijderen uit het voorste deel van een verzameling en retourneren *alle andere* items.

```
skip([<collection>], <count>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Matrix | De verzameling met items die u wilt verwijderen | 
| <*Aantal*> | Ja | Geheel getal | Een positief geheel getal voor het aantal items te verwijderen op de voorgrond | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*bijgewerkt verzameling*>] | Matrix | De bijgewerkte verzameling na het verwijderen van de opgegeven items | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt één item, de waarde 0, van het begin van de opgegeven matrix verwijderd: 

```
skip([0, 1, 2, 3], 1)
```

En retourneert deze matrix met de resterende items: `[1,2,3]`

<a name="split"></a>

### <a name="split"></a>split

Retourneert een matrix met subtekenreeksen, gescheiden door komma's, op basis van het opgegeven scheidingsteken teken in de oorspronkelijke reeks. 

```
split('<text>', '<delimiter>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks te splitsen in subtekenreeksen op basis van het opgegeven scheidingsteken in de oorspronkelijke reeks |  
| <*scheidingsteken*> | Ja | Reeks | Het teken in de oorspronkelijke tekenreeks die moet worden gebruikt als het scheidingsteken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*substring1*>, <*substring2*>,...] | Matrix | Een matrix met subtekenreeksen uit de oorspronkelijke reeks, gescheiden door komma 's |
|||| 

*Voorbeeld* 

In dit voorbeeld wordt een matrix gemaakt met subtekenreeksen uit de opgegeven tekenreeks op basis van het opgegeven teken als scheidingsteken: 

```
split('a_b_c', '_')
```

En worden deze matrix geretourneerd als het resultaat: `["a","b","c"]`

<a name="startOfDay"></a>

### <a name="startofday"></a>startOfDay

Retourneert het begin van de dag van een tijdstempel. 

```
startOfDay('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De opgegeven timestamp, maar vanaf het hele uur uitgevoerd voor de dag | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt gezocht naar het begin van de dag voor dit tijdstempel:

```
startOfDay('2018-03-15T13:30:30Z')
```

En dit resultaat wordt weergegeven: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

### <a name="startofhour"></a>startOfHour

Retourneert het begin van het uur voor een tijdstempel. 

```
startOfHour('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De opgegeven timestamp, maar vanaf de nul minuten durende is ingeschakeld voor het uur | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt gezocht naar het begin van het uur voor dit tijdstempel:

```
startOfHour('2018-03-15T13:30:30Z')
```

En dit resultaat wordt weergegeven: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

### <a name="startofmonth"></a>startOfMonth

Retourneert het begin van de maand voor een tijdstempel. 

```
startOfMonth('<timestamp>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | Vanaf de eerste dag van de maand op het hele uur uitgevoerd, maar de opgegeven timestamp | 
|||| 

*Voorbeeld* 

In dit voorbeeld retourneert het begin van de maand voor dit tijdstempel:

```
startOfMonth('2018-03-15T13:30:30Z')
```

En dit resultaat wordt weergegeven: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

### <a name="startswith"></a>startsWith

Controleren of een tekenreeks met een specifieke subtekenreeks begint. Retourneert waar wanneer de subtekenreeks wordt gevonden, of onwaar retourneren wanneer niet gevonden. Deze functie is niet hoofdlettergevoelig.

```
startsWith('<text>', '<searchText>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks om te controleren | 
| <*Brontekst*> | Ja | Reeks | De eerste tekenreeks om te zoeken | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| waar of ONWAAR  | Booleaans | Retourneert waar wanneer de eerste subtekenreeks wordt gevonden. Retourneert de waarde false wanneer niet gevonden. | 
|||| 

*Voorbeeld 1* 

In dit voorbeeld controleert of de tekenreeks "Hallo wereld" met de subtekenreeks "Hallo begint":

```
startsWith('hello world', 'hello')
```

En dit resultaat wordt weergegeven: `true`

*Voorbeeld 2*

In dit voorbeeld controleert of de tekenreeks "Hallo wereld" met de subtekenreeks 'groeten begint':

```
startsWith('hello world', 'greetings')
```

En dit resultaat wordt weergegeven: `false`

<a name="string"></a>

### <a name="string"></a>tekenreeks

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
| <*tekenreeks-waarde*> | Reeks | De tekenreeksversie voor de opgegeven waarde | 
|||| 

*Voorbeeld 1* 

In dit voorbeeld maakt u de tekenreeksversie voor dit nummer:

```
string(10)
```

En dit resultaat wordt weergegeven: `"10"`

*Voorbeeld 2*

In dit voorbeeld maakt u een tekenreeks op voor het opgegeven JSON-object en maakt gebruik van de backslash-teken (\\) als een escape-teken voor het aanhalingsteken (").

```
string( { "name": "Sophie Owen" } )
```

En dit resultaat wordt weergegeven: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

### <a name="sub"></a>Sub

Retourneert het resultaat van af te trekken van het tweede nummer van het eerste getal.

```
sub(<minuend>, <subtrahend>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*aftrektal*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal waarvan u wilt de *aftrekker* | 
| <*aftrekker*> | Ja | Geheel getal of een gegevenstype met drijvende komma | Het getal moet worden afgetrokken van de *aftrektal* | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Resultaat*> | Geheel getal of een gegevenstype met drijvende komma | Het resultaat van het tweede nummer van het eerste getal af te trekken | 
|||| 

*Voorbeeld* 

In dit voorbeeld trekt het tweede nummer van het eerste getal:

```
sub(10.3, .3)
```

En dit resultaat wordt weergegeven: `10`

<a name="substring"></a>

### <a name="substring"></a>de subtekenreeks

Tekens retourneren uit een tekenreeks, beginnend vanaf de opgegeven positie, of de index. Index waarden beginnen met de waarde 0. 

```
substring('<text>', <startIndex>, <length>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks waarvan de gewenste tekens | 
| <*startIndex*> | Ja | Geheel getal | Een positief getal voor de beginpositie of indexwaarde | 
| <*Lengte*> | Ja | Geheel getal | Een positief aantal tekens dat u wilt dat in de subtekenreeks | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*de subtekenreeks-resultaat*> | Reeks | Een subtekenreeks met het opgegeven aantal tekens, vanaf de positie van de opgegeven index in de brontekenreeks | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een subtekenreeks vijf tekens van de opgegeven tekenreeks, beginnend met de indexwaarde 6:

```
substring('hello world', 6, 5)
```

En dit resultaat wordt weergegeven: `"world"`

<a name="subtractFromTime"></a>

### <a name="subtractfromtime"></a>subtractFromTime

Een aantal eenheden van een tijdstempel aftrekken. Zie ook [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks met het tijdstempel | 
| <*interval*> | Ja | Geheel getal | Het aantal eenheden van de opgegeven tijd om af te trekken | 
| <*timeUnit*> | Ja | Reeks | De tijdseenheid voor gebruik met *interval*: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month', 'Year' | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*bijgewerkt timestamp*> | Reeks | De tijdstempel minus het opgegeven aantal tijdseenheden | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt één dag van deze timestamp afgetrokken:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day') 
```

En dit resultaat wordt weergegeven: `"2018-01-01T00:00:00:0000000Z"`

*Voorbeeld 2*

In dit voorbeeld wordt één dag van deze timestamp afgetrokken:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D') 
```

En dit resultaat wordt met behulp van de optionele "D"-indeling: `"Monday, January, 1, 2018"`

<a name="take"></a>

### <a name="take"></a>toets maken

Items retourneren vanaf het begin van een verzameling. 

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Verzameling*> | Ja | Tekenreeks of matrix | De verzameling met items die u wilt | 
| <*Aantal*> | Ja | Geheel getal | Een positief geheel getal voor het aantal items die u wilt dat uit het voorste deel | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*subset*> of [<*subset*>] | Tekenreeks of matrix, respectievelijk | Een tekenreeks of een matrix met het opgegeven aantal items die zijn overgenomen uit het begin van de oorspronkelijke verzameling | 
|||| 

*Voorbeeld*

Het opgegeven aantal items ophalen in deze voorbeelden uit het voorste deel van deze verzamelingen:

```
take('abcde`, 3)
take([0, 1, 2, 3, 4], 3)
```

En deze resultaten worden geretourneerd:

* Eerste voorbeeld: `"abc"`
* Tweede voorbeeld: `[0, 1, 2]`

<a name="ticks"></a>

### <a name="ticks"></a>tikken

Retourneert de `ticks` eigenschapwaarde voor een opgegeven timestamp. Een *maatstreepjes* is een interval van 100 nanoseconden.

```
ticks('<timestamp>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tijdstempel*> | Ja | Reeks | De tekenreeks voor een tijdstempel | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*tikken-nummer*> | Geheel getal | Het aantal tikken sinds de opgegeven timestamp | 
|||| 

<a name="toLower"></a>

### <a name="tolower"></a>toLower

Retourneert een tekenreeks in kleine letters indeling. Als een teken in de tekenreeks geen in kleine letters, blijft het teken ongewijzigd in de geretourneerde tekenreeks.

```
toLower('<text>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks die moet worden geretourneerd in kleine letters-indeling | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*tekst met kleine letters*> | Reeks | De oorspronkelijke reeks in kleine letters-indeling | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt deze tekenreeks geconverteerd naar kleine letters: 

```
toLower('Hello World')
```

En dit resultaat wordt weergegeven: `"hello world"`

<a name="toUpper"></a>

### <a name="toupper"></a>toUpper

Retourneert een tekenreeks in hoofdletters. Als een teken in de tekenreeks geen hoofdletters, blijft het teken ongewijzigd in de geretourneerde tekenreeks.

```
toUpper('<text>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks die moet worden geretourneerd in hoofdletters | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*tekst met hoofdletters*> | Reeks | De oorspronkelijke reeks in hoofdletters | 
|||| 

*Voorbeeld* 

In dit voorbeeld converteert deze tekenreeks naar hoofdletters:

```
toUpper('Hello World')
```

En dit resultaat wordt weergegeven: `"HELLO WORLD"`

<a name="trigger"></a>

### <a name="trigger"></a>Trigger

Retourneert de uitvoer van een trigger in runtime, of waarden van andere naam en waarde-paren van JSON, die u aan een expressie toewijzen kunt. 

* In de invoer van een trigger retourneert deze functie de uitvoer van de vorige uitvoering. 

* In de voorwaarde van een trigger retourneert deze functie de uitvoer van de huidige uitvoering. 

Standaard de functie verwijst naar de volledige triggerobject, maar u kunt optioneel een eigenschap opgeven waarvan de waarde die u wilt. Bovendien is deze functie is een verkorte versie van versies die beschikbaar zijn, raadpleegt u [triggerOutputs()](#triggerOutputs) en [triggerBody()](#triggerBody). 

```
trigger()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*trigger-uitvoer*> | Reeks | De uitvoer van een trigger tijdens runtime | 
|||| 

<a name="triggerBody"></a>

### <a name="triggerbody"></a>triggerBody

Resultaat van een trigger `body` uitvoer tijdens runtime. De korte benaming voor `trigger().outputs.body`. Zie [trigger()](#trigger). 

```
triggerBody()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*trigger-instantie-uitvoer*> | Reeks | De `body` uitvoer van de trigger | 
|||| 

<a name="triggerFormDataMultiValues"></a>

### <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

Retourneert een matrix met waarden die overeenkomen met een naam in van een trigger *formuliergegevens* of *formuliercodes* uitvoer. 

```
triggerFormDataMultiValues('<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan u wilt dat de waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [<*matrix met sleutel waarden*>] | Matrix | Een matrix met de waarden die overeenkomen met de opgegeven sleutel | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt een matrix gemaakt van de sleutel 'feedUrl'-waarde in een RSS-trigger formuliergegevens of formuliercodes uitvoer: 

```
triggerFormDataMultiValues('feedUrl')
```

En worden deze matrix geretourneerd als een voorbeeld van resultaat: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

### <a name="triggerformdatavalue"></a>triggerFormDataValue

Retourneert een tekenreeks zijn met één waarde die overeenkomt met een naam in van een trigger *formuliergegevens* of *formuliercodes* uitvoer. Als de functie vindt meer dan één overeenkomst, is de functie genereert een fout.

```
triggerFormDataValue('<key>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Sleutel*> | Ja | Reeks | De naam voor de sleutel waarvan u wilt dat de waarde |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*sleutel / waarde-*> | Reeks | De waarde in de opgegeven sleutel | 
|||| 

*Voorbeeld* 

In dit voorbeeld maakt u een tekenreeks van de sleutelwaarde 'feedUrl' in een RSS-trigger formuliergegevens of formuliercodes uitvoer:

```
triggerFormDataValue('feedUrl')
```

En worden deze tekenreeks geretourneerd als een voorbeeld van resultaat: `"http://feeds.reuters.com/reuters/topNews"` 

<a name="triggerMultipartBody"></a>

### <a name="triggermultipartbody"></a>triggerMultipartBody

Retourneert de instantie voor een bepaald deel in de uitvoer van een trigger die bestaat uit meerdere delen. 

```
triggerMultipartBody(<index>)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Index*> | Ja | Geheel getal | De indexwaarde voor het onderdeel dat u wilt |
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*De hoofdtekst*> | Reeks | De instantie voor het opgegeven onderdeel van een trigger meerdelige uitvoer | 
|||| 

<a name="triggerOutputs"></a>

### <a name="triggeroutputs"></a>triggerOutputs

Retourneert de uitvoer van een trigger in runtime, of waarden van andere naam en waarde-paren van JSON. De korte benaming voor `trigger().outputs`. Zie [trigger()](#trigger). 

```
triggerOutputs()
```

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*trigger-uitvoer*> | Reeks | De uitvoer van een trigger tijdens runtime  | 
|||| 

<a name="trim"></a>

### <a name="trim"></a>Trim

Voorloop-en volgspaties verwijderen uit een tekenreeks en retourneert de bijgewerkte tekenreeks.

```
trim('<text>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Tekst*> | Ja | Reeks | De tekenreeks die de voorloopspaties en afsluitende spaties verwijderen | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*updatedText*> | Reeks | Een bijgewerkte versie van de oorspronkelijke reeks zonder voorloopspatie of afsluitende spatie | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt de witruimte voorloop- en volgspaties uit de tekenreeks "Hallo wereld" verwijderd.  

```
trim(' Hello World  ')
```

En dit resultaat wordt weergegeven: `"Hello World"`

<a name="union"></a>

### <a name="union"></a>Union

Retourneert een verzameling met *alle* de items van de gespecificeerde verzamelingen. Een item kan worden weergegeven in het resultaat, worden weergegeven in een verzameling die is doorgegeven aan deze functie. Als een of meer items dezelfde naam hebben, wordt het laatste item met die naam in het resultaat weergegeven. 

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*verzameling1*>, <*verzameling2*>,...  | Ja | Matrix of Object, maar niet beide | De verzamelingen van waar u *alle* de items | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*updatedCollection*> | Matrix of één Object, respectievelijk | Een verzameling met alle items uit de gespecificeerde verzamelingen - geen dubbele waarden | 
|||| 

*Voorbeeld* 

In dit voorbeeld wordt *alle* de items van deze verzamelingen: 

```
union([1, 2, 3], [1, 2, 10, 101])
```

En dit resultaat wordt weergegeven: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

### <a name="uricomponent"></a>uriComponent

Retourneert een uniform resource identifier (URI) gecodeerde versie voor een tekenreeks-URL-onveilige tekens te vervangen door een escape-tekens. Deze functie wilt gebruiken in plaats van [encodeUriComponent()](#encodeUriComponent). Hoewel beide functies werken op dezelfde manier, `uriComponent()` verdient de voorkeur.

```
uriComponent('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks die moet worden geconverteerd naar de URI-notatie | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gecodeerde-uri*> | Reeks | De URI-gecodeerde tekenreeks met escape-tekens | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u een URI-gecodeerde versie voor deze tekenreeks:

```
uriComponent('https://contoso.com')
```

En dit resultaat wordt weergegeven: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

### <a name="uricomponenttobinary"></a>uriComponentToBinary

Retourneert de binaire versie voor een uniform resource identifier (URI)-component.

```
uriComponentToBinary('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De URI-gecodeerde tekenreeks converteren | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*binair-voor-encoded-uri*> | Reeks | De binaire versie voor de URI-gecodeerde tekenreeks. De binaire inhoud wordt base64-gecodeerd en wordt vertegenwoordigd door `$content`. | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u de binaire versie voor deze URI-gecodeerde tekenreeks: 

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

En dit resultaat wordt weergegeven: 

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

### <a name="uricomponenttostring"></a>uriComponentToString

Retourneert dat de tekenreeksversie voor een uniform resource identifier (URI) gecodeerde tekenreeks, effectief decoderen van de URI-gecodeerde tekenreeks.

```
uriComponentToString('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De URI-gecodeerde tekenreeks moet worden gedecodeerd | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*gedecodeerd-uri*> | Reeks | De gedecodeerde versie voor de URI-gecodeerde tekenreeks | 
|||| 

*Voorbeeld*

In dit voorbeeld maakt u de gedecodeerde tekenreeksversie voor deze URI-gecodeerde tekenreeks: 

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

En dit resultaat wordt weergegeven: `"https://contoso.com"` 

<a name="uriHost"></a>

### <a name="urihost"></a>uriHost

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
| <*Host-waarde*> | Reeks | De `host` waarde voor de opgegeven URI | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt gezocht naar de `host` waarde voor deze URI: 

```
uriHost('https://www.localhost.com:8080')
```

En dit resultaat wordt weergegeven: `"www.localhost.com"`

<a name="uriPath"></a>

### <a name="uripath"></a>uriPath

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

### <a name="uripathandquery"></a>uriPathAndQuery

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
| <*pad-query-waarde*> | Reeks | De `path` en `query` waarden voor de opgegeven URI. Als `path` niet een waarde opgeeft, retourneert het teken '/'. | 
|||| 

*Voorbeeld*

In dit voorbeeld wordt gezocht naar de `path` en `query` waarden voor deze URI:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

En dit resultaat wordt weergegeven: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

### <a name="uriport"></a>uriPort

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
| <*poort-waarde*> | Geheel getal | De `port` waarde voor de opgegeven URI. Als `port` niet een waarde opgeeft, kunt u de standaardpoort voor het protocol retourneren. | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de `port` waarde voor deze URI:

```
uriPort('http://www.localhost:8080')
```

En dit resultaat wordt weergegeven: `8080`

<a name="uriQuery"></a>

### <a name="uriquery"></a>uriQuery

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
| <*waarde opvragen*> | Reeks | De `query` waarde voor de opgegeven URI | 
|||| 

*Voorbeeld*

In dit voorbeeld retourneert de `query` waarde voor deze URI: 

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

En dit resultaat wordt weergegeven: `"?date=today"`

<a name="uriScheme"></a>

### <a name="urischeme"></a>uriScheme

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

### <a name="utcnow"></a>utcNow

Retourneert de huidige tijdstempel. 

```
utcNow('<format>')
```

Desgewenst kunt u een andere indeling met de <*indeling*> parameter. 


| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Indeling*> | Nee | Reeks | Een van beide een [één indelingsopgave](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of een [aangepast](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). De standaardnotatie voor de tijdstempel is ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (jjjj-MM-ddT:mm:ss:fffffffK), die voldoet aan [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) en behoudt u informatie over de tijdzone. | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*huidige tijdstempel*> | Reeks | De huidige datum en tijd | 
|||| 

*Voorbeeld 1*

Stel vandaag is 15 April 2018 om 1:00:00 uur. In dit voorbeeld wordt de huidige tijdstempel: 

```
utcNow()
```

En dit resultaat wordt weergegeven: `"2018-04-15T13:00:00.0000000Z"`

*Voorbeeld 2*

Stel vandaag is 15 April 2018 om 1:00:00 uur. In dit voorbeeld wordt de huidige tijdstempel met behulp van de optionele "D"-indeling:

```
utcNow('D')
```

En dit resultaat wordt weergegeven: `"Sunday, April 15, 2018"`

<a name="variables"></a>

### <a name="variables"></a>Variabelen

Retourneert de waarde voor een opgegeven variabele. 

```
variables('<variableName>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*variableName*> | Ja | Reeks | De naam van de variabele waarvan u wilt dat de waarde | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*waarde van variabele*> | Alle | De waarde voor de opgegeven variabele | 
|||| 

*Voorbeeld*

Stel dat de huidige waarde voor een variabele "numItems" is 20. In dit voorbeeld wordt de integerwaarde voor deze variabele:

```
variables('numItems')
```

En dit resultaat wordt weergegeven: `20`

<a name="workflow"></a>

### <a name="workflow"></a>werkstroom

Als resultaat de details over de werkstroom zelf tijdens runtime. 

```
workflow().<property>
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*De eigenschap*> | Nee | Reeks | De naam op voor de eigenschap van de werkstroom waarvan u wilt dat de waarde <p>Een werkstroomobject heeft deze eigenschappen: **naam**, **type**, **id**, **locatie**, en **uitvoeren**. De **uitvoeren** eigenschapswaarde is ook een object met deze eigenschappen: **naam**, **type**, en **id**. | 
||||| 

*Voorbeeld*

In dit voorbeeld retourneert de naam voor de huidige uitvoering van een werkstroom:

```
workflow().run.name
```

<a name="xml"></a>

### <a name="xml"></a>xml

De XML-versie voor een tekenreeks met een JSON-object retourneren. 

```
xml('<value>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*Waarde*> | Ja | Reeks | De tekenreeks met de JSON-object converteren <p>Het JSON-object moet slechts één root-eigenschap hebben. <br>Gebruik de backslashteken (\\) als een escape-teken voor de dubbele aanhalingstekens ("). | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*XML-versie*> | Object | Het gecodeerde XML-bestand voor de opgegeven tekenreeks of JSON-object | 
|||| 

*Voorbeeld 1*

In dit voorbeeld maakt u de XML-versie voor deze tekenreeks, die een JSON-object bevat: 

`xml( '{ \"name\": \"Sophia Owen\" }' )`

En dit resultaat XML retourneert: 

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

Dit voorbeeld wordt de XML voor een tekenreeks zijn met dit JSON-object gemaakt:

`xml( '{ \"person\": { \"name\": \"Sophia Owen\", \"city\": \"Seattle\" } }' )`

En dit resultaat XML retourneert: 

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

### <a name="xpath"></a>XPath

Controleren of XML knooppunten of waarden die overeenkomen met een (XML Path-taal) XPath-expressie en retourneert de overeenkomende waarden of knooppunten. Een XPath-expressie, of alleen 'XPath', kunt u een XML-documentstructuur navigeren, zodat u knooppunten of compute waarden in de XML-inhoud selecteren kunt.

```
xpath('<xml>', '<xpath>')
```

| Parameter | Vereist | Type | Beschrijving | 
| --------- | -------- | ---- | ----------- | 
| <*XML*> | Ja | Alle | De XML-tekenreeks te zoeken naar knooppunten of waarden die overeenkomen met de waarde van een XPath-expressie | 
| <*XPath*> | Ja | Alle | De XPath-expressie gebruikt om te zoeken naar overeenkomende XML-knooppunt of waarden | 
||||| 

| Retourwaarde | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*XML-knooppunt*> | XML | Een XML-knooppunt wanneer slechts één knooppunt overeenkomt met de opgegeven XPath-expressie | 
| <*Waarde*> | Alle | De waarde van een XML-knooppunt wanneer slechts één waarde overeenkomt met de opgegeven XPath-expressie | 
| [<*xml-knooppunt1*>, <*xml-Knooppunt2*>,...] </br>-of- </br>[<*value1*>, <*value2*>,...] | Matrix | Een matrix met waarden die overeenkomen met de opgegeven XPath-expressie of XML-knooppunt | 
|||| 

*Voorbeeld 1*

In dit voorbeeld wordt gezocht naar knooppunten die overeenkomen met de `<name></name>` knooppunt in de opgegeven argumenten en retourneert een matrix met de waarden van het knooppunt: 

`xpath(xml(parameters('items')), '/produce/item/name')`

Hier volgen de argumenten:

* De tekenreeks 'items', die deze XML-code bevat:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  Het voorbeeld wordt de [parameters()](#parameters) werken voor de XML-tekenreeks van het argument 'items', maar moet ook de tekenreeks converteren naar XML-indeling met behulp van de [xml()](#xml) functie. 

* Deze XPath-expressie die wordt doorgegeven als een tekenreeks:

  `"/produce/item/name"`

Hier volgt de resultaat-matrix met knooppunten die overeenkomen met `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*Voorbeeld 2*

In dit voorbeeld van de te volgen in voorbeeld 1, wordt gezocht naar de knooppunten die overeenkomen met de `<count></count>` knooppunt en voegt deze knooppuntwaarden met de `sum()` functie:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

En dit resultaat wordt weergegeven: `30`

*Voorbeeld 3*

In dit voorbeeld vindt beide expressies knooppunten die overeenkomen met de `<location></location>` knooppunt in de opgegeven argumenten, waaronder XML met een naamruimte. De expressies gebruiken de backslash-teken (\\) als een escape-teken voor de dubbele aanhalingstekens (").

* *Expressie 1*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *Expressie 2* 

  `xpath(xml(body('Http')), '/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]')`

Hier volgen de argumenten:

* Deze XML, waaronder de XML-document-naamruimte, `xmlns="http://contoso.com"`: 

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* De XPath-expressie hier:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]`

Hier is het resultaat-knooppunt dat overeenkomt met de `<location></location` knooppunt:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Voorbeeld 4*

In voorbeeld 3 te volgen, in dit voorbeeld wordt de waarde in de `<location></location>` knooppunt: 

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

En dit resultaat wordt weergegeven: `"Paris"`

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Definitietaal van werkstroom](../logic-apps/logic-apps-workflow-definition-language.md)

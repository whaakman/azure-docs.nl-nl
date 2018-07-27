---
title: Schemaverwijzing voor Definitietaal van werkstroom - Azure Logic Apps | Microsoft Docs
description: Schrijven van aangepaste werkstroomdefinities voor Azure Logic Apps met de taal van de definitie van werkstroom
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 04/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 9268ca3db6c99c4e660690e25a2331a1fa1cdf96
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263671"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Definitietaal van werkstroom in Azure Logic Apps-schemaverwijzing

Wanneer u een werkstroom voor logische Apps met maakt [Azure Logic Apps](../logic-apps/logic-apps-overview.md), uw onderliggende werkstroomdefinitie beschrijving van de werkelijke logica die wordt uitgevoerd voor uw logische app. Deze beschrijving volgt een structuur die is gedefinieerd en gevalideerd door het schema Definitietaal van werkstroom, die gebruikmaakt van [JavaScript Object Notation (JSON)](https://www.json.org/). 
  
## <a name="workflow-definition-structure"></a>Structuur van de werkstroom-definitie

De werkstroomdefinitie van een is ten minste één trigger waarmee een instantie van uw logische app, plus een of meer acties die uw logische app wordt uitgevoerd. 

Hier volgt de structuur op hoog niveau voor de werkstroomdefinitie van een:  
  
```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```
  
| Element | Vereist | Beschrijving | 
|---------|----------|-------------| 
| definitie | Ja | Het eerste element voor uw werkstroomdefinitie | 
| $schema | Alleen wanneer u extern verwijst naar een definitie van de werkstroom | De locatie voor de JSON-schema-bestand dat beschrijft de versie Definitietaal van werkstroom, die u hier kunt vinden: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| Sjablooneigenschap | Nee | Het versienummer voor de werkstroomdefinitie van uw, namelijk '1.0.0.0"standaard. Om te identificeren en controleer of de definitie van de juiste bij het implementeren van een werkstroom, Geef een waarde te gebruiken. | 
| parameters | Nee | De definities voor een of meer parameters die het doorgeven van gegevens in uw werkstroom <p><p>Maximum aantal parameters: 50 | 
| Triggers | Nee | De definities voor een of meer triggers die exemplaar maken van uw werkstroom. U kunt meer dan één trigger, maar alleen met de Werkstroomdefinitietaal, visueel niet via de ontwerper van logische Apps definiëren. <p><p>Maximale triggers: 10 | 
| acties | Nee | De definities voor een of meer acties uit te voeren op de werkstroom-runtime <p><p>Maximaal aantal acties: 250 | 
| uitvoer | Nee | De definities voor de uitvoer die uit een werkstroom ophalen <p><p>Maximale uitvoer: 10 |  
|||| 

## <a name="parameters"></a>Parameters

In de `parameters` sectie, het definiëren van de werkstroomparameters die uw logische app tijdens de implementatie worden gebruikt voor het accepteren van invoer. Zowel parameterdeclaraties en parameterwaarden zijn vereist tijdens de implementatie. Voordat u deze parameters in andere gedeelten van de werkstroom gebruiken kunt, ervoor zorgen dat u de parameters in deze secties declareren. 

Hier volgt de algemene structuur voor een parameterdefinitie:  

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": { 
      "key": { 
        "name": "<key-value>"
      } 
    }
  }
},
```

| Element | Vereist | Type | Beschrijving |  
|---------|----------|------|-------------|  
| type | Ja | int, float, string, securestring, bool, matrix, JSON-object, secureobject <p><p>**Houd er rekening mee**: voor alle wachtwoorden, sleutels en geheimen, gebruikt u de `securestring` en `secureobject` omdat de `GET` bewerking deze typen niet retourneren. | Het type voor de parameter |
| defaultValue | Nee | Gelijk aan `type` | De standaardwaarde voor parameter wanneer er geen waarde is opgegeven wanneer de werkstroom waarmee een wordt gemaakt | 
| allowedValues | Nee | Gelijk aan `type` | Een matrix met waarden die de parameter kunt accepteren |  
| metagegevens | Nee | JSON-object | Andere details van de parameter, bijvoorbeeld de naam of een leesbare beschrijving op voor uw logische app of het moment van ontwerp gegevens die worden gebruikt door Visual Studio of andere hulpprogramma 's |  
||||

## <a name="triggers-and-actions"></a>Triggers en acties  

In het werkstroomdefinitie van een, de `triggers` en `actions` secties definiëren de aanroepen die ontstaan tijdens de uitvoering van uw werkstroom. Zie voor de syntaxis en meer informatie over deze secties, [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Uitvoer 

In de `outputs` sectie, het definiëren van de gegevens die door uw werkstroom kunt wanneer u klaar bent met. Bijvoorbeeld, voor het volgen van een specifieke status of de waarde van elke uitvoering, opgeven dat de werkstroomuitvoer die gegevens retourneert. 

> [!NOTE]
> Als u reageert op binnenkomende aanvragen van een service een REST-API, gebruik dan niet `outputs`. Gebruik in plaats daarvan de `Response` actietype. Zie voor meer informatie, [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).

Hier volgt de algemene structuur voor de uitvoerdefinitie van een: 

```json
"outputs": {
  "<key-name>": {  
    "type": "<key-type>",  
    "value": "<key-value>"  
  }
} 
```

| Element | Vereist | Type | Beschrijving | 
|---------|----------|------|-------------| 
| <*naam van de sleutel*> | Ja | Reeks | De naam van de sleutel voor de uitvoer waarde retourneren |  
| type | Ja | int, float, string, securestring, bool, matrix, JSON-object | Het type voor de geretourneerde waarde van uitvoer | 
| waarde | Ja | Gelijk aan `type` | De geretourneerde waarde van uitvoer |  
||||| 

Als u de uitvoer van een werkstroom, bekijk de uitvoeringsgeschiedenis en details in de Azure-portal van de logische app of gebruik de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows). U kunt ook uitvoer doorgeven aan externe systemen, bijvoorbeeld Power BI, zodat u kunt dashboards maken. 

<a name="expressions"></a>

## <a name="expressions"></a>Expressies

Met JSON hebt u letterlijke waarden die aanwezig zijn in de ontwerpfase, bijvoorbeeld:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

U kunt ook de waarden die niet tot uitvoering hebben. Als u wilt deze waarden vertegenwoordigen, kunt u *expressies*, die tijdens de uitvoering worden geëvalueerd. Een expressie is een reeks met een of meer [functies](#functions), [operators](#operators), variabelen, expliciete waarden of constanten. In het definitie van de werkstroom, kunt u een expressie overal in een JSON-tekenreeks-waarde door het voorvoegsel van de expressie met het apenstaartje (\@). Bij het evalueren van een expressie die een JSON-waarde vertegenwoordigt de hoofdtekst van de expressie wordt opgehaald door het verwijderen van de \@ teken en altijd resultaten in een andere JSON-waarde. 

Bijvoorbeeld: voor de eerder gedefinieerde `customerName` eigenschap, kunt u de waarde van de eigenschap ophalen met behulp van de [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) werken in een expressie en toe te wijzen die waarde aan de `accountName` eigenschap:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Tekenreeks interpolatie* ook kunt u meerdere expressies in tekenreeksen die zijn ingepakt door de \@ teken en de accolades ({}). Hier volgt de syntaxis:

```json
@{ "<expression1>", "<expression2>" }
```

Het resultaat is altijd een tekenreeks, waardoor deze mogelijkheid die vergelijkbaar is met de `concat()` functioneren, bijvoorbeeld: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Als u een letterlijke tekenreeks begint hebt met de \@ teken, het voorvoegsel de \@ teken met een andere \@ teken als een escape-teken: \@\@

Deze voorbeelden geven weer hoe expressies worden geëvalueerd:

| JSON-waarde | Resultaat |
|------------|--------| 
| "Sophia Owen" | Deze tekens retourneren: 'Sophia Owen' |
| "[1] array" | Deze tekens retourneren: 'array [1]' |
| "\@\@" | Deze tekens retourneren als een tekenreeks met één teken: '\@' |   
| " \@" | Deze tekens retourneren als een tekenreeks van twee tekens: ' \@' |
|||

Stel dat u 'myBirthMonth' gelijk is aan "Januari" en "myAge" gelijk aan het aantal 42 definiëren voor deze voorbeelden:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Deze voorbeelden laten zien hoe de volgende expressies worden geëvalueerd:

| JSON-expressie | Resultaat |
|-----------------|--------| 
| "\@parameters('myBirthMonth')" | Deze tekenreeks als resultaat: "Januari" |  
| "\@{parameters('myBirthMonth')}" | Deze tekenreeks als resultaat: "Januari" |  
| "\@parameters('myAge')" | Retourneert dit getal: 42 |  
| "\@{parameters('myAge')}" | Retourneert dit getal als een tekenreeks: "42" |  
| "Mijn leeftijd is \@{parameters('myAge')}" | Deze tekenreeks als resultaat: "42 mijn leeftijd is" |  
| "\@concat ('mijn leeftijd is', string(parameters('myAge')))" | Deze tekenreeks als resultaat: "42 mijn leeftijd is" |  
| "Mijn leeftijd is \@ \@{parameters('myAge')}" | Deze tekenreeks, waaronder de expressie als resultaat: "mijn leeftijd is \@{parameters('myAge')}' | 
||| 

Wanneer u visueel in de ontwerper van logische Apps werkt, kunt u bijvoorbeeld expressies via de opbouwfunctie voor expressies maken: 

![Ontwerper van logische Apps > opbouwfunctie voor expressies](./media/logic-apps-workflow-definition-language/expression-builder.png)

Wanneer u klaar bent, de expressie wordt weergegeven voor de overeenkomstige eigenschap in de definitie van de werkstroom, bijvoorbeeld, de `searchQuery` eigenschap hier:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
       "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>Operators

In [expressies](#expressions) en [functies](#functions), operators uitvoeren van specifieke taken, zoals een eigenschap of een waarde in een matrix. 

| Operator | Taak | 
|----------|------|
| ' | Voor het gebruik van een letterlijke tekenreeks zijn als invoer of in expressies en functies, verpakken de tekenreeks alleen met enkele aanhalingstekens, bijvoorbeeld `'<myString>'`. Gebruik geen dubbele aanhalingstekens (""), die in strijd zijn met de JSON-indeling om een volledige expressie. Bijvoorbeeld: <p>**Ja**: length('Hello') </br>**Geen**: length("Hello") <p>Als u matrices of getallen worden gehaald, hoeft u geen interpunctie wrapping. Bijvoorbeeld: <p>**Ja**: lengte ([1, 2, 3]) </br>**Geen**: lengte ('[1, 2, 3]') | 
| [] | Als u wilt verwijzen naar een waarde op een specifieke positie (index) in een matrix, gebruikt u vierkante haken. Als u bijvoorbeeld het tweede item in een matrix ophalen: <p>`myArray[1]` | 
| . | Als u wilt verwijzen naar een eigenschap van een object, gebruikt u de puntoperator. Bijvoorbeeld, om op te halen de `name` eigenschap voor een `customer` JSON-object: <p>`"@parameters('customer').name"` | 
| ? | Als u verwijst naar null eigenschappen in een object zonder een runtime-fout, gebruikt u de operator vraagteken. Bijvoorbeeld, voor het afhandelen van null uitvoer van een trigger, kunt u deze expressie: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Functions

Sommige expressies worden de waarden van de runtime-acties die mogelijk nog niet wanneer een logische app begint te lopen. U kunt gebruiken om te verwijzen naar of te werken met deze waarden in expressies, [ *functies*](../logic-apps/workflow-definition-language-functions-reference.md). Bijvoorbeeld, kunt u wiskundige functies voor berekeningen, zoals de [add()](../logic-apps/workflow-definition-language-functions-reference.md#add) functie, de som van gehele getallen of tekst berekent. Zie voor gedetailleerde informatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).
Of, leren over de functies en hun algemeen gebruik.

Hier volgen een paar voorbeelden van taken die u met functions uitvoeren kunt: 

| Taak | Syntaxis van de functie | Resultaat | 
| ---- | --------------- | ------ | 
| Retourneert een tekenreeks in kleine letters indeling. | toLower ('<*tekst*>') <p>Bijvoorbeeld: toLower('Hello') | "Hallo" | 
| Retourneert een globally unique identifier (GUID). | GUID() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Dit voorbeeld laat zien hoe krijgt u de waarde van de `customerName` parameter en wijs deze waarde aan de `accountName` eigenschap met behulp van de [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) functie in een expressie:

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

Zie voor gedetailleerde informatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).
Of, leren over de functies op basis van hun algemeen gebruik.

<a name="string-functions"></a>

### <a name="string-functions"></a>Tekenreeksfuncties

Als u wilt werken met tekenreeksen, kunt u deze tekenreeksfuncties en ook enkele [verzameling functies](#collection-functions). Tekenreeks-functies werken alleen op tekenreeksen. 

| Tekenreeksfunctie | Taak | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Twee of meer tekenreeksen combineren en retourneert de gecombineerde tekenreeks. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Controleer of een tekenreeks met de opgegeven subtekenreeks eindigt. | 
| [GUID](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Genereer een globally unique identifier (GUID) als een tekenreeks. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Retourneert de beginpositie voor een subtekenreeks. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Retourneert de positie van de einddatum voor een subtekenreeks. | 
| [vervangen](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Vervangen door de opgegeven tekenreeks een subtekenreeks en retourneert de bijgewerkte tekenreeks. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Retourneert een matrix die alle tekens uit een tekenreeks en gescheiden van elkaar gehaald met de specifieke scheidingsteken. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Controleren of een tekenreeks met een specifieke subtekenreeks begint. | 
| [de subtekenreeks](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Tekens retourneren uit een tekenreeks, beginnend vanaf de opgegeven positie. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Retourneert een tekenreeks in kleine letters indeling. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Retourneert een tekenreeks in hoofdletters. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Voorloop-en volgspaties verwijderen uit een tekenreeks en retourneert de bijgewerkte tekenreeks. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Verzameling functies

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

### <a name="comparison-functions"></a>Vergelijkingsfuncties

Als u wilt werken met voorwaarden, waarden en de expressieresultaten vergelijken of verschillende soorten logische evalueren, kunt u deze van vergelijkingsfuncties. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Vergelijkingsfunctie. | Taak | 
| ------------------- | ---- | 
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

### <a name="conversion-functions"></a>Conversiefuncties

Het type of de indeling van een waarde wilt wijzigen, kunt u deze conversiefuncties. U kunt bijvoorbeeld een waarde van een Booleaanse waarde wijzigen naar een geheel getal. Zie voor meer informatie hoe logische Apps inhoudstypen verwerken tijdens de conversie, [inhoudstypen verwerken](../logic-apps/logic-apps-content-type.md). Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

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

### <a name="math-functions"></a>Wiskundige functies

Als u wilt werken met gehele getallen en landen, kunt u deze wiskundige functies. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

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

### <a name="date-and-time-functions"></a>Datum- en tijdfuncties

Als u wilt werken met datums en tijden, kunt u deze functies date en time.
Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

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

### <a name="workflow-functions"></a>Werkstroomfuncties

Deze werkstroomfuncties kunnen u helpen:

* Informatie ophalen over een werkstroomexemplaar dat tijdens de uitvoering. 
* Werken met de invoer die is gebruikt voor het instantiëren van logische apps.
* Verwijzen naar de uitvoer van triggers en acties.

U kunt bijvoorbeeld verwijzen naar de uitvoer van een actie en die gegevens gebruiken in een latere actie. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Werkstroomfunctie | Taak | 
| ----------------- | ---- | 
| [Actie](../logic-apps/workflow-definition-language-functions-reference.md#action) | Retourneert de huidige actie-uitvoer in runtime, of waarden die van andere naam en waarde-paren van JSON. Zie ook [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Resultaat van een actie `body` uitvoer tijdens runtime. Zie ook [hoofdtekst](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Retourneert de uitvoer van een actie tijdens runtime. Zie [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Retourneert de uitvoer van een actie in runtime, of waarden van andere naam en waarde-paren van JSON. Zie ook [actie](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
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

### <a name="uri-parsing-functions"></a>Functies voor URI parseren

Als u wilt werken met uniform resource-id's (URI) en waarden van verschillende eigenschappen voor deze URI's te halen, kunt u deze functies voor URI parseren. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

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

### <a name="json-and-xml-functions"></a>JSON en XML-functies

Als u wilt werken met JSON-objecten en XML-knooppunt, kunt u deze functies voor het bewerken. Zie voor de volledige naslaginformatie over elke functie, de [alfabetische naslagartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Functie voor het bewerken | Taak | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Een eigenschap en de waarde of de naam / waarde-paar, toevoegen aan een JSON-object en het bijgewerkte object retourneren. | 
| [samenvoegen](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Retourneert de eerste niet-null-waarde van een of meer parameters. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Verwijderen van een eigenschap van een JSON-object en het bijgewerkte object retourneren. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Stel de waarde voor eigenschap van een JSON-object en het bijgewerkte object geretourneerd. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Controleren of XML knooppunten of waarden die overeenkomen met een (XML Path-taal) XPath-expressie en retourneert de overeenkomende waarden of knooppunten. | 
||| 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Definitietaal van werkstroom acties en triggers](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Meer informatie over het programmatisch maken en beheren van logische apps met de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows)

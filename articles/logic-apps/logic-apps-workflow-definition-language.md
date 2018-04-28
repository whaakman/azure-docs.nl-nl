---
title: Werkstroom Definition Language schema - Azure Logic Apps | Microsoft Docs
description: Schrijven van aangepaste werkstroomdefinities voor Azure Logic Apps met de werkstroom Definition Language
services: logic-apps
author: ecfan
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan
ms.openlocfilehash: 7c253fd83bcc1f1dde93ac6ef0c26da1fa1a9a4b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="logic-apps-workflow-definitions-with-the-workflow-definition-language-schema"></a>Logic Apps werkstroomdefinities met het schema van de werkstroom Definition Language

Wanneer u een werkstroom logic app met maakt [Azure Logic Apps](../logic-apps/logic-apps-overview.md), uw onderliggende werkstroomdefinitie beschrijft de werkelijke logica die wordt uitgevoerd voor uw logische app. Deze beschrijving volgt een structuur die is gedefinieerd en gevalideerd door de werkstroom Definition Language-schema, dat gebruikmaakt van [notatie JSON (JavaScript Object)](https://www.json.org/) indeling. 
  
## <a name="workflow-definition-structure"></a>Werkstroom definitie structuur

De werkstroomdefinitie van een is ten minste één trigger die uw logische app instantieert, plus een of meer acties die uw logische app wordt uitgevoerd. 

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
| definitie | Ja | Het eerste element voor de werkstroomdefinitie van de | 
| $schema | Alleen wanneer u extern verwijst naar de werkstroomdefinitie van een | De locatie voor de JSON-schema-bestand dat beschrijft de werkstroom Definition Language-versie die u hier kunt vinden: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Nee | Het versienummer voor de werkstroomdefinitie van de, namelijk '1.0.0.0' standaard. Geef een waarde te gebruiken om te bepalen en Bevestig de juiste definitie bij het implementeren van een werkstroom. | 
| parameters | Nee | De definities voor een of meer parameters die het doorgeven van gegevens in uw werkstroom <p><p>Maximum aantal parameters: 50 | 
| Triggers | Nee | De definities voor een of meer triggers die exemplaar maken van uw werkstroom. U kunt meer dan één worden geactiveerd, maar alleen met de werkstroom Definition Language, visueel niet via de ontwerpfunctie van Logic Apps definiëren. <p><p>Maximale triggers: 10 | 
| acties | Nee | De definities voor een of meer acties uit te voeren op de workflowruntime <p><p>Maximaal aantal acties: 250 | 
| uitvoer | Nee | De definities voor de uitvoer die van een werkstroom uitvoeren retourneren <p><p>Maximale uitvoer: 10 |  
|||| 

## <a name="parameters"></a>Parameters

In de `parameters` sectie, het definiëren van de parameters die invoer voor de werkstroom tijdens runtime accepteren. Voordat u deze parameters in andere gedeelten van de werkstroom gebruiken kunt, ervoor zorgen dat u de parameters in deze secties declareren.

Hier volgt de algemene structuur voor de parameterdefinitie van een:  

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
| type | Ja | int, float, string, securestring, bool, array, JSON-object, secureobject <p><p>**Opmerking**: voor alle wachtwoorden, sleutels en geheimen, gebruikt u de `securestring` en `secureobject` typen omdat de `GET` bewerking deze typen niet retourneren. | Het type voor de parameter |
| defaultValue | Nee | Zelfde als `type` | De standaardwaarde voor parameter wanneer er geen waarde wordt opgegeven wanneer de werkstroom ServiceHost | 
| allowedValues | Nee | Zelfde als `type` | Een matrix met waarden die de parameter kan accepteren |  
| metagegevens | Nee | JSON-object | Andere parameterdetails, bijvoorbeeld de naam of een leesbare beschrijving voor uw logische app of het moment van ontwerp gegevens die worden gebruikt door Visual Studio of andere hulpprogramma 's |  
||||
  
## <a name="triggers-and-actions"></a>Triggers en acties  

In de werkstroomdefinitie van een, de `triggers` en `actions` secties de oproepen die ontstaan tijdens de uitvoering van de werkstroom definiëren. Zie voor de syntaxis en meer informatie over deze secties [werkstroom triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Uitvoer 

In de `outputs` sectie, het definiëren van de gegevens die de werkstroom kan worden geretourneerd wanneer u klaar bent met het uitgevoerd. Bijvoorbeeld, als u wilt bijhouden van een specifieke status of de waarde van elke uitvoeren, opgeven dat de uitvoer van de werkstroom die gegevens retourneert. 

Hier volgt de algemene structuur voor de definitie van een uitvoer: 

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
| <*naam van de sleutel*> | Ja | Tekenreeks | De naam van de sleutel voor de uitvoer waarde retourneren |  
| type | Ja | int, float, string, securestring, bool, array, JSON-object | Het type voor de retourwaarde van uitvoer | 
| waarde | Ja | Zelfde als `type` | De retourwaarde van uitvoer |  
||||| 

Raadpleeg de logische app uitvoeringsgeschiedenis en details in de Azure portal als u de uitvoer van een werkstroom uitvoert, of gebruik de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows). U kunt ook uitvoer doorgeven met externe systemen, bijvoorbeeld Power BI zodat u kunt dashboards maken. 

> [!NOTE]
> Bij het reageren op binnenkomende aanvragen van een service REST API, gebruik geen `outputs`. Gebruik in plaats daarvan de `Response` action-type. Zie voor meer informatie [werkstroom triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).

<a name="expressions"></a>

## <a name="expressions"></a>Expressies

Met JSON hebt u letterlijke waarden die bestaan in de ontwerpfase, bijvoorbeeld:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

U kunt ook waarden die niet tot uitvoering hebben. Als u wilt deze waarden vertegenwoordigen, kunt u *expressies*, die tijdens runtime worden geëvalueerd. Een expressie is een reeks met een of meer [functies](#functions), [operators](#operators), variabelen, expliciete waarden of constanten. In de werkstroomdefinitie van de, kunt u een expressie overal in een JSON-string-waarde door het voorvoegsel van de expressie met het apenstaartje (@). Bij het evalueren van een expressie die een JSON-waarde vertegenwoordigt de hoofdtekst van de expressie wordt opgehaald door het verwijderen van de @-teken en altijd resulteert in een andere JSON-waarde. 

Bijvoorbeeld: voor het eerder gedefinieerde `customerName` eigenschap, krijgt u de waarde van eigenschap met behulp van de [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) functioneren in een expressie en die waarde toewijzen aan de `accountName` eigenschap:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Tekenreeks interpolatie* ook kunt u meerdere expressies binnen tekenreeksen die zijn ingepakt door de @-teken en de accolades ({}). Hier volgt de syntaxis:

```json
@{ "<expression1>", "<expression2>" }
```

Het resultaat is altijd een tekenreeks, waardoor deze mogelijkheid vergelijkbaar met de `concat()` functioneren, bijvoorbeeld: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Als u een letterlijke tekenreeks die begint hebt met het voorvoegsel @-teken, het teken met een ander teken als een escape-teken @ @: @@

Deze voorbeelden laten zien hoe expressies worden geëvalueerd:

| JSON-waarde | Resultaat |
|------------|--------| 
| 'Sophia Owen' | Resultaat van deze tekens bevatten: 'Sophia Owen' |
| "array [1]" | Resultaat van deze tekens bevatten: 'array [1]' |
| "@@" | Deze tekens worden geretourneerd als een tekenreeks met één teken: ' @' |   
| " @" | Deze tekens worden geretourneerd als een tekenreeks van twee tekens: ' @' |
|||

Stel dat u 'myBirthMonth' gelijk is aan 'Januari' en 'myAge' gelijk aan het aantal 42 definiëren voor deze voorbeelden:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Deze voorbeelden laten zien hoe de volgende expressies worden geëvalueerd:

| JSON-expressie | Resultaat |
|-----------------|--------| 
| "@parameters(myBirthMonth)" | Deze tekenreeks geretourneerd: 'Januari' |  
| "@{parameters('myBirthMonth')}" | Deze tekenreeks geretourneerd: 'Januari' |  
| "@parameters(myAge)" | Dit getal retourneren: 42 |  
| "@{parameters('myAge')}" | Dit nummer als een tekenreeks retourneren: '42' |  
| "Mijn leeftijd is @{parameters('myAge')}" | Deze tekenreeks geretourneerd: 'mijn leeftijd is 42' |  
| '@concat('Mijn leeftijd is', string(parameters('myAge'))) ' | Deze tekenreeks geretourneerd: 'mijn leeftijd is 42' |  
| "Mijn leeftijd is @@ {parameters('myAge')}" | Deze tekenreeks geretourneerd, waaronder de expressie: "mijn leeftijd is @{parameters('myAge')}' | 
||| 

Wanneer u visueel in de ontwerpfunctie voor Logic Apps werkt, kunt u bijvoorbeeld expressies via de opbouwfunctie maken: 

![Logic Apps Designer > opbouwfunctie](./media/logic-apps-workflow-definition-language/expression-builder.png)

Wanneer u bent klaar, de expressie wordt weergegeven voor de overeenkomstige eigenschap in de werkstroomdefinitie van de, bijvoorbeeld, de `searchQuery` eigenschap hier:

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

In [expressies](#expressions) en [functies](#functions), operators specifieke taken uitvoeren, zoals de verwijzing naar een eigenschap of een waarde in een matrix. 

| Operator | Taak | 
|----------|------|
| ' | Als u wilt gebruiken een letterlijke tekenreeks als invoer of in expressies en functies, bijvoorbeeld de tekenreeks alleen met enkele aanhalingstekens teruglopen `'<myString>'`. Gebruik geen dubbele aanhalingstekens (""), die in strijd zijn met de JSON-opmaak rond een gehele expressie. Bijvoorbeeld: <p>**Ja**: length('Hello') </br>**Geen**: length("Hello") <p>Wanneer u matrices of getallen doorgeeft, hoeft u niet wrapping leestekens. Bijvoorbeeld: <p>**Ja**: lengte ([1, 2, 3]) </br>**Geen**: lengte ('[1, 2, 3]') | 
| [] | Om te verwijzen naar een waarde op een specifieke positie (index) in een matrix, gebruikt u vierkante haken. Als u bijvoorbeeld het tweede item in een matrix ophalen: <p>`myArray[1]` | 
| . | Om te verwijzen naar een eigenschap van een object, gebruikt u de puntoperator. Bijvoorbeeld, om op te halen de `name` eigenschap voor een `customer` JSON-object: <p>`"@parameters('customer').name"` | 
| ? | Om te verwijzen naar null eigenschappen in een object zonder een runtime-fout, gebruikt u de operator vraagteken. U kunt bijvoorbeeld deze expressie gebruiken voor het afhandelen van null uitvoer van een trigger: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Functies

Aantal expressies worden de waarden van de runtime-acties die mogelijk nog niet wanneer een logische app wordt gestart. U kunt gebruiken om te verwijzen naar of te werken met deze waarden in expressies voor *functies*. Bijvoorbeeld, kunt u wiskundige functies voor berekeningen, zoals de [add()](../logic-apps/workflow-definition-language-functions-reference.md#add) functie de som van gehele getallen of tekst retourneert. 

Hier volgen een paar voorbeelden van taken die u met functies kunt uitvoeren: 

| Taak | Syntaxis van de functie | Resultaat | 
| ---- | --------------- | -------------- | 
| Retourneert een tekenreeks in kleine letter. | toLower ('<*tekst*>') <p>Bijvoorbeeld: toLower('Hello') | 'Hallo' | 
| Retourneert een globally unique identifier (GUID). | GUID() |'c2ecc88d-88c8-4096-912c-d6f2e2b138ce' | 
|||| 

Dit voorbeeld ziet u hoe u Haal de waarde uit de `customerName` parameter en de toewijzing van waarde naar de `accountName` eigenschap met behulp van de [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) functie in een expressie:

```json
"accountName": "@parameters('customerName')"
```

Hier volgen enkele algemene manieren van dat u kunt functies gebruiken in expressies voor:

| Taak | De syntaxis van de functie in een expressie | 
| ---- | -------------------------------- | 
| Werken met een item door door te geven dat het item aan een functie uitvoeren. | "@<*functienaam*> (<*item*>)" | 
| 1. Ophalen van de *parameterName*van waarde met behulp van de geneste `parameters()` functie. </br>2. Werken met het resultaat uitvoeren door door te geven dat de waarde voor *functienaam*. | "@<*functienaam*> (parameters ('<*parameterName*>'))" | 
| 1. Ophalen van het resultaat van de geneste functie binnenste *functienaam*. </br>2. Het resultaat doorgeven aan de buitenste functie *functionName2*. | "@<*functionName2*> (<*functienaam*> (<*item*>))" | 
| 1. Het resultaat wordt verkregen uit *functienaam*. </br>2. Gezien het feit dat het resultaat een object met de eigenschap is *propertyName*, waarde van de eigenschap ophalen. | "@<*functienaam*>(<*item*>). <*propertyName*>" | 
||| 

Bijvoorbeeld, de `concat()` functie twee of meer string-waarden als parameters kunt nemen. Deze functie combineert deze tekenreeksen in één tekenreeks. U kunt doorgeven in de letterlijke tekenreeks, bijvoorbeeld 'Sophia' en 'Owen' zodat u een gecombineerde tekenreeks, 'SophiaOwen' ophalen:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Of u kunt de tekenreekswaarden ophalen van parameters. In dit voorbeeld wordt de `parameters()` functie in elk `concat()` parameter en de `firstName` en `lastName` parameters. U geeft u de resulterende tekenreeksen voor de `concat()` werken, zodat u een gecombineerde tekenreeks op, bijvoorbeeld 'SophiaOwen' ophalen:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

In beide gevallen beide voorbeelden toewijzen het resultaat dat de `customerName` eigenschap. 

Zie voor gedetailleerde informatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).
Of gaan leren over functies die op basis van hun algemene doeleinden.

<a name="string-functions"></a>

### <a name="string-functions"></a>Tekenreeks-functies

Om te werken met tekenreeksen, kunt u deze tekenreeksfuncties en ook enkele [verzameling functies](#collection-functions). Tekenreeks-functies werken alleen op tekenreeksen. 

| Tekenreeksfunctie | Taak | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Twee of meer reeksen combineren en de gecombineerde tekenreeks retourneren. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Controleren of een tekenreeks op de opgegeven subtekenreeks eindigt. | 
| [GUID](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Een globally unique identifier (GUID) genereren als een tekenreeks. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Retourneert de beginpositie voor een subtekenreeks. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | De eindpositie in voor een subtekenreeks retourneren. | 
| [vervangen](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Een subtekenreeks vervangen door de opgegeven tekenreeks en de bijgewerkte tekenreeks retourneren. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Retourneert een matrix die alle tekens uit een tekenreeks bevat en elk teken gescheiden met het specifieke scheidingsteken. | 
| [StartsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Controleren of een tekenreeks met een specifieke subtekenreeks begint. | 
| [de subtekenreeks](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Tekens retourneren van een tekenreeks, te beginnen vanaf de opgegeven positie. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Retourneert een tekenreeks in kleine letter. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Retourneert een tekenreeks in hoofdletters. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Voorloop-en volgspaties verwijderen uit een tekenreeks en de bijgewerkte tekenreeks retourneren. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Functies van de verzameling

Om te werken met verzamelingen, in het algemeen matrices tekenreeksen en soms woordenlijsten, kunt u de functies van deze verzameling. 

| Functie van de verzameling | Taak | 
| ------------------- | ---- | 
| [Bevat](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Controleer of een verzameling een specifiek item heeft. |
| [leeg](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Controleren of een verzameling leeg is. | 
| [eerste](../logic-apps/workflow-definition-language-functions-reference.md#first) | Retourneert het eerste item uit een verzameling. | 
| [snijpunt](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Retourneert een verzameling met *alleen* de algemene items in de opgegeven verzamelingen. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | Een tekenreeks retourneert die heeft *alle* de items uit een array, gescheiden door het opgegeven teken. | 
| [laatste](../logic-apps/workflow-definition-language-functions-reference.md#last) | Retourneert het laatste item uit een verzameling. | 
| [lengte](../logic-apps/workflow-definition-language-functions-reference.md#length) | Retourneert het aantal items in een tekenreeks of matrix. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Items verwijderen uit het begin van een verzameling en terug te keren *alle andere* items. | 
| [duren](../logic-apps/workflow-definition-language-functions-reference.md#take) | Items retourneren vanaf het begin van een verzameling. | 
| [Union](../logic-apps/workflow-definition-language-functions-reference.md#union) | Retourneert een verzameling met *alle* de items van de gespecificeerde verzamelingen. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Vergelijking van functies

Als u wilt werken met voorwaarden, waarden en expressieresultaten te vergelijken of verschillende soorten logica evalueren, kunt u deze vergelijking van functies. Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Vergelijkingsfunctie. | Taak | 
| ------------------- | ---- | 
| [En](../logic-apps/workflow-definition-language-functions-reference.md#and) | Controleer of alle expressies voldaan wordt. | 
| [is gelijk aan](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Controleer of beide waarden gelijk zijn. | 
| [groter](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Controleer of de eerste waarde groter dan de tweede waarde. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Controleer of de eerste waarde groter dan of gelijk zijn aan de tweede waarde. | 
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) | Controleer of een expressie waar of ONWAAR. Op basis van het resultaat, een opgegeven waarde retourneren. | 
| [minder](../logic-apps/workflow-definition-language-functions-reference.md#less) | Controleer of de eerste waarde kleiner is dan de tweede waarde. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Controleer of de eerste waarde kleiner dan of gelijk zijn aan de tweede waarde. | 
| [niet](../logic-apps/workflow-definition-language-functions-reference.md#not) | Controleer of een expressie ingesteld op false is. | 
| [or](../logic-apps/workflow-definition-language-functions-reference.md#or) | Controleer of er ten minste één expressie waar is. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Van conversiefuncties

Het type of de indeling van een waarde wilt wijzigen, kunt u deze conversiefuncties. U kunt bijvoorbeeld een waarde van een Booleaanse waarde wijzigen in een geheel getal. Zie voor meer informatie over hoe Logic Apps typen inhoud verwerkt tijdens de conversie, [typen inhoud verwerken](../logic-apps/logic-apps-content-type.md). Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Functie voor het converteren | Taak | 
| ------------------- | ---- | 
| [matrix](../logic-apps/workflow-definition-language-functions-reference.md#array) | Retourneert een matrix van één opgegeven invoer. Zie voor meerdere invoer [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | De versie base64-gecodeerd om een tekenreeks retourneren. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | De binaire versie voor een base64-gecodeerde tekenreeks retourneren. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | De tekenreeksversie voor een base64-gecodeerde tekenreeks retourneren. | 
| [Binaire](../logic-apps/workflow-definition-language-functions-reference.md#binary) | De binaire versie voor een invoerwaarde retourneren. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | De Booleaanse versie voor een invoerwaarde retourneren. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Retourneert een matrix uit meerdere invoer. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | De gegevens-URI voor een invoerwaarde retourneren. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | De binaire versie voor een gegevens-URI geretourneerd. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | De tekenreeksversie voor een gegevens-URI geretourneerd. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | De tekenreeksversie voor een base64-gecodeerde tekenreeks retourneren. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | De binaire versie voor een gegevens-URI geretourneerd. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Een tekenreeks of vervangt escape-tekens met versie gedecodeerde retourneren. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Retourneert een tekenreeks die URL onveilige tekens vervangen door escape-tekens. | 
| [Float](../logic-apps/workflow-definition-language-functions-reference.md#float) | Retourneert een drijvende komma voor een invoerwaarde. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | De versie van het geheel getal voor een tekenreeks retourneren. | 
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | De waarde van type Notation JSON (JavaScript Object) of het object voor een tekenreeks of het XML-retourneren. | 
| [Tekenreeks](../logic-apps/workflow-definition-language-functions-reference.md#string) | De tekenreeksversie voor een invoerwaarde retourneren. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | De versie-URI-codering voor een invoerwaarde URL onveilige tekens te vervangen door een escape-tekens retourneren. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | De binaire versie voor een URI-gecodeerde tekenreeks retourneren. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | De tekenreeksversie voor een URI-gecodeerde tekenreeks retourneren. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | De XML-versie voor een tekenreeks retourneren. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Rekenkundige functies

Om te werken met gehele getallen en tekst, kunt u deze functies math. Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Math functie | Taak | 
| ------------- | ---- | 
| [add](../logic-apps/workflow-definition-language-functions-reference.md#add) | Retourneert het resultaat van het toevoegen van twee getallen. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Retourneert het resultaat van de deling van twee getallen. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Retourneert de hoogste waarde uit een reeks cijfers of een matrix. | 
| [min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Retourneert de laagste waarde uit een reeks cijfers of een matrix. | 
| [Mod](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Retourneert de rest van de deling van twee getallen. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Het product van twee getallen vermenigvuldigen geretourneerd. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Retourneert een willekeurig geheel uit een opgegeven adresbereik. | 
| [bereik](../logic-apps/workflow-definition-language-functions-reference.md#range) | Retourneert een matrix die wordt gestart van een geheel getal zijn opgegeven. | 
| [sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Geeft het resultaat van het tweede nummer van het eerste nummer af te trekken. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Datum- en tijdfuncties

Met datums en tijden wilt werken, kunt u deze datum- en tijdfuncties.
Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Datum of tijd functie | Taak | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Een aantal dagen aan een tijdstempel toevoegen. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Een aantal uren toevoegen aan een tijdstempel. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Een aantal minuten toevoegen aan een tijdstempel. | 
| [addSeconds](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Een aantal seconden aan een tijdstempel toevoegen. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Een aantal tijdseenheden toevoegen aan een tijdstempel. Zie ook [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Een tijdstempel van Coordinated van Universal Time (UTC) naar de doel-tijdzone converteren. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Een tijdstempel van de tijdzone van de bron converteren naar de doel-tijdzone. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Een tijdstempel van de tijdzone van de bron voor Coordinated van Universal Time (UTC) converteren. | 
| [DayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | De dag van het onderdeel van de maand retourneren van een tijdstempel. | 
| [DayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | De dag van het onderdeel van de week van een tijdstempel retourneren. | 
| [DayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | De dag van het onderdeel van het jaar retourneren van een tijdstempel. | 
| [formatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | De datum van een tijdstempel geretourneerd. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Retourneert de huidige tijdstempel plus de opgegeven periode-eenheden. Zie ook [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Retourneert de huidige tijdstempel minus de opgegeven periode-eenheden. Zie ook [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Het begin van de dag van een tijdstempel retourneren. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Het begin van het uur voor een tijdstempel retourneren. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Het begin van de maand voor een tijdstempel retourneren. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Een aantal tijdseenheden van een tijdstempel afgetrokken. Zie ook [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [ticks](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Retourneert de `ticks` eigenschapwaarde voor een tijdstempel voor een opgegeven. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Huidig timestamp worden geretourneerd als een tekenreeks. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>Werkstroom-functies

Deze werkstroom-functies kunnen u helpen:

* Informatie ophalen over een workflowexemplaar tijdens runtime. 
* Werken met de invoer gebruikt voor het instantiëren van logische apps.
* Verwijzen naar de uitvoer van triggers en acties.

U kunt bijvoorbeeld verwijzen naar de uitvoer van een actie en die gegevens gebruiken in een latere actie. Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Werkstroomfunctie | Taak | 
| ----------------- | ---- | 
| [Actie](../logic-apps/workflow-definition-language-functions-reference.md#action) | Retourneert de huidige actie uitvoer in runtime of waarden van een andere naam en waarde-paren van JSON. Zie ook [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Retourneren van een actie `body` uitvoer tijdens runtime. Zie ook [hoofdtekst](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Retourneert een actie uitvoer tijdens runtime. Zie [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [acties](../logic-apps/workflow-definition-language-functions-reference.md#actions) | De uitvoer van de actie in runtime of waarden van andere JSON-naam en waarde-paren worden geretourneerd. Zie ook [actie](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [Hoofdtekst](#body) | Retourneren van een actie `body` uitvoer tijdens runtime. Zie ook [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Maken van een matrix met de waarden die overeenkomen met de naam van een sleutel in *formuliergegevens* of *formulier gecodeerd* actie uitvoer. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Retourneert een waarde die overeenkomt met de naam van een sleutel in een actie *formuliergegevens* of *formulier gecodeerde uitvoer*. | 
| [item](../logic-apps/workflow-definition-language-functions-reference.md#item) | Wanneer u binnen een terugkerende actie via een matrix door het huidige item in de matrix tijdens de huidige herhaling van de actie te retourneren. | 
| [Items](../logic-apps/workflow-definition-language-functions-reference.md#items) | Wanneer u binnen een voor elke of komen tot lus retourneren het huidige item van de lus wordt opgegeven.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | De 'callback URL geretourneerd ' die een actie of -trigger aanroept. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | De instantie voor een bepaald deel in de uitvoer van een actie die meerdere onderdelen heeft retourneren. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Retourneert de waarde voor een parameter die wordt beschreven in de definitie van de logische app. | 
| [Trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Retourneert een trigger uitvoer tijdens runtime, of van andere JSON-naam en waarde-paren. Zie ook [triggerOutputs](#triggerOutputs) en [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Retourneren van een trigger `body` uitvoer tijdens runtime. Zie [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Die overeenkomt met de naam van een sleutel in één waarde retourneren *formuliergegevens* of *formulier gecodeerd* uitvoer activeren. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | De instantie voor een bepaald deel van een trigger meerdelige uitvoer retourneren. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Maken van een matrix waarvan de waarden overeenkomen met de naam van een sleutel in *formuliergegevens* of *formulier gecodeerd* uitvoer activeren. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | De uitvoer van een trigger in runtime of waarden van andere JSON-naam en waarde-paren worden geretourneerd. Zie [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [variabelen](../logic-apps/workflow-definition-language-functions-reference.md#variables) | De waarde voor een opgegeven variabele retourneren. | 
| [Werkstroom](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Retourneert de details over de werkstroom zichzelf tijdens runtime. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>URI bij het parseren van functies

Als u wilt werken met uniform resource-id's (URI) en verschillende eigenschapswaarden ophalen voor deze URI's, kunt u deze URI bij het parseren van functies. Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Bij het parseren van de functie URI | Taak | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Retourneert de `host` waarde voor een uniform resource identifier (URI). | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Retourneert de `path` waarde voor een uniform resource identifier (URI). | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Retourneert de `path` en `query` waarden voor een uniform resource identifier (URI). | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Retourneert de `port` waarde voor een uniform resource identifier (URI). | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Retourneert de `query` waarde voor een uniform resource identifier (URI). | 
| [UriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Retourneert de `scheme` waarde voor een uniform resource identifier (URI). | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>JSON- en XML-functies

Om te werken met JSON-objecten en XML-knooppunten, kunt u deze functies voor het bewerken. Zie voor de volledige naslaginformatie over elke functie de [alfabetische verwijzingsartikel](../logic-apps/workflow-definition-language-functions-reference.md).

| Functie voor het bewerken | Taak | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Een eigenschap en de waarde of de naam / waarde-paar toevoegen aan een JSON-object en het bijgewerkte object retourneren. | 
| [Coalesce](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Retourneert de eerste niet-null-waarde uit een of meer parameters. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Verwijderen van een eigenschap van een JSON-object en het bijgewerkte object retourneren. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Stel de waarde voor eigenschap van een JSON-object en het bijgewerkte object retourneren. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Controleer de XML-gegevens voor knooppunten of waarden die overeenkomen met een expressie XPath (XML Path Language) en de overeenkomende knooppunten of waarden retourneren. | 
||| 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [werkstroom Definition Language acties en wordt geactiveerd](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Meer informatie over programmatisch maken en beheren van logic apps met de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows)

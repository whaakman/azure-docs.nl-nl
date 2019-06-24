---
title: Definitietaal van werkstroom - Azure Logic Apps-schemaverwijzing
description: Naslaggids voor Definitietaal van werkstroomschema in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: reference
ms.date: 05/13/2019
ms.openlocfilehash: 3b0ad33ea6348f24079b3c88f972437244c0bc93
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596758"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Definitietaal van werkstroom in Azure Logic Apps-schemaverwijzing

Wanneer u een logische app in maakt [Azure Logic Apps](../logic-apps/logic-apps-overview.md), uw logische app heeft een onderliggende werkstroomdefinitie die worden beschreven van de werkelijke logica die in uw logische app wordt uitgevoerd. Deze werkstroomdefinitie maakt gebruik van [JSON](https://www.json.org/) en een structuur die gevalideerd door het schema Definitietaal van werkstroom volgt. Deze referentie bevat een overzicht van deze structuur en hoe kenmerken in het schema wordt gedefinieerd in de werkstroomdefinitie van de.

## <a name="workflow-definition-structure"></a>Structuur van de werkstroom-definitie

Altijd een werkstroomdefinitie bevat een trigger voor het instantiëren van uw logische app, plus een of meer acties die worden uitgevoerd na de trigger wordt geactiveerd.

Hier volgt de structuur op hoog niveau voor de werkstroomdefinitie van een:

```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "actions": { "<workflow-action-definitions>" },
  "contentVersion": "<workflow-definition-version-number>",
  "outputs": { "<workflow-output-definitions>" },
  "parameters": { "<workflow-parameter-definitions>" },
  "staticResults": { "<static-results-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" }
}
```

| Kenmerk | Vereist | Description |
|-----------|----------|-------------|
| `definition` | Ja | Het eerste element voor uw werkstroomdefinitie |
| `$schema` | Alleen wanneer u extern verwijst naar een definitie van de werkstroom | De locatie voor de JSON-schema-bestand dat beschrijft de versie Definitietaal van werkstroom, die u hier kunt vinden: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json`</p> |
| `actions` | Nee | De definities voor een of meer acties uit te voeren op de werkstroom-runtime. Zie voor meer informatie, [Triggers en acties](#triggers-actions). <p><p>Maximaal aantal acties: 250 |
| `contentVersion` | Nee | Het versienummer voor de werkstroomdefinitie van uw, namelijk '1.0.0.0"standaard. Om te identificeren en controleer of de definitie van de juiste bij het implementeren van een werkstroom, Geef een waarde te gebruiken. |
| `outputs` | Nee | De definities voor de uitvoer die in een werkstroom uitvoert resulteren. Zie voor meer informatie, [uitvoer](#outputs). <p><p>Maximale uitvoer: 10 |
| `parameters` | Nee | De definities voor een of meer parameters die het doorgeven van gegevens in uw werkstroom. Zie voor meer informatie, [Parameters](#parameters). <p><p>Maximum aantal parameters: 50 |
| `staticResults` | Nee | De definities voor een of meer statische resultaten geretourneerd door acties als mock uitvoer als statische resultaten zijn ingeschakeld op deze acties. In het definitie van elke actie, de `runtimeConfiguration.staticResult.name` kenmerk verwijst naar de bijbehorende definitie binnen `staticResults`. Zie voor meer informatie, [statische resultaten](#static-results). |
| `triggers` | Nee | De definities voor een of meer triggers die exemplaar maken van uw werkstroom. U kunt meer dan één trigger, maar alleen met de Werkstroomdefinitietaal, visueel niet via de ontwerper van logische Apps definiëren. Zie voor meer informatie, [Triggers en acties](#triggers-actions). <p><p>Maximum-triggers: 10 |
||||

<a name="triggers-actions"></a>

## <a name="triggers-and-actions"></a>Triggers en acties

In het werkstroomdefinitie van een, de `triggers` en `actions` secties definiëren de aanroepen die ontstaan tijdens de uitvoering van uw werkstroom. Zie voor de syntaxis en meer informatie over deze secties, [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).

<a name="outputs"></a>

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

| Kenmerk | Vereist | Type | Description |
|-----------|----------|------|-------------|
| <*key-name*> | Ja | String | De naam van de sleutel voor de uitvoer waarde retourneren |
| <*key-type*> | Ja | int, float, string, securestring, bool, array, JSON-object | Het type voor de geretourneerde waarde van uitvoer |
| <*key-value*> | Ja | Hetzelfde als <*sleutel-type*> | De geretourneerde waarde van uitvoer |
|||||

Als u de uitvoer van een werkstroom, bekijk de uitvoeringsgeschiedenis en details in de Azure-portal van uw logische app of gebruik de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows). U kunt ook uitvoer doorgeven aan externe systemen, bijvoorbeeld Power BI, zodat u kunt dashboards maken.

<a name="parameters"></a>

## <a name="parameters"></a>Parameters

In de `parameters` sectie, het definiëren van de werkstroomparameters die gebruikmaakt van de werkstroomdefinitie van de bij de implementatie voor het accepteren van invoer. Zowel parameterdeclaraties en parameterwaarden zijn vereist tijdens de implementatie. Voordat u deze parameters in andere gedeelten van de werkstroom gebruiken kunt, ervoor zorgen dat u de parameters in deze secties declareren. 

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

| Kenmerk | Vereist | Type | Description |
|-----------|----------|------|-------------|
| <*parameter-type*> | Ja | int, float, string, securestring, bool, array, JSON-object, secureobject <p><p>**Opmerking**: Voor alle wachtwoorden, sleutels en geheimen, gebruikt u de `securestring` en `secureobject` omdat de `GET` bewerking deze typen niet retourneren. Zie voor meer informatie over het beveiligen van parameters [uw logische app beveiligen](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters) | Het type voor de parameter |
| <*standaard-parameterwaarden*> | Ja | Gelijk aan `type` | De standaardwaarde voor parameter wanneer er geen waarde is opgegeven wanneer de werkstroom waarmee een wordt gemaakt |
| <*array-with-permitted-parameter-values*> | Nee | Array | Een matrix met waarden die de parameter kunt accepteren |
| `metadata` | Nee | JSON-object | Andere details van de parameter, bijvoorbeeld de naam of een leesbare beschrijving op voor uw logische app of stroom of het moment van ontwerp-gegevens die worden gebruikt door Visual Studio of andere hulpprogramma 's |
||||

<a name="static-results"></a>

## <a name="static-results"></a>Statische resultaten

In de `staticResults` kenmerk, het definiëren van een actie nagebootste `outputs` en `status` dat de actie wordt geretourneerd wanneer de actie statische resultaat instelling is ingeschakeld. In de definitie van de actie, de `runtimeConfiguration.staticResult.name` kenmerk verwijst naar de naam van de definitie van de statische resultaat binnen `staticResults`. Leer hoe u kunt [testen van logische apps met Hiermee mock-gegevens door het instellen van statische resultaten](../logic-apps/test-logic-apps-mock-data-static-results.md).

```json
"definition": {
   "$schema": "<...>",
   "actions": { "<...>" },
   "contentVersion": "<...>",
   "outputs": { "<...>" },
   "parameters": { "<...>" },
   "staticResults": {
      "<static-result-definition-name>": {
         "outputs": {
            <output-attributes-and-values-returned>,
            "headers": { <header-values> },
            "statusCode": "<status-code-returned>"
         },
         "status": "<action-status>"
      }
   },
   "triggers": { "<...>" }
}
```

| Kenmerk | Vereist | Type | Description |
|-----------|----------|------|-------------|
| <*statische-resultaat-definition-naam*> | Ja | String | De naam van de definitie van een statische resultaat dat de actiedefinitie van een kan verwijzen naar via een `runtimeConfiguration.staticResult` object. Zie voor meer informatie, [Runtime-configuratie-instellingen](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-config-options). <p>U kunt een unieke naam die u wilt gebruiken. Standaard wordt deze unieke naam toegevoegd met een cijfer, indien nodig wordt verhoogd. |
| <*output-attributes-and-values-returned*> | Ja | Varieert | De vereisten voor deze kenmerken hangen af van verschillende voorwaarden. Bijvoorbeeld, wanneer de `status` is `Succeeded`, wordt de `outputs` kenmerk bevat kenmerken en waarden als mock uitvoer geretourneerd door de actie. Als de `status` is `Failed`, wordt de `outputs` kenmerk bevat de `errors` kenmerk een matrix met een of meer fouten is `message` objecten die informatie over de fout hebben. |
| <*header-values*> | Nee | JSON | Headerwaarden die zijn geretourneerd door de actie |
| <*status code geretourneerd*> | Ja | String | De statuscode die wordt geretourneerd door de actie |
| <*action-status*> | Ja | String | Status van de actie, bijvoorbeeld `Succeeded` of `Failed` |
|||||

Bijvoorbeeld, in de definitie van deze HTTP-actie, de `runtimeConfiguration.staticResult.name` verwijzingen van het kenmerk `HTTP0` binnen de `staticResults` kenmerk waar de mock uitvoer voor de actie worden gedefinieerd. De `runtimeConfiguration.staticResult.staticResultOptions` kenmerk geeft aan dat de instelling van de statische resultaat `Enabled` op de HTTP-actie.

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.microsoft.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "staticResult": {
            "name": "HTTP0",
            "staticResultOptions": "Enabled"
         }
      },
      "type": "Http"
   }
},
```

De HTTP-actie retourneert de uitvoer in de `HTTP0` definitie binnen `staticResults`. In dit voorbeeld, voor de statuscode de mock-uitvoer is `OK`. Voor headerwaarden, de mock-uitvoer is `"Content-Type": "application/JSON"`. Voor de status van de actie, de mock-uitvoer is `Succeeded`.

```json
"definition": {
   "$schema": "<...>",
   "actions": { "<...>" },
   "contentVersion": "<...>",
   "outputs": { "<...>" },
   "parameters": { "<...>" },
   "staticResults": {
      "HTTP0": {
         "outputs": {
            "headers": {
               "Content-Type": "application/JSON"
            },
            "statusCode": "OK"
         },
         "status": "Succeeded"
      }
   },
   "triggers": { "<...>" }
},
```

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
| "\@parameters('myBirthMonth')" | Deze tekenreeks geretourneerd: "Januari" |
| "\@{parameters('myBirthMonth')}" | Deze tekenreeks geretourneerd: "Januari" |
| "\@parameters('myAge')" | Retourneert dit getal: 42 |
| "\@{parameters('myAge')}" | Retourneert dit getal als een tekenreeks: "42" |
| "Mijn leeftijd is \@{parameters('myAge')}" | Deze tekenreeks geretourneerd: "Mijn leeftijd is 42" |
| "\@concat ('mijn leeftijd is', string(parameters('myAge')))" | Deze tekenreeks geretourneerd: "Mijn leeftijd is 42" |
| "Mijn leeftijd is \@ \@{parameters('myAge')}" | Deze tekenreeks, waaronder de expressie retourneren: ' Mijn leeftijd is \@{parameters('myAge')}' |
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

Sommige expressies worden de waarden van de runtime-acties die mogelijk nog niet wanneer de werkstroomdefinitie van de wordt gestart om uit te voeren. U kunt gebruiken om te verwijzen naar of te werken met deze waarden in expressies, [ *functies* ](../logic-apps/workflow-definition-language-functions-reference.md) waarmee de Definitietaal van werkstroom.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Definitietaal van werkstroom acties en triggers](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Meer informatie over het programmatisch maken en beheren van logische apps met de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows)

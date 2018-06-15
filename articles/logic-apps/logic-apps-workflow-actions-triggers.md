---
title: Werkstroom triggers en acties - Azure Logic Apps | Microsoft Docs
description: Meer informatie over triggers en acties in de werkstroomdefinities voor Azure Logic Apps
services: logic-apps
author: kevinlam1
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 5/8/2018
ms.author: klam; LADocs
ms.openlocfilehash: f44de1a316a8375618cfef2e4a98d40c2b21e019
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300144"
---
# <a name="triggers-and-actions-for-workflow-definitions-in-azure-logic-apps"></a>Triggers en acties voor werkstroomdefinities in Azure Logic Apps

In [Azure Logic Apps](../logic-apps/logic-apps-overview.md), alle logic app werkstromen beginnen met triggers gevolgd door acties. In dit artikel beschrijft de triggers en acties die u gebruiken kunt voor het bouwen van logic apps voor automatisering zakelijke werkstromen of processen in uw oplossingen voor integratie. U kunt bouwen van logic apps visueel met de ontwerpfunctie van Logic Apps of door rechtstreeks ontwerpen van de onderliggende workflowdefinitie met de [werkstroom Definition Language](../logic-apps/logic-apps-workflow-definition-language.md). U kunt de Azure-portal of Visual Studio gebruiken. Meer informatie over hoe [works voor triggers en acties prijzen](../logic-apps/logic-apps-pricing.md).

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Triggers-overzicht

Alle logische apps beginnen met een trigger die definieert de oproepen die kunnen instantiëren en een logische app werkstroom starten. Hier volgen de soorten triggers die u kunt gebruiken:

* Een *polling* trigger waarmee HTTP-eindpunt van een service wordt regelmatig gecontroleerd
* Een *push* activeert, welke aanroepen de [REST-API van Workflow](https://docs.microsoft.com/rest/api/logic/workflows)
 
Alle triggers hebben deze op het hoogste niveau elementen, hoewel sommige optioneel zijn:  
  
```json
"<triggerName>": {
   "type": "<triggerType>",
   "inputs": { "<trigger-behavior-settings>" },
   "recurrence": { 
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": "<recurrence-interval-based-on-frequency>"
   },
   "conditions": [ <array-with-required-conditions> ],
   "splitOn": "<property-used-for-creating-runs>",
   "operationOptions": "<optional-trigger-operations>"
}
```

*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*Triggernaam*> | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | De trigger typt, bijvoorbeeld: 'Http' of 'ApiConnection' | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
| recurrence | JSON-Object | De frequentie en het interval dat wordt beschreven hoe vaak de trigger wordt geactiveerd |  
| frequency | Reeks | De eenheid die wordt beschreven hoe vaak de trigger wordt geactiveerd: ' Seconde ', 'Minute', 'Uur', 'Dag', 'Week' of 'Maand' | 
| interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de trigger wordt geactiveerd op basis van de frequentie. <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uurs: 1-12.000 uur </br>-Minuut: 1-72.000 minuten </br>-Tweede: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval is ingesteld op 6 en de frequentie is "maand", is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| [Voorwaarden](#trigger-conditions) | Matrix | Een of meer voorwaarden die of bepalen de werkstroom uitvoeren | 
| [splitOn](#split-on-debatch) | Reeks | Een expressie die splitst, of *debatches*, matrixitems in meerdere workflowexemplaren voor verwerking. Deze optie is beschikbaar voor de triggers die een matrix retourneert en alleen als u werkt in de codeweergave. | 
| [operationOptions](#trigger-operation-options) | Reeks | Sommige triggers bieden extra opties waarmee u het standaardgedrag van de trigger wijzigen | 
||||| 

## <a name="trigger-types-and-details"></a>Triggertypen en details  

Elk triggertype heeft een andere interface en invoerwaarden die van de trigger gedrag definiëren. 

| Triggertype | Beschrijving | 
| ------------ | ----------- | 
| [**Terugkeerpatroon**](#recurrence-trigger) | Deze gebeurtenis wordt gestart op basis van een ingesteld schema. U kunt een toekomstige datum en tijd voor het starten van deze trigger instellen. Op basis van de frequentie, u kunt ook opgeven tijdstippen en dagen voor het uitvoeren van de werkstroom. | 
| [**Aanvraag**](#request-trigger)  | Uw logische app maakt in een aanroepbare eindpunt, ook wel bekend als een 'handmatig' trigger. Zie bijvoorbeeld [aanroepen, trigger of werkstromen met HTTP-eindpunten nesten](../logic-apps/logic-apps-http-endpoint.md). | 
| [**HTTP**](#http-trigger) | Controleert, of *polls*, een HTTP-web-eindpunt. Het HTTP-eindpunt moet voldoen aan een specifieke trigger contract met behulp van een '202' asynchrone patroon of door te retourneren van een matrix. | 
| [**ApiConnection**](#apiconnection-trigger) | Als de HTTP-trigger werkt, maar gebruikt [Microsoft beheerde API's](../connectors/apis-list.md). | 
| [**HTTPWebhook**](#httpwebhook-trigger) | Net als bij de aanvraag-trigger, maar een opgegeven URL voor het registreren en de registratie van aanroepen. |
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | Als de trigger HTTPWebhook werkt, maar gebruikt [Microsoft beheerde API's](../connectors/apis-list.md). | 
||| 

<a name="recurrence-trigger"></a>

## <a name="recurrence-trigger"></a>Terugkeerpatroon trigger  

Deze trigger wordt uitgevoerd op basis van uw opgegeven terugkeerpatroon en planning en biedt een eenvoudige manier voor het uitvoeren van regelmatig een werkstroom. 

Hier volgt de definitie van de trigger:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month",
      "interval": <recurrence-interval-based-on-frequency>,
      "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
      "timeZone": "<time-zone>",
      "schedule": {
         // Applies only when frequency is Day or Week. Separate values with commas.
         "hours": [ <one-or-more-hour-marks> ], 
         // Applies only when frequency is Day or Week. Separate values with commas.
         "minutes": [ <one-or-more-minute-marks> ], 
         // Applies only when frequency is Week. Separate values with commas.
         "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
      }
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```
*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| Terugkeerpatroon | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | De triggertype, "Recurrence" | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
| recurrence | JSON-Object | De frequentie en het interval dat wordt beschreven hoe vaak de trigger wordt geactiveerd |  
| frequency | Reeks | De eenheid die wordt beschreven hoe vaak de trigger wordt geactiveerd: ' Seconde ', 'Minute', 'Uur', 'Dag', 'Week' of 'Maand' | 
| interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de trigger wordt geactiveerd op basis van de frequentie. <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uurs: 1-12.000 uur </br>-Minuut: 1-72.000 minuten </br>-Tweede: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval is ingesteld op 6 en de frequentie is "maand", is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| startTime | Reeks | De begindatum en -tijd in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone opgeven <p>-of- <p>JJJJ-MM-ssZ als u een tijdzone geen opgeeft <p>Dus bijvoorbeeld, als u wilt dat 18 September 2017 op 14:00 uur, geeft u "2017-09-18T14:00:00 ' en geef een tijdzone zoals 'Pacific (standaardtijd)', of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** de begintijd moet volgen de [ISO 8601 datum, tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u een tijdzone niet opgeeft, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze 'Z"verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor complexe schema's van de trigger vroeger dan de begintijd wordt niet gestart. Zie voor meer informatie over start datums en tijden [maken en plannen die regelmatig actieve taken](../connectors/connectors-native-recurrence.md). | 
| timeZone | Reeks | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Geef de tijdzone die u wilt toepassen. | 
| hours | Geheel getal of een matrix van geheel getal | Als u 'Day' of 'Week' opgeeft voor `frequency`, kunt u een of meer gehele getallen tussen 0 en 23, gescheiden door komma's als de uren van de dag waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, als u '10', '12' en '14' opgeeft, krijgt u 10 uur, 12 uur en 14 uur als de markeringen uur. | 
| minutes | Geheel getal of een matrix van geheel getal | Als u 'Dag' of 'Week' opgeeft voor `frequency`, kunt u een of meer gehele getallen van 0 tot 59, gescheiden door komma's als de minuten van het uur waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, kunt u '30' als de minuut is ingeschakeld en met het vorige voorbeeld voor het uur van de dag, krijgt u 10:30 AM, 12:30 PM en 2:30 PM. | 
| weekDays | String of string array | Als u 'Week' opgeeft voor `frequency`, kunt u een of meer dagen, gescheiden door komma's, als u wilt uitvoeren van de werkstroom: "Maandag", "Dinsdag", "Woensdag", "Donderdag", "Vrijdag", "Zaterdag" en "Zondag" | 
| Gelijktijdigheid van taken | JSON-Object | Dit object geeft voor terugkerende en pollen triggers, het maximum aantal workflowexemplaren die kunnen worden uitgevoerd op hetzelfde moment. Gebruik deze waarde om te beperken van de aanvragen die back-endsystemen ontvangen. <p>Deze waarde wordt bijvoorbeeld de limiet voor gelijktijdige ingesteld op 10 exemplaren: `"concurrency": { "runs": 10 }` | 
| operationOptions | Reeks | De `singleInstance` optie geeft aan dat de trigger wordt geactiveerd nadat alle actieve sessies zijn voltooid. Zie [Triggers: gestart nadat er actief wordt uitgevoerd voltooien](#single-instance). | 
|||| 

*Voorbeeld 1*

Deze trigger basis terugkeerpatroon dagelijks wordt uitgevoerd:

```json
"recurrenceTriggerName": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

*Voorbeeld 2*

U kunt een begindatum en -tijd voor het starten van de trigger opgeven. Deze trigger terugkeerpatroon begint op de opgegeven datum en vervolgens wordt dagelijks geactiveerd:

```json
"recurrenceTriggerName": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1,
      "startTime": "2017-09-18T00:00:00Z"
   }
}
```

*Voorbeeld 3*

Deze trigger terugkeerpatroon begint op 9 September 2017 op 14:00 uur en wordt geactiveerd Wekelijks elke maandag op 10:30 uur, 12:30 PM en 2:30 uur Pacific (standaardtijd):

``` json
"myRecurrenceTrigger": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "hours": [ 10, 12, 14 ],
         "minutes": [ 30 ],
         "weekDays": [ "Monday" ]
      },
      "startTime": "2017-09-07T14:00:00",
      "timeZone": "Pacific Standard Time"
   }
}
```

Zie voor meer informatie plus voorbeelden voor deze trigger [maken en plannen die regelmatig actieve taken](../connectors/connectors-native-recurrence.md).

<a name="request-trigger"></a>

## <a name="request-trigger"></a>Aanvraag trigger

Deze trigger kunt u uw logische app aanroepbare door het maken van een eindpunt dat inkomende HTTP-aanvragen kan accepteren. Deze trigger aanroepen, moet u de `listCallbackUrl` API in de [REST-API van werkstroom](https://docs.microsoft.com/rest/api/logic/workflows). Zie voor meer informatie over het gebruik van deze trigger als een HTTP-eindpunt, [aanroepen, trigger of werkstromen met HTTP-eindpunten nesten](../logic-apps/logic-apps-http-endpoint.md).

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "GET | POST | PUT | PATCH | DELETE | HEAD",
      "relativePath": "<relative-path-for-accepted-parameter>",
      "schema": {
         "type": "object",
         "properties": { 
            "<propertyName>": {
               "type": "<property-type>"
            }
         },
         "required": [ "<required-properties>" ]
      }
   }
}
```

*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| Handmatig | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | Het triggertype is 'Vragen' | 
| type | Reeks | Het type van de aanvraag, dit is van 'Http' | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| method | Reeks | De methode die aanvragen gebruiken moeten om aan te roepen de trigger: 'Ophalen', 'Plaatsen', 'Posten', 'PATCH', 'Verwijderen' of 'HEAD' |
| RelativePath | Reeks | Het relatieve pad voor de parameter die uw HTTP-eindpunt-URL accepteert | 
| Schema | JSON-Object | De JSON-schema die beschrijft en valideert de nettolading of invoer, die de trigger ontvangt van de binnenkomende aanvraag. Dit schema helpt daaropvolgende werkstroomacties weten welke eigenschappen u moet verwijzen naar. | 
| properties | JSON-Object | Een of meer eigenschappen in de JSON-schema dat de nettolading beschrijft | 
| vereist | Matrix | Een of meer eigenschappen waarvoor waarden | 
|||| 

*Voorbeeld*

Deze aanvraag trigger geeft aan dat een inkomende aanvraag HTTP POST-methode aan te roepen de trigger en een schema te valideren en invoer van de binnenkomende aanvraag: 

```json
"myRequestTrigger": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "POST",
      "schema": {
         "type": "Object",
         "properties": {
            "customerName": {
               "type": "String"
            },
            "customerAddress": { 
               "type": "Object",
               "properties": {
                  "streetAddress": {
                     "type": "String"
                  },
                  "city": {
                     "type": "String"
                  }
               }
            }
         }
      }
   }
} 
```

<a name="http-trigger"></a>

## <a name="http-trigger"></a>HTTP-trigger  

Deze trigger een opgegeven eindpunt worden opgevraagd en het antwoord gecontroleerd. Het antwoord bepaalt of de werkstroom moet worden uitgevoerd of niet. De `inputs` JSON-object bevat en vereist dat de `method` en `uri` parameters die zijn vereist voor het maken van de HTTP-aanroep:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET | PUT | POST | PATCH | DELETE | HEAD",
      "uri": "<HTTP-or-HTTPS-endpoint-to-poll>",
      "queries": "<query-parameters>",
      "headers": { "<headers-for-request>" },
      "body": { "<payload-to-send>" },
      "authentication": { "<authentication-method>" },
      "retryPolicy": {
          "type": "<retry-policy-type>",
          "interval": "<retry-interval>",
          "count": <number-retry-attempts>
      }
   },
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": <recurrence-interval-based-on-frequency>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```

*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| HTTP | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | Het triggertype is 'Http' | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
| method | Ja | Reeks | De HTTP-methode voor het opgegeven eindpunt polling: 'Ophalen', 'Plaatsen', 'Posten', 'PATCH', 'Verwijderen' of 'HEAD' | 
| uri | Ja| Reeks | De HTTP of HTTPS-eindpunt-URL die de trigger wordt gecontroleerd of worden opgevraagd <p>Maximale grootte van tekenreeks: 2 KB | 
| recurrence | JSON-Object | De frequentie en het interval dat wordt beschreven hoe vaak de trigger wordt geactiveerd |  
| frequency | Reeks | De eenheid die wordt beschreven hoe vaak de trigger wordt geactiveerd: ' Seconde ', 'Minute', 'Uur', 'Dag', 'Week' of 'Maand' | 
| interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de trigger wordt geactiveerd op basis van de frequentie. <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uurs: 1-12.000 uur </br>-Minuut: 1-72.000 minuten </br>-Tweede: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval is ingesteld op 6 en de frequentie is "maand", is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| query's | JSON-Object | Queryparameters die u wilt opnemen in de URL <p>Dit element wordt bijvoorbeeld toegevoegd de `?api-version=2015-02-01` querytekenreeks naar de URL: <p>`"queries": { "api-version": "2015-02-01" }` <p>Resultaat: `https://contoso.com?api-version=2015-02-01` | 
| headers | JSON-Object | Een of meer heaers om met de aanvraag te verzenden <p>Als u bijvoorbeeld de taal en het type voor een aanvraag voor instellen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | JSON-Object | De nettolading (gegevens) om te verzenden naar het eindpunt | 
| verificatie | JSON-Object | De methode die voor de binnenkomende aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). Afgezien van Scheduler, de `authority` eigenschap wordt ondersteund. Als niet wordt opgegeven, is de standaardwaarde `https://login.windows.net`, maar u kunt een andere waarde, zoals`https://login.windows\-ppe.net`. | 
| retryPolicy | JSON-Object | Dit object wordt het gedrag voor opnieuw proberen voor onregelmatige fouten die 4xx of 5xx statuscodes hebt aangepast. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| Gelijktijdigheid van taken | JSON-Object | Dit object geeft voor terugkerende en pollen triggers, het maximum aantal workflowexemplaren die kunnen worden uitgevoerd op hetzelfde moment. Gebruik deze waarde om te beperken van de aanvragen die back-endsystemen ontvangen. <p>Deze waarde wordt bijvoorbeeld de limiet voor gelijktijdige ingesteld op 10 exemplaren: <p>`"concurrency": { "runs": 10 }` | 
| operationOptions | Reeks | De `singleInstance` optie geeft aan dat de trigger wordt geactiveerd nadat alle actieve sessies zijn voltooid. Zie [Triggers: gestart nadat er actief wordt uitgevoerd voltooien](#single-instance). | 
|||| 

Om te werken goed met uw logische app, is de HTTP-trigger vereist dat de HTTP-API in overeenstemming zijn met een specifieke patroon. De HTTP-trigger herkent deze eigenschappen:  
  
| Antwoord | Vereist | Beschrijving | 
| -------- | -------- | ----------- |  
| Statuscode | Ja | De ' 200 OK ' statuscode een uitvoering wordt gestart. Andere statuscode niet een uitvoering niet starten. | 
| Probeer het opnieuw na-header | Nee | Het aantal seconden totdat de logische app, controleert of het eindpunt opnieuw | 
| Locatieheader | Nee | De URL om aan te roepen op de volgende pollinginterval. Als niet wordt opgegeven, wordt de oorspronkelijke URL gebruikt. | 
|||| 

*Voorbeeld-gedrag voor verschillende aanvragen*

| Statuscode | Opnieuw proberen na | Gedrag | 
| ----------- | ----------- | -------- | 
| 200 | {geen} | Het uitvoeren van de werkstroom vervolgens opnieuw controleren voor meer gegevens na de gedefinieerde terugkeerpatroon. | 
| 200 | 10 seconden | Het uitvoeren van de werkstroom vervolgens opnieuw controleren voor meer gegevens na 10 seconden. |  
| 202 | 60 seconden | De werkstroom niet wordt geactiveerd. De volgende poging gebeurt in één minuut, onderworpen aan de gedefinieerde terugkeerpatroon. Als de gedefinieerde terugkeer minder dan één minuut is, wordt de koptekst opnieuw proberen na voorrang. Anders wordt wordt de gedefinieerde herhaling gebruikt. | 
| 400 | {geen} | Onjuiste aanvraag, niet de werkstroom wordt uitgevoerd. Als er geen `retryPolicy` is gedefinieerd, wordt het standaardbeleid gebruikt. Nadat u het aantal nieuwe pogingen is bereikt, controleert de trigger opnieuw gegevens na de gedefinieerde terugkeerpatroon. | 
| 500 | {geen}| Server-fout, de werkstroom niet uitvoeren. Als er geen `retryPolicy` is gedefinieerd, wordt het standaardbeleid gebruikt. Nadat u het aantal nieuwe pogingen is bereikt, controleert de trigger opnieuw gegevens na de gedefinieerde terugkeerpatroon. | 
|||| 

### <a name="http-trigger-outputs"></a>HTTP-trigger-uitvoer

| Elementnaam | Type | Beschrijving |
| ------------ | ---- | ----------- |
| headers | JSON-Object | De headers van het HTTP-antwoord | 
| hoofdtekst | JSON-Object | De hoofdtekst van het HTTP-antwoord | 
|||| 

<a name="apiconnection-trigger"></a>

## <a name="apiconnection-trigger"></a>APIConnection trigger  

Deze trigger werkt als de [HTTP-trigger](#http-trigger), maar gebruikt [Microsoft beheerde API's](../connectors/apis-list.md) dus de parameters voor deze trigger verschillend zijn. 

Hier volgt de definitie van de trigger, hoewel veel secties optioneel, zijn zodat de werking van de trigger is afhankelijk van of secties opgenomen zijn:

```json
"<APIConnectionTriggerName>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "api": {
            "runtimeUrl": "<managed-API-endpoint-URL>"
         },
         "connection": {
            "name": "@parameters('$connections')['<connection-name>'].name"
         },
      },
      "method": "GET | PUT | POST | PATCH | DELETE | HEAD",
      "queries": "<query-parameters>",
      "headers": { "<headers-for-request>" },
      "body": { "<payload-to-send>" },
      "authentication": { "<authentication-method>" },
      "retryPolicy": {
          "type": "<retry-policy-type>",
          "interval": "<retry-interval>",
          "count": <number-retry-attempts>
      }
   },
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": "<recurrence-interval-based-on-frequency>"
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```

*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| *APIConnectionTriggerName* | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | De triggertype, 'ApiConnection' | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
| host | JSON-Object | De JSON-object dat de host-gateway en de ID voor de beheerde API beschrijft <p>De `host` JSON-object heeft deze elementen: `api` en `connection` | 
| api | JSON-Object | De eindpunt-URL voor de beheerde API: <p>`"runtimeUrl": "<managed-API-endpoint-URL>"` | 
| verbinding | JSON-Object | De naam voor de beheerde API verbinding die gebruikmaakt van de werkstroom, waaronder een verwijzing naar een parameter met de naam moet `$connection`: <p>`"name": "@parameters('$connections')['<connection-name>'].name"` | 
| method | Reeks | De HTTP-methode voor het communiceren met de beheerde API: 'Ophalen', 'Plaatsen', 'Posten', 'PATCH', 'Verwijderen' of 'HEAD' | 
| recurrence | JSON-Object | De frequentie en het interval dat wordt beschreven hoe vaak de trigger wordt geactiveerd |  
| frequency | Reeks | De eenheid die wordt beschreven hoe vaak de trigger wordt geactiveerd: ' Seconde ', 'Minute', 'Uur', 'Dag', 'Week' of 'Maand' | 
| interval | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de trigger wordt geactiveerd op basis van de frequentie. <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uurs: 1-12.000 uur </br>-Minuut: 1-72.000 minuten </br>-Tweede: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval is ingesteld op 6 en de frequentie is "maand", is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| query's | JSON-Object | Queryparameters die u wilt opnemen in de URL <p>Dit element wordt bijvoorbeeld toegevoegd de `?api-version=2015-02-01` querytekenreeks naar de URL: <p>`"queries": { "api-version": "2015-02-01" }` <p>Resultaat: `https://contoso.com?api-version=2015-02-01` | 
| headers | JSON-Object | Een of meer heaers om met de aanvraag te verzenden <p>Als u bijvoorbeeld de taal en het type voor een aanvraag voor instellen: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | JSON-Object | De JSON-object die worden beschreven de nettolading (gegevens) om te verzenden naar de beheerde API | 
| verificatie | JSON-Object | De methode die een binnenkomende aanvraag moet worden gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
| retryPolicy | JSON-Object | Dit object wordt het gedrag voor opnieuw proberen voor onregelmatige fouten die 4xx of 5xx statuscodes hebt aangepast: <p>`"retryPolicy": { "type": "<retry-policy-type>", "interval": "<retry-interval>", "count": <number-retry-attempts> }` <p>Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| Gelijktijdigheid van taken | JSON-Object | Dit object geeft voor terugkerende en pollen triggers, het maximum aantal workflowexemplaren die kunnen worden uitgevoerd op hetzelfde moment. Gebruik deze waarde om te beperken van de aanvragen die back-endsystemen ontvangen. <p>Deze waarde wordt bijvoorbeeld de limiet voor gelijktijdige ingesteld op 10 exemplaren: `"concurrency": { "runs": 10 }` | 
| operationOptions | Reeks | De `singleInstance` optie geeft aan dat de trigger wordt geactiveerd nadat alle actieve sessies zijn voltooid. Zie [Triggers: gestart nadat er actief wordt uitgevoerd voltooien](#single-instance). | 
||||

*Voorbeeld*

```json
"Create_daily_report": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "api": {
            "runtimeUrl": "https://myReportsRepo.example.com/"
         },
         "connection": {
            "name": "@parameters('$connections')['<connection-name>'].name"
         }     
      },
      "method": "POST",
      "body": {
         "category": "statusReports"
      }  
   },
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

### <a name="apiconnection-trigger-outputs"></a>APIConnection trigger-uitvoer
 
| Elementnaam | Type | Beschrijving |
| ------------ | ---- | ----------- |
| headers | JSON-Object | De headers van het HTTP-antwoord | 
| hoofdtekst | JSON-Object | De hoofdtekst van het HTTP-antwoord | 
|||| 

<a name="httpwebhook-trigger"></a>

## <a name="httpwebhook-trigger"></a>HTTPWebhook trigger  

Deze trigger werkt als de [aanvraag trigger](#request-trigger) door het maken van een aanroepbare eindpunt voor uw logische app. Deze trigger roept echter ook een opgegeven eindpunt-URL voor het registreren of registratie van een abonnement. U kunt limieten opgeven van een webhook-trigger op dezelfde manier als [HTTP asynchrone limieten](#asynchronous-limits). 

Hier wordt de definitie van de trigger, hoewel veel secties optioneel zijn en van de trigger werking is afhankelijk van de secties die u gebruikt of weglaten:

```json
"HTTP_Webhook": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "<subscribe-to-endpoint-URL>",
            "headers": { "<headers-for-request>" },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "<subscription-topic>"
            },
            "authentication": {},
            "retryPolicy": {}
        },
        "unsubscribe": {
            "method": "POST",
            "url": "<unsubscribe-from-endpoint-URL>",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "<subscription-topic>"
            },
            "authentication": {}
        }
    },
}
```

*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| HTTP_Webhook | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | De triggertype, 'HttpWebhook' | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
| abonneren | JSON-Object| De uitgaande aanvraag aanroepen en het uitvoeren van de initiële registratie als de trigger wordt gemaakt. Deze aanroep gebeurt zodat de trigger kunt beginnen met luisteren op gebeurtenissen op het eindpunt. Zie voor meer informatie [abonneren en afmelden](#subscribe-unsubscribe). | 
| method | Reeks | De HTTP-methode gebruikt voor de Abonnementaanvraag: 'Ophalen', 'Plaatsen', 'Posten', 'PATCH', 'Verwijderen' of 'HEAD' | 
| uri | Reeks | De eindpunt-URL voor de locatie waar de Abonnementaanvraag verzenden | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| afmelden | JSON-Object | De uitgaande aanvraag aan automatisch aanroepen en het abonnement annuleren wanneer een bewerking de trigger ongeldig maakt. Zie voor meer informatie [abonneren en afmelden](#subscribe-unsubscribe). | 
| method | Reeks | De HTTP-methode moet worden gebruikt voor de annuleringsaanvraag: 'Ophalen', 'Plaatsen', 'Posten', 'PATCH', 'Verwijderen' of 'HEAD' | 
| uri | Reeks | De eindpunt-URL voor de locatie waar u de annuleringsaanvraag verzenden | 
| hoofdtekst | JSON-Object | De JSON-object dat de nettolading (gegevens) voor de aanvraag-abonnement of annulering beschrijft | 
| verificatie | JSON-Object | De methode die een binnenkomende aanvraag moet worden gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
| retryPolicy | JSON-Object | Dit object wordt het gedrag voor opnieuw proberen voor onregelmatige fouten die 4xx of 5xx statuscodes hebt aangepast: <p>`"retryPolicy": { "type": "<retry-policy-type>", "interval": "<retry-interval>", "count": <number-retry-attempts> }` <p>Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
|||| 

*Voorbeeld*

```json
"myAppSpotTrigger": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "POST",
         "uri": "https://pubsubhubbub.appspot.com/subscribe",
         "headers": {},
         "body": {
            "hub.callback": "@{listCallbackUrl()}",
            "hub.mode": "subscribe",
            "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
         },
      },
      "unsubscribe": {
         "method": "POST",
         "url": "https://pubsubhubbub.appspot.com/subscribe",
         "body": {
            "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
            "hub.mode": "unsubscribe",
            "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
         },
      }
   },
}
```

<a name="subscribe-unsubscribe"></a>

### <a name="subscribe-and-unsubscribe"></a>`subscribe` en `unsubscribe`

De `subscribe` aanroep gebeurt er wanneer de werkstroom op een manier, bijvoorbeeld wijzigingen wanneer de referenties worden vernieuwd of parameters wijzigen door de trigger de invoer. De aanroep maakt gebruik van dezelfde parameters als standaard HTTP-acties. 
 
De `unsubscribe` aanroep gebeurt automatisch wanneer een bewerking de trigger HTTPWebhook ongeldig, bijvoorbeeld maakt:

* Het verwijderen of uitschakelen van de trigger. 
* Het verwijderen of uitschakelen van de werkstroom. 
* Het verwijderen of uitschakelen van het abonnement. 

Ter ondersteuning van deze aanroepen de `@listCallbackUrl()` functie retourneert een unieke 'retouraanroep-URL voor deze trigger. Deze URL vertegenwoordigt een unieke id voor de eindpunten die van de service REST API gebruikmaken. De parameters voor deze functie zijn hetzelfde als de HTTP-trigger.

### <a name="httpwebhook-trigger-outputs"></a>HTTPWebhook trigger-uitvoer

| Elementnaam | Type | Beschrijving |
| ------------ | ---- | ----------- |
| headers | JSON-Object | De headers van het HTTP-antwoord | 
| hoofdtekst | JSON-Object | De hoofdtekst van het HTTP-antwoord | 
|||| 

<a name="apiconnectionwebhook-trigger"></a>

## <a name="apiconnectionwebhook-trigger"></a>ApiConnectionWebhook trigger

Deze trigger werkt als de [HTTPWebhook trigger](#httpwebhook-trigger), maar gebruikt [Microsoft beheerde API's](../connectors/apis-list.md). 

Hier volgt de definitie van de trigger:

```json
"<ApiConnectionWebhookTriggerName>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },        
      "body": {
          "NotificationUrl": "@{listCallbackUrl()}"
      },
      "queries": "<query-parameters>"
   }
}
```

*Vereist*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| <*ApiConnectionWebhookTriggerName*> | JSON-Object | De naam voor de trigger een object dat wordt beschreven in de notatie JSON (Javascript Object)-indeling is  | 
| type | Reeks | De triggertype, 'ApiConnectionWebhook' | 
| invoer | JSON-Object | De trigger invoerwaarden die van de trigger gedrag definiëren | 
| host | JSON-Object | De JSON-object dat de host-gateway en de ID voor de beheerde API beschrijft <p>De `host` JSON-object heeft deze elementen: `api` en `connection` | 
| verbinding | JSON-Object | De naam voor de beheerde API verbinding die gebruikmaakt van de werkstroom, waaronder een verwijzing naar een parameter met de naam moet `$connection`: <p>`"name": "@parameters('$connections')['<connection-name>']['connectionId']"` | 
| hoofdtekst | JSON-Object | De JSON-object die worden beschreven de nettolading (gegevens) om te verzenden naar de beheerde API | 
| NotificationUrl | Reeks | Retourneert een unieke 'retouraanroep-URL voor deze trigger die de beheerde API gebruiken kunt. | 
|||| 

*Optioneel*

| Elementnaam | Type | Beschrijving | 
| ------------ | ---- | ----------- | 
| query's | JSON-Object | Queryparameters die u wilt opnemen in de URL <p>Dit element wordt bijvoorbeeld toegevoegd de `?folderPath=Inbox` querytekenreeks naar de URL: <p>`"queries": { "folderPath": "Inbox" }` <p>Resultaat: `https://<managed-API-URL>?folderPath=Inbox` | 
|||| 

<a name="trigger-conditions"></a>

## <a name="triggers-conditions"></a>Triggers: voorwaarden

U kunt een matrix met een of meer voorwaarden om te bepalen of de werkstroom moet worden uitgevoerd of niet opnemen voor een trigger. In dit voorbeeld wordt de trigger rapport alleen tijdens de werkstroom wordt geactiveerd `sendReports` parameter is ingesteld op true. 

```json
"myDailyReportTrigger": {
   "type": "Recurrence",
   "conditions": [ {
      "expression": "@parameters('sendReports')"
   } ],
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

Voorwaarden kunnen ook verwijzen naar de statuscode van de trigger. Stel bijvoorbeeld dat u wilt een werkstroom alleen starten als uw website '500' statuscode geretourneerd:

``` json
"conditions": [ {
   "expression": "@equals(triggers().code, 'InternalServerError')"  
} ]  
```  

> [!NOTE]
> Standaard een trigger wordt geactiveerd alleen op de ontvangst een ' 200 OK ' antwoord. Als een expressie verwijst naar een trigger statuscode in op een manier, wordt het standaardgedrag van de trigger vervangen. Dus als u wilt dat de trigger moet worden gestart voor meerdere statuscodes, bijvoorbeeld 200-statuscode en -statuscode 201, moet u deze instructie als de voorwaarde opnemen: 
>
> `@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="triggers-split-an-array-into-multiple-runs"></a>Triggers: Een matrix splitsen in meerdere wordt uitgevoerd

Als de trigger een matrix voor uw logische app retourneert verwerken, soms een lus 'voor elk' te lang zou duren voor het verwerken van elk matrixitem. In plaats daarvan kunt u de **SplitOn** eigenschap in de trigger *debatch* de matrix. 

Debatching splitst de items van de matrix en start een nieuwe logische app kopie die wordt uitgevoerd voor elk matrixitem. Deze aanpak is bijvoorbeeld handig als u een eindpunt dat meerdere nieuwe items tussen polling-intervallen retourneren mogelijk polling wilt.
Voor het maximum aantal matrix-items die **SplitOn** kan verwerken in een uitvoering van één logische app, Zie [en -configuratie](../logic-apps/logic-apps-limits-and-config.md). 

> [!NOTE]
> U kunt toevoegen **SplitOn** alleen voor triggers door handmatig definiëren of in de weergave van de code voor uw logische app JSON-definitie te overschrijven. U kunt geen gebruiken **SplitOn** als u wilt implementeren een patroon synchrone antwoord. Elke werkstroom die gebruikmaakt van **SplitOn** en bevat een antwoord actie asynchroon uitgevoerd en onmiddellijk een `202 ACCEPTED` antwoord.

Als de trigger Swagger-bestand een nettolading die een matrix beschrijft de **SplitOn** eigenschap wordt automatisch toegevoegd aan de trigger. Anders wordt deze eigenschap binnen de nettolading van antwoord met de matrix die u wilt debatch toevoegen. 

Stel bijvoorbeeld dat u hebt een API die dit antwoord geretourneerd: 
  
```json
{
    "Status": "Succeeded",
    "Rows": [ 
        { 
            "id": 938109380,
            "name": "customer-name-one"
        },
        {
            "id": 938109381,
            "name": "customer-name-two"
        }
    ]
}
```
  
Uw logische app hoeft alleen de inhoud van `Rows`, zodat u een trigger zoals in dit voorbeeld kunt maken.

``` json
"myDebatchTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "Second",
        "interval": 1
    },
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> Als u de `SplitOn` uitvoert, en u de eigenschappen die zich buiten de matrix kan niet ophalen. Bijvoorbeeld, er dus kan niet de `status` eigenschap in het antwoord geretourneerd van de API.
> 
> Om te voorkomen dat een fout als de `Rows` eigenschap bestaat niet, in dit voorbeeld wordt de `?` operator.

De werkstroomdefinitie van de kan nu gebruikmaken van `@triggerBody().name` ophalen `customer-name-one` van de eerste uitvoering en `customer-name-two` van de tweede uitvoeren. Ja, de trigger uiterlijk levert, zoals deze voorbeelden:

```json
{
    "body": {
        "id": 938109380,
        "name": "customer-name-one"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "customer-name-two"
    }
}
```

<a name="trigger-operation-options"></a>

## <a name="triggers-operation-options"></a>Triggers: Opties voor bewerking

Deze triggers biedt meer mogelijkheden waarmee u het standaardgedrag wijzigen.

| Trigger | Optie bij bewerking | Beschrijving |
|---------|------------------|-------------|
| [Terugkeerpatroon](#recurrence-trigger), <br>[HTTP](#http-trigger), <br>[ApiConnection](#apiconnection-trigger) | singleInstance | Fire de trigger nadat alle alleen actief wordt uitgevoerd is voltooid. |
||||

<a name="single-instance"></a>

### <a name="triggers-fire-only-after-active-runs-finish"></a>Triggers: Gestart alleen als actief wordt uitgevoerd voltooien

Triggers waarin u het terugkeerpatroon kunt instellen, kunt u opgeven dat de trigger brand nadat alle alleen actief wordt uitgevoerd hebt voltooid. Als een geplande terugkeer gebeurt terwijl een werkstroomexemplaar dat wordt uitgevoerd, wordt de trigger wordt overgeslagen en wordt gewacht totdat de volgende terugkeerpatroon geplande voordat u opnieuw controleert. Bijvoorbeeld:

```json
"myRecurringTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Hour",
        "interval": 1,
    },
    "operationOptions": "singleInstance"
}
```

<a name="actions-overview"></a>

## <a name="actions-overview"></a>Overzicht van de acties

Er zijn veel verschillende soorten acties, elk met unieke gedrag. Elk actietype heeft verschillende invoerwaarden die een actie gedrag definiëren. Verzameling acties kunnen veel andere acties in zichzelf bevatten. 

### <a name="standard-actions"></a>Standaardacties  

| Actietype | Beschrijving | 
| ----------- | ----------- | 
| **HTTP** | Hiermee wordt een HTTP-web-eindpunt. | 
| **ApiConnection**  | Net als bij de HTTP-actie, maar gebruikt [Microsoft beheerde API's](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | Werkt zoals HTTPWebhook, maar gebruikt door Microsoft beheerde API's. | 
| **Antwoord** | Hiermee definieert u de reactie voor een inkomend gesprek. | 
| **Opstellen** | Hiermee wordt een willekeurig object uit de invoer van de actie. | 
| **Functie** | Hiermee geeft u een Azure-functie. | 
| **Wacht** | Wacht een vast bedrag tijd of totdat een bepaalde tijd. | 
| **Werkstroom** | Hiermee geeft u een geneste werkstroom. | 
| **Opstellen** | Hiermee wordt een willekeurig object uit de invoer van de actie. | 
| **Query** | Een matrix die op basis van een voorwaarde filtert. | 
| **Selecteren** | Elk element van een matrix projecteert in een nieuwe waarde. U kunt bijvoorbeeld een matrix van getallen omzetten in een matrix met objecten. | 
| **Tabel** | Converteert een matrix van items naar een CSV of HTML-tabel. | 
| **Beëindigen** | Een werkstroom wordt gestopt. | 
| **Wacht** | Wacht een vast bedrag tijd of totdat een bepaalde tijd. | 
| **Werkstroom** | Hiermee geeft u een geneste werkstroom. | 
||| 

### <a name="collection-actions"></a>Verzameling acties

| Actietype | Beschrijving | 
| ----------- | ----------- | 
| **If** | Een expressie niet evalueren en op basis van het resultaat, de bijbehorende vertakking wordt uitgevoerd. | 
| **Switch** | Andere acties op basis van specifieke waarden van een object uitvoeren. | 
| **ForEach** | Deze actie samenvoegartikel een matrix doorlopen en interne acties uitvoert op elk matrixitem. | 
| **pas** | Deze actie samenvoegartikel worden interne acties uitgevoerd totdat een voorwaarde in waar resulteert. | 
| **Bereik** | Gebruik dit voor andere acties logisch groeperen. | 
|||  

## <a name="http-action"></a>HTTP-actie  

Een HTTP-bewerking aangeroepen een opgegeven eindpunt en het antwoord om te bepalen of de werkstroom moet worden uitgevoerd of niet wordt gecontroleerd. Bijvoorbeeld:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest"
    }
}
```

Hier de `inputs` object nodig deze parameters die zijn vereist voor het maken van een HTTP-aanroep: 

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| method | Ja | Reeks | Maakt gebruik van een van deze methoden HTTP: 'Ophalen', 'Posten', 'Plaatsen', 'Verwijderen', 'PATCH' of 'HEAD' | 
| uri | Ja| Reeks | De HTTP of HTTPs-eindpunt dat de trigger wordt gecontroleerd. Maximale grootte van tekenreeks: 2 KB | 
| query's | Nee | JSON-Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | JSON-Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | JSON-Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | JSON-Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nee | Reeks | Definieert de set met speciaal gedrag negeren. | 
| verificatie | Nee | JSON-Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). <p>Afgezien van Scheduler, er is een meer ondersteunde eigenschap: `authority`. Deze waarde is standaard `https://login.windows.net` wanneer niet wordt opgegeven, maar u kunt een andere waarde, zoals`https://login.windows\-ppe.net`. | 
||||| 

HTTP-acties en APIConnection acties ondersteunen *beleid probeer*. Een beleid voor opnieuw proberen is van toepassing op onregelmatige problemen, gekenmerkt als HTTP-statuscodes 408 429 en 5xx, naast eventuele uitzonderingen connectiviteit. Kunt u dit beleid met de `retryPolicy` object als volgt te werk:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

In dit voorbeeld HTTP-actie pogingen ophalen van het laatste nieuws twee keer als er onregelmatige problemen voor een totaal van drie uitvoeringen en een vertraging van 30 seconden tussen elke poging:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy": {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

Het interval voor opnieuw proberen is opgegeven in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601). Dit interval heeft een waarde standaard en een minimum van 20 seconden, terwijl de maximale waarde een uur is. Het standaard- en maximum aantal is gelijk aan vier uur. Als u een definitie van het beleid voor opnieuw proberen niet opgeeft een `fixed` strategie met een standaardwaarde opnieuw telling en het interval wordt gebruikt. Als wilt uitschakelen in het beleid voor opnieuw proberen, stelt u het type in op `None`.

### <a name="asynchronous-patterns"></a>Asynchrone patronen

Standaard ondersteunt alle HTTP-gebaseerde acties het patroon standaard asynchrone bewerking. Als de externe server geeft aan dat de aanvraag is geaccepteerd voor verwerking met een antwoord '202 GEACCEPTEERDE', blijft de Logic Apps-engine zodat de URL die is opgegeven in de antwoordheader locatie tot het bereiken van een definitieve status heeft een antwoord is niet 202 gedelegeerd.
  
Om te schakelen het asynchrone gedrag die eerder is beschreven, stelt `operationOptions` naar `DisableAsyncPattern` in de invoer in te grijpen. In dit geval wordt is de uitvoer van de actie gebaseerd op de eerste 202 reactie van de server. Bijvoorbeeld:
  
```json
"invokeLongRunningOperationAction": {
    "type": "Http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

<a name="asynchronous-limits"></a>

#### <a name="asynchronous-limits"></a>Asynchrone limieten

U kunt de duur van een asynchrone patroon voor een bepaald tijdsinterval beperken. Als het tijdsinterval is verstreken zonder dat een definitieve status bereikt, de status van de actie is gemarkeerd `Cancelled` met een `ActionTimedOut` code. De time-out voor de limiet is opgegeven in de ISO 8601-notatie. Dit voorbeeld ziet hoe u beperkingen kunt opgeven:


``` json
"<action-name>": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>APIConnection actie

Deze actie verwijst naar een connector voor Microsoft beheerde, vereisen een verwijzing naar een geldige verbinding en informatie over de API en de parameters. Hier volgt een voorbeeld APIConnection actie:

```json
"Send_Email": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "POST",
        "body": {
            "Subject": "New tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
}
```

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | JSON-Object | Hiermee geeft u de connector-informatie, zoals de `runtimeUrl` en verwijzing naar het object connection. | 
| method | Ja | Reeks | Maakt gebruik van een van deze methoden HTTP: 'Ophalen', 'Posten', 'Plaatsen', 'Verwijderen', 'PATCH' of 'HEAD' | 
| pad | Ja | Reeks | Het pad voor de API-bewerking | 
| query's | Nee | JSON-Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | JSON-Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | JSON-Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | JSON-Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nee | Reeks | Definieert de set met speciaal gedrag negeren. | 
| verificatie | Nee | JSON-Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
||||| 

Een beleid voor opnieuw proberen is van toepassing op onregelmatige problemen, gekenmerkt als HTTP-statuscodes 408 429 en 5xx, naast eventuele uitzonderingen connectiviteit. Kunt u dit beleid met de `retryPolicy` object als volgt te werk:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

## <a name="apiconnection-webhook-action"></a>APIConnection webhook actie

De actie APIConnectionWebhook verwijst naar een connector door Microsoft beheerd. Deze actie vereist een verwijzing naar een geldige verbinding en informatie over de API en de parameters. U kunt limieten opgeven van een webhook-actie op dezelfde manier als [HTTP asynchrone limieten](#asynchronous-limits).

```json
"Send_approval_email": {
    "type": "ApiConnectionWebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | JSON-Object | Hiermee geeft u de connector-informatie, zoals de `runtimeUrl` en verwijzing naar het object connection. | 
| pad | Ja | Reeks | Het pad voor de API-bewerking | 
| query's | Nee | JSON-Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | JSON-Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | JSON-Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | JSON-Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nee | Reeks | Definieert de set met speciaal gedrag negeren. | 
| verificatie | Nee | JSON-Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Antwoord actie  

Met deze actie de nettolading van het volledige antwoord van een HTTP-aanvraag bevat en bevat een `statusCode`, `body`, en `headers`:
  
```json
"myResponseAction": {
    "type": "Response",
    "inputs": {
        "statusCode": 200,
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        }
    },
    "runAfter": {}
}
```

De actie antwoord heeft speciale beperkingen die niet van toepassing op andere acties specifiek:  
  
* Omdat de binnenkomende aanvraag is een deterministische antwoord vereist, kunt u de reacties hebben in parallelle vertakkingen binnen een definitie van logic Apps.
  
* Als de werkstroom een actie antwoord bereikt nadat de binnenkomende aanvraag is al een antwoord ontvangen, wordt de actie antwoord beschouwd als mislukte of conflicterend wordt beschouwd. Als gevolg hiervan is gemarkeerd als de logische app uitgevoerd `Failed`.
  
* Kan niet worden gebruikt in een werkstroom met reacties de `splitOn` opdracht in de definitie van de trigger omdat de oproep wordt gemaakt van meerdere wordt uitgevoerd. Als gevolg hiervan controleren voor deze aanvraag bij de bewerking van de werkstroom wordt geplaatst en een 'onjuiste aanvraag' antwoord retourneren.

## <a name="compose-action"></a>Actie opstellen

Deze actie kunt u een willekeurig object samenstellen en de uitvoer is het resultaat van evaluatie van de invoer van de actie. 

> [!NOTE]
> U kunt de `Compose` actie voor het maken van uitvoer, met inbegrip van objecten, matrices en een ander type systeemeigen worden ondersteund door logische apps zoals XML en binary.

U kunt bijvoorbeeld de `Compose` actie voor het samenvoegen van de uitvoer van meerdere acties:

```json
"composeUserRecordAction": {
    "type": "Compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
    }
}
```

## <a name="function-action"></a>Functie actie

Deze actie kunt u vertegenwoordigen en aanroep een [Azure functie](../azure-functions/functions-overview.md), bijvoorbeeld:

```json
"<my-Azure-Function-name>": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/sites/<your-Azure-function-app-name>/functions/<your-Azure-function-name>"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()"
        },
        "method": "POST",
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- |  
| functie-id | Ja | Reeks | De resource-ID voor de Azure-functie die u wilt aanroepen. | 
| method | Nee | Reeks | De HTTP-methode die wordt gebruikt voor het aanroepen van de functie. Als niet wordt opgegeven, is "POST" de standaardmethode. | 
| query's | Nee | JSON-Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | JSON-Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | JSON-Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
|||||

Wanneer u uw logische app opslaat, voert de Logic Apps-engine sommige controles op de functie waarnaar wordt verwezen:

* U moet toegang hebben tot de functie.
* U kunt alleen een standaard HTTP-trigger of generic Webhook JSON-trigger.
* De functie mag geen enkele route gedefinieerd.
* Alleen 'de functie' en 'anonymous' machtigingsniveaus zijn toegestaan.

> [!NOTE]
> De engine voor Logic Apps opgehaald en plaatst de trigger-URL die wordt gebruikt tijdens runtime. Dus als een bewerking wordt ongeldig gemaakt van de URL in de cache, mislukt de actie tijdens runtime. U kunt dit probleem omzeilen, sla de logische app opnieuw, waardoor de logische app op te halen en de trigger-URL opnieuw in de cache.

## <a name="select-action"></a>Selecteer actie

Deze actie kunt u elk element van een matrix project in een nieuwe waarde. In dit voorbeeld wordt een matrix van getallen in een matrix met objecten geconverteerd:

```json
"selectNumbersAction": {
    "type": "Select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| uit | Ja | Matrix | De bronmatrix |
| selecteren | Ja | Alle | De projectie toegepast op elk element in de bronmatrix |
||||| 

De uitvoer van de `select` actie is een matrix met de dezelfde kardinaliteit als de invoermatrix. Elk element wordt omgezet, zoals gedefinieerd door de `select` eigenschap. Als de invoer een lege matrix is, wordt de uitvoer is ook een lege matrix.

## <a name="terminate-action"></a>Actie beëindigen

Deze actie stopt een workflow uitvoeren, annuleren van eventuele acties uitgevoerd en alle overige acties wordt overgeslagen. De actie beëindigen heeft geen invloed op reeds voltooide acties.

Bijvoorbeeld, een uitvoering met stoppen `Failed` status:

```json
"HandleUnexpectedResponse": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response",
        }
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Ja | Reeks | Het doel uitvoert de status een is `Failed` of `Cancelled` |
| runError | Nee | JSON-Object | De details van de fout. Ondersteunde alleen wanneer `runStatus` is ingesteld op `Failed`. |
| runError code | Nee | Reeks | De run foutcode: |
| runError bericht | Nee | Reeks | Foutbericht van de uitvoering | 
||||| 

## <a name="query-action"></a>Queryactie

Deze actie kunt u filteren op basis van een voorwaarde matrix. 

> [!NOTE]
> U kunt de actie opstellen niet gebruiken om eventuele uitvoer, met inbegrip van objecten, matrices en een ander type systeemeigen worden ondersteund door logische apps zoals XML en binaire samen te stellen.

Als u bijvoorbeeld getallen die groter is dan twee selecteren:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| uit | Ja | Matrix | De bronmatrix |
| waar | Ja | Reeks | De voorwaarde die wordt toegepast op elk element van de bronmatrix. Als geen waarden voldoen aan de `where` voorwaarde, het resultaat is een lege matrix. |
||||| 

De uitvoer van de `query` actie is een matrix met elementen van de invoermatrix die voldoen aan de voorwaarde.

## <a name="table-action"></a>Tabel actie

Deze actie kunt u een matrix converteren naar een CSV of HTML-tabel. 

```json
"ConvertToTable": {
    "type": "Table",
    "inputs": {
        "from": "<source-array>",
        "format": "CSV | HTML"
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| uit | Ja | Matrix | De bronmatrix. Als de `from` waarde van de eigenschap is een lege matrix, de uitvoer is een lege tabel. | 
| Indeling | Ja | Reeks | De tabelindeling die u wilt, 'CSV' of 'HTML' | 
| Kolommen | Nee | Matrix | De tabelkolommen die u wilt. Met de vorm van de tabel standaard wordt genegeerd. | 
| kolomkop | Nee | Reeks | De kolomkop | 
| waarde in de kolom | Ja | Reeks | De waarde in de kolom | 
||||| 

Stel dat u een actie van de tabel zoals in dit voorbeeld definiëren:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "HTML"
    }
}
```

En gebruik deze matrix voor `@triggerBody()`:

```json
[ {"ID": 0, "Name": "apples"},{"ID": 1, "Name": "oranges"} ]
```

Dit is de uitvoer van dit voorbeeld:

<table><thead><tr><th>Id</th><th>Naam</th></tr></thead><tbody><tr><td>0</td><td>appels</td></tr><tr><td>1</td><td>appels</td></tr></tbody></table>

Voor het aanpassen van deze tabel kunt u de kolommen expliciet, bijvoorbeeld:

```json
"ConvertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [ 
            {
                "header": "Produce ID",
                "value": "@item().id"
            },
            {
              "header": "Description",
              "value": "@concat('fresh ', item().name)"
            }
        ]
    }
}
```

Dit is de uitvoer van dit voorbeeld:

<table><thead><tr><th>ID maken</th><th>Beschrijving</th></tr></thead><tbody><tr><td>0</td><td>nieuwe appels</td></tr><tr><td>1</td><td>nieuwe appels</td></tr></tbody></table>

## <a name="wait-action"></a>Actie wachten  

Deze actie onderbreekt de uitvoering van de werkstroom voor het opgegeven interval. In dit voorbeeld wordt de werkstroom 15 minuten wachten:
  
```json
"waitForFifteenMinutesAction": {
    "type": "Wait",
    "inputs": {
        "interval": {
            "unit": "minute",
            "count": 15
        }
    }
}
```
  
Als u wilt wachten tot een bepaald moment, kunt u ook in dit voorbeeld gebruiken:
  
```json
"waitUntilOctoberAction": {
    "type": "Wait",
    "inputs": {
        "until": {
            "timestamp": "2017-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> Kunt u de duur van de wachttijd opgeven met ofwel de `interval` object of de `until` , maar niet beide.

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| tot | Nee | JSON-Object | De duur van de wachttijd op basis van een punt in tijd | 
| Pas de timestamp | Ja | Reeks | Punt in tijd in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) wanneer de wachttijd is verlopen | 
| interval | Nee | JSON-Object | De duur van de wachttijd op basis van de intervaleenheid en het aantal | 
| intervaleenheid | Ja | Reeks | Tijdseenheid. Gebruik slechts één van deze waarden: 'tweede', 'minuut', 'uur', 'day', 'week' of 'maand' | 
| interval aantal | Ja | Geheel getal | Een positief geheel getal dat het aantal gebruikt voor de duur van de wachttijd intervaleenheden | 
||||| 

## <a name="workflow-action"></a>Werkstroomactie

Deze actie kunt u een werkstroom worden genest. De Logic Apps-engine voert een toegangscontrole in de werkstroom onderliggende meer specifiek, de trigger, dus u toegang tot de onderliggende werkstroom hebben moet. Bijvoorbeeld:

```json
"<my-nested-workflow-action-name>": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/<my-subscription-ID>/resourceGroups/<my-resource-group-name>/providers/Microsoft.Logic/<my-nested-workflow-action-name>",
            "triggerName": "mytrigger001"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- |  
| host-id | Ja | Reeks| De resource-ID voor de werkstroom die u wilt aanroepen | 
| host triggernaam | Ja | Reeks | De naam van de trigger die u wilt aanroepen | 
| query's | Nee | JSON-Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | JSON-Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | JSON-Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
||||| 

De uitvoer van deze actie zijn gebaseerd op wat u definieert in de `Response` actie voor de onderliggende werkstroom. Als de onderliggende werkstroom bevat geen definitie van een `Response` -actie voor de uitvoer is leeg.

## <a name="collection-actions-overview"></a>Overzicht van de verzameling acties

Om te bepalen van de uitvoering van de werkstroom, verzameling acties kunnen andere acties omvatten. U kunt rechtstreeks verwijzen voor verwijzingen naar acties in een verzameling buiten de verzameling. Als u bijvoorbeeld een `Http` actie in een bereik vervolgens `@body('http')` nog geldig overal in een werkstroom. Ook kunnen de acties in een verzameling alleen 'uitgevoerd na' andere acties in dezelfde verzameling.

## <a name="if-action"></a>Als actie

Deze actie een voorwaarde is, kunt u een voorwaarde evalueren en een vertakking op basis van of de expressie wordt geëvalueerd als waar uitvoeren. Als de voorwaarde waar is, wordt de voorwaarde gemarkeerd met de status 'Voltooid'. Acties die zich in de `actions` of `else` objecten worden geëvalueerd tot deze waarden:

* 'Geslaagd' wanneer ze worden uitgevoerd en mislukt
* 'Is mislukt' wanneer ze worden uitgevoerd en mislukt
* 'Overgeslagen' wanneer de respectieve vertakking kan niet worden uitgevoerd

Meer informatie over [voorwaardelijke instructies in logic apps](../logic-apps/logic-apps-control-flow-conditional-statement.md).

``` json
"<my-condition-name>": {
  "type": "If",
  "expression": "<condition>",
  "actions": {
    "if-true-run-this-action": {
      "type": <action-type>,
      "inputs": {},
      "runAfter": {}
    }
  },
  "else": {
    "actions": {
        "if-false-run-this-action": {
            "type": <action-type>,
            "inputs": {},
            "runAfter": {}
        }
    }
  },
  "runAfter": {}
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| acties | Ja | JSON-Object | De interne acties uit te voeren wanneer `expression` resulteert in `true` | 
| expressie | Ja | Reeks | De expressie om te evalueren |
| anders | Nee | JSON-Object | De interne acties uit te voeren wanneer `expression` resulteert in `false` |
||||| 

Bijvoorbeeld:

```json
"myCondition": {
    "type": "If",
    "actions": {
        "if-true-check-this-website": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://this-url"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if-false-check-this-other-website": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://this-other-url"
                },
                "runAfter": {}
            }
        }
    }
}
```  

### <a name="how-conditions-can-use-expressions-in-actions"></a>Hoe voorwaarden expressies in acties kunnen gebruiken

Hier volgen enkele voorbeelden die laten zien hoe u expressies kunt gebruiken in omstandigheden:
  
| JSON-expressie | Resultaat | 
| --------------- | ------ | 
| `"expression": "@parameters('hasSpecialAction')"` | Een waarde die wordt geëvalueerd als waar, wordt dit probleem op te geven. Ondersteunt alleen Booleaanse expressies. Als u wilt converteren andere typen naar Booleaanse waarde, gebruik van deze functies: `empty` of `equals` | 
| `"expression": "@greater(actions('action1').output.value, parameters('threshold'))"` | Vergelijking van functies ondersteunt. Bijvoorbeeld, de actie wordt alleen uitgevoerd als de uitvoer van action1 groter dan de drempelwaarde is. | 
| `"expression": "@or(greater(actions('action1').output.value, parameters('threshold')), less(actions('action1').output.value, 100))"` | Biedt ondersteuning voor bedrijfslogica-functies voor het maken van geneste Booleaanse expressies. In dit voorbeeld wordt de actie uitgevoerd wanneer de uitvoer van action1 meer dan de drempelwaarde of onder de 100 is. | 
| `"expression": "@equals(length(actions('action1').outputs.errors), 0))"` | Om te controleren dat of een matrix geen objecten heeft, kunt u matrixfuncties. In dit voorbeeld wordt de actie uitgevoerd wanneer de fouten matrix leeg is. | 
| `"expression": "parameters('hasSpecialAction')"` | Deze expressie wordt een fout veroorzaakt en is niet een geldige voorwaarde. Voorwaarden moeten gebruikmaken van de ' @ ' symbool. | 
||| 

## <a name="switch-action"></a>Switch-actie

Deze actie een switch-instructie wordt, voert verschillende acties op basis van specifieke waarden van een object, een expressie of een token. Deze actie evalueert het object, de expressie of het token, kiest de aanvraag die overeenkomt met het resultaat en acties voor alleen deze aanvraag wordt uitgevoerd. Als er geen enkel geval overeenkomt met het resultaat, wordt de standaardactie uitgevoerd. Wanneer de switch-instructie wordt uitgevoerd, hebben slechts één case moet overeenkomen met het resultaat. Meer informatie over [instructies in logic apps-switch](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"<my-switch-statement-name>": {
   "type": "Switch",
   "expression": "<evaluate-this-object-expression-token>",
   "cases": {
      "myCase1": {
         "actions": {
           "myAction1": {}
         },
         "case": "<result1>"
      },
      "myCase2": {
         "actions": {
           "myAction2": {}
         },
         "case": "<result2>"
      }
   },
   "default": {
      "actions": {
          "myDefaultAction": {}
      }
   },
   "runAfter": {}
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| expressie | Ja | Reeks | Het object, expressie of token evalueren | 
| gevallen | Ja | JSON-Object | Bevat de sets met interne acties die worden uitgevoerd op basis van het resultaat van de expressie. | 
| Aanvraag | Ja | Reeks | De waarde waarmee moet overeenkomen met het resultaat | 
| acties | Ja | JSON-Object | De interne acties die worden uitgevoerd voor de aanvraag die overeenkomt met het resultaat van de expressie | 
| standaardinstelling | Nee | JSON-Object | De interne acties die worden uitgevoerd als er geen cases overeenkomen met het resultaat | 
||||| 

Bijvoorbeeld:

``` json
"myApprovalEmailAction": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {...}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions": {
           "Send_an_email_2": {...}
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

## <a name="foreach-action"></a>Foreach-actie

Deze actie samenvoegartikel een matrix doorlopen en interne acties uitvoert op elk matrixitem. Standaard wordt de lus Foreach parallel uitgevoerd. Voor het maximale aantal parallelle replicatiecycli die "voor elk' lussen kan uitvoeren, Zie [limieten en config](../logic-apps/logic-apps-limits-and-config.md). Voor elke cyclus worden opeenvolgend uitgevoerd, stelt de `operationOptions` -parameter voor `Sequential`. Meer informatie over [Foreach lus in logic apps](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop).

```json
"<my-forEach-loop-name>": {
    "type": "Foreach",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    },
    "foreach": "<array>",
    "runAfter": {}
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| acties | Ja | JSON-Object | De interne acties uit te voeren binnen de lus | 
| foreach | Ja | Reeks | De matrix te doorlopen | 
| operationOptions | Nee | Reeks | Hiermee geeft u alle opties voor de bewerking voor het aanpassen van gedrag. Ondersteunt momenteel alleen `Sequential` voor opeenvolgend uitgevoerd iteraties waar het standaardgedrag parallelle is. |
||||| 

Bijvoorbeeld:

```json
"forEach_EmailAction": {
    "type": "Foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "Send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "foreach": "@body('email_filter')",
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Totdat de actie

Deze samenvoegartikel actie wordt de interne acties uitgevoerd totdat een voorwaarde wordt geëvalueerd als waar. Meer informatie over ['tot' Lussen in logic apps](../logic-apps/logic-apps-control-flow-loops.md#until-loop).

```json
 "<my-Until-loop-name>": {
    "type": "Until",
    "actions": {
        "myActionName": {
            "type": "<action-type>",
            "inputs": {},
            "runAfter": {}
        }
    },
    "expression": "<myCondition>",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| acties | Ja | JSON-Object | De interne acties uit te voeren binnen de lus | 
| expressie | Ja | Reeks | De expressie na elke iteratie evalueren | 
| Limiet | Ja | JSON-Object | De limieten voor de lus. Moet ten minste één limiet definiëren. | 
| aantal | Nee | Geheel getal | De limiet voor het aantal iteraties om uit te voeren | 
| timeout | Nee | Reeks | De time-outlimiet in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601) die specificeert hoe lang de lus moet worden uitgevoerd |
||||| 

Bijvoorbeeld:

```json
 "runUntilSucceededAction": {
    "type": "Until",
    "actions": {
        "Http": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 100,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

## <a name="scope-action"></a>Bereikactie

Deze actie kunt u logische groep acties in een werkstroom. Het bereik wordt ook een eigen status nadat alle acties in dat bereik voltooien uitgevoerd. Meer informatie over [scopes](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md).

```json
"<my-scope-action-name>": {
    "type": "Scope",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- |  
| acties | Ja | JSON-Object | De interne acties uit te voeren binnen het bereik |
||||| 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [werkstroom Definition Language](../logic-apps/logic-apps-workflow-definition-language.md)
* Meer informatie over [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
---
title: Trigger en actie van het type referentie - Azure Logic Apps | Microsoft Docs
description: Meer informatie over typen trigger en actie in Azure Logic Apps, zoals is beschreven in de werkstroom van werkstroomschema
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: reference
ms.date: 06/22/2018
ms.openlocfilehash: c817f017c7394943864e7f20a130c90d3f8485d9
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885975"
---
# <a name="trigger-and-action-types-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Documentatie over de trigger en actie typen voor Definitietaal van werkstroom in Azure Logic Apps

In [Azure Logic Apps](../logic-apps/logic-apps-overview.md), alle werkstromen voor logische app begint met triggers gevolgd door acties. Dit artikel wordt de trigger en de actie die u gebruiken kunt bij het maken van logische apps voor het automatiseren van taken, processen en werkstromen beschreven. U kunt logische app werkstromen maken met de ontwerper van logische Apps visueel of door het ontwerpen van de definitie van de onderliggende werkstroom met de [Werkstroomdefinitietaal](../logic-apps/logic-apps-workflow-definition-language.md). U kunt logische apps in de Azure portal of Visual Studio maken. De onderliggende definitie voor de gehele werkstroom, met inbegrip van de trigger en acties, maakt gebruik van Javascript Object Notation (JSON).

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Triggers-overzicht

Alle logische apps beginnen met een trigger, waarin de aanroepen die exemplaar maken en een werkstroom voor logische apps te starten. Hier volgen de algemene trigger categorieën:

* Een *polling* trigger, die controleert of van een service-eindpunt met regelmatige intervallen

* Een *push* trigger, die een abonnement op een eindpunt maakt en biedt een *URL voor terugbellen* , zodat het eindpunt de trigger melden kan wanneer de opgegeven gebeurtenis plaatsvindt of gegevens beschikbaar zijn. De trigger wordt vervolgens gewacht voor van het eindpunt te antwoorden voordat starten. 

Triggers hebben deze elementen op het hoogste niveau, hoewel sommige optioneel zijn:  
  
```json
"<trigger-name>": {
   "type": "<trigger-type>",
   "inputs": { "<trigger-inputs>" },
   "recurrence": { 
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "conditions": [ "<array-with-conditions>" ],
   "runtimeConfiguration": { "<runtime-config-options>" },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
},
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*trigger-name*> | String | De naam van de trigger | 
| <*trigger-type*> | String | Het triggertype zoals 'Http' of "ApiConnection" | 
| <*trigger-inputs*> | JSON-Object | De invoer die van de trigger-gedrag bepalen | 
| <*time-unit*> | String | De tijdseenheid waarin wordt beschreven hoe vaak de trigger wordt geactiveerd: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month' | 
| <*nummer van de tijd eenheden*> | Geheel getal | Een waarde die aangeeft hoe vaak de trigger wordt geactiveerd op basis van de frequentie, het aantal tijdseenheden is moet worden gewacht tot het opnieuw door de trigger wordt geactiveerd <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequency 'Maand' is, is het terugkeerpatroon van de zes maanden. | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*array-with-conditions*> | Matrix | Een matrix met een of meer [voorwaarden](#trigger-conditions) die bepalen of de werkstroom uitvoert. Alleen beschikbaar voor triggers. | 
| <*runtime-config-options*> | JSON-Object | U kunt trigger runtimegedrag wijzigen door in te stellen `runtimeConfiguration` eigenschappen. Zie voor meer informatie, [Runtime-configuratie-instellingen](#runtime-config-options). | 
| <*splitOn-expression*> | String | Voor triggers die een matrix retourneert, kunt u een expressie opgeven die [splitst of *debatches* ](#split-on-debatch) items in meerdere werkstroomexemplaren voor de verwerking van een matrix. | 
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

## <a name="trigger-types-list"></a>Lijst met bestandstypen trigger

Elk triggertype heeft een andere interface- en invoer die van de trigger-gedrag bepalen. 

### <a name="built-in-triggers"></a>Ingebouwde triggers

| Triggertype | Description | 
|--------------|-------------| 
| [**HTTP**](#http-trigger) | Controleert of *polls* een willekeurig eindpunt. Dit eindpunt moet voldoen aan een contract specifieke trigger met behulp van een "202" asynchroon patroon of door te retourneren van een matrix. | 
| [**HTTPWebhook**](#http-webhook-trigger) | Hiermee maakt u een aanroepbare eindpunt voor uw logische app, maar roept de opgegeven URL om te registreren of registratie ongedaan maken. |
| [**Terugkeerpatroon**](#recurrence-trigger) | Wordt geactiveerd op basis van een ingesteld schema. U kunt een toekomstige datum en tijd voor het starten van deze trigger instellen. Op basis van de frequentie, u kunt ook opgeven tijdstippen en dagen voor het uitvoeren van uw werkstroom. | 
| [**Aanvraag**](#request-trigger)  | Hiermee maakt u een aanroepbare eindpunt voor uw logische app en wordt ook wel bekend als een 'handmatig' trigger. Zie bijvoorbeeld [aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten](../logic-apps/logic-apps-http-endpoint.md). | 
||| 

### <a name="managed-api-triggers"></a>Beheerde API-triggers

| Triggertype | Description | 
|--------------|-------------| 
| [**ApiConnection**](#apiconnection-trigger) | Controleert of *polls* een eindpunt met behulp van [Microsoft beheerde API's](../connectors/apis-list.md). | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | Hiermee maakt u een aanroepbare eindpunt voor uw logische app door het aanroepen van [Microsoft beheerde API's](../connectors/apis-list.md) abonneren en afmelden. | 
||| 

## <a name="triggers---detailed-reference"></a>Triggers - uitgebreide referentie

<a name="apiconnection-trigger"></a>

### <a name="apiconnection-trigger"></a>APIConnection trigger  

Deze trigger wordt gecontroleerd of *polls* een eindpunt met behulp van [Microsoft beheerde API's](../connectors/apis-list.md) , zodat de parameters voor deze trigger kan verschillen op basis van het eindpunt. Veel secties in deze definitie van de trigger zijn optioneel. Gedrag van de trigger is afhankelijk van de uitleenstatus secties opgenomen zijn.

```json
"<APIConnection_trigger_name>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },
      "method": "<method-type>",
      "path": "/<api-operation>",
      "retryPolicy": { "<retry-behavior>" },
      "queries": { "<query-parameters>" }
   },
   "recurrence": { 
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*APIConnection_trigger_name*> | String | De naam van de trigger | 
| <*naam van de verbinding*> | String | De naam voor de verbinding met de beheerde API die gebruikmaakt van de werkstroom | 
| <*type methode*> | String | De HTTP-methode om te communiceren met de beheerde API: 'OPHALEN', 'PLAATS', 'POST', "PATCH", "VERWIJDEREN" | 
| <*api-operation*> | String | De API-bewerking aan te roepen | 
| <*time-unit*> | String | De tijdseenheid waarin wordt beschreven hoe vaak de trigger wordt geactiveerd: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month' | 
| <*nummer van de tijd eenheden*> | Geheel getal | Een waarde die aangeeft hoe vaak de trigger wordt geactiveerd op basis van de frequentie, het aantal tijdseenheden is moet worden gewacht tot het opnieuw door de trigger wordt geactiveerd <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequency 'Maand' is, is het terugkeerpatroon van de zes maanden. | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de API-aanroepen. Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` bij de aanroep. | 
| <*max-runs*> | Geheel getal | Standaard logic app workflow-exemplaren worden uitgevoerd op hetzelfde moment of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Geheel getal | Als uw logische app al het maximum aantal exemplaren wordt uitgevoerd, die u kunt wijzigen op basis van de `runtimeConfiguration.concurrency.runs` eigenschap, een nieuwe uitvoeringen worden aangeboden in deze wachtrij wordt geplaatst op de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | 
| <*splitOn-expression*> | String | Voor triggers die matrices retourneren, is deze expressie verwijst naar de matrix te gebruiken zodat u kunt maken en een werkstroomexemplaar voor elk matrixitem wordt uitgevoerd, in plaats van een lus 'voor elke' gebruiken. <p>Deze expressie vertegenwoordigt bijvoorbeeld een item in de matrix die in de hoofdtekst van de trigger: `@triggerbody()?['value']` |
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). |
||||

*Uitvoer*
 
| Element | Type | Description |
|---------|------|-------------|
| Headers | JSON-Object | De headers van de reactie |
| hoofdtekst | JSON-Object | De hoofdtekst van het antwoord |
| Statuscode | Geheel getal | De statuscode van het antwoord |
|||| 

*Voorbeeld*

Deze definitie van de trigger wordt elke dag in het postvak in voor een Office 365 Outlook-account voor e-mailbericht gecontroleerd: 

```json
"When_a_new_email_arrives": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "get",
      "path": "/Mail/OnNewEmail",
      "queries": {
          "fetchOnlyWithAttachment": false,
          "folderPath": "Inbox",
          "importance": "Any",
          "includeAttachments": false
      }
   },
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

<a name="apiconnectionwebhook-trigger"></a>

### <a name="apiconnectionwebhook-trigger"></a>ApiConnectionWebhook trigger

Deze trigger een aanvraag verzendt naar een eindpunt met behulp van een [Microsoft beheerde API](../connectors/apis-list.md), biedt een *URL voor terugbellen* waar het eindpunt kunt verzenden een reactie en wacht op het eindpunt om te reageren. Zie voor meer informatie, [eindpunt abonnementen](#subscribe-unsubscribe).

```json
"<ApiConnectionWebhook_trigger_name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "body": {
          "NotificationUrl": "@{listCallbackUrl()}"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },
      "retryPolicy": { "<retry-behavior>" },
      "queries": "<query-parameters>"
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-run-queue>
      }
   },
   "splitOn": "<splitOn-expression>",
   "operationOptions": "<operation-option>"
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*naam van de verbinding*> | String | De naam voor de verbinding met de beheerde API die gebruikmaakt van de werkstroom | 
| <*body-content*> | JSON-Object | Alle inhoud van het bericht te verzenden als de nettolading van de beheerde API | 
|||| 

*Optioneel*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de API-aanroep <p>Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` bij de aanroep. | 
| <*max-runs*> | Geheel getal | Standaard logic app workflow-exemplaren worden uitgevoerd op hetzelfde moment of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Geheel getal | Als uw logische app al het maximum aantal exemplaren wordt uitgevoerd, die u kunt wijzigen op basis van de `runtimeConfiguration.concurrency.runs` eigenschap, een nieuwe uitvoeringen worden aangeboden in deze wachtrij wordt geplaatst op de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | 
| <*splitOn-expression*> | String | Voor triggers die matrices retourneren, is deze expressie verwijst naar de matrix te gebruiken zodat u kunt maken en een werkstroomexemplaar voor elk matrixitem wordt uitgevoerd, in plaats van een lus 'voor elke' gebruiken. <p>Deze expressie vertegenwoordigt bijvoorbeeld een item in de matrix die in de hoofdtekst van de trigger: `@triggerbody()?['value']` |
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

*Voorbeeld*

Deze definitie van de trigger zich abonneert op de Office 365 Outlook-API, biedt een callback-URL voor de API-eindpunt en wacht tot het eindpunt om te reageren wanneer een nieuwe e-mail binnenkomt.

```json
"When_a_new_email_arrives_(webhook)": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "body": {
         "NotificationUrl": "@{listCallbackUrl()}" 
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "path": "/MailSubscription/$subscriptions",
      "queries": {
          "folderPath": "Inbox",
          "hasAttachment": "Any",
          "importance": "Any"
      }
   },
   "splitOn": "@triggerBody()?['value']"
}
```

<a name="http-trigger"></a>

### <a name="http-trigger"></a>HTTP-trigger

Deze trigger wordt gecontroleerd of het opgegeven eindpunt op basis van de opgegeven terugkeerschema worden opgevraagd. Reactie van het eindpunt wordt bepaald of de werkstroom wordt uitgevoerd.

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<endpoint-URL>",
      "headers": { "<header-content>" },
      "body": "<body-content>",
      "authentication": { "<authentication-method>" },
      "retryPolicy": { "<retry-behavior>" },
      "queries": "<query-parameters>"
   },
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*type methode*> | String | De HTTP-methode moet worden gebruikt voor het opvragen van configuratiegegevens bij het opgegeven eindpunt: 'OPHALEN', 'PLAATS', 'POST', "PATCH", "VERWIJDEREN" | 
| <*endpoint-URL*> | String | De HTTP of HTTPS-URL voor het eindpunt te peilen <p>Maximale grootte van tekenreeks: 2 KB | 
| <*time-unit*> | String | De tijdseenheid waarin wordt beschreven hoe vaak de trigger wordt geactiveerd: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month' | 
| <*nummer van de tijd eenheden*> | Geheel getal | Een waarde die aangeeft hoe vaak de trigger wordt geactiveerd op basis van de frequentie, het aantal tijdseenheden is moet worden gewacht tot het opnieuw door de trigger wordt geactiveerd <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequency 'Maand' is, is het terugkeerpatroon van de zes maanden. | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*header-content*> | JSON-Object | De headers met de aanvraag verzenden <p>Als u bijvoorbeeld het instellen van de taal en het type voor een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*body-content*> | String | De inhoud van het bericht te verzenden als de aanvraag-nettolading | 
| <*methode voor netwerkverificatie*> | JSON-Object | De methode van de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie, [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). Naast de Scheduler, de `authority` eigenschap wordt ondersteund. Als niet is opgegeven, is de standaardwaarde `https://login.windows.net`, maar u kunt een andere waarde, zoals`https://login.windows\-ppe.net`. |
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). |  
 <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de aanvraag <p>Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` aan de aanvraag. | 
| <*max-runs*> | Geheel getal | Standaard logic app workflow-exemplaren worden uitgevoerd op hetzelfde moment of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Geheel getal | Als uw logische app al het maximum aantal exemplaren wordt uitgevoerd, die u kunt wijzigen op basis van de `runtimeConfiguration.concurrency.runs` eigenschap, een nieuwe uitvoeringen worden aangeboden in deze wachtrij wordt geplaatst op de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | 
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

*Uitvoer*

| Element | Type | Description |
|---------|------|-------------| 
| Headers | JSON-Object | De headers van de reactie | 
| hoofdtekst | JSON-Object | De hoofdtekst van het antwoord | 
| Statuscode | Geheel getal | De statuscode van het antwoord | 
|||| 

*Vereisten voor binnenkomende aanvragen*

Als u wilt werken ook met uw logische app, moet het eindpunt in overeenstemming zijn met een specifieke triggerpatroon of het contract en herkent deze eigenschappen:  
  
| Antwoord | Vereist | Beschrijving | 
|----------|----------|-------------| 
| Statuscode | Ja | De ' 200 OK ' statuscode wordt een uitvoering gestart. Een uitvoering elke andere statuscode niet wordt gestart. | 
| De koptekst opnieuw proberen na | Nee | Het aantal seconden totdat de logische app het eindpunt opnieuw peilt | 
| Location-header | Nee | De URL om aan te roepen op basis van de volgende pollinginterval. Indien niet opgegeven, wordt de oorspronkelijke URL gebruikt. | 
|||| 

*Voorbeeld-gedrag voor verschillende aanvragen*

| Statuscode | Opnieuw proberen na | Gedrag | 
|-------------|-------------|----------|
| 200 | {geen} | Het uitvoeren van de werkstroom en klik vervolgens nogmaals controleren voor meer gegevens na het terugkeerpatroon van de gedefinieerde. | 
| 200 | 10 seconden | De werkstroom uitvoert, en vervolgens opnieuw controleren voor meer gegevens na tien seconden. |  
| 202 | 60 seconden | De werkstroom wordt niet geactiveerd. De volgende poging gebeurt in een minuut, afhankelijk van het terugkeerpatroon van de gedefinieerde. Als het terugkeerpatroon van de gedefinieerde minder dan één minuut is, wordt de koptekst opnieuw proberen na voorrang. Anders wordt wordt het terugkeerpatroon van de gedefinieerde gebruikt. | 
| 400 | {geen} | Ongeldige aanvraag, niet de werkstroom uitvoert. Als er geen `retryPolicy` is gedefinieerd, wordt het standaardbeleid gebruikt. Nadat het aantal nieuwe pogingen is bereikt, de trigger wordt gecontroleerd opnieuw voor de gegevens na het terugkeerpatroon van de gedefinieerde. | 
| 500 | {geen}| Serverfout, de werkstroom niet uitvoeren. Als er geen `retryPolicy` is gedefinieerd, wordt het standaardbeleid gebruikt. Nadat het aantal nieuwe pogingen is bereikt, de trigger wordt gecontroleerd opnieuw voor de gegevens na het terugkeerpatroon van de gedefinieerde. | 
|||| 

<a name="http-webhook-trigger"></a>

### <a name="httpwebhook-trigger"></a>HTTPWebhook trigger  

Deze trigger kunt u uw logische app aanroepbare door het maken van een eindpunt dat een abonnement registreren kunt door het aanroepen van de opgegeven eindpunt-URL. Wanneer u deze trigger in uw werkstroom maakt, maakt een uitgaande aanvraag de aanroep voor registratie van het abonnement. Op die manier kunnen de trigger kunt starten luisteren naar gebeurtenissen. Wanneer een bewerking deze trigger ongeldige maakt, maakt een uitgaande aanvraag automatisch de aanroep van het abonnement hebt geannuleerd. Zie voor meer informatie, [eindpunt abonnementen](#subscribe-unsubscribe).

U kunt ook opgeven [asynchrone limieten](#asynchronous-limits) op een **HTTPWebhook** trigger.
Gedrag van de trigger is afhankelijk van de secties die u gebruiken of weglaten. 

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<endpoint-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "retryPolicy": { "<retry-behavior>" }
         },
      },
      "unsubscribe": {
         "method": "<method-type>",
         "url": "<endpoint-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" }
      }
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

Sommige waarden, zoals <*type methode*>, zijn beschikbaar voor zowel de `"subscribe"` en `"unsubscribe"` objecten.

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*type methode*> | String | De HTTP-methode moet worden gebruikt voor de Abonnementaanvraag: 'Ophalen', 'PUT","POST","PATCH"of"Verwijderen" | 
| <*endpoint-subscribe-URL*> | String | De eindpunt-URL waar de Abonnementaanvraag verzenden | 
|||| 

*Optioneel*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*type methode*> | String | De HTTP-methode moet worden gebruikt voor de aanvraag voor annulering: 'Ophalen', 'PUT","POST","PATCH"of"Verwijderen" | 
| <*endpoint-unsubscribe-URL*> | String | De eindpunt-URL waar de aanvraag voor annulering | 
| <*body-content*> | String | Elk bericht inhoud in het abonnement of bij annulering aanvraag verzenden | 
| <*methode voor netwerkverificatie*> | JSON-Object | De methode van de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie, [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*max-runs*> | Geheel getal | Standaard logic app workflow-exemplaren worden uitgevoerd op hetzelfde moment of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Geheel getal | Als uw logische app al het maximum aantal exemplaren wordt uitgevoerd, die u kunt wijzigen op basis van de `runtimeConfiguration.concurrency.runs` eigenschap, een nieuwe uitvoeringen worden aangeboden in deze wachtrij wordt geplaatst op de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | 
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

*Uitvoer* 

| Element | Type | Description |
|---------|------|-------------| 
| Headers | JSON-Object | De headers van de reactie | 
| hoofdtekst | JSON-Object | De hoofdtekst van het antwoord | 
| Statuscode | Geheel getal | De statuscode van het antwoord | 
|||| 

*Voorbeeld*

Deze trigger maakt u een abonnement op het opgegeven eindpunt, biedt een unieke callback-URL en wacht tot technologie onlangs gepubliceerde artikelen.

```json
"HTTP_Webhook": {
   "type": "HttpWebhook",
   "inputs": {
      "subscribe": {
         "method": "POST",
         "uri": "https://pubsubhubbub.appspot.com/subscribe",
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
         }
      }
   }
}
```

<a name="recurrence-trigger"></a>

### <a name="recurrence-trigger"></a>Trigger met terugkeerpatroon  

Deze trigger wordt uitgevoerd op basis van het opgegeven terugkeerschema en biedt een eenvoudige manier voor het maken van een werkstroom regelmatig wordt uitgevoerd. 

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
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
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-runs-queue>
      }
   },
   "operationOptions": "<operation-option>"
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*time-unit*> | String | De tijdseenheid waarin wordt beschreven hoe vaak de trigger wordt geactiveerd: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month' | 
| <*nummer van de tijd eenheden*> | Geheel getal | Een waarde die aangeeft hoe vaak de trigger wordt geactiveerd op basis van de frequentie, het aantal tijdseenheden is moet worden gewacht tot het opnieuw door de trigger wordt geactiveerd <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uur: 1-12.000-uur </br>-Minuut: 1-72,000 minuten </br>-Seconde: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval 6 is en de frequency 'Maand' is, is het terugkeerpatroon van de zes maanden. | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*start-date-time-with-format-YYYY-MM-DDThh:mm:ss*> | String | De begindatum en -tijd in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone opgeeft <p>-of- <p>JJJJ-MM-ddTHH als u geen dat een tijdzone opgeeft <p>Bijvoorbeeld als u wilt dat 18 September 2017 om 14:00 uur, geeft ' 2017-09-18T14:00:00 "en geef een tijdzone, zoals 'Pacific Standard Time', of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** De begintijd moet volgen de [ISO 8601 datum tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u geen een tijdzone opgeeft, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze "Z" verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor complexe schema's, de trigger alle eerder dan de begintijd wordt niet gestart. Zie voor meer informatie over start datums en tijden [maken en plannen die regelmatig actieve taken](../connectors/connectors-native-recurrence.md). | 
| <*time-zone*> | String | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Geef de tijdzone die u wilt toepassen. | 
| <*one-or-more-hour-marks*> | Geheel getal of een matrix met gehele getallen | Als u 'Day' of 'Week' opgeeft voor `frequency`, kunt u een of meer gehele getallen van 0 tot 23 in, gescheiden door komma's, het uur van de dag waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, als u "10", "12" en "14" opgeeft, krijgt u 10 uur, 12 PM en 2 uur als de merken uur. | 
| <*one-or-more-minute-marks*> | Geheel getal of een matrix met gehele getallen | Als u 'Dag' of 'Week' opgeeft voor `frequency`, kunt u een of meer gehele getallen tussen 0 en 59, gescheiden door komma's, als de minuten van het uur waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, u kunt "30" opgeven als de minuut is ingeschakeld en met behulp van het vorige voorbeeld uur van de dag, krijgt u 10:30 uur, 12:30 PM en 14:30 uur. | 
| weekDays | Tekenreeks of tekenreeksmatrix van | Als u 'Week' opgeeft voor `frequency`, kunt u een of meer dagen, gescheiden door komma's, als u wilt uitvoeren van de werkstroom: "Maandag", "Dinsdag", "Woensdag", "Donderdag", "Vrijdag", "Zaterdag" en "Zondag" | 
| <*max-runs*> | Geheel getal | Standaard logic app workflow-exemplaren worden uitgevoerd op hetzelfde moment of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Geheel getal | Als uw logische app al het maximum aantal exemplaren wordt uitgevoerd, die u kunt wijzigen op basis van de `runtimeConfiguration.concurrency.runs` eigenschap, een nieuwe uitvoeringen worden aangeboden in deze wachtrij wordt geplaatst op de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | 
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

*Voorbeeld 1*

Deze eenvoudige terugkeertrigger wordt dagelijks uitgevoerd:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

*Voorbeeld 2*

U kunt een begindatum en tijdstip voor het starten van de trigger. Deze trigger met terugkeerpatroon wordt gestart op de opgegeven datum en vervolgens wordt dagelijks geactiveerd:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1,
      "startTime": "2017-09-18T00:00:00Z"
   }
}
```

*Voorbeeld 3*

Deze trigger met terugkeerpatroon wordt gestart op 9 September 2017 om 14:00 uur en wordt geactiveerd Wekelijks elke maandag om 10:30 uur, 12:30 PM en 14:30 uur Pacific (standaardtijd):

``` json
"Recurrence": {
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

Zie voor meer informatie en voorbeelden voor deze trigger [maken en plannen die regelmatig actieve taken](../connectors/connectors-native-recurrence.md).

<a name="request-trigger"></a>

### <a name="request-trigger"></a>Trigger voor de aanvraag

Deze trigger kunt u uw logische app aanroepbare door het maken van een eindpunt dat inkomende aanvragen kan accepteren. Geef een JSON-schema dat beschrijft en valideert de nettolading of de invoer die de trigger van de inkomende aanvraag ontvangt voor deze trigger. Het schema ook eenvoudiger trigger-eigenschappen om te verwijzen naar van latere acties in de werkstroom. 

Voor het aanroepen van deze trigger, moet u de `listCallbackUrl` API, die wordt beschreven in de [werkstroom Service REST API](https://docs.microsoft.com/rest/api/logic/workflows). Zie voor meer informatie over het gebruik van deze trigger als een HTTP-eindpunt, [aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten](../logic-apps/logic-apps-http-endpoint.md).

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "<method-type>",
      "relativePath": "<relative-path-for-accepted-parameter>",
      "schema": {
         "type": "object",
         "properties": { 
            "<property-name>": {
               "type": "<property-type>"
            }
         },
         "required": [ "<required-properties>" ]
      }
   },
   "runTimeConfiguration": {
      "concurrency": {
         "runs": <max-runs>,
         "maximumWaitingRuns": <max-run-queue>
      },
   },
   "operationOptions": "<operation-option>"
}
```

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*property-name*> | String | De naam van een eigenschap in het JSON-schema, waarin wordt beschreven van de nettolading | 
| <*property-type*> | String | Van de eigenschap type | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*type methode*> | String | De methode die inkomende aanvragen gebruiken moeten voor uw logische app aanroepen: 'OPHALEN', 'PLAATS', 'POST', "PATCH", "VERWIJDEREN" |
| <*relative-path-for-accepted-parameter*> | String | Het relatieve pad voor de parameter van uw eindpunt-URL kan accepteren | 
| <*required-properties*> | Matrix | Een of meer eigenschappen waarvoor waarden | 
| <*max-runs*> | Geheel getal | Standaard logic app workflow-exemplaren worden uitgevoerd op hetzelfde moment of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency). | 
| <*max-runs-queue*> | Geheel getal | Als uw logische app al het maximum aantal exemplaren wordt uitgevoerd, die u kunt wijzigen op basis van de `runtimeConfiguration.concurrency.runs` eigenschap, een nieuwe uitvoeringen worden aangeboden in deze wachtrij wordt geplaatst op de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | 
| <*operation-option*> | String | U kunt het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

*Voorbeeld*

Deze trigger geeft aan dat een binnenkomende aanvraag de HTTP POST-methode gebruiken moet voor het aanroepen van de trigger en bevat een schema dat invoer van de inkomende aanvraag worden gevalideerd: 

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "POST",
      "schema": {
         "type": "object",
         "properties": {
            "customerName": {
               "type": "String"
            },
            "customerAddress": { 
               "type": "Object",
               "properties": {
                  "streetAddress": {
                     "type": "string"
                  },
                  "city": {
                     "type": "string"
                  }
               }
            }
         }
      }
   }
}
```

<a name="trigger-conditions"></a>

## <a name="trigger-conditions"></a>Activeringsvoorwaarden

Voor een trigger, en alleen triggers, kunt u een matrix met een of meer expressies voor voorwaarden vaststelt die bepalen of de werkstroom moet worden uitgevoerd opnemen. Om toe te voegen de `conditions` eigenschap met een trigger in uw logische app, opent u uw logische app in de weergave-editor.

U kunt bijvoorbeeld opgeven dat een trigger wordt geactiveerd wanneer een website retourneert alleen een interne serverfout door te verwijzen naar de statuscode van de trigger in de `conditions` eigenschap:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Hour",
      "interval": 1
   },
   "conditions": [ {
      "expression": "@equals(triggers().code, 'InternalServerError')"
   } ]
}
```

Standaard een trigger wordt geactiveerd alleen nadat ze een ' 200 OK ' antwoord. Als een expressie verwijst naar de statuscode van een trigger, wordt het standaardgedrag van de trigger wordt vervangen. Dus als u wilt dat de trigger wordt geactiveerd voor meer dan één statuscode, zoals de '200' en "201" statuscode, moet u deze expressie als de voorwaarde opnemen: 

`@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="trigger-multiple-runs"></a>Verschillende runs activeren

Als de trigger een matrix voor uw logische app retourneert te verwerken, soms een lus 'voor elke' te lang zou duren voor het verwerken van elk matrixitem. In plaats daarvan kunt u de **SplitOn** eigenschap in de trigger op *debatch* de matrix. Debatching splitst de matrixitems en start een nieuwe logische app-exemplaar, die wordt uitgevoerd voor elk matrixitem. Deze aanpak is bijvoorbeeld handig als u wilt pollen van een eindpunt dat meerdere nieuwe items tussen de pollingintervallen kan retourneren.
Voor het maximum aantal matrix-items die **SplitOn** kan verwerken in een enkele logische app uitvoeren, Zie [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). 

> [!NOTE]
> U kunt geen gebruiken **SplitOn** met een synchrone reactie-patroon. Elke werkstroom die gebruikmaakt van **SplitOn** en een antwoord bevat actie wordt asynchroon uitgevoerd en onmiddellijk een `202 ACCEPTED` antwoord.

Als de Swagger-bestand van de trigger een nettolading die is een matrix, beschrijft de **SplitOn** eigenschap wordt automatisch toegevoegd aan de trigger. Anders wordt deze eigenschap in de nettolading van de reactie met de matrix die u wilt debatch toevoegen. 

*Voorbeeld*

Stel dat u hebt een API die wordt dit antwoord geretourneerd: 
  
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
 
Uw logische app hoeft alleen de inhoud van de matrix in `Rows`, zodat u een trigger zoals in dit voorbeeld kunt maken:

``` json
"HTTP_Debatch": {
   "type": "Http",
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
   "recurrence": {
      "frequency": "Second",
      "interval": 1
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> Als u de `SplitOn` opdracht, u de eigenschappen die zich buiten de matrix niet ophalen. Zodat u niet voor dit voorbeeld ophalen de `status` eigenschap in het antwoord geretourneerd door de API.
> 
> Om te voorkomen dat een fout als de `Rows` eigenschap bestaat niet, in dit voorbeeld wordt de `?` operator.

De werkstroomdefinitie van de kan nu gebruikmaken van `@triggerBody().name` om op te halen de `name` -waarden die `"customer-name-one"` van de eerste uitvoering en `"customer-name-two"` van de tweede uitvoeren. Dus de trigger voert bekijken, zoals deze voorbeelden:

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

<a name="actions-overview"></a>

## <a name="actions-overview"></a>Overzicht van acties

Met Azure Logic Apps biedt verschillende actietypen - allemaal zijn voorzien van verschillende soorten invoer die uniek gedrag van een actie definiëren. 

Acties hebben deze elementen op hoog niveau, hoewel sommige optioneel zijn:

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { 
      "<input-name>": { "<input-value>" },
      "retryPolicy": "<retry-behavior>" 
   },
   "runAfter": { "<previous-trigger-or-action-status>" },
   "runtimeConfiguration": { "<runtime-config-options>" },
   "operationOptions": "<operation-option>"
},
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------|
| <*naam van de actie*> | String | De naam voor de actie | 
| <*action-type*> | String | Het actietype, bijvoorbeeld 'Http' of "ApiConnection"| 
| <*input-name*> | String | De naam voor de invoer die van de actie gedrag definieert | 
| <*input-value*> | Verschillende | De invoerwaarde, dit kan een tekenreeks, geheel getal, JSON-object, enzovoort | 
| <*previous-trigger-or-action-status*> | JSON-Object | De naam en de resulterende status voor de trigger of actie die moet worden uitgevoerd onmiddellijk voordat deze huidige actie kan worden uitgevoerd | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------|
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, beleid voor opnieuw proberen. | 
| <*runtime-config-options*> | JSON-Object | Voor sommige acties, kunt u gedrag van de actie wijzigen tijdens de uitvoering door in te stellen `runtimeConfiguration` eigenschappen. Zie voor meer informatie, [Runtime-configuratie-instellingen](#runtime-config-options). | 
| <*operation-option*> | String | Voor sommige acties, kunt u het standaardgedrag wijzigen door in te stellen de `operationOptions` eigenschap. Zie voor meer informatie, [bewerkingsopties](#operation-options). | 
|||| 

## <a name="action-types-list"></a>Lijst met typen

Hier volgen enkele veelgebruikte actietypen: 

* [Ingebouwde actietypen](#built-in-actions) zoals deze voorbeelden en meer: 

  * [**HTTP** ](#http-action) voor het aanroepen van eindpunten via HTTP of HTTPS

  * [**Antwoord** ](#response-action) voor het reageren op aanvragen

  * [**Functie** ](#function-action) voor het aanroepen van Azure Functions

  * Gegevens bewerking acties zoals [ **Join**](#join-action), [ **opstellen**](#compose-action), [ **tabel** ](#table-action), [ **Selecteer**](#select-action), en anderen die maken of transformeren van gegevens uit verschillende invoer

  * [**Werkstroom** ](#workflow-action) voor het aanroepen van een andere werkstroom voor logische Apps

* [Beheerde API-actietypen](#managed-api-actions) zoals [ **ApiConnection** ](#apiconnection-action) en [ **ApiConnectionWebHook** ](#apiconnectionwebhook-action) die verschillende aanroepen connectors en API's die worden beheerd door Microsoft, bijvoorbeeld, Azure Service Bus, Office 365 Outlook, Power BI, Azure Blob-opslag, OneDrive, GitHub en meer

* [Beheren van de werkstroom actietypen](#control-workflow-actions) zoals [ **als**](#if-action), [ **Foreach**](#foreach-action), [ **Switch**  ](#switch-action), [ **Bereik**](#scope-action), en [ **totdat**](#until-action), die andere acties bevatten en u helpen indelen van uitvoering

<a name="built-in-actions"></a>

### <a name="built-in-actions"></a>Ingebouwde acties

| Actietype | Description | 
|-------------|-------------| 
| [**Opstellen**](#compose-action) | Maakt een enkele van de uitvoer van de invoer, die verschillende typen kunnen hebben. | 
| [**Function**](#function-action) | Een Azure Function aanroept. | 
| [**HTTP**](#http-action) | Hiermee wordt een HTTP-eindpunt. | 
| [**Koppelen**](#join-action) | Een tekenreeks van de items in een matrix maakt en deze items worden gescheiden met een opgegeven scheidingsteken. | 
| [**JSON parseren**](#parse-json-action) | Maakt gebruiksvriendelijke tokens van eigenschappen in JSON-inhoud. U kunt vervolgens verwijzen naar deze eigenschappen door op te nemen van de tokens in uw logische app. | 
| [**Query’s uitvoeren**](#query-action) | Hiermee maakt u een matrix van items in een andere matrix op basis van een voorwaarde of filteren. | 
| [**Antwoord**](#response-action) | Hiermee maakt u een reactie op een binnenkomende oproep of een aanvraag. | 
| [**Selecteer**](#select-action) | Maakt een matrix met JSON-objecten met het transformeren van items uit een andere matrix op basis van de opgegeven map. | 
| [**Tabel**](#table-action) | Hiermee maakt u een CSV of HTML-tabel uit een matrix. | 
| [**Beëindigen**](#terminate-action) | Hiermee stopt u een actief actieve werkstroom. | 
| [**Wait**](#wait-action) | Hiermee wordt de werkstroom onderbroken gedurende een opgegeven periode of totdat de opgegeven datum en tijd. | 
| [**Werkstroom**](#workflow-action) | Genest in een werkstroom in een andere werkstroom. | 
||| 

<a name="managed-api-actions"></a>

### <a name="managed-api-actions"></a>Beheerde API-acties

| Actietype | Description | 
|-------------|-------------|  
| [**ApiConnection**](#apiconnection-action) | Een HTTP-eindpunt aanroept met behulp van een [Microsoft beheerde API](../connectors/apis-list.md). | 
| [**ApiConnectionWebhook**](#apiconnectionwebhook-action) | Werkt hetzelfde als HTTP-Webhook, maar maakt gebruik van een [Microsoft beheerde API](../connectors/apis-list.md). | 
||| 

<a name="control-workflow-actions"></a>

### <a name="control-workflow-actions"></a>Besturingselement werkstroomacties

Deze acties kunnen u bepalen de uitvoering en andere acties opnemen. Van buiten een besturingselement werkstroomactie, u kunt rechtstreeks verwijzen naar acties in de werkstroomactie van dat besturingselement. Als u hebt bijvoorbeeld een `Http` actie binnen een scope, kunt u verwijzen naar de `@body('Http')` expressie overal in de werkstroom. Echter kunnen acties die aanwezig zijn in de werkstroomactie van een besturingselement alleen "uitgevoerd na' andere acties die zich in dezelfde structuur van de werkstroom besturingselement.

| Actietype | Description | 
|-------------|-------------| 
| [**ForEach**](#foreach-action) | Dezelfde acties worden uitgevoerd in een lus voor elk item in een matrix. | 
| [**Als**](#if-action) | Voer acties op basis van of de opgegeven voorwaarde is true of false. | 
| [**Bereik**](#scope-action) | Acties op basis van de groepsstatus van de van een set acties worden uitgevoerd. | 
| [**Switch**](#switch-action) | Uitvoeren van acties die zijn ingedeeld in gevallen wanneer de opgegeven waarden van elke aanvraag overeenkomen met de waarden van expressies, objecten of -tokens. | 
| [**Tot**](#until-action) | Acties in een lus uitgevoerd totdat de opgegeven voorwaarde waar is. | 
|||  

## <a name="actions---detailed-reference"></a>Acties - uitgebreide referentie

<a name="apiconnection-action"></a>

### <a name="apiconnection-action"></a>APIConnection actie

Met deze actie verzendt een HTTP-aanvraag naar een [Microsoft beheerde API](../connectors/apis-list.md) en informatie over de API en parameters plus een verwijzing naar een geldige verbinding vereist. 

``` json
"<action-name>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<api-name>']['connectionId']"
         },
         "<other-action-specific-input-properties>"        
      },
      "method": "<method-type>",
      "path": "/<api-operation>",
      "retryPolicy": "<retry-behavior>",
      "queries": { "<query-parameters>" },
      "<other-action-specific-properties>"
    },
    "runAfter": {}
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*naam van de actie*> | String | De naam van de actie die is opgegeven door de connector | 
| <*api-name*> | String | De naam van de Microsoft beheerde API die wordt gebruikt voor de verbinding | 
| <*type methode*> | String | De HTTP-methode voor het aanroepen van de API: 'Ophalen', 'PUT","POST","PATCH"of"Verwijderen" | 
| <*api-operation*> | String | De API-bewerking aan te roepen | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*other-action-specific-input-properties*> | JSON-Object | Andere invoer eigenschappen die betrekking hebben op deze specifieke actie | 
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de API-aanroepen. <p>Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` bij de aanroep. | 
| <*andere-actie-specifieke-properties*> | JSON-Object | Andere eigenschappen die betrekking hebben op deze specifieke actie | 
|||| 

*Voorbeeld*

Deze definitie beschrijft de **een e-mailbericht verzenden** actie voor Office 365 Outlook-connector een beheerd door Microsoft API is: 

```json
"Send_an_email": {
   "type": "ApiConnection",
   "inputs": {
      "body": {
         "Body": "Thank you for your membership!",
         "Subject": "Hello and welcome!",
         "To": "Sophie.Owen@contoso.com"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "POST",
      "path": "/Mail"
    },
    "runAfter": {}
}
```

<a name="apiconnection-webhook-action"></a>

### <a name="apiconnectionwebhook-action"></a>APIConnectionWebhook actie

Deze actie verzendt een aanvraag via HTTP naar een eindpunt met behulp van een [Microsoft beheerde API](../connectors/apis-list.md), biedt een *URL voor terugbellen* waar het eindpunt kunt verzenden een reactie en wacht op het eindpunt op reageren op. Zie voor meer informatie, [eindpunt abonnementen](#subscribe-unsubscribe).

```json
"<action-name>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "subscribe": {
         "method": "<method-type>",
         "uri": "<api-subscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "retryPolicy": "<retry-behavior>",
         "queries": { "<query-parameters>" },
         "<other-action-specific-input-properties>"
      },
      "unsubscribe": {
         "method": "<method-type>",
         "uri": "<api-unsubscribe-URL>",
         "headers": { "<header-content>" },
         "body": "<body-content>",
         "authentication": { "<authentication-method>" },
         "<other-action-specific-properties>"
      },
   },
   "runAfter": {}
}
```

Sommige waarden, zoals <*type methode*>, zijn beschikbaar voor zowel de `"subscribe"` en `"unsubscribe"` objecten.

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*naam van de actie*> | String | De naam van de actie die is opgegeven door de connector | 
| <*type methode*> | String | De HTTP-methode moet worden gebruikt voor het abonnement of abonnement opzegt op een eindpunt: 'Ophalen', 'PUT","POST","PATCH"of"Verwijderen" | 
| <*api-subscribe-URL*> | String | De URI die moet worden gebruikt voor het abonneren op de API | 
|||| 

*Optioneel*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*api-unsubscribe-URL*> | String | De URI moet worden gebruikt voor het abonnement opzegt op de API | 
| <*header-content*> | JSON-Object | Kopteksten in de aanvraag verzenden <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*body-content*> | JSON-Object | Alle inhoud van het bericht in de aanvraag verzenden | 
| <*methode voor netwerkverificatie*> | JSON-Object | De methode van de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie, [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de API-aanroep <p>Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` bij de aanroep. | 
| <*other-action-specific-input-properties*> | JSON-Object | Andere invoer eigenschappen die betrekking hebben op deze specifieke actie | 
| <*andere-actie-specifieke-properties*> | JSON-Object | Andere eigenschappen die betrekking hebben op deze specifieke actie | 
|||| 

U kunt ook beperkingen opleggen op een **ApiConnectionWebhook** actie op dezelfde manier als [HTTP asynchrone limieten](#asynchronous-limits).

<a name="compose-action"></a>

### <a name="compose-action"></a>Actie opstellen

Deze actie maakt een enkele van de uitvoer van meerdere invoergegevens, met inbegrip van expressies. De uitvoer en de invoer kunnen geen type dat Azure Logic Apps systeemeigen ondersteuning, zoals matrices, JSON-objecten, XML en binaire biedt hebben.
U kunt vervolgens de uitvoer van de actie in andere acties. 

```json
"Compose": {
   "type": "Compose",
   "inputs": "<inputs-to-compose>",
   "runAfter": {}
},
```

*Vereist* 

| Value | Type | Description | 
|-------|------|-------------| 
| <*inputs-to-compose*> | Alle | De invoer voor het maken van een enkele uitvoer | 
|||| 

*Voorbeeld 1*

<!-- markdownlint-disable MD038 -->
De definitie van deze actie worden samengevoegd `abcdefg ` met een spatie en de waarde `1234`:
<!-- markdownlint-enable MD038 -->

```json
"Compose": {
   "type": "Compose",
   "inputs": "abcdefg 1234",
   "runAfter": {}
},
```

Hier volgt de uitvoer die deze actie wordt gemaakt:

`abcdefg 1234`

*Voorbeeld 2*

De definitie van deze actie voegt een string-variabele die bevat `abcdefg` en een geheel getal variabele die bevat `1234`:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@{variables('myString')}@{variables('myInteger')}",
   "runAfter": {}
},
```

Hier volgt de uitvoer die deze actie wordt gemaakt:

`"abcdefg1234"`

<a name="function-action"></a>

### <a name="function-action"></a>Functie-actie

Deze actie wordt een eerder gemaakte [Azure-functie](../azure-functions/functions-create-first-azure-function.md).

```json
"<Azure-function-name>": {
   "type": "Function",
   "inputs": {
     "function": {
        "id": "<Azure-function-ID>"
      },
      "method": "<method-type>",
      "headers": { "<header-content>" },
      "body": { "<body-content>" },
      "queries": { "<query-parameters>" } 
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------|  
| <*Azure-function-ID*> | String | De resource-ID voor de Azure-functie die u wilt aanroepen. Hier volgt de indeling voor deze waarde:<p>"/subscriptions/<*Azure-subscription-ID*>/resourceGroups/<*Azure-resource-group*>/providers/Microsoft.Web/sites/<*Azure-function-app-name*>/functions/<*Azure-function-name*>" | 
| <*type methode*> | String | De HTTP-methode moet worden gebruikt voor het aanroepen van de functie: 'Ophalen', 'PUT","POST","PATCH"of"Verwijderen" <p>Indien niet opgegeven, is de standaardinstelling de methode 'POST'. | 
||||

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------|  
| <*header-content*> | JSON-Object | Kopteksten te verzenden met de aanroep <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*body-content*> | JSON-Object | Alle inhoud van het bericht in de aanvraag verzenden | 
| <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de API-aanroep <p>Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` bij de aanroep. | 
| <*other-action-specific-input-properties*> | JSON-Object | Andere invoer eigenschappen die betrekking hebben op deze specifieke actie | 
| <*andere-actie-specifieke-properties*> | JSON-Object | Andere eigenschappen die betrekking hebben op deze specifieke actie | 
||||

Wanneer u uw logische app opslaat, voert de Logic Apps-engine deze controles op de waarnaar wordt verwezen, functie:

* De werkstroom moet toegang hebben tot de functie.

* Uw werkstroom kunt alleen een standaard HTTP-trigger of een algemene webhook-trigger JSON. 

  De Logic Apps-engine opgehaald en in cache opgeslagen URL op van de trigger, die wordt gebruikt tijdens runtime. Echter, als een bewerking, wordt de URL in de cache ongeldig de **functie** actie is mislukt tijdens runtime. Om op te lossen dit probleem, sla de logische app opnieuw op zodat de logische app opgehaald en de URL van de trigger opnieuw de cache opslaat.

* De functie kan niet een route gedefinieerd hebben.

* Alleen 'de functie' en 'anoniem' machtigingsniveaus zijn toegestaan. 

*Voorbeeld*

De definitie van deze actie wordt de eerder gemaakte 'GetProductID'-functie:

```json
"GetProductID": {
   "type": "Function",
   "inputs": {
     "function": {
        "id": "/subscriptions/<XXXXXXXXXXXXXXXXXXXX>/resourceGroups/myLogicAppResourceGroup/providers/Microsoft.Web/sites/InventoryChecker/functions/GetProductID"
      },
      "method": "POST",
      "headers": { 
          "x-ms-date": "@utcnow()"
       },
      "body": { 
          "Product_ID": "@variables('ProductID')"
      }
   },
   "runAfter": {}
}
```

<a name="http-action"></a>

### <a name="http-action"></a>HTTP-actie

Met deze actie wordt een aanvraag verzonden naar het opgegeven eindpunt en controleert de reactie om te bepalen of de werkstroom moet worden uitgevoerd. 

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "<method-type>",
      "uri": "<HTTP-or-HTTPS-endpoint-URL>"
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*type methode*> | String | De methode die u wilt gebruiken voor het verzenden van de aanvraag: 'Ophalen', 'PUT","POST","PATCH"of"Verwijderen" | 
| <*HTTP-or-HTTPS-endpoint-URL*> | String | De HTTP of HTTPS-eindpunt om aan te roepen. Maximale grootte van tekenreeks: 2 KB | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*header-content*> | JSON-Object | Kopteksten met de aanvraag verzenden <p>Als u bijvoorbeeld het instellen van de taal en het type: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` |
| <*body-content*> | JSON-Object | Alle inhoud van het bericht in de aanvraag verzenden | 
| <*gedrag voor opnieuw proberen*> | JSON-Object | Hiermee past u het gedrag voor opnieuw proberen voor onregelmatige fouten, waarvoor de 408, 429, en 5XX-statuscode en eventuele verbindingsuitzonderingen. Zie voor meer informatie, [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies). | 
| <*query-parameters*> | JSON-Object | Alle queryparameters moeten worden opgenomen met de aanvraag <p>Bijvoorbeeld, de `"queries": { "api-version": "2018-01-01" }` object toevoegen `?api-version=2018-01-01` bij de aanroep. | 
| <*other-action-specific-input-properties*> | JSON-Object | Andere invoer eigenschappen die betrekking hebben op deze specifieke actie | 
| <*andere-actie-specifieke-properties*> | JSON-Object | Andere eigenschappen die betrekking hebben op deze specifieke actie | 
|||| 

*Voorbeeld*

De definitie van deze actie wordt het laatste nieuws op een aanvraag verzenden naar het opgegeven eindpunt:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest"
   }
}
```

<a name="join-action"></a>

### <a name="join-action"></a>Actie toevoegen

Met deze actie wordt een tekenreeks van de items in een matrix gemaakt en deze items worden gescheiden met het opgegeven scheidingsteken. 

```json
"Join": {
   "type": "Join",
   "inputs": {
      "from": <array>,
      "joinWith": "<delimiter>"
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*Matrix*> | Matrix | De ingevoerde matrix of een expressie waarmee de bronitems. Als u een expressie opgeven, plaatst u deze expressie met dubbele aanhalingstekens. | 
| <*delimiter*> | Eén tekenreeks | Het teken dat elk item in de tekenreeks worden gescheiden | 
|||| 

*Voorbeeld*

Stel dat u hebt een eerder gemaakte "myIntegerArray"-variabele die deze matrix met gehele getallen bevat: 

`[1,2,3,4]` 

De definitie van deze actie haalt de waarden van de variabele met behulp van de `variables()` functioneren in een expressie en maakt deze tekenreeks met deze waarden, die worden gescheiden door een door komma's: `"1,2,3,4"`

```json
"Join": {
   "type": "Join",
   "inputs": {
      "from": "@variables('myIntegerArray')",
      "joinWith": ","
   },
   "runAfter": {}
}
```

<a name="parse-json-action"></a>

### <a name="parse-json-action"></a>De actie JSON parseren

Deze actie wordt gemaakt van gebruiksvriendelijke velden of *tokens* vanuit de eigenschappen in JSON-inhoud. U kunt vervolgens toegang tot deze eigenschappen in uw logische app met behulp van de tokens in plaats daarvan. Bijvoorbeeld, als u gebruiken JSON-uitvoer van services zoals Azure Service Bus en Azure Cosmos DB wilt, kunt u opnemen met deze actie in uw logische app zodat u kunt eenvoudig kunt verwijzen naar de gegevens in die uitvoer. 

```json
"Parse_JSON": {
   "type": "ParseJson",
   "inputs": {
      "content": "<JSON-source>",
         "schema": { "<JSON-schema>" }
      },
      "runAfter": {}
},
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*JSON-source*> | JSON-Object | De JSON-inhoud die u wilt parseren | 
| <*JSON-schema*> | JSON-Object | De JSON-schema dat de onderliggende de JSON-inhoud, waardoor de actie wordt gebruikt beschrijft voor het parseren van de bron-JSON-inhoud. <p>**Tip**: In Logic Apps Designer kunt u het schema opgeven of geven een voorbeeld van payload zodat het schema kan worden gegenereerd door de actie. | 
|||| 

*Voorbeeld*

De definitie van deze actie wordt gemaakt met deze tokens die u in uw werkstroom voor logische Apps, maar alleen in acties of Voer de volgende gebruiken kunt de **JSON parseren** actie: 

`FirstName`, `LastName`, en `Email`

```json
"Parse_JSON": {
   "type": "ParseJson",
   "inputs": {
      "content": {
         "Member": {
            "Email": "Sophie.Owen@contoso.com",
            "FirstName": "Sophie",
            "LastName": "Owen"
         }
      },
      "schema": {
         "type": "object",
         "properties": {
            "Member": {
               "type": "object",
               "properties": {
                  "Email": {
                     "type": "string"
                  },
                  "FirstName": {
                     "type": "string"
                  },
                  "LastName": {
                     "type": "string"
                  }
               }
            }
         }
      }
   },
   "runAfter": { }
},
```

In dit voorbeeld wordt de eigenschap "materiaal" Hiermee geeft u de JSON-inhoud voor de actie die moet worden geparseerd. U kunt ook deze JSON-inhoud opgeven als het voorbeeld van payload voor het genereren van het schema.

```json
"content": {
   "Member": { 
      "FirstName": "Sophie",
      "LastName": "Owen",
      "Email": "Sophie.Owen@contoso.com"
   }
},
```

De eigenschap 'schema' Hiermee geeft u het JSON-schema gebruikt voor het beschrijven van de JSON-inhoud:

```json
"schema": {
   "type": "object",
   "properties": {
      "Member": {
         "type": "object",
         "properties": {
            "FirstName": {
               "type": "string"
            },
            "LastName": {
               "type": "string"
            },
            "Email": {
               "type": "string"
            }
         }
      }
   }
}
```

<a name="query-action"></a>

### <a name="query-action"></a>Queryactie.

Deze actie wordt een matrix van items in een andere matrix op basis van een opgegeven voorwaarde of filter gemaakt.

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": <array>,
      "where": "<condition-or-filter>"
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*Matrix*> | Matrix | De ingevoerde matrix of een expressie waarmee de bronitems. Als u een expressie opgeven, plaatst u deze expressie met dubbele aanhalingstekens. |
| <*voorwaarde-of-filter*> | String | De voorwaarde die wordt gebruikt voor het filteren van items in de bronmatrix <p>**Opmerking**: Als er geen waarden voldoen aan de voorwaarde, klikt u vervolgens de actie een lege matrix wordt gemaakt. |
|||| 

*Voorbeeld*

De definitie van deze actie wordt een matrix met waarden die groter is dan de opgegeven waarde, die twee gemaakt:

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": [ 1, 3, 0, 5, 4, 2 ],
      "where": "@greater(item(), 2)"
   }
}
```

<a name="response-action"></a>

### <a name="response-action"></a>Reactie  

Deze actie wordt de nettolading van het antwoord op een HTTP-aanvraag gemaakt. 

```json
"Response" {
    "type": "Response",
    "kind": "http",
    "inputs": {
        "statusCode": 200,
        "headers": { <response-headers> },
        "body": { <response-body> }
    },
    "runAfter": {}
},
```

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*response-status-code*> | Geheel getal | De HTTP-statuscode die wordt verzonden naar de inkomende aanvraag. De standaardwaarde is "200 OK ', maar de code kan geen geldige statuscode die met 2xx, 4xx of 5xx, maar niet met 3xxx begint. | 
|||| 

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------| 
| <*antwoord-headers*> | JSON-Object | Een of meer kopteksten om op te nemen met de reactie | 
| <*hoofdtekst van reactie*> | Verschillende | De antwoordtekst, dit kan een tekenreeks, JSON-object of zelfs binaire inhoud van een eerdere actie | 
|||| 

*Voorbeeld*

De definitie van deze actie wordt een reactie op een HTTP-aanvraag gemaakt met de opgegeven statuscode, de berichttekst en de berichtkoppen:

```json
"Response": {
   "type": "Response",
   "inputs": {
      "statusCode": 200,
      "body": {
         "ProductID": 0,
         "Description": "Organic Apples"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "content-type": "application/json"
      }
   },
   "runAfter": {}
}
```

*Beperkingen*

In tegenstelling tot andere acties, de **antwoord** actie bevat speciale beperkingen: 

* Uw werkstroom kunt gebruiken de **antwoord** actie alleen wanneer de werkstroom wordt gestart met een HTTP-aanvraagtrigger, wat betekent dat de werkstroom moet worden geactiveerd door een HTTP-aanvraag.

* Uw werkstroom kunt gebruiken de **antwoord** actie overal *behalve* binnen **Foreach** lussen, **totdat** lussen, met inbegrip van sequentiële lus en de parallelle vertakkingen. 

* De oorspronkelijke HTTP-aanvraag haalt de reactie van uw werkstroom alleen wanneer alle acties vereist door de **antwoord** actie zijn voltooid binnen de [HTTP-aanvraag time-outlimiet](../logic-apps/logic-apps-limits-and-config.md#request-limits).

  Echter, als een andere logische app als een geneste werkstroom wordt aangeroepen door de werkstroom, de bovenliggende werkstroom moet wachten totdat de geneste werkstroom is voltooid, ongeacht hoeveel tijd verstrijkt voordat de geneste werkstroom is voltooid.

* Wanneer de werkstroom gebruikt de **antwoord** actie en een synchrone reactie-patroon, de werkstroom ook niet gebruiken de **splitOn** opdracht in de Triggerdefinitie omdat deze opdracht maakt u meerdere wordt uitgevoerd. Controleer in dit geval als de PUT-methode wordt gebruikt, en als de waarde true, "Ongeldige aanvraag" reactie retourneren.

  Als uw werkstroom maakt gebruik van de **splitOn** opdracht en een **antwoord** actie, de werkstroom wordt asynchroon uitgevoerd en retourneert onmiddellijk een respons '202 geaccepteerd'.

* Wanneer de uitvoering van uw werkstroom bereikt de **antwoord** actie, maar de binnenkomende aanvraag heeft al een antwoord ontvangen de **antwoord** actie is gemarkeerd als 'Mislukt' vanwege het conflict. En als gevolg hiervan uw logische app ook met de status 'Mislukt' is gemarkeerd.

<a name="select-action"></a>

### <a name="select-action"></a>Actie selecteren

Deze actie wordt een matrix met JSON-objecten gemaakt door het transformeren van items uit een andere matrix op basis van de opgegeven map. De uitvoermatrix en bronmatrix hebben altijd hetzelfde aantal items. Hoewel u het aantal objecten in de uitvoermatrix niet wijzigen, kunt u toevoegen of verwijderen van de eigenschappen en hun waarden op die objecten. De `select` -eigenschap geeft op ten minste één sleutel / waarde-paar dat de kaart voor het transformeren van items in de bronmatrix te definiëren. Een sleutel / waarde-paar vertegenwoordigt een eigenschap en de waarde ervan op alle objecten in de uitvoermatrix. 

```json
"Select": {
   "type": "Select",
   "inputs": {
      "from": <array>,
      "select": { 
          "<key-name>": "<expression>",
          "<key-name>": "<expression>"        
      }
   },
   "runAfter": {}
},
```

*Vereist* 

| Value | Type | Description | 
|-------|------|-------------| 
| <*Matrix*> | Matrix | De ingevoerde matrix of een expressie waarmee de bronitems. Zorg ervoor dat u plaatst u een expressie met dubbele aanhalingstekens. <p>**Opmerking**: Als de bronmatrix leeg is, wordt in de actie wordt een lege matrix gemaakt. | 
| <*key-name*> | String | De naam van de eigenschap toegewezen aan het resultaat van <*expressie*> <p>Om toe te voegen een nieuwe eigenschap op alle objecten in de uitvoermatrix, bieden een <*sleutelnaam*> voor die eigenschap en een <*expressie*> voor de waarde van eigenschap. <p>Als u wilt verwijderen een eigenschap van alle objecten in de matrix, laat de <*sleutelnaam*> voor die eigenschap. | 
| <*expression*> | String | De expressie die het item in de bronmatrix worden getransformeerd en wijst het resultaat dat <*sleutel-naam*> | 
|||| 

De **Selecteer** actie wordt een matrix gemaakt als uitvoer, zodat een actie op die met behulp van deze uitvoer moet ofwel een matrix accepteren, of u moet de matrix converteren naar het type dat de actie consument accepteert. Bijvoorbeeld, als u wilt de uitvoermatrix converteren naar een tekenreeks, u kunt doorgeven die matrix naar de **opstellen** actie, en deze vervolgens de uitvoer van de **opstellen** actie in de andere acties.

*Voorbeeld*

Deze definitie actie maakt een JSON-object-matrix van een matrix met gehele getallen. De actie die de bronmatrix, doorloopt elke integerwaarde opgehaald met behulp van de `@item()` expressie en wijst elke waarde die moet worden de '`number`'-eigenschap in elk JSON-object: 

```json
"Select": {
   "type": "Select",
   "inputs": {
      "from": [ 1, 2, 3 ],
      "select": { 
         "number": "@item()" 
      }
   },
   "runAfter": {}
},
```

Dit is de matrix die deze actie wordt gemaakt:

`[ { "number": 1 }, { "number": 2 }, { "number": 3 } ]`

Voor het gebruik van deze matrix uitvoer in een andere acties, geven deze uitvoer in een **opstellen** actie:

```json
"Compose": {
   "type": "Compose",
   "inputs": "@body('Select')",
   "runAfter": {
      "Select": [ "Succeeded" ]
   }
},
```

Vervolgens kunt u de uitvoer van de **opstellen** actie in de andere acties, bijvoorbeeld de **Office 365 Outlook - een e-mail verzenden** actie:

```json
"Send_an_email": {
   "type": "ApiConnection",
   "inputs": {
      "body": {
         "Body": "@{outputs('Compose')}",
         "Subject": "Output array from Select and Compose actions",
         "To": "<your-email@domain>"
      },
      "host": {
         "connection": {
            "name": "@parameters('$connections')['office365']['connectionId']"
         }
      },
      "method": "post",
      "path": "/Mail"
   },
   "runAfter": {
      "Compose": [ "Succeeded" ]
   }
},
```

<a name="table-action"></a>

### <a name="table-action"></a>Tabel-actie

Deze actie wordt een CSV of HTML-tabel gemaakt uit een matrix. Voor matrices met JSON-objecten maakt deze actie automatisch de kolomkoppen van de namen van eigenschappen van de objecten. U moet de kolomkoppen en de waarden opgeven voor matrices met andere gegevenstypen. Deze matrix bevat bijvoorbeeld de eigenschappen "ID" en "Productnaam" die deze actie voor de naam van de kolom header gebruiken kunt:

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]` 

```json
"Create_<CSV | HTML>_table": {
   "type": "Table",
   "inputs": {
      "format": "<CSV | HTML>",
      "from": <array>,
      "columns": [ 
         {
            "header": "<column-name>",
            "value": "<column-value>"
         },
         {
            "header": "<column-name>",
            "value": "<column-value>"
         } 
      ]
   },
   "runAfter": {}
}
```

*Vereist* 

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| < CSV *of* HTML >| String | De indeling voor de tabel die u wilt maken | 
| <*Matrix*> | Matrix | De ingevoerde matrix of een expressie waarmee de bronitems voor de tabel <p>**Opmerking**: Als de bronmatrix leeg is, wordt in de actie wordt gemaakt van een lege tabel. | 
|||| 

*Optioneel*

Als u wilt opgeven of kolomkoppen en waarden aanpassen, gebruikt u de `columns` matrix. Wanneer `header-value` paren hebben dezelfde naam van de header, hun waarden worden weergegeven in dezelfde kolom onder de headernaam van deze. Elke unieke header geeft anders een unieke kolom.

| Value | Type | Description | 
|-------|------|-------------| 
| <*column-name*> | String | De naam van de header voor een kolom | 
| <*column-value*> | Alle | De waarde in die kolom | 
|||| 

*Voorbeeld 1*

Stel dat u een eerder gemaakte "myItemArray"-variabele hebt bevat die momenteel deze matrix: 

`[ {"ID": 0, "Product_Name": "Apples"}, {"ID": 1, "Product_Name": "Oranges"} ]`

Deze definitie actie maakt een CSV-tabel van de variabele 'myItemArray'. De expressie die wordt gebruikt door de `from` eigenschap wordt de matrix van 'myItemArray' met behulp van de `variables()` functie: 

```json
"Create_CSV_table": {
   "type": "Table",
   "inputs": {
      "format": "CSV",
      "from": "@variables('myItemArray')"
   },
   "runAfter": {}
}
```

Dit is de CSV-tabel die deze actie wordt gemaakt: 

```
ID,Product_Name 
0,Apples 
1,Oranges 
```

*Voorbeeld 2*

Deze definitie actie maakt een HTML-tabel van de variabele 'myItemArray'. De expressie die wordt gebruikt door de `from` eigenschap wordt de matrix van 'myItemArray' met behulp van de `variables()` functie: 

```json
"Create_HTML_table": {
   "type": "Table",
   "inputs": {
      "format": "HTML",
      "from": "@variables('myItemArray')"
   },
   "runAfter": {}
}
```

Dit is de HTML-tabel die deze actie wordt gemaakt: 

<table><thead><tr><th>Id</th><th>Productnaam</th></tr></thead><tbody><tr><td>0</td><td>Appels</td></tr><tr><td>1</td><td>Sinaasappels</td></tr></tbody></table>

*Voorbeeld 3*

Deze definitie actie maakt een HTML-tabel van de variabele 'myItemArray'. Echter in dit voorbeeld overschrijft de koptekst van de standaardkolomnamen met 'Stock_ID' en 'Description', en wordt het woord 'Organische' toegevoegd aan de waarden in de kolom 'Description'.

```json
"Create_HTML_table": {
   "type": "Table",
   "inputs": {
      "format": "HTML",
      "from": "@variables('myItemArray')",
      "columns": [ 
         {
            "header": "Stock_ID",
            "value": "@item().ID"
         },
         {
            "header": "Description",
            "value": "@concat('Organic ', item().Product_Name)"
         }
      ]
    },
   "runAfter": {}
},
```

Dit is de HTML-tabel die deze actie wordt gemaakt: 

<table><thead><tr><th>Stock_ID</th><th>Beschrijving</th></tr></thead><tbody><tr><td>0</td><td>Organische appels</td></tr><tr><td>1</td><td>Organische sinaasappels</td></tr></tbody></table>

<a name="terminate-action"></a>

### <a name="terminate-action"></a>Actie beëindigen

Deze actie stopt met het uitvoeren van de logische app werkstroomexemplaar, annuleert alle acties worden uitgevoerd, slaat alle resterende acties en retourneert de opgegeven status. Bijvoorbeeld, kunt u de **beëindigen** actie wanneer uw logische app volledig van een foutstatus afsluiten moet. Met deze actie niet van invloed op reeds voltooide acties en kan niet worden weergegeven in **Foreach** en **totdat** lussen, met inbegrip van opeenvolgende lussen. 

```json
"Terminate": {
   "type": "Terminate",
   "inputs": {
       "runStatus": "<status>",
       "runError": {
            "code": "<error-code-or-name>",
            "message": "<error-message>"
       }
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*status*> | String | De status voor de uitvoering: "Mislukt", "Geannuleerd" of "Voltooid" |
|||| 

*Optioneel*

De eigenschappen voor het object "runStatus" gelden alleen wanneer de eigenschap 'runStatus' is ingesteld op de status 'Mislukt'.

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*Fout-code-of-naam*> | String | De code of de naam op voor de fout |
| <*error-message*> | String | Het bericht of de tekst die de fout en acties die beschrijft de appgebruiker kan maken | 
|||| 

*Voorbeeld*

Deze definitie actie stopt een uitvoering van de werkstroom, stelt de status van de run 'Mislukt' en retourneert de status, een foutcode en een foutbericht weergegeven:

```json
"Terminate": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "Unexpected response",
            "message": "The service received an unexpected response. Please try again."
        }
   },
   "runAfter": {}
}
```

<a name="wait-action"></a>

### <a name="wait-action"></a>Actie wachten  

Deze actie onderbreekt de uitvoering van de werkstroom voor het opgegeven interval of tot de opgegeven tijd, maar niet beide. 

*Opgegeven interval*

```json
"Delay": {
   "type": "Wait",
   "inputs": {
      "interval": {
         "count": <number-of-units>,
         "unit": "<interval>"
      }
   },
   "runAfter": {}
},
```

*Opgegeven periode*

```json
"Delay_until": {
   "type": "Wait",
   "inputs": {
      "until": {
         "timestamp": "<date-time-stamp>"
      }
   },
   "runAfter": {}
},
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*aantal eenheden*> | Geheel getal | Voor de **vertraging** actie, het aantal eenheden moet worden gewacht | 
| <*interval*> | String | Voor de **vertraging** actie, het interval moet worden gewacht: 'Tweede', 'Minute', 'Uur', 'Dag', 'Week', 'Month' | 
| <*date-time-stamp*> | String | Voor de **vertraging tot** actie, de datum en tijd uitvoering hervatten. Deze waarde moet gebruiken de [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time). | 
|||| 

*Voorbeeld 1*

De definitie van deze actie wordt de werkstroom onderbroken gedurende 15 minuten:

```json
"Delay": {
   "type": "Wait",
   "inputs": {
      "interval": {
         "count": 15,
         "unit": "Minute"
      }
   },
   "runAfter": {}
},
```

*Voorbeeld 2*

De definitie van deze actie wordt de werkstroom onderbroken tot de opgegeven tijd:

```json
"Delay_until": {
   "type": "Wait",
   "inputs": {
      "until": {
         "timestamp": "2017-10-01T00:00:00Z"
      }
   },
   "runAfter": {}
},
```

<a name="workflow-action"></a>

### <a name="workflow-action"></a>Werkstroomactie

Deze actie wordt nog een eerder gemaakte logische app, wat betekent dat u kunt opnemen en andere werkstromen voor logische app opnieuw. U kunt ook de uitvoer van de onderliggende of *geneste* logische app in de acties die volgen op de geneste logische app, mits de onderliggende logische app een antwoord retourneert.

De Logic Apps-engine controleert de toegang tot de trigger die u wilt aanroepen, dus zorg ervoor dat u hebt toegang tot deze trigger. Bovendien moet de geneste logische app aan deze criteria voldoen:

* Maakt een trigger van de geneste logische app aanroepbare, zoals een [aanvragen](#request-trigger) of [HTTP](#http-trigger) trigger

* Hetzelfde Azure-abonnement als de bovenliggende logische app

* Als u de uitvoer van de geneste logische app in de bovenliggende logische app, de geneste logische app moet een [antwoord](#response-action) actie 

```json
"<nested-logic-app-name>": {
   "type": "Workflow",
   "inputs": {
      "body": { "<body-content" },
      "headers": { "<header-content>" },
      "host": {
         "triggerName": "<trigger-name>",
         "workflow": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group>/providers/Microsoft.Logic/<nested-logic-app-name>"
         }
      }
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*nested-logic-app-name*> | String | De naam van de logische app die u wilt aanroepen | 
| <*trigger-name*> | String | De naam van de trigger in de geneste logische app die u wilt aanroepen | 
| <*Azure-subscription-ID*> | String | De Azure-abonnement-ID voor de geneste logische app |
| <*Azure-resource-group*> | String | Naam van de Azure-resourcegroep voor de geneste logische app |
| <*nested-logic-app-name*> | String | De naam van de logische app die u wilt aanroepen |
||||

*Optioneel*

| Value | Type | Description | 
|-------|------|-------------|  
| <*header-content*> | JSON-Object | Kopteksten te verzenden met de aanroep | 
| <*body-content*> | JSON-Object | Alle inhoud van het bericht te verzenden met de aanroep | 
||||

*Uitvoer*

De uitvoer van deze actie afhankelijk van de reactie van de geneste logische app. Als de geneste logische app niet een reactie bevat, wordt de uitvoer is leeg.

*Voorbeeld*

Nadat de actie 'Start_search' voltooid is, wordt deze actie werkstroomdefinitie een andere logische app met de naam 'Get_product_information', wordt doorgegeven in de opgegeven invoer: 

```json
"actions": {
   "Start_search": { <action-definition> },
   "Get_product_information": {
      "type": "Workflow",
      "inputs": {
         "body": {
            "ProductID": "24601",
         },
         "host": {
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/InventoryManager-RG/providers/Microsoft.Logic/Get_product_information",
            "triggerName": "Find_product"
         },
         "headers": {
            "content-type": "application/json"
         }
      },
      "runAfter": { 
         "Start_search": [ "Succeeded" ]
      }
   }
},
```

## <a name="control-workflow-action-details"></a>Details van besturingselement werkstroom actie

<a name="foreach-action"></a>

### <a name="foreach-action"></a>Foreach-actie

Deze herhaling actie doorloopt een matrix en voert acties uit voor elk matrixitem. Standaard wordt de lus 'voor elke' tot een maximum aantal lussen parallel uitgevoerd. Zie voor dit maximum [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Informatie over [maken 'voor elke' lussen](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop).

```json
"For_each": {
   "type": "Foreach",
   "actions": { 
      "<action-1>": { "<action-definition-1>" },
      "<action-2>": { "<action-definition-2>" }
   },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": <count>
      }
    },
    "operationOptions": "<operation-option>"
}
```

*Vereist* 

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*actie-1,... n*> | String | De namen van de acties die worden uitgevoerd voor elk matrixitem | 
| <*action-definition-1...n*> | JSON-Object | De definities van de acties die worden uitgevoerd | 
| <*for-each-expression*> | String | De expressie die verwijst naar elk item in de opgegeven matrix | 
|||| 

*Optioneel*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*count*> | Geheel getal | Standaard wordt de 'voor elke'-lus iteraties op hetzelfde moment of parallel tot uitvoeren de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Deze limiet wijzigen door in te stellen een nieuwe <*aantal*> waarde, Zie [wijzigen van de lus 'voor elke' gelijktijdigheid](#change-for-each-concurrency). | 
| <*operation-option*> | String | Om uit te voeren een lus 'voor elke' sequentieel worden verwerkt, in plaats van parallel, stel <*bewerkingsoptie*> naar `Sequential` of <*aantal*> naar `1`, maar niet beide. Zie voor meer informatie, [uitvoeren 'voor elke' wordt uitgevoerd na elkaar](#sequential-for-each). | 
|||| 

*Voorbeeld*

Deze lus 'voor elke' verzendt een e-mail voor elk item in de matrix die bijlagen van een binnenkomende e-mailbericht bevat. De lus verzendt een e-mailbericht, met inbegrip van de bijlage, naar een persoon die de bijlage controleert.

```json
"For_each": {
   "type": "Foreach",
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "body": {
               "Body": "@base64ToString(items('For_each')?['Content'])",
               "Subject": "Review attachment",
               "To": "Sophie.Owen@contoso.com"
                },
            "host": {
               "connection": {
                  "id": "@parameters('$connections')['office365']['connectionId']"
               }
            },
            "method": "post",
            "path": "/Mail"
         },
         "runAfter": {}
      }
   },
   "foreach": "@triggerBody()?['Attachments']",
   "runAfter": {}
}
```

Als u alleen een matrix die wordt doorgegeven als uitvoer van de trigger, deze expressie haalt de <*matrixnaam*> matrix van de trigger-instantie. Als u wilt voorkomen dat een fout als de matrix niet bestaat, in de expressie wordt de `?` operator:

`@triggerBody()?['<array-name>']` 

<a name="if-action"></a>

### <a name="if-action"></a>Als actie

Deze actie wordt een *voorwaardelijke statement*, evalueert een expressie die een voorwaarde vertegenwoordigt en wordt uitgevoerd een andere vertakking op basis van de voorwaarde waar is of ONWAAR. Als de voorwaarde waar is, wordt de voorwaarde is gemarkeerd met de status 'Voltooid'. Informatie over [over het maken van voorwaardelijke instructies](../logic-apps/logic-apps-control-flow-conditional-statement.md).

``` json
"Condition": {
   "type": "If",
   "expression": { "<condition>" },
   "actions": {
      "<action-1>": { "<action-definition>" }
   },
   "else": {
      "actions": {
        "<action-2>": { "<action-definition" }
      }
   },
   "runAfter": {}
}
```

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*voorwaarde*> | JSON-Object | De voorwaarde, dit kan een expressie om te evalueren | 
| <*actie-1*> | JSON-Object | De actie om uit te voeren wanneer <*voorwaarde*> resulteert in waar | 
| <*action-definition*> | JSON-Object | De definitie voor de actie | 
| <*actie-2*> | JSON-Object | De actie om uit te voeren wanneer <*voorwaarde*> wordt geëvalueerd als onwaar | 
|||| 

De acties in de `actions` of `else` objecten ophalen deze statussen:

* 'Geslaagd' wanneer ze worden uitgevoerd en mislukt
* "Mislukt" wanneer ze worden uitgevoerd en mislukt
* "Overgeslagen' wanneer de respectieve vertakking kan niet worden uitgevoerd

*Voorbeeld*

Deze voorwaarde geeft aan wanneer de integer-variabele een waarde die groter zijn dan nul heeft, de werkstroom een website controleert. Als de variabele nul of minder is, controleert de werkstroom een andere website.

```json
"Condition": {
   "type": "If",
   "expression": {
      "and": [ {
         "greater": [ "@variables('myIntegerVariable')", 0 ] 
      } ]
   },
   "actions": { 
      "HTTP - Check this website": {
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
         "HTTP - Check this other website": {
            "type": "Http",
            "inputs": {
               "method": "GET",
               "uri": "http://this-other-url"
            },
            "runAfter": {}
         }
      }
   },
   "runAfter": {}
}
```  

#### <a name="how-conditions-use-expressions"></a>Hoe voorwaarden expressies gebruiken

Hier volgen enkele voorbeelden die laten zien hoe u expressies in voorwaarden kunt gebruiken:
  
| JSON | Resultaat | 
|------|--------| 
| "expressie": "@parameters('<*hasSpecialAction*>')" | Voor alleen Booleaanse expressies, de voorwaarde wordt voldaan voor elke waarde die wordt geëvalueerd op waar. <p>Als u wilt andere typen converteren naar een Booleaanse waarde, gebruikt u deze functies: `empty()` of `equals()`. | 
| "expressie": "@greater(actions('<*action*>').output.value, parameters ('<*drempelwaarde*>")) " | Van vergelijkingsfuncties, de actie wordt uitgevoerd alleen wanneer de uitvoer van <*actie*> is meer dan de <*drempelwaarde*> waarde. | 
| "expressie": "@or(groter (actions('<*action*>').output.value, parameters ('<*drempelwaarde*>")), minder (acties ('<*dezelfde actie*>').output.Value, 100)) " | Voor logische functies en het maken van Booleaanse expressies geneste, de actie wordt uitgevoerd als de uitvoer van <*actie*> is meer dan de <*drempelwaarde*> waarde of onder de 100. | 
| "expressie": "@equals(lengte (actions('<*action*>').outputs.errors), 0))" | Matrixfuncties kunt u controleren of de matrix geen items bevat. De actie wordt uitgevoerd als de `errors` matrix leeg is. | 
||| 

<a name="scope-action"></a>

### <a name="scope-action"></a>Scope-actie

Deze actie acties in logisch groepen *scopes*, die hun eigen status ophalen nadat de acties in dat bereik uitgevoerd. U kunt de status van de scope vervolgens gebruiken om te bepalen of andere acties worden uitgevoerd. Informatie over [scopes maken](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md).

```json
"Scope": {
   "type": "Scope",
   "actions": {
      "<inner-action-1>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      },
      "<inner-action-2>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      }
   }
}
```

*Vereist*

| Value | Type | Beschrijving | 
|-------|------|-------------|  
| <*interne-actie-1... n*> | JSON-Object | Een of meer acties die worden uitgevoerd binnen het bereik |
| <*action-inputs*> | JSON-Object | De invoer voor elke actie |
|||| 

<a name="switch-action"></a>

### <a name="switch-action"></a>Switch-actie

Deze actie, ook wel bekend als een *instructie switch*, organiseert van andere acties in *gevallen*, en wordt een waarde toegewezen aan elke aanvraag, met uitzondering van de standaard-aanvraag als er een bestaat. Wanneer de werkstroom wordt uitgevoerd, wordt de **Switch** actie wordt de waarde van een expressie, een object of een token op basis van de waarden opgegeven voor elk geval vergeleken. Als de **Switch** actie vindt een overeenkomende aanvraag, de werkstroom wordt uitgevoerd alleen de acties voor deze aanvraag. Telkens wanneer de **Switch** wordt uitgevoerd, geen van beide slechts één overeenkomende gevallen bestaat en of er geen overeenkomsten bestaan. Als er geen overeenkomsten bestaat, de **Switch** actie wordt uitgevoerd op de standaardacties. Informatie over [over het maken van de switch-instructies](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"Switch": {
   "type": "Switch",
   "expression": "<expression-object-or-token>",
   "cases": {
      "Case": {
         "actions": {
           "<action-name>": { "<action-definition>" }
         },
         "case": "<matching-value>"
      },
      "Case_2": {
         "actions": {
           "<action-name>": { "<action-definition>" }
         },
         "case": "<matching-value>"
      }
   },
   "default": {
      "actions": {
         "<default-action-name>": { "<default-action-definition>" }
      }
   },
   "runAfter": {}
}
```

*Vereist*

| Value | Type | Description | 
|-------|------|-------------| 
| <*expression-object-or-token*> | Varieert | De expressie, een JSON-object of een token dat u wilt evalueren | 
| <*naam van de actie*> | String | De naam van de actie om uit te voeren voor de overeenkomende aanvraag | 
| <*action-definition*> | JSON-Object | De definitie voor de actie om uit te voeren voor de overeenkomende aanvraag | 
| <*matching-value*> | Varieert | De waarde die moet worden vergeleken met het geëvalueerde resultaat | 
|||| 

*Optioneel*

| Value | Type | Beschrijving | 
|-------|------|-------------| 
| <*Standaard actienaam*> | String | De naam van het standaardactie om uit te voeren wanneer er geen overeenkomende gevallen bestaat | 
| <*default-action-definition*> | JSON-Object | De definitie voor de actie moet worden uitgevoerd wanneer er geen overeenkomende gevallen bestaat | 
|||| 

*Voorbeeld*

De definitie van deze actie wordt geëvalueerd of de persoon die reageren op de e-mail voor goedkeuring aanvraag geselecteerd de optie 'Goedkeuren' of 'Afwijzen'. Op basis van deze optie is de **Switch** actie worden de acties voor de overeenkomende aanvraag die naar een ander e-mailbericht verzenden naar het eindpunt, maar met andere woorden in elk geval wordt uitgevoerd. 

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
            "Send_an_email": { 
               "type": "ApiConnection",
               "inputs": {
                  "Body": "Thank you for your approval.",
                  "Subject": "Response received", 
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions": {
            "Send_an_email_2": { 
               "type": "ApiConnection",
               "inputs": {
                  "Body": "Thank you for your response.",
                  "Subject": "Response received", 
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}     
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": { 
         "Send_an_email_3": { 
            "type": "ApiConnection",
            "inputs": {
               "Body": "Please respond with either 'Approve' or 'Reject'.",
               "Subject": "Please respond", 
               "To": "Sophie.Owen@contoso.com"
            },
            "host": {
               "connection": {
                  "name": "@parameters('$connections')['office365']['connectionId']"
               }
            },
            "method": "post",
            "path": "/Mail"
         },
         "runAfter": {} 
      }
   },
   "runAfter": {
      "Send_approval_email": [ 
         "Succeeded"
      ]
   }
}
```

<a name="until-action"></a>

### <a name="until-action"></a>Tot actie

Deze lusactie bevat acties die worden uitgevoerd totdat de opgegeven voorwaarde waar is. Nadat alle andere acties hebt uitgevoerd, controleert de lus de voorwaarde als laatste stap. U kunt meer dan één actie in kunt opnemen de `"actions"` -object en de actie moet ten minste één limiet definiëren. Informatie over [maken 'tot' lussen](../logic-apps/logic-apps-control-flow-loops.md#until-loop). 

```json
 "Until": {
   "type": "Until",
   "actions": {
      "<action-name>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      },
      "<action-name>": {
         "type": "<action-type>",
         "inputs": { "<action-inputs>" },
         "runAfter": {}
      }
   },
   "expression": "<condition>",
   "limit": {
      "count": <loop-count>,
      "timeout": "<loop-timeout>"
   },
   "runAfter": {}
}
```

| Value | Type | Description | 
|-------|------|-------------| 
| <*naam van de actie*> | String | De naam voor de actie die u wilt uitvoeren binnen de lus | 
| <*action-type*> | String | Het actietype dat uit te voeren | 
| <*action-inputs*> | Verschillende | De invoer voor de actie om uit te voeren | 
| <*voorwaarde*> | String | De voorwaarde of expressie om te evalueren nadat alle acties in de lus uitgevoerd | 
| <*loop-count*> | Geheel getal | De limiet op het hoogste aantal lussen die de actie kan worden uitgevoerd. De standaardwaarde `count` waarde is 60. | 
| <*loop-timeout*> | String | De limiet voor de meeste tijd die de lus kan worden uitgevoerd. De standaardwaarde `timeout` waarde `PT1H`, dit is de vereiste [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601). |
|||| 

*Voorbeeld*

Deze definitie van de actie lus verzendt een HTTP-aanvraag naar de opgegeven URL tot een van deze voorwaarden wordt voldaan: 

* De aanvraag wordt een antwoord met de ' 200 OK ' statuscode.
* De lus is 60 keer uitgevoerd.
* De lus is uitgevoerd voor één uur.

```json
 "Run_until_loop_succeeds_or_expires": {
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
    "expression": "@equals(outputs('Http')['statusCode', 200])",
    "limit": {
        "count": 60,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

<a name="subscribe-unsubscribe"></a>

## <a name="webhooks-and-subscriptions"></a>Webhooks en abonnementen

Op basis van een Webhook-triggers en acties niet regelmatig controleren op eindpunten, maar in plaats daarvan wachten op specifieke gebeurtenissen of gegevens op deze eindpunten. Deze triggers en acties *abonneren* naar de eindpunten door te geven een *URL voor terugbellen* waar het eindpunt antwoorden kan verzenden.

De `subscribe` aanroep gebeurt er wanneer de werkstroom op een manier, bijvoorbeeld wijzigingen wanneer de referenties worden vernieuwd of wanneer de invoerparameters voor een trigger of actie wijzigen. Deze aanroep maakt gebruik van dezelfde parameters als standaard HTTP-acties. 

De `unsubscribe` aanroep gebeurt automatisch wanneer een bewerking de trigger of actie ongeldig is, bijvoorbeeld wordt:

* Het verwijderen of uitschakelen van de trigger. 
* Het verwijderen of uitschakelen van de werkstroom. 
* Het verwijderen of uitschakelen van het abonnement. 

Ter ondersteuning van deze aanroepen, de `@listCallbackUrl()` expressie retourneert een unieke 'URL voor terugbellen' voor de trigger of actie. Deze URL vertegenwoordigt een unieke id voor de eindpunten die van de service REST API gebruikmaken. De parameters voor deze functie zijn hetzelfde als de webhook-trigger of actie.

<a name="asynchronous-limits"></a>

## <a name="change-asynchronous-duration"></a>Duur van asynchrone wijzigen

Voor triggers en acties, kunt u de duur van het asynchrone patroon om een bepaald tijdsinterval beperken door toe te voegen de `limit.timeout` eigenschap. Op die manier als de actie die nog niet voltooid wanneer de timelapses interval, de status van de actie is gemarkeerd als `Cancelled` met de `ActionTimedOut` code. De `timeout` maakt gebruik van de eigenschap [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). 

``` json
"<trigger-or-action-name>": {
   "type": "Workflow | Webhook | Http | ApiConnectionWebhook | ApiConnection",
   "inputs": {},
   "limit": {
      "timeout": "PT10S"
   },
   "runAfter": {}
}
```

<a name="runtime-config-options"></a>

## <a name="runtime-configuration-settings"></a>Runtime-configuratie-instellingen

U kunt het standaardgedrag van de runtime voor triggers en acties met deze wijzigen `runtimeConfiguration` eigenschappen in het definitie van de trigger of actie.

| Eigenschap | Type | Description | Trigger of actie | 
|----------|------|-------------|-------------------| 
| `runtimeConfiguration.concurrency.runs` | Geheel getal | Wijzig de [ *standaardlimiet* ](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) op het aantal exemplaren van een logische app die op hetzelfde moment of parallel kunnen worden uitgevoerd. Deze waarde kan helpen bij het beperken van het aantal aanvragen dat back-endsystemen ontvangt. <p>Instellen van de `runs` eigenschap `1` werkt op dezelfde manier als de instelling de `operationOptions` eigenschap `SingleInstance`. U kunt de eigenschap, maar niet beide instellen. <p>De standaardlimiet Zie [wijziging trigger gelijktijdigheid](#change-trigger-concurrency) of [exemplaren sequentieel activeren](#sequential-trigger). | Alle triggers | 
| `runtimeConfiguration.concurrency.maximumWaitingRuns` | Geheel getal | Wijzig de [ *standaardlimiet* ](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) op het aantal logische app-exemplaren die worden uitgevoerd wanneer u uw logische app wordt al uitgevoerd voor het maximum aantal gelijktijdige exemplaren kan wachten. U kunt de limiet voor gelijktijdigheid van taken in de `concurrency.runs` eigenschap. <p>De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](#change-waiting-runs). | Alle triggers | 
| `runtimeConfiguration.concurrency.repetitions` | Geheel getal | Wijziging de [ *standaardlimiet* ](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits) op 'voor elke' het aantal iteraties die kunnen worden uitgevoerd op hetzelfde moment of parallel in een lus. <p>Instellen van de `repetitions` eigenschap `1` werkt op dezelfde manier als de instelling de `operationOptions` eigenschap `SingleInstance`. U kunt de eigenschap, maar niet beide instellen. <p>De standaardlimiet Zie [wijzigen 'voor elke' gelijktijdigheid](#change-for-each-concurrency) of [uitvoeren 'voor elke' wordt uitgevoerd na elkaar](#sequential-for-each). | Actie: <p>[Foreach](#foreach-action) | 
||||| 

<a name="operation-options"></a>

## <a name="operation-options"></a>Bewerkingsopties

U kunt het standaardgedrag voor triggers en acties met de `operationOptions` eigenschap in definitie van de trigger of actie.

| Bewerkingsoptie | Type | Description | Trigger of actie | 
|------------------|------|-------------|-------------------| 
| `DisableAsyncPattern` | String | HTTP-gebaseerde acties uitvoeren synchroon plaats asynchroon. <p><p>Als u wilt deze optie instelt, Zie [acties synchroon uitgevoerd](#asynchronous-patterns). | Acties: <p>[ApiConnection](#apiconnection-action), <br>[HTTP](#http-action), <br>[Antwoord](#response-action) | 
| `OptimizedForHighThroughput` | String | Wijziging de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) op het aantal actie-uitvoeringen per vijf minuten aan de [maximumlimiet](../logic-apps/logic-apps-limits-and-config.md#throughput-limits). <p><p>Als u wilt deze optie instelt, Zie [uitvoeren in de modus voor hoge doorvoer](#run-high-throughput-mode). | Alle acties | 
| `Sequential` | String | Voer 'voor elke' iteraties één tegelijk, in plaats van alles op hetzelfde moment parallel in een lus. <p>Deze optie werkt op dezelfde manier als de instelling de `runtimeConfiguration.concurrency.repetitions` eigenschap `1`. U kunt de eigenschap, maar niet beide instellen. <p><p>Als u wilt deze optie instelt, Zie [uitvoeren 'voor elke' wordt uitgevoerd na elkaar](#sequential-for-each).| Actie: <p>[Foreach](#foreach-action) | 
| `SingleInstance` | String | De trigger voor elke logische app-instantie worden opeenvolgend uitgevoerd en wacht tot de eerder active uitvoeren om te voltooien voordat u het volgende exemplaar van de logische app activeert. <p><p>Deze optie werkt op dezelfde manier als de instelling de `runtimeConfiguration.concurrency.runs` eigenschap `1`. U kunt de eigenschap, maar niet beide instellen. <p>Als u wilt deze optie instelt, Zie [exemplaren sequentieel activeren](#sequential-trigger). | Alle triggers | 
||||

<a name="change-trigger-concurrency"></a>

### <a name="change-trigger-concurrency"></a>Gelijktijdigheid van de trigger wijzigen

Standaard logic app-exemplaren worden uitgevoerd op hetzelfde moment, gelijktijdig of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Er wordt dus elk exemplaar van de trigger geactiveerd voordat het voorgaande logic app-exemplaar is voltooid. Deze limiet helpt te bepalen het aantal aanvragen dat back-endsystemen ontvangen. 

Als u wilt de standaardlimiet wijzigen, kunt u de code-editor voor weergave of de ontwerper van logische Apps omdat wijzigen van de instelling voor gelijktijdigheid via de ontwerpfunctie wordt toegevoegd of bijgewerkt omdat de `runtimeConfiguration.concurrency.runs` eigenschap in de onderliggende definitie van de trigger en vice versa. Deze eigenschap bepaalt het maximum aantal logic app-exemplaren die gelijktijdig kunnen worden uitgevoerd. 

> [!NOTE] 
> Als u de trigger om uit te voeren sequentieel worden verwerkt door met behulp van de designer of de code-editor weergeven, van de trigger niet ingesteld `operationOptions` eigenschap `SingleInstance` in de weergave-editor. Anders krijgt u een validatiefout. Zie voor meer informatie, [exemplaren sequentieel activeren](#sequential-trigger).

#### <a name="edit-in-code-view"></a>Bewerken in de codeweergave 

In de onderliggende definitie, toevoegen of bijwerken van de `runtimeConfiguration.concurrency.runs` eigenschap een waarde tussen `1` en `50` liggen.

Hier volgt een voorbeeld dat gelijktijdige uitvoeringen naar 10 exemplaren beperkt:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": 10
      }
   }
}
```

#### <a name="edit-in-logic-apps-designer"></a>Bewerken in de ontwerper van logische Apps

1. In de rechterbovenhoek van de trigger, kies de knop met weglatingstekens (...) en kies vervolgens **instellingen**.

2. Onder **gelijktijdigheidsbeheer**, stel **limiet** naar **op**. 

3. Sleep de **graad van parallelle uitvoering** schuifregelaar naar de gewenste waarde. Als u wilt uw logische app na elkaar uitgevoerd, sleept u de waarde van de schuifregelaar naar **1**.

<a name="change-for-each-concurrency"></a>

### <a name="change-for-each-concurrency"></a>'Voor elke' gelijktijdigheid wijzigen

Standaard lus 'voor elke' iteraties op hetzelfde moment of parallel, tot uitvoeren de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Als u wilt de standaardlimiet wijzigen, kunt u de code-editor voor weergave of de ontwerper van logische Apps omdat wijzigen van de instelling voor gelijktijdigheid via de ontwerpfunctie wordt toegevoegd of bijgewerkt omdat de `runtimeConfiguration.concurrency.repetitions` eigenschap in de actie 'voor elke' onderliggende definitie en vice versa. Deze eigenschap bepaalt het maximum aantal iteraties die parallel kunnen worden uitgevoerd.

> [!NOTE] 
> Als u de actie 'voor elke' om uit te voeren sequentieel worden verwerkt door met behulp van de designer of de code-editor weergeven, niet van de actie ingesteld `operationOptions` eigenschap `Sequential` in de weergave-editor. Anders krijgt u een validatiefout. Zie voor meer informatie, [uitvoeren 'voor elke' wordt uitgevoerd na elkaar](#sequential-for-each).

#### <a name="edit-in-code-view"></a>Bewerken in de codeweergave 

In de onderliggende definitie van 'voor elke' toevoegen of bijwerken de `runtimeConfiguration.concurrency.repetitions` eigenschap een waarde tussen `1` en `50` liggen. 

Hier volgt een voorbeeld dat gelijktijdige uitvoeringen naar 10 iteraties beperkt:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": 10
      }
   }
}
```

#### <a name="edit-in-logic-apps-designer"></a>Bewerken in de ontwerper van logische Apps

1. In de **voor elk** actie in de rechterbovenhoek, kiest u de knop met weglatingstekens (...) en kies vervolgens **instellingen**.

2. Onder **gelijktijdigheidsbeheer**, stel **gelijktijdigheidsbeheer** naar **op**. 

3. Sleep de **graad van parallelle uitvoering** schuifregelaar naar de gewenste waarde. Als u wilt uw logische app na elkaar uitgevoerd, sleept u de waarde van de schuifregelaar naar **1**.

<a name="change-waiting-runs"></a>

### <a name="change-waiting-runs-limit"></a>Wachten op wordt uitgevoerd limiet wijzigen

Standaard logic app-exemplaren worden uitgevoerd op hetzelfde moment, gelijktijdig of parallel tot de [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits). Elk exemplaar van de trigger wordt geactiveerd voordat het eerder active logic app-exemplaar is voltooid. Weliswaar u kunt [wijzigen van deze standaardlimiet](#change-trigger-concurrency), wanneer het aantal exemplaren van logische app van de nieuwe limiet voor gelijktijdigheid, bereikt eventuele andere nieuwe exemplaren moeten wachten om uit te voeren. 

Het aantal uitvoeringen dat aanvragen kan wachten heeft ook een [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits), die u kunt wijzigen. Echter, nadat uw logische app de limiet voor het wachten wordt uitgevoerd bereikt, de Logic Apps-engine niet meer accepteert nieuwe wordt uitgevoerd. Aanvraag- en webhook-triggers 429-fouten retourneren en terugkerende triggers gestart polling pogingen wordt overgeslagen.

Als u wilt wijzigen van de standaardlimiet voor het wachten wordt uitgevoerd, in de onderliggende definitie activeren, voegt de `runtimeConfiguration.concurency.maximumWaitingRuns` eigenschap met een waarde tussen `0` en `100`. 

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "maximumWaitingRuns": 50
      }
   }
}
```

<a name="sequential-trigger"></a>

### <a name="trigger-instances-sequentially"></a>Exemplaren sequentieel activeren

Voor het uitvoeren van elke logica instellen wordt uitgevoerd, app-exemplaar dat alleen nadat het vorige exemplaar is voltooid de trigger is die moet worden opeenvolgend uitgevoerd. U kunt de code-editor voor weergave of de ontwerper van logische Apps gebruiken omdat ook wijzigen van de instelling voor gelijktijdigheid via designer wordt toegevoegd of bijgewerkt de `runtimeConfiguration.concurrency.runs` eigenschap in de onderliggende definitie van de trigger en vice versa. 

> [!NOTE] 
> Bij het instellen van een trigger om uit te voeren sequentieel worden verwerkt door met behulp van de designer of de code-editor voor weergave van de trigger geen instelt `operationOptions` eigenschap `Sequential` in de weergave-editor. Anders krijgt u een validatiefout. 

#### <a name="edit-in-code-view"></a>Bewerken in de codeweergave

In de Triggerdefinitie van de instellen van deze eigenschappen, maar niet beide. 

Stel de `runtimeConfiguration.concurrency.runs` eigenschap `1`:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": 1
      }
   }
}
```

*-of-*

Stel de `operationOptions` eigenschap `SingleInstance`:

```json
"<trigger-name>": {
   "type": "<trigger-name>",
   "recurrence": {
      "frequency": "<time-unit>",
      "interval": <number-of-time-units>,
   },
   "operationOptions": "SingleInstance"
}
```

#### <a name="edit-in-logic-apps-designer"></a>Bewerken in de ontwerper van logische Apps

1. In de rechterbovenhoek van de trigger, kies de knop met weglatingstekens (...) en kies vervolgens **instellingen**.

2. Onder **gelijktijdigheidsbeheer**, stel **limiet** naar **op**. 

3. Sleep de **graad van parallelle uitvoering** schuifregelaar op het getal `1`. 

<a name="sequential-for-each"></a>

### <a name="run-for-each-loops-sequentially"></a>Voer 'voor elke' wordt na elkaar uitgevoerd

Iteratie alleen nadat de vorige iteratie is voltooid instellen wordt uitgevoerd, de actie 'voor elke' worden opeenvolgend uitgevoerd voor het uitvoeren van een lus 'voor elke'. U kunt de code-editor voor weergave of de ontwerper van logische Apps gebruiken omdat ook wijzigen van de actie gelijktijdigheid via designer wordt toegevoegd of bijgewerkt omdat de `runtimeConfiguration.concurrency.repetitions` eigenschap in de onderliggende definitie van de actie en vice versa. 

> [!NOTE] 
> Bij het instellen van een actie 'voor elke' om uit te voeren sequentieel worden verwerkt door met behulp van de designer of code-editor voor weergave van de actie niet instellen `operationOptions` eigenschap `Sequential` in de weergave-editor. Anders krijgt u een validatiefout. 

#### <a name="edit-in-code-view"></a>Bewerken in de codeweergave

In de definitie van de actie, instellen van deze eigenschappen, maar niet beide. 

Stel de `runtimeConfiguration.concurrency.repetitions` eigenschap `1`:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "runtimeConfiguration": {
      "concurrency": {
         "repetitions": 1
      }
   }
}
```

*-of-*

Stel de `operationOptions` eigenschap `Sequential`:

```json
"For_each" {
   "type": "Foreach",
   "actions": { "<actions-to-run>" },
   "foreach": "<for-each-expression>",
   "runAfter": {},
   "operationOptions": "Sequential"
}
```

#### <a name="edit-in-logic-apps-designer"></a>Bewerken in de ontwerper van logische Apps

1. In de **voor elk** rechterbovenhoek van de actie, kiest u de knop met weglatingstekens (...) en kies vervolgens **instellingen**.

2. Onder **gelijktijdigheidsbeheer**, stel **gelijktijdigheidsbeheer** naar **op**. 

3. Sleep de **graad van parallelle uitvoering** schuifregelaar op het getal `1`. 

<a name="asynchronous-patterns"></a>

### <a name="run-actions-synchronously"></a>Acties synchroon uitgevoerd

Standaard alle HTTP-gebaseerde acties Ga als volgt het patroon standaard asynchrone bewerking. Dit patroon geeft aan dat wanneer een actie op basis van HTTP een aanvraag naar het opgegeven eindpunt verzendt, de RAS-server een '202 geaccepteerd' antwoord terug stuurt. Dit antwoord betekent dat de server de aanvraag voor de verwerking van geaccepteerd. De Logic Apps-engine wordt de URL die is opgegeven door de locatieheader van het item tot de verwerking stopt, dit is een niet-202-antwoord gecontroleerd.

Aanvragen hebben echter een time-out beperken, zodat voor langdurige acties, u het gedrag van asynchrone uitschakelen kunt door toe te voegen en de `operationOptions` eigenschap `DisableAsyncPattern` onder van de actie-invoer.
  
```json
"<some-long-running-action>": {
   "type": "Http",
   "inputs": { "<action-inputs>" },
   "operationOptions": "DisableAsyncPattern",
   "runAfter": {}
}
```

<a name="run-high-throughput-mode"></a>

### <a name="run-in-high-throughput-mode"></a>Uitvoeren in de modus voor hoge doorvoer

Voor een enkele logische app uitvoeren, het aantal acties die worden uitgevoerd om de 5 minuten heeft een [standaardlimiet](../logic-apps/logic-apps-limits-and-config.md#throughput-limits). Deze limiet te verhogen de [maximale](../logic-apps/logic-apps-limits-and-config.md#throughput-limits) mogelijk, stel de `operationOptions` eigenschap `OptimizedForHighThroughput`. Deze instelling worden uw logische app in de modus 'hoge doorvoer' geplaatst. 

> [!NOTE]
> Modus voor hoge doorvoer is beschikbaar als preview. U kunt ook een werklast verdelen over meer dan één logische app zo nodig.

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": { "<action-inputs>" },
   "operationOptions": "OptimizedForHighThroughput",
   "runAfter": {}
}
```

<a name="connector-authentication"></a>

## <a name="authenticate-http-triggers-and-actions"></a>HTTP-triggers en acties verifiëren

HTTP-eindpunten ondersteunen verschillende soorten verificatie. U kunt verificatie instellen voor deze HTTP-triggers en acties:

* [HTTP](../connectors/connectors-native-http.md)
* [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)
* [HTTP-webhook](../connectors/connectors-native-webhook.md)

Dit zijn de soorten verificatie die u kunt instellen:

* [Basisverificatie](#basic-authentication)
* [Clientverificatie via certificaat](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth-verificatie](#azure-active-directory-oauth-authentication)

> [!IMPORTANT]
> Zorg ervoor dat u gevoelige informatie die verantwoordelijk is voor de definitie van de werkstroom van de logische app beveiligen. Gebruik beveiligde parameters en de gegevens zo nodig coderen. Zie voor meer informatie over het gebruik en beveiliging van de parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Basisverificatie

Voor [basisverificatie](../active-directory-b2c/active-directory-b2c-custom-rest-api-netfw-secure-basic.md) met behulp van Azure Active Directory, de definitie van de trigger of actie kan bevatten een `authentication` JSON-object, dat de eigenschappen zijn opgegeven door de volgende tabel. Voor toegang tot de parameterwaarden tijdens runtime, kunt u de `@parameters('parameterName')` expressie die wordt geleverd door de [Werkstroomdefinitietaal](https://aka.ms/logicappsdocs). 

| Eigenschap | Vereist | Value | Beschrijving | 
|----------|----------|-------|-------------| 
| **type** | Ja | "Basic" | Het verificatietype dat moet worden gebruikt, die hier 'Basic' | 
| **gebruikersnaam** | Ja | "@parameters('userNameParam')" | De naam van de gebruiker voor het verifiëren van toegang tot het doel-service-eindpunt |
| **wachtwoord** | Ja | "@parameters(passwordParam)" | Het wachtwoord voor het verifiëren van toegang tot het doel-service-eindpunt |
||||| 

In dit voorbeeld definitie van de HTTP-actie de `authentication` sectie geeft u `Basic` verificatie. Zie voor meer informatie over het gebruik en beveiliging van de parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

> [!IMPORTANT]
> Zorg ervoor dat u gevoelige informatie die verantwoordelijk is voor de definitie van de werkstroom van de logische app beveiligen. Gebruik beveiligde parameters en de gegevens zo nodig coderen. Zie voor meer informatie over het beveiligen van parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>Verificatie van clientcertificaten

Voor [certificaten gebaseerde verificatie](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) met Azure Active Directory, de definitie van de trigger of actie kunt opnemen een `authentication` JSON-object, dat de eigenschappen zijn opgegeven door de volgende tabel. Voor toegang tot de parameterwaarden tijdens runtime, kunt u de `@parameters('parameterName')` expressie die wordt geleverd door de [Werkstroomdefinitietaal](https://aka.ms/logicappsdocs). Zie voor de limieten voor het aantal clientcertificaten kunt u [limieten en configuratie voor Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md).

| Eigenschap | Vereist | Value | Description |
|----------|----------|-------|-------------|
| **type** | Ja | "ClientCertificate" | Het verificatietype dat moet worden gebruikt voor clientcertificaten Secure Sockets Layer (SSL). Zelfondertekende certificaten worden ondersteund, worden niet zelfondertekende certificaten voor SSL ondersteund. |
| **pfx** | Ja | "@parameters('pfxParam') | De met base64 gecodeerde inhoud uit een Personal Information Exchange (PFX)-bestand |
| **wachtwoord** | Ja | "@parameters(passwordParam)" | Het wachtwoord voor toegang tot het PFX-bestand |
||||| 

In dit voorbeeld definitie van de HTTP-actie de `authentication` sectie geeft u `ClientCertificate` verificatie. Zie voor meer informatie over het gebruik en beveiliging van de parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Zorg ervoor dat u gevoelige informatie die verantwoordelijk is voor de definitie van de werkstroom van de logische app beveiligen. Gebruik beveiligde parameters en de gegevens zo nodig coderen. Zie voor meer informatie over het beveiligen van parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-ad-oauth-authentication"></a>Azure Active Directory (AD) OAuth-verificatie

Voor [Azure AD OAuth-verificatie](../active-directory/develop/authentication-scenarios.md), kan de definitie van de trigger of actie bevatten een `authentication` JSON-object, dat de eigenschappen zijn opgegeven door de volgende tabel. Voor toegang tot de parameterwaarden tijdens runtime, kunt u de `@parameters('parameterName')` expressie die wordt geleverd door de [Werkstroomdefinitietaal](https://aka.ms/logicappsdocs).

| Eigenschap | Vereist | Value | Beschrijving |
|----------|----------|-------|-------------|
| **type** | Ja | `ActiveDirectoryOAuth` | Het verificatietype dat moet worden gebruikt, namelijk 'ActiveDirectoryOAuth' voor Azure AD OAuth |
| **instantie** | Nee | <*URL-for-authority-token-issuer*> | De URL voor de instantie waarmee het verificatietoken |
| **tenant** | Ja | <*tenant-ID*> | De tenant-ID voor de Azure AD-tenant |
| **Doelgroep** | Ja | <*resource-tot-toestaan*> | De resource die u wilt gebruiken voor autorisatie, bijvoorbeeld: `https://management.core.windows.net/` |
| **ClientId** | Ja | <*client-ID*> | De client-ID voor de app toestemming vragen |
| **credentialType** | Ja | 'Certificaat' of 'Geheim' | Het referentietype de client wordt gebruikt voor het aanvragen van autorisatie. Deze eigenschap en waarde worden niet weergegeven in het definitie van de onderliggende, maar de vereiste parameters voor het referentietype bepaalt. |
| **pfx** | Ja, alleen voor het referentietype 'Certificaat' | "@parameters('pfxParam') | De met base64 gecodeerde inhoud uit een Personal Information Exchange (PFX)-bestand |
| **wachtwoord** | Ja, alleen voor het referentietype 'Certificaat' | "@parameters(passwordParam)" | Het wachtwoord voor toegang tot het PFX-bestand |
| **geheim** | Ja, alleen voor het "Geheim" referentietype | "@parameters('secretParam')" | Het clientgeheim voor het aanvragen van autorisatie |
|||||

In dit voorbeeld definitie van de HTTP-actie de `authentication` sectie geeft u `ActiveDirectoryOAuth` verificatie en het "geheim" referentietype. Zie voor meer informatie over het gebruik en beveiliging van de parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://www.microsoft.com",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
         "audience": "https://management.core.windows.net/",
         "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Zorg ervoor dat u gevoelige informatie die verantwoordelijk is voor de definitie van de werkstroom van de logische app beveiligen. Gebruik beveiligde parameters en de gegevens zo nodig coderen. Zie voor meer informatie over het beveiligen van parameters [beveiligen van uw logische app](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Definitietaal van werkstroom](../logic-apps/logic-apps-workflow-definition-language.md)

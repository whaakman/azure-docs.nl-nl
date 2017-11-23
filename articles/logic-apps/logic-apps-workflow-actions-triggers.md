---
title: Werkstroom triggers en acties - Azure Logic Apps | Microsoft Docs
description: Meer informatie over de soorten triggers en acties die u gebruiken kunt voor het maken en automatisering van werkstromen en processen met Azure Logic Apps
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 9f95c0c486401e0d709829ce8d560f030932eea7
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="triggers-and-actions-for-logic-app-workflows"></a>Triggers en acties voor logic app-werkstromen

Alle logische apps beginnen met een trigger gevolgd door acties. Dit onderwerp beschrijft de soorten triggers en acties die u gebruiken kunt voor het systeemintegraties maken en zakelijke werkstromen of processen automatiseren met het bouwen van logic apps. 
  
## <a name="triggers-overview"></a>Triggers-overzicht 

Alle logische apps beginnen met een trigger, waarmee de oproepen die u met een logische app uitgevoerd beginnen kunnen. Hier volgen de twee manieren waarop u initiëren starten kunt, een uitvoering van uw werkstroom:  

* Een polling-trigger  
* Een push-signaal roept de [REST-API van Workflow](https://docs.microsoft.com/rest/api/logic/workflows)  
  
Alle triggers bevatten deze op het hoogste niveau elementen:  
  
```json
"trigger-name": {
    "type": "trigger-type",
    "inputs": { call-settings },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Day|Week|Month",
        "interval": recurrence-interval-based-on-frequency
    },
    "conditions": [ array-of-required-conditions ],
    "splitOn": "property-used-for-creating-separate-workflows",
    "operationOptions": "operation-options-for-trigger"
}
```

### <a name="trigger-types-and-inputs"></a>Triggertypen en invoer  

Elk triggertype heeft een andere interface en andere *invoer* het gedrag te definiëren. 

| Triggertype | Beschrijving | 
| ------------ | ----------- | 
| **Terugkeerpatroon** | Deze gebeurtenis wordt gestart op basis van een ingesteld schema. U kunt een toekomstige datum en tijd voor het starten van deze trigger instellen. Op basis van de frequentie, u kunt ook opgeven tijdstippen en dagen voor het uitvoeren van de werkstroom. | 
| **Aanvraag**  | Uw logische app maakt in een eindpunt dat u aanroepen kunt, ook wel bekend als een 'handmatig' trigger. | 
| **HTTP** | Controleert, of *polls*, een HTTP-web-eindpunt. Het HTTP-eindpunt moet voldoen aan een specifieke activerende contract met behulp van een '202' asynchrone patroon of door te retourneren van een matrix. | 
| **ApiConnection** | Als een HTTP-trigger worden opgevraagd, maar gebruikt [Microsoft beheerde API's](../connectors/apis-list.md). | 
| **HTTPWebhook** | Uw logische app maakt in een aanroepbare eindpunt, zoals de aanvraag worden geactiveerd, maar een opgegeven URL voor het registreren en de registratie van aanroepen. |
| **ApiConnectionWebhook** | Werkt als de **HTTPWebhook** trigger gebruikt, maar Microsoft beheerde API's. | 
||| 

Zie voor meer informatie over andere details [werkstroom Definition Language](../logic-apps/logic-apps-workflow-definition-language.md). 
  
## <a name="recurrence-trigger"></a>Terugkeerpatroon trigger  

Deze trigger wordt uitgevoerd op basis van het terugkeerpatroon en het schema dat u opgeeft en biedt een eenvoudige manier voor het uitvoeren van regelmatig een werkstroom. Hier volgt een voorbeeld van een trigger basis terugkeerpatroon die dagelijks wordt uitgevoerd:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```
U kunt ook een begindatum en -tijd voor het starten van de trigger plannen. U kunt bijvoorbeeld een wekelijks rapport elke maandag starten, de logische app te starten op een specifieke maandag zoals in dit voorbeeld plannen: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2017-09-18T00:00:00Z"
    }
}
```

Hier volgt de definitie voor deze trigger: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "second|minute|hour|day|week|month",
        "interval": recurrence-interval-based-on-frequency,
        "schedule": {
            // Applies only when frequency is Day or Week. Separate values with commas.
            "hours": [ one-or-more-hour-marks ], 
            // Applies only when frequency is Day or Week. Separate values with commas.
            "minutes": [ one-or-more-minute-marks ], 
            // Applies only when frequency is Week. Separate values with commas.
            "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
        },
        "startTime": "start-date-time-with-format-YYYY-MM-DDThh:mm:ss",
        "timeZone": "specify-time-zone"
    }
}
```

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| frequency | Ja | Tekenreeks | Tijdseenheid voor hoe vaak de trigger wordt geactiveerd. Gebruik slechts één van deze waarden: 'tweede', 'minuut', 'uur', 'day', 'week' of 'maand' | 
| interval | Ja | Geheel getal | Een positief geheel getal dat wordt beschreven hoe vaak de werkstroom wordt uitgevoerd op basis van de frequentie. <p>Hier volgen de minimale en maximale intervallen: <p>-Maand: 1-16 maanden </br>-Dag: 1-500 dagen </br>-Uurs: 1-12.000 uur </br>-Minuut: 1-72.000 minuten </br>-Tweede: 1-9,999,999 seconden<p>Bijvoorbeeld, als het interval is ingesteld op 6 en de frequentie is "maand", is klikt u vervolgens het terugkeerpatroon elke 6 maanden. | 
| timeZone | Nee | Tekenreeks | Geldt alleen wanneer u een begintijd opgeeft omdat deze trigger niet accepteren [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Geef de tijdzone die u wilt toepassen. | 
| startTime | Nee | Tekenreeks | Geef de datum en tijd in deze indeling: <p>JJJJ-MM-ddTUU als u een tijdzone opgeven <p>-of- <p>JJJJ-MM-ssZ als u een tijdzone geen opgeeft <p>Dus bijvoorbeeld, als u wilt dat 18 September 2017 op 14:00 uur, geeft u "2017-09-18T14:00:00 ' en geeft u een tijdzone zoals"Pacific (standaardtijd)". Of geef ' 2017-09-18T14:00:00Z ' zonder een tijdzone. <p>**Opmerking:** de begintijd moet volgen de [ISO 8601 datum, tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Als u een tijdzone niet opgeeft, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze 'Z"verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). <p>Voor eenvoudige schema's van de begintijd is de eerste keer voorkomt, terwijl voor complexe schema's van de trigger vroeger dan de begintijd wordt niet gestart. Zie voor meer informatie over start datums en tijden [maken en plannen die regelmatig actieve taken](../connectors/connectors-native-recurrence.md). | 
| weekDays | Nee | String of string array | Als u 'Week' opgeeft voor `frequency`, kunt u een of meer dagen, gescheiden door komma's, als u wilt uitvoeren van de werkstroom: "Maandag", "Dinsdag", "Woensdag", "Donderdag", "Vrijdag", "Zaterdag" en "Zondag" | 
| hours | Nee | Geheel getal of een matrix van geheel getal | Als u 'Day' of 'Week' opgeeft voor `frequency`, kunt u een of meer gehele getallen tussen 0 en 23, gescheiden door komma's als de uren van de dag waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, als u '10', '12' en '14' opgeeft, krijgt u 10 uur, 12 uur en 14 uur als de markeringen uur. | 
| minutes | Nee | Geheel getal of een matrix van geheel getal | Als u 'Dag' of 'Week' opgeeft voor `frequency`, kunt u een of meer gehele getallen van 0 tot 59, gescheiden door komma's als de minuten van het uur waarop u wilt uitvoeren van de werkstroom. <p>Bijvoorbeeld, kunt u '30' als de minuut is ingeschakeld en met het vorige voorbeeld voor het uur van de dag, krijgt u 10:30 AM, 12:30 PM en 2:30 PM. | 
|||||| 

Deze trigger terugkeerpatroon geeft bijvoorbeeld aan dat er Wekelijks elke maandag op uw logische app wordt uitgevoerd op 10:30 uur, 12:30 PM en 2:30 uur Pacific (standaardtijd), geen vroeger dan 9 September 2017 op 14:00 uur wordt gestart:

``` json
{
    "triggers": {
        "myRecurrenceTrigger": {
            "type": "Recurrence",
            "recurrence": {
                "frequency": "Week",
                "interval": 1,
                "schedule": {
                    "hours": [
                        10,
                        12,
                        14
                    ],
                    "minutes": [
                        30
                    ],
                    "weekDays": [
                        "Monday"
                    ]
                },
               "startTime": "2017-09-07T14:00:00",
               "timeZone": "Pacific Standard Time"
            }
        }
    }
}
```

Zie voor meer informatie over het terugkeerpatroon en voorbeelden van begin-tijd voor deze trigger [maken en plannen die regelmatig actieve taken](../connectors/connectors-native-recurrence.md).

## <a name="request-trigger"></a>Aanvraag trigger

Deze trigger fungeert als een eindpunt dat u gebruiken kunt voor het aanroepen van uw logische app via een HTTP-aanvraag. Een aanvraag-trigger ziet er in dit voorbeeld:  
  
```json
"myRequestTrigger": {
    "type": "Request",
    "kind": "Http",
    "inputs": {
        "schema": {
            "type": "Object",
            "properties": {
                "myInputProperty1": { "type" : "string" },
                "myInputProperty2": { "type" : "number" }
            },
            "required": [ "myInputProperty1" ]
        }
    }
} 
```

Deze trigger heeft een optionele eigenschap aangeroepen *schema*:
  
| Elementnaam | Vereist | Type | Beschrijving |
| ------------ | -------- | ---- | ----------- |
| Schema | Nee | Object | Een JSON-schema te valideren en de binnenkomende aanvraag. Handig voor het helpen werkstroomstappen van de volgende weten welke eigenschappen om te verwijzen. | 
||||| 

Om aan te roepen dit eindpunt, moet u aan te roepen de *listCallbackUrl* API. Zie [REST-API van werkstroom](https://docs.microsoft.com/rest/api/logic/workflows).

## <a name="http-trigger"></a>HTTP-trigger  

HTTP-triggers peilen op een opgegeven eindpunt en controleert u het antwoord om te bepalen of de werkstroom moet worden uitgevoerd. Hier de `inputs` object nodig deze parameters die zijn vereist voor het maken van een HTTP-aanroep:  

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| Methode | Ja | Tekenreeks | Maakt gebruik van een van deze methoden HTTP: 'Ophalen', 'Posten', 'Plaatsen', 'Verwijderen', 'PATCH' of 'HEAD' | 
| URI | Ja| Tekenreeks | De HTTP of HTTPs-eindpunt dat de trigger wordt gecontroleerd. Maximale grootte van tekenreeks: 2 KB | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| Verificatie | Nee | Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). <p>Afgezien van Scheduler, er is een meer ondersteunde eigenschap: `authority`. Deze waarde is standaard `https://login.windows.net` wanneer niet wordt opgegeven, maar u kunt een andere waarde, zoals`https://login.windows\-ppe.net`. | 
||||| 
 
Om te werken goed met uw logische app, vereist de HTTP-trigger dat de HTTP-API om te voldoen aan een specifiek patroon. De trigger herkent deze eigenschappen:  
  
| Antwoord | Vereist | Beschrijving | 
| -------- | -------- | ----------- |  
| Statuscode | Ja | De statuscode 200 ('OK') zorgt ervoor dat een uitvoering. Andere statuscode niet leiden tot een uitvoering. | 
| Probeer het opnieuw na-header | Nee | Het aantal seconden totdat de logische app het eindpunt opnieuw worden opgevraagd. | 
| Locatieheader | Nee | De URL om aan te roepen op de volgende pollinginterval. Als niet wordt opgegeven, wordt de oorspronkelijke URL gebruikt. | 
|||| 

Hier volgen enkele voorbeeld-gedrag voor verschillende soorten aanvragen:
  
| Reactiecode | Opnieuw proberen na | Gedrag | 
| ------------- | ----------- | -------- | 
| 200 | {geen} | Het uitvoeren van de werkstroom vervolgens opnieuw controleren voor meer gegevens na de gedefinieerde terugkeerpatroon. | 
| 200 | 10 seconden | Het uitvoeren van de werkstroom vervolgens opnieuw controleren voor meer gegevens na 10 seconden. |  
| 202 | 60 seconden | De werkstroom niet wordt geactiveerd. De volgende poging gebeurt in één minuut, onderworpen aan de gedefinieerde terugkeerpatroon. Als de gedefinieerde terugkeer minder dan één minuut is, wordt de koptekst opnieuw proberen na voorrang. Anders wordt wordt de gedefinieerde herhaling gebruikt. | 
| 400 | {geen} | Onjuiste aanvraag, niet de werkstroom wordt uitgevoerd. Als er geen `retryPolicy` is gedefinieerd, wordt het standaardbeleid gebruikt. Nadat u het aantal nieuwe pogingen is bereikt, controleert de trigger opnieuw gegevens na de gedefinieerde terugkeerpatroon. | 
| 500 | {geen}| Server-fout, de werkstroom niet uitvoeren. Als er geen `retryPolicy` is gedefinieerd, wordt het standaardbeleid gebruikt. Nadat u het aantal nieuwe pogingen is bereikt, controleert de trigger opnieuw gegevens na de gedefinieerde terugkeerpatroon. | 
|||| 

Hier volgen de HTTP-trigger uitvoer: 
  
| Elementnaam | Type | Beschrijving |
| ------------ | ---- | ----------- |
| headers | Object | De headers van de HTTP-antwoord | 
| hoofdtekst | Object | De hoofdtekst van het HTTP-antwoord | 
|||| 

## <a name="api-connection-trigger"></a>API-verbinding activeren  

De API verbinding trigger is vergelijkbaar met de HTTP-trigger in de basisfunctionaliteit. De parameters voor het identificeren van de actie zijn echter verschillend. Hier volgt een voorbeeld:  
  
```json
"myDailyReportTrigger": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            }
        },
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| host | Ja | Object | De gehoste-gateway en de ID voor de API-App | 
| Methode | Ja | Tekenreeks | Maakt gebruik van een van deze methoden HTTP: 'Ophalen', 'Posten', 'Plaatsen', 'Verwijderen', 'PATCH' of 'HEAD' | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| Verificatie | Nee | Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). | 
||||| 

Voor de `host` object hier worden de eigenschappen:  
  
| Elementnaam | Vereist | Beschrijving | 
| ------------ | -------- | ----------- | 
| API-runtimeUrl | Ja | Het eindpunt voor de beheerde API | 
| Verbindingsnaam |  | De naam van de beheerde API-verbinding die gebruikmaakt van de werkstroom. Moet verwijzen naar een parameter genaamd `$connection`. |
|||| 

Hier volgen de uitvoer voor een API-verbinding activeren:
  
| Elementnaam | Type | Beschrijving |
| ------------ | ---- | ----------- |
| headers | Object | De headers van de HTTP-antwoord | 
| hoofdtekst | Object | De hoofdtekst van het HTTP-antwoord | 
|||| 
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook trigger  

De trigger HTTPWebhook biedt een eindpunt, vergelijkbaar met de aanvraag worden geactiveerd, maar de trigger HTTPWebhook aanroepen ook een opgegeven URL voor het registreren en de registratie. Hier volgt een voorbeeld van wat een trigger HTTPWebhook als volgt uitzien:  

```json
"myAppsSpotTrigger": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
}
```

Veel van deze gedeelten zijn optioneel en de werking van de trigger HTTPWebhook is afhankelijk van de volgende secties u opgeven of uitsluiten. Hier worden de eigenschappen voor de trigger HTTPWebhook:
  
| Elementnaam | Vereist | Beschrijving | 
| ------------ | -------- | ----------- |  
| abonneren | Nee | Hiermee geeft u de uitgaande aanvraag aan te roepen als de trigger wordt gemaakt en de initiële registratie uitvoert. | 
| afmelden | Nee | Hiermee geeft u de uitgaande aanvraag aan te roepen wanneer de trigger wordt verwijderd. | 
|||| 

U kunt limieten opgeven van een webhook-actie op dezelfde manier als [HTTP asynchrone limieten](#asynchronous-limits). Hier vindt u meer informatie over de `subscribe` en `unsubscribe` acties:

* `subscribe`wordt aangeroepen, zodat de trigger kunt beginnen met luisteren op gebeurtenissen. Deze uitgaande aanroep wordt gestart met dezelfde parameters als standaard HTTP-acties. Deze aanroep wordt uitgevoerd wanneer de werkstroom op een manier, bijvoorbeeld wijzigingen wanneer de referenties worden hersteld of van de trigger-invoerparameters wijzigen. 
  
  Ter ondersteuning van deze aanroep de `@listCallbackUrl()` functie wordt een unieke URL voor deze specifieke trigger in de werkstroom. Deze URL vertegenwoordigt de unieke id voor de eindpunten die van de service REST API gebruikmaken.
  
* `unsubscribe`wordt automatisch aangeroepen wanneer een bewerking renders deze trigger ongeldig, met inbegrip van deze bewerkingen:

  * Het verwijderen of uitschakelen van de trigger. 
  * Het verwijderen of uitschakelen van de werkstroom. 
  * Het verwijderen of uitschakelen van het abonnement. 
  
  De parameters voor deze functie zijn hetzelfde als de HTTP-trigger.

Hier worden de uitvoer van de HTTPWebhook activeren en de inhoud van de binnenkomende aanvraag zijn:
  
| Elementnaam | Type | Beschrijving |
| ------------ | ---- | ----------- |
| headers | Object | De headers van de HTTP-antwoord | 
| hoofdtekst | Object | De hoofdtekst van het HTTP-antwoord | 
|||| 

## <a name="conditions"></a>Voorwaarden  

U kunt een of meer voorwaarden om te bepalen of de werkstroom moet worden uitgevoerd of niet gebruiken voor een trigger. Bijvoorbeeld:  

```json
"myDailyReportTrigger": {
    "type": "Recurrence",
    "conditions": [ 
        {
            "expression": "@parameters('sendReports')"
        } 
    ],
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

In dit geval wordt het rapport alleen triggers terwijl de werkstroom `sendReports` parameter is ingesteld op true. Ten slotte voorwaarden kunnen verwijzen naar de statuscode van de trigger. U kunt bijvoorbeeld een werkstroom starten alleen als uw website een statuscode 500, bijvoorbeeld retourneert:
  
``` json
"conditions": [ 
    {  
      "expression": "@equals(triggers().code, 'InternalServerError')"  
    }  
]  
```  
  
> [!NOTE]  
> Als een expressie verwijst naar een trigger statuscode in op een manier, wordt het standaardgedrag die trigger op de 200 'OK', vervangen. Bijvoorbeeld, als u activeren op zowel de statuscode 200 als de statuscode 201 wilt, hebt u omvatten: `@or(equals(triggers().code, 200),equals(triggers().code,201))` als de voorwaarde.
  
## <a name="start-multiple-runs-for-a-request"></a>Meerdere wordt uitgevoerd voor een aanvraag starten

Activeer meerdere wordt uitgevoerd voor een enkele aanvraag `splitOn` is nuttig, bijvoorbeeld wanneer u wilt pollen een eindpunt dat meerdere nieuwe items tussen polling-intervallen kan hebben.
  
Met `splitOn`, geeft u de eigenschap binnen de nettolading van antwoord met de matrix van items die u gebruiken wilt voor het starten van een uitvoering van de trigger. Stel dat u hebt een API die dit antwoord geretourneerd:  
  
```json
{
    "status": "Succeeded",
    "rows": [
        {  
            "id" : 938109380,
            "name" : "myFirstRow"
        },
        {
            "id" : 938109381,
            "name" : "mySecondRow"
        }
    ]
}
```
  
Uw logische app hoeft alleen de `rows` inhoud, zodat u de trigger zoals in dit voorbeeld kunt maken:  

```json
"mySplitterTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "minute",
        "interval": 1
    },
    "intputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.rows"
}
```
> [!NOTE]  
> Als u de `SplitOn` uitvoert, en u de eigenschappen die zich buiten de matrix kan niet ophalen voor dit voorbeeld er dus kan niet de `status` eigenschap in het antwoord geretourneerd van de API.
> Ook in dit voorbeeld gebruiken we de `?` operator zodat er een storing vermijden kunt als de `rows` eigenschap bestaat niet. 

Dus in de werkstroomdefinitie `@triggerBody().name` retourneert `myFirstRow` voor het eerst wordt uitgevoerd, en `mySecondRow` voor de tweede uitvoeren. De trigger uitvoer eruit zien in dit voorbeeld:  

```json
{
    "body": {
        "id": 938109380,
        "name": "mySecondRow"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "mySecondRow"
    }
}
```
  
## <a name="single-run-instance"></a>SIS uitvoeren

U kunt terugkeerpatroon triggers configureren zodat ze alleen wanneer alle actieve sessies hebt gestart. Als een geplande terugkeer gebeurt terwijl werkstroomexemplaar wordt uitgevoerd, wordt de trigger wordt overgeslagen en wacht tot het volgende geplande terugkeerpatroon opnieuw te controleren.
Om deze instelling configureert, stel de `operationOptions` eigenschap `singleInstance`:

```json
"triggers": {
    "myHTTPTrigger": {
        "type": "Http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="actions-overview"></a>Overzicht van de acties

Er zijn veel verschillende soorten acties, elk met unieke gedrag. Elk actietype heeft verschillende invoerwaarden die een actie gedrag definiëren. Verzameling acties kunnen veel andere acties in zichzelf bevatten. 

### <a name="standard-actions"></a>Standaardacties  

| Actietype | Beschrijving | 
| ----------- | ----------- | 
| **HTTP** | Hiermee wordt een HTTP-web-eindpunt. | 
| **ApiConnection**  | Net als bij de HTTP-actie, maar gebruikt [Microsoft beheerde API's](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | Werkt zoals HTTPWebhook, maar gebruikt door Microsoft beheerde API's. | 
| **Antwoord** | Hiermee definieert u de reactie voor een inkomend gesprek. | 
| **Functie** | Hiermee geeft u een Azure-functie. | 
| **Wacht** | Wacht een vast bedrag tijd of totdat een bepaalde tijd. | 
| **Werkstroom** | Hiermee geeft u een geneste werkstroom. | 
| **Opstellen** | Hiermee wordt een object arbitary uit de invoer van de actie. | 
| **Query** | Een matrix die op basis van een voorwaarde filtert. | 
| **Selecteren** | Elk element van een matrix projecteert in een nieuwe waarde. U kunt bijvoorbeeld een matrix van getallen omzetten in een matrix met objecten. | 
| **Tabel** | Converteert een matrix van items naar een CSV of HTML-tabel. | 
| **Beëindigen** | Een werkstroom wordt gestopt. | 
||| 

### <a name="collection-actions"></a>Verzameling acties

| Actietype | Beschrijving | 
| ----------- | ----------- | 
| **Voorwaarde** | Evalueert een expressie en op basis van het resultaat, de bijbehorende vertakking wordt uitgevoerd. | 
| **Bereik** | Gebruik dit voor andere acties logisch groeperen. | 
| **ForEach** | Deze actie samenvoegartikel een matrix doorlopen en interne acties uitvoert op elk matrixitem. | 
| **Pas** | Deze actie samenvoegartikel worden interne acties uitgevoerd totdat een voorwaarde in waar resulteert. | 
||| 

## <a name="http-action"></a>HTTP-actie  

Acties voor HTTP-aanroepen van een opgegeven eindpunt en controleren van het antwoord om te bepalen of de werkstroom moet worden uitgevoerd. Bijvoorbeeld:
  
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
| Methode | Ja | Tekenreeks | Maakt gebruik van een van deze methoden HTTP: 'Ophalen', 'Posten', 'Plaatsen', 'Verwijderen', 'PATCH' of 'HEAD' | 
| URI | Ja| Tekenreeks | De HTTP of HTTPs-eindpunt dat de trigger wordt gecontroleerd. Maximale grootte van tekenreeks: 2 KB | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nee | Tekenreeks | Definieert de set met speciaal gedrag negeren. | 
| Verificatie | Nee | Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). <p>Afgezien van Scheduler, er is een meer ondersteunde eigenschap: `authority`. Deze waarde is standaard `https://login.windows.net` wanneer niet wordt opgegeven, maar u kunt een andere waarde, zoals`https://login.windows\-ppe.net`. | 
||||| 

In dit voorbeeld HTTP-actie pogingen ophalen van het laatste nieuws twee keer als er onregelmatige problemen voor een totaal van drie uitvoeringen en een vertraging van 30 seconden tussen elke poging:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

Het interval voor opnieuw proberen is opgegeven in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601). Dit interval heeft een waarde standaard en een minimum van 20 seconden, terwijl de maximale waarde een uur is. Het standaard- en maximum aantal is gelijk aan vier uur. Als u de definitie van het beleid voor opnieuw proberen niet opgeeft een `fixed` strategie met een standaardwaarde opnieuw telling en het interval wordt gebruikt. Als wilt uitschakelen in het beleid voor opnieuw proberen, stelt u het type in op `None`.

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

U kunt de duur van een asynchrone patroon voor een bepaald tijdsinterval beperken. Als het tijdsinterval is verstreken zonder dat een definitieve status bereikt, de status van de actie is gemarkeerd `Cancelled` met een `ActionTimedOut` code. De time-out voor de limiet is opgegeven in de ISO 8601-notatie. U kunt limieten opgeven als volgt te werk:

``` json
"action-name": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>APIConnection actie

De actie APIConnection verwijst naar een connector door Microsoft beheerd. Deze actie vereist een verwijzing naar een geldige verbinding en informatie over de API en de parameters.
Hier volgt een voorbeeld APIConnection actie:

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
| host | Ja | Object | Hiermee geeft u de connector-informatie, zoals de `runtimeUrl` en verwijzing naar het object connection. | 
| Methode | Ja | Tekenreeks | Maakt gebruik van een van deze methoden HTTP: 'Ophalen', 'Posten', 'Plaatsen', 'Verwijderen', 'PATCH' of 'HEAD' | 
| Pad | Ja | Tekenreeks | Het pad voor de API-bewerking | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nee | Tekenreeks | Definieert de set met speciaal gedrag negeren. | 
| Verificatie | Nee | Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
||||| 

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
| host | Ja | Object | Hiermee geeft u de connector-informatie, zoals de `runtimeUrl` en verwijzing naar het object connection. | 
| Pad | Ja | Tekenreeks | Het pad voor de API-bewerking | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
| retryPolicy | Nee | Object | Dit object gebruiken voor het aanpassen van het gedrag voor opnieuw proberen voor 4xx of 5xx-fouten. Zie voor meer informatie [beleid probeer](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Nee | Tekenreeks | Definieert de set met speciaal gedrag negeren. | 
| Verificatie | Nee | Object | Hiermee geeft u de methode die voor de aanvraag wordt gebruikt voor verificatie. Zie voor meer informatie [Scheduler uitgaande verificatie](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Antwoord actie  

Met deze actie de nettolading van het volledige antwoord van een HTTP-aanvraag bevat en bevat een `statusCode`, `body`, en `headers`:
  
```json
"myResponseAction": {
    "type": "response",
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

## <a name="function-action"></a>Functie actie   

Deze actie kunt u vertegenwoordigen en aanroep een [Azure functie](../azure-functions/functions-overview.md), bijvoorbeeld:

```json
"my-Azure-Function-name": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Web/sites/{your-Azure-function-app-name}/functions/{your-Azure-function-name}"
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
| functie-id | Ja | Tekenreeks | De resource-ID voor de Azure-functie die u wilt aanroepen. | 
| Methode | Nee | Tekenreeks | De HTTP-methode die wordt gebruikt voor het aanroepen van de functie. Als niet wordt opgegeven, is "POST" de standaardmethode. | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
|||||

Wanneer u uw logische app opslaat, controleert Azure Logic Apps op de functie waarnaar wordt verwezen:

* U moet toegang hebben tot de functie.
* U kunt alleen de standaard HTTP-triggers of algemene JSON webhook triggers gebruiken.
* De functie mag geen enkele route gedefinieerd.
* Alleen 'functioneren' en 'anonymous' autorisatieniveau is toegestaan.

De URL van de trigger is opgehaald, in de cache opgeslagen en gebruikt tijdens runtime. Dus als een bewerking wordt ongeldig gemaakt van de URL in de cache, mislukt de actie tijdens runtime. U kunt dit probleem omzeilen, sla de logische app opnieuw, waardoor de logische app op te halen en de trigger-URL opnieuw in de cache.

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
> De duur van de wachttijd kan worden ofwel opgegeven met de `until` object of `interval` , maar niet beide.
  
| Elementnaam | Vereist | Type | Beschrijving | 
| ------------ | -------- | ---- | ----------- | 
| pas | Nee | Object | De duur van de wachttijd op basis van een punt in tijd | 
| Pas de timestamp | Ja | Tekenreeks | Punt in tijd in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) wanneer de wachttijd is verlopen | 
| interval | Nee | Object | De duur van de wachttijd op basis van de intervaleenheid en het aantal | 
| intervaleenheid | Ja | Tekenreeks | Tijdseenheid. Gebruik slechts één van deze waarden: 'tweede', 'minuut', 'uur', 'day', 'week' of 'maand' | 
| interval aantal | Ja | Geheel getal | Een positief geheel getal dat het aantal gebruikt voor de duur van de wachttijd intervaleenheden | 
||||| 

## <a name="workflow-action"></a>Werkstroomactie   

Deze actie vertegenwoordigt een andere werkstroom. Logic Apps voert een toegangscontrole op de werkstroom of een meer specifiek, de trigger, wat betekent dat u moet toegang hebben tot de workflow.

De uitvoer van deze actie zijn gebaseerd op wat u definieert in de `response` actie voor de onderliggende werkstroom. Als u dit nog niet hebt gedefinieerd een `response` actie wordt de uitvoer is leeg.

```json
"myNestedWorkflowAction": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
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
| host-id | Ja | Tekenreeks| De resource-ID voor de werkstroom die u wilt aanroepen | 
| host triggernaam | Ja | Tekenreeks | De naam van de trigger die u wilt aanroepen | 
| Query 's | Nee | Object | Hiermee geeft u de queryparameters die u wilt opnemen in de URL. <p>Bijvoorbeeld: `"queries": { "api-version": "2015-02-01" }` voegt `?api-version=2015-02-01` naar de URL. | 
| headers | Nee | Object | Hiermee geeft u elke koptekst die in de aanvraag verzonden. <p>Als u bijvoorbeeld de taal instellen en typt u op een aanvraag: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| hoofdtekst | Nee | Object | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. | 
|||||   

## <a name="compose-action"></a>Actie opstellen

Deze actie kunt u een willekeurig object samenstellen en de uitvoer is het resultaat van evaluatie van de invoer van de actie. 

> [!NOTE]
> U kunt de `Compose` actie voor het maken van uitvoer, met inbegrip van objecten, matrices en een ander type systeemeigen worden ondersteund door logische apps zoals XML en binary.

Bijvoorbeeld, kunt u de actie opstellen voor het samenvoegen van de uitvoer van meerdere acties:

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

## <a name="select-action"></a>Selecteer actie

Deze actie kunt u elk element van een matrix project in een nieuwe waarde.
Bijvoorbeeld, als u wilt converteren van een matrix van getallen in een matrix met objecten, kunt u gebruiken:

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

## <a name="query-action"></a>Queryactie

Deze actie kunt u filteren op basis van een voorwaarde matrix. In dit voorbeeld worden getallen die groter is dan twee geselecteerd:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

De uitvoer van de `query` actie is een matrix met elementen van de invoermatrix die voldoen aan de voorwaarde.

> [!NOTE]
> Als geen waarden voldoen aan de `where` voorwaarde, het resultaat is een lege matrix.

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| uit | Ja | Matrix | De bronmatrix |
| waar | Ja | Tekenreeks | De voorwaarde die wordt toegepast op elk element van de bronmatrix |
||||| 

## <a name="table-action"></a>Tabel actie

Deze actie kunt u bij het converteren van een matrix van items in een **CSV** of **HTML** tabel. Stel bijvoorbeeld dat u hebt een `@triggerBody()` met deze matrix:

```json
[ 
    {
      "id": 0,
      "name": "apples"
    },
    {
      "id": 1, 
      "name": "oranges"
    }
]
```

En definiëren van een tabel in te grijpen als volgt:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

Het resultaat van dit voorbeeld lijkt op deze HTML-tabel: 

<table><thead><tr><th>id</th><th>naam</th></tr></thead><tbody><tr><td>0</td><td>appels</td></tr><tr><td>1</td><td>appels</td></tr></tbody></table>

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

Het resultaat van dit voorbeeld lijkt op deze HTML-tabel: 

<table><thead><tr><th>ID maken</th><th>Beschrijving</th></tr></thead><tbody><tr><td>0</td><td>nieuwe appels</td></tr><tr><td>1</td><td>nieuwe appels</td></tr></tbody></table>

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| uit | Ja | Matrix | De bronmatrix. Als de `from` waarde van de eigenschap is een lege matrix, de uitvoer is een lege tabel. | 
| Indeling | Ja | Tekenreeks | De tabelindeling die u, ofwel wilt **CSV** of **HTML** | 
| Kolommen | Nee | Matrix | De tabelkolommen die u wilt. Met de vorm van de tabel standaard wordt genegeerd. | 
| kolomkop | Nee | Tekenreeks | De kolomkop | 
| waarde in de kolom | Ja | Tekenreeks | De waarde in de kolom | 
||||| 

## <a name="terminate-action"></a>Actie beëindigen

Deze actie Hiermee stopt u de werkstroom uitvoeren, annuleert onderweg acties en slaat eventuele resterende acties. De actie beëindigen heeft geen invloed op alle voltooide acties.

Bijvoorbeeld, als u wilt stoppen run dat '' status mislukt is, kunt u dit voorbeeld:

```json
"handleUnexpectedResponseAction": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response"
        }
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Ja | Tekenreeks | Het doel uitvoert de status een is `Failed` of`Cancelled` |
| runError | Nee | Object | De details van de fout. Ondersteunde alleen wanneer `runStatus` is ingesteld op `Failed`. |
| runError code | Nee | Tekenreeks | De run foutcode: |
| runError bericht | Nee | Tekenreeks | Foutbericht van de uitvoering |
||||| 

## <a name="collection-actions-overview"></a>Overzicht van de verzameling acties

Sommige handelingen kunnen acties in zichzelf bevatten. Referentie-acties in een verzameling kunnen worden verwezen rechtstreeks buiten de verzameling. Bijvoorbeeld, als u definiëren `Http` in een `scope`, klikt u vervolgens `@body('http')` nog geldig overal in de werkstroom. Zijn er acties in een verzameling `runAfter` alleen in combinatie met andere acties in dezelfde verzameling.

## <a name="condition-if-action"></a>Voorwaarde: Als actie

Deze actie kunt u een voorwaarde evalueren en uitvoeren van een vertakking op basis van of de expressie resulteert in `true`. 
  
```json
"myCondition": {
    "type": "If",
    "actions": {
        "if_true": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {}
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {}
}
``` 

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| acties | Ja | Object | De interne acties uit te voeren wanneer `expression` resulteert in`true` | 
| expressie | Ja | Tekenreeks | De expressie om te evalueren |
| anders | Nee | Object | De interne acties uit te voeren wanneer `expression` resulteert in`false` |
||||| 

Als de voorwaarde is geëvalueerd, de voorwaarde is gemarkeerd als `Succeeded`. Acties in ofwel de `actions` of `else` objecten worden geëvalueerd tot: 

* `Succeeded`wanneer ze worden uitgevoerd en mislukt
* `Failed`wanneer ze worden uitgevoerd en mislukt
* `Skipped`Wanneer de respectieve vertakking niet wordt uitgevoerd

Hier volgen voorbeelden die laten zien hoe voorwaarden expressies in een actie kunnen gebruiken:
  
| JSON-waarde | Resultaat | 
| ---------- | -------| 
| `"expression": "@parameters('hasSpecialAction')"` | Een waarde die wordt geëvalueerd op true, zal dit probleem op te geven. Ondersteunt alleen Booleaanse expressies. Als u wilt converteren andere typen naar Booleaanse waarde, gebruik van deze functies: `empty` en`equals` | 
| `"expression": "@greater(actions('act1').output.value, parameters('threshold'))"` | Vergelijking van functies ondersteunt. Bijvoorbeeld, de actie kan alleen worden uitgevoerd wanneer de uitvoer van `act1` is groter dan de drempelwaarde. | 
| `"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"` | Biedt ondersteuning voor bedrijfslogica-functies voor het maken van geneste Booleaanse expressies. In dit voorbeeld wordt de actie uitgevoerd als de uitvoer van `act1` boven de drempelwaarde of onder de 100 is. | 
| `"expression": "@equals(length(actions('act1').outputs.errors), 0))"` | Om te controleren dat of een matrix geen objecten heeft, kunt u matrixfuncties. In dit voorbeeld wordt de actie uitgevoerd als de `errors` matrix leeg is. | 
| `"expression": "parameters('hasSpecialAction')"` | Fout, niet een geldig voorwaarde omdat @ is vereist voor voorwaarden. |  
|||

## <a name="scope-action"></a>Bereikactie

Deze actie kunt u logische groep acties in een werkstroom.

```json
"myScope": {
    "type": "Scope",
    "actions": {
        "call_bing": {
            "type": "Http",
             "inputs": {
                "url": "http://www.bing.com"
            }
        }
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- |  
| acties | Ja | Object | De interne acties uit te voeren binnen het bereik |
||||| 

## <a name="foreach-action"></a>ForEach-actie

Deze actie samenvoegartikel een matrix doorlopen en interne acties uitvoert op elk matrixitem. Standaard de `foreach` lus wordt parallel uitgevoerd en 20 uitvoeringen parallel tegelijk kunt uitvoeren. U stelt regels worden uitgevoerd met de `operationOptions` parameter.

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
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| acties | Ja | Object | De interne acties uit te voeren binnen de lus | 
| foreach | Ja | Tekenreeks | De matrix te doorlopen | 
| operationOptions | Nee | Tekenreeks | Hiermee geeft u alle opties voor de bewerking voor het aanpassen van gedrag. Ondersteunt momenteel alleen `Sequential` voor opeenvolgend uitgevoerd iteraties waar het standaardgedrag parallelle is. |
||||| 

## <a name="until-action"></a>Totdat de actie

Deze actie samenvoegartikel binnenste acties uitgevoerd tot aan een voorwaarde resultaten op true.

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
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| Naam | Vereist | Type | Beschrijving | 
| ---- | -------- | ---- | ----------- | 
| acties | Ja | Object | De interne acties uit te voeren binnen de lus | 
| expressie | Ja | Tekenreeks | De expressie na elke iteratie evalueren | 
| Limiet | Ja | Object | De limieten voor de lus. Moet ten minste één limiet definiëren. | 
| aantal | Nee | Geheel getal | De limiet voor het aantal iteraties om uit te voeren | 
| timeout | Nee | Tekenreeks | De time-outlimiet in [ISO 8601-notatie](https://en.wikipedia.org/wiki/ISO_8601) die specificeert hoe lang de lus moet worden uitgevoerd |
||||| 

## <a name="next-steps"></a>Volgende stappen

* [Definitietaal van workflow](../logic-apps/logic-apps-workflow-definition-language.md)
* [Werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows)

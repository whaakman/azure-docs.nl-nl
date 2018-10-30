---
title: Bereiken die worden uitgevoerd op basis van status van de groep - Azure Logic Apps acties toevoegen | Microsoft Docs
description: Over het maken van de bereiken die werkstroomacties op basis van status van de actie groep in Azure Logic Apps worden uitgevoerd
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.reviewer: klam, LADocs
ms.date: 10/03/2018
ms.topic: article
ms.openlocfilehash: aac59e087ba106bc20d94fea85cb8a3cd9273482
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233069"
---
# <a name="run-actions-based-on-group-status-with-scopes-in-azure-logic-apps"></a>Uitvoeren van acties op basis van status van de groep met een bereik in Azure Logic Apps

Als u wilt acties worden uitgevoerd nadat een andere groep met acties slagen of mislukken, groep personen deze acties binnen een *bereik*. Deze structuur is handig als u wilt organiseren acties als een logische groep, evalueren de status van die groep en acties uitvoeren die zijn gebaseerd op de status van de scope. Nadat alle acties in een bereik uitgevoerd hebt, wordt het bereik ook een eigen status. Bijvoorbeeld, kunt u bereiken als u wilt implementeren [uitzondering en foutafhandeling](../logic-apps/logic-apps-exception-handling.md#scopes). 

Om te controleren of de status van een scope, kunt u dezelfde criteria die u gebruikt om te bepalen van een logische apps uitvoeringsstatus, zoals "Geslaagd", "Mislukt", "Geannuleerd", enzovoort. Standaard wanneer de acties van het bereik is gelukt, is het bereik van de status gemarkeerd als 'Voltooid'. Maar als een actie binnen het bereik is mislukt of is geannuleerd, de status van de scope is gemarkeerd als 'Mislukt'. Zie voor de limieten voor scopes [limieten en configuratie](../logic-apps/logic-apps-limits-and-config.md). 

Dit is bijvoorbeeld een op hoog niveau logische app die gebruikmaakt van een scope specifieke acties en een voorwaarde om te controleren of de status van het bereik uit te voeren. Als alle acties in de scope mislukken of onverwacht wordt beëindigd, het bereik is gemarkeerd als 'Mislukt' of "Afgebroken" respectievelijk en de logische app verzendt een bericht 'Bereik is mislukt'. Als alle acties die binnen het bereik is gelukt, verzendt de logische app een bericht 'Bereik geslaagd'.

![Instellen van de trigger "Planning--terugkeerpatroon"](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen in het voorbeeld in dit artikel, moet u deze items:

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Een e-mailaccount van een e-mailprovider die door Logic Apps worden ondersteund. In dit voorbeeld maakt gebruik van Outlook.com. Als u een andere provider gebruikt, wordt de algemene stroom blijft hetzelfde, maar uw gebruikersinterface wordt weergegeven op een andere.

* Een sleutel voor Bing kaarten. Als u deze sleutel, Zie <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">ophalen van een sleutel voor Bing kaarten</a>.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-sample-logic-app"></a>Voorbeeld van logische app maken

Maak eerst deze voorbeeld-logica-app zodat u kunt later een bereik toevoegen:

![Voorbeeld van logische app maken](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* Een **planning - terugkeerpatroon** trigger waarmee wordt gecontroleerd of de Bing Maps-service met een interval dat u opgeeft
* Een **Bing kaarten - route ophalen** actie die controleert op de reistijd tussen twee locaties
* Een voorwaarde waarmee wordt gecontroleerd of de reistijd uw opgegeven reistijd overschrijdt
* Een actie die wordt verzonden e-die actuele reistijd groter is dan de opgegeven periode

U kunt uw logische app op elk gewenst moment opslaan, zo vaak Sla uw werk.

1. Aanmelden bij de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, als u dat nog niet gedaan hebt. Een lege, logische app maken.

1. Voeg de **planning - terugkeerpatroon** trigger met de volgende instellingen: **Interval** = "1" en **frequentie** = 'Minuut'

   ![Instellen van de trigger "Planning--terugkeerpatroon"](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Visueel uw weergave te vereenvoudigen en de details van elke actie in de ontwerpfunctie verbergen, samenvouwen van elke actie vorm als u wordt tijdens de volgende stappen hebt uitgevoerd.

1. Voeg de **Bing kaarten - route ophalen** actie. 

   1. Als u nog een verbinding met Bing kaarten hebt, wordt u gevraagd om een verbinding te maken.

      | Instelling | Waarde | Beschrijving |
      | ------- | ----- | ----------- |
      | **Verbindingsnaam** | BingMapsConnection | Geef een naam op voor uw verbinding. | 
      | **API-sleutel** | <*your-Bing-Maps-key*> | Voer de sleutel voor Bing Kaarten in die u eerder hebt ontvangen. | 
      ||||  

   1. Instellen van uw **route ophalen** actie zoals wordt weergegeven in de tabel onder deze afbeelding:

      ![Instellen van "Bing kaarten - route ophalen" actie](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Zie [Een route berekenen](https://msdn.microsoft.com/library/ff701717.aspx) voor meer informatie over deze parameters.

      | Instelling | Waarde | Beschrijving |
      | ------- | ----- | ----------- |
      | **Routepunt 1** | <*Start*> | Voer beginpunt van uw route. | 
      | **Routepunt 2** | <*end*> | Voer de bestemming van uw route. | 
      | **Vermijden** | Geen | Voer items om te voorkomen op uw route, zoals snelwegen, tolwegen, enzovoort. Zie voor mogelijke waarden [een route berekenen](https://msdn.microsoft.com/library/ff701717.aspx). | 
      | **Optimaliseren** | timeWithTraffic | Selecteer een parameter voor het optimaliseren van uw route, zoals afstand, tijd met informatie over de huidige gegevensverkeer, enzovoort. In dit voorbeeld maakt gebruik van deze waarde: "timeWithTraffic" | 
      | **Afstandseenheid** | <*your-preference*> | Voer de Afstandseenheid die voor het berekenen van uw route. In dit voorbeeld maakt gebruik van deze waarde: "Mijl" | 
      | **Vervoermiddel** | Auto | Geef de vervoersoptie voor uw route. In dit voorbeeld wordt deze waarde "Aangedreven" | 
      | **Datum/tijd openbaar vervoer** | Geen | Van toepassing op het vervoermiddel openbaar alleen. | 
      | **Doorvoer datumtype Type** | Geen | Van toepassing op het vervoermiddel openbaar alleen. | 
      ||||  

1. [Een voorwaarde toevoegen](../logic-apps/logic-apps-control-flow-conditional-statement.md) die controleert of de actuele reistijd met verkeer hoger is dan een opgegeven periode. Volg deze stappen in dit voorbeeld:

   1. Wijzig de naam van de voorwaarde in deze beschrijving: **als verkeer tijd meer dan een opgegeven periode**

   1. Klik in de meest linkse kolom in de **een waarde kiezen** vak, zodat de lijst met dynamische inhoud wordt weergegeven. Uit de lijst, selecteer de **reistijd in huidige verkeerssituatie** veld, in seconden. 

      ![Voorwaarde bouwen](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Selecteer in het middelste vak, deze operator: **is groter dan**

   1. Geef deze vergelijkingswaarde, in seconden en equivlent tot 10 minuten in de meest rechtse kolom: **600**

      Wanneer u klaar bent, ziet de voorwaarde eruit zoals in dit voorbeeld:

      ![Voltooide voorwaarde](./media/logic-apps-control-flow-run-steps-group-scopes/finished-condition.png)

1. In de **als de waarde true** vertakking, een actie 'e-mail verzenden' toevoegen voor uw e-mailprovider. Instellen van deze actie met de volgende stappen in deze afbeelding:

   ![Actie 'Een e-mail verzenden' toevoegen 'indien waar' vertakking](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. In de **naar** en voer uw e-mailadres voor testdoeleinden.

   1. In de **onderwerp** veld, voert u deze tekst:

      ```Time to leave: Traffic more than 10 minutes```

   1. In de **hoofdtekst** veld, voert u deze tekst met een spatie: 

      ```Travel time: ```

      Terwijl de cursor wordt weergegeven in de **hoofdtekst** veld, de lijst met dynamische inhoud blijft geopend zodat u de parameters die beschikbaar op dit moment zijn kunt selecteren.

   1. Kies in de lijst met dynamische inhoud voor **Expressie**.

   1. Zoek en selecteer de **div()** functie. 
   Plaats de cursor in van de functie haakjes.

   1. Terwijl de cursor van de functie haakjes is, kies **dynamische inhoud** zodat de lijst met dynamische inhoud wordt weergegeven. 
   
   1. Uit de **route ophalen** sectie, selecteer de **verkeer verkeerssituatie** veld.

      ![Selecteer "Verkeer verkeerssituatie"](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   1. Nadat het veld is omgezet naar JSON-indeling, voegt u toe een **met door komma's** (```,```) gevolgd door het aantal ```60``` zodat u de waarde in converteren **verkeer verkeerssituatie** van seconden naar minuten. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      De expressie ziet er nu uit zoals in dit voorbeeld:

      ![Expressie voltooien](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   1. Wanneer u klaar bent, kiest u **OK**.

  1. Nadat de expressie wordt omgezet, deze tekst met een toonaangevende spatie toevoegen: ``` minutes```
  
     Uw **hoofdtekst** veld ziet er nu uit zoals in dit voorbeeld:

     ![Veld is voltooid 'Hoofdtekst'](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)

1. Sla uw logische app op.

Voeg vervolgens een bereik zodat u kunt specifieke acties te groeperen en hun status evalueren.

## <a name="add-a-scope"></a>Een bereik toevoegen

1. Als u niet hebt gedaan, opent u uw logische app in Logic App Designer. 

1. Een bereik op de locatie van de werkstroom die u wilt toevoegen. Als u wilt toevoegen een bereik tussen bestaande stappen in de werkstroom voor logische Apps, als volgt te werk: 

   1. Plaats de muisaanwijzer boven de pijl waar u wilt toevoegen van het bereik. 
   Kies de **plusteken** (**+**) > **een actie toevoegen**.

      ![Een bereik toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

   1. Typ 'bereik' als filter in het zoekvak. 
   Selecteer de **bereik** actie.

## <a name="add-steps-to-scope"></a>Voeg stappen toe aan het bereik

1. Nu de stappen toevoegen of bestaande stappen die u wilt uitvoeren binnen het bereik te slepen. Voor dit voorbeeld, sleept u deze acties binnen het bereik:
      
   * **Route ophalen**
   * **Als verkeer tijd groter dan de opgegeven tijd is**, waaronder zowel de **waar** en **false** vertakkingen

   Uw logische app ziet er nu uit zoals in dit voorbeeld:

   ![Bereik toegevoegd](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

1. Onder het bereik, moet u een voorwaarde waarmee wordt gecontroleerd of de status van de scope toevoegen. Wijzig de naam van de voorwaarde in deze beschrijving: **als bereik is mislukt**

   ![Voorwaarde om te controleren of de status van het bereik toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
1. In de voorwaarde, voegt u deze expressies te controleren of de status van het bereik gelijk aan 'Mislukt' of 'Afgebroken is'. 

   1. Als u wilt toevoegen van een nieuwe rij, kies **toevoegen**. 

   1. Klik in elke rij in het vak links, zodat de lijst met dynamische inhoud wordt weergegeven. 
   Kies in de lijst met dynamische inhoud, **expressie**. Voer deze expressie in het invoervak en kies vervolgens **OK**: 
   
      `result('Scope')[0]['status']`

      ![Expressie waarmee wordt gecontroleerd of de status van de scope toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/check-scope-status.png)

   1. Selecteer voor beide rijen **is gelijk aan** als de operator. 
   
   1. Voer voor de van de vergelijkingswaarden in de eerste rij `Failed`. 
   Voer in de tweede rij `Aborted`. 

      Wanneer u klaar bent, ziet de voorwaarde eruit zoals in dit voorbeeld:

      ![Expressie waarmee wordt gecontroleerd of de status van de scope toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/check-scope-status-finished.png)

      Stel nu van de voorwaarde `runAfter` , zodat de voorwaarde de status van het bereik controleert en de bijbehorende actie wordt uitgevoerd die u definieert in latere stappen.

   1. Op de **als bereik is mislukt** voorwaarde, kiest u de **weglatingsteken** (...) knop en kies vervolgens **uitvoeren na configureren**.

      ![De eigenschap 'runAfter' configureren](./media/logic-apps-control-flow-run-steps-group-scopes/configure-run-after.png)

   1. Selecteer alle statussen van deze scope: **is geslaagd**, **is mislukt**, **wordt overgeslagen**, en **is een time-out**

      ![Selecteer de statussen van het bereik](./media/logic-apps-control-flow-run-steps-group-scopes/select-run-after-statuses.png)

   1. Wanneer u klaar bent, kiest u **Gereed**. 
   De voorwaarde wordt nu een 'gegevens'-pictogram weergegeven.

1. In de **als de waarde true** en **indien onwaar** vertakkingen, voeg de acties die u wilt uitvoeren op basis van elke omvang aan te status, bijvoorbeeld een e-mailadres of een bericht verzenden.

   ![Acties te ondernemen op basis van status van de scope toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

1. Sla uw logische app op.

Uw voltooide logische app ziet er nu uit zoals in dit voorbeeld:

![Voltooide logische app met een bereik](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>Testen van uw werk

Kies op de werkbalk van de ontwerper **uitvoeren**. Als alle acties die binnen het bereik is gelukt, krijgt u een bericht 'Bereik geslaagd'. Als alle acties binnen het bereik niet is gelukt, krijgt u een bericht 'Bereik is mislukt'. 

<a name="scopes-json"></a>

## <a name="json-definition"></a>JSON-definitie

Als u in de codeweergave werkt, kunt u in plaats daarvan de structuur van een bereik definiëren in de JSON-definitie van uw logische app. Dit is bijvoorbeeld de JSON-definitie voor triggers en acties in de vorige logische app:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    }
  }
}
```

```json
"actions": {
  "If_scope_failed": {
    "type": "If",
    "actions": {
      "Scope_failed": {
        "type": "ApiConnection",
        "inputs": {
          "body": {
            "Body": "Scope failed. Scope status: @{result('Scope')[0]['status']}",
            "Subject": "Scope failed",
            "To": "<your-email@domain.com>"
          },
          "host": {
            "connection": {
              "name": "@parameters('$connections')['outlook']['connectionId']"
            }
          },
          "method": "post",
          "path": "/Mail"
        },
        "runAfter": {}
      }
    },
    "else": {
      "actions": {
        "Scope_succeded": {
          "type": "ApiConnection",
          "inputs": {
            "body": {
              "Body": "Scope succeeded. Scope status: @{result('Scope')[0]['status']}",
              "Subject": "Scope succeeded",
              "To": "<your-email@domain.com>"
            },
            "host": {
              "connection": {
               "name": "@parameters('$connections')['outlook']['connectionId']"
              }
            },
            "method": "post",
            "path": "/Mail"
          },
          "runAfter": {}
        }
      }
    },
    "expression": {
      "or": [ 
         {
            "equals": [ 
              "@result('Scope')[0]['status']", 
              "Failed"
            ]
         },
         {
            "equals": [
               "@result('Scope')[0]['status']", 
               "Aborted"
            ]
         } 
      ]
    },
    "runAfter": {
      "Scope": [
        "Failed",
        "Skipped",
        "Succeeded",
        "TimedOut"
      ]
    }
  },
  "Scope": {
    "type": "Scope",
    "actions": {
      "Get_route": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['bingmaps']['connectionId']"
            }
          },
          "method": "get",
          "path": "/REST/V1/Routes/Driving",
          "queries": {
            "distanceUnit": "Mile",
            "optimize": "timeWithTraffic",
            "travelMode": "Driving",
            "wp.0": "<start>",
            "wp.1": "<end>"
          }
        },
        "runAfter": {}
      },
      "If_traffic_time_is_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'],60)} minutes",
                 "Subject": "Time to leave: Traffic more than 10 minutes",
                 "To": "<your-email@domain.com>"
              },
              "host": {
                "connection": {
                   "name": "@parameters('$connections')['outlook']['connectionId']"
                }
              },
              "method": "post",
              "path": "/Mail"
            },
            "runAfter": {}
          }
        },
        "expression": {
          "and" : [
            {
               "greater": [ 
                  "@body('Get_route')?['travelDurationTraffic']", 
                  600
               ]
            }
          ]
        },
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u wilt indienen of hierop stemmen op de functies en suggesties, gaat u naar de [site voor gebruikersfeedback van Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch-instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)

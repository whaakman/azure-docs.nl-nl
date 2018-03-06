---
title: Voer stappen op basis van status van het actie - gegroepeerd Azure Logic Apps | Microsoft Docs
description: Groep acties in scopes en stappen op basis van status van de groep uitvoeren
services: logic-apps
keywords: vertakkingen, parallel verwerken
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 052af45962f442e96ca28f05ffaa1b9814b2588b
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="scopes-run-steps-based-on-group-status-in-logic-apps"></a>Scopes: Uitgevoerd uit op basis van status van de groep in logic apps

Om uit te voeren stappen nadat een andere groep van acties slagen of mislukken, plaatsen die groep binnen een *bereik*. Deze structuur is handig als u wilt acties als een logische groep ordenen, evalueren de status van die groep en acties uitvoeren die zijn gebaseerd op de status van de scope. Nadat alle acties in een bereik uitgevoerd zijn, wordt het bereik ook een eigen status. Bijvoorbeeld, u kunt bereiken als u wilt implementeren [uitzondering en foutafhandeling](../logic-apps/logic-apps-exception-handling.md#scopes). 

Een scope om status te controleren, kunt u dezelfde criteria die u gebruikt om te bepalen een logic apps uitvoeren status, zoals 'Geslaagd', 'Is mislukt', 'Geannuleerd', enzovoort. Standaard, wanneer de acties van de scope is gelukt, status van de scope gemarkeerd als 'Voltooid'. Maar als geen actie in het bereik is mislukt of is geannuleerd, de status van de scope is gemarkeerd als 'Mislukt'. Zie voor de limieten voor scopes [limieten en config](../logic-apps/logic-apps-limits-and-config.md). 

Hier is bijvoorbeeld een op hoog niveau logische app die gebruikmaakt van een scope om uit te voeren van specifieke acties en een voorwaarde om het bereik van status te controleren. Als alle acties in het bereik mislukken of onverwacht wordt beëindigd, het bereik is gemarkeerd als 'Mislukt' of 'Afgebroken' respectievelijk en de logische app verzendt een bericht 'Bereik is mislukt'. Als de scoped acties is gelukt, verzendt de logische app een bericht 'Bereik is voltooid'.

!["Planning--Recurrence" trigger instellen](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Vereisten

Volg het voorbeeld in dit artikel, moet u deze items:

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/). 

* Een e-mailaccount van elke e-provider wordt ondersteund door logische Apps. In dit voorbeeld gebruikt een Outlook.com. Als u een andere provider gebruikt, verloopt in het algemeen blijft hetzelfde, maar de gebruikersinterface wordt weergegeven op een andere.

* Een sleutel Bing-kaarten. Als u deze sleutel, Zie <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">een Bing Maps-code</a>.

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-sample-logic-app"></a>Voorbeeld logische app maken

Maak eerst in dit voorbeeld logische app zodat u kunt later een scope toevoegen:

![Voorbeeld logische app maken](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* Een **schema - terugkeerpatroon** trigger waarmee wordt gecontroleerd of de Bing Maps-service met een interval dat u opgeeft
* Een **Bing Maps - Get-route** actie die de reizen tijd tussen twee locaties controleert
* Een voorwaarde waarmee wordt gecontroleerd of de tijd reizen uw tijd opgegeven reizen overschrijdt
* Een actie die u verzendt dat u e-die huidige reizen tijd groter is dan de opgegeven periode

U kunt uw logische app op elk gewenst moment opslaan, dus vaak Sla uw werk op.

1. Aanmelden bij de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, als u dat nog niet gedaan hebt. Een lege, logische app maken.

2. Voeg de **schema - terugkeerpatroon** trigger met deze instellingen: **Interval** = '1' en **frequentie** = 'Minuut'

   !["Planning--Recurrence" trigger instellen](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Samenvouwen om visueel vereenvoudigen de weergave details te verbergen elke actie in de ontwerpfunctie, elke actie vorm bij deze stappen.

3. Voeg de **Bing Maps - Get-route** in te grijpen. 

   1. Als u een verbinding met Bing Maps nog geen hebt, wordt u gevraagd om een verbinding te maken.

      | Instelling | Waarde | Beschrijving |
      | ------- | ----- | ----------- |
      | **Verbindingsnaam** | BingMapsConnection | Geef een naam op voor uw verbinding. | 
      | **API-sleutel** | <*your-Bing-Maps-key*> | Voer de sleutel voor Bing Kaarten in die u eerder hebt ontvangen. | 
      ||||  

   2. Instellen van uw **Get route** actie, zoals wordt weergegeven in de tabel hieronder deze installatiekopie:

      ![Instellen van 'Bing Maps - Get-route' actie](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Zie [Een route berekenen](https://msdn.microsoft.com/library/ff701717.aspx) voor meer informatie over deze parameters.

      | Instelling | Waarde | Beschrijving |
      | ------- | ----- | ----------- |
      | **Routepunt 1** | <*start*> | Voer uw route oorsprong. | 
      | **Routepunt 2** | <*end*> | Voer uw route bestemming. | 
      | **Vermijden** | Geen | Voer items om te voorkomen op uw route, zoals wegen, tolgelden, enzovoort. Zie voor mogelijke waarden [berekenen een route](https://msdn.microsoft.com/library/ff701717.aspx). | 
      | **Optimaliseren** | timeWithTraffic | Selecteer een parameter voor het optimaliseren van uw route toe, zoals afstand, tijd met de huidige gegevens, enzovoort. In dit voorbeeld wordt deze waarde: 'timeWithTraffic' | 
      | **Afstandseenheid** | <*your-preference*> | Geef de eenheid van afstand voor het berekenen van de route. In dit voorbeeld wordt deze waarde: 'Mijl' | 
      | **Vervoermiddel** | Auto | Geef de modus van reizen voor de route. In dit voorbeeld wordt deze waarde 'Aangedreven' | 
      | **Datum/tijd openbaar vervoer** | None | Van toepassing op alleen de modus voor de doorvoer. | 
      | **Doorvoer datumtype Type** | None | Van toepassing op alleen de modus voor de doorvoer. | 
      ||||  

4. Een voorwaarde toevoegen om te controleren of de huidige tijd reizen met verkeer groter is dan een opgegeven periode. Volg de stappen onder deze installatiekopie voor dit voorbeeld:

   ![Voorwaarde bouwen](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Wijzig de naam van de voorwaarde van deze beschrijving: **als verkeer wanneer meer dan een opgegeven periode**

   2. Selecteer in de lijst met parameters, de **reizen duur verkeer** veld, in seconden. 

   3. Selecteer deze operator voor de vergelijkingsoperator: **is groter dan**

   4. Voer voor de vergelijkingswaarde **600**, deze bevindt zich in seconden en is gelijk aan 10 minuten.

5. In de toestand **als de waarde true** vertakking, een actie 'e-mail verzenden' voor uw e-mailprovider toevoegen. Om deze actie met de details te stellen zoals wordt weergegeven in de tabel onder deze installatiekopie:

   ![Actie "E-mailbericht verzenden" toevoegen "als true" vertakking](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. Voor de **naar** en voer uw e-mailadres voor testdoeleinden.

   2. Voor de **onderwerp** en voer deze tekst:

      ```Time to leave: Traffic more than 10 minutes```

   3. Voor de **hoofdtekst** en voer deze tekst met een spatie: 

      ```Travel time: ```

      Terwijl de cursor wordt weergegeven in de **hoofdtekst** veld de lijst met dynamische inhoud blijft geopend zodat u de parameters die beschikbaar op dit moment zijn kunt selecteren.

   4. Kies in de lijst met dynamische inhoud voor **Expressie**.

   5. Zoek en selecteer de **div ()** functie.

   6. De cursor is de functie haakjes, kies **dynamische inhoud** zodat u kunt toevoegen de **verkeer duur verkeer** parameter volgende.

   7. Onder **Get route** in de lijst van de dynamische parameter, selecteer de **verkeer duur verkeer** veld.

      ![Selecteer 'Duur-verkeer, verkeer'](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   8. Nadat het veld wordt omgezet naar JSON-indeling, voegt u toe een **met door komma's** (```,```) gevolgd door het nummer ```60``` zodat u de waarde in converteren **verkeer duur verkeer** seconden minuten. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      De expressie nu ziet er in dit voorbeeld:

      ![Expressie voltooien](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   9. Zorg ervoor dat u kiest **OK** wanneer u klaar bent.

  10. Nadat de expressie wordt omgezet, wordt deze tekst met een voorloopspaties toevoegen: ``` minutes```
  
      Uw **hoofdtekst** veld ziet er nu uit zoals dit voorbeeld:

      ![Voltooide 'Hoofdtekst' veld](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)

6. Sla uw logische app op.

Vervolgens voegt u een scope zodat u kunt specifieke acties te groeperen en hun status evalueren.

## <a name="add-a-scope"></a>Een scope toevoegen

1. Als u nog niet gedaan hebt, opent u uw logische app in Logic App-ontwerper. 

2. Een bereik op de locatie van de werkstroom die u wilt toevoegen. Bijvoorbeeld:

   * Als u wilt een scope tussen bestaande stappen in de logic app-werkstroom toevoegen, de aanwijzer boven de pijl waar u wilt toevoegen, het bereik. 
   Kies de **plusteken** (**+**) > **toevoegen van een scope**.

     ![Een scope toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

     Als u wilt toevoegen van een scope aan het einde van de werkstroom, aan de onderkant van uw logische app, kiest u **+ een nieuwe stap** > **... Meer** > **toevoegen van een scope**.

3. Nu de stappen toevoegen of bestaande stappen die u wilt uitvoeren binnen het bereik te slepen. Sleep deze acties in het bereik voor dit voorbeeld:
      
   * **Route ophalen**
   * **Als verkeer wanneer meer dan een opgegeven periode**, waaronder zowel de **true** en **false** vertakkingen

   Uw logische app ziet er nu als volgt uit:

   ![Bereik toegevoegd](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

4. Binnen het bereik moet u een voorwaarde waarmee wordt gecontroleerd of de status van de scope toevoegen. Wijzig de naam van de voorwaarde van deze beschrijving: **als bereik is mislukt**

   ![Voorwaarde bereik status controleren toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
5. Bouw deze expressie op waarmee wordt gecontroleerd of de status van het bereik gelijk is aan `Failed` of `Aborted`.

   ![Expressie die de status van de scope controleert toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/build-expression-check-scope-status.png)

   Of voer deze expressie als tekst, de optie kiezen **bewerken in de geavanceerde modus**.

   ```@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')```

6. In de **als de waarde true** en **indien false** vertakkingen, voeg de acties die u uitvoeren wilt, bijvoorbeeld e-mailadres of een bericht verzenden.

   ![Expressie die de status van de scope controleert toevoegen](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

7. Sla uw logische app op.

In dit voorbeeld ziet uw klaar logische app nu er met alle uitgevouwen vormen:

![Voltooide logische app met een bereik](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>Testen van uw werk

Kies op de werkbalk designer **uitvoeren**. Als de scoped acties is gelukt, krijgt u een bericht 'Bereik is voltooid'. Als geen bereik acties niet slaagt, krijgt u een bericht 'Bereik is mislukt'. 

<a name="scopes-json"></a>

## <a name="json-definition"></a>JSON-definitie

Als u in de codeweergave werkt, kunt u in plaats daarvan de structuur van een bereik definiëren in uw logische app JSON-definitie. Dit is bijvoorbeeld de JSON-definitie voor de trigger en acties in de vorige logische app:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    },
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
            "Body": "Scope failed",
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
              "Body": "None",
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
    "expression": "@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')",
    "runAfter": {
      "Scope": [
        "Succeeded"
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
      "If_traffic_time_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'], 60)} minutes",
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
        "expression": "@greater(body('Get_route')?['travelDurationTraffic'], 600)",
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
}
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u wilt verzenden of stemmen over de functies en suggesties, gaat u naar de [Azure Logic Apps gebruiker feedback site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
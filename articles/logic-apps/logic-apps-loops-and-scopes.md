---
title: Lussen en -scopes maken of gegevens in werkstromen - Azure Logic Apps debatch | Microsoft Docs
description: Lussen om te doorlopen gegevens, acties in bereiken,-groepen maken of debatch gegevens om meer werkstromen te starten in Azure Logic Apps.
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 413a2ba9107ca259ed577825bf0a17ff5622f1ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Lussen, bereiken en debatching van logische apps
  
Logische Apps biedt een aantal manieren om te werken met matrices, verzamelingen, batches, en binnen een werkstroom een lus.
  
## <a name="foreach-loop-and-arrays"></a>ForEach-lus en matrices
  
Logic Apps kunt u een set gegevens doorlopen en een actie uitvoert voor elk item.  Dit is mogelijk via de `foreach` in te grijpen.  In de ontwerpfunctie, kunt u opgeven om toe te voegen een voor elke lus.  Na het selecteren van de matrix die u wilt doorlopen, kunt u beginnen acties toe te voegen.  U bent momenteel beperkt tot één actie per foreach lus, maar deze beperking wordt in de komende weken worden opgeheven.  Eenmaal binnen de lus kunt u beginnen om op te geven wat er moet gebeuren op elke waarde van de matrix.

Als weergave code wordt gebruikt, kunt u een voor elke lus zoals hieronder.  Dit is een voorbeeld van een voor elke lus die voor elke e-mailadres dat 'microsoft.com' bevat een e-mailbericht verzendt:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
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
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  Een `foreach` actie kunt sequentieel over matrices maximaal 5000 rijen.  Elke herhaling wordt parallel uitgevoerd standaard.  

### <a name="sequential-foreach-loops"></a>Sequentiële ForEach lussen

Om in te schakelen van een lus foreach uitvoeren opeenvolgend, de `Sequential` optie bij bewerking moet worden toegevoegd.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Pas de lus
  
  U kunt een actie of een reeks acties uitvoeren totdat er een voorwaarde wordt voldaan.  De meest voorkomende scenario voor dit kan een eindpunt wordt aangeroepen totdat u het antwoord die u zoekt.  In de ontwerpfunctie, kunt u opgeven om toe te voegen een tot lus.  Na het toevoegen van acties binnen de lus, kunt u de voorwaarde voor het afsluiten, evenals de lus instellen limieten.  Er is een vertraging van 1 minuut tussen de cycli lus.
  
  Als weergave code wordt gebruikt, kunt u een totdat de lus, zoals hieronder.  Dit is een voorbeeld van een aanroep van een HTTP-eindpunt totdat de antwoordtekst de waarde heeft 'Voltooid'.  De taak voltooid wanneer beide 
  
  * HTTP-antwoord heeft de status 'Voltooid'
  * Er is geprobeerd 1 uur
  * Het is 100 keer herhaald
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn en debatching

Een trigger wordt soms een matrix van items die u wilt debatch en starten van een werkstroom per artikel.  Dit kan worden geconfigureerd via de `spliton` opdracht.  Als uw swagger trigger is opgegeven voor een nettolading met een matrix is standaard een `spliton` wordt toegevoegd en een reeks per object te starten.  SplitOn kan alleen worden toegevoegd aan een trigger.  Dit kan handmatig worden geconfigureerd of overschreven in de codeweergave definitie.  Momenteel SplitOn kunt debatch matrices van maximaal 5.000 items.  U kunt geen een `spliton` en ook het patroon synchrone antwoord te implementeren.  Een werkstroom aangeroepen die heeft een `response` actie naast `spliton` wordt asynchroon uitgevoerd en een onmiddellijke verzonden `202 Accepted` antwoord.  

SplitOn kan worden opgegeven in de weergave van de code als het volgende voorbeeld.  Hiermee ontvangt een matrix van items en debatches op elke rij.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Scopes

Het is mogelijk een reeks acties samen met een scope groeperen.  Dit is vooral handig voor het implementeren van afhandeling van uitzonderingen.  U kunt in de ontwerpfunctie voor toevoegen van een nieuwe scope en gaan alle acties daarbinnen toevoegen.  U kunt bereiken definiëren in de codeweergave als volgt:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
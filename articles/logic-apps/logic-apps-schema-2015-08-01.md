---
title: Schema-updates augustus-1-2015 preview - Azure Logic Apps | Microsoft Docs
description: JSON-definities voor Azure Logic Apps maken met 08-01-preview-schemaversie 2015
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.openlocfilehash: 35d7a56d5607dcc18a4407c65b92962d3d0dcd1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Schema-updates voor Azure Logic Apps - preview 1 augustus 2015

Deze nieuwe schema en de API-versie voor Azure Logic Apps bevat belangrijke verbeteringen die logische apps betrouwbaarder en eenvoudiger te gebruiken:

*   De **APIApp** actietype wordt bijgewerkt naar een nieuwe [ **APIConnection** ](#api-connections) action-type.
*   **Herhaal** is gewijzigd in [ **Foreach**](#foreach).
*   De [ **HTTP-Listener** API-App](#http-listener) is niet langer vereist.
*   Het aanroepen van onderliggende werkstromen gebruikt een [nieuwe schema](#child-workflows).

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a>Verplaatsen naar de API-verbindingen

De grootste wijziging is dat u niet langer API-Apps implementeren in uw Azure-abonnement moet, zodat u API's kunt gebruiken. Hier volgen de manieren waarop u API's kunt gebruiken:

* Beheerde API 's
* Uw aangepaste Web-API 's

Elke manier wordt anders verwerkt omdat het beheer en het hosten van modellen verschillend zijn. Een voordeel van dit model is dat u bent niet meer beperkt tot resources die zijn geïmplementeerd in uw Azure-resourcegroep. 

### <a name="managed-apis"></a>Beheerde API 's

Microsoft beheert sommige API's namens u, zoals Office 365, Salesforce, Twitter en FTP. U kunt sommige beheerde API's als-is, zoals Bing vertalen, terwijl anderen configuratie vereist. Deze configuratie wordt aangeroepen een *verbinding*.

Wanneer u Office 365 gebruikt, moet u bijvoorbeeld een verbinding met uw Office 365-in-token maken. Dit token is veilig opgeslagen en wordt vernieuwd zodat uw logische app kunt altijd de Office 365-API aanroepen. Als u wilt verbinding maken met uw SQL- of FTP-server, moet u ook een verbinding met de verbindingsreeks maken. 

In deze definitie deze acties worden genoemd `APIConnection`. Hier volgt een voorbeeld van een verbinding die Office 365 voor het verzenden van een e-mailbericht aanroept:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

De `host` object is gedeelte van de ingangen die uniek is voor de API-verbindingen en bestaat uit twee delen: `api` en `connection`.

De `api` heeft de runtime URL van het waar dat beheerd API wordt gehost. U kunt zien alle beschikbare beheerde API's door het aanroepen van `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wanneer u een API gebruikt, de API kan of kunnen geen *verbindingsparameters* gedefinieerd. De API niet het geval, geen *verbinding* is vereist. Als de API is, moet u een verbinding te maken. De gemaakte verbinding heeft de naam die u kiest. U vervolgens verwijzen naar de naam in de `connection` object binnen de `host` object. Een verbinding wilt maken in een resourcegroep, aanroepen:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Met de volgende hoofdtekst:

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Beheerde API's in een Azure Resource Manager-sjabloon implementeren

U kunt een volledige toepassing in een Azure Resource Manager-sjabloon maken, zolang interactief aanmelden is niet vereist.
Als aanmelden vereist is, kunt u een alles met Azure Resource Manager-sjabloon instellen, maar u hebt nog wel gaat u naar de portal voor het autoriseren van verbindingen. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/', parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:, Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

U kunt zien in dit voorbeeld is dat de verbindingen zijn alleen bronnen die bevinden zich in de resourcegroep. Ze verwijzen naar de beheerde API's die u in uw abonnement.

### <a name="your-custom-web-apis"></a>Uw aangepaste Web-API 's

Als u uw eigen API's, die niet door Microsoft beheerde gebruiken de ingebouwde **HTTP** actie die moet worden ze aanroept. Voor een ideaal ervaring, moet u een Swagger-eindpunt weergeven voor uw API. Dit eindpunt maakt de Logic App-ontwerper voor het weergeven van de invoer en uitvoer voor uw API. Zonder Swagger, kan de designer alleen weergeven in- en uitgangen als ondoorzichtige JSON-objecten.

Hier volgt een voorbeeld van de nieuwe `metadata.apiDefinitionUrl` eigenschap:

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

Als u uw Web-API op Azure App Service host, wordt uw Web-API automatisch weergegeven in de lijst met acties die beschikbaar zijn in de ontwerpfunctie. Als dat niet het geval is, hebt u rechtstreeks in de URL plakken. Het Swagger-eindpunt moet worden niet-geverifieerde om te worden gebruikt in de ontwerpfunctie voor Logic App Hoewel u de API zichzelf met welke methoden die ondersteuning biedt voor Swagger kunt beveiligen.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Aanroepen van geïmplementeerde API-apps met 2015-08-01-preview

Als u een API-App hebt geïmplementeerd, kunt u de app met bellen de **HTTP** in te grijpen.

Als u Dropbox weergeven van bestanden, bijvoorbeeld uw **2014-12-01-preview** versie schemadefinitie wellicht ongeveer als volgt:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

U kunt de equivalente HTTP-actie zoals in dit voorbeeld maken terwijl de parameters-sectie van de definitie van Logic Apps ongewijzigd blijft:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata": {
              "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method": "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Stap voor stap één voor één van deze eigenschappen:

| Eigenschap Action | Beschrijving |
| --- | --- |
| `type` |`Http`In plaats van`APIapp` |
| `metadata.apiDefinitionUrl` |Als u wilt gebruiken met deze actie in de ontwerpfunctie voor Logic App, zijn de metagegevenseindpunt is samengesteld uit:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Gemaakt op basis van:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Altijd`POST` |
| `inputs.body` |Identiek aan de API-App-parameters |
| `inputs.authentication` |Identiek aan de API-App-verificatie |

Deze aanpak moet werken voor alle acties van de API-App. Vergeet echter niet dat deze vorige API-Apps niet langer worden ondersteund. Daarom moet u overstappen op een van de twee andere vorige opties, een beheerde API of die als host fungeert voor uw aangepaste Web-API.

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a>De naam 'Herhaal' gewijzigd in 'foreach'

We ontvangen voor de vorige schemaversie veel feedback van klanten die **herhalen** verwarrend is en niet goed vast te leggen die **herhalen** werd echt een voor elke lus. We hebben als gevolg hiervan hernoemd `repeat` naar `foreach`. Bijvoorbeeld zou eerder u schrijven:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

U zou nu schrijven:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

De functie `@repeatItem()` eerder werd gebruikt om te verwijzen naar het huidige item wordt iteratie. Deze functie is nu eenvoudiger te `@item()`. 

### <a name="reference-outputs-from-foreach"></a>Verwijzing naar uitvoer van 'foreach'

Voor de uitvoer van vereenvoudiging `foreach` acties niet zijn ingepakt in een object aangeroepen `repeatItems`. Terwijl de uitvoer van de vorige `repeat` voorbeeld zijn:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Nu zijn deze uitgangen:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Voorheen om aan de hoofdtekst van de actie die verwijzen naar deze uitvoer:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

Nu kunt u doen in plaats daarvan:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

Met deze wijzigingen, de functies `@repeatItem()`, `@repeatBody()`, en `@repeatOutputs()` worden verwijderd.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Native HTTP-listener

De HTTP-Listener-mogelijkheden zijn nu ingebouwd in. Daarom moet u niet meer voor het implementeren van een HTTP-Listener API-App. Zie [de volledige details voor het maken van uw logische app eindpunt aanroepbare hier](../logic-apps/logic-apps-http-endpoint.md). 

Met deze wijzigingen verwijderd we de `@accessKeys()` functie, die wordt vervangen door de `@listCallbackURL()` functie voor het ophalen van het eindpunt indien nodig. Bovendien moet u ten minste één trigger nu definiëren in uw logische app. Als u wilt `/run` de werkstroom, hebt u een van deze triggers: `manual`, `apiConnectionWebhook`, of `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Aanroepen van onderliggende werkstromen

Het aanroepen van onderliggende werkstromen vereist voorheen gaan naar de werkstroom, het toegangstoken ophalen en het token in de definitie van logic Apps waar u aan te roepen die onderliggende werkstroom wilt plakken. Met het nieuwe schema genereert de Logic Apps-engine automatisch een SAS tijdens runtime voor de onderliggende werkstroom zodat u hoeft geen geheimen in de definitie te plakken. Hier volgt een voorbeeld:

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
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
    "conditions": []
}
```

Een tweede verbetering is dat we zijn de onderliggende werkstromen volledige toegang geven tot de binnenkomende aanvraag. Dit betekent dat u kunt de parameters in doorgeven dat de *query's* sectie en in de *headers* object en die u kunt de volledige hoofdtekst volledig definiëren.

Er zijn ten slotte vereist wijzigingen in de onderliggende werkstroom. Terwijl u onderliggende werkstroom eerder rechtstreeks aanroepen kan, moet nu u een trigger-eindpunt in de werkstroom voor de bovenliggende aan te roepen. In het algemeen, voegt u een trigger die heeft `manual` typen en vervolgens deze trigger te gebruiken in de definitie van de bovenliggende. Opmerking de `host` eigenschap specifiek heeft een `triggerName` omdat u altijd waarvan de trigger moet opgeven dat u aanroept.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="new-queries-property"></a>Nieuwe 'query'-eigenschap

Alle actietypen ondersteunen nu een nieuwe invoer aangeroepen `queries`. Deze gegevens kan worden gestructureerd object, in plaats van u hoeft voor het samenstellen van de tekenreeks met de hand.

### <a name="renamed-parse-function-to-json"></a>Nieuwe naam parse() functie 'json()'

We toevoegen meer inhoudstypen snel, zodat we hernoemd de `parse()` functie `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Binnenkort beschikbaar: Enterprise Integration-API's

Er zijn momenteel geen beheerde versies nog van de Enterprise Integration-API's, zoals AS2. Ondertussen kunt u uw bestaande BizTalk-APIs geïmplementeerde via de HTTP-actie. Voor meer informatie Zie ' de reeds geïmplementeerde API-apps gebruiken ' in de [integratie roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 

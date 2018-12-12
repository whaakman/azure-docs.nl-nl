---
title: Schema-updates voor augustus-1-2015 preview - Azure Logic Apps | Microsoft Docs
description: Bijgewerkte schema versie 2015-08-01-preview voor definities voor logische apps in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.topic: article
ms.date: 05/31/2016
ms.openlocfilehash: ec6f98ca0f0260a0d7bed16538f557931cd2e33e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53080007"
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Schema-updates voor Azure Logic Apps - 1 augustus 2015 preview

Dit schema en de API-versie voor Azure Logic Apps bevat belangrijke verbeteringen die logische apps nog betrouwbaar en eenvoudiger te gebruiken:

* De **APIApp** actietype heet nu [ **APIConnection**](#api-connections).
* De **Herhaal** actie heet nu [ **Foreach**](#foreach).
* De [ **HTTP-Listener** API-App](#http-listener) is niet langer vereist.
* Aanroepen van onderliggende werkstromen gebruikt een [nieuwe schema](#child-workflows).

<a name="api-connections"></a>

## <a name="move-to-api-connections"></a>Verplaatsen naar de API-verbindingen

De grootste wijziging is dat u niet meer voor het implementeren van API Apps in uw Azure-abonnement, zodat u API's kunt gebruiken. Dit zijn de manieren waarop u API's kunt gebruiken:

* Beheerde API 's
* Uw aangepaste Web-API 's

Elke manier wordt iets anders verwerkt omdat het beheer en het hosten van modellen verschillend zijn. Een voordeel van dit model is dat u bent niet meer beperkt tot de resources die zijn geïmplementeerd in uw Azure-resourcegroep. 

### <a name="managed-apis"></a>Beheerde API 's

Microsoft beheert aantal API's op uw naam, zoals Office 365, Salesforce, Twitter en FTP. U kunt sommige beheerde API's als-is, zoals Bing, vertalen, terwijl anderen configuratie vereist is, ook wel genoemd een *verbinding*.

Wanneer u Office 365 gebruikt, moet u bijvoorbeeld een verbinding met uw Office 365-aanmelden-token maken. Uw token worden veilig opgeslagen en vernieuwd zodat uw logische app kunt altijd de Office 365-API aanroepen. Als u wilt verbinding maken met uw SQL- of FTP-server, moet u een verbinding met de verbindingsreeks te maken. 

In deze definitie van deze acties worden genoemd `APIConnection`. Hier volgt een voorbeeld van een verbinding die Office 365 om een e-mail te verzenden aanroept:

``` json
{
   "actions": {
      "Send_an_email": {
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
            "method": "POST",
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

De `host` object is een onderdeel van de invoer die uniek is voor de API-verbindingen en bevat de volgende onderdelen: `api` en `connection`. De `api` object Hiermee geeft u de runtime-URL voor waar die API beheerde wordt gehost. Door deze methode aanroept, ziet u alle beschikbare beheerde API's:

```text
GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/locations/<location>/managedApis?api-version=2015-08-01-preview
```

Wanneer u een API, die API kan of kunnen niet zijn gedefinieerd een *verbindingsparameters*. Dus als de API bevat geen definitie van deze parameters, is er is geen verbinding vereist. Als de API deze parameters definieert, moet u een verbinding maken met een opgegeven naam.  
U vervolgens verwijzen naar die naam bestaat in de `connection` object binnen de `host` object. Als u wilt een verbinding maakt in een resourcegroep, deze methode niet aanroepen:

```text
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/<name>?api-version=2015-08-01-preview
```

Met de volgende tekst:

``` json
{
   "properties": {
      "api": {
         "id": "/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/azureblob"
      },
      "parameterValues": {
         "accountName": "<Azure-storage-account-name-with-different-parameters-for-each-API>"
      }
   },
   "location": "<logic-app-location>"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Beheerde API's in een Azure Resource Manager-sjabloon implementeren

Wanneer u interactief aanmelden is niet vereist, kunt u een volledige app maken met behulp van Resource Manager-sjabloon.
Als aanmelden vereist is, kunt u nog steeds een Resource Manager-sjabloon gebruiken, maar u moet autoriseren van de verbindingen via de Azure-portal. 

``` json
"resources": [ {
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
    },
},
{
   "type": "Microsoft.Logic/workflows",
   "apiVersion": "2015-08-01-preview",
   "name": "[parameters('logicAppName')]",
   "location": "[resourceGroup().location]",
   "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"],
   "properties": {
      "sku": {
         "name": "[parameters('sku')]",
         "plan": {
            "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('svcPlanName'))]"
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
      },
      "definition": {
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "type": "Object",
            "$connections": {
               "defaultValue": {},
 
            }
         },
         "triggers": {
            "Recurrence": {
               "type": "Recurrence",
               "recurrence": {
                  "frequency": "Day",
                  "interval": 1
               }
            }
         },
         "actions": {
            "Create_file": {
               "type": "ApiConnection",
               "inputs": {
                  "host": {
                     "api": {
                        "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                     },
                     "connection": {
                       "name": "@parameters('$connections')['azureblob']['connectionId']"
                     }
                  },
                  "method": "POST",
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
         "outputs": {}
      }
   }
} ]
```

U kunt zien in dit voorbeeld is dat de verbindingen zijn alleen bronnen die bevinden zich in de resourcegroep. Ze verwijzen naar de beheerde API's die u in uw abonnement.

### <a name="your-custom-web-apis"></a>Uw aangepaste Web-API 's

Als u uw eigen API's in plaats van Microsoft worden beheerd die zijn gebruikt, gebruikt u de ingebouwde **HTTP** actie voor het aanroepen van uw API's. In het ideale geval moet u een Swagger-eindpunt opgeven voor uw API. Dit eindpunt helpt Logic App Designer weergeven van de invoer en uitvoer van uw API's. Zonder een Swagger-eindpunt, kunt de ontwerpfunctie alleen de invoer en uitvoer als weergeven ondoorzichtige JSON-objecten.

Hier volgt een voorbeeld van de nieuwe `metadata.apiDefinitionUrl` eigenschap:

``` json
"actions": {
   "mycustomAPI": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
      },
      "inputs": {
         "uri": "https://mysite.azurewebsites.net/api/getsomedata",
         "method": "GET"
      }
   }
}
```

Als u uw Web-API in Azure App Service host, wordt uw Web-API automatisch weergegeven in de lijst met acties die beschikbaar zijn in de ontwerpfunctie. Als dat niet het geval is, hebt u rechtstreeks in de URL plakken. Het Swagger-eindpunt moet worden niet-geverifieerde zodanig kan worden gebruikt in de Logic App Designer, hoewel u de API zelf met de methoden die ondersteuning biedt voor Swagger kunt beveiligen.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Geïmplementeerde API apps aanroepen met 2015-08-01-preview

Als u een API-App hebt geïmplementeerd, kunt u die app met bellen de **HTTP** actie.
Bijvoorbeeld, als u Dropbox weergeven van bestanden, uw **2014-12-01-preview** versie schemadefinitie mogelijk ongeveer als volgt:

``` json
"definition": {
   "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
         "defaultValue": "eyJ0eX...wCn90",
         "type": "String",
         "metadata": {
            "token": {
               "name": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
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
                "id": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                "gateway": "https://avdemo.azurewebsites.net"
             },
             "operation": "ListFiles",
             "parameters": {
                "FolderPath": "/myfolder"
             },
             "authentication": {
                "type": "Raw",
                "scheme": "Zumo",
                "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
             }
          }
       }
    }
}
```

Nu u kunt nu een vergelijkbare HTTP-actie bouwen en laat u de logische app-definitie `parameters` sectie ongewijzigd, bijvoorbeeld:

``` json
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
            "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
         }
      }
   }
}
```

Stap voor stap één voor één van deze eigenschappen:

| Actie-eigenschap | Description |
| --- | --- |
| `type` | `Http` In plaats van `APIapp` |
| `metadata.apiDefinitionUrl` | Als u deze actie in Logic App Designer, zijn onder andere een eindpunt van de metagegevens die is samengesteld uit: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Samengesteld uit: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Altijd `POST` |
| `inputs.body` | Hetzelfde als de API-App-parameters |
| `inputs.authentication` | Dezelfde is als de verificatie van de API-App |

Deze methode werkt voor alle API-App-acties. Vergeet echter niet dat deze vorige API-Apps niet langer worden ondersteund. U moet dus verplaatsen naar een van de twee andere vorige opties, een beheerde API of die als host fungeert voor uw aangepaste Web-API.

<a name="foreach"></a>

## <a name="renamed-repeat-to-foreach"></a>Gewijzigd van 'herhalen op' in 'foreach'

Voor de vorige schemaversie van het er veel feedback van klanten ontvangen dat de **Herhaal** Actienaam is verwarrend en is niet correct vastleggen die **Herhaal** werd echt een voor elke lus. Dus we gewijzigd `repeat` naar `foreach`. Eerder schrijft u deze actie zoals in dit voorbeeld:

``` json
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
```

Nu schrijft u deze versie in plaats daarvan:

``` json
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
```

Ook de `repeatItem()` functie, die het item dat de lus wordt verwerkt tijdens de huidige iteratie wordt verwezen, is nu gewijzigd `item()`. 

### <a name="reference-outputs-from-foreach"></a>Referentie-uitvoer van 'foreach'

Voor de uitvoer van vereenvoudiging `foreach` acties zijn niet meer samengevoegd in een object met de naam `repeatItems`. Met deze wijzigingen ook de `repeatItem()`, `repeatBody()`, en `repeatOutputs()` functies zijn verwijderd.

Ja, met behulp van de vorige `repeat` voorbeeld krijgt u deze uitvoer:

``` json
"repeatItems": [ {
   "name": "pingBing",
   "inputs": {
      "uri": "https://www.bing.com/search?q=0",
      "method": "GET"
   },
   "outputs": {
      "headers": { },
      "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
   },
   "status": "Succeeded"
} ]
```

U krijgt nu deze uitvoer in plaats daarvan:

``` json
[ {
   "name": "pingBing",
      "inputs": {
         "uri": "https://www.bing.com/search?q=0",
         "method": "GET"
      },
      "outputs": {
         "headers": { },
         "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
      },
      "status": "Succeeded"
} ]
```

Voorheen was om op te halen de `body` van de actie bij verwijzingen naar deze uitvoer:

``` json
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
```

Nu kunt u deze versie in plaats daarvan gebruiken:

``` json
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
```

<a name="http-listener"></a>

## <a name="native-http-listener"></a>Native HTTP-listener

HTTP-listener-functies zijn nu geïntegreerd, dus u hoeft te implementeren van een HTTP-Listener API-App. Informatie voor meer informatie over hoe u [maken uw logische app-eindpunt aanroepbare](../logic-apps/logic-apps-http-endpoint.md). 

Met deze wijzigingen, Logic Apps wordt vervangen door de `@accessKeys()` werken met de `@listCallbackURL()` functie Hiermee haalt u het eindpunt nodig. Ook moet nu u definiëren ten minste één trigger in uw logische app. Als u wilt `/run` de werkstroom, die u hebt een van de volgende triggertypen te gebruiken: `Manual`, `ApiConnectionWebhook`, of `HttpWebhook`

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Aanroepen van onderliggende werkstromen

Aanroepen van onderliggende werkstromen vereist eerder, gaan naar de werkstroom, ophalen van het toegangstoken en plakt het token in de definitie van de logische app waar u aan te roepen die onderliggende werkstroom. In dit schema genereert de Logic Apps-engine automatisch een SAS tijdens runtime voor de onderliggende werkstroom, zodat u hoeft geen geheimen in de definitie van de plakken. Hier volgt een voorbeeld:

``` json
"myNestedWorkflow": {
   "type": "Workflow",
   "inputs": {
      "host": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/myWorkflow001",
         "triggerName": "myEndpointTrigger"
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

Ook onderliggende werkstromen volledige toegang krijgen tot de inkomende aanvraag. Ja, u kunt parameters doorgeven in de `queries` sectie en in de `headers` object. U kunt ook volledig definiëren de hele `body` sectie.

Ten slotte hebben onderliggende werkstromen deze vereiste wijzigingen. Hoewel u kan eerder en rechtstreeks aanroepen van een onderliggende werkstroom, moet u nu een trigger-eindpunt definiëren in de werkstroom voor de bovenliggende om aan te roepen. In het algemeen, voegt u een trigger waarvoor `Manual` typt en vervolgens deze trigger in de definitie van de bovenliggende. De `host` eigenschap specifiek heeft een `triggerName` omdat u moet altijd de trigger u bellen.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="new-queries-property"></a>Nieuwe 'query'-eigenschap

Alle actietypen bieden nu ondersteuning voor een nieuwe invoer met de naam `queries`. Deze invoer kan worden gestructureerd object, in plaats van u hoeft voor het samenstellen van de tekenreeks met de hand.

### <a name="renamed-parse-function-to-json"></a>Hernoemd parse() functie 'json()'

De `parse()` functie is nu gewijzigd de `json()` functie voor toekomstige inhoudstypen.

## <a name="enterprise-integration-apis"></a>Enterprise Integration-API 's

Dit schema biedt geen beheerde versies nog ondersteuning voor Enterprise Integration-API's, zoals AS2. U kunt echter bestaande geïmplementeerde BizTalk APIs via de HTTP-actie. Voor meer informatie, Zie "met behulp van uw reeds geïmplementeerde API-apps' in de [integratie roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 

---
title: Schema-updates voor augustus-1-2015 preview - Azure Logic Apps | Microsoft Docs
description: Bijgewerkte schema versie 2015-08-01-preview voor definities voor logische apps in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: stepsic-microsoft-com
ms.author: stepsic
ms.reviewer: klam, estfan, LADocs
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.topic: article
ms.date: 05/31/2016
ms.openlocfilehash: dd05543c2a727f010432ecb54c2dc3e77a245de4
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122774"
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

De `host` object is een onderdeel van de invoer die uniek is voor de API-verbindingen en bevat de volgende onderdelen: `api` en `connection`. De `api` object Hiermee geeft u de runtime-URL voor waar die API beheerde wordt gehost. Ziet u alle beschikbare beheerde API's door het aanroepen van `GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wanneer u een API, die API kan of kunnen niet zijn gedefinieerd een *verbindingsparameters*. Dus als de API bevat geen definitie van deze parameters, is er is geen verbinding vereist. Als de API deze parameters definieert, moet u een verbinding maken met een opgegeven naam.  
U vervolgens verwijzen naar die naam bestaat in de `connection` object binnen de `host` object. Als u wilt een verbinding maakt in een resourcegroep, deze methode niet aanroepen:

```
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

U kunt een volledige app maken in een Azure Resource Manager-sjabloon, zolang interactief aanmelden is niet vereist.
Als aanmelden vereist is, kunt u een alles wat met de Azure Resource Manager-sjabloon instellen, maar u moet nog steeds gaat u naar de Azure portal voor het autoriseren van verbindingen. 

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

Als u uw eigen API's, niet-beheerd door Microsoft zijn, gebruikt u de ingebouwde **HTTP** actie die moet worden ze aanroept. Voor een optimale ervaring, moet u een Swagger-eindpunt beschikbaar maken voor uw API. Dit eindpunt kunt Logic App Designer om weer te geven van de invoer en uitvoer voor uw API. Zonder Swagger, kan de ontwerpfunctie alleen de invoer en uitvoer als weergeven ondoorzichtige JSON-objecten.

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

U kunt nu de equivalente HTTP-actie, zoals het volgende voorbeeld wordt nu maken terwijl het verlaten van de parametersectie voor de definitie van de logische app niet worden gewijzigd:

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

| Actie-eigenschap | Beschrijving |
| --- | --- |
| `type` | `Http` In plaats van `APIapp` |
| `metadata.apiDefinitionUrl` | Als u deze actie in Logic App Designer, zijn onder andere een eindpunt van de metagegevens die is samengesteld uit: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Samengesteld uit: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Altijd `POST` |
| `inputs.body` | Identiek aan de API-App-parameters |
| `inputs.authentication` | Identiek aan de API-App-verificatie |

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

De HTTP-Listener-mogelijkheden zijn nu ingebouwd in. Daarom moet u niet langer een HTTP-Listener API-App implementeren. Zie [de volledige details voor het maken van uw logische app-eindpunt aanroepbare hier](../logic-apps/logic-apps-http-endpoint.md). 

Met deze wijzigingen die we hebben verwijderd de `@accessKeys()` functie, die wordt vervangen door de `@listCallbackURL()` functie voor het ophalen van het eindpunt nodig. Bovendien moet u ten minste één trigger nu definiëren in uw logische app. Als u wilt `/run` de werkstroom, moet u een van deze triggers hebben: `manual`, `apiConnectionWebhook`, of `httpWebhook`.

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Aanroepen van onderliggende werkstromen

Aanroepen van onderliggende werkstromen vereist eerder, gaan naar de werkstroom, ophalen van het toegangstoken en plakt het token in de definitie van de logische app waar u aan te roepen die onderliggende werkstroom. Met het nieuwe schema genereert de Logic Apps-engine automatisch een SAS tijdens runtime voor de onderliggende werkstroom, zodat u hoeft geen geheimen in de definitie van de plakken. Hier volgt een voorbeeld:

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

Een tweede verbetering is dat we zijn de onderliggende-werkstromen volledig toegang geven tot de inkomende aanvraag. Dit betekent dat u kunt parameters doorgeven in de *query's* sectie en in de *headers* object en dat u de volledige hoofdtekst volledig kunt definiëren.

Er zijn ten slotte vereist wijzigingen in de onderliggende werkstroom. Terwijl u een onderliggende werkstroom eerder rechtstreeks aanroepen kunt, nu u moet een trigger eindpunt definiëren in de werkstroom voor de bovenliggende om aan te roepen. In het algemeen, voegt u een trigger waarvoor `manual` typt en vervolgens deze trigger in de definitie van de bovenliggende. Houd er rekening mee de `host` eigenschap specifiek heeft een `triggerName` omdat u altijd waarvan de trigger opgeven moet u aanroept.

## <a name="other-changes"></a>Andere wijzigingen

### <a name="new-queries-property"></a>Nieuwe 'query'-eigenschap

Alle actietypen bieden nu ondersteuning voor een nieuwe invoer met de naam `queries`. Deze invoer kan worden gestructureerd object, in plaats van u hoeft voor het samenstellen van de tekenreeks met de hand.

### <a name="renamed-parse-function-to-json"></a>Hernoemd parse() functie 'json()'

Er meer inhoudstypen binnenkort wordt toegevoegd, zodat we gewijzigd de `parse()` functie `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Binnenkort beschikbaar: Enterprise Integration-API's

Er zijn geen beheerde versies nog van de Enterprise Integration-API's, zoals AS2. Ondertussen kunt u uw bestaande BizTalk-APIs geïmplementeerd via de HTTP-actie. Voor meer informatie, Zie "met behulp van uw reeds geïmplementeerde API-apps' in de [integratie roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 

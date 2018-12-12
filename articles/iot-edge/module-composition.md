---
title: Declareer modules en routes met implementatie manifesten - Azure IoT Edge | Microsoft Docs
description: Leer hoe een manifest van de implementatie wordt gedeclareerd welke modules te implementeren, om ze te implementeren, en hoe u berichtroutes daartussen maken.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0b221274923a6270e980d027aadc58154c7054b9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53099967"
---
# <a name="learn-how-to-deploy-modules-and-establish-routes-in-iot-edge"></a>Meer informatie over het implementeren van modules en routes instellen in IoT Edge

Elke IoT Edge-apparaat wordt uitgevoerd ten minste twee modules: $edgeAgent en $edgeHub, die deel uitmaken van de IoT Edge-runtime. Bovendien kan meerdere modules om uit te voeren van een willekeurig aantal processen worden uitgevoerd door elk IoT Edge-apparaat. U implementeert alle deze modules op een apparaat in één keer, zodat de IoT Edge biedt een manier om aan te geven welke modules te installeren en configureren om samen te werken. 

De *implementatie manifest* is een JSON-document dat wordt beschreven:

* De **IoT Edge agent** moduledubbel, met inbegrip van de installatiekopie van de container voor elke module, de referenties voor toegang tot persoonlijke containerregisters en instructies voor hoe elke module moet worden gemaakt en beheerd.
* De **IoT Edge hub** moduledubbel, waaronder hoe berichten stromen tussen modules en uiteindelijk naar IoT Hub.
* (Optioneel) de gewenste eigenschappen van elke aanvullende moduledubbels.

Alle IoT Edge-apparaten moeten worden geconfigureerd met een manifest van de implementatie. Een nieuw geïnstalleerde IoT Edge-runtime rapporteert een foutcode totdat geconfigureerd met een manifest. 

In de Azure IoT Edge-zelfstudies bouwt u een manifest van de implementatie door te gaan met een wizard in de portal voor Azure IoT Edge. U kunt ook een implementatie-manifest programmatisch met behulp van REST- of de SDK voor IoT Hub-Service. Zie voor meer informatie, [inzicht in IoT Edge-implementaties](module-deployment-monitoring.md).

## <a name="create-a-deployment-manifest"></a>Maken van een manifest van de implementatie

Op hoog niveau is een manifest van de implementatie van een lijst met moduledubbels die zijn geconfigureerd met de gewenste eigenschappen. Een manifest van de implementatie wordt een IoT Edge-apparaat (of een groep apparaten) aangegeven welke modules te installeren en configureren. Implementatie manifesten bevatten de *gewenste eigenschappen* voor elke moduledubbel. IoT Edge-apparaten rapporteren de *gerapporteerde eigenschappen* voor elke module. 

Twee modules vereist zijn in elke implementatie-manifest: `$edgeAgent`, en `$edgeHub`. Deze modules zijn onderdeel van de IoT Edge-runtime die u beheert de IoT Edge-apparaat en de modules die erop worden uitgevoerd. Zie voor meer informatie over deze modules [IoT Edge-runtime en de bijbehorende architectuur begrijpen](iot-edge-runtime.md).

Naast de twee runtimemodules, kunt u maximaal 20 modules van uw eigen om uit te voeren op een IoT Edge-apparaat toevoegen. 

Een implementatie-manifest dat alleen de IoT Edge-runtime (edgeAgent en edgeHub bevat) is geldig.

Implementatie manifesten volgen deze structuur:

```json
{
    "modulesContent": {
        "$edgeAgent": { // required
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
            }
        },
        "$edgeHub": { //required
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "{module1}": {  // optional
            "properties.desired": {
                // desired properties of {module1}
            }
        },
        "{module2}": {  // optional
            "properties.desired": {
                // desired properties of {module2}
            }
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Modules configureren

Definiëren hoe de modules in IoT Edge-runtime wordt geïnstalleerd in uw implementatie. De IoT Edge-agent is de runtime-onderdelen die installatie, updates en rapportage voor een IoT Edge-apparaat de status beheerd. Daarom vereist de moduledubbel $edgeAgent de configuratie en van beheergegevens voor alle modules. Deze informatie omvat de configuratieparameters voor de Edge agent zelf. 

Zie voor een volledige lijst van eigenschappen die kunnen of moeten worden opgenomen, [eigenschappen van de Edge agent en Edge hub](module-edgeagent-edgehub.md).

De eigenschappen $edgeAgent volgen deze structuur:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
            "settings":{
                "registryCredentials":{ // give the edge agent access to container images that aren't public
                    }
                }
            }
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "{module1}": { // optional
                // configuration and management details
            },
            "{module2}": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Routes declareren

De IoT Edge hub beheert de communicatie tussen modules, IoT-Hub en een leaf-apparaten. De moduledubbel $edgeHub bevat dus een gewenste eigenschap genaamd *routes* dat verklaart hoe berichten worden doorgegeven binnen een implementatie. U kunt meerdere routes binnen dezelfde implementatie hebben.

Routes zijn gedefinieerd in de **$edgeHub** gewenste eigenschappen met de volgende syntaxis:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "{route1}": "FROM <source> WHERE <condition> INTO <sink>",
            "{route2}": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Elke route heeft een bron en sink, maar de voorwaarde is een optioneel onderdeel dat u gebruiken kunt voor het filteren van berichten. 


### <a name="source"></a>Bron

De bron geeft aan waar de berichten afkomstig zijn uit. IoT Edge kunt versturen berichten van het leaf-apparaten of modules.

De broneigenschap kan een van de volgende waarden zijn:

| Bron | Description |
| ------ | ----------- |
| `/*` | Alle apparaat-naar-cloud-berichten of dubbele wijzigingsmeldingen vanaf elk apparaat module of leaf |
| `/twinChangeNotifications` | Elke wijziging met dubbele (gerapporteerde eigenschappen) die afkomstig zijn van een module of leaf-apparaat |
| `/messages/*` | Een apparaat-naar-cloud bericht verzonden door een module of leaf-apparaat via bepaalde of geen uitvoer |
| `/messages/modules/*` | Een apparaat-naar-cloud bericht verzonden door een module door bepaalde of geen uitvoer |
| `/messages/modules/{moduleId}/*` | Een apparaat-naar-cloud bericht verzonden door een specifieke module via bepaalde of geen uitvoer |
| `/messages/modules/{moduleId}/outputs/*` | Een apparaat-naar-cloud bericht verzonden door een specifieke module door sommige uitvoer |
| `/messages/modules/{moduleId}/outputs/{output}` | Een apparaat-naar-cloud bericht verzonden door een specifieke module door een specifieke uitvoer |

### <a name="condition"></a>Voorwaarde
De voorwaarde is optioneel in de declaratie van een route. Als u wilt dat alle berichten van de sink doorgeven aan de bron, laat u uit de **waar** component volledig. Of u kunt de [IoT Hub-querytaal](../iot-hub/iot-hub-devguide-routing-query-syntax.md) berichten of berichttypen die voldoen aan de voorwaarde voor bepaalde filteren. IoT Edge-routes bieden geen ondersteuning voor filters gebruiken om berichten op basis van de apparaatdubbel-tags of eigenschappen. 

De berichten die modules in IoT Edge tussen worden opgemaakt gelijk zijn aan de berichten die de apparaten en Azure IoT Hub tussen. Alle berichten worden opgemaakt als JSON en hebben **systemProperties**, **appProperties**, en **hoofdtekst** parameters. 

U kunt query's om een van de drie parameters op in de volgende syntaxis bouwen: 

* Systeemeigenschappen: `$<propertyName>` of `{$<propertyName>}`
* Eigenschappen van de toepassing: `<propertyName>`
* Eigenschappen van rapporthoofdtekst: `$body.<propertyName>` 

Zie voor meer voorbeelden over het maken van query's voor eigenschappen van berichten [apparaat-naar-cloud bericht routes query-expressies](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

Een voorbeeld die specifiek is voor IoT Edge is als u wilt filteren op berichten dat is ontvangen op een gateway-apparaat vanaf een leaf-apparaat. Berichten die afkomstig van modules zijn bevatten een systeemeigenschap, genaamd **connectionModuleId**. Dus als u routeren van berichten van het leaf-apparaten rechtstreeks naar IoT Hub wilt, gebruikt u de volgende route wilt uitsluiten van berichten van de module:

```query
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Sink
De sink definieert waar de berichten worden verzonden. Alleen modules en IoT-Hub ontvangen berichten. Berichten kunnen niet worden doorgestuurd naar andere apparaten. Er zijn geen opties jokertekens in de sink-eigenschap. 

De eigenschap sink is een van de volgende waarden:

| Sink | Description |
| ---- | ----------- |
| `$upstream` | Het bericht verzenden naar IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Het bericht verzenden naar een specifieke invoer van een specifieke module |

IoT Edge biedt op-één keer garanties. De Edge hub slaat de berichten lokaal in het geval een route kan niet het bericht niet naar de sink verzenden. Bijvoorbeeld, als de Edge hub kan geen verbinding met IoT Hub of de doel-module is niet verbonden.

Edge hub slaat de berichten tot aan de tijd die is opgegeven in de `storeAndForwardConfiguration.timeToLiveSecs` eigenschap van de [Edge hub gewenste eigenschappen](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Gewenste eigenschappen bijwerken of definiëren 

Het manifest implementatie Hiermee geeft u de gewenste eigenschappen voor elke module die is geïmplementeerd op het IoT Edge-apparaat. Gewenste eigenschappen in het manifest van de implementatie van overschreven de gewenste eigenschappen die momenteel in de moduledubbel.

Als u de gewenste eigenschappen van de moduledubbel in het manifest van de implementatie niet opgeeft, niet de moduledubbel op geen enkele manier gewijzigd in IoT Hub. In plaats daarvan kunt u de gewenste eigenschappen programmatisch instellen.

De dezelfde mechanismen waarmee u kunt wijzigen van apparaatdubbels worden gebruikt voor het wijzigen van moduledubbels. Zie voor meer informatie de [module dubbele Ontwikkelaarshandleiding voor](../iot-hub/iot-hub-devguide-module-twins.md).   

## <a name="deployment-manifest-example"></a>Voorbeeld van de implementatie-manifest

Het volgende voorbeeld laat zien hoe een geldige implementatie manifest document eruit kan zien.

```json
{
  "modulesContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "ContosoRegistry": {
                "username": "myacr",
                "password": "{password}",
                "address": "myacr.azurecr.io"
              }
            }
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
              "createOptions": ""
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
              "createOptions": ""
            }
          }
        },
        "modules": {
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
              "createOptions": "{}"
            }
          },
          "filtermodule": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "myacr.azurecr.io/filtermodule:latest",
              "createOptions": "{}"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {
          "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 10
        }
      }
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een volledige lijst van eigenschappen die kunnen of moeten worden opgenomen in $edgeAgent en $edgeHub [eigenschappen van de Edge agent en Edge hub](module-edgeagent-edgehub.md).

* Nu dat u hoe IoT Edge-modules worden gebruikt weet, [informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules](module-development.md).

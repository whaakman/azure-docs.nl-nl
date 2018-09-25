---
title: Azure IoT Edge modulesamenstelling | Microsoft Docs
description: Leer hoe een manifest van de implementatie wordt gedeclareerd welke modules te implementeren, om ze te implementeren, en hoe u berichtroutes daartussen maken.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a65eb029dbf10b194bd28bf7ad82f5aa839338a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990617"
---
# <a name="learn-how-to-use-deployment-manifests-to-deploy-modules-and-establish-routes"></a>Informatie over het gebruik van implementatie manifesten implementeren modules en routes tot stand brengen

Elke IoT Edge-apparaat wordt uitgevoerd ten minste twee modules: $edgeAgent en $edgeHub, die gezamenlijk de IoT Edge-runtime. Naast deze standaard twee kunt elk IoT Edge-apparaat meerdere modules om uit te voeren van een willekeurig aantal processen uitvoeren. Wanneer u alle deze modules in één keer op een apparaat implementeert, moet u een manier om aan te geven welke modules worden geleverd en hoe ze met elkaar communiceren. 

De *implementatie manifest* is een JSON-document dat wordt beschreven:

* De configuratie van de Edge-agent, met inbegrip van de installatiekopie van de container voor elke module, de referenties voor toegang tot persoonlijke containerregisters en instructies voor hoe elke module moet worden gemaakt en beheerd.
* De configuratie van de Edge hub, waaronder hoe berichten stromen tussen modules en uiteindelijk naar IoT Hub.
* (Optioneel) de gewenste eigenschappen van de moduledubbels.

Alle IoT Edge-apparaten moeten worden geconfigureerd met een manifest van de implementatie. Een nieuw geïnstalleerde IoT Edge-runtime rapporteert een foutcode totdat geconfigureerd met een manifest. 

In de Azure IoT Edge-zelfstudies bouwt u een manifest van de implementatie door te gaan met een wizard in de portal voor Azure IoT Edge. U kunt ook een implementatie-manifest programmatisch met behulp van REST- of de SDK voor IoT Hub-Service. Zie voor meer informatie, [inzicht in IoT Edge-implementaties][lnk-deploy].

## <a name="create-a-deployment-manifest"></a>Maken van een manifest van de implementatie

Het manifest van de implementatie op hoog niveau, Hiermee configureert u de gewenste eigenschappen van de moduledubbel voor IoT Edge-modules die zijn geïmplementeerd op een IoT Edge-apparaat. Twee van deze modules zijn altijd aanwezig: `$edgeAgent`, en `$edgeHub`.

Een implementatie-manifest dat alleen de IoT Edge-runtime (agent en de hub bevat) is geldig.

Deze structuur wordt gevolgd door het manifest:

```json
{
    "modulesContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
            }
        },
        "$edgeHub": {
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "{module1}": {  // optional
            "properties.desired": {
                // desired properties of module with id {module1}
            }
        },
        "{module2}": {  // optional
            ...
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Modules configureren

U moet de IoT Edge-runtime vertellen hoe ze moeten de modules in uw implementatie worden geïnstalleerd. De configuratie en beheer gegevens voor alle modules gaat binnen de **$edgeAgent** gewenste eigenschappen. Deze informatie omvat de configuratieparameters voor de Edge agent zelf. 

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

Edge hub biedt een manier om declaratief routeren van berichten tussen modules en tussen modules en IoT-Hub. De Edge hub beheert alle communicatie, zodat de informatie over de route komt binnen de **$edgeHub** gewenste eigenschappen. U kunt meerdere routes binnen dezelfde implementatie hebben.

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
De bron geeft aan waar de berichten afkomstig zijn uit. Het kan een van de volgende waarden zijn:

| Bron | Beschrijving |
| ------ | ----------- |
| `/*` | Alle apparaat-naar-cloud-berichten van elk apparaat of de module |
| `/messages/*` | Een apparaat-naar-cloud bericht verzonden door een apparaat of een module door bepaalde of geen uitvoer |
| `/messages/modules/*` | Een apparaat-naar-cloud bericht verzonden door een module door bepaalde of geen uitvoer |
| `/messages/modules/{moduleId}/*` | Een apparaat-naar-cloud bericht wordt verzonden door {moduleId} met geen uitvoer |
| `/messages/modules/{moduleId}/outputs/*` | Een apparaat-naar-cloud bericht wordt verzonden door {moduleId} met enkele uitvoer |
| `/messages/modules/{moduleId}/outputs/{output}` | Een apparaat-naar-cloud bericht verzonden met behulp van {moduleId} {uitvoer} |

### <a name="condition"></a>Voorwaarde
De voorwaarde is optioneel in de declaratie van een route. Als u wilt dat alle berichten van de sink doorgeven aan de bron, laat u uit de **waar** component volledig. Of u kunt de [IoT Hub-querytaal] [ lnk-iothub-query] berichten of berichttypen die voldoen aan de voorwaarde voor bepaalde filteren.

De berichten die modules in IoT Edge tussen worden opgemaakt gelijk zijn aan de berichten die de apparaten en Azure IoT Hub tussen. Alle berichten worden opgemaakt als JSON en hebben **systemProperties**, **appProperties**, en **hoofdtekst** parameters. 

U kunt query's over alle drie de parameters met de volgende syntaxis bouwen: 

* Systeemeigenschappen: `$<propertyName>` of `{$<propertyName>}`
* Eigenschappen van de toepassing: `<propertyName>`
* Eigenschappen van rapporthoofdtekst: `$body.<propertyName>` 

Zie voor meer voorbeelden over het maken van query's voor eigenschappen van berichten [apparaat-naar-cloud bericht routes query-expressies](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

Een voorbeeld die specifiek is voor IoT Edge is als u wilt filteren op berichten dat is ontvangen op een gateway-apparaat vanaf een leaf-apparaat. Berichten die afkomstig van modules zijn bevatten een systeemeigenschap, genaamd **connectionModuleId**. Dus als u routeren van berichten van het leaf-apparaten rechtstreeks naar IoT Hub wilt, gebruikt u de volgende route wilt uitsluiten van berichten van de module:

```sql
FROM /messages/\* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Sink
De sink definieert waar de berichten worden verzonden. Het kan een van de volgende waarden zijn:

| Sink | Beschrijving |
| ---- | ----------- |
| `$upstream` | Het bericht verzenden naar IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Verzenden van het bericht voor het invoeren van `{input}` van module `{moduleId}` |

IoT Edge biedt op-één keer garanties. De Edge hub slaat de berichten lokaal in het geval een route kan niet het bericht niet naar de sink verzenden. Bijvoorbeeld, als de Edge hub kan geen verbinding met IoT Hub of de doel-module is niet verbonden.

Edge hub slaat de berichten tot aan de tijd die is opgegeven in de `storeAndForwardConfiguration.timeToLiveSecs` eigenschap van de [Edge hub gewenste eigenschappen](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Gewenste eigenschappen bijwerken of definiëren 

Het manifest van de implementatie kunt gewenste eigenschappen opgeven voor de moduledubbel van elke module die is geïmplementeerd op het IoT Edge-apparaat. Als de gewenste eigenschappen zijn opgegeven in het manifest van de implementatie, overschrijven ze elke gewenste eigenschappen die momenteel in de moduledubbel.

IoT Hub wordt niet de moduledubbel op geen enkele manier wijzigen als u niet de gewenste eigenschappen van de moduledubbel in het manifest van de implementatie opgeeft, en kunt u zich op de gewenste eigenschappen programmatisch in te stellen.

De dezelfde mechanismen waarmee u kunt wijzigen van apparaatdubbels worden gebruikt voor het wijzigen van moduledubbels. Zie voor meer informatie de [device twin Ontwikkelaarshandleiding voor](../iot-hub/iot-hub-devguide-device-twins.md).   

## <a name="deployment-manifest-example"></a>Voorbeeld van de implementatie-manifest

Dit een voorbeeld van een implementatie manifest JSON-document.

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

* Nu dat u hoe IoT Edge-modules worden gebruikt weet, [informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-routing-query-syntax.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md

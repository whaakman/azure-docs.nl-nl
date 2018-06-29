---
title: Azure IoT rand modulesamenstelling | Microsoft Docs
description: Informatie over hoe een deployment manifest wordt gedeclareerd welke modules voor het implementeren van het implementeren ervan en het berichtroutes tussen hen maken.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 209f159d9003838edb36728828758b76730118ff
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098461"
---
# <a name="learn-how-to-use-deployment-manifests-to-deploy-modules-and-establish-routes"></a>Informatie over het gebruik van implementatiemanifesten modules implementeren en routes tot stand brengen

Elke IoT randapparaat voert ten minste twee modules: $edgeAgent en $edgeHub, waaruit de rand van de IoT-runtime. Naast de standaard twee kunt elke IoT randapparaat meerdere modules voor het uitvoeren van een willekeurig aantal processen uitvoeren. Wanneer u alle deze modules tegelijk op een apparaat implementeert, moet u een manier om aan te geven welke modules zijn opgenomen en hoe ze met elkaar communiceren. 

De *implementatiemanifest* is een JSON-document dat wordt beschreven:

* De configuratie van de Edge-agent, waaronder de installatiekopie van de container voor elke module, de referenties voor toegang tot persoonlijke container registers en instructies voor hoe elke module moet worden gemaakt en beheerd.
* De configuratie van de Edge-hub die bevat hoe berichten stromen tussen modules en uiteindelijk naar IoT Hub.
* Optioneel, de gewenste eigenschappen van de module horende.

Alle rand van de IoT-apparaten moeten worden geconfigureerd met een implementatiemanifest. Een nieuw geïnstalleerde IoT rand runtime rapporten een foutcode totdat geconfigureerd met een manifest. 

In de zelfstudies voor Azure IoT Edge bouwt u een manifest voor implementatie door te gaan met een wizard in de portal voor Azure IoT rand. U kunt ook een deployment manifest programmatisch met behulp van REST- of de IoT-Hub SDK toepassen. Zie voor meer informatie [begrijpen IoT rand implementaties][lnk-deploy].

## <a name="create-a-deployment-manifest"></a>Maken van een manifest voor implementatie

Het implementatiemanifest configureert op een hoog niveau, een module-twin gewenste eigenschappen voor IoT Edge-modules die zijn geïmplementeerd op een Edge van de IoT-apparaat. Twee van deze modules altijd aanwezig zijn: `$edgeAgent`, en `$edgeHub`.

Een implementatiemanifest dat bevat alleen de rand van de IoT-runtime (agent en hub) ongeldig is.

Het manifest volgt deze structuur:

```json
{
    "moduleContent": {
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

U moet de rand van de IoT-runtime uitleggen hoe de modules installeren in uw implementatie. De configuratie en beheer van gegevens voor alle modules gaat binnen de **$edgeAgent** gewenst eigenschappen. Deze informatie omvat de configuratieparameters voor de Edge-agent zelf. 

Zie voor een volledige lijst met eigenschappen die kunnen of moeten worden opgenomen, [eigenschappen van de rand agent en de rand hub](module-edgeagent-edgehub.md).

De eigenschappen $edgeAgent volgt u deze structuur:

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

Rand hub biedt een manier declaratief om berichten te routeren tussen modules en tussen modules en IoT Hub. De hub rand beheert alle communicatie, zodat de route-informatie gaat binnen de **$edgeHub** gewenst eigenschappen. U kunt meerdere routes binnen dezelfde implementatie hebben.

Routes zijn gedefinieerd in de **$edgeHub** gewenst eigenschappen met de volgende syntaxis:

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

Elke route moet een bron- en een sink, maar de voorwaarde is een optioneel onderdeel dat u gebruiken kunt voor het filteren van berichten. 


### <a name="source"></a>Bron
De bron geeft aan waar de berichten afkomstig zijn uit. Dit kan een van de volgende waarden zijn:

| Bron | Beschrijving |
| ------ | ----------- |
| `/*` | Alle apparaat-naar-cloud-berichten van elk apparaat of de module |
| `/messages/*` | Elk apparaat-naar-cloud bericht verzonden door een apparaat of een module via bepaalde of geen uitvoer |
| `/messages/modules/*` | Elk apparaat-naar-cloud bericht verzonden door een module via bepaalde of geen uitvoer |
| `/messages/modules/{moduleId}/*` | Een apparaat-naar-cloud bericht wordt verzonden door {moduleId} met geen uitvoer |
| `/messages/modules/{moduleId}/outputs/*` | Een apparaat-naar-cloud bericht wordt verzonden door {moduleId} met een uitvoer |
| `/messages/modules/{moduleId}/outputs/{output}` | Elk apparaat-naar-cloud bericht verzonden met behulp van {moduleId} {uitvoergegevens} |

### <a name="condition"></a>Voorwaarde
De voorwaarde is optioneel in de declaratie van een route. Als u wilt dat alle berichten van de sink doorgeven aan de bron, stelt u uit de **waar** component volledig. Of u kunt de [IoT Hub-querytaal] [ lnk-iothub-query] om te filteren op bepaalde berichten of berichttypen die voldoen aan de voorwaarde.

De berichten die tussen modules in IoT Edge worden doorgegeven, worden opgemaakt hetzelfde zijn als de berichten die tussen uw apparaten en de Azure IoT Hub worden doorgegeven. Alle berichten worden opgemaakt als JSON en hebben **systemProperties**, **appProperties**, en **hoofdtekst** parameters. 

U kunt bouwen query's om alle drie parameters met de volgende syntaxis: 

* Systeemeigenschappen: `$<propertyName>` of `{$<propertyName>}`
* Toepassingseigenschappen van de: `<propertyName>`
* Instantie-eigenschappen: `$body.<propertyName>` 

Zie voor voorbeelden over het maken van query's voor berichteigenschappen [apparaat-naar-cloud bericht routes query-expressies](../iot-hub/iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).

Een voorbeeld die specifiek is voor de IoT-rand is wanneer u wilt filteren op berichten dat is ontvangen op een gateway-apparaat van een leaf-apparaat. Berichten die afkomstig van modules zijn bevatten een systeemeigenschap **connectionModuleId**. Dus als u routeren van berichten van leaf-apparaten rechtstreeks naar IoT Hub wilt, gebruikt u de volgende route moeten worden uitgesloten van berichten van de module:

```sql
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Sink
De sink definieert waar de berichten worden verzonden. Dit kan een van de volgende waarden zijn:

| Sink | Beschrijving |
| ---- | ----------- |
| `$upstream` | Het bericht verzenden met IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Verzenden van het bericht als invoer `{input}` van module `{moduleId}` |

IoT-Edge biedt op in de minste eenmaal garanties. De hub rand slaat berichten lokaal geval een route kan niet het bericht niet naar de sink verzenden. Bijvoorbeeld, als de Edge-hub kan geen verbinding met IoT-Hub of de doel-module is niet verbonden.

Rand hub slaat de berichten naar de tijd die is opgegeven de `storeAndForwardConfiguration.timeToLiveSecs` eigenschap van de [rand hub gewenst eigenschappen](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Definiëren of de eigenschappen van de gewenste bijwerken 

Het implementatiemanifest kunt gewenste eigenschappen opgeven voor de module-twin van elke module die is geïmplementeerd op de rand van de IoT-apparaat. Als de gewenste eigenschappen zijn opgegeven in het implementatiemanifest, overschrijven ze de gewenste eigenschappen die momenteel in de module-twin.

Als u een module-twin gewenste eigenschappen in het implementatiemanifest niet opgeeft, IoT-Hub wordt de module-twin op elke manier hierdoor niet gewijzigd en u zich voor het instellen van de gewenste eigenschappen programmatisch.

Dezelfde mechanismen waarmee u kunt wijzigen horende apparaten worden gebruikt voor het wijzigen van de module horende. Zie voor meer informatie de [apparaat twin ontwikkelaarshandleiding](../iot-hub/iot-hub-devguide-device-twins.md).   

## <a name="deployment-manifest-example"></a>Voorbeeld van de implementatie-manifest

Deze een voorbeeld van een deployment manifest JSON-document.

```json
{
  "moduleContent": {
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

* Zie voor een volledige lijst van eigenschappen die kunnen of moeten worden opgenomen in $edgeAgent en $edgeHub [eigenschappen van de rand agent en de rand hub](module-edgeagent-edgehub.md).

* Als u weet hoe de rand van de IoT-modules worden gebruikt, [de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules begrijpen][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md

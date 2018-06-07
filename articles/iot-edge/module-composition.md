---
title: Azure IoT rand modulesamenstelling | Microsoft Docs
description: Meer informatie over wat in Azure IoT rand modules terechtkomt en hoe ze kunnen worden hergebruikt
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/23/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c886d1d9dea120a243693c12ae861a58126daadc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631680"
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Begrijpen hoe de rand van de IoT-modules kunnen worden gebruikt, is geconfigureerd, en hergebruikt - voorbeeld

Elke IoT randapparaat voert ten minste twee modules: $edgeAgent en $edgeHub, waaruit de rand van de IoT-runtime. Naast de standaard twee kunt elke IoT randapparaat meerdere modules voor het uitvoeren van een willekeurig aantal processen uitvoeren. Wanneer u alle deze modules tegelijk op een apparaat implementeert, moet u een manier om aan te geven welke modules zijn opgenomen hoe ze met elkaar communiceren. 

De *implementatiemanifest* is een JSON-document dat wordt beschreven:

* Welke rand van de IoT-modules moeten worden geïmplementeerd, samen met hun opties maken en beheren.
* De configuratie van de Edge-hub die bevat hoe berichten stromen tussen modules en uiteindelijk naar IoT Hub.
* Eventueel de waarden worden ingesteld in de gewenste eigenschappen van de horende module voor het configureren van de afzonderlijke module-toepassingen.

Alle rand van de IoT-apparaten moeten worden geconfigureerd met een implementatiemanifest. Een nieuw geïnstalleerde IoT rand runtime rapporten een foutcode totdat geconfigureerd met een manifest. 

In de zelfstudies voor Azure IoT Edge bouwt u een manifest voor implementatie door te gaan met een wizard in de portal voor Azure IoT rand. U kunt ook een deployment manifest programmatisch met behulp van REST- of de IoT-Hub SDK toepassen. Raadpleeg [implementeren en bewaken] [ lnk-deploy] voor meer informatie over IoT Edge-implementaties.

## <a name="create-a-deployment-manifest"></a>Maken van een manifest voor implementatie

Het implementatiemanifest configureert op een hoog niveau, een module-twin gewenste eigenschappen voor IoT Edge-modules die zijn geïmplementeerd op een Edge van de IoT-apparaat. Twee van deze modules altijd aanwezig zijn: de Edge-agent en de Edge-hub.

Een implementatiemanifest dat bevat alleen de rand van de IoT-runtime (agent en hub) ongeldig is.

Het manifest volgt deze structuur:

```json
{
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
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

Naast het tot stand brengen van de gewenste eigenschappen van modules die u wilt implementeren, moet u de rand van de IoT-runtime uitleggen hoe ze te installeren. De configuratie en beheer van gegevens voor alle modules gaat binnen de **$edgeAgent** gewenst eigenschappen. Deze informatie omvat de configuratieparameters voor de Edge-agent zelf. 

Zie voor een volledige lijst met eigenschappen die kunnen of moeten worden opgenomen, [eigenschappen van de rand agent en de rand hub](module-edgeagent-edgehub.md).

De eigenschappen $edgeAgent volgt u deze structuur:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
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
            "{module1}": { //optional
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

Het is belangrijk te weten dat rand hub op in de minste eenmaal garanties, wat betekent biedt dat berichten lokaal worden opgeslagen als een route kan niet het bericht niet naar de sink verzenden, bijvoorbeeld de Edge-hub kan geen verbinding met IoT-Hub of de doel-module is niet verbonden.

Rand hub slaat de berichten naar de tijd die is opgegeven de `storeAndForwardConfiguration.timeToLiveSecs` eigenschap van de [rand hub gewenst eigenschappen](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Definiëren of de eigenschappen van de gewenste bijwerken 

Het implementatiemanifest kunt gewenste eigenschappen opgeven voor de module-twin van elke module die is geïmplementeerd op de rand van de IoT-apparaat. Als de gewenste eigenschappen zijn opgegeven in het implementatiemanifest, overschrijven ze de gewenste eigenschappen die momenteel in de module-twin.

Als u een module-twin gewenste eigenschappen in het implementatiemanifest niet opgeeft, IoT-Hub wordt de module-twin op elke manier hierdoor niet gewijzigd en u zich voor het instellen van de gewenste eigenschappen programmatisch.

Dezelfde mechanismen waarmee u kunt wijzigen horende apparaten worden gebruikt voor het wijzigen van de module horende. Raadpleeg de [apparaat twin ontwikkelaarshandleiding](../iot-hub/iot-hub-devguide-device-twins.md) voor meer informatie.   

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
                    "loggingOptions": ""
                }
            },
            "systemModules": {
                "edgeAgent": {
                    "type": "docker",
                    "settings": {
                    "image": "microsoft/azureiotedge-agent:1.0-preview",
                    "createOptions": ""
                    }
                },
                "edgeHub": {
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "microsoft/azureiotedge-hub:1.0-preview",
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
                    "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
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

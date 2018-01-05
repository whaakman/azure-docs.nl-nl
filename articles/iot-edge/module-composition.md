---
title: Azure IoT rand modulesamenstelling | Microsoft Docs
description: Meer informatie over wat in Azure IoT rand modules terechtkomt en hoe ze kunnen worden hergebruikt
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f3bc2f14b182e502c651ff44ef49b88cd34e1f50
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Begrijpen hoe de rand van de IoT-modules kunnen worden gebruikt, is geconfigureerd, en hergebruikt - voorbeeld

Azure IoT-rand kunt u voor het opstellen van meerdere IoT rand modules voordat u IoT Edge-apparaten implementeert. Dit artikel wordt uitgelegd dat de belangrijkste concepten rond het opstellen van meerdere IoT rand modules vóór de implementatie. 

## <a name="the-deployment-manifest"></a>Het implementatiemanifest
De *implementatiemanifest* is een JSON-document dat wordt beschreven:

* Welke rand van de IoT-modules moeten worden geïmplementeerd, samen met hun maken en beheeropties;
* De configuratie van de rand hub, beschrijven hoe berichten moeten stromen tussen modules en tussen modules en IoT Hub;
* Eventueel de waarden worden ingesteld in de gewenste eigenschappen van de horende module voor het configureren van de afzonderlijke module-toepassingen.

In de zelfstudies voor Azure IoT Edge bouwt u een manifest voor implementatie door te gaan met een wizard in de portal voor Azure IoT rand. U kunt ook een deployment manifest programmatisch met behulp van REST- of de IoT-Hub SDK toepassen. Raadpleeg [implementeren en bewaken] [ lnk-deploy] voor meer informatie over IoT Edge-implementaties.

Het implementatiemanifest configureert op een hoog niveau de gewenste eigenschappen van de rand van de IoT-modules geïmplementeerd op een Edge van de IoT-apparaat. Twee van deze modules altijd aanwezig zijn: de Edge-agent en de Edge-hub.

Het manifest volgt deze structuur:

    {
        "moduleContent": {
            "$edgeAgent": {
                "properties.desired": {
                    // desired properties of the Edge agent
                }
            },
            "$edgeHub": {
                "properties.desired": {
                    // desired properties of the Edge hub
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

Een voorbeeld van een implementatiemanifest is gerapporteerd aan het einde van deze sectie.

> [!IMPORTANT]
> Alle rand van de IoT-apparaten moeten worden geconfigureerd met een implementatiemanifest. Een nieuw geïnstalleerde IoT rand runtime rapporten een foutcode totdat geconfigureerd met een manifest. 

### <a name="specify-the-modules"></a>Geef de modules
De gewenste eigenschappen van de module-twin van de Edge-agent bevatten: de gewenste modules, hun configuratie en beheer opties, samen met de parameters voor de configuratie voor de agent van de rand.

Op hoog niveau bevat dit gedeelte van het manifest de verwijzingen naar de module-installatiekopieën en opties voor de IoT-rand runtime-modules (Microsoft Edge-agent en rand hub) en de gebruiker opgegeven modules.

Raadpleeg de [gewenst eigenschappen van de rand agent] [ lnk-edgeagent-desired] voor de gedetailleerde beschrijving van deze sectie van het manifest.

> [!NOTE]
> Een manifest voor implementatie met alleen de rand van de IoT-runtime (agent en hub) is ongeldig.


### <a name="specify-the-routes"></a>Geef de routes
Rand hub biedt een manier declaratief om berichten te routeren tussen modules en tussen modules en IoT Hub.

Routes hebben de volgende syntaxis:

        FROM <source>
        [WHERE <condition>]
        INTO <sink>

De *bron* kan van alles van de volgende zijn:

| Bron | Beschrijving |
| ------ | ----------- |
| `/*` | Alle apparaat-naar-cloud-berichten van elk apparaat of de module |
| `/messages/*` | Elk apparaat-naar-cloud bericht verzonden door een apparaat of een module via bepaalde of geen uitvoer |
| `/messages/modules/*` | Elk apparaat-naar-cloud bericht verzonden door een module via bepaalde of geen uitvoer |
| `/messages/modules/{moduleId}/*` | Een apparaat-naar-cloud bericht wordt verzonden door {moduleId} met geen uitvoer |
| `/messages/modules/{moduleId}/outputs/*` | Een apparaat-naar-cloud bericht wordt verzonden door {moduleId} met een uitvoer |
| `/messages/modules/{moduleId}/outputs/{output}` | Elk apparaat-naar-cloud bericht verzonden met behulp van {moduleId} {uitvoergegevens} |

De voorwaarde is een voorwaarde wordt ondersteund door de [IoT Hub-querytaal] [ lnk-iothub-query] voor regels voor het doorsturen van IoT-Hub.

De sink zijn:

| sink | Beschrijving |
| ---- | ----------- |
| `$upstream` | Het bericht verzenden met IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Verzenden van het bericht als invoer `{input}` van module`{moduleId}` |

Het is belangrijk te weten dat rand hub op in de minste eenmaal garanties, wat betekent biedt dat berichten lokaal worden opgeslagen als een route kan niet het bericht niet naar de sink verzenden, bijvoorbeeld de Edge-hub kan geen verbinding met IoT-Hub of de doel-module is niet verbonden.

Rand hub slaat de berichten naar de tijd die is opgegeven de `storeAndForwardConfiguration.timeToLiveSecs` eigenschap van de rand hub gewenst eigenschappen.

### <a name="specifying-the-desired-properties-of-the-module-twin"></a>Geef de gewenste eigenschappen van de module-twin

Het implementatiemanifest kunt de gewenste eigenschappen van de module-twin van elk van de gebruikersmodules die zijn opgegeven in het gedeelte van de rand agent opgeven.

Als de gewenste eigenschappen zijn opgegeven in het implementatiemanifest, overschrijven ze de gewenste eigenschappen die momenteel in de module-twin.

Als u een module-twin gewenste eigenschappen in het implementatiemanifest niet opgeeft, IoT-Hub wordt de module-twin op elke manier hierdoor niet gewijzigd en u zich voor het instellen van de gewenste eigenschappen programmatisch.

### <a name="deployment-manifest-example"></a>Voorbeeld van de implementatie-manifest

Deze een voorbeeld van een deployment manifest JSON-document.

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

## <a name="reference-edge-agent-module-twin"></a>Naslaginformatie: Rand agent module twin

De module-twin voor de agent rand wordt aangeroepen `$edgeAgent` en coördineert de communicatie tussen de rand agent wordt uitgevoerd op een apparaat en IoT Hub.
De gewenste eigenschappen worden ingesteld wanneer een manifest voor implementatie op een specifiek apparaat toegepast als onderdeel van een implementatie met één apparaat of geschaald. Zie [implementatie en controle] [ lnk-deploy] voor meer informatie over het implementeren van modules die zich in IoT Edge-apparaten.

### <a name="edge-agent-twin-desired-properties"></a>Rand twin gewenst Agenteigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| Runtime.type | Moet 'docker' | Ja |
| runtime.settings.minDockerVersion | Ingesteld op de minimaal vereiste door deze implementatiemanifest Docker-versie | Ja |
| runtime.settings.loggingOptions | Een stringified JSON met de opties voor logboekregistratie voor de container van de agent rand. [Docker-opties voor logboekregistratie][lnk-docker-logging-options] | Nee |
| systemModules.edgeAgent.type | Moet 'docker' | Ja |
| systemModules.edgeAgent.settings.image | De URI van de installatiekopie van de Edge-agent. Op dit moment wordt kan de Edge-agent niet automatisch te laten bijwerken. | Ja |
| systemModules.edgeAgent.settings.createOptions | Een stringified JSON met de opties voor het maken van de rand agent-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| systemModules.edgeAgent.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze wordt ingesteld door de IoT Hub wanneer dit manifest wordt toegepast met behulp van een implementatie. Geen deel uit van een manifest voor implementatie. |
| systemModules.edgeHub.type | Moet 'docker' | Ja |
| systemModules.edgeHub.status | Heeft ' actief ' | Ja |
| systemModules.edgeHub.restartPolicy | Moet 'altijd' | Ja |
| systemModules.edgeHub.settings.image | De URI van de installatiekopie van de Edge-hub. | Ja |
| systemModules.edgeHub.settings.createOptions | Een stringified JSON met de opties voor het maken van de rand hub-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| systemModules.edgeHub.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze wordt ingesteld door de IoT Hub wanneer dit manifest wordt toegepast met behulp van een implementatie. Geen deel uit van een manifest voor implementatie. |
| modules. {moduleId} .version | Een gebruiker gedefinieerde tekenreeks die de versie van deze module vertegenwoordigt. | Ja |
| modules. {moduleId} .type | Moet 'docker' | Ja |
| modules. {moduleId} .restartPolicy | {{'nooit' \| 'op-is mislukt' \| 'op-slecht' \| "always"} | Ja |
| modules. {moduleId}.settings.image | De URI op de installatiekopie van de module. | Ja |
| modules. {moduleId}.settings.createOptions | Een stringified JSON met de opties voor het maken van de module-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| modules. {moduleId}.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze wordt ingesteld door de IoT Hub wanneer dit manifest wordt toegepast met behulp van een implementatie. Geen deel uit van een manifest voor implementatie. |

### <a name="edge-agent-twin-reported-properties"></a>Rand agent twin gerapporteerd eigenschappen

De agent rand gerapporteerd eigenschappen zijn onder meer de drie belangrijkste stukjes informatie:

1. De status van de toepassing van de laatst gezien gewenste eigenschappen;
2. De status van de modules die momenteel worden uitgevoerd op het apparaat, zoals gemeld door de agent rand; en
3. Een kopie van de gewenste eigenschappen die momenteel worden uitgevoerd op het apparaat.

Deze laatste stukje informatie is nuttig wanneer de meest recente gewenste eigenschappen niet door de runtime toegepast zijn en het apparaat wordt nog steeds uitgevoerd voor een eerdere versie van het implementatiemanifest.

> [!NOTE]
> De gerapporteerde eigenschappen van de Edge-agent zijn handig als ze kunnen worden opgevraagd met de [IoT Hub-querytaal] [ lnk-iothub-query] voor het onderzoeken van de status van implementaties op grote schaal. Raadpleeg [implementaties] [ lnk-deploy] voor meer informatie over het gebruik van deze functie.

De volgende tabel bevat niet de informatie die wordt opgehaald uit de gewenste eigenschappen.

| Eigenschap | Beschrijving |
| -------- | ----------- |
| lastDesiredVersion | Deze int verwijst naar de laatste versie van de gewenste eigenschappen verwerkt door de agent van de rand. |
| lastDesiredStatus.code | Dit zijn de statuscode die verwijst naar de laatste gewenste eigenschappen die zichtbaar zijn voor de agent van de rand. Toegestane waarden: `200` geslaagd, `400` ongeldige configuratie `412` Ongeldige schemaversie `417` de gewenste eigenschappen zijn leeg is, `500` mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| DeviceHealth | `healthy`Als de runtimestatus van de van alle modules `running` of `stopped`, `unhealthy` anders |
| configurationHealth. .health {deploymentId} | `healthy`Als de runtimestatus van de van alle modules die zijn ingesteld door de implementatie {deploymentId} `running` of `stopped`, `unhealthy` anders |
| runtime.platform.OS | Melden van het besturingssysteem dat wordt uitgevoerd op het apparaat |
| Runtime.platform.Architecture | Rapportage van de architectuur van de CPU-capaciteit op het apparaat |
| systemModules.edgeAgent.runtimeStatus | De gerapporteerde status van agent van de rand: {'actief' \| 'slechte'} |
| systemModules.edgeAgent.statusDescription | De tekstbeschrijving van de gerapporteerde status van de agent van de rand. |
| systemModules.edgeHub.runtimeStatus | Huidige status van de rand hub: {'actief' \| 'is gestopt' \| 'is mislukt' \| 'backoff' \| 'slechte'} |
| systemModules.edgeHub.statusDescription | De tekstbeschrijving van de huidige status van de rand hub als slecht. |
| systemModules.edgeHub.exitCode | Als het is afgesloten, de afsluitcode gerapporteerd door de hub rand container |
| systemModules.edgeHub.startTimeUtc | Tijdstip waarop rand hub voor het laatst is gestart |
| systemModules.edgeHub.lastExitTimeUtc | Tijd wanneer Edge hub laatste is afgesloten |
| systemModules.edgeHub.lastRestartTimeUtc | Keer wanneer Edge hub laatst opnieuw is gestart |
| systemModules.edgeHub.restartCount | Het aantal keren dat deze module opnieuw als onderdeel van het beleid opnieuw op te starten gestart is. |
| modules. {moduleId} .runtimeStatus | Huidige status van de module: {'actief' \| 'is gestopt' \| 'is mislukt' \| 'backoff' \| 'slechte'} |
| modules. {moduleId} .statusDescription | Beschrijving van de huidige status van de module als slecht. |
| modules. {moduleId} .exitCode | Als het is afgesloten, de afsluitcode gerapporteerd door de module-container |
| modules. {moduleId} .startTimeUtc | Tijdstip waarop de module voor het laatst is gestart |
| modules. {moduleId} .lastExitTimeUtc | Tijd wanneer de module laatste is afgesloten |
| modules. {moduleId} .lastRestartTimeUtc | Tijd wanneer de module voor het laatst is gestart |
| modules. {moduleId} .restartCount | Het aantal keren dat deze module opnieuw als onderdeel van het beleid opnieuw op te starten gestart is. |

## <a name="reference-edge-hub-module-twin"></a>Naslaginformatie: Rand hub module twin

De module-twin voor de hub rand heet `$edgeHub` en coördineert de communicatie tussen de rand hub uitgevoerd op een apparaat en IoT Hub.
De gewenste eigenschappen worden ingesteld wanneer een manifest voor implementatie op een specifiek apparaat toegepast als onderdeel van een implementatie met één apparaat of geschaald. Zie [implementaties] [ lnk-deploy] voor meer informatie over het implementeren van modules die zich in IoT Edge-apparaten.

### <a name="edge-hub-twin-desired-properties"></a>Rand hub twin gewenst eigenschappen

| Eigenschap | Beschrijving | In het implementatiemanifest is vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| routes. {routeName} | Een tekenreeks die een Edge hub route vertegenwoordigt. | De `routes` element kunt aanwezig maar leeg zijn. |
| storeAndForwardConfiguration.timeToLiveSecs | De tijd in seconden waarmee rand hub blijft berichten in geval van een niet-verbonden routering eindpunten, bijvoorbeeld losgekoppeld van de IoT-Hub of lokale module | Ja |

### <a name="edge-hub-twin-reported-properties"></a>Rand hub twin gerapporteerd eigenschappen

| Eigenschap | Beschrijving |
| -------- | ----------- |
| lastDesiredVersion | Deze int verwijst naar de laatste versie van de gewenste eigenschappen verwerkt door de Edge-hub. |
| lastDesiredStatus.code | Dit zijn de statuscode die verwijst naar de laatste gewenste eigenschappen die zichtbaar zijn voor de Edge-hub. Toegestane waarden: `200` geslaagd, `400` ongeldige configuratie `500` mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| -clients. {apparaat of module identiteit} .status | De status van dit apparaat of de module. Mogelijke waarden {'verbonden' \| "de verbinding verbroken'}. Module-identiteiten kunnen zich in de status voor verbroken verbindingen. Downstream apparaten verbinden met rand hub worden alleen weergegeven wanneer verbonden. |
| -clients. {apparaat of module identiteit} .lastConnectTime | Laatste keer dat het apparaat of de module verbonden |
| -clients. {apparaat of module identiteit} .lastDisconnectTime | Laatste keer dat het apparaat of de module verbroken |

## <a name="next-steps"></a>Volgende stappen

Als u weet hoe de rand van de IoT-modules worden gebruikt, [de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules begrijpen][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-edgeagent-desired]: #edge-agent-twin-desired-properties
[lnk-edgeagent-reported]: #edge-agent-twin-reported-properties
[lnk=edgehub-desired]: #edge-hub-twin-desired-properties
[lnk-edgehub-reported]: #edge-hub-twin-reported-properties
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md

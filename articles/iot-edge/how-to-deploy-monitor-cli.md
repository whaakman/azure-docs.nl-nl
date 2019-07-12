---
title: Automatische implementaties maken uit vanaf de opdrachtregel - Azure IoT Edge | Microsoft Docs
description: De IoT-extensie voor Azure CLI gebruiken voor automatische implementaties voor groepen van IoT Edge-apparaten maken
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 61a3c1cdccf01b266581a13fe3c660bd57f59b2c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67796207"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Implementeren en bewaken van IoT Edge-modules op schaal met behulp van de Azure CLI

Maak een **automatische implementatie van IoT Edge** met behulp van de Azure-opdrachtregelinterface voor het beheren van doorlopende implementaties voor een groot aantal apparaten in één keer. Automatische implementaties voor IoT Edge maken deel uit van de [automatische Apparaatbeheer](/azure/iot-hub/iot-hub-automatic-device-management) functie van IoT-Hub. Implementaties zijn dynamische processen waarmee u kunt meerdere modules implementeren op meerdere apparaten, de status en integriteit van de modules volgen en wijzig indien nodig. 

Zie voor meer informatie, [inzicht in IoT Edge-automatische implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md).

In dit artikel hebt instellen u Azure CLI en de IoT-extensie. Vervolgens leert u hoe u modules naar een set met IoT Edge-apparaten implementeren en de voortgang bekijken met behulp van de beschikbare CLI-opdrachten.

## <a name="cli-prerequisites"></a>CLI-vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [IoT Edge-apparaten](how-to-register-device-cli.md) met IoT Edge-runtime geïnstalleerd.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw Azure CLI-versie moet ten minste 2.0.24 of hoger. Gebruik `az --version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Een manifest van de implementatie configureren

Het manifest voor een implementatie is een JSON-document waarin wordt beschreven welke modules te implementeren, hoe gegevens stromen tussen de modules, en de gewenste eigenschappen van de moduledubbels. Zie voor meer informatie, [meer informatie over het implementeren van modules en routes instellen in IoT Edge](module-composition.md).

Sla het manifest van implementatie lokaal als een txt-bestand voor het implementeren van modules met behulp van Azure CLI. Gebruikt u het bestandspad in de volgende sectie tijdens het uitvoeren van de opdracht uit om de configuratie toepassen op uw apparaat. 

Hier volgt een manifest eenvoudige implementatie met één module als een voorbeeld:

```json
{
  "content": {
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
                "registryName": {
                  "username": "",
                  "password": "",
                  "address": ""
                }
              }
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": "{}"
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{}"
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
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "route": "FROM /* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      },
      "tempSensor": {
        "properties.desired": {}
      }
    }
  }
}
```

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met behulp van tags

Voordat u een implementatie maken kunt, moet u opgeven welke apparaten die u wilt toepassen. Azure IoT Edge-apparaten met identificeert **tags** op het dubbele apparaat. Elk apparaat kan hebben meerdere labels die u op een manier die zinvol is voor uw oplossing definieert. Als u een campus van slimme gebouwen beheert, kunt u bijvoorbeeld de volgende codes toevoegen aan een apparaat:

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

Zie voor meer informatie over apparaatdubbels en tags [apparaatdubbels begrijpen en gebruiken in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Een implementatie maken

U implementeren modules naar uw apparaten door het maken van een implementatie die uit een manifest voor de implementatie, evenals andere parameters bestaat. 

Gebruik de [az iot edge-implementatie maken](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-create) opdracht om een implementatie te maken:

```cli
az iot edge deployment create --deployment-id [deployment id] --hub-name [hub name] --content [file path] --labels "[labels]" --target-condition "[target query]" --priority [int]
```

De implementatie maakt opdracht wordt de volgende parameters: 

* **--implementatie-id** -de naam van de implementatie die wordt gemaakt in de IoT-hub. Geef uw implementatie een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie wordt gemaakt. De hub moet zich in het huidige abonnement. Wijzigen van uw huidige abonnement met de `az account set -s [subscription name]` opdracht.
* **--inhoud** -bestandspad naar de implementatie manifest van de JSON. 
* **--labels** -labels voor het bijhouden van uw implementaties toevoegen. Labels zijn naam, waarde-paren die uw implementatie te beschrijven. Labels worden JSON opmaak voor de namen en waarden. Bijvoorbeeld: `{"HostPlatform":"Linux", "Version:"3.0.1"}`
* **--doelvoorwaarde** -Geef een doelvoorwaarde om te bepalen welke apparaten doelgroepen voor deze implementatie. De voorwaarde is gebaseerd op het apparaat apparaatdubbel-tags of apparaatdubbel gerapporteerde eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld `tags.environment='test' and properties.reported.devicemodel='4000x'`. 
* **--prioriteit** -een positief geheel getal zijn. In het geval dat twee of meer implementaties zijn gericht op hetzelfde apparaat, wordt de implementatie met de hoogste numerieke waarde voor prioriteit wordt toegepast.

## <a name="monitor-a-deployment"></a>Controleer de implementatie van een

Gebruik de [az iot edge-implementatie weergeven](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show) opdracht om de details van één implementatie weer te geven:

```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
```

De implementatie weergeven opdracht wordt de volgende parameters:
* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

Controleer de implementatie in het opdrachtvenster. De **metrische gegevens** eigenschap bevat een aantal voor elke metrische gegevens die wordt geëvalueerd door elke hub:

* **targetedCount** -een systeem-metric die Hiermee geeft u het aantal dubbele apparaten in IoT-Hub die overeenkomen met de doelitems voorwaarde.
* **appliedCount** -een systeem metrische waarde geeft het aantal apparaten waarvoor de implementatie-inhoud toegepast op hun moduledubbels in IoT Hub.
* **reportedSuccessfulCount** -apparaat metric die Hiermee geeft u het aantal IoT Edge-apparaten in de implementatie voltooid van de IoT Edge-runtime client melden.
* **reportedFailedCount** -apparaat metric die Hiermee geeft u het aantal IoT Edge-apparaten in de implementatie reporting fout van de IoT Edge-runtime-client.

U kunt een lijst van apparaat-id's of objecten voor elk van de metrische gegevens weergeven met behulp van de [az iot edge-implementatie weergeven-metriek](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show-metric) opdracht:

```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
```

De implementatie weergeven-metriek-opdracht worden de volgende parameters: 
* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--metriek-id** : de naam van de metrische gegevens waarvoor u wilt bijvoorbeeld de lijst met apparaat-id's, Zie `reportedFailedCount`
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="modify-a-deployment"></a>Een implementatie wijzigen

Wanneer u een implementatie wijzigt, worden de wijzigingen onmiddellijk gerepliceerd naar alle apparaten uit de doelgroep. 

Als u de doelvoorwaarde bijwerkt, gebeuren de volgende updates:

* Als een apparaat niet voldoet aan de oude doelvoorwaarde, maar voldoet aan de nieuwe doelvoorwaarde en deze implementatie de hoogste prioriteit voor het apparaat is, wordt klikt u vervolgens deze implementatie toegepast op het apparaat. 
* Als een apparaat op dit moment met deze implementatie niet meer voldoet aan de doelvoorwaarde, verwijdert deze implementatie en neemt de volgende implementatie met hoogste prioriteit. 
* Als een apparaat op dit moment met deze implementatie niet meer voldoet aan de doelvoorwaarde en niet voldoet aan de doelvoorwaarde van alle andere implementaties, treedt er geen wijziging op op het apparaat. Het apparaat wordt uitgevoerd de huidige modules in hun huidige status, maar niet als onderdeel van deze implementatie niet meer wordt beheerd. Als deze voldoet aan de doelvoorwaarde van elke andere implementatie, deze implementatie wordt verwijderd en wordt op de nieuwe computer. 

Gebruik de [az iot edge-implementatie-update](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-update) opdracht voor het bijwerken van een implementatie:

```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
```

De implementatie van update-opdracht worden de volgende parameters:
* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--ingesteld** -Update een eigenschap in de implementatie. U kunt de volgende eigenschappen bijwerken:
  * targetCondition - voorbeeld `targetCondition=tags.location.state='Oregon'`
  * labels 
  * priority


## <a name="delete-a-deployment"></a>Een implementatie verwijderen

Wanneer u een implementatie verwijdert, worden alle apparaten op de volgende implementatie met hoogste prioriteit. Als uw apparaten niet voldoen aan de doelvoorwaarde van elke andere implementatie, klikt u vervolgens de modules niet verwijderd wanneer de implementatie wordt verwijderd. 

Gebruik de [az iot edge-implementatie verwijderen](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-delete) opdracht voor het verwijderen van een implementatie:

```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
```

De implementatieopdracht worden de volgende parameters: 
* **--implementatie-id** -de naam van de implementatie die deel uitmaakt van de IoT-hub.
* **--hubnaam** -naam van de IoT-hub waarin de implementatie bestaat. De hub moet zich in het huidige abonnement. Schakel over naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [modules op IoT Edge-apparaten implementeren](module-deployment-monitoring.md).

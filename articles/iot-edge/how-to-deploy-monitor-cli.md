---
title: Implementeren en bewaken van modules voor Azure IoT rand (CLI) | Microsoft Docs
description: De modules die worden uitgevoerd op de edge-apparaten beheren
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 705f7bfa62154bff62b2357bd8f33c01e97404d1
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035953"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Implementeren en controleren van de rand van de IoT-modules op grote schaal met de Azure CLI

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IoT-rand kunt u analytics verplaatsen naar de rand en biedt een interface voor de cloud zodat u kunt beheren en controleren van uw IoT-randapparaten zonder fysieke toegang tot elkaar. De mogelijkheid tot extern kunt beheren apparaten wordt steeds belangrijker naarmate Internet der dingen oplossingen grotere en complexere groeien. Azure IoT-rand is ontworpen ter ondersteuning van uw zakelijke doelstellingen, ongeacht hoeveel apparaten die u toevoegt.

U kunt afzonderlijke apparaten beheren en implementeren van modules aan hen één tegelijk. Echter, als u wijzigingen aanbrengen voor apparaten op grote schaal wilt, kunt u een **IoT rand automatische implementatie**, die deel uitmaakt van automatische Apparaatbeheer in IoT-Hub. Implementaties zijn dynamische processen waarmee u meerdere modules in één keer in te implementeren op meerdere apparaten, de status en de status van de modules bijhouden en wijzig indien nodig. 

In dit artikel leert u hoe u Azure CLI 2.0 en de IoT-extensie instelt. Vervolgens u informatie over het implementeren van modules voor een set met de randapparaten van IoT en de voortgang met behulp van de beschikbare CLI-opdrachten.

## <a name="cli-prerequisites"></a>CLI-vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [De randapparaten van IoT](how-to-register-device-cli.md) met de IoT-rand runtime geïnstalleerd.
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configureren van een manifest voor implementatie

Een implementatiemanifest is een JSON-document dat wordt beschreven welke modules te implementeren en hoe de gegevens tussen de modules en de gewenste eigenschappen van de module horende loopt. Zie voor meer informatie over hoe implementatie manifesten werken en hoe ze worden gemaakt, [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

Sla het implementatiemanifest lokaal als een txt-bestand voor het implementeren van modules met behulp van Azure CLI 2.0. U gebruikt het bestandspad in de volgende sectie tijdens het uitvoeren van de opdracht uit om de configuratie van toepassing op uw apparaat. 

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
               },
               "systemModules": {
                 "edgeAgent": {
                   "type": "docker",
                   "settings": {
                     "image": "microsoft/azureiotedge-agent:1.0-preview",
                     "createOptions": "{}"
                   }
                 },
                 "edgeHub": {
                   "type": "docker",
                   "status": "running",
                   "restartPolicy": "always",
                   "settings": {
                     "image": "microsoft/azureiotedge-hub:1.0-preview",
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


## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met tags

Voordat u een implementatie maken kunt, moet u kunnen opgeven welke apparaten die u wilt beïnvloeden. Azure IoT-rand identificeert apparaten met behulp van **labels** in de apparaat-twin. Elk apparaat kan meerdere labels hebben, en kunt u ze een manier die zinvol is voor uw oplossing. Als u een bereik van de smartcard gebouwen beheert, kunt u de volgende codes toevoegen op een apparaat:

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

Zie voor meer informatie over apparaat horende en labels [Understand and gebruik apparaat horende in IoT-Hub][lnk-device-twin].

## <a name="create-a-deployment"></a>Een implementatie maken

U implementeert modules voor uw apparaten door het maken van een implementatie die uit het implementatiemanifest, evenals andere parameters bestaat. 

Gebruik de volgende opdracht om een implementatie te maken:

   ```cli
   az iot edge deployment create --deployment-id [deployment id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int]
   ```

* **--implementatie-id** -de naam van de implementatie die wordt gemaakt in de IoT-hub. Geef uw implementatie een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.
* **--labels** -labels voor het bijhouden van uw implementaties toevoegen. Labels zijn naam, waarde-paren die uw implementatie te beschrijven. Bijvoorbeeld, `HostPlatform, Linux` of `Version, 3.0.1`
* **--inhoud** -bestandspad naar de implementatie manifest JSON. 
* **--naam hub** -naam van de IoT-hub waarin u de implementatie wordt gemaakt. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--doelvoorwaarden** -Geef een doel-voorwaarde om te bepalen welke apparaten moeten worden toegepast met deze implementatie. De voorwaarde is gebaseerd op het apparaat twin tags of apparaat twin gewenst eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld: `tags.environment='test'` of `properties.desired.devicemodel='4000x'`. 
* **--prioriteit** -een positief geheel getal. In het geval dat twee of meer implementaties zijn gericht op hetzelfde apparaat, gelden de implementatie met de hoogste numerieke waarde voor de prioriteit.

## <a name="monitor-a-deployment"></a>Controleer de implementatie van een.

Gebruik de volgende opdracht om de inhoud van een implementatie weer te geven:

   ```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
   ```
* **--implementatie-id** -de naam van de implementatie die in de IoT-hub voorkomt.
* **--naam hub** -naam van de IoT-hub die de implementatie bevat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

Controleer de implementatie in het opdrachtvenster. De **metrische gegevens** eigenschappenlijsten een aantal voor elke metriek die wordt geëvalueerd door elke hub:
* **targetedCount** -systeem metric die het aantal apparaten horende in IoT-Hub die overeenkomen met de doel-voorwaarde aangeeft.
* **appliedCount** -systeem metric geeft het aantal apparaten waarvoor de implementatie-inhoud toegepast op hun horende module in IoT-Hub.
* **reportedSuccessfulCount** -apparaat metric waarmee het aantal apparaten van de rand in de bevestiging van de rand van de IoT-client-runtime-implementatie.
* **reportedFailedCount** -apparaat metric waarmee het aantal apparaten van de rand in de implementatie reporting fout van de rand van de IoT-client-runtime.

U kunt een lijst met apparaat-id's of objecten voor elk van de metrische gegevens weergeven met behulp van de volgende opdracht:

   ```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
   ```

* **--implementatie-id** -de naam van de implementatie die in de IoT-hub voorkomt.
* **--metriek-id** : de naam van de metrische gegevens waarvoor u een overzicht van de apparaat-id's, bijvoorbeeld wilt maken `reportedFailedCount`
* **--naam hub** -naam van de IoT-hub die de implementatie bevat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="modify-a-deployment"></a>Een implementatie wijzigen

Wanneer u een implementatie wijzigt, worden de wijzigingen direct repliceren naar alle apparaten. 

Als u de doelvoorwaarden bijwerkt, gebeuren de volgende updates:
* Als een apparaat niet aan de oude doel voorwaarde voldoet, maar voldoet aan de nieuwe doel-voorwaarde en deze implementatie de hoogste prioriteit voor dat apparaat is, wordt deze implementatie toegepast op het apparaat. 
* Als een apparaat momenteel met deze implementatie niet meer voldoet aan de doelvoorwaarden, verwijdert deze implementatie en op de volgende hoogste prioriteit implementatie duurt. 
* Als een apparaat momenteel met deze implementatie niet meer voldoet aan de voorwaarde doel en voldoet niet aan de doelvoorwaarden van alle andere implementaties, vervolgens wordt er niets gewijzigd op het apparaat. Het apparaat wordt verder uitgevoerd, de huidige modules in de huidige status, maar niet als onderdeel van deze implementatie niet meer wordt beheerd. Zodra het voldoet aan de voorwaarde van het doel van andere implementaties, verwijdert deze implementatie en wordt op de nieuwe. 

Gebruik de volgende opdracht in een implementatie bijwerken:

   ```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **--implementatie-id** -de naam van de implementatie die in de IoT-hub voorkomt.
* **--naam hub** -naam van de IoT-hub die de implementatie bevat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`
* **--ingesteld** -Update een eigenschap in de implementatie. U kunt de volgende eigenschappen bijwerken:
    * targetCondition - voorbeeld `targetCondition=tags.location.state='Oregon'`
    * labels 
    * prioriteit


## <a name="delete-a-deployment"></a>Verwijderen van een implementatie

Wanneer u een implementatie hebt verwijderd, krijgen alle apparaten hun volgende hoogste prioriteit-implementatie. Als uw apparaten niet voldoen aan de voorwaarde van het doel van andere implementaties, worden klikt u vervolgens de modules niet verwijderd wanneer de implementatie is verwijderd. 

Gebruik de volgende opdracht om te verwijderen van een implementatie:

   ```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
   ```
* **--implementatie-id** -de naam van de implementatie die in de IoT-hub voorkomt.
* **--naam hub** -naam van de IoT-hub die de implementatie bevat. De hub moet in het huidige abonnement. Overschakelen naar het gewenste abonnement met de opdracht `az account set -s [subscription name]`

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [modules implementeren op apparaten van de rand][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment

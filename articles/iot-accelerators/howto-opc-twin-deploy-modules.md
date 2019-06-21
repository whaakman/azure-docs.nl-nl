---
title: Over het implementeren van OPC-Twin-module voor Azure maken | Microsoft Docs
description: Over het implementeren van OPC dubbele helemaal.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 41d544fd23d258393cc83ea09371332655223581
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203932"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>OPC-Twin-module en afhankelijkheden helemaal implementeren

De OPC-Twin-module wordt uitgevoerd op IoT Edge en biedt verschillende edge-services naar de apparaatdubbel OPC en Registerservices. 

Er zijn verschillende opties voor het implementeren van modules naar uw [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) daartussen-Gateway

- [Implementeren vanuit Azure portal IoT Edge-blade](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [Implementatie met behulp van de AZ-CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Zie voor meer informatie over de details van de implementatie en instructies, de GitHub [opslagplaats](https://github.com/Azure/azure-iiot-components).

## <a name="deployment-manifest"></a>Distributiemanifest

Alle modules worden geïmplementeerd met behulp van een manifest van de implementatie.  Een voorbeeld-manifest te implementeren op beide [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) en [OPC dubbele](https://github.com/Azure/azure-iiot-opc-twin-module) wordt hieronder weergegeven.

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
              "registryCredentials": {}
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
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "opctwin": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-twin:latest",
                "createOptions": "{\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"CapAdd\":[\"NET_ADMIN\"]}}"
              }
            },
            "opcpublisher": {
              "version": "2.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-publisher:latest",
                "createOptions": "{\"Hostname\":\"publisher\",\"Cmd\":[\"publisher\",\"--pf=./pn.json\",\"--di=60\",\"--to\",\"--aa\",\"--si=0\",\"--ms=0\"],\"ExposedPorts\":{\"62222/tcp\":{}},\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"PortBindings\":{\"62222/tcp\":[{\"HostPort\":\"62222\"}]}}}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
            "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      }
    }
  }
}
```

## <a name="deploying-from-azure-portal"></a>Implementeren vanuit Azure portal

De eenvoudigste manier om de modules implementeren naar een Azure IoT Edge-gateway-apparaat is via de Azure-portal.  

### <a name="prerequisites"></a>Vereisten

1. Implementeren van het OPC-dubbele [afhankelijkheden](howto-opc-twin-deploy-dependencies.md) en de resulterende `.env` bestand. Houd er rekening mee de geïmplementeerde `hub name` van de `PCS_IOTHUBREACT_HUB_NAME` variabele in de resulterende `.env` bestand.

2. Registreren en start een [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) of [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows) IoT Edge-gateway en noteer de `device id`.

### <a name="deploy-to-an-edge-device"></a>Implementeren naar een edge-apparaat

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw IoT-hub.

2. Selecteer **IoT Edge** in het menu links.

3. Klik op de ID van het doelapparaat uit de lijst met apparaten.

4. Selecteer **Modules instellen**.

5. In de **implementatie modules** sectie van de pagina, selecteer **toevoegen** en **IoT Edge-Module.**

6. In de **IoT Edge-Module voor aangepaste** dialoogvenster gebruik `opctwin` als naam voor de module, geeft u vervolgens de container *URI installatiekopie* als

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   Als *opties maken* gebruikt u de volgende JSON:

   ```json
   {"HostConfig":{"NetworkMode":"host","CapAdd":["NET_ADMIN"]}}
   ```

   Vul de optionele velden indien nodig. Voor meer informatie over container maken van opties voor beleid voor opnieuw opstarten en gewenste status zien [EdgeAgent gewenste eigenschappen](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties). Zie voor meer informatie over de moduledubbel [definiëren of update gewenste eigenschappen](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

7. Selecteer **opslaan** en herhaalt u stap **5**.  

8. Gebruik in het dialoogvenster IoT Edge-Module voor aangepaste `opcpublisher` als naam voor de module en de container *URI installatiekopie* als 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   Als *opties maken* gebruikt u de volgende JSON:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. Selecteer **opslaan** en vervolgens **volgende** om door te gaan naar de sectie routes.

10. Plak de volgende code in het tabblad routes 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/outputs/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/outputs/* INTO $upstream"
      }
    }
    ```

    en selecteer **volgende**

11. Lees de informatie van uw implementatie en het manifest.  Het moet eruitzien als de bovenstaande manifest van de implementatie.  Selecteer **Indienen**.

12. Nadat u hebt modules geïmplementeerd op uw apparaat, vindt u alle mappen in de **Apparaatdetails** pagina van de portal. Deze pagina weergegeven de naam van elke geïmplementeerde module, evenals de nuttige informatie als de implementatie de status- en uitgiftemodules code.

## <a name="deploying-using-azure-cli"></a>Implementeren met behulp van Azure CLI

### <a name="prerequisites"></a>Vereisten

1. Installeer de nieuwste versie van de [Azure opdrachtregelinterface (AZ)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) van [hier](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="quickstart"></a>Snelstartgids

1. Opslaan van de bovenstaande manifest van de implementatie in een `deployment.json` bestand.  

2. Gebruik de volgende opdracht toe te passen van de configuratie op een IoT Edge-apparaat:

   ```bash
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   De `device id` parameter is hoofdlettergevoelig. De inhoud parameter verwijst naar de implementatie van het manifest-bestand dat u hebt opgeslagen. 
    ![az IoT Edge set-modules output](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Nadat u hebt modules geïmplementeerd op uw apparaat, vindt u alle mappen met de volgende opdracht:

   ```bash
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   De apparaat-ID-parameter is hoofdlettergevoelig. ![AZ iot hub-module-identity-lijstuitvoer](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u over het implementeren van OPC dubbele helemaal hebt geleerd, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [OPC-Twin implementeren op een bestaand project](howto-opc-twin-deploy-existing.md)
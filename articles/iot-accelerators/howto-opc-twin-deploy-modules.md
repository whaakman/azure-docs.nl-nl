---
title: Een volledig nieuwe OPC-dubbele module voor Azure implementeren | Microsoft Docs
description: Hoe u een OPC-twee-na-kras implementeert.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: df1dd45d58baf82710b5e362afaf055aad140b98
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302648"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>OPC dubbele module en afhankelijkheden helemaal zelf implementeren

De OPC-dubbele module wordt uitgevoerd op IoT Edge en biedt verschillende Edge-services aan de OPC Device-dubbele en register Services. 

Er zijn verschillende opties voor het implementeren van modules op uw [Azure IOT Edge](https://azure.microsoft.com/services/iot-edge/) gateway.

- [Implementeren vanaf de IoT Edge Blade van Azure Portal](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [Implementeren met AZ CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Zie de GitHub- [opslag plaats](https://github.com/Azure/azure-iiot-components)voor meer informatie over de implementatie details en instructies.

## <a name="deployment-manifest"></a>Distributiemanifest

Alle modules worden geïmplementeerd met een implementatie manifest.  Hieronder ziet u een voor beeld van een manifest voor het implementeren van de [OPC-Uitgever](https://github.com/Azure/iot-edge-opc-publisher) en de [OPC dubbele](https://github.com/Azure/azure-iiot-opc-twin-module) .

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
                "createOptions": "{\"NetworkingConfig\": {\"EndpointsConfig\": {\"host\": {}}}, \"HostConfig\": {\"NetworkMode\": \"host\" }}"
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

## <a name="deploying-from-azure-portal"></a>Implementeren vanaf Azure Portal

De eenvoudigste manier om de modules op een Azure IoT Edge gateway-apparaat te implementeren, is via de Azure Portal.  

### <a name="prerequisites"></a>Vereisten

1. Implementeer de OPC- [](howto-opc-twin-deploy-dependencies.md) dubbele afhankelijkheden en haal `.env` het resulterende bestand op. Houd er rekening `hub name` mee `PCS_IOTHUBREACT_HUB_NAME` dat de variabele in het `.env` resulterende bestand is geïmplementeerd.

2. Registreer en start een [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) -of [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows) IOT Edge gateway en noteer `device id`deze.

### <a name="deploy-to-an-edge-device"></a>Implementeren op een edge-apparaat

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw IoT-hub.

2. Selecteer **IOT Edge** in het menu aan de linkerkant.

3. Klik op de ID van het doelapparaat uit de lijst met apparaten.

4. Selecteer **Modules instellen**.

5. Selecteer in de sectie **implementatie modules** van de pagina de module **toevoegen** en **IOT Edge.**

6. In het dialoog venster **aangepaste module IOT Edge** gebruikt u `opctwin` de naam voor de module en geeft u vervolgens de URI van de container *installatie kopie* op als

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   Gebruik de volgende JSON als *container opties*voor het maken van containers:

   ```json
   {"NetworkingConfig": {"EndpointsConfig": {"host": {}}}, "HostConfig": {"NetworkMode": "host" }}
   ```

   Vul de optionele velden indien nodig. Voor meer informatie over container maken van opties voor beleid voor opnieuw opstarten en gewenste status zien [EdgeAgent gewenste eigenschappen](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties). Zie voor meer informatie over de moduledubbel [definiëren of update gewenste eigenschappen](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

7. Selecteer **Opslaan** en herhaal stap **5**.  

8. Gebruik `opcpublisher` in het dialoog venster aangepaste module IOT Edge als naam voor de module en de URI van de container *installatie kopie* als 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   Gebruik de volgende JSON als *container opties*voor het maken van containers:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. Selecteer **Opslaan** en klik vervolgens op **volgende** om door te gaan naar de sectie routes.

10. Plak het volgende op het tabblad routes: 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
      }
    }
    ```

    en selecteer **volgende**

11. Controleer uw implementatie-informatie en-manifest.  Deze moet eruitzien als in het bovenstaande implementatie manifest.  Selecteer **indienen**.

12. Nadat u hebt modules geïmplementeerd op uw apparaat, vindt u alle mappen in de **Apparaatdetails** pagina van de portal. Deze pagina weergegeven de naam van elke geïmplementeerde module, evenals de nuttige informatie als de implementatie de status- en uitgiftemodules code.

## <a name="deploying-using-azure-cli"></a>Implementeren met behulp van Azure CLI

### <a name="prerequisites"></a>Vereisten

1. Installeer [hier](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)de nieuwste versie van de [Azure-opdracht regel interface (AZ)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) .

### <a name="quickstart"></a>Snelstartgids

1. Sla het bovenstaande implementatie manifest op in `deployment.json` een bestand.  

2. Gebruik de volgende opdracht toe te passen van de configuratie op een IoT Edge-apparaat:

   ```bash
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   De `device id` para meter is hoofdletter gevoelig. De inhoud parameter verwijst naar de implementatie van het manifest-bestand dat u hebt opgeslagen. 
    ![az IoT Edge set-modules output](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Nadat u hebt modules geïmplementeerd op uw apparaat, vindt u alle mappen met de volgende opdracht:

   ```bash
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   De para meter voor de apparaat-ID is hoofdletter gevoelig. ![AZ iot hub-module-identity-lijstuitvoer](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u OPC twee ledig kunt implementeren, is dit de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [OPC van twee tot een bestaand project implementeren](howto-opc-twin-deploy-existing.md)

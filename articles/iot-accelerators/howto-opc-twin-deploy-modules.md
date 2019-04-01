---
title: Over het implementeren van Azure IoT OPC UA device management-module helemaal | Microsoft Docs
description: Over het implementeren van OPC dubbele helemaal.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: fe1b141ecacbd1d96c217322e69709828a3bf36c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759409"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>OPC-Twin-Module en afhankelijkheden helemaal implementeren

De OPC-Apparaatdubbel-module wordt uitgevoerd op IoT Edge en verschillende edge services levert aan de OPC-Apparaatdubbel en de Register-services. 

Er zijn verschillende opties voor het implementeren van modules naar uw [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) daartussen-Gateway

- [Implementeren vanuit Azure portal IoT Edge-blade](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [Implementatie met behulp van de AZ-CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Zie voor meer informatie over de details van de implementatie en instructies, de GitHub [opslagplaats](https://github.com/Azure/azure-iiot-components).

## <a name="deployment-manifest"></a>Distributiemanifest

Alle modules worden geïmplementeerd met behulp van een manifest van de implementatie.  Een voorbeeld-manifest te implementeren op beide [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) en [OPC dubbele](https://github.com/Azure/azure-iiot-opc-twin-module) wordt hieronder weergegeven.

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
            "restartPolicy": "never",
            "settings": {
              "image": "mcr.microsoft.com/iotedge/opc-twin:latest",
              "createOptions": "{\"HostConfig\": { \"NetworkMode\": \"host\", \"CapAdd\": [\"NET_ADMIN\"] } }"
            }
          },
          "opcpublisher": {
            "version": "2.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "never",
            "settings": {
              "image": "mcr.microsoft.com/iotedge/opc-publisher:latest",
              "createOptions": "{\"Hostname\": \"publisher\", \"Cmd\": [ \"publisher\", \"--pf=./pn.json\", \"--di=60\", \"--to\", \"--aa\", \"--si=0\", \"--ms=0\" ], \"ExposedPorts\": { \"62222/tcp\": {} }, \"HostConfig\": { \"PortBindings\": { \"62222/tcp\": [{ \"HostPort\": \"62222\" }] } } }"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {
          "opctwinToIoTHub": "FROM /messages/modules/opctwin/outputs/* INTO $upstream",
          "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/outputs/* INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 7200
        }
      }
    }
  }
}
```

## <a name="deploying-from-azure-portal"></a>Implementeren vanuit Azure portal

De eenvoudigste manier om de modules implementeren naar een Azure IoT Edge-gateway-apparaat is via de Azure-portal.  

### <a name="prerequisites"></a>Vereisten

1. De OPC UA-Apparaatbeheer implementeren [afhankelijkheden](howto-opc-twin-deploy-dependencies.md) en de resulterende `.env` bestand. Houd er rekening mee de geïmplementeerde `hub name` van de `PCS_IOTHUBREACT_HUB_NAME` variabele in de resulterende `.env` bestand.

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

## <a name="run-and-debug-locally"></a>Uitvoeren en lokaal fouten opsporen

Voor problemen bij probleemoplossing en foutopsporing uit voeren is handig om uit te voeren van het Edge-modules lokaal via de [IoT Edge ontwikkeling Simulator](https://github.com/Azure/iotedgehubdev).  Het biedt een lokale ontwikkelervaring met een simulator voor het maken, ontwikkelen, testen, uitvoeren en foutopsporing van Azure IoT Edge-modules en -oplossingen met behulp van dezelfde bits /-code die worden gebruikt in productie.

### <a name="prerequisites"></a>Vereisten

1. De OPC UA-Apparaatbeheer implementeren [afhankelijkheden](howto-opc-twin-deploy-dependencies.md).

2. Installeer [Docker CE (18.02.0+)](https://www.docker.com/community-edition) op [Windows](https://docs.docker.com/docker-for-windows/install/), [macOS](https://docs.docker.com/docker-for-mac/install/) of [Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce).

3. Installeer [Docker Compose (1.20.0+)](https://docs.docker.com/compose/install/#install-compose) (alleen vereist voor **Linux**. Stel al is opgenomen in Windows/macOS Docker CE-installatie)

4. Installeer [Python (2.7 / 3.5+) en Pip](https://www.python.org/)

5. Installeer iotedgehubdev onderstaande opdracht in de terminal service wordt uitgevoerd

   ```bash
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Installeren `iotedgehubdev` naar **hoofdmap** in Linux/macOS (*gebruik niet '--gebruiker ' optie in de opdracht 'pip install'*).
> Zorg ervoor dat er geen Azure IoT Edge-runtime die op dezelfde computer met iotedgehubdev, aangezien ze nodig dezelfde poorten hebben.

### <a name="quickstart"></a>Snelstartgids

1. Volg de instructies voor [een Edge-apparaat maken in Azure portal](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).  Kopieer de verbindingsreeks van de edge-apparaat.

2. Instellen van de simulator met behulp van de edge-verbindingsreeks.

    ```bash
    iotedgehubdev setup -c <edge-device-connection-string>
    ```

3. Exemplaar boven in het manifest een `deployment.json` bestand in dezelfde map.  De implementatie te starten in de simulator gebruiken

    ```bash
    iotedgehubdev start -d deployment.json
    ```

4. Stop de simulator gebruiken

   ```bash
   iotedgehubdev stop
   ```

## <a name="next-steps"></a>Volgende stappen

Nu dat u over het implementeren van OPC dubbele helemaal hebt geleerd, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [OPC-Twin implementeren op een bestaand project](howto-opc-twin-deploy-existing.md)
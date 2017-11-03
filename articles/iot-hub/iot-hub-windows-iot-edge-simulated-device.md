---
title: Simuleren van een apparaat met Azure IoT kant (Windows) | Microsoft Docs
description: Het gebruik van Azure IoT Edge in Windows voor het maken van een gesimuleerd apparaat dat telemetrie via een Edge van Azure IoT gateway naar een iothub verzendt.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.openlocfilehash: 0aa1836ee1445894022b95fefc2338ef53698240
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Gebruik Azure IoT rand apparaat-naar-cloud-berichten verzenden met een gesimuleerd apparaat (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

De **build.cmd** script genereert de uitvoer ervan weergegeven in de **bouwen** map in de lokale kopie van de **iot-edge** opslagplaats. Deze uitvoer bevat de vier IoT Edge-modules die in dit voorbeeld worden gebruikt.

Het script build maakt de volgende bestanden:

* **Logger.dll** in de **bouwen\\modules\\berichtenlogboek\\Debug** map.
* **iothub.dll** in de **bouwen\\modules\\iothub\\Debug** map.
* **identiteit\_map.dll** in de **bouwen\\modules\\identitymap\\Debug** map.
* **gesimuleerde\_device.dll** in de **bouwen\\modules\\gesimuleerde\_apparaat\\Debug** map.

Deze paden voor de **pad module** waarden zoals weergegeven in de gesimuleerde\_apparaat\_cloud\_uploaden\_win JSON-instellingenbestand.

De gesimuleerde\_apparaat\_cloud\_uploaden voorbeeld wordt het pad naar een JSON-configuratiebestand als een opdrachtregelargument. Het volgende voorbeeld JSON-bestand is opgegeven in de SDK-opslagplaats op **voorbeelden\\gesimuleerde\_apparaat\_cloud\_uploaden\_voorbeeld\\src\\ gesimuleerde\_apparaat\_cloud\_uploaden\_win.json**. Dit configuratiebestand werkt zoals tenzij u het build script wijzigen voor de rand van de IoT-modules of voorbeeld uitvoerbare bestanden in niet-standaardlocaties plaatsen.

> [!NOTE]
> De module-paden zijn ten opzichte van de map waar de gesimuleerde\_apparaat\_cloud\_uploaden\_sample.exe zich bevindt. Het voorbeeld JSON-configuratiebestand wordt standaard ingesteld op het schrijven naar 'deviceCloudUploadGatewaylog.log' in de huidige werkmap.

Open het bestand in een teksteditor **voorbeelden\\gesimuleerde\_apparaat\_cloud\_uploaden\\src\\gesimuleerde\_apparaat\_cloud\_uploaden\_win.json** in het lokale exemplaar van de **iot-edge** opslagplaats. Dit bestand configureren van de rand van de IoT-modules in de voorbeeld-gateway:

* De **IoTHub** module maakt verbinding met uw IoT-hub. U configureert om gegevens te verzenden naar uw IoT-hub. Stel de **IoTHubName** van waarde naar de naam van uw IoT-hub en stel de **IoTHubSuffix** van waarde naar **azure devices.net**. Stel de **Transport** waarde in op een van: **HTTP**, **AMQP**, of **MQTT**. Op dit moment alleen **HTTP** deelt een TCP-verbinding voor alle apparaatberichten. Als u de waarde instelt op **AMQP**, of **MQTT**, de gateway onderhoudt een afzonderlijke TCP-verbinding met IoT Hub voor elk apparaat.
* De **toewijzing** module toegewezen MAC-adressen van uw gesimuleerde apparaten aan de apparaat-id's van uw IoT-Hub. Stel de **deviceId** waarden voor de id's van de twee apparaten die u hebt toegevoegd aan uw IoT-hub. Stel de **deviceKey** waarden aan de sleutels van de twee apparaten.
* De **BLE1** en **BLE2** modules zijn de gesimuleerde apparaten. Houd er rekening mee hoe de module MAC-adressen overeenkomen met de adressen in de **toewijzing** module.
* De **berichtenlogboek** module registreert de activiteit van uw gateway in een bestand.
* De **pad module** waarden die worden weergegeven in het volgende voorbeeld zijn ten opzichte van de map waar de gesimuleerde\_apparaat\_cloud\_uploaden\_sample.exe zich bevindt.
* De **koppelingen** matrix aan de onderkant van het JSON-bestand verbindt de **BLE1** en **BLE2** -modules mogen worden de **toewijzing** -module, en de **toewijzing** module die u wilt de **IoTHub** module. Dit zorgt er ook voor dat alle berichten worden vastgelegd door de **berichtenlogboek** module.

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Sla de wijzigingen die u hebt aangebracht in het configuratiebestand.

Uitvoeren van de steekproef:

1. Bij een opdrachtprompt, gaat u naar de **bouwen** map in de lokale kopie van de **iot-edge** opslagplaats.
2. Voer de volgende opdracht uit:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. U kunt de [apparaat explorer] [ lnk-device-explorer] of [iothub explorer] [ lnk-iothub-explorer] hulpprogramma voor het bewaken van de berichten die IoT-hub van de gateway ontvangt. Bijvoorbeeld, kunt met iothub explorer u controleren apparaat-naar-cloud-berichten met de volgende opdracht:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Volgende stappen

Om toegang te krijgen van een uitgebreidere kennis van IoT-rand en Experimenteer met enkele codevoorbeelden, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

* [Apparaat-naar-cloud-berichten verzenden vanaf een fysiek apparaat met IoT-rand][lnk-physical-device]
* [Azure IoT-rand][lnk-iot-edge]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [Beveiligen van uw IoT-oplossing bouwen up][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

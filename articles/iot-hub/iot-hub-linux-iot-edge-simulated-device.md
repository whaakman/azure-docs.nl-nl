---
title: Simuleren van een apparaat met Azure IoT kant (Linux) | Microsoft Docs
description: Het gebruik van Azure IoT Edge op Linux te maken van een gesimuleerd apparaat dat telemetrie via een IoT Edge gateway naar een iothub verzendt.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a>Gebruik Azure IoT rand apparaat-naar-cloud-berichten verzenden met een gesimuleerd apparaat (Linux)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Het voorbeeld uitvoeren

De uitvoer van het script **build.sh** wordt gegenereerd in de map **build** in de lokale kopie van de **iot-edge**-opslagplaats. Deze uitvoer bevat de vier IoT Edge-modules die in dit voorbeeld worden gebruikt.

De build script plaatsen de:

* **liblogger.so** in de **modules-build-logboekregistratie** map.
* **libiothub.so** in de **modules-build/iothub** map.
* **lib\_identiteit\_map.so** in de **modules-build/identitymap** map.
* **libsimulated\_device.so** in de **build, modules/gesimuleerde\_apparaat** map.

Deze paden voor de **pad module** waarden zoals weergegeven in het volgende JSON-bestand met instellingen:

De gesimuleerde\_apparaat\_cloud\_uploaden\_voorbeeld wordt het pad naar een JSON-configuratiebestand als een opdrachtregelargument. Het volgende voorbeeld JSON-bestand is opgegeven in de SDK-opslagplaats op **voorbeelden\\gesimuleerde\_apparaat\_cloud\_uploaden\_voorbeeld\\src\\gesimuleerde\_apparaat\_cloud\_uploaden\_voorbeeld\_lin.json**. Dit configuratiebestand werkt zoals tenzij u het build script wijzigen voor de rand van de IoT-modules of voorbeeld uitvoerbare bestanden in niet-standaardlocaties plaatsen.

> [!NOTE]
> De module-paden zijn ten opzichte van de map van waaruit u de gesimuleerde uitvoert\_apparaat\_cloud\_uploaden\_voorbeeld uitvoerbaar bestand, niet de map waar het uitvoerbare bestand zich bevindt. Het voorbeeld JSON-configuratiebestand wordt standaard ingesteld op het schrijven naar 'deviceCloudUploadGatewaylog.log' in de huidige werkmap.

Open het bestand in een teksteditor **samples/gesimuleerde\_apparaat\_cloud\_uploaden\_voorbeeld, src/gesimuleerde\_apparaat\_cloud\_uploaden\_lin.json** in het lokale exemplaar van de **iot-edge** opslagplaats. Dit bestand configureren van de rand van de IoT-modules in de voorbeeld-gateway:

* De **IoTHub** module maakt verbinding met uw IoT-hub. U configureert om gegevens te verzenden naar uw IoT-hub. Stel de **IoTHubName** van waarde naar de naam van uw IoT-hub en stel de **IoTHubSuffix** van waarde naar **azure devices.net**. Stel de **Transport** waarde in op een van: **HTTP**, **AMQP**, of **MQTT**. Op dit moment alleen **HTTP** deelt een TCP-verbinding voor alle apparaatberichten. Als u de waarde instelt op **AMQP**, of **MQTT**, de gateway onderhoudt een afzonderlijke TCP-verbinding met IoT Hub voor elk apparaat.
* De **toewijzing** module toegewezen MAC-adressen van uw gesimuleerde apparaten aan uw IoT Hub apparaat-id. Zorg ervoor dat **deviceId** waarden overeenkomen met de id's van de twee apparaten die u hebt toegevoegd aan uw IoT-hub en dat de **deviceKey** waarden bevatten de sleutels van de twee apparaten.
* De **BLE1** en **BLE2** modules zijn de gesimuleerde apparaten. Houd er rekening mee hoe hun MAC-adressen overeenkomen met de adressen in de **toewijzing** module.
* De **berichtenlogboek** module registreert de activiteit van uw gateway in een bestand.
* De **pad module** waarden weergegeven in het voorbeeld wordt ervan uitgegaan dat u het voorbeeld uitvoert de **bouwen** map in de lokale kopie van de **iot-edge** opslagplaats.
* De **koppelingen** matrix aan de onderkant van het JSON-bestand verbindt de **BLE1** en **BLE2** -modules mogen worden de **toewijzing** -module, en de **toewijzing** module die u wilt de **IoTHub** module. Dit zorgt er ook voor dat alle berichten worden vastgelegd door de **berichtenlogboek** module.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "01:01:01:01:01:01"
            }
          },
        {
            "name": "BLE2",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "02:02:02:02:02:02"
            }
          },
        {
            "name": "Logger",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Sla de wijzigingen die u hebt aangebracht in het configuratiebestand.

Uitvoeren van de steekproef:

1. Uw shell, gaat u naar de **iot-rand/build** map.
2. Voer de volgende opdracht uit:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. U kunt de [apparaat explorer] [ lnk-device-explorer] of [iothub explorer] [ lnk-iothub-explorer] hulpprogramma voor het bewaken van de berichten die IoT-hub van de gateway ontvangt. Bijvoorbeeld, kunt met iothub explorer u controleren apparaat-naar-cloud-berichten met de volgende opdracht:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Volgende stappen

Om toegang te krijgen van een uitgebreidere kennis van Azure IoT rand en Experimenteer met enkele codevoorbeelden, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

* [Apparaat-naar-cloud-berichten verzenden vanaf een fysiek apparaat met Azure IoT rand][lnk-physical-device]
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

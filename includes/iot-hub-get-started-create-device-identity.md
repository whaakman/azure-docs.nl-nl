## <a name="create-a-device-identity"></a>Een apparaat-id maken

In deze sectie maakt u een Node.js-hulpprogramma met de naam [iothub-explorer] [ iot-hub-explorer] te maken van een apparaat-id voor deze zelfstudie. Apparaat-id's zijn hoofdlettergevoelig.

1. Voer het volgende in uw omgeving opdrachtregel:

    `npm install -g iothub-explorer@latest`

1. Voer de volgende opdracht uit om aan te melden bij uw hub. Vervang `{iot hub connection string}` met de IoT Hub-verbindingsreeks die u eerder hebt gekopieerd:

    `iothub-explorer login "{iot hub connection string}"`

1. Maak ten slotte een nieuwe apparaat-id met de naam `myDeviceId` met de opdracht:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Noteer de apparaatverbindingsreeks van het resultaat. Deze verbindingsreeks van het apparaat wordt gebruikt door de apparaat-app verbinden met uw IoT-Hub als apparaat.

![][img-identity]

Raadpleeg [aan de slag met IoT Hub] [ lnk-getstarted] apparaat-id's via een programma te maken.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/quickstart-send-telemetry-dotnet.md

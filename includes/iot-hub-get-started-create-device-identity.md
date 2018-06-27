## <a name="create-a-device-identity"></a>Een apparaat-id maken

In deze sectie gebruikt u een Node.js-hulpprogramma aangeroepen [iothub explorer] [ iot-hub-explorer] voor het maken van een apparaat-id voor deze zelfstudie. Apparaat-id's zijn hoofdlettergevoelig.

1. Voer het volgende in uw omgeving opdrachtregel:

    `npm install -g iothub-explorer@latest`

1. Voer de volgende opdracht aan te melden bij uw hub. Vervang `{iot hub connection string}` met de IoT Hub-verbindingsreeks die u eerder hebt gekopieerd:

    `iothub-explorer login "{iot hub connection string}"`

1. Maak ten slotte een nieuw apparaat-id genoemd `myDeviceId` met de opdracht:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Noteer de apparaat-verbindingsreeks uit het resultaat. Deze verbindingsreeks van het apparaat wordt gebruikt door de app voor het apparaat verbinding maken met uw IoT-Hub als een apparaat.

![][img-identity]

Raadpleeg [aan de slag met IoT Hub] [ lnk-getstarted] programmatisch apparaat-id's te maken.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md

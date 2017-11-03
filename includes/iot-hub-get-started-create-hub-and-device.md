## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nu dat u een IoT-hub hebt gemaakt, zoek de belangrijke informatie waarmee u verbinding maken met apparaten en toepassingen naar uw IoT-hub. 

1. Nadat uw IoT-hub is gemaakt, klikt u erop in het dashboard. Noteer de **Hostname** (Hostnaam) en klik vervolgens op **Shared access policies** (Gedeeld toegangsbeleid).

   ![De hostnaam van uw IoT-hub ophalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

1. Klik in het deelvenster **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer en noteer de **Verbindingsreeks** van uw IoT-hub. Zie voor meer informatie [Toegang tot IoT Hub regelen](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
U hoeft deze iothubowner-verbindingsreeks niet te installeren voor deze zelfstudie. U hebt deze echter mogelijk na het voltooien van deze configuratie nodig voor een aantal van de zelfstudies over andere IoT-scenario's.

   ![Uw IoT-hub-verbindingsreeks ophalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Een apparaat in de IoT-hub voor uw apparaat registreren

1. Open uw IoT-hub in [Azure Portal](https://portal.azure.com/).

2. Klik op **Apparatenverkenner**.
3. Klik in het deelvenster Apparatenverkenner op **Toevoegen** om een apparaat toe te voegen aan uw IoT-hub. Ga daarna als volgt te werk:

   **Apparaat-ID**: voer de ID van het nieuwe apparaat in. Apparaat-id's zijn hoofdlettergevoelig.

   **Verificatietype**: selecteer **Symmetrische sleutel**.

   **Automatisch sleutels genereren**: schakel dit selectievakje in.

   **Het apparaat verbinden met IoT Hub**: klik op **Inschakelen**.

   ![Een apparaat in de Apparatenverkenner toevoegen aan uw IoT-hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klik op **Opslaan**.
5. Nadat het apparaat is gemaakt, opent u het apparaat in het deelvenster **Apparatenverkenner**.
6. Noteer de primaire sleutel van de verbindingsreeks.

   ![De verbindingsreeks ophalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)

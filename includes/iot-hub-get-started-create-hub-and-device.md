## <a name="create-an-iot-hub"></a>Een IoT Hub maken

1. Klik in [Azure Portal](https://portal.azure.com/) op **Nieuw** > **Internet of Things** > **IoT-hub**.

   ![Een IoT-hub maken in Azure Portal](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. Voer in het deelvenster **IoT Hub** deelvenster de volgende informatie in voor uw IoT-hub:

     **Naam**: voer de naam in van uw IoT-hub. Als de door u opgegeven naam geldig is, wordt er een groen vinkje weergegeven.

     **Prijs- en schaalniveau**: selecteer het niveau **F1 - gratis**. Deze optie is voldoende voor deze demo. Zie [Prijs- en schaalniveau](https://azure.microsoft.com/pricing/details/iot-hub/) voor meer informatie.

     **Resourcegroep**: maak een resourcegroep voor het hosten van de IoT-hub of gebruik een bestaande. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../articles/azure-resource-manager/resource-group-portal.md) voor meer informatie.

     **Locatie**: selecteer de dichtstbijzijnde locatie in uw buurt waar de IoT-hub is gemaakt.

     **Vastmaken aan dashboard**: selecteer deze optie voor eenvoudige toegang tot uw IoT-hub vanuit het dashboard.

   ![Informatie invoeren voor het maken van uw IoT-hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. Klik op **Create**. Het maken van de IoT-hub kan een paar minuten duren. U kunt de voortgang bekijken in het deelvenster **Meldingen**.

   ![Meldingen over de voortgang van uw IoT-hub bekijken](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. Nadat uw IoT-hub is gemaakt, klikt u erop in het dashboard. Noteer de **Hostname** (Hostnaam) en klik vervolgens op **Shared access policies** (Gedeeld toegangsbeleid).

   ![De hostnaam van uw IoT-hub ophalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. Klik in het deelvenster **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer en noteer de **Verbindingsreeks** van uw IoT-hub. Zie voor meer informatie [Toegang tot IoT Hub regelen](../articles/iot-hub/iot-hub-devguide-security.md).

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

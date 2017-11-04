## <a name="create-an-iot-hub"></a>Een IoT Hub maken
Een IoT Hub maken waarmee uw gesimuleerde apparaat-app verbinding kan maken. De volgende stappen laten zien hoe u deze taak kunt uitvoeren met behulp van Azure Portal.

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Nu dat u een IoT-hub hebt gemaakt, zoek de belangrijke informatie waarmee u verbinding maken met apparaten en toepassingen naar uw IoT-hub. 

1. Wanneer de IoT Hub is gemaakt, klikt u op de nieuwe tegel voor uw IoT Hub in Azure Portal. Het venster met eigenschappen van de nieuwe IoT Hub wordt dan geopend. Noteer de **Hostname** (Hostnaam) en klik vervolgens op **Shared access policies** (Gedeeld toegangsbeleid).
   
    ![Venster nieuwe IoT Hub][4]
1. Klik in **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer en noteer de IoT Hub-verbindingsreeks in het venster **iothubowner**. Zie voor meer informatie [Toegangsbeheer][lnk-access-control] in de Ontwikkelaarshandleiding voor IoT Hub.
   
    ![Gedeeld toegangsbeleid][5]

<!-- Images. -->
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md

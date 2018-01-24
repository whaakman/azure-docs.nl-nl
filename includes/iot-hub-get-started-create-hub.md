## <a name="create-an-iot-hub"></a>Een IoT Hub maken
Een IoT Hub maken waarmee uw gesimuleerde apparaat-app verbinding kan maken. De volgende stappen laten zien hoe u deze taak kunt uitvoeren met behulp van Azure Portal.

1. Meld u aan bij [Azure Portal][lnk-portal].

1. Selecteer **Nieuw** > **Internet of Things** > **IoT Hub**.
   
    ![Snelbalk Azure Portal][1]

1. Voer in het deelvenster **IoT Hub** deelvenster de volgende informatie in voor uw IoT-hub:

   * **Naam**: verzin een naam voor uw IoT-hub. Als de door u opgegeven naam geldig is, wordt er een groen vinkje weergegeven.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   * **Prijs- en schaalniveau**: selecteer in deze zelfstudie het niveau **F1 - gratis**. Zie [Prijs- en schaalniveau][lnk-pricing] voor meer informatie.

   * **Resourcegroep**: maak een resourcegroep voor het hosten van de IoT-hub of gebruik een bestaande. Zie [Resourcegroepen gebruiken om Azure-resources te beheren][lnk-resource-groups] voor meer informatie

   * **Locatie**: selecteer de locatie die het dichtst bij u in de buurt is.

   * **Vastmaken aan dashboard**: vink deze optie aan voor eenvoudige toegang tot uw IoT-hub vanuit het dashboard.

    ![IoT Hub-venster][2]

1. Klik op **Create**. Het maken van de IoT-hub kan een paar minuten duren. U kunt de voortgang bewaken via het deelvenster **Meldingen**.

1. Wanneer de IoT Hub is gemaakt, klikt u op de nieuwe tegel voor uw IoT Hub in Azure Portal. Het venster met eigenschappen van de nieuwe IoT Hub wordt dan geopend. Nu dat u een IoT-hub hebt gemaakt, zoek de belangrijke informatie waarmee u verbinding maken met apparaten en toepassingen naar uw IoT-hub. Noteer de **Hostname** (Hostnaam) en klik vervolgens op **Shared access policies** (Gedeeld toegangsbeleid).
   
    ![Venster nieuwe IoT Hub][4]

1. Klik in **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer en noteer de IoT Hub-verbindingsreeks in het venster **iothubowner**. Zie voor meer informatie [Toegangsbeheer][lnk-access-control] in de Ontwikkelaarshandleiding voor IoT Hub.
   
    ![Gedeeld toegangsbeleid][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md

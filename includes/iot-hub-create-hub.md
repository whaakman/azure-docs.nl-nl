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
<!-- Images -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
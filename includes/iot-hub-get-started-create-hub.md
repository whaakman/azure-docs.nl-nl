## <a name="create-an-iot-hub"></a>Een IoT Hub maken
Een IoT Hub maken waarmee uw gesimuleerde apparaat-app verbinding kan maken. De volgende stappen laten zien hoe u deze taak kunt uitvoeren met behulp van Azure Portal.

1. Meld u aan bij [Azure Portal][lnk-portal].
1. Klik in de snelbalk op **Nieuw** > **Internet van dingen** > **IoT Hub**.
   
    ![Snelbalk Azure Portal][1]
1. In het venster **IoT Hub** kiest u de configuratie voor uw IoT Hub.
   
    ![IoT Hub-venster][2]
   
   1. In het vak **Naam** typt u een naam voor uw IoT Hub. Als de **naam** geldig en beschikbaar is, verschijnt er een groen vinkje in het vak **Naam**.
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. Selecteer een [prijs- en schaalcategorie][lnk-pricing]. Voor deze zelfstudie is geen bepaalde laag vereist. Gebruik voor deze zelfstudie de gratis F1-laag.
   1. Maak in **Resourcegroep** een nieuwe resourcegroep of selecteer een bestaande. Zie [Resourcegroepen gebruiken om Azure-resources te beheren][lnk-resource-groups] voor meer informatie.
   1. In **Locatie** selecteert u de locatie voor het hosten van uw IoT Hub. Voor deze zelfstudie kiest uw dichtstbijzijnde locatie.
1. Wanneer u de configuratieopties voor uw IoT Hub hebt gekozen, klikt u op **Maken**.  Het kan enkele minuten duren voordat Azure uw IoT Hub heeft gemaakt. Als u de status wilt zien, kunt u de voortgang bekijken via het startboard in het meldingenvenster.
   
1. Wanneer de IoT Hub is gemaakt, klikt u op de nieuwe tegel voor uw IoT Hub in Azure Portal. Het venster met eigenschappen van de nieuwe IoT Hub wordt dan geopend. Noteer de **Hostname** (Hostnaam) en klik vervolgens op **Shared access policies** (Gedeeld toegangsbeleid).
   
    ![Venster nieuwe IoT Hub][4]
1. Klik in **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer en noteer de IoT Hub-verbindingsreeks in het venster **iothubowner**. Zie voor meer informatie [Toegangsbeheer][lnk-access-control] in de Ontwikkelaarshandleiding voor IoT Hub.
   
    ![Gedeeld toegangsbeleid][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md

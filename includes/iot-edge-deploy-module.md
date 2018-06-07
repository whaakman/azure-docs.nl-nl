Een van de belangrijkste mogelijkheden van Azure IoT Edge is dat u er modules voor uw IoT Edge-apparaten mee kunt implementeren vanuit de cloud. Een IoT Edge-module is een uitvoerbaar pakket dat is geïmplementeerd als container. In deze sectie implementeert u een module die telemetrie genereert voor uw gesimuleerde apparaat. 

1. Ga in Azure Portal naar uw IoT-hub.
1. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
1. Selecteer **Modules instellen**.
1. Selecteer **IoT Edge-module toevoegen**.
1. Voer in het veld **Naam** `tempSensor` in. 
1. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in. 
1. Laat de overige instellingen ongewijzigd en selecteer **Opslaan**.

   ![Sla de IoT Edge-module op nadat u de URI van de naam en installatiekopie het ingevoerd](./media/iot-edge-deploy-module/name-image.png)

1. Ga terug naar de stap **Modules toevoegen** en selecteer **Volgende**.
1. Selecteer **Volgende** in de stap **Routes opgeven**.
1. Selecteer de optie **Indienen** in de stap **Sjabloon controleren**.
1. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**. Nu ziet u de nieuwe tempSensor-module, die samen met de runtime van IoT Edge wordt uitgevoerd. 

   ![Geef tempSensor weer in een lijst met geïmplementeerde modules][1]

<!-- Images -->
[1]: ../articles/iot-edge/media/tutorial-simulate-device-windows/view-module.png
## <a name="configure-the-nodejs-simulated-device"></a>Het gesimuleerde apparaat Node.js configureren
1. Klik op het dashboard voor externe controle **+ een apparaat toevoegt** en voeg vervolgens een *aangepast apparaat*. Noteer de IoT-Hub hostnaam, apparaat-id en apparaatsleutel. U moet deze verderop in deze zelfstudie wanneer u de clienttoepassing remote_monitoring.js-apparaat voorbereiden.
2. Zorg dat Node.js-versie 0.12.x of hoger is geïnstalleerd op uw ontwikkelcomputer. Voer `node --version` achter de opdrachtprompt of in een shell controleren de versie. Zie voor meer informatie over het gebruik van een Pakketbeheer Node.js installeren op Linux [Node.js installeren via Pakketbeheer][node-linux].
3. Wanneer u Node.js hebt geïnstalleerd, kloon de nieuwste versie van de [azure-iot-sdk-knooppunt] [ lnk-github-repo] opslagplaats op uw ontwikkelcomputer. Gebruik altijd de **master** vertakking voor de nieuwste versie van de bibliotheken en voorbeelden.
4. In uw lokale exemplaar van de [azure-iot-sdk-knooppunt] [ lnk-github-repo] -opslagplaats, de volgende twee bestanden kopiëren van de apparaat-knooppunt-voorbeelden map naar een lege map op uw ontwikkelcomputer:
   
   * Packages.JSON
   * remote_monitoring.js
5. Open het bestand remote_monitoring.js en zoekt u naar de definitie van de volgende variabele:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Vervang **[IoT Hub apparaat-verbindingsreeks]** met de verbindingsreeks van uw apparaat. Gebruik de waarden voor uw IoT Hub-hostnaam, het apparaat-id en de apparaatsleutel die u een genoteerd in stap 1 hebt. Een apparaat-verbindingsreeks heeft de volgende indeling:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Als uw IoT Hub-hostnaam **contoso** en uw apparaat-id is **mydevice**, de verbindingsreeks eruit ziet het volgende fragment:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Sla het bestand op. Voer de volgende opdrachten in een shell of opdrachtprompt in de map met deze bestanden voor het installeren van de vereiste pakketten en voer vervolgens de voorbeeldtoepassing:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Houd rekening met dynamische telemetrie in actie
Het dashboard toont de temperatuur en vochtigheid telemetrie van de bestaande gesimuleerde apparaten:

![Het standaarddashboard][image1]

Als u het Node.js gesimuleerde apparaat die u in de vorige sectie hebt uitgevoerd selecteert, ziet u temperatuur en vochtigheid externe temperatuur telemetrie:

![Externe temperatuur toevoegen aan het dashboard][image2]

De oplossing voor externe controle automatisch het type van de telemetrie aanvullende externe temperatuur detecteert en voegt het toe aan de grafiek in het dashboard.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-v1-send-external-temperature/image1.png
[image2]: media/iot-suite-v1-send-external-temperature/image2.png
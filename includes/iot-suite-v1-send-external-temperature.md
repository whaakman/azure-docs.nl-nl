## <a name="configure-the-nodejs-simulated-device"></a>Configureren van het gesimuleerde apparaat met Node.js
1. Klik op het dashboard voor externe controle **+ een apparaat toevoegen** en voeg een *aangepast apparaat*. Noteer de IoT-Hub-hostnaam, de apparaat-id en de apparaatsleutel. Moet u ze later in deze zelfstudie als u de clienttoepassing remote_monitoring.js apparaat voorbereiden.
2. Zorg ervoor dat Node.js-versie 0.12.x of hoger is geïnstalleerd op uw ontwikkelcomputer. Voer `node --version` achter de opdrachtprompt of in een shell om de versie te controleren. Zie voor meer informatie over het gebruik van een pakketbeheerder Node.js installeren in Linux [Node.js installeren via Pakketbeheer][node-linux].
3. Als u Node.js hebt geïnstalleerd, de meest recente versie van de kloon de [azure-iot-sdk-knooppunt] [ lnk-github-repo] opslagplaats naar uw ontwikkelmachine. Gebruik altijd de **master** vertakking voor de nieuwste versie van de bibliotheken en voorbeelden.
4. In uw lokale exemplaar van de [azure-iot-sdk-knooppunt] [ lnk-github-repo] opslagplaats, de volgende twee bestanden kopiëren van de apparaat-knooppunt-voorbeelden-map naar een lege map op uw ontwikkelcomputer:
   
   * Packages.JSON
   * remote_monitoring.js
5. Open het bestand remote_monitoring.js en zoek naar de definitie van de volgende variabele:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Vervang **[IoT Hub apparaat-verbindingsreeks]** met de verbindingsreeks van uw apparaat. Gebruik de waarden voor uw IoT Hub-hostnaam, de apparaat-id en de apparaatsleutel die u een genoteerd in stap 1 hebt. De verbindingsreeks voor een apparaat heeft de volgende indeling:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Als uw IoT Hub-hostnaam **contoso** en uw apparaat-id is **mijnapparaat**, de verbindingsreeks ziet eruit als in het volgende codefragment:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Sla het bestand op. Voer de volgende opdrachten in een shell of opdrachtprompt in de map met deze bestanden voor het installeren van de vereiste pakketten en voer de voorbeeldtoepassing:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Dynamische telemetrie in actie zien
Het dashboard ziet u de temperatuur en vochtigheid telemetrie van de bestaande gesimuleerde apparaten:

![De standaard-dashboard][image1]

Als u het Node.js gesimuleerde apparaat die u in de vorige sectie hebt uitgevoerd selecteert, ziet u de temperatuur, vochtigheid en externe temperatuurtelemetrie:

![Externe temperatuur aan het dashboard toevoegen][image2]

De oplossing voor externe controle automatisch detecteert het telemetrietype extra externe temperatuur en toegevoegd aan de grafiek op het dashboard.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-v1-send-external-temperature/image1.png
[image2]: media/iot-suite-v1-send-external-temperature/image2.png
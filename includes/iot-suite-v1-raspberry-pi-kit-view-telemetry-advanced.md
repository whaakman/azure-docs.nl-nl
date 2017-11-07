## <a name="view-the-telemetry"></a>De telemetrie weergeven

De Pi frambozen is nu met het verzenden van telemetrie naar de oplossing voor externe controle. U kunt de telemetrie weergeven op het dashboard van oplossing. U kunt ook berichten verzenden naar uw Pi frambozen vanuit het dashboard van oplossing.

- Ga naar het dashboard van de oplossing.
- Selecteer het apparaat in de **apparaat naar de weergave** vervolgkeuzelijst.
- De telemetrie van de Pi frambozen wordt weergegeven op het dashboard.

![Telemetrie weergegeven van de Pi frambozen][img-telemetry-display]

## <a name="initiate-the-firmware-update"></a>Starten van de firmware-update

Het updateproces firmware downloadt en installeert een bijgewerkte versie van de apparaat-clienttoepassing op de frambozen Pi. Zie voor meer informatie over het bijwerkproces firmware, de beschrijving van de firmware-update-patroon in [overzicht van Apparaatbeheer met IoT Hub][lnk-update-pattern].

U start het updateproces firmware door het aanroepen van een methode op het apparaat. Deze methode is asynchroon en retourneert zodra de update wordt gestart. Het apparaat worden gemeld eigenschappen op de hoogte van de oplossing over de voortgang van de update wilt.

U kunt methoden aanroepen op uw Pi frambozen vanuit het dashboard van oplossing. Wanneer de Pi frambozen is eerst verbinding met de oplossing voor externe controle maakt, stuurt informatie over de methoden die wordt ondersteund. 

[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md

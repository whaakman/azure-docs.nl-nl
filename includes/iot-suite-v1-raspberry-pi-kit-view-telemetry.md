## <a name="view-the-telemetry"></a>De telemetrie weergeven

De Pi frambozen is nu met het verzenden van telemetrie naar de oplossing voor externe controle. U kunt de telemetrie weergeven op het dashboard van oplossing. U kunt ook berichten verzenden naar uw Pi frambozen vanuit het dashboard van oplossing.

- Ga naar het dashboard van de oplossing.
- Selecteer het apparaat in de **apparaat naar de weergave** vervolgkeuzelijst.
- De telemetrie van de Pi frambozen wordt weergegeven op het dashboard.

![Telemetrie weergegeven van de Pi frambozen][img-telemetry-display]

## <a name="act-on-the-device"></a>Reageren op het apparaat

U kunt vanuit het dashboard oplossing methoden aanroepen op uw frambozen Pi. Wanneer de Pi frambozen verbinding met de oplossing voor externe controle, stuurt informatie over de methoden die wordt ondersteund.

- Klik in het dashboard van de oplossing op **apparaten** bezoeken de **apparaten** pagina. Selecteer uw Raspberry Pi in de **lijst met apparaten**. Kies vervolgens **methoden**:

    ![Lijst met apparaten in het dashboard][img-list-devices]

- Op de **methode Invoke** pagina **LightBlink** in de **methode** vervolgkeuzelijst.

- Kies **InvokeMethod**. De LED verbonden met de flitsen frambozen Pi meermaals. De app op de Pi frambozen verzendt een bevestiging terug naar het dashboard van oplossing:

    ![Overzicht van de methode weergeven][img-method-history]

- Kunt u de LED in- en uitschakelen met behulp van de **ChangeLightStatus** methode met een **LightStatusValue** ingesteld op **1** voor op of **0** voor uitschakelen.

> [!WARNING]
> Als u de oplossing voor externe controle uitgevoerd in uw Azure-account laat, wordt u gefactureerd voor de tijd die wordt uitgevoerd. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config]. De vooraf geconfigureerde oplossing verwijderen uit uw Azure-account wanneer u klaar bent met het gebruik van maken.


[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
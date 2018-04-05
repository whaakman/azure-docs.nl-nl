## <a name="view-device-telemetry"></a>Telemetrie van apparaten weergeven

U kunt de telemetrie van uw apparaat op verzonden weergeven de **apparaten** pagina in de oplossing.

1. Selecteer het apparaat dat u hebt ingericht in de lijst met apparaten op de **apparaten** pagina. Een paneel geeft informatie weer over uw apparaat met inbegrip van de tekening van de apparaattelemetrie:

    ![Zie de details van het apparaat](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Kies **druk** de telemetrie-weergave wijzigen:

    ![Druk telemetrie van paginaweergaven](media/iot-suite-visualize-connecting/devicespressure.png)

1. Als u diagnostische informatie over uw apparaat, schuif omlaag naar **Diagnostics**:

    ![Weergave apparaat diagnostische gegevens](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Reageren op uw apparaat

Gebruiken om aan te roepen methoden op uw apparaten, de **apparaten** pagina in de oplossing voor externe controle. Bijvoorbeeld, in de oplossing voor externe controle **Koelunit** apparaten implementeren een **FirmwareUpdate** methode.

1. Kies **apparaten** om te navigeren naar de **apparaten** pagina in de oplossing.

1. Selecteer het apparaat dat u hebt ingericht in de lijst met apparaten op de **apparaten** pagina:

    ![Selecteer het fysieke apparaat](media/iot-suite-visualize-connecting/devicesselect.png)

1. Voor een lijst van de methoden die u op uw apparaat aanroepen kunt, kies **planning**. Als u een methode uit te voeren op meerdere apparaten plannen, kunt u meerdere apparaten in de lijst. De **planning** deelvenster toont de typen van de methode algemene op alle apparaten die u hebt geselecteerd.

1. Kies **FirmwareUpdate**, de taaknaam van de ingesteld op **UpdatePhysicalChiller**. Ingesteld **firmwareversie** naar **2.0.0**stelt **Firmware URI** naar **http://contoso.com/updates/firmware.bin**, en kies vervolgens **toepassen**:

    ![De firmware-update plannen](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Een reeks berichten weergegeven in de console die de apparaatcode van uw wordt uitgevoerd terwijl het gesimuleerde apparaat de methode verwerkt.

1. Wanneer de update voltooid is, wordt de nieuwe firmwareversie weergegeven op de **apparaten** pagina:

    ![Bijwerken voltooid](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Om bij te houden de status van de taak in de oplossing, kies **weergave**.

## <a name="next-steps"></a>Volgende stappen

Het artikel [aanpassen van de vooraf geconfigureerde oplossing voor externe controle](../articles/iot-suite/iot-suite-remote-monitoring-customize.md) enkele manieren voor het aanpassen van de vooraf geconfigureerde oplossing beschrijft.
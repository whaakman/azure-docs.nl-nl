> [!div class="op_single_selector"]
> * [C op Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C op Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (algemeen)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Node.js op Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [C op Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-c.md)

In deze zelfstudie implementeert u een **Koelunit** apparaat dat de volgende telemetrie naar de externe controle verzendt [vooraf geconfigureerde oplossing](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Temperatuur
* Druk
* Vochtigheid

Voor het gemak, genereert de code telemetrie voorbeeldwaarden voor de **Koelunit**. U kunt het voorbeeld kan uitbreiden door echte sensoren verbinden met uw apparaat en het verzenden van telemetrie echte.

Het apparaat voorbeeld ook:

* Metagegevens verzendt naar de oplossing voor het beschrijven van de mogelijkheden ervan.
* Reageert op acties geactiveerd vanuit de **apparaten** pagina in de oplossing.
* Reageert op wijzigingen in de configuratie verzenden vanuit de **apparaten** pagina in de oplossing.

U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="before-you-start"></a>Voordat u begint

Voordat u code voor uw apparaat schrijven, wordt uw vooraf geconfigureerde oplossing voor externe controle implementeert en een nieuwe fysieke apparaat toevoegt aan de oplossing.

### <a name="deploy-your-remote-monitoring-preconfigured-solution"></a>Implementeer uw vooraf geconfigureerde oplossing voor externe controle

De **Koelunit** apparaat die u in deze zelfstudie maakt gegevens verzendt naar een exemplaar van de [externe controle](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) vooraf geconfigureerde oplossing. Als u dit nog niet hebt in uw Azure-account al en de vooraf geconfigureerde oplossing voor externe controle ingericht, Zie [implementeren van de vooraf geconfigureerde oplossing voor externe controle](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Wanneer het implementatieproces voor de oplossing voor externe controle is voltooid, klikt u op **starten** dashboard van de oplossing in uw browser te openen.

![Dashboard van de oplossing](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Uw apparaat toevoegt aan de oplossing voor externe controle

> [!NOTE]
> Als u al een apparaat in uw oplossing hebt toegevoegd, kunt u deze stap overslaan. De volgende stap vereist echter de verbindingsreeks van uw apparaat. U kunt ophalen van een apparaat-verbindingsreeks uit de [Azure-portal](https://portal.azure.com) of met behulp van de [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI-hulpprogramma.

Een apparaat kan alleen verbinding maken met de vooraf geconfigureerde oplossing als het zichzelf met geldige referenties kan identificeren bij IoT Hub. U hebt de mogelijkheid om op te slaan de apparaat-verbindingsreeks met deze referenties wanneer u het apparaat toevoegen de oplossing. De verbindingsreeks van het apparaat in uw clienttoepassing verderop in deze zelfstudie te nemen.

Als u wilt een apparaat toevoegt aan uw oplossing voor externe controle, kunt u de volgende stappen uitvoeren op de **apparaten** pagina in de oplossing:

1. Kies **+ nieuw apparaat**, en kies vervolgens **fysieke** als de **apparaattype**:

    ![Een fysiek apparaat toevoegen](media/iot-suite-selector-connecting/devicesprovision.png)

1. Voer **fysiek Koelunit** als de apparaat-ID. Kies de **symmetrische sleutel** en **automatisch genereren van sleutels** opties:

    ![Opties voor apparaten kiezen](media/iot-suite-selector-connecting/devicesoptions.png)

1. Kies **toepassen**. Maak een notitie van de **apparaat-ID**, **primaire sleutel**, en **Connection string primaire sleutel** waarden:

    ![Referenties ophalen](media/iot-suite-selector-connecting/credentials.png)

U hebt nu een fysiek apparaat toegevoegd aan de vooraf geconfigureerde oplossing voor externe controle en de verbindingsreeks van het apparaat hebt genoteerd. In de volgende secties vindt implementeren u de clienttoepassing die de verbindingsreeks van het apparaat verbinding maakt met uw oplossing.

De clienttoepassing implementeert de ingebouwde **Koelunit** Apparaatmodel. Een model van de vooraf geconfigureerde oplossing apparaat Hiermee geeft u de volgende met betrekking tot een apparaat:

* De eigenschappen van het apparaat rapporteert aan de oplossing. Bijvoorbeeld, een **Koelunit** apparaat rapporteert informatie over de firmware en de locatie.
* De typen telemetrie het apparaat verzendt naar de oplossing. Bijvoorbeeld, een **Koelunit** apparaat verzendt temperatuur en vochtigheid druk waarden.
* De methoden die u kunt vanuit de oplossing wilt laten uitvoeren op het apparaat. Bijvoorbeeld, een **Koelunit** apparaat moet worden geïmplementeerd **opnieuw opstarten**, **FirmwareUpdate**, **EmergencyValveRelease**, en  **IncreasePressure** methoden.
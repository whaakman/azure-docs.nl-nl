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

U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="before-you-start"></a>Voordat u begint

Voordat u code voor het apparaat gaat schrijven, moet u de vooraf geconfigureerde oplossing voor externe controle inrichten en een nieuw aangepast apparaat in die oplossing inrichten.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>De vooraf geconfigureerde oplossing voor externe controle inrichten

De **Koelunit** apparaat die u in deze zelfstudie maakt gegevens verzendt naar een exemplaar van de [externe controle](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) vooraf geconfigureerde oplossing. Als u dit nog niet hebt in uw Azure-account al en de vooraf geconfigureerde oplossing voor externe controle ingericht, Zie [implementeren van de vooraf geconfigureerde oplossing voor externe controle](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Wanneer het inrichtingsproces voor de oplossing voor externe controle is voltooid, klikt u op **Starten** om het dashboard van de oplossing in uw browser te openen.

![Dashboard van de oplossing](media/iot-suite-selector-connecting/dashboard.png)

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Het apparaat inrichten in de oplossing voor externe controle

> [!NOTE]
> Als u al een apparaat in uw oplossing hebt ingericht, kunt u deze stap overslaan. De referenties van het apparaat moet u bij het maken van de clienttoepassing.

Een apparaat kan alleen verbinding maken met de vooraf geconfigureerde oplossing als het zichzelf met geldige referenties kan identificeren bij IoT Hub. U kunt de apparaat-referenties ophalen uit de oplossing **apparaten** pagina. Verderop in deze zelfstudie neemt u de referenties van het apparaat op in de clienttoepassing.

Als u wilt een apparaat toevoegt aan uw oplossing voor externe controle, kunt u de volgende stappen uitvoeren op de **apparaten** pagina in de oplossing:

1. Kies **inrichten**, en kies vervolgens **fysieke** als de **apparaattype**:

    ![Een fysiek apparaat inrichten](media/iot-suite-selector-connecting/devicesprovision.png)

1. Voer **fysiek Koelunit** als de apparaat-ID. Kies de **symmetrische sleutel** en **automatisch genereren van sleutels** opties:

    ![Opties voor apparaten kiezen](media/iot-suite-selector-connecting/devicesoptions.png)

Als u wilt de referenties op die het apparaat gebruiken moet voor verbinding met de vooraf geconfigureerde oplossing hebt gevonden, gaat u naar de Azure-portal in uw browser. Meld u aan uw abonnement.

1. Zoek de resourcegroep met de Azure-services die maakt gebruik van uw oplossing voor externe controle. De resourcegroep heeft dezelfde naam als de externe bewakingsoplossing die u hebt ingericht.

1. Navigeer naar de IoT-hub in deze resourcegroep. Kies vervolgens **IoT-apparaten**:

    ![Apparaat explorer](media/iot-suite-selector-connecting/deviceexplorer.png)

1. Kies de **apparaat-ID** u hebt gemaakt op de **apparaten** pagina in de oplossing voor externe controle.

1. Noteer de **apparaat-ID** en **primaire sleutel** waarden. U kunt deze waarden gebruiken wanneer u code toevoegen aan uw apparaat aansluit op de oplossing.

U hebt nu een fysiek apparaat in de externe controle ingericht vooraf geconfigureerde oplossing. In de volgende secties vindt implementeren u de clienttoepassing die de referenties van het apparaat verbinding maakt met uw oplossing.

De clienttoepassing implementeert de ingebouwde **Koelunit** Apparaatmodel. Een model van de vooraf geconfigureerde oplossing apparaat Hiermee geeft u de volgende met betrekking tot een apparaat:

* De eigenschappen van het apparaat rapporteert aan de oplossing. Bijvoorbeeld, een **Koelunit** apparaat rapporteert informatie over de firmware en de locatie.
* De typen telemetrie het apparaat verzendt naar de oplossing. Bijvoorbeeld, een **Koelunit** apparaat verzendt temperatuur en vochtigheid druk waarden.
* De methoden die u kunt vanuit de oplossing wilt laten uitvoeren op het apparaat. Bijvoorbeeld, een **Koelunit** apparaat moet worden geïmplementeerd **opnieuw opstarten**, **FirmwareUpdate**, **EmergencyValveRelease**, en  **IncreasePressuree** methoden.
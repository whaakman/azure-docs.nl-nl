> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)
> 
> 

In dit overzicht van het [Simulated Device Cloud Upload sample] (Voorbeeld van het uploaden van een gesimuleerd apparaat naar de cloud) ziet u hoe u de [Azure IoT Gateway SDK][lnk-sdk] gebruikt om vanuit gesimuleerde apparaten apparaat-naar-cloud telemetrie naar IoT Hub te verzenden.

Dit overzicht omvat:

1. **Architectuur**: belangrijke informatie over de architectuur in het voorbeeld van de cloudupload van gesimuleerde apparaten.
2. **Ontwikkeling en uitvoering**: de stappen die nodig zijn om het voorbeeld te bouwen en uit te voeren.

## <a name="architecture"></a>Architectuur
In het voorbeeld van het uploaden van een gesimuleerd apparaat naar de cloud ziet u hoe u de SDK gebruikt om een gateway te maken die telemetrie van gesimuleerde apparaten naar een IoT Hub verzendt. De gesimuleerde apparaten kunnen geen rechtstreekse verbinding maken met IoT Hub omdat:

* De apparaten gebruiken geen communicatieprotocol dat door IoT Hub wordt begrepen.
* De apparaten kunnen de identiteit niet onthouden die aan hen is toegewezen door IoT Hub.

Deze problemen worden door de gateway op de volgende manieren voor de gesimuleerde apparaten opgelost:

* De gateway begrijpt het protocol dat door de gesimuleerde apparaten wordt gebruikt, ontvangt apparaat-naar-cloud telemetrie van de apparaten en stuurt deze berichten door naar IoT Hub met behulp van een protocol dat de IoT Hub begrijpt.
* De gateway slaat IoT Hub-identiteiten op namens de gesimuleerde apparaten en fungeert als een proxy wanneer de gesimuleerde apparaten berichten naar IoT Hub verzenden.

In het volgende diagram ziet u de belangrijkste onderdelen van het voorbeeld, waaronder de gateway-modules:

![][1]

> [!NOTE]
> De modules geven geen berichten rechtstreeks aan elkaar door. De modules publiceren berichten naar een interne broker, en deze bezorgt de berichten met behulp van een abonnementmechanisme aan de andere modules, zoals in het volgende diagram wordt geïllustreerd. Voor meer informatie raadpleegt u [Aan de slag met de Azure IoT-gateway-SDK][lnk-gw-getstarted].
> 
> 

### <a name="protocol-ingestion-module"></a>Module voor het protocolopname
Deze module is het beginpunt om via de gateway gegevens van apparaten op te halen in de cloud. In het voorbeeld worden door de module vier taken uitgevoerd:

1. Er worden gesimuleerde temperatuurgegevens gemaakt. Let wel: als u echte apparaten gebruikt , leest de module gegevens van deze fysieke apparaten.
2. De gesimuleerde temperatuurgegevens worden in de inhoud van een bericht geplaatst.
3. Vervolgens wordt een eigenschap met een nep-MAC-adres toegevoegd aan het bericht dat de gesimuleerde temperatuurgegevens bevat.
4. Het bericht wordt beschikbaar gesteld voor de volgende module in de keten.

> [!NOTE]
> De module **Protocol X-opname** in bovenstaand diagram heet in de broncode **Gesimuleerd apparaat**.
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>Module MAC &lt;-&gt; IoT Hub-id
Deze module zoekt naar berichten die een eigenschap bevatten die het MAC-adres van de gesimuleerde apparaat-app bevat dat door de protocolopnamemodule is toegevoegd. Als de module een dergelijke eigenschap vindt, wordt een andere eigenschap met een IoT Hub-sleutel aan het bericht toegevoegd en wordt het bericht beschikbaar gesteld voor de volgende module in de keten. Op die manier worden in het voorbeeld de identiteiten van IoT Hub-apparaten aan gesimuleerde apparaten gekoppeld. De ontwikkelaar stelt de toewijzing tussen MAC-adressen en IoT Hub-identiteiten handmatig in als onderdeel van de moduleconfiguratie. 

> [!NOTE]
> In dit voorbeeld wordt een MAC-adres gebruikt als een unieke apparaat-id en wordt deze gekoppeld aan de id van een IoT Hub-apparaat. U kunt echter uw eigen module schrijven die een andere unieke id gebruikt. Misschien hebt u apparaten met unieke serienummers of telemetriegegevens waarin een unieke apparaatnaam is opgenomen. In dat geval zou u die kunnen gebruiken om de id van een IoT Hub-apparaat te bepalen.
> 
> 

### <a name="iot-hub-communication-module"></a>Module voor IoT Hub-communicatie
Deze module gebruikt berichten waaraan door de vorige module een IoT Hub-apparaat-id is toegewezen, en stuurt de berichtinhoud via HTTP naar IoT Hub. HTTP is een van de drie protocollen die door IoT Hub wordt begrepen.

In plaats van voor elke gesimuleerde apparaat-app een verbinding met IoT Hub te openen, opent deze module één HTTP-verbinding vanuit de gateway naar de IoT Hub en gebruikt die verbinding om verbindingen vanuit alle gesimuleerde apparaten simultaan over te seinen. Op dit manier kan één gateway verbinding maken met veel meer apparaten, gesimuleerde of anderszins, dan mogelijk zou zijn als er voor elk apparaat een unieke verbinding zou zijn geopend.

![][2]

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Simulated Device Cloud Upload sample]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md

<!--HONumber=Feb17_HO1-->



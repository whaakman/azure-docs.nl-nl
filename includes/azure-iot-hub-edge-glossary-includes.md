## <a name="iot-edge"></a>IoT Edge
Azure IoT-rand kunt cloud gebaseerde implementatie van Azure-services en oplossings-specifieke code voor het on-premises apparaten. IoT Edge-apparaten aggregeert gegevens van andere apparaten uit te voeren computing en analytics voordat de gegevens worden verzonden naar de cloud. Zie voor meer informatie [Azure IoT rand](https://docs.microsoft.com/azure/iot-edge/).

## <a name="iot-edge-agent"></a>Rand van de IoT-agent
Het gedeelte van de rand van de IoT-runtime verantwoordelijk voor het implementeren en bewaken van modules.

## <a name="iot-edge-device"></a>IoT Edge-apparaat
IoT Edge-apparaten hebben de rand van de IoT-runtime is geïnstalleerd en zijn gemarkeerd als 'IoT rand apparaat' in de details van het apparaat. Meer informatie over hoe [Azure IoT rand implementeren op een gesimuleerd apparaat in Linux - voorbeeld](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux).

## <a name="iot-edge-deployment"></a>Rand van de IoT-implementatie
De implementatie van een IoT-rand configureert u een doelset IoT randapparaten modules voor een set van IoT rand uitvoeren. Elke implementatie is continu zorgt ervoor dat alle apparaten die de doelvoorwaarden overeenkomen met de opgegeven reeks modules, zelfs wanneer nieuwe apparaten worden gemaakt of zijn gewijzigd zodat deze overeenkomen met de doel-voorwaarde. Elke IoT randapparaat wordt alleen de hoogste prioriteit implementatie waarvan het voldoet aan doelvoorwaarden ontvangt. Meer informatie over [IoT rand implementatie](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

## <a name="iot-edge-deployment-manifest"></a>IoT-rand deployment manifest
Een Json-document met de gegevens wilt kopiëren op een of meer IoT randapparaten van de module twin(s) voor het implementeren van een reeks modules, routes en bijbehorende module gewenste eigenschappen.

## <a name="iot-edge-gateway-device"></a>IoT gateway randapparaat
Een IoT randapparaat met downstream-apparaat. Het downstream apparaat kan IoT rand of niet-Edge van de IoT-apparaat zijn.

## <a name="iot-edge-hub"></a>Rand van de IoT hub
Het deel van de rand van de IoT-runtime die verantwoordelijk is voor de module-module-communicatie (naar IoT Hub) upstream- en downstream (weg van IoT Hub) communicatie. 

## <a name="iot-edge-leaf-device"></a>IoT leaf randapparaat
Een IoT randapparaat met geen downstream-apparaat. 

## <a name="iot-edge-module"></a>Rand van de IoT-module
Een IoT-Edge-module is een Docker-container die u aan de rand van de IoT-apparaten kunt implementeren. Hiermee een specifieke taak, zoals het opnemen van een bericht van een apparaat, een bericht transformeren of een bericht verzenden naar een IoT-hub worden uitgevoerd. Deze communiceert met andere modules en verzendt gegevens naar de rand van de IoT-runtime. [Overzicht van de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>IoT-rand module identiteit
Een record in het identiteitenregister van de IoT Hub-module met gedetailleerde informatie over de referenties bestaan en de beveiliging moet worden gebruikt door een module voor verificatie met een edge-hub of IoT Hub.

## <a name="iot-edge-module-image"></a>Afbeelding van IoT Edge-module
De docker-afbeelding die wordt gebruikt door de runtime IoT rand instantiëren van exemplaren van de module.

## <a name="iot-edge-module-twin"></a>IoT-rand module twin
Een Json-document permanent in de IoT-Hub waarmee de statusinformatie voor een exemplaar van de module worden opgeslagen.

## <a name="iot-edge-priority"></a>IoT-rand prioriteit
Wanneer twee implementaties van IoT rand gericht op hetzelfde apparaat, wordt de implementatie met een hogere prioriteit wordt toegepast. Als twee implementaties dezelfde prioriteit hebben, wordt de implementatie met de latere aanmaakdatum toegepast. Meer informatie over [prioriteit](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#priority).

## <a name="iot-edge-runtime"></a>IoT Edge-runtime
IoT-rand runtime omvat alle informatie die Microsoft distribueert om te worden geïnstalleerd op een Edge van de IoT-apparaat. Het Edge-agent, Edge hub en rand CTL hulpprogramma opgenomen.

## <a name="iot-edge-set-modules-to-a-single-device"></a>IoT-rand modules ingesteld op één apparaat
Een bewerking die de inhoud van een manifest van de rand van IoT op één apparaat kopieert ' module twin. De onderliggende API is een algemeen 'configuratie toepassen', die nodig is gewoon een manifest IoT rand als invoer.

## <a name="iot-edge-target-condition"></a>IoT-rand doelvoorwaarden
In de implementatie van een IoT-rand doelvoorwaarden is een Boole-voorwaarde apparaat horende tags selecteren de doelapparaten van de implementatie, bijvoorbeeld ' tag.environment = prod '. De doelvoorwaarden wordt continu op te nemen van nieuwe apparaten die voldoen aan de vereisten of verwijderen van apparaten die niet langer geëvalueerd. Meer informatie over [voorwaarde zijn gericht](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#target-condition)
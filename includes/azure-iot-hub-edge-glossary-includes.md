---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 11/09/2018
ms.author: dobett
ms.openlocfilehash: c95bca125ea70cf32acad0d5ea67c3ad195ed704
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279576"
---
## <a name="automatic-device-management"></a>Automatisch apparaatbeheer
Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel herhaalde en complexe taken van grote apparaat vloten via het geheel van de levenscycli hiervan te beheren. Met automatisch beheer van apparaten, kunt u een set met apparaten op basis van hun eigenschappen als doel, definieert een gewenste configuratie en IoT Hub-apparaten bijgewerkt wanneer ze bereik binnenkomen, kunnen.  Bestaat uit [automatische apparaatconfiguraties](../articles/iot-hub/iot-hub-auto-device-config.md) en [IoT Edge-automatische implementaties](../articles/iot-edge/how-to-deploy-monitor.md).

## <a name="iot-edge"></a>IoT Edge
Azure IoT Edge kunt implementeren in de cloud gebaseerde van Azure-services en oplossingsspecifieke code op on-premises apparaten. IoT Edge-apparaten kunnen de gegevens samenvoegen vanuit andere apparaten om uit te voeren computing en analyses voordat de gegevens worden verzonden naar de cloud. Zie voor meer informatie, [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).

## <a name="iot-edge-agent"></a>IoT Edge-agent
Het deel van de IoT Edge-runtime die verantwoordelijk is voor het implementeren en controleren van modules.

## <a name="iot-edge-device"></a>IoT Edge-apparaat
IoT Edge-apparaten hebben van de IoT Edge-runtime geïnstalleerd en worden gemarkeerd als **IoT Edge-apparaat** in de Apparaatdetails van het. Meer informatie over het [Azure IoT Edge implementeren op een gesimuleerd apparaat in Linux - voorbeeld](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux).

## <a name="iot-edge-automatic-deployment"></a>Automatische implementatie van IoT Edge
Een automatische IoT Edge-implementatie configureert u een doelset IoT Edge-apparaten een set van IoT Edge-modules uitvoeren. Elke implementatie is voortdurend zorgt ervoor dat alle apparaten die voldoen aan de doelvoorwaarde de opgegeven set modules worden uitgevoerd, zelfs wanneer nieuwe apparaten worden gemaakt of worden gewijzigd zodat deze overeenkomt met de doelvoorwaarde. Elke IoT Edge-apparaat ontvangt alleen de hoogste prioriteit implementatie waarvan deze voldoet aan de doelvoorwaarde. Meer informatie over [automatische implementatie van IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

## <a name="iot-edge-deployment-manifest"></a>Manifest van de IoT Edge-implementatie
Een Json-document met de informatie in een of meer IoT Edge-apparaten de module twin(s) implementeren van een reeks modules worden gekopieerd, gewenste routes en de bijbehorende module eigenschappen.

## <a name="iot-edge-gateway-device"></a>IoT Edge-gateway-apparaat
Een IoT Edge-apparaat met downstream-apparaat. De downstream-apparaat kan zijn op IoT Edge of niet IoT Edge-apparaat.

## <a name="iot-edge-hub"></a>IoT Edge hub
Het deel van de IoT Edge-runtime die verantwoordelijk is voor communicatie van de module-module, upstream (dichter bij IoT Hub) en downstream (weg van IoT Hub) communicatie. 

## <a name="iot-edge-leaf-device"></a>IoT Edge leaf-apparaat
Een IoT Edge-apparaat met geen downstream-apparaat. 

## <a name="iot-edge-module"></a>IoT Edge-module
Een IoT Edge-module is een Docker-container die u op IoT Edge-apparaten implementeren kunt. Een specifieke taak, zoals opnemen van een bericht van een apparaat, een bericht te transformeren of een bericht verzenden naar een IoT-hub wordt uitgevoerd. Deze communiceert met andere modules en verzendt gegevens naar de IoT Edge-runtime. [Informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>IoT Edge module identiteit
Een record in het identiteitenregister van de IoT Hub-module met gedetailleerde informatie over de referenties bestaan en de beveiliging moet worden gebruikt door een module om te verifiëren met een edge hub of IoT-Hub.

## <a name="iot-edge-module-image"></a>Installatiekopie voor IoT Edge-module
De docker-installatiekopie die wordt gebruikt door de IoT Edge-runtime voor het starten van module-exemplaren.

## <a name="iot-edge-module-twin"></a>IoT Edge-moduledubbel
Een Json-document vastgehouden in de IoT-Hub waarin de informatie over de status voor een module-exemplaar.

## <a name="iot-edge-priority"></a>IoT Edge-prioriteit
Wanneer twee IoT Edge-implementaties hetzelfde apparaat, wordt de implementatie met een hogere prioriteit wordt toegepast. Als twee implementaties dezelfde prioriteit hebben, de implementatie met latere aanmaakdatum toegepast. Meer informatie over [prioriteit](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#priority).

## <a name="iot-edge-runtime"></a>IoT Edge-runtime
IoT Edge-runtime bevat alles wat Microsoft distribueert om te worden geïnstalleerd op een IoT Edge-apparaat. Hiertoe behoren Edge agent, Edge hub en de IoT Edge-daemon voor beveiliging.

## <a name="iot-edge-set-modules-to-a-single-device"></a>IoT Edge-modules instellen op één apparaat
Een bewerking die de inhoud van een IoT Edge-manifest op één apparaat kopieert ' moduledubbel. De onderliggende API is een algemene 'configuratie toepassen', die gewoon duurt een IoT Edge-manifest als invoer.

## <a name="iot-edge-target-condition"></a>De doelvoorwaarde IoT Edge
In een IoT Edge-implementatie, is de doelvoorwaarde een Boole-voorwaarde voor apparaatdubbels en de labels selecteren de doelapparaten van de implementatie, bijvoorbeeld **tag.environment = prod**. De doelvoorwaarde wordt voortdurend geëvalueerd, zodat nieuwe apparaten die voldoen aan de vereisten worden opgenomen of apparaten die niet langer worden verwijderd. Meer informatie over [voorwaarde als doel](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#target-condition)
---
title: Wat is Azure IoT Central? | Microsoft Docs
description: Azure IoT Central is een end-to-end SaaS-oplossing die u gebruiken kunt om te bouwen en beheren van uw aangepaste IoT-oplossing. In dit artikel vindt u een overzicht van de functies van Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 04/24/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 84fa7aa006a6bc5365527dbf8043797617543590
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704542"
---
<!---
Purpose of an Overview article: 
1. To give a TECHNICAL overview of a service/product: What is it? Why should I use it? It's a "learn" topic that describes key benefits and our competitive advantage. It's not a "do" topic.
2. To help audiences who are new to service but who may be familiar with related concepts. 
3. To compare the service to another service/product that has some similar functionality, ex. SQL Database / SQL Data Warehouse, if appropriate. This info can be in a short list or table. 
-->

# <a name="what-is-azure-iot-central"></a>Wat is Azure IoT Central?

Azure IoT Central is een volledig beheerde IoT-software-as-a-service-oplossing, waarmee u eenvoudig te maken van producten die verbinding maken met de fysieke en digitale werelden. U kunt uw productvisie voor verbonden apparaten als volgt tot leven wekken door:

- Nieuwe inzichten af te leiden uit verbonden apparaten, om uw klanten betere producten en ervaringen te kunnen bieden.
- Nieuwe zakelijke kansen voor uw organisatie te creëren.

Azure IoT Central, in vergelijking met een typisch IoT-project:

- Vermindert het werk te verrichten.
- Operationele kosten en overhead vermindert.
- Kunt u eenvoudig uw toepassing, tijdens het werken met aanpassen:
  - Toonaangevende technologieën zoals [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) en [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/).
  - Beveiligingsfuncties op bedrijfsniveau, zoals end-to-endversleuteling.

De volgende video biedt een overzicht van Azure IoT Central:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

In dit artikel geeft een overzicht, voor Azure IoT Central:

- De typische persona's die aan een project zijn gekoppeld.
- Het maken van uw toepassing.
- Het maken van verbinding tussen uw apparaten en uw toepassing
- Het beheren van uw toepassing.

## <a name="personas"></a>Persona's

De Azure IoT Central documentatie verwijst naar vier personen die met een Azure IoT Central-toepassing werken:

- Een _maker_ is verantwoordelijk voor het definiëren van de typen apparaten die verbinding maken met de toepassing en voor het aanpassen van de toepassing voor de operator.
- Een _operator_ beheert de apparaten die met de toepassing zijn verbonden.
- Een _beheerder_ is verantwoordelijk voor beheertaken zoals het beheer van gebruikers en rollen binnen de toepassing.
- Een _apparaatontwikkelaar_ maakt de code die wordt uitgevoerd op een apparaat dat met uw toepassing is verbonden.

## <a name="create-your-azure-iot-central-application"></a>Uw Azure IoT Central-toepassing maken

Als maker gebruikt u Azure IoT Central om een aangepaste, in de cloud gehoste IoT-oplossing voor uw organisatie te maken. Een aangepaste IoT-oplossing bestaat meestal uit:

- Een cloudtoepassing die telemetriegegevens van uw apparaten ontvangt en die u in staat stelt om deze apparaten te beheren.
- Meerdere apparaten waarop aangepaste code wordt uitgevoerd en die met uw cloudtoepassing zijn verbonden.

U kunt snel een nieuwe Azure IoT Central-toepassing implementeren en vervolgens aan te passen aan uw specifieke vereisten in uw browser. Als een opbouwfunctie voor expressies, u de web-hulpprogramma's gebruiken om te maken van een _apparaat sjabloon_ voor de apparaten die verbinding met uw toepassing maken. Een apparaat-sjabloon is de blauwdruk die de kenmerken en het gedrag van een type apparaat, zoals definieert de:

- Telemetrie die wordt verzonden.
- Bedrijfseigenschappen die kunnen worden gewijzigd door een operator.
- Apparaateigenschappen die zijn ingesteld door een apparaat en die het kenmerk alleen-lezen hebben in de toepassing.
- Drempelwaarden voor de toepassing reageert op.
- Instellingen die het gedrag van het apparaat bepalen.

U kunt uw apparaatsjablonen en uw toepassing direct testen met gesimuleerde gegevens die Azure IoT Central voor u genereert.

Als maker kunt u de gebruikersinterface van de Azure IoT Central-toepassing ook aanpassen voor de operators die verantwoordelijk zijn voor het dagelijks gebruik van de toepassing. Voorbeelden van aanpassingen die een maker kan maken, zijn:

- Het definiëren van de indeling van eigenschappen en instellingen in een apparaatsjabloon.
- Het configureren van aangepaste dashboards, zodat operators nieuwe inzichten kunnen krijgen en problemen sneller kunnen oplossen.
- Het configureren van aangepaste analyses om tijdseriegegevens van uw verbonden apparaten te verkennen.

## <a name="connect-your-devices"></a>Uw apparaten verbinden

Nadat de maker heeft gedefinieerd welke typen apparaten verbinding kunnen maken met de toepassing, maakt een apparaatontwikkelaar de code die op de apparaten moet worden uitgevoerd. Als apparaatontwikkelaar gebruikt u de [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) van Microsoft, die open-source zijn, om de code voor uw apparaat te maken. Deze SDK's hebben uitgebreide taal-, platform- en protocolondersteuning, zodat ze kunnen voldoen aan uw behoeften bij het verbinden van uw apparaten met uw Azure IoT Central-toepassing. De SDK's kunnen u de volgende mogelijkheden voor apparaten implementeren:

- Een beveiligde verbinding maken.
- Telemetrie verzenden.
- Status melden.
- Configuratie-updates ontvangen.

Lees de blogpost [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don't](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Voordelen van het gebruik van de Azure IoT SDK's en valkuilen die u moet vermijden als u deze niet gebruikt) voor meer informatie.

## <a name="manage-your-application"></a>Uw toepassing beheren

Azure IoT Central-toepassingen worden volledig gehost door Microsoft, waardoor de overheadkosten voor het beheer van uw toepassingen worden verlaagd.

Als operator gebruikt u de Azure IoT Central-toepassing om de apparaten in uw Azure IoT Central-oplossing te beheren. Operatoren uitvoeren zoals taken:

- Het controleren van de apparaten die met de toepassing zijn verbonden.
- Het oplossen en verhelpen van problemen met apparaten.
- Het inrichten van nieuwe apparaten.

Als een opbouwfunctie voor expressies, kunt u aangepaste regels en acties die worden uitgevoerd via het streamen van verbonden apparaten van gegevens definiëren. Een operator kan deze regels op apparaatniveau in- of uitschakelen om taken binnen de toepassing te beheren en automatiseren.

Beheerders beheren de toegang tot uw toepassing met [gebruikersrollen en machtigingen](howto-administer.md).

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van Azure IoT Central hebt, zijn dit mogelijke volgende stappen:

- Leer wat de verschillen zijn tussen de oplossingsversnellers [Azure IoT Central en Azure IoT](overview-iot-options.md).
- Raak vertrouwd met de [gebruikersinterface van Azure IoT Central](overview-iot-central-tour.md).
- Ga aan de slag door [een Azure IoT Central-toepassing te maken](quick-deploy-iot-central.md).
- Volg een reeks zelfstudies die u leren hoe u:
  - [Als maker een apparaatsjabloon maakt](tutorial-define-device-type.md)
  - [Als maker regels toevoegt om uw oplossing te automatiseren](tutorial-configure-rules.md)
  - [Als maker de toepassing aanpast voor uw operators](tutorial-customize-operator.md)
  - [Als operator uw apparaten bewaakt](tutorial-monitor-devices.md)
  - [Als operator een echt apparaat toevoegt aan uw oplossing](tutorial-add-device.md)
  - [Als apparaatontwikkelaar code voor uw apparaten maakt](tutorial-add-device.md#prepare-the-client-code)

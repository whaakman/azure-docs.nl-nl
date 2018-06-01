---
title: Wat is Azure IoT Central? | Microsoft Docs
description: Azure IoT Central is een end-to-end-SaaS-oplossing die u kunt gebruiken om uw aangepaste IoT-oplossing te maken en beheren. In dit artikel vindt u een overzicht van de functies van Azure IoT Central.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 41eb9049b45c6835dd866c988af486a4975c4831
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201163"
---
<!---
Purpose of an Overview article: 
1. To give a TECHNICAL overview of a service/product: What is it? Why should I use it? It's a "learn" topic that describes key benefits and our competitive advantage. It's not a "do" topic.
2. To help audiences who are new to service but who may be familiar with related concepts. 
3. To compare the service to another service/product that has some similar functionality, ex. SQL Database / SQL Data Warehouse, if appropriate. This info can be in a short list or table. 
-->

# <a name="what-is-azure-iot-central"></a>Wat is Azure IoT Central?

Microsoft Azure IoT Central is een volledig beheerde Software-as-a-Service-oplossing voor IoT-software, waarmee u gemakkelijk producten kunt maken die de echte wereld en de digitale wereld met elkaar verbinden. U kunt uw productvisie voor verbonden apparaten als volgt tot leven wekken door:

- Nieuwe inzichten af te leiden uit verbonden apparaten, om uw klanten betere producten en ervaringen te kunnen bieden.
- Nieuwe zakelijke kansen voor uw organisatie te creëren.

In vergelijking met een typisch IoT-project, maakt Azure IoT Central het beheer van een IoT-oplossing eenvoudiger door:

- De beheerlast te verminderen.
- De operationele kosten en overheadkosten te verminderen.
- Het u makkelijk te maken om uw toepassing aan te passen en gebruik te maken van:
  - Toonaangevende technologieën zoals [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) en [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/).
  - Beveiligingsfuncties op bedrijfsniveau, zoals end-to-endversleuteling.

De volgende video biedt een overzicht van Azure IoT Central:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

De rest van dit artikel biedt een overzicht van de volgende aspecten van Azure IoT Central:

- De typische persona's die aan een project zijn gekoppeld.
- Het maken van uw toepassing.
- Het maken van verbinding tussen uw apparaten en uw toepassing
- Het beheren van uw toepassing.

## <a name="personas"></a>Persona's

In de documentatie over Azure IoT Central wordt verwezen naar vier typische persona's die interactie hebben met een Azure IoT Central-toepassing:

- Een _maker_ is verantwoordelijk voor het definiëren van de typen apparaten die verbinding maken met de toepassing en voor het aanpassen van de toepassing voor de operator.
- Een _operator_ beheert de apparaten die met de toepassing zijn verbonden.
- Een _beheerder_ is verantwoordelijk voor beheertaken zoals het beheer van gebruikers en rollen binnen de toepassing.
- Een _apparaatontwikkelaar_ maakt de code die wordt uitgevoerd op een apparaat dat met uw toepassing is verbonden.

## <a name="create-your-azure-iot-central-application"></a>Uw Azure IoT Central-toepassing maken

Als maker gebruikt u Azure IoT Central om een aangepaste, in de cloud gehoste IoT-oplossing voor uw organisatie te maken. Een aangepaste IoT-oplossing bestaat meestal uit:

- Een cloudtoepassing die telemetriegegevens van uw apparaten ontvangt en die u in staat stelt om deze apparaten te beheren.
- Meerdere apparaten waarop aangepaste code wordt uitgevoerd en die met uw cloudtoepassing zijn verbonden.

U kunt een nieuwe Azure IoT Central-toepassing snel implementeren en deze vervolgens rechtstreeks in uw browser aanpassen aan uw specifieke vereisten. Als maker in Azure IoT Central kunt u de webgebaseerde hulpprogramma's gebruiken om een _apparaatsjabloon_ te maken voor de apparaten die verbinding maken met uw toepassing. Een apparaatsjabloon is de blauwdruk van een apparaatmodel, die alle apparaten hebben gemaakt op basis van de share voor apparaatsjablonen. Een apparaatsjabloon definieert de eigenschappen en het gedrag van een bepaald type apparaat, zoals:

- De telemetriegegevens die het verstuurt.
- Bedrijfseigenschappen die kunnen worden gewijzigd door een operator.
- Apparaateigenschappen die zijn ingesteld door een apparaat en die het kenmerk alleen-lezen hebben in de toepassing.
- De drempelwaarden waar de toepassing op reageert.
- Instellingen die het gedrag van het apparaat bepalen.

U kunt uw apparaatsjablonen en uw toepassing direct testen met gesimuleerde gegevens die Azure IoT Central voor u genereert.

Als maker kunt u de gebruikersinterface van de Azure IoT Central-toepassing ook aanpassen voor de operators die verantwoordelijk zijn voor het dagelijks gebruik van de toepassing. Voorbeelden van aanpassingen die een maker kan maken, zijn:

- Het definiëren van de indeling van eigenschappen en instellingen in een apparaatsjabloon.
- Het configureren van aangepaste dashboards, zodat operators nieuwe inzichten kunnen krijgen en problemen sneller kunnen oplossen.
- Het configureren van aangepaste analyses om tijdseriegegevens van uw verbonden apparaten te verkennen.

## <a name="connect-your-devices"></a>Uw apparaten verbinden

Nadat de maker heeft gedefinieerd welke typen apparaten verbinding kunnen maken met de toepassing, maakt een apparaatontwikkelaar de code die op de apparaten moet worden uitgevoerd. Als apparaatontwikkelaar gebruikt u de [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) van Microsoft, die open-source zijn, om de code voor uw apparaat te maken. Deze SDK's hebben uitgebreide taal-, platform- en protocolondersteuning, zodat ze kunnen voldoen aan uw behoeften bij het verbinden van uw apparaten met uw Azure IoT Central-toepassing. De SDK's helpen u met het uitvoeren van de volgende taken op het apparaat dat is verbonden met Azure IoT Central:

- Een beveiligde verbinding maken.
- Telemetrie verzenden.
- Status melden.
- Configuratie-updates ontvangen.

Lees de blogpost [Benefits of using the Azure IoT SDKs, and pitfalls to avoid if you don't](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) (Voordelen van het gebruik van de Azure IoT SDK's en valkuilen die u moet vermijden als u deze niet gebruikt) voor meer informatie.

## <a name="manage-your-application"></a>Uw toepassing beheren

Azure IoT Central-toepassingen worden volledig gehost door Microsoft, waardoor de overheadkosten voor het beheer van uw toepassingen worden verlaagd.

Als operator gebruikt u de Azure IoT Central-toepassing om de apparaten in uw Azure IoT Central-oplossing te beheren. Operators kunnen taken uitvoeren zoals:

- Het controleren van de apparaten die met de toepassing zijn verbonden.
- Het oplossen en verhelpen van problemen met apparaten.
- Het inrichten van nieuwe apparaten.

Een maker kan aangepaste regels en acties definiëren die werken via streaminggegevens op het niveau van de apparaatsjabloon. Een operator kan deze regels op apparaatniveau in- of uitschakelen om taken binnen de toepassing te beheren en automatiseren.

Beheerders kunnen de toegang tot uw toepassing beheren met [gebruikersrollen en -machtigingen](howto-administer.md).

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van Azure IoT Central hebt, zijn dit mogelijke volgende stappen:

- Leer wat de verschillen zijn tussen [Azure IoT Central en Azure IoT Suite](overview-iot-options.md).
- Raak vertrouwd met de [gebruikersinterface van Azure IoT Central](overview-iot-central-tour.md).
- Ga aan de slag door [een Azure IoT Central-toepassing te maken](quick-deploy-iot-central.md).
- Volg een reeks zelfstudies die u leren hoe u:
  - [Als maker een apparaatsjabloon maakt](tutorial-define-device-type.md)
  - [Als maker regels toevoegt om uw oplossing te automatiseren](tutorial-configure-rules.md)
  - [Als maker de toepassing aanpast voor uw operators](tutorial-customize-operator.md)
  - [Als operator uw apparaten bewaakt](tutorial-monitor-devices.md)
  - [Als operator een echt apparaat toevoegt aan uw oplossing](tutorial-add-device.md)
  - [Als apparaatontwikkelaar code voor uw apparaten maakt](tutorial-add-device.md#prepare-the-client-code)

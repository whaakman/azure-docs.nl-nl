---
title: Wat is Azure IoT Edge | Microsoft Docs
description: Overzicht van de service Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: chipalost
ms.service: iot-edge
services: iot-edge
ms.topic: overview
ms.date: 04/17/2019
ms.author: kgremban
ms.custom: mvc
ms.openlocfilehash: 70a8e17ecdd318a800c51e04b4453c182d1fbdc1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699146"
---
# <a name="what-is-azure-iot-edge"></a>Wat is Azure IoT Edge?

Azure IoT Edge verplaatst cloud-analyse en aangepaste bedrijfslogica naar apparaten, zodat uw organisatie zich kan richten op zakelijke inzichten in plaats van gegevensbeheer. Uw IoT-oplossing uitbreiden door het verpakken van uw bedrijfslogica in standard containers, kunt u deze containers implementeren in een van uw apparaten en deze geheel vanuit de cloud te bewaken. 

Analyses stimuleren bedrijfswaarde in IoT-oplossingen, maar niet alle analyses hoeven zich in de cloud te bevinden. Als u zo snel mogelijk op noodsituaties reageert wilt, kunt u anomaly detection workloads uitvoeren aan de rand. Als u wilt bandbreedtekosten verlagen en de overdracht van terabytes van onbewerkte gegevens voorkomen, kunt u opschonen en de gegevens lokaal te aggregeren vervolgens alleen de inzichten te verzenden naar de cloud voor analyse. 

Azure IoT Edge bestaat uit drie onderdelen:
* **IoT Edge-modules** zijn containers die Azure-services, services van derden of uw eigen code worden uitgevoerd. De modules zijn geïmplementeerd op IoT Edge-apparaten en worden lokaal uitgevoerd op deze apparaten. 
* De **IoT Edge-runtime** wordt uitgevoerd op elk IoT Edge-apparaat en beheert de modules die zijn geïmplementeerd op elk apparaat. 
* Een **cloud-gebaseerde interface** kunt u op afstand bewaken en beheren van IoT Edge-apparaten.

>[!NOTE]
>Azure IoT Edge is alleen beschikbaar in de prijscategorieën gratis en standaard van IoT Hub. De gratis optie is alleen bedoeld voor testen en evalueren. Raadpleeg [How to choose the right IoT Hub tier](../iot-hub/iot-hub-scaling.md) (De juiste IoT Hub-prijscategorie kiezen) voor meer informatie over de Basic- en Standard-prijscategorieën.

## <a name="iot-edge-modules"></a>IoT Edge-modules

IoT Edge-modules zijn eenheden van uitvoerbewerking die als compatibele Docker-containers zijn geïmplementeerd, die uw bedrijfslogica aan de rand uitvoeren. Er kunnen meerdere modules worden geconfigureerd om met elkaar te communiceren, waardoor er een pijplijn van gegevensverwerking ontstaat. U kunt aangepaste modules ontwikkelen of bepaalde Azure-services ordenen in modules die offline en aan de rand inzichten geven. 

### <a name="artificial-intelligence-at-the-edge"></a>Kunstmatige intelligentie aan de rand

Met Azure IoT Edge kunt u verwerking van complexe gebeurtenissen, machine learning, afbeeldingsherkenning en andere vormen van kunstmatige intelligentie (AI) van hoge waarde implementeren zonder deze intern te schrijven. Azure-services zoals Azure Functions, Azure Stream Analytics en Azure Machine Learning kunnen allemaal worden on-premises uitvoert via Azure IoT Edge. U bent niet beperkt tot Azure-services, maar. Iedereen kan AI-modules maken en deze beschikbaar stellen aan de community voor gebruik via Microsoft Azure Marketplace. 

### <a name="bring-your-own-code"></a>Uw eigen code gebruiken

Als u uw eigen code op uw apparaten wilt implementeren, ondersteunt Azure IoT Edge deze ook. Azure IoT Edge bevat hetzelfde programmeermodel als de andere Azure IoT-services. Dezelfde code kan worden uitgevoerd op een apparaat of in de cloud. Azure IoT Edge ondersteunt Linux en Windows, dus u kunt coderen voor het platform van uw keuze. Het programma biedt ondersteuning voor Java, .NET Core 2.0, Node.js, C en Python. Uw ontwikkelaars kunnen dus in een taal coderen die ze al kennen en bestaande bedrijfslogica gebruiken.

## <a name="iot-edge-runtime"></a>IoT Edge-runtime

De Azure IoT Edge-runtime maakt aangepaste en cloudlogica op IoT Edge-apparaten mogelijk. Deze bevindt zich op het IoT Edge-apparaat en voert beheer- en communicatiebewerkingen uit. De runtime voert verschillende functies uit:

* Workloads op het apparaat installeren en bijwerken.
* De Azure IoT Edge-beveiligingsstandaarden op het apparaat onderhouden.
* Ervoor zorgen dat de IoT Edge-modules altijd worden uitgevoerd.
* De status van de module aan de cloud rapporteren voor externe bewaking.
* De communicatie tussen downstream bladknooppuntapparaten en een IoT Edge-apparaat, tussen modules op een IoT Edge-apparaat en tussen een IoT Edge-apparaat en de cloud beheren.

![IoT Edge-runtime verzendt inzichten en rapportages naar IoT Hub](./media/about-iot-edge/runtime.png)

Hoe u een Azure IoT Edge-apparaat gebruikt, is aan u. De runtime wordt vaak gebruikt voor het implementeren van AI aan gatewayapparaten die aggregeren en verwerken van gegevens van andere on-premises apparaten, maar dit implementatiemodel is slechts één optie. 

De Azure IoT Edge-runtime wordt uitgevoerd op een groot aantal IoT-apparaten, zodat u de runtime op tal van manieren kunt gebruiken. Deze biedt ondersteuning voor Linux- en Windows-besturingssystemen en abstraheert hardwaregegevens. Gebruik een apparaat kleiner is dan een Raspberry Pi 3 als u niet veel gegevens verwerkt bent of een industrie-server gebruiken om uit te voeren van de resource-intensieve workloads.

## <a name="iot-edge-cloud-interface"></a>Cloudinterface van IoT Edge

Het is moeilijk te beheren van de softwarelevenscyclus voor miljoenen IoT-apparaten die vaak verschillende merken en modellen zijn of geografisch verspreid. Werkbelastingen worden gemaakt en geconfigureerd voor een bepaald type apparaat, op al uw apparaten geïmplementeerd en gecontroleerd om eventuele metagegevenscaching apparaten. Deze activiteiten kunnen niet per apparaat worden uitgevoerd, maar moeten op schaal worden uitgevoerd.

Azure IoT Edge integreert naadloos met Azure IoT-oplossingsversnellers en biedt één controlelaag voor de behoeften van uw oplossing. Met cloudservices kunt u het volgende doen:

* een workload maken en configureren die op een specifiek type apparaat wordt uitgevoerd;
* een workload verzenden naar een reeks apparaten;
* workloads bewaken die worden uitgevoerd op apparaten in het veld.

![Apparaattelemetrie en acties worden gecoördineerd in de cloud](./media/about-iot-edge/cloud-interface.png)

## <a name="next-steps"></a>Volgende stappen

Probeer deze concepten uit door [IoT Edge te implementeren op een gesimuleerd apparaat](quickstart.md).
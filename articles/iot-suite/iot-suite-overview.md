---
title: Overzicht van Microsoft Azure IoT Suite | Microsoft Docs
description: Overzicht van hoe Azure IoT Suite vooraf geconfigureerde oplossingen voor het Internet of Things biedt voor het verzamelen, analyseren en opslaan van gegevens, voor het bieden van visualisaties en voor integratie met andere systemen.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfa8dbbd0b1d943a9eb7a042df0bac25189d9ac9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-azure-iot-suite"></a>Overview of Azure IoT Suite (Engelstalig)

De Azure Internet of Things-services (IoT) bieden een breed scala aan mogelijkheden. Met deze hoogwaardige services kunt u het volgende doen:

* Gegevens van apparaten verzamelen
* Gegevensstromen in beweging analyseren
* Grote gegevenssets opslaan en er query’s op uitvoeren
* Gegevens in realtime en historische gegevens visualiseren
* Integraties met back-officesystemen uitvoeren
* Uw apparaten beheren

Om deze mogelijkheden te kunnen bieden, bevatten Azure IoT Suite-pakketten meerdere Azure-services met aangepaste extensies als *vooraf geconfigureerde oplossingen*. Deze vooraf geconfigureerde oplossingen zijn basisimplementaties van algemene patronen van IoT-oplossingen die u helpen de tijd voor het leveren van uw IoT-oplossingen te verkorten. Met de [IoT-SDK's (Software Development Kits)][lnk-sdks] kunt u deze oplossingen aanpassen en uitbreiden al naargelang uw eigen behoeften. U kunt deze oplossingen gebruiken als voorbeelden of sjablonen als u nieuwe IoT-oplossingen ontwikkelt.

De volgende video toont een inleiding tot Azure IoT Suite:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT-services in Azure IoT Suite
De vooraf geconfigureerde oplossingen gebruiken doorgaans de volgende services:

* De kern van Azure IoT Suite is de service [Azure IoT Hub][lnk-iot-hub]. Deze service verzorgt de berichtgeving van het apparaat naar de cloud en van de cloud naar het apparaat, en fungeert als toegangspoort tot de cloud en de andere belangrijke IoT Suite-services. Door deze service kunt u op grote schaal berichten ontvangen van uw apparaten en opdrachten naar uw apparaten verzenden. Met deze service kunt u ook [uw apparaten beheren][lnk-device-management]. U kunt bijvoorbeeld een of meer apparaten die zijn verbonden met de hub configureren, opnieuw opstarten of hierop de fabrieksinstellingen terugzetten.
* [Azure Stream Analytics][lnk-asa] biedt de mogelijkheid om gegevens in beweging te analyseren. IoT Suite gebruikt deze service om binnenkomende telemetriegegevens te verwerken, aggregatiebewerkingen uit te voeren en gebeurtenissen te detecteren. De vooraf geconfigureerde maken ook gebruik van Stream Analytics voor het verwerken van informatieve berichten met gegevens, zoals metagegevens of reacties van apparaten op opdrachten. De oplossingen maken gebruik van Stream Analytics om de berichten van uw apparaten te verwerken en deze berichten bij andere services af te leveren.
* [Azure Storage][lnk-azure-storage] en [Azure Cosmos DB][lnk-document-db] bieden de mogelijkheid om gegevens op te slaan. De vooraf geconfigureerde oplossingen gebruiken Blob Storage voor het opslaan van telemetriegegevens en om deze beschikbaar te maken voor analyse. De oplossingen gebruiken Cosmos DB om metagegevens van apparaten op te slaan. Cosmos DB zorgt er tevens voor dat de oplossingen gebruik kunnen maken van mogelijkheden voor apparaatbeheer.
* [Azure Web Apps][lnk-web-apps] en [Microsoft Power BI][lnk-power-bi] bieden de mogelijkheden om gegevens te visualiseren. Door de flexibiliteit van Power BI kunt u snel uw eigen interactieve dashboards bouwen die gebruikmaken van de gegevens van IoT Suite.

Zie [Microsoft Azure en het Internet of Things (IoT)][iot-suite-what-is-azure-iot] voor een overzicht van de architectuur van een typische IoT-oplossing.

## <a name="preconfigured-solutions"></a>Vooraf geconfigureerde oplossingen

IoT Suite bevat vooraf geconfigureerde oplossingen zodat u algemene IoT-scenario's snel kunt gaan gebruiken en verkennen. Een aantal scenario's zijn:

* Externe bewaking
* Voorspellend onderhoud
* Verbonden factory

U kunt deze oplossingen implementeren in uw Azure-abonnement en vervolgens een volledig end-to-end-IoT-scenario uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Nu u een idee hebt van wat IoT Suite kan doen en wat de belangrijkste onderdelen zijn, kunt u zich verder gaan verdiepen in de vooraf geconfigureerde oplossingen in IoT Suite. Zie [Wat zijn de vooraf geconfigureerde Azure IoT Suite-oplossingen?][lnk-what-are-preconfig] voor meer informatie.

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md

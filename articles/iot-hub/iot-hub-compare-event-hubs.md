---
title: Azure IoT Hub met Azure Eventhubs vergelijken | Microsoft Docs
description: Een vergelijking van de IoT Hub en Event Hubs-Azure-services markering functionele verschillen en use-cases. De vergelijking bevat de ondersteunde protocollen, beheer van apparaten, bewaking, en uploaden van bestanden.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 7a589ba80b61ea5ef9ea1c941e9a0218a1653c99
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669735"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IoT-apparaten verbinden met Azure: IoT Hub en Eventhubs

Azure biedt speciaal ontwikkeld voor diverse typen connectiviteit en communicatie kunt u uw gegevens verbinden met de kracht van de cloud. Zowel Azure IoT Hub en Azure Event Hubs zijn cloud-services die u kunnen opnemen van grote hoeveelheden gegevens en verwerken of opslaan van die gegevens voor zakelijke inzichten worden verkregen. De twee services zijn vergelijkbaar in dat deze beide ondersteuning bieden voor opname van gegevens met lage latentie en hoge betrouwbaarheid, maar ze zijn ontworpen voor verschillende doeleinden. IoT Hub is ontwikkeld om de unieke vereisten van de IoT-apparaten verbinden met de Azure-cloud terwijl Event Hubs is ontworpen voor het streamen van big data op te lossen. Microsoft raadt het gebruik van Azure IoT Hub IoT-apparaten verbinden met Azure

Azure IoT Hub is de cloudgateway die wordt aangesloten IoT-apparaten voor het verzamelen van gegevens en tot zakelijke inzichten en automatisering. IoT Hub bevat bovendien functies die de relatie tussen uw apparaten en uw back-endsystemen verrijken. Bi-directionele communicatiemogelijkheden betekenen dat terwijl u gegevens ontvangt van apparaten u ook opdrachten te verzenden kunt en beleidsregels voor back-ups op apparaten maken. Gebruik bijvoorbeeld cloud-naar-apparaat berichten voor het bijwerken van eigenschappen of aanroepen van acties voor het beheer van apparaten. Communicatie van cloud-naar-apparaat kunt u ook met Azure IoT Edge intelligentie van de cloud naar uw edge-apparaten verzenden. De unieke id van op apparaatniveau opgegeven IoT Hub biedt betere beveiliging, helpt uw IoT-oplossing van mogelijke aanvallen. 

[Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) is van de big data. de service van Azure. Het is ontworpen voor hoge gegevensdoorvoer streaming scenario's waarbij klanten miljarden aanvragen per dag verzenden. Eventhubs maakt gebruik van een basis van gepartitioneerd gebruik-model uit uw stream te schalen en is geïntegreerd in de big data- en analyseservices van Azure, met inbegrip van Databricks, Stream Analytics, ADLS en HDInsight. Deze service is ontworpen met functies zoals Event Hubs Capture en automatisch vergroten, ter ondersteuning van uw big data-toepassingen en oplossingen. IoT Hub worden Event Hubs gebruikt voor het pad van de stroom telemetrie, zodat uw IoT-oplossing ook van de enorme mogelijkheden van Event Hubs profiteren.

Beide oplossingen zijn ontworpen om samen te vatten, voor opname van gegevens op een zeer grote schaal. Alleen IoT Hub biedt de uitgebreide IoT-specifieke mogelijkheden die zijn ontworpen voor u de waarde voor het bedrijf van uw IoT-apparaten verbinden met de Azure-cloud te maximaliseren.  Als uw IoT-traject net begint, basismodel beginnend met IoT Hub ter ondersteuning van uw gegevens opnemen's u hebt direct toegang tot de complete IoT-capaciteiten zodra uw zakelijke en technische behoeften ze nodig hebben.

De volgende tabel bevat informatie over hoe de twee niveaus van IoT-Hub met Event Hubs vergelijken als u ze voor IoT-mogelijkheden evalueren wilt. Zie voor meer informatie over de lagen standard en basic van IoT-Hub, [u bij het kiezen van de juiste IoT Hub-laag](iot-hub-scaling.md).

| IoT-mogelijkheid | Standard-laag van IoT Hub | IoT Hub basic-laag | Event Hubs |
| --- | --- | --- | --- |
| Apparaat-naar-cloud-berichten | ![Selecteren][checkmark] | ![Selecteren][checkmark] | ![Selecteren][checkmark] |
| Protocollen: HTTPS, AMQP, AMQP via webSockets | ![Selecteren][checkmark] | ![Selecteren][checkmark] | ![Selecteren][checkmark] |
| Protocollen: MQTT-, MQTT via webSockets | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Id per apparaat | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Bestanden van apparaten uploaden | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Device Provisioning Service | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Cloud-naar-apparaat-berichten | ![Selecteren][checkmark] |  |  |
| Dubbele apparaat- en Apparaatbeheer | ![Selecteren][checkmark] |  |  |
| Apparaat-streams (preview) | ![Selecteren][checkmark] |  |  |
| IoT Edge | ![Selecteren][checkmark] |  |  |

Zelfs als de enige use-case opname van apparaat-naar-cloud-gegevens wordt sterk aangeraden met behulp van IoT Hub biedt een service die is ontworpen voor connectiviteit van IoT-apparaten. 

### <a name="next-steps"></a>Volgende stappen

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
